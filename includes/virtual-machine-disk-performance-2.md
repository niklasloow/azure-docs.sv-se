---
title: inkludera fil
description: inkludera fil
services: virtual-machines
author: albecker1
ms.service: virtual-machines
ms.topic: include
ms.date: 07/07/2020
ms.author: albecker1
ms.custom: include file
ms.openlocfilehash: 8882625d28871135223dd30e3fd96a385a13e8fe
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91376829"
---
![Dokumentation om Dsv3](media/vm-disk-performance/dsv3-documentation.jpg)

Max gränsen för ej **cachelagrade** diskar är den maximala lagrings gränsen som den virtuella datorn kan hantera. Max gränsen för **cachelagrade** lagrings data är en separat gräns när du aktiverar cachelagring av värden. Du kan aktivera cachelagring av värden när du skapar en virtuell dator och kopplar diskar. Du kan också ändra för att aktivera och inaktivera cachelagrade diskar på en befintlig virtuell dator:

![Cachelagring av värden](media/vm-disk-performance/host-caching.jpg)

Cachelagringen av värden kan justeras så att den överensstämmer med dina arbets belastnings krav för varje disk. Du kan ställa in att värd-cachelagringen ska vara skrivskyddad för arbets belastningar som bara har Läs åtgärder och Läs-och skriv åtgärder för arbets belastningar som utför balans mellan Läs-och skriv åtgärder. Om din arbets belastning inte följer något av dessa mönster kan du tyvärr inte använda cachelagring av värden. 

Nu ska vi fortsätta med ett exempel med vår Standard_D8s_v3 virtuella dator. Förutom den här gången aktiverar vi cachelagring av värden på diskarna och nu är den virtuella datorns IOPS-gräns 16 000 IOPS. Anslutna till den virtuella datorn är tre underliggande P30-diskar som kan hantera 5 000 IOPS.

Konfigurera:
- Standard_D8s_v3 
    - Cachelagrad IOPS: 16 000
    - Ej cachelagrad IOPS: 12 800
- P30 OS-disk 
    - IOPS: 5 000
    - Cachelagring av värdar är aktiverat 
- 2 P30 data diskar
    - IOPS: 5 000
    - Cachelagring av värdar är aktiverat

![Exempel på värd-cachelagring](media/vm-disk-performance/host-caching-example-without-remote.jpg)

Nu gör programmet som använder den här Standard_D8s_v3 virtuella datorn med cachelagring aktiverat en begäran om 15 000 IOPS. Dessa förfrågningar delas upp som 5 000 IOPS till varje underliggande disk och ingen prestanda capping uppstår.

## <a name="combined-uncached-and-cached-limits"></a>Kombinerade gränser som inte är cachelagrade och cachelagrade

Gränsen för cachelagring av en virtuell dator skiljer sig från deras cache-gränser. Det innebär att du kan aktivera cachelagring av värdar på diskar som är anslutna till en virtuell dator samtidigt som du inte aktiverar cachelagring av värden på andra diskar så att dina virtuella datorer kan få en total lagrings-IO för den cachelagrade gränsen plus den ej cachelagrade gränsen. Vi går igenom ett exempel på detta för att hjälpa stärka hur dessa gränser fungerar tillsammans och vi fortsätter med konfigurationen för den Standard_D8s_v3 virtuella datorn och Premium-diskar som är ansluten.

Konfigurera:
- Standard_D8s_v3 
    - Cachelagrad IOPS: 16 000
    - Ej cachelagrad IOPS: 12 800
- P30 OS-disk 
    - IOPS: 5 000
    - Cachelagring av värdar är aktiverat 
- 2 P30 data diskar X 2
    - IOPS: 5 000
    - Cachelagring av värdar är aktiverat
- 2 P30 data diskar X 2
    - IOPS: 5 000
    - Värd-cachelagring är inaktiverat

![Exempel på värd-cachelagring med Fjärrlagring](media/vm-disk-performance/host-caching-example-with-remote.jpg)

Nu är programmet som körs på Standard_D8s_v3 virtuell dator med en begäran om 25 000 IOPS. Denna begäran är uppdelad som 5 000 IOPS till varje underliggande disk som är ansluten där 3 av diskarna använder cachelagring av värden och 2 av diskarna. Eftersom 3 som använder cachelagring av värden ligger inom de cachelagrade gränserna för 16 000 slutförs dessa förfrågningar och inget capping för lagrings prestanda inträffar. Eftersom de två diskarna som inte använder cachelagring av värden ligger inom de ej cachelagrade gränserna för 12 800 slutförs även dessa förfrågningar och ingen capping uppstår.

## <a name="metrics-for-disk-performance"></a>Mått för disk prestanda
Vi har mått på Azure som ger insikt om hur dina virtuella datorer och diskar presterar. De här måtten kan visas visuellt via Azure Portal eller så kan de hämtas via ett API-anrop. Måtten beräknas med en minuters intervall. Följande mått är tillgängliga för att få insikt om VM och disk-i/o och data flödes prestanda:
- **OS-diskens** ködjup – antalet aktuella utestående IO-begäranden som väntar på att läsas från eller skrivas till OS-disken.
- **Lästa byte i OS-disk/SEK** – antalet byte som läses i en sekund från operativ system disken.
- **Läs åtgärder för operativ system disk/SEK** – antalet ingångs åtgärder som läses i en sekund från OS-disken.
- **Skrivna byte på OS-disk/SEK** – antalet byte som skrivs i en sekund från operativ system disken.
- **Skriv åtgärder för operativ system disk/SEK** – antalet utgående åtgärder som skrivs i en sekund från OS-disken.
- **Data disk Queue djup** – antalet aktuella utestående IO-begäranden som väntar på att läsas från eller skrivs till data diskarna.
- **Lästa byte på datadisk/SEK** – antalet byte som läses in en sekund från data diskarna.
- **Läs åtgärder för data disk/SEK** – antalet indata-åtgärder som läses i en sekund från en eller flera data diskar.
- **Data disk-skrivna byte/s** – antalet byte som skrivs i en sekund från data diskarna.
- **Skriv åtgärder för data disk/SEK** – antalet utgående åtgärder som skrivs i en sekund från en eller flera data diskar.
- **Disk-lästa byte/s** – antalet totalt antal byte som läses i en sekund från alla diskar som är kopplade till en virtuell dator.
- **Disk-Läs åtgärder/SEK** – antalet ingångs åtgärder som läses i en sekund från alla diskar som är kopplade till en virtuell dator.
- **Disk-skrivna byte/s** – antalet byte som skrivs i en sekund från alla diskar som är kopplade till en virtuell dator.
- **Disk skrivnings åtgärder/SEK** – antalet utgående åtgärder som skrivs i en sekund från alla diskar som är kopplade till en virtuell dator.

## <a name="storage-io-utilization-metrics"></a>Mått för lagring i i/o-användning
Mått som hjälper till att diagnostisera disk-i/o-capping:
- **Data disk IOPS förbrukad procent** – den procent andel som beräknas av den data disk IOPS som slutförts över den etablerade data disken IOPS. Om den här mängden är på 100% blir ditt program i/o-gräns från data diskens IOPS-gräns.
- **Data disk bandbredd förbrukad procent** – den procent andel som beräknas av data disk data flödet som slutförts via data flödet för den allokerade datadisken. Om den här mängden är på 100% blir ditt program i/o-gräns från data diskens bandbredds gräns.
- **Förbrukad procent** andel av OS-diskar – den procent andel som beräknats av OS-disken IOPS som har slutförts via den etablerade OS-disken IOPS. Om den här mängden är på 100% kommer ditt program att köras vara i/o-gräns från din OS-disks IOPS-gräns.
- **Förbrukad procent** andel av operativ system diskar – den procent andel som beräknats av OS-dataflödet som slutförts via det ALLOKERAde OS-dataflöde Om den här mängden är på 100% kommer ditt program att köras vara i/o-gräns från din OS-disks bandbredds gräns.

Mått som hjälper till att diagnostisera VM-IO-capping:
- **VM cachelagrad IOPS förbrukad procent** – den procent andel som beräknats av den totala IOPS som slutförts via den maximala IOPS-gränsen för virtuella datorer. Om den här mängden är på 100% kommer ditt program att köras vara i/o-gräns från den virtuella datorns cachelagrade IOPs-gräns.
- **VM cachelagrad bandbredd i procent** – den procent andel som beräknas av det totala disk data flödet som slutförts via det maximala data flödet för cachelagrade virtuella datorer. Om den här mängden är på 100% blir ditt program i/o-gräns från den virtuella datorns cachelagrade bandbredds gräns.
- **VM Uncached IOPS förbrukad procent** – den procent andel som beräknats av den totala IOPS på en virtuell dator som har slutförts via den maximala IOPS-gränsen på den virtuella datorn. Om den här mängden är på 100% kommer ditt program att köras vara i/o-gräns från den virtuella datorns ej cachelagrade IOPs-gräns.
- **Virtuell dator i cacheminnet för förbrukad bandbredd** i procent – den procent andel som beräknas av den totala disk data flödet på en virtuell dator som har slutförts via det högsta allokerade virtuella dator data flödet. Om den här mängden är på 100% kommer ditt program att köras vara i/o-gräns från den virtuella datorns ej cachelagrade bandbredds gräns.