---
title: Resurs gränser för Azure NetApp Files | Microsoft Docs
description: Beskriver gränser för Azure NetApp Files resurser och hur du begär ökning av resurs gränsen.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 9/16/2020
ms.author: b-juche
ms.openlocfilehash: 0ddb9998c1e1b9b70303aeb4608bc0b53bc103ae
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325495"
---
# <a name="resource-limits-for-azure-netapp-files"></a>Resursbegränsningar för Azure NetApp Files

Att förstå resurs gränser för Azure NetApp Files hjälper dig att hantera dina volymer.

## <a name="resource-limits"></a>Resursbegränsningar

I följande tabell beskrivs resurs gränser för Azure NetApp Files:

|  Resurs  |  Standardgräns  |  Justerbar via supportbegäran  |
|----------------|---------------------|--------------------------------------|
|  Antal NetApp-konton per Azure-region   |  10    |  Yes   |
|  Antal kapacitets pooler per NetApp-konto   |    25     |   Yes   |
|  Antal volymer per prenumeration   |    500     |   Yes   |
|  Antal volymer per kapacitets grupp     |    500   |    Yes     |
|  Antal ögonblicks bilder per volym       |    255     |    No        |
|  Antal undernät som har delegerats till Azure NetApp Files (Microsoft. NetApp/Volumes) per Azure-Virtual Network    |   1   |    No    |
|  Antal använda IP-adresser i ett VNet (inklusive direkt peer-virtuella nätverk) med Azure NetApp Files   |    1000   |    No   |
|  Minsta storlek på en pool med enskild kapacitet   |  4 TiB     |    No  |
|  Maximal storlek för en pool med enskild kapacitet    |  500 TiB   |   No   |
|  Minsta storlek på en enskild volym    |    100 GiB    |    No    |
|  Maximal storlek på en enskild volym     |    100 TiB    |    No    |
|  Maximal storlek för en enskild fil     |    16 TiB    |    No    |    
|  Maximal storlek på katalogens metadata i en enskild katalog      |    320 MB    |    No    |    
|  Maximalt antal filer ([maxfiles](#maxfiles)) per volym     |    100 000 000    |    Yes    |    
|  Lägsta tilldelade data flöde för en manuell QoS-volym     |    1 MiB/s   |    No    |    
|  Maximalt kopplat data flöde för en manuell QoS-volym     |    4 500 MiB/s    |    No    |    
|  Antal data skydds volymer för replikering mellan regioner (mål volymer)     |    5    |    Yes    |     

Mer information finns i [vanliga frågor och svar om kapacitets hantering](azure-netapp-files-faqs.md#capacity-management-faqs).

## <a name="maxfiles-limits"></a>Maxfiles-gränser <a name="maxfiles"></a> 

Azure NetApp Files volymer har en gräns som kallas *maxfiles*. Gränsen för maxfiles är antalet filer som en volym kan innehålla. Maxfiles-gränsen för en Azure NetApp Files volym indexeras baserat på volymens storlek (kvot). Maxfiles-gränsen för en volym ökar eller minskar med hastigheten på 20 000 000 filer per TiB med etablerad volym storlek. 

Tjänsten justerar dynamiskt maxfiles-gränsen för en volym baserat på dess etablerade storlek. Till exempel skulle en volym som har kon figurer ATS ursprungligen med en storlek på 1 TiB ha en maxfiles gräns på 20 000 000. Efterföljande ändringar av volymens storlek skulle leda till automatisk omanpassning av maxfiles-gränsen baserat på följande regler: 

|    Volym storlek (kvot)     |  Automatisk justering av maxfiles-gränsen    |
|----------------------------|-------------------|
|    <= 1 TiB                |    20 000 000     |
|    > 1 TiB men <= 2 TiB    |    40 000 000     |
|    > 2 TiB men <= 3 TiB    |    60 000 000     |
|    > 3 TiB men <= 4 TiB    |    80 000 000     |
|    > 4 TiB                 |    100 000 000    |

Om du redan har tilldelat minst 4 TiB av kvoten för en volym kan du initiera en [support förfrågan](#limit_increase) för att öka maxfiles-gränsen bortom 100 000 000.

## <a name="request-limit-increase"></a>Begär gräns ökning <a name="limit_increase"></a> 

Du kan skapa en support förfrågan för Azure för att öka de justerbara gränserna från tabellen ovan. 

Från Azure Portal navigerings plan: 

1. Klicka på **Hjälp + Support**.
2. Klicka på **+ ny supportbegäran**.
3. Ange följande information på fliken grundläggande: 
    1. Typ av problem: Välj **begränsningar för tjänsten och prenumerationen (kvoter)**.
    2. Prenumerationer: Välj prenumerationen för den resurs du behöver för att öka kvoten.
    3. Kvot typ: Välj **lagring: Azure NetApp Files gränser**.
    4. Klicka på **Nästa: lösningar**.
4. På fliken information:
    1. I rutan Beskrivning anger du följande information för motsvarande resurs typ:

        |  Resurs  |    Överordnade resurser      |    Begärda nya gränser     |    Orsak till kvotökning       |
        |----------------|------------------------------|---------------------------------|------------------------------------------|
        |  Konto |  *Prenumerations-ID*   |  *Begärt nytt maximalt **konto** nummer*    |  *Vilket scenario eller vilka användnings fall ber begäran?*  |
        |  Pool    |  *Prenumerations-ID, NetApp-konto-URI*  |  *Begärt nytt nummer för högsta **pool***   |  *Vilket scenario eller vilka användnings fall ber begäran?*  |
        |  Volym  |  *Prenumerations-ID, NetApp-konto-URI, URI för kapacitets pool*   |  *Begärt nytt maximalt **volym** nummer*     |  *Vilket scenario eller vilka användnings fall ber begäran?*  |
        |  Maxfiles  |  *Prenumerations-ID, NetApp-konto-URI, URI för kapacitets pool, volym-URI*   |  *Begärt nytt maximalt **maxfiles** -nummer*     |  *Vilket scenario eller vilka användnings fall ber begäran?*  |    
        |  Replikering av data skydds volymer över flera regioner  |  *Prenumerations-ID, mål NetApp konto-URI, URI för mål kapacitets grupp, käll NetApp konto-URI, URI för käll kapacitets pool, käll volym-URI*   |  *Begärt nytt maximalt antal **data skydds volymer för replikering mellan regioner (mål volymer)***     |  *Vilket scenario eller vilka användnings fall ber begäran?*  |    

    2. Ange lämplig support metod och ange din kontrakts information.

    3. Klicka på **Nästa: granska + skapa** för att skapa begäran. 


## <a name="next-steps"></a>Nästa steg  

- [Förstå lagringshierarkin för Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
- [Kostnadsmodell för Azure NetApp Files](azure-netapp-files-cost-model.md)
