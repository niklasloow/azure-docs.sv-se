---
title: inkludera fil
titleSuffix: Azure
description: inkludera fil
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: include
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 34a23ce76ed0e9285a686073e1cbeb95347f7b7d
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "81678603"
---
Kör kommandot **Get-AzPeering** för att hämta listan över peer-kopplingar.

```powershell
Get-AzPeering ResourceGroupName "PeeringResourceGroup" -Name "SeattleExchangePeering"
```

Detta exempel svar visar när end-to-end-etableringen har slutförts.

```powershell
    Name                     : SeattleExchangePeering
    Sku                      : Basic_Exchange_Free
    Kind                     : Exchange
    PeeringLocation          : Seattle
    ProvisioningState        : Succeeded
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
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.101
    MicrosoftIPv4Address     : 10.21.31.51
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:101
    MicrosoftIPv6Address     : fe01::3e:51
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```