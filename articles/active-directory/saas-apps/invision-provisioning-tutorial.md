---
title: 'Självstudie: Konfigurera insikter för automatisk användar etablering med Azure Active Directory | Microsoft Docs'
description: Lär dig att automatiskt etablera och avetablera användar konton från Azure AD till insikt.
services: active-directory
documentationcenter: ''
author: Zhchia
writer: Zhchia
manager: beatrizd
ms.assetid: 72518dda-d485-45c8-849e-6b27ee09d9a8
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/25/2020
ms.author: Zhchia
ms.openlocfilehash: 7fde4a82ff3a9e0cfa67e8ce5afeebc000757b7c
ms.sourcegitcommit: 4a7a4af09f881f38fcb4875d89881e4b808b369b
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/04/2020
ms.locfileid: "89462062"
---
# <a name="tutorial-configure-invision-for-automatic-user-provisioning"></a>Självstudie: Konfigurera insikter för automatisk användar etablering

I den här självstudien beskrivs de steg som du måste utföra i både insikter och Azure Active Directory (Azure AD) för att konfigurera automatisk användar etablering. När Azure AD har kon figurer ATS etablerar och avetablerar Azure AD automatiskt användare och grupper till [insikter](https://www.invisionapp.com/) med Azure AD Provisioning-tjänsten. Viktig information om vad den här tjänsten gör, hur det fungerar och vanliga frågor finns i [Automatisera användar etablering och avetablering för SaaS-program med Azure Active Directory](../manage-apps/user-provisioning.md). 


## <a name="capabilities-supported"></a>Funktioner som stöds
> [!div class="checklist"]
> * Skapa användare i insikter
> * Ta bort användare i insikter när de inte behöver åtkomst längre
> * Behåll användarattribut synkroniserade mellan Azure AD och insikter
> * [Enkel inloggning](https://docs.microsoft.com/azure/active-directory/saas-apps/invision-tutorial) till insikt (krävs)

## <a name="prerequisites"></a>Krav

Det scenario som beskrivs i den här självstudien förutsätter att du redan har följande krav:

* [En Azure AD-klient](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant) 
* Ett användar konto i Azure AD med [behörighet](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) att konfigurera etablering (t. ex. program administratör, moln program administratör, program ägare eller global administratör). 
* Ett [insikt företags konto](https://www.invisionapp.com/enterprise) med SSO aktiverat.
* Ett användar konto i insikter med administratörs behörighet.

## <a name="step-1-plan-your-provisioning-deployment"></a>Steg 1. Planera etablerings distributionen
1. Läs om [hur etablerings tjänsten fungerar](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).
2. Ta reda på vem som kommer att vara inom [omfånget för etablering](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts).
3. Ta reda på vilka data som ska [mappas mellan Azure AD och insikter](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes). 

## <a name="step-2-configure-invision-to-support-provisioning-with-azure-ad"></a>Steg 2. Konfigurera insikter för att ge stöd åt etablering med Azure AD

1. Logga in på ditt [insikter Enterprise-konto](https://www.invisionapp.com/enterprise) som administratör eller ägare. Öppna **team inställnings** lådan längst ned till vänster och välj **Inställningar**.

   ![Konfiguration av SCIM-konfiguration](./media/invision-provisioning-tutorial/invision-scim-settings.png)

2. Välj **ändra** vid **användar etablering med inställningen scim** .

   ![SCIM etablerings inställningar](./media/invision-provisioning-tutorial/invision-provisioning-settings.png)

3. Välj växla för att aktivera SCIM-etablering. Observera att du måste konfigurera SSO först för att kunna aktivera SCIM:

   ![SCIM Aktivera etablering](./media/invision-provisioning-tutorial/enable-scim-provisioning.png)

4. Kopiera **scim API-URL** och Lägg till i URL: en `/scim/v2` . Kopiera autentiseringstoken **.** Spara värdena för senare användning i fälten klient- **URL** och **hemligt token** på fliken etablering i det insiktade programmet i Azure Portal.

   ![SCIM-åtkomsttoken](./media/invision-provisioning-tutorial/invision-access-token.png)


## <a name="step-3-add-invision-from-the-azure-ad-application-gallery"></a>Steg 3. Lägg till insikt från Azure AD-programgalleriet

Lägg till insikt från Azure AD-programgalleriet för att börja hantera etablering till insikter. Om du tidigare har konfigurerat insikten för SSO kan du använda samma program. Vi rekommenderar dock att du skapar en separat app när du testar integreringen från början. Lär dig mer om att lägga till ett program från galleriet [här](https://docs.microsoft.com/azure/active-directory/manage-apps/add-gallery-app). 

## <a name="step-4-define-who-will-be-in-scope-for-provisioning"></a>Steg 4. Definiera vem som ska finnas inom omfånget för etablering 

Med Azure AD Provisioning-tjänsten kan du definiera omfång som ska tillhandahållas baserat på tilldelning till programmet och eller baserat på attribut för användaren/gruppen. Om du väljer att omfånget som ska tillhandahållas till din app baserat på tilldelning kan du använda följande [steg](../manage-apps/assign-user-or-group-access-portal.md) för att tilldela användare och grupper till programmet. Om du väljer att omfånget som endast ska tillhandahållas baserat på attribut för användaren eller gruppen kan du använda ett omfångs filter enligt beskrivningen [här](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 

* När du tilldelar användare och grupper till insikter måste du välja en annan roll än **standard åtkomst**. Användare med standard åtkomst rollen undantas från etablering och markeras som inte faktiskt berättigade i etablerings loggarna. Om den enda rollen som är tillgänglig i programmet är standard åtkomst rollen kan du [Uppdatera applikations manifestet](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps) för att lägga till ytterligare roller. 

* Starta litet. Testa med en liten uppsättning användare och grupper innan de distribueras till alla. När omfång för etablering har angetts till tilldelade användare och grupper kan du styra detta genom att tilldela en eller två användare eller grupper till appen. När omfång är inställt på alla användare och grupper kan du ange ett [omfångs filter för attribut](https://docs.microsoft.com/azure/active-directory/manage-apps/define-conditional-rules-for-provisioning-user-accounts). 


## <a name="step-5-configure-automatic-user-provisioning-to-invision"></a>Steg 5. Konfigurera automatisk användar etablering till insikt 

Det här avsnittet vägleder dig genom stegen för att konfigurera Azure AD Provisioning-tjänsten för att skapa, uppdatera och inaktivera användare och/eller grupper i TestApp baserat på användar-och/eller grupp tilldelningar i Azure AD.

### <a name="to-configure-automatic-user-provisioning-for-invision-in-azure-ad"></a>Konfigurera automatisk användar etablering för insikt i Azure AD:

1. Logga in på [Azure-portalen](https://portal.azure.com). Välj **företags program**och välj sedan **alla program**.

    ![Bladet Företagsprogram](common/enterprise-applications.png)

2. I listan program väljer du **insikter**.

    ![Länken insikt i program listan](common/all-applications.png)

3. Välj fliken **etablering** .

    ![Fliken etablering](common/provisioning.png)

4. Ställ in **etablerings läget** på **automatiskt**.

    ![Etableringsläge](common/provisioning-automatic.png)

5. Under avsnittet **admin credentials** kan du läsa in scim API URL-värdet som hämtades tidigare i **klient-URL**: en. Mata in token för autentisering som hämtades tidigare i **hemlig token**. Klicka på **Testa anslutning** för att se till att Azure AD kan ansluta till en insikt. Om anslutningen Miss lyckas kontrollerar du att ditt insikts konto har administratörs behörighet och försöker igen.

    ![Admin-autentiseringsuppgifter](./media/inVision-provisioning-tutorial/provisioning.png)

6. I fältet **e-postavisering** anger du e-postadressen till den person eller grupp som ska få etablerings fel meddelanden och markerar kryss rutan **Skicka ett e-postmeddelande när ett fel inträffar** .

    ![E-postmeddelande](common/provisioning-notification-email.png)

7. Välj **Spara**.

8. Under avsnittet **mappningar** väljer du **etablera Azure Active Directory användare**.

9. Granska de användarattribut som synkroniseras från Azure AD till insikt i avsnittet **attribut-mappning** . Attributen som väljs som **matchande** egenskaper används för att matcha användar kontona i insikten för uppdaterings åtgärder. Om du väljer att ändra [matchande målattribut](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes)måste du se till att API: et för insikt stöder filtrering av användare baserat på attributet. Välj knappen **Spara** för att spara ändringarna.

   |Attribut|Typ|Stöds för filtrering|
   |---|---|---|
   |userName|Sträng|&check;|
   |aktiv|Boolesk|
   |e-postmeddelanden [typ EQ "Work"]. värde|Sträng|
   |Name. givenName|Sträng|
   |Name. familyName|Sträng|

10. Information om hur du konfigurerar omfångs filter finns i följande instruktioner i [kursen omfångs filter](../manage-apps/define-conditional-rules-for-provisioning-user-accounts.md).

11. Om du vill aktivera Azure AD Provisioning-tjänsten för insikt ändrar du **etablerings statusen** till **på** i avsnittet **Inställningar** .

    ![Etablerings status växlad på](common/provisioning-toggle-on.png)

12. Definiera de användare och/eller grupper som du vill etablera till insikten genom att välja önskade värden i **omfång** i avsnittet **Inställningar** .

    ![Etablerings omfång](common/provisioning-scope.png)

13. När du är redo att etablera klickar du på **Spara**.

    ![Etablerings konfigurationen sparas](common/provisioning-configuration-save.png)

Den här åtgärden startar den första synkroniseringen av alla användare och grupper som definierats i **omfånget** i avsnittet **Inställningar** . Den första cykeln tar längre tid att utföra än efterföljande cykler, vilket inträffar ungefär var 40: e minut, förutsatt att Azure AD Provisioning-tjänsten körs. 

## <a name="step-6-monitor-your-deployment"></a>Steg 6. Övervaka distributionen
När du har konfigurerat etableringen använder du följande resurser för att övervaka distributionen:

1. Använd [etablerings loggarna](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs) för att avgöra vilka användare som har etablerats eller har misslyckats
2. Kontrol lera [förlopps indikatorn](https://docs.microsoft.com/azure/active-directory/app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user) för att se status för etablerings cykeln och hur nära den är att slutföras
3. Om etablerings konfigurationen verkar vara i ett ohälsosamt tillstånd, kommer programmet att placeras i karantän. Lär dig mer om karantän tillstånd [här](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-quarantine-status).

## <a name="additional-resources"></a>Ytterligare resurser

* [Hantera användar konto etablering för företags program](../manage-apps/configure-automatic-user-provisioning-portal.md)
* [Vad är program åtkomst och enkel inloggning med Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Nästa steg

* [Lär dig hur du granskar loggar och hämtar rapporter om etablerings aktivitet](../manage-apps/check-status-user-account-provisioning.md)
