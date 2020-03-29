---
title: SQL ve Python kullanarak SQL Server'da özellikler oluşturma - Takım Veri Bilimi Süreci
description: Team Data Science Process'in bir parçası olan SQL ve Python'u kullanarak Azure'da SQL Server VM'de depolanan veriler için özellikler oluşturun.
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
ms.openlocfilehash: 58fa98005d7d89e84404d99cf4f55e456fd91f21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76721753"
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>SQL ve Python kullanarak SQL Server’daki verilerin özelliklerini oluşturma
Bu belge, algoritmaların verilerden daha verimli bilgi edinmelerine yardımcı olan Azure'daki BIR SQL Server VM'de depolanan veriler için nasıl özellikler üretilmeye yardımcı olduğunu gösterir. Bu görevi gerçekleştirmek için SQL veya Python gibi bir programlama dilini kullanabilirsiniz. Her iki yaklaşım da burada gösterilmiştir.

Bu [görev, Ekip Veri Bilimi Süreci'nin (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımıdır.

> [!NOTE]
> Pratik bir örnek için, [NYC Taksi veri setine](https://www.andresmh.com/nyctaxitrips/) danışabilir ve uçtan uca bir geçiş için [IPython Notebook ve SQL Server'ı kullanarak NYC Veri çekişmesi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) başlıklı IPNB'ye bakabilirsiniz.
> 
> 

## <a name="prerequisites"></a>Ön koşullar
Bu makalede, sahip olduğunuzu varsayar:

* Bir Azure depolama hesabı oluşturdu. Yönergelere ihtiyacınız [varsa,](../../storage/common/storage-account-create.md) bkz.
* Verilerinizi SQL Server'da depolayın. Yoksa, verileri oraya nasıl taşıyacağınız hakkında talimatlar için [verileri Azure Machine Learning için Azure SQL Veritabanına Taşı'ya](move-sql-azure.md) bakın.

## <a name="feature-generation-with-sql"></a><a name="sql-featuregen"></a>SQL ile özellik oluşturma
Bu bölümde, SQL kullanarak özellikler oluşturma yollarını açıklıyoruz:  

* [Count tabanlı Özellik Oluşturma](#sql-countfeature)
* [Binning Özellik Oluşturma](#sql-binningfeature)
* [Özellikleri tek bir sütundan çıkarma](#sql-featurerollout)

> [!NOTE]
> Ek özellikler oluşturduktan sonra, bunları varolan tabloya sütun olarak ekleyebilir veya özgün tabloyla birleştirilebilen ek özellikler ve birincil anahtariçeren yeni bir tablo oluşturabilirsiniz.
> 
> 

### <a name="count-based-feature-generation"></a><a name="sql-countfeature"></a>Count tabanlı özellik oluşturma
Bu belge, sayım özellikleri oluşturmanın iki yolunu gösterir. İlk yöntem koşullu toplamı kullanır ve ikinci yöntem 'nerede' yan tümcesini kullanır. Bu yeni özellikler daha sonra özgün verilerin yanında sayım özelliklerine sahip olmak için özgün tabloyla (birincil anahtar sütunları kullanarak) birleştirilmiş olabilir.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="binning-feature-generation"></a><a name="sql-binningfeature"></a>Binning Özellik Oluşturma
Aşağıdaki örnek, bunun yerine özellik olarak kullanılabilecek bir sayısal sütunu binning (beş kutu kullanarak) ile binned özellikleri oluşturmak için nasıl gösterir:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="rolling-out-the-features-from-a-single-column"></a><a name="sql-featurerollout"></a>Özellikleri tek bir sütundan çıkarma
Bu bölümde, ek özellikler oluşturmak için tabloda tek bir sütunun nasıl dağıtılanacağı gösterilmiştir. Örnek, tabloda özellikler oluşturmaya çalıştığınız bir enlem veya boylam sütunu olduğunu varsayar.

Burada enlem / boylam konum verileri (stackoverflow `https://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`kaynak) kısa bir astar olduğunu. Alandan özellikler oluşturmadan önce konum verileri hakkında anlamanın bazı yararlı özellikleri şunlardır:

* İşaret, dünyada kuzey veya güney, doğu mu, doğu mu batı mı olduğumuzu gösteriyor.
* Sıfır olmayan yüzlerce basamak boylam gösterir, enlem kullanılmaz.
* Onluk basamak yaklaşık 1.000 kilometrelik bir konum verir. Hangi kıtada veya okyanusta olduğumuz hakkında yararlı bilgiler verir.
* Birim basamak (bir ondalık derece) 111 kilometre (60 deniz mili, yaklaşık 69 mil) kadar bir konum verir. Kabaca, ne kadar büyük bir devlette veya ülkede/bölgede olduğumuzu gösterir.
* İlk ondalık basamak 11,1 km'ye kadar değer: büyük bir şehrin konumunu komşu büyük bir şehirden ayırt edebilir.
* İkinci ondalık basamak 1,1 km'ye kadar değer: bir köyü diğerinden ayırabilir.
* Üçüncü ondalık basamak 110 m'ye kadar değer: büyük bir tarım alanı veya kurumsal kampüs belirleyebilir.
* Dördüncü ondalık basamak 11 m'ye kadar değer: bir parsel araziyi belirleyebilir. Hiçbir girişim olmadan düzeltilmemiş bir GPS ünitesinin tipik doğruluğu ile karşılaştırılabilir.
* Beşinci ondalık basamak 1,1 m'ye kadar değer: ağaçları birbirinden ayırır. Ticari GPS üniteleri ile bu seviyeye doğruluk sadece diferansiyel düzeltme ile elde edilebilir.
* Altıncı ondalık basamak 0,11 m'ye kadar değer: bu seviyeyi yapıları ayrıntılı olarak ortaya koymak, manzara tasarlamak, yollar inşa etmek için kullanabilirsiniz. Buzulların ve nehirlerin hareketlerini izlemek için fazlasıyla iyi olmalı. Bu hedefe GPS ile özenli önlemler alınarak elde edilebilir, farklı düzeltilmiş GPS gibi.

Konum bilgileri, bölge, konum ve şehir bilgilerini ayırarak elde edilebilir. Bing Maps API gibi bir REST bitiş noktasını `https://msdn.microsoft.com/library/ff701710.aspx` da çağırabilirsiniz (bölge/bölge bilgilerini almak için bkz.

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
> Seçtiğiniz dili kullanarak kayıtları programlı olarak ekleyebilirsiniz. Yazma verimliliğini artırmak için verileri parçalara eklemeniz gerekebilir. [Burada pyodbc kullanarak bunu yapmak için nasıl bir örnektir.](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)
> Başka bir alternatif [BCP yardımcı programı](https://msdn.microsoft.com/library/ms162802.aspx) kullanarak veritabanına veri eklemektir
> 
> 

### <a name="connecting-to-azure-machine-learning"></a><a name="sql-aml"></a>Azure Machine Learning'e bağlanma
Yeni oluşturulan özellik, varolan bir tabloya sütun olarak eklenebilir veya yeni bir tabloda depolanabilir ve makine öğrenimi için özgün tabloyla birleştirilir. Azure ML'deki [Alma Verisi](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modülü kullanılarak aşağıda gösterildiği gibi, özellikler zaten oluşturulmuşsa oluşturulabilir veya erişilebilir:

![Azure ML okuyucular](./media/sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="using-a-programming-language-like-python"></a><a name="python"></a>Python gibi bir programlama dilini kullanma
Veriler SQL Server'da olduğunda özellikler oluşturmak için Python'u kullanmak, Python kullanarak Azure blob'daki verileri işlemeye benzer. Karşılaştırma için veri [bilimi ortamınızdaki Azure Blob verilerini işleme](data-blob.md)ye bakın. Daha fazla işlemek için veritabanından verileri pandalar veri çerçevesine yükleyin. Veritabanına bağlanma ve verileri veri çerçevesine yükleme işlemi bu bölümde belgelenmiştir.

Aşağıdaki bağlantı dize biçimi pyodbc kullanarak Python'dan bir SQL Server veritabanına bağlanmak için kullanılabilir (sunucu adı, dbname, kullanıcı adı ve parolayı belirli değerlerle değiştirin):

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python'daki [Pandalar kitaplığı,](https://pandas.pydata.org/) Python programlaması için veri işleme için zengin bir veri yapıları kümesi ve veri çözümleme araçları sağlar. Aşağıdaki kod, SQL Server veritabanından Pandas veri çerçevesine döndürülen sonuçları okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Artık Pandas veri çerçevesi ile konular asıkapsadığı gibi Çalışabilirsiniz [Panda kullanarak Azure blob depolama verileri için özellikler oluştur.](create-features-blob.md)

