---
title: Felsöka Azure Cosmos DB tjänsten otillgängliga undantag
description: Lär dig hur du diagnostiserar och åtgärdar Azure Cosmos DB tjänsten otillgängliga undantag.
author: j82w
ms.service: cosmos-db
ms.date: 08/06/2020
ms.author: jawilley
ms.topic: troubleshooting
ms.reviewer: sngun
ms.openlocfilehash: 749592b778612c6903c9c15e336de3fb00978199
ms.sourcegitcommit: 927dd0e3d44d48b413b446384214f4661f33db04
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/26/2020
ms.locfileid: "88870875"
---
# <a name="diagnose-and-troubleshoot-azure-cosmos-db-service-unavailable-exceptions"></a>Diagnostisera och Felsök Azure Cosmos DB tjänsten otillgängliga undantag
Det gick inte att ansluta till Azure Cosmos DB i SDK.

## <a name="troubleshooting-steps"></a>Felsökningsanvisningar
Följande lista innehåller kända orsaker och lösningar för otillgängliga tjänst undantag.

### <a name="the-required-ports-are-being-blocked"></a>De portar som krävs blockeras
Kontrol lera att alla [nödvändiga portar](performance-tips-dotnet-sdk-v3-sql.md#networking) är aktiverade.

### <a name="client-side-transient-connectivity-issues"></a>Problem med tillfälliga anslutningar på klient Sidan
Undantag för otillgängliga tjänster kan vara en yta när det finns tillfälliga anslutnings problem som orsakar tids gränser. Normalt innehåller stack spårningen som är relaterad till det här scenariot ett `TransportException` fel. Exempel:

```C#
TransportException: A client transport error occurred: The request timed out while waiting for a server response. 
(Time: xxx, activity ID: xxx, error code: ReceiveTimeout [0x0010], base error: HRESULT 0x80131500
```

Följ [fel söknings stegen för begäran](troubleshoot-dot-net-sdk-request-timeout.md#troubleshooting-steps) för att lösa problemet.

### <a name="service-outage"></a>Tjänstavbrott
Kontrol lera [Azure-statusen](https://status.azure.com/status) för att se om det finns ett pågående problem.


## <a name="next-steps"></a>Nästa steg
* [Diagnostisera och Felsök](troubleshoot-dot-net-sdk.md) problem när du använder Azure Cosmos dB .NET SDK.
* Lär dig mer om prestanda rikt linjer för [.net v3](performance-tips-dotnet-sdk-v3-sql.md) och [.NET v2](performance-tips.md).