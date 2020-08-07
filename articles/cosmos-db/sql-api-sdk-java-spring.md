---
title: SQL API sürüm notları ve kaynakları için Spring Data Azure Cosmos DB
description: Sürüm tarihleri, kullanımdan kaldırma tarihleri ve SQL zaman uyumsuz Java SDK 'sının her Azure Cosmos DB bir sürümü arasında yapılan değişiklikler dahil olmak üzere SQL API için Spring Data Azure Cosmos DB hakkında bilgi edinin.
author: anfeldma-ms
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 08/05/2020
ms.author: anfeldma
ms.custom: devx-track-java
ms.openlocfilehash: f94ea37966c4942f72ae2d7b701e742891fa6cd2
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87854310"
---
# <a name="spring-data-azure-cosmos-db-for-core-sql-api-release-notes-and-resources"></a>Spring Data Azure Cosmos DB for Core (SQL) API: sürüm notları ve kaynakları
> [!div class="op_single_selector"]
> * [.NET SDK v3](sql-api-sdk-dotnet-standard.md)
> * [.NET SDK v2](sql-api-sdk-dotnet.md)
> * [.NET Core SDK v2](sql-api-sdk-dotnet-core.md)
> * [.NET değişiklik akışı SDK v2](sql-api-sdk-dotnet-changefeed.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Java SDK’sı v4](sql-api-sdk-java-v4.md)
> * [Zaman uyumsuz Java SDK v2](sql-api-sdk-async-java.md)
> * [Zaman uyumlu Java SDK v2](sql-api-sdk-java.md)
> * [Spring Data](sql-api-sdk-java-spring.md)
> * [Spark Bağlayıcısı](sql-api-sdk-java-spark.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Toplu yürütücü-.NET v2](sql-api-sdk-bulk-executor-dot-net.md)
> * [Toplu yürütücü-Java](sql-api-sdk-bulk-executor-java.md)

Çekirdek (SQL) için Spring Data Azure Cosmos DB, geliştiricilerin Spring uygulamalarında Azure Cosmos DB kullanmasına izin verir. Spring Data Azure Cosmos DB veritabanlarını ve koleksiyonları işlemek, belgelerle çalışmak ve sorgu vermek için Spring Data arabirimini kullanıma sunar. Aynı Maven yapıtı içinde hem eşitleme hem de Async (reaktif) API 'Leri desteklenir. 

[Spring Framework](https://spring.io/projects/spring-framework) , Java uygulaması geliştirmeyi kolaylaştırın bir programlama ve yapılandırma modelidir. Kuruluşun Web sitesini teklife göre yay, bağımlılık ekleme kullanarak uygulamaların "sıhhi tesisat" düzeyini kolaylaştırır. Uygulama oluşturma ve test etme işlemleri daha basit hale geldiği için Spring gibi birçok geliştirici. [Spring Boot](https://spring.io/projects/spring-boot) , Web uygulaması ve mikro hizmet geliştirme ile ilgili bir gözle sıhhi tesisat 'yi işlemeye yönelik bu fikir düzeyini genişletiyor. [Spring Data](https://spring.io/projects/spring-data) , Spring veya Spring Boot uygulaması bağlamından Azure Cosmos DB gibi veri depolarına erişmek için bir programlama modelidir. 

Spring Data Azure Cosmos DB, [Azure Spring Cloud](https://azure.microsoft.com/services/spring-cloud/) uygulamalarınızda kullanabilirsiniz.

> [!IMPORTANT]  
> Bu sürüm notları yalnızca SQL API Azure Cosmos DB içindir. 
>
> Aşağıdaki kılavuzlar diğer Azure Cosmos DB API 'Lerinde Spring verilerini destekler:
> * [Azure Cosmos DB ile Apache Cassandra için yay verileri](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-apache-cassandra-with-cosmos-db)
> * [Azure Cosmos DB ile Spring Data MongoDB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-mongodb-with-cosmos-db)
> * [Yay verileri Gremlin Azure Cosmos DB](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-data-gremlin-java-app-with-cosmos-db)
>
> Hızlı bir şekilde geçmek ister misiniz?
> 1. SDK 'yı kullanabilmek için [desteklenen en düşük Java çalışma zamanı 'nı (JDK 8](/java/azure/jdk/?view=azure-java-stable) ) yükleyebilirsiniz.
> 2. Starter 'ı kullanarak bir yay verileri Azure Cosmos DB uygulaması [oluşturun!](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db)
> 3. Temel Azure Cosmos DB isteklerinde izlenecek [yay verileri Azure Cosmos DB Geliştirici Kılavuzu](https://docs.microsoft.com/azure/developer/java/spring-framework/how-to-guides-spring-data-cosmosdb) aracılığıyla çalışın.
>
> Bahar ön yükleme başlangıç uygulamalarını [Spring ınitialku](https://start.spring.io/)ile hızlı bir şekilde çalıştırabilirsiniz!
>

## <a name="helpful-content"></a>Yardımcı içerik

| İçerik | Bağlantı |
|---|---|
| **SDK indirmesi** | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/spring-data-cosmosdb) |
|**API belgeleri** | [Yay verileri Azure Cosmos DB başvuru belgeleri]() |
|**SDK 'ya katkıda bulunma** | [Spring Data Azure Cosmos DB GitHub 'da depo](https://github.com/microsoft/spring-data-cosmosdb) | 
|**Spring Boot Starter**| [Java için Azure Cosmos DB Spring Boot Starter istemci kitaplığı](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmosdb) |
|**Spring TODO uygulama örneği Azure Cosmos DB**| [App Service Linux 'ta uçtan uca Java deneyimi (Bölüm 2)](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2) |
|**Geliştirici Kılavuzu** | [Yay verileri Azure Cosmos DB Geliştirici Kılavuzu](https://docs.microsoft.com/azure/developer/java/spring-framework/how-to-guides-spring-data-cosmosdb) | 
|**Starter kullanma kılavuzu** | [Azure Cosmos DB SQL API’si ile Spring Boot Starter’ı kullanma](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db) <br> [Azure Spring Boot Starter Cosmos DB için GitHub deposu](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/spring/azure-spring-boot-starter-cosmosdb) |
|**Uygulama hizmetleriyle örnek** | [Linux üzerinde App Service ile Spring ve Cosmos DB’yi kullanma](https://docs.microsoft.com/azure/developer/java/spring-framework/configure-spring-app-with-cosmos-db-on-app-service-linux) <br> [TODO uygulaması örneği](https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git) |

## <a name="release-history"></a>Yayın geçmişi

### <a name="230-2020-05-21"></a>2.3.0 (2020-05-21)
#### <a name="new-features"></a>Yeni özellikler
* Spring Boot sürümünü 2.3.0 olarak güncelleştir 
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="225-2020-05-19"></a>2.2.5 (2020-05-19)
#### <a name="new-features"></a>Yeni özellikler
* Azure Cosmos DB sürümü v 3.7.3 'ye güncelleştirildi
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Cosmos SDK v 3.7.3 'ten bellek sızıntısı düzeltmeleri ve Netty sürüm yükseltmeleri içerir 

### <a name="224-2020-04-06"></a>2.2.4 (2020-04-06)
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* CosmosDbConfig 'den dikkate almanız gereken sabit Allowtelemetri bayrağı
* Kapsayıcıda sabit TTL özelliği

### <a name="223-2020-02-25"></a>2.2.3 (2020-02-25)
#### <a name="new-features"></a>Yeni özellikler
* Bölüm anahtarı API 'sine göre yeni findAll eklendi
* Azure-Cosmos sürümü 3.7.0 olarak güncelleştirildi
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Fixed collectionName-> containerName
* & domainClass-> domainType sabit entityClass
* "Giriş varlığı yerine depo tarafından kaydedilen geri dönüş varlık koleksiyonu"

### <a name="2110-2020-02-25"></a>2.1.10 (2020-02-25)
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* "Giriş varlığı yerine depo tarafından kaydedilen dönüş varlık koleksiyonu" için biriktirme listesi çözümü

### <a name="222-2020-01-15"></a>2.2.2 (2020-01-15)
#### <a name="new-features"></a>Yeni özellikler
* Azure-Cosmos sürümü v 3.6.0 'ye güncelleştirildi
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="221-2019-12-31"></a>2.2.1 (2019-12-31)
#### <a name="new-features"></a>Yeni özellikler
* Cosmos DB SDK sürümü 3.5.0 olarak güncelleştirildi
* Otomatik oluşturma toplamayı etkinleştirmek/devre dışı bırakmak için ek açıklama alanı eklendi
* CosmosDBAccessException aracılığıyla daha iyi özel durum işleme, sunulan CosmosClientException
* ResponseDiagnostics aracılığıyla sunulan Requestücret ve ActivityId
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* SDK 3.5.0 güncelleştirme düzeltmeleri "Cosmos DB HTTP yanıt üst bilgisi 8192 bayttan büyük olduğunda özel durum", "başsıfıellik Ilkesi. Defaultsıfıen Level (), sınırlanmış eskime ve tutarlı ön ek üzerinde başarısız olur"
* Sabit Findbyıd API 'Si davranışı, özel durum oluşturmak yerine boş döndürme bulunamadı
* CosmosPageRequest kullanılırken sıralama sonraki sayfada uygulanamayan bir hata düzeltildi

### <a name="219-2019-12-26"></a>2.1.9 (2019-12-26)
#### <a name="new-features"></a>Yeni özellikler
* Otomatik oluşturma toplamayı etkinleştirmek/devre dışı bırakmak için ek açıklama alanı eklendi
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Sabit Findbyıd API 'Si davranışı, özel durum oluşturmak yerine boş döndürme bulunamadı

### <a name="220-2019-10-21"></a>2.2.0 (2019-10-21)
#### <a name="new-features"></a>Yeni özellikler
* Reaktif Cosmos depo desteğini tamamlamayı
* Cosmos DB tanılama dizesi ve sorgu ölçümleri desteğini ıste
* 3.3.1 SDK sürüm güncelleştirmesi Cosmos DB
* Spring Framework sürümü 5.2.0. RELEASE 'e yükseltilir
* Spring Data Commons sürümü 2.2.0. RELEASE 'e yükseltilir
* Findbyıdandpartitionkey, Deletebyıdandpartitionkey API 'Leri eklendi
* Azure-doumentdb bağımlılığını kaldırma
* Cosmos 'ye yeniden markalı DocumentDb
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Fixed "sıralama, pageSize, depodaki toplam öğeden az olduğunda özel durum oluşturur"

### <a name="218-2019-10-18"></a>2.1.8 (2019-10-18)
#### <a name="new-features"></a>Yeni özellikler
* Belge DB API 'Lerini kullanımdan kaldırma
* Findbyıdandpartitionkey, Deletebyıdandpartitionkey API 'Leri eklendi.
* _Etag temelinde Iyimser kilitleme eklendi
* Belge koleksiyonu adı için etkin SPeL ifadesi
* ObjectMapper geliştirmeleri.
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="217-2019-10-18"></a>2.1.7 (2019-10-18)
#### <a name="new-features"></a>Yeni özellikler
* Cosmos SDK V3 bağımlılığı eklendi
* Reaktif Cosmos deposu eklendi
* DocumentDbTemplate 'in, Cosmos SDK V3 'yi kullanması için uygulanması güncelleştirildi.
* Reaktif Cosmos depo desteği için diğer yapılandırma değişiklikleri.
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="212-2019-03-19"></a>2.1.2 'yi (2019-03-19)
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* İçin ApplicationInsights bağımlılığını kaldır
    * Bağımlılık kirletinin potansiyel riski
    * Java 11 uyumsuzluğu
    * CPU ve/veya bellek için olası performans etkisini önleme.

### <a name="207-2019-03-20"></a>2.0.7 (2019-03-20)
#### <a name="new-features"></a>Yeni özellikler
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Backport, için ApplicationInsights bağımlılığını kaldırır
    * Bağımlılık kirletinin potansiyel riski
    * Java 11 uyumsuzluğu
    * CPU ve/veya bellek için olası performans etkisini önleme.

### <a name="211-2019-03-07"></a>2.1.1 (2019-03-07)
#### <a name="new-features"></a>Yeni özellikler
* Ana sürümü 2.1.1 olarak güncelleştir
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="206-2019-03-07"></a>2.0.6 (2019-03-07)
#### <a name="new-features"></a>Yeni özellikler
* Telemetrinin tüm özel durumlarını yoksay
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="210-2018-12-17"></a>2.1.0 (2018-12-17)
#### <a name="new-features"></a>Yeni özellikler
* Sorunu gidermek için 2.1.0 sürümünü güncelleştirin
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri

### <a name="205-2018-09-13"></a>2.0.5 (2018-09-13)
#### <a name="new-features"></a>Yeni özellikler
* Anahtar sözcük ekleme var, startsWith
* Benioku dosyasını güncelleştir
#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* "Doğrudan varlık için kendi kendine kendi href çağırılamaz" düzeltmesini düzeltir
* "Koleksiyon oluşturulmadıysa findAll başarısız olur" düzeltmesini düzeltir

### <a name="204-pre-release-2018-08-23"></a>2.0.4 (yayın öncesi) (2018-08-23)
#### <a name="new-features"></a>Yeni özellikler
* DocumentDB 'den cosmosdb 'ye paketi yeniden adlandırma,
* Sorgu yöntemi anahtar sözcüğünün yeni özelliği, desteklenen SQL API 'den 16 anahtar sözcük ekleyin.
* Sayfalama ve sıralama özelliklerine sahip yeni bir sorgu özelliği ekleyin.
* Spring-Data-cosmosdb yapılandırmasını kolaylaştırın.
* DeleteCollection ve deleteAll API 'SI ekleyin.

#### <a name="key-bug-fixes"></a>Anahtar hata düzeltmeleri
* Hata düzeltilme ve hata geliştirme.

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="next-steps"></a>Sonraki adımlar
Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası.

Spring Framework hakkında daha fazla bilgi için bkz. [proje giriş sayfası](https://spring.io/projects/spring-framework).

Spring Boot hakkında daha fazla bilgi için bkz. [proje giriş sayfası](https://spring.io/projects/spring-boot).

Yay verileri hakkında daha fazla bilgi için bkz. [proje giriş sayfası](https://spring.io/projects/spring-data).