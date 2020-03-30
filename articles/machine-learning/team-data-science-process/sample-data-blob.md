---
title: Azure blob depolamasında örnek veriler - Ekip Veri Bilimi Süreci
description: Azure blob depolama alanında depolanan verileri programlı olarak indirerek ve Python'da yazılan yordamları kullanarak örnekleme.
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
ms.openlocfilehash: 4832762a88073f4d819925659bf9078e18f60c2d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720291"
---
# <a name="sample-data-in-azure-blob-storage"></a><a name="heading"></a>Azure blob depolamadaki verileri örneklendirme

Bu makalede, Azure blob depolama alanında depolanan örnekleme verileri, programlı olarak indirilerek ve Python'da yazılmış yordamları kullanarak örnekleme yi kapsamaktadır.

**Neden verilerinizi örneklediniz?**
Çözümlemayı planladığınız veri kümesi büyükse, verileri daha küçük ancak temsili ve daha yönetilebilir boyuta düşürmek için verileri alttan örneklemek genellikle iyi bir fikirdir. Örnekleme, veri anlaşılmasını, keşfini ve özellik mühendisliğini kolaylaştırır. Cortana Analitik Süreci'ndeki rolü, veri işleme işlevlerinin ve makine öğrenimi modellerinin hızlı prototiplemesini sağlamaktır.

Bu örnekleme [görevi, Ekip Veri Bilimi Süreci'nin (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımıdır.

## <a name="download-and-down-sample-data"></a>İndirme ve alt örneklem verileri
1. Aşağıdaki örnek Python kodundan Blob hizmetini kullanarak Azure blob depolamasından verileri indirin: 
   
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

2. Verileri yukarıda indirilen dosyadan Pandalar veri çerçevesine okuyun.
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. `numpy`'s'leri `random.choice` kullanarak verileri aşağıdan örnekle:
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Artık daha fazla keşif ve özellik oluşturma için bir yüzde örneği ile yukarıdaki veri çerçevesi ile çalışabilirsiniz.

## <a name="upload-data-and-read-it-into-azure-machine-learning"></a><a name="heading"></a>Azure Machine Learning'e veri yükleyin ve okuyun
Verileri aşağıdan tatmak ve doğrudan Azure Machine Learning'de kullanmak için aşağıdaki örnek kodu kullanabilirsiniz:

1. Veri çerçevesini yerel bir dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Aşağıdaki örnek kodu kullanarak yerel dosyayı Azure blob'una yükleyin:
   
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

3. Aşağıdaki resimde gösterildiği gibi Azure Machine Learning [Alma Verilerini](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) kullanarak Azure blob'undaki verileri okuyun:

![okuyucu blob](./media/sample-data-blob/reader_blob.png)

