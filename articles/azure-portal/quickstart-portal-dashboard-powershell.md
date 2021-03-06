---
title: Skapa en Azure Portal instrument panel med PowerShell
description: Lär dig hur du skapar en instrument panel i Azure Portal med Azure PowerShell.
author: mgblythe
ms.service: azure-portal
ms.topic: quickstart
ms.custom: devx-track-azurepowershell
ms.author: mblythe
ms.date: 07/24/2020
ms.openlocfilehash: 6b7a4f6d4ad7f5e94d19b9d531992f54ff13fec0
ms.sourcegitcommit: 14bf4129a73de2b51a575c3a0a7a3b9c86387b2c
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/30/2020
ms.locfileid: "87440786"
---
# <a name="quickstart-create-an-azure-portal-dashboard-with-powershell"></a>Snabb start: skapa en Azure Portal instrument panel med PowerShell

En instrument panel i Azure Portal är en fokuserad och ordnad vy av dina moln resurser. Den här artikeln fokuserar på processen att använda PowerShell-modulen AZ. Portal för att skapa en instrument panel.
Instrument panelen visar prestanda för en virtuell dator (VM) samt en statisk information och länkar.

## <a name="requirements"></a>Krav

Om du inte har en Azure-prenumeration kan du skapa ett [kostnads fritt](https://azure.microsoft.com/free/) konto innan du börjar.

Om du väljer att använda PowerShell lokalt kräver den här artikeln att du installerar AZ PowerShell-modulen och ansluter till ditt Azure-konto med hjälp av cmdleten [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount) . Mer information om hur du installerar AZ PowerShell-modulen finns i [installera Azure PowerShell](https://docs.microsoft.com/powershell/azure/install-az-ps).

> [!IMPORTANT]
> Även om modulen **AZ. Portal** PowerShell är i för hands version måste du installera den separat från AZ PowerShell-modulen med hjälp av `Install-Module` cmdleten. När den här PowerShell-modulen blir allmänt tillgänglig blir den en del av framtida versioner av AZ PowerShell-modulen som är tillgängliga internt inifrån Azure Cloud Shell.

```azurepowershell-interactive
Install-Module -Name Az.Portal
```

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

## <a name="choose-a-specific-azure-subscription"></a>Välj en enskild Azure-prenumeration

Om du har flera Azure-prenumerationer väljer du lämplig prenumeration där resurserna ska faktureras. Välj en speciell prenumeration med cmdleten [set-AzContext](https://docs.microsoft.com/powershell/module/az.accounts/set-azcontext) .

```azurepowershell-interactive
Set-AzContext -SubscriptionId 00000000-0000-0000-0000-000000000000
```

## <a name="define-variables"></a>Definiera variabler

Du kommer att använda flera olika informations delar flera gånger. Skapa variabler för att lagra informationen.

```azurepowershell-interactive
# Name of resource group used throughout this article
$resourceGroupName = 'myResourceGroup'

# Azure region
$location = 'centralus'

# Dashboard Title
$dashboardTitle = 'Simple VM Dashboard'

# Dashboard Name
$dashboardName = $dashboardTitle -replace '\s'

# Your Azure Subscription ID
$subscriptionID = (Get-AzContext).Subscription.Id

# Name of test VM
$vmName = 'SimpleWinVM'
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en [Azure-resurs grupp](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) med cmdlet: en [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup) . En resursgrupp är en logisk container där Azure-resurser distribueras och hanteras som en grupp.

I följande exempel skapas en resurs grupp baserat på namnet i `$resourceGroupName` variabeln i den region som anges i `$location` variabeln.

```azurepowershell-interactive
New-AzResourceGroup -Name $resourceGroupName -Location $location
```

## <a name="create-a-virtual-machine"></a>Skapa en virtuell dator

Den instrument panel som du skapar i nästa del av den här snabb starten kräver en befintlig virtuell dator. Skapa en virtuell dator genom att följa dessa steg.

Lagra inloggnings uppgifter för den virtuella datorn i en variabel. Lösen ordet måste vara komplext. Detta är ett nytt användar namn och lösen ord. Det är till exempel inte det konto som du använder för att logga in på Azure. Mer information finns i [användar namn krav](../virtual-machines/windows/faq.md#what-are-the-username-requirements-when-creating-a-vm) och [lösen ords krav](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).

```azurepowershell-interactive
$Cred = Get-Credential
```

Skapa den virtuella datorn.

```azurepowershell-interactive
$AzVmParams = @{
  ResourceGroupName = $resourceGroupName
  Name = $vmName
  Location = $location
  Credential = $Cred
}
New-AzVm @AzVmParams
```

Distributionen av virtuella datorer startar nu och tar vanligt vis några minuter att slutföra. När distributionen är klar går du vidare till nästa avsnitt.

## <a name="download-the-dashboard-template"></a>Hämta instrument panelens mall

Eftersom Azure-instrumentpaneler är resurser kan de representeras som JSON. Följande kod laddar ned en JSON-representation av ett exempel på en instrument panel. Mer information finns i [strukturen för Azure-instrumentpaneler](/azure/azure-portal/azure-portal-dashboards-structure).

```azurepowershell-interactive
$myPortalDashboardTemplateUrl = 'https://raw.githubusercontent.com/Azure/azure-docs-powershell-samples/master/azure-portal/portal-dashboard-template-testvm.json'

$myPortalDashboardTemplatePath = "$env:TEMP\portal-dashboard-template-testvm.json"

Invoke-WebRequest -Uri $myPortalDashboardTemplateUrl -OutFile $myPortalDashboardTemplatePath -UseBasicParsing
```

## <a name="customize-the-template"></a>Anpassa mallen

Anpassa den nedladdade mallen genom att köra följande kod.

```azurepowershell
$Content = Get-Content -Path $myPortalDashboardTemplatePath -Raw
$Content = $Content -replace '<subscriptionID>', $subscriptionID
$Content = $Content -replace '<rgName>', $resourceGroupName
$Content = $Content -replace '<vmName>', $vmName
$Content = $Content -replace '<dashboardTitle>', $dashboardTitle
$Content = $Content -replace '<location>', $location
$Content | Out-File -FilePath $myPortalDashboardTemplatePath -Force
```

Mer information finns i [referens för Microsoft Portal-instrumentpaneler](/azure/templates/microsoft.portal/dashboards).

## <a name="deploy-the-dashboard-template"></a>Distribuera instrument panelens mall

Du kan använda `New-AzPortalDashboard` cmdleten som är en del av modulen AZ. Portal för att distribuera mallen direkt från PowerShell.

```azurepowershell
$DashboardParams = @{
  DashboardPath = $myPortalDashboardTemplatePath
  ResourceGroupName = $resourceGroupName
  DashboardName = $dashboardName
}
New-AzPortalDashboard @DashboardParams
```

## <a name="review-the-deployed-resources"></a>Granska de distribuerade resurserna

Kontrol lera att instrument panelen har skapats.

```azurepowershell
Get-AzPortalDashboard -Name $dashboardName -ResourceGroupName $resourceGroupName
```

Kontrol lera att du kan se data om den virtuella datorn i Azure Portal.

1. Öppna Azure-portalen och välj **Instrumentpanel**.

   ![Azure Portal navigering till instrument panelen](media/quickstart-portal-dashboard-powershell/navigate-to-dashboards.png)

1. På instrument panels sidan väljer du **enkel instrument panel för virtuell dator**.

   ![Navigera till en enkel instrument panel för virtuell dator](media/quickstart-portal-dashboard-powershell/select-simple-vm-dashboard.png)

1. Granska instrumentpanelen. Du kan se att en del av innehållet är statiskt, men det finns även diagram som visar prestanda för den virtuella datorn.

   ![Granska enkel instrument panel för virtuell dator](media/quickstart-portal-dashboard-powershell/review-simple-vm-dashboard.png)

## <a name="clean-up-resources"></a>Rensa resurser

Om du vill ta bort den virtuella datorn och tillhör ande instrument panel tar du bort resurs gruppen som innehåller dem.

> [!CAUTION]
> I följande exempel tas den angivna resurs gruppen och alla resurser som ingår i den bort.
> Om resurser utanför omfånget för den här artikeln finns i den angivna resurs gruppen kommer de också att tas bort.

```azurepowershell-interactive
Remove-AzResourceGroup -Name $resourceGroupName
```

## <a name="next-steps"></a>Nästa steg

Mer information om de cmdletar som finns i modulen AZ. Portal PowerShell finns i:

> [!div class="nextstepaction"]
> [Microsoft Azure PowerShell: portalens instrument panels cmdletar](https://docs.microsoft.com/powershell/module/Az.Portal/)
