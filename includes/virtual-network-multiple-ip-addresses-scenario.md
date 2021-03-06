---
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 11/09/2018
ms.author: genli
ms.openlocfilehash: 3df4108907a4e1e65a444faf1049163966b7accf
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "67187265"
---
## <a name="scenario"></a>Scenario
En virtuell dator med ett enda nätverkskort skapas och ansluts till ett virtuellt nätverk. Den virtuella datorn kräver tre olika *privata* IP-adresser och två *offentliga* IP-adresser. IP-adresserna tilldelas följande IP-konfigurationer:

* **Ipconfig-1:** Tilldelar en *statisk* privat IP-adress och en *statisk* offentlig IP-adress.
* **Ipconfig-2:** Tilldelar en *statisk* privat IP-adress och en *statisk* offentlig IP-adress.
* **Ipconfig-3:** Tilldelar en *statisk* privat IP-adress och ingen offentlig IP-adress.
  
    ![Flera IP-adresser](./media/virtual-network-multiple-ip-addresses-scenario/multiple-ipconfigs.png)

IP-konfigurationerna är kopplade till NÄTVERKSKORTet när NÄTVERKSKORTet skapas och NÄTVERKSKORTet är kopplat till den virtuella datorn när den virtuella datorn skapas. De typer av IP-adresser som används för scenariot är för illustration. Du kan tilldela vilken typ av IP-adress och tilldelning du behöver.

> [!NOTE]
> Även om stegen i den här artikeln tilldelar alla IP-konfigurationer till ett enda nätverkskort, kan du även tilldela flera IP-konfigurationer till alla nätverkskort i en virtuell dator med flera nätverkskort. Information om hur du skapar en virtuell dator med flera nätverkskort finns i artikeln [skapa en virtuell dator med flera nätverkskort](../articles/virtual-machines/windows/multiple-nics.md) .