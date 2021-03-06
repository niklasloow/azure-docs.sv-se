---
title: Snabb start – skapa en Azure API Management-instans med PowerShell | Microsoft Docs
description: Skapa en ny Azure API Management-instans med hjälp av Azure PowerShell.
services: api-management
documentationcenter: ''
author: vladvino
ms.service: api-management
ms.topic: quickstart
ms.custom: mvc
ms.date: 09/14/2020
ms.author: apimpm
ms.openlocfilehash: eb2c42d26a85a07518a018ba5b8817f13d3cd17f
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707074"
---
# <a name="quickstart-create-a-new-azure-api-management-service-instance-by-using-powershell"></a>Snabb start: skapa en ny Azure API Management Service-instans med hjälp av PowerShell

Azure API Management (APIM) hjälper organisationer att publicera API:er till externa partner och interna utvecklare så att de kan få ut maximalt av sina data och tjänster. API Management lägger grunden till ett effektivt API-program genom engagerade utvecklare, affärsinsikter, analyser, hög säkerhet och skydd. Med APIM kan du skapa och hantera moderna API-gatewayer för befintliga serverdeltjänster som ligger var som helst. Mer information finns i [Översikt](api-management-key-concepts.md).

I den här snabb starten beskrivs stegen för att skapa en ny API Management-instans med hjälp av Azure PowerShell-cmdletar.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda PowerShell lokalt behöver du ha version 1.0 eller senare av Azure PowerShell-modulen för den här självstudien. Kör `Get-Module -ListAvailable Az` för att hitta versionen. Om du behöver uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/install-Az-ps) (Installera Azure PowerShell-modul). Om du kör PowerShell lokalt måste du också köra `Connect-AzAccount` för att skapa en anslutning till Azure.

## <a name="create-resource-group"></a>Skapa resursgrupp

Skapa en Azure-resursgrupp med [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup). En resursgrupp är en logisk container där Azure-resurser distribueras och hanteras. 

Följande kommando skapar en resurs grupp med namnet *myResourceGroup* på platsen västra USA:

```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location WestUS
```

## <a name="create-an-api-management-service"></a>Skapa en API Management-tjänst

Nu när du har en resurs grupp kan du skapa en API Management tjänst instans. Skapa ett med hjälp av [New-AzApiManagement](/powershell/module/az.apimanagement/new-azapimanagement) och ange ett tjänst namn och utgivar information. Tjänst namnet måste vara unikt inom Azure.

I följande exempel används *myapim* som tjänst namn. Uppdatera namnet till ett unikt värde. Uppdatera också organisations namnet för API-utgivaren och e-postadressen för administratören för att ta emot meddelanden.

Som standard skapar kommandot instansen på nivån utvecklare, ett ekonomiskt alternativ för att utvärdera Azure API Management. Den här nivån är inte för produktions användning. Mer information om att skala API Management-nivåerna finns i avsnittet [Uppgradera och skala](upgrade-and-scale.md).

> [!NOTE]
> Detta är en tids krävande åtgärd. Det kan ta mellan 30 och 40 minuter att skapa och aktivera en API Management tjänst på den här nivån.

```azurepowershell-interactive
New-AzApiManagement -Name "myapim" -ResourceGroupName "myResourceGroup" `
  -Location "West US" -Organization "Contoso" -AdminEmail "admin@contoso.com" 
```

När kommandot returnerar kör [Get-AzApiManagement](/powershell/module/az.apimanagement/get-azapimanagement) för att visa egenskaperna för Azure API Management-tjänsten. Efter aktiveringen slutförs etablerings statusen och tjänst instansen har flera associerade URL: er. Exempel:

```azurepowershell-interactive
Get-AzApiManagement -Name "myapim" -ResourceGroupName "myResourceGroup" 
```

Exempel på utdata:

```console
PublicIPAddresses                     : {203.0.113.1}
PrivateIPAddresses                    :
Id                                    : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.ApiManagement/service/myapim
Name                                  : myapim
Location                              : West US
Sku                                   : Developer
Capacity                              : 1
CreatedTimeUtc                        : 9/9/2020 9:07:43 PM
ProvisioningState                     : Succeeded
RuntimeUrl                            : https://myapim.azure-api.net
RuntimeRegionalUrl                    : https://myapi-westus-01.regional.azure-api.net
PortalUrl                             : https://myapim.portal.azure-api.net
DeveloperPortalUrl                    : https://myapim.developer.azure-api.net
ManagementApiUrl                      : https://myapim.management.azure-api.net
ScmUrl                                : https://myapim.scm.azure-api.net
PublisherEmail                        : admin@contoso.com
OrganizationName                      : Contoso
NotificationSenderEmail               : apimgmt-noreply@mail.windowsazure.com
VirtualNetwork                        :
VpnType                               : None
PortalCustomHostnameConfiguration     :
ProxyCustomHostnameConfiguration      : {myapim.azure-api.net}
ManagementCustomHostnameConfiguration :
ScmCustomHostnameConfiguration        :
DeveloperPortalHostnameConfiguration  :
SystemCertificates                    :
Tags                                  : {}
AdditionalRegions                     : {}
SslSetting                            : Microsoft.Azure.Commands.ApiManagement.Models.PsApiManagementSslSetting
Identity                              :
EnableClientCertificate               :
ResourceGroupName                     : myResourceGroup

```

När du har distribuerat din API Management tjänst instans är du redo att använda den. Börja med självstudien för att [Importera och publicera ditt första API](import-and-publish.md).

## <a name="clean-up-resources"></a>Rensa resurser

När den inte längre behövs kan du använda kommandot [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) för att ta bort resursgruppen och alla relaterade resurser.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Importera och publicera ditt första API](import-and-publish.md)
