---
title: Azure-resurs grupp som en Event Grid källa
description: Beskriver de egenskaper som har angetts för resurs grupps händelser med Azure Event Grid
ms.topic: conceptual
ms.date: 07/07/2020
ms.openlocfilehash: ed01bfdb67d9b8a3dd5875ec3fd8c6edf8922520
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/08/2020
ms.locfileid: "86105922"
---
# <a name="azure-resource-group-as-an-event-grid-source"></a>Azure-resurs grupp som en Event Grid källa

Den här artikeln innehåller egenskaper och schema för resurs grupps händelser.En introduktion till händelse scheman finns i [Azure Event Grid händelse schema](event-schema.md).

Azure-prenumerationer och resurs grupper genererar samma händelse typer. Händelse typerna är relaterade till resurs ändringar eller åtgärder. Den främsta skillnaden är att resurs grupper genererar händelser för resurser i resurs gruppen och Azure-prenumerationer genererar händelser för resurser i prenumerationen.

Resurs händelser skapas för åtgärderna skicka, korrigera, skicka och ta bort `management.azure.com` . GET-åtgärder skapar inte händelser. Åtgärder som skickas till data planet (som `myaccount.blob.core.windows.net` ) skapar inte händelser. Åtgärds händelser innehåller händelse data för åtgärder som att Visa nycklar för en resurs.

När du prenumererar på händelser för en resurs grupp tar slut punkten emot alla händelser för resurs gruppen. Händelserna kan innehålla händelser som du vill se, till exempel uppdatering av en virtuell dator, men även händelser som kanske inte är viktiga för dig, till exempel att skriva en ny post i distributions historiken. Du kan ta emot alla händelser vid slut punkten och skriva kod som bearbetar de händelser som du vill hantera. Du kan också ange ett filter när du skapar händelse prenumerationen.

Om du vill hantera händelser program mässigt kan du sortera händelser genom att titta på `operationName` värdet. Till exempel kan händelse slut punkten bara bearbeta händelser för åtgärder som är lika med `Microsoft.Compute/virtualMachines/write` eller `Microsoft.Storage/storageAccounts/write` .

Händelsens ämne är resurs-ID för den resurs som är mål för åtgärden. För att filtrera händelser för en resurs, ange resurs-ID när du skapar händelse prenumerationen.  Om du vill filtrera efter en resurs typ använder du ett värde i följande format:`/subscriptions/<subscription-id>/resourcegroups/<resource-group>/providers/Microsoft.Compute/virtualMachines`


## <a name="event-grid-event-schema"></a>Event Grid-händelseschema

### <a name="available-event-types"></a>Tillgängliga händelse typer

Resurs grupper genererar hanterings händelser från Azure Resource Manager, till exempel när en virtuell dator skapas eller om ett lagrings konto tas bort.

| Händelsetyp | Description |
| ---------- | ----------- |
| Microsoft. Resources. ResourceActionCancel | Utlöses när åtgärd för resurs avbryts. |
| Microsoft. Resources. ResourceActionFailure | Utlöses när en åtgärd på resursen Miss lyckas. |
| Microsoft. Resources. ResourceActionSuccess | Utlöses när åtgärden för resursen slutförs. |
| Microsoft. Resources. ResourceDeleteCancel | Utlöses när borttagnings åtgärden avbryts. Den här händelsen inträffar när en mall för distribution avbryts. |
| Microsoft. Resources. ResourceDeleteFailure | Utlöses när borttagnings åtgärden Miss lyckas. |
| Microsoft. Resources. ResourceDeleteSuccess | Utlöses när borttagnings åtgärden slutfördes. |
| Microsoft. Resources. ResourceWriteCancel | Utlöses när åtgärden Skapa eller uppdatera avbryts. |
| Microsoft. Resources. ResourceWriteFailure | Utlöses när Create eller Update-åtgärden Miss lyckas. |
| Microsoft. Resources. ResourceWriteSuccess | Utlöses när åtgärden Skapa eller uppdatera lyckades. |

### <a name="example-event"></a>Exempel händelse

I följande exempel visas schemat för en **ResourceWriteSuccess** -händelse. Samma schema används för **ResourceWriteFailure** -och **ResourceWriteCancel** -händelser med olika värden för `eventType` .

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceWriteSuccess",
  "eventTime": "2018-07-19T18:38:04.6117357Z",
  "id": "4db48cba-50a2-455a-93b4-de41a3b5b7f6",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/write",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "{expiration}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourcegroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/write",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

I följande exempel visas schemat för en **ResourceDeleteSuccess** -händelse. Samma schema används för **ResourceDeleteFailure** -och **ResourceDeleteCancel** -händelser med olika värden för `eventType` .

```json
[{
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
  "eventType": "Microsoft.Resources.ResourceDeleteSuccess",
  "eventTime": "2018-07-19T19:24:12.763881Z",
  "id": "19a69642-1aad-4a96-a5ab-8d05494513ce",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
      "action": "Microsoft.Storage/storageAccounts/delete",
      "evidence": {
        "role": "Subscription Admin"
      }
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "_claim_names": "{\"groups\":\"src1\"}",
      "_claim_sources": "{\"src1\":{\"endpoint\":\"{URI}\"}}",
      "http://schemas.microsoft.com/claims/authnclassreference": "1",
      "aio": "{token}",
      "http://schemas.microsoft.com/claims/authnmethodsreferences": "rsa,mfa",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/2012/01/devicecontext/claims/identifier": "{ID}",
      "e_exp": "262800",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname": "{last-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname": "{first-name}",
      "ipaddr": "{IP-address}",
      "name": "{full-name}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "onprem_sid": "{ID}",
      "puid": "{ID}",
      "http://schemas.microsoft.com/identity/claims/scope": "user_impersonation",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",
      "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name": "{user-name}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn": "{user-name}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "DELETE",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}?api-version=2018-02-01"
    },
    "resourceProvider": "Microsoft.Storage",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-name}",
    "operationName": "Microsoft.Storage/storageAccounts/delete",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}"
}]
```

I följande exempel visas schemat för en **ResourceActionSuccess** -händelse. Samma schema används för **ResourceActionFailure** -och **ResourceActionCancel** -händelser med olika värden för `eventType` .

```json
[{   
  "subject": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
  "eventType": "Microsoft.Resources.ResourceActionSuccess",
  "eventTime": "2018-10-08T22:46:22.6022559Z",
  "id": "{ID}",
  "data": {
    "authorization": {
      "scope": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
      "action": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
      "evidence": {
        "role": "Contributor",
        "roleAssignmentScope": "/subscriptions/{subscription-id}",
        "roleAssignmentId": "{ID}",
        "roleDefinitionId": "{ID}",
        "principalId": "{ID}",
        "principalType": "ServicePrincipal"
      }     
    },
    "claims": {
      "aud": "{audience-claim}",
      "iss": "{issuer-claim}",
      "iat": "{issued-at-claim}",
      "nbf": "{not-before-claim}",
      "exp": "{expiration-claim}",
      "aio": "{token}",
      "appid": "{ID}",
      "appidacr": "2",
      "http://schemas.microsoft.com/identity/claims/identityprovider": "{URL}",
      "http://schemas.microsoft.com/identity/claims/objectidentifier": "{ID}",
      "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier": "{ID}",       "http://schemas.microsoft.com/identity/claims/tenantid": "{ID}",
      "uti": "{ID}",
      "ver": "1.0"
    },
    "correlationId": "{ID}",
    "httpRequest": {
      "clientRequestId": "{ID}",
      "clientIpAddress": "{IP-address}",
      "method": "POST",
      "url": "https://management.azure.com/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey/listKeys?api-version=2017-04-01"
    },
    "resourceProvider": "Microsoft.EventHub",
    "resourceUri": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.EventHub/namespaces/{namespace}/AuthorizationRules/RootManageSharedAccessKey",
    "operationName": "Microsoft.EventHub/namespaces/AuthorizationRules/listKeys/action",
    "status": "Succeeded",
    "subscriptionId": "{subscription-id}",
    "tenantId": "{tenant-id}"
  },
  "dataVersion": "2",
  "metadataVersion": "1",
  "topic": "/subscriptions/{subscription-id}/resourceGroups/{resource-group}" 
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
| id | sträng | Unikt ID för händelsen. |
| data | objekt | Resurs grupps händelse data. |
| Dataversion | sträng | Dataobjektets schemaversion. Utgivaren definierar schemaversion. |
| Metadataversion | sträng | Schemaversionen av händelsens metadata. Event Grid definierar schemat för de översta egenskaperna. Event Grid ger det här värdet. |

Data-objektet har följande egenskaper:

| Egenskap | Typ | Description |
| -------- | ---- | ----------- |
| auktorisering | objekt | Den begärda auktoriseringen för åtgärden. |
| gällande | objekt | Egenskaperna för anspråken. Mer information finns i [JWT-specifikation](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html). |
| correlationId | sträng | Ett åtgärds-ID för fel sökning. |
| httpRequest | objekt | Information om åtgärden. Det här objektet ingår bara när du uppdaterar en befintlig resurs eller tar bort en resurs. |
| resourceProvider | sträng | Resurs leverantören för åtgärden. |
| resourceUri | sträng | URI för resursen i åtgärden. |
| operationName | sträng | Åtgärden som vidtogs. |
| status | sträng | Status för åtgärden. |
| subscriptionId | sträng | Resursens prenumerations-ID. |
| tenantId | sträng | Klient organisationens ID för resursen. |

## <a name="tutorials-and-how-tos"></a>Självstudier och instruktioner
|Titel  |Beskrivning  |
|---------|---------|
| [Självstudie: övervaka ändringar av virtuella datorer med Azure Event Grid och Logic Apps](monitor-virtual-machine-changes-event-grid-logic-app.md) | En Logic app övervakar ändringar i en virtuell dator och skickar e-postmeddelanden om ändringarna. |
| [Azure CLI: prenumerera på händelser för en resurs grupp](./scripts/event-grid-cli-resource-group.md)| Exempel skript som prenumererar på händelser för en resurs grupp. Den skickar händelser till en webhook. |
| [Azure CLI: prenumerera på händelser för en resurs grupp och filtrera efter en resurs](./scripts/event-grid-cli-resource-group-filter.md) | Exempel skript som prenumererar på händelser för en resurs grupp och filtrerar händelser för en resurs. |
| [PowerShell: prenumerera på händelser för en resurs grupp](./scripts/event-grid-powershell-resource-group.md) | Exempel skript som prenumererar på händelser för en resurs grupp. Den skickar händelser till en webhook. |
| [PowerShell: prenumerera på händelser för en resurs grupp och filtrera efter en resurs](./scripts/event-grid-powershell-resource-group-filter.md) | Exempel skript som prenumererar på händelser för en resurs grupp och filtrerar händelser för en resurs. |
| [Resource Manager-mall: resurs prenumeration](https://github.com/Azure/azure-quickstart-templates/tree/master/101-event-grid-resource-events-to-webhook) | Prenumererar på händelser för en Azure-prenumeration eller resurs grupp. Den skickar händelser till en webhook. |

## <a name="next-steps"></a>Nästa steg

* En introduktion till Azure Event Grid finns i [Vad är event Grid?](overview.md)
* Mer information om hur du skapar en Azure Event Grid-prenumeration finns i [Event Grid prenumerations schema](subscription-creation-schema.md).
