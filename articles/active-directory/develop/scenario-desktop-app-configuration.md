---
title: 'Konfigurera skrivbordsappar som anropar webb-API: er – Microsoft Identity Platform | Azure'
description: 'Lär dig hur du konfigurerar koden för en stationär app som anropar webb-API: er'
services: active-directory
author: jmprieur
manager: CelesteDG
ms.service: active-directory
ms.subservice: develop
ms.topic: conceptual
ms.workload: identity
ms.date: 10/30/2019
ms.author: jmprieur
ms.custom: aaddev, devx-track-python
ms.openlocfilehash: 56d2e01d35f25ac469e808a805382463d2b23ceb
ms.sourcegitcommit: b8702065338fc1ed81bfed082650b5b58234a702
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/11/2020
ms.locfileid: "88121110"
---
# <a name="desktop-app-that-calls-web-apis-code-configuration"></a>Skriv bords app som anropar webb-API: kod konfiguration

Nu när du har skapat ditt program får du lära dig hur du konfigurerar koden med programmets koordinater.

## <a name="microsoft-authentication-libraries"></a>Microsoft Authentication libraries

Följande Microsoft-MSALs (Authentication Libraries) stöder Skriv bords program.

  Microsofts autentiseringsbibliotek | Beskrivning
  ------------ | ----------
  ![MSAL.NET](media/sample-v2-code/logo_NET.png) <br/> MSAL.NET  | Har stöd för att skapa ett Skriv bords program på flera plattformar, till exempel Linux, Windows och macOS.
  ![Python](media/sample-v2-code/logo_python.png) <br/> MSAL python | Har stöd för att skapa ett Skriv bords program på flera plattformar.
  ![Java](media/sample-v2-code/logo_java.png) <br/> MSAL Java | Har stöd för att skapa ett Skriv bords program på flera plattformar.
  ![MSAL iOS](media/sample-v2-code/logo_iOS.png) <br/> MSAL iOS | Stöder Skriv bords program som bara körs på macOS.

## <a name="public-client-application"></a>Offentligt klientprogram

I kodvyn är Skriv bords program offentliga klient program. Konfigurationen är lite annorlunda beroende på om du använder interaktiv autentisering eller inte.

# <a name="net"></a>[.NET](#tab/dotnet)

Du behöver bygga och manipulera MSAL.NET `IPublicClientApplication` .

![IPublicClientApplication](media/scenarios/public-client-application.png)

### <a name="exclusively-by-code"></a>Enbart med kod

Följande kod instansierar ett offentligt klient program och loggar in användare i det Microsoft Azure offentliga molnet med ett arbets-eller skol konto eller en personlig Microsoft-konto.

```csharp
IPublicClientApplication app = PublicClientApplicationBuilder.Create(clientId)
    .Build();
```

Om du avser att använda interaktiv autentisering eller enhets kod flöde, som tidigare sett, använder du `.WithRedirectUri` modifieraren.

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="use-configuration-files"></a>Använda konfigurationsfiler

Följande kod instansierar ett offentligt klient program från ett konfigurations objekt, som kan fyllas i program mässigt eller läsas från en konfigurations fil.

```csharp
PublicClientApplicationOptions options = GetOptions(); // your own method
IPublicClientApplication app = PublicClientApplicationBuilder.CreateWithApplicationOptions(options)
        .WithDefaultRedirectUri()
        .Build();
```

### <a name="more-elaborated-configuration"></a>Mer avancerad konfiguration

Du kan utveckla program byggnaden genom att lägga till ett antal modifierare. Om du till exempel vill att ditt program ska vara ett program med flera innehavare i ett nationellt moln, till exempel amerikanska myndigheter som visas här, kan du skriva:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithDefaultRedirectUri()
        .WithAadAuthority(AzureCloudInstance.AzureUsGovernment,
                         AadAuthorityAudience.AzureAdMultipleOrgs)
        .Build();
```

MSAL.NET innehåller också en modifierare för Active Directory Federation Services (AD FS) 2019:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithAdfsAuthority("https://consoso.com/adfs")
        .Build();
```

Slutligen, om du vill hämta token för en Azure Active Directory (Azure AD) B2C-klient, anger du din klient organisation så som visas i följande kodfragment:

```csharp
IPublicClientApplication app;
app = PublicClientApplicationBuilder.Create(clientId)
        .WithB2CAuthority("https://fabrikamb2c.b2clogin.com/tfp/{tenant}/{PolicySignInSignUp}")
        .Build();
```

### <a name="learn-more"></a>Mer information

Lär dig mer om hur du konfigurerar ett MSAL.NET Desktop-program:

- En lista över alla modifierare som är tillgängliga på `PublicClientApplicationBuilder` finns i referens dokumentationen [PublicClientApplicationBuilder](/dotnet/api/microsoft.identity.client.publicclientapplicationbuilder#methods).
- En beskrivning av alla alternativ som visas i finns i `PublicClientApplicationOptions` [PublicClientApplicationOptions](/dotnet/api/microsoft.identity.client.publicclientapplicationoptions) i referens dokumentationen.

### <a name="complete-example-with-configuration-options"></a>Slutför exempel med konfigurations alternativ

Föreställ dig ett .NET Core-konsolprogram som har följande `appsettings.json` konfigurations fil:

```json
{
  "Authentication": {
    "AzureCloudInstance": "AzurePublic",
    "AadAuthorityAudience": "AzureAdMultipleOrgs",
    "ClientId": "ebe2ab4d-12b3-4446-8480-5c3828d04c50"
  },

  "WebAPI": {
    "MicrosoftGraphBaseEndpoint": "https://graph.microsoft.com"
  }
}
```

Du har lite kod att läsa i den här filen med hjälp av. NET-tillhandahållet konfigurations ramverk:

```csharp
public class SampleConfiguration
{
 /// <summary>
 /// Authentication options
 /// </summary>
 public PublicClientApplicationOptions PublicClientApplicationOptions { get; set; }

 /// <summary>
 /// Base URL for Microsoft Graph (it varies depending on whether the application runs
 /// in Microsoft Azure public clouds or national or sovereign clouds)
 /// </summary>
 public string MicrosoftGraphBaseEndpoint { get; set; }

 /// <summary>
 /// Reads the configuration from a JSON file
 /// </summary>
 /// <param name="path">Path to the configuration json file</param>
 /// <returns>SampleConfiguration as read from the json file</returns>
 public static SampleConfiguration ReadFromJsonFile(string path)
 {
  // .NET configuration
  IConfigurationRoot Configuration;
  var builder = new ConfigurationBuilder()
                    .SetBasePath(Directory.GetCurrentDirectory())
                    .AddJsonFile(path);
  Configuration = builder.Build();

  // Read the auth and graph endpoint configuration
  SampleConfiguration config = new SampleConfiguration()
  {
   PublicClientApplicationOptions = new PublicClientApplicationOptions()
  };
  Configuration.Bind("Authentication", config.PublicClientApplicationOptions);
  config.MicrosoftGraphBaseEndpoint =
  Configuration.GetValue<string>("WebAPI:MicrosoftGraphBaseEndpoint");
  return config;
 }
}
```

Skriv nu följande kod för att skapa programmet:

```csharp
SampleConfiguration config = SampleConfiguration.ReadFromJsonFile("appsettings.json");
var app = PublicClientApplicationBuilder.CreateWithApplicationOptions(config.PublicClientApplicationOptions)
           .WithDefaultRedirectUri()
           .Build();
```

Innan du anropar `.Build()` -metoden kan du åsidosätta konfigurationen med anrop till `.WithXXX` metoder som visas ovan.

# <a name="java"></a>[Java](#tab/java)

Här är klassen som används i MSAL Java-utvecklingsprogram för att konfigurera exemplen: [testdata](https://github.com/AzureAD/microsoft-authentication-library-for-java/blob/dev/src/samples/public-client/).

```Java
PublicClientApplication pca = PublicClientApplication.builder(CLIENT_ID)
        .authority(AUTHORITY)
        .build();
```

# <a name="python"></a>[Python](#tab/python)

```Python
config = json.load(open(sys.argv[1]))

app = msal.PublicClientApplication(
    config["client_id"], authority=config["authority"],
    # token_cache=...  # Default cache is in memory only.
                       # You can learn how to use SerializableTokenCache from
                       # https://msal-python.rtfd.io/en/latest/#msal.SerializableTokenCache
    )
```

# <a name="macos"></a>[MacOS](#tab/macOS)

Följande kod instansierar ett offentligt klient program och loggar in användare i det Microsoft Azure offentliga molnet med ett arbets-eller skol konto eller en personlig Microsoft-konto.

### <a name="quick-configuration"></a>Snabb konfiguration

Mål-C:

```objc
NSError *msalError = nil;

MSALPublicClientApplicationConfig *config = [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"];
MSALPublicClientApplication *application = [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&msalError];
```

Införliva
```swift
let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>")
if let application = try? MSALPublicClientApplication(configuration: config){ /* Use application */}
```

### <a name="more-elaborated-configuration"></a>Mer avancerad konfiguration

Du kan utveckla program byggnaden genom att lägga till ett antal modifierare. Om du till exempel vill att ditt program ska vara ett program med flera innehavare i ett nationellt moln, till exempel amerikanska myndigheter som visas här, kan du skriva:

Mål-C:

```objc
MSALAADAuthority *aadAuthority =
                [[MSALAADAuthority alloc] initWithCloudInstance:MSALAzureUsGovernmentCloudInstance
                                                   audienceType:MSALAzureADMultipleOrgsAudience
                                                      rawTenant:nil
                                                          error:nil];

MSALPublicClientApplicationConfig *config =
                [[MSALPublicClientApplicationConfig alloc] initWithClientId:@"<your-client-id-here>"
                                                                redirectUri:@"<your-redirect-uri-here>"
                                                                  authority:aadAuthority];

NSError *applicationError = nil;
MSALPublicClientApplication *application =
                [[MSALPublicClientApplication alloc] initWithConfiguration:config error:&applicationError];
```

Införliva

```swift
let authority = try? MSALAADAuthority(cloudInstance: .usGovernmentCloudInstance, audienceType: .azureADMultipleOrgsAudience, rawTenant: nil)

let config = MSALPublicClientApplicationConfig(clientId: "<your-client-id-here>", redirectUri: "<your-redirect-uri-here>", authority: authority)
if let application = try? MSALPublicClientApplication(configuration: config) { /* Use application */}
```
---

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Hämta en token för en desktop-app](scenario-desktop-acquire-token.md)