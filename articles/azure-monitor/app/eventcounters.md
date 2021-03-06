---
title: Händelse räknare i Application Insights | Microsoft Docs
description: Övervaka system och anpassade .NET/.NET Core-EventCounters i Application Insights.
ms.topic: conceptual
ms.date: 09/20/2019
ms.custom: devx-track-csharp
ms.openlocfilehash: f8ae36545eecbbad2a6695ca979fb7da8380e8cc
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/10/2020
ms.locfileid: "89657009"
---
# <a name="eventcounters-introduction"></a>Introduktion till EventCounters

`EventCounter` är .NET/.NET Core-mekanismen för att publicera och använda räknare eller statistik. [Det här](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.Tracing/documentation/EventCounterTutorial.md) dokumentet ger en översikt över `EventCounters` och exempel på hur du publicerar och använder dem. EventCounters stöds i alla OS-plattformar – Windows, Linux och macOS. Det kan ses som en likvärdig plattform för [PerformanceCounters](/dotnet/api/system.diagnostics.performancecounter) som endast stöds i Windows-system.

Användare kan publicera alla anpassade efter `EventCounters` deras behov, men .net Core 3,0 och högre kör tid publicerar en uppsättning av dessa räknare som standard. Det här dokumentet går igenom de steg som krävs för att samla in och visa `EventCounters` (systemdefinierad eller användardefinierad) i Azure Application insikter.

## <a name="using-application-insights-to-collect-eventcounters"></a>Använda Application Insights för att samla in EventCounters

Application Insights stöder insamling `EventCounters` med sitt `EventCounterCollectionModule` , som är en del av det nyligen utgivna NuGet-paketet [Microsoft. ApplicationInsights. EventCounterCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.EventCounterCollector). `EventCounterCollectionModule` aktive ras automatiskt när du använder antingen [AspNetCore](asp-net-core.md) eller [WorkerService](worker-service.md). `EventCounterCollectionModule` samlar in räknare med en icke konfigurerbar samlings frekvens på 60 sekunder. Det finns inga särskilda behörigheter som krävs för att samla in EventCounters.

## <a name="default-counters-collected"></a>Insamlade standard räknare

För appar som körs i .NET Core 3,0 eller högre samlas följande räknare in automatiskt av SDK: n. Namnet på räknarna kommer att ha formatet "kategori | Counter ".

|Kategori | Räknare|
|---------------|-------|
|`System.Runtime` | `cpu-usage` |
|`System.Runtime` | `working-set` |
|`System.Runtime` | `gc-heap-size` |
|`System.Runtime` | `gen-0-gc-count` |
|`System.Runtime` | `gen-1-gc-count` |
|`System.Runtime` | `gen-2-gc-count` |
|`System.Runtime` | `time-in-gc` |
|`System.Runtime` | `gen-0-size` |
|`System.Runtime` | `gen-1-size` |
|`System.Runtime` | `gen-2-size` |
|`System.Runtime` | `loh-size` |
|`System.Runtime` | `alloc-rate` |
|`System.Runtime` | `assembly-count` |
|`System.Runtime` | `exception-count` |
|`System.Runtime` | `threadpool-thread-count` |
|`System.Runtime` | `monitor-lock-contention-count` |
|`System.Runtime` | `threadpool-queue-length` |
|`System.Runtime` | `threadpool-completed-items-count` |
|`System.Runtime` | `active-timer-count` |

> [!NOTE]
> Från och med 2.15.0-beta3-versionen av antingen [ASPNETCORE SDK](asp-net-core.md) eller [WorkerService SDK](worker-service.md)samlas inga räknare in som standard. Modulen är aktive rad så att användarna enkelt kan lägga till önskade räknare för att samla in dem.

## <a name="customizing-counters-to-be-collected"></a>Anpassa räknare som ska samlas in

I följande exempel visas hur du lägger till/tar bort räknare. Den här anpassningen görs i- `ConfigureServices` metoden för ditt program när Application Insights telemetri-samling har Aktiver ATS med hjälp av `AddApplicationInsightsTelemetry()` eller `AddApplicationInsightsWorkerService()` . Följande är en exempel kod från ett ASP.NET Core-program. Information om andra typer av program finns i [det här](worker-service.md#configuring-or-removing-default-telemetrymodules) dokumentet.

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;
    using Microsoft.Extensions.DependencyInjection;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        // The following code shows how to configure the module to collect
        // additional counters.
        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // This removes all default counters.
                module.Counters.Clear();

                // This adds a user defined counter "MyCounter" from EventSource named "MyEventSource"
                module.Counters.Add(new EventCounterCollectionRequest("MyEventSource", "MyCounter"));

                // This adds the system counter "gen-0-size" from "System.Runtime"
                module.Counters.Add(new EventCounterCollectionRequest("System.Runtime", "gen-0-size"));
            }
        );
    }
```

## <a name="disabling-eventcounter-collection-module"></a>Inaktiverar EventCounter-samlings modul

`EventCounterCollectionModule` kan inaktive ras med hjälp av `ApplicationInsightsServiceOptions` . Ett exempel på hur du använder ASP.NET Core SDK visas nedan.

```csharp
    using Microsoft.ApplicationInsights.AspNetCore.Extensions;
    using Microsoft.Extensions.DependencyInjection;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        var applicationInsightsServiceOptions = new ApplicationInsightsServiceOptions();
        applicationInsightsServiceOptions.EnableEventCounterCollectionModule = false;
        services.AddApplicationInsightsTelemetry(applicationInsightsServiceOptions);
    }
```

En liknande metod kan även användas för WorkerService SDK, men namn området måste ändras så som visas i exemplet nedan.

```csharp
    using Microsoft.ApplicationInsights.WorkerService;
    using Microsoft.Extensions.DependencyInjection;

    var applicationInsightsServiceOptions = new ApplicationInsightsServiceOptions();
    applicationInsightsServiceOptions.EnableEventCounterCollectionModule = false;
    services.AddApplicationInsightsTelemetryWorkerService(applicationInsightsServiceOptions);
```

## <a name="event-counters-in-metric-explorer"></a>Händelse räknare i Metric Explorer

Om du vill visa EventCounter-mått i [Metric Explorer](../platform/metrics-charts.md)väljer du Application Insights resurs och väljer loggbaserade mått som mått namn område. Sedan visas EventCounter mått under anpassad kategori.

> [!div class="mx-imgBorder"]
> ![Händelse räknare som rapporter ATS i Application Insights Metric Explorer](./media/event-counters/metrics-explorer-counter-list.png)

## <a name="event-counters-in-analytics"></a>Händelse räknare i Analytics

Du kan också söka efter och Visa rapporter för händelse räknare i [Analytics](../log-query/log-query-overview.md)i **customMetrics** -tabellen.

Kör till exempel följande fråga för att se vilka räknare som samlas in och är tillgängliga för fråga:

```Kusto
customMetrics | summarize avg(value) by name
```

> [!div class="mx-imgBorder"]
> ![Händelse räknare som rapporter ATS i Application Insights Analytics](./media/event-counters/analytics-event-counters.png)

Kör följande fråga för att hämta ett diagram över en viss räknare (till exempel: `ThreadPool Completed Work Item Count` ) under den senaste perioden.

```Kusto
customMetrics 
| where name contains "System.Runtime|ThreadPool Completed Work Item Count"
| where timestamp >= ago(1h)
| summarize  avg(value) by cloud_RoleInstance, bin(timestamp, 1m)
| render timechart
```
> [!div class="mx-imgBorder"]
> ![Chatta med en enda räknare i Application Insights](./media/event-counters/analytics-completeditems-counters.png)

Precis som andra telemetri innehåller **customMetrics** också en kolumn `cloud_RoleInstance` som anger identiteten för den värd Server instans där appen körs. Frågan ovan visar räknar värdet per instans och kan användas för att jämföra prestanda för olika Server instanser.

## <a name="alerts"></a>Aviseringar
Precis som med andra mått kan du [Ange en avisering](../platform/alerts-log.md) som varnar dig om en händelse räknare går utanför en gräns som du anger. Öppna fönstret aviseringar och klicka på Lägg till avisering.

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

### <a name="can-i-see-eventcounters-in-live-metrics"></a>Kan jag se EventCounters i Live-mått?

Live mått visar inte EventCounters från och med idag. Använd Metric Explorer eller Analytics för att se Telemetrin.

### <a name="i-have-enabled-application-insights-from-azure-web-app-portal-but-i-cant-see-eventcounters"></a>Jag har aktiverat Application Insights från Azure Web App-portalen. Men jag kan inte se EventCounters.?

 [Application Insights-tillägget](./azure-web-apps.md) för ASP.net Core har ännu inte stöd för den här funktionen. Det här dokumentet kommer att uppdateras när den här funktionen stöds.

## <a name="next-steps"></a><a name="next"></a>Nästa steg

* [Beroende spårning](./asp-net-dependencies.md)

