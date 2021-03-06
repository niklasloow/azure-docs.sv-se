---
title: Optimera prestanda för virtuella datorer i Azure Lsv2-serien – lagring
description: Lär dig hur du optimerar prestanda för din lösning på virtuella datorer i Lsv2-serien med ett Linux-exempel.
services: virtual-machines-linux
author: laurenhughes
ms.service: virtual-machines-linux
ms-subservice: sizes
ms.topic: conceptual
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/05/2019
ms.author: joelpell
ms.openlocfilehash: 650164556223a73a722bc91ecb31491ee98cb8a5
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91307109"
---
# <a name="optimize-performance-on-the-lsv2-series-linux-virtual-machines"></a>Optimera prestanda för virtuella Linux-datorer i Lsv2-serien

Virtuella datorer i Lsv2-serien har stöd för en mängd arbets belastningar som kräver höga I/O och data flöden i lokal lagring i en mängd olika program och branscher.  Lsv2-serien är idealisk för Big data, SQL, NoSQL-databaser, data lager hantering och stora transaktions databaser, inklusive Cassandra, MongoDB, Cloudera och Redis.

Utformningen av Lsv2-serien Virtual Machines (VM) maximerar AMD EPYC™ 7551-processorn för att ge bästa möjliga prestanda mellan processor, minne, NVMe-enheter och de virtuella datorerna. Att arbeta med partner i Linux är flera versioner tillgängliga Azure Marketplace som är optimerade för Lsv2-serie prestanda och som för närvarande omfattar:

- Ubuntu 18.04
- Ubuntu 16.04
- RHEL 8,0
- Debian 9
- Debian 10

Den här artikeln innehåller tips och förslag på hur du kan se till att dina arbets belastningar och program uppnår högsta möjliga prestanda för de virtuella datorerna. Informationen på den här sidan uppdateras kontinuerligt eftersom mer Lsv2 optimerade avbildningar läggs till i Azure Marketplace.

## <a name="amd-eypc-chipset-architecture"></a>Arkitektur för AMD EYPC™ chipset

Virtuella datorer i Lsv2-serien använder AMD-EYPC™ Server processorer baserat på Zen mikroarkitektur. AMD utvecklade Oändlighets infrastruktur (om) för EYPC™ som skalbara Interconnect för den NUMA-modell som kan användas för on-die-, paket-och multi-Pack-kommunikation. Jämfört med QPI (Quick-Path Interconnect) och UPI (Ultra-Path Interconnect) som används på Intel moderna monolitisk-Die-processorer kan AMD: s många-NUMA-arkitektur ha både prestanda-och utmaningar. Den faktiska effekten av minnes bandbredd och latens begränsningar kan variera beroende på vilken typ av arbets belastningar som körs.

## <a name="tips-to-maximize-performance"></a>Tips för att maximera prestanda

* Observera att accelererat nätverk är **inaktiverat** som standard om du överför en anpassad Linux-gäst för din arbets belastning. Om du vill aktivera accelererat nätverk aktiverar du det när du skapar den virtuella datorn för bästa prestanda.

* Maskin varan som ger de virtuella datorerna i Lsv2-serien använder NVMe-enheter med åtta I/O-ködjup (QP) s. Varje NVMe-enhet I/O-kö är faktiskt ett par: en sändnings kö och en slut för ande kö. NVMe-drivrutinen har kon figurer ATS för att optimera användningen av dessa åtta I/O-frågor per sekund genom att distribuera i/O i ett schema med resursallokering (Round Robin). För att få högsta prestanda kan du köra åtta jobb per enhet för att matcha.

* Undvik att blanda NVMe-administratörs kommandon (t. ex. en SMART informations fråga för NVMe, osv.) med kommandona I/O för NVMe under aktiva arbets belastningar. Lsv2 NVMe-enheter backas upp av Hyper-V NVMe Direct-teknik, som växlar till "långsamt läge" när några NVMe-administratörs kommandon väntar. Lsv2-användare kan se en dramatisk prestanda minskning i i/O-prestanda i NVMe om det händer.

* Lsv2-användare bör inte förlita sig på enhetens NUMA-information (all 0) som rapporteras från den virtuella datorn för data enheter för att bestämma NUMA-tillhörighet för sina appar. Det rekommenderade sättet för bättre prestanda är att sprida arbets belastningar mellan CPU: er om det är möjligt.

* Det maximala ködjup som stöds per I/O-ködjup för Lsv2 VM NVMe-enhet är 1024 (vs. Amazon i3 KÖDJUP 32 Limit). Lsv2-användare bör begränsa sina (syntetiska) arbets belastningar till ködjup 1024 eller lägre för att undvika att utlösa kön med fullständiga villkor, vilket kan minska prestandan.

## <a name="utilizing-local-nvme-storage"></a>Använd lokal NVMe-lagring

Den lokala lagringen på 1,92 TB NVMe-disken på alla virtuella Lsv2-datorer är tillfällig. Vid en lyckad standard omstart av den virtuella datorn kommer data på den lokala NVMe-disken att bevaras. Data bevaras inte i NVMe om den virtuella datorn distribueras om, avallokeras eller tas bort. Data bevaras inte om ett annat problem gör att den virtuella datorn eller den maskin vara som körs på inte är felfri. När detta händer raderas alla data på den gamla värden på ett säkert sätt.

Det kan också finnas fall då den virtuella datorn måste flyttas till en annan värddator, till exempel under en planerad underhålls åtgärd. Planerade underhålls åtgärder och vissa maskin varu problem kan förväntas med [schemalagda händelser](scheduled-events.md). Schemalagda händelser bör användas för att uppdatera för förväntade underhålls-och återställnings åtgärder.

Om en planerad underhålls händelse kräver att den virtuella datorn återskapas på en ny värd med tomma lokala diskar, måste data synkroniseras igen (återigen, med data på den gamla värden på ett säkert sätt raderas). Detta beror på att virtuella datorer i Lsv2-serien inte har stöd för Direktmigrering på den lokala NVMe-disken.

Det finns två lägen för planerat underhåll.

### <a name="standard-vm-customer-controlled-maintenance"></a>Standard kund styrt underhåll av virtuell dator

- Den virtuella datorn flyttas till en uppdaterad värd under en 30-dagars period.
- Lsv2 lokala lagrings data kan gå förlorade, så säkerhets kopiering av data före händelsen rekommenderas.

### <a name="automatic-maintenance"></a>Automatiskt underhåll

- Inträffar om kunden inte utför kundstyrt underhåll, eller i händelse av nöd procedurer som ett värdepapper som är noll dagar.
- Avsedd för att bevara kund information, men det finns en liten risk för en VM-frysning eller omstart.
- Lsv2 lokala lagrings data kan gå förlorade, så säkerhets kopiering av data före händelsen rekommenderas.

För alla kommande tjänste händelser använder du den kontrollerade underhålls processen för att välja en tid som är mest praktisk för uppdateringen. Före händelsen kan du säkerhetskopiera dina data i Premium Storage. När underhålls händelsen har slutförts kan du returnera dina data till den uppdaterade Lsv2 VMS lokalt NVMe-lagring.

Scenarier som upprätthåller data på lokala NVMe-diskar är:

- Den virtuella datorn körs och är felfri.
- Den virtuella datorn har startats om på plats (av dig eller Azure).
- Den virtuella datorn har pausats (Stoppad utan att ta bort tilldelningen).
- Merparten av de planerade underhålls åtgärderna för underhåll.

Scenarier som skyddar kunderna på ett säkert sätt är:

- Den virtuella datorn distribueras om, stoppas (avallokeras) eller tas bort (av dig).
- Den virtuella datorn blir ohälsosam och måste serva till en annan nod på grund av ett maskin varu problem.
- Ett litet antal planerade underhålls åtgärder som kräver att den virtuella datorn allokeras om till en annan värd för underhåll.

Mer information om alternativ för att säkerhetskopiera data i lokal lagring finns i [säkerhets kopiering och haveri beredskap för Azure IaaS-diskar](../backup-and-disaster-recovery-for-azure-iaas-disks.md).

## <a name="frequently-asked-questions"></a>Vanliga frågor och svar

* **Hur gör jag för att du börja distribuera virtuella datorer i Lsv2-serien?**  
   På samma sätt som andra virtuella datorer kan du använda [portalen](quick-create-portal.md), [Azure CLI](quick-create-cli.md)eller [PowerShell](quick-create-powershell.md) för att skapa en virtuell dator.

* **Kommer en enskild NVMe-disk orsaka att alla virtuella datorer på värden Miss lyckas?**  
   Om ett diskfel upptäcks på den fjärrnoden, är maskin varan i ett felaktigt tillstånd. När detta inträffar är alla virtuella datorer i noden automatiskt avallokerade och flyttas till en felfri nod. För virtuella datorer i Lsv2-serien innebär detta att kundens data på den misslyckade noden också raderas på ett säkert sätt och att de måste återskapas av kunden på den nya noden. Innan Direktmigrering blir tillgängligt på Lsv2, kommer data på den felaktiga noden att proaktivt flyttas med de virtuella datorerna när de överförs till en annan nod.

* **Behöver jag göra några ändringar i rq_affinity för prestanda?**  
   Inställningen rq_affinity är en mindre justering när du använder absoluta maximala in-/utdata-åtgärder per sekund (IOPS). När allt annat fungerar bra kan du försöka ange rq_affinity till 0 för att se om det är en skillnad.

* **Måste jag ändra blk_mq inställningarna?**  
   RHEL/CentOS 7. x använder automatiskt BLK-MQ för NVMe-enheter. Inga konfigurations ändringar eller inställningar krävs. Inställningen scsi_mod. use_blk_mq är endast för SCSI och användes vid för hands versionen av Lsv2 eftersom NVMe-enheterna är synliga i de virtuella gäst datorerna som SCSI-enheter. För närvarande visas NVMe-enheterna som NVMe-enheter, så inställningen för SCSI-BLK är irrelevant.

* **Måste jag ändra "FIO"?**  
   Om du vill få högsta IOPS med ett prestanda Mät verktyg som "FIO" i L64v2 och L80v2 VM-storlekar, anger du "rq_affinity" till 0 på varje NVMe-enhet.  Den här kommando raden kommer till exempel att ange "rq_affinity" till noll för alla 10 NVMe-enheter i en virtuell L80v2-dator:

   ```console
   for i in `seq 0 9`; do echo 0 >/sys/block/nvme${i}n1/queue/rq_affinity; done
   ```

   Observera också att bästa prestanda erhålls när I/O görs direkt till var och en av RAW NVMe-enheter utan partitionering, inga fil system, ingen RAID 0-konfiguration osv. Innan du startar en testsession ska du se till att konfigurationen är i ett känt tillstånd för att rensa och rensa genom att köra `blkdiscard` på varje NVMe-enhet.
   
## <a name="next-steps"></a>Nästa steg

* Se specifikationer för alla [virtuella datorer som är optimerade för lagrings prestanda](../sizes-storage.md) på Azure
