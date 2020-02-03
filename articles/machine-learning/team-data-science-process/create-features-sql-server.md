---
title: SQL ve Python - Team Data Science Process kullanarak SQL Server özelliklerini oluşturma
description: SQL ve Python - Team Data Science Process parçası kullanarak azure'da bir SQL Server VM'si depolanan verilerin özelliklerini oluşturur.
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
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721753"
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a>SQL ve Python kullanarak SQL Server’daki verilerin özelliklerini oluşturma
Bu belge, daha verimli bir şekilde verilerden bilgi algoritmaları yardımcı olan bir SQL Server VM'si, azure'da depolanan verilerin özelliklerini oluşturma adımları anlatılmaktadır. Bu görevi gerçekleştirmek için SQL veya Python gibi bir programlama dili kullanabilirsiniz. Her iki yaklaşım burada gösterilmiştir.

Bu görev, [ekip veri bilimi işlemindeki (TDSP)](https://docs.microsoft.com/azure/machine-learning/team-data-science-process/)bir adımdır.

> [!NOTE]
> Pratik bir örnek için, [NYC TAXI veri kümesine](https://www.andresmh.com/nyctaxitrips/) başvurabilirsiniz ve [IPython Not defteri 'ni kullanarak ıpnb başlıklı NYC Data denetimi](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) öğesine başvurabilirsiniz ve uçtan uca bir izlenecek yol SQL Server.
> 
> 

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, olduğunu varsayar:

* Bir Azure depolama hesabı oluşturuldu. Yönergelere ihtiyacınız varsa bkz. [Azure depolama hesabı oluşturma](../../storage/common/storage-account-create.md)
* Verilerinizi SQL Server'da depolanan. Aksi takdirde, verileri buraya taşıma hakkında yönergeler için bkz. [verileri Azure SQL veritabanına taşıma Azure Machine Learning](move-sql-azure.md) .

## <a name="sql-featuregen"></a>SQL ile özellik oluşturma
Bu bölümde, biz özellikleri SQL kullanarak oluşturma yolları açıklanmaktadır:  

* [Sayı tabanlı özellik oluşturma](#sql-countfeature)
* [Özellik oluşturmayı atma](#sql-binningfeature)
* [Tek bir sütundan özellikler kullanıma alınıyor](#sql-featurerollout)

> [!NOTE]
> Ek özellikler oluşturduktan sonra bunları mevcut tabloya sütun olarak ekleyin veya özgün tabloyla birleştirilen birincil anahtar ve ek özellikler ile yeni bir tablo oluşturabilirsiniz.
> 
> 

### <a name="sql-countfeature"></a>Sayı tabanlı özellik oluşturma
Bu belge sayısı özellikleri oluşturmanın iki yolunu gösterir. İlk yöntem koşullu toplamı ve ikinci yöntem 'where' yan tümcesi kullanır. Bu yeni özellikler daha sonra orijinal verilerle birlikte sayı özelliklerinin olması için özgün tabloyla (birincil anahtar sütunları kullanılarak) birleştirilmiş olabilir.

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <a name="sql-binningfeature"></a>Özellik oluşturmayı atma
Aşağıdaki örnek, bunun yerine bir özelliği olarak kullanılabilir bir sayısal sütun göre gruplama (beş depo kullanarak) binned özellikleri oluşturma adımları anlatılmaktadır:

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <a name="sql-featurerollout"></a>Tek bir sütundan özellikler kullanıma alınıyor
Bu bölümde ek özellikler oluşturmak için bir tablodaki tek bir sütun kullanıma sunma gösterilmektedir. Örnek özellikleri oluşturmak çalıştığınız tablosunda bir enlem veya boylam sütunu olduğunu varsayar.

Latitude/Boylam konum verilerine (StackOverflow `https://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`kaynaklı kaynak) ilişkin kısa bir açıklama aşağıda verilmiştir. Özellikler alanı oluşturmadan önce ilgili konum verileri anlamak için yararlı işlemlerden bazıları aşağıda verilmiştir:

* Oturum, biz Kuzey olup veya Güney, Doğu veya Batı dünya üzerindeki gösterir.
* Sıfır olmayan bir yüz basamağı belirtir boylam, enlem değil kullanılıyor.
* Onlarca basamaklı bir konuma yaklaşık 1.000 kilometre sağlar. Bu, hangi Kıta veya üzerinde duyuyoruz Okyanusu hakkında yararlı bilgiler verir.
* Birimleri basamak (bir ondalık derece) 111 kilometre (60 Deniz mili, yaklaşık 69 mil) bir konum sağlar. Kabaca, hangi büyük eyalet veya ülke/bölge olduğunu gösterir.
* En fazla 11.1 km ilk ondalık yerdir: komşu büyük Şehir'dan büyük bir şehir konumunu ayırt edebilir.
* En fazla 1.1 km ikinci ondalık yerdir: sonraki bir village ayırabilirsiniz.
* Üçüncü ondalık en fazla 110 m: büyük Tarım alan veya Kurumsal kampüs tanımlayabilirsiniz yerdir.
* Dördüncü ondalık en fazla 11 m: olan bir paket tanımanıza yerdir. Tipik doğruluğunu düzeltilemeyen bir GPS birim için hiçbir engelleme ile karşılaştırılabilir.
* Beşinci ondalık en fazla 1.1 m: ağaçları birbirinden ayıran yerdir. Bu düzey ticari GPS birimleri ile tutarlılık yalnızca fark düzeltme ile gerçekleştirilebilir.
* Altıncı ondalık basamak 0,11 ' ye kadar olur: Bu düzeyi, yapıları tasarlamak için, yapıları oluşturmak amacıyla yapıları ayrıntılı olarak düzenlemek için kullanabilirsiniz. Hareketleri glaciers rivers ve izlemek için birden fazla yeterince iyi olmalıdır. Bu amaca, farklı şekilde düzeltilen GPS gibi GPS ile sorunsuz ölçüler elde edilebilir.

Konum bilgileri, bölge, konum ve Şehir bilgilerini ayrılarak özellikleri tespit olabilir. Bir kez, Bing Haritalar API 'SI gibi bir REST uç noktasını da çağırabilir (bölge/bölge bilgilerini almak için bkz. `https://msdn.microsoft.com/library/ff701710.aspx`).

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

Bu konum tabanlı özellikleri, daha fazla ek sayısı özellikleri daha önce açıklandığı gibi oluşturmak için kullanılabilir.

> [!TIP]
> İstediğiniz dilde kullanarak kayıtları program aracılığıyla ekleyebilirsiniz. Veri yazma verimliliğini artırmak için öbekler halinde eklemek gerekebilir. [Bu, pyodbc kullanarak bunun nasıl yapılacağını gösteren bir örnektir](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).
> [Bcp yardımcı programını](https://msdn.microsoft.com/library/ms162802.aspx) kullanarak veritabanına veri eklemek diğer bir alternatiftir
> 
> 

### <a name="sql-aml"></a>Azure Machine Learning bağlanılıyor
Yeni oluşturulan özellik bir sütun olarak var olan bir tabloya eklenebilir veya yeni bir tablo içinde saklanan ve machine learning için özgün tablo ile birleştirilmiş. Aşağıda gösterildiği gibi, Azure ML 'de [veri Içeri aktarma](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modülü kullanılarak önceden oluşturulmuş özellikler oluşturulabilir veya erişilebilir:

![Azure ML okuyucular](./media/sql-server-virtual-machine/reader_db_featurizedinput.png)

## <a name="python"></a>Python gibi bir programlama dilini kullanma
Veriler SQL Server'da olduğunda özellikler oluşturmak için Python kullanarak Python kullanarak Azure blob veri işleme için benzerdir. Karşılaştırma için bkz. [veri bilimi ortamınızda Azure blob verilerini işleme](data-blob.md). Verileri veritabanından daha fazla işlemek için bir pandas veri çerçevesine yükleyin. Veritabanına bağlanma ve veri çerçevesine veri yükleme işlemi, bu bölümde belgelenmiştir.

Aşağıdaki bağlantı dizesi biçimi python'dan pyodbc (Değiştir servername, dbname, kullanıcı adı ve parola, belirli değerleri içeren) kullanarak bir SQL Server veritabanına bağlanmak için kullanılabilir:

    #Set up the SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

Python 'daki [Pandas kitaplığı](https://pandas.pydata.org/) , Python programlamasına yönelik veri işleme için zengin veri yapıları ve veri çözümleme araçları sağlar. Aşağıdaki kod, sonuçları bir SQL Server veritabanından bir Pandas veri çerçevesine döndürülen okur:

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <columnname2>... from <tablename>''', conn)

Artık, [Panda kullanarak Azure Blob depolama verilerine yönelik özellikler oluşturma](create-features-blob.md)konu başlığı altında, Pandas veri çerçevesiyle birlikte çalışabilirsiniz.

