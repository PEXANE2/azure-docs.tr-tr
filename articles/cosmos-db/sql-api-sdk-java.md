---
title: 'Azure Cosmos DB: SQL Java API, SDK ve kaynakları'
description: Tüm SQL Java API ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB SQL Java SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 03/13/2019
ms.author: sngun
ms.openlocfilehash: 3a7eaabedd6736c107625204f982a530f2bab73d
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76718182"
---
# <a name="azure-cosmos-db-java-sdk-for-sql-api-release-notes-and-resources"></a>SQL API'si için Azure Cosmos DB Java SDK: yayın notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

SQL API Java SDK'sı, zaman uyumlu işlemler destekler. Zaman uyumsuz destek için, [SQL API zaman uyumsuz Java SDK 'sını](sql-api-sdk-async-java.md)kullanın. 

| |  |
|---|---|
|**SDK Indirmesi**|[Maven](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)|
|**API belgeleri**|[Java API başvuru belgeleri](/java/api/com.microsoft.azure.documentdb)|
|**SDK 'ya katkıda bulunma**|[GitHub](https://github.com/Azure/azure-documentdb-java/)|
|**Başlarken**|[Java SDK 'sını kullanmaya başlama](sql-api-java-get-started.md)|
|**Web uygulaması öğreticisi**|[Azure Cosmos DB ile Web uygulaması geliştirme](sql-api-java-application.md)|
|**Desteklenen en düşük çalışma zamanı**|[Java Development Kit (JDK) 7 +](https://aka.ms/azure-jdks)|

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name246246"></a><a name="2.4.6"/>2.4.6
* DatabaseAccount üzerinde doğru istemci tarafı çoğaltma ilkesi etiketi güncelleştirildi ve önbellekten databaseAccount yapılandırma okumaları yaptı.

### <a name="a-name245245"></a><a name="2.4.5"/>2.4.5
* Kullanıcı Prangeıd sağlıyorsa geçersiz bölüm anahtar aralığı hatası durumunda yeniden denemeyi önleme.

### <a name="a-name244244"></a><a name="2.4.4"/>2.4.4
* İyileştirilmiş bölüm anahtar aralığı önbelleği yenilemeleri.
* SDK 'nın bölüm bölme ipucunu sunucudan çözmeyeceği ve hatalı istemci tarafı yönlendirme önbellekleri yenilemesine neden olan senaryoyu düzeltir.

### <a name="a-name242242"></a><a name="2.4.2"/>2.4.2 sections
* İyileştirilmiş koleksiyon önbelleği yenilemeleri.

### <a name="a-name241241"></a><a name="2.4.1"/>2.4.1
* İstek tanılama dizesinden iç özel durum iletisini almak için destek eklendi.

### <a name="a-name240240"></a><a name="2.4.0"/>2.4.0
* PartitionKeyDefinition üzerinde sürüm API 'si kullanıma sunuldu.

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Doğrudan mod için ayrı zaman aşımı desteği eklendi.

### <a name="a-name223223"></a><a name="2.2.3"/>2.2.3
* Hizmetten null hata iletisi tüketiyor ve belge istemci özel durumu üretiliyor.

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Yuva bağlantısı geliştirmesi, SoKeepAlive varsayılan değeri.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* İstek tanılama dizesi desteği eklendi.

### <a name="a-name213213"></a><a name="2.1.3"/>2.1.3
* Karma v2 için PartitionKey içindeki hata düzeltildi.

### <a name="a-name212212"></a><a name="2.1.2"/>2.1.2 'yi
* Bileşik dizinler için destek eklendi.
* Yenilemeyi zorlamak için genel uç nokta yöneticisinde hata düzeltildi.
* Doğrudan modda ön koşullara sahip olan uptts için hata düzeltildi.

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1
* Ağ Geçidi adres önbelleğinde hata düzeltildi.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Çok bölgeli yazma desteği için doğrudan modu eklendi.
* Bir proxy sunucudan ServiceUnavailable özel durumlar olarak durum IOExceptions işlemek için destek eklendi.
* Uç nokta bulma yeniden deneme ilkesi bir hata düzeltildi.
* Null işaretçi BaseDatabaseAccountConfigurationProvider içinde özel durumlar değil emin olmak için bir hata düzeltildi.
* Sorgu yineleyicisinin null değeri döndürmediğinden emin olmak için hata düzeltildi.
* Büyük PartitionKey izin emin olmak için bir hata düzeltildi

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Çok bölgeli yazma desteği için ağ geçidi modu eklendi.

### <a name="a-name11641164"></a><a name="1.16.4"/>1.16.4
* Bir sorgu aralıklarını okuma bölüm anahtarı olarak düzeltildi.

### <a name="a-name11631163"></a><a name="1.16.3"/>1.16.3
* Devamlılık belirteci üstbilgi boyutu DirectHttps modunda ayarlama bir hata düzeltildi.

### <a name="a-name11621162"></a><a name="1.16.2"/>1.16.2
* Eklenen akış yük devretme desteği.
* Özel meta verileri desteği eklendi.
* Geliştirilmiş oturum işleme mantığı.
* Bölüm anahtar aralığı önbellekte bir hata düzeltildi.
* Doğrudan modda NPE düzeltildi.

### <a name="a-name11611161"></a><a name="1.16.1"/>1.16.1
* Benzersiz dizin için destek eklendi.
* Akış seçenekleri devamlılık belirteci boyutunu sınırlamak için destek eklendi.
* Json serileştirme (zaman damgası) bir hata düzeltildi.
* Json serileştirme (enum) bir hata düzeltildi.
* Bağımlılık com.fasterxml.jackson.core:jackson databind 2.9.5 için yükseltildi.

### <a name="a-name11601160"></a><a name="1.16.0"/>1.16.0
* Geliştirilmiş bağlantı havuzu için doğrudan modu.
* Önceden getirme geliştirme orderby olmayan çapraz bölüm sorgusunun için İyileştirildi.
* Geliştirilmiş UUID oluşturma.
* Geliştirilmiş oturum tutarlılık mantığı.
* Multipolygon desteği eklendi.
* Bölüm anahtar aralığı istatistikleri koleksiyonu için destek eklendi.
* Çok bölgeli desteği düzeltildi.

### <a name="a-name11501150"></a><a name="1.15.0"/>1.15.0
* Json seri hale getirme performansı İyileştirildi.
* Bu SDK sürümü, Azure Cosmos DB öykünücüsü 'nin https://aka.ms/cosmosdb-emulator'den indirileceği en son sürümünü gerektirir.

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
* İç değişiklik Microsoft arkadaş kitaplıkları.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Tek bölüm anahtar aralığı okuma bir sorun düzeltildi.
* ResourceId içinde bir sorun düzeltildi, ayrıştırma, veritabanı kısa adları ile etkiler.
* Anahtar kodlaması için bölüm tarafından bir sorunu neden düzeltildi.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* İşleme sırasında bölüm bölmelerini istemek için Kritik hata düzeltmeleri.
* Güçlü ve BoundedStaleness tutarlılık düzeyleri ile bir sorun düzeltildi.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Yeni bir tutarlılık düzeyi için destek eklendi ConsistentPrefix çağrılır.
* Koleksiyon oturumu modunda okuma bir hata düzeltildi.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Etkin desteği ile bölümlenmiş koleksiyonu olarak düşük 2.500 RU/sn ve 100 RU/sn artışlarla ölçeklendirme.
* Bazı sorgularda NullRef özel durum neden yerel derlemedeki bir hata düzeltildi.

### <a name="a-name196196"></a><a name="1.9.6"/>1.9.6
* Ağ geçidi modunda sorguları için özel durumlar neden olabilecek sorgu altyapısı yapılandırmasında bir hata düzeltildi.
* Koleksiyon oluşturulduktan hemen sonra istekleri için bir "sahibi kaynak bulunamadı" özel neden olabilecek oturumu kapsayıcıdaki bazı hatalar düzeltildi.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) için destek eklendi. Bkz. [toplama desteği](sql-query-aggregates.md).
* Değişiklik akışı desteği eklendi.
* RequestOptions.setPopulateQuotaInfo aracılığıyla kota bilgilerin toplanması için destek eklendi.
* Saklı yordam betik günlüğü RequestOptions.setScriptLoggingEnabled desteği eklendi.
* DirectHttps modundaki sorgunun, kısıtlama hatalarıyla karşılaşıldığında yanıt vermemesine neden olan bir hata düzeltildi.
* Oturum tutarlılığı modunda bir hata düzeltildi.
* İstek hızı yüksek olduğunda, NullReferenceException HttpContext içinde neden olabilir bir hata düzeltildi.
* DirectHttps modunun performansı İyileştirildi.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* ConnectionPolicy.setProxy() API'si ile eklenen basit istemci proxy örneği tabanlı desteği.
* Düzgün kapatmayı DocumentClient örneğine eklenen DocumentClient.close() API.
* Ağ geçidi yerine yerel derleme sorgu planı türeterek doğrudan bağlantı moduna sorgu performansı geliştirildi.
* Ayarlama FAIL_ON_UNKNOWN_PROPERTIES = false, böylece kullanıcılar kendi POJO'ya JsonIgnoreProperties tanımlamanız gerekmez.
* SLF4J kullanmak için UIMap'e yeniden işlenmiş günlüğe kaydetmeyi'ı seçin.
* Tutarlılık okuyucusunda bazı hatalar düzeltildi.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Doğrudan bağlantı modunda bağlantı sızıntılarını önlemek için bağlantı yönetiminde bir hata düzeltildi.
* ÜSTTEKI sorguda bir hata düzeltildi ve bu, NullReference özel durumu oluşturmayabilir.
* İç önbellekler için ağ çağrısı sayısını azaltarak performansı İyileştirildi.
* ActivityID ve daha iyi sorun giderme DocumentClientException istek URI'SİNDE eklenen durum kodu.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Bağlantı yönetiminde kararlılık için bir sorun düzeltildi.

### <a name="a-name191191"></a><a name="1.9.1"/>1.9.1
* BoundedStaleness tutarlılık düzeyi için destek eklendi.
* Bölümlenmiş koleksiyonlar için CRUD işlemleri için doğrudan bağlantı için destek eklendi.
* Bir veritabanını SQL ile sorgulama içinde bir hata düzeltildi.
* Oturum belirteci yanlış yere ayarlanabilir oturum önbelleğinde bir hata düzeltildi.

### <a name="a-name190190"></a><a name="1.9.0"/>1.9.0
* Çapraz bölüm paralel sorgular için eklenen destek.
* ÜST/ORDER BY sorguları bölümlenmiş koleksiyonlar için destek eklendi.
* Güçlü tutarlılık için destek eklendi.
* Doğrudan bağlantı kullanırken ad tabanlı istekleri için destek eklendi.
* ActivityID tüm istek yeniden denemeler arasında tutarlı kalmasını sağlamak sabit.
* Aynı ada sahip bir koleksiyon tekrar oluşturulurken oturum önbelleğiyle ilgili bir hata düzeltildi.
* Eklenen Çokgen ve dizin oluşturma ilkesi şirketin coğrafı uzamsal sorgular için koleksiyon belirtirken LineString veri türleri.
* Java 1.8 için Java Doc birlikte sorunlar düzeltildi.

### <a name="a-name181181"></a><a name="1.8.1"/>1.8.1
* Tek bölüm koleksiyonlarını önbellek ve ek getirme önemli istekleri bölümleme yapmamaya PartitionKeyDefinitionMap içinde bir hata düzeltildi.
* Bir hatalı bölüm anahtarı değeri sağlandığında yeniden olmayan bir hata düzeltildi.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Çoklu bölge veritabanı hesapları için destek eklendi.
* Otomatik yeniden deneme seçenekleri en fazla yeniden deneme girişimleri ve en fazla yeniden deneme bekleme süresini özelleştirmek için daraltılmış istekler için destek eklendi.  RetryOptions ve ConnectionPolicy.getRetryOptions() bakın.
* Kullanım dışı IPartitionResolver özel bölümleme kod temel. Lütfen daha yüksek depolama ve aktarım hızı için bölümlenmiş koleksiyonları kullanın.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* Oran sınırlandırma için eklenen yeniden deneme ilkesi desteği.  

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Belgeler için eklenen zaman canlı (TTL) desteği.

### <a name="a-name160160"></a><a name="1.6.0"/>1.6.0
* [Bölümlenmiş koleksiyonlar](partition-data.md) ve [Kullanıcı tanımlı performans düzeyleri](performance-levels.md)uygulandı.

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* Diğer SDK'ları ile tutarlı olmasını endian karma değerleri oluşturmak için HashPartitionResolver içinde bir hata düzeltildi.

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Karma & aralığı Ekle Çözümleyicileri parçalara ayırma uygulamaları ile birden çok bölümde yardımcı olmak üzere bölümleme.

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Upsert uygulayın. Upsert özelliğini desteklemek için eklenen yeni upsertXXX yöntemleri.
* Kimlik tabanlı yönlendirme uygulayın. Genel API değişiklik, tüm değişiklikleri iç.

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Sürüm numarası diğer SDK'lar ile uyumlu duruma getirmek için sürüm atlandı

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Jeo-uzamsal dizin destekler
* Tüm kaynaklar için ID özelliğini doğrular. Kaynak kimlikleri,?,/, # \, karakter içeremez veya boşlukla bitemez.
* Yeni üst bilgi "dizin dönüştürme ilerleme durumu" için ResourceResponse ekler.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* V2 dizin oluşturma ilkesini uygular

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK'SI

## <a name="release-and-retirement-dates"></a>Yayın ve sona erme tarihleri
Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için SDK 'nın devre dışı bırakılmasının ardından en az **12 ay** önce bildirim sağlayacaktır.

Geçerli SDK'sı yalnızca eklenen yeni özellikler ve işlevsellik ve en iyi duruma getirme, olduğundan bu nedenle önerilir, her zaman en son SDK sürümüne erken mümkün olduğunca yükseltmeniz.

Cosmos DB devre dışı bırakılan bir SDK'sını kullanarak yapılan tüm istekleri hizmet tarafından reddedilir.

> [!WARNING]
> Java için SQL SDK 'sının tüm **1. x** sürümleri **30 Mayıs 2020**tarihinde kullanımdan kaldırılacaktır.
> 
>

> [!WARNING]
> Sürüm **1.0.0** öncesinde Java IÇIN SQL SDK 'nın tüm sürümleri **, 29 Şubat 2016**tarihinde devre dışı bırakıldı.
> 
> 

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [2.1.3](#2.1.3) |13 Mar, 2018 |--- |
| [2.1.2 'yi](#2.1.2) |09 Mar, 2018 |--- |
| [2.1.1](#2.1.1) |13. ara, 2018 |--- |
| [2.1.0](#2.1.0) |20 Kasım 2018 |--- |
| [2.0.0](#2.0.0) |21 Eylül 2018 |--- |
| [1.16.4](#1.16.4) |10 Eylül 2018 |30 Mayıs 2020 |
| [1.16.3](#1.16.3) |09 Eylül 2018 |30 Mayıs 2020 |
| [1.16.2](#1.16.2) |29 Haziran 2018'e |30 Mayıs 2020 |
| [1.16.1](#1.16.1) |16 Mayıs 2018 |30 Mayıs 2020 |
| [1.16.0](#1.16.0) |15 Mart 2018 |30 Mayıs 2020 |
| [1.15.0](#1.15.0) |14 Kasım 2017 |30 Mayıs 2020 |
| [1.14.0](#1.14.0) |28 Ekim 2017 |30 Mayıs 2020 |
| [1.13.0](#1.13.0) |25 Ağustos 2017 |30 Mayıs 2020 |
| [1.12.0](#1.12.0) |11 Temmuz 2017 |30 Mayıs 2020 |
| [1.11.0](#1.11.0) |10 Mayıs 2017 |30 Mayıs 2020 |
| [1.10.0](#1.10.0) |11 Mart 2017 |30 Mayıs 2020 |
| [1.9.6](#1.9.6) |21 Şubat 2017 |30 Mayıs 2020 |
| [1.9.5](#1.9.5) |31 Ocak 2017 |30 Mayıs 2020 |
| [1.9.4](#1.9.4) |24 Kasım 2016 |30 Mayıs 2020 |
| [1.9.3](#1.9.3) |30 Ekim 2016 |30 Mayıs 2020 |
| [1.9.2](#1.9.2) |28 Ekim 2016 |30 Mayıs 2020 |
| [1.9.1](#1.9.1) |26 Ekim 2016 |30 Mayıs 2020 |
| [1.9.0](#1.9.0) |03 Ekim 2016 |30 Mayıs 2020 |
| [1.8.1](#1.8.1) |30 Haziran 2016 |30 Mayıs 2020 |
| [1.8.0](#1.8.0) |14 Haziran 2016 |30 Mayıs 2020 |
| [1.7.1](#1.7.1) |30 Nisan 2016 |30 Mayıs 2020 |
| [1.7.0](#1.7.0) |27 Nisan 2016 |30 Mayıs 2020 |
| [1.6.0](#1.6.0) |29 Mart 2016 |30 Mayıs 2020 |
| [1.5.1](#1.5.1) |31 Aralık 2015 |30 Mayıs 2020 |
| [1.5.0](#1.5.0) |04 Aralık 2015 |30 Mayıs 2020 |
| [1.4.0](#1.4.0) |05 Ekim 2015 |30 Mayıs 2020 |
| [1.3.0](#1.3.0) |05 Ekim 2015 |30 Mayıs 2020 |
| [1.2.0](#1.2.0) |05 Ağustos 2015 |30 Mayıs 2020 |
| [1.1.0](#1.1.0) |09 Temmuz 2015 |30 Mayıs 2020 |
| 1.0.1 |12 Mayıs 2015 |30 Mayıs 2020 |
| [1.0.0](#1.0.0) |07 Nisan 2015 |30 Mayıs 2020 |
| 0.9.5-prelease |09 Mar 2015 |29 Şubat 2016 |
| 0.9.4-prelease |17 Şubat 2015 |29 Şubat 2016 |
| 0.9.3-prelease |13 Ocak 2015 |29 Şubat 2016 |
| 0.9.2-prelease |19 Aralık 2014 |29 Şubat 2016 |
| 0.9.1-prelease |19 Aralık 2014 |29 Şubat 2016 |
| 0.9.0-prelease |10 Aralık 2014 |29 Şubat 2016 |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

