---
title: Importera containeravbildningar
description: 'Importera behållar avbildningar till ett Azure Container Registry med hjälp av Azure API: er, utan att behöva köra Docker-kommandon.'
ms.topic: article
ms.date: 08/17/2020
ms.openlocfilehash: 66c3a8b19e2288c1f8720dd4fe79f348a11f052e
ms.sourcegitcommit: d18a59b2efff67934650f6ad3a2e1fe9f8269f21
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88660503"
---
# <a name="import-container-images-to-a-container-registry"></a>Importera behållar avbildningar till ett behållar register

Du kan enkelt importera (kopiera) behållar avbildningar till ett Azure Container Registry utan att använda Docker-kommandon. Du kan till exempel importera bilder från ett utvecklings register till ett produktions register eller kopiera bas avbildningar från ett offentligt register.

Azure Container Registry hanterar ett antal vanliga scenarier för att kopiera avbildningar från ett befintligt register:

* Importera från ett offentligt register

* Importera från ett annat Azure Container Registry i samma eller en annan Azure-prenumeration

* Importera från ett icke-Azure privat behållar register

Avbildnings import till ett Azure Container Registry har följande fördelar jämfört med Docker CLI-kommandon:

* Eftersom klient miljön inte behöver en lokal Docker-installation importerar du en behållar avbildning, oavsett vilken typ av operativ system som stöds.

* När du importerar avbildningar med flera arkitekturer (till exempel officiella Docker-avbildningar) kopieras avbildningar för alla arkitekturer och plattformar som anges i manifest listan.

* Åtkomst till käll-och mål registren behöver inte använda registrens offentliga slut punkter.

Den här artikeln kräver att du kör Azure CLI i Azure Cloud Shell eller lokalt (version 2.0.55 eller senare rekommenderas) för att importera behållar avbildningar. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI][azure-cli].

> [!NOTE]
> Om du behöver distribuera identiska behållar avbildningar i flera Azure-regioner stöder Azure Container Registry också [geo-replikering](container-registry-geo-replication.md). Genom geo-replikering av ett register (Premium service nivå krävs) kan du hantera flera regioner med identiska bild-och taggnamn från ett enda register.
>

## <a name="prerequisites"></a>Förutsättningar

Om du inte redan har ett Azure Container Registry skapar du ett register. Anvisningar finns i [snabb start: skapa ett privat behållar register med hjälp av Azure CLI](container-registry-get-started-azure-cli.md).

Om du vill importera en avbildning till ett Azure Container Registry måste identiteten ha Skriv behörighet till mål registret (minst deltagar rollen eller en anpassad roll som tillåter åtgärden importImage). Se [Azure Container Registry roller och behörigheter](container-registry-roles.md#custom-roles). 

## <a name="import-from-a-public-registry"></a>Importera från ett offentligt register

### <a name="import-from-docker-hub"></a>Importera från Docker Hub

Använd exempelvis kommandot [AZ ACR import][az-acr-import] för att importera avbildningen av flera arkitekturer `hello-world:latest` från Docker-hubb till ett register med namnet *unregistry*. Eftersom `hello-world` är en officiell avbildning från Docker Hub finns den här avbildningen i standard `library` lagrings platsen. Inkludera databas namnet och eventuellt en tagg i värdet för `--source` parametern image. (Du kan också identifiera en avbildning med dess manifest sammandrag i stället för efter tagg, som garanterar en viss version av en avbildning.)
 
```azurecli
az acr import \
  --name myregistry \
  --source docker.io/library/hello-world:latest \
  --image hello-world:latest
```

Du kan kontrol lera att flera manifest är associerade med den här avbildningen genom att köra `az acr repository show-manifests` kommandot:

```azurecli
az acr repository show-manifests \
  --name myregistry \
  --repository hello-world
```

I följande exempel importeras en offentlig avbildning från `tensorflow` lagrings platsen i Docker Hub:

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/tensorflow/tensorflow:latest-gpu \
  --image tensorflow:latest-gpu
```

### <a name="import-from-microsoft-container-registry"></a>Importera från Microsoft Container Registry

Importera till exempel `ltsc2019` Windows Server Core-avbildningen från `windows` lagrings platsen i Microsoft container Registry.

```azurecli
az acr import \
--name myregistry \
--source mcr.microsoft.com/windows/servercore:ltsc2019 \
--image servercore:ltsc2019
```

## <a name="import-from-another-azure-container-registry"></a>Importera från ett annat Azure Container Registry

Du kan importera en avbildning från ett annat Azure Container Registry med hjälp av integrerade Azure Active Directory behörigheter.

* Din identitet måste ha Azure Active Directory behörighet att läsa från käll registret (läsar roll) och för att kunna importera till mål registret (deltagar rollen eller en [anpassad roll](container-registry-roles.md#custom-roles) som tillåter åtgärden importImage).

* Registret kan finnas i samma eller en annan Azure-prenumeration i samma Active Directory klient.

* [Offentlig åtkomst](container-registry-access-selected-networks.md#disable-public-network-access) till käll registret kan inaktive ras. Om offentlig åtkomst är inaktive rad anger du käll registret per resurs-ID i stället för inloggnings Server namnet för registret.

### <a name="import-from-a-registry-in-the-same-subscription"></a>Importera från ett register i samma prenumeration

Importera till exempel `aci-helloworld:latest` avbildningen från ett käll register *mysourceregistry* till *registret* i samma Azure-prenumeration.

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld:latest \
  --image aci-helloworld:latest
```

I följande exempel importeras `aci-helloworld:latest` avbildningen till *registret* från en *mysourceregistry* för käll registret där åtkomst till registrets offentliga slut punkt är inaktive rad. Ange resurs-ID för käll registret med `--registry` parametern. Observera att `--source` parametern endast anger käll databasen och taggen, inte namnet på inloggnings servern för registret.

```azurecli
az acr import \
  --name myregistry \
  --source aci-helloworld:latest \
  --image aci-helloworld:latest \
  --registry /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sourceResourceGroup/providers/Microsoft.ContainerRegistry/registries/mysourceregistry
```

I följande exempel importeras en avbildning av manifest sammandrag (SHA-256-hash, som visas `sha256:...` ) i stället för taggen:

```azurecli
az acr import \
  --name myregistry \
  --source mysourceregistry.azurecr.io/aci-helloworld@sha256:123456abcdefg 
```

### <a name="import-from-a-registry-in-a-different-subscription"></a>Importera från ett register i en annan prenumeration

I följande exempel finns *mysourceregistry* i en annan prenumeration än *registret* i samma Active Directory-klient. Ange resurs-ID för käll registret med `--registry` parametern. Observera att `--source` parametern endast anger käll databasen och taggen, inte namnet på inloggnings servern för registret.

```azurecli
az acr import \
  --name myregistry \
  --source samples/aci-helloworld:latest \
  --image aci-hello-world:latest \
  --registry /subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/sourceResourceGroup/providers/Microsoft.ContainerRegistry/registries/mysourceregistry
```

### <a name="import-from-a-registry-using-service-principal-credentials"></a>Importera från ett register med autentiseringsuppgifter för tjänstens huvud namn

Om du vill importera från ett register som du inte kan komma åt med Active Directory behörigheter kan du använda autentiseringsuppgifterna för tjänstens huvud namn (om de är tillgängliga). Ange appID och lösen ordet för en Active Directory [tjänstens huvud namn](container-registry-auth-service-principal.md) som har ACRPull åtkomst till käll registret. Att använda ett huvud namn för tjänsten är användbart för att bygga system och andra obevakade system som behöver importera avbildningar till registret.

```azurecli
az acr import \
  --name myregistry \
  --source sourceregistry.azurecr.io/sourcerrepo:tag \
  --image targetimage:tag \
  --username <SP_App_ID> \
  –-password <SP_Passwd>
```

## <a name="import-from-a-non-azure-private-container-registry"></a>Importera från ett icke-Azure privat behållar register

Importera en avbildning från ett privat register genom att ange autentiseringsuppgifter som aktiverar åtkomst till registret. Hämta till exempel en avbildning från ett privat Docker-register: 

```azurecli
az acr import \
  --name myregistry \
  --source docker.io/sourcerepo/sourceimage:tag \
  --image sourceimage:tag \
  --username <username> \
  --password <password>
```

## <a name="next-steps"></a>Nästa steg

I den här artikeln har du lärt dig hur du importerar behållar avbildningar till ett Azure Container Registry från ett offentligt register eller ett annat privat register. Ytterligare alternativ för avbildnings import finns i kommando referens för [AZ ACR import][az-acr-import] . 


<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-import]: /cli/azure/acr#az-acr-import
[azure-cli]: /cli/azure/install-azure-cli
