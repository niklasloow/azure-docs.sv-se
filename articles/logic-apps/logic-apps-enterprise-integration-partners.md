---
title: Lägg till handels partner för B2B-integreringar
description: Skapa handels partner i integrations kontot som ska användas med Azure Logic Apps
services: logic-apps
ms.suite: integration
author: divyaswarnkar
ms.author: divswa
ms.reviewer: jonfan, estfan, logicappspm
ms.topic: article
ms.date: 06/22/2019
ms.openlocfilehash: e58cbe85f30ea09adde45d55bb7b80c710c45495
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "74792438"
---
# <a name="add-trading-partners-to-integration-accounts-for-azure-logic-apps"></a>Lägga till handelspartner till integrationskonton för Azure Logic Apps

I [Azure Logic Apps](../logic-apps/logic-apps-overview.md)kan du skapa automatiserade integrations arbets flöden för Business-to-Business (B2B) genom att använda ett [integrations konto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) med dina Logic Apps. För att representera din organisation och andra, skapar du och lägger till handels partner som artefakter till ditt integrations konto. Partner är entiteter som deltar i B2B-transaktioner och utbyter meddelanden med varandra.

Innan du skapar dessa partner ska du se till att diskutera och dela information med dina partner om hur du identifierar och validerar de meddelanden som de andra skickar. När du har samtycker till dessa uppgifter är du redo att skapa partner i ditt integrations konto.

## <a name="partner-roles-in-integration-accounts"></a>Partner roller i integrations konton

Om du vill definiera information om de meddelanden som utbyts med dina partner skapar du och lägger till [avtal](../logic-apps/logic-apps-enterprise-integration-agreements.md) som artefakter till ditt integrations konto. Avtal kräver minst två partner i ditt integrations konto. Din organisation är alltid *värd partner* i avtalet. Organisationen som utbyter meddelanden med din organisation är *gäst partnern*. En gäst partner kan vara ett annat företag eller till och med en avdelning i din organisation. När du har lagt till dessa partner kan du skapa ett avtal.

I ett avtal anger du information om hur du hanterar inkommande och utgående meddelanden från värd partner perspektivet. För inkommande meddelanden anger **mottagnings inställningarna** hur värd partnern tar emot meddelanden från gäst partnern i avtalet. För utgående meddelanden anger **sändnings inställningarna** hur värd partner skickar meddelanden till gäst partnern.

## <a name="prerequisites"></a>Krav

* En Azure-prenumeration. Om du inte har någon Azure-prenumeration ännu kan du [Registrera dig för ett kostnads fritt Azure-konto](https://azure.microsoft.com/free/).

* Ett [integrations konto](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) för att lagra dina partners, avtal och andra B2B-artefakter. Det här integrations kontot måste vara associerat med din Azure-prenumeration.

## <a name="create-partner"></a>Skapa partner

1. Logga in på [Azure-portalen](https://portal.azure.com).

1. På huvud menyn i Azure väljer du **alla tjänster**. I rutan Sök anger du "integration" och väljer **integrations konton**.

   ![Välj integrations konton](./media/logic-apps-enterprise-integration-partners/find-integration-accounts.png)

1. Under **integrations konton**väljer du det integrations konto där du vill lägga till dina partner.

   ![Välj integrations konto](./media/logic-apps-enterprise-integration-partners/select-integration-account.png)

1. Välj panelen **partner** .

   ![Välj panelen partner](./media/logic-apps-enterprise-integration-partners/choose-partners.png)

1. Under **partner**väljer du **Lägg till**. Under **Lägg till partner**anger du partnerns information som beskrivs i tabellen nedan.

   ![Välj "Lägg till" och ange partner information](./media/logic-apps-enterprise-integration-partners/add-partners.png)

   | Egenskap | Krävs | Beskrivning |
   |----------|----------|-------------|
   | **Namn** | Ja | Partnerns namn |
   | **Kvalificerarattribut** | Ja | Den autentiserande text som ger organisationer unika affärs identiteter, till exempel **D-U-N-S (Dun & Bradstreet)**. <p>Partner kan välja en ömsesidigt definierad affärs identitet. I dessa scenarier väljer du **ömsesidigt definierade** för EDIFACT eller **ömsesidigt definierat (X12)** för X12. <p>För RosettaNet väljer du endast **duns**, som är standard. |
   | **Värde** | Ja | Ett värde som identifierar de dokument som dina Logic Apps tar emot. <p>För RosettaNet måste värdet vara ett 9-siffrigt tal som motsvarar DUNS-numret. |
   ||||

   > [!NOTE]
   > För partner som använder RosettaNet kan du ange ytterligare information genom att först skapa dessa partner och sedan [redigera dem efteråt](#edit-partner).

1. När du är klar väljer du **OK**.

   Din nya partner visas nu i listan **partner** . Dessutom uppdaterar panelen **partners** det aktuella antalet partner.

   ![Ny partner](./media/logic-apps-enterprise-integration-partners/new-partner.png)

<a name="edit-partner"></a>

## <a name="edit-partner"></a>Redigera partner

1. I [Azure Portal](https://portal.azure.com)letar du reda på och väljer ditt integrations konto.
Välj panelen **partner** .

   ![Välj panelen partner](./media/logic-apps-enterprise-integration-partners/edit.png)

1. Under **partner**väljer du den partner som du vill redigera och väljer **Redigera**. Under **Redigera**, gör du ändringarna.

   ![Gör och spara dina ändringar](./media/logic-apps-enterprise-integration-partners/edit-partner.png)

   För RosettaNet, under **Egenskaper för RosettaNet-partner**, kan du ange ytterligare information:

   | Egenskap | Krävs | Beskrivning |
   |----------|----------|-------------|
   | **Partner klassificering** | No | Partnerns organisations typ |
   | **Kod för leverans kedja** | No | Partnerns kod för leverans kedja, till exempel "informations teknik" eller "elektroniska komponenter" |
   | **Kontakt namn** | No | Partnerns kontakt namn |
   | **E-post** | No | Partnerns e-postadress |
   | **Fax** | No | Partnerns Fax nummer |
   | **Telefon** | No | Partnerns telefonnummer |
   ||||

1. När du är klar väljer du **OK** för att spara ändringarna.

## <a name="delete-partner"></a>Ta bort partner

1. I [Azure Portal](https://portal.azure.com)letar du reda på och väljer ditt integrations konto. Välj panelen **partner** .

   ![Välj panelen partner](./media/logic-apps-enterprise-integration-partners/choose-partners-to-delete.png)

1. Under **partner**väljer du den partner som du vill ta bort. Välj **Ta bort**.

   ![Ta bort partner](./media/logic-apps-enterprise-integration-partners/delete-partner.png)

## <a name="next-steps"></a>Nästa steg

* Läs mer om [avtal](../logic-apps/logic-apps-enterprise-integration-agreements.md)