---
title: 'Självstudie: Azure Active Directory integration med enkel inloggning (SSO) med SonarQube | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och SonarQube.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/18/2019
ms.author: jeedes
ms.openlocfilehash: b3790c52598b2857f56cac54acad156b3ba0bf51
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2020
ms.locfileid: "88545018"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sonarqube"></a>Självstudie: Azure Active Directory integration med enkel inloggning (SSO) med SonarQube

I den här självstudien får du lära dig hur du integrerar SonarQube med Azure Active Directory (Azure AD). När du integrerar SonarQube med Azure AD kan du:

* Kontroll i Azure AD som har åtkomst till SonarQube.
* Gör det möjligt för användarna att logga in automatiskt till SonarQube med sina Azure AD-konton.
* Hantera dina konton på en central plats – Azure Portal.

Mer information om SaaS app integration med Azure AD finns i [Vad är program åtkomst och enkel inloggning med Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Förutsättningar

För att komma igång behöver du följande objekt:

* En Azure AD-prenumeration. Om du inte har någon prenumeration kan du få ett [kostnads fritt konto](https://azure.microsoft.com/free/).
* SonarQube för enkel inloggning (SSO) aktive rad.

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien konfigurerar och testar du Azure AD SSO i en test miljö.

* SonarQube stöder **SP** -INITIERAd SSO

> [!NOTE]
> ID för det här programmet är ett fast sträng värde så att endast en instans kan konfigureras i en klient.

## <a name="adding-sonarqube-from-the-gallery"></a>Lägga till SonarQube från galleriet

Om du vill konfigurera integreringen av SonarQube i Azure AD måste du lägga till SonarQube från galleriet i listan över hanterade SaaS-appar.

1. Logga in på [Azure Portal](https://portal.azure.com) med antingen ett arbets-eller skol konto eller en personlig Microsoft-konto.
1. I det vänstra navigerings fönstret väljer du tjänsten **Azure Active Directory** .
1. Navigera till **företags program** och välj sedan **alla program**.
1. Välj **nytt program**om du vill lägga till ett nytt program.
1. I avsnittet **Lägg till från galleriet** , skriver du **SonarQube** i sökrutan.
1. Välj **SonarQube** från resultat panelen och Lägg sedan till appen. Vänta några sekunder medan appen läggs till i din klient organisation.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sonarqube"></a>Konfigurera och testa enkel inloggning med Azure AD för SonarQube

Konfigurera och testa Azure AD SSO med SonarQube med hjälp av en test användare som heter **B. Simon**. För att SSO ska fungera måste du upprätta en länk relation mellan en Azure AD-användare och den relaterade användaren i SonarQube.

Om du vill konfigurera och testa Azure AD SSO med SonarQube, slutför du följande Bygg stenar:

1. **[Konfigurera Azure AD SSO](#configure-azure-ad-sso)** – så att användarna kan använda den här funktionen.
    1. **[Skapa en Azure AD-test](#create-an-azure-ad-test-user)** för att testa enkel inloggning med Azure AD med B. Simon.
    1. **[Tilldela Azure AD-testuser](#assign-the-azure-ad-test-user)** -för att aktivera B. Simon för att använda enkel inloggning med Azure AD.
1. **[Konfigurera SONARQUBE SSO](#configure-sonarqube-sso)** – för att konfigurera inställningarna för enkel inloggning på program sidan.
    1. **[Skapa SonarQube test User](#create-sonarqube-test-user)** -om du vill ha en motsvarighet till B. Simon i SonarQube som är länkad till Azure AD-representation av användare.
1. **[Testa SSO](#test-sso)** – för att kontrol lera om konfigurationen fungerar.

## <a name="configure-azure-ad-sso"></a>Konfigurera Azure AD SSO

Följ de här stegen för att aktivera Azure AD SSO i Azure Portal.

1. I [Azure Portal](https://portal.azure.com/)går du till sidan för program integrering i **SonarQube** , letar upp avsnittet **Hantera** och väljer **enkel inloggning**.
1. På sidan **Välj metod för enkel inloggning** väljer du **SAML**.
1. På sidan **Konfigurera enkel inloggning med SAML** klickar du på ikonen Redigera/penna för **grundläggande SAML-konfiguration** för att redigera inställningarna.

   ![Redigera grundläggande SAML-konfiguration](common/edit-urls.png)

1. I avsnittet **grundläggande SAML-konfiguration** anger du värden för följande fält:

    I rutan **Inloggnings-URL** anger du en URL: 

    * **För produktions miljö**

        `https://servicessonar.corp.microsoft.com/`

    * **För utvecklings miljö**

        `https://servicescode-dev.westus.cloudapp.azure.com`

1. På sidan **Konfigurera enkel inloggning med SAML** , i avsnittet **SAML-signeringscertifikat** , Sök efter **certifikat (base64)** och välj **Ladda ned** för att ladda ned certifikatet och spara det på din dator.

    ![Länk för nedladdning av certifikatet](common/certificatebase64.png)

1. I avsnittet **Konfigurera SonarQube** kopierar du lämpliga URL: er baserat på ditt krav.

    ![Kopiera konfigurations-URL:er](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Skapa en Azure AD-testanvändare

I det här avsnittet ska du skapa en test användare i Azure Portal som kallas B. Simon.

1. I den vänstra rutan i Azure Portal väljer du **Azure Active Directory**, väljer **användare**och väljer sedan **alla användare**.
1. Välj **ny användare** överst på skärmen.
1. I **användar** egenskaperna följer du de här stegen:
   1. I **Namn**-fältet skriver du `B.Simon`.  
   1. I fältet **användar namn** anger du username@companydomain.extension . Till exempel `B.Simon@contoso.com`.
   1. Markera kryssrutan **Visa lösenord** och skriv sedan ned det värde som visas i rutan **Lösenord**.
   1. Klicka på **Skapa**.

### <a name="assign-the-azure-ad-test-user"></a>Tilldela Azure AD-testanvändaren

I det här avsnittet ska du aktivera B. Simon för att använda enkel inloggning med Azure genom att bevilja åtkomst till SonarQube.

1. I Azure Portal väljer du **företags program**och väljer sedan **alla program**.
1. I listan program väljer du **SonarQube**.
1. På sidan Översikt för appen letar du reda på avsnittet **Hantera** och väljer **användare och grupper**.

   ![Länken ”Användare och grupper”](common/users-groups-blade.png)

1. Välj **Lägg till användare**och välj sedan **användare och grupper** i dialog rutan **Lägg till tilldelning** .

    ![Länken Lägg till användare](common/add-assign-user.png)

1. I dialog rutan **användare och grupper** väljer du **B. Simon** från listan användare och klickar sedan på knappen **Välj** längst ned på skärmen.
1. Om du förväntar dig ett roll värde i SAML Assertion, i dialog rutan **Välj roll** , väljer du lämplig roll för användaren i listan och klickar sedan på knappen **Välj** längst ned på skärmen.
1. Klicka på knappen **tilldela** i dialog rutan **Lägg till tilldelning** .

## <a name="configure-sonarqube-sso"></a>Konfigurera SonarQube SSO

1. Öppna ett nytt webbläsarfönster och logga in på din SonarQube företags webbplats som administratör.

2. Installera SAML-plugin-programmet från SonarQube-marknads platsen.

3. Klicka på **administratör** längst upp till vänster på sidan och gå sedan till **SAML**.

4. Utför följande steg på sidan **SAML** :

    ![SonarQube-konfiguration](./media/sonarqube-tutorial/config01.png)

    a. Växla alternativet **aktive rad** till **Ja**.

    b. I text rutan **program-ID** anger du namnet som **SonarQube**.

    c. I text rutan **namn på Provider** anger du namnet som **SAML**.

    d. I text rutan **Provider-ID** klistrar du in värdet för **Azure AD-identifierare**, som du har kopierat från Azure Portal.

    e. I text rutan **URL för SAML-inloggning** klistrar du in värdet för **inloggnings-URL: en**som du har kopierat från Azure Portal.

    f. Öppna det Base64-kodade certifikatet i anteckningar, kopiera dess innehåll och klistra in det i text rutan **Provider-certifikat** .

    ex. I text rutan **attribut för SAML-användar inloggning** anger du värdet `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` .

    h. I text rutan **SAML User Name-attribut** anger du värdet `http://schemas.microsoft.com/identity/claims/displayname` .

    i. Ange värdet i text rutan **e-postattribut för SAML-användare** `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name` .

    j. Klicka på **Spara**.

### <a name="create-sonarqube-test-user"></a>Skapa SonarQube test användare

I det här avsnittet skapar du en användare som heter B. Simon i SonarQube. Arbeta med [SonarQube-klientens support team](https://www.sonarsource.com/support/) för att lägga till användare i SonarQube-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning. 

## <a name="test-sso"></a>Testa SSO 

I det här avsnittet testar du konfigurationen för enkel inloggning Azure AD med hjälp av åtkomstpanelen.

När du klickar på panelen SonarQube på åtkomst panelen, bör du loggas in automatiskt på den SonarQube som du ställer in SSO för. Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ytterligare resurser

- [ Lista över självstudier om hur du integrerar SaaS-appar med Azure Active Directory ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Vad är programåtkomst och enkel inloggning med Azure Active Directory? ](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Vad är villkorlig åtkomst i Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Prova SonarQube med Azure AD](https://aad.portal.azure.com/)

