---
title: inkludera fil
description: inkludera fil
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 04/25/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 6e0612a017650f0c6e4c9f63d9a5fd097b0b92c4
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89303932"
---
## <a name="update-resources"></a>Uppdatera resurser

Det finns vissa begränsningar för vad som kan uppdateras. Följande objekt kan uppdateras: 

Galleri för delad avbildning:
- Beskrivning

Bild definition:
- Rekommenderad virtuella processorer
- Rekommenderat minne
- Beskrivning
- Datum för slut på livs längd

Avbildnings version:
- Antal regionala repliker
- Mål regioner
- Undantag från senaste
- Datum för slut på livs längd

Om du planerar att lägga till replik regioner ska du inte ta bort den hanterade käll avbildningen. Den käll hanterade avbildningen krävs för att replikera avbildnings versionen till ytterligare regioner. 

Om du vill uppdatera beskrivningen av ett galleri använder du [Update-AzGallery](https://docs.microsoft.com/powershell/module/az.compute/update-azgallery).

```azurepowershell-interactive
Update-AzGallery `
   -Name $gallery.Name ` 
   -ResourceGroupName $resourceGroup.Name
```

Det här exemplet visar hur du använder [Update-AzGalleryImageDefinition](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimagedefinition) för att uppdatera livs längds datumet för vår avbildnings definition.

```azurepowershell-interactive
Update-AzGalleryImageDefinition `
   -GalleryName $gallery.Name `
   -Name $galleryImage.Name `
   -ResourceGroupName $resourceGroup.Name `
   -EndOfLifeDate 01/01/2030
```

Det här exemplet visar hur du använder [Update-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimageversion) för att utesluta att den här avbildnings versionen används som den *senaste* avbildningen.

```azurepowershell-interactive
Update-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryName $gallery.Name `
   -Name $galleryVersion.Name `
   -ResourceGroupName $resourceGroup.Name `
   -PublishingProfileExcludeFromLatest
```

Det här exemplet illustrerar hur du använder [Update-AzGalleryImageVersion](https://docs.microsoft.com/powershell/module/az.compute/update-azgalleryimageversion) för att ta med den här avbildnings versionen som ska beaktas för den *senaste* avbildningen.

```azurepowershell-interactive
Update-AzGalleryImageVersion `
   -GalleryImageDefinitionName $galleryImage.Name `
   -GalleryName $gallery.Name `
   -Name $galleryVersion.Name `
   -ResourceGroupName $resourceGroup.Name `
   -PublishingProfileExcludeFromLatest:$false
```

## <a name="clean-up-resources"></a>Rensa resurser

När du tar bort resurser måste du börja med det sista objektet i de kapslade resurserna – avbildnings versionen. När du har tagit bort versioner kan du ta bort avbildnings definitionen. Du kan inte ta bort galleriet förrän alla resurser under den har tagits bort.

```azurepowershell-interactive
$resourceGroup = "myResourceGroup"
$gallery = "myGallery"
$imageDefinition = "myImageDefinition"
$imageVersion = "myImageVersion"

Remove-AzGalleryImageVersion `
   -GalleryImageDefinitionName $imageDefinition `
   -GalleryName $gallery `
   -Name $imageVersion `
   -ResourceGroupName $resourceGroup

Remove-AzGalleryImageDefinition `
   -ResourceGroupName $resourceGroup `
   -GalleryName $gallery `
   -GalleryImageDefinitionName $imageDefinition

Remove-AzGallery `
   -Name $gallery `
   -ResourceGroupName $resourceGroup

Remove-AzResourceGroup -Name $resourceGroup
```

