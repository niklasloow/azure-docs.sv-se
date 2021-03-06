---
title: inkludera fil
description: inkludera fil
documentationcenter: partner-center-commercial-marketplace
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: include
ms.date: 08/24/2020
ms.custom: include file
author: mingshen-ms
ms.author: mingshen
ms.openlocfilehash: 7709f26b30d41b932569227b8b6f05b5725dc98f
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88815680"
---
När du publicerar ditt erbjudande på den kommersiella Marketplace via partner Center måste du ansluta ditt erbjudande till ditt CRM-system. På så sätt kan du ta emot kund kontakt uppgifter direkt efter att en kund har fått ett intresse eller distribuerar din produkt.

1. Välj ett leadmål där du vill att vi ska skicka kundleads. Följande CRM-system stöds:

    * [Dynamics 365](../commercial-marketplace-lead-management-instructions-dynamics.md) för kund engagemang
    * [Marketo](../commercial-marketplace-lead-management-instructions-marketo.md)
    * [Salesforce](../commercial-marketplace-lead-management-instructions-salesforce.md)

    Om ditt CRM-system inte uttryckligen stöds i listan kan du lagra kundens lead-data med hjälp av följande alternativ. Sedan kan du exportera eller importera dessa data till CRM-systemet.

    * [Azure-tabell](../commercial-marketplace-lead-management-instructions-azure-table.md)
    * [HTTPS-slutpunkt](../commercial-marketplace-lead-management-instructions-https.md)

1. Läs den tillämpliga länkade dokumentationen ovan för att lära dig hur du konfigurerar din lead-destination och tar emot leads från dina kommersiella Marketplace-erbjudanden.
1. När du har anslutit ditt erbjudande till ditt lead-mål väljer du **publicera** på ditt erbjudande i Partner Center. Vi validerar anslutningen och skickar ett test lead. När du visar erbjudandet innan du går live kan du också testa din lead-anslutning genom att försöka skaffa erbjudandet själv i för hands versionen.
1. Se till att anslutningen till mål platsen hålls uppdaterad så att du inte förlorar några leads. Se till att du uppdaterar anslutningarna när något har ändrats.
