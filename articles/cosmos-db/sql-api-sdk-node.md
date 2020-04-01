---
title: 'Azure Cosmos DB: SQL Node.js API, SDK & kaynakları'
description: Sürüm tarihleri, emeklilik tarihleri ve Azure Cosmos DB Node.js SDK'nın her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL Node.js API ve SDK hakkında her şeyi öğrenin.
author: deborahc
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: nodejs
ms.topic: reference
ms.date: 09/24/2018
ms.author: dech
ms.openlocfilehash: 181f43e060e3b4b41babeee375365a97e1d224aa
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80411278"
---
# <a name="azure-cosmos-db-nodejs-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için Azure Cosmos DB Düğümü.js SDK: Sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET Değişim Akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu uygulayıcı - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu uygulayıcı - Java](sql-api-sdk-bulk-executor-java.md)

|Kaynak  |Bağlantı  |
|---------|---------|
|SDK'yı İndir  |   [NPM](https://www.npmjs.com/package/@azure/cosmos) 
|API Dokümantasyon  |  [JavaScript SDK başvuru belgeleri](https://docs.microsoft.com/javascript/api/%40azure/cosmos/?view=azure-node-latest)
|SDK kurulum talimatları  |  [Yükleme yönergeleri](https://github.com/Azure/azure-cosmos-js#installation)
|SDK'ya Katkıda Bulunun | [GitHub](https://github.com/Azure/azure-cosmos-js/tree/master)
| Örnekler | [Düğüm.js kod örnekleri](sql-api-nodejs-samples.md)
| Öğreticiye başlarken | [JavaScript SDK ile başlayın](sql-api-nodejs-get-started.md)
| Web uygulaması eğitimi | [Azure Cosmos DB kullanarak bir Düğüm.js web uygulaması oluşturma](sql-api-nodejs-application.md)
| Geçerli desteklenen platform | [Düğüm.js v12.x](https://nodejs.org/en/blog/release/v12.7.0/) - SDK Sürüm 3.x.x<br/>[Düğüm.js v10.x](https://nodejs.org/en/blog/release/v10.6.0/) - SDK Sürüm 3.x.x<br/>[Düğüm.js v8.x](https://nodejs.org/en/blog/release/v8.16.0/) - SDK Sürüm 3.x.x<br/>[Düğüm.js v6.x](https://nodejs.org/en/blog/release/v6.10.3/) - SDK Sürüm 2.x.x<br/>[Düğüm.js v4.2.0](https://nodejs.org/en/blog/release/v4.2.0/)- SDK Sürüm 1.x.x<br/> [Düğüm.js v0.12](https://nodejs.org/en/blog/release/v0.12.0/)- SDK Sürüm 1.x.x<br/> [Düğüm.js v0.10](https://nodejs.org/en/blog/release/v0.10.0/)- SDK Sürüm 1.x.x

## <a name="release-notes"></a>Sürüm notları

### <a name=""></a><a name="3.1.0"/>3.1.0</a>
* Varsayılan ResponseContinuationTokenLimitInKB'yi 1kb olarak ayarlayın. Varsayılan olarak, uzun üstbilgi (Node.js genel bir üstbilgi boyutu sınırı vardır) önlemek için 1kb bu kapama vardır. Bir kullanıcı bu alanı, arka uç sorgusu yürütmeyi en iyi duruma getirmelerine yardımcı olabilecek daha uzun üstbilgi için izin verecek şekilde ayarlayabilir.
* Devre dışı bırakılmasıSSLVerification kaldırın. Bu [seçenek, #388](https://github.com/Azure/azure-cosmos-js/pull/388) açıklanan yeni alternatifler vardır

### <a name=""></a><a name="3.0.4"/>3.0.4</a>
* Başlangıç Üstharflerin bölüm anahtar üstbilgisini açıkça ayarlamasına izin ver
* Gereksiz dosyaların yayımlanmasını önlemek için package.json#dosyalarını kullanma
* Düğüm+v8'in eski sürümünde yönlendirme eşleme sıralama hatasini düzeltme
* Kullanıcı kısmi yeniden deneme seçenekleri sağladığında hatayı giderir

### <a name=""></a><a name="3.0.3"/>3.0.3</a>
* Webpack'in gereksinimli olarak adlandırılan modülleri çözmesini engelleme

### <a name=""></a><a name="3.0.2"/>3.0.2</a>
* RUs'un her zaman toplu sorgular için 0 olarak raporlandığı uzun ve olağanüstü bir hatayı düzeltir

### <a name=""></a><a name="3.0.0"/>3.0.0</a>

🎉 v3 sürümü! 🎉 Birçok yeni özellik, hata düzeltmeleri ve birkaç kesme değişiklikleri. Bu sürümün birincil hedefleri:

* Önemli yeni özellikleri uygulayın
  * DISTINCT sorguları
  * LIMIT/OFSET sorguları
  * Kullanıcı iptal edilebilir istekler
* Tüm konteynerlerin sınırsız ölçekte olduğu en son Cosmos REST API sürümüne güncelleştirin
* Tarayıcıdan Cosmos'u kullanmayı kolaylaştırın
* Yeni Azure JS SDK yönergeleriyle daha iyi hizala

#### <a name="migration-guide-for-breaking-changes"></a>Değişiklikleri kırmak için geçiş kılavuzu
##### <a name="improved-client-constructor-options"></a>Geliştirilmiş istemci oluşturucu seçenekleri

Oluşturucu seçenekleri basitleştirilmiştir:

* masterKey anahtar olarak değiştirildi ve üst düzeye taşındı
* Options.auth altında daha önce özellikler üst düzeye taşındı

``` js
// v2
const client = new CosmosClient({
    endpoint: "https://your-database.cosmos.azure.com",
    auth: {
        masterKey: "your-primary-key"
    }
})

// v3
const client = new CosmosClient({
    endpoint: "https://your-database.cosmos.azure.com",
    key: "your-primary-key"
})
```

##### <a name="simplified-queryiterator-api"></a>Basitleştirilmiş QueryIterator API
v2'de bir sorgudan sonuçları doğrulamanın veya almanın birçok farklı yolu vardı. V3 API'sini basitleştirmeye ve benzer veya yinelenen API'leri kaldırmaya çalıştık:

* Remove iterator.next() ve iterator.current(). Sonuçların sayfalarını almak için fetchNext() kullanın.
* Reerator.forEach(). Bunun yerine async yineleyiciler kullanın.
* reerator.executeNext() reerator.fetchNext() olarak yeniden adlandırıldı
* iterator.toArray() reerator.fetchAll() olarak yeniden adlandırıldı
* Sayfalar artık düz JS nesneleri yerine uygun Yanıt nesneleridir
* const konteyner = client.database(dbId).container(containerId)

``` js
// v2
container.items.query('SELECT * from c').toArray()
container.items.query('SELECT * from c').executeNext()
container.items.query('SELECT * from c').forEach(({ body: item }) => { console.log(item.id) })

// v3
container.items.query('SELECT * from c').fetchAll()
container.items.query('SELECT * from c').fetchNext()
for await(const { result: item } in client.databases.readAll().getAsyncIterator()) {
    console.log(item.id)
}
```

##### <a name="fixed-containers-are-now-partitioned"></a>Sabit kapsayıcılar artık bölümlendi
Cosmos hizmeti artık, daha önce sabit kapsayıcı olarak oluşturulmuş olanlar da dahil olmak üzere tüm kapsayıcılarda bölüm anahtarlarını destekler. V3 SDK, bu değişikliği uygulayan en son API sürümüne güncelleştirir, ancak kırılmaz. İşlemler için bir bölüm anahtarı sağlamazsanız, varolan tüm kapsayıcılar ve belgelerle çalışan bir sistem anahtarına varsayılan olarak geliriz.

##### <a name="upsert-removed-for-stored-procedures"></a>Depolanan yordamlar için kaldırılabilir
Daha önce upsert bölümlenmemiş koleksiyonlar için izin verildi, ancak API sürüm güncelleştirmesi ile, tüm koleksiyonları bölümlenir, bu yüzden tamamen kaldırıldı.

##### <a name="item-reads-will-not-throw-on-404"></a>Öğe 404 atmayacak okur
const konteyner = client.database(dbId).container(containerId)

``` js
// v2
try {
    container.items.read(id, undefined)
} catch (e) {
    if (e.code === 404) { console.log('item not found') }
}

// v3
const { result: item }  = container.items.read(id, undefined)
if (item === undefined) { console.log('item not found') }
```

##### <a name="default-multi-region-write"></a>Varsayılan çok bölgeli yazma
Cosmos yapılandırmanız destekliyorsa SDK artık varsayılan olarak birden çok bölgeye yazacaktır. Bu daha önce kabul davranışı ydı.

##### <a name="proper-error-objects"></a>Uygun hata nesneleri
Başarısız istekler artık uygun Hata veya Hata alt sınıfları atmak. Daha önce düz JS nesneleri attı.

#### <a name="new-features"></a>Yeni özellikler
##### <a name="user-cancelable-requests"></a>Kullanıcı tarafından iptal edilebilen istekler
Dahili olarak getirme hamlesi, kullanıcı tarafından iptal edilebilen işlemleri desteklemek için Tarayıcı AbortController API'yi kullanmamıza olanak tanır. Birden çok isteğin devam etme potansiyeli olduğu işlemlerde (çapraz bölüm sorguları gibi), işlem için tüm istekler iptal edilir. Modern tarayıcı kullanıcıları zaten AbortController olacaktır. Düğüm.js kullanıcılarının çok dolgu kitaplığı kullanması gerekir

``` js
 const controller = new AbortController()
 const {result: item} = await items.query('SELECT * from c', { abortSignal: controller.signal});
 controller.abort()
```

##### <a name="set-throughput-as-part-of-dbcontainer-create-operation"></a>DB/konteyner oluşturma işleminin bir parçası olarak iş kısmını ayarlama
``` js
const { database }  = client.databases.create({ id: 'my-database', throughput: 10000 })
database.containers.create({ id: 'my-container', throughput: 10000 })
```

##### <a name="azurecosmos-sign"></a>@azure/cosmos-sign
Başlık belirteç oluşturma yeni bir kütüphaneye @azure/cosmos-signayrılmıştır. Cosmos REST API'yi doğrudan arayan herkes, içinde @azure/cosmosaradığımız kodu kullanarak üstbilgi imzalamak için bunu kullanabilir.

##### <a name="uuid-for-generated-ids"></a>Oluşturulan kimlikler için UUID
v2 öğe iT'leri oluşturmak için özel kod vardı. Biz tanınmış ve bakımlı toplum kütüphanesi uuid geçti.

##### <a name="connection-strings"></a>Bağlantı dizeleri
Azure portalından kopyalanan bir bağlantı dizesini geçmek artık mümkündür:

``` js
const client = new CosmosClient("AccountEndpoint=https://test-account.documents.azure.com:443/;AccountKey=c213asdasdefgdfgrtweaYPpgoeCsHbpRTHhxuMsTaw==;")
Add DISTINCT and LIMIT/OFFSET queries (#306)
 const { results } = await items.query('SELECT DISTINCT VALUE r.name FROM ROOT').fetchAll()
 const { results } = await items.query('SELECT * FROM root r OFFSET 1 LIMIT 2').fetchAll()
```

#### <a name="improved-browser-experience"></a>Geliştirilmiş tarayıcı deneyimi
Bu tarayıcıda v2 SDK kullanmak mümkün iken ideal bir deneyim değildi. Birkaç düğüm.js dahili kitaplıkları polyfill ve Webpack veya Parcel gibi bir bundler kullanmanız gerekiyordu. V3 SDK tarayıcı kullanıcıları için kutunun dışında deneyim çok daha iyi hale getirir.

* İstek iç lerini getir (#245) ile değiştirme
* Arabellek kullanımını kaldırma (#330)
* Evrensel paketler/API'ler (#328) lehine yerleşik düğüm kullanımını kaldırma
* Düğüm-iptal denetleyicisine geçiş (#294)

#### <a name="bug-fixes"></a>Hata düzeltmeleri
* Teklifi okuma ve teklif testlerini geri getirme (#224)
* EnableEndpointDiscovery'yi düzeltme (#207)
* Paginated sonuçlarda eksik RUs'ları düzeltme (#360)
* SQL sorgu parametre türünü genişletme (#346)
* ItemDefinition'a ttl ekleme (#341)
* CP sorgu ölçümlerini düzeltme (#311)
* FeedResponse'a activityId ekleme (#293)
* _ts türde dizeden sayıya geçiş (#252)(#295)
* İstek Ücret Toplama (#289) düzeltme
* Boş dize bölüm tuşlarına izin ver (#277)
* Çakışma sorgu türüne dize ekleme (#237)
* Kapsayıcıya uniqueKeyPolicy ekleyin (#234)

#### <a name="engineering-systems"></a>Mühendislik sistemleri
Her zaman en görünür değişiklikler değil, ama ekibimizin daha hızlı, daha iyi kod lar göndermelerine yardımcı olur.

* Üretim yapılarında toplama yı kullanma (#104)
* Typescript 3.5 (#327) olarak güncelleştir
* TS proje başvurularına dönüştürün. Ayıklama test klasörü (#270)
* Kullanılmayan Yerel leri ve NoUnusedParametreleri etkinleştirme (#275)
* CI yapılar için Azure Pipelines YAML (#298)

### <a name=""></a><a name="2.1.5"/>2.1.5</a>
* Kod değişikliği yok. Bazı ek dosyaların 2.1.4 paketine dahil edildiği bir sorunu giderir.

### <a name=""></a><a name="2.1.4"/>2.1.4</a>
* Yeniden deneme ilkesi içindeki bölgesel başarısızları düzeltme
* Fix ChangeFeed hasMoreResults özelliği
* Dev bağımlılık güncelleştirmeleri
* PolicheckExclusions.txt ekle

### <a name=""></a><a name="2.1.3"/>2.1.3</a>
* _ts tür demle dize
* Varsayılan dizin oluşturma testlerini düzeltme
* Backport uniqueKeyPolicy v2 için
* Demo ve demo hata ayıklama düzeltmeleri

### <a name=""></a><a name="2.1.2"/>2.1.2</a>
* V3 dalından Backport teklif düzeltmeleri
* yürütmedeki hatayı düzeltmeNext() türü imza
* Yazım hatası düzeltmeleri

### <a name=""></a><a name="2.1.1"/>2.1.1</a>
* Yeniden yapılanma oluşturun. SDK sürümünü niçin oluşturma zamanı çekmenizi sağlar.

### <a name=""></a><a name="2.1.0"/>2.1.0</a>
#### <a name="new-features"></a>Yeni Özellikler
* ChangeFeed desteği eklendi (#196)
* Dizin oluşturma için MultiPolygon veri türü eklendi (#191)
* MasterKey (#202) için diğer ad olarak oluşturucuya "anahtar" özelliği ekleme

#### <a name="fixes"></a>Düzeltmeler
* Sonraki() yineleyiciüzerinde yanlış değer döndürdü hata düzeltme

#### <a name="engineering-improvements"></a>Mühendislik Geliştirmeleri
* Yazı tipi tüketimi için tümleştirme testi ekleme (#199)
* Doğrudan GitHub'dan yüklemeyi etkinleştirin (#194)

### <a name=""></a><a name="2.0.5"/>2.0.5</a>
* Düğüm Agent türü için arabirim ekler. Typescript kullanıcıları artık bağımlılık @types/node olarak yüklemek zorunda
* Tercih edilen yerler artık uygun şekilde onurlandırılıyor
* Katkıda bulunan geliştirici belgelerinde iyileştirmeler
* Çeşitli yazım hatası düzeltmeleri

### <a name=""></a><a name="2.0.4"/>2.0.4</a>
* 2.0.3'te tanıtılan tür tanımı sorununu giderir

### <a name=""></a><a name="2.0.3"/>2.0.3</a>
* Bağımlılığı `big-integer` kaldırma
* AsyncIterable türü için başvuru yönergelerine geçin. Typescript kullanıcıları artık kendi "lib" ayarı özelleştirmek zorunda.
* Typo Düzeltmeler

### <a name=""></a><a name="2.0.2"/>2.0.2</a>
* Okuma bağlantıları düzeltme

### <a name=""></a><a name="2.0.1"/>2.0.1</a>
* Yeniden deneme arabirimi uygulamasını düzeltme

### <a name=""></a><a name="2.0.0"/>2.0.0</a>
* JavaScript SDK Sürüm 2.0.0 GA
* Çok bölgeli yazılar için destek eklendi.

### <a name=""></a><a name="2.0.0-3"/>2.0.0-3</a>
* Genel önizleme için JavaScript SDK Sürüm 2.0.0 RC1.
* Üst düzey CosmosClient ve yöntemleri ilgili Veritabanı, Kapsayıcı ve Öğe sınıfları arasında bölünmüş yeni nesne modeli. 
* [Vaatler](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises)için destek . 
* SDK TypeScript dönüştürülür.

### <a name=""></a><a name="1.14.4"/>1.14.4</a>
* npm belgeleri düzeltildi.

### <a name=""></a><a name="1.14.3"/>1.14.3</a>
* Bağlantı sorunları yla ilgili varsayılan yeniden denemeler için destek eklendi.
* Koleksiyon değişikliği akışını okumak için destek eklendi.
* Aralıklı olarak "okuma oturumu kullanılamıyor" neden olan oturum tutarlılık hatası düzeltildi.
* Sorgu ölçümleri için destek eklendi.
* Değiştirilmiş http Agent'ın maksimum bağlantı sayısı.

### <a name=""></a><a name="1.14.2"/>1.14.2</a>
* Azure DocumentDB yerine Azure Cosmos DB'ye başvurmak için güncelleştirilmiş belgeler.
* ConnectionPolicy'de proxyUrl ayarı için Destek eklendi.

### <a name=""></a><a name="1.14.1"/>1.14.1</a>
* Büyük/küçük harf duyarlı dosya sistemleri için küçük düzeltme.

### <a name=""></a><a name="1.14.0"/>1.14.0</a>
* Oturum Tutarlılığı için destek ekler.
* Bu SDK sürümü, Azure Cosmos DB Emulator'un https://aka.ms/cosmosdb-emulatoren son sürümünü n için gerekli.

### <a name=""></a><a name="1.13.0"/>1.13.0</a>
* Bölünmüş provalı çapraz bölüm sorguları.
* Satır aralığı ve sondaki kesiklerle (ve ilgili testlere) kaynak bağlantısı için destek ekler.

### <a name=""></a><a name="1.12.2"/>1.12.2</a>
*   npm belgeleri düzeltildi.

### <a name=""></a><a name="1.12.1"/>1.12.1</a>
* Ilgili belgelerin özel Unicode karakterleri (LS, PS) olduğu executeStoredProcedure'deki bir hata düzeltildi.
* Bölüm anahtarında Unicode karakterleri olan belgeleri işlemedeki bir hata düzeltildi.
* Ad ortamıyla koleksiyon oluşturmak için sabit destek. GitHub sorunu #114.
* İzin verme belirteci için sabit destek. GitHub sorunu #178.

### <a name=""></a><a name="1.12.0"/>1.12.0</a>
* Consistent Prefix adlı yeni bir [tutarlılık düzeyi](consistency-levels.md) için destek eklendi.
* UriFactory için destek eklendi.
* Unicode destek hatası düzeltildi. GitHub sorunu #171.

### <a name=""></a><a name="1.11.0"/>1.11.0</a>
* Toplama sorguları (COUNT, MIN, MAX, SUM ve AVG) için destek eklendi.
* Çapraz bölüm sorguları için paralellik derecesini denetleme seçeneği eklendi.
* Azure Cosmos DB Emulator'a karşı çalışırken TLS doğrulamasını devre dışı bırakma seçeneği eklendi.
* 10.100 RU/s'den 2500 RU/s'ye kadar bölümlenmiş koleksiyonlarda minimum iş belini düşürür.
* Tek bölüm koleksiyonu için devam belirteç hatası düzeltildi. GitHub sorunu #107.
* 0'ı tek param olarak işlemede executeStoredProcedure hatası düzeltildi. GitHub sorunu #155.

### <a name=""></a><a name="1.10.2"/>1.10.2</a>
* SDK sürümünü içerecek şekilde kullanıcı aracıüst'i düzeltildi.
* Küçük kod temizleme.

### <a name=""></a><a name="1.10.1"/>1.10.1</a>
* Emülatör (hostname=localhost) hedeflemek için SDK kullanırken TLS doğrulamasını devre dışı bırakmak.
* Depolanan yordam yürütme sırasında komut dosyası günlüğe kaydetmeyi etkinleştirmek için destek eklendi.

### <a name=""></a><a name="1.10.0"/>1.10.0</a>
* Çapraz bölme paralel sorguları için destek eklendi.
* Bölümlenmiş koleksiyonlar için TOP/ORDER sorguları için destek eklendi.

### <a name=""></a><a name="1.9.0"/>1.9.0</a>
* Daraltılmış istekler için yeniden deneme ilkesi desteği eklendi. (Daraltılmış istekler çok büyük bir özel durum, hata kodu 429 bir istek oranı alır.) Varsayılan olarak, Azure Cosmos DB hata kodu 429 ile karşılaşıldığında her istek için dokuz kez yeniden çalışır ve yanıt üstbilgisinde yeniden denemeyi onurlandırır. Yeniden deneme aralığı, yeniden denemeler arasında sunucu tarafından döndürülen yeniden denemesüresini göz ardı etmek istiyorsanız, ConnectionPolicy nesnesi üzerindeki RetryOptions özelliğinin bir parçası olarak ayarlanabilir. Azure Cosmos DB artık daraltılmış olan her istek için en fazla 30 saniye bekler (yeniden deneme sayısına bakılmaksızın) ve yanıtı hata kodu 429 ile döndürür. Bu süre ConnectionPolicy nesnesi üzerindeki RetryOptions özelliğinde de geçersiz kılınabilir.
* Cosmos DB şimdi x-ms-throttle-retry-count ve x-ms-throttle-retry-wait-time-ms'i, gaz yeniden deneme sayısını ve isteğin yeniden denemeler arasında beklediği kümülatif süreyi belirtmek için her istekteki yanıt başlıkları olarak döndürür.
* Yeniden Deneme Seçenekleri sınıfı eklendi ve ConnectionPolicy sınıfında varsayılan yeniden deneme seçeneklerinden bazılarını geçersiz kılmak için kullanılabilecek RetryOptions özelliği açığa çıkarıldı.

### <a name=""></a><a name="1.8.0"/>1.8.0</a>
* Çok bölgeli veritabanı hesapları için destek eklendi.

### <a name=""></a><a name="1.7.0"/>1.7.0</a>
* Belgeler için Time To Live(TTL) özelliği için destek eklendi.

### <a name=""></a><a name="1.6.0"/>1.6.0</a>
* Uygulanan [bölümlenmiş koleksiyonlar](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri.](performance-levels.md)

### <a name=""></a><a name="1.5.6"/>1.5.6</a>
* Sabit RangePartitionResolver.resolveForRead hata nerede sonuçları kötü bir concat nedeniyle bağlantıları döndürme değildi.

### <a name=""></a><a name="1.5.5"/>1.5.5</a>
* Sabit hashPartitionResolver resolveForRead(): Hiçbir bölüm anahtarı sağlanan istisna atma, yerine tüm kayıtlı bağlantıların bir listesini döndürdü.

### <a name=""></a><a name="1.5.4"/>1.5.4</a>
* #100 giderir [-](https://github.com/Azure/azure-documentdb-node/issues/100) Özel HTTPS Aracısı: Azure Cosmos DB amaçları için genel aracıyı değiştirmekten kaçının. Lib'in tüm istekleri için özel bir aracı kullanın.

### <a name=""></a><a name="1.5.3"/>1.5.3</a>
* [Sorunu #81](https://github.com/Azure/azure-documentdb-node/issues/81) giderir - Ortam kimliklerinde tireleri düzgün şekilde işler.

### <a name=""></a><a name="1.5.2"/>1.5.2</a>
* Sorunu [#95](https://github.com/Azure/azure-documentdb-node/issues/95) giderir - EventEmitter dinleyici sızıntısı uyarısı.

### <a name=""></a><a name="1.5.1"/>1.5.1</a>
* Sorunu [#92](https://github.com/Azure/azure-documentdb-node/issues/90) giderir - karma klasörü, büyük/küçük harf duyarlı sistemler için karma olarak yeniden adlandırın.

### <a name=""></a><a name="1.5.0"/>1.5.0</a>
* Karma & aralığı bölüm çözümleyicileri ekleyerek parçalama desteği uygulayın.

### <a name=""></a><a name="1.4.0"/>1.4.0</a>
* Upsert'i uygulayın. DocumentClient'da yeni upsertXXX yöntemleri.

### <a name=""></a><a name="1.3.0"/>1.3.0</a>
* Sürüm numaralarını diğer SDK'larla uyumlu getirmek için atlanır.

### <a name=""></a><a name="1.2.2"/>1.2.2</a>
* Split Q yeni depo için sarma layıcı vaat ediyor.
* npm kayıt defteri için paket dosyasına güncelleştirin.

### <a name=""></a><a name="1.2.1"/>1.2.1</a>
* Kimlik Tabanlı Yönlendirme'yi uygular.
* Sorunları [#49](https://github.com/Azure/azure-documentdb-node/issues/49) Giderir - geçerli özellik yöntem akımı ile çakışıyor().

### <a name=""></a><a name="1.2.0"/>1.2.0</a>
* GeoSpatial endeksi için destek eklendi.
* Tüm kaynaklar için kimlik özelliğini doğrular. Kaynaklariçin kullanılan bilgiler ?, /, #, &#47;&#47;, karakterler içeremez veya boşlukla sonlandırılamaz.
* ResourceResponse'a yeni üstbilgi "dizin dönüştürme ilerlemesi" ekler.

### <a name=""></a><a name="1.1.0"/>1.1.0</a>
* V2 dizin oluşturma ilkesini uygular.

### <a name=""></a><a name="1.0.3"/>1.0.3</a>
* Sorun [#40](https://github.com/Azure/azure-documentdb-node/issues/40) - Çekirdek ve söz SDK eslint ve homurtu yapılandırmaları uygulanmaktadır.

### <a name=""></a><a name="1.0.2"/>1.0.2</a>
* Sorun [#45](https://github.com/Azure/azure-documentdb-node/issues/45) - Promises sarıcı hata ile üstbilgi içermez.

### <a name=""></a><a name="1.0.1"/>1.0.1</a>
* ReadConflicts, readConflictAsync ve queryConflicts ekleyerek çakışmaları sorgulama yeteneği uygulanmıştır.
* Güncelleştirilmiş API belgeleri.
* Sorun [#41](https://github.com/Azure/azure-documentdb-node/issues/41) - istemci.createDocumentAsync hatası.

### <a name=""></a><a name="1.0.0"/>1.0.0</a>
* GA SDK.

## <a name="release--retirement-dates"></a>Emeklilik Tarihleri & Yayın
Microsoft, daha yeni/desteklenen bir sürüme geçişi kolaylaştırmak için SDK'nın emekliye geçmesinden en az **12 ay** önce bildirim sağlar.

Yeni özellikler, işlevsellik ve optimizasyonlar yalnızca geçerli SDK'ya eklenir, bu nedenle her zaman en son SDK sürümüne mümkün olduğunca erken yükseltmeniz önerilir.

Cosmos DB'ye emekli bir SDK kullanarak yapılan tüm istekler hizmet tarafından reddedilecektir.

> [!WARNING]
> SQL API için Düğüm istemcisi **SDK'nın 1.x** tüm sürümleri **30 Ağustos 2020'de**kullanımdan kaldırılacaktır. Bu yalnızca istemci tarafındaki Düğüm SDK'yı etkiler ve sunucu tarafındaki komut dosyalarını (depolanan yordamlar, tetikleyiciler ve UDF'leri) etkilemez.
> 
>
<br/>

| Sürüm | Yayın Tarihi | Emeklilik Tarihi |
| --- | --- | --- |
| [3.1.0](#3.1.0) |Temmuz 26, 2019 |--- |
| [3.0.4](#3.0.4) |Temmuz 22, 2019 |--- |
| [3.0.3](#3.0.3) |Temmuz 17, 2019 |--- |
| [3.0.2](#3.0.2) |9 Temmuz 2019 |--- |
| [3.0.0](#3.0.0) |Haziran 28, 2019 |--- |
| [2.1.5](#2.1.5) |20 Mart 2019 |--- |
| [2.1.4](#2.1.4) |15 Mart 2019 |--- |
| [2.1.3](#2.1.3) |Mart 8, 2019 |--- |
| [2.1.2](#2.1.2) |Ocak 28, 2019 |--- |
| [2.1.1](#2.1.1) |5 Aralık 2018 |--- |
| [2.1.0](#2.1.0) |4 Aralık 2018 |--- |
| [2.0.5](#2.0.5) |Kasım 7, 2018 |--- |
| [2.0.4](#2.0.4) |Ekim 30, 2018 |--- |
| [2.0.3](#2.0.3) |Ekim 30, 2018 |--- |
| [2.0.2](#2.0.2) |Ekim 10, 2018 |--- |
| [2.0.1](#2.0.1) |25 Eylül 2018 |--- |
| [2.0.0](#2.0.0) |Eylül 24, 2018 |--- |
| [2.0.0-3 (RC)](#2.0.0-3) |Ağustos 2, 2018 |--- |
| [1.14.4](#1.14.4) |03 Mayıs 2018 |Ağustos 30, 2020 |
| [1.14.3](#1.14.3) |03 Mayıs 2018 |Ağustos 30, 2020 |
| [1.14.2](#1.14.2) |21 Aralık 2017 |Ağustos 30, 2020 |
| [1.14.1](#1.14.1) |10 Kasım 2017 |Ağustos 30, 2020 |
| [1.14.0](#1.14.0) |9 Kasım 2017 |Ağustos 30, 2020 |
| [1.13.0](#1.13.0) |Ekim 11, 2017 |Ağustos 30, 2020 |
| [1.12.2](#1.12.2) |Ağustos 10, 2017 |Ağustos 30, 2020 |
| [1.12.1](#1.12.1) |Ağustos 10, 2017 |Ağustos 30, 2020 |
| [1.12.0](#1.12.0) |10 Mayıs 2017 |Ağustos 30, 2020 |
| [1.11.0](#1.11.0) |16 Mart 2017 |Ağustos 30, 2020 |
| [1.10.2](#1.10.2) |27 Ocak 2017 |Ağustos 30, 2020 |
| [1.10.1](#1.10.1) |22 Aralık 2016 |Ağustos 30, 2020 |
| [1.10.0](#1.10.0) |Ekim 03, 2016 |Ağustos 30, 2020 |
| [1.9.0](#1.9.0) |Temmuz 07, 2016 |Ağustos 30, 2020 |
| [1.8.0](#1.8.0) |Haziran 14, 2016 |Ağustos 30, 2020 |
| [1.7.0](#1.7.0) |26 Nisan 2016 |Ağustos 30, 2020 |
| [1.6.0](#1.6.0) |29 Mart 2016 |Ağustos 30, 2020 |
| [1.5.6](#1.5.6) |08 Mart 2016 |Ağustos 30, 2020 |
| [1.5.5](#1.5.5) |Şubat 02, 2016 |Ağustos 30, 2020 |
| [1.5.4](#1.5.4) |Şubat 01, 2016 |Ağustos 30, 2020 |
| [1.5.2](#1.5.2) |26 Ocak 2016 |Ağustos 30, 2020 |
| [1.5.2](#1.5.2) |22 Ocak 2016 |Ağustos 30, 2020 |
| [1.5.1](#1.5.1) |4 Ocak 2016 |Ağustos 30, 2020 |
| [1.5.0](#1.5.0) |31 Aralık 2015 |Ağustos 30, 2020 |
| [1.4.0](#1.4.0) |06 Ekim 2015 |Ağustos 30, 2020 |
| [1.3.0](#1.3.0) |06 Ekim 2015 |Ağustos 30, 2020 |
| [1.2.2](#1.2.2) |Eylül 10, 2015 |Ağustos 30, 2020 |
| [1.2.1](#1.2.1) |Ağustos 15, 2015 |Ağustos 30, 2020 |
| [1.2.0](#1.2.0) |05 Ağustos 2015 |Ağustos 30, 2020 |
| [1.1.0](#1.1.0) |Temmuz 09, 2015 |Ağustos 30, 2020 |
| [1.0.3](#1.0.3) |04 Haziran 2015 |Ağustos 30, 2020 |
| [1.0.2](#1.0.2) |Mayıs 23, 2015 |Ağustos 30, 2020 |
| [1.0.1](#1.0.1) |Mayıs 15, 2015 |Ağustos 30, 2020 |
| [1.0.0](#1.0.0) |08 Nisan 2015 |Ağustos 30, 2020 |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi edinmek için [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfasına bakın.

