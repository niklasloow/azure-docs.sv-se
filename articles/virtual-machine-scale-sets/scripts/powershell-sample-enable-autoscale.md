---
title: Azure PowerShell exempel – Aktivera värdbaserad autoskalning
description: Det här skriptet skapar en VM-skalningsuppsättning som kör Windows Server 2016 och använder värdbaserade mått för att automatiskt skala allteftersom CPU-belastningen ändras.
author: ju-shim
ms.author: jushiman
ms.topic: sample
ms.service: virtual-machine-scale-sets
ms.subservice: autoscale
ms.date: 06/25/2020
ms.custom: avverma, devx-track-azurepowershell
ms.openlocfilehash: 9530f34ee919547049df06fb0974971c3ba4b2a9
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89079633"
---
# <a name="automatically-scale-a-virtual-machine-scale-set-with-powershell"></a>Skala automatiskt en VM-skalningsuppsättning med PowerShell
Det här skriptet skapar en VM-skalningsuppsättning som kör Windows Server 2016 och använder värdbaserade mått för att automatiskt skala allteftersom CPU-belastningen ändras.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="sample-script"></a>Exempelskript


[!code-powershell[main](../../../powershell_scripts/virtual-machine-scale-sets/auto-scale-host-metrics/auto-scale-host-metrics.ps1 "Automatically scale a virtual machine scale set")]

## <a name="clean-up-deployment"></a>Rensa distribution
Kör följande kommando för att ta bort resursgruppen, skalningsuppsättningen och alla relaterade resurser.

```powershell
Remove-AzResourceGroup -Name myResourceGroup
```

## <a name="script-explanation"></a>Förklaring av skript
Det här skriptet använder följande kommandon för att skapa distributionen. Varje post i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Anteckningar |
|---|---|
| [New-AzVmss](/powershell/module/az.compute/new-azvmss) | Skapar VM-skalningsuppsättningen och alla stödresurser, inklusive virtuellt nätverk, lastbalansering och NAT-regler. |
| [Get-AzVmss](/powershell/module/az.compute/get-azvmss) | Hämtar information om en VM-skalningsuppsättning. |
| [Add-AzVmssExtension](/powershell/module/az.compute/add-azvmssextension) | Lägger till ett virtuellt datortillägg för att det anpassade skriptet ska installera ett grundläggande webbprogram. |
| [Update-AzVmss](/powershell/module/az.compute/update-azvmss) | Uppdaterar modellen för VM-skalningsuppsättningen för att använda det virtuella datortillägget. |
| [Get-AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) | Hämtar information om den offentliga IP-adress som används av lastbalanseraren. |
| [Remove-AzResourceGroup](/powershell/module/az.resources/remove-azresourcegroup) | Tar bort en resursgrupp och alla resurser som ingår i gruppen. |

## <a name="next-steps"></a>Nästa steg
Mer information om Azure PowerShell-modulen finns i [Azure PowerShell-dokumentationen](/powershell/azure/).

