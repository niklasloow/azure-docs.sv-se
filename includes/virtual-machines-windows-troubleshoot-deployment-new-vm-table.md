---
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 14abae6f6f72d724fffb1ccaa12f56fb6976f7a1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "67187343"
---
I följande tabell visas de möjliga uppladdnings-och infångnings kombinationerna av OS-avbildningar för Windows generaliserad (generation.) och specialiserad (spec.). De kombinationer som ska bearbetas utan fel anges med ett Y, och de som ska utlösa fel anges av en N. Orsakerna och lösningarna för de olika fel som du kommer att köra anges under tabellen.

| Operativsystem | Ladda upp specifikation. | Ladda upp gen. | Infångnings specifikation. | Avbilda gen. |
| --- | --- | --- | --- | --- |
| Windows gen. |N<sup>1</sup> |Y |N<sup>3</sup> |Y |
| Windows-specifikation. |Y |N<sup>2</sup> |Y |N<sup>4</sup> |

