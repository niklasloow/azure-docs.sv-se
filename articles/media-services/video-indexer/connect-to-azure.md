---
title: Skapa ett Video Indexer-konto som är anslutet till Azure
titleSuffix: Azure Media Services
description: Lär dig hur du skapar ett Video Indexer-konto som är anslutet till Azure.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/08/2020
ms.author: juliako
ms.openlocfilehash: 405533aad8247350d45cc53009abe6b58a511264
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/02/2020
ms.locfileid: "83005935"
---
# <a name="create-a-video-indexer-account-connected-to-azure"></a>Skapa ett Video Indexer-konto som är anslutet till Azure

När du skapar ett Video Indexer konto kan du välja ett kostnads fritt utvärderings konto (där du får ett visst antal kostnads fria indexerings minuter) eller ett betalt alternativ (där du inte är begränsad till kvoten). Med en kostnads fri utvärderings version tillhandahåller Video Indexer upp till 600 minuter kostnads fri indexering för webbplats användare och upp till 2400 minuter kostnads fri indexering för API-användare. Med alternativet betald kan du skapa ett Video Indexer-konto som är kopplat till din Azure-prenumeration och ett Azure Media Services-konto. Du betalar för minuter som har indexerats samt de kostnader som gäller för medie kontot.

Den här artikeln visar hur du skapar ett Video Indexer-konto som är länkat till en Azure-prenumeration och ett Azure Media Services-konto. Avsnittet innehåller steg för att ansluta till Azure med hjälp av det automatiska (standard) flödet. Det visar också hur du ansluter till Azure manuellt (avancerat).

Om du flyttar från en *utvärdering* till *betald* video Indexer konto kan du välja att kopiera alla videor och modell anpassningar till det nya kontot, enligt beskrivningen i avsnittet [Importera ditt innehåll från utvärderings kontot](#import-your-content-from-the-trial-account) .

## <a name="prerequisites"></a>Krav

* En Azure-prenumeration.

    Om du ännu inte har en Azure-prenumeration kan du registrera dig för en [kostnads fri utvärderings version av Azure](https://azure.microsoft.com/free/).

* En Azure Active Directory-domän (Azure AD).

    Om du inte har en Azure AD-domän skapar du den här domänen med din Azure-prenumeration. Mer information finns i [Hantera anpassade domän namn i Azure AD](../../active-directory/users-groups-roles/domains-manage.md)

* En användare i din Azure AD-domän med rollen **program administratör** . Du använder den här medlemmen när du ansluter ditt Video Indexer-konto till Azure.

    Den här användaren bör vara en Azure AD-användare med ett arbets-eller skol konto. Använd inte ett personligt konto, till exempel outlook.com, live.com eller hotmail.com.

    ![alla AAD-användare](./media/create-account/all-aad-users.png)

### <a name="additional-prerequisites-for-automatic-flow"></a>Ytterligare krav för automatisk flöde

* En användare och medlem i din Azure AD-domän.

    Du använder den här medlemmen när du ansluter ditt Video Indexer-konto till Azure.

    Den här användaren bör vara medlem i din Azure-prenumeration med antingen en **ägar** roll eller både rollen **deltagare** och **administratör för användar åtkomst** . En användare kan läggas till två gånger, med två roller. En gång med deltagare och en gång med administratör för användar åtkomst.

    ![åtkomst kontroll](./media/create-account/access-control-iam.png)

### <a name="additional-prerequisites-for-manual-flow"></a>Ytterligare krav för manuellt flöde

* Registrera EventGrid-resurs leverantören med hjälp av Azure Portal.

    I [Azure Portal](https://portal.azure.com/)går du till **prenumerationer**-> [prenumeration]->**ResourceProviders**.

    Sök efter **Microsoft. Media** och **Microsoft. EventGrid**. Om du inte har statusen "registrerad" klickar du på **Registrera**. Det tar några minuter att registrera sig.

    ![EventGrid](./media/create-account/event-grid.png)

## <a name="connect-to-azure"></a>Anslut till Azure

> [!NOTE]
> Om din Azure-prenumeration använder certifikatbaserad Multi-Factor Authentication är det viktigt att du utför följande steg på en enhet som har de certifikat som krävs installerade.

1. Gå till [Video Indexer](https://www.videoindexer.ai/)-webbplatsen och logga in.

2. Välj knappen **Skapa nytt konto** :

    ![Skapa nytt Video Indexer konto](./media/create-account/connect-to-azure.png)

3. När listan prenumerationer visas väljer du den prenumeration som du vill använda.

    ![Ansluta Video Indexer till Azure](./media/create-account/connect-vi-to-azure-subscription.png)

4. Välj en Azure-region från de platser som stöds: USA, västra 2, Nord Europa eller Asien, östra.
5. Under **Azure Media Services konto**väljer du något av följande alternativ:

    * Om du vill skapa ett nytt Media Services konto väljer du **Skapa ny resurs grupp**. Ange ett namn för resurs gruppen.

        Azure kommer att skapa ditt nya konto i din prenumeration, inklusive ett nytt Azure Storage konto. Ditt nya Media Services-konto har en ursprunglig standard konfiguration med en slut punkt för direkt uppspelning och 10 S3-reserverade enheter.
    * Om du vill använda ett befintligt Media Services-konto väljer du **Använd befintlig resurs**. Välj ditt konto i listan konton.

        Ditt Media Services-konto måste ha samma region som ditt Video Indexer-konto.

        > [!NOTE]
        > För att minimera Indexeringens varaktighet och låg genom strömning rekommenderar vi starkt att du justerar typen och antalet [reserverade enheter](../previous/media-services-scale-media-processing-overview.md ) i ditt Media Services-konto till **10 S3-reserverade enheter**. Se [använda Portal för att ändra reserverade enheter](../previous/media-services-portal-scale-media-processing.md).

    * Om du vill konfigurera anslutningen manuellt väljer du länken **Växla till manuell konfiguration** .

        Detaljerad information finns i avsnittet [ansluta till Azure manuellt](#connect-to-azure-manually-advanced-option) (avancerat alternativ) som följer.
6. När du är klar väljer du **Anslut**. Den här åtgärden kan ta upp till några minuter.

    När du har anslutit till Azure visas ditt nya Video Indexer-konto i konto listan:

    ![nytt konto](./media/create-account/new-account.png)

7. Bläddra till det nya kontot.

## <a name="connect-to-azure-manually-advanced-option"></a>Anslut till Azure manuellt (avancerat alternativ)

Om anslutningen till Azure misslyckades kan du försöka felsöka problemet genom att ansluta manuellt.

> [!NOTE]
> Vi rekommenderar starkt att ha följande tre konton i samma region: det Video Indexer konto som du ansluter till Media Services-kontot, samt det Azure Storage-konto som är anslutet till samma Media Services-konto.

### <a name="create-and-configure-a-media-services-account"></a>Skapa och konfigurera ett Media Services konto

1. Använd [Azure](https://portal.azure.com/) Portal för att skapa ett Azure Media Services konto, enligt beskrivningen i [skapa ett konto](../previous/media-services-portal-create-account.md).

    När du skapar ett lagrings konto för ditt Media Services-konto väljer du **StorageV2** för konto Natura och **Geo-redundant (GRS)** för replikeringsalternativ.

    ![Nytt AMS-konto](./media/create-account/create-ams-account1.png)

    > [!NOTE]
    > Se till att skriva ned Media Services resurs-och konto namn. Du behöver dem för stegen i nästa avsnitt.

2. Justera typen och antalet [reserverade enheter](../previous/media-services-scale-media-processing-overview.md ) till **10 S3-reserverade enheter** i det Media Services konto som du skapade. Se [använda Portal för att ändra reserverade enheter](../previous/media-services-portal-scale-media-processing.md).
3. Innan du kan spela upp videor i Video Indexer-webbappen måste du starta standard **slut punkten för direkt uppspelning** av det nya Media Services-kontot.

    I det nya Media Services-kontot väljer du **slut punkter för direkt uppspelning**. Välj sedan slut punkten för direkt uppspelning och tryck på Starta.

    ![Nytt AMS-konto](./media/create-account/create-ams-account2.png)

4. För att Video Indexer ska kunna autentiseras med Media Services API måste en AD-App skapas. Följande steg vägleder dig genom processen för Azure AD-autentisering som beskrivs i [komma igång med Azure AD-autentisering med hjälp av Azure Portal](../previous/media-services-portal-get-started-with-aad.md):

    1. Välj **API-åtkomst**i det nya Media Services kontot.
    2. Välj [autentiseringsmetod för tjänstens huvud namn](../previous/media-services-portal-get-started-with-aad.md).
    3. Hämta klient-ID och klient hemlighet

        När du har valt **Inställningar** -> **nycklar**, Lägg till **Beskrivning**, tryck på **Spara**och nyckel värdet fylls i.

        Om nyckeln upphör att gälla måste konto ägaren kontakta Video Indexer support för att förnya nyckeln.

        > [!NOTE]
        > Se till att skriva ned nyckelvärdet och program-ID: t. Du behöver den för stegen i nästa avsnitt.

### <a name="connect-manually"></a>Anslut manuellt

I dialog rutan **anslut video Indexer till en Azure-prenumeration** på din [video Indexer](https://www.videoindexer.ai/) sida väljer du länken **Växla till manuell konfiguration** .

Ange följande information i dialog rutan:

|Inställningen|Beskrivning|
|---|---|
|Video Indexer konto region|Namnet på Video Indexer konto region. För bättre prestanda och lägre kostnader rekommenderar vi starkt att du anger namnet på den region där Azure Media Services resursen och Azure Storage kontot finns. |
|Azure AD-klient|Namnet på Azure AD-klienten, till exempel "contoso.onmicrosoft.com". Klient informationen kan hämtas från Azure Portal. Placera markören över namnet på den inloggade användaren i det övre högra hörnet. Hitta namnet till höger om **domänen**.|
|Prenumerations-ID:t|Azure-prenumerationen som den här anslutningen ska skapas under. Prenumerations-ID kan hämtas från Azure Portal. Välj **alla tjänster** i den vänstra panelen och Sök efter "prenumerationer". Välj **prenumerationer** och välj önskat ID i listan med dina prenumerationer.|
|Namn på Azure Media Services resurs grupp|Namnet på resurs gruppen där du skapade Media Services-kontot.|
|Medie tjänst resurs namn|Namnet på det Azure Media Services konto som du skapade i föregående avsnitt.|
|Program-ID|Azure AD-programmets ID (med behörigheter för det angivna Media Services kontot) som du skapade i föregående avsnitt.|
|Program nyckel|Den Azure AD-programnyckel som du skapade i föregående avsnitt. |

## <a name="import-your-content-from-the-trial-account"></a>Importera ditt innehåll från *utvärderings* kontot

När du [skapar ett nytt konto](#connect-to-azure)har du ett alternativ för att importera innehållet från *utvärderings* kontot till det nya kontot. Om du markerar alternativet *Importera* i dialog rutan **skapa ett nytt konto i en Azure-prenumeration** kopieras alla anpassningar av medie-och innehålls modeller från *utvärderings* kontot till det nya kontot.

Möjligheten att importera innehållet är giltig för både automatiserade och manuella metoder som beskrivs ovan.

> [!NOTE]
> Innehållet kan bara importeras en gång från varje konto.

## <a name="considerations"></a>Att tänka på

Följande Azure Media Services relaterade överväganden gäller:

* Om du ansluter automatiskt visas en ny resurs grupp, Media Services konto och ett lagrings konto i din Azure-prenumeration.
* Om du ansluter automatiskt, anger Video Indexer de medie **reserverade enheterna** till 10 S3-enheter:

    ![Media Services reserverade enheter](./media/create-account/ams-reserved-units.png)

* Om du ansluter till ett befintligt Media Services-konto kan Video Indexer inte ändra konfigurationen för den befintliga enheten för **reserverade enheter** .

   Du kan behöva justera typ och antal enheternas reserverade enheter enligt den planerade belastningen. Tänk på att om belastningen är hög och du inte har tillräckligt med enheter eller hastighet kan video bearbetningen leda till timeout-problem.

* Om du ansluter till ett nytt Media Services konto startar Video Indexer automatiskt standard **slut punkten för direkt uppspelning** i den:

    ![Slut punkt för Media Services strömning](./media/create-account/ams-streaming-endpoint.png)

    Slut punkter för direkt uppspelning har en betydande start tid. Det kan därför ta flera minuter från det att du anslöt ditt konto till Azure tills dina videor kan strömmas och bevakas i Video Indexer webbappen.

* Om du ansluter till ett befintligt Media Services konto kan Video Indexer inte ändra standard konfigurationen för strömnings slut punkten. Om det inte finns någon **slut punkt för direkt uppspelning**kan du inte titta på videor från det här Media Services kontot eller i video Indexer.

## <a name="next-steps"></a>Nästa steg

Du kan interagera med ditt utvärderings konto och/eller med dina Video Indexer-konton som är anslutna till Azure genom att följa anvisningarna i: [använda API: er](video-indexer-use-apis.md).

Du bör använda samma Azure AD-användare som du använde när du anslöt till Azure.
