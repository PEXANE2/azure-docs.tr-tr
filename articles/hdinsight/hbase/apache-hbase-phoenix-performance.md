---
title: Azure HDInsight'ta Phoenix performansı
description: Azure HDInsight kümeleri için Apache Phoenix performansını optimize etmek için en iyi uygulamalar
author: ashishthaps
ms.author: ashishth
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/27/2019
ms.openlocfilehash: 7f8f20be81e815414c283f7ec48aa6503e3b60ed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75552653"
---
# <a name="apache-phoenix-performance-best-practices"></a>Apache Phoenix performansı için en iyi yöntemler

[Apache Phoenix](https://phoenix.apache.org/) performansının en önemli yönü altta yatan [Apache HBase](https://hbase.apache.org/)optimize etmektir. Phoenix, HBase'in tepesinde, SQL sorgularını taramalar gibi HBase işlemlerine dönüştüren ilişkisel bir veri modeli oluşturur. Tablo şemanızın tasarımı, birincil anahtarınızdaki alanların seçimi ve sıralanması ve dizin kullanımınız Phoenix performansını etkiler.

## <a name="table-schema-design"></a>Tablo şeması tasarımı

Phoenix'te bir tablo oluşturduğunuzda, bu tablo bir HBase tablosunda depolanır. HBase tablosu, birlikte erişilen sütun grupları (sütun aileleri) içerir. Phoenix tablosundaki bir satır, HBase tablosundaki bir satırdır ve her satır bir veya daha fazla sütunla ilişkili sürümhücrelerden oluşur. Mantıksal olarak, tek bir HBase satırı, her biri aynı satır anahtar değerine sahip anahtar değer çiftlerinden oluşan bir koleksiyondur. Diğer bir zamanda, her anahtar değer çiftinin bir satır anahtar özniteliği vardır ve bu satır tuşu özniteliğinin değeri belirli bir satır için aynıdır.

Phoenix tablosunun şema tasarımı birincil anahtar tasarımını, sütun aile tasarımını, tek tek sütun tasarımını ve verilerin nasıl bölümleniyi içerir.

### <a name="primary-key-design"></a>Birincil anahtar tasarımı

Phoenix'te bir tabloda tanımlanan birincil anahtar, verilerin temel HBase tablosunun satır anahtarı içinde nasıl depolandığını belirler. HBase'de, belirli bir satıra erişmenin tek yolu satır tuşudur. Buna ek olarak, HBase tablosunda depolanan veriler rowkey tarafından sıralanır. Phoenix, satırdaki sütunların her birinin değerlerini birincil anahtarda tanımlanan sırayla birleştirerek satır anahtar değerini oluşturur.

Örneğin, ilgili kişilerin tablosunda ad, soyad, telefon numarası ve adres vardır. Artan sıra numarasına dayalı birincil anahtar tanımlayabilirsiniz:

|rowkey|       adres|   telefon| firstName| lastName|
|------|--------------------|--------------|-------------|--------------|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    John|Dole|
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji|

Ancak, lastName ile sık sık sorgu yaparsanız bu birincil anahtar iyi performans göstermeyebilir, çünkü her sorgu, her son Adın değerini okumak için tam bir tablo tayini gerektirir. Bunun yerine, soyadı, ilkad ve sosyal güvenlik numarası sütunlarında birincil bir anahtar tanımlayabilirsiniz. Bu son sütun, aynı adreste aynı adreste yaşayan iki sakini, baba ve oğul gibi aynı isimle ayrıştırmaktır.

|rowkey|       adres|   telefon| firstName| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  1000|1111 San Gabriel Dr.|1-425-000-0002|    John|Dole| 111 |
|  8396|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Bu yeni birincil anahtar ile Phoenix tarafından oluşturulan satır tuşları olacaktır:

|rowkey|       adres|   telefon| firstName| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole-John-111|1111 San Gabriel Dr.|1-425-000-0002|    John|Dole| 111 |
|  Raji-Calvin-222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Yukarıdaki ilk satırda, satır anahtarının verileri gösterildiği gibi gösterilir:

|rowkey|       anahtar|   value|
|------|--------------------|---|
|  Dole-John-111|adres |1111 San Gabriel Dr.|  
|  Dole-John-111|telefon |1-425-000-0002|  
|  Dole-John-111|firstName |John|  
|  Dole-John-111|lastName |Dole|  
|  Dole-John-111|socialSecurityNum |111|

Bu satır tuşu artık verilerin yinelenen bir kopyasını depolar. Bu değer, temel HBase tablosundaki her hücreye dahil olduğundan, birincil anahtarınıza eklediğiniz sütunların boyutunu ve sayısını göz önünde bulundurun.

Ayrıca, birincil anahtar monoton olarak artan değerlere sahipse, yazma etkin noktalarını oluşturmamak için *tuz kovalı* tablo oluşturmanız gerekir - [Bkz. Bölüm verileri.](#partition-data)

### <a name="column-family-design"></a>Sütun aile tasarımı

Bazı sütunlara diğerlerinden daha sık erişiliyorsa, sık erişilen sütunları nadiren erişilen sütunlardan ayırmak için birden çok sütun ailesi oluşturmanız gerekir.

Ayrıca, belirli sütunlara birlikte erişme eğilimindeyse, bu sütunları aynı sütun ailesine koyun.

### <a name="column-design"></a>Sütun tasarımı

* Büyük sütunların G/Ç maliyetleri nedeniyle VARCHAR sütunlarını yaklaşık 1 MB'ın altında tutun. Sorguları işlerken, HBase hücreleri istemciye göndermeden önce tam olarak somutlaştırır ve istemci bunları uygulama koduna teslim etmeden önce tam olarak alır.
* Protobuf, Avro, msgpack veya BSON gibi kompakt bir biçim kullanarak sütun değerlerini depolayın. JSON tavsiye edilmez, çünkü daha büyük.
* Gecikme ve G/Ç maliyetlerini azaltmak için depolamadan önce verileri sıkıştırmayı düşünün.

### <a name="partition-data"></a>Verileri bölümleme

Phoenix, verilerinizin dağıtıldığı bölgelerin sayısını denetlemenize olanak tanır ve bu da okuma/yazma performansını önemli ölçüde artırabilir. Phoenix tablosu oluştururken, verilerinizi tuzlayabilir veya önceden bölebilirsiniz.

Oluşturma sırasında bir tabloyu tuzlamak için tuz kovalarının sayısını belirtin:

    CREATE TABLE CONTACTS (...) SALT_BUCKETS = 16

Bu tuzlama, değerleri otomatik olarak seçerek tabloyu birincil anahtarların değerleri boyunca böler. 

Tablo bölmelerinin nerede oluştuğunu denetlemek için, bölmenin gerçekleştiği aralık değerlerini sağlayarak tabloyu önceden bölebilirsiniz. Örneğin, üç bölge boyunca bölünmüş bir tablo oluşturmak için:

    CREATE TABLE CONTACTS (...) SPLIT ON ('CS','EU','NA')

## <a name="index-design"></a>Dizin tasarımı

Phoenix dizini, dizinlenen tablodaki verilerin bir kısmını veya tamamını depolayan bir HBase tablosudur. Dizin, belirli sorgu türleri için performansı artırır.

Birden çok dizin tanımlandığınızda ve ardından bir tabloyu sorguladığında, Phoenix sorgu için en iyi dizini otomatik olarak seçer. Birincil dizin seçtiğiniz birincil anahtarları temel alınerek otomatik olarak oluşturulur.

Beklenen sorgular için, sütunlarını belirterek ikincil dizinler de oluşturabilirsiniz.

Dizinlerinizi tasarlarken:

* Yalnızca ihtiyacınız olan dizinleri oluşturun.
* Sık güncelleştirilen tablolardaki dizin sayısını sınırlayın. Tablodaki güncelleştirmeler hem ana tabloya hem de dizin tablolarına yazıolarak çevirir.

## <a name="create-secondary-indexes"></a>İkincil dizinler oluşturma

İkincil dizinler, tam tablo tonu olacak şeyi depolama alanı ve yazma hızı pahasına bir nokta aramasına dönüştürerek okuma performansını artırabilir. İkincil dizinler tablo oluşturulduktan sonra eklenebilir veya kaldırılabilir ve varolan sorgularda değişiklik gerektirmez – sorgular daha hızlı çalışır. İhtiyaçlarınıza bağlı olarak, kapalı dizinler, işlevsel dizinler veya her ikisi oluşturmayı düşünün.

### <a name="use-covered-indexes"></a>Kapsanan dizinleri kullanma

Kapsanan dizinler, dizine eklenen değerlere ek olarak satırdan gelen verileri içeren dizinlerdir. İstenilen dizin girişini bulduktan sonra birincil tabloya erişmeye gerek yoktur.

Örneğin, örnek iletişim tablosunda sadece socialSecurityNum sütununda ikincil bir dizin oluşturabilirsiniz. Bu ikincil dizin, socialSecurityNum değerlerine göre filtrelenen sorguları hızlandıracaktır, ancak diğer alan değerlerini almak için ana tabloya karşı başka bir okuma gerekir.

|rowkey|       adres|   telefon| firstName| lastName| socialSecurityNum |
|------|--------------------|--------------|-------------|--------------| ---|
|  Dole-John-111|1111 San Gabriel Dr.|1-425-000-0002|    John|Dole| 111 |
|  Raji-Calvin-222|5415 San Gabriel Dr.|1-230-555-0191|  Calvin|Raji| 222 |

Ancak, genellikle socialSecurityNum verilen ilk Ad ve soyad'ı aramak istiyorsanız, dizin tablosundaki gerçek veri olarak ilk adı ve soyadını içeren kapalı bir dizin oluşturabilirsiniz:

    CREATE INDEX ssn_idx ON CONTACTS (socialSecurityNum) INCLUDE(firstName, lastName);

Bu kapsanan dizin, ikincil dizini içeren tablodan okuyarak aşağıdaki sorgunun tüm verileri elde etmesini sağlar:

    SELECT socialSecurityNum, firstName, lastName FROM CONTACTS WHERE socialSecurityNum > 100;

### <a name="use-functional-indexes"></a>İşlevsel dizinleri kullanma

İşlevsel dizinler, sorgularda kullanılmasını beklediğiniz rasgele bir ifade üzerinde bir dizin oluşturmanıza olanak sağlar. İşlevsel bir dizin yerleştirdikten ve bir sorgu bu ifadeyi kullandığında, dizin veri tablosu yerine sonuçları almak için kullanılabilir.

Örneğin, bir kişinin birleştirilmiş adı ve soyadı üzerinde büyük/küçük harf duyarsız aramalar yapmanıza olanak tanıyan bir dizin oluşturabilirsiniz:

     CREATE INDEX FULLNAME_UPPER_IDX ON "Contacts" (UPPER("firstName"||' '||"lastName"));

## <a name="query-design"></a>Sorgu tasarımı

Sorgu tasarımında dikkat edilmesi gereken başlıca hususlar şunlardır:

* Sorgu planını anlayın ve beklenen davranışını doğrulayın.
* Verimli bir şekilde katılın.

### <a name="understand-the-query-plan"></a>Sorgu planını anlama

[SQLLine'da,](http://sqlline.sourceforge.net/)Phoenix'in gerçekleştireceği işlem planını görüntülemek için SQL sorgunuzun ardından EXPLAIN'ı kullanın. Planı kontrol edin:

* Gerektiğinde birincil anahtarınızı kullanır.
* Veri tablosu yerine uygun ikincil dizinler kullanır.
* TABLE SCAN yerine mümkün olduğunda RANGE SCAN veya SKIP SCAN kullanır.

#### <a name="plan-examples"></a>Plan örnekleri

Örnek olarak, uçuş gecikme bilgilerini depolayan UÇUŞLAR adlı bir tablonuz olduğunu varsamayın.

Airlineid'in `19805`birincil anahtarda veya herhangi bir indekste olmayan bir alan olduğu bir havayolu ile tüm uçuşları seçmek için:

    select * from "FLIGHTS" where airlineid = '19805';

Açıklama komutunu aşağıdaki gibi çalıştırın:

    explain select * from "FLIGHTS" where airlineid = '19805';

Sorgu planı aşağıdaki gibi görünür:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN FULL SCAN OVER FLIGHTS
        SERVER FILTER BY AIRLINEID = '19805'

Bu planda, UÇUŞLAR ÜZERİnDE TAM TCAN ifadesini not edin. Bu ifade, yürütmenin daha verimli RANGE SCAN veya SKIP SCAN seçeneğini kullanmak yerine tablodaki tüm satırlar üzerinde bir TABLO TKAN'ı yaptığını gösterir.

Şimdi, uçuşnumun 1'den büyük olduğu taşıyıcı `AA` için 2 Ocak 2014'teki uçuşları sorgulamak istediğinizi varsasınız. Sütunların yıl, ay, gün ayı, taşıyıcı ve flightnum'un örnek tabloda bulunduğunu ve kompozit birincil anahtarın bir parçası olduğunu varsayalım. Sorgu aşağıdaki gibi görünür:

    select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Bu sorgunun planını aşağıdakilerle inceleyelim:

    explain select * from "FLIGHTS" where year = 2014 and month = 1 and dayofmonth = 2 and carrier = 'AA' and flightnum > 1;

Ortaya çıkan plan:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER FLIGHTS [2014,1,2,'AA',2] - [2014,1,2,'AA',*]

Kare ayraçlar daki değerler birincil anahtarlar için değerler aralığıdır. Bu durumda, aralık değerleri 2014 yılı, ay 1 ve gün 2 ile sabitlenir, ancak flightnum`*`için 2 ve yukarı () ile başlayan değerlere izin verir. Bu sorgu planı, birincil anahtarın beklendiği gibi kullanıldığını doğrular.

Ardından, yalnızca taşıyıcı alanında olan `carrier2_idx` UÇUŞLAR tablosunda bir dizin oluşturun. Bu dizin, verileri dizinde de depolanan kapalı sütunlar olarak uçuş tarihi, tailnum, menşe ve flightnum'u da içerir.

    CREATE INDEX carrier2_idx ON FLIGHTS (carrier) INCLUDE(FLIGHTDATE,TAILNUM,ORIGIN,FLIGHTNUM);

Aşağıdaki sorguda olduğu gibi, uçuş tarihi ve tailnum ile birlikte taşıyıcı almak istediğinizi varsan:

    explain select carrier,flightdate,tailnum from "FLIGHTS" where carrier = 'AA';

Bu dizinin kullanıldığını görmeniz gerekir:

    CLIENT 1-CHUNK PARALLEL 1-WAY ROUND ROBIN RANGE SCAN OVER CARRIER2_IDX ['AA']

Plan sonuçlarını açıklayabilen öğelerin tam listesi [için, Apache Phoenix Tuning Guide'daki](https://phoenix.apache.org/tuning_guide.html)Planları Açıkla bölümüne bakın.

### <a name="join-efficiently"></a>Verimli bir şekilde katılın

Genellikle, bir taraf küçük olmadıkça, özellikle sık sık sorguları birleştirmekten kaçınmak istersiniz.

Gerekirse, `/*+ USE_SORT_MERGE_JOIN */` ipucu ile büyük birleştirmeler yapabilirsiniz, ancak büyük bir birleştirme satır çok sayıda üzerinde pahalı bir işlemdir. Tüm sağ taraftaki tabloların genel boyutu kullanılabilir belleği aşarsa, ipucunu `/*+ NO_STAR_JOIN */` kullanın.

## <a name="scenarios"></a>Senaryolar

Aşağıdaki yönergeler bazı ortak desenleri açıklar.

### <a name="read-heavy-workloads"></a>Okuma ağırlıklı iş yükleri

Okuma ağırlıklı kullanım örnekleri için dizin kullandığınızdan emin olun. Ayrıca, okuma zamanı ek yükü kaydetmek için, kapalı dizinler oluşturmayı düşünün.

### <a name="write-heavy-workloads"></a>Yazma-ağır iş yükleri

Birincil anahtarın monoton olarak arttığı yazma ağırlıklı iş yükleri için, gereken ek taramalar nedeniyle genel okuma iş tükenti pahasına etkin noktaların yazılmasından kaçınmaya yardımcı olmak için tuz kovaları oluşturun. Ayrıca, çok sayıda kayıt yazmak için UPSERT'i kullanırken, autoCommit'i kapatın ve kayıtları toplu olarak ekleyin.

### <a name="bulk-deletes"></a>Toplu silme

Büyük bir veri kümesini silerken, delete sorgusunu vermeden önce otomatik Commit'i açın, böylece istemcinin silinen tüm satırların satır anahtarlarını hatırlamasına gerek yoktur. AutoCommit istemcisi DELETE etkilenen satırları arabelleğe engeller, böylece Phoenix istemciye iade pahasına olmadan doğrudan bölge sunucularında silebilir.

### <a name="immutable-and-append-only"></a>Değişmez ve Yalnızca Ek

Senaryonuz veri bütünlüğü ne olursa, yazma hızını destekliyorsa, tablolarınızı oluştururken yazma günlüğünü devre dışı bırakmayı düşünün:

    CREATE TABLE CONTACTS (...) DISABLE_WAL=true;

Bu ve diğer seçenekler hakkında ayrıntılı bilgi için [Apache Phoenix Grammar'a](https://phoenix.apache.org/language/index.html#options)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Apache Phoenix Tuning Kılavuzu](https://phoenix.apache.org/tuning_guide.html)
* [İkincil Dizinler](https://phoenix.apache.org/secondary_indexing.html)
