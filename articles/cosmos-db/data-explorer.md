---
title: Använd Azure Cosmos DB Explorer för att hantera dina data
description: Azure Cosmos DB Explorer är ett fristående webbaserat gränssnitt som gör att du kan visa och hantera de data som lagras i Azure Cosmos DB.
author: deborahc
ms.service: cosmos-db
ms.topic: how-to
ms.date: 09/23/2020
ms.author: dech
ms.openlocfilehash: ebfb175de67d7bb8ea011ac340b57f5d62d9e223
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91318814"
---
# <a name="work-with-data-using-azure-cosmos-explorer"></a>Arbeta med data med hjälp av Azure Cosmos Explorer 

Azure Cosmos DB Explorer är ett fristående webbaserat gränssnitt som gör att du kan visa och hantera de data som lagras i Azure Cosmos DB. Azure Cosmos DB Explorer motsvarar fliken befintlig **datautforskaren** som är tillgänglig i Azure Portal när du skapar ett Azure Cosmos DB-konto. De främsta fördelarna med Azure Cosmos DB Explorer över befintliga data Utforskaren är:

* Du har en hel skärms fastighet för att visa dina data, köra frågor, lagrade procedurer, utlösare och visa resultatet.  

* Du kan tillhandahålla temporär eller permanent Läs-eller Läs-och Skriv behörighet till ditt databas konto och dess samlingar till andra användare som inte har åtkomst till Azure Portal eller prenumerationen.  

* Du kan dela frågeresultaten med andra användare som inte har till gång till Azure Portal eller prenumerationen.  

## <a name="access-azure-cosmos-db-explorer"></a>Åtkomst till Azure Cosmos DB Explorer

1. Logga in på [Azure-portalen](https://portal.azure.com/). 

2. Från **alla resurser**, leta upp och navigera till ditt Azure Cosmos DB konto, Välj nycklar och kopiera den **primära anslutnings strängen**.  

3. Gå till https://cosmos.azure.com/ , klistra in anslutnings strängen och välj **Anslut**. Genom att använda anslutnings strängen kan du komma åt Azure Cosmos DB Explorer utan några tids gränser.  

   Om du vill ge andra användare tillfällig åtkomst till ditt Azure Cosmos DB-konto kan du göra det med URL: erna Läs-och Läs behörighet. 

4. Öppna bladet **datautforskaren** och välj **Öppna hel skärms läge**. I popup-dialogrutan kan du Visa två åtkomst webb adresser – **Läs-och skriv-** och **Läs**behörighet. Med dessa URL: er kan du dela ditt Azure Cosmos DB-konto tillfälligt med andra användare. Åtkomst till kontot upphör att gälla om 24 timmar efter vilken du kan återansluta med hjälp av en ny åtkomst-URL eller anslutnings strängen. 

   **Read-Write** – när du delar URL: en med Läs-och Skriv behörighet till andra användare kan de Visa och ändra databaser, samlingar, frågor och andra resurser som är kopplade till det aktuella kontot.

   **Läs** – när du delar den skrivskyddade URL: en med andra användare kan de se databaser, samlingar, frågor och andra resurser som är kopplade till det aktuella kontot. Om du till exempel vill dela resultatet av en fråga med dina grupp medlemmar som inte har åtkomst till Azure Portal eller ditt Azure Cosmos DB-konto kan du ange dem med denna URL.

   Välj den typ av åtkomst du vill öppna kontot med och klicka på **Öppna**. När du har öppnat Utforskaren är det samma sak som du hade med fliken Datautforskaren i Azure Portal.

   :::image type="content" source="./media/data-explorer/open-data-explorer-with-access-url.png" alt-text="Öppna Azure Cosmos DB Explorer":::

## <a name="known-issues"></a>Kända problem

För närvarande är den **Öppna hel skärms** upplevelsen som gör att du kan dela temporär Läs-och Läs behörighet ännu inte stöd för Azure Cosmos DB Gremlin-och tabell-API-konton. Du kan fortfarande visa dina Gremlin och Tabell-API-konton genom att skicka anslutnings strängen till Azure Cosmos DB Explorer. 

För närvarande stöds inte visning av dokument som innehåller ett UUID i Datautforskaren. Detta påverkar inte inläsning av samlingar, endast enskilda dokument eller frågor som innehåller dessa dokument. För att kunna visa och hantera dessa dokument bör användarna fortsätta att använda verktyget som ursprungligen användes för att skapa dessa dokument.

Kunder som tar emot HTTP-401-fel kan bero på otillräckliga RBAC-behörigheter för kundens Azure-konto, särskilt om kontot har en anpassad RBAC-roll. Alla anpassade roller måste ha `Microsoft.DocumentDB/databaseAccounts/listKeys/*` åtgärd för att kunna använda datautforskaren om du loggar in med sina Azure Active Directory autentiseringsuppgifter.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur du kommer igång med Azure Cosmos DB Explorer för att hantera dina data, kan du göra följande:

* Börja definiera [frågor](sql-api-query-reference.md) med SQL-syntax och utför [Server sidans programmering](stored-procedures-triggers-udfs.md) genom att använda lagrade procedurer, UDF: er, utlösare. 
