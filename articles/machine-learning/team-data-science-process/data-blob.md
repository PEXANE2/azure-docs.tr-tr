---
title: Gelişmiş analitikle Azure blob verilerini işleme - Ekip Veri Bilimi Süreci
description: Gelişmiş analizleri kullanarak Azure Blob depolama alanında depolanan verilerden verileri keşfedin ve özellikler oluşturun.
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
ms.openlocfilehash: 4c47dfb8b221b6cb4b6237669ecd17c1637107a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721107"
---
# <a name="process-azure-blob-data-with-advanced-analytics"></a><a name="heading"></a>Azure blob verilerini gelişmiş analiz ile işleme
Bu belge, Azure Blob depolama alanında depolanan verilerden veri keşfetmeyi ve özellikler oluşturmayı kapsar. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Verileri Pandalar veri çerçevesine yükleme
Bir veri kümesini keşfetmek ve işlemek için, blob kaynağından pandalar veri çerçevesine yüklenebilen yerel bir dosyaya indirilmesi gerekir. Bu yordam için izlenir adımlar şunlardır:

1. Blob hizmetini kullanarak aşağıdaki örnek Python koduyla Azure blob'undan verileri indirin. Aşağıdaki koddaki değişkeni belirli değerlerinizle değiştirin: 
   
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
2. Verileri indirilen dosyadan Pandas veri çerçevesine okuyun.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Artık verileri keşfetmeye ve bu veri setinde özellikler oluşturmaya hazırsınız.

## <a name="data-exploration"></a><a name="blob-dataexploration"></a>Veri Arama
Pandaları kullanarak verileri keşfetmenin birkaç yolu verilmiştir:

1. Satır ve sütun sayısını denetleme 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Veri kümesindeki ilk veya son birkaç satırı aşağıdaki gibi inceleyin:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Her sütunun aşağıdaki örnek kodu kullanarak içe aktarılyış edildiği veri türünü denetleyin
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Veri kümesindeki sütunların temel istatistiklerini aşağıdaki gibi denetleyin
   
        dataframe_blobdata.describe()
5. Her sütun değeri için giriş sayısına aşağıdaki gibi bakın
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Aşağıdaki örnek kodu kullanarak her sütundaki gerçek giriş sayısına karşılık eksik değerleri sayma
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Verilerde belirli bir sütun için eksik değerleriniz varsa, bunları aşağıdaki gibi bırakabilirsiniz:
   
        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape
   
   Eksik değerleri değiştirmenin başka bir yolu mod işlevidir:
   
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Bir değişkenin dağılımını çizmek için değişken sayıda depo kutusu kullanarak bir histogram çizimi oluşturma    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bir dağılım çizimi kullanarak veya yerleşik korelasyon işlevini kullanarak değişkenler arasındaki korelasyonlara bakın
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="feature-generation"></a><a name="blob-featuregen"></a>Özellik Oluşturma
Python'u kullanarak aşağıdaki gibi özellikler oluşturabiliriz:

### <a name="indicator-value-based-feature-generation"></a><a name="blob-countfeature"></a>Gösterge değeri tabanlı Özellik Oluşturma
Kategorik özellikler aşağıdaki gibi oluşturulabilir:

1. Kategorik sütunun dağılımını inceleyin:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Sütun değerlerinin her biri için gösterge değerleri oluşturma
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Orijinal veri çerçevesiyle gösterge sütununa katılma 
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. Özgün değişkenin kendisini kaldırın:
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="binning-feature-generation"></a><a name="blob-binningfeature"></a>Binning Özellik Oluşturma
Binned özellikleri oluşturmak için, aşağıdaki gibi devam edin:

1. Sayısal bir sütunu depolamak için bir sütun dizisi ekleme
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Binning'i boolean değişkenleri dizisine dönüştürün
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Son olarak, sahte değişkenleri orijinal veri çerçevesine geri katılma
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <a name="writing-data-back-to-azure-blob-and-consuming-in-azure-machine-learning"></a><a name="sql-featuregen"></a>Azure Machine Learning'de verileri Azure blob'una geri yazma ve tüketme
Verileri araştırdıktan ve gerekli özellikleri oluşturduktan sonra, verileri (örneklenmiş veya örneklenmiş) bir Azure blob'una yükleyebilir ve aşağıdaki adımları kullanarak Azure Machine Learning'de tüketebilirsiniz: Azure Machine Learning'de ek özellikler oluşturulabilir Stüdyo (klasik) de. 

1. Veri çerçevesini yerel dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Verileri Azure blob'una aşağıdaki gibi yükleyin:
   
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
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Artık veriler aşağıdaki ekranda gösterildiği gibi Azure Machine Learning [Import Data][import-data] modüllerini kullanarak blob'dan okunabilir:

![okuyucu blob][1]

[1]: ./media/data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

