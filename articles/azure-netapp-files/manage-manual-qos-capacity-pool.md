---
title: Hantera en manuell pool för QoS-kapacitet för Azure NetApp Files | Microsoft Docs
description: Beskriver hur du hanterar en pool som använder den manuella QoS-typen, inklusive hur du konfigurerar en manuell pool för QoS-kapacitet och hur du ändrar en pool med manuell QoS.
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
ms.topic: how-to
ms.date: 09/22/2020
ms.author: b-juche
ms.openlocfilehash: 1bbd6ad9e33aab8d3564865b86485d70df5a108f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91345509"
---
# <a name="manage-a-manual-qos-capacity-pool"></a>Hantera en manuell pool för QoS-kapacitet

Den här artikeln beskriver hur du hanterar en pool som använder den manuella QoS-typen.  

Se [Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md) -och [prestanda överväganden för Azure NetApp Files](azure-netapp-files-performance-considerations.md) för att förstå överväganden om QoS-typer.  

## <a name="register-the-feature"></a>Registrera funktionen
Funktionen för manuell QoS-typ är för närvarande en för hands version. Om du använder den här funktionen för första gången måste du registrera funktionen först.
  
1.  Registrera funktionen:

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFFlexPool
    ```

2. Kontrol lera status för funktions registreringen: 

    > [!NOTE]
    > **RegistrationState** kan vara i ett `Registering` tillstånd i upp till 60 minuter innan den ändras till `Registered` . Vänta tills statusen har **registrerats** innan du fortsätter.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFFlexPool
    ```
Du kan också använda [Azure CLI-kommandon](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest&preserve-view=true) `az feature register` och `az feature show` Registrera funktionen och Visa registrerings status. 

## <a name="set-up-a-new-manual-qos-capacity-pool"></a>Konfigurera en ny pool för manuell QoS-kapacitet 

För att skapa en ny kapacitets uppsättning med hjälp av den manuella QoS-typen:

1. Följ stegen i [Konfigurera en pool för kapacitet](azure-netapp-files-set-up-capacity-pool.md).  

2. I fönstret ny kapacitets pool väljer du typ av **manuell QoS** .  

## <a name="change-a-capacity-pool-to-use-manual-qos"></a><a name="change-to-qos"></a>Ändra en pool för kapacitet att använda manuell QoS

Du kan ändra en pool som för närvarande använder den automatiska QoS-typen för att använda den manuella QoS-typen.  

> [!IMPORTANT]
> Att ställa in kapacitets typen till manuell QoS är en permanent ändring. Du kan inte konvertera ett manuellt kapacitets verktyg för QoS-typ till en pool med automatisk QoS-kapacitet. 

1. Från hanterings bladet för ditt NetApp-konto klickar du på **kapacitets grupper** för att visa befintliga kapacitets grupper.   
 
2.  Klicka på den kapacitets uppsättning som du vill ändra för att använda manuell QoS.

3.  Klicka på **ändra QoS-typ**. Ange sedan **ny QoS-typ** till **manuell**. Klicka på **OK**. 

![Ändra QoS-typ](../media/azure-netapp-files/change-qos-type.png)


## <a name="monitor-the-throughput-of-a-manual-qos-capacity-pool"></a>Övervaka data flödet för en manuell pool med QoS-kapacitet  

Mått är tillgängliga som hjälper dig att övervaka Läs-och Skriv data flödet för en volym.  Se [mått för Azure NetApp Files](azure-netapp-files-metrics.md).  

## <a name="modify-the-allotted-throughput-of-a-manual-qos-volume"></a>Ändra det tilldelade data flödet för en manuell QoS-volym 

Om en volym ingår i en manuell pool för QoS-kapacitet kan du ändra den tilldelade volymens data flöde vid behov.

1. På sidan **volymer** väljer du den volym vars data flöde du vill ändra.   

2. Klicka på **ändra data flöde**. Ange det **data flöde (MIB/S)** som du vill använda. Klicka på **OK**. 

    ![Ändra QoS-dataflöde](../media/azure-netapp-files/change-qos-throughput.png)

## <a name="next-steps"></a>Nästa steg  

* [Konfigurera en kapacitetspool](azure-netapp-files-set-up-capacity-pool.md)
* [Mått för Azure NetApp Files](azure-netapp-files-metrics.md)
* [Saker att tänka på gällande prestanda för Azure NetApp Files](azure-netapp-files-performance-considerations.md)
* [Felsök problem med kapacitets pooler](troubleshoot-capacity-pools.md)
