---
title: inkludera fil
description: inkludera fil
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: include
ms.date: 10/07/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e2a950037aed2a8ded4d4e55920721285cbfc05c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "82204491"
---
Tänk på följande när du arbetar med anpassade IPsec-principer:

* **IKE** – för IKE kan du välja vilken parameter som helst från IKE-kryptering, plus vilken parameter som helst från IKE-integritet, samt vilken parameter som helst från DH-gruppen.
* **IPSec** -för IPSec kan du välja valfri parameter från IPSec-kryptering, plus vilken parameter som helst från IPSec-integritet, plus PFS. Om någon av parametrarna för IPsec-kryptering eller IPsec-integritet är GCM måste parametrarna för båda inställningarna vara GCM.

>[!NOTE]
> Med anpassade IPsec-principer finns det inget koncept för responder och Initiator (till skillnad från IPsec-standardprinciper). Både sidor (lokala och Azure VPN-gateway) kommer att använda samma inställningar för IKE fas 1 och IKE fas 2. Både IKEv1-och IKEv2-protokoll stöds. Det finns inget stöd för Azure som en responder.
>

**Tillgängliga inställningar och parametrar**

| Inställningen | Parametrar |
|--- |--- |
| IKE-kryptering | GCMAES256, GCMAES128, AES256, AES128 |
| IKE-integritet | SHA384, SHA256 |
| DH-grupp | ECP384, ECP256, DHGroup24, DHGroup14 |
| IPsec-kryptering | GCMAES256, GCMAES128, AES256, AES128, ingen |
| IPsec Integrity | GCMAES256, GCMAES128, SHA256 |
| PFS-grupp | ECP384, ECP256, PFS24, PFS14, ingen |
