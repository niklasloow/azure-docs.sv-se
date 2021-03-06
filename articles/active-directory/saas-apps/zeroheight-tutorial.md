---
title: 'Självstudie: Azure Active Directory integration med enkel inloggning (SSO) med zeroheight | Microsoft Docs'
description: Lär dig hur du konfigurerar enkel inloggning mellan Azure Active Directory och zeroheight.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/09/2020
ms.author: jeedes
ms.openlocfilehash: bcfd9e1b132ef47c83d028acf5e2bcb3fc637ef5
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91369390"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-zeroheight"></a>Självstudie: Azure Active Directory integration med enkel inloggning (SSO) med zeroheight

I den här självstudien får du lära dig hur du integrerar zeroheight med Azure Active Directory (Azure AD). När du integrerar zeroheight med Azure AD kan du:

* Kontroll i Azure AD som har åtkomst till zeroheight.
* Gör det möjligt för användarna att logga in automatiskt till zeroheight med sina Azure AD-konton.
* Hantera dina konton på en central plats – Azure Portal.

## <a name="prerequisites"></a>Förutsättningar

För att komma igång behöver du följande objekt:

* En Azure AD-prenumeration. Om du inte har någon prenumeration kan du få ett [kostnads fritt konto](https://azure.microsoft.com/free/).
* zeroheight för enkel inloggning (SSO) aktive rad.

## <a name="scenario-description"></a>Scenariobeskrivning

I den här självstudien konfigurerar och testar du Azure AD SSO i en test miljö.

* zeroheight stöder **SP** -INITIERAd SSO

## <a name="adding-zeroheight-from-the-gallery"></a>Lägga till zeroheight från galleriet

Om du vill konfigurera integreringen av zeroheight i Azure AD måste du lägga till zeroheight från galleriet i listan över hanterade SaaS-appar.

1. Logga in på Azure Portal med antingen ett arbets-eller skol konto eller en personlig Microsoft-konto.
1. I det vänstra navigerings fönstret väljer du tjänsten **Azure Active Directory** .
1. Navigera till **företags program** och välj sedan **alla program**.
1. Välj **nytt program**om du vill lägga till ett nytt program.
1. I avsnittet **Lägg till från galleriet** , skriver du **zeroheight** i sökrutan.
1. Välj **zeroheight** från resultat panelen och Lägg sedan till appen. Vänta några sekunder medan appen läggs till i din klient organisation.


## <a name="configure-and-test-azure-ad-sso-for-zeroheight"></a>Konfigurera och testa Azure AD SSO för zeroheight

Konfigurera och testa Azure AD SSO med zeroheight med hjälp av en test användare som heter **B. Simon**. För att SSO ska fungera måste du upprätta en länk relation mellan en Azure AD-användare och den relaterade användaren i zeroheight.

Om du vill konfigurera och testa Azure AD SSO med zeroheight, slutför du följande Bygg stenar:

1. **[Konfigurera Azure AD SSO](#configure-azure-ad-sso)** – så att användarna kan använda den här funktionen.
    1. **[Skapa en Azure AD-test](#create-an-azure-ad-test-user)** för att testa enkel inloggning med Azure AD med B. Simon.
    1. **[Tilldela Azure AD-testuser](#assign-the-azure-ad-test-user)** -för att aktivera B. Simon för att använda enkel inloggning med Azure AD.
1. **[Konfigurera ZEROHEIGHT SSO](#configure-zeroheight-sso)** – för att konfigurera inställningarna för enkel inloggning på program sidan.
    1. **[Skapa zeroheight test User](#create-zeroheight-test-user)** -om du vill ha en motsvarighet till B. Simon i zeroheight som är länkad till Azure AD-representation av användare.
1. **[Testa SSO](#test-sso)** – för att kontrol lera om konfigurationen fungerar.

## <a name="configure-azure-ad-sso"></a>Konfigurera Azure AD SSO

Följ de här stegen för att aktivera Azure AD SSO i Azure Portal.

1. I Azure Portal går du till sidan för program integrering i **zeroheight** , letar upp avsnittet **Hantera** och väljer **enkel inloggning**.
1. På sidan **Välj metod för enkel inloggning** väljer du **SAML**.
1. På sidan **Konfigurera enkel inloggning med SAML** klickar du på ikonen Redigera/penna för **grundläggande SAML-konfiguration** för att redigera inställningarna.

   ![Redigera grundläggande SAML-konfiguration](common/edit-urls.png)

1. I avsnittet **grundläggande SAML-konfiguration** anger du värden för följande fält:

    a. I rutan **Inloggnings-URL** anger du följande URL: `https://zeroheight.com/sso`

    b. I textrutan **Identifierare (entitets-ID)** anger du en URL enligt följande mönster: `zeroheight:<CUSTOM_ID>`

    c. Skriv en URL i text rutan **svars-URL** med följande mönster: `https://zeroheight.com/sso/acs/<CUSTOM_ID>`

    > [!NOTE]
    > Dessa värden är inte verkliga. Uppdatera dessa värden med den faktiska identifieraren och svars-URL. Kontakta [Zeroheight client support team](mailto:support@zeroheight.com) för att hämta dessa värden. Du kan även se mönstren som visas i avsnittet **Grundläggande SAML-konfiguration** i Azure-portalen.

1. zeroheight-programmet förväntar sig SAML-intyg i ett särskilt format, vilket innebär att du kan lägga till anpassade mappningar av attribut i konfigurationen för SAML-token. I följande skärmbild visas listan över standardattribut.

    ![image](common/default-attributes.png)

1. Utöver ovan förväntar sig zeroheight-programmet att fler attribut skickas tillbaka i SAML-svar, som visas nedan. Dessa attribut är också förifyllda, men du kan granska dem enligt dina krav.
    
    | Name |  Källattribut|
    | ---------- | --------- |
    | e-post | user.mail |

1. På sidan **Konfigurera enkel inloggning med SAML** , i avsnittet **SAML-signeringscertifikat** , klickar du på Kopiera för att kopiera **URL: en för appens Federations-metadata** och spara den på din dator.

    ![Länk för nedladdning av certifikatet](common/copy-metadataurl.png)

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

I det här avsnittet ska du aktivera B. Simon för att använda enkel inloggning med Azure genom att bevilja åtkomst till zeroheight.

1. I Azure Portal väljer du **företags program**och väljer sedan **alla program**.
1. I listan program väljer du **zeroheight**.
1. På sidan Översikt för appen letar du reda på avsnittet **Hantera** och väljer **användare och grupper**.
1. Välj **Lägg till användare**och välj sedan **användare och grupper** i dialog rutan **Lägg till tilldelning** .
1. I dialog rutan **användare och grupper** väljer du **B. Simon** från listan användare och klickar sedan på knappen **Välj** längst ned på skärmen.
1. Om du förväntar dig att en roll ska tilldelas användarna kan du välja den från List rutan **Välj en roll** . Om ingen roll har kon figurer ATS för den här appen ser du rollen "standard åtkomst" vald.
1. Klicka på knappen **tilldela** i dialog rutan **Lägg till tilldelning** .

## <a name="configure-zeroheight-sso"></a>Konfigurera zeroheight SSO

Om du vill konfigurera enkel inloggning på **zeroheight** sida måste du skicka **URL: en för appens Federations-metadata** till [support teamet för zeroheight](mailto:support@zeroheight.com). De anger inställningen så att SAML SSO-anslutningen ställs in korrekt på båda sidorna.

### <a name="create-zeroheight-test-user"></a>Skapa zeroheight test användare

I det här avsnittet skapar du en användare som heter Britta Simon i zeroheight. Arbeta med [zeroheight support team](mailto:support@zeroheight.com) för att lägga till användare i zeroheight-plattformen. Användare måste skapas och aktiveras innan du använder enkel inloggning.

## <a name="test-sso"></a>Testa SSO 

I det här avsnittet ska du testa Azure AD-konfigurationen för enkel inloggning med följande alternativ. 

1. Klicka på **testa det här programmet** i Azure Portal. Detta omdirigeras till zeroheight-inloggnings-URL där du kan starta inloggnings flödet. 

2. Gå till zeroheight-inloggnings-URL: en direkt och starta inloggnings flödet därifrån.

3. Du kan använda Microsoft Access-panelen. När du klickar på panelen zeroheight i åtkomst panelen omdirigeras den till zeroheight-inloggnings-URL. Mer information om åtkomstpanelen finns i [introduktionen till åtkomstpanelen](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="next-steps"></a>Efterföljande moment

När du har konfigurerat zeroheight kan du genomdriva session Control, som skyddar exfiltrering och intrånget för organisationens känsliga data i real tid. Kontroll av sessionen sträcker sig från villkorlig åtkomst. [Lär dig hur du tvingar fram en session med Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

