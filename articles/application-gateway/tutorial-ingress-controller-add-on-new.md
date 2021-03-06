---
title: 'Självstudie: aktivera tillägg för ingångs kontroll för ett nytt AKS-kluster med en ny Azure Application Gateway-instans'
description: I den här självstudien får du lära dig hur du använder Azure CLI för att aktivera tillägg för ingångs kontroll för ditt nya AKS-kluster med en ny Application Gateway-instans.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: tutorial
ms.date: 09/24/2020
ms.author: caya
ms.openlocfilehash: ab917fe476a40eb8ea559bc08e52d4bbf16a8436
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91285612"
---
# <a name="tutorial-enable-the-ingress-controller-add-on-preview-for-a-new-aks-cluster-with-a-new-application-gateway-instance"></a>Självstudie: aktivera tillägg för ingångs kontroll (för hands version) för ett nytt AKS-kluster med en ny Application Gateway instans

Du kan använda Azure CLI för att aktivera AGIC-tillägget [(Application Gateway ingress Controller)](ingress-controller-overview.md) för ett [Azure Kubernetes Services-kluster (AKS)](https://azure.microsoft.com/services/kubernetes-service/) . Tillägget är för närvarande en för hands version.

I den här självstudien skapar du ett AKS-kluster med AGIC-tillägget aktiverat. När klustret skapas skapas automatiskt en Azure Application Gateway-instans som ska användas. Sedan distribuerar du ett exempel program som ska använda tillägget för att exponera programmet genom Application Gateway. 

Tillägget är ett mycket snabbare sätt att distribuera AGIC för ditt AKS-kluster än [tidigare via Helm](ingress-controller-overview.md#difference-between-helm-deployment-and-aks-add-on). Det ger också en fullständigt hanterad upplevelse.    

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Skapa en resursgrupp. 
> * Skapa ett nytt AKS-kluster med AGIC-tillägget aktiverat. 
> * Distribuera ett exempel program genom att använda AGIC för ingress i AKS-klustret.
> * Kontrol lera att programmet kan kontaktas via Application Gateway.

## <a name="prerequisites"></a>Krav

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0.4 eller senare i den här självstudien. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa informationen i [Installera Azure CLI](/cli/azure/install-azure-cli).

Registrera funktions flaggan *AKS-IngressApplicationGatewayAddon* med hjälp av kommandot [AZ Feature register](https://docs.microsoft.com/cli/azure/feature#az-feature-register) , som du ser i följande exempel. Du behöver bara göra detta en gång per prenumeration medan tillägget fortfarande finns i en för hands version.
```azurecli-interactive
az feature register --name AKS-IngressApplicationGatewayAddon --namespace Microsoft.ContainerService
```

Det kan ta några minuter innan status visas `Registered` . Du kan kontrol lera registrerings statusen med hjälp av kommandot [AZ feature list](https://docs.microsoft.com/cli/azure/feature#az-feature-register) :
```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKS-IngressApplicationGatewayAddon')].{Name:name,State:properties.state}"
```

När du är klar uppdaterar du registreringen av resurs leverantören Microsoft. container service med hjälp av kommandot [AZ Provider register](https://docs.microsoft.com/cli/azure/provider#az-provider-register) :
```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

Installera eller uppdatera AKS – för hands versions tillägget för den här självstudien. Använd följande Azure CLI-kommandon:
```azurecli-interactive
az extension add --name aks-preview
az extension list
```
```azurecli-interactive
az extension update --name aks-preview
az extension list
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

I Azure allokerar du relaterade resurser till en resursgrupp. Skapa en resursgrupp med hjälp av [az group create](/cli/azure/group#az-group-create). I följande exempel skapas en resurs grupp med namnet *myResourceGroup* på *indiensödra* -platsen (region): 

```azurecli-interactive
az group create --name myResourceGroup --location canadacentral
```

## <a name="deploy-an-aks-cluster-with-the-add-on-enabled"></a>Distribuera ett AKS-kluster med tillägget aktiverat

Nu ska du distribuera ett nytt AKS-kluster med AGIC-tillägget aktiverat. Om du inte anger en befintlig Application Gateway-instans som ska användas i den här processen skapas och konfigureras automatiskt en ny Application Gateway-instans för att betjäna trafik till AKS-klustret.  

> [!NOTE]
> Tillägget Application Gateway ingångs kort stöder *endast* Application Gateway v2 SKU: er (standard och WAF) och *inte* Application Gateway v1 SKU: er. När du distribuerar en ny Application Gateway-instans via AGIC-tillägget kan du bara distribuera en Application Gateway Standard_v2 SKU. Om du vill aktivera tillägget för en Application Gateway WAF_v2 SKU använder du någon av följande metoder:
>
> - Aktivera WAF på Application Gateway via portalen. 
> - Skapa först WAF_v2 Application Gateway-instansen och följ sedan anvisningarna om hur du [aktiverar AGIC-tillägget med ett befintligt AKS-kluster och befintlig Application Gateway-instans](tutorial-ingress-controller-add-on-existing.md). 

I följande exempel distribuerar du ett nytt AKS-kluster *med namnet IT-kluster med hjälp* av [Azure cni](https://docs.microsoft.com/azure/aks/concepts-network#azure-cni-advanced-networking) och [hanterade identiteter](https://docs.microsoft.com/azure/aks/use-managed-identity). AGIC-tillägget aktive ras i resurs gruppen som du skapade, *myResourceGroup*. 

Om du distribuerar ett nytt AKS-kluster med AGIC-tillägget aktiverat utan att ange en befintlig Application Gateway instans, innebär det att en automatisk generering av en Standard_v2 SKU Application Gateway-instans skapas automatiskt. Så du kan också ange namn och under näts adress utrymme för Application Gateway-instansen. Namnet på Application Gateway-instansen kommer att vara *myApplicationGateway*och adress utrymmet för under nätet som vi använder är 10.2.0.0/16. Se till att du har lagt till eller uppdaterat tillägget AKS-Preview i början av den här självstudien. 

```azurecli-interactive
az aks create -n myCluster -g myResourceGroup --network-plugin azure --enable-managed-identity -a ingress-appgw --appgw-name myApplicationGateway --appgw-subnet-prefix "10.2.0.0/16" 
```

Information om hur du konfigurerar ytterligare parametrar för `az aks create` kommandot finns i [följande referenser](https://docs.microsoft.com/cli/azure/aks?view=azure-cli-latest#az-aks-create). 

> [!NOTE]
> Det AKS-kluster som du har skapat visas i resurs gruppen som du skapade, *myResourceGroup*. Den automatiskt skapade Application Gateway-instansen kommer dock att finnas i resurs gruppen för noden där agent poolerna är. Resurs gruppen för noden med namnet *MC_resource-Group-name_cluster-name_location* som standard, men kan ändras. 

## <a name="deploy-a-sample-application-by-using-agic"></a>Distribuera ett exempel program med hjälp av AGIC

Nu ska du distribuera ett exempel program till det AKS-kluster som du har skapat. Programmet kommer att använda AGIC-tillägget för ingress och ansluta Application Gateway-instansen till AKS-klustret. 

Börja med att hämta autentiseringsuppgifter till AKS-klustret genom att köra `az aks get-credentials` kommandot: 

```azurecli-interactive
az aks get-credentials -n myCluster -g myResourceGroup
```

Nu när du har autentiseringsuppgifter kör du följande kommando för att konfigurera ett exempel program som använder AGIC för ingress till klustret. AGIC kommer att uppdatera Application Gateway-instansen som du ställer in tidigare med motsvarande routningsregler till det nya exempel program som du har distribuerat.  

```azurecli-interactive
kubectl apply -f https://raw.githubusercontent.com/Azure/application-gateway-kubernetes-ingress/master/docs/examples/aspnetapp.yaml 
```

## <a name="check-that-the-application-is-reachable"></a>Kontrol lera att programmet kan kommas åt

Nu när Application Gateway-instansen har kon figurer ATS för att betjäna trafik till AKS-klustret ska vi kontrol lera att programmet kan kommas åt. Börja med att hämta IP-adressen för ingressen: 

```azurecli-interactive
kubectl get ingress
```

Kontrol lera att det exempel program som du har skapat körs av antingen:

- Gå till IP-adressen för den Application Gateway-instans som du fick från att köra föregående kommando.
- Använda `curl` . 

Application Gateway kan ta en minut att hämta uppdateringen. Om Application Gateway fortfarande är i ett **uppdaterings** tillstånd på portalen kan det avslutas innan du försöker komma åt IP-adressen. 

## <a name="clean-up-resources"></a>Rensa resurser

När du inte längre behöver dem tar du bort resurs gruppen, Application Gateway-instansen och alla relaterade resurser:

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Läs om hur du inaktiverar AGIC-tillägget](./ingress-controller-disable-addon.md)

