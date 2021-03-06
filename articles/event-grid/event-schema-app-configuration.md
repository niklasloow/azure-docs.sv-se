---
title: Azure App konfiguration som Event Grid källa
description: I den här artikeln beskrivs hur du använder Azure App konfiguration som en Event Grid händelse källa. Det innehåller schemat och länkar till självstudier och instruktions artiklar.
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: bdd077c291bd1e1c441217740daf39c8bcaad732
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86117006"
---
# <a name="azure-app-configuration-as-an-event-grid-source"></a>Azure App konfiguration som en Event Grid källa
Den här artikeln innehåller egenskaper och schema för Azure App konfigurations händelser. En introduktion till händelse scheman finns i [Azure Event Grid händelse schema](event-schema.md). Du får också en lista med snabb starter och självstudier för att använda Azure App konfiguration som en händelse källa.

## <a name="event-grid-event-schema"></a>Event Grid-händelseschema

### <a name="available-event-types"></a>Tillgängliga händelse typer

Azure App konfiguration avger följande händelse typer:

| Händelsetyp | Description |
| ---------- | ----------- |
| Microsoft. AppConfiguration. KeyValueModified | Utlöses när ett nyckel värde skapas eller ersätts. |
| Microsoft. AppConfiguration. KeyValueDeleted | Utlöses när ett nyckel värde tas bort. |

### <a name="example-event"></a>Exempel händelse

I följande exempel visas schemat för en händelse med nyckel värdes ändringar: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueModified",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```

Schemat för en händelse som tar bort nyckel värde liknar: 

```json
[{
  "id": "84e17ea4-66db-4b54-8050-df8f7763f87b",
  "topic": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/testrg/providers/microsoft.appconfiguration/configurationstores/contoso",
  "subject": "https://contoso.azconfig.io/kv/Foo?label=FizzBuzz",
  "data": {
    "key": "Foo",
    "label": "FizzBuzz",
    "etag": "FnUExLaj2moIi4tJX9AXn9sakm0"
  },
  "eventType": "Microsoft.AppConfiguration.KeyValueDeleted",
  "eventTime": "2019-05-31T20:05:03Z",
  "dataVersion": "1",
  "metadataVersion": "1"
}]
```
 
### <a name="event-properties"></a>Händelse egenskaper

En händelse har följande data på översta nivån:

| Egenskap | Typ | Description |
| -------- | ---- | ----------- |
| ämne | sträng | Fullständig resurs Sök väg till händelse källan. Det går inte att skriva till det här fältet. Event Grid ger det här värdet. |
| motiv | sträng | Utgivardefinierad sökväg till händelseobjektet. |
| Händelsetyp | sträng | En av de registrerade händelsetyperna för den här händelsekällan. |
| Händelsetid | sträng | Tiden då händelsen genereras baserat på providerns UTC-tid. |
| ID | sträng | Unikt ID för händelsen. |
| data | objekt | Händelse data för app-konfiguration. |
| Dataversion | sträng | Dataobjektets schemaversion. Utgivaren definierar schemaversion. |
| Metadataversion | sträng | Schemaversionen av händelsens metadata. Event Grid definierar schemat för de översta egenskaperna. Event Grid ger det här värdet. |

Data-objektet har följande egenskaper:

| Egenskap | Typ | Description |
| -------- | ---- | ----------- |
| key | sträng | Nyckeln till det nyckel värde som ändrades eller togs bort. |
| etikett | sträng | Etiketten, om det finns, för det nyckel värde som ändrades eller togs bort. |
| etag | sträng | För `KeyValueModified` etag för det nya nyckel värdet. För `KeyValueDeleted` etag för det nyckel värde som har tagits bort. |

## <a name="tutorials-and-how-tos"></a>Självstudier och instruktioner

|Titel | Beskrivning |
|---------|---------|
| [Reagera på Azure App konfigurations händelser med Event Grid](../azure-app-configuration/concept-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Översikt över att integrera Azure App-konfiguration med Event Grid. |
| [Snabb start: dirigera Azure App konfigurations händelser till en anpassad webb slut punkt med Azure CLI](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json) | Visar hur du använder Azure CLI för att skicka Azure App konfigurations händelser till en webhook. |

## <a name="next-steps"></a>Nästa steg

* En introduktion till Azure Event Grid finns i [Vad är event Grid?](overview.md)
* Mer information om hur du skapar en Azure Event Grid-prenumeration finns i [Event Grid prenumerations schema](subscription-creation-schema.md).
* En introduktion till att arbeta med Azure App konfigurations händelser finns i [dirigera Azure App konfigurations händelser – Azure CLI](../azure-app-configuration/howto-app-configuration-event.md?toc=%2fazure%2fevent-grid%2ftoc.json). 