---
title: Förstå Azure IoT Hub-meddelanden | Microsoft Docs
description: Utvecklings guide – meddelanden från enhet till moln och meddelanden från moln till enhet med IoT Hub. Innehåller information om meddelande format och kommunikations protokoll som stöds.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.custom:
- 'Role: Cloud Development'
ms.openlocfilehash: e0466aa3497edf5f271c01944438c033bba152f4
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87327675"
---
# <a name="send-device-to-cloud-and-cloud-to-device-messages-with-iot-hub"></a>Skicka meddelanden från enheten till molnet och från moln till enhet med IoT Hub

IoT Hub möjliggör dubbelriktad kommunikation med dina enheter. Använd IoT Hub meddelanden för att kommunicera med dina enheter genom att skicka meddelanden från dina enheter till lösningens Server del och skicka kommandon från dina IoT-lösningens Server del till dina enheter. Läs mer om det [IoT Hub meddelande formatet](iot-hub-devguide-messages-construct.md).

## <a name="sending-device-to-cloud-messages-to-iot-hub"></a>Skicka "enhet till molnet"-meddelanden till IoT Hub

IoT Hub har en inbyggd tjänst slut punkt som kan användas av backend-tjänster för att läsa telemetri-meddelanden från dina enheter. Den här slut punkten är kompatibel med [Event Hubs](https://docs.microsoft.com/azure/event-hubs/) och du kan använda standard-IoT Hub SDK: er för att [läsa från den här inbyggda slut punkten](iot-hub-devguide-messages-read-builtin.md).

IoT Hub stöder också [anpassade slut punkter](iot-hub-devguide-endpoints.md#custom-endpoints) som kan definieras av användare för att skicka data och händelser för enhets telemetri till Azure-tjänster med hjälp av [meddelanderoutning](iot-hub-devguide-messages-d2c.md).

## <a name="sending-cloud-to-device-messages-from-iot-hub"></a>Skicka meddelanden från moln till enhet från IoT Hub

Du kan skicka meddelanden från [molnet till enheten](iot-hub-devguide-messages-c2d.md) från Server delen av lösningen till dina enheter.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Kärn egenskaper för IoT Hub meddelande funktioner är tillförlitlighet och hållbarhet för meddelanden. De här egenskaperna möjliggör återhämtning till tillfälliga anslutningar på enhets sidan och för att läsa in toppar i händelse bearbetning på moln sidan. IoT Hub implementerar *minst en gång* till leverans garantier för både enhet till molnet och meddelanden från molnet till enheten.

## <a name="choosing-the-right-type-of-iot-hub-messaging"></a>Välja rätt typ av IoT Hub meddelanden

Använd enhets-till-moln-meddelanden för att skicka telemetri och aviseringar för Time Series från din enhets app och meddelanden från molnet till enheten för enkelriktade aviseringar till din enhets app.

* Information om hur du kan välja mellan meddelanden från enhet till moln, rapporterade egenskaper eller fil uppladdning finns i [rikt linjer för kommunikation mellan enheter och moln](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-d2c-guidance) .

* Mer information om hur du kan välja mellan meddelanden från molnet till enheten, önskade egenskaper eller direkta metoder finns i [kommunikation mellan moln-till-enhet](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-c2d-guidance) .

## <a name="next-steps"></a>Nästa steg

* Lär dig mer [om IoT Hub meddelanderoutning](iot-hub-devguide-messages-d2c.md).

* Lär dig mer om IoT Hub [meddelanden från molnet till enheten](iot-hub-devguide-messages-c2d.md).