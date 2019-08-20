---
title: "Azure Cosmos DB: SQL zaman uyumsuz Java API 'SI, SDK & kaynakları"
description: Tüm SQL Async Java API'si ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB SQL Async Java SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: moderakh
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: java
ms.topic: reference
ms.date: 07/01/2019
ms.author: moderakh
ms.openlocfilehash: 3bee473adde9c4762cf400dff6190eb76a4cc8d7
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69624700"
---
# <a name="azure-cosmos-db-async-java-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için zaman uyumsuz Java SDK Azure Cosmos DB: Sürüm notları ve kaynakları
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

SQL API Async Java SDK'sı desteği ile zaman uyumsuz işlemleri sağlayarak SQL API Java SDK'sından farklıdır [Netty Kitaplığı](https://netty.io/). Önceden varolan [SQL API Java SDK'sı](sql-api-sdk-java.md) zaman uyumsuz işlemleri desteklemez. 

| |  |
|---|---|
| **SDK'sını indirme** | [Maven](https://mvnrepository.com/artifact/com.microsoft.azure/azure-cosmosdb) |
|**API belgeleri** |[Java API başvuru belgeleri](https://docs.microsoft.com/java/api/com.microsoft.azure.cosmosdb.rx.asyncdocumentclient?view=azure-java-stable) | 
|**SDK'sı için katkıda bulunan** | [GitHub](https://github.com/Azure/azure-cosmosdb-java) | 
|**Kullanmaya başlama** | [Async Java SDK'sı ile çalışmaya başlama](https://github.com/Azure-Samples/azure-cosmos-db-sql-api-async-java-getting-started) | 
|**Kod örneği** | [GitHub](https://github.com/Azure/azure-cosmosdb-java#usage-code-sample)| 
| **Performans ipuçları**| [GitHub Benioku dosyası](https://github.com/Azure/azure-cosmosdb-java#guide-for-prod)| 
| **En düşük desteklenen çalışma zamanı**|[JDK 8](https://aka.ms/azure-jdks) | 

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name250250"></a><a name="2.5.0"/>2.5.0
* TCP modu artık varsayılan olarak açık
* Çapraz bölümdeki sorgu ölçümleri şimdi tüm bölümleri döndürüyor
* Genel Strong artık düzgün şekilde çalışmaktadır
* Sorgular için yük devretme çoklu yönetici için düzgün şekilde yeniden denenmez
* Güvenlik düzeltmeleri için bağımlılık çarpma

### <a name="a-name245245"></a><a name="2.4.5"/>2.4.5
* Karma v2 desteği için bugdüzeltmesini

### <a name="a-name243243"></a><a name="2.4.3"/>2.4.3
* İstemci # Close () ([github #88](https://github.com/Azure/azure-cosmosdb-java/issues/88)) üzerinde kaynak sızıntısı için bugdüzeltmesini.

### <a name="a-name242242"></a><a name="2.4.2"/>2.4.2 sections
* Çapraz bölüm sorguları için devamlılık belirteci desteği eklendi.

### <a name="a-name241241"></a><a name="2.4.1"/>2.4.1
* Doğrudan modda bazı hatalar düzeltildi.
* Doğrudan modda günlük geliştirildi.
* İyileştirilmiş bağlantı yönetimi.

### <a name="a-name240240"></a><a name="2.4.0"/>2.4.0
* Doğrudan mod bağlantısı artık genel kullanıma sunulmuştur (GA). Doğrudan mod bağlantısı kullanan bir örnek için bkz. [Azure-cosmosdb-Java](https://github.com/Azure/azure-cosmosdb-java) GitHub deposu.
* Queryölçümler için destek eklendi.
* Bunun yerine Java. util. List ' i kabul etmek için önemli olan Java. util. Collection 'ı kabul eden API 'Ler değiştirilmiştir. Şimdi ConnectionPolicy # getPreferredLocations (), JsonSerialization ve PartitionKey (.) Accept List.

### <a name="a-name240-beta-1240-beta-1"></a><a name="2.4.0-beta-1"/>2.4.0-Beta-1
* Doğrudan mod bağlantısı desteği eklendi.
* Bunun yerine Java. util. List ' i kabul etmek için önemli olan Java. util. Collection 'ı kabul eden API 'Ler değiştirilmiştir.
  Şimdi ConnectionPolicy # getPreferredLocations (), JsonSerialization ve PartitionKey (.) Accept List.
* Ağ Geçidi modundaki belge sorgusu için bir oturum hatası düzeltildi.
* Yükseltilen bağımlılıklar (Netty 0.4.20 [github #79](https://github.com/Azure/azure-cosmosdb-java/issues/79), rxjava 1.3.8).

### <a name="a-name231231"></a><a name="2.3.1"/>2.3.1
* Çok büyük sorgu yanıtlarını işleme düzeltmeleri.
* İstemci örneği oluşturulurken kaynak belirteci işlemesini düzeltir ([github #78](https://github.com/Azure/azure-cosmosdb-java/issues/78)).
* Yükseltilen güvenlik açığı olan fekson-DataBind ([github #77](https://github.com/Azure/azure-cosmosdb-java/pull/77)).

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0
* Bir kaynak sızıntısı hata düzeltildi.
* MultiPolygon desteği eklendi
* Özel üst bilgilerinde RequestOptions desteği eklendi.

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Paketleme hata düzeltildi.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Yazma yeniden deneme yolunda NPE düzeltildi.
* Uç nokta yönetiminde NPE düzeltildi.
* Güvenlik açığı olan bağımlılıklar ([GitHub #68](https://github.com/Azure/azure-cosmosdb-java/issues/68)).
* Sorun giderme için günlük Netty ağ desteği eklendi.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Çok bölgeli yazma desteği eklendi.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Proxy için destek eklendi.
* Kaynak belirteci yetkilendirme için destek eklendi.
* Büyük bölüm anahtarlarını işlemede hata düzeltildi ([GitHub #63](https://github.com/Azure/azure-cosmosdb-java/issues/63)).
* Belgeleri geliştirdik.
* SDK daha ayrıntılı modüllerine yeniden oluşturulamaz.

### <a name="a-name201201"></a><a name="2.0.1"/>2.0.1
* İngilizce olmayan yerel ayarlarda hata düzeltildi ([GitHub #51](https://github.com/Azure/azure-cosmosdb-java/issues/51)).
* Çakışma kaynağında ek yardımcı yöntemler.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0
* Performans nedenleri ve lisanslama ([GitHub #29](https://github.com/Azure/azure-cosmosdb-java/issues/29)) nedeniyle, org. JSON bağımlılığı Jackson tarafından değiştiriliyor.
* Kullanım dışı OfferV2 sınıfı kaldırıldı.
* İçerik aktarım hızı için teklif sınıfı eklendi erişimci yöntemi.
* Herhangi bir yöntem belge/kaynağında org.json türleri bir jackson nesne türü döndürmek için değiştirildi.
* jackson ObjectNode döndürülecek genişletme JsonSerializable değiştirilen sınıfları getObject(.) yöntemi yazın.
* getCollection(.) yöntemi, koleksiyon ObjectNode döndürülecek değiştirildi.
* Kaldırılan JsonSerializable kılabileceği oluşturucularla org.json.JSONObject bağımsız değişken.
* JsonSerializable.toJson (SerializationFormattingPolicy.Indented), artık iki boşluk girintileme için kullanır.
  
### <a name="a-name102102"></a><a name="1.0.2"/>1.0.2
* Benzersiz bir dizin ilke için destek eklendi.
* Akış seçenekleri yanıt devamlılık belirteci boyutu sınırlama için destek eklendi.
* Çapraz bölüm sorgusunun bölüm bölmedeki desteği eklendi.
* JSON zaman damgası serileştirme ([GitHub #32](https://github.com/Azure/azure-cosmosdb-java/issues/32)) içindeki bir hata düzeltildi.
* Json enum serileştirme düzeltildi.
* 2MB boyutunun ([GitHub #33](https://github.com/Azure/azure-cosmosdb-java/issues/33)) belgelerinin yönetiminde oluşan hata düzeltildi.
* Bağımlılık com. fasterxml. Jackson. Core: Jackson-DataBind bir hata nedeniyle 'nın 2.9.5 sürümüyle 'e yükseltildi ([Jackson-DataBind: GitHub #1599](https://github.com/FasterXML/jackson-databind/issues/1599))
* Rxjava-Extras bağımlılığı bir hata nedeniyle 0.8.0.17 'e yükseltildi ([rxjava-ekstralar: GitHub #30](https://github.com/davidmoten/rxjava-extras/issues/30)).
* Meta veri açıklamasını pom dosyasına satır içi belgeler geri kalanıyla olacak şekilde güncelleştirildi.
* Sözdizimi geliştirmesi ([github #41](https://github.com/Azure/azure-cosmosdb-java/issues/41)), ([GitHub #40](https://github.com/Azure/azure-cosmosdb-java/issues/40)).

### <a name="a-name101101"></a><a name="1.0.1"/>1.0.1
* Sorgu geri baskısı desteği eklendi.
* Sorgu bölüm anahtar aralığı kimliği desteği eklendi.
* İstek üstbilgisinde daha büyük devamlılık belirtecine izin vermek için düzeltilir (bugdüzeltmesini GitHub #24).
* Ana iş parçacığı sonlandırıldıktan sonra JVM sağlamak için 4.1.22.Final yükseltilmiş netty bağımlılık kapanır.
* Oturum belirteci ana kaynaklarını okurken geçirmekten kaçının düzeltin.
* Daha fazla örnek eklendi.
* Daha fazla Kıyaslama senaryoları eklenir.
* Uygun java belge oluşturma için sabit Java üst bilgi dosyaları.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK'sı ile engelleyici olmayan g/ç kullanarak uçtan uca Destek [Netty Kitaplığı](https://netty.io/) ağ geçidi modunda. 

## <a name="release-and-retirement-dates"></a>Yayın ve sona erme tarihleri
Sağlar; Microsoft bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş hafifletmek için bir SDK'yı devre dışı bırakmadan önce.

Yeni özellikleri ve işlevselliği ve iyileştirmeler yalnızca geçerli SDK'sı eklenir. Bu nedenle, her zaman en son SDK sürümüne mümkün olduğunca erken yükseltmeniz önerilir.

Cosmos DB devre dışı bırakılan bir SDK'sını kullanarak yapılan tüm istekleri hizmet tarafından reddedilir.

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [2.4.3](#2.4.3) |5 Mar, 2019|--- |
| [2.4.2 sections](#2.4.2) |1 Mar, 2019|--- |
| [2.4.1](#2.4.1) |20 Şubat 2019|--- |
| [2.4.0](#2.4.0) |8 Şubat 2019|--- |
| [2.4.0-Beta-1](#2.4.0-beta-1) |4 Şubat 2019|--- |
| [2.3.1](#2.3.1) |15 Ocak 2019|--- |
| [2.3.0](#2.3.0) |29 Kasım 2018|--- |
| [2.2.2](#2.2.2) |8 Kasım 2018|--- |
| [2.2.1](#2.2.1) |2 Kasım 2018|--- |
| [2.2.0](#2.2.0) |22 Eylül 2018'den|--- |
| [2.1.0](#2.1.0) |5 Eylül 2018'den|--- |
| [2.0.1](#2.0.1) |16 Ağustos 2018|--- |
| [2.0.0](#2.0.0) |20 Haziran 2018|--- |
| [1.0.2](#1.0.2) |18 Mayıs 2018|--- |
| [1.0.1](#1.0.1) |20 Nisan 2018|--- |
| [1.0.0](#1.0.0) |27 Şubat 2018|--- |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmeti sayfası.

