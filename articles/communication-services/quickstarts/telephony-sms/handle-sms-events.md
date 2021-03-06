---
title: Snabb start – hantera SMS-händelser
titleSuffix: An Azure Communication Services quickstart
description: Lär dig hur du hanterar SMS-händelser med Azure Communication Services.
author: mikben
manager: jken
services: azure-communication-services
ms.author: mikben
ms.date: 03/10/2020
ms.topic: overview
ms.service: azure-communication-services
ms.openlocfilehash: 02a0a5dd5cfe3a1ecaa264f1e81c848a8da6c58d
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948224"
---
# <a name="quickstart-handle-sms-events"></a>Snabb start: Hantera SMS-händelser

[!INCLUDE [Private Preview Notice](../../includes/private-preview-include.md)]

Kom igång med Azure Communication Services genom att använda Azure Event Grid för att hantera SMS-händelser för kommunikations tjänster. 

## <a name="about-azure-event-grid"></a>Om Azure Event Grid

[Azure Event Grid](https://docs.microsoft.com/azure/event-grid/overview) är en molnbaserad händelse tjänst. I den här artikeln får du lära dig hur du prenumererar på händelser för [kommunikations tjänst händelser](../../concepts/event-handling.md)och utlöser en händelse för att visa resultatet. Normalt kan du skicka händelser till en slutpunkt som bearbetar informationen om händelsen och utför åtgärder. I den här artikeln ska vi skicka händelserna till en webbapp som samlar in och visar meddelandena.

## <a name="prerequisites"></a>Förutsättningar
- Ett Azure-konto med en aktiv prenumeration. [Skapa ett konto kostnads fritt](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- En Azure Communication service-resurs. Mer information finns i snabb starten för att [skapa en Azure Communication-resurs](../create-communication-resource.md) .
- Ett SMS-aktiverat telefonnummer. [Hämta ett telefonnummer](./get-phone-number.md).

## <a name="setting-up"></a>Konfigurera

### <a name="enable-event-grid-resource-provider"></a>Aktivera Event Grid-resursprovider

Om du inte tidigare har använt Event Grid i din Azure-prenumeration kan du behöva registrera Event Grid resurs leverantören enligt stegen nedan:

I Azure-portalen:

1. Välj **prenumerationer** på den vänstra menyn.
2. Välj den prenumeration som du använder för Event Grid.
3. Välj **resurs leverantörer**på den vänstra menyn under **Inställningar**.
4. Hitta **Microsoft.EventGrid**.
5. Om du inte är registrerad väljer du **Registrera**. 

Det kan ta en stund att slutföra registreringen. Välj **Uppdatera** för att uppdatera statusen. När **Status** är **Registrerad** är du redo att fortsätta.

### <a name="event-grid-viewer-deployment"></a>Distribution av Event Grids hanterare

I den här snabb starten ska vi använda [exemplet på Azure Event Grid Viewer](https://docs.microsoft.com/samples/azure-samples/azure-event-grid-viewer/azure-event-grid-viewer/) för att visa händelser i nära real tid. Detta ger användaren erfarenhet av en feed i real tid. Dessutom bör nytto lasten för varje händelse också vara tillgänglig för granskning.  

## <a name="subscribe-to-the-sms-events-using-web-hooks"></a>Prenumerera på SMS-händelser med hjälp av Webhooks

I portalen navigerar du till den Azure Communication Services-resurs som du skapade. I kommunikations tjänst resursen väljer du **händelser** från menyn till vänster på sidan **kommunikations tjänster** .

:::image type="content" source="./media/handle-sms-events/select-events.png" alt-text="Skärm bild som visar knappen händelse prenumeration på sidan händelser på en resurs.":::

Tryck på **Lägg till händelse prenumeration** för att ange guiden skapa.

På sidan **Skapa händelse prenumeration** anger du ett **namn** för händelse prenumerationen.

Du kan prenumerera på vissa händelser för att berätta Event Grid vilka SMS-händelser du vill spåra och var de ska skickas. Välj de händelser som du vill prenumerera på på list menyn. För SMS har du möjlighet att välja `SMS Received` och `SMS Delivery Report Received` . 

Ta en titt på den fullständiga listan med [händelser som stöds av Azure Communication Services](../../concepts/event-handling.md).

:::image type="content" source="./media/handle-sms-events/select-events-create-eventsub.png" alt-text="Skärm bild som visar mottagar rapporten mottagna och SMS-leverans mottagna händelse typer.":::

Välj **Web Hook** för **slut punkts typ**. 

:::image type="content" source="./media/handle-sms-events/select-events-create-linkwebhook.png" alt-text="Skärm bild som visar fältet typ av slut punkts typ som Web Hook.":::

För **slut punkt**klickar du på **Välj en slut punkt**och anger URL: en för din webbapp.

I det här fallet kommer vi att använda URL: en från [Azure Event Grid Viewer-exemplet](https://docs.microsoft.com/samples/azure-samples/azure-event-grid-viewer/azure-event-grid-viewer/) som vi har konfigurerat tidigare i snabb starten. URL: en för exemplet är i formatet: `https://{{site-name}}.azurewebsites.net/api/updates`

Välj sedan **Bekräfta markering**.

:::image type="content" source="./media/handle-sms-events/select-events-create-selectwebhook-epadd.png" alt-text="Skärm bild som visar bekräftelse på en webhook-slutpunkt.":::

## <a name="viewing-sms-events"></a>Visa SMS-händelser

### <a name="triggering-sms-events"></a>Utlösa SMS-händelser

Om du vill visa händelse utlösare måste vi generera händelser på den första platsen.

- `SMS Received` händelser skapas när telefon numret för kommunikations tjänster får ett SMS. Om du vill utlösa en händelse skickar du bara ett meddelande från din telefon till telefonnumret som är kopplat till kommunikations tjänst resursen.
- `SMS Delivery Report Received` händelser skapas när du skickar ett SMS till en användare med hjälp av ett telefonnummer för kommunikations tjänster. För att utlösa och händelse måste du aktivera `Delivery Report` i alternativen för det [skickade SMS](../telephony-sms/send.md). Försök att skicka ett meddelande till din telefon med `Delivery Report` . Att slutföra den här åtgärden innebär en låg kostnad av några USD cent eller mindre i ditt Azure-konto.

Ta en titt på den fullständiga listan med [händelser som stöds av Azure Communication Services](../../concepts/event-handling.md).

### <a name="receiving-sms-events"></a>Ta emot SMS-händelser

När du har slutfört ovanstående åtgärd kommer du att märka att  `SMS Received` och `SMS Delivery Report Received` händelser skickas till din slut punkt. Dessa händelser visas i [Azure Event Grid Viewer-exemplet](https://docs.microsoft.com/samples/azure-samples/azure-event-grid-viewer/azure-event-grid-viewer/) som vi har konfigurerat i början. Du kan trycka på ögon ikonen bredvid händelsen om du vill se hela nytto lasten. Händelserna kommer att se ut så här:

:::image type="content" source="./media/handle-sms-events/sms-received.png" alt-text="Skärm bild som visar Event Grid schema för en SMS-inhämtad händelse.":::

:::image type="content" source="./media/handle-sms-events/SMS-Delivery-Report-Received.png" alt-text="Skärm bild som visar Event Grid schema för en SMS-leverans rapport händelse.":::

Läs mer om [händelse scheman och andra händelse koncept](../../concepts/event-handling.md)

## <a name="clean-up-resources"></a>Rensa resurser

Om du vill rensa och ta bort en kommunikations tjänst prenumeration kan du ta bort resursen eller resurs gruppen. Om du tar bort resurs gruppen raderas även andra resurser som är kopplade till den. Läs mer om att [Rensa resurser](../create-communication-resource.md#clean-up-resources).

## <a name="next-steps"></a>Nästa steg

I den här snabb starten har du lärt dig hur du använder SMS-händelser. Du kan ta emot SMS-meddelanden genom att skapa en Event Grid-prenumeration.

> [!div class="nextstepaction"] 
> [Skicka SMS](../telephony-sms/send.md)

Du kanske också vill:

 - [Lär dig mer om händelse hanterings koncept](../../concepts/event-handling.md)
 - [Läs mer om Event Grid](https://docs.microsoft.com/azure/event-grid/overview)
