---
title: Konvertera en äldre Exchange-peering till en Azure-resurs med hjälp av PowerShell
titleSuffix: Azure
description: Konvertera en äldre Exchange-peering till en Azure-resurs med hjälp av PowerShell
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 769522238939fe8ee786ae51a1b3b6051604451e
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89071677"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-powershell"></a>Konvertera en äldre Exchange-peering till en Azure-resurs med hjälp av PowerShell

Den här artikeln beskriver hur du konverterar en befintlig äldre Exchange-peering till en Azure-resurs med hjälp av PowerShell-cmdletar.

Om du vill kan du slutföra den här guiden med hjälp av Azure [Portal](howto-legacy-exchange-portal.md).

## <a name="before-you-begin"></a>Innan du börjar
* Granska [kraven](prerequisites.md) och [genom gången av Exchange-peering](walkthrough-exchange-all.md) innan du påbörjar konfigurationen.

### <a name="work-with-azure-powershell"></a>Arbeta med Azure PowerShell
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Konvertera en äldre Exchange-peering till en Azure-resurs

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Logga in på ditt Azure-konto och välj din prenumeration
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-legacy-exchange-peering-for-conversion"></a><a name= get></a>Hämta äldre Exchange-peering för konvertering
Det här exemplet visar hur du hämtar äldre Exchange-peering på platsen för Seattle-peering:

```powershell
$legacyPeering = Get-AzLegacyPeering -Kind Exchange -PeeringLocation "Seattle"
$legacyPeering
```

Svaret ser ut ungefär som i följande exempel:
```powershell
    Kind                     : Exchange
    PeeringLocation          : Seattle
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```

### <a name="convert-legacy-peering"></a>Konvertera äldre peering
Det här kommandot kan användas för att konvertera äldre Exchange-peering till en Azure-resurs:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> När du konverterar äldre peering till en Azure-resurs stöds inte ändringar.
&nbsp;

Detta exempel svar visar när end-to-end-etableringen har slutförts:

```powershell
    Name                     : SeattleExchangePeering
    Kind                     : Exchange
    Sku                      : Basic_Exchange_Free
    PeeringLocation          : Seattle
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```
## <a name="additional-resources"></a>Ytterligare resurser
Du kan få detaljerade beskrivningar av alla parametrar genom att köra följande kommando:

```powershell
Get-Help Get-AzPeering -detailed
```
Mer information finns i [vanliga frågor och svar om Internet-peering](faqs.md).

## <a name="next-steps"></a>Nästa steg

* [Skapa eller ändra en Exchange-peering med hjälp av PowerShell](howto-exchange-powershell.md)
