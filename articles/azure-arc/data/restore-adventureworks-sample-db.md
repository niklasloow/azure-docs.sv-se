---
title: Återställa AdventureWorks-exempel databasen till SQL-hanterad instans
description: Återställa AdventureWorks-exempel databasen till SQL-hanterad instans
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 09/22/2020
ms.topic: how-to
ms.openlocfilehash: bb5cc6fe3b0b6dd1250bbe241086fd4557866d5b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90941497"
---
# <a name="restore-the-adventureworks-sample-database-into-sql-managed-instance---azure-arc"></a>Återställa exempel databasen AdventureWorks till SQL Managed instance – Azure Arc

[AdventureWorks](/sql/samples/adventureworks-install-configure?view=sql-server-ver15&tabs=tsql&preserve-view=true) är en exempel databas som innehåller en OLTP-databas som ofta används i självstudier och exempel. Den tillhandahålls och underhålls av Microsoft som en del av [SQL Server-GitHub-lagringsplatsen](https://github.com/microsoft/sql-server-samples/tree/master/samples/databases).

Det här dokumentet beskriver en enkel process för att hämta AdventureWorks-exempel databasen som återställs till din SQL-hanterade instans – Azure-båge.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="download-the-adventureworks-backup-file"></a>Hämta säkerhets kopie filen AdventureWorks

Hämta filen AdventureWorks Backup (. bak) till din SQL-hanterade instans behållare. I det här exemplet använder du `kubectl exec` kommandot för att fjärrköra ett kommando inuti behållaren SQL-hanterad instans för att ladda ned. bak-filen till behållaren. Ladda ned den här filen från valfri plats som är tillgänglig `wget` om du har andra säkerhets kopierings filer för databasen som du vill hämta i behållaren SQL-hanterad instans. När det är i en SQL-hanterad instans-behållare är det enkelt att återställa med hjälp av standard `RESTORE DATABASE` T-SQL.

Kör ett kommando som detta för att ladda ned. bak-filen och Ersätt värdet för Pod-namn och namn områdes namn innan du kör det.
> [!NOTE]
>  Din behållare måste ha Internet anslutning över 443 för att kunna hämta filen från GitHub

```console
kubectl exec <SQL pod name> -n <namespace name> -c mssql-miaa -- wget https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak -O /var/opt/mssql/data/AdventureWorks2019.bak
```

Exempel

```console
kubectl exec sqltest1-0 -n arc -c mssql-miaa -- wget https://github.com/Microsoft/sql-server-samples/releases/download/adventureworks/AdventureWorks2019.bak -O /var/opt/mssql/data/AdventureWorks2019.bak
```

## <a name="restore-the-adventureworks-database"></a>Återställa AdventureWorks-databasen

På samma sätt kan du köra ett `kubectl` exec-kommando för att använda `sqlcmd` CLI-verktyget som ingår i behållaren SQL-hanterad instans för att köra T-SQL-kommandot för att återställa databasen.

Kör ett kommando som detta för att återställa databasen. Ersätt värdet för Pod-namnet, lösen ordet och namn områdets namn innan du kör det.

```console
kubectl exec <SQL pod name> -n <namespace name> -c mssql-miaa -- /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P <password> -Q "RESTORE DATABASE AdventureWorks2019 FROM  DISK = N'/var/opt/mssql/data/AdventureWorks2019.bak' WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf', MOVE 'AdventureWorks2017_Log' TO '/var/opt/mssql/data/AdventureWorks2019_Log.ldf'"
```
Exempel

```console
kubectl exec sqltest1-0 -n arc -- -c mssql-miaa /opt/mssql-tools/bin/sqlcmd -S localhost -U sa -P MyPassword! -Q "RESTORE DATABASE AdventureWorks2019 FROM  DISK = N'/var/opt/mssql/data/AdventureWorks2019.bak' WITH MOVE 'AdventureWorks2017' TO '/var/opt/mssql/data/AdventureWorks2019.mdf', MOVE 'AdventureWorks2017_Log' TO '/var/opt/mssql/data/AdventureWorks2019_Log.ldf'"
```
