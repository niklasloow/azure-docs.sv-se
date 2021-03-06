---
title: Samar beta med andra – QnA Maker
description: ''
ms.topic: conceptual
ms.date: 05/15/2020
ms.openlocfilehash: 080e6549579675e27486e6173d5907d92bbaad70
ms.sourcegitcommit: 6fd8dbeee587fd7633571dfea46424f3c7e65169
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 05/21/2020
ms.locfileid: "83724929"
---
# <a name="collaborate-with-other-authors-and-editors"></a>Samar beta med andra författare och redigerare

Samar beta med andra författare och redigerare som använder rollbaserad åtkomst kontroll (RBAC) som placeras på din QnA Maker-resurs.

## <a name="access-is-provided-on-the-qna-maker-resource"></a>Åtkomst finns på QnA Maker resursen

Alla behörigheter styrs av de behörigheter som har placerats på QnA Maker resursen. Behörigheterna anpassas till Läs-, Skriv-, publicerings-och fullständig åtkomst.

Den här RBAC-funktionen innehåller:
* Azure Active Directory (AAD) är 100% bakåtkompatibel med nyckelbaserad autentisering för ägare och deltagare. Kunder kan använda antingen nyckelbaserad autentisering eller RBAC-baserad autentisering i sina begär Anden.
* Lägg snabbt till författare och redigerare i alla kunskaps banker i resursen eftersom kontrollen finns på resurs nivå, inte på kunskaps bas nivån.

## <a name="access-is-provided-by-a-defined-role"></a>Åtkomst tillhandahålls av en definierad roll

[!INCLUDE [RBAC permissions table](../includes/role-based-access-control.md)]

## <a name="authentication-flow"></a>Autentiseringsflöde

Följande diagram visar flödet från författarens perspektiv för att logga in på QnA Maker-portalen och använda redigerings-API: erna.

> [!div class="mx-imgBorder"]
> ![Följande diagram visar flödet från författarens perspektiv för att logga in på QnA Maker-portalen och använda redigerings-API: erna.](../media/qnamaker-how-to-collaborate-knowledge-base/rbac-flow-from-portal-to-service.png)

|Steg|Beskrivning|
|--|--|
|1|Portalen hämtar token för QnA Maker resurs.|
|2|Portalen anropar lämplig QnA Maker redigerings-API (APIM) som skickar token i stället för nycklar.|
|3|API för QnA Maker validerar token.|
|4 |API för QnA Maker anropar QnAMaker-tjänsten.|

Om du tänker anropa redigerings- [API: erna](../How-To/collaborate-knowledge-base.md)kan du läsa mer om hur du konfigurerar autentisering.

## <a name="authenticate-by-qna-maker-portal"></a>Autentisera med QnA Maker Portal

Om du skapar och samarbetar med hjälp av QnA Maker Portal hanterar QnA Maker portalen alla åtkomst behörigheter när du har [lagt till en lämplig roll till resursen för en medarbetare](../How-To/collaborate-knowledge-base.md).

## <a name="authenticate-by-qna-maker-apis-and-sdks"></a>Autentisera med QnA Maker-API: er och SDK: er

Om du skapar och samarbetar med hjälp av API: er, antingen via REST eller SDK: er, måste du [skapa ett huvud namn för tjänsten](../../authentication.md#assign-a-role-to-a-service-principal) för att hantera autentiseringen.

## <a name="next-step"></a>Nästa steg

* Utforma en kunskaps bas för [språk](design-language-culture.md) och för [klient program](integration-with-other-applications.md)
