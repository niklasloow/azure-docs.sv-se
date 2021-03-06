---
title: Ansluta enheter med X. 509-certifikat i ett Azure IoT Central-program
description: Så här ansluter du enheter med X. 509-certifikat med hjälp av Node.js Device SDK för IoT Central program
author: v-krghan
ms.author: v-krghan
ms.date: 08/12/2020
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 6de711567e87bcdd1e58185f90264d0c9aecdfde
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91346192"
---
# <a name="how-to-connect-devices-with-x509-certificates-using-nodejs-device-sdk-for-iot-central-application"></a>Så här ansluter du enheter med X. 509-certifikat med hjälp av Node.js Device SDK för IoT Central program

IoT Central stöder de båda certifikaten för signaturer för delad åtkomst (SAS) och X. 509 för att skydda kommunikationen mellan en enhet och ditt program. Självstudien [skapa och Anslut ett klient program till Azure IoT Central-programmet](./tutorial-connect-device-nodejs.md) använder SAS. I den här artikeln får du lära dig hur du ändrar kod exemplet för att använda X. 509.  X. 509-certifikat rekommenderas i produktions miljöer. Mer information finns i [Anslut till Azure IoT Central](./concepts-get-connected.md).

Den här artikeln visar två sätt att använda X. 509 – [grupp registreringar](how-to-connect-devices-x509.md#use-a-group-enrollment) som vanligt vis används i en produktions miljö och [enskilda registreringar](how-to-connect-devices-x509.md#use-an-individual-enrollment) är användbara för testning.

## <a name="prerequisites"></a>Förutsättningar

- Slut för ande av [skapa och Anslut ett klient program till din Azure IoT Central program-självstudie (Node.js)](./tutorial-connect-device-nodejs.md) .
- [Git](https://git-scm.com/download/).
- Hämta och installera [openssl](https://www.openssl.org/). Om du använder Windows kan du använda binärfilerna från [openssl-sidan på SourceForge](https://sourceforge.net/projects/openssl/).

## <a name="use-a-group-enrollment"></a>Använd en grupp registrering

Använd X. 509-certifikat med en grupp registrering i en produktions miljö. I en grupp registrering lägger du till ett rot-eller mellanliggande X. 509-certifikat till ditt IoT Central-program. Enheter med löv certifikat härledda från rot-eller mellanliggande certifikat kan ansluta till ditt program.


## <a name="generate-root-and-device-cert"></a>Generera rot-och enhets certifikat

I det här avsnittet ska du använda ett X. 509-certifikat för att ansluta en enhet med ett certifikat som härletts från registrerings gruppens certifikat, som kan ansluta till ditt IoT Central-program.

> [!WARNING]
> Det här sättet att generera X. 509-certifikat är bara för testning. För en produktions miljö bör du använda den officiella, säkra mekanismen för att skapa certifikat.

1. Öppna en kommandotolk. Klona GitHub-lagringsplatsen för skript för generering av certifikat:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-node.git
    ```

2. Navigera till skriptet för certifikat generatorn och installera de nödvändiga paketen:

    ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    npm install
    ```

3. Skapa ett rot certifikat och Härled sedan ett enhets certifikat genom att köra skriptet. Se till att endast använda gemener alfanumeriska tecken och bindestreck för certifikat namn.

    ```cmd/sh
    node create_test_cert.js root mytestrootcert
    node create_test_cert.js device mytestdevice mytestrootcert
    ```

Detta skapar tre filer för roten och enhets certifikatet

filename | innehåller
-------- | --------
\<name\>_cert. pem | Den offentliga delen av X509-certifikatet
\<name\>_key. pem | Den privata nyckeln för X509-certifikatet
\<name\>_fullchain. pem | Hela nyckel ringen för X509-certifikatet.


## <a name="create-a-group-enrollment"></a>Skapa en grupp registrering


1. Öppna IoT Central programmet och gå till **Administration**  i den vänstra rutan och klicka på **enhets anslutning**. 

2. Välj + **skapa registrerings grupp**och skapa en ny registrerings grupp med namnet _MyX509Group_ med attesterings typ för **certifikat (X. 509)**:


3. Öppna den registrerings grupp som du har skapat och klicka på **Hantera primär**. 

4. Välj fil alternativ och ladda upp rot certifikat filen som heter _mytestrootcert_cert. pem_ som du skapade tidigare:


    ![Certifikat uppladdning](./media/how-to-connect-devices-x509/certificate-upload.png)



5. Slutför verifieringen genom att kopiera verifierings koden och skapa ett verifierings certifikat för X. 509 med den koden i kommando tolken.

    ```cmd/sh
    node create_test_cert.js verification --ca mytestrootcert_cert.pem --key mytestrootcert_key.pem --nonce  {verification-code}
    ```

6. Ladda upp det signerade verifierings certifikatet _verification_cert. pem_ för att slutföra verifieringen.

    ![Verifierat certifikat](./media/how-to-connect-devices-x509/verified.png)


Nu kan du ansluta enheter som har ett X. 509-certifikat som härletts från det här primära rot certifikatet. När du har sparat registrerings gruppen ska du anteckna ID-omfånget.


## <a name="run-sample-device-code"></a>Kör exempel på enhets kod


1. I Azure IoT Central-programmet klickar du på **enheter**och skapar en ny enhet med _mytestdevice_ som **enhets-ID** från enhets mal len för miljö sensor.


2. Kopiera _mytestdevice_key. pem_ och _mytestdevice_cert. pem_ till mappen som innehåller _environmentalSensor.js_ programmet när du har slutfört [guiden Anslut en enhet (Node.js)](./tutorial-connect-device-nodejs.md).

3. Navigera till mappen som innehåller environmentalSensor.js programmet och kör följande kommando för att installera X. 509-paketet:

    ```cmd/sh
    npm install azure-iot-security-x509 --save
    ```

4. Redigera **environmentalSensor.js** -filen.
    - Ersätt `idScope` värdet med **ID-omfånget** som du antecknade tidigare 
    - Ersätt `registrationId` värde med `mytestdevice` .

5. Redigera `require` uttrycken enligt följande:

    ```javascript
    var iotHubTransport = require('azure-iot-device-mqtt').Mqtt;
    var Client = require('azure-iot-device').Client;
    var Message = require('azure-iot-device').Message;
    var ProvisioningTransport = require('azure-iot-provisioning-device-mqtt').Mqtt;
    var ProvisioningDeviceClient = require('azure-iot-provisioning-device').ProvisioningDeviceClient;
    var fs = require('fs');
    var X509Security = require('azure-iot-security-x509').X509Security;
    ```

6. Redigera avsnittet som skapar klienten på följande sätt:

    ```javascript
    var provisioningHost = 'global.azure-devices-provisioning.net';
    var deviceCert = {
      cert: fs.readFileSync('mytestdevice_cert.pem').toString(),
      key: fs.readFileSync('mytestdevice_key.pem').toString()
    };
    var provisioningSecurityClient = new X509Security(registrationId, deviceCert);
    var provisioningClient = ProvisioningDeviceClient.create(provisioningHost, idScope, new ProvisioningTransport(), provisioningSecurityClient);
    var hubClient;
    ```

7. Ändra avsnittet som öppnar anslutningen enligt följande:

   ```javascript
    var connectionString = 'HostName=' + result.assignedHub + ';DeviceId=' + result.deviceId + ';x509=true';
    hubClient = Client.fromConnectionString(connectionString, iotHubTransport);
    hubClient.setOptions(deviceCert);
    ```

8. Kör skriptet och verifiera att enheten etablerats korrekt.

    ```cmd/sh
    node environmentalSensor.js
    ```   

    Du kan också kontrol lera att telemetri visas på instrument panelen.

    ![Verifiera telemetri för enhet](./media/how-to-connect-devices-x509/telemetry.png)

## <a name="use-an-individual-enrollment"></a>Använd en enskild registrering

Använd X. 509-certifikat med en enskild registrering för att testa enheten och lösningen. I en enskild registrering finns inget rot-eller mellanliggande X. 509-certifikat i ditt IoT Central-program. Enheter använder ett självsignerat X. 509-certifikat för att ansluta till ditt program.

## <a name="generate-self-signed-device-cert"></a>Generera ett självsignerat enhets certifikat


I det här avsnittet ska du använda ett självsignerat X. 509-certifikat för att ansluta enheter för enskild registrering, som används för att registrera en enskild enhet. Självsignerade certifikat är endast för testning.

Skapa ett självsignerat X. 509-enhets certifikat genom att köra skriptet. Se till att endast använda gemener alfanumeriska tecken och bindestreck för certifikat namn.

  ```cmd/sh
    cd azure-iot-sdk-node/provisioning/tools
    node create_test_cert.js device mytestselfcertprimary
    node create_test_cert.js device mytestselfcertsecondary 
  ```

## <a name="create-individual-enrollment"></a>Skapa enskild registrering

1. I Azure IoT Central-programmet väljer du **enheter**och skapar en ny enhet med **enhets-ID** som _mytestselfcertprimary_ från enhets mal len för miljö sensor. Anteckna **ID-omfånget**

2. Öppna enheten som du har skapat och välj **Anslut**

3. Välj **enskilda registreringar** som metoden Connect och **certifikat (X. 509)** som mekanism.

    ![Individuell registrering](./media/how-to-connect-devices-x509/individual-device-connect.png)


4. Välj alternativet Arkiv under primär och överför certifikat filen med namnet _mytestselfcertprimary_cert. pem_ som du skapade tidigare. 

5. Välj fil alternativet för det sekundära certifikatet och ladda upp certifikat filen med namnet _mytestselfcertsecondary_cert. pem._ Välj sedan **Spara**

    ![Överföring av enskilda registrerings certifikat](./media/how-to-connect-devices-x509/individual-enrollment.png)

Enheten har nu tillhandahållits med X. 509-certifikat.



## <a name="run-a-sample-individual-enrollment-device"></a>Kör ett exempel på en enskild registrerings enhet

1. Kopiera _mytestselfcertprimary_key. pem_ och _mytestselfcertprimary_cert. pem_till mappen som innehåller environmentalSensor.js programmet när du har slutfört [guiden Anslut en enhet (Node.js)](./tutorial-connect-device-nodejs.md).


2. Redigera **environmentalSensor.js** -filen på följande sätt och spara den.
    - Ersätt `idScope` värdet med **ID-omfånget** som du antecknade tidigare.
    - Ersätt `registrationId` värde med `mytestselfcertprimary` .
    - Ersätt **var deviceCert** som:
    ```cmd\sh
    var deviceCert = {
    cert: fs.readFileSync('mytestselfcertprimary_cert.pem').toString(),
    key: fs.readFileSync('mytestselfcertprimary_key.pem').toString()
    };
    ```

3. Kör skriptet och verifiera att enheten etablerats korrekt.

    ```cmd/sh
    node environmentalSensor.js
    ```   

    Du kan också kontrol lera att telemetri visas på instrument panelen.

    ![Telemetri för enskilda registreringar](./media/how-to-connect-devices-x509/telemetry-primary.png)

Du kan även upprepa stegen ovan för _mytestselfcertsecondary_ -certifikat.

## <a name="next-steps"></a>Nästa steg

Nu när du har lärt dig hur du ansluter enheter med X. 509-certifikat, är det föreslagna nästa steg att lära dig hur du [övervakar enhets anslutning med hjälp av Azure CLI](howto-monitor-devices-azure-cli.md)

