---
title: Skapa definition av hanterade program – Azure PowerShell
description: Innehåller ett Azure PowerShell skript exempel som skapar en definition för ett hanterat program i Azure-prenumerationen.
author: tfitzmac
ms.devlang: powershell
ms.topic: sample
ms.date: 10/27/2017
ms.author: tomfitz
ms.openlocfilehash: b605d63db09b81bfe73eca8f4abd7a38d2325004
ms.sourcegitcommit: 62717591c3ab871365a783b7221851758f4ec9a4
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/22/2020
ms.locfileid: "86056029"
---
# <a name="create-a-managed-application-definition-with-powershell"></a>Skapa en definition för ett hanterat program med PowerShell

Det här skriptet publicerar en definition för hanterade program i en tjänstkatalog.

[!INCLUDE [quickstarts-free-trial-note](../../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!INCLUDE [updated-for-az](../../../../includes/updated-for-az.md)]

[!code-powershell[main](../../../../powershell_scripts/managed-applications/create-definition/create-definition.ps1 "Create definition")]


## <a name="script-explanation"></a>Förklaring av skript

Det här skriptet använder följande kommando för att skapa definitionen för det hanterade programmet. Varje kommando i tabellen länkar till kommandospecifik dokumentation.

| Kommando | Kommentarer |
|---|---|
| [New-AzManagedApplicationDefinition](/powershell/module/az.resources/new-azmanagedapplicationdefinition) | Skapa en definition för det hanterade programmet. Ange det paket som innehåller filerna som krävs. |


## <a name="next-steps"></a>Nästa steg

* En introduktion till hanterade program finns i [Azure Managed Application overview](../overview.md) (Översikt över Azure Managed Application).
* Mer information om PowerShell finns i [Azure PowerShell-dokumentationen](/powershell/azure/get-started-azureps).
