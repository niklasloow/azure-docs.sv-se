---
title: Installera och använda Azure IoT Explorer | Microsoft Docs
description: Installera Azure IoT Explorer-verktyget och Använd det för att interagera med IoT-Plug and Play förhands gransknings enheter anslutna till IoT Hub.
author: rido-min
ms.author: rmpablos
ms.date: 05/06/2020
ms.topic: how-to
ms.service: iot-pnp
services: iot-pnp
ms.custom: mvc
ms.openlocfilehash: 083dcde44e56af34f17d952c46e554b234818f27
ms.sourcegitcommit: 46f8457ccb224eb000799ec81ed5b3ea93a6f06f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/28/2020
ms.locfileid: "87352367"
---
# <a name="install-and-use-azure-iot-explorer"></a>Installera och använda Azure IoT Explorer

Azure IoT Explorer är ett grafiskt verktyg för att interagera med och testa din IoT Plug and Play Preview-enheter. När du har installerat verktyget på den lokala datorn kan du använda det för att ansluta till en hubb. Du kan använda verktyget för att Visa telemetri som enheterna skickar, arbeta med enhets egenskaper och anropa kommandon.

Den här artikeln visar hur du:

- Installera och konfigurera Azure IoT Explorer-verktyget.
- Använd verktyget för att interagera med och testa dina enheter.

## <a name="prerequisites"></a>Förutsättningar

Om du vill använda Azure IoT Explorer-verktyget behöver du:

- En Azure IoT-hubb. Det finns många sätt att lägga till en IoT-hubb i din Azure-prenumeration, till exempel [skapa en IoT-hubb med hjälp av Azure CLI](../iot-hub/iot-hub-create-using-cli.md). Du behöver den IoT Hub-anslutningssträngen för att köra Azure IoT Explorer-verktyget. Om du inte har någon Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.
- En enhet som är registrerad i IoT Hub. Du kan registrera en enhet med hjälp av följande Azure CLI-kommando. Se till att ersätta `{YourIoTHubName}` `{YourDeviceID}` plats hållarna och med dina värden:

    ```azurecli-interactive
    az iot hub device-identity create --hub-name {YourIoTHubName} --device-id {YourDeviceID}
    ```

## <a name="install-azure-iot-explorer"></a>Installera Azure IoT Explorer

Gå till [Azure IoT Explorer-versioner](https://github.com/Azure/azure-iot-explorer/releases) och expandera listan med till gångar för den senaste versionen. Hämta och installera den senaste versionen av programmet.

>[!Important]
>Från version 0.11.0 IoT Explorer har endast stöd för IoT Plug and Play som kan 2020 för hands version. Om du vill använda de funktioner som är tillgängliga i den tidigare för hands versionen från aug 2019 måste du installera en 0,10. x-version.

## <a name="use-azure-iot-explorer"></a>Använda Azure IoT Explorer

För en enhet kan du antingen ansluta din egen enhet eller använda en av våra exempel simulerade enheter. Följ [de här anvisningarna](https://github.com/Azure/azure-iot-sdk-c/tree/public-preview/iothub_client/samples) för att köra exemplet på simulerad enhet.

### <a name="connect-to-your-hub"></a>Anslut till hubben

Första gången du kör Azure IoT Explorer uppmanas du att ange anslutnings strängen för IoT Hub. När du har lagt till anslutnings strängen väljer du **Anslut**. Du kan använda verktygets inställningar för att växla till en annan IoT-hubb genom att uppdatera anslutnings strängen.

Modell definitionen för en IoT Plug and Play-enhet lagras antingen i det offentliga lagret, på den anslutna enheten eller i en lokal mapp. Som standard letar verktyget efter din modell definition i den offentliga lagrings platsen och den anslutna enheten. Du kan lägga till och ta bort källor eller konfigurera prioriteten för källorna i **inställningarna**:

Så här lägger du till en källa:

1. Gå till **Start-/IoT-plug and Play inställningar**
2. Välj **Lägg till** och välj din källa, från en lagrings plats eller en lokal mapp.

Så här tar du bort en källa:

1. Gå till **Start-/IoT-plug and Play inställningar**
2. Hitta källan som du vill ta bort.
3. Välj **X** för att ta bort den.

Ändra käll prioriteringarna:

Du kan dra och släppa en av modell definitions källorna till en annan rangordning i listan. 

### <a name="view-devices"></a>Visa enheter

När verktyget har anslutit till din IoT-hubb visas **sidan enhets lista med** en lista över enhets identiteter som registrerats med IoT Hub. Du kan välja valfri post i listan om du vill se mer information.

På sidan **enhets** lista kan du:

- Välj **nytt** om du vill registrera en ny enhet med hubben. Ange sedan ett enhets-ID. Använd standardinställningarna för att automatiskt generera autentiseringsinställningar och aktivera anslutningen till hubben.
- Välj en enhet och välj sedan **ta bort** för att ta bort en enhets identitet. Granska enhets detaljerna innan du slutför den här åtgärden för att vara säker på att du tar bort rätt enhets identitet.

## <a name="interact-with-a-device"></a>Interagera med en enhet

På sidan enhets **lista väljer** du ett värde i kolumnen **enhets-ID** för att Visa informations sidan för den registrerade enheten. Det finns två avsnitt: **enhet** och **digital**, för varje enhet.

### <a name="device"></a>Enhet

Det här avsnittet innehåller flikarna **enhets identitet**, **enhets**-ID, **telemetri**, **direkt metod**, **moln-till-enhet**, flikar för **modul identitet** .

- Du kan visa och uppdatera [enhetens identitets](../iot-hub/iot-hub-devguide-identity-registry.md) information på fliken **enhets identitet** .
- Du kan komma åt [enhetens dubbla](../iot-hub/iot-hub-devguide-device-twins.md) information på fliken **enhets dubbla** .
- Om en enhet är ansluten och aktivt skickar data, kan du Visa [Telemetrin](../iot-hub/iot-hub-devguide-messages-read-builtin.md) på fliken **telemetri** .
- Du kan anropa en [direkt metod](../iot-hub/iot-hub-devguide-direct-methods.md) på enheten på fliken **direkt metod** .
- Du kan skicka ett [meddelande från moln till enhet](../iot-hub/iot-hub-devguide-messages-c2d.md) på fliken **meddelanden från molnet till enheten** .
- Du kan komma åt [modulens dubbla](../iot-hub/iot-hub-devguide-module-twins.md) information.

### <a name="iot-plug-and-play-components"></a>IoT Plug and Play-komponenter

Om enheten är ansluten till hubben med ett **modell-ID**, visar verktyget **IoT plug and Play-komponenter** -fliken där du kan se **modell-ID: t**.

Om **modell-ID: t** är tillgängligt i någon av de konfigurerade källorna – offentliga lagrings platsen eller lokala mappar, visas listan med komponenter. Om du väljer en komponent visas egenskaper, kommandon och telemetri som är tillgängliga.

På sidan **komponent** kan du visa skrivskyddade egenskaper, uppdatera skrivbara egenskaper, anropa kommandon och se telemetri-meddelanden som skapas av den här komponenten.

:::image type="content" source="media/howto-use-iot-explorer/components-iot-explorer.png" alt-text="Visa komponenter i Azure IoT Explorer":::

#### <a name="properties"></a>Egenskaper

:::image type="content" source="media/howto-use-iot-explorer/properties-iot-explorer.png" alt-text="Visa egenskaper i Azure IoT Explorer":::

Du kan visa de skrivskyddade egenskaperna som definierats i ett gränssnitt på fliken **egenskaper (skrivskyddad)** . Du kan uppdatera de skrivbara egenskaperna som definierats i ett gränssnitt på fliken **egenskaper (skrivbar)** :

1. Gå till fliken **egenskaper (skrivbar)** .
1. Klicka på den egenskap som du vill uppdatera.
1. Ange det nya värdet för egenskapen.
1. Förhandsgranska nytto lasten som ska skickas till enheten.
1. Skicka ändringen.

När du har skickat in en ändring kan du spåra uppdaterings statusen: **synkronisering**, **lyckades**eller **fel**. När synkroniseringen är klar visas det nya värdet för egenskapen i kolumnen **rapporterad egenskap** . Om du navigerar till andra sidor innan synkroniseringen är klar meddelar verktyget fortfarande dig när uppdateringen är klar. Du kan också använda verktygets meddelande Center för att se meddelande historiken.

#### <a name="commands"></a>Kommandon

Om du vill skicka ett kommando till en enhet går du till fliken **kommandon** :

1. I listan med kommandon expanderar du kommandot som du vill utlösa.
1. Ange de värden som krävs för kommandot.
1. Förhandsgranska nytto lasten som ska skickas till enheten.
1. Skicka kommandot.

#### <a name="telemetry"></a>Telemetri

Om du vill visa telemetri för det valda gränssnittet går du till fliken **telemetri** .

## <a name="next-steps"></a>Nästa steg

I den här instruktions artikeln har du lärt dig hur du installerar och använder Azure IoT Explorer för att interagera med IoT Plug and Play-enheter. Ett förslag till nästa steg är att lära dig att använda [Azure CLI IoT plug and Play-kommandon](./howto-use-iot-pnp-cli.md).
