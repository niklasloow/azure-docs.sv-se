---
title: Felsöka Azure Application insikter Snapshot Debugger
description: Den här artikeln innehåller fel söknings steg och information för att hjälpa utvecklare som har problem med att aktivera eller använda Application Insights Snapshot Debugger.
ms.topic: conceptual
author: cweining
ms.date: 03/07/2019
ms.reviewer: mbullwin
ms.openlocfilehash: 935e1832629827b0286a79ab8ea6d1dfbb143e1c
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/16/2020
ms.locfileid: "90707840"
---
# <a name="troubleshoot-problems-enabling-application-insights-snapshot-debugger-or-viewing-snapshots"></a><a id="troubleshooting"></a> Felsöka problem med att aktivera Application Insights Snapshot Debugger eller Visa ögonblicks bilder
Om du har aktiverat Application Insights Snapshot Debugger för ditt program, men inte ser några ögonblicks bilder för undantag, kan du använda dessa instruktioner för att felsöka. Det kan finnas många olika orsaker till att ögonblicksbilder inte genereras. Du kan köra hälso kontrollen av ögonblicks bilder för att identifiera några av de möjliga vanliga orsakerna.

## <a name="use-the-snapshot-health-check"></a>Använd ögonblicks bildens hälso kontroll
Flera vanliga problem resulterar i att den öppna fel söknings ögonblicks bilden inte visas. Om du använder en föråldrad Snapshot Collector, t. ex. nått den dagliga uppladdnings gränsen. eller så kanske ögonblicks bilden tar lång tid att ladda upp. Använd ögonblicks bildens hälso kontroll för att felsöka vanliga problem.

Det finns en länk i undantags fönstret för den vy från slut punkt till slut punkt som tar dig till hälso kontrollen för ögonblicks bilder.

![Ange hälso kontroll för ögonblicks bild](./media/snapshot-debugger/enter-snapshot-health-check.png)

Det interaktiva, chat-liknande gränssnittet söker efter vanliga problem och hjälper dig att åtgärda dem.

![Hälso kontroll](./media/snapshot-debugger/healthcheck.png)

Om detta inte löser problemet kan du läsa följande manuella fel söknings steg.

## <a name="verify-the-instrumentation-key"></a>Verifiera Instrumentation-nyckeln

Kontrol lera att du använder rätt Instrumentation-nyckel i det publicerade programmet. Instrumentation-nyckeln läses vanligt vis från ApplicationInsights.config-filen. Kontrol lera att värdet är samma som Instrumentation-nyckeln för den Application Insights resurs som du ser i portalen.

## <a name="check-ssl-client-settings-aspnet"></a><a id="SSL"></a>Kontrol lera inställningar för SSL-klient (ASP.NET)

Om du har ett ASP.NET-program som finns i Azure App Service eller i IIS på en virtuell dator kan programmet inte ansluta till Snapshot Debugger-tjänsten på grund av ett saknat SSL-säkerhetsprotokoll.
[Snapshot debugger-slutpunkten kräver TLS version 1,2](snapshot-debugger-upgrade.md?toc=/azure/azure-monitor/toc.json). Uppsättningen SSL-säkerhetsprotokoll är ett av knep som Aktiver ATS av httpRuntime targetFramework-värdet i avsnittet system. Web i web.config. Om httpRuntime targetFramework är 4.5.2 eller lägre ingår inte TLS 1,2 som standard.

> [!NOTE]
> HttpRuntime targetFramework-värdet är oberoende av mål ramverket som används när du skapar ditt program.

Du kontrollerar inställningen genom att öppna web.config-filen och leta upp avsnittet system. Web. Se till att `targetFramework` for `httpRuntime` är inställt på 4,6 eller högre.

   ```xml
   <system.web>
      ...
      <httpRuntime targetFramework="4.7.2" />
      ...
   </system.web>
   ```

> [!NOTE]
> Att ändra värdet för httpRuntime targetFramework ändrar körnings knep som tillämpas på ditt program och kan orsaka andra, diskreta beteende ändringar. Se till att testa programmet noggrant när du har gjort den här ändringen. En fullständig lista över kompatibilitetsinställningar finns i https://docs.microsoft.com/dotnet/framework/migration-guide/application-compatibility#retargeting-changes

> [!NOTE]
> Om targetFramework är 4,7 eller högre, fastställer Windows de tillgängliga protokollen. I Azure App Service är TLS 1,2 tillgängligt. Men om du använder din egen virtuella dator kan du behöva aktivera TLS 1,2 i operativ systemet.

## <a name="preview-versions-of-net-core"></a>För hands versioner av .NET Core
Om programmet använder en för hands version av .NET Core och Snapshot Debugger har Aktiver ATS genom [Application Insightss fönstret](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json) i portalen, kan Snapshot debugger starta. Följ anvisningarna på [aktivera Snapshot debugger för andra miljöer för](snapshot-debugger-vm.md?toc=/azure/azure-monitor/toc.json) att först inkludera paketet [Microsoft. ApplicationInsights. SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet med programmet, ***förutom*** att aktivera i [Application Insightss fönstret](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json).


## <a name="upgrade-to-the-latest-version-of-the-nuget-package"></a>Uppgradera till den senaste versionen av NuGet-paketet

Om Snapshot Debugger har Aktiver ATS via [Application Insightss fönstret i portalen](snapshot-debugger-appservice.md?toc=/azure/azure-monitor/toc.json)bör ditt program redan köra det senaste NuGet-paketet. Om Snapshot Debugger har Aktiver ATS genom att inkludera paketet [Microsoft. ApplicationInsights. SnapshotCollector](https://www.nuget.org/packages/Microsoft.ApplicationInsights.SnapshotCollector) NuGet, använder du Visual Studios NuGet Package Manager för att kontrol lera att du använder den senaste versionen av Microsoft. ApplicationInsights. SnapshotCollector. Viktig information finns på https://github.com/Microsoft/ApplicationInsights-Home/issues/167

## <a name="check-the-uploader-logs"></a>Kontrol lera avuppladdnings loggarna

När en ögonblicks bild har skapats skapas en MiniDump-fil (. dmp) på disk. En separat överförings process skapar den Minidump-filen och laddar upp den, tillsammans med eventuella associerade PDBs, för att Application Insights Snapshot Debugger-lagring. När Minidump har laddats upp tas den bort från disken. Loggfilerna för Inhämtnings processen sparas på disken. I en App Service-miljö kan du hitta dessa loggar i `D:\Home\LogFiles` . Använd webbplatsen för hantering av kudu för App Service för att hitta loggfilerna.

1. Öppna ditt App Service-program i Azure Portal.
2. Klicka på **Avancerade verktyg**eller Sök efter **kudu**.
3. Klicka på **gå**.
4. I list rutan **fel söknings konsol** väljer du **cmd**.
5. Klicka på **loggfiler**.

Du bör se minst en fil med ett namn som börjar med `Uploader_` eller `SnapshotUploader_` och ett `.log` fil namns tillägg. Klicka på lämplig ikon för att ladda ned loggfiler eller öppna dem i en webbläsare.
Fil namnet innehåller ett unikt suffix som identifierar App Service-instansen. Om App Service-instansen finns på fler än en dator finns det separata loggfiler för varje dator. När överföraren identifierar en ny MiniDump-fil registreras den i logg filen. Här är ett exempel på en lyckad ögonblicks bild och uppladdning:

```
SnapshotUploader.exe Information: 0 : Received Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Creating minidump from Fork request ID 139e411a23934dc0b9ea08a626db16c5 from process 6368 (Low pri)
    DateTime=2018-03-09T01:42:41.8571711Z
SnapshotUploader.exe Information: 0 : Dump placeholder file created: 139e411a23934dc0b9ea08a626db16c5.dm_
    DateTime=2018-03-09T01:42:41.8728496Z
SnapshotUploader.exe Information: 0 : Dump available 139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7525022Z
SnapshotUploader.exe Information: 0 : Successfully wrote minidump to D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Uploading D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp, 214.42 MB (uncompressed)
    DateTime=2018-03-09T01:42:45.7681360Z
SnapshotUploader.exe Information: 0 : Upload successful. Compressed size 86.56 MB
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Extracting PDB info from D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp.
    DateTime=2018-03-09T01:42:59.6184651Z
SnapshotUploader.exe Information: 0 : Matched 2 PDB(s) with local files.
    DateTime=2018-03-09T01:42:59.6809606Z
SnapshotUploader.exe Information: 0 : Stamp does not want any of our matched PDBs.
    DateTime=2018-03-09T01:42:59.8059929Z
SnapshotUploader.exe Information: 0 : Deleted D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\139e411a23934dc0b9ea08a626db16c5.dmp
    DateTime=2018-03-09T01:42:59.8530649Z
```

> [!NOTE]
> Exemplet ovan är från version 1.2.0 av paketet Microsoft. ApplicationInsights. SnapshotCollector NuGet. I tidigare versioner anropas överförings processen `MinidumpUploader.exe` och loggen är mindre detaljerad.

I det föregående exemplet är Instrumentation-nyckeln `c12a605e73c44346a984e00000000000` . Det här värdet ska matcha Instrumentation-nyckeln för ditt program.
Minidump är kopplad till en ögonblicks bild med ID: t `139e411a23934dc0b9ea08a626db16c5` . Du kan använda det här ID: t senare för att hitta den associerade undantags Telemetrin i Application Insights Analytics.

Överförings tjänsten söker efter nya PDBs om var 15: e minut. Här är ett exempel:

```
SnapshotUploader.exe Information: 0 : PDB rescan requested.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Scanning D:\home\site\wwwroot for local PDBs.
    DateTime=2018-03-09T01:47:19.4457768Z
SnapshotUploader.exe Information: 0 : Local PDB scan complete. Found 2 PDB(s).
    DateTime=2018-03-09T01:47:19.4614027Z
SnapshotUploader.exe Information: 0 : Deleted PDB scan marker : D:\local\Temp\Dumps\c12a605e73c44346a984e00000000000\6368.pdbscan
    DateTime=2018-03-09T01:47:19.4614027Z
```

För program som _inte_ finns i App Service finns överförings loggarna i samma mapp som minidumpar: `%TEMP%\Dumps\<ikey>` (där `<ikey>` är din Instrumentation-nyckel).

## <a name="troubleshooting-cloud-services"></a>Felsöka Cloud Services
För roller i Cloud Services kan den tillfälliga standardmappen vara för liten för att rymma MiniDump-filerna, vilket leder till förlorade ögonblicks bilder.
Vilket utrymme som krävs beror på den totala arbets uppsättningen för ditt program och antalet samtidiga ögonblicks bilder.
Arbets minnet för en 32-bitars ASP.NET-webbroll är vanligt vis mellan 200 MB och 500 MB.
Tillåt för minst två samtidiga ögonblicks bilder.
Om ditt program t. ex. använder 1 GB av den totala arbets mängden, bör du se till att det finns minst 2 GB disk utrymme för att lagra ögonblicks bilder.
Följ dessa steg om du vill konfigurera en moln tjänst roll med en dedikerad lokal resurs för ögonblicks bilder.

1. Lägg till en ny lokal resurs i moln tjänsten genom att redigera csdef-filen (Cloud Service definition). I följande exempel definieras en resurs `SnapshotStore` som heter med en storlek på 5 GB.
   ```xml
   <LocalResources>
     <LocalStorage name="SnapshotStore" cleanOnRoleRecycle="false" sizeInMB="5120" />
   </LocalResources>
   ```

2. Ändra din Rolls start kod för att lägga till en miljö variabel som pekar på den `SnapshotStore` lokala resursen. För arbets roller ska koden läggas till i din Rolls `OnStart` metod:
   ```csharp
   public override bool OnStart()
   {
       Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
       return base.OnStart();
   }
   ```
   För Web roles (ASP.NET) ska koden läggas till i ditt webb programs `Application_Start` metod:
   ```csharp
   using Microsoft.WindowsAzure.ServiceRuntime;
   using System;

   namespace MyWebRoleApp
   {
       public class MyMvcApplication : System.Web.HttpApplication
       {
          protected void Application_Start()
          {
             Environment.SetEnvironmentVariable("SNAPSHOTSTORE", RoleEnvironment.GetLocalResource("SnapshotStore").RootPath);
             // TODO: The rest of your application startup code
          }
       }
   }
   ```

3. Uppdatera din Rolls ApplicationInsights.config fil för att åsidosätta den tillfälliga mapplats som används av `SnapshotCollector`
   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Use the SnapshotStore local resource for snapshots -->
      <TempFolder>%SNAPSHOTSTORE%</TempFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

## <a name="overriding-the-shadow-copy-folder"></a>Åsidosätter mappen Shadow Copy

När Snapshot Collector startar görs ett försök att hitta en mapp på disken som är lämplig för att köra processen för ögonblicks bilds uppladdning. Den valda mappen kallas skugg kopia-mappen.

Snapshot Collector söker efter några välkända platser och kontrollerar att de har behörighet att kopiera binärfiler för ögonblicks bilder. Följande miljövariabler används:
- Fabric_Folder_App_Temp
- LOCALAPPDATA
- APPDATA
- STYR

Om det inte går att hitta en lämplig mapp, Snapshot Collector rapportera ett fel som säger att _det inte gick att hitta en lämplig mapp för skugg kopior._

Om kopieringen Miss lyckas, Snapshot Collector rapportera ett `ShadowCopyFailed` fel.

Om inladdningen inte kan startas, Snapshot Collector rapportera ett `UploaderCannotStartFromShadowCopy` fel. Bröd texten i meddelandet innehåller ofta `System.UnauthorizedAccessException` . Det här felet uppstår vanligt vis på grund av att programmet körs under ett konto med minskad behörighet. Kontot har behörighet att skriva till mappen Shadow Copy, men det har inte behörighet att köra kod.

Eftersom dessa fel vanligt vis inträffar under starten, kommer de vanligt vis att följas av ett `ExceptionDuringConnect` fel meddelande _om att det inte gick att överföra "uppladdning"._

Du kan undvika de här felen genom att ange mappen Shadow Copy manuellt via `ShadowCopyFolder` konfigurations alternativet. Du kan till exempel använda ApplicationInsights.config:

   ```xml
   <TelemetryProcessors>
    <Add Type="Microsoft.ApplicationInsights.SnapshotCollector.SnapshotCollectorTelemetryProcessor, Microsoft.ApplicationInsights.SnapshotCollector">
      <!-- Override the default shadow copy folder. -->
      <ShadowCopyFolder>D:\SnapshotUploader</ShadowCopyFolder>
      <!-- Other SnapshotCollector configuration options -->
    </Add>
   </TelemetryProcessors>
   ```

Eller, om du använder appsettings.jspå med ett .NET Core-program:

   ```json
   {
     "ApplicationInsights": {
       "InstrumentationKey": "<your instrumentation key>"
     },
     "SnapshotCollectorConfiguration": {
       "ShadowCopyFolder": "D:\\SnapshotUploader"
     }
   }
   ```

## <a name="use-application-insights-search-to-find-exceptions-with-snapshots"></a>Använd Application Insights Sök för att hitta undantag med ögonblicks bilder

När en ögonblicks bild skapas taggas det Utlös ande undantaget med ett ögonblicks bild-ID. Detta ögonblicks bild-ID ingår som en anpassad egenskap när telemetri för undantag rapporteras till Application Insights. Med hjälp av **Sök** i Application Insights kan du hitta all telemetri med den `ai.snapshot.id` anpassade egenskapen.

1. Bläddra till Application Insights resursen i Azure Portal.
2. Klicka på **Sök**.
3. Skriv `ai.snapshot.id` text rutan Sök och tryck på RETUR.

![Sök efter telemetri med ett ögonblicks bild-ID i portalen](./media/snapshot-debugger/search-snapshot-portal.png)

Om sökningen inte returnerar några resultat rapporterades inga ögonblicks bilder till Application Insights för ditt program under det valda tidsintervallet.

Om du vill söka efter ett särskilt ögonblicks bild-ID från överförings loggarna skriver du detta ID i sökrutan. Om du inte hittar telemetri för en ögonblicks bild som du vet har laddats upp följer du dessa steg:

1. Kontrol lera att du tittar på rätt Application Insights resurs genom att kontrol lera Instrumentation-nyckeln.

2. Med tidsintervallet i överförings loggen kan du justera tidsintervalls filtret för sökningen så att det tar detta tidsintervall.

Om du fortfarande inte ser ett undantag med det ögonblicks bilds-ID: t rapporterades inte Telemetrin till Application Insights. Den här situationen kan inträffa om ditt program kraschade efter det att det tog ögonblicks bilden, men innan det rapporterade telemetri. I det här fallet kontrollerar du App Service loggarna under `Diagnose and solve problems` för att se om det uppstod oväntade omstarter eller ohanterade undantag.

## <a name="edit-network-proxy-or-firewall-rules"></a>Redigera nätverks proxy-eller brand Väggs regler

Om ditt program ansluter till Internet via en proxy eller en brand vägg, kan du behöva redigera reglerna för att tillåta att ditt program kommunicerar med Snapshot Debugger-tjänsten. De IP-adresser som används av Snapshot Debugger ingår i Azure Monitor Service tag.
