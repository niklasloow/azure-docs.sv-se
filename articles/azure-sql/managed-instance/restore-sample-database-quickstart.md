---
title: 'Snabb start: återställa en säkerhets kopia (SSMS)'
titleSuffix: Azure SQL Managed Instance
description: I den här snabb starten lär du dig att återställa en databas säkerhets kopia till Azure SQL-hanterad instans med hjälp av SQL Server Management Studio (SSMS).
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova
ms.date: 12/14/2018
ms.openlocfilehash: 18f717ca05e93c9a8f06ac8868e9a6e5ff80eadb
ms.sourcegitcommit: d95cab0514dd0956c13b9d64d98fdae2bc3569a0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91355541"
---
# <a name="quickstart-restore-a-database-to-azure-sql-managed-instance-with-ssms"></a>Snabb start: återställa en databas till en Azure SQL-hanterad instans med SSMS
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

I den här snabb starten använder du SQL Server Management Studio (SSMS) för att återställa en databas (Wide World Importers – standard säkerhets kopierings fil) från Azure Blob Storage till [Azure SQL Managed instance](sql-managed-instance-paas-overview.md).

> [!VIDEO https://www.youtube.com/embed/RxWYojo_Y3Q]

> [!NOTE]
> Mer information om migrering med hjälp av Azure Database Migration Service finns i [migrering av SQL-hanterad instans med hjälp av Database migration service](../../dms/tutorial-sql-server-to-managed-instance.md).
> Mer information om olika metoder för migrering finns i [SQL Server migrering till Azure SQL-hanterad instans](migrate-to-instance-from-sql-server.md).

## <a name="prerequisites"></a>Förutsättningar

Den här snabbstarten:

- Använder resurser från snabb start för att [skapa en hanterad instans](instance-create-quickstart.md) .
- Kräver att den senaste versionen av [SSMS](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) är installerad.
- Kräver att SSMS används för att ansluta till SQL-hanterad instans. Läs följande snabbstarter om hur man ansluter:
  - [Aktivera en offentlig slut punkt](public-endpoint-configure.md) på SQL-hanterad instans – det här är den rekommenderade metoden för den här självstudien.
  - [Anslut till SQL-hanterad instans från en virtuell Azure-dator](connect-vm-instance-configure.md).
  - [Konfigurera en punkt-till-plats-anslutning till SQL-hanterad instans från den lokala](point-to-site-p2s-configure.md)platsen.

> [!NOTE]
> Mer information om hur du säkerhetskopierar och återställer en SQL Server-databas med hjälp av Azure Blob Storage och en [SAS-nyckel](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) finns i [SQL Server-säkerhetskopiering till URL](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url?view=sql-server-2017).

## <a name="restore-from-a-backup-file"></a>Återställa från en säkerhets kopia

I SQL Server Management Studio följer du de här stegen för att återställa Wide World Importers-databasen till SQL-hanterad instans. Säkerhetskopian av databasfilen lagras i ett förkonfigurerat Azure Blob Storage-konto.

1. Öppna SSMS och Anslut till din hanterade instans.
2. I **Object Explorer**högerklickar du på din hanterade instans och väljer **ny fråga** för att öppna ett nytt frågefönster.
3. Kör följande SQL-skript, som använder ett förkonfigurerat lagrings konto och SAS-nyckel för att [skapa en autentiseringsuppgift](https://docs.microsoft.com/sql/t-sql/statements/create-credential-transact-sql) i din hanterade instans.

   ```sql
   CREATE CREDENTIAL [https://mitutorials.blob.core.windows.net/databases]
   WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
   , SECRET = 'sv=2017-11-09&ss=bfqt&srt=sco&sp=rwdlacup&se=2028-09-06T02:52:55Z&st=2018-09-04T18:52:55Z&spr=https&sig=WOTiM%2FS4GVF%2FEEs9DGQR9Im0W%2BwndxW2CQ7%2B5fHd7Is%3D'
   ```

    ![skapa autentiseringsuppgift](./media/restore-sample-database-quickstart/credential.png)

4. Kontrollera dina autentiseringsuppgifter genom att köra följande skript som använder en [container](https://azure.microsoft.com/services/container-instances/)-URL för att hämta en lista över säkerhetskopior.

   ```sql
   RESTORE FILELISTONLY FROM URL =
      'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![file list](./media/restore-sample-database-quickstart/file-list.png)

5. Återställ Wide World Importers-databasen genom att köra följande skript.

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://mitutorials.blob.core.windows.net/databases/WideWorldImporters-Standard.bak'
   ```

    ![Skärm bild som visar skriptet som körs i Object Explorer med ett meddelande om att det är klart.](./media/restore-sample-database-quickstart/restore.png)

6. Kör följande skript för att spåra status för återställningen.

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time
   FROM sys.dm_exec_requests r
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')
   ```

7. När återställningen är klar kan du Visa databasen i Object Explorer. Du kan kontrol lera att databas återställningen har slutförts med hjälp av [sys. dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) -vyn.

> [!NOTE]
> En databas återställnings åtgärd är asynkron och går att försöka igen. Du kan få ett fel meddelande i SQL Server Management Studio om anslutningen bryts eller om timeout går ut. Azure SQL Database fortsätter att försöka återställa databasen i bakgrunden och du kan följa förloppet för återställningen med hjälp av vyerna [sys. dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) och [sys. dm_operation_status](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-operation-status-azure-sql-database) .
> I vissa faser av återställnings processen visas en unik identifierare i stället för det faktiska databas namnet i systemvyerna. Lär dig mer om `RESTORE` instruktions skillnader [här](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-transact-sql-information#restore-statement).

## <a name="next-steps"></a>Nästa steg

- Om du i steg 5 har återställt en databas återställning med meddelande-ID 22003, skapar du en ny säkerhets kopia som innehåller kontroll summor för säkerhets kopior och utför återställningen igen. Se [Aktivera eller inaktivera kontroll summor för säkerhets kopiering under säkerhets kopiering eller återställning](https://docs.microsoft.com/sql/relational-databases/backup-restore/enable-or-disable-backup-checksums-during-backup-or-restore-sql-server).
- Information om hur du felsöker en säkerhets kopia till en URL finns i [SQL Server säkerhets kopiering till URL metod tips och fel sökning](https://docs.microsoft.com/sql/relational-databases/backup-restore/sql-server-backup-to-url-best-practices-and-troubleshooting).
- En översikt över anslutnings alternativ för appar finns i [ansluta dina program till SQL-hanterad instans](connect-application-instance.md).
- Om du vill fråga med hjälp av dina favorit verktyg eller språk, se [snabb starter: Azure SQL Database Anslut och fråga](../database/connect-query-content-reference-guide.md).
