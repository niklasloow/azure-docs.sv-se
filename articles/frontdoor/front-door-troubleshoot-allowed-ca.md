---
title: Tillåten certifikat utfärdare för att aktivera anpassad HTTPS på Azures front dörr
description: Om du använder ett eget certifikat för att aktivera HTTPS på en Azure 0custom-domän måste du använda en tillåten certifikat utfärdare (CA) för att skapa den.
services: frontdoor
documentationcenter: ''
author: duongau
ms.assetid: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/23/2018
ms.author: duau
ms.openlocfilehash: 973df2505eefc2a46aa105b874f32b61fe6e8b36
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91269817"
---
# <a name="allowed-certificate-authorities-for-enabling-custom-https-on-azure-front-door"></a>Tillåtna certifikat utfärdare för att aktivera anpassad HTTPS på Azures front dörr

När du [aktiverar https-funktionen med hjälp av ditt eget certifikat](front-door-custom-domain-https.md?tabs=option-2-enable-https-with-your-own-certificate)för en anpassad domän för Azure-hemdörr måste du använda en tillåten certifikat utfärdare (ca) för att skapa ett TLS/SSL-certifikat. Annars, om du använder en icke-tillåten CA eller ett självsignerat certifikat, kommer din begäran att avvisas.

[!INCLUDE [cdn-front-door-allowed-ca](../../includes/cdn-front-door-allowed-ca.md)]
