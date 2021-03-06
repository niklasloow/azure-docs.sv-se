---
title: Exempel data i Azure Blob Storage – team data science process
description: Samplings data som lagras i Azure Blob Storage genom att ladda ned dem program mässigt och sedan sampla dem med hjälp av procedurer skrivna i python.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 01/10/2020
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 04528d28e9f54710cd0a63372e32b099c2e07fb5
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/07/2020
ms.locfileid: "86026176"
---
# <a name="sample-data-in-azure-blob-storage"></a><a name="heading"></a>Exempeldata i Azure Blob Storage

Den här artikeln beskriver samplings data som lagras i Azure Blob Storage genom att ladda ned dem program mässigt och sedan sampla dem med hjälp av procedurer skrivna i python.

**Varför ska du testa dina data?**
Om data uppsättningen som du planerar att analysera är stor är det vanligt vis en bra idé att stänga av data för att minska den till en mindre men representativ och mer hanterbar storlek. Sampling underlättar data förståelse, utforskning och funktions teknik. Dess roll i Cortana Analytics-processen är att möjliggöra snabb prototyper av data bearbetnings funktioner och maskin inlärnings modeller.

Den här samplings aktiviteten är ett steg i [TDSP (Team data science process)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/).

## <a name="download-and-down-sample-data"></a>Hämta och stänga av exempel data
1. Hämta data från Azure Blob Storage med hjälp av Blob Service från följande exempel på python-kod: 

    ```python
    from azure.storage.blob import BlobService
    import tables

    STORAGEACCOUNTNAME= <storage_account_name>
    STORAGEACCOUNTKEY= <storage_account_key>
    LOCALFILENAME= <local_file_name>        
    CONTAINERNAME= <container_name>
    BLOBNAME= <blob_name>

    #download from blob
    t1=time.time()
    blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
    blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
    t2=time.time()
    print(("It takes %s seconds to download "+blobname) % (t2 - t1))
    ```

2. Läs data till en Pandas data-ram från den hämtade filen ovan.

    ```python
    import pandas as pd

    #directly ready from file on disk
    dataframe_blobdata = pd.read_csv(LOCALFILE)
    ```

3. Nedåt – sampla data med hjälp av `numpy` följande `random.choice` :

    ```python
    # A 1 percent sample
    sample_ratio = 0.01 
    sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
    sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
    dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]
    ```

Nu kan du arbeta med ovanstående data ram med ett procent prov för ytterligare utforskning och funktion.

## <a name="upload-data-and-read-it-into-azure-machine-learning"></a><a name="heading"></a>Ladda upp data och Läs den i Azure Machine Learning
Du kan använda följande exempel kod för att hämta exempel på data och använda dem direkt i Azure Machine Learning:

1. Skriva data ramen till en lokal fil

    ```python
    dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
    ```

2. Ladda upp den lokala filen till en Azure-Blob med följande exempel kod:

    ```python
    from azure.storage.blob import BlobService
    import tables

    STORAGEACCOUNTNAME= <storage_account_name>
    LOCALFILENAME= <local_file_name>
    STORAGEACCOUNTKEY= <storage_account_key>
    CONTAINERNAME= <container_name>
    BLOBNAME= <blob_name>

    output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
    localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory

    try:

    #perform upload
    output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)

    except:            
        print ("Something went wrong with uploading to the blob:"+ BLOBNAME)
    ```

3. Läs data från Azure-blobben med Azure Machine Learning [Importera data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) som visas på bilden nedan:

![läsar BLOB](./media/sample-data-blob/reader_blob.png)

