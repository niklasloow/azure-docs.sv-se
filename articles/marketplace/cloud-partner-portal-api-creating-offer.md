---
title: Skapa eller ändra ett erbjudande – Azure Marketplace
description: API för att skapa ett nytt eller uppdatera ett befintligt erbjudande.
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: reference
author: mingshen-ms
ms.author: mingshen
ms.date: 07/29/2020
ms.openlocfilehash: dba47073a6676b17c091af048f583ca3e7712999
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2020
ms.locfileid: "87420234"
---
# <a name="create-or-modify-an-offer"></a>Skapa eller ändra ett erbjudande

> [!NOTE]
> Cloud Partner Portal API: er är integrerade i och fortsätter att fungera i Partner Center. Över gången introducerar små ändringar. Granska ändringarna som anges i [Cloud Partner Portal API-referensen](./cloud-partner-portal-api-overview.md) för att se till att koden fortsätter att fungera efter över gången till Partner Center. CPP-API: er bör endast användas för befintliga produkter som redan har integrerats före över gången till Partner Center. nya produkter bör använda API: er för överföring av Partner Center.

Det här anropet uppdaterar ett enskilt erbjudande i utgivarens namnrymd eller skapar ett nytt erbjudande.

  `PUT https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>?api-version=2017-10-31`

## <a name="uri-parameters"></a>URI-parametrar

|  **Namn**         |  **Beskrivning**                      |  **Datatyp**  |
|  --------         |  ----------------                     |  -------------  |
| publisherId       |  Utgivar identifierare, till exempel`contoso` |   Sträng |
| offerId           |  Erbjudande-ID                     |   Sträng        |
| api-version       |  Senaste versionen av API: et            |   Datum           |
|  |  |  |

## <a name="header"></a>Huvud

|  **Namn**        |  **Värde**               |
|  ---------       |  ----------              | 
| Content-Type     | `application/json`       |
| Auktorisering    | `Bearer YOUR_TOKEN`      |
|  |  |

## <a name="body-example"></a>Body-exempel

I följande exempel skapas ett erbjudande med offerID av `contosovirtualmachine` .

### <a name="request"></a>Förfrågan

``` json
  {
      "publisherId": "contoso",
      "offerTypeId": "microsoft-azure-virtualmachines",
      "id": "contosovirtualmachine",
      "offerTypeVersions": {
          "microsoft-azure-virtualmachines": 87,
          "microsoft-azure-marketplace": 39
      },
      "definition": {
          "displayText": "Contoso Virtual Machine Offer",
          "offer": {
          "microsoft-azure-marketplace.title": "Contoso App",
          "microsoft-azure-marketplace.summary": "Contoso App makes dev ops a breeze",
          "microsoft-azure-marketplace.longSummary": "Contoso App makes dev ops a breeze",
          "microsoft-azure-marketplace.description": "Contoso App makes dev ops a breeze",
          "microsoft-azure-marketplace.offerMarketingUrlIdentifier": "contosoapp",
          "microsoft-azure-marketplace.allowedSubscriptions": ["59160c40-2e25-4dcf-a2fd-6514cb08bf08"],
          "microsoft-azure-marketplace.usefulLinks": [{
              "linkTitle": "Contoso App for Azure",
              "linkUrl": "https://azuremarketplace.microsoft.com"
          }],
          "microsoft-azure-marketplace.categoryMap": [
                {
                    "categoryL1": "analytics",
                    "categoryL2-analytics": [
                        "visualization-and-reporting"
                    ]
                },
                {
                    "categoryL1": "ai-plus-machine-learning",
                    "categoryL2-ai-plus-machine-learning":[
                        "bot-services",
                        "cognitive-services",
                        "other"
                    ]
                }
            ],
          "microsoft-azure-marketplace.smallLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/6218c455-9cbc-450c-9920-f2e7a69ee132.png?sv=2014-02-14&sr=b&sig=6O8MM9dgiJ48VK0MwddkyVbprRAnBszyhVkVHGShhkI%3D&se=2019-03-28T19%3A46%3A50Z&sp=r",
          "microsoft-azure-marketplace.mediumLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/557e714b-2f31-4e12-b0cc-e48dd840edf4.png?sv=2014-02-14&sr=b&sig=NwL67NTQf9Gc9VScmZehtbHXpYmxhwZc2foy3o4xavs%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
          "microsoft-azure-marketplace.largeLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/142485da-784c-44cb-9523-d4f396446258.png?sv=2014-02-14&sr=b&sig=xaMxhwx%2FlKYfz33mJGIg8UBdVpsOwVvqhjTJ883o0iY%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
          "microsoft-azure-marketplace.wideLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/48af9013-1df7-4c94-8da8-4626e5039ce0.png?sv=2014-02-14&sr=b&sig=%2BnN7f2tprkrqb45ID6JlT01zXcy1PMTkWXtLKD6nfoE%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
          "microsoft-azure-marketplace.heroLogo": "https://publishingapistore.blob.core.windows.net/testcontent/D6191_publishers_contoso/contosovirtualmachine/c46ec74d-d214-4fb5-9082-3cee55200eba.png?sv=2014-02-14&sr=b&sig=RfDvjowFGpP4WZGAHylbF2CuXwO2NXOrwycrXEJvJI4%3D&se=2019-03-28T19%3A46%3A49Z&sp=r",
          "microsoft-azure-marketplace.screenshots": [],
          "microsoft-azure-marketplace.videos": [],
          "microsoft-azure-marketplace.leadDestination": "None",
          "microsoft-azure-marketplace.privacyURL": "https://azuremarketplace.microsoft.com",
          "microsoft-azure-marketplace.termsOfUse": "Terms of use",
          "microsoft-azure-marketplace.engineeringContactName": "Jon Doe",
          "microsoft-azure-marketplace.engineeringContactEmail": "jondoe@outlook.com",
          "microsoft-azure-marketplace.engineeringContactPhone": "555-555-5555",
          "microsoft-azure-marketplace.supportContactName": "Jon Doe",
          "microsoft-azure-marketplace.supportContactEmail": "jondoe@outlook.com",
          "microsoft-azure-marketplace.supportContactPhone": "555-555-5555",
          "microsoft-azure-marketplace.publicAzureSupportUrl": "",
          "microsoft-azure-marketplace.fairfaxSupportUrl": ""
      },
      "plans": 
      [
          {
              "planId": "contososkuidentifier",
              "microsoft-azure-virtualmachines.skuTitle": "Contoso App",
              "microsoft-azure-virtualmachines.skuSummary": "Contoso App makes dev ops a breeze.",
              "microsoft-azure-virtualmachines.skuDescription": "This is a description for the Contoso App that makes dev ops a breeze.",
              "microsoft-azure-virtualmachines.hideSKUForSolutionTemplate": false,
              "microsoft-azure-virtualmachines.cloudAvailability": ["PublicAzure"],
              "virtualMachinePricing": {
                  "isByol": true,
                  "freeTrialDurationInMonths": 0
              },
              "microsoft-azure-virtualmachines.operatingSystemFamily": "Windows",
              "microsoft-azure-virtualmachines.windowsOSType": "Other",
              "microsoft-azure-virtualmachines.operationSystem": "Contoso App",
              "microsoft-azure-virtualmachines.recommendedVMSizes": ["a0-basic", "a0-standard", "a1-basic", "a1-standard", "a2-basic", "a2-standard"],
              "microsoft-azure-virtualmachines.openPorts": [],
              "microsoft-azure-virtualmachines.vmImages": 
              {
                  "1.0.1": 
                  {
                      "osVhdUrl": "http://contosoteststorage.blob.core.windows.net/test/contosoVM.vhd?sv=2014-02-14&sr=c&sig=WlDo6Q4xwYH%2B5QEJbItPUVdgHhBcrVxPBmntZ2vU96w%3D&st=2016-06-25T18%3A30%3A00Z&se=2017-06-25T18%3A30%3A00Z&sp=rl",
                      "lunVhdDetails": []
                  }
              },
              "regions": ["AZ"]
          }
          ]
      },
      "eTag": "W/\"datetime'2017-06-07T06%3A15%3A40.4771399Z'\"",
      "version": 5
  }
```

### <a name="response"></a>Svarsåtgärder

``` json
 {
         "offerTypeId": "microsoft-azure-virtualmachines",
         "publisherId": "contoso",
         "status": "neverPublished",
         "id": "contosovirtualmachine",
         "version": 1,
         "definition": {
         "displayText": "Contoso Virtual Machine Offer",
         "offer": 
         {
             "microsoft-azure-marketplace-testdrive.videos": [],
             "microsoft-azure-marketplace.title": "Contoso App",
             "microsoft-azure-marketplace.summary": "Contoso App makes dev ops a breeze",
             "microsoft-azure-marketplace.longSummary": "Contoso App makes dev ops a breeze",
             "microsoft-azure-marketplace.description": "Contoso App makes dev ops a breeze",
             "microsoft-azure-marketplace.offerMarketingUrlIdentifier": "contosoapp",
             "microsoft-azure-marketplace.allowedSubscriptions": 
             [
                 "59160c40-2e25-4dcf-a2fd-6514cb08bf08"
             ],
             "microsoft-azure-marketplace.usefulLinks": 
             [
                 {
                     "linkTitle": "Contoso App for Azure",
                     "linkUrl": "https://azuremarketplace.microsoft.com"
                 }
             ],
             "microsoft-azure-marketplace.categoryMap":
             [
                 {
                    "categoryL1": "analytics",
                    "categoryL2-analytics": [
                    "visualization-and-reporting"
                    ]
                 },
                 {
                    "categoryL1": "ai-plus-machine-learning",
                    "categoryL2-ai-plus-machine-learning": [
                    "bot-services",
                    "cognitive-services",
                    "other"
                    ]
                 }
             ],
             "microsoft-azure-marketplace.smallLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/8affcd28-60a5-4839-adf8-c560e069fd61.png?sv=2014-02-14&sr=b&sig=nGErAgn%2BDUecrX892wcmk32kh0MHgIZeJ5jcKyY%2Fuew%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.mediumLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/39550bca-1110-432c-9ea9-e12b3a2288cd.png?sv=2014-02-14&sr=b&sig=4X0hlkXYtuZOmcYq%2BsbYVZz3k5k26kngcFX6yBAJjNI%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.largeLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/ce3576e3-df12-4074-b0a3-0b8d3f329df1.png?sv=2014-02-14&sr=b&sig=mFhtCUQh%2FbFz10nlIWbqsz6jq5MBZ0M%2F5cIREE9P6V0%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.wideLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/476d6edd-12d3-4414-9def-d2970c4a9de4.png?sv=2014-02-14&sr=b&sig=pg4MDSZjAb8w8D%2FrQ9RT%2BodpynSy%2FlYOvpx0yeam2Bw%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.heroLogo": "https://publishingstoredm.blob.core.windows.net/prodcontent/D6191_publishers_marketplace:2Dtest/testaoffer/46c85b7b-4438-4e0d-8218-24fb5651727a.png?sv=2014-02-14&sr=b&sig=wIsCOO5%2BDj8NsLVSwwzwTgogF71oA7Q1XjKhNB1ni5g%3D&se=2020-03-28T22%3A27%3A13Z&sp=r",
             "microsoft-azure-marketplace.screenshots": [],
             "microsoft-azure-marketplace.videos": [],
             "microsoft-azure-marketplace.leadDestination": "None",
             "microsoft-azure-marketplace.tableLeadConfiguration": {},
             "microsoft-azure-marketplace.blobLeadConfiguration": {},
             "microsoft-azure-marketplace.salesForceLeadConfiguration": {},
             "microsoft-azure-marketplace.crmLeadConfiguration": {},
             "microsoft-azure-marketplace.httpsEndpointLeadConfiguration": {},
             "microsoft-azure-marketplace.marketoLeadConfiguration": {},
             "microsoft-azure-marketplace.privacyURL": "https://azuremarketplace.microsoft.com",
             "microsoft-azure-marketplace.termsOfUse": "Terms of use",
             "microsoft-azure-marketplace.engineeringContactName": "Jon Doe",
             "microsoft-azure-marketplace.engineeringContactEmail": "jondoe@outlook.com",
             "microsoft-azure-marketplace.engineeringContactPhone": "555-555-5555",
             "microsoft-azure-marketplace.supportContactName": "Jon Doe",
             "microsoft-azure-marketplace.supportContactEmail": "jondoe@outlook.com",
             "microsoft-azure-marketplace.supportContactPhone": "555-555-5555",
             "microsoft-azure-marketplace.publicAzureSupportUrl": "",
             "microsoft-azure-marketplace.fairfaxSupportUrl": ""
         },
         "plans": 
         [
             {
                 "planId": "contososkuidentifier",
                 "microsoft-azure-virtualmachines.skuTitle": "Contoso App (Old Title)",
                 "microsoft-azure-virtualmachines.skuSummary": "Contoso App makes dev ops a breeze.",
                 "microsoft-azure-virtualmachines.skuDescription": "This is a description for the Contoso App that makes dev ops a breeze.",
                 "microsoft-azure-virtualmachines.hideSKUForSolutionTemplate": false,
                 "microsoft-azure-virtualmachines.cloudAvailability": 
                 [
                     "PublicAzure"
                 ],
                 "microsoft-azure-virtualmachines.certificationsFairfax": [],
                 "virtualMachinePricing": {
                     "isByol": true,
                     "freeTrialDurationInMonths": 0
                 },
                 "microsoft-azure-virtualmachines.operatingSystemFamily": "Windows",
                 "microsoft-azure-virtualmachines.operationSystem": "Contoso App",
                 "microsoft-azure-virtualmachines.recommendedVMSizes": 
                 [
                     "a0-basic",
                     "a0-standard",
                     "a1-basic",
                     "a1-standard",
                     "a2-basic",
                     "a2-standard"
                 ],
                 "microsoft-azure-virtualmachines.openPorts": [],
                 "microsoft-azure-virtualmachines.vmImages": 
                 {
                     "1.0.1": 
                     {
                         "osVhdUrl": "http://contosoteststorage.blob.core.windows.net/test/contosoVM.vhd?sv=2014-02-14&sr=c&sig=WlDo6Q4xwYH%2B5QEJbItPUVdgHhBcrVxPBmntZ2vU96w%3D&st=2016-06-25T18%3A30%3A00Z&se=2017-06-25T18%3A30%3A00Z&sp=rl",
                         "lunVhdDetails": []
                     }
                 },
                 "regions":
                 [
                     "AZ"
                 ]
             } 
         ]
     },
     "changedTime": "2018-03-28T22:27:13.8363879Z"
 }
```

> [!NOTE]
> Om du vill ändra det här erbjudandet lägger du till ett **If-Match-** huvud inställt på * på begäran ovan. Använd samma begär ande text som ovan, men ändra värdena efter behov. 

### <a name="response-status-codes"></a>Svars status koder

| **Kod**  |  **Beskrivning**                                                                            |
| --------  |  ---------------                                                                            |
|  200      | `OK`. Begäran har bearbetats och erbjudandet har ändrats.           |
|  201      | `Created`. Begäran har bearbetats och erbjudandet har skapats.   |
|  400      | `Bad/Malformed request`. Fel svars texten kan innehålla mer information.            |
|  403      | `Forbidden`. Klienten har inte åtkomst till det begärda namn området.                     |
|  404      | `Not found`. Den entitet som klienten refererar till finns inte.                           |
|  412      | Servern uppfyller inte ett av de förutsättningar som beställaren angav i begäran. Klienten bör kontrol lera att ETAG har skickats med begäran. |
|  |  |

## <a name="uploading-artifacts"></a>Laddar upp artefakter

Artefakter, till exempel bilder och logo typer, bör delas genom att ladda upp dem till en tillgänglig plats på webben, inklusive varje som en URI i förfrågan-begäran, som i exemplet ovan. Systemet upptäcker att dessa filer inte finns på Azure Marketplace-lagringen och laddar ned dessa filer till lagringen.  Därför kan du se att framtida GET-begäranden returnerar en URL för Azure Marketplace-tjänsten för de här filerna.

## <a name="categories-and-industries"></a>Kategorier och branscher

När du skapar ett nytt erbjudande måste du ange en kategori för ditt erbjudande i Marketplace. Om du vill kan du även ange branscher för vissa typer av erbjudanden. Baserat på erbjudande typen, anger du de kategorier/branscher som är tillämpliga för erbjudandet med hjälp av vissa nyckel värden från följande tabeller.

### <a name="azure-marketplace-categories"></a>Azure Marketplace-kategorier

Dessa kategorier och deras respektive nycklar gäller för Azure-appar, Virtual Machines, kärn Virtual Machines, behållare, behållar appar, IoT Edge moduler och SaaS-erbjudanden. Objekt i fet kursiv stil (till exempel ***analys***) är kategorier och standard text objekt (som data insikter) är under kategorier under dem. Använd de exakta nyckel värdena, utan att ändra avstånd eller Skift läge.

| Kategori | SaaS-nycklar | Azure App nycklar | Virtuell dator, behållare, behållar appar, IoT Edge modul, kärn nycklar för virtuella datorer |
| --- | --- | --- | --- |
| ***Analys*** | ***tjänstprogrammet*** | ***Analytics – Azure-Apps*** | ***analys-amp*** |
| Data insikter | data insikter | data insikter | data insikter |
| Dataanalys | data analys | data analys | data analys |
| Stordata | Big-data | bigData | Big-data |
| Förutsägelseanalys | förutsägande analys | förutsägande analys | förutsägande analys |
| Real tids-/strömnings analys | Real tids strömnings analys | Real tids strömnings analys | Real tids strömnings analys |
| Övrigt | övrigt | annan – analys | övrigt |
| ***AI + Machine Learning*** | ***ArtificialIntelligence*** | ***AI – plus-Machine-Learning*** | ***AI – plus-Machine-Learning*** |
| Robot tjänster | robot-tjänster | robot-tjänster | robot-tjänster |
| Cognitive Services | kognitiva tjänster | kognitiva tjänster | kognitiva tjänster |
| ML-tjänst | ml-tjänst | ml-tjänst | ml-tjänst |
| Automatisk ML | automatiserad ml | automatiserad ml | automatiserad ml |
| Automatisering av Business/robot process | affärs robot – process automatisering | affärs robot – process automatisering | affärs robot – process automatisering |
| Data märkning | märkning av data | märkning av data | märkning av data |
| Förberedelse av data | förberedelse av data | förberedelse av data | förberedelse av data |
| Kunskaps utvinning | kunskap – gruv drift | kunskap – gruv drift | kunskap – gruv drift |
| ML-åtgärder | ml-åtgärder | ml-åtgärder | ml-åtgärder |
| Övrigt | övrigt-AI-plus-Machine-Learning | övrigt | övrigt |
| ***Blockkedja*** | ***Blockchain*** | ***Blockchain*** | ***Blockchain*** |
| App-acceleratorer | app-acceleratorer | app-acceleratorer | app-acceleratorer |
| Redovisning med en nod | Single-Node-Ledger | Single-Node-Ledger | Single-Node-Ledger |
| Redovisning med flera noder | flera noder – redovisning | flera noder – redovisning | flera noder – redovisning |
| Verktyg | tools | tools | tools |
| Övrigt | övrigt | övrigt | övrigt |
| ***Beräkning*** | ***Compute-SaaS*** | ***Compute-Azure-Apps*** | ***beräknings*** |
| Program infrastruktur | appInfra | appInfrastructure | program infrastruktur |
| Operativsystem | klientbegäran | klientbegäran | operativ system |
| Cache | cache | cache | cache |
| Övrigt | annan – beräkning | annan – beräkning | övrigt |
| ***Containrar*** | ***artiklar*** | ***artiklar*** | ***artiklar*** |
| Behållar appar | container – appar | container – appar | container – appar |
| Behållar avbildningar | behållare – avbildningar | behållare – avbildningar | behållare – avbildningar |
| Kom igång med behållare | Get-Started-with-containers | Get-Started-with-containers | Get-Started-with-containers |
| Övrigt | övrigt | övrigt | övrigt |
| ***Databaser*** | ***databaser – SaaS*** | ***databas*** | ***samling*** |
| NoSQL-databaser | NoSQL – databaser | NoSQL – databaser | NoSQL – databaser |
| Relationsdatabaser | Relations databaser | Relations databaser | Relations databaser |
| Ledger/blockchain-databaser | Ledger-blockchain – databaser | Ledger-blockchain – databaser | Ledger-blockchain – databaser |
| Data sjöar | data-sjöar | data-sjöar | data-sjöar |
| -informationslager | data lager | data lager | data lager |
| Övrigt | andra – databaser | andra – databaser | övrigt |
| ***Utvecklarverktyg*** | ***Developer-Tools-SaaS*** | ***utvecklare – verktyg – Azure-Apps*** | ***utvecklare – verktyg*** |
| Verktyg | verktyg – utvecklare-verktyg | verktyg – utvecklare-verktyg | verktyg – utvecklare-verktyg |
| Skript | alfabet | alfabet | alfabet |
| Developer Service | devService | devService | Developer-Service |
| Övrigt | övrigt-utvecklare-verktyg | övrigt-utvecklare-verktyg | övrigt |
| ***DevOps*** | ***DevOps*** | ***DevOps*** | ***DevOps*** |
| Övrigt | övrigt | övrigt | övrigt |
| ***Identitet*** | ***Autentiseringsidentitet*** | ***Autentiseringsidentitet*** | ***Autentiseringsidentitet*** |
| Åtkomsthantering | åtkomst hantering | åtkomst hantering | åtkomst hantering |
| Övrigt | övrigt | övrigt | övrigt |
| ***Integrering*** | ***samordning*** | ***samordning*** | ***samordning*** |
| Meddelandetjänster | hantera | hantera | hantera |
| Övrigt | övrigt | övrigt | övrigt |
| ***Sakernas Internet*** | ***IoT*** | ***Sakernas Internet – Azure-Apps*** | ***Sakernas Internet*** |
| IoT Core-tjänster | E.t. | IoT-Core – tjänster | IoT-Core – tjänster |
| IoT Edge moduler | E.t. | IoT-Edge-modules | IoT-Edge-modules |
| IoT-lösningar | IoT-lösningar | IoT-lösningar | IoT-lösningar |
| Visualisering av data analys & | data analys-och-visualisering | data analys-och-visualisering | data analys-och-visualisering |
| IoT-anslutning | IoT – anslutning | IoT – anslutning | IoT – anslutning |
| Övrigt | andra – Sakernas Internet | andra – Sakernas Internet | övrigt |
| ***Hanterings verktyg för IT-&*** | ***ITandAdministration*** | ***IT-och-Management-tools-Azure-Apps*** | ***IT-och-Management-verktyg*** |
| Hanteringslösningar | hantering – lösningar | hantering – lösningar | hantering – lösningar |
| Företagsprogram | businessApplication | businessApplication | affärs program |
| Övrigt | andra – IT-Management – verktyg | andra – IT-Management – verktyg | övrigt |
| ***Övervaka & diagnostik*** | ***övervakning och diagnostik*** | ***övervakning och diagnostik*** | ***övervakning och diagnostik*** |
| Övrigt | övrigt | övrigt | övrigt |
| ***Media*** | ***mellanhand*** | ***mellanhand*** | ***mellanhand*** |
| Media Services | media-services | media-services | media-services |
| Content Protection | innehålls skydd | innehålls skydd | innehålls skydd |
| Live & strömning på begäran | Live-och-på-begäran-strömning | Live-och-på-begäran-strömning | Live-och-på-begäran-strömning |
| Övrigt | övrigt | övrigt | övrigt |
| ***Migrering*** | ***migreringsarkivet*** | ***migreringsarkivet*** | ***migreringsarkivet*** |
| Datamigrering | data – migrering | data – migrering | data – migrering |
| Övrigt | övrigt | övrigt | övrigt |
| ***Mixed Reality*** | ***Mixed-verklighet*** | ***Mixed-verklighet*** | ***Mixed-verklighet*** |
| Övrigt | övrigt | övrigt | övrigt |
| ***Nätverk*** | ***nätverk*** | ***nätverk*** | ***nätverk*** |
| Utrustnings hanterare | apparat-chefer | apparat-chefer | apparat-chefer |
| Anslutning | anslutning | anslutning | anslutning |
| Brandväggar | brand väggar | brand väggar | brand väggar |
| Lastbalanserare | belastnings utjämning | belastnings utjämning | belastnings utjämning |
| Övrigt | övrigt | övrigt | övrigt |
| ***Säkerhet*** | ***Bullet*** | ***Bullet*** | ***Bullet*** |
| Identitets & åtkomst hantering | identitets-och åtkomst hantering | identitets-och åtkomst hantering | identitets-och åtkomst hantering |
| Hotskydd | Hot-skydd | Hot-skydd | Hot-skydd |
| Information Protection | informations skydd | informations skydd | informations skydd |
| Övrigt | övrigt | övrigt | övrigt |
| ***Lagring*** | ***lagring – SaaS*** | ***lagring – Azure-Apps*** | ***lagrings*** |
| Säkerhets kopiering & återställning | säkerhetskopiering | säkerhetskopiering | säkerhets kopiering och återställning |
| Enterprise hybrid Storage | Enterprise – hybrid-lagring | Enterprise – hybrid-lagring | Enterprise – hybrid-lagring |
| Fildelning | fildelning | fildelning | fildelning |
| Data livs cykel hantering | data – livs cykel – hantering | data – livs cykel – hantering | data – livs cykel – hantering |
| Övrigt | annat – lagring | annat – lagring | övrigt |
| ***Webb*** | ***World*** | ***World*** | ***World*** |
| Bloggar & CMSs | Bloggar-och-CMSS | Bloggar-och-CMSS | Bloggar-och-CMSS |
| Start Web Apps | Starter – Web-Apps | Starter – Web-Apps | Starter – Web-Apps |
| ECommerce | eCommerce | eCommerce | eCommerce |
| Web Apps ramverk | Web-Apps – ramverk | Web-Apps – ramverk | Web-Apps – ramverk |
| Web Apps | webb-appar | webb-appar | webb-appar |
| Övrigt | övrigt | övrigt | övrigt |
||||

### <a name="microsoft-appsource-categories"></a>Microsoft AppSource kategorier

Dessa kategorier och deras respektive nycklar är tillämpliga för SaaS, PowerBI app, Dynamics 365 Business Central, Dynamics 365 för kund engagemang och Dynamics 365 för drift erbjudande typer. Objekt med fet kursiv stil (som ***analys***) är kategorier och standard text objekt (som avancerad analys) är under kategorier under dem. Använd de exakta nyckel värdena, utan att ändra avstånd eller Skift läge.

| Kategori | SaaS-nycklar | Dynamics 365 Business Central, Dynamics 365 för kund engagemang, Dynamics 365 för drift nycklar | PowerBI app-nycklar |
| --- | --- | --- | --- |
| ***Analys*** | ***tjänstprogrammet*** | ***Analys*** | ***Analys*** |
| Avancerad analys | avancerad – analys | avancerad – analys | avancerad – analys |
| Rapportering av visualiserings & | visualisering – rapportering | visualisering – rapportering | visualisering – rapportering |
| Övrigt | övrigt | annan – analys | annan – analys |
| ***AI + Machine Learning*** | ***ArtificialIntelligence*** | ***AI – plus-Machine-Learning – Dynamics*** | ***AI – plus-Machine-Learning – appsource*** |
| AI för företag | AI – för företag | AI – för företag | AI – för företag |
| Robot-appar | robot-appar | robot-appar | robot-appar |
| Övrigt | övrigt-AI-plus-Machine-Learning | övrigt-AI-plus-Machine-Learning | övrigt-AI-plus-Machine-Learning |
| ***Samarbete*** | ***Samarbete*** | ***Samarbete*** | ***samarbete*** |
| Kontakta & personer | kontakt – personer | kontakt – personer | kontakt och personer |
| Mötes hantering | Mötes hantering | Mötes hantering | Mötes hantering |
| Webbplats design & hantering | webbplats design – hantering | webbplats design – hantering | plats-design-och-hantering |
| Aktivitet & projekt hantering | uppgift – projekt hantering | uppgift – projekt hantering | aktivitet-och-projekt-hantering |
| Röst & video konferens | röst-video-konferenser | röst-video-konferenser | röst-och-video-konferenser |
| Övrigt | annat samarbete | annat samarbete | övrigt |
| ***Efterlevnad & Legal*** | ***Fastställ*** | ***Fastställ*** | ***efterlevnad-och-Legal*** |
| Skatte & granskning | skatte granskning | skatte granskning | skatt och-Audit |
| Juridisk information | Juridisk information | Juridisk information | A4 |
| Data, styrning & sekretess | data styrning – sekretess | data styrning – sekretess | data-styrning-och-sekretess |
| Hälso & säkerhet | hälsa – säkerhet | hälsa – säkerhet | hälsa och säkerhet |
| Övrigt | annan-efterlevnad – juridisk | annan-efterlevnad – juridisk | övrigt |
| ***Customer Service*** | ***CustomerService*** | ***CustomerService*** | ***kund-service*** |
| Kontakta Center | Kontakta-Center | Kontakta-Center | Kontakta-Center |
| Ansikte till ansikts tjänst | ansikts-till-ansikts-tjänst | ansikts-till-ansikts-tjänst | ansikts-till-ansikts-tjänst |
| Säkerhetskopiera Office & Employee service | tillbaka – Office – personal-service | tillbaka – Office – personal-service | Back-Office-och-Employee-service |
| Hantering av kunskaps & | kunskap – ärende hantering | kunskap – ärende hantering | Knowledge-och-Case-management |
| Socialt medium & Omnichannel-engagemang | social-media – Omnichannel – engagemang | social-media – Omnichannel – engagemang | social-media-och-Omnichannel – engagemang |
| Övrigt | annan – kund service | annan – kund service | övrigt |
| ***Ekonomi*** | ***Ekonomi*** | ***Ekonomi*** | ***räntor*** |
| Redovisning | redovisning | redovisning | redovisning |
| Tillgångshantering | till gång – hantering | till gång – hantering | till gång – hantering |
| Analys, konsolidering & rapportering | analys – konsolidering – rapportering | analys – konsolidering – rapportering | analys – konsolidering och rapportering |
| Kredit & samlingar | kredit – samlingar | kredit – samlingar | kredit-och-samlingar |
| Hantering av & riskhantering | kompatibilitet – risk hantering | kompatibilitet – risk hantering | efterlevnad-och-risk-hantering |
| Övrigt | annan – ekonomi | annan – ekonomi | övrigt |
| ***Human Resources*** | ***Personal*** | ***Personal*** | ***mänskliga resurser*** |
| Personal-förvärv | personal – förvärv | personal – förvärv | personal – förvärv |
| Hantering av personal | personal – hantering | personal – hantering | personal – hantering |
| HR-åtgärder | HR-åtgärder | HR-åtgärder | HR-åtgärder |
| Personal planering & analys | personal planering – analys | personal planering – analys | personal – planering och analys |
| Övrigt | andra – mänskliga resurser | andra – mänskliga resurser | övrigt |
| ***Sakernas Internet*** | ***IoT*** | ***Sakernas Internet – Dynamics*** | ***appsource av Sakernas Internet*** |
| & åtgärder för till gångs hantering | till gång – hantering – åtgärder | till gång – hantering – åtgärder | till gång – hantering och åtgärder |
| Anslutna produkter | anslutna – produkter | anslutna – produkter | anslutna – produkter |
| Intelligent leverans kedja | Intelligent-Supply-kedja | Intelligent-Supply-kedja | Intelligent-Supply-kedja |
| Förutsägande underhåll | förutsägelse-underhåll | förutsägelse-underhåll | förutsägelse-underhåll |
| Fjärrövervakning | fjärr styrning | fjärr styrning | fjärr styrning |
| Säkerhet & säkerhet | säkerhet – säkerhet | säkerhet – säkerhet | säkerhet och säkerhet |
| & resurser för smart infrastruktur | Smart-infrastruktur – resurser | Smart-infrastruktur – resurser | smarta-infrastruktur-och-resurser |
| Fordon & Mobility | fordon – mobilitet | fordon – mobilitet | fordon och mobilitet |
| Övrigt | andra – Sakernas Internet | andra – Sakernas Internet | övrigt |
| ***Hanterings verktyg för IT-&*** | ***ITandAdministration*** | ***ITandAdministration*** | ***IT-och-Management-verktyg*** |
| Hanteringslösningar | hantering – lösningar | hantering – lösningar | hantering – lösningar |
| Företagsprogram | businessApplication | businessApplication | affärs program |
| Övrigt | andra – IT-Management – verktyg | andra – IT-Management – verktyg | övrigt |
| ***Marknadsföring*** | ***Marknadsföring*** | ***Marknadsföring*** | ***affärs*** |
| Annons | annons | annons | annons |
| Analys | analys-marknadsföring | analys-marknadsföring | analys-marknadsföring |
| & automatisering av kampanj hantering | kampanj – hantering – automatisering | kampanj – hantering – automatisering | kampanj – hantering och automatisering |
| E-postmarknadsföring | e-post – marknadsföring | e-post – marknadsföring | e-post – marknadsföring |
| L2-händelser & resurs hantering | händelser-resurs hantering | händelser-resurs hantering | händelser-och-resurs-hantering |
| Analys av forsknings & | forsknings analys | forsknings analys | forskning och analys |
| Sociala medier | sociala medier | sociala medier | sociala medier |
| Övrigt | annan – marknadsföring | annan – marknadsföring | övrigt |
| ***Åtgärder & leverans kedja*** | ***OperationsSupplyChain*** | ***OperationsSupplyChain*** | ***drift-och-leverans-kedja*** |
| Produktions hantering för till gång & | till gång – produktion – hantering | till gång – produktion – hantering | till gång-och-produktion-hantering |
| Efterfrågeprognostisering | efter frågan – Prognosticering | efter frågan – Prognosticering | efter frågan – Prognosticering |
| Informations hantering & anslutning | informations hantering – anslutning | informations hantering – anslutning | informations hantering-och-anslutning |
| Planering, köpa & rapportering | planering-inköp – rapportering | planering-inköp – rapportering | planering – inköp och rapportering |
| Hantering av Quality &-tjänster | Quality-service – hantering | Quality-service – hantering | hantering av kvalitet och tjänst |
| Försäljning & order hantering | försäljning – order hantering | försäljning – order hantering | försäljning och beställning – hantering |
| Transport & lager hantering | transport – lager hantering | transport – lager hantering | transport-och-lager-hantering |
| Övrigt | andra åtgärder – leverans kedja | andra åtgärder – leverans kedja | övrigt |
| ***Produktivitet*** | ***Produktivitet*** | ***Produktivitet*** | ***produktivitet*** |
| & hantering av innehålls skapande | innehålls skapande – hantering | innehålls skapande – hantering | innehålls skapande och hantering |
| Språk & Översättning | språk översättning | språk översättning | språk-och-översättning |
| Dokumenthantering | dokument hantering | dokument hantering | dokument hantering |
| E-posthantering | e-posthantering | e-posthantering | e-posthantering |
| Sök & referens | Sök referens | Sök referens | Sök-och-referens |
| Övrigt | annan – produktivitet | annan – produktivitet | övrigt |
| Spelifiering | Spelifiering | Spelifiering | spelifieringsprogram |
| ***Försäljning*** | ***Försäljning*** | ***Försäljning*** | ***Försäljning*** |
| Telefonförsäljning | telefonförsäljning | telefonförsäljning | telefonförsäljning |
| Konfigurera, pris, offert (CPQ) | Konfigurera pris – offert | Konfigurera pris – offert | Konfigurera pris – offert |
| Kontraktshantering | kontrakt – hantering | kontrakt – hantering | kontrakt – hantering |
| UPPMANA | crm | crm | crm |
| E-handel | e-handel | e-handel | e-handel |
| Affärs data-anrikning | affärs data – anrikning | affärs data – anrikning | affärs data – anrikning |
| Försäljnings aktivering | försäljning – aktivering | försäljning – aktivering | försäljning – aktivering |
| Övrigt | annan – försäljning | annan – försäljning | annan – försäljning |
| ***Geoplats*** | ***geoplats*** | ***geoplats*** | ***geoplats*** |
| Maps | maps | maps | maps |
| Nyheter & väder | Nyheter-och-väder | Nyheter-och-väder | Nyheter-och-väder |
| Övrigt | annan – geolokalisering | annan – geolokalisering | annan – geolokalisering |
||||

### <a name="microsoft-appsource-industries"></a>Microsoft AppSource branscher

Dessa branscher och deras respektive nycklar är tillämpliga för SaaS, PowerBI app, Dynamics 365 Business Central, Dynamics 365 för kund engagemang och Dynamics 365 för drift erbjudande typer. Objekt i fet kursiv stil (t. ex. ***bil***) är kategorier och standard text objekt (som AutomotiveL2) är under kategorier under dem. Använd de exakta nyckel värdena, utan att ändra avstånd eller Skift läge.

| Industri | SaaS, Dynamics 365 Business Central, Dynamics 365 för kund engagemang, Dynamics 365 för drift nycklar | Nycklar för PowerBI-appar |
| --- | --- | --- |
| ***Fordon*** | ***Fordon*** | ***fordon*** |
| Fordon | AutomotiveL2 | AutomotiveL2 |
| ***Jordbruk*** | ***Jordbruk*** | ***sektorn*** |
| Annan – segmenterad | Jordbruk \_ OtherUnsegmented | annan – segmenterad |
| ***Distribution*** | ***Distribution*** | ***distribuera*** |
| Maskiner | Maskiner | maskiner |
| Paket leverans av paket & | ParcelAndPackageShipping | paket-och-paket-leverans |
| ***Education*** | ***Education*** | ***kontor*** |
| Högre utbildning | HigherEducation | högre utbildning |
| Primär & sekundär utbildning/K-12 | PrimaryAndSecondaryEducationK12 | primär-och-sekundär-utbildning |
| Bibliotek & Museums | LibrariesAndMuseums | bibliotek-och-Museums |
| ***Ekonomiska tjänster*** | ***FinancialServices*** | ***finansiella tjänster*** |
| Bank & kapital marknader | BankingAndCapitalMarkets | Bank-och-kapital-marknad |
| Försäkringsbokf | Försäkringsbokf | försäkringsbokf |
| ***Myndigheter*** | ***Myndigheter*** | ***stat*** |
| Försvars & intelligens | DefenseAndIntelligence | försvar och information |
| Allmän säkerhets & rättvisa | PublicSafetyAndJustice | offentlig – säkerhet och rättvisa |
| Civila myndigheter | CivilianGovernment | civila myndigheter |
| ***Hälsovård*** | ***HealthCareandLifeSciences*** | ***Healthcare*** |
| Hälso betalare | HealthPayor | hälso tillstånd – betalare |
| Hälso leverantör | HealthProvider | hälso tillstånd-Provider |
| Pharmaceuticals | Pharmaceuticals | Pharmaceuticals |
| ***& resurser för produktion*** | ***Tillverkningsindustrin*** | ***tillverkning och resurser*** |
| Kemisk & Agrochemical | ChemicalAndAgrochemical | kemisk-och-agrochemical |
| Diskret tillverkning | DiscreteManufacturing | diskret tillverkning |
| Energi | Energi | energy |
| ***Butiks & konsument varor*** | ***RetailandConsumerGoods*** | ***återförsäljar-och-konsument-varor*** |
| Konsument varor | ConsumerGoods | konsument varor |
| Detaljist | Detaljist | detaljist |
| ***Media & kommunikation*** | ***MediaAndCommunications*** | ***Media-och-kommunikation*** |
| Media & underhållning | MediaandEntertainment | Media-och-underhållning |
| Telekommunikation | Telekommunikation | marknaden |
| ***Professionella tjänster*** | ***ProfessionalServices*** | ***Professional – tjänster*** |
| Juridisk information | Juridisk information | A4 |
| Partner Professional-tjänster | PartnerProfessionalServices | partner – Professional – tjänster |
| ***Arkitektur & konstruktion*** | ***ArchitectureAndConstruction*** | ***arkitektur-och-konstruktion*** |
| Annan – segmenterad | ArchitectureAndConstruction \_ OtherUnsegmented | annan – segmenterad |
| ***Service & Travel*** | ***HospitalityandTravel*** | ***service-och-resa*** |
|    Hotell & fritid | HotelsAndLeisure | hotell-och-fritid |
| Res & transport | TravelAndTransportation | resor och transport |
| Restauranger & mat tjänster | RestaurantsAndFoodServices | restauranger-och-mat-tjänster |
| ***Andra branscher i offentlig sektor*** | ***OtherPublicSectorIndustries*** | ***andra – offentliga sektorn – branscher*** |
| Skogsbruk & fiske | ForestryAndFishing | skogsbruk och fiske |
| Ideella föreningar | Ideella föreningar | ideella föreningar |
| ***Fastigheter*** | ***RealEstate*** | ***Fastighets mäklare*** |
| Annan – segmenterad | RealEstate \_ OtherUnsegmented | annan – segmenterad |
|||
