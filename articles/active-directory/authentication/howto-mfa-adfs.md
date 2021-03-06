---
title: Skydda resurser med Azure MFA och ADFS-Azure Active Directory
description: Det här är sidan om Azure Multi-Factor Authentication som beskriver hur du kommer igång med Azure MFA och AD FS i molnet.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: how-to
ms.date: 07/11/2018
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf57bea87fcb5e1d1f1bde4eada5a79d2fef52c8
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90706327"
---
# <a name="securing-cloud-resources-with-azure-multi-factor-authentication-and-ad-fs"></a>Skydda molnresurser med Azure Multi-Factor Authentication och AD FS

Om din organisation är federerad med Azure Active Directory använder du Azure Multi-Factor Authentication eller Active Directory Federation Services (AD FS) för att skydda dessa resurser. Skydda dina resurser i Azure Active Directory med Azure Multi-Factor Authentication eller Active Directory Federation Services genom att använda följa steg.

## <a name="secure-azure-ad-resources-using-ad-fs"></a>Skydda Azure AD-resurser med hjälp av AD FS

Ställ in en anspråksregel så att Active Directory Federation Services genererar multipleauthn-kravet när en användare utför tvåstegsverifiering om du vill skydda din molnresurs. Det här anspråket överförs till Azure AD. Följ dessa steg:

1. Öppna AD FS-hantering.
2. Välj **förlitande parts förtroenden**till vänster.
3. Högerklicka på **Microsoft Office 365 Identity Platform** och välj **Redigera anspråksregler**.

   ![ADFS-konsol-förtroende för förlitande part](./media/howto-mfa-adfs/trustedip1.png)

4. För Utfärdande av transformeringsregler klickar du på **Lägg till regel**.

   ![Redigera omvandlings regler för utfärdande](./media/howto-mfa-adfs/trustedip2.png)

5. I guiden Lägg till anspråksregel för transformering väljer du **Släpp igenom eller Filtrera ett inkommande anspråk** i listrutan och klickar sedan på **Nästa**.

   ![Guiden Lägg till anspråksregel för transformering](./media/howto-mfa-adfs/trustedip3.png)

6. Namnge din regel. 
7. Välj **Autentiseringsmetodreferenser** som den inkommande anspråkstypen.
8. Välj **Släpp igenom alla anspråksvärden**.
    ![Guiden Lägg till anspråksregel för transformering](./media/howto-mfa-adfs/configurewizard.png)
9. Klicka på **Slutför**. Stäng AD FS-hanteringskonsolen.

## <a name="trusted-ips-for-federated-users"></a>Tillförlitliga IP-adresser för federerade användare

Tillförlitliga IP-adresser gör att administratörer kan kringgå tvåstegsverifiering för specifika IP-adresser eller federerade användare som har förfrågningar som kommer inifrån det egna intranätet. Följande avsnitt beskriver hur du konfigurerar tillförlitliga IP-adresser för Azure Multi-Factor Authentication med federerade användare och hur du kringgår tvåstegsverifiering när en begäran kommer inifrån en federerad användares intranät. Du gör detta genom att konfigurera AD FS att släppa igenom eller filtrera en mall för inkommande anspråk med anspråkstypen I företagsnätverket.

I det här exemplet används Microsoft 365 för våra förlitande parts förtroenden.

### <a name="configure-the-ad-fs-claims-rules"></a>Konfigurera anspråksreglerna för AD FS

Det första vi måste göra är att konfigurera AD FS-anspråken. Skapa två anspråksregler, en för anspråkstypen inom företagsnätverket och ytterligare en som gör att våra användare förblir inloggade.

1. Öppna AD FS-hantering.
2. Välj **förlitande parts förtroenden**till vänster.
3. Högerklicka på **Microsoft Office 365 identitets plattform** och välj **Redigera anspråks regler...** 
    ![ ADFS-konsol – redigera anspråks regler](./media/howto-mfa-adfs/trustedip1.png)
4. Klicka på **Lägg till regel** på regler för utfärdande av utfärdande. 
    ![ Lägga till en anspråks regel](./media/howto-mfa-adfs/trustedip2.png)
5. I guiden Lägg till anspråksregel för transformering väljer du **Släpp igenom eller Filtrera ett inkommande anspråk** i listrutan och klickar sedan på **Nästa**.
   ![Guiden Lägg till anspråksregel för transformering](./media/howto-mfa-adfs/trustedip3.png)
6. I rutan bredvid Anspråksregelns namn ger du regeln ett namn. Exempel: InsideCorpNet.
7. Välj **Inom företagsnätverket** i listrutan bredvid Typ av inkommande anspråk.
   ![Lägger till i företags nätverks anspråk](./media/howto-mfa-adfs/trustedip4.png)
8. Klicka på **Slutför**.
9. För Utfärdande av transformeringsregler klickar du på **Lägg till regel**.
10. I guiden Lägg till anspråksregel för transformering väljer du **Skicka anspråk med hjälp av en anpassad regel** i listrutan och klickar sedan på **Nästa**.
11. I rutan under Anspråksregelns namn skriver du *Håll användarna inloggade*.
12. I rutan Anpassad regel anger du:

```ad-fs-claim-rule
        c:[Type == "http://schemas.microsoft.com/2014/03/psso"]
            => issue(claim = c);
    ![Create custom claim to keep users signed in](./media/howto-mfa-adfs/trustedip5.png)
```

13. Klicka på **Slutför**.
14. Klicka på **Applicera**.
15. Klicka på **OK**.
16. Stäng AD FS-hantering.

### <a name="configure-azure-multi-factor-authentication-trusted-ips-with-federated-users"></a>Konfigurera tillförlitliga IP-adresser med federerade användare i Azure Multi-Factor Authentication

När nu anspråken är på plats kan vi konfigurera tillförlitliga IP-adresser.

1. Logga in på [Azure-portalen](https://portal.azure.com).
2. Välj **Azure Active Directory**  >  **Security**  >  platser för**villkorlig åtkomst**  >  **med**säkerhet
3. Från bladet **villkorlig åtkomst – namngivna platser** väljer du **Konfigurera MFA-betrodda IP-adresser**

   ![Namngivna platser för villkorlig åtkomst i Azure AD Konfigurera MFA-betrodda IP-adresser](./media/howto-mfa-adfs/trustedip6.png)

4. På sidan Tjänstinställningar, under **Tillförlitliga IP-adresser** väljer du **Hoppa över multi-factor authentication för förfrågningar från federerade användare som kommer från mitt intranät**.  
5. Klicka på **Spara**.

Klart! I det här läget bör federerade Microsoft 365 användare bara behöva använda MFA när ett anspråk härstammar från utanför företags intranätet.
