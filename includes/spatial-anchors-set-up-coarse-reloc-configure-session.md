---
author: bucurb
ms.author: bobuc
ms.date: 09/18/2019
ms.service: azure-spatial-anchors
ms.topic: include
ms.openlocfilehash: 574003a150ef294aa6a2ebc035fe48cf877dec66
ms.sourcegitcommit: c5021f2095e25750eb34fd0b866adf5d81d56c3a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/25/2020
ms.locfileid: "76545232"
---
## <a name="configure-the-cloud-spatial-anchor-session"></a>Konfigurera sessionen för moln rums ankare

Vi tar hand om konfigureringen av molnets spatiala Anchor-session härnäst. På den första raden ställer vi in sensor leverantören i sessionen. Från och med nu kommer alla ankare som vi skapar under sessionen att associeras med en uppsättning sensor avläsningar. Härnäst instansierar vi en nära enhet som letar efter kriterier och initierar den för att matcha program kraven. Slutligen instruerar vi sessionen att använda sensor data när du söker efter ankare genom att skapa en bevakare från vårt nära enhets kriterium.