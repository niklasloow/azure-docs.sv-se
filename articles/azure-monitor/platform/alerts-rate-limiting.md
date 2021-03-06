---
title: Hastighets begränsning för SMS, e-post, push-meddelanden
description: Förstå hur Azure begränsar antalet möjliga SMS-, e-Azure App push-eller webhook-aviseringar från en åtgärds grupp.
author: dkamstra
ms.author: dukek
ms.topic: conceptual
ms.date: 3/12/2018
ms.subservice: alerts
ms.openlocfilehash: 39b9fd8396601d777aa802a147bee3acc4a22cd0
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87045294"
---
# <a name="rate-limiting-for-voice-sms-emails-azure-app-push-notifications-and-webhook-posts"></a>Hastighets begränsning för röst, SMS, e-post, Azure App push-meddelanden och webhook-inlägg
Hastighets begränsning är ett avbrott i meddelanden som inträffar när för många skickas till ett visst telefonnummer, en e-postadress eller en enhet. Hastighets begränsning garanterar att aviseringar kan hanteras och åtgärdas.

Tröskelvärden för frekvens gränser är:

- **SMS**: högst 1 SMS var 5: e minut.
- **Röst**: högst 1 röst samtal var 5: e minut.
- **E-post**: högst 100 e-postmeddelanden på en timme.
 
  Andra åtgärder är inte begränsade.

## <a name="rate-limit-rules"></a>Hastighets begränsnings regler
- Ett visst telefonnummer eller e-postmeddelande är begränsat när det får fler meddelanden än tröskelvärdet tillåter.
- Ett telefonnummer eller e-postmeddelande kan ingå i åtgärds grupper över flera prenumerationer. Hastighets begränsningen gäller för alla prenumerationer. Den gäller så snart tröskelvärdet har nåtts, även om meddelanden skickas från flera prenumerationer.
- När en e-postadress är begränsad så skickas ytterligare ett meddelande för att kommunicera med hastighets begränsningen. E-posttillstånden när hastighets begränsningen upphör att gälla.

## <a name="next-steps"></a>Nästa steg ##
* Lär dig mer om [SMS-aviserings beteende](alerts-sms-behavior.md).
* Få en [Översikt över aktivitets logg aviseringar](alerts-overview.md)och lär dig hur du tar emot aviseringar.  
* Lär dig hur du [konfigurerar aviseringar när ett meddelande om tjänst hälsa har publicerats](../../service-health/alerts-activity-log-service-notifications-portal.md).
