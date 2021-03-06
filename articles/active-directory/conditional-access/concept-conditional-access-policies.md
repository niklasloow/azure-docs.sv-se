---
title: Skapa en princip för villkorlig åtkomst – Azure Active Directory
description: Vilka alternativ är tillgängliga för att skapa en princip för villkorlig åtkomst och vad innebär det?
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36ab632010ec2bbbc19ac71cbeccab2ff6b3565f
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88948393"
---
# <a name="building-a-conditional-access-policy"></a>Skapa en princip för villkorlig åtkomst

Som det beskrivs i artikeln [Vad är villkorlig åtkomst](overview.md)är en princip för villkorlig åtkomst en if-then-instruktion, **tilldelnings** -och **åtkomst kontroller**. En princip för villkorlig åtkomst ger signaler till varandra för att fatta beslut och tillämpa organisations principer.

Hur skapar en organisation dessa principer? Vad krävs?

![Villkorlig åtkomst (signaler + beslut + tvång = principer)](./media/concept-conditional-access-policies/conditional-access-signal-decision-enforcement.png)

## <a name="assignments"></a>Tilldelningar

Tilldelnings delen styr vem, vad och var för principen för villkorlig åtkomst.

### <a name="users-and-groups"></a>Användare och grupper

[Användare och grupper](concept-conditional-access-users-groups.md) som principen ska inkludera eller exkludera. Den här tilldelningen kan omfatta alla användare, vissa grupper av användare, katalog roller eller externa gäst användare. 

### <a name="cloud-apps-or-actions"></a>Molnappar eller åtgärder

[Molnappar eller-åtgärder](concept-conditional-access-cloud-apps.md) kan inkludera eller exkludera moln program eller användar åtgärder som ska omfattas av principen.

### <a name="conditions"></a>Villkor

En princip kan innehålla flera [villkor](concept-conditional-access-conditions.md).

#### <a name="sign-in-risk"></a>Inloggningsrisk

För organisationer med [Azure AD Identity Protection](../identity-protection/overview-identity-protection.md)kan de risk identifieringar som genererats påverka dina principer för villkorlig åtkomst.

#### <a name="device-platforms"></a>Enhetsplattformar

Organisationer med flera plattformar för operativ system enheter kanske vill genomdriva vissa principer på olika plattformar. 

Informationen som används för att beräkna enhets plattformen kommer från overifierade källor som användar Agent strängar som kan ändras.

#### <a name="locations"></a>Platser

Plats data tillhandahålls av IP-datageolokalisering. Administratörer kan välja att definiera platser och välja att markera vissa som betrodda som de för deras organisations nätverks platser.

#### <a name="client-apps"></a>Klientappar

Som standard tillämpas principer för villkorlig åtkomst på webb läsar appar, mobilappar och skriv bords klienter som stöder modern autentisering. 

Med det här tilldelnings villkoret kan principer för villkorlig åtkomst rikta in sig på specifika klient program som inte använder modern autentisering. Programmen innehåller Exchange ActiveSync-klienter, äldre Office-program som inte använder modern autentisering och e-postprotokoll som IMAP, MAPI, POP och SMTP.

#### <a name="device-state"></a>Enhets tillstånd

Den här kontrollen används för att undanta enheter som är hybrid Azure AD-anslutna eller som är kompatibla med Intune. Detta undantag kan göras för att blockera ohanterade enheter. 

## <a name="access-controls"></a>Åtkomstkontroller

Åtkomst kontroll delen av principen för villkorlig åtkomst styr hur en princip tillämpas.

### <a name="grant"></a>Bevilja

[Tillåt](concept-conditional-access-grant.md) ger administratörer ett sätt att tillämpa principer där de kan blockera eller bevilja åtkomst.

#### <a name="block-access"></a>Blockera åtkomst

Blockera åtkomst är bara att blockera åtkomsten under de angivna tilldelningarna. Block kontrollen är kraftfull och bör vara wielded med lämplig kunskap.

#### <a name="grant-access"></a>Bevilja åtkomst

Kontrollen Grant kan utlösa verk ställande av en eller flera kontroller. 

- Kräv Multi-Factor Authentication (Azure Multi-Factor Authentication)
- Kräv att enheten är markerad som kompatibel (Intune)
- Kräv hybrid Azure AD-ansluten enhet
- Kräv godkänd klientapp
- Kräva appskyddsprincip

Administratörer kan välja att kräva en av de tidigare kontrollerna eller alla valda kontroller med följande alternativ. Standardvärdet för flera kontroller är att kräva alla.

- Kräv alla markerade kontroller (kontroll och kontroll)
- Kräv en av de valda kontrollerna (kontroll eller kontroll)

### <a name="session"></a>Session

[Sessions kontroller](concept-conditional-access-session.md) kan begränsa upplevelsen 

- Använd app-framtvingade begränsningar
   - Fungerar för närvarande endast med Exchange Online och SharePoint Online.
      - Skickar enhets information så att du får kontroll över hur du får fullständig eller begränsad åtkomst.
- Använd Appkontroll för villkorsstyrd åtkomst
   - Använder signaler från Microsoft Cloud App Security till exempel: 
      - Blockera nedladdning, klipp ut, kopiera och skriv ut känsliga dokument.
      - Övervaka beteende för riskfyllda sessioner.
      - Kräv etiketter för känsliga filer.
- Inloggnings frekvens
   - Möjlighet att ändra standard inloggnings frekvensen för modern autentisering.
- Beständig webbläsarsession
   - Tillåter att användare förblir inloggade efter att ha stängt och öppnat sitt webbläsarfönster igen.

## <a name="simple-policies"></a>Enkla principer

En princip för villkorlig åtkomst måste innehålla minst följande för att kunna tillämpas:

- **Namnet** på principen.
- **Tilldelningar**
   - **Användare och/eller grupper** som principen ska tillämpas på.
   - **Molnappar eller åtgärder** för att tillämpa principen på.
- **Åtkomstkontroller**
   - **Bevilja** eller **blockera** kontroller

![Tom princip för villkorlig åtkomst](./media/concept-conditional-access-policies/conditional-access-blank-policy.png)

Artikeln [vanliga principer för villkorlig åtkomst](concept-conditional-access-policy-common.md) innehåller vissa principer som vi tror är användbara för de flesta organisationer.

## <a name="next-steps"></a>Nästa steg

[Simulera inloggnings beteende med hjälp av What If verktyget för villkorlig åtkomst](troubleshoot-conditional-access-what-if.md)

[Planera en molnbaserad distribution av Azure Multi-Factor Authentication](../authentication/howto-mfa-getstarted.md)

[Hantera enheternas kompatibilitet med Intune](/intune/device-compliance-get-started)

[Microsoft Cloud App Security och villkorlig åtkomst](/cloud-app-security/proxy-intro-aad)