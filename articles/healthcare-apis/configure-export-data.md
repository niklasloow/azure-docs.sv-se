---
title: Konfigurera export inställningar i Azure API för FHIR
description: Den här artikeln beskriver hur du konfigurerar export inställningar i Azure API för FHIR
author: matjazl
ms.service: healthcare-apis
ms.subservice: fhir
ms.topic: reference
ms.date: 3/5/2020
ms.author: matjazl
ms.openlocfilehash: 7ed4e471764fbf8ecc9b82a7d614ae52b97360f1
ms.sourcegitcommit: bcda98171d6e81795e723e525f81e6235f044e52
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89267266"
---
# <a name="configure-export-setting-and-set-up-the-storage-account"></a>Konfigurera export inställning och konfigurera lagrings kontot

Azure API för FHIR stöder $export kommandot som gör att du kan exportera data från Azure API för FHIR-konto till ett lagrings konto.

Det finns tre steg när du konfigurerar export i Azure API för FHIR:

1. Aktivera hanterad identitet på Azure API för FHIR-tjänsten
2. Skapa ett Azure Storage-konto (om det inte gjorts tidigare) och tilldela behörighet till Azure API för FHIR till lagrings kontot
3. Välja lagrings kontot i Azure API för FHIR som export Storage-konto

## <a name="enabling-managed-identity-on-azure-api-for-fhir"></a>Aktivera hanterad identitet på Azure API för FHIR

Det första steget i att konfigurera Azure API för FHIR för export är att aktivera systemomfattande hanterad identitet för tjänsten. Du kan läsa mer om hanterade identiteter i Azure [här](../active-directory/managed-identities-azure-resources/overview.md).

Det gör du genom att gå till Azure API för FHIR-tjänsten och välja identitets blad. Genom att ändra status till på aktive ras hanterad identitet i Azure API för FHIR-tjänsten.

![Aktivera hanterad identitet](media/export-data/fhir-mi-enabled.png)

Nu kan vi gå vidare till nästa steg och skapa ett lagrings konto och tilldela behörighet till vår tjänst.

## <a name="adding-permission-to-storage-account"></a>Lägger till behörighet till lagrings kontot

Nästa steg i exporten är att tilldela behörighet för Azure API för FHIR-tjänsten att skriva till lagrings kontot.

När vi har skapat ett lagrings konto navigerar du till bladet Access Control (IAM) i lagrings kontot och väljer Lägg till roll tilldelningar

![Aktivera hanterad identitet](media/export-data/fhir-export-role-assignment.png)

Här lägger vi till roll lagrings data deltagare i vårt tjänst namn.

![Aktivera hanterad identitet](media/export-data/fhir-export-role-add.png)

Nu är vi redo för nästa steg där vi kan välja lagrings kontot i Azure API för FHIR som ett standard lagrings konto för $export.

## <a name="selecting-the-storage-account-for-export"></a>Välja lagrings konto för $export

Sista steget är att tilldela det Azure Storage-konto som Azure API för FHIR ska använda för att exportera data till. Det gör du genom att gå till integrations bladet i Azure API for FHIR service i Azure Portal och välja lagrings kontot

![Aktivera hanterad identitet](media/export-data/fhir-export-storage.png)

När vi är redo att exportera data med hjälp av kommandot $export.

>[!div class="nextstepaction"]
>[Ytterligare inställningar](azure-api-for-fhir-additional-settings.md)
