---
title: R ve SQL veri türleri ve nesneleriyle çalışma
titleSuffix: Azure SQL Database Machine Learning Services (preview)
description: Karşılaşabileceğiniz yaygın sorunlar da dahil olmak üzere Machine Learning Services (Önizleme) kullanarak Azure SQL veritabanı ile R 'deki veri türleri ve veri nesneleriyle nasıl çalışacağınızı öğrenin.
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
ms.openlocfilehash: 7dfd12729c5697d1935d098cbd4ed863a4551acd
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76719883"
---
# <a name="work-with-r-and-sql-data-in-azure-sql-database-machine-learning-services-preview"></a>Azure SQL veritabanı 'nda R ve SQL verileriyle çalışma Machine Learning Services (Önizleme)

Bu makalede, verileri [Azure SQL veritabanı 'nda Machine Learning Services (r ile)](sql-database-machine-learning-services-overview.md)içinde r ve SQL veritabanı arasında taşırken karşılaşabileceğiniz bazı yaygın sorunlar ele alınmaktadır. Bu alıştırmada elde ettiğiniz deneyim, kendi betikinizdeki verilerle çalışırken önemli bir arka plan sağlar.

Karşılaşabileceğiniz yaygın sorunlar şunlardır:

- Veri türleri bazen eşleşmez
- Örtük dönüştürmeler gerçekleşmeyebilir
- Atama ve dönüştürme işlemleri bazen gereklidir
- R ve SQL farklı veri nesneleri kullanma

[!INCLUDE[ml-preview-note](../../includes/sql-database-ml-preview-note.md)]

## <a name="prerequisites"></a>Önkoşullar

- Azure aboneliğiniz yoksa başlamadan önce [bir hesap oluşturun](https://azure.microsoft.com/free/) .

- Örnek kodu bu alıştırmalarda çalıştırmak için öncelikle Machine Learning Services (R ile) etkin bir Azure SQL veritabanınızın olması gerekir. Microsoft, genel önizleme sırasında, mevcut veya yeni veritabanınız için sizi kullanıma sunulacaktır ve makine öğrenimini etkinleştirecektir. [Önizlemeye kaydolma](sql-database-machine-learning-services-overview.md#signup)adımlarını izleyin.

- En son [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/sql-server-management-studio-ssms) (SSMS) yüklediğinizden emin olun. R komut dosyalarını diğer veritabanı yönetimini veya sorgu araçlarını kullanarak çalıştırabilirsiniz, ancak bu hızlı başlangıçta SSMS 'yi kullanacaksınız.

## <a name="working-with-a-data-frame"></a>Veri çerçevesiyle çalışma

Betiğinizdeki sonuçları R 'den SQL 'e döndürürse, verileri **Data. Frame**olarak döndürmelidir. Betikte oluşturduğunuz diğer nesne türleri (liste, faktör, vektör veya ikili veriler gibi), saklı yordam sonuçlarının bir parçası olarak çıktısını almak istiyorsanız bir veri çerçevesine dönüştürülüp dönüştürülmemelidir. Neyse ki, diğer nesneleri bir veri çerçevesine değiştirmeyi desteklemeye yönelik birden çok R işlevi vardır. Hatta bir ikili modeli seri hale getirebilirsiniz ve bu makalede daha sonra kullanacağınız bir veri çerçevesinde döndürebilirsiniz.

İlk olarak, bazı temel R nesneleriyle (vektör, matrisler ve listeler) deneme yapın ve bir veri çerçevesine dönüştürmenin SQL 'e geçirilen çıktıyı nasıl değiştirdiğini görün.

Bu iki "Merhaba Dünya" komut dosyasını R 'de karşılaştırın. Betikler neredeyse özdeş, ancak birincisi üç değerden oluşan tek bir sütunu döndürür, ikincisi ise tek bir değere sahip üç sütun döndürür.

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

Sonuçlar neden farklı?

Yanıt genellikle R `str()` komutu kullanılarak bulunabilir. Belirtilen R nesnesinin veri şemasına bir bilgi iletisi olarak döndürüldüğünden, işlevi R betiğinizde herhangi bir yere `str(object_name)` ekleyin. İletileri SSMS 'deki **iletiler** sekmesinde görüntüleyebilirsiniz.

Örnek 1 ve örnek 2 ' nin bu tür farklı sonuçlara neden olduğunu anlamak için, aşağıdaki gibi her bir deyime `@script` değişken tanımının sonuna satır `str(OutputDataSet)` ekleyin:

**Str işlevi eklenen örnek 1**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
mytextvariable <- c("hello", " ", "world");
OutputDataSet <- as.data.frame(mytextvariable);
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

**Str işlevi eklenen 2 örnek 2**

```sql
EXECUTE sp_execute_external_script @language = N'R'
    , @script = N'
OutputDataSet <- data.frame(c("hello"), " ", c("world"));
str(OutputDataSet);
'
    , @input_data_1 = N'  ';
```

Şimdi çıktının nasıl farklı olduğunu görmek için **iletilerdeki** metni gözden geçirin.

**Sonuçlar-örnek 1**

```text
STDOUT message(s) from external script:
'data.frame':   3 obs. of  1 variable:
$ mytextvariable: Factor w/ 3 levels " ","hello","world": 2 1 3
```

**Sonuçlar-örnek 2**

```text
STDOUT message(s) from external script:
'data.frame':   1 obs. of  3 variables:
$ c..hello..: Factor w/ 1 level "hello": 1
$ X...      : Factor w/ 1 level " ": 1
$ c..world..: Factor w/ 1 level "world": 1
```

Gördüğünüz gibi, R sözdiziminde küçük bir değişiklik sonuçların şeması üzerinde büyük bir etkiye sahiptir. Tüm ayrıntılar için R veri türlerindeki farklılıklar, [Hadley Wickhemi tarafından "Advanced R"](http://adv-r.had.co.nz)Içindeki *veri yapıları* bölümündeki Ayrıntılar bölümünde açıklanmaktadır.

Şimdilik, veri çerçevelerine zorlama R nesneleri olduğunda beklenen sonuçları denetlemeniz gerektiğini unutmayın.

> [!TIP]
> Ayrıca, iç veri yapısı hakkında bilgi döndürmek için `is.matrix`, `is.vector`gibi R Identity işlevlerini de kullanabilirsiniz.

## <a name="implicit-conversion-of-data-objects"></a>Veri nesnelerinin örtük dönüştürmesi

Her R veri nesnesi, iki veri nesnesi aynı sayıda boyuta sahipse veya herhangi bir veri nesnesi heterojen veri türleri içeriyorsa, diğer veri nesneleriyle birleştirildiğinde değerlerin nasıl işlendiğine ilişkin kendi kurallarına sahiptir.

Örneğin, R kullanarak matris çarpma gerçekleştirmek istediğinizi varsayın. Tek sütunlu matrisi dört değerli bir dizi ile üç değerle çarpmak ve sonuç olarak bir 4x3 matrisi beklemeniz gerekir.

İlk olarak, küçük bir test verileri tablosu oluşturun.

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

Şimdi aşağıdaki betiği çalıştırın.

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

Kapsıyorsa, üç değerin sütunu tek sütunlu bir matrise dönüştürülür. Matris yalnızca R 'deki bir dizide özel bir durumdur çünkü, iki bağımsız değişkeni uyumlu hale getirmek için dizi `y` örtük olarak tek sütunlu bir matriste.

**Sonuçlar**

|Süt|Col2|Col3|Col4|
|---|---|---|---|
|12|13|14|15|
|120|130|140|150|
|1200|1300|1400|1500|

Ancak, `y`dizisinin boyutunu değiştirdiğinizde ne olacağını aklınızda bulabilirsiniz.

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

Şimdi R, sonuç olarak tek bir değer döndürür.

**Sonuçlar**
    
|Süt|
|---|
|1542|

Neden? Bu durumda, iki bağımsız değişken aynı uzunlukta vektör olarak işlenebildiğinden, R iç ürünü matris olarak döndürür.  Bu, doğrusal algeköşeli kurallara göre beklenen davranıştır. Ancak, aşağı akış uygulamanız çıkış şemasının hiçbir şekilde değişmemesi durumunda sorun oluşmasına neden olabilir!

## <a name="merge-or-multiply-columns-of-different-length"></a>Farklı uzunluktaki sütunları birleştirin veya çarpın

R, farklı boyutlardaki vektörlerle çalışmak ve bu sütun benzeri yapıları veri çerçevelerine birleştirmek için harika esneklik sağlar. Vektör listeleri tablo gibi görünebilir, ancak veritabanı tablolarını yöneten tüm kurallara uymalıdır.

Örneğin, aşağıdaki betik 6 uzunluğunda bir sayısal diziyi tanımlar ve `df1`R değişkeninde depolar. Sayısal dizi daha sonra, üç (3) değer içeren RTestData tablosunun (yukarıda oluşturulan) tamsayılarla birleştirilir ve yeni bir veri çerçevesini `df2`.

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

R, veri çerçevesini doldurmak için, RTestData 'dan alınan öğeleri, dizi `df1`öğe sayısıyla eşleştirmek için gereken sayıda yineler.

**Sonuçlar**
    
|*Col2*|*Col3*|
|----|----|
|1|1|
|10|2|
|100|3|
|1|4|
|10|5|
|100|6|

Bir veri çerçevesinin yalnızca bir tablo gibi göründüğünü ve aslında vektör listesi olduğunu unutmayın.

## <a name="cast-or-convert-sql-data"></a>SQL verilerini atama veya dönüştürme

R ve SQL aynı veri türlerini kullanmaz. bu nedenle, verileri almak için SQL 'de bir sorgu çalıştırıp daha sonra bunu R çalışma zamanına geçirirseniz, genellikle bir tür örtük dönüştürme gerçekleşir. R 'den SQL 'e veri döndürdüğünüzde başka dönüştürmeler kümesi gerçekleşir.

- SQL, verileri sorgudan R işlemine gönderir ve daha fazla verimlilik için bir iç gösterimle dönüştürür.
- R çalışma zamanı verileri bir Data. Frame değişkenine yükler ve verileri üzerinde kendi işlemlerini gerçekleştirir.
- Veritabanı altyapısı, güvenli bir iç bağlantı kullanarak verileri SQL 'e döndürür ve verileri SQL veri türleri bakımından gösterir.
- SQL sorguları yayınlayarak tablo veri kümelerini işleyemeyen bir istemciyi veya ağ kitaplığını kullanarak SQL 'e bağlanarak verileri alırsınız. Bu istemci uygulaması, verileri başka yollarla etkileyebilir.

Bunun nasıl çalıştığını görmek için, [AdventureWorksDW](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) veri ambarında bir sorgu çalıştırın. Bu görünüm, tahminler oluşturmak için kullanılan satış verilerini döndürür.

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
> Herhangi bir AdventureWorks sürümünü kullanabilir veya kendi veritabanınızı kullanarak farklı bir sorgu oluşturabilirsiniz. Nokta, metin, tarih saat ve sayısal değerler içeren bazı verileri işlemeye çalışır.

Şimdi, saklı yordamın girişi olarak bu sorguyu kullanmayı deneyin.

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

Bir hata alırsanız, muhtemelen sorgu metninde bazı düzenlemeler yapmanız gerekir. Örneğin, WHERE yan tümcesindeki dize koşulunun iki tek tırnak işareti kümesi içine alınması gerekir.

Sorguyu çalıştıktan sonra, R 'nin giriş verilerini nasıl değerlendirçalıştığını görmek için `str` işlevin sonuçlarını gözden geçirin.

**Sonuçlar**

```text
STDOUT message(s) from external script: 'data.frame':    37 obs. of  3 variables:
STDOUT message(s) from external script: $ ReportingDate: POSIXct, format: "2010-12-24 23:00:00" "2010-12-24 23:00:00"
STDOUT message(s) from external script: $ ProductSeries: Factor w/ 1 levels "M200 Europe",..: 1 1 1 1 1 1 1 1 1 1
STDOUT message(s) from external script: $ Amount       : num  3400 16925 20350 16950 16950
```

- DateTime sütunu R veri türü, **Posixct**kullanılarak işlendi.
- "ProductSeries" metin sütunu bir **faktör**olarak tanımlanmıştır ve bu da kategorik bir değişken anlamına gelir. Dize değerleri varsayılan olarak faktör olarak işlenir. R 'ye bir dize geçirirseniz, iç kullanım için bir tamsayıya dönüştürülür ve sonra çıktıda dizeye geri eşlenir.

## <a name="summary"></a>Özet

Bu kısa örneklerden bile, SQL sorgularını giriş olarak geçirirken veri dönüştürmesinin etkilerini kontrol etme gereksinimini de görebilirsiniz. Bazı SQL veri türleri R tarafından desteklenmediğinden, hatalardan kaçınmak için şu yolları göz önünde bulundurun:

- Verilerinizi önceden test edin ve Þemada R koduna geçerken bir sorun olabilecek sütunları veya değerleri doğrulayın.
- `SELECT *`kullanmak yerine giriş veri kaynağınızdaki sütunları ayrı ayrı belirtin ve her bir sütunun nasıl işleneceğini öğrenin.
- Giriş verilerinizi hazırlarken, sürprizleri önlemek için gerekli olan açık yayınları gerçekleştirin.
- Hatalara neden olan ve modelleme için faydalı olmayan veri sütunlarını (GUID 'ler veya ROWGUID 'ler gibi) geçirmekten kaçının.

Desteklenen ve desteklenmeyen R veri türleri hakkında daha fazla bilgi için bkz. [r kitaplıkları ve veri türleri](/sql/advanced-analytics/r/r-libraries-and-data-types).
