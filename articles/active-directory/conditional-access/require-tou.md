---
title: Villkorlig åtkomst kräver användnings villkor – Azure Active Directory
description: I den här snabb starten får du lära dig hur du kan kräva att användnings villkoren godkänns innan åtkomst till valda molnappar beviljas av Azure Active Directory villkorlig åtkomst.
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: quickstart
ms.date: 11/21/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: 53f99456449e778ff7c1fd3ab096e1afd3562c1e
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/27/2020
ms.locfileid: "88948869"
---
# <a name="quickstart-require-terms-of-use-to-be-accepted-before-accessing-cloud-apps"></a>Snabb start: Kräv användnings villkor för att godkännas innan du får åtkomst till molnappar

Innan du får åtkomst till vissa molnappar i din miljö kan du vilja få tillåtelse från användare i form av att godkänna dina användnings villkor (ToU). Azure Active Directory (Azure AD) villkorlig åtkomst ger dig följande:

- En enkel metod för att konfigurera ToU
- Alternativet för att kräva att du accepterar användnings villkoren via en princip för villkorlig åtkomst  

Den här snabb starten visar hur du konfigurerar en [princip för villkorlig åtkomst i Azure AD](./overview.md) som kräver att en ToU godkänns för en vald molnbaserad app i din miljö.

![Skapa princip](./media/require-tou/5555.png)

Om du inte har någon Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="prerequisites"></a>Förutsättningar

För att slutföra scenariot i den här snabb starten behöver du:

- **Åtkomst till en Azure AD Premium-utgåva** – villkorlig åtkomst i Azure AD är en Azure AD Premium funktion.
- **Ett test konto med namnet Isabella Simonsen** – om du inte vet hur du skapar ett test konto läser du [lägga till molnbaserade användare](../fundamentals/add-users-azure-active-directory.md#add-a-new-user).

## <a name="test-your-sign-in"></a>Testa din inloggning

Målet med det här steget är att få en överblick över inloggnings upplevelsen utan en princip för villkorlig åtkomst.

**Så här testar du inloggningen:**

1. Logga in på [Azure Portal](https://portal.azure.com/) som Isabella Simonsen.
1. Logga ut.

## <a name="create-your-terms-of-use"></a>Skapa dina användnings villkor

Det här avsnittet innehåller stegen för att skapa en exempel-ToU. När du skapar en ToU väljer du ett värde för **tvinga med mallar för villkorsstyrd åtkomst**. Om du väljer **anpassad princip** öppnas dialog rutan för att skapa en ny princip för villkorlig åtkomst så snart din ToU har skapats.

**Så här skapar du användnings villkoren:**

1. Skapa ett nytt dokument i Microsoft Word.
1. Skriv **Mina användnings villkor**och spara sedan dokumentet på datorn som **mytou.pdf**.
1. Logga in på [Azure Portal](https://portal.azure.com) som global administratör, säkerhets administratör eller en administratör för villkorlig åtkomst.
1. Klicka på **Azure Active Directory**i Azure Portal i det vänstra navigerings fältet.

   ![Azure Active Directory](./media/require-tou/02.png)

1. Klicka på **villkorlig åtkomst**i avsnittet **säkerhet** på sidan **Azure Active Directory** .

   ![Villkorlig åtkomst](./media/require-tou/03.png)

1. I avsnittet **Hantera** klickar du på **användningsvillkor**.

   ![Villkor för användning](./media/require-tou/04.png)

1. Klicka på **nya villkor**i menyn högst upp.

   ![Villkor för användning](./media/require-tou/05.png)

1. På sidan **nya** användnings villkor:

   ![Villkor för användning](./media/require-tou/112.png)

   1. Skriv **min tou**i text rutan **namn** .
   1. Skriv **min tou**i text rutan **visnings namn** .
   1. Överför dina användnings villkor för PDF-filen.
   1. Som **språk**väljer du **engelska**.
   1. Välj **på**som **Kräv att användarna expanderar användnings villkoren**.
   1. Som **tvingande med principmallar för villkorlig åtkomst**väljer du **anpassad princip**.
   1. Klicka på **Skapa**.

## <a name="create-your-conditional-access-policy"></a>Skapa en princip för villkorlig åtkomst

I det här avsnittet visas hur du skapar den nödvändiga principen för villkorlig åtkomst. Scenariot i den här snabb starten använder:

- Azure Portal som plats hållare för en molnbaserad app som kräver att dina ToU godkänns. 
- Exempel användaren för att testa principen för villkorlig åtkomst.  

Ange följande i principen:

| Inställning | Värde |
| --- | --- |
| Användare och grupper | Isabella Simonsen |
| Molnappar | Microsoft Azure hantering |
| Bevilja åtkomst | Mina TOU |

![Skapa princip](./media/require-tou/1234.png)

**Så här konfigurerar du en princip för villkorlig åtkomst:**

1. På sidan **nytt** i text rutan **namn** skriver du **Kräv tou för Isabella**.

   ![Namn](./media/require-tou/71.png)

1. I avsnittet **tilldelning** klickar du på **användare och grupper**.

   ![Användare och grupper](./media/require-tou/06.png)

1. På sidan **användare och grupper** :

   ![Användare och grupper](./media/require-tou/24.png)

   1. Klicka på **Välj användare och grupper**och välj sedan **användare och grupper**.
   1. Klicka på **Välj**.
   1. På sidan **Välj** väljer du **Isabella Simonsen**och klickar sedan på **Välj**.
   1. På sidan **användare och grupper** klickar du på **Slutför**.
1. Klicka på **molnappar**.

   ![Molnappar](./media/require-tou/08.png)

1. På sidan **Cloud Apps** :

   ![Välj molnappar](./media/require-tou/26.png)

   1. Klicka på **Välj appar**.
   1. Klicka på **Välj**.
   1. På sidan **Välj** väljer du **Microsoft Azure hantering**och klickar sedan på **Välj**.
   1. På sidan **molnappar** klickar du på **Slutför**.
1. I avsnittet **åtkomst kontroller** klickar du på **bevilja**.

   ![Åtkomstkontroller](./media/require-tou/10.png)

1. På sidan **beviljande** :

   ![Bevilja](./media/require-tou/111.png)

   1. Välj **Bevilja åtkomst**.
   1. Välj **min tou**.
   1. Klicka på **Välj**.
1. I avsnittet **Aktivera princip** klickar du **på på**.

   ![Aktivera princip](./media/require-tou/18.png)

1. Klicka på **Skapa**.

## <a name="evaluate-a-simulated-sign-in"></a>Utvärdera en simulerad inloggning

Nu när du har konfigurerat din princip för villkorlig åtkomst vill du förmodligen veta om den fungerar som förväntat. Som ett första steg använder du den villkorliga åtkomsten vad gör om-princip verktyget för att simulera en inloggning av test användaren. Simuleringen uppskattar inloggningens inverkan på dina principer och genererar en simuleringsrapport.  

För att initiera utvärderings verktyget för **What If** policy, ange:

- **Isabella Simonsen** som användare
- **Microsoft Azure hantering** som molnbaserad app

Om du klickar på **What If** skapas en simulerings rapport som visar:

- **KRÄV tou för Isabella** under **principer som ska gälla**
- **Mina tou** som **beviljade kontroller**.

![Verktyget om princip](./media/require-tou/79.png)

**Så här utvärderar du principen för villkorlig åtkomst:**

1. På sidan [villkorlig åtkomst – principer](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ConditionalAccessBlade/Policies) går du till menyn högst upp och klickar på **What If**.  

   ![What If](./media/require-tou/14.png)

1. Klicka på **användare**, Välj **Isabella Simonsen**och klicka sedan på **Välj**.

   ![Användare](./media/require-tou/15.png)

1. Så här väljer du en molnbaserad app:

   ![Molnappar](./media/require-tou/16.png)

   1. Klicka på **molnappar**.
   1. På **sidan molnappar**klickar du på **Välj appar**.
   1. Klicka på **Välj**.
   1. På sidan **Välj** väljer du **Microsoft Azure hantering**och klickar sedan på **Välj**.
   1. På sidan molnappar klickar du på **Slutför**.
1. Klicka på **What If**.

## <a name="test-your-conditional-access-policy"></a>Testa din princip för villkorlig åtkomst

I föregående avsnitt har du lärt dig hur du utvärderar en simulerad inloggning. Förutom en simulering bör du också testa principen för villkorlig åtkomst för att säkerställa att den fungerar som förväntat.

Testa principen genom att försöka logga in på din [Azure Portal](https://portal.azure.com) med ditt **Isabella Simonsen** test konto. Du bör se en dialog ruta som kräver att du godkänner användnings villkoren.

![Villkor för användning](./media/require-tou/57.png)

## <a name="clean-up-resources"></a>Rensa resurser

Ta bort test användaren och principen för villkorlig åtkomst när de inte längre behövs:

- Om du inte vet hur du tar bort en Azure AD-användare kan du läsa [ta bort användare från Azure AD](../fundamentals/add-users-azure-active-directory.md#delete-a-user).
- Om du vill ta bort principen väljer du principen och klickar sedan på **ta bort** i verktygsfältet snabb åtkomst.

    ![Multi-Factor Authentication](./media/require-tou/33.png)

- Om du vill ta bort användnings villkoren markerar du det och klickar sedan på **ta bort termer** i verktygsfältet överst.

    ![Multi-Factor Authentication](./media/require-tou/29.png)

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Kräv MFA för specifika appar](../authentication/tutorial-enable-azure-mfa.md)