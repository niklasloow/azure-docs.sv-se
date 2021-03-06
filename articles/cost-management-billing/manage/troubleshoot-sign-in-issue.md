---
title: Felsöka problem med inloggning för Azure-prenumeration
description: Hjälper till att lösa de problem som gör att du inte kan logga in på Azure-portalen eller Azure-kontocentret.
services: cost-management-billing
author: v-miegge
manager: dcscontentpm
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: troubleshooting
ms.date: 08/20/2020
ms.author: v-miegge
ms.openlocfilehash: 3ee600cb72d06781f87c8f68640576afa50cea06
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/20/2020
ms.locfileid: "88686504"
---
# <a name="troubleshoot-azure-subscription-sign-in-issues"></a>Felsöka problem med inloggning för Azure-prenumeration

Den här guiden hjälper till att lösa de problem som gör att du inte kan logga in på Azure-portalen eller Azure-kontocentret.

> [!NOTE]
> Om du har problem med att registrera dig för ett nytt Azure-konto läser du [Felsöka problem med registreringen av en Azure-prenumeration](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-azure-sign-up).

## <a name="page-hangs-in-the-loading-status"></a>Sidan låser sig under inläsningen

Om sidan i webbläsaren låser sig provar du vart och ett av följande steg tills du kommer till Azure-portalen.

- Uppdatera sidan.
- Använd en annan webbläsare.
- Använd privat surfläge för webbläsaren:

   - **Edge:** Öppna **Inställningar** (de tre punkterna bredvid din profilbild), välj **Nytt InPrivate-fönster** och gå sedan till och logga in på [Azure-portalen](https://portal.azure.com/) eller [Azure-kontocenter](https://account.azure.com/Subscriptions). 
   - **Chrome:** Välj **Incognito**-läge.
   - **Safari:** Välj **Arkiv** och sedan **Nytt privat fönster**.

- Rensa cacheminnet och ta bort Internet-cookies:

   - **Edge:** Öppna **Inställningar** och välj **Sekretess och tjänster**. Följ stegen under **Rensa webbdata**. Kontrollera att kryssrutorna för **Webbhistorik**, **Nedladdningshistorik** och **Cachelagrade bilder och filer** är markerade och välj sedan **Ta bort**.
   - **Chrome:** Välj **Inställningar** och välj sedan **Rensa webbinformation** under **Sekretess och säkerhet**.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Du loggas automatiskt in som en annan användare

Det här problemet kan uppstå om du använder mer än ett användarkonto i en webbläsare.

Försök att lösa problemet på något av följande sätt:

- Rensa cacheminnet och ta bort Internet-cookies.

   - **Edge:** Öppna **Inställningar** och välj **Sekretess och tjänster**. Följ stegen under **Rensa webbdata**. Kontrollera att kryssrutorna för **Webbhistorik**, **Nedladdningshistorik**, **Cookies** och **Cachelagrade bilder och filer** är markerade och välj sedan **Ta bort**.
   - **Chrome:** Välj **Inställningar** och välj sedan **Rensa webbinformation** under **Sekretess och säkerhet**.
- Återställ webbläsarinställningarna till standardinställningarna.
- Använd privat surfläge för webbläsaren. 
   - **Edge:** Öppna **Inställningar** (de tre punkterna bredvid din profilbild), välj **Nytt InPrivate-fönster** och gå sedan till och logga in på [Azure-portalen](https://portal.azure.com/) eller [Azure-kontocenter](https://account.azure.com/Subscriptions). 
   - **Chrome:** Välj **Incognito**-läge.
   - **Safari:** Välj **Arkiv** och sedan **Nytt privat fönster**.

## <a name="i-can-sign-in-but-i-see-the-error-no-subscriptions-found"></a>Jag kan logga in, men jag får ett felmeddelande om att inga prenumerationer hittades

Det här problemet inträffar om du har valt fel katalog eller om kontot inte har tillräcklig behörighet.

**Scenario 1:** Du får felmeddelandet när du loggar in på [Azure-portalen](https://portal.azure.com/)

Så här åtgärdar du problemet:

- Kontrollera att rätt Azure-katalog har valts genom att välja ditt konto i det övre högra hörnet.
- Om rätt Azure-katalog har valts men du fortfarande får felmeddelandet [lägger du till ditt konto som ägare](https://docs.microsoft.com/azure/cost-management-billing/manage/add-change-subscription-administrator).

**Scenario 2:** Du får felmeddelandet när du loggar in till [Azure-kontocenter](https://account.windowsazure.com/Subscriptions)

Kontrollera om kontot du använder är en kontoadministratör. Så här kontrollerar du vem som är kontoadministratör:

1.  Logga in i [vyn Prenumerationer i Azure-portalen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade).
1.  Välj den prenumeration som du vill använda och välj sedan **Inställningar**.
1.  Välj **Egenskaper**. Prenumerationens kontoadministratör visas i rutan **Kontoadministratör**.

## <a name="additional-help-resources"></a>Fler hjälpresurser

Här är några fler felsökningsartiklar som gäller fakturering och prenumerationer i Azure

- [Nekat kort](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-declined-card)
- [Problem med prenumerationsregistrering](https://docs.microsoft.com/azure/cost-management-billing/manage/troubleshoot-azure-sign-up)
- [Inga prenumerationer hittades](https://docs.microsoft.com/azure/cost-management-billing/manage/no-subscriptions-found)
- [Företagskostnadsvy inaktiverad](https://docs.microsoft.com/azure/cost-management-billing/manage/enterprise-mgmt-grp-troubleshoot-cost-view)
- [Dokumentation om Azure-fakturering](https://docs.microsoft.com/azure/cost-management-billing/)

## <a name="contact-us-for-help"></a>Kontakta oss om du behöver hjälp

Om du har frågor eller behöver hjälp kan du [skapa en supportbegäran](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).
