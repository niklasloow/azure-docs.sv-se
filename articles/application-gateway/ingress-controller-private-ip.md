---
title: Använd privat IP-adress för intern routning för en ingress-slutpunkt
description: Den här artikeln innehåller information om hur du använder privata IP-adresser för intern Routning och därmed exponerar slut punkten i ett kluster till resten av det virtuella nätverket.
services: application-gateway
author: caya
ms.service: application-gateway
ms.topic: how-to
ms.date: 11/4/2019
ms.author: caya
ms.openlocfilehash: 33b70ba8ab7ffef90c42f53e58a2d27e619862f0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "84806788"
---
# <a name="use-private-ip-for-internal-routing-for-an-ingress-endpoint"></a>Använd privat IP för intern routning för en ingress-slutpunkt 

Den här funktionen gör det möjligt att exponera ingångs slut punkten i `Virtual Network` med en privat IP-adress.

## <a name="pre-requisites"></a>Förutsättningar  
Application Gateway med en [privat IP-konfiguration](https://docs.microsoft.com/azure/application-gateway/configure-application-gateway-with-private-frontend-ip)

Det finns två sätt att konfigurera styrenheten att använda privat IP för ingress,

## <a name="assign-to-a-particular-ingress"></a>Tilldela till ett visst ingress
Om du vill exponera ett visst ingress över privat IP använder du anteckningen [`appgw.ingress.kubernetes.io/use-private-ip`](./ingress-controller-annotations.md#use-private-ip) i ingress.

### <a name="usage"></a>Användning
```yaml
appgw.ingress.kubernetes.io/use-private-ip: "true"
```

För programgatewayer utan privata IP-adresser ignoreras ingressen som kommenteras med `appgw.ingress.kubernetes.io/use-private-ip: "true"` . Detta visas i loggen ingångs händelser och AGIC pod.

* Fel som anges i ingress-händelsen

    ```bash
    Events:
    Type     Reason       Age               From                                                                     Message
    ----     ------       ----              ----                                                                     -------
    Warning  NoPrivateIP  2m (x17 over 2m)  azure/application-gateway, prod-ingress-azure-5c9b6fcd4-bctcb  Ingress default/hello-world-ingress requires Application Gateway 
    applicationgateway3026 has a private IP address
    ```

* Fel som anges i AGIC-loggar

    ```bash
    E0730 18:57:37.914749       1 prune.go:65] Ingress default/hello-world-ingress requires Application Gateway applicationgateway3026 has a private IP address
    ```


## <a name="assign-globally"></a>Tilldela globalt
Om det är ett krav är det att begränsa alla ingångs händelser som ska exponeras över privat IP, använda `appgw.usePrivateIP: true` i `helm` config.

### <a name="usage"></a>Användning
```yaml
appgw:
    subscriptionId: <subscriptionId>
    resourceGroup: <resourceGroupName>
    name: <applicationGatewayName>
    usePrivateIP: true
```

Detta gör att en ingångs kontroll filtrerar IP-adresserna för en privat IP-adress när du konfigurerar klient delens lyssnare på Application Gateway.
AGIC kommer att panik och krascha om `usePrivateIP: true` ingen privat IP-adress har tilldelats.

> [!NOTE]
> Application Gateway v2-SKU kräver en offentlig IP-adress. Om du behöver Application Gateway vara privat måste du ansluta en [`Network Security Group`](https://docs.microsoft.com/azure/virtual-network/security-overview) till Application Gateway-undernätet för att begränsa trafiken.
