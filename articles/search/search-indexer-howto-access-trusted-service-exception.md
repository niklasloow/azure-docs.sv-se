---
title: Tillåt åtkomst till lagring via betrott tjänst undantag
titleSuffix: Azure Cognitive Search
description: Anvisningar som beskriver hur du konfigurerar ett betrott tjänst undantag för att komma åt data från lagrings konton på ett säkert sätt.
manager: nitinme
author: arv100kri
ms.author: arjagann
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 09/22/2020
ms.openlocfilehash: 30fc71e6f59766a759cdb8e4e503123623f48bd9
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91320480"
---
# <a name="accessing-data-in-storage-accounts-securely-via-trusted-service-exception"></a>Åtkomst till data i lagrings konton på ett säkert sätt via ett betrott tjänst undantag

Indexerare som har åtkomst till data i lagrings konton kan använda funktionen för [betrott tjänst undantag](https://docs.microsoft.com/azure/storage/common/storage-network-security#exceptions) för säker åtkomst till data. Den här mekanismen ger kunder som inte kan bevilja [indexerare åtkomst via IP-brandvägg-regler](search-indexer-howto-access-ip-restricted.md) ett enkelt, säkert och kostnads fritt alternativ för åtkomst till data i lagrings konton.

> [!NOTE]
> Stöd för åtkomst till data i lagrings konton via ett betrott tjänst undantag är begränsat till Azure Blob Storage och Azure Data Lake Gen2-lagring. Azure Table Storage stöds inte.

## <a name="step-1-configure-connection-to-the-storage-account-via-identity"></a>Steg 1: konfigurera anslutningen till lagrings kontot via identitet

Följ informationen som beskrivs i [hanterad identitets åtkomst guide](search-howto-managed-identities-storage.md) för att konfigurera indexerare för åtkomst till lagrings konton via Sök tjänstens hanterade identitet.

## <a name="step-2-allow-trusted-microsoft-services-to-access-the-storage-account"></a>Steg 2: Tillåt att betrodda Microsoft-tjänster får åtkomst till lagrings kontot

I Azure Portal går du till fliken "brand väggar och virtuella nätverk" i lagrings kontot. Kontrol lera att alternativet "Tillåt betrodda Microsoft-tjänster för att komma åt det här lagrings kontot" är markerat. Det här alternativet tillåter endast den angivna search service-instansen med lämplig rollbaserad åtkomst till lagrings kontot (stark autentisering) för att komma åt data i lagrings kontot, även om det skyddas av IP-brandväggs regler.

![Undantag för betrodda tjänster](media\search-indexer-howto-secure-access\exception.png "Undantag för betrodda tjänster")

Indexerare kommer nu att kunna komma åt data i lagrings kontot, även om kontot skyddas via regler för IP-brandvägg.

## <a name="next-steps"></a>Nästa steg

Läs mer om Azure Storage indexerare:

- [Azure Blob-indexeraren](search-howto-indexing-azure-blob-storage.md)
- [Azure Data Lake Storage Gen2 indexerare](search-howto-index-azure-data-lake-storage.md)
- [Azure Table-indexeraren](search-howto-indexing-azure-tables.md)
