---
title: Azure Application Insights – Azure Functions funktioner som stöds
description: Application Insights funktioner som stöds för Azure Functions
ms.topic: reference
author: TimothyMothra
ms.author: tilee
ms.date: 4/23/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 333bba2b1d3cd83457196e38b827daa78199f235
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/23/2020
ms.locfileid: "87033521"
---
# <a name="application-insights-for-azure-functions-supported-features"></a>Application Insights för Azure Functions stödda funktioner

Azure Functions erbjuder [inbyggd integrering](../../azure-functions/functions-monitoring.md) med Application Insights, som är tillgänglig via ILogger-gränssnittet. Nedan visas en lista över funktioner som stöds för närvarande. Granska Azure Functionss guide för att [komma igång](../../azure-functions/functions-monitoring.md#enable-application-insights-integration).

Mer information om functions runtime-versioner finns [här](../../azure-functions/functions-versions.md).

Mer information om kompatibla versioner av Application Insights finns i [beroenden](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Logging.ApplicationInsights/).

## <a name="supported-features"></a>Funktioner som stöds

| Azure Functions                       | V1                | V2 & V3   | 
|-----------------------------------    |---------------    |------------------ |
| | | | 
| **Automatisk insamling av**        |                 |                   |               
| &bull;Autentiseringsbegäran                     | Ja             | Ja               | 
| &bull;Undantag                   | Ja             | Ja               | 
| &bull;Prestanda räknare         | Ja             | Ja               |
| &bull;Relation                   |                   |                   |               
| &nbsp;&nbsp;&nbsp;&mdash;INKOMMANDE      |                 | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash;Service Bus|                 | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash;EventHub  |                 | Yes               | 
| &nbsp;&nbsp;&nbsp;&mdash;SQL       |                 | Yes               | 
| | | | 
| **Funktioner som stöds**                |                   |                   |               
| &bull;QuickPulse/LiveMetrics       | Ja             | Ja               | 
| &nbsp;&nbsp;&nbsp;&mdash;Säker kontroll kanal|                 | Yes               | 
| &bull;Ta                     | Ja             | Ja               | 
| &bull;Pulsslag                   |                 | Yes               | 
| | | | 
| **Korrelation**                       |                   |                   |               
| &bull;Service Bus                     |                   | Yes               | 
| &bull;EventHub                       |                   | Yes               | 
| | | | 
| **Konfigurerbar**                      |                   |                   |           
| &bull;Helt konfigurerbart.<br/>Se [Azure Functions](https://github.com/Microsoft/ApplicationInsights-aspnetcore/issues/759#issuecomment-426687852) för instruktioner.<br/>Se [ASP.net Core](https://github.com/Microsoft/ApplicationInsights-aspnetcore/wiki/Custom-Configuration) för alla alternativ.               |                   | Yes                   | 


## <a name="performance-counters"></a>Prestandaräknare

Automatisk insamling av prestanda räknare fungerar bara på Windows-datorer.


## <a name="live-metrics--secure-control-channel"></a>Live Metrics & säker kontroll kanal

De anpassade filter kriterier som du anger skickas tillbaka till komponenten Live Metrics i Application Insights SDK. Filtren kan eventuellt innehålla känslig information, till exempel customerID. Du kan göra kanalen säker med en hemlig API-nyckel. Mer information finns i [skydda kontroll kanalen](./live-stream.md#secure-the-control-channel) .

## <a name="sampling"></a>Samling

Azure Functions aktiverar sampling som standard i konfigurationen. Mer information finns i [Konfigurera sampling](../../azure-functions/functions-monitoring.md#configure-sampling).

Om projektet tar ett beroende på Application Insights SDK för att utföra manuell telemetri, kan det uppstå onormalt beteende om samplings konfigurationen skiljer sig från funktionerna i samplings konfigurationen. 

Vi rekommenderar att du använder samma konfiguration som-funktionerna. Med **Functions v2**kan du hämta samma konfiguration med hjälp av beroende inmatning i konstruktorn:

```csharp
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.Extensibility;

public class Function1 
{

    private readonly TelemetryClient telemetryClient;

    public Function1(TelemetryConfiguration configuration)
    {
        this.telemetryClient = new TelemetryClient(configuration);
    }

    [FunctionName("Function1")]
    public async Task<IActionResult> Run(
        [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, ILogger logger)
    {
        this.telemetryClient.TrackTrace("C# HTTP trigger function processed a request.");
    }
}
```
