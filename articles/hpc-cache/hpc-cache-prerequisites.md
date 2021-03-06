---
title: Krav för Azure HPC-cache
description: Krav för att använda Azure HPC cache
author: ekpgh
ms.service: hpc-cache
ms.topic: how-to
ms.date: 09/03/2020
ms.author: v-erkel
ms.openlocfilehash: 7731839d23ec49ddfee814cc6b2f6b3459372f03
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613986"
---
# <a name="prerequisites-for-azure-hpc-cache"></a>Krav för Azure HPC-cache

Innan du använder Azure Portal för att skapa en ny Azure HPC-cache kontrollerar du att din miljö uppfyller dessa krav.

## <a name="video-overviews"></a>Video översikter

Titta på dessa videor för en snabb översikt över systemets komponenter och vad de behöver för att arbeta tillsammans.

(Klicka på video bilden eller länken för att titta.)

* [Så här fungerar det](https://azure.microsoft.com/resources/videos/how-hpc-cache-works/) – förklarar hur Azure HPC-cache interagerar med lagring och klienter

  [![video miniatyr bild: Azure HPC-cache: hur det fungerar (Klicka för att besöka video sidan)](media/video-2-components.png)](https://azure.microsoft.com/resources/videos/how-hpc-cache-works/)  

* [Krav – beskriver](https://azure.microsoft.com/resources/videos/hpc-cache-prerequisites/) kraven för NAS-lagring, Azure Blob Storage, nätverks åtkomst och klient åtkomst

  [![video miniatyr bild: Azure HPC-cache: förutsättningar (Klicka för att besöka video sidan)](media/video-3-prerequisites.png)](https://azure.microsoft.com/resources/videos/hpc-cache-prerequisites/)

Läs resten av den här artikeln för vissa rekommendationer.

## <a name="azure-subscription"></a>Azure-prenumeration

En betald prenumeration rekommenderas.
<!-- 
> [!NOTE]
> The Azure HPC Cache team must add your subscription to the access list before it can be used to create a cache instance. This procedure helps ensure that each customer gets high-quality responsiveness from their caches. Fill out [this form](https://aka.ms/onboard-hpc-cache) to request access.  -->

## <a name="network-infrastructure"></a>Nätverksinfrastruktur

Två nätverksrelaterade krav bör ställas in innan du kan använda din cache:

* Ett dedikerat undernät för Azure HPC cache-instansen
* DNS-stöd så att cachen kan komma åt lagring och andra resurser

### <a name="cache-subnet"></a>Cache-undernät

Azure HPC-cachen behöver ett dedikerat undernät med följande kvaliteter:

* Under nätet måste ha minst 64 IP-adresser tillgängliga.
* Under nätet kan inte vara värd för andra virtuella datorer, även för relaterade tjänster som klient datorer.
* Om du använder flera Azure HPC cache-instanser behöver var och en av sina egna undernät.

Det bästa sättet är att skapa ett nytt undernät för varje cache. Du kan skapa ett nytt virtuellt nätverk och undernät som en del av att skapa cachen.

### <a name="dns-access"></a>DNS-åtkomst

Cachen behöver DNS för att få åtkomst till resurser utanför det virtuella nätverket. Beroende på vilka resurser du använder kan du behöva konfigurera en anpassad DNS-server och konfigurera vidarebefordran mellan servern och Azure DNS servrar:

* För att få åtkomst till Azure Blob Storage-slutpunkter och andra interna resurser behöver du den Azure-baserade DNS-servern.
* För att komma åt lokal lagring måste du konfigurera en anpassad DNS-server som kan matcha dina lagrings-värdnamn.

Om du bara behöver åtkomst till Blob Storage kan du använda standard-Azure-standardservern för cacheminnet. Men om du behöver åtkomst till andra resurser bör du skapa en anpassad DNS-server och konfigurera den så att den vidarebefordrar alla Azure-speciella matchnings begär anden till Azure DNS-servern.

En enkel DNS-server kan också användas för att belastningsutjämna klient anslutningar bland alla tillgängliga cache-monterings punkter.

Lär dig mer om Azure Virtual Networks och DNS-serverkonfigurationer i [namn matchning för resurser i virtuella Azure-nätverk](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances).

## <a name="permissions"></a>Behörigheter

Kontrol lera de här behörighets kraven innan du börjar skapa din cache.

* Cache-instansen måste kunna skapa virtuella nätverks gränssnitt (NIC). Den användare som skapar cachen måste ha tillräcklig behörighet i prenumerationen för att skapa nätverkskort.

* Om du använder Blob Storage behöver Azure HPC cache auktorisering för att komma åt ditt lagrings konto. Använd rollbaserad åtkomst kontroll (RBAC) för att ge cache åtkomst till blob-lagringen. Två roller krävs: lagrings konto deltagare och data deltagare i Storage blob.

  Följ instruktionerna i [Lägg till lagrings mål](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) för att lägga till rollerna.

## <a name="storage-infrastructure"></a>Lagrings infrastruktur

Cachen stöder export av Azure Blob-behållare eller NFS-maskinvara. Lägg till lagrings mål när du har skapat cacheminnet.

Varje lagrings typ har särskilda krav.

### <a name="blob-storage-requirements"></a>Blob Storage-krav

Om du vill använda Azure Blob Storage med ditt cacheminne behöver du ett kompatibelt lagrings konto och antingen en tom BLOB-behållare eller en behållare som är ifylld med Azure HPC cache-formaterade data enligt beskrivningen i [Flytta data till Azure Blob Storage](hpc-cache-ingest.md).

Skapa kontot innan du försöker lägga till ett lagrings mål. Du kan skapa en ny behållare när du lägger till målet.

Använd följande inställningar om du vill skapa ett kompatibelt lagrings konto:

* Prestanda: **standard**
* Konto typ: **StorageV2 (generell användning v2)**
* Replikering: **Lokalt Redundant lagring (LRS)**
* Åtkomst nivå (standard): **snabb**

Det är en bra idé att använda ett lagrings konto på samma plats som din cache.

Du måste också ge cache-programmet åtkomst till ditt Azure Storage-konto som anges i [behörigheter](#permissions)ovan. Följ proceduren i [Lägg till lagrings mål](hpc-cache-add-storage.md#add-the-access-control-roles-to-your-account) för att ge cachen de nödvändiga åtkomst rollerna. Om du inte är lagrings kontots ägare kan du låta ägaren göra detta steg.

### <a name="nfs-storage-requirements"></a>Krav för NFS-lagring
<!-- linked from configuration.md -->

Om du använder ett NFS-filsystem (till exempel ett lokalt maskinvaru-NAS-system) kontrollerar du att det uppfyller dessa krav. Du kan behöva arbeta med nätverks administratörer eller brand Väggs hanterare för ditt lagrings system (eller data Center) för att verifiera inställningarna.

> [!NOTE]
> Det gick inte att skapa lagrings mål om cachen har otillräcklig åtkomst till NFS-lagrings systemet.

Mer information finns i [Felsöka problem med NAS-konfiguration och NFS-lagring](troubleshoot-nas.md).

* **Nätverks anslutning:** Azure HPC-cachen behöver nätverks åtkomst med hög bandbredd mellan cache-undernät och NFS-systemets Data Center. [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) eller liknande åtkomst rekommenderas. Om du använder en VPN-anslutning kan du behöva konfigurera den till dessutom foga ihop TCP MSS på 1350 för att se till att stora paket inte blockeras. Läs [storleks begränsningar för VPN-paket](troubleshoot-nas.md#adjust-vpn-packet-size-restrictions) för ytterligare hjälp vid fel sökning av VPN-inställningar.

* **Port åtkomst:** Cachen behöver åtkomst till vissa TCP/UDP-portar på lagrings systemet. Olika typer av lagrings enheter har olika port krav.

  Följ den här proceduren om du vill kontrol lera inställningarna för lagrings systemet.

  * Skicka ett `rpcinfo` kommando till lagrings systemet för att kontrol lera vilka portar som behövs. Kommandot nedan visar portarna och formaterar relevanta resultat i en tabell. (Använd systemets IP-adress i stället för *<storage_IP>* term.)

    Du kan skicka det här kommandot från valfri Linux-klient som har NFS-infrastruktur installerad. Om du använder en klient i klustrets undernät kan du också kontrol lera anslutningen mellan under nätet och lagrings systemet.

    ```bash
    rpcinfo -p <storage_IP> |egrep "100000\s+4\s+tcp|100005\s+3\s+tcp|100003\s+3\s+tcp|100024\s+1\s+tcp|100021\s+4\s+tcp"| awk '{print $4 "/" $3 " " $5}'|column -t
    ```

  Se till att alla portar som returneras av ``rpcinfo`` frågan tillåter obegränsad trafik från Azure HPC-nätets undernät.

  * Om du inte kan använda `rpcinfo` kommandot kontrollerar du att dessa ofta använda portar tillåter inkommande och utgående trafik:

    | Protokoll | Port  | Tjänst  |
    |----------|-------|----------|
    | TCP/UDP  | 111   | rpcbind  |
    | TCP/UDP  | 2049  | NFS      |
    | TCP/UDP  | 4045  | nlockmgr |
    | TCP/UDP  | 4046  | monterad   |
    | TCP/UDP  | 4047  | status   |

    Vissa system använder olika port nummer för dessa tjänster. Läs mer i dokumentationen för lagrings systemet.

  * Kontrol lera brand Väggs inställningarna så att de tillåter trafik på alla de portar som krävs. Se till att kontrol lera brand väggar som används i Azure samt lokala brand väggar i ditt data Center.

* **Katalog åtkomst:** Aktivera `showmount` kommandot på lagrings systemet. Azure HPC cache använder det här kommandot för att kontrol lera att din lagrings mål konfiguration pekar på en giltig export, och också för att se till att flera monteringar inte har åtkomst till samma under kataloger (en risk för fil kollision).

  > [!NOTE]
  > Om ditt NFS Storage-System använder NetApp ONTAP 9,2-operativ system ska **du inte `showmount` Aktivera **. [Kontakta Microsofts tjänst och support](hpc-cache-support-ticket.md) om du behöver hjälp.

  Läs mer om åtkomst till katalog listor i [fel söknings artikeln](troubleshoot-nas.md#enable-export-listing)för NFS-lagrings målet.

* **Rot åtkomst** (Läs/skriv): cachen ansluter till backend-systemet som användar-ID 0. Kontrol lera inställningarna på ditt lagrings system:
  
  * Aktivera `no_root_squash` . Med det här alternativet ser du till att fjärranslutna rot användare kan komma åt filer som ägs av roten.

  * Kontrol lera export principerna för att se till att de inte innehåller begränsningar för rot åtkomst från cachens undernät.

  * Om lagrings platsen har en export som är under kataloger till en annan export, se till att cachen har rot åtkomst till det lägsta segmentet i sökvägen. Läs [rot åtkomst för katalog Sök vägar](troubleshoot-nas.md#allow-root-access-on-directory-paths) i fel söknings artikeln NFS-lagrings mål för mer information.

* NFS-backend-lagring måste vara en kompatibel maskin-/program varu plattform. Kontakta Azure HPC-teamet för mer information.

## <a name="set-up-azure-cli-access-optional"></a>Konfigurera Azure CLI-åtkomst (valfritt)

Om du vill skapa eller hantera Azure HPC-cache från Azures kommando rads gränssnitt (Azure CLI) måste du installera CLI-programvaran och HPC-cache-tillägget. Följ instruktionerna i [Konfigurera Azure CLI för Azure HPC cache](az-cli-prerequisites.md).

## <a name="next-steps"></a>Nästa steg

* [Skapa en Azure HPC cache-instans](hpc-cache-create.md) från Azure Portal
