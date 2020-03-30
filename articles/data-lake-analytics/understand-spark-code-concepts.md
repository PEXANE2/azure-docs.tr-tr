---
title: Azure Veri Gölü Analytics U-SQL geliştiricileri için Apache Spark kod kavramlarını anlayın.
description: Bu makalede, U-SQL geliştiricilerinin Spark kodu kavramlarını anlamalarına yardımcı olmak için Apache Spark kavramları açıklanmaktadır.
author: guyhay
ms.author: guyhay
ms.reviewer: jasonh
ms.service: data-lake-analytics
ms.topic: conceptual
ms.custom: Understand-apache-spark-code-concepts
ms.date: 10/15/2019
ms.openlocfilehash: bdb38e36a9f1344a3adde15d349a2ec176c0fe95
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74424012"
---
# <a name="understand-apache-spark-code-for-u-sql-developers"></a>U-SQL geliştiricileri için Apache Spark kodunu anlama

Bu bölümde, U-SQL Komut Dosyalarının Apache Spark'a dönüştürülmesi konusunda üst düzey kılavuz sağlar.

- Bu iki [dilin işleme paradigmaları](#understand-the-u-sql-and-spark-language-and-processing-paradigms) bir karşılaştırma ile başlar
- Nasıl yapılabilen ipuçları sağlar:
   - [U-SQL'in rowset ifadelerini](#transform-u-sql-rowset-expressions-and-sql-based-scalar-expressions) içeren [komut dosyalarını dönüştürme](#transform-u-sql-scripts)
   - [.NET kodu](#transform-net-code)
   - [Veri türleri](#transform-typed-values)
   - [Katalog nesneleri](#transform-u-sql-catalog-objects).

## <a name="understand-the-u-sql-and-spark-language-and-processing-paradigms"></a>U-SQL ve Kıvılcım dil ve işleme paradigmalarını anlama

Azure Data Lake Analytics'in U-SQL komut dosyalarını Spark'a geçirmeden önce, iki sistemdeki genel dili ve işleme felsefelerini anlamak yararlıdır.

U-SQL, veri akışı paradigması kullanan ve .NET (c#), Python ve R'de yazılmış kullanıcı kodunu kolayca gömmenize ve ölçeklendirmenize olanak tanıyan SQL benzeri bir bildirimsel sorgu dilidir. Kullanıcı uzantıları basit ifadeler veya kullanıcı tanımlı işlevler uygulayabilir, ancak aynı zamanda kullanıcıya rowset düzey dönüşümleri, çıkarmalar ve yazma çıktısı gerçekleştirmek için özel işleçler uygulayan kullanıcı tanımlı işleçleri uygulama olanağı da sağlayabilir.

Spark, Scala, Java, Python, .NET vb. dillerde öncelikle kodunuzu bu dillerden birine yazdığınız, esnek dağıtılmış veri kümeleri (RDD), veri çerçeveleri ve veri kümeleri adı verilen veri soyutlamaları oluşturduğunuz çeşitli dil ciltleri sunan ölçeklenmiş bir çerçevedir ve sonra bunları dönüştürmek için LINQ benzeri etki alanına özgü bir dil (DSL) kullanın. Ayrıca, veri çerçevesi ve veri kümesi soyutlamaları üzerinde bildirimsel bir alt dil olarak SparkSQL sağlar. DSL iki işlem, dönüşüm ve eylem kategorisi sağlar. Veri soyutlamalarına dönüşüm uygulamak dönüşümü yürütmez, bunun yerine bir eylemle değerlendirilmek üzere sunulacak yürütme planını oluşturur (örneğin, sonucu geçici bir tabloya veya dosyaya yazmak veya sonuç olarak).

Bu nedenle, bir U-SQL komut dosyasını Bir Kıvılcım programına çevirirken, en azından veri çerçevesi soyutlamasını oluşturmak için hangi dili kullanmak istediğinize (şu anda en sık kullanılan veri soyutlama) ve bildirimsel komut dosyası yazmak isteyip istemediğinize karar vermeniz gerekecektir. DSL veya SparkSQL kullanarak veri akışı dönüşümleri. Daha karmaşık bazı durumlarda, U-SQL komut dosyanızı Azure Toplu İş veya Azure İşfonksiyonları ile uygulanan bir Spark dizisine ve diğer adımlara bölmeniz gerekebilir.

Ayrıca, Azure Data Lake Analytics sunucusuz bir iş hizmeti ortamında U-SQL sunarken, hem Azure Databrick'leri hem de Azure HDInsight, Spark'ı bir küme hizmeti biçiminde sunar. Uygulamanızı dönüştürürken, kümeleri şimdi oluşturma, boyutlandırma, ölçeklendirme ve devre dışı bırakmanın sonuçlarını dikkate almanız gerekir.

## <a name="transform-u-sql-scripts"></a>U-SQL komut dosyalarını dönüştürme

U-SQL komut dosyaları aşağıdaki işleme deseni izleyin:

1. Veriler, `EXTRACT` ifade, konum veya dosya kümesi belirtimi ve yerleşik veya kullanıcı tanımlı çıkarıcı ve istenen şema kullanılarak yapılandırılmamış dosyalardan veya U-SQL tablolarından (yönetilen veya harici tablolar) okunur. Bir satır kümesi olarak temsil edilir.
2. Satır kümeleri, U-SQL ifadelerini satır kümelerine uygulayan ve yeni satır kümeleri üreten birden çok U-SQL deyiminde dönüştürülür.
3. Son olarak, ortaya çıkan satır kümeleri, `OUTPUT` konum(lar) ve yerleşik veya kullanıcı tanımlı çıktılayıcıyı belirten deyimi kullanarak veya U-SQL tablosuna girerek dosyalara çıkar.

Komut dosyası tembelce değerlendirilir, yani her çıkarma ve dönüştürme adımı bir ifade ağacına oluşur ve küresel olarak değerlendirilir (veri akışı).

Kıvılcım programları, verileri okumak ve veri çerçevelerini oluşturmak için Spark bağlayıcılarını kullanmanız, ardından LINQ benzeri DSL veya SparkSQL kullanarak veri çerçevelerindeki dönüşümleri uygulayacağınız ve sonucu dosyalara, geçici Kıvılcım tablolarına yazacağınız şeklinde benzerdir. bazı programlama dili türleri veya konsol.

## <a name="transform-net-code"></a>.NET kodunu dönüştürün

U-SQL'in ifade dili C# olup, özel .NET kodunu ölçeklendirmek için çeşitli yollar sunar.

Spark şu anda .NET kodunun yürütülmesini yerel olarak desteklemediğinden, ifadelerinizi eşdeğer bir Spark, Scala, Java veya Python ifadesine yeniden yazmanız veya .NET kodunuzu aramanın bir yolunu bulmanız gerekir. Komut dosyanız .NET kitaplıklarını kullanıyorsa, aşağıdaki seçeneklere sahipsiniz:

- .NET kodunuzu Scala veya Python'a çevirin.
- U-SQL komut dosyanızı,.NET dönüşümlerini uygulamak için Azure Toplu İşlemlerini kullandığınız (kabul edilebilir ölçek elde edebilirseniz) birkaç adıma bölün
- Açık Kaynak kodlu Moebius'ta bulunan .NET dil bağlama yı kullanın. Bu proje desteklenen bir durumda değildir.

Her durumda, U-SQL komut dosyalarınızda büyük miktarda .NET mantığı nız varsa, daha fazla kılavuz için lütfen Microsoft Hesabı temsilciniz aracılığıyla bize ulaşın.

Aşağıdaki ayrıntılar U-SQL komut dosyalarındaki .NET ve C# kullanımlarının farklı durumları içindir.

### <a name="transform-scalar-inline-u-sql-c-expressions"></a>Skaler satır içi U-SQL C# ifadelerini dönüştür

U-SQL'in ifade dili C#'dır. Skaler satır içi U-SQL ifadelerinin çoğu gelişmiş performans için yerel olarak uygulanırken, .NET çerçevesine çağrılar ekerek daha karmaşık ifadeler yürütülebilir.

Spark kendi skaler ifade dili (DSL veya SparkSQL bir parçası olarak) vardır ve kullanıcı tanımlı işlevleri barındırma dilinde yazılmış çağrı sağlar.

U-SQL'de skaler ifadeler varsa, en iyi performansı elde etmek için önce en uygun yerel olarak anlaşılan Kıvılcım ifadesini bulmalı ve ardından diğer ifadeleri seçtiğiniz Spark barındırma dilinin kullanıcı tanımlı işlevine eşlemelisiniz.

.NET ve C#'ın, Spark barındırma dillerinden ve Spark'ın DSL'sinden farklı türsemantiklere sahip olduğunu unutmayın. Tür sistemi farklılıkları hakkında daha fazla bilgi için [aşağıya](#transform-typed-values) bakın.

### <a name="transform-user-defined-scalar-net-functions-and-user-defined-aggregators"></a>Kullanıcı tanımlı skaler .NET fonksiyonlarını ve kullanıcı tanımlı toplayıcıları dönüştürün

U-SQL rasgele skaler .NET işlevlerini çağırmanın ve .NET'te yazılmış kullanıcı tanımlı toplayıcıları çağırmanın yollarını sağlar.

Spark ayrıca, Spark'ın DSL ve SparkSQL'inden çağrılabilen barındırma dillerinin çoğunda yazılmış kullanıcı tanımlı işlevler ve kullanıcı tanımlı toplayıcılar için destek de sunar.

### <a name="transform-user-defined-operators-udos"></a>Kullanıcı tanımlı operatörleri (UdOs) dönüştürme

U-SQL, .NET 'de (ve bir dereceye kadar Python ve R'de) yazılabilen ayıklayıcılar, çıktılar, indirgeyiciler, işlemciler, appliers ve birleştiriciler gibi kullanıcı tanımlı işleçler (UdOs) kategorileri sağlar.

Kıvılcım operatörler için aynı genişletilebilirlik modelini sunmaz, ancak bazıları için eşdeğer özelliklere sahiptir.

Çıkarıcılara ve çıkışlara eşdeğer kıvılcım, Kıvılcım konektörleridir. Birçok U-SQL çıkarıcıiçin Spark topluluğunda eşdeğer bir bağlayıcı bulabilirsiniz. Diğerleri için, özel bir bağlayıcı yazmak zorunda kalacak. U-SQL ekstraktörü karmaşıksa ve birkaç .NET kitaplığı kullanıyorsa, Scala'da verilerin gerçek işlenmesini yapan .NET kitaplığına çağırmak için interop kullanan bir bağlayıcı oluşturmak tercih edilebilir. Bu durumda, .NET Core çalışma zamanını Spark kümesine dağıtmanız ve başvurulan .NET kitaplıklarının .NET Standart 2.0 uyumlu olduğundan emin olmak gerekir.

Diğer U-SQL UdO türlerinin kullanıcı tanımlı işlevler ve toplayıcılar ve semantik olarak uygun Spark DLS veya SparkSQL ifadesi kullanılarak yeniden yazılması gerekir. Örneğin, bir işlemci, bir veri çerçevesini bağımsız değişken olarak alan ve bir veri çerçevesi döndüren bir işlev olarak paketlenmiş çeşitli UDF çağrılarının SELECT'ine eşlenebilir.

### <a name="transform-u-sqls-optional-libraries"></a>U-SQL'in isteğe bağlı kitaplıklarını dönüştürün

U-SQL [Python,](data-lake-analytics-u-sql-python-extensions.md) [R](data-lake-analytics-u-sql-r-extensions.md), [JSON, XML, AVRO desteği](https://github.com/Azure/usql/tree/master/Examples/DataFormats)ve bazı bilişsel hizmetler yetenekleri sunan isteğe bağlı ve demo kitaplıklar bir dizi sağlar. [cognitive services capabilities](data-lake-analytics-u-sql-cognitive.md)

Spark sırasıyla kendi Python ve R entegrasyonunu, pySpark ve SparkR'ı sunar ve JSON, XML ve AVRO'nun okuma ve yazma konektörlerini sağlar.

Bilişsel hizmetler kitaplıklarına atıfta bulunan bir komut dosyasını dönüştürmeniz gerekiyorsa, Microsoft Hesap temsilciniz aracılığıyla bizimle iletişime geçmenizi öneririz.

## <a name="transform-typed-values"></a>Yazılan değerleri dönüştürme

U-SQL'in tür sistemi .NET türü sistemine dayandığından ve Spark'ın ana bilgisayar dil bağlamadan etkilenen kendi tür sistemine sahip olduğundan, üzerinde çalıştığınız türlerin yakın olduğundan ve belirli türler için tür aralıklarının, hassasiyetin ve/veya ölçeğin biraz farklı olabileceğinden emin olmanız gerekir. Ayrıca, U-SQL ve Spark değerleri farklı şekilde ele alar. `null`

### <a name="data-types"></a>Veri türleri

Aşağıdaki tablo, verilen U-SQL türleri için Spark, Scala ve PySpark'taki eşdeğer türleri verir.

| U-SQL | Spark |  Scala | PySpark |
| ------ | ------ | ------ | ------ |
|`byte`       ||||
|`sbyte`      |`ByteType` |`Byte` | `ByteType`|
|`int`        |`IntegerType` |`Int` | `IntegerType`|
|`uint`       ||||
|`long`       |`LongType` |`Long` | `LongType`|
|`ulong`      ||||
|`float`      |`FloatType` |`Float` | `FloatType`|
|`double`     |`DoubleType` |`Double` | `DoubleType`|
|`decimal`    |`DecimalType` |`java.math.BigDecimal` | `DecimalType`|
|`short`      |`ShortType` |`Short` | `ShortType`|
|`ushort`     ||||
|`char`   | |`Char`||
|`string` |`StringType` |`String` |`StringType` |
|`DateTime`   |`DateType`, `TimestampType` |`java.sql.Date`, `java.sql.Timestamp` | `DateType`, `TimestampType`|
|`bool`   |`BooleanType` |`Boolean` | `BooleanType`|
|`Guid`   ||||
|`byte[]` |`BinaryType` |`Array[Byte]` | `BinaryType`|
|`SQL.MAP<K,V>`   |`MapType(keyType, valueType, valueContainsNull)` |`scala.collection.Map` | `MapType(keyType, valueType, valueContainsNull=True)`|
|`SQL.ARRAY<T>`   |`ArrayType(elementType, containsNull)` |`scala.collection.Seq` | `ArrayType(elementType, containsNull=True)`|

Daha fazla bilgi için bkz.

- [org.apache.spark.sql.types](https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.types.package)
- [SQL ve DataFrameS Türlerini Kıvılcımla](https://spark.apache.org/docs/latest/sql-reference.html#data-types)
- [Scala değer türleri](https://www.scala-lang.org/api/current/scala/AnyVal.html)
- [pyspark.sql.types](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#module-pyspark.sql.types)

### <a name="treatment-of-null"></a>NULL tedavisi

Spark'ta, varsayılan başına türler NULL değerlerine izin verirken, U-SQL'de açıkça skaler, nesne olmayan ları nullable olarak işaretlersiniz. Spark bir sütunu nullable olarak tanımlamanıza izin verir, ancak kısıtlamayı zorlamaz ve [yanlış sonuca yol açabilir.](https://medium.com/@weshoffman/apache-spark-parquet-and-troublesome-nulls-28712b06f836)

Spark'ta NULL, değerin bilinmediğini gösterir. Kıvılcım NULL değeri kendisi de dahil olmak üzere herhangi bir değerden farklıdır. İki Spark NULL değeri veya bir NULL değeri ile başka bir değer arasındaki karşılaştırmalar, her NULL'un değeri bilinmemekle birlikte bilinmiyor.  

Bu davranış, herhangi bir değerden farklı ama kendisine `null` eşit olan C# semantiklerini izleyen U-SQL'den farklıdır.  

Böylece, `SELECT` U-SQL'de `WHERE column_name = NULL` `column_name`null değerleri olsa bile sıfır satır döndürür kullanan bir SparkSQL `column_name` deyimi, `null`satırları ' ya ayarlandığı yerde döndürür. Benzer şekilde, U-SQL'de `SELECT` `WHERE column_name != NULL` `column_name`null olmayan değerler olsa bile sıfır satır döndürür kullanan bir Kıvılcım deyimi, null olmayan satırları döndürür. Bu nedenle, U-SQL null-check semantiği istiyorsanız, sırasıyla [isnull](https://spark.apache.org/docs/2.3.0/api/sql/index.html#isnull) ve [isnotnull](https://spark.apache.org/docs/2.3.0/api/sql/index.html#isnotnull) (veya DSL eşdeğeri) kullanmalısınız.

## <a name="transform-u-sql-catalog-objects"></a>U-SQL katalog nesnelerini dönüştürme

Önemli bir fark, U-SQL Scripts katalog nesneleri, birçoğu doğrudan Spark eşdeğeri var yararlanabilirsiniz.

Spark, Hive Meta depolama kavramları, özellikle veritabanları ve tablolar için destek sağlar, böylece U-SQL veritabanlarını ve şemaları Spark tablolarına eşleyebilir ve U-SQL tablolarını Spark tablolarına göre eşleyebilirsiniz (bkz. [U-SQL tablolarında depolanan verileri taşıma),](understand-spark-data-formats.md#move-data-stored-in-u-sql-tables)ancak görünümler, tablo değeri olan işlevler (TVF' ler), depolanmış yordamlar, U-SQL derlemeleri, harici veri kaynakları vb. için destek yoktur.

Görünümler, TVF'ler, depolanan yordamlar ve derlemeler gibi U-SQL kod nesneleri Spark'taki kod işlevleri ve kitaplıklar aracılığıyla modellenebilir ve ana bilgisayar dilinin işlevi ve yordamsal soyutlama mekanizmaları kullanılarak başvurulabilir (örneğin, alma yoluyla Python modülleri veya Scala işlevlerine başvuru).

U-SQL kataloğu, projeler ve takımlar arasında veri ve kod nesnelerini paylaşmak için kullanılmışsa, paylaşım için eşdeğer mekanizmalar kullanılmalıdır (örneğin, kod nesnelerini paylaşmak için Maven).

## <a name="transform-u-sql-rowset-expressions-and-sql-based-scalar-expressions"></a>U-SQL rowset ifadelerini ve SQL tabanlı skaler ifadeleri dönüştürme

U-SQL'in temel dili sıra kümelerini dönüştürüyor ve SQL'e dayalıdır. U-SQL'de sunulan en yaygın satır kümesi ifadelerinin kapsamlı olmayan bir listesi aşağıda veda edin:

- `SELECT`/`FROM`/`WHERE`/`GROUP BY`+Agregalar+`HAVING`/`ORDER BY`+`FETCH`
- `INNER`/`OUTER`/`CROSS`/`SEMI``JOIN` ifadeler
- `CROSS`/`OUTER``APPLY` ifadeler
- `PIVOT`/`UNPIVOT`Ifa -de
- `VALUES`rowset constructor

- İfadeleri ayarlama`UNION`/`OUTER UNION`/`INTERSECT`/`EXCEPT`

Buna ek olarak, U-SQL gibi SQL tabanlı skaler ifadeler çeşitli sağlar

- `OVER`pencereli ifadeler
- çeşitli dahili toplayıcılar ve sıralama fonksiyonları`SUM` `FIRST` (, vb.)
- Bazı `CASE`en tanıdık SQL skaler ifadeler: `LIKE`,`NOT` `IN`, `AND` `OR` ( ) , , vb.

Spark, bu ifadelerin çoğu için hem DSL hem de SparkSQL formunda eşdeğer ifadeler sunar. Spark'ta yerel olarak desteklenmeyen bazı ifadelerin, yerel Kıvılcım ifadeleri ve anlamsal olarak eşdeğer desenlerin bir birleşimi kullanılarak yeniden yazılması gerekir. Örneğin, `OUTER UNION` projeksiyonlar ve sendikalar eşdeğer bir arada çevrilmesi gerekir.

NULL değerlerinin farklı işlenmesi nedeniyle, karşılaştırılan sütunların her ikisi de NULL değeri içeriyorsa, Açık null denetimler eklenmedikçe Spark'taki birbirleştirme bu sütunlarla eşleşmezse, U-SQL join her zaman bir satırla eşleşir.

## <a name="transform-other-u-sql-concepts"></a>Diğer U-SQL kavramlarını dönüştürme

U-SQL ayrıca SQL Server veritabanları, parametreler, skaler ve lambda ifade değişkenleri, sistem değişkenleri, `OPTION` ipuçlarına karşı federe sorgular gibi çeşitli diğer özellikler ve kavramlar da sunar.

### <a name="federated-queries-against-sql-server-databasesexternal-tables"></a>SQL Server veritabanları/harici tablolara karşı Federe Sorgular

U-SQL, Azure SQL Veritabanı'na karşı doğrudan sorguların yanı sıra veri kaynağı ve harici tabloları da sağlar. Spark aynı nesne soyutlamalarını sunmazken, SQL veritabanlarını sorgulamak için kullanılabilecek [Azure SQL Veritabanı için Spark bağlayıcısı](../sql-database/sql-database-spark-connector.md) sağlar.

### <a name="u-sql-parameters-and-variables"></a>U-SQL parametreleri ve değişkenleri

Parametreler ve kullanıcı değişkenleri, Spark ve barındırma dillerinde eşdeğer kavramlara sahiptir.

Örneğin `var` Scala'da, anahtar kelimeyle bir değişken tanımlayabilirsiniz:

```
var x = 2 * 3;
println(x)
```

U-SQL'in sistem değişkenleri (başlangıç `@@`ile başlayan değişkenler) iki kategoriye ayrılabilir:

- Komut dosyaları davranışını etkilemek için belirli değerlere ayarlanabilen ayarlanabilir sistem değişkenleri
- Sistem ve iş düzeyi bilgilerini sorgulayan bilgi sistemi değişkenleri

Ayarlanabilir sistem değişkenlerinin çoğunun Spark'ta doğrudan eşdeğeri yoktur. Bazı bilgi sistemi değişkenleri iş yürütme sırasında bağımsız değişkenler olarak bilgi geçirerek modellenebilir, diğerleri Spark barındırma dilinde eşdeğer bir işleve sahip olabilir.

### <a name="u-sql-hints"></a>U-SQL ipuçları

U-SQL sorgu optimize edici ve yürütme motoru için ipuçları sağlamak için çeşitli sözdizimi yolları sunar:  

- U-SQL sistem değişkenini ayarlama
- bir `OPTION` veri veya plan ipucu sağlamak için satır kümesi ifadesi ile ilişkili bir yan tümce
- birleştirme ifadesinin sözdiziminde birbirleştirme ipucu `BROADCASTLEFT`(örneğin, )

Spark'ın maliyet tabanlı sorgu optimize edicisi, ipuçları sağlamak ve sorgu performansını ayarlamak için kendi yeteneklerine sahiptir. Lütfen ilgili belgelere bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [U-SQL geliştiricileri için Spark veri biçimlerini anlama](understand-spark-data-formats.md)
- [Apache Spark için .NET](https://docs.microsoft.com/dotnet/spark/what-is-apache-spark-dotnet)
- [Azure Veri Gölü Depolama Gen1'den büyük veri analizi çözümlerinizi Azure Veri Gölü Depolama Gen2'ye yükseltin](../storage/blobs/data-lake-storage-upgrade.md)
- [Azure Veri Fabrikası'ndaki Spark etkinliğini kullanarak verileri dönüştürme](../data-factory/transform-data-using-spark.md)
- [Azure Veri Fabrikası'ndaki Hadoop Hive etkinliğini kullanarak verileri dönüştürme](../data-factory/transform-data-using-hadoop-hive.md)
- [Azure HDInsight’ta Apache Spark nedir?](../hdinsight/spark/apache-spark-overview.md)
