---
title: Utöka lagringen automatiskt – Azure Portal – Azure Database for MySQL
description: I den här artikeln beskrivs hur du kan aktivera lagring med automatisk storleks ökning för Azure Database for MySQL med Azure Portal
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 3/18/2020
ms.openlocfilehash: d4dc5c2690be7b9abbda685e78ea562878626b5c
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90902854"
---
# <a name="auto-grow-storage-in-azure-database-for-mysql-using-the-azure-portal"></a>Utöka lagringen automatiskt i Azure Database for MySQL att använda Azure Portal
I den här artikeln beskrivs hur du kan konfigurera en Azure Database for MySQL Server lagring så att den växer utan att arbets belastningen påverkas.

När en server når den tilldelade lagrings gränsen markeras servern som skrivskyddad. Men om du aktiverar automatisk storleks ökning ökar server lagringen för att rymma växande data. För servrar med mindre än 100 GB allokerat lagrings utrymme ökas den allokerade lagrings storleken med 5 GB så snart det lediga lagrings utrymmet är lägre än eller lika med 1 GB eller 10% av det allokerade lagrings utrymmet. För servrar som har mer än 100 GB allokerat lagrings utrymme ökas den allokerade lagrings storleken med 5% när det lediga lagrings utrymmet är under 5% av den allokerade lagrings storleken. De maximala lagrings gränser som anges [här](https://docs.microsoft.com/azure/mysql/concepts-pricing-tiers#storage) gäller.

## <a name="prerequisites"></a>Förutsättningar
För att slutföra den här instruktions guiden behöver du:
- En [Azure Database for MySQL-server](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="enable-storage-auto-grow"></a>Aktivera automatisk storleks ökning för lagring 

Följ de här stegen för att ställa in MySQL-serverns lagrings storlek automatiskt:

1. I [Azure Portal](https://portal.azure.com/)väljer du din befintliga Azure Database for MySQL-server.

2. På sidan MySQL-server under **inställnings** rubrik klickar du på **pris nivå** för att öppna sidan pris nivå.

3. I avsnittet automatisk utökning väljer du **Ja** för att aktivera automatisk storleks ökning.

    :::image type="content" source="./media/howto-auto-grow-storage-portal/3-auto-grow.png" alt-text="Azure Database for MySQL-Settings_Pricing_tier-Auto-EXPTREND":::

4. Spara ändringarna genom att klicka på **OK**.

5. Ett meddelande bekräftar att den automatiska utökningen har Aktiver ATS.

    :::image type="content" source="./media/howto-auto-grow-storage-portal/5-auto-grow-success.png" alt-text="Azure Database for MySQL-automatisk tillväxt lyckades":::

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [hur du skapar aviseringar för mått](howto-alert-on-metric.md).
