---
title: inkludera fil
description: inkludera fil
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/19/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 3031ec6ba17c61c780351169c4e8c2e5e94ab257
ms.sourcegitcommit: 6e1124fc25c3ddb3053b482b0ed33900f46464b3
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606875"
---
När du genererar ett klient certifikat installeras det automatiskt på den dator som du använde för att generera det. Om du vill installera klient certifikatet på en annan klient dator måste du exportera det klient certifikat som du skapade.

1. Öppna **Hantera användarcertifikat** om du vill exportera ett certifikat. De klient certifikat som du har genererat är som standard placerade i "certificates-Current User\Personal\Certificates". Högerklicka på det klient certifikat som du vill exportera, klicka på **alla aktiviteter**och klicka sedan på **Exportera** för att öppna **guiden Exportera certifikat**.

   ![Skärm bild som visar fönstret certifikat för den aktuella användaren med certifikat som valts och en snabb meny med alla aktiviteter och sedan Exportera markerade.](./media/vpn-gateway-certificates-export-client-cert-include/export.png)
2. I guiden Exportera certifikat klickar du på **Nästa** för att fortsätta.

   ![Skärm bild som visar välkomst meddelande i guiden Exportera certifikat.](./media/vpn-gateway-certificates-export-client-cert-include/next.png)
3. Välj **Ja, exportera den privata nyckeln**och klicka sedan på **Nästa**.

   ![Exportera privat nyckel](./media/vpn-gateway-certificates-export-client-cert-include/privatekeyexport.png)
4. På sidan **Filformat för export** låter du standardalternativen vara markerade. Se till att **Ta med om möjligt alla certifikat i certifieringssökvägen** har valts. Den här inställningen exporterar även rot certifikat informationen som krävs för lyckad klientautentisering. Utan den Miss lyckas klientautentisering eftersom klienten inte har det betrodda rot certifikatet. Klicka sedan på **Nästa**.

   ![fil format för export](./media/vpn-gateway-certificates-export-client-cert-include/includeallcerts.png)
5. Du måste skydda den privata nyckeln på sidan **Säkerhet**. Om du väljer att använda ett lösenord måste du vara noga med att skriva ned eller komma ihåg lösenordet som du anger för det här certifikatet. Klicka sedan på **Nästa**.

   ![Skärm bild som visar säkerhets sidan för guiden Exportera certifikat med det angivna lösen ordet och bekräftat och nästa markerat.](./media/vpn-gateway-certificates-export-client-cert-include/security.png)
6. På sidan **Fil som ska exporteras****bläddrar** du till den plats som du vill exportera certifikatet till. För **Filnamn** anger du ett namn för certifikatfilen. Klicka sedan på **Nästa**.

   ![fil att exportera](./media/vpn-gateway-certificates-export-client-cert-include/filetoexport.png)
7. Klicka på **Slutför** för att exportera certifikatet.

   ![Skärm bild som visar den sista sidan i guiden Exportera certifikat med de valda inställningarna.](./media/vpn-gateway-certificates-export-client-cert-include/finish.png)