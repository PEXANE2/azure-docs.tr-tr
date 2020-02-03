---
title: Gelişmiş analiz - Team Data Science Process ile Azure blob verilerini işleme
description: Verileri araştırmak ve Gelişmiş analiz kullanarak Azure Blob depolamada depolanan verilerden özellikler oluşturun.
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
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721107"
---
# <a name="heading"></a>Gelişmiş analiz ile Azure blob verilerini işleme
Bu belge, veri keşfetmek ve Azure Blob Depolama alanında depolanan verilerden oluşturma özellikleri kapsar. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Verileri bir Pandas veri çerçevesine yükleyin
Bir veri kümesini araştırmak ve işlemek için, blob kaynağından bir Pandas veri çerçevesine yüklenebilecek bir yerel dosyaya indirilmelidir. Bu yordam için izlenmesi gereken adımlar şunlardır:

1. Blob hizmetini kullanarak aşağıdaki örnek Python koduna sahip Azure blobundan verileri indirin. Değişkeni aşağıdaki kodda belirli değerleriniz ile değiştirin: 
   
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
2. İçine bir Pandas veri çerçevesine indirilen dosyadaki verileri okuyamadı.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Verileri keşfetme ve bu veri kümesi özellikleri oluşturmak hazırsınız.

## <a name="blob-dataexploration"></a>Veri araştırması
Panda kullanarak verileri araştırmak için gösteren bazı örnekleri şunlardır:

1. Satır ve sütun sayısını denetleme 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Aşağıda gösterildiği gibi veri kümesindeki ilk veya son birkaç satır inceleyin:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Her sütun, aşağıdaki örnek kodu kullanarak olarak içeri aktarılan veri türünü denetleyin
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Veri kümesindeki sütunları temel istatistikleri gibi denetleyin
   
        dataframe_blobdata.describe()
5. Girdi sayısı bu değeri için her bir sütun değeri şu şekilde bakın
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Aşağıdaki örnek kodu kullanarak her bir sütunun girişleri gerçek sayısı eksik değerleri Say
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Verileri belirli bir sütun için eksik değerler varsa, bunları aşağıdaki gibi silebilirsiniz:
   
        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape
   
   Eksik değerleri değiştirmek için başka bir yol ile modu işlevdir:
   
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Bir değişkenin dağıtım çizmek için değişken sayıda depo kullanarak bir çubuk grafik çizim oluşturma    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bir dağılım grafiği veya yerleşik bağıntı işlevi kullanarak değişkenleri arasında bağıntılar bakın
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="blob-featuregen"></a>Özellik oluşturma
Biz, Python kullanarak aşağıdaki gibi özellikleri oluşturabilirsiniz:

### <a name="blob-countfeature"></a>Gösterge değer tabanlı özellik oluşturma
Kategorik özellikleri şu şekilde oluşturulabilir:

1. Kategorik sütunun dağılımını inceleyin:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Her sütun değerleri için değerler gösterge oluştur
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Gösterge sütunu özgün veri çerçevesi ile birleştirme 
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. Özgün değişken kaldırın:
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="blob-binningfeature"></a>Özellik oluşturmayı atma
Binned özellikler oluşturmak için size aşağıdaki gibi ilerleyin:

1. Bir dizi sayısal bir sütun depo için sütunları Ekle
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Bir dizi Boole değişkenleri için gruplama işlemi Dönüştür
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Son olarak, özgün veri çerçevesine sahte değişkenleri katılın
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <a name="sql-featuregen"></a>Verileri Azure Blob 'a geri yazma ve Azure Machine Learning kullanma
Verileri araştırdıktan ve gerekli özellikleri oluşturduktan sonra, verileri (örneklendirilmiş veya daha fazla) bir Azure blobuna yükleyebilir ve aşağıdaki adımları kullanarak Azure Machine Learning kullanabilirsiniz: ek özellikler Azure Machine Learning oluşturulabilir Studio (klasik) de. 

1. Veri çerçevesinin yerel bir dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Verileri Azure blobuna şu şekilde yükleyin:
   
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
3. Artık veriler, aşağıdaki ekranda gösterildiği gibi Azure Machine Learning [veri alma][import-data] modülünü kullanarak bloba okunabilir:

![Okuyucu blob][1]

[1]: ./media/data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

