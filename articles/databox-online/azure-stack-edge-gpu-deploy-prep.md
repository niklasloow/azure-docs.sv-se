---
title: Självstudie för att förbereda Azure Portal Data Center miljö för att distribuera Azure Stack Edge Pro GPU | Microsoft Docs
description: Den första självstudien om att distribuera Azure Stack Edge Pro GPU innebär att du förbereder Azure Portal.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: tutorial
ms.date: 09/08/2020
ms.author: alkohli
Customer intent: As an IT admin, I need to understand how to prepare the portal to deploy Azure Stack Edge Pro so I can use it to transfer data to Azure.
ms.openlocfilehash: cf7719487d4f03b8d9524234e1a58cf792a4843b
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/22/2020
ms.locfileid: "90899757"
---
# <a name="tutorial-prepare-to-deploy-azure-stack-edge-pro-with-gpu"></a>Självstudie: förbereda för att distribuera Azure Stack Edge Pro med GPU 

Det här är den första självstudien i serien med distributions kurser som krävs för att helt Distribuera Azure Stack Edge Pro med GPU. I den här självstudien beskrivs hur du förbereder Azure Portal för att distribuera en Azure Stack Edge-resurs.

Du måste ha administratörsbehörighet för att utföra installationen och konfigurationen. Portalförberedelserna tar mindre än tio minuter.

I den här guiden får du lära dig att:

> [!div class="checklist"]
> * Skapa en ny resurs
> * Hämta aktiveringsnyckeln

### <a name="get-started"></a>Kom igång

För Azure Stack Edge Pro-distribution måste du först förbereda din miljö. När miljön är klar följer du de steg som krävs och om det behövs, valfria steg och procedurer för att distribuera enheten fullständigt. Anvisningarna för steg-för-steg-distribution anger när du bör utföra var och en av dessa obligatoriska och valfria steg.

| Steg | Beskrivning |
| --- | --- |
| **Förberedelse** |De här stegen måste utföras i förberedelser inför den kommande distributionen. |
| **[Check lista för distributions konfiguration](#deployment-configuration-checklist)** |Använd den här checklistan för att samla in och registrera information före och under distributionen. |
| **[Distributions krav](#prerequisites)** |När de här kraven är uppfyllda är miljön klar för distribution. |
|  | |
|**Distributions självstudier** |De här självstudierna krävs för att distribuera din Azure Stack Edge Pro-enhet i produktion. |
|**[1. Förbered Azure Portal för Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-prep.md)** |Skapa och konfigurera din Azure Stack Edge-resurs innan du installerar en fysisk enhet i Azure Stack Box Edge. |
|**[2. Installera Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-install.md)**|Packa upp, racka och kablar Azure Stack fysiska enheten för Edge Pro.  |
|**[3. Anslut till Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-connect.md)** |När enheten har installerats ansluter du till enhetens lokala webb gränssnitt.  |
|**[4. Konfigurera nätverks inställningar för Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-configure-network-compute-web-proxy.md)** |Konfigurera nätverket inklusive inställningar för beräknings nätverk och webbproxy för enheten.   |
|**[5. Konfigurera enhets inställningar för Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-set-up-device-update-time.md)** |Tilldela ett enhets namn och en DNS-domän, konfigurera uppdaterings Server och enhets tid. |
|**[6. Konfigurera säkerhets inställningar för Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-configure-certificates.md)** |Konfigurera certifikat för din enhet. Använd enhets genererade certifikat eller ta med dina egna certifikat.   |
|**[7. Aktivera Azure Stack Edge Pro](azure-stack-edge-gpu-deploy-activate.md)** |Använd aktiverings nyckeln från tjänsten för att aktivera enheten. Enheten är redo att konfigurera SMB-eller NFS-resurser eller ansluta via REST. |
|**[8. Konfigurera Compute](azure-stack-edge-gpu-deploy-configure-compute.md)** |Konfigurera beräknings rollen på enheten. Detta kommer också att skapa ett Kubernetes-kluster. |
|**[9a. Överföra data med gräns resurser](azure-stack-edge-j-series-deploy-add-shares.md)** |Lägg till resurser och anslut till resurser via SMB eller NFS. |
|**[9b. Överföra data med gräns lagrings konton](azure-stack-edge-j-series-deploy-add-storage-accounts.md)** |Lägg till lagrings konton och Anslut till Blob Storage via REST-API: er. |


Nu kan du börja samla in information om program varu konfigurationen för din Azure Stack Edge Pro-enhet.

## <a name="deployment-configuration-checklist"></a>Checklista för distributionskonfiguration

Innan du distribuerar enheten måste du samla in information för att konfigurera program varan på din Azure Stack Edge Pro-enhet. Att förbereda en del av den här informationen i förväg bidrar till att effektivisera processen att distribuera enheten i din miljö. Använd [Check listan Azure Stack Edge Pro Deployment Configuration](azure-stack-edge-gpu-deploy-checklist.md) för att anteckna konfigurations informationen när du distribuerar enheten.


## <a name="prerequisites"></a>Förutsättningar

Följande är konfigurations kraven för din Azure Stack Edge-resurs, din Azure Stack Edge-enhet och data Center nätverket.

### <a name="for-the-azure-stack-edge-resource"></a>För Azure Stack Edge-resursen

Innan du börjar ska du kontrollera att:

- Din Microsoft Azure prenumeration är aktive rad för en Azure Stack Edge-resurs. Se till att du har använt en prenumeration som stöds, till exempel [Microsoft Enterprise-avtal (EA)](https://azure.microsoft.com/overview/sales-number/), [Cloud Solution Provider (CSP)](https://docs.microsoft.com/partner-center/azure-plan-lp)eller [Microsoft Azure-sponsring](https://azure.microsoft.com/offers/ms-azr-0036p/). Prenumerationer med principen betala per användning stöds inte.
- Du har ägar-eller deltagar åtkomst på resurs grupps nivå för Azure Stack Edge Pro/Data Box Gateway, IoT Hub och Azure Storage resurser.

    - Om du vill skapa en Azure Stack gräns-/Data Box Gateway-resurs, bör du ha behörighet som deltagare (eller högre) som är begränsade till resurs grupps nivå. Du måste också kontrol lera att `Microsoft.DataBoxEdge` providern är registrerad. Information om hur du registrerar den finns i [Registrera resursprovider](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers).
    - Om du vill skapa en IoT Hub resurs måste du kontrol lera att Microsoft. providers-providern är registrerad. Information om hur du registrerar den finns i [Registrera resursprovider](azure-stack-edge-manage-access-power-connectivity-mode.md#register-resource-providers).
    - Om du vill skapa en lagrings konto resurs måste du igen med deltagar-eller högre åtkomst omfång på resurs grupps nivå. Azure Storage är som standard en registrerad resurs leverantör.
- Du har administratörs-eller användar åtkomst till Azure Active Directory Graph API. Mer information finns i [Azure Active Directory Graph API](https://docs.microsoft.com/previous-versions/azure/ad/graph/howto/azure-ad-graph-api-permission-scopes#default-access-for-administrators-users-and-guest-users-).
- Du har ditt Microsoft Azure lagringskonto med autentiseringsuppgifter.

### <a name="for-the-azure-stack-edge-pro-device"></a>För Azure Stack Edge Pro-enhet

Innan du distribuerar en fysisk enhet kontrollerar du att:

- Du har granskat säkerhets informationen som ingick i försändelse paketet.
- Du har en 1U-plats som är tillgänglig i ett standardiserat 19-tums rack i ditt data Center för rack montering av enheten.
- Du har tillgång till en plan, stabil och jämn arbetsyta där enheten kan stå på ett säkert sätt.
- Platsen där du tänker konfigurera enheten har standardnätström från en oberoende källa eller en strömfördelare på racket (PDU) med en avbrottsfri kraftfälla (UPS).
- Du har tillgång till en fysisk enhet.


### <a name="for-the-datacenter-network"></a>För datacenternätverket

Innan du börjar ska du kontrollera att:

- Nätverket i data centret konfigureras enligt nätverks kraven för din Azure Stack Edge Pro-enhet. Mer information finns i [Azure Stack Edge Pro system-krav](azure-stack-edge-system-requirements.md).

- För normala drift villkor för Azure Stack Edge Pro har du:

    - Minst 10 Mbit/s Ladda ned bandbredd för att se till att enheten förblir uppdaterad.
    - Minst 20 Mbit/s dedikerad överföring och nedladdning av bandbredd för överföring av filer.

## <a name="create-a-new-resource"></a>Skapa en ny resurs

Om du har en befintlig Azure Stack Edge-resurs för att hantera din fysiska enhet kan du hoppa över det här steget och gå till [Hämta aktiverings nyckeln](#get-the-activation-key).

För att skapa en Azure Stack Edge-resurs, utför följande steg i Azure Portal.

1. Använd dina Microsoft Azure autentiseringsuppgifter för att logga in på Azure Portal på denna URL: [https://portal.azure.com](https://portal.azure.com) .

2. I den vänstra rutan väljer du **+ skapa en resurs**. Sök efter och välj **Azure Stack gräns/data Box Gateway**. Välj **Skapa**. 

3. Välj den prenumeration som du vill använda för Azure Stack Edge Pro-enheten. Välj det land där du vill skicka den här fysiska enheten. Välj **Visa enheter**.

    ![Skapa en resurs 1](media/azure-stack-edge-gpu-deploy-prep/create-resource-1.png)

4. Välj enhets typ. Under **Azure Stack Edge Pro**väljer du **Azure Stack Edge Pro med GPU** och väljer sedan **Välj**. Om du ser några problem eller inte kan välja enhets typ går du till [Felsöka beställnings problem](azure-stack-edge-troubleshoot-ordering.md).

    ![Skapa en resurs 3](media/azure-stack-edge-gpu-deploy-prep/create-resource-3.png)

5. Utifrån dina affärs behov kan du välja Azure Stack Edge Pro med 1 eller 2 grafik processorer (GPU: er) från NVIDIA. 

    ![Skapa en resurs 4](media/azure-stack-edge-gpu-deploy-prep/create-resource-4.png)

6. På fliken **grundläggande** anger eller väljer du följande **projekt information**.
    
    |Inställningen  |Värde  |
    |---------|---------|
    |Prenumeration    |Detta fylls i automatiskt baserat på den tidigare markeringen. Prenumerationen är kopplad till ditt faktureringskonto. |
    |Resursgrupp  |Välj en befintlig grupp eller skapa en ny grupp.<br>Lär dig mer om [Azures resurs grupper](../azure-resource-manager/resource-group-overview.md).     |

7. Ange eller Välj följande **instans information**.

    |Inställningen  |Värde  |
    |---------|---------|
    |Namn   | Ett eget namn som identifierar resursen.<br>Namnet innehåller mellan 2 och 50 tecken som består av bokstäver, siffror och bindestreck.<br> Namnet börjar och slutar med en bokstav eller en siffra.        |
    |Region     |För en lista över alla regioner där Azure Stack Edge-resursen är tillgänglig, se [Azure-produkter tillgängliga per region](https://azure.microsoft.com/global-infrastructure/services/?products=databox&regions=all). Om du använder Azure Government är alla myndigheter tillgängliga som de visas i Azure- [regionerna](https://azure.microsoft.com/global-infrastructure/regions/).<br> Välj den plats som är närmast den geografiska region där du vill distribuera enheten.|

    ![Skapa en resurs 5](media/azure-stack-edge-gpu-deploy-prep/create-resource-5.png)


8. Välj **Nästa: leverans adress**.

    - Om du redan har en enhet väljer du kombinations rutan för **Jag har en Azure Stack Edge Pro-enhet**.

        ![Skapa en resurs 6](media/azure-stack-edge-gpu-deploy-prep/create-resource-6.png)

    - Om det här är den nya enhet som du beställer anger du kontakt namn, företag, adress för att leverera enheten och kontakt information.

        ![Skapa en resurs 7](media/azure-stack-edge-gpu-deploy-prep/create-resource-7.png)

9. Välj **Nästa: Taggar**. Du kan också ange taggar för att kategorisera resurser och konsolidera fakturering. Välj **Nästa: Granska + skapa**.

10. På fliken **Granska + skapa** granskar du **pris informationen**, **användningsvillkor**och informationen för resursen. Välj kombinations rutan för **Jag har granskat sekretess villkoren**.

    ![Skapa en resurs 8](media/azure-stack-edge-gpu-deploy-prep/create-resource-8.png)

11. Välj **Skapa**.

Det tar några minuter att skapa resursen. När resursen har skapats och distribuerats får du ett meddelande. Välj **Gå till resurs**.

![Gå till Azure Stack Edge Pro-resursen](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-1.png)

När ordern har placerats, granskar Microsoft ordern och når dig (via e-post) med leverans information.

<!--![Notification for review of the Azure Stack Edge Pro order](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-2.png)-->

Om du stöter på problem under beställnings processen går du till [Felsöka beställnings problem](azure-stack-edge-troubleshoot-ordering.md).

## <a name="get-the-activation-key"></a>Hämta aktiveringsnyckeln

När Azure Stack Edge-resursen är igång måste du hämta aktiverings nyckeln. Den här nyckeln används för att aktivera och ansluta din Azure Stack Edge Pro-enhet med resursen. Du kan hämta den här nyckeln nu när du befinner dig på Azure-portalen.

1. Välj den resurs som du har skapat. Välj **Översikt** och välj sedan **enhets konfiguration**.

    ![Välj enhets konfiguration](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-2.png)

2. På panelen **Aktivera** väljer du **generera nyckel** för att skapa en aktiverings nyckel. Välj kopieringsikonen för att kopiera nyckeln och spara den för senare användning.

    ![Hämta aktiveringsnyckeln](media/azure-stack-edge-gpu-deploy-prep/azure-stack-edge-resource-3.png)

> [!IMPORTANT]
> - Aktiveringsnyckeln upphör att gälla tre dagar efter att den skapats.
> - Om nyckeln har upphört att gälla genererar du en ny nyckel. Den äldre nyckeln är inte giltig.

## <a name="next-steps"></a>Nästa steg

I den här självstudien har du lärt dig om Azure Stack Edge Pro-ämnen som:

> [!div class="checklist"]
> * Skapa en ny resurs
> * Hämta aktiveringsnyckeln

Gå vidare till nästa självstudie och lär dig hur du installerar Azure Stack Edge Pro.

> [!div class="nextstepaction"]
> [Installera Azure Stack Edge Pro](./azure-stack-edge-gpu-deploy-install.md)



