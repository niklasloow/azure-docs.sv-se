---
title: Installera uppdatering på Azure Data Box Gateway serie enhet | Microsoft Docs
description: Beskriver hur du tillämpar uppdateringar med hjälp av Azure Portal och lokalt webb gränssnitt för Azure Data Box Gateway serie enhet
services: databox
author: alkohli
ms.service: databox
ms.topic: article
ms.date: 06/30/2020
ms.author: alkohli
ms.openlocfilehash: 1b3f0faa2b5f67a23317935f0ad868e3872cf86e
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/13/2020
ms.locfileid: "90055971"
---
# <a name="update-your-azure-data-box-gateway"></a>Uppdatera din Azure Data Box Gateway

I den här artikeln beskrivs de steg som krävs för att installera uppdateringen på din Azure Data Box Gateway via det lokala webb gränssnittet och via Azure Portal. Du installerar program uppdateringar eller hotfixar för att hålla din Data Box Gateway enhet uppdaterad.

> [!IMPORTANT]
>
> - Uppdatering **1911** motsvarar **1.6.1049.786** program varu version på enheten. Information om den här uppdateringen finns i [viktig information](data-box-gateway-1911-release-notes.md).
>
> - Kom ihåg enheten startas om när du installerar en uppdatering eller korrigering. Eftersom Data Box Gateway är en enhet med en enda nod avbryts alla pågående I/O-åtgärder och enheten drabbas av driftstopp på upp till 30 minuter medan programvaran uppdateras.

Vart och ett av dessa steg beskrivs i följande avsnitt.

## <a name="use-the-azure-portal"></a>Använda Azure-portalen

Vi rekommenderar att du installerar uppdateringar via Azure Portal. Enheten söker automatiskt efter uppdateringar en gång om dagen. När uppdateringarna är tillgängliga visas ett meddelande i portalen. Sedan kan du hämta och installera uppdateringarna.

> [!NOTE]
> Kontrol lera att enheten är felfri och att statusen visas som **online** innan du installerar uppdateringarna.

1. När uppdateringarna är tillgängliga för din enhet visas ett meddelande. Välj meddelandet eller från det övre kommando fältet, **Uppdatera enhet**. Detta gör att du kan tillämpa enhets program uppdateringar.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-01a.png)

2. På bladet **enhets uppdateringar** kontrollerar du att du har granskat de licens villkor som är associerade med nya funktioner i viktig information.

    Du kan välja att **Ladda ned och installera** uppdateringarna eller bara **Hämta** uppdateringarna. Du kan sedan välja att installera uppdateringarna senare.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-02.png)

    Om du vill hämta och installera uppdateringarna markerar du alternativet som uppdateras automatiskt när hämtningen är klar.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-03.png)

3. Hämtningen av uppdateringar startar. Du ser ett meddelande om att nedladdningen pågår.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-05.png)

    En meddelande banderoll visas också i Azure Portal. Detta anger hämtnings förloppet.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-08a.png)

    Du kan välja det här meddelandet eller välja **Uppdatera enhet** för att visa detaljerad status för uppdateringen.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-09.png)

4. När hämtningen är klar uppdateras meddelande banderollen för att visa att åtgärden har slutförts. Om du väljer att hämta och installera uppdateringarna startar installationen automatiskt.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-10a.png)

    Om du väljer att bara hämta uppdateringar väljer du meddelandet för att öppna bladet med **enhets uppdateringar** . Välj **installera**.
  
    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-11a.png)

5. Du ser ett meddelande om att installationen pågår.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-12a.png)

    Portalen visar också en informations avisering som indikerar att installationen pågår. <!-- The device goes offline and is in maintenance mode.-->

    <!-- ![Software version after update](./media/data-box-gateway-apply-updates/update-13.png)-->

6. Eftersom det här är en enhet med 1 nod startar enheten om när uppdateringarna har installerats. Den kritiska varningen under omstarten indikerar att enhetens pulsslag förloras.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-19a.png)

    Välj aviseringen för att se motsvarande enhets händelse.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-20a.png)

7. Enhets statusen uppdateras till **online** efter att uppdateringarna har installerats.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-23a.png)

    Välj **enhets uppdateringar**från det övre kommando fältet. Kontrol lera att uppdateringen har installerats och att enhetens program varu version återspeglar detta.

    ![Program varu version efter uppdatering](./media/data-box-gateway-apply-updates/portal-apply-update-24.png)

## <a name="use-the-local-web-ui"></a>Använd det lokala webb gränssnittet

Det finns två steg när du använder det lokala webb gränssnittet:

- Hämta uppdateringen eller snabb korrigeringen
- Installera uppdateringen eller snabb korrigeringen

Vart och ett av dessa steg beskrivs ingående i följande avsnitt.

### <a name="download-the-update-or-the-hotfix"></a>Hämta uppdateringen eller snabb korrigeringen

Utför följande steg för att ladda ned uppdateringen. Du kan hämta uppdateringen från den Microsoft-angivna platsen eller från Microsoft Update katalogen.

Utför följande steg för att ladda ned uppdateringen från Microsoft Update katalogen.

1. Starta webbläsaren och gå till [https://catalog.update.microsoft.com](https://catalog.update.microsoft.com) .

   ![Sökkatalog](./media/data-box-gateway-apply-updates/download-update-1.png)

2. I rutan Sök i Microsoft Updates katalogen anger du Knowledge Base-numret för snabb korrigeringen eller villkoren för den uppdatering som du vill ladda ned. Ange till exempel **Azure Data Box Gateway**och klicka sedan på **Sök**.

   Uppdaterings listan visas som **Azure Data Box Gateway 1911**.

   ![Sökkatalog](./media/data-box-gateway-apply-updates/download-update-2.png)

3. Välj **Hämta**. Det finns en enda fil att ladda ned, vilket kallas *SoftwareUpdatePackage.exe* som motsvarar enhetens program uppdatering. Ladda ned filen till en mapp i det lokala systemet. Du kan också kopiera mappen till en nätverks resurs som kan kontaktas från enheten.

   ![Sökkatalog](./media/data-box-gateway-apply-updates/download-update-3.png)

### <a name="install-the-update-or-the-hotfix"></a>Installera uppdateringen eller snabb korrigeringen

Innan du installerar uppdateringen eller hotfixen bör du kontrol lera att:

- Du har uppdateringen eller snabb korrigeringen som hämtats antingen lokalt på värden eller kan nås via en nätverks resurs.
- Enhetens status är felfri så som visas på sidan **Översikt** i det lokala webb gränssnittet.

   ![uppdatera enhet](./media/data-box-gateway-apply-updates/local-ui-update-1.png)

Den här proceduren tar cirka 20 minuter att slutföra. Utför följande steg för att installera uppdateringen eller snabb korrigeringen.

1. I det lokala webb gränssnittet går du till **Underhåll**  >  **program uppdatering**. Anteckna den program varu version som du kör.

   ![uppdatera enhet](./media/data-box-gateway-apply-updates/local-ui-update-2.png)

2. Ange sökvägen till uppdaterings filen. Du kan också bläddra till installations filen för uppdateringen om den placeras på en nätverks resurs. Välj program uppdaterings filen med *SoftwareUpdatePackage.exe* suffix.

   ![uppdatera enhet](./media/data-box-gateway-apply-updates/local-ui-update-3.png)

3. Välj **Använd**.

   ![uppdatera enhet](./media/data-box-gateway-apply-updates/local-ui-update-4.png)

4. När du uppmanas att bekräfta, väljer du **Ja** för att fortsätta. Om enheten är en enskild nod enhet startas enheten om när uppdateringen har installerats och det finns avbrott.
   ![uppdatera enhet](./media/data-box-gateway-apply-updates/local-ui-update-5.png)

5. Uppdateringen startar. När enheten har uppdaterats startas den om. Det lokala användar gränssnittet är inte tillgängligt under denna varaktighet.

6. När omstarten är klar tas du till **inloggnings** sidan. Kontrol lera att enhetens program vara har uppdaterats genom att gå till **Underhåll**  >  **program uppdatering**i det lokala webb gränssnittet. Den program varu version som visas i det här exemplet är **1.6.1049.786**.

   ![uppdatera enhet](./media/data-box-gateway-apply-updates/local-ui-update-6.png)

## <a name="next-steps"></a>Nästa steg

Läs mer om hur [du administrerar dina Azure Data Box Gateway](data-box-gateway-manage-users.md).
