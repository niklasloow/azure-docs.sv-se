---
title: Migrera från Google Maps till Azure Maps | Microsoft Azure Maps
description: 'Så här migrerar du från Google Maps till Microsoft Azure Maps. Vägledningen vägleder dig genom hur du växlar till Azure Maps API: er och SDK: er.'
author: rbrundritt
ms.author: richbrun
ms.date: 09/23/2020
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: cpendle
ms.custom: ''
ms.openlocfilehash: c60890b301ba650c95584e33b5326217086c08c4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91264175"
---
# <a name="migrate-from-google-maps-to-azure-maps"></a>Migrera från Google Maps till Azure Maps

Den här artikeln innehåller insikter om hur du migrerar webb-, mobil-och serverbaserade program från Google Maps till Microsoft Azure Maps-plattformen. Den här självstudien innehåller jämför ande kod exempel, förslag på migrering och metod tips för migrering till Azure Maps.

## <a name="azure-maps-platform-overview"></a>Översikt över Azure Maps plattform

Azure Maps ger utvecklare från alla branscher kraftfulla geospatiala funktioner. Funktionerna packas med regelbundna uppdaterade kart data för att tillhandahålla geografisk kontext för webb-och mobil program. Azure Maps har en Azure One API-kompatibel uppsättning REST API: er. REST-API: erna ger kartor över åter givning, sökning, routning, trafik, tids zoner, geolokalisering, avgränsning, kart data, väder, rörlighet och rums drift. Åtgärder åtföljs av både webb-och Android-SDK: er för att utveckla enkla, flexibla och portabla över flera plattformar.

## <a name="high-level-platform-comparison"></a>Plattforms jämförelse på hög nivå

Tabellen innehåller en övergripande lista med Azure Maps funktioner som motsvarar Google Maps-funktioner. Den här listan visar inte alla Azure Maps funktionerna. Ytterligare Azure Maps funktioner är: hjälpmedel, ISO Kron, spatial drift, direkt kart panels åtkomst, batch Services och data täcknings jämförelser (det vill säga bilder-täckning).

| Google Maps-funktion         | Azure Maps support                     |
|-----------------------------|:--------------------------------------:|
| Web SDK                     | ✓                                      |
| Android SDK                 | ✓                                      |
| iOS SDK                     | Planerad                                |
| API: er för REST service           | ✓                                      |
| Vägvisningar (routning)        | ✓                                      |
| Avstånds mat ris             | ✓                                      |
| Höjning                   | Planerad                                |
| Landskod (framåt/omvänd) | ✓                                      |
| Geolocation                 | Saknas                                    |
| Närmaste vägar               | ✓                                      |
| Sök efter platser               | ✓                                      |
| Plats information              | Saknas – webbplats & telefonnummer är tillgängligt |
| Placerar foton               | Saknas                                    |
| Placera Autoavsluta          | ✓                                      |
| Fäst vid väg                | ✓                                      |
| Hastighets begränsningar                | ✓                                      |
| Statiska Maps                 | ✓                                      |
| Statisk gata vy          | Saknas                                    |
| Tidszon                   | ✓                                      |
| Mappar inbäddat API           | Saknas                                    |
| Kart-URL: er                    | Saknas                                    |

Google Maps innehåller grundläggande nyckelbaserad autentisering. Azure Maps tillhandahåller både grundläggande nyckelbaserad autentisering och Azure Active Directory autentisering. Azure Active Directory-autentisering innehåller fler säkerhetsfunktioner jämfört med den grundläggande nyckelbaserade autentiseringen.

## <a name="licensing-considerations"></a>Licens överväganden

När du migrerar till Azure Maps från Google Maps bör du tänka på följande saker om licensiering.

- Azure Maps avgifter för användning av interaktiva kartor, som baseras på antalet inlästa kart paneler. Google Maps å andra sidan debiteras för inläsning av kart kontrollen. I de interaktiva Azure Maps SDK: erna cachelagras kart paneler automatiskt för att minska utvecklings kostnaden. En Azure Maps transaktion skapas för varje 15 kart paneler som läses in. I de interaktiva Azure Maps SDK: erna används 512-pixel paneler och i genomsnitt skapas en eller färre transaktioner per sid visning.
- Ofta är det mer kostnads effektivt att ersätta statiska kart bilder från Google Maps-webbtjänster med Azure Maps Web SDK. Azure Maps Web SDK använder kart paneler. Om inte användaren Pans och zoomar in kartan genererar tjänsten ofta bara en bråkdel av en transaktion per kart överföring. Azure Maps Web SDK har alternativ för att inaktivera panorering och zoomning, om så önskas. Dessutom innehåller Azure Maps Web SDK ett mer visualiserings alternativ än den statiska kart webb tjänsten.
- Azure Maps tillåter att data från sin plattform lagras i Azure. Dessutom kan data cachelagras på andra platser i upp till sex månader enligt [användnings villkoren](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46).

Här följer några relaterade resurser för Azure Maps:

- [Sidan Azure Maps prissättning](https://azure.microsoft.com/pricing/details/azure-maps/)
- [Priskalkylator för Azure](https://azure.microsoft.com/pricing/calculator/?service=azure-maps)
- [Azure Maps användnings villkor](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=46) (ingår i villkoren för Microsoft Online Services)
- [Välj rätt pris nivå i Azure Maps](https://docs.microsoft.com/azure/azure-maps/choose-pricing-tier)

## <a name="suggested-migration-plan"></a>Föreslagen migrations plan

Följande är en uppgraderings plan med höga nivåer.

1. Ta inventering av Google Maps SDK: er och tjänster som används i programmet. Kontrol lera att Azure Maps tillhandahåller alternativa SDK: er och tjänster.
2. Om du inte redan har ett kan du skapa en Azure-prenumeration på [https://azure.com](https://azure.com) .
3. Skapa ett Azure Maps konto ([dokumentation](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys)) och en autentiseringsnyckel eller Azure Active Directory ([dokumentation](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication)).
4. Migrera din program kod.
5. Testa det migrerade programmet.
6. Distribuera det migrerade programmet till produktion.

## <a name="create-an-azure-maps-account"></a>Skapa ett Azure Maps-konto

Följ dessa steg om du vill skapa ett Azure Maps konto och få åtkomst till Azure Maps-plattformen:

1. Om du inte har någon Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) innan du börjar.
2. Logga in på [Azure-portalen](https://portal.azure.com/).
3. Skapa ett [Azure Maps-konto](https://docs.microsoft.com/azure/azure-maps/how-to-manage-account-keys). 
4. [Hämta din Azure Maps prenumerations nyckel](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication#view-authentication-details) eller konfigurera Azure Active Directory autentisering för förbättrad säkerhet.

## <a name="azure-maps-technical-resources"></a>Azure Maps tekniska resurser

Här är en lista över användbara tekniska resurser för Azure Maps.

- : [https://azure.com/maps](https://azure.com/maps)
- Handlingar [https://aka.ms/AzureMapsDocs](https://aka.ms/AzureMapsDocs)
- Kod exempel för webb-SDK: [https://aka.ms/AzureMapsSamples](https://aka.ms/AzureMapsSamples)
- Forum för utvecklare: [https://aka.ms/AzureMapsForums](https://aka.ms/AzureMapsForums)
- Videos [https://aka.ms/AzureMapsVideos](https://aka.ms/AzureMapsVideos)
- Blogg [https://aka.ms/AzureMapsBlog](https://aka.ms/AzureMapsBlog)
- Teknisk blogg: [https://aka.ms/AzureMapsTechBlog](https://aka.ms/AzureMapsTechBlog)
- Azure Maps feedback (UserVoice): [https://aka.ms/AzureMapsFeedback](https://aka.ms/AzureMapsFeedback)
- [Azure Maps Jupyter Notebook](https://github.com/Azure-Samples/Azure-Maps-Jupyter-Notebook)

## <a name="migration-support"></a>Stöd för migrering

Utvecklare kan söka efter stöd för migrering via [forumen](https://aka.ms/AzureMapsForums) eller via ett av de många support alternativen för Azure: [https://azure.microsoft.com/support/options](https://azure.microsoft.com/support/options)

Du kan lära dig att migrera ditt Google Maps-program med: 

[Migrera en Android-app](migrate-from-google-maps-android-app.md) 

[Migrera en webbtjänst](migrate-from-google-maps-web-services.md) 

[Migrera en webbapp](migrate-from-google-maps-web-app.md)