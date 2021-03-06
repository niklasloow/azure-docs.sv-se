---
title: Hantera ögonblicks bilder med Azure NetApp Files | Microsoft Docs
description: Beskriver hur du skapar och hanterar ögonblicks bilder med hjälp av Azure NetApp Files.
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
ms.date: 09/04/2020
ms.author: b-juche
ms.openlocfilehash: e1be0879af02fac0f7ae926a02ea23fd6be84de4
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325697"
---
# <a name="manage-snapshots-by-using-azure-netapp-files"></a>Hantera ögonblicksbilder med hjälp av Azure NetApp Files

Azure NetApp Files har stöd för att skapa ögonblicks bilder och använda ögonblicks bilder för att schemalägga automatisk skapande av ögonblicks bilder.  Du kan också återställa en ögonblicks bild till en ny volym eller återställa en enskild fil med hjälp av en klient.  

## <a name="create-an-on-demand-snapshot-for-a-volume"></a>Skapa en ögonblicks bild på begäran för en volym

Du kan skapa ögonblicks bilder av volymen på begäran. 

1.  Gå till den volym som du vill skapa en ögonblicks bild för. Klicka på **ögonblicks bilder**.

    ![Navigera till ögonblicks bilder](../media/azure-netapp-files/azure-netapp-files-navigate-to-snapshots.png)

2.  Klicka på **+ Lägg till ögonblicks bild** för att skapa en ögonblicks bild på begäran för en volym.

    ![Lägg till ögonblicks bild](../media/azure-netapp-files/azure-netapp-files-add-snapshot.png)

3.  I fönstret ny ögonblicks bild anger du ett namn för den nya ögonblicks bilden som du skapar.   

    ![Ny ögonblicks bild](../media/azure-netapp-files/azure-netapp-files-new-snapshot.png)

4. Klicka på **OK**. 

## <a name="manage-snapshot-policies"></a>Hantera ögonblicks bilds principer

Du kan schemalägga att ögonblicks bilder av volymen ska tas automatiskt med hjälp av ögonblicks bilds principer. Du kan också ändra en ögonblicks bild princip efter behov eller ta bort en ögonblicks bild princip som du inte längre behöver.  

### <a name="register-the-feature"></a>Registrera funktionen

Funktionen för **ögonblicks bilds principen** är för närvarande en för hands version. Om du använder den här funktionen för första gången måste du registrera funktionen först. 

1. Registrera funktionen: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```

2. Kontrol lera status för funktions registreringen: 

    > [!NOTE]
    > **RegistrationState** kan vara i ett `Registering` tillstånd i upp till 60 minuter innan den ändras till `Registered` . Vänta tills statusen har **registrerats** innan du fortsätter.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFSnapshotPolicy
    ```
Du kan också använda [Azure CLI-kommandon](https://docs.microsoft.com/cli/azure/feature?view=azure-cli-latest&preserve-view=true) `az feature register` och `az feature show` Registrera funktionen och Visa registrerings status. 

### <a name="create-a-snapshot-policy"></a>Skapa en ögonblicks bild princip 

Med en ögonblicks bild princip kan du ange frekvensen för ögonblicks bild skapande i varje timme, varje dag, varje vecka eller månads vis. Du måste också ange det maximala antal ögonblicks bilder som ska behållas för volymen.  

1.  I vyn NetApp konto klickar du på **ögonblicks bild princip**.

    ![Navigering för ögonblicks bild princip](../media/azure-netapp-files/snapshot-policy-navigation.png)

2.  I fönstret ögonblicks bild princip anger du princip tillstånd till **aktive rad**. 

3.  Klicka på fliken varje **timme**, **varje dag**, **varje vecka**eller varje **månad** för att skapa dagliga, dagliga, vecko Visa eller månatliga ögonblicks bilds principer. Ange det **antal ögonblicks bilder som ska behållas**.  

    Se [resurs gränser för Azure NetApp Files](azure-netapp-files-resource-limits.md) om det maximala antalet ögonblicks bilder som tillåts för en volym. 

    I följande exempel visas konfiguration av Tim ögonblicks bild princip. 

    ![Ögonblicks bild princip varje timme](../media/azure-netapp-files/snapshot-policy-hourly.png)

    I följande exempel visas daglig princip konfiguration för ögonblicks bilder.

    ![Ögonblicks bild princip varje dag](../media/azure-netapp-files/snapshot-policy-daily.png)

    I följande exempel visas konfigurationen för vecko ögonblicks bilds principer.

    ![Ögonblicks bild princip varje vecka](../media/azure-netapp-files/snapshot-policy-weekly.png)

    I följande exempel visas månatlig konfiguration av ögonblicks bilds principen.

    ![Ögonblicks bild princip varje månad](../media/azure-netapp-files/snapshot-policy-monthly.png) 

4.  Klicka på **Spara**.  

Om du behöver skapa ytterligare ögonblicks bilds principer upprepar du steg 3.
De principer som du har skapat visas på sidan princip för ögonblicks bild.

Om du vill att en volym ska använda ögonblicks bild principen måste du [tillämpa principen på volymen](azure-netapp-files-manage-snapshots.md#apply-a-snapshot-policy-to-a-volume). 

### <a name="apply-a-snapshot-policy-to-a-volume"></a>Tillämpa en ögonblicks bild princip på en volym

Om du vill att en volym ska använda en ögonblicks bilds princip som du har skapat måste du tillämpa principen på volymen. 

1.  Gå till sidan **volymer** , högerklicka på den volym som du vill tillämpa en ögonblicks bild princip på och välj **Redigera**.

    ![Volymer på snabb menyn för volymer](../media/azure-netapp-files/volume-right-cick-menu.png) 

2.  I redigerings fönstret, under **ögonblicks bild princip**, väljer du en princip som ska användas för volymen.  Klicka på **OK** för att tillämpa principen.  

    ![Redigera ögonblicks redigerings princip](../media/azure-netapp-files/snapshot-policy-edit.png) 

### <a name="modify-a-snapshot-policy"></a>Ändra en ögonblicks bild princip 

Du kan ändra en befintlig ögonblicks bild princip om du vill ändra principens tillstånd, frekvens för ögonblicks bilder (varje timme, varje dag, varje vecka eller månad) eller antal ögonblicks bilder som ska behållas.  
 
1.  I vyn NetApp konto klickar du på **ögonblicks bild princip**.

2.  Högerklicka på den ögonblicks bilds princip som du vill ändra och välj sedan **Redigera**.

    ![Ögonblicks bild princip, snabb meny](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Gör ändringarna i fönstret princip för ögonblicks bild som visas och klicka sedan på **Spara**. 

### <a name="delete-a-snapshot-policy"></a>Ta bort en ögonblicks bild princip 

Du kan ta bort en ögonblicks bilds princip som du inte längre vill behålla.   

1.  I vyn NetApp konto klickar du på **ögonblicks bild princip**.

2.  Högerklicka på den ögonblicks bilds princip som du vill ändra och välj sedan **ta bort**.

    ![Ögonblicks bild princip, snabb meny](../media/azure-netapp-files/snapshot-policy-right-click-menu.png) 

3.  Klicka på **Ja** för att bekräfta att du vill ta bort ögonblicks bild principen.   

    ![Bekräftelse av borttagning av ögonblicks princip](../media/azure-netapp-files/snapshot-policy-delete-confirm.png) 

## <a name="restore-a-snapshot-to-a-new-volume"></a>Återställa en ögonblicks bild till en ny volym

För närvarande kan du bara återställa en ögonblicks bild till en ny volym. 
1. Välj **ögonblicks bilder** från volym bladet för att Visa ögonblicks bild listan. 
2. Högerklicka på den ögonblicks bild som du vill återställa och välj **Återställ till ny volym** från meny alternativet.  

    ![Återställ ögonblicks bild till ny volym](../media/azure-netapp-files/azure-netapp-files-snapshot-restore-to-new-volume.png)

3. Ange information för den nya volymen i fönstret Skapa en volym:  
    * **Namn**   
        Ange namnet på den volym du skapar.  
        
        Namnet måste vara unikt inom sin resursgrupp. Det måste innehålla minst tre tecken.  Du kan använda alla alfanumeriska tecken.

    * **Kvot**  
        Ange mängden logisk lagring som du vill tilldela till volymen.  

    ![Återställ till ny volym](../media/azure-netapp-files/snapshot-restore-new-volume.png) 

4. Klicka på **Granska + skapa**.  Klicka på **Skapa**.   
    Den nya volymen använder samma protokoll som ögonblicks bilden använder.   
    Den nya volymen som ögonblicks bilden återställs till visas på bladet volymer.

## <a name="restore-a-file-from-a-snapshot-using-a-client"></a>Återställa en fil från en ögonblicks bild med hjälp av en klient

Om du inte vill [återställa hela ögonblicks bilden till en volym](#restore-a-snapshot-to-a-new-volume)kan du välja att återställa en fil från en ögonblicks bild med hjälp av en klient som har volymen monterad.  

Den monterade volymen innehåller en ögonblicks bilds katalog med namnet  `.snapshot` (i NFS-klienter) eller `~snapshot` (i SMB-klienter) som är tillgänglig för klienten. Katalogen för ögonblicks bilder innehåller under kataloger som motsvarar ögonblicks bilderna av volymen. Varje under katalog innehåller filerna i ögonblicks bilden. Om du av misstag tar bort eller skriver över en fil, kan du återställa filen till den överordnade katalogen för skriv skydd genom att kopiera filen från en under katalog i ögonblicks bild till katalogen för Läs-och skriv åtgärder. 

Om du har markerat kryss rutan Dölj ögonblicks bilds Sök väg när du skapade volymen döljs ögonblicks bild katalogen. Du kan visa status för Dölj ögonblicks bilds Sök väg för volymen genom att välja volymen. Du kan redigera alternativet Dölj ögonblicks bild Sök väg genom att klicka på **Redigera** på volymens sida.  

![Redigera alternativ för ögonblicks bild av volym](../media/azure-netapp-files/volume-edit-snapshot-options.png) 

### <a name="restore-a-file-by-using-a-linux-nfs-client"></a>Återställa en fil med hjälp av en Linux NFS-klient 

1. Använd `ls` Linux-kommandot för att visa en lista över filen som du vill återställa från `.snapshot` katalogen. 

    Exempel:

    `$ ls my.txt`   
    `ls: my.txt: No such file or directory`   

    `$ ls .snapshot`   
    `daily.2020-05-14_0013/              hourly.2020-05-15_1106/`   
    `daily.2020-05-15_0012/              hourly.2020-05-15_1206/`   
    `hourly.2020-05-15_1006/             hourly.2020-05-15_1306/`   

    `$ ls .snapshot/hourly.2020-05-15_1306/my.txt`   
    `my.txt`

2. Använd `cp` kommandot för att kopiera filen till den överordnade katalogen.  

    Exempel: 

    `$ cp .snapshot/hourly.2020-05-15_1306/my.txt .`   

    `$ ls my.txt`   
    `my.txt`   

### <a name="restore-a-file-by-using-a-windows-client"></a>Återställa en fil med hjälp av en Windows-klient 

1. Om `~snapshot` katalogen på volymen är dold, [Visa dolda objekt](https://support.microsoft.com/help/4028316/windows-view-hidden-files-and-folders-in-windows-10) i den överordnade katalogen som ska visas `~snapshot` .

    ![Visa dolda objekt](../media/azure-netapp-files/snapshot-show-hidden.png) 

2. Navigera till under katalogen i `~snapshot` för att hitta den fil som du vill återställa.  Högerklicka på filen. Välj **Kopiera**.  

    ![Kopiera fil som ska återställas](../media/azure-netapp-files/snapshot-copy-file-restore.png) 

3. Återgå till den överordnade katalogen. Högerklicka på den överordnade katalogen och välj `Paste` att klistra in filen i katalogen.

    ![Klistra in fil som ska återställas](../media/azure-netapp-files/snapshot-paste-file-restore.png) 

4. Du kan också högerklicka på den överordnade katalogen, välja **Egenskaper**, klicka på fliken **tidigare versioner** om du vill se en lista över ögonblicks bilder och välja **Återställ** för att återställa en fil.  

    ![Egenskaper tidigare versioner](../media/azure-netapp-files/snapshot-properties-previous-version.png) 

## <a name="next-steps"></a>Nästa steg

* [Felsöka ögonblicks bilds principer](troubleshoot-snapshot-policies.md)
* [Resursbegränsningar för Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Azure NetApp Files ögonblicks bilder 101 video](https://www.youtube.com/watch?v=uxbTXhtXCkw&feature=youtu.be)
