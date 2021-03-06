---
title: Översikt över Azure HPC cache
description: Beskriver Azure HPC cache, en lösning för fil åtkomst Accelerator för data behandling med höga prestanda
author: ekpgh
ms.service: hpc-cache
ms.topic: overview
ms.date: 09/03/2020
ms.author: v-erkel
ms.custom: references_regions
ms.openlocfilehash: 0c898b0d5a58654547cb6205ee0c845c9cfa4aa9
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89611748"
---
# <a name="what-is-azure-hpc-cache"></a>Vad är Azure HPC Cache?

Azure HPC cache ger åtkomst till dina data för HPC-uppgifter (data behandling med höga prestanda). Genom att cachelagra filer i Azure ger Azure HPC-cache skalbarhet för molnbaserad data behandling till ditt befintliga arbets flöde. Den här tjänsten kan användas även för arbets flöden där dina data lagras i WAN-länkar, t. ex. i din lokala NAS-miljö (Data Center Network-Attached Storage).

Azure HPC-cache är enkelt att starta och övervaka från Azure Portal. Befintlig NFS-lagring eller nya BLOB-behållare kan bli en del av dess sammanlagda namnrymd, vilket gör klient åtkomsten enkel även om du ändrar Server dels lagrings målet.

## <a name="overview-video"></a>Översiktsvideo

[![video miniatyr: översikt över Azure HPC-cache – Klicka för att besöka video Sidan](media/video-1-overview.png)](https://azure.microsoft.com/resources/videos/hpc-cache-overview/)

Klicka på bilden ovan om du vill se en [kort översikt över Azure HPC-cache](https://azure.microsoft.com/resources/videos/hpc-cache-overview/).

## <a name="use-cases"></a>Användningsfall

Azure HPC cache förbättrar produktiviteten bäst för arbets flöden som dessa:

* Läs – tungt arbets flöde för fil åtkomst
* Data som lagras i NFS-tillgängligt lagrings utrymme, Azure-Blob eller båda
* Beräknings grupper med upp till 75 000 CPU-kärnor

Azure HPC-cache kan läggas till i en mängd olika arbets flöden i flera branscher. Alla system där ett stort antal datorer behöver få åtkomst till en uppsättning filer i skala och med låg latens drar nytta av den här tjänsten. I avsnitten nedan ges vissa exempel.

### <a name="visual-effects-vfx-rendering"></a>Rendering av visuella effekter (VFX)

I media och underhållning kan Azure HPC-cache påskynda data åtkomsten för tids kritiska åter givnings projekt. VFX rendering-arbetsflöden kräver ofta sista minuters bearbetning med ett stort antal datornoder. Data för de här arbets flödena finns vanligt vis i en lokal NAS-miljö. Azure HPC cache kan cachelagra fildata i molnet för att minska svars tiden och förbättra flexibiliteten för rendering på begäran.

### <a name="life-sciences"></a>Biovetenskap

Många arbets flöden i Life Science kan dra nytta av skalbar cachelagring av filer.

Ett forsknings institut som vill hamna i sina genomiks analys arbets flöden i Azure kan enkelt byta till dem med hjälp av Azure HPC-cache. Eftersom cachen tillhandahåller POSIX-filåtkomst behövs inga ändringar på klient sidan för att köra befintliga klient arbets flöden i molnet.

Azure HPC cache kan också användas för att förbättra effektiviteten i uppgifter som sekundär analys, farmakologisk simulering eller AI-driven bild analys.

### <a name="financial-services-analytics"></a>Analyser för ekonomiska tjänster

En Azure HPC cache-distribution kan hjälpa dig att påskynda kvantitativa analys beräkningar, riskhanterings arbets belastningar och Monte Carlo-simuleringar för att ge finansiella tjänste företag bättre insikt i att fatta strategiska beslut.

## <a name="region-availability"></a>Regional tillgänglighet

Besök sidan [Azures globala infrastruktur produkter efter region](https://azure.microsoft.com/global-infrastructure/services/?products=hpc-cache) för att lära dig var Azure HPC cache är tillgängligt.

[Funktionen Kundhanterade nycklar](customer-keys.md) stöds bara i följande Azure-regioner:

* East US
* USA, södra centrala
* USA, västra 2
* Europa, västra
* US Gov, Arizona
* US Gov, Virginia

<!-- ## Service availability

You must request access for each subscription you will use with Azure HPC Cache. This restriction helps ensure service quality.

Request access by filling out [this form](https://aka.ms/onboard-hpc-cache). After your subscription is added to the access list, you can create caches. -->

## <a name="next-steps"></a>Nästa steg

* Läs mer om funktionerna i [produkt sidan för Azure HPC cache](https://azure.microsoft.com/services/hpc-cache)
* Läs om produkt [krav](hpc-cache-prerequisites.md)
* [Skapa en Azure HPC-cache](hpc-cache-create.md) från Azure Portal
