---
title: 'Azure Cosmos DB: SQL Java API, SDK kaynakları &'
description: Sürüm tarihleri, emeklilik tarihleri ve Azure Cosmos DB SQL Java SDK'nın her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL Java API ve SDK hakkında her şeyi öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 02/21/2020
ms.author: sngun
ms.openlocfilehash: 514982727509788918c159e07f8061962df32336
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77558937"
---
# <a name="azure-cosmos-db-java-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için Azure Cosmos DB Java SDK: Sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET Değişim Akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [Geri kalanı](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu uygulayıcı - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu uygulayıcı - Java](sql-api-sdk-bulk-executor-java.md)

SQL API Java SDK senkron işlemleri destekler. Eşzamanlı destek için [SQL API Async Java SDK'yı](sql-api-sdk-async-java.md)kullanın. 

| |  |
|---|---|
|**SDK İndir**|[Maven](https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22com.microsoft.azure%22%20AND%20a%3A%22azure-documentdb%22)|
|**API belgeleri**|[Java API başvuru belgeleri](/java/api/com.microsoft.azure.documentdb)|
|**SDK'ya Katkıda Bulunun**|[GitHub](https://github.com/Azure/azure-documentdb-java/)|
|**Kullanmaya başlayın**|[Java SDK ile başlayın](sql-api-java-get-started.md)|
|**Web uygulaması eğitimi**|[Azure Cosmos DB ile web uygulaması geliştirme](sql-api-java-application.md)|
|**Desteklenen minimum çalışma süresi**|[Java Geliştirme Kiti (JDK) 7+](https://aka.ms/azure-jdks)|

## <a name="release-notes"></a>Sürüm notları

### <a name="247"></a><a name="2.4.7"/>2.4.7
* Bağlantı havuzu zaman adabı sorununu giderir.
* Dahili yeniden denemelerde auth belirteci yenilemesini düzeltir.

### <a name="246"></a><a name="2.4.6"/>2.4.6
* DatabaseAccount'ta güncelleştirilen doğru istemci tarafı çoğaltma ilkesi etiketi ve veritabanıHesabı yapılandırması önbellekten okur.

### <a name="245"></a><a name="2.4.5"/>2.4.5
* Kullanıcı pkRangeId sağlıyorsa, geçersiz bölüm anahtar aralığı hatası yeniden denemekten kaçınmak.

### <a name="244"></a><a name="2.4.4"/>2.4.4
* Optimize edilmiş bölüm anahtar aralığı önbelleği yeniler.
* SDK'nın sunucudan bölüm bölme ipucunu eğlendirmediği ve istemci tarafı yönlendirme önbelleklerinin yenilenmesiyle sonuçlanan senaryoyu düzeltir.

### <a name="242"></a><a name="2.4.2"/>2.4.2
* Optimize edilmiş koleksiyon önbelleği yeniler.

### <a name="241"></a><a name="2.4.1"/>2.4.1
* İstek tanılama dizesinden iç özel durum iletisini almak için destek eklendi.

### <a name="240"></a><a name="2.4.0"/>2.4.0
* PartitionKeyDefinition sürümü api tanıtıldı.

### <a name="230"></a><a name="2.3.0"/>2.3.0
* Doğrudan mod için ayrı zaman ayırma desteği eklendi.

### <a name="223"></a><a name="2.2.3"/>2.2.3
* Hizmetten gelen null hata iletisi tüketilmesi ve belge istemcisi özel durum oluşturma.

### <a name="222"></a><a name="2.2.2"/>2.2.2
* Soket bağlantı geliştirme, SoKeepAlive varsayılan doğru ekleyerek.

### <a name="220"></a><a name="2.2.0"/>2.2.0
* İstek tanılama dize desteği eklendi.

### <a name="213"></a><a name="2.1.3"/>2.1.3
* Hash V2 için PartitionKey sabit hata.

### <a name="212"></a><a name="2.1.2"/>2.1.2
* Bileşik dizinler için destek eklendi.
* Yenilemeyi zorlamak için genel uç nokta yöneticisindeki hata düzeltildi.
* Ön koşullara sahip ups için doğrudan modda hata düzeltildi.

### <a name="211"></a><a name="2.1.1"/>2.1.1
* Ağ geçidi adresi önbelleğindeki hata düzeltildi.

### <a name="210"></a><a name="2.1.0"/>2.1.0
* Doğrudan mod için çok bölgeli yazma desteği eklendi.
* Bir proxy'den ServiceUnavailable özel durumlar olarak atılan IOException'ları işlemek için destek eklendi.
* Uç nokta bulma yeniden deneme ilkesindeki bir hata düzeltildi.
* Null pointer özel durumları BaseDatabaseAccountConfigurationProvider atılmaz sağlamak için bir hata düzeltildi.
* QueryIterator nulls döndürmez emin olmak için bir hata düzeltildi.
* Büyük PartitionKey'e izin verildiğinden emin olmak için hata düzeltildi

### <a name="200"></a><a name="2.0.0"/>2.0.0
* Ağ geçidi modu için çok bölgeli yazma desteği eklendi.

### <a name="1164"></a><a name="1.16.4"/>1.16.4
* Sorgu için Oku bölümü Anahtar aralıkları bir hata düzeltildi.

### <a name="1163"></a><a name="1.16.3"/>1.16.3
* DoğrudanHttps modunda belirteç üstbilgi boyutunu ayarlamada bir hata düzeltildi.

### <a name="1162"></a><a name="1.16.2"/>1.16.2
* Destek üzerinden akış başarısız eklendi.
* Özel meta veriler için destek eklendi.
* Geliştirilmiş oturum işleme mantığı.
* Bölüm anahtar aralığı önbelleğindeki bir hata düzeltildi.
* Doğrudan modda bir NPE hatası düzeltildi.

### <a name="1161"></a><a name="1.16.1"/>1.16.1
* Benzersiz Diziliş için destek eklendi.
* Özet akışı seçeneklerinde devam belirteç boyutunu sınırlamak için destek eklendi.
* Json Serialization (zaman damgası) bir hata düzeltildi.
* Json Serialization (enum) bir hata düzeltildi.
* com.fasterxml.jackson.core bağımlılık: jackson-databind yükseltilmiş 2.9.5.

### <a name="1160"></a><a name="1.16.0"/>1.16.0
* Doğrudan Mod için Geliştirilmiş Bağlantı Havuzu.
* Sıralı olmayan çapraz bölüm sorgusu için geliştirilmiş Prefetch geliştirme.
* Geliştirilmiş UUID üretimi.
* Geliştirilmiş Oturum tutarlılık mantığı.
* Multipoligon için destek eklendi.
* Toplama için Bölüm Anahtar Aralığı İstatistikleri için destek eklendi.
* Çok bölgeli destekteki bir hata düzeltildi.

### <a name="1150"></a><a name="1.15.0"/>1.15.0
* Geliştirilmiş Json Serileştirme performansı.
* Bu SDK sürümü, Azure Cosmos DB Emulator'un https://aka.ms/cosmosdb-emulatoren son sürümünü n için gerekli.

### <a name="1140"></a><a name="1.14.0"/>1.14.0
* Microsoft arkadaş kitaplıkları için iç değişiklikler.

### <a name="1130"></a><a name="1.13.0"/>1.13.0
* Tek bölüm anahtar aralıklarını okumada bir sorun giderildi.
* Kısa adlarla veritabanını etkileyen ResourceID ayrıştırma sorunu düzeltildi.
* Bölüm anahtarı kodlaması ile bir sorun nedeni düzeltildi.

### <a name="1120"></a><a name="1.12.0"/>1.12.0
* Bölüm bölmeleri sırasında işleme isteğinde bulunmak için kritik hata giderir.
* Güçlü ve BoundedStaleness tutarlılık düzeyleri ile ilgili bir sorun giderildi.

### <a name="1110"></a><a name="1.11.0"/>1.11.0
* Consistent Prefix adlı yeni bir tutarlılık düzeyi için destek eklendi.
* Oturum modunda okuma koleksiyonundaki bir hata düzeltildi.

### <a name="1100"></a><a name="1.10.0"/>1.10.0
* 2.500 RU/sn'ye kadar düşük bölümlenmiş toplama desteği ve 100 RU/sn'lik artışlarla ölçeklendi.
* Bazı sorgularda NullRef özel durum neden olabilir yerel derlemede bir hata düzeltildi.

### <a name="196"></a><a name="1.9.6"/>1.9.6
* Sorgu motoru yapılandırmasında Ağ Geçidi modundaki sorgular için özel durumlara neden olabilecek bir hata düzeltildi.
* Toplama oluşturulduktan hemen sonra istekler için "Sahip kaynağı bulunamadı" özel duruma neden olabilecek oturum kapsayıcısındaki birkaç hata düzeltildi.

### <a name="195"></a><a name="1.9.5"/>1.9.5
* Toplama sorguları (COUNT, MIN, MAX, SUM ve AVG) için destek eklendi. Bkz. [Toplama desteği.](sql-query-aggregates.md)
* Değişiklik akışı için destek eklendi.
* RequestOptions.setPopulateQuotaInfo aracılığıyla toplama kotası bilgileri için destek eklendi.
* RequestOptions.setScriptLoggingEnabled üzerinden saklanan yordam komut dosyası günlüğe kaydetme desteği eklendi.
* DirectHttps modunda sorgunun gaz hatalarıyla karşılaştığında yanıt vermeyi durdurabileceği bir hata düzeltildi.
* Oturum tutarlılık modunda bir hata düzeltildi.
* İstek oranı yüksek olduğunda HttpContext'ta NullReferenceException'a neden olabilecek bir hata düzeltildi.
* DirectHttps modunun geliştirilmiş performansı.

### <a name="194"></a><a name="1.9.4"/>1.9.4
* ConnectionPolicy.setProxy() API ile basit istemci örnek tabanlı proxy desteği eklendi.
* DocumentClient örneğini düzgün bir şekilde kapatmak için DocumentClient.close() API eklendi.
* Ağ Geçidi yerine yerel derlemeden sorgu planı türeterek doğrudan bağlantı modunda sorgu performansını artırdı.
* Set FAIL_ON_UNKNOWN_PROPERTIES = yanlış böylece kullanıcıların KENDI POJO JsonIgnoreProperties tanımlamak gerekmez.
* SLF4J kullanmak için yeniden düzenlemeli günlük.
* Tutarlılık okuyucuda birkaç hata daha düzeltildi.

### <a name="193"></a><a name="1.9.3"/>1.9.3
* Doğrudan bağlantı modunda bağlantı sızıntılarını önlemek için bağlantı yönetimindeki bir hata düzeltildi.
* NULLReference özel durum atabileceği TOP sorgusundaki bir hata düzeltildi.
* Dahili önbellekler için ağ çağrısı sayısını azaltarak performansı artırın.
* Daha iyi sorun giderme için DocumentClientException'da durum kodu, ActivityID ve Request URI eklendi.

### <a name="192"></a><a name="1.9.2"/>1.9.2
* İstikrar için bağlantı yönetiminde bir sorun giderildi.

### <a name="191"></a><a name="1.9.1"/>1.9.1
* BoundedStaleness tutarlılık düzeyi için destek eklendi.
* Bölümlenmiş koleksiyonlar için CRUD işlemleri için doğrudan bağlantı desteği eklendi.
* Sql ile bir veritabanı sorgulayan bir hata düzeltildi.
* Oturum belirtecinin yanlış ayarlanabileceği oturum önbelleğindeki bir hata düzeltildi.

### <a name="190"></a><a name="1.9.0"/>1.9.0
* Çapraz bölme paralel sorguları için destek eklendi.
* Bölümlenmiş koleksiyonlar için TOP/ORDER sorguları için destek eklendi.
* Güçlü tutarlılık için destek eklendi.
* Doğrudan bağlantı kullanırken ad tabanlı istekler için destek eklendi.
* ActivityId'in tüm istek yeniden denemeleri arasında tutarlı kalmasını sağlamak için düzeltildi.
* Aynı ada sahip bir koleksiyon yeniden oluştururken oturum önbelleğiyle ilgili bir hata düzeltildi.
* Coğrafi eskrim uzamsal sorguları için koleksiyon dizini ilkesi ni belirtirken Çokgen ve LineString DataTypes eklendi.
* Java 1.8 için Java Dokümanı ile ilgili sorunlar giderildi.

### <a name="181"></a><a name="1.8.1"/>1.8.1
* BölümKeyDefinitionMap'te tek bölüm koleksiyonlarını önbelleğe almak ve ekstra bölüm anahtarı isteklerini almamak için bir hata düzeltildi.
* Yanlış bölüm anahtar değeri sağlandığında hatanın yeniden denemeyecek şekilde düzeltilmesi.

### <a name="180"></a><a name="1.8.0"/>1.8.0
* Çok bölgeli veritabanı hesapları için destek eklendi.
* Maksimum yeniden deneme denemelerini özelleştirme seçenekleri ve maksimum yeniden deneme bekleme süresini özelleştirme seçenekleriyle daraltılmış isteklerde otomatik yeniden deneme desteği eklendi.  Bkz. RetryOptions ve ConnectionPolicy.getRetryOptions().
* Deprecated IPartitionResolver tabanlı özel bölümleme kodu. Lütfen daha yüksek depolama ve iş elde etmek için bölümlenmiş koleksiyonları kullanın.

### <a name="171"></a><a name="1.7.1"/>1.7.1
* Oran sınırlaması için yeniden deneme ilkesi desteği eklendi.  

### <a name="170"></a><a name="1.7.0"/>1.7.0
* Belgeler için canlı (TTL) desteği eklendi.

### <a name="160"></a><a name="1.6.0"/>1.6.0
* Uygulanan [bölümlenmiş koleksiyonlar](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri.](performance-levels.md)

### <a name="151"></a><a name="1.5.1"/>1.5.1
* HashPartitionResolver'da diğer SDK'larla tutarlı olacak şekilde küçük endian'da karma değerler oluşturmak için bir hata düzeltildi.

### <a name="150"></a><a name="1.5.0"/>1.5.0
* Birden çok bölüm deki parçaları niçin parçalama uygulamalarına yardımcı olmak için Karma & Aralığı bölüm çözümleyicileri ekleyin.

### <a name="140"></a><a name="1.4.0"/>1.4.0
* Upsert'i uygulayın. Upsert özelliğini desteklemek için yeni upsertXXX yöntemleri eklendi.
* Kimlik Tabanlı Yönlendirme'yi uygulayın. Genel API değişikliği yok, tüm değişiklikler dahili.

### <a name="130"></a><a name="1.3.0"/>1.3.0
* Sürüm numarasını diğer SDK'larla uyumlu olarak getirmek için atlandı

### <a name="120"></a><a name="1.2.0"/>1.2.0
* Jeouzamsal Endeksi Destekler
* Tüm kaynaklar için kimlik özelliğini doğrular. Kaynaklar için ids ?, /, \, #, karakterler veya bir boşluk ile biten içeremez.
* ResourceResponse'a yeni üstbilgi "dizin dönüştürme ilerlemesi" ekler.

### <a name="110"></a><a name="1.1.0"/>1.1.0
* V2 dizin oluşturma ilkesini uygular

### <a name="100"></a><a name="1.0.0"/>1.0.0
* GA SDK

## <a name="release-and-retirement-dates"></a>Serbest bırakma ve emeklilik tarihleri
Microsoft, daha yeni/desteklenen bir sürüme geçişi kolaylaştırmak için bir SDK'nın emekliye geçmesinden en az **12 ay** önce bildirim de verecektir.

Yeni özellikler, işlevsellik ve optimizasyonlar yalnızca geçerli SDK'ya eklenir, bu nedenle her zaman en son SDK sürümüne mümkün olduğunca erken yükseltmeniz tavsiye edilir.

Cosmos DB'ye emekli bir SDK kullanarak yapılan tüm istekler hizmet tarafından reddedilecektir.

> [!WARNING]
> Java için SQL **SDK'nın 1.x** tüm sürümleri **30 Mayıs 2020'de**kullanımdan kaldırılacaktır.
> 
>

> [!WARNING]
> Java için SQL SDK'nın **sürüm 1.0.0'dan** önceki tüm sürümleri **29 Şubat 2016'da**kullanımdan kaldırılmıştır.
> 
> 

<br/>

| Sürüm | Yayın Tarihi | Emeklilik Tarihi |
| --- | --- | --- |
| [2.4.7](#2.4.7) |Şub 20, 2020 |--- |
| [2.4.6](#2.4.6) |24 Ocak 2020 |--- |
| [2.4.5](#2.4.5) |Kasım 10, 2019 |--- |
| [2.4.4](#2.4.4) |24 Ekim 2019 |--- |
| [2.4.2](#2.4.2) |26 Eylül 2019 |--- |
| [2.4.1](#2.4.1) |Temmuz 18, 2019 |--- |
| [2.4.0](#2.4.0) |Mayıs 04, 2019 |--- |
| [2.3.0](#2.3.0) |24 Nisan 2019 |--- |
| [2.2.3](#2.2.3) |16 Nisan 2019 |--- |
| [2.2.2](#2.2.2) |Nisan 05, 2019 |--- |
| [2.2.0](#2.2.0) |Mar 27, 2019 |--- |
| [2.1.3](#2.1.3) |Mar 13, 2019 |--- |
| [2.1.2](#2.1.2) |Mar 09, 2019 |--- |
| [2.1.1](#2.1.1) |Aralık 13, 2018 |--- |
| [2.1.0](#2.1.0) |Kasım 20, 2018 |--- |
| [2.0.0](#2.0.0) |21 Eylül 2018 |--- |
| [1.16.4](#1.16.4) |10 Eylül 2018 |Mayıs 30, 2020 |
| [1.16.3](#1.16.3) |09 Eylül 2018 |Mayıs 30, 2020 |
| [1.16.2](#1.16.2) |29 Haziran 2018 |Mayıs 30, 2020 |
| [1.16.1](#1.16.1) |Mayıs 16, 2018 |Mayıs 30, 2020 |
| [1.16.0](#1.16.0) |15 Mart 2018 |Mayıs 30, 2020 |
| [1.15.0](#1.15.0) |Kasım 14, 2017 |Mayıs 30, 2020 |
| [1.14.0](#1.14.0) |28 Ekim 2017 |Mayıs 30, 2020 |
| [1.13.0](#1.13.0) |Ağustos 25, 2017 |Mayıs 30, 2020 |
| [1.12.0](#1.12.0) |11 Temmuz 2017 |Mayıs 30, 2020 |
| [1.11.0](#1.11.0) |10 Mayıs 2017 |Mayıs 30, 2020 |
| [1.10.0](#1.10.0) |11 Mart 2017 |Mayıs 30, 2020 |
| [1.9.6](#1.9.6) |Şubat 21, 2017 |Mayıs 30, 2020 |
| [1.9.5](#1.9.5) |31 Ocak 2017 |Mayıs 30, 2020 |
| [1.9.4](#1.9.4) |Kasım 24, 2016 |Mayıs 30, 2020 |
| [1.9.3](#1.9.3) |Ekim 30, 2016 |Mayıs 30, 2020 |
| [1.9.2](#1.9.2) |Ekim 28, 2016 |Mayıs 30, 2020 |
| [1.9.1](#1.9.1) |Ekim 26, 2016 |Mayıs 30, 2020 |
| [1.9.0](#1.9.0) |Ekim 03, 2016 |Mayıs 30, 2020 |
| [1.8.1](#1.8.1) |Haziran 30, 2016 |Mayıs 30, 2020 |
| [1.8.0](#1.8.0) |Haziran 14, 2016 |Mayıs 30, 2020 |
| [1.7.1](#1.7.1) |30 Nisan 2016 |Mayıs 30, 2020 |
| [1.7.0](#1.7.0) |27 Nisan 2016 |Mayıs 30, 2020 |
| [1.6.0](#1.6.0) |29 Mart 2016 |Mayıs 30, 2020 |
| [1.5.1](#1.5.1) |31 Aralık 2015 |Mayıs 30, 2020 |
| [1.5.0](#1.5.0) |04 Aralık 2015 |Mayıs 30, 2020 |
| [1.4.0](#1.4.0) |05 Ekim 2015 |Mayıs 30, 2020 |
| [1.3.0](#1.3.0) |05 Ekim 2015 |Mayıs 30, 2020 |
| [1.2.0](#1.2.0) |05 Ağustos 2015 |Mayıs 30, 2020 |
| [1.1.0](#1.1.0) |Temmuz 09, 2015 |Mayıs 30, 2020 |
| 1.0.1 |Mayıs 12, 2015 |Mayıs 30, 2020 |
| [1.0.0](#1.0.0) |07 Nisan 2015 |Mayıs 30, 2020 |
| 0.9.5-prelease |Mar 09, 2015 |Şubat 29, 2016 |
| 0.9.4-prelease |Şubat 17, 2015 |Şubat 29, 2016 |
| 0.9.3-prelease |13 Ocak 2015 |Şubat 29, 2016 |
| 0.9.2-prelease |19 Aralık 2014 |Şubat 29, 2016 |
| 0.9.1-prelease |19 Aralık 2014 |Şubat 29, 2016 |
| 0.9.0-prelease |10 Aralık 2014 |Şubat 29, 2016 |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi edinmek için [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfasına bakın.

