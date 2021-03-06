---
title: Använda virtuella Azure-datorer
description: Lär dig hur du använder virtuella Azure-datorer för att spara pengar.
author: cynthn
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 07/20/2020
ms.author: cynthn
ms.reviewer: jagaveer
ms.openlocfilehash: c0b8f395dde1d94c4c1efa32a2f78707d1456d88
ms.sourcegitcommit: d39f2cd3e0b917b351046112ef1b8dc240a47a4f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88818029"
---
# <a name="use-spot-vms-in-azure"></a>Använda virtuella datorer i Azure

Med hjälp av virtuella datorer kan du dra nytta av vår outnyttjade kapacitet till betydande besparingar. Vid alla tidpunkter när Azure behöver kapaciteten tillbaka, tar Azure-infrastrukturen bort virtuella datorer. De virtuella datorerna är därför fantastiska för arbets belastningar som kan hantera avbrott som bearbetnings jobb, utvecklings-/test miljöer, stora beräknings arbets belastningar med mera.

Mängden tillgänglig kapacitet kan variera beroende på storlek, region, tid och dag. När du distribuerar virtuella datorer, allokerar Azure de virtuella datorerna om det finns tillgänglig kapacitet, men det finns inget service avtal för dessa virtuella datorer. En VM-VM ger inga garantier för hög tillgänglighet. Vid alla tidpunkter när Azure behöver kapaciteten tillbaka, tar Azure-infrastrukturen bort virtuella platser med 30 sekunders varsel. 


## <a name="eviction-policy"></a>Avlägsnandeprincip

Virtuella datorer kan avlägsnas baserat på kapacitet eller det högsta pris som du har angett. När du skapar en virtuell dator för virtuella datorer kan du ange att principen ska *avallokeras* eller *tas bort*. 

Principen *frigör* flyttar den virtuella datorn till statusen stoppad-frigjord, så att du kan distribuera den igen senare. Det finns dock ingen garanti för att allokeringen ska lyckas. De friallokerade virtuella datorerna räknas av mot kvoten och du debiteras lagrings kostnaderna för de underliggande diskarna. 

Om du vill att den virtuella datorn ska tas bort när den tas bort kan du ange vilken borttagnings princip som ska *tas bort*. De avlägsnade virtuella datorerna tas bort tillsammans med deras underliggande diskar, så du kan inte fortsätta att debiteras för lagringen. 

Du kan välja att ta emot meddelanden i virtuella datorer via [Azure schemalagda händelser](./linux/scheduled-events.md). Detta meddelar dig om dina virtuella datorer avlägsnas och du har 30 sekunder på dig att slutföra jobben och utföra avstängnings uppgifter innan avlägsnandet. 


| Alternativ | Resultat |
|--------|---------|
| Högsta pris är inställt på >= det aktuella priset. | Den virtuella datorn distribueras om kapacitet och kvot är tillgängliga. |
| Högsta pris är inställt på < det aktuella priset. | Den virtuella datorn har inte distribuerats. Du får ett fel meddelande om att max priset måste vara >= aktuellt pris. |
| Starta om en virtuell dator för att stoppa/frigöra om max priset är >= aktuellt pris | Om det finns kapacitet och kvot distribueras den virtuella datorn. |
| Starta om en virtuell dator för att stoppa/frigöra om högsta pris är < det aktuella priset | Du får ett fel meddelande om att max priset måste vara >= aktuellt pris. | 
| Priset för den virtuella datorn har blivit nu > det högsta priset. | Den virtuella datorn har avlägsnats. Du får ett 30 s-meddelande innan du börjar med den faktiska borttagningen. | 
| Efter avlägsnandet går priset för den virtuella datorn tillbaka till < det högsta priset. | Den virtuella datorn kommer inte att startas om automatiskt. Du kan starta om den virtuella datorn själv och debiteras enligt det aktuella priset. |
| Om max priset är inställt på `-1` | Den virtuella datorn kommer inte att avlägsnas av prissättnings skäl. Det högsta priset är det aktuella priset, upp till priset för virtuella datorer med standard typ. Du debiteras aldrig enligt standard priset.| 
| Ändra Max priset | Du måste frigöra den virtuella datorn för att ändra det högsta priset. Frigör den virtuella datorn, ange ett nytt max pris och uppdatera den virtuella datorn. |


## <a name="limitations"></a>Begränsningar

Följande VM-storlekar stöds inte för virtuella datorer på platsen:
 - B-serien
 - Kampanj versioner av valfri storlek (t. ex. dv2, NV, NC, H kampanj storlek)

Virtuella datorer kan distribueras till vilken region som helst, förutom Microsoft Azure Kina 21Vianet.

<a name="channel"></a>

Följande [typer av erbjudanden](https://azure.microsoft.com/support/legal/offer-details/) stöds för närvarande:

-   Enterprise-avtal
-   Betala per användning
-   Sponsrat
- För Cloud Service Provider (CSP) kontaktar du din partner


## <a name="pricing"></a>Prissättning

Priser för virtuella datorer i virtuella datorer är varierande, baserat på region och SKU. Mer information finns i prissättning för virtuella datorer för [Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/) och [Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/). 


Med varierande priser har du möjlighet att ange ett högsta pris i USD (USD) med upp till 5 decimaler. Värdet skulle till exempel `0.98765` vara ett max pris på $0,98765 USD per timme. Om du anger det högsta priset så `-1` kommer den virtuella datorn inte att avlägsnas baserat på priset. Priset för den virtuella datorn är det aktuella priset för dekor pris eller priset för en standard-VM, som någonsin är mindre, så länge det finns kapacitet och tillgänglig kvot.


##  <a name="frequently-asked-questions"></a>Vanliga frågor och svar

**F:** När den har skapats är en virtuell dator på samma sätt som vanlig standard-VM?

**A:** Ja, förutom att det inte finns något service avtal för virtuella datorer på plats och de kan avlägsnas när som helst.


**F:** Vad ska jag göra när du har avlägsnat, men behöver fortfarande kapacitet?

**A:** Vi rekommenderar att du använder virtuella standard datorer i stället för virtuella datorer för virtuella datorer om du behöver kapacitet direkt.


**F:** Hur hanteras kvoten för virtuella datorer med virtuella datorer?

**A:** Virtuella datorer med virtuella datorer kommer att ha en separat kvotmall. Kvoten för kvoten kommer att delas mellan virtuella datorer och skalnings uppsättnings instanser. Läs mer i dokumentationen om [Azure-prenumeration och tjänstbegränsningar, kvoter och krav](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).


**F:** Kan jag begära ytterligare kvot för platsen?

**A:** Ja, du kommer att kunna skicka begäran om att öka din kvot för virtuella datorer med hjälp av [standard kvot processen](https://docs.microsoft.com/azure/azure-portal/supportability/per-vm-quota-requests).


**F:** Var kan jag skicka frågor?

**A:** Du kan skicka och tagga din fråga med `azure-spot` på [Q&A](https://docs.microsoft.com/answers/topics/azure-spot.html). 

## <a name="next-steps"></a>Nästa steg
Använd [CLI](./linux/spot-cli.md), [Portal](./windows/spot-portal.md), [arm-mallen](./linux/spot-template.md)eller [PowerShell](./windows/spot-powershell.md) för att distribuera virtuella datorer.

Du kan också distribuera en [skalnings uppsättning med virtuella dator instanser](../virtual-machine-scale-sets/use-spot.md).

Om du stöter på ett fel, se [felkoder](./error-codes-spot.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
