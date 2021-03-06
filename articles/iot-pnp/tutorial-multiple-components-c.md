---
title: Ansluta IoT Plug and Play Preview exempel C enhets kod till IoT Hub | Microsoft Docs
description: Skapa och kör IoT Plug and Play Preview exempel C enhets kod som använder flera komponenter och ansluter till en IoT-hubb. Använd Azure IoT Explorer-verktyget för att visa informationen som skickas av enheten till hubben.
author: ericmitt
ms.author: ericmitt
ms.date: 07/22/2020
ms.topic: tutorial
ms.service: iot-pnp
services: iot-pnp
ms.openlocfilehash: 29017ec11429b26018093980ca71c317b12085b5
ms.sourcegitcommit: 59ea8436d7f23bee75e04a84ee6ec24702fb2e61
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/07/2020
ms.locfileid: "89505892"
---
# <a name="tutorial-connect-an-iot-plug-and-play-multiple-component-device-applications-running-on-linux-or-windows-to-iot-hub-c"></a>Självstudie: Anslut en IoT Plug and Play flera komponent enhets program som körs på Linux eller Windows till IoT Hub (C)

[!INCLUDE [iot-pnp-tutorials-device-selector.md](../../includes/iot-pnp-tutorials-device-selector.md)]

I den här självstudien får du lära dig hur du skapar ett exempel på IoT Plug and Play enhets program med komponenter och rot gränssnitt, ansluter det till din IoT-hubb och använder Azure IoT Explorer-verktyget för att visa den information som skickas till hubben. Exempel programmet skrivs i C och ingår i Azure IoT-enhetens SDK för C. Ett Solution Builder kan använda Azure IoT Explorer-verktyget för att förstå funktionerna i en IoT Plug and Play-enhet utan att behöva visa någon enhets kod.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Krav

Du kan slutföra den här kursen på Linux eller Windows. Shell-kommandona i den här självstudien följer Linux-konventionen för avgränsare " `/` ", om du följer med i Windows måste du byta avgränsare för " `\` ".

Kraven varierar beroende på operativ system:

### <a name="linux"></a>Linux

I den här självstudien förutsätter vi att du använder Ubuntu Linux. Stegen i den här självstudien har testats med Ubuntu 18,04.

Om du vill slutföra den här självstudien på Linux installerar du följande program vara i din lokala Linux-miljö:

Installera **gcc**, **git**, **cmake**och alla nödvändiga beroenden med `apt-get` kommandot:

```sh
sudo apt-get update
sudo apt-get install -y git cmake build-essential curl libcurl4-openssl-dev libssl-dev uuid-dev
```

Kontrol lera att versionen av `cmake` är över **2.8.12** och att versionen av **gcc** är över **4.4.7**.

```sh
cmake --version
gcc --version
```

### <a name="windows"></a>Windows

Om du vill slutföra den här självstudien i Windows installerar du följande program vara i din lokala Windows-miljö:

* [Visual Studio (community, Professional eller Enterprise)](https://visualstudio.microsoft.com/downloads/) – se till att du inkluderar **Skriv bords utveckling med C++** -arbetsbelastning när du [installerar](https://docs.microsoft.com/cpp/build/vscpp-step-0-installation?view=vs-2019) Visual Studio.
* [Git](https://git-scm.com/download/).
* [Cmake](https://cmake.org/download/).

### <a name="azure-iot-explorer"></a>Azure IoT Explorer

Om du vill interagera med exempel enheten i den andra delen av den här självstudien använder du **Azure IoT Explorer** -verktyget. [Hämta och installera den senaste versionen av Azure IoT Explorer](./howto-use-iot-explorer.md) för ditt operativ system.

[!INCLUDE [iot-pnp-prepare-iot-hub.md](../../includes/iot-pnp-prepare-iot-hub.md)]

Kör följande kommando för att hämta _anslutnings strängen för IoT Hub_ för hubben. Anteckna den här anslutnings strängen, du använder den senare i den här självstudien:

```azurecli-interactive
az iot hub show-connection-string --hub-name <YourIoTHubName> --output table
```

> [!TIP]
> Du kan också använda Azure IoT Explorer-verktyget för att hitta anslutnings strängen för IoT Hub.

Kör följande kommando för att hämta _enhets anslutnings strängen_ för den enhet som du har lagt till i hubben. Anteckna den här anslutnings strängen, du använder den senare i den här självstudien:

```azurecli-interactive
az iot hub device-identity show-connection-string --hub-name <YourIoTHubName> --device-id <YourDeviceID> --output table
```

[!INCLUDE [iot-pnp-download-models.md](../../includes/iot-pnp-download-models.md)]

## <a name="download-the-code"></a>Ladda ned koden

I den här självstudien förbereder du en utvecklings miljö som du kan använda för att klona och bygga Azure IoT Hub Device C SDK.

Öppna en kommando tolk i valfri mapp. Kör följande kommando för att klona [Azure IoT C SDK: er och bibliotek](https://github.com/Azure/azure-iot-sdk-c) GitHub-lagringsplatsen på den här platsen:

```cmd/bash
git clone https://github.com/Azure/azure-iot-sdk-c.git
cd azure-iot-sdk-c
git submodule update --init
```

Den här åtgärden kan förväntas ta flera minuter att slutföra.

## <a name="build-and-run-the-code"></a>Skapa och kör koden

Du kan skapa och köra koden med Visual Studio eller `cmake` på kommando raden.

### <a name="use-visual-studio"></a>Använda Visual Studio

1. Öppna rotmappen för den klonade lagrings platsen. Efter några sekunder skapar **cmake** -stödet i Visual Studio allt du behöver för att köra och felsöka projektet.
1. När Visual Studio är klar går du till **Solution Explorer**och navigerar till exempel *iothub_client/samples/PnP/pnp_temperature_controller/*.
1. Högerklicka på filen *pnp_temperature_controller. c* och välj **Lägg till fel söknings konfiguration**. Välj **standard**.
1. Denlaunch.vs.jsfilen öppnas * i* Visual Studio. Redigera den här filen som du ser i följande kodfragment för att ange de miljövariabler som krävs:

    ```json
    {
      "version": "0.2.1",
      "defaults": {},
      "configurations": [
        {
          "type": "default",
          "project": "iothub_client\\samples\\pnp\\pnp_temperature_controller\\pnp_temperature_controller.c",
          "projectTarget": "",
          "name": "pnp_temperature_controller.c",
          "env": {
            "IOTHUB_DEVICE_SECURITY_TYPE": "connectionString",
            "IOTHUB_DEVICE_CONNECTION_STRING": "<Your device connection string>"
          }
        }
      ]
    }
    ```

1. Högerklicka på filen *pnp_temperature_controller. c* och välj **Ange som start objekt**.
1. Om du vill spåra kod körningen i Visual Studio lägger du till en Bryt punkt i `main` funktionen i filen *pnp_temperature_controller. c* .
1. Nu kan du köra och felsöka exemplet från **fel söknings** menyn.

Enheten är nu redo att ta emot kommandon och egenskaps uppdateringar och har börjat skicka telemetridata till hubben. Se till att exemplet körs när du slutför nästa steg.

### <a name="use-cmake-at-the-command-line"></a>Använd `cmake` på kommando raden

Så här skapar du exemplet:

1. Skapa en _cmake_ -undermapp i rotmappen för den klonade enhetens SDK och navigera till mappen:

    ```cmd/bash
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

1. Kör följande kommandon för att generera och bygga projektfiler för SDK och exempel:

    ```cmd/bash
    cmake ..
    cmake --build .
    ```

Så här kör du exemplet:

1. Skapa två miljövariabler för att konfigurera exemplet för att använda en anslutnings sträng för att ansluta till din IoT-hubb:

    * **IOTHUB_DEVICE_SECURITY_TYPE** med värdet `"connectionString"`
    * **IOTHUB_DEVICE_CONNECTION_STRING** att lagra enhets anslutnings strängen som du antecknade tidigare.

1. Från mappen _cmake_ navigerar du till mappen som innehåller den körbara filen och kör den:

    ```bash
    # Bash
    cd iothub_client/samples/pnp/pnp_temperature_controller/
    ./pnp_temperature_controller
    ```

    ```cmd
    REM Windows
    iothub_client\samples\pnp\pnp_temperature_controller\Debug\pnp_temperature_controller.exe
    ```

Enheten är nu redo att ta emot kommandon och egenskaps uppdateringar och har börjat skicka telemetridata till hubben. Se till att exemplet körs när du slutför nästa steg.

### <a name="use-the-azure-iot-explorer-to-validate-the-code"></a>Använd Azure IoT Explorer för att verifiera koden

När enhets klient exemplet startar använder du verktyget Azure IoT Explorer för att kontrol lera att det fungerar.

[!INCLUDE [iot-pnp-iot-explorer.md](../../includes/iot-pnp-iot-explorer.md)]

## <a name="review-the-code"></a>Granska koden

Det här exemplet implementerar en IoT Plug and Play temperatur styrenhets enhet. Det här exemplet implementerar en modell med [flera komponenter](concepts-components.md). [DTDL-modell filen (Digital Definition Language) för temperatur enheten](https://github.com/Azure/opendigitaltwins-dtdl/blob/master/DTDL/v2/samples/TemperatureController.json) definierar telemetri, egenskaper och kommandon som enheten implementerar.

### <a name="iot-plug-and-play-helper-functions"></a>Hjälp funktioner för IoT Plug and Play

I det här exemplet använder koden vissa hjälp funktioner från mappen */vanliga* :

*pnp_device_client_ll* innehåller metoden Connect för IoT-plug and Play med `model-id` inkluderad som parameter: `PnP_CreateDeviceClientLLHandle` .

*pnp_protocol*: innehåller IoT plug and Play Helper-funktioner:

* `PnP_CreateReportedProperty`
* `PnP_CreateReportedPropertyWithStatus`
* `PnP_ParseCommandName`
* `PnP_CreateTelemetryMessageHandle`
* `PnP_ProcessTwinData`
* `PnP_CopyPayloadToString`
* `PnP_CreateDeviceClientLLHandle_ViaDps`

Dessa hjälp funktioner är allmänt tillräckliga för att användas i ditt eget projekt. I det här exemplet används de tre filer som motsvarar varje komponent i modellen:

* *pnp_deviceinfo_component*
* *pnp_temperature_controller*
* *pnp_thermostat_component*

I *pnp_deviceinfo_component* -filen `SendReportedPropertyForDeviceInformation` använder funktionen exempelvis två hjälp funktioner:

```c
if ((jsonToSend = PnP_CreateReportedProperty(componentName, propertyName, propertyValue)) == NULL)
{
    LogError("Unable to build reported property response for propertyName=%s, propertyValue=%s", propertyName, propertyValue);
}
else
{
    const char* jsonToSendStr = STRING_c_str(jsonToSend);
    size_t jsonToSendStrLen = strlen(jsonToSendStr);

    if ((iothubClientResult = IoTHubDeviceClient_LL_SendReportedState(deviceClientLL, (const unsigned char*)jsonToSendStr, jsonToSendStrLen, NULL, NULL)) != IOTHUB_CLIENT_OK)
    {
        LogError("Unable to send reported state for property=%s, error=%d", propertyName, iothubClientResult);
    }
    else
    {
        LogInfo("Sending device information property to IoTHub.  propertyName=%s, propertyValue=%s", propertyName, propertyValue);
    }
}
```

Varje komponent i exemplet följer det här mönstret.

### <a name="code-flow"></a>Kod flöde

`main`Funktionen initierar anslutningen och skickar modell-ID:

```c
deviceClient = CreateDeviceClientAndAllocateComponents();
```

Koden använder `PnP_CreateDeviceClientLLHandle` för att ansluta till IoT Hub, anges `modelId` som ett alternativ och konfigurera enhets metoden och enhetens dubbla återanrops hanterare för direkta metoder och enhets dubbla uppdateringar:

```c
g_pnpDeviceConfiguration.deviceMethodCallback = PnP_TempControlComponent_DeviceMethodCallback;
g_pnpDeviceConfiguration.deviceTwinCallback = PnP_TempControlComponent_DeviceTwinCallback;
g_pnpDeviceConfiguration.modelId = g_temperatureControllerModelId;
...

deviceClient = PnP_CreateDeviceClientLLHandle(&g_pnpDeviceConfiguration);
```

`&g_pnpDeviceConfiguration` innehåller även anslutnings informationen. Miljövariabeln **IOTHUB_DEVICE_SECURITY_TYPE** bestämmer om exemplet använder en anslutnings sträng eller enhets etablerings tjänsten för att ansluta till IoT Hub.

När enheten skickar ett modell-ID blir den en IoT Plug and Play-enhet.

Med återanrops hanterare på plats reagerar enheten på dubbla uppdateringar och direkta metod anrop:

* För enhetens dubbla motringning `PnP_TempControlComponent_DeviceTwinCallback` anropar `PnP_ProcessTwinData` funktionen för att bearbeta data. `PnP_ProcessTwinData` använder *besöks mönster* för att parsa JSON och sedan besöka varje egenskap och anropar `PnP_TempControlComponent_ApplicationPropertyCallback` varje element.

* För kommandona motringning `PnP_TempControlComponent_DeviceMethodCallback` använder funktionen hjälp funktionen för att parsa kommando-och komponent namn:

    ```c
    PnP_ParseCommandName(methodName, &componentName, &componentNameSize, &pnpCommandName);
    ```

    `PnP_TempControlComponent_DeviceMethodCallback`Funktionen anropar sedan kommandot på komponenten:

    ```c
    LogInfo("Received PnP command for component=%.*s, command=%s", (int)componentNameSize, componentName, pnpCommandName);
    if (strncmp((const char*)componentName, g_thermostatComponent1Name, g_thermostatComponent1Size) == 0)
    {
        result = PnP_ThermostatComponent_ProcessCommand(g_thermostatHandle1, pnpCommandName, rootValue, response, responseSize);
    }
    else if (strncmp((const char*)componentName, g_thermostatComponent2Name, g_thermostatComponent2Size) == 0)
    {
        result = PnP_ThermostatComponent_ProcessCommand(g_thermostatHandle2, pnpCommandName, rootValue, response, responseSize);
    }
    else
    {
        LogError("PnP component=%.*s is not supported by TemperatureController", (int)componentNameSize, componentName);
        result = PNP_STATUS_NOT_FOUND;
    }
    ```

`main`Funktionen initierar skrivskyddade egenskaper som skickas till IoT Hub:

```c
PnP_TempControlComponent_ReportSerialNumber_Property(deviceClient);
PnP_DeviceInfoComponent_Report_All_Properties(g_deviceInfoComponentName, deviceClient);
PnP_TempControlComponent_Report_MaxTempSinceLastReboot_Property(g_thermostatHandle1, deviceClient);
PnP_TempControlComponent_Report_MaxTempSinceLastReboot_Property(g_thermostatHandle2, deviceClient);
```

`main`Funktionen anger en slinga för att uppdatera händelse-och telemetridata för varje komponent:

```c
while (true)
{
    PnP_TempControlComponent_SendWorkingSet(deviceClient);
    PnP_ThermostatComponent_SendTelemetry(g_thermostatHandle1, deviceClient);
    PnP_ThermostatComponent_SendTelemetry(g_thermostatHandle2, deviceClient);
}
```

`PnP_ThermostatComponent_SendTelemetry`Funktionen visar hur du använder `PNP_THERMOSTAT_COMPONENT` struct. Exemplet använder den här structen för att lagra information om de två termostater i temperatur styrenheten. Koden använder `PnP_CreateTelemetryMessageHandle` funktionen för att förbereda meddelandet och skicka det:

```c
messageHandle = PnP_CreateTelemetryMessageHandle(pnpThermostatComponent->componentName, temperatureStringBuffer);
...
iothubResult = IoTHubDeviceClient_LL_SendEventAsync(deviceClientLL, messageHandle, NULL, NULL);
```

`main`Funktionen förstör slutligen de olika komponenterna och stänger anslutningen till hubben.

[!INCLUDE [iot-pnp-clean-resources.md](../../includes/iot-pnp-clean-resources.md)]

## <a name="next-steps"></a>Nästa steg

I den här självstudien har du lärt dig hur du ansluter en IoT Plug and Play-enhet med komponenter till en IoT-hubb. Mer information om IoT Plug and Play enhets modeller finns i:

> [!div class="nextstepaction"]
> [IoT Plug and Play Preview Modeling Developer Guide](concepts-developer-guide.md)
