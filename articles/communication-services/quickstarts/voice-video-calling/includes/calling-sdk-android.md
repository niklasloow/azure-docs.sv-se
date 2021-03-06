---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: aec9d2049a69aebc7102a70274e5fb2a3ef865a8
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376600"
---
## <a name="prerequisites"></a>Förutsättningar

- Ett Azure-konto med en aktiv prenumeration. [Skapa ett konto kostnads fritt](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 
- En distribuerad kommunikations tjänst resurs. [Skapa en kommunikations tjänst resurs](../../create-communication-resource.md).
- A `User Access Token` för att aktivera anrops klienten. Mer information om [hur du hämtar en `User Access Token` ](../../access-tokens.md)
- Valfritt: Slutför snabb starten för att [komma igång med att lägga till samtal till ditt program](../getting-started-with-calling.md)

## <a name="setting-up"></a>Konfigurera

### <a name="install-the-package"></a>Installera paketet

<!-- TODO: update with instructions on how to download, install and add package to project -->
Leta upp build. gradle på projekt nivå och se till att lägga till `mavenCentral()` i listan över databaser under `buildscript` och `allprojects`
```groovy
buildscript {
    repositories {
    ...
        mavenCentral()
    ...
    }
}
```

```groovy
allprojects {
    repositories {
    ...
        mavenCentral()
    ...
    }
}
```
Sedan lägger du till följande rader i avsnittet beroenden i din modulnivå build. gradle

```groovy
dependencies {
    ...
    implementation 'com.azure.android:azure-communication-calling:1.0.0-beta.1'
    ...
}

```

## <a name="object-model"></a>Objekt modell

Följande klasser och gränssnitt hanterar några av de viktigaste funktionerna i Azure Communication Services som anropar klient biblioteket:

| Name                                  | Beskrivning                                                  |
| ------------------------------------- | ------------------------------------------------------------ |
| CallClient| CallClient är den huvudsakliga start punkten för det anropande klient biblioteket.|
| CallAgent | CallAgent används för att starta och hantera samtal. |
| CommunicationUserCredential | CommunicationUserCredential används som token-autentiseringsuppgifter för att instansiera CallAgent.|

## <a name="initialize-the-callclient-create-a-callagent-and-access-the-devicemanager"></a>Initiera CallClient, skapa en CallAgent och få åtkomst till DeviceManager

Om du vill skapa en `CallAgent` instans måste du anropa- `createCallAgent` metoden på en `CallClient` instans. Ett instans objekt returneras asynkront `CallAgent` .
`createCallAgent`Metoden tar ett `CommunicationUserCredential` as-argument som kapslar in en [åtkomsttoken](../../access-tokens.md).
För att få åtkomst till `DeviceManager` måste en callAgent-instans skapas först och sedan kan du använda `CallClient.getDeviceManager` metoden för att hämta devicemanager.

```java
String userToken = '<user token>';
CallClient callClient = new CallClient();
CommunicationUserCredential tokenCredential = new CommunicationUserCredential(userToken);
android.content.Context appContext = this.getApplicationContext(); // From within an Activity for instance
CallAgent callAgent = await callClient.createCallAgent((appContext, tokenCredential).get();
DeviceManage deviceManager = await callClient.getDeviceManager().get();
```

## <a name="place-an-outgoing-call-and-join-a-group-call"></a>Placera ett utgående samtal och delta i ett grupp anrop

Om du vill skapa och starta ett samtal måste du anropa `CallAgent.call()` -metoden och ange `Identifier` för-eller-anropen.
Om du vill ansluta till ett grupp anrop måste du anropa- `CallAgent.join()` metoden och ange ett välliggande metod nummer. Grupp-ID: n måste vara i GUID-eller UUID-format.

Skapande av anrop och start är synkront. Anrops instansen gör att du kan prenumerera på alla händelser på anropet.

### <a name="place-a-11-call-to-a-user"></a>Placera ett 1:1-anrop till en användare
För att placera ett anrop till en annan kommunikations tjänst användare, anropa `call` metoden på `callAgent` och skicka ett objekt med `communicationUserId` nyckeln.
```java
StartCallOptions startCallOptions = new StartCallOptions();
Context appContext = this.getApplicationContext();
CommunicationUser acsUserId = new CommunicationUser(<USER_ID>);
CommunicationUser participants[] = new CommunicationUser[]{ acsUserId };
call oneToOneCall = callAgent.call(appContext, participants, startCallOptions);
```

### <a name="place-a-1n-call-with-users-and-pstn"></a>Placera ett 1: n-anrop med användare och PSTN
> [!WARNING]
> För närvarande är PSTN-anrop inte tillgängligt för att placera ett 1: n-Anrop till en användare och ett PSTN-nummer som du måste ange telefonnumret för samtalet.
Resursen för kommunikations tjänster måste konfigureras för att tillåta PSTN-anrop:
```java
CommunicationUser acsUser1 = new CommunicationUser(<USER_ID>);
PhoneNumber acsUser2 = new PhoneNumber("<PHONE_NUMBER>");
CommunicationIdentifier participants[] = new CommunicationIdentifier[]{ acsUser1, acsUser2 };
StartCallOptions startCallOptions = new StartCallOptions();
Context appContext = this.getApplicationContext();
Call groupCall = callAgent.call(participants, startCallOptions);
```

### <a name="place-a-11-call-with-with-video-camera"></a>Placera ett 1:1-anrop med video kameran
> [!WARNING]
> För närvarande stöds endast en utgående lokal video ström för att placera ett samtal med video som du måste räkna upp lokala kameror med hjälp av `deviceManager` `getCameraList` API: et.
När du har valt en önskad kamera använder du den för att skapa en `LocalVideoStream` instans och skickar den till `videoOptions` som ett objekt i `localVideoStream` matrisen till en `call` metod.
När anropet ansluter börjar den automatiskt att skicka en video ström från den valda kameran till andra deltagare.
```java
Context appContext = this.getApplicationContext();
VideoDeviceInfo desiredCamera = callClient.getDeviceManager().get().getCameraList().get(0);
LocalVideoStream currentVideoStream = new LocalVideoStream(desiredCamera, appContext);
VideoOptions videoOptions = new VideoOptions(currentVideoStream);

CommunicationUser[] participants = new CommunicationUser[]{ new CommunicationUser("<acs user id>") };
StartCallOptions startCallOptions = new StartCallOptions();
startCallOptions.setVideoOptions(videoOptions);
Call call = callAgent.call(context, participants, startCallOptions);
```

### <a name="join-a-group-call"></a>Anslut till ett grupp anrop
Om du vill starta ett nytt grupp anrop eller ansluta ett pågående grupp anrop måste du anropa Join-metoden och skicka ett objekt med en `groupId` egenskap. Värdet måste vara ett GUID.
```java
Context appContext = this.getApplicationContext();
GroupCallContext groupCallContext = new groupCallContext("<GUID>");
JoinCallOptions joinCallOptions = new JoinCallOptions();

call = callAgent.join(context, groupCallContext, joinCallOptions);
```

## <a name="push-notification"></a>Push-meddelande

### <a name="overview"></a>Översikt
Mobilt push-meddelande är popup-meddelandet som du får på en mobil enhet. För att anropa, kommer vi att fokusera på VoIP (Voice-of-Internet Protocol) push-meddelanden. Vi kommer att erbjuda dig de funktioner som du kan registrera för push-meddelanden, för att hantera push-meddelanden och för att avregistrera push-meddelanden.

### <a name="prerequisite"></a>Förutsättning

Den här självstudien förutsätter att du har en Firebase med Cloud Messaging (FCM) aktive rad och att ditt Firebase Cloud Messaging är anslutet till en Azure Notification Hub-instans (ANH). Mer information finns i [ansluta Firebase till Azure](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-android-push-notification-google-fcm-get-started) .
Dessutom förutsätter du själv studie kursen att du använder Android Studio version 3,6 eller senare för att bygga ditt program.

En uppsättning behörigheter krävs för att Android-programmet ska kunna ta emot meddelanden från FCM. I AndroidManifest.xml-filen lägger du till följande behörighets uppsättning direkt efter *<manifestet... >* eller under *</application>* taggen

```XML
    <uses-permission android:name="android.permission.INTERNET"/>
    <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
    <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
```

### <a name="register-for-push-notification"></a>Registrera dig för push-meddelanden

- För att du ska kunna registrera för push-meddelanden måste programmet anropa registerPushNotification () på en *CallAgent* -instans med en token för enhets registrering.

- Så här hämtar du token för enhets registrering
1. Se till att lägga till klient biblioteket Firebase i Application modules *build. gradle* -filen genom att lägga till följande rader i avsnittet *beroenden* om det inte redan finns:
```
    // Add the client library for Firebase Cloud Messaging
    implementation 'com.google.firebase:firebase-core:16.0.8'
    implementation 'com.google.firebase:firebase-messaging:20.2.4'
```

2. På projekt nivåns *build. gradle* -fil lägger du till följande i avsnittet *beroenden* om det inte redan finns där
```
    classpath 'com.google.gms:google-services:4.3.3'
```

3. Lägg till följande plugin-program i början av filen om det inte redan finns
```
apply plugin: 'com.google.gms.google-services'
```

4. Välj *Synkronisera nu* i verktygsfältet

5. Lägg till följande kodfragment för att hämta token för enhets registrering som genereras av FCM-klient biblioteket för klient program instansen 
- Lägg till dessa import i huvud aktivitetens huvud aktivitet för instansen. De krävs för att kodfragmentet ska kunna hämta token
```
import com.google.android.gms.tasks.OnCompleteListener;
import com.google.android.gms.tasks.Task;
import com.google.firebase.iid.FirebaseInstanceId;
import com.google.firebase.iid.InstanceIdResult;
```
- Lägg till det här kodfragmentet för att hämta token
```
        FirebaseInstanceId.getInstance().getInstanceId()
                .addOnCompleteListener(new OnCompleteListener<InstanceIdResult>() {
                    @Override
                    public void onComplete(@NonNull Task<InstanceIdResult> task) {
                        if (!task.isSuccessful()) {
                            Log.w(TAG, "getInstanceId failed", task.getException());
                            return;
                        }

                        // Get new Instance ID token
                        String deviceToken = task.getResult().getToken();
                        // Log
                        Log.d(TAG, "Device Registration token retrieved successfully");
                    }
                });
```
6. Registrera token för enhets registrering med klient biblioteket för anrops tjänster för inkommande push-meddelanden för push-meddelanden

```java
String deviceRegistrationToken = "some_token";
try {
    callAgent.registerPushNotification(deviceRegistrationToken).get();
}
catch(Exception e) {
    System.out.println("Something went wrong while registering for Incoming Calls Push Notifications.")
}
```

### <a name="push-notification-handling"></a>Hantering av push-meddelanden

- För att kunna ta emot inkommande push-meddelanden anropar du *handlePushNotification ()* på en *CallAgent* -instans med en nytto Last.

1. Så här hämtar du nytto lasten från FCM:
- Skapa en ny tjänst (File > New > service > service) som utökar *FirebaseMessagingService* Firebase-klient biblioteks klassen och se till att åsidosätta *onMessageReceived* -metoden. Den här metoden är händelse hanteraren som anropas när FCM levererar push-meddelanden till programmet.

```java
public class MyFirebaseMessagingService extends FirebaseMessagingService {
    private java.util.Map<String, String> pushNotificationMessageData;

    @Override
    public void onMessageReceived(RemoteMessage remoteMessage) {
        // Check if message contains a notification payload.
        if (remoteMessage.getNotification() != null) {
            Log.d(TAG, "Message Notification Body: " + remoteMessage.getNotification().getBody());
        }
        else {
            pushNotificationMessageData = serializeDictionaryAsJson(remoteMessage.getData());
        }
    }
}
```
- Lägg också till följande tjänst definition i AndroidManifest.xml-filen inuti- <application> taggen.

```
        <service
            android:name=".MyFirebaseMessagingService"
            android:exported="false">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT" />
            </intent-filter>
        </service>
```

- När nytto lasten har hämtats kan den skickas till klient biblioteket för *kommunikations tjänster* som ska hanteras genom att anropa metoden *HandlePushNotification* på en *CallAgent* -instans.

```java
java.util.Map<String, String> pushNotificationMessageDataFromFCM = remoteMessage.getData();
try {
    callAgent.handlePushNotification(pushNotificationMessageDataFromFCM).get();
}
catch(Exception e) {
    System.out.println("Something went wrong while handling the Incoming Calls Push Notifications.");
}
```
När hanteringen av push-meddelandet lyckas och alla händelse hanterare registreras korrekt, kommer programmet att ringas.

### <a name="unregister-push-notification"></a>Avregistrera push-meddelanden

- Program kan avregistrera push-meddelanden när som helst. Anropa `unregisterPushNotification()` metoden på callAgent för avregistrering.

```java
try {
    callAgent.unregisterPushNotifications().get();
}
catch(Exception e) {
    System.out.println("Something went wrong while un-registering for all Incoming Calls Push Notifications.")
}
```

## <a name="call-management"></a>Anrops hantering
Du kan komma åt anrops egenskaper och utföra olika åtgärder under ett anrop för att hantera inställningar som rör video och ljud.

### <a name="call-properties"></a>Anrops egenskaper
* Hämta det unika ID: t för det här anropet.
```java
String callId = call.getCallId();
```

* Lär dig mer om andra deltagare i anrops inspektions `remoteParticipant` samlingen på `call` instansen:
```java
List<RemoteParticipant> remoteParticipants = call.getRemoteParticipants();
```

* Anroparens identitet om samtalet är inkommande.
```java
CommunicationIdentifier callerId = call.getCallerId();
```

* Hämta status för anropet.
```java
CallState callState = call.getState();
```
Den returnerar en sträng som representerar det aktuella status för ett anrop:
* Ingen-första anrops status
* ' Inkommande ' – anger att anropet är inkommande, måste antingen godkännas eller avvisas
* Ansluter – ursprungligt över gångs tillstånd när anropet har placerats eller godkänts
* ' Ringing ' – för ett utgående samtal – indikerar att samtal rings upp för fjärranslutna deltagare, det är "inkommande" på sin sida
* ' EarlyMedia ' – anger ett tillstånd där ett meddelande spelas upp innan anropet ansluts
* Ansluten – anropet är anslutet
* Hold '-anropet är spärrat, inget medium flödar mellan lokal slut punkt och fjärran sluten deltagare
* "Kopplar från"-över gångs läget innan anrop övergår till frånkopplat läge
* Frånkopplat-avslutande anrops tillstånd


* Om du vill veta varför ett anrop avslutat, kontrollerar du `callEndReason` egenskapen.
Den innehåller kod/under kod (att länka till dokumentationen)
```java
CallEndReason callEndReason = call.getCallEndReason();
int code = callEndReason.getCode();
int subCode = callEndReason.getSubCode();
```

* För att se om det aktuella anropet är ett inkommande anrop, inspektera `isIncoming` egenskap:
```java
boolean isIncoming = call.getIsIncoming();
```

*  För att se om den aktuella mikrofonen är avstängd, kontrollerar du `muted` egenskapen:
```java
boolean muted = call.getIsMicrophoneMuted();
```

* Om du vill kontrol lera aktiva video strömmar kontrollerar du `localVideoStreams` samlingen:
```java
List<LocalVideoStream> localVideoStreams = call.getLocalVideoStreams();
```

### <a name="mute-and-unmute"></a>Ljud av och på
Om du vill stänga av eller stänga av den lokala slut punkten kan du använda de `mute` `unmute` asynkrona API: erna:
```java
call.mute().get();
call.unmute().get();
```

### <a name="start-and-stop-sending-local-video"></a>Starta och stoppa sändning av lokal video
Om du vill starta en video måste du räkna upp kameror med hjälp av `getCameraList` API: et för `deviceManager` objektet.
Skapa sedan en ny instans av `LocalVideoStream` att skicka önskad kamera och skicka den i `startVideo` API som ett argument
```java
VideoDeviceInfo desiredCamera = <get-video-device>;
Context appContext = this.getApplicationContext();
currentVideoStream = new LocalVideoStream(desiredCamera, appContext);
videoOptions = new VideoOptions(currentVideoStream);
Future startVideoFuture = call.startVideo(currentVideoStream);
startVideoFuture.get();
```

När du har startat sändningen kommer en `LocalVideoStream` instans att läggas till i `localVideoStreams` samlingen på anrops instansen.
```java
currentVideoStream == call.getLocalVideoStreams().get(0);
```

Stoppa den lokala videon genom att skicka den `localVideoStream` tillgängliga instansen i `localVideoStreams` samlingen:
```java
call.stopVideo(localVideoStream).get();
```

Du kan växla till en annan kamera enhet medan video skickas genom att anropa `switchSource` en `localVideoStream` instans:
```java
localVideoStream.switchSource(source).get();
```

## <a name="remote-participants-management"></a>Hantering av fjärranslutna deltagare

Alla fjärranslutna deltagare representeras av `RemoteParticipant` Type och är tillgängliga via `remoteParticipants` samlingen på en samtals instans.

### <a name="list-participants-in-a-call"></a>Lista deltagare i ett samtal
`remoteParticipants`Samlingen returnerar en lista över fjärranslutna deltagare i angivet anrop:
```java
List<RemoteParticipant> remoteParticipants = call.getRemoteParticipants(); // [remoteParticipant, remoteParticipant....]
```

### <a name="remote-participant-properties"></a>Egenskaper för fjärran sluten part
En viss fjärran sluten deltagare har en uppsättning egenskaper och samlingar som är associerade med den:

* Hämta ID: t för den här fjärran deltagaren.
Identiteten är en av typen ' Identifier '
```java
CommunicationIdentifier participantIdentity = remoteParticipant.getIdentifier();
```

* Hämta tillstånd för denna fjärran sluten deltagare.
```java
ParticipantState state = remoteParticipant.getState();
```
Tillstånd kan vara en av
* Passiv-initial status
* ' Connect ' – över gångs tillstånd medan deltagare ansluter till anropet
* Ansluten – deltagaren är ansluten till anropet
* Hold '-deltagare är stoppad
* ' EarlyMedia ' – meddelandet spelas upp innan deltagaren ansluts till anropet
* ' Frånkopplad ' – slutligt tillstånd-deltagare är frånkopplat från anropet


* Om du vill veta varför en deltagare lämnat samtalet, kontrollerar du `callEndReason` egenskapen:
```java
CallEndReason callEndReason = remoteParticipant.getCallEndReason();
```

* Kontrol lera om den här fjärrparten är avstängd eller inte genom att kontrol lera `isMuted` egenskapen:
```java
boolean isParticipantMuted = remoteParticipant.getIsMuted();
```

* Om du vill kontrol lera om den här fjärrparten är intalad eller inte kontrollerar du `isSpeaking` egenskapen:
```java
boolean isParticipantSpeaking = remoteParticipant.getIsSpeaking();
```

* Om du vill kontrol lera alla video strömmar som en viss deltagare skickar i det här anropet kontrollerar du `videoStreams` samlingen:
```java
List<RemoteVideoStream> videoStreams = remoteParticipant.getVideoStreams(); // [RemoteVideoStream, RemoteVideoStream, ...]
```


### <a name="add-a-participant-to-a-call"></a>Lägg till en deltagare i ett samtal

Om du vill lägga till en deltagare till ett samtal (antingen en användare eller ett telefonnummer) kan du anropa `addParticipant` . Fjärran sluten instans skickas synkront.

```java
const acsUser = new CommunicationUser("<acs user id>");
const acsPhone = new PhoneNumber("<phone number>");
RemoteParticipant remoteParticipant1 = call.addParticipant(acsUser);
RemoteParticipant remoteParticipant2 = call.addParticipant(acsPhone);
```

### <a name="remove-participant-from-a-call"></a>Ta bort deltagare från ett samtal
Om du vill ta bort en deltagare från ett samtal (antingen en användare eller ett telefonnummer) kan du anropa `removeParticipant` .
Detta kommer att lösa asynkront när deltagaren tas bort från anropet.
Deltagaren tas också bort från `remoteParticipants` samlingen.
```java
RemoteParticipant remoteParticipant = call.getParticipants().get(0);
call.removeParticipant(acsUser).get();
call.removeParticipant(acsPhone).get();
```

## <a name="render-remote-participant-video-streams"></a>Återge video strömmar för fjärranslutna deltagare
För att visa en lista över video strömmar och skärm delnings strömmar av fjärranslutna deltagare, inspektera `videoStreams` samlingarna:
```java
RemoteParticipant remoteParticipant = call.getRemoteParticipants().get(0);
RemoteVideoStream remoteParticipantStream = remoteParticipant.getVideoStreams().get(0);
MediaStreamType streamType = remoteParticipantStream.getType(); // of type MediaStreamType.Video or MediaStreamType.ScreenSharing
```
 
Om du vill rendera en `RemoteVideoStream` från en fjärran sluten deltagare måste du prenumerera på en `OnVideoStreamsUpdated` händelse.
I händelse `isAvailable` indikerar egenskapen ändra till True att fjärrdeltagaren för närvarande skickar en ström när den händer, skapar en ny instans av en `Renderer` och sedan skapar en ny `RendererView` med asynkront `createView` API och kopplar `view.target` var som helst i användar gränssnittet för ditt program.
När tillgängligheten för en fjärrström ändras kan du välja att förstöra hela åter givningen, en speciell `RendererView` eller behålla dem, men det leder till att tomma video rutor visas.

```java
Renderer remoteVideoRenderer = new Renderer(remoteParticipantStream, appContext);
View uiView = remoteVideoRenderer.createView(new RenderingOptions(ScalingMode.Fit));
layout.addView(uiView);

remoteParticipant.addOnVideoStreamsUpdatedListener(e -> onRemoteParticipantVideoStreamsUpdated(p, e));

void onRemoteParticipantVideoStreamsUpdated(RemoteParticipant participant, RemoteVideoStreamsEvent args) {
    for(RemoteVideoStream stream : args.getAddedRemoteVideoStreams()) {
        if(stream.getIsAvailable()) {
            startRenderingVideo();
        } else {
            renderer.dispose();
        }
    }
}
```

### <a name="remote-video-stream-properties"></a>Egenskaper för fjärran sluten video ström
Fjärran sluten video strömmar har ett par egenskaper

* `Id` -ID för en fjärran sluten video ström
```java
int id = remoteVideoStream.getId();
```

* `MediaStreamType` -Kan vara video eller ScreenSharing
```java
MediaStreamType type = remoteVideoStream.getType();
```

* `isAvailable` -Anger om slut punkten för den fjärranslutna parten skickar ström
```java
boolean availability = remoteVideoStream.getIsAvailable();
```

### <a name="renderer-methods-and-properties"></a>Åter givnings metoder och egenskaper
Åter givnings objekt följande API: er

* Skapa en `RendererView` instans som senare kan bifogas i programmets användar gränssnitt för att rendera en fjärran sluten video ström.
```java
// Create a view for a video stream
renderer.createView()
```
* Ta bort åter givning och alla `RendererView` associerade med den här åter givningen
```java
renderer.dispose()
```

* `StreamSize` -storlek (bredd/höjd) för en fjärran sluten video ström
```java
StreamSize renderStreamSize = remoteVideoStream.getSize();
int width = renderStreamSize.getWidth();
int height = renderStreamSize.getHeight();
```


### <a name="rendererview-methods-and-properties"></a>Metoder och egenskaper för RendererView
När du skapar en `RendererView` kan du ange vilka `scalingMode` och `mirrored` egenskaper som ska gälla för den här vyn: skalnings läget kan vara antingen "Sträck ut" | Beskär | Passa om `mirrored` är inställt på `true` , vänds den renderade strömmen lodrätt.

```java
Renderer remoteVideoRenderer = new Renderer(remoteVideoStream, appContext);
RendererView rendererView = remoteVideoRenderer.createView(new RenderingOptions(ScalingMode.Fit));
```

De skapade RendererView kan sedan kopplas till program gränssnittet med hjälp av följande kodfragment:
```java
layout.addView(rendererView);
```

Du kan senare uppdatera skalnings läget genom att anropa `updateScalingMode` API på RendererView-objektet med en av ScalingMode. utsträckt | ScalingMode. beskärning | ScalingMode. passa som ett argument.
```java
// Update the scale mode for this view.
rendererView.updateScalingMode(ScalingMode.Crop)
```


## <a name="device-management"></a>Enhetshantering

`DeviceManager` gör att du kan räkna upp lokala enheter som kan användas i ett anrop för att överföra ljud-och video strömmar. Du kan också begära behörighet från en användare för att få åtkomst till mikrofonen och kameran med hjälp av det inbyggda webb läsar-API: et.

Du kan komma åt `deviceManager` genom att anropa `callClient.getDeviceManager()` metoden.
> [!WARNING]
> För närvarande `callAgent` måste ett objekt först instansieras för att få åtkomst till devicemanager

```java
DeviceManager deviceManager = callClient.getDeviceManager().get();
```

### <a name="enumerate-local-devices"></a>Räkna upp lokala enheter

Du kan använda uppräknings metoder på Enhetshanteraren för att komma åt lokala enheter. Uppräkning är en synkron åtgärd.

```java
//  Get a list of available video devices for use.
List<VideoDeviceInfo> localCameras = deviceManager.getCameraList(); // [VideoDeviceInfo, VideoDeviceInfo...]

// Get a list of available microphone devices for use.
List<AudioDeviceInfo> localMicrophones = deviceManager.getMicrophoneList(); // [AudioDeviceInfo, AudioDeviceInfo...]

// Get a list of available speaker devices for use.
List<AudioDeviceInfo> localSpeakers = deviceManager.getSpeakerList(); // [AudioDeviceInfo, AudioDeviceInfo...]
```

### <a name="set-default-microphonespeaker"></a>Ange standard mikrofon/talare

Med enhets hanteraren kan du ange en standardenhet som ska användas när ett samtal startas.
Om klientens standardvärden inte har angetts, kommer kommunikations tjänsterna att återgå till standardinställningarna för operativ systemet.

```java

// Get the microphone device that is being used.
AudioDeviceInfo defaultMicrophone = deviceManager.getMicrophoneList().get(0);

// Set the microphone device to use.
deviceManager.setMicrophone(defaultMicrophone);

// Get the speaker device that is being used.
AudioDeviceInfo defaultSpeaker = deviceManager.getSpeakerList().get(0);

// Set the speaker device to use.
deviceManager.setSpeaker(defaultSpeaker);
```

### <a name="local-camera-preview"></a>Lokal kamera för hands version

Du kan använda `DeviceManager` och `Renderer` för att börja rendera strömmar från den lokala kameran. Den här data strömmen skickas inte till andra deltagare. Det är en lokal förhands gransknings matning. Detta är en asynkron åtgärd.

```java
VideoDeviceInfo videoDevice = <get-video-device>;
Context appContext = this.getApplicationContext();
currentVideoStream = new LocalVideoStream(videoDevice, appContext);
videoOptions = new VideoOptions(currentVideoStream);

Renderer previewRenderer = new Renderer(currentVideoStream, appContext);
View uiView previewRenderer.createView(new RenderingOptions(ScalingMode.Fit));

// Attach the renderingSurface to a viewable location on the app at this point
layout.addView(uiView);
```

## <a name="eventing-model"></a>Händelse modell
Du kan prenumerera på de flesta egenskaper och samlingar för att få ett meddelande när värdena ändras.

### <a name="properties"></a>Egenskaper
Så här prenumererar du på `property changed` händelser:

```java
// subscribe
PropertyChangedListener callStateChangeListener = new PropertyChangedListener()
{
    @Override
    public void onPropertyChanged(PropertyChangedEvent args)
    {
        Log.d("The call state has changed.");
    }
}
call.addOnCallStateChangedListener(callStateChangeListener);

//unsubscribe
call.removeOnCallStateChangedListener(callStateChangeListener);
```

### <a name="collections"></a>Samlingar
Så här prenumererar du på `collection updated` händelser:

```java
LocalVideoStreamsChangedListener localVideoStreamsChangedListener = new LocalVideoStreamsChangedListener()
{
    @Override
    public void onLocalVideoStreamsUpdated(LocalVideoStreamsEvent localVideoStreamsEventArgs) {
        Log.d(localVideoStreamsEventArgs.getAddedStreams().size());
        Log.d(localVideoStreamsEventArgs.getRemovedStreams().size());
    }
}
call.addOnLocalVideoStreamsChangedListener(localVideoStreamsChangedListener);
// To unsubscribe
call.removeOnLocalVideoStreamsChangedListener(localVideoStreamsChangedListener);
```
