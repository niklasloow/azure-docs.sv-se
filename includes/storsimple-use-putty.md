---
author: alkohli
ms.service: storsimple
ms.topic: include
ms.date: 10/26/2018
ms.author: alkohli
ms.openlocfilehash: a5c62c67cd401c043352b06e6e6070a7fc0f1296
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "67187381"
---
#### <a name="to-connect-through-the-serial-console"></a>Anslut via seriekonsolen
1. Ansluta din seriekabel till enheten (direkt eller via en USB-serieadapter).
2. Öppna **Kontrollpanelen** och öppna sedan **Enhetshanteraren**.
3. Identifiera COM-portarna som det visas i följande bild.
   
     ![Anslut via seriekonsol](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. Starta PuTTY. 
5. I den högra rutan, ändrar du **Anslutningstyp** till **Seriell**.
6. I den högra rutan, skriver du in lämplig COM-port. Kontrollera att de parametrarna för seriekonfigurationen är inställda på följande sätt:
   
   * Hastighet: 115 200
   * Databitar: 8
   * Stoppbitar: 1
   * Paritet: ingen
   * Flödeskontroll: ingen
     
     Inställningarna visas i följande bild.
     
     ![PuTTY-inställningar](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > Om standardinställningen för flödeskontroll inte fungerar, testa att ställa in flödeskontrollen på XON/XOFF.
     > 
     > 
7. Klicka på **Öppna**, för att starta en seriesession.

