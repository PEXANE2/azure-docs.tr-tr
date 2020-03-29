---
title: Azure'da SQL Server'da Örnek Veriler - Takım Veri Bilimi Süreci
description: SQL veya Python programlama dilini kullanarak Azure'da SQL Server'da depolanan verileri örnekleyin ve ardından Azure Machine Learning'e taşıyın.
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
ms.openlocfilehash: 71a2ec9dc4d644fb8739db3817e2cd1d09913da7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76717650"
---
# <a name="sample-data-in-sql-server-on-azure"></a><a name="heading"></a>Azure’da SQL Server’daki örnek veriler

Bu makalede, SQL veya Python programlama dilini kullanarak Azure'da SQL Server'da depolanan verilerin nasıl örneklenir. Ayrıca, örneklenmiş verileri bir dosyaya kaydederek, Azure blob'una yükleyerek ve ardından Azure Machine Learning Studio'ya okuyarak Azure Machine Learning'e nasıl taşıyarak nasıl taşıyılabildiğini de gösterir.

Python örneklemesi, örnekleme yapmak için Azure'daki SQL Server'a ve [Pandalar](https://pandas.pydata.org/) kitaplığına bağlanmak için [pyodbc](https://code.google.com/p/pyodbc/) ODBC kitaplığını kullanır.

> [!NOTE]
> Bu belgedeki örnek SQL kodu, verilerin Azure'daki bir SQL Server'da olduğunu varsayar. Değilse, verilerinizi Azure'da SQL Server'a nasıl taşıyacağınıza ilişkin talimatlar için [verilerinizi Azure'daki SQL Server'a taşıyın](move-sql-server-virtual-machine.md) makalesine bakın.
> 
> 

**Neden verilerinizi örneklediniz?**
Çözümlemayı planladığınız veri kümesi büyükse, verileri daha küçük ancak temsili ve daha yönetilebilir boyuta düşürmek için verileri alttan örneklemek genellikle iyi bir fikirdir. Örnekleme, veri anlaşılmasını, keşfini ve özellik mühendisliğini kolaylaştırır. Ekip Veri [Bilimi Süreci (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/) rolü veri işleme fonksiyonları ve makine öğrenme modelleri hızlı prototipleme sağlamaktır.

Bu örnekleme [görevi, Ekip Veri Bilimi Süreci'nin (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımıdır.

## <a name="using-sql"></a><a name="SQL"></a>SQL kullanma
Bu bölümde, veritabanındaki verilere karşı basit rasgele örnekleme gerçekleştirmek için SQL kullanarak çeşitli yöntemler açıklanmaktadır. Veri boyutunuza ve dağıtımına göre bir yöntem seçin.

Aşağıdaki iki öğe, örneklemeyi gerçekleştirmek için SQL Server'da nasıl kullanılacağını `newid` gösterir. Seçtiğiniz yöntem, örneğin ne kadar rasgele olmasını istediğinize bağlıdır (aşağıdaki örnek koddaki pk_id otomatik olarak oluşturulmuş birincil anahtar olarak kabul edilir).

1. Daha az katı rasgele örneklem
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. Daha rasgele örneklem 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

Tablo örneği, verileri örneklemek için de kullanılabilir. Veri boyutunuz büyükse (farklı sayfalardaki verilerin ilişkili olmadığını varsayarsak) ve sorgunun makul bir süre içinde tamamlanması gerekiyorsa, bu seçenek daha iyi bir yaklaşım olabilir.

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> Bu örneklenmiş verileri yeni bir tabloda depolayarak keşfedebilir ve oluşturabilirsiniz
> 
> 

### <a name="connecting-to-azure-machine-learning"></a><a name="sql-aml"></a>Azure Machine Learning'e bağlanma
Azure Machine Learning [Import Data][import-data] modülündeki yukarıdaki örnek sorguları doğrudan kullanarak anında verileri alttaki verileri örnekleyebilir ve azure machine learning deneyine dahil edebilirsiniz. Örneklenmiş verileri okumak için okuyucu modülünün kullanılmasının ekran görüntüsü burada gösterilmiştir:

![okuyucu sql][1]

## <a name="using-the-python-programming-language"></a><a name="python"></a>Python programlama dilini kullanma
Bu bölümde Python'da bir SQL sunucu veritabanına ODBC bağlantısı kurmak için [pyodbc kitaplığı](https://code.google.com/p/pyodbc/) kullanılmıştır. Veritabanı bağlantı dizesi aşağıdaki gibidir: (sunucu adı, dbname, kullanıcı adı ve parolayı yapılandırmanızla değiştirin):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python'daki [Pandalar](https://pandas.pydata.org/) kitaplığı, Python programlaması için veri işleme için zengin bir veri yapıları kümesi ve veri çözümleme araçları sağlar. Aşağıdaki kod, Azure SQL Veritabanı'ndaki bir tablodaki verilerin %0,1'lik bir örneğini Pandas verilerine göre okur:

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, column2... from <table_name> tablesample (0.1 percent)''', conn)

Artık Pandalar veri çerçevesinde örneklenmiş verilerle çalışabilirsiniz. 

### <a name="connecting-to-azure-machine-learning"></a><a name="python-aml"></a>Azure Machine Learning'e bağlanma
Aşağıda örneklenmiş verileri bir dosyaya kaydetmek ve azure blob'una yüklemek için aşağıdaki örnek kodu kullanabilirsiniz. Blob'daki [veriler, İçe Aktarma Verileri][import-data] modüllü olarak doğrudan bir Azure Machine Learning Experiment'e okunabilir. Adımlar şu şekildedir: 

1. Pandalar veri çerçevesini yerel bir dosyaya yazma
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Azure blob'una yerel dosya yükleme
   
        from azure.storage import BlobService
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
3. Azure Machine Learning [Alma Verisi][import-data] modüllerini kullanarak Azure blob'undaki verileri aşağıdaki ekran kapmada gösterildiği gibi okuyun:

![okuyucu blob][2]

## <a name="the-team-data-science-process-in-action-example"></a>Takım Veri Bilimi Süreci Eylem örneği
Ortak bir veri kümesi kullanarak Takım Veri Bilimi İşlemi örneği üzerinden yürümek için, [Takım Veri Bilimi İşlemi Eylem: SQL Server kullanarak](sql-walkthrough.md)bakın.

[1]: ./media/sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
