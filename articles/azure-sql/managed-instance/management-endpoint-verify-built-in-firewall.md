---
title: Verifiera port säkerhet i den inbyggda brand väggen
description: Lär dig hur du verifierar det inbyggda brand Väggs skyddet i Azure SQL-hanterad instans.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: da9f3e2b6b8936c74e20a226b606082fb3e0d3ea
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91263172"
---
# <a name="verify-the-azure-sql-managed-instance-built-in-firewall"></a>Kontrollera den inbyggda brandväggen i Azure SQL Managed Instance
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

De [obligatoriska inkommande säkerhets reglerna](connectivity-architecture-overview.md#mandatory-inbound-security-rules) för Azure SQL Managed instance kräver att hanterings portarna 9000, 9003, 1438, 1440 och 1452 är öppna från **vilken källa som helst** i nätverks säkerhets gruppen (NSG) som skyddar SQL-hanterad instans. Även om dessa portar är öppna på NSG-nivån, skyddas de på nätverks nivå av den inbyggda brand väggen.

## <a name="verify-firewall"></a>Verifiera brand väggen

Verifiera portarna genom att använda ett säkerhets skanner verktyg för att testa portarna. Följande skärm bild visar hur du använder något av dessa verktyg.

![Verifiera inbyggd brand vägg](./media/management-endpoint-verify-built-in-firewall/03_verify_firewall.png)

## <a name="next-steps"></a>Nästa steg

Mer information om SQL-hanterad instans och anslutningar finns i [anslutnings arkitektur för Azure SQL Managed instance](connectivity-architecture-overview.md).
