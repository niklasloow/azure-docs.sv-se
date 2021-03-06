---
title: Anslut till Common Data Service
description: Skapa och hantera Common Data Service poster med Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: jdaly, logicappspm
ms.topic: conceptual
ms.date: 05/08/2020
tags: connectors
ms.openlocfilehash: de85a61cbd699ec9ac2669f8abb6217254038de9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91334590"
---
# <a name="create-and-manage-records-in-common-data-service-by-using-azure-logic-apps"></a>Skapa och hantera poster i Common Data Service med Azure Logic Apps

Med [Azure Logic Apps](../logic-apps/logic-apps-overview.md) och [common data service Connector](/connectors/commondataservice/)kan du bygga automatiserade arbets flöden som hanterar poster i din [common data service](/powerapps/maker/common-data-service/data-platform-intro) databas. Dessa arbets flöden kan skapa poster, uppdatera poster och utföra andra åtgärder. Du kan också hämta information från Common Data Service-databasen och göra utdata tillgängliga för andra åtgärder som ska användas i din Logic app. När en post till exempel uppdateras i Common Data Service-databasen kan du skicka ett e-postmeddelande med hjälp av Office 365 Outlook Connector.

Den här artikeln visar hur du kan skapa en Logic app som skapar en uppgifts post när en ny leadpost skapas.

## <a name="prerequisites"></a>Förutsättningar

* En Azure-prenumeration. Om du heller inte har någon Azure-prenumeration kan du [registrera ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/).

* En [common data service miljö](/power-platform/admin/environments-overview), som är ett utrymme där din organisation lagrar, hanterar och delar affärs data och en common data service databas. Mer information finns i följande resurser:<p>

  * [Lär dig: kom igång med Common Data Service](/learn/modules/get-started-with-powerapps-common-data-service/)
  * [Översikt över Power Platform-miljöer](/power-platform/admin/environments-overview)

* Grundläggande information om [hur du skapar](../logic-apps/quickstart-create-first-logic-app-workflow.md) Logi Kap par och vilken Logic-app du vill använda för att komma åt posterna i din common data service databas. Om du vill starta din Logic app med en Common Data Service utlösare behöver du en tom Logic-app. Om du inte har använt Azure Logic Apps bör du gå igenom [snabb starten: skapa ditt första arbets flöde med hjälp av Azure Logic Apps](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="add-common-data-service-trigger"></a>Lägg till Common Data Service-utlösare

[!INCLUDE [Create connection general intro](../../includes/connectors-create-connection-general-intro.md)]

I det här exemplet lägger du till den Common Data Service utlösare som utlöses när en ny post skapas.

1. Öppna din tomma Logic-app i Logic App Designer, om den inte redan är öppen, i [Azure Portal](https://portal.azure.com).

1. Skriv `common data service` i sökrutan. I det här exemplet väljer du den här utlösaren under listan utlösare: **när en post skapas**

   ![Välj "Utlös när en post skapas"](./media/connect-common-data-service/select-when-record-created-trigger.png)

1. Logga in på Common Data Service om du uppmanas till det.

1. I utlösaren anger du information om miljön där du vill övervaka nya "leads"-poster, till exempel:

   ![Utlös information för miljö som ska övervakas](./media/connect-common-data-service/when-record-created-trigger-details.png)

   | Egenskap | Krävs | Beskrivning |
   |----------|----------|-------------|
   | **Miljö** | Yes | Miljön som ska övervakas, till exempel "Fabrikam Sales Production". Mer information finns i [Översikt över Power Platform-miljöer](/power-platform/admin/environments-overview). |
   | **Entitetsnamn** | Yes | Entiteten som ska övervakas, till exempel "leads" |
   | **Omfång** | Yes | Källan som skapade den nya posten, till exempel en användare i din affär senhet eller någon annan användare i din organisation. I det här exemplet används "affär senhet". |
   ||||

## <a name="add-common-data-service-action"></a>Lägg till Common Data Service åtgärd

Lägg nu till en Common Data Service-åtgärd som skapar en uppgifts post för en ny "leads"-post.

1. Under **när en post skapas** utlösare väljer du **nytt steg**.

1. Skriv `common data service` i sökrutan. I listan åtgärder väljer du den här åtgärden: **skapa en ny post**

   ![Välj åtgärd för att skapa en ny post](./media/connect-common-data-service/select-create-new-record-action.png)

1. I åtgärden anger du information om miljön där du vill skapa den nya aktivitets posten. Om det är tillgängligt visas även andra egenskaper baserat på den entitet som du har valt för den här åtgärden, till exempel:

   ![Åtgärds information för miljön där posten ska skapas](./media/connect-common-data-service/create-new-record-action-details.png)

   | Egenskap | Krävs | Beskrivning |
   |----------|----------|-------------|
   | **Organisationsnamn** | Yes | Miljön där du vill skapa posten, som inte behöver vara samma miljö i utlösaren, men är "Fabrikam Sales Production" i det här exemplet |
   | **Entitetsnamn** | Yes | Entiteten där du vill skapa posten, till exempel "aktiviteter" |
   | **Ämne** | Ja, baserat på den entitet som valts i det här exemplet | En kort beskrivning av målet för den här uppgiften |
   ||||

   1. För egenskapen **subject** anger du den här texten med ett avslutande blank steg:

      `Follow up with new lead:`

   1. Håll pekaren inuti rutan **ämne** så att listan över dynamiskt innehåll förblir synlig.
   
   1. I listan, från avsnittet **när en post skapas** , väljer du de utlösare som du vill inkludera i uppgifts posten, till exempel:

      ![Välj Utlös utdata som ska användas i uppgifts posten](./media/connect-common-data-service/create-new-record-action-select-trigger-outputs.png)

      | Utlös utdata | Description |
      |----------------|-------------|
      | **Förnamn** | Förnamnet från lead-posten som ska användas som primär kontakt i uppgifts posten |
      | **Efter namn** | Efter namnet från lead-posten som ska användas som primär kontakt i uppgifts posten |
      | **Beskrivning** | Andra utdata som ska inkluderas i uppgifts posten, t. ex. e-postadress och telefonnummer till arbetet |
      |||

   När du är klar kan åtgärden se ut som i det här exemplet:

   ![Åtgärden att skapa en ny post har skapats](./media/connect-common-data-service/finished-create-record-action-details.png)

1. Spara logikappen. I verktygsfältet designer väljer du **Spara**.

1. Starta Logic-appen manuellt genom att klicka på **Kör**i verktygsfältet i designern. Om du vill testa din Logic-app skapar du en ny "leads"-post.

## <a name="trigger-only-on-updated-attributes"></a>Utlös endast för uppdaterade attribut

För utlösare som körs när poster uppdateras, t. ex. **när en post uppdateras** , kan du använda filtrera attribut så att din Logi Kap par bara körs när de angivna attributen uppdateras. Den här funktionen hjälper dig att förhindra att onödiga appar körs.

1. I utlösaren i listan **Lägg till ny parameter** väljer du **attribut filter**.

   ![Skärm bild som visar åtgärden "när en post uppdateras" och den öppnade listan "Lägg till ny parameter" med egenskapen "Attribute filter" vald.](./media/connect-common-data-service/when-record-updated-trigger-add-attribute-filters.png)

1. För varje **attribut filter objekt**väljer du det attribut som du vill övervaka för uppdateringar, till exempel:

   ![Lägg till egenskapen "Attribute filters"](./media/connect-common-data-service/when-record-updated-trigger-select-attribute-filter.png)

## <a name="list-records-based-on-a-filter"></a>Lista poster baserat på ett filter

För åtgärder som returnerar poster, till exempel åtgärden **lista poster** , kan du använda en OData-fråga som returnerar poster baserat på det angivna filtret. Du kan till exempel ha åtgärds listan endast poster för aktiva konton.

1. I åtgärden öppnar du listan **Lägg till ny parameter** och väljer egenskapen **filtrera fråga** .

   ![Lägg till egenskapen filter fråga](./media/connect-common-data-service/list-records-action-filter-query.png)

1. I den **filter** egenskaps fråga som nu visas i åtgärden, anger du den här OData-filter frågan: `statuscode eq 1`

   ![Ange ODATA filter fråga för att filtrera poster](./media/connect-common-data-service/list-records-action-filter-query-value.png)

Mer information om `$filter` alternativ för system frågor finns i [common data service-filter resultat](/powerapps/developer/common-data-service/webapi/query-data-web-api#filter-results).

## <a name="list-records-based-on-an-order"></a>Lista poster baserat på en order

För åtgärder som returnerar poster, till exempel åtgärden **lista poster** , kan du använda en OData-fråga som returnerar poster i en angiven ordning, beroende på vilka poster som åtgärden returnerar. Du kan till exempel låta åtgärden lista posterna baserat på kontots namn.

1. Öppna listan **Lägg till ny parameter** i åtgärden och välj egenskapen **Sortera efter** .

   ![Lägg till "order by"-egenskap](./media/connect-common-data-service/list-records-action-order-by.png)

1. I **order by** -egenskapen som nu visas i åtgärden, anger du den här OData-filter frågan: `name`

   ![Ange ODATA filter-fråga för att ordna poster](./media/connect-common-data-service/list-records-action-order-by-value.png)

Mer information om `$orderby` alternativ för system frågor finns i [common data service-order-resultat](/powerapps/developer/common-data-service/webapi/query-data-web-api#order-results).

## <a name="field-data-types"></a>Fält data typer

Oavsett om du anger ett värde manuellt eller väljer ett värde i listan med dynamiskt innehåll för ett fält i en utlösare eller åtgärd, måste värdets datatyp matcha fältens obligatoriska datatyp.

I den här tabellen beskrivs några fält typer och de data typer som fälten kräver för sina värden.

| Fält | Datatyp | Beskrivning |
|-------|-----------|-------------|
| Textfält | Enskild rad med text | Kräver antingen en enskild rad med text eller dynamiskt innehåll som har data typen text, till exempel följande egenskaper: <p><p>- **Beteckning** <br>- **Kategori** |
| Heltals fält | Heltal | Kräver antingen ett heltal eller dynamiskt innehåll som har data typen Integer, till exempel följande egenskaper: <p><p>- **Procent slutfört** <br>- **Giltighet** |
| Datum fält | Datum och tid | Kräver antingen ett datum i formatet MM/DD/YYY/YYY eller dynamiskt innehåll som har data typen date, till exempel följande egenskaper: <p><p>- **Skapad den** <br>- **Start datum** <br>- **Faktisk start** <br>- **Verkligt slut** <br>- **Förfallo datum** |
| Fält som refererar till en annan enhets post | Primärnyckel | Kräver både ett post-ID, till exempel ett GUID och en uppslags typ, vilket innebär att värden från den dynamiska innehålls listan inte fungerar, till exempel dessa egenskaper: <p><p>- **Owner**: måste vara ett giltigt användar-ID eller ett team post-ID. <br>- **Ägar typ**: måste vara en uppslags typ, t `systemusers` . ex. respektive `teams` . <p><p>- **Angående**: måste vara ett giltigt post-ID, till exempel ett konto-ID eller ett ID för kontakt post. <br>- **Angående typ**: måste vara en uppslags typ, till exempel respektive `accounts` `contacts` respektive. <p><p>- **Kund**: måste vara ett giltigt post-ID, till exempel ett konto-ID eller ett kontakt post-ID. <br>- **Kundtyp**: måste vara en uppslags typ, t `accounts` `contacts` . ex. respektive. |
||||

I det här exemplet visas hur åtgärden **skapa en ny post** skapar en ny "tasks"-post som är kopplad till andra entitetsposter, specifikt en användar post och en konto post. Åtgärden anger ID: n och Sök typerna för dessa entitetsposter genom att använda värden som matchar de förväntade data typerna för relevanta egenskaper.

* Baserat på egenskapen **Owner** , som anger ett användar-ID och egenskapen **ägar typ** , som anger `systemusers` uppslags typen, associerar åtgärden den nya posten "tasks" med en speciell användare.

* Baserat på egenskapen **ämne** , som anger ett post-ID, och information **om typ** -egenskapen som anger `accounts` uppslags typen, associerar åtgärden den nya posten "tasks" med ett särskilt konto.

![Skapa posten "tasks" som är associerad med ID: n och Sök typerna](./media/connect-common-data-service/create-new-record-task-properties.png)

## <a name="connector-reference"></a>Referens för anslutningsapp

Teknisk information som baseras på kopplingens Swagger beskrivning, till exempel utlösare, åtgärder, gränser och annan information finns på [kopplingens referens sida](/connectors/commondataservice/).

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om andra [anslutningar för Azure Logic Apps](../connectors/apis-list.md)
