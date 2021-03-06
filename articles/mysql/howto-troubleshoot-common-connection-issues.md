---
title: Felsöka anslutnings problem – Azure Database for MySQL
description: Lär dig hur du felsöker anslutnings problem till Azure Database for MySQL, inklusive tillfälliga fel som kräver nya försök, brand Väggs problem och avbrott.
keywords: MySQL-anslutning, anslutnings sträng, anslutnings problem, tillfälligt fel, anslutnings fel
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: troubleshooting
ms.date: 3/18/2020
ms.openlocfilehash: e4afcb8756f64ab9b66044a1bf1304427330e365
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82100897"
---
# <a name="troubleshoot-connection-issues-to-azure-database-for-mysql"></a>Felsöka anslutningsproblem till Azure Database for MySQL

Anslutnings problem kan orsakas av olika saker, inklusive:

* Brandväggsinställningar
* Anslutnings-timeout
* Felaktig inloggnings information
* Max gränsen har nåtts för vissa Azure Database for MySQL resurser
* Problem med tjänstens infrastruktur
* Underhåll utförs i tjänsten
* Compute-allokeringen av servern ändras genom att skala antalet virtuella kärnor eller flytta till en annan tjänst nivå

I allmänhet kan anslutnings problem till Azure Database for MySQL klassificeras på följande sätt:

* Tillfälliga fel (kort livs längd eller intermittent)
* Beständiga eller icke-tillfälliga fel (fel som upprepas regelbundet)

## <a name="troubleshoot-transient-errors"></a>Felsök tillfälliga fel

Tillfälliga fel uppstår när underhåll utförs, ett fel uppstår i systemet med maskin vara eller program vara, eller så ändrar du virtuella kärnor eller tjänst nivån på servern. Tjänsten Azure Database for MySQL har inbyggd hög tillgänglighet och är utformad för att minimera dessa typer av problem automatiskt. Programmet förlorar dock sin anslutning till servern under en kort tids period på vanligt vis mindre än 60 sekunder. Vissa händelser kan ibland ta längre tid att undvika, till exempel när en stor transaktion orsakar en tids krävande återställning.

### <a name="steps-to-resolve-transient-connectivity-issues"></a>Steg för att lösa problem med tillfälliga anslutningar

1. Kontrol lera [instrument panelen för Microsoft Azures tjänsten](https://azure.microsoft.com/status) för eventuella kända avbrott som uppstod under den tid då felen rapporterades av programmet.
2. Program som ansluter till en moln tjänst som Azure Database for MySQL ska förvänta sig tillfälliga fel och implementera omprövnings logik för att hantera dessa fel i stället för att visa dem som program fel till användare. Granska [hanteringen av tillfälliga anslutnings fel för Azure Database for MySQL](concepts-connectivity.md) för bästa praxis och design rikt linjer för att hantera tillfälliga fel.
3. När en server närmar sig resurs begränsningarna kan det verka som om ett tillfälligt anslutnings problem uppstår. Se [begränsningar i Azure Database for MySQL](concepts-limits.md).
4. Om problem med anslutningen fortsätter eller om varaktigheten för programmet stöter på felet överskrider 60 sekunder eller om du ser flera förekomster av felet under en dag, kan du skicka en support förfrågan till Azure genom att välja **få support** på support webbplatsen för [Azure](https://azure.microsoft.com/support/options) .

## <a name="troubleshoot-persistent-errors"></a>Felsöka beständiga fel

Om programmet inte kan ansluta till Azure Database for MySQL, indikerar det vanligt vis ett problem med något av följande:

* Konfiguration av Server brand vägg: kontrol lera att Azure Database for MySQL server-brandväggen är konfigurerad för att tillåta anslutningar från klienten, inklusive proxyservrar och gatewayer.
* Konfiguration av klient brand väggen: brand väggen på klienten måste tillåta anslutningar till din databas server. IP-adresser och portar på den server som du inte kan tillåta måste vara tillåtna samt program namn som MySQL i vissa brand väggar.
* Användar fel: du kan ange anslutnings parametrar som inte har angetts, till exempel Server namnet i anslutnings strängen eller ett * \@ servername* -suffix som saknas i användar namnet.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Steg för att lösa problem med beständiga anslutningar

1. Konfigurera [brand Väggs regler](howto-manage-firewall-using-portal.md) som tillåter KLIENTens IP-adress. För temporär testning kan du konfigurera en brand Väggs regel med 0.0.0.0 som första IP-adress och använda 255.255.255.255 som sista IP-adress. Då öppnas servern med alla IP-adresser. Om detta löser problemet med anslutningen tar du bort den här regeln och skapar en brand Väggs regel för en lämplig, begränsad IP-adress eller ett IP-adressintervall.
2. Kontrol lera att port 3306 är öppen för utgående anslutningar på alla brand väggar mellan klienten och Internet.
3. Kontrol lera anslutnings strängen och andra anslutnings inställningar. Granska [hur du ansluter program till Azure Database for MySQL](howto-connection-string.md).
4. Kontrol lera tjänstens hälso tillstånd på instrument panelen. Om du tror att det finns ett regionalt avbrott, se [Översikt över affärs kontinuitet med Azure Database for MySQL](concepts-business-continuity.md) för att återställa till en ny region.

## <a name="next-steps"></a>Nästa steg

* [Hantering av tillfälliga anslutnings fel för Azure Database for MySQL](concepts-connectivity.md)
