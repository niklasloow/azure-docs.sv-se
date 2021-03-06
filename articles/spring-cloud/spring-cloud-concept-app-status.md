---
title: Förstå appstatus i Azure Spring Cloud
description: Lär dig app status-kategorierna i Azure våren Cloud
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: ec87c5523d6f608363ca615037f20365705609df
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90892855"
---
# <a name="understanding-app-status-in-azure-spring-cloud"></a>Förstå appstatus i Azure Spring Cloud

**Den här artikeln gäller för:** ✔️ Java ✔️ C #

Azure våren Cloud-gränssnittet ger information om statusen för program som körs.  Det finns ett **Apps** -alternativ för varje resurs grupp i en prenumeration som visar allmänna status för program typer.  För varje program typ visas **program instanser**.

## <a name="apps-status"></a>Status för appar
Om du vill visa allmän status för en program typ väljer du **appar** i det vänstra navigerings fönstret i en resurs grupp. Resultatet visar statusen för den distribuerade appen:

* **Etablerings status** visar distributionens etablerings status
* **Kör instans** visar hur många App-instanser som körs/hur många App-instanser som önskas. Om appen ska stoppas visas den här kolumnen *stoppad*.
* **Registrerad instans** visar hur många App-instanser som är registrerade på Eureka/hur många App-instanser som önskas. Om appen ska stoppas visas den här kolumnen *stoppad*.


 ![Status för appar](media/spring-cloud-concept-app-status/apps-ui-status.png)

**Distributions statusen rapporteras som ett av följande värden:**

| Enum | Definition |
|:--:|:----------------:|
| Körs | Distributionen ska köras. |
| Stoppad | Distributionen ska stoppas. |

**Etablerings statusen är bara tillgänglig från CLI.  Den rapporteras som ett av följande värden:**

| Enum | Definition |
|:--:|:----------------:|
| Skapar | Resursen skapas. |
| Uppdatera | Resursen uppdateras. |
| Lyckades | Har tillhandahållit resurser och distribuerat binärfilen. |
| Misslyckades | Det gick inte att uppnå det *slutförda* målet. |
| Tas bort | Resursen tas bort. Detta förhindrar åtgärden och resursen är inte tillgänglig i denna status. |

## <a name="app-instances-status"></a>Status för App-instanser

Om du vill visa status för en angiven instans av en distribuerad app klickar du på **namnet** på appen i användar gränssnittet för **appar** . Resultaten visas:
* **Status**: oavsett om instansen körs eller om dess tillstånd
* **DiscoveryStatus**: app-instansens registrerade status i Eureka-servern

 ![Status för App-instanser](media/spring-cloud-concept-app-status/apps-ui-instance-status.png)

**Instans statusen rapporteras som ett av följande värden:**

| Enum | Definition |
|:--:|:----------------:|
| Startar | Binärfilen har distribuerats till den aktuella instansen. Instansen som startar jar-filen kan Miss lyckas eftersom jar inte kan köras korrekt. |
| Körs | Instansen fungerar. |
| Misslyckades | App-instansen kunde inte starta användarens binärfil efter flera omförsök. |
| Avslutande | App-instansen stängs av. |

**Identifierings statusen för instansen rapporteras som ett av följande värden:**

| Enum | Definition |
|:--:|:----------------:|
| Ange | App-instansen är registrerad på Eureka och redo att ta emot trafik |
| OUT_OF_SERVICE | App-instansen registreras på Eureka och kan ta emot trafik. men stänger av trafik för avsiktligt. |
| NED | App-instansen har inte registrerats på Eureka eller är registrerad men kan inte ta emot trafik. |


## <a name="see-also"></a>Se även
* [Förbered ett våren-eller Steeltoe-program för distribution i Azure våren Cloud](spring-cloud-tutorial-prepare-app-deployment.md)