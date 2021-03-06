---
title: Konfigurera Azure Defender för lagring
titleSuffix: Azure Storage
description: Konfigurera Azure Defender för lagring för att identifiera avvikelser i konto aktivitet och få meddelanden om potentiellt skadliga försök att komma åt ditt konto.
services: storage
author: tamram
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: tamram
ms.reviewer: ozgun
ms.openlocfilehash: c7e0c9aee1ce6b4a2524ac756673784b63be3b31
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91289777"
---
# <a name="configure-azure-defender-for-storage"></a>Konfigurera Azure Defender för lagring

Azure Defender för lagring ger ytterligare ett lager med säkerhets information som identifierar ovanliga och potentiellt skadliga försök att komma åt eller utnyttja lagrings konton. Med det här skydds lagret kan du åtgärda hot utan att vara säkerhets expert eller hantera säkerhets övervaknings system.

Säkerhets varningar utlöses när avvikelser i aktivitet inträffar. Dessa säkerhets aviseringar är integrerade med [Azure Security Center](https://azure.microsoft.com/services/security-center/)och skickas också via e-post till prenumerations administratörer med information om misstänkt aktivitet och rekommendationer om hur du undersöker och åtgärdar hot.

Tjänsten matar in resurs loggar med Läs-, skriv-och borttagnings begär anden till Blob Storage och Azure Files för hot identifiering. Om du vill undersöka aviseringar från Azure Defender kan du Visa relaterad lagrings aktivitet med hjälp av Lagringsanalys loggning. Mer information finns i **Konfigurera loggning** i [övervaka ett lagrings konto i Azure Portal](storage-monitor-storage-account.md#configure-logging).

## <a name="availability"></a>Tillgänglighet

Azure Defender för lagring är för närvarande tillgängligt för Blob Storage, Azure Files och Azure Data Lake Storage Gen2. Konto typer som stöder Azure Defender inkluderar generell-Purpose v2-, Block-Blob-och Blob Storage-konton. Azure Defender för lagring är tillgängligt i alla offentliga moln och i amerikanska myndigheter, men inte i andra suveräna eller Azure Government moln regioner.

Konton med hierarkiska namn rymder aktiverade för Data Lake Storage stöd transaktioner med både Azure Blob Storage-API: er och Data Lake Storage-API: er. Azure-filresurser stöder transaktioner över SMB.

För pris information, inklusive en kostnads fri 30-dagars utvärderings version, se [sidan Azure Security Center prissättning](https://azure.microsoft.com/pricing/details/security-center/).

I följande lista sammanfattas tillgängligheten för Azure Defender för lagring:

- Versions tillstånd:
  - [Blob Storage](https://azure.microsoft.com/services/storage/blobs/) (allmän tillgänglighet)
  - [Azure Files](https://docs.microsoft.com/azure/storage/files/storage-files-introduction) (allmän tillgänglighet)
  - Azure Data Lake Storage Gen2 (allmän tillgänglighet)
- Moln<br>
    ✔ Kommersiella moln<br>
    ✔ US Gov<br>
    ✘ Kina gov, andra gov

## <a name="set-up-azure-defender"></a>Konfigurera Azure Defender

Du kan konfigurera Azure Defender för lagring på flera olika sätt, som beskrivs i följande avsnitt.

### <a name="azure-security-center"></a>[Azure Security Center](#tab/azure-security-center)

När du prenumererar på standard nivån i Azure Security Center konfigureras Azure Defender automatiskt på alla dina lagrings konton. Du kan aktivera eller inaktivera Azure Defender för dina lagrings konton under en speciell prenumeration på följande sätt:

1. Starta **Azure Security Center** i [Azure Portal](https://portal.azure.com).
1. Välj **pris & inställningar**under **hantering**i huvud menyn.
1. Välj den prenumeration som du vill aktivera eller inaktivera Azure Defender.
1. Välj **Azure Defender på** för att aktivera Azure Defender för prenumerationen.
1. Leta upp **lagrings** raden under **Välj Azure Defender-plan efter resurs typ**och välj **aktive rad** i kolumnen **plan** .
1. Spara ändringarna.

    :::image type="content" source="media/azure-defender-storage-configure/enable-azure-defender-security-center.png" alt-text="Skärm bild som visar hur du aktiverar Azure Defender för lagring i Security Center":::

Azure Defender har nu Aktiver ATS för alla lagrings konton i den här prenumerationen.

### <a name="portal"></a>[Portal](#tab/azure-portal)

1. Starta [Azure Portal](https://portal.azure.com/).
1. Navigera till ditt lagringskonto. Under **Inställningar**väljer du **avancerad säkerhet**.
1. Välj **Aktivera Azure Defender för lagring**.

    :::image type="content" source="media/azure-defender-storage-configure/enable-azure-defender-portal.png" alt-text="Skärm bild som visar hur du aktiverar Azure Defender för ett Azure Storage konto":::

Azure Defender har nu Aktiver ATS för det här lagrings kontot.

### <a name="template"></a>[Mall](#tab/template)

Använd en Azure Resource Manager mall för att distribuera ett Azure Storage konto med Azure Defender aktiverat. Mer information finns i [lagrings konto med avancerat skydd](https://azure.microsoft.com/resources/templates/201-storage-advanced-threat-protection-create/).

### <a name="azure-policy"></a>[Azure Policy](#tab/azure-policy)

Använd en Azure Policy för att aktivera Azure Defender över lagrings konton under en speciell prenumeration eller resurs grupp.

1. Starta sidan Azure **policy-definitions** .
1. Sök efter principen **Distribuera Azure Defender på lagrings konton** .

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy-definitions.png" alt-text="Använd princip för att aktivera Azure Defender för lagrings konton":::

1. Välj en Azure-prenumeration eller resurs grupp.

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy2.png" alt-text="Välj en prenumeration eller resurs grupp för principens omfattning ":::

1. Tilldela principen.

    :::image type="content" source="media/azure-defender-storage-configure/storage-atp-policy1.png" alt-text="Tilldela en princip för att aktivera Azure Defender för lagring":::

### <a name="rest-api"></a>[REST-API](#tab/rest-api)

Använd REST API-kommandon för att skapa, uppdatera eller hämta Azure Defender-inställningen för ett angivet lagrings konto.

- [Avancerat skydd – skapa](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/create)
- [Avancerat skydd – Hämta](https://docs.microsoft.com/rest/api/securitycenter/advancedthreatprotection/get)

### <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Använd följande PowerShell-cmdletar:

- [Aktivera avancerat skydd](https://docs.microsoft.com/powershell/module/az.security/enable-azsecurityadvancedthreatprotection)
- [Hämta Avancerat skydd](https://docs.microsoft.com/powershell/module/az.security/get-azsecurityadvancedthreatprotection)
- [Inaktivera Avancerat skydd](https://docs.microsoft.com/powershell/module/az.security/disable-azsecurityadvancedthreatprotection)

---

## <a name="explore-security-anomalies"></a>Utforska säkerhets avvikelser

När avvikelser i lagrings aktiviteter inträffar får du ett e-postmeddelande med information om den misstänkta säkerhets händelsen. Information om händelsen är:

- Avvikelsens karaktär
- Lagringskontots namn
- Händelse tiden
- Lagrings typ
- Möjliga orsaker
- Undersöknings stegen
- Reparations stegen

E-postmeddelandet innehåller också information om möjliga orsaker och rekommenderade åtgärder för att undersöka och minimera det potentiella hotet.

:::image type="content" source="media/azure-defender-storage-configure/storage-advanced-threat-protection-alert-email.png" alt-text="Azure Defender för lagrings aviserings-e-post":::

Du kan granska och hantera dina aktuella säkerhets aviseringar från Azure Security Center [rutan säkerhets aviseringar](../../security-center/security-center-managing-and-responding-alerts.md). Om du klickar på en enskild avisering visas information och åtgärder för att undersöka det aktuella hotet och åtgärda framtida hot.

:::image type="content" source="media/azure-defender-storage-configure/storage-advanced-threat-protection-alert.png" alt-text="Azure Defender för lagrings avisering":::

## <a name="security-alerts"></a>Säkerhetsaviseringar

Aviseringar genereras av ovanliga och potentiellt skadliga försök att komma åt eller utnyttja lagrings konton. En lista över aviseringar för Azure Storage finns i [varningar för Azure Storage](../../security-center/alerts-reference.md#alerts-azurestorage).

## <a name="next-steps"></a>Nästa steg

- Läs mer om [loggar i Azure Storage-konton](/rest/api/storageservices/About-Storage-Analytics-Logging)
- Läs mer om [Azure Security Center](../../security-center/security-center-intro.md)
