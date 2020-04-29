---
title: 'Azure Cosmos DB: SQL Python API, SDK & kaynakları'
description: Sürüm tarihleri, emeklilik tarihleri ve Azure Cosmos DB Python SDK 'nın her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL Python API 'SI ve SDK hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: reference
ms.date: 11/29/2018
ms.author: sngun
ms.openlocfilehash: b81a3921ec11d589dadbdebd698ab9ad67d7649c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80982914"
---
# <a name="azure-cosmos-db-python-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için Python SDK Azure Cosmos DB: sürüm notları ve kaynakları
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

| |  |
|---|---|
|**SDK 'Yı indir**|[PyPI](https://pypi.org/project/azure-cosmos)|
|**API belgeleri**|[Python API başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cosmos/?view=azure-python)|
|**SDK yükleme yönergeleri**|[Python SDK yükleme yönergeleri](https://github.com/Azure/azure-cosmos-python)|
|**SDK 'ya katkıda bulunma**|[GitHub](https://github.com/Azure/azure-cosmos-python)|
|**başlarken**|[Python SDK 'Yı kullanmaya başlama](sql-api-python-application.md)|
|**Desteklenen geçerli platform**|[Python 2,7](https://www.python.org/downloads/) ve [Python 3,5](https://www.python.org/downloads/)|

## <a name="release-notes"></a>Sürüm notları

### <a name="302"></a><a name="3.0.2"/>3.0.2
* MultiPolygon veri türü için destek eklendi
* Oturum okuma yeniden deneme ilkesinde hata düzeltmesini
* Base 64 dizelerinin kodu çözülürken yanlış doldurma sorunları için hata düzeltilme

### <a name="301"></a><a name="3.0.1"/>3.0.1
* LocationCache 'de hata düzeltilme
* Hata düzeltmesini uç noktası yeniden deneme mantığı
* Sabit belge

### <a name="300"></a><a name="3.0.0"/>3.0.0
* Çok bölgeli yazma desteği.
* Ad alanı, Azure. Cosmos olarak değiştirildi.
* Koleksiyon ve belge kavramları kapsayıcı ve öğe olarak yeniden adlandırılır, document_client cosmos_client olarak yeniden adlandırılır. 

### <a name="233"></a><a name="2.3.3"/>2.3.3
* Proxy desteği eklendi
* Değişiklik akışını okuma desteği eklendi
* Koleksiyon kota üstbilgileri için destek eklendi
* Büyük oturum belirteçleri sorunu için bugdüzeltmesini
* ReadMedia API için bugdüzeltmesini
* Bölüm anahtar aralığı önbelleğinde bugdüzeltmesini

### <a name="232"></a><a name="2.3.2"/>2.3.2
* Bağlantı sorunlarında varsayılan yeniden denemeler için destek eklendi.

### <a name="231"></a><a name="2.3.1"/>2.3.1
* Belgeler, Azure DocumentDB yerine Azure Cosmos DB başvuracak şekilde güncelleştirildi.

### <a name="230"></a><a name="2.3.0"/>2.3.0
* Bu SDK sürümü, [Azure Cosmos DB öykünücü](https://aka.ms/cosmosdb-emulator)'un en son sürümünü gerektirir.

### <a name="221"></a><a name="2.2.1"/>2.2.1
* Toplu sözlük için hata düzeltilme.
* Kaynak bağlantısında eğik çizgi kırpma için hata düzeltildi.
* Unicode kodlaması için testler eklendi.

### <a name="220"></a><a name="2.2.0"/>2.2.0
* Tutarlık ön eki olarak adlandırılan yeni bir tutarlılık düzeyi için destek eklendi.


### <a name="210"></a><a name="2.1.0"/>2.1.0
* Toplama sorguları (COUNT, MIN, MAX, SUM ve AVG) için destek eklendi.
* Cosmos DB öykünücüsüyle çalışırken TLS doğrulamasını devre dışı bırakma seçeneği eklendi.
* Bağımlı istekler modülünün kısıtlaması tam olarak 2.10.0 olarak kaldırıldı.
* 10.100 RU/sn 'den 2500 RU/sn 'ye kadar bölümlenmiş koleksiyonlar üzerinde en düşük aktarım hızını düşürdü.
* Saklı yordam yürütülürken betik günlüğü tutmayı etkinleştirme desteği eklendi.
* REST API sürümü bu sürümle ' 2017-01-19 ' öğesine göre tamponlanır.

### <a name="201"></a><a name="2.0.1"/>desteklemez
* Belge açıklamalarında düzenleme değişiklikleri yapıldı.

### <a name="200"></a><a name="2.0.0"/>2.0.0
* Python 3,5 için destek eklendi.
* İstekler modülü kullanılarak bağlantı havuzu oluşturma desteği eklendi.
* Oturum tutarlılığı için destek eklendi.
* Bölümlenmiş koleksiyonlar için TOP/ORDERBY sorguları için destek eklendi.

### <a name="190"></a><a name="1.9.0"/>1.9.0
* Kısıtlanmış istekler için yeniden deneme ilkesi desteği eklendi. (Kısıtlanan istekler çok büyük bir istek hızı alıyor, hata kodu 429.) Varsayılan olarak Azure Cosmos DB, 429 hata kodu ile karşılaşıldığında her bir istek için dokuz kez yeniden dener ve yanıt üst bilgisinde retryAfter saati belirlenir. Yeniden denemeler arasında sunucu tarafından döndürülen retryAfter süresini yoksaymak istiyorsanız, artık, ConnectionPolicy nesnesindeki RetryOptions özelliğinin bir parçası olarak sabit bir yeniden deneme aralığı zaman ayarlanabilir. Azure Cosmos DB, kısıtlanmış olan her istek için en fazla 30 saniye bekler (yeniden deneme sayısı ne olursa olsun) ve 429 hata koduyla yanıtı döndürür. Bu zaman, ConnectionPolicy nesnesindeki RetryOptions özelliğinde de geçersiz kılınabilir.
* Cosmos DB, kısıtlama yeniden deneme sayısını ve isteğin yeniden denemeler arasında bekleyeceği kümülatif süreyi belirtmek için her istekteki yanıt üst bilgileri olarak x-MS-azaltma-retry-Count ve x-MS-azaltma-retry-WAIT-Time-MS döndürür.
* RetryPolicy sınıfı ve document_client sınıfında kullanıma sunulan karşılık gelen Özellik (retry_policy) kaldırıldı ve bunun yerine, varsayılan yeniden deneme seçeneklerinden bazılarını geçersiz kılmak için kullanılan ConnectionPolicy sınıfında RetryOptions özelliğini kullanıma sunan bir RetryOptions sınıfı getirdi.

### <a name="180"></a><a name="1.8.0"/>1.8.0
* Çok bölgeli veritabanı hesapları için destek eklendi.

### <a name="170"></a><a name="1.7.0"/>1.7.0
* Belgeler için yaşam süresi (TTL) özelliğine yönelik destek eklendi.

### <a name="161"></a><a name="1.6.1"/>1.6.1
* Bölüm anahtarı yolunda özel karakterlere izin vermek için sunucu tarafı bölümleme ile ilgili hata düzeltmeleri.

### <a name="160"></a><a name="1.6.0"/>1.6.0
* [Bölümlenmiş koleksiyonlar](partition-data.md) ve [Kullanıcı tanımlı performans düzeyleri](performance-levels.md)uygulandı. 

### <a name="150"></a><a name="1.5.0"/>1.5.0
* Birden çok bölüm genelinde uygulama oluşturmaya yardımcı olmak için karma & aralığı bölüm Çözümleyicileri ekleyin.

### <a name="142"></a><a name="1.4.2"/>1.4.2
* Upsert uygulama. Upsert özelliği desteklemek için yeni UpsertXXX yöntemleri eklenmiştir.
* KIMLIK tabanlı yönlendirmeyi uygulayın. Ortak API değişikliği yok, tüm değişiklikler iç.

### <a name="120"></a><a name="1.2.0"/>1.2.0
* Jeo-uzamsal dizini destekler.
* Tüm kaynaklar için ID özelliğini doğrular. Kaynak kimlikleri,?,/, #, \, karakter içeremez veya boşluk ile bitemez.
* Yeni "Dizin dönüştürme ilerlemesi" üst bilgisini Resourcereslik 'e ekler.

### <a name="110"></a><a name="1.1.0"/>1.1.0
* V2 dizin oluşturma ilkesini uygular.

### <a name="101"></a><a name="1.0.1"/>1.0.1
* Proxy bağlantısını destekler.

### <a name="100"></a><a name="1.0.0"/>1.0.0
* GA SDK.

## <a name="release--retirement-dates"></a>Yayın & kullanımdan kaldırma tarihleri
Microsoft, daha yeni/desteklenen bir sürüme geçişi düzgünleştirmek için SDK 'nın devre dışı bırakılmasının ardından en az **12 ay** önce bildirim sağlar.

Yeni özellikler ve işlevler ve iyileştirmeler yalnızca geçerli SDK 'ya eklenir, bu nedenle en son SDK sürümüne her zaman olabildiğince erken yükseltmeniz önerilir. 

Kullanımdan kaldırılan bir SDK 'Yı kullanarak Cosmos DB istek, hizmet tarafından reddedilir.

> [!WARNING]
> Sürüm **1.0.0** ÖNCESI SQL API için Python SDK 'sının tüm sürümleri **, 29 Şubat 2016**tarihinde kullanımdan kalkmıştı. 
> 
> 

> [!WARNING]
> SQL API için Python SDK 'sının tüm 1. x ve 2. x sürümleri **30 ağustos 2020**tarihinde kullanımdan kaldırılacaktır. 
> 
> 

<br/>

| Sürüm | Yayın Tarihi | Emeklilik tarihi |
| --- | --- | --- |
| [3.0.2](#3.0.2) |15 Kasım 2018 |--- |
| [3.0.1](#3.0.1) |Eki 04, 2018 |--- |
| [2.3.3](#2.3.3) |08 Eyl, 2018 |30 Ağustos 2020 |
| [2.3.2](#2.3.2) |08 Mayıs 2018 |30 Ağustos 2020 |
| [2.3.1](#2.3.1) |21 Aralık 2017 |30 Ağustos 2020 |
| [2.3.0](#2.3.0) |10 Kasım 2017 |30 Ağustos 2020 |
| [2.2.1](#2.2.1) |29 Eyl, 2017 |30 Ağustos 2020 |
| [2.2.0](#2.2.0) |10 Mayıs 2017 |30 Ağustos 2020 |
| [2.1.0](#2.1.0) |01 Mayıs 2017 |30 Ağustos 2020 |
| [desteklemez](#2.0.1) |30 Ekim 2016 |30 Ağustos 2020 |
| [2.0.0](#2.0.0) |29 Eylül 2016 |30 Ağustos 2020 |
| [1.9.0](#1.9.0) |07 Temmuz 2016 |30 Ağustos 2020 |
| [1.8.0](#1.8.0) |14 Haziran 2016 |30 Ağustos 2020 |
| [1.7.0](#1.7.0) |26 Nisan 2016 |30 Ağustos 2020 |
| [1.6.1](#1.6.1) |08 Nisan 2016 |30 Ağustos 2020 |
| [1.6.0](#1.6.0) |29 Mart 2016 |30 Ağustos 2020 |
| [1.5.0](#1.5.0) |03 Ocak 2016 |30 Ağustos 2020 |
| [1.4.2](#1.4.2) |06 Ekim 2015 |30 Ağustos 2020 |
| 1.4.1 |06 Ekim 2015 |30 Ağustos 2020 |
| [1.2.0](#1.2.0) |06 Ağustos 2015 |30 Ağustos 2020 |
| [1.1.0](#1.1.0) |09 Temmuz 2015 |30 Ağustos 2020 |
| [1.0.1](#1.0.1) |25 Mayıs 2015 |30 Ağustos 2020 |
| [1.0.0](#1.0.0) |07 Nisan 2015 |30 Ağustos 2020 |
| 0.9.4-ürününün |14 Ocak 2015 |29 Şubat 2016 |
| 0.9.3-ürününün |09 Aralık 2014 |29 Şubat 2016 |
| 0.9.2-ürününün |25 Kasım 2014 |29 Şubat 2016 |
| 0.9.1-ürününün |23 Eylül 2014 |29 Şubat 2016 |
| 0.9.0-ürününün |21 Ağustos 2014 |29 Şubat 2016 |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz. [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası. 

