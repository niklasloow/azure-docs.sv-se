---
title: Översikt över Azure Multi-Factor Authentication
description: Lär dig hur Azure Multi-Factor Authentication hjälper till att skydda åtkomsten till data och program samtidigt som du kan möta användarnas behov av en enkel inloggnings process.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/14/2020
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: b5f8d08f0814ec69719c002ea9efd39bb38b7d16
ms.sourcegitcommit: 6fc156ceedd0fbbb2eec1e9f5e3c6d0915f65b8e
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/21/2020
ms.locfileid: "88718024"
---
# <a name="how-it-works-azure-multi-factor-authentication"></a>Så här fungerar Azure Multi-Factor Authentication

Multi-Factor Authentication är en process där en användare uppmanas att ange en ytterligare typ av identifiering, till exempel för att ange en kod på mobilen eller för att tillhandahålla en finger avsökning.

Om du bara använder ett lösen ord för att autentisera en användare lämnar den osäker vektor för angrepp. Om lösen ordet är svagt eller har exponerats någon annan stans, är det verkligen användaren som loggar in med användar namnet och lösen ordet, eller är det en angripare? När du behöver en andra form av autentisering, ökar säkerheten eftersom den här ytterligare faktorn inte är något som är lätt för en angripare att hämta eller duplicera.

![Konceptuell bild av de olika formerna av Multi-Factor Authentication](./media/concept-mfa-howitworks/methods.png)

Azure Multi-Factor Authentication fungerar genom att kräva två eller fler av följande autentiseringsmetoder:

* Något du känner, vanligt vis ett lösen ord.
* Något du har, till exempel en betrodd enhet som inte är enkel att duplicera, till exempel en telefon eller maskin varu nyckel.
* Något du är-biometrik som ett finger avtryck eller en ansikts skanning.

Användare kan registrera sig själva för lösen ords återställning via självbetjäning och Azure Multi-Factor Authentication i ett steg för att förenkla den här upplevelsen. Administratörer kan definiera vilka former av sekundär autentisering som kan användas. Azure Multi-Factor Authentication kan också krävas när användare utför en lösen ords återställning via självbetjäning för att ytterligare skydda processen.

![Autentiseringsmetoder som används på inloggnings skärmen](media/concept-authentication-methods/overview-login.png)

Azure Multi-Factor Authentication hjälper till att skydda åtkomsten till data och program samtidigt som användarnas skull bibehålls. Den ger ytterligare säkerhet genom att kräva en andra form av autentisering och ger stark autentisering via en rad enkla att använda [autentiseringsmetoder](concept-authentication-methods.md). Användare kan kanske inte anropas för MFA baserat på konfigurations beslut som en administratör gör.

Dina program eller tjänster behöver inte göra några ändringar för att använda Azure Multi-Factor Authentication. Verifierings frågorna är en del av inloggnings händelsen i Azure AD, som automatiskt begär och bearbetar MFA-utmaningen vid behov.

## <a name="available-verification-methods"></a>Tillgängliga verifierings metoder

När en användare loggar in till ett program eller en tjänst och får en MFA-prompt kan de välja någon av sina registrerade former av ytterligare verifiering. En administratör kan behöva registrera dessa Azure-Multi-Factor Authentication verifierings metoder, eller så kan användaren komma åt sin egen [min profil](https://myprofile.microsoft.com) för att redigera eller lägga till verifierings metoder.

Följande ytterligare typer av verifiering kan användas med Azure Multi-Factor Authentication:

* Microsoft Authenticator-appen
* OATH-token för maskin vara
* SMS
* Röstsamtal

## <a name="how-to-enable-and-use-azure-multi-factor-authentication"></a>Så här aktiverar och använder du Azure Multi-Factor Authentication

Användare och grupper kan aktive ras för Azure Multi-Factor Authentication för att begära ytterligare verifiering under inloggnings händelsen. [Säkerhets inställningar](../fundamentals/concept-fundamentals-security-defaults.md) är tillgängliga för alla Azure AD-klienter för att snabbt kunna använda Microsoft Authenticator-appen för alla användare.

För mer detaljerade kontroller kan principer för [villkorlig åtkomst](../conditional-access/overview.md) användas för att definiera händelser eller program som kräver MFA. Dessa principer kan möjliggöra vanliga inloggnings händelser när användaren är i företags nätverket eller en registrerad enhet, men du måste ange ytterligare verifierings faktorer vid fjärr anslutning eller på en personlig enhet.

![Översikts diagram över hur villkorlig åtkomst fungerar för att skydda inloggnings processen](media/tutorial-enable-azure-mfa/conditional-access-overview.png)

## <a name="next-steps"></a>Nästa steg

Mer information om licensiering finns i [funktioner och licenser för Azure Multi-Factor Authentication](concept-mfa-licensing.md).

Om du vill se MFA i praktiken aktiverar du Azure Multi-Factor Authentication för en uppsättning test användare i följande självstudie:

> [!div class="nextstepaction"]
> [Aktivera Azure Multi-Factor Authentication](./tutorial-enable-azure-mfa.md)