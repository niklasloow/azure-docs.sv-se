---
title: Vanliga frågor och svar – Säkerhetskopiera SAP HANA-databaser på virtuella Azure-datorer
description: I den här artikeln hittar du svar på vanliga frågor om hur du säkerhetskopierar SAP HANA databaser med hjälp av tjänsten Azure Backup.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: dcbf1bf6b39b2afa3fb5aaf2a7f18c5d0e8e4afb
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513514"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Vanliga frågor och svar – säkerhetskopiera SAP HANA databaser på virtuella Azure-datorer

I den här artikeln besvaras vanliga frågor om att säkerhetskopiera SAP HANA-databaser med hjälp av tjänsten Azure Backup.

## <a name="backup"></a>Backup

### <a name="how-many-full-backups-are-supported-per-day"></a>Hur många fullständiga säkerhets kopieringar stöds per dag?

Vi stöder bara en fullständig säkerhets kopiering per dag. Du kan inte ha differentiell säkerhets kopiering och fullständig säkerhets kopiering aktive rad på samma dag.

### <a name="do-successful-backup-jobs-create-alerts"></a>Skapar lyckade säkerhetskopieringsjobb aviseringar?

Nej. Lyckade säkerhetskopieringsjobb genererar inte aviseringar. Aviseringar skickas bara för säkerhetskopieringsjobb som misslyckas. Detaljerade funktioner för Portal varningar dokumenteras [här](./backup-azure-monitoring-built-in-monitor.md). Men om du är intresse rad av aviseringar även för lyckade jobb kan du använda [Azure Monitor](./backup-azure-monitoring-use-azuremonitor.md).

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Kan jag se schemalagda säkerhets kopierings jobb på menyn säkerhets kopierings jobb?

Menyn säkerhets kopierings jobb visar bara ad hoc-säkerhetskopierade jobb. För schemalagda jobb använder du [Azure Monitor](./backup-azure-monitoring-use-azuremonitor.md).

### <a name="are-future-databases-automatically-added-for-backup"></a>Läggs framtida databaser automatiskt till för säkerhetskopiering?

Nej, detta stöds inte för närvarande.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Vad händer med säkerhets kopior om jag tar bort en databas från en instans?

Om en databas bryts från en SAP HANA instans, görs fortfarande databas säkerhets kopiorna. Detta innebär att den borttagna databasen börjar visas som ohälsosam under **säkerhets kopierings objekt** och fortfarande är skyddad.
Det korrekta sättet att sluta skydda den här databasen är att **stoppa säkerhets kopieringen med ta bort data** på den här databasen.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Vad händer om jag ändrar namnet på databasen efter att det har skyddats?

En döpt databas behandlas som en ny databas. Tjänsten kommer därför att behandla den här situationen som om databasen inte hittades och att säkerhets kopieringen Miss lyckas. Den omdöpta databasen visas som en ny databas och måste konfigureras för skydd.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>Vilka är kraven för att säkerhetskopiera SAP HANA databaser på en virtuell Azure-dator?

Se [kraven](tutorial-backup-sap-hana-db.md#prerequisites) och [Vad skriptet gör för registrering](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does) .

### <a name="what-permissions-should-be-set-so-azure-can-back-up-sap-hana-databases"></a>Vilka behörigheter ska ställas in så att Azure kan säkerhetskopiera SAP HANA databaser?

Genom att köra skriptet före registreringen anger du de behörigheter som krävs för att Azure ska kunna säkerhetskopiera SAP HANA-databaser. Du hittar mer information om skriptet för för [registrering.](tutorial-backup-sap-hana-db.md#what-the-pre-registration-script-does)

### <a name="will-backups-work-after-migrating-sap-hana-from-sdc-to-mdc"></a>Kommer säkerhets kopieringar att fungera efter migrering av SAP HANA från SDC till MDC?

Se [det här avsnittet](./backup-azure-sap-hana-database-troubleshoot.md#sdc-to-mdc-upgrade-with-a-change-in-sid) i fel söknings guiden.

### <a name="can-azure-hana-backup-be-set-up-against-a-virtual-ip-load-balancer-and-not-a-virtual-machine"></a>Kan Azure HANA-säkerhetskopiering konfigureras mot en virtuell IP-adress (belastningsutjämnare) och inte en virtuell dator?

För närvarande har vi inte möjlighet att konfigurera lösningen mot enbart en virtuell IP-adress. Vi behöver en virtuell dator för att köra lösningen.

### <a name="how-can-i-move-an-on-demand-backup-to-the-local-file-system-instead-of-the-azure-vault"></a>Hur kan jag flytta en säkerhets kopia på begäran till det lokala fil systemet i stället för Azure-valvet?

1. Vänta tills den pågående säkerhets kopieringen har slutförts för den önskade databasen (kontrol lera att Studio är klar).
1. Inaktivera logg säkerhets kopior och ange katalog säkerhets kopian till **fil systemet** för den önskade databasen med hjälp av följande steg:
1. Dubbelklicka på **SYSTEMDB**  ->  **konfiguration**  ->  **Välj databas**  ->  **filter (logg)**
    1. Ange enable_auto_log_backup till **Nej**
    1. Ange catalog_backup_using_backint till **falskt**
1. Ta en säkerhets kopia på begäran (fullständig/differentiell/stegvis) på önskad databas och vänta tills säkerhets kopieringen och katalogen har slutförts.
1. Om du även vill flytta logg säkerhets kopiorna till fil systemet anger du enable_auto_log_backup till **Ja**
1. Återgå till de tidigare inställningarna så att säkerhets kopieringar kan flöda till Azure-valvet:
    1. Ange enable_auto_log_backup till **Ja**
    1. Ange catalog_backup_using_backint till **Sant**

>[!NOTE]
>Om du flyttar säkerhets kopior till det lokala fil systemet och växlar tillbaka till Azure-valvet kan logg kedjan brytas över logg säkerhets kopiorna i valvet. Detta utlöser en fullständig säkerhets kopiering, vilket innebär att när du har slutfört säkerhets kopieringen av loggarna.

### <a name="how-can-i-use-sap-hana-backup-with-my-hana-replication-set-up"></a>Hur kan jag använda SAP HANA säkerhets kopiering med min HANA-replikering?

För närvarande har Azure Backup inte möjlighet att förstå en HSR-konfiguration. Det innebär att de primära och sekundära noderna i HSR kommer att behandlas som två enskilda, icke-relaterade virtuella datorer. Du måste först konfigurera säkerhets kopiering på den primära noden. När ett misslyckande inträffar måste säkerhets kopieringen konfigureras på den sekundära noden (som nu blir den primära noden). Det finns ingen automatisk redundans av säkerhets kopiering till den andra noden.

Om du vill säkerhetskopiera data från den aktiva (primära) noden vid en viss tidpunkt kan du **Växla skydd** till den sekundära noden, som nu är primärt efter redundans.

Följ dessa steg om du vill utföra det här **växel skyddet**:

- [Stoppa skyddet](sap-hana-db-manage.md#stop-protection-for-an-sap-hana-database) (med Behåll data) på primär
- Kör [skriptet för för registrering](https://aka.ms/scriptforpermsonhana) på den sekundära noden
- [Identifiera databaserna](tutorial-backup-sap-hana-db.md#discover-the-databases) på den sekundära noden och [Konfigurera säkerhets kopior](tutorial-backup-sap-hana-db.md#configure-backup) på dem

De här stegen måste utföras manuellt efter varje redundans. Du kan utföra dessa steg genom kommando raden/HTTP REST förutom Azure Portal. Du kan automatisera de här stegen genom att använda en Azure-Runbook.

Här är ett detaljerat exempel på hur **växel skydd** måste utföras:

I det här exemplet har du två noder-Node 1 (primär) och nod 2 (sekundär) i HSR-konfiguration.  Säkerhets kopior konfigureras på nod 1. Som nämnts ovan försöker du inte konfigurera säkerhets kopior på nod 2 ännu.

När den första redundansväxlingen sker blir nod 2 primär. Dra

1. Stoppa skyddet av nod 1 (tidigare primär) med alternativet Kvarhåll data.
1. Kör skriptet för för registrering på nod 2 (som nu är den primära).
1. Identifiera databaser på nod 2, tilldela säkerhets kopierings policy och konfigurera säkerhets kopior.

Sedan utlöses en första fullständig säkerhets kopiering på nod 2 och när det är klart startar logg säkerhets kopieringen.

När nästa misslyckade händer blir nod 1 primär igen och nod 2 blir sekundär. Upprepa nu processen:

1. Stoppa skyddet av nod 2 med alternativet för kvarhållning av data.
1. Kör skriptet för för registrering på nod 1 (som har blivit primärt igen)
1. [Återuppta sedan säkerhets kopieringen](sap-hana-db-manage.md#resume-protection-for-an-sap-hana-database) på nod 1 med den nödvändiga principen (eftersom säkerhets kopiorna stoppades tidigare på nod 1).

Sedan aktive ras den fullständiga säkerhets kopieringen på nod 1 och när den är klar startar logg säkerhets kopieringen.

## <a name="restore"></a>Återställ

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>Varför kan jag inte se det HANA-system jag vill att min databas ska återställas till?

Kontrol lera om alla krav för att återställa till mål SAP HANA instansen är uppfyllda. Mer information finns i [krav-Restore SAP HANA-databaser i Azure VM](./sap-hana-db-restore.md#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>Varför går det inte att återställa Skriv över DB för min databas?

Se till att alternativet **Framtvinga överskrivning** är valt vid återställning.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>Varför visas fel meddelandet "käll-och mål system för återställning är inkompatibla"?

Läs SAP HANA anmärkning [1642148](https://launchpad.support.sap.com/#/notes/1642148) för att se vilka återställnings typer som stöds för närvarande.

### <a name="can-i-use-a-backup-of-a-database-running-on-sles-to-restore-to-an-rhel-hana-system-or-vice-versa"></a>Kan jag använda en säkerhets kopia av en databas som körs på SLES för att återställa till ett RHEL HANA-system eller vice versa?

Ja, du kan använda strömmande säkerhets kopieringar som har utlösts på en HANA-databas som körs på SLES för att återställa den till ett RHEL HANA-system och vice versa. Det vill säga det går att återställa mellan operativ system med hjälp av strömmande säkerhets kopior. Du måste dock se till att det HANA-system som du vill återställa till, och det HANA-system som används för återställning, båda är kompatibla för återställning enligt SAP. Läs SAP HANA anmärkning [1642148](https://launchpad.support.sap.com/#/notes/1642148) för att se vilka återställnings typer som är kompatibla.

## <a name="next-steps"></a>Nästa steg

Lär dig hur du [säkerhetskopierar SAP HANA databaser](./backup-azure-sap-hana-database.md) som körs på virtuella Azure-datorer.
