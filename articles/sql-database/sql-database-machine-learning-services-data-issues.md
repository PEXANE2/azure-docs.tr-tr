---
title: R ve SQL veri türleri ve nesneleri ile çalışma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Karşılaşabileceğiniz sık karşılaşılabilecek sorunlar da dahil olmak üzere Machine Learning Services (önizleme) kullanarak Azure SQL Veritabanı ile R'deki veri türleri ve veri nesneleri ile nasıl çalışacağınızı öğrenin.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning
ms.custom: ''
ms.devlang: r
ms.topic: conceptual
author: garyericson
ms.author: garye
ms.reviewer: davidph
manager: cgronlun
ms.date: 04/11/2019
ROBOTS: NOINDEX
ms.openlocfilehash: e81cca3e20d5b6c050489e80b91d013d5e934cce
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81453208"
---
# <a name="work-with-r-and-sql-data-in-azure-sql-database-machine-learning-services-preview"></a>Azure SQL Veritabanı Makine Öğrenme Hizmetleri'nde R ve SQL verileriyle çalışma (önizleme)

Bu makalede, [Azure SQL Veritabanı'nda Machine Learning Services'da (R ile)](sql-database-machine-learning-services-overview.md)R ve SQL Veritabanı arasında veri taşınırken karşılaşabileceğiniz sık karşılaşılan bazı sorunlar açıklanmaktadır. Bu alıştırma yla kazandığınız deneyim, kendi komut dosyanızdaki verilerle çalışırken temel arka planı sağlar.

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

Karşılaşabileceğiniz sık karşılaşılabilecek sorunlar şunlardır:

- Veri türleri bazen eşleşmiyor
- Örtülü dönüşümler gerçekleşebilir
- Döküm ve dönüştürme işlemleri bazen gereklidir
- R ve SQL farklı veri nesneleri kullanın

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa, başlamadan önce [bir hesap oluşturun.](https://azure.microsoft.com/free/)

- Bu alıştırmalarda örnek kodu çalıştırmak için öncelikle [Machine Learning Services (R ile) özellikli Azure SQL Veritabanı'nı](sql-database-machine-learning-services-overview.md) etkinleştirmeniz gerekir.

- En son [SQL Server Management Studio'yu](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) yüklediğinizden emin olun. Diğer veritabanı yönetimi veya sorgu araçlarını kullanarak R komut dosyalarını çalıştırabilirsiniz, ancak bu hızlı başlatmada SSMS'i kullanırsınız.

## <a name="working-with-a-data-frame"></a>Veri çerçevesi ile çalışma

Komut dosyanız Sonuçları R'den SQL'e döndürdüğünde, **verileri data.frame**olarak döndürmesi gerekir. Komut dosyanızda oluşturduğunuz diğer nesne türlerinde (ister liste, faktör, vektör veya ikili veri olsun- depolanan yordam sonuçlarının bir parçası olarak çıktıalmak istiyorsanız veri çerçevesine dönüştürülmelidir. Neyse ki, diğer nesnelerin bir veri çerçevesine değiştirilmesini destekleyen birden çok R işlevi vardır. Hatta bir ikili modeli seri hale getirip, bu makalede daha sonra yapacağınız bir veri çerçevesi içinde döndürebilirsiniz.

İlk olarak, bazı temel R nesnelerini deneyelim - vektörler, matrisler ve listeler - ve veri çerçevesine dönüştürmenin SQL'e geçen çıktıyı nasıl değiştirdiğini görelim.

R bu iki "Hello World" komut karşılaştırın. Komut dosyaları hemen hemen aynı görünür, ancak ilk üç değerden oluşan tek bir sütun döndürür, ikinci ise her biri tek bir değere sahip üç sütun döndürür.

**Örnek 1**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
'
    , @input_data_1 = N'';
```

**Örnek 2**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N' OutputDataSet<- data.frame(c("hello"), " ", c("world"));'
    , @input_data_1 = N'';
```

Sonuçlar neden bu kadar farklı?

Yanıt genellikle R `str()` komutu kullanılarak bulunabilir. Belirtilen R `str(object_name)` nesnesinin veri şemasını bilgilendirme iletisi olarak döndürecek şekilde R komut dosyanızdaki herhangi bir yere işlevi ekleyin. İletileri SSMS'teki **Mesajlar** sekmesinde görüntüleyebilirsiniz.

Örnek 1 ve Örnek 2'nin neden bu kadar `str(OutputDataSet)` farklı sonuçlara `@script` sahip olduğunu anlamak için, her deyimdeki değişken tanımının sonuna satırı ekleyin:

**Str fonksiyonu ekli Örnek 1**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

**Str fonksiyonu eklendi örnek 2**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(c("hello"), " ", c("world"));
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

Şimdi, çıktının neden farklı olduğunu görmek için **Mesajlar'daki** metni gözden geçirin.

**Sonuçlar - Örnek 1**

```text
STDOUT message(s) from external script:
'data.frame':    3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Sonuçlar - Örnek 2**

```text
STDOUT message(s) from external script:
'data.frame':    1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Gördüğünüz gibi, R sözdiziminde yapılan küçük bir değişiklik sonuçların şeması üzerinde büyük bir etkiye sahipti. Tüm ayrıntılar için, R veri türleri farklılıkları [Hadley Wickham tarafından "Advanced R"](http://adv-r.had.co.nz) *Veri Yapıları* bölümünde ayrıntılı olarak açıklanmıştır.

Şimdilik, R nesnelerini veri çerçevelerine zorlarken beklenen sonuçları kontrol etmeniz gerektiğini unutmayın.

> [!TIP]
> Ayrıca, iç veri yapısı hakkında `is.matrix` `is.vector`bilgi döndürmek için R kimlik işlevlerini de kullanabilirsiniz.

## <a name="implicit-conversion-of-data-objects"></a>Veri nesnelerinin örtülü dönüşümü

Her R veri nesnesinin, iki veri nesnesi aynı sayıda boyuta sahipse veya herhangi bir veri nesnesi heterojen veri türleri içeriyorsa, diğer veri nesneleriyle birleştirildiğinde değerlerin nasıl işlendiğine dair kendi kuralları vardır.

Örneğin, R kullanarak matris çarpımı gerçekleştirmek istediğinizi varsayalım. Tek sütunlu bir matrisi üç değerle dört değere sahip bir diziyle çarpmak ve sonuç olarak 4x3 matris beklemek istiyorsunuz.

İlk olarak, küçük bir test verisi tablosu oluşturun.

```sql
CREATE TABLE RTestData (col1 INT NOT NULL)

INSERT INTO RTestData
VALUES (1);

INSERT INTO RTestData
VALUES (10);

INSERT INTO RTestData
VALUES (100);
GO
```

Şimdi aşağıdaki komut dosyasını çalıştırın.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:15);
OutputDataSet <- as.data.frame(x %*% y);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col1] INT
            , [Col2] INT
            , [Col3] INT
            , Col4 INT
            ));
```

Kapakların altında, üç değerden oluşan sütun tek sütunlu bir matrise dönüştürülür. Matris, R'deki bir dizinin özel bir örneği `y` olduğundan, dizi iki bağımsız değişkeni uyumlu hale getirmek için örtülü olarak tek sütunlu bir matrise zorlanır.

**Sonuçlar**

|Süt1|Süt2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1500|

Ancak, dizinin `y`boyutunu değiştirdiğinizde ne olur dikkat edin.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
x <- as.matrix(InputDataSet);
y <- array(12:14);
OutputDataSet <- as.data.frame(y %*% x);
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS(([Col1] INT));
```

Şimdi R sonuç olarak tek bir değer döndürür.

**Sonuçlar**
    
|Süt1|
|---|
|1542|

Neden? Bu durumda, iki bağımsız değişken aynı uzunluktaki vektörler olarak işlenebileceğinden, R iç ürünü matris olarak döndürür.  Bu lineer cebir kurallarına göre beklenen davranıştır. Ancak, downstream uygulamanız çıktı şemasının asla değişmemesini bekliyorsa sorunlara neden olabilir!

## <a name="merge-or-multiply-columns-of-different-length"></a>Farklı uzunluktaki sütunları birleştirme veya çoğaltma

R, farklı boyutlardaki vektörlerle çalışmak ve bu sütun benzeri yapıları veri çerçeveleriyle birleştirmek için büyük esneklik sağlar. Vektör listeleri bir tablo gibi görünebilir, ancak veritabanı tablolarını yöneten tüm kurallara uymaz.

Örneğin, aşağıdaki komut dosyası 6 uzunluğunda numerik bir dizi tanımlar `df1`ve R değişkeninde depolar. Sayısal dizi daha sonra yeni bir veri çerçevesi yapmak için üç (3) değer içeren RTestData tablosunun (yukarıda `df2`oluşturulmuş) tamsayılarla birleştirilir.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
df1 <- as.data.frame( array(1:6) );
df2 <- as.data.frame( c( InputDataSet , df1 ));
OutputDataSet <- df2;
'
    , @input_data_1 = N' SELECT [Col1]  from RTestData;'
WITH RESULT SETS((
            [Col2] INT NOT NULL
            , [Col3] INT NOT NULL
            ));
```

Veri çerçevesini doldurmak için R, RTestData'dan alınan öğeleri dizideki `df1`öğe sayısını eşleştirmek için gerektiği kadar yineler.

**Sonuçlar**
    
|*Süt2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

Bir veri çerçevesinin yalnızca bir tablogibi göründüğünü, ancak aslında vektörlerin bir listesi olduğunu unutmayın.

## <a name="cast-or-convert-sql-data"></a>SQL verilerini dökme veya dönüştürme

R ve SQL aynı veri türlerini kullanmaz, bu nedenle veri almak için SQL'de bir sorgu çalıştırDığınızda ve bunu R çalışma süresine aktardığınızda, genellikle bir tür örtük dönüştürme gerçekleşir. R'den SQL'e veri döndürdüğünde başka bir dönüşüm kümesi gerçekleşir.

- SQL sorgudaki verileri R işlemine iter ve daha fazla verimlilik için bir iç gösterime dönüştürür.
- R çalışma zamanı verileri bir veri.frame değişkenine yükler ve veriler üzerinde kendi işlemlerini gerçekleştirir.
- Veritabanı altyapısı güvenli bir iç bağlantı kullanarak verileri SQL'e döndürür ve verileri SQL veri türleri açısından sunar.
- SQL sorguları verebilir ve tabular veri kümeleri işleyebilir bir istemci veya ağ kitaplığı kullanarak SQL bağlanarak verileri alırsınız. Bu istemci uygulaması verileri başka şekillerde etkileyebilir.

Bunun nasıl çalıştığını görmek için [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) veri ambarında bunun gibi bir sorgu çalıştırın. Bu görünüm, tahminler oluştururken kullanılan satış verilerini döndürür.

```sql
USE AdventureWorksDW
GO

SELECT ReportingDate
    , CAST(ModelRegion AS VARCHAR(50)) AS ProductSeries
    , Amount
FROM [AdventureWorksDW].[dbo].[vTimeSeries]
WHERE [ModelRegion] = 'M200 Europe'
ORDER BY ReportingDate ASC
```

> [!NOTE]
> AdventureWorks'ün herhangi bir sürümünü kullanabilir veya kendi veritabanınızı kullanarak farklı bir sorgu oluşturabilirsiniz. Önemli olan, metin, datetime ve sayısal değerler içeren bazı verileri işlemeye çalışmaktır.

Şimdi, bu sorguyu depolanan yordamın girişi olarak kullanmayı deneyin.

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
str(InputDataSet);
OutputDataSet <- InputDataSet;
'
    , @input_data_1 = N'
           SELECT ReportingDate
         , CAST(ModelRegion as varchar(50)) as ProductSeries
         , Amount
           FROM [AdventureWorksDW].[dbo].[vTimeSeries]
           WHERE [ModelRegion] = ''M200 Europe''
           ORDER BY ReportingDate ASC ;'
WITH RESULT SETS undefined;
```

Bir hata alırsanız, büyük olasılıkla sorgu metninde bazı denetimler yapmanız gerekir. Örneğin, WHERE yan tümcesindeki dize yüklemi, iki tek tırnak işareti kümesiyle eklenmelidir.

Sorguyu çalıştırdıktan sonra, R'nin `str` giriş verilerini nasıl işlediğini görmek için işlevin sonuçlarını gözden geçirin.

**Sonuçlar**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- Datetime sütunu R veri türü, **POSIXct**kullanılarak işlenmiştir.
- Metin sütunu "ProductSeries" kategorik bir değişken anlamına gelen bir **faktör**olarak tanımlanmıştır. Dize değerleri varsayılan olarak etken olarak işlenir. Bir dizeyi R'ye geçirirseniz, dahili kullanım için bir tamsayıya dönüştürülür ve çıktıüzerindeki dizeye eşlenir.

## <a name="summary"></a>Özet

Bu kısa örneklerden bile, SQL sorgularını giriş olarak geçerken veri dönüştürmenin etkilerini denetleme gereksinimini görebilirsiniz. Bazı SQL veri türleri R tarafından desteklenmedığından, hataları önlemek için şu yolları göz önünde bulundurun:

- Verilerinizi önceden test edin ve şemanızdaki R koduna geçirildiğinde sorun olabilecek sütunları veya değerleri doğrulayın.
- Giriş veri kaynağınızdaki sütunları kullanmak `SELECT *`yerine tek tek belirtin ve her sütunun nasıl işleneceğini bilin.
- Sürprizlerden kaçınmak için giriş verilerinizi hazırlarken gerektiği gibi açık dökümler gerçekleştirin.
- Hatalara neden olan ve modelleme için yararlı olmayan veri sütunlarını (GUIDS veya satır kılavuzları gibi) geçirmekten kaçının.

Desteklenen ve desteklenmeyen R veri türleri hakkında daha fazla bilgi için [Bkz.](/sql/advanced-analytics/r/r-libraries-and-data-types)
