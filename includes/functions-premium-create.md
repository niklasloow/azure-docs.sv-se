---
title: inkludera fil
description: inkludera fil
services: functions
author: jeffhollan
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2020
ms.author: jehollan, glenga
ms.custom: include file
ms.openlocfilehash: 4dc85988d904fdec72e1e6d92f03582a2a8f1427
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "85121334"
---
1. I menyn i Azure-portalen eller på sidan **Start** väljer du **Skapa en resurs**.

1. På den **nya** sidan väljer du **Compute**  >  **Funktionsapp**.

1. På sidan **grundläggande** inställningar använder du funktionen appinställningar som anges i följande tabell:

    | Inställning      | Föreslaget värde  | Beskrivning |
    | ------------ | ---------------- | ----------- |
    | **Prenumeration** | Din prenumeration | Prenumerationen som den nya funktionsappen skapas under. |
    | **[Resurs grupp](../articles/azure-resource-manager/management/overview.md)** |  *myResourceGroup* | Namnet på den nya resursgrupp där du vill skapa funktionsappen. |
    | **Funktionsappens namn** | Globalt unikt namn | Namn som identifierar din nya funktionsapp. Giltiga tecken är `a-z` (skiftlägesokänsligt), `0-9` och `-`.  |
    |**Publicera**| Kod | Alternativ för att publicera kodfiler eller en Docker-container. |
    | **Körningsstack** | Önskat språk | Välj en körning som stöder det funktionsprogrammeringsspråk som du föredrar. Välj **.NET** för C#- och F#-funktioner. |
    |**Region**| Önskad region | Välj en [region](https://azure.microsoft.com/regions/) nära dig eller nära andra tjänster som dina funktioner kommer åt. |

    ![Sidan Grundinställningar](./media/functions-premium-create/function-app-create-basics.png)

1. Välj **Nästa: värd**. Ange följande inställningar på sidan **värd** :

    | Inställning      | Föreslaget värde  | Beskrivning |
    | ------------ | ---------------- | ----------- |
    | **[Lagringskonto](../articles/storage/common/storage-account-create.md)** |  Globalt unikt namn |  Skapa ett lagringskonto som används av din funktionsapp. Namnet på ett lagringskonto måste vara mellan 3 och 24 tecken långt och får endast innehålla siffror och gemener. Du kan också använda ett befintligt konto som måste uppfylla kraven för [lagrings kontot](../articles/azure-functions/functions-scale.md#storage-account-requirements). |
    |**Operativsystem**| Önskat operativ system | Ett operativ system är i förväg valt för dig baserat på ditt val av körnings stack, men du kan ändra inställningen om det behövs. Python stöds endast på Linux. |
    | **[Planera](../articles/azure-functions/functions-scale.md)** | Premium | Värdplan som definierar hur resurser allokeras till din funktionsapp. Välj **Premium**. Som standard skapas en ny App Service plan. Standard- **SKU och-storlek** är **EP1**, där EP står för _elastisk Premium_. Mer information finns i [listan över Premium-SKU: er](../articles/azure-functions/functions-premium-plan.md#available-instance-skus).<br/>När du kör JavaScript-funktioner i en Premium-plan bör du välja en instans som har färre virtuella processorer. Mer information finns i [Välj Premium-](../articles/azure-functions/functions-reference-node.md#considerations-for-javascript-functions)prenumerationer med en kärna.  |

    ![Värd sida](./media/functions-premium-create/function-app-premium-create-hosting.png)

1. Välj **Nästa: övervakning**. Ange följande inställningar på sidan **övervakning** :

    | Inställning      | Föreslaget värde  | Beskrivning |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](../articles/azure-functions/functions-monitoring.md)** | Default | Skapar en Application Insights resurs av samma *app-namn* i den närmaste region som stöds. Genom att utöka den här inställningen kan du ändra det **nya resurs namnet** eller välja en annan plats i ett [Azure-geografiskt](https://azure.microsoft.com/global-infrastructure/geographies/) **område** för att lagra dina data. |

    ![Sidan övervakning](./media/functions-create-function-app-portal/function-app-create-monitoring.png)

1. Välj **Granska + skapa** för att granska konfigurations valen för appen.

1. På sidan **Granska + skapa** granskar du inställningarna och väljer sedan **skapa** för att etablera och distribuera Function-appen.

1. Välj **aviserings** ikonen i det övre högra hörnet i portalen och titta efter ett meddelande om att **distributionen har slutförts** .

1. Välj **Gå till resurs** att visa den nya funktionsappen. Du kan också välja **Fäst vid instrument panelen**. Genom att fästa blir det enklare att återgå till den här funktions program resursen från instrument panelen.

    ![Distributionsmeddelande](./media/functions-premium-create/function-app-create-notification2.png)
