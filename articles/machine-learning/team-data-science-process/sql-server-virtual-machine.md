---
title: SQL Server sanal makinesindeki verileri keşfedin - Ekip Veri Bilimi Süreci
description: Azure'daki bir SQL Server sanal makinesinde Python veya SQL kullanarak + verileri inceleyin ve özellikler oluşturun.
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
ms.openlocfilehash: d3eb4d2faf58d1861fda9d04437f9f9530c77672
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76718489"
---
# <a name="process-data-in-sql-server-virtual-machine-on-azure"></a><a name="heading"></a>Azure üzerindeki SQL Server Sanal Makinesi verilerini işleme
Bu belge, Azure'daki bir SQL Server VM'de depolanan veriler için verilerin nasıl keşfedilen ve özellikler oluşturacağı hakkında bilgi kapsar. Bu hedef, SQL kullanarak veya Python gibi bir programlama dili kullanılarak veri çekişmesi ile tamamlanabilir.

> [!NOTE]
> Bu belgedeki örnek SQL deyimleri verilerin SQL Server'da olduğunu varsayar. Değilse, verilerinizi SQL Server'a nasıl taşıyarak taşıyabildiğini öğrenmek için bulut veri bilimi işlem haritasına bakın.
> 
> 

## <a name="using-sql"></a><a name="SQL"></a>SQL kullanma
Bu bölümde SQL kullanarak aşağıdaki veri çekişme görevlerini açıklıyoruz:

1. [Veri Arama](#sql-dataexploration)
2. [Özellik Oluşturma](#sql-featuregen)

### <a name="data-exploration"></a><a name="sql-dataexploration"></a>Veri Arama
Burada, SQL Server'daki veri depolarını keşfetmek için kullanılabilecek birkaç örnek SQL komut dosyası verebilirsiniz.

> [!NOTE]
> Pratik bir örnek için, [NYC Taksi veri kümesini](https://www.andresmh.com/nyctaxitrips/) kullanabilir ve uçtan uca bir geçiş için [IPython Notebook ve SQL Server'ı kullanarak NYC Veri çekişmesi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) başlıklı IPNB'ye bakabilirsiniz.
> 
> 

1. Günlük gözlem sayısını alın
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. Düzeyleri kategorik bir sütunda alma
   
    `select  distinct <column_name> from <databasename>`
3. İki kategorik sütunun birleşimiyle düzey sayısını alma 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. Sayısal sütunlar için dağıtımı alın
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <a name="feature-generation"></a><a name="sql-featuregen"></a>Özellik Oluşturma
Bu bölümde, SQL kullanarak özellikler oluşturma yollarını açıklıyoruz:  

1. [Count tabanlı Özellik Oluşturma](#sql-countfeature)
2. [Binning Özellik Oluşturma](#sql-binningfeature)
3. [Özellikleri tek bir sütundan çıkarma](#sql-featurerollout)

> [!NOTE]
> Ek özellikler oluşturduktan sonra, bunları varolan tabloya sütun olarak ekleyebilir veya özgün tabloyla birleştirilebilen ek özellikler ve birincil anahtariçeren yeni bir tablo oluşturabilirsiniz. 
> 
> 

### <a name="count-based-feature-generation"></a><a name="sql-countfeature"></a>Count tabanlı Özellik Oluşturma
Aşağıdaki örnekler, sayım özellikleri oluşturmanın iki yolunu göstermektedir. İlk yöntem koşullu toplamı kullanır ve ikinci yöntem 'nerede' yan tümcesini kullanır. Bu sonuçlar daha sonra özgün verilerin yanında sayım özelliklerine sahip olmak için özgün tabloyla (birincil anahtar sütunları kullanarak) birleştirilmiş olabilir.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <a name="binning-feature-generation"></a><a name="sql-binningfeature"></a>Binning Özellik Oluşturma
Aşağıdaki örnek, bunun yerine özellik olarak kullanılabilecek bir sayısal sütunu binning (beş kutu kullanarak) ile binned özellikleri oluşturmak için nasıl gösterir:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="rolling-out-the-features-from-a-single-column"></a><a name="sql-featurerollout"></a>Özellikleri tek bir sütundan çıkarma
Bu bölümde, ek özellikler oluşturmak için tabloda tek bir sütunun nasıl dağıtılanacağı gösterilmiştir. Örnek, tabloda özellikler oluşturmaya çalıştığınız bir enlem veya boylam sütunu olduğunu varsayar.

Burada enlem / boylam konum verileri (stackoverflow nasıl [enlem ve boylam doğruluğunu ölçmek için](https://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)kaynak) kısa bir astar nedir? Bu kılavuz, konumu bir veya daha fazla özellik olarak dahil etmeden önce anlamak için yararlıdır:

* İşaret bize dünyada kuzey mi, güney mi, doğu mu, batı mı olduğunu söylüyor.
* Sıfır olmayan yüzlerce rakam bize boylam kullandığımızı söylüyor, enlem değil!
* Onluk basamak yaklaşık 1.000 kilometrelik bir konum verir. Hangi kıtada veya okyanusta olduğumuz hakkında bize yararlı bilgiler verir.
* Birim basamak (bir ondalık derece) 111 kilometre (60 deniz mili, yaklaşık 69 mil) kadar bir konum verir. Kabaca hangi eyalette, ülkede veya bölgede olduğunuzu söyleyebilir.
* İlk ondalık basamak 11,1 km'ye kadar değer: büyük bir şehrin konumunu komşu büyük bir şehirden ayırt edebilir.
* İkinci ondalık basamak 1,1 km'ye kadar değer: bir köyü diğerinden ayırabilir.
* Üçüncü ondalık basamak 110 m'ye kadar değer: büyük bir tarım alanı veya kurumsal kampüs belirleyebilir.
* Dördüncü ondalık basamak 11 m'ye kadar değer: bir parsel araziyi belirleyebilir. Hiçbir girişim olmadan düzeltilmemiş bir GPS ünitesinin tipik doğruluğu ile karşılaştırılabilir.
* Beşinci ondalık basamak 1,1 m'ye kadar değer: ağaçları birbirinden ayırır. Ticari GPS üniteleri ile bu seviyeye doğruluk sadece diferansiyel düzeltme ile elde edilebilir.
* Altıncı ondalık basamak 0,11 m'ye kadar değer: Bunu yapıları ayrıntılı olarak ortaya koymak, manzara tasarlamak, yollar inşa etmek için kullanabilirsiniz. Buzulların ve nehirlerin hareketlerini izlemek için fazlasıyla iyi olmalı. Bu, farklı olarak düzeltilmiş GPS gibi GPS ile özenli önlemler alınarak elde edilebilir.

Konum bilgileri, bölge, konum ve şehir bilgilerini birbirinden ayırarak aşağıdaki gibi kullanılabilir. Bölge/bölge bilgilerini almak için [Konum Bul'da](https://msdn.microsoft.com/library/ff701710.aspx) bulunan Bing Maps API gibi bir DINLENME bitiş noktasını da arayabilirsiniz.

    select 
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

Bu konum tabanlı özellikler, daha önce açıklandığı gibi ek sayı özellikleri oluşturmak için daha fazla kullanılabilir. 

> [!TIP]
> Seçtiğiniz dili kullanarak kayıtları programlı olarak ekleyebilirsiniz. Yazma verimliliğini artırmak için verileri yığınlara eklemeniz gerekebilir (pyodbc kullanarak bunu nasıl yapacağınız gibi bir örnek [için, python ile SQLServer'a erişmek için HelloWorld örneğine](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)bakın). Başka bir alternatif [BCP yardımcı programı](https://msdn.microsoft.com/library/ms162802.aspx)kullanarak veritabanına veri eklemektir.
> 
> 

### <a name="connecting-to-azure-machine-learning"></a><a name="sql-aml"></a>Azure Machine Learning'e bağlanma
Yeni oluşturulan özellik, varolan bir tabloya sütun olarak eklenebilir veya yeni bir tabloda depolanabilir ve makine öğrenimi için özgün tabloyla birleştirilir. Azure Machine Learning'de Aşağıda gösterildiği gibi [Veri Alma][import-data] modülü kullanılarak, özellikler zaten oluşturulmuşsa oluşturulabilir veya erişilebilir:

![azureml okuyucular][1] 

## <a name="using-a-programming-language-like-python"></a><a name="python"></a>Python gibi bir programlama dilini kullanma
Veriler SQL Server'da yken verileri keşfetmek ve özellikler oluşturmak için Python'u kullanmak, [veri bilimi ortamınızda İşlem Azure Blob verilerinde](data-blob.md)belgelenen Python'u kullanarak Azure blob'daki verileri işlemeye benzer. Daha fazla işleme için veritabanındaki verileri pandalar veri çerçevesine yükleyin. Veritabanına bağlanma ve verileri bu bölümdeki veri çerçevesine yükleme işlemini belgeliyoruz.

Aşağıdaki bağlantı dize biçimi pyodbc kullanarak Python'dan bir SQL Server veritabanına bağlanmak için kullanılabilir (sunucu adı, dbname, kullanıcı adı ve parolayı belirli değerlerle değiştirin):

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python'daki [Pandalar kitaplığı,](https://pandas.pydata.org/) Python programlaması için veri işleme için zengin bir veri yapıları kümesi ve veri çözümleme araçları sağlar. Aşağıdaki kod, SQL Server veritabanından Pandas veri çerçevesine döndürülen sonuçları okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Artık veri bilimi ortamınızda Azure [Blob verilerini işleyebilir makalesinde](data-blob.md)kapsandığınız pandalar veri çerçevesi ile çalışabilirsiniz.

## <a name="azure-data-science-in-action-example"></a>Azure Veri Bilimi Eylem Örneği
Genel bir veri kümesi ni kullanarak Azure Veri Bilimi [Süreci'nin](sql-walkthrough.md)uçtan uca bir geçiş örneği için bkz.

[1]: ./media/sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

