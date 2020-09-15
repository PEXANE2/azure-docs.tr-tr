---
title: SQL API sürüm notları ve kaynakları için Azure Cosmos DB Java SDK v4
description: SQL API 'SI ve SDK için Azure Cosmos DB Java SDK v4, sürüm tarihleri, kullanımdan kaldırma tarihleri ve Azure Cosmos DB SQL zaman uyumsuz Java SDK 'sının her sürümü arasında yapılan değişiklikler dahil olmak üzere.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/12/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: aabd52d47bfc59de7a1d79bbe5ffbdda90d099bf
ms.sourcegitcommit: 51df05f27adb8f3ce67ad11d75cb0ee0b016dc5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90060705"
---
# <a name="azure-cosmos-db-java-sdk-v4-for-core-sql-api-release-notes-and-resources"></a>Core (SQL) API 'SI için Java SDK v4 Azure Cosmos DB: sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET değişiklik akışı SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK’sı v4](sql-api-sdk-java-v4.md)
> * [Zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md)
> * [Zaman uyumlu Java SDK v2](sql-api-sdk-java.md)
> * [Spring Data v2](sql-api-sdk-java-spring-v2.md)
> * [Spring Data v3](sql-api-sdk-java-spring-v3.md)
> * [Spark Bağlayıcısı](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

Core (SQL) için Java SDK 'Sı v4 Azure Cosmos DB, zaman uyumsuz bir API ve eşitleme API 'sini tek bir Maven yapıtı halinde birleştirir. V4 SDK 'Sı, proje reaktör ve [Netty kitaplığı](https://netty.io/)temelinde gelişmiş performans, yeni API özellikleri ve zaman uyumsuz destek sunar. Kullanıcılar, Azure Cosmos DB Java SDK v4 ile [Azure Cosmos DB zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md) ve [Azure Cosmos DB Sync Java SDK v2](sql-api-sdk-java.md)ile daha iyi performans bekleyebilir.

> [!IMPORTANT]  
> Bu sürüm notları yalnızca Azure Cosmos DB Java SDK v4 içindir. Şu anda v4 'den daha eski bir sürüm kullanıyorsanız, v4 'ye yükseltme konusunda yardım için [Azure Cosmos DB Java SDK 'sı v4](migrate-java-v4-sdk.md) Kılavuzu ' na bakın.
>
> İşte hızlı bir şekilde çalışmaya yönelik üç adım!
> 1. SDK 'yı kullanabilmek için [desteklenen en düşük Java çalışma zamanı 'nı (JDK 8](/java/azure/jdk/?view=azure-java-stable) ) yükleyebilirsiniz.
> 2. Maven yapıtına erişmenizi sağlayan [Azure Cosmos DB Java SDK v4 Için hızlı başlangıç kılavuzu](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java) aracılığıyla çalışın ve temel Azure Cosmos DB isteklerine kılavuzluk eder.
> 3. Uygulamanızın SDK 'sını iyileştirmek için Azure Cosmos DB Java SDK v4 [Performans ipuçları](performance-tips-java-sdk-v4-sql.md) ve [sorun giderme](troubleshoot-java-sdk-v4-sql.md) kılavuzlarını okuyun.
>
> [Azure Cosmos DB atölyeler ve Labs](https://aka.ms/cosmosworkshop) , Java SDK 'sı v4 Azure Cosmos DB kullanmayı öğrenmek için başka harika bir kaynaktır!
>

## <a name="helpful-content"></a>Yardımcı içerik

| Content | Bağlantı |
|---|---|
|**SDK indirmesi**| [Maven](https://mvnrepository.com/artifact/com.azure/azure-cosmos) |
|**API belgeleri** | [Java API başvuru belgeleri](https://docs.microsoft.com/java/api/overview/azure/cosmosdb/client?view=azure-java-stable) |
|**SDK 'ya katkıda bulunma** | [GitHub 'da Java merkezi deposu için Azure SDK](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/cosmos/azure-cosmos) | 
|**Kullanmaya başlama** | [Hızlı başlangıç: Azure Cosmos DB SQL API verilerini yönetmek için bir Java uygulaması oluşturma](https://docs.microsoft.com/azure/cosmos-db/create-sql-api-java) <br> [Hızlı başlangıç kodu ile GitHub deposu](https://github.com/Azure-Samples/azure-cosmos-java-getting-started) | 
|**Temel kod örnekleri** | [Azure Cosmos DB: SQL API'si için Java örnekleri](sql-api-java-sdk-samples.md) <br> [Örnek kodla GitHub deposu](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-samples)|
|**Değişiklik akışı olan konsol uygulaması**| [Değişiklik akışı-Java SDK v4 örneği](create-sql-api-java-changefeed.md) <br> [Örnek kodla GitHub deposu](https://github.com/Azure-Samples/azure-cosmos-java-sql-app-example)| 
|**Web uygulaması örneği**| [Java SDK v4 ile Web uygulaması oluşturma](sql-api-java-application.md) <br> [Örnek kodla GitHub deposu](https://github.com/Azure-Samples/azure-cosmos-java-sql-api-todo-app)|
| **Performans ipuçları**| [Java SDK v4 için performans ipuçları](performance-tips-java-sdk-v4-sql.md)| 
| **Sorun giderme** | [Java SDK v4 sorunlarını giderme](troubleshoot-java-sdk-v4-sql.md) |
| **Daha eski bir SDK 'dan v4 'ye geçiş** | [Java V4 SDK’ya geçirme](migrate-java-v4-sdk.md) |
| **Desteklenen en düşük çalışma zamanı**|[JDK 8](/java/azure/jdk/?view=azure-java-stable) | 
| **Azure Cosmos DB atölyeler ve Labs** |[Cosmos DB atölyeler giriş sayfası](https://aka.ms/cosmosworkshop)

## <a name="release-history"></a>Yayın geçmişi

### <a name="450-beta1-unreleased"></a>4.5.0-Beta. 1 (serbest bırakılmamış)

### <a name="440-2020-09-12"></a>4.4.0 (2020-09-12)
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Bağımlılık etkinleştirilirken düzeltilen RequestTimeoutException `netty-tcnative-boringssl` .
* Moddaki işlemler üzerinde sabit bellek sızıntısı sorunu `Delete` `GATEWAY` .
* `CosmosClient`Endpoint Uri geçersiz olduğunda örnekleme sırasında bir sızıntı düzeltildi.
* Geliştirilmiş `CPU History` Tanılamalar.

### <a name="431-2020-08-13"></a>4.3.1 (2020-08-13)
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* `GROUP BY`Sorgunun yalnızca bir sayfa döndüğü bir sorun düzeltildi.
* Merkezi SDK kılavuzlarıyla uyum sağlamak için sabit Kullanıcı Aracısı dize biçimi.
* Sorgu planı tanılamayı dahil etmek için geliştirilmiş tanılama bilgileri.

### <a name="430-2020-07-29"></a>4.3.0 (2020-07-29)
#### <a name="new-features"></a>Yeni özellikler
* Reaktör-çekirdek kitaplık sürümü sürümüne güncelleştirildi `3.3.8.RELEASE` . 
* Reaktör-Netty kitaplık sürümü sürümüne güncelleştirildi `0.9.10.RELEASE` . 
* Netty kitaplık sürümü olarak güncelleştirildi `4.1.51.Final` . 
* İle için yeni aşırı yükleme API 'Leri eklendi `upsertItem` `partitionKey` . 
* Açık telemetri izleme desteği eklendi. 
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Ağ GEÇIDI modundaki isteklerin iptali durumunda SSLException 'nın oluşturulduğu sorun düzeltildi.
* Saklı yordamlar yürütülürken sabit kaynak kısıtlama yeniden deneme ilkesi.
* SDK 'nın günlük düzeyi hata ayıklama modunda asılı olduğu sorun düzeltildi. 
* Doğrudan modda gecikme süresi içinde düzeltilen düzenli artışlar. 
* Yüksek istemci başlatma zamanı sorunu düzeltildi. 
* İstemci doğrudan mod ve ağ geçidi moduyla özelleştirilirken düzeltilen http proxy hatası. 
* Kullanıcılarda düzeltilen olası NPE, null seçenekleri geçiyor. 
* Tanılama dizesinde timeUnit eklendi `requestLatency` .
* Tanılama dizesinden yinelenen URI dizesi kaldırıldı. 
* Nokta işlemleri için doğru JSON biçiminde sabit tanılama dizesi.
* Operatör ile ilgili sorun düzeltildi `.single()` özel durum durumunda reaktör zincirinin geliyor 'a neden olması. 

### <a name="420-2020-07-14"></a>4.2.0 (2020-07-14)
#### <a name="new-features"></a>Yeni özellikler
* Betik günlüğü etkin API öğesine eklendi `CosmosStoredProcedureRequestOptions` .
* `DirectConnectionConfig`Varsayılan değer `idleEndpointTimeout` 1 ' dir ve varsayılan `connectTimeout` olarak 0,5 s olarak güncelleştirilir.
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* `GatewayConnectionConfig` `idleConnectionTimeout` Geçersiz kılan sorun düzeltildi `DirectConnectionConfig` `idleConnectionTimeout` .
* `responseContinuationTokenLimitInKb`' De bir sabit get ve set API 'leri `CosmosQueryRequestOptions` .
* Koleksiyonda sorun düzeltildi ve aynı ada sahip koleksiyonu yeniden oluştururken akış değiştir.
* En üstteki sorgu oluşturma ClassCastException ile ilgili sorun düzeltildi.
* Sorgu ' NullPointerException 'ı oluşturan sıralamada sorun düzeltildi.
* İptal edilen isteklerin doğrudan modunda işlenmesinde, yeniden aktörin çağrılmasına neden olan sorun düzeltildi `onErrorDropped` . 

### <a name="410-2020-06-25"></a>4.1.0 (2020-06-25)
#### <a name="new-features"></a>Yeni özellikler
* Sorgu için destek eklendi `GROUP BY` .
* DirectConnectionConfig içinde maxConnectionsPerEndpoint varsayılan değeri 130 olarak artar.
* DirectConnectionConfig içinde varsayılan maxRequestsPerConnection değeri 30 ' a yükseltilir.
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Devamlılık belirteci kullanarak devam eden sorgu yinelenen sonuçları döndürmeyle sıralama ile ilgili sorunlar düzeltildi. 
* Değer sorgusu ile ilgili sorunlar, iç içe geçmiş nesnenin null değerlerini döndürüyor.
* RntbdClientChannelPool içindeki istek yöneticisinde boş işaretçi özel durumu düzeltildi.

### <a name="401-2020-06-10"></a>4.0.1 (2020-06-10)
#### <a name="new-features"></a>Yeni özellikler
* Olarak yeniden adlandırıldı `QueryRequestOptions` `CosmosQueryRequestOptions` .
* `ChangeFeedProcessorBuilder`Oluşturucu düzenine güncelleştirildi.
* `CosmosPermissionProperties`Yeni kapsayıcı adı ve alt kaynaklar API 'leri ile güncelleştirildi.
* Daha fazla örnek eklendi & belgeleri uygulamasına zenginleştirir `CosmosClientBuilder` . 
* `CosmosDatabase`  &  `CosmosContainer` Otomatik ölçeklendirme/Autopilot desteği için throughputproperties ile API 'ler güncelleştirildi. 
* Olarak yeniden adlandırıldı `CosmosClientException` `CosmosException` . 
* `AccessCondition`  &  `AccessConditionType` `ifMatchETag()`  &  `ifNoneMatchETag()` API 'ler tarafından değiştirilmiştir. 
* Tüm `Cosmos*AsyncResponse`  &  `CosmosResponse` türleri tek bir türe birleştirildi `CosmosResponse` .
* Olarak yeniden adlandırıldı `CosmosResponseDiagnostics` `CosmosDiagnostics` .  
* Sarmalanmış `FeedResponseDiagnostics` `CosmosDiagnostics` . 
* `jackson`Azure 'da bağlı Azure-Cosmos & bağımlılığı kaldırıldı. 
* `CosmosKeyCredential`Tür ile değiştirilmiştir `AzureKeyCredential` . 
* `ProxyOptions`API 'ler eklendi `GatewayConnectionConfig` . 
* SDK `Instant` , yerine türünü kullanacak şekilde güncelleştirildi `OffsetDateTime` . 
* Yeni enum türü eklendi `OperationKind` . 
* Olarak yeniden adlandırıldı `FeedOptions` `QueryRequestOptions` . 
* `getETag()`  &  `getTimestamp()` Türlere API 'ler eklendi `Cosmos*Properties` . 
* `userAgent`İçindeki bilgiler eklendi `CosmosException`  &  `CosmosDiagnostics` . 
* Yeni satır karakteri, `Diagnostics` sistem yeni satır karakteriyle güncelleştirildi. 
* `readAll*`API 'ler kaldırıldı, sorguyu kullanın, bunun yerine tüm API 'leri seçin.
* `ChangeFeedProcessor`Tahmin gecikme API 'si eklendi.   
* SDK 'ya otomatik ölçeklendirme/Autopilot üretilen iş sağlama desteği eklendi.  
* `ConnectionPolicy`Yeni bağlantı yapılandırmaları ile değiştirilmiştir. `DirectConnectionConfig`  &  `GatewayConnectionConfig` `CosmosClientBuilder` Doğrudan & ağ geçidi modu bağlantı yapılandırmalarına yönelik API 'ler kullanıma sunuldu.
* `JsonSerializable`  &  `Resource` Uygulama paketine taşındı. 
* `contentResponseOnWriteEnabled`Yazma işlemlerinde tam yanıt içeriğini devre dışı bırakan CosmosClientBuilder 'a API eklendi.
* `getETag()`Yanıt türlerinde sunulan API 'ler.
* `CosmosAuthorizationTokenResolver`Uygulamaya taşındı. 
* `preferredLocations`  &  `multipleWriteLocations` API olarak yeniden adlandırıldı `preferredRegions`  &  `multipleWriteRegions` . 
* `reactor-core`3.3.5. Release, `reactor-netty` 0.9.7. Release & to `netty` 4.1.49. final sürümlerine güncelleştirildi. 
* SDK 'da için destek eklendi `analyticalStoreTimeToLive` .     
* `CosmosClientException` genişletiliyor `AzureException` . 
* API 'leri, `maxItemCount`  &  `requestContinuationToken` `FeedOptions` yerine API `byPage()` 'ler kullanılarak kaldırılır `CosmosPagedFlux`  &  `CosmosPagedIterable` .
* `CosmosPermissionProperties`API 'ler için genel yüzeyde tanıtılmıştır `Permission` .
* Kaldırılmış `SqlParameterList` tür & ile değiştirilmiş `List`
* Doğrudan TCP istemcisinde birden çok bellek sızıntısı düzeltildi. 
* Sorgular için destek eklendi `DISTINCT` . 
* Dış bağımlılıklar üzerinde kaldırıldı `fasterxml.uuid, guava, commons-io, commons-collection4, commons-text` .  
* `CosmosPagedFlux`  &  `CosmosPagedIterable` Pakete taşındı `utils` . 
* Netty 4.1.45. final & projesi yeniden aktör 3.3.3 sürümüne güncelleştirildi.
* Genel Rest sözleşmeleri sınıflara güncelleştirildi `Final` .
* Nokta işlemlerine yönelik gelişmiş tanılama desteği eklendi.
* Paketi şu şekilde güncelleştirildi `com.azure.cosmos`
* `models`Model/Rest sözleşmeleri için paket eklendi
* `utils`Türler için paket eklendi `CosmosPagedFlux`  &  `CosmosPagedIterable` . 
* SDK genelinde kullanılacak ortak API 'Ler güncelleştirildi `Duration` .
* Tüm Rest sözleşmeleri pakete eklendi `models` .
* `RetryOptions` olarak yeniden adlandırıldı `ThrottlingRetryOptions` .
* `CosmosPagedFlux`  &  `CosmosPagedIterable` Sorgu API 'leri için sayfalandırma türleri eklendi. 
* İçindeki yeni bir API kullanarak Cosmoclient 'ın birden çok örneği arasında Cosmosclistaların paylaşılmasına yönelik destek eklendi `CosmosClientBuilder#connectionSharingAcrossClientsEnabled(true)`
* Çift serileştirme/seri kaldırma kaldırarak sorgu Iyileştirmeleri. 
* Gereksiz kopyalama geri ve ileri kaldırılarak yanıt üstbilgileri iyileştirmeleri. 
* `ByteBuffer`Ara dize örneklemeleri kaldırılarak iyileştirilmiş serileştirme/seri hale getirme.

#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Sabit ConnectionPolicy `toString()` null Işaretçisi özel durumu.
* Sorguya göre değer sırası durumunda sorgu sonuçlarının ayrıştırılmasındaki sorun düzeltildi. 
* Doğrudan TCP istemcisiyle sabit yuva sızıntısı sorunları.
* `orderByQuery`Devamlılık belirteci hatası ile düzeltildi.
* `ChangeFeedProcessor` Bölüm bulunamadığında bölüm bölmelerini işlemek için hata düzeltilme &.
* `ChangeFeedProcessor` farklı iş parçacıkları genelinde kira güncelleştirmelerini eşitlerken hata düzeltildi.
* `ArrayIndexOutOfBound`StoreReader içinde özel duruma neden olan sabit yarış durumu

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Sonraki adımlar
Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.