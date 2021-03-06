---
title: Konfigurera gransknings loggar – Azure Portal-Azure Database for MySQL – flexibel Server
description: Den här artikeln beskriver hur du konfigurerar och får åtkomst till gransknings loggarna i Azure Database for MySQL flexibel Server från Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 9/21/2020
ms.openlocfilehash: b8fe32a079358fda48c6f5ee0c7eec9894a543a5
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91295914"
---
# <a name="configure-and-access-audit-logs-for-azure-database-for-mysql---flexible-server-using-the-azure-portal"></a>Konfigurera och få åtkomst till gransknings loggar för Azure Database for MySQL-flexibel server med hjälp av Azure Portal

> [!IMPORTANT]
> Azure Database for MySQL-flexibel Server är för närvarande en offentlig för hands version.

Du kan konfigurera Azure Database for MySQL flexibla Server [gransknings loggar](concepts-audit-logs.md) och diagnostikinställningar från Azure Portal.

## <a name="prerequisites"></a>Förutsättningar
Anvisningarna i den här artikeln kräver att du har en [flexibel Server](quickstart-create-server-portal.md).

## <a name="configure-audit-logging"></a>Konfigurera gransknings loggning

>[!IMPORTANT]
> Vi rekommenderar att du bara loggar de händelse typer och användare som krävs för gransknings syfte för att säkerställa att serverns prestanda inte påverkas kraftigt.

Aktivera och konfigurera gransknings loggning.

1. Logga in på [Azure-portalen](https://portal.azure.com/).

1. Välj din flexibla Server.

1. Under avsnittet **Inställningar** på sid panelen väljer du **Server parametrar**.
    <!--:::image type="content" source="./media/howto-configure-audit-logs-portal/server-parameters.png" alt-text="Server parameters":::-->

1. Uppdatera **audit_log_enabled** -parametern till på.
    <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-enabled.png" alt-text="Enable audit logs":::-->

1. Välj de [händelse typer](concepts-audit-logs.md#configure-audit-logging) som ska loggas genom att uppdatera **audit_log_events** -parametern.
    <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-events.png" alt-text="Audit log events":::-->

1. Lägg till alla MySQL-användare som ska undantas från loggning genom att uppdatera **audit_log_exclude_users** -parametern. Ange användare genom att ange sitt användar namn för MySQL.
    <!--:::image type="content" source="./media/howto-configure-audit-logs-portal/audit-log-exclude-users.png" alt-text="Audit log exclude users":::-->

1. När du har ändrat parametrarna kan du klicka på **Spara**. Eller så kan du **Ignorera** dina ändringar.
    <!--:::image type="content" source="./media/howto-configure-audit-logs-portal/save-parameters.png" alt-text="Save":::-->

## <a name="set-up-diagnostics"></a>Konfigurera diagnostik

> [!NOTE]
> Att integrera med Azure Monitor diagnostikinställningar för att komma åt loggarna är under distributionen och att alla funktioner blir tillgängliga snart.

Gransknings loggar är integrerade med Azure Monitor diagnostikinställningar så att du kan skicka loggar till Azure Monitor loggar, Event Hubs eller Azure Storage.

1. Under avsnittet **övervakning** i sid panelen väljer du **diagnostikinställningar**.

1. Klicka på "+ Lägg till diagnostisk inställning"  <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/add-diagnostic-setting.png" alt-text="Add diagnostic setting":::-->

1. Ange ett namn på en diagnostisk inställning.

1. Ange vilka destinationer som gransknings loggarna (lagrings konto, händelsehubben och/eller Log Analytics arbets yta) ska skickas till.

1. Välj **MySqlAuditLogs** som logg typ.
    <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/configure-diagnostic-setting.png" alt-text="Configure diagnostic setting"::: -->

1. När du har konfigurerat data Sinks att skicka in gransknings loggarna till kan du klicka på **Spara**.
    <!-- :::image type="content" source="./media/howto-configure-audit-logs-portal/save-diagnostic-setting.png" alt-text="Save diagnostic setting":::-->

1. Få åtkomst till gransknings loggarna genom att utforska dem i de data mottagare du konfigurerade. Det kan ta upp till 10 minuter innan loggarna visas.

Om du skickas gransknings loggarna till Azure Monitor loggar (Log Analytics) kan du läsa några [exempel frågor](concepts-audit-logs.md#analyze-logs-in-azure-monitor-logs) som du kan använda för analys.  

## <a name="next-steps"></a>Nästa steg

- Läs mer om [gransknings loggar](concepts-audit-logs.md)
- Läs mer om [långsamma Query-loggar](concepts-slow-query-logs.md)
<!-- - Learn how to configure audit logs in the [Azure CLI](howto-configure-audit-logs-cli.md)-->