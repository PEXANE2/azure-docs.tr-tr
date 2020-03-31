---
title: 'Azure Cosmos DB: SQL Python API, SDK & kaynakları'
description: Sürüm tarihleri, emeklilik tarihleri ve Azure Cosmos DB Python SDK'nın her sürümü arasında yapılan değişiklikler dahil olmak üzere SQL Python API ve SDK hakkında her şeyi öğrenin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: python
ms.topic: reference
ms.date: 11/29/2018
ms.author: sngun
ms.openlocfilehash: 6bc636b751d12bdb576e54f26536ac0045839229
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70137335"
---
# <a name="azure-cosmos-db-python-sdk-for-sql-api-release-notes-and-resources"></a>SQL API için Azure Cosmos DB Python SDK: Sürüm notları ve kaynakları
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

| |  |
|---|---|
|**SDK'yı İndir**|[PyPI](https://pypi.org/project/azure-cosmos)|
|**API belgeleri**|[Python API başvuru belgeleri](https://docs.microsoft.com/python/api/azure-cosmos/?view=azure-python)|
|**SDK kurulum talimatları**|[Python SDK yükleme talimatları](https://github.com/Azure/azure-cosmos-python)|
|**SDK'ya Katkıda Bulunun**|[GitHub](https://github.com/Azure/azure-cosmos-python)|
|**Kullanmaya başlayın**|[Python SDK ile başlayın](sql-api-python-application.md)|
|**Geçerli desteklenen platform**|[Python 2.7](https://www.python.org/downloads/) ve [Python 3.5](https://www.python.org/downloads/)|

## <a name="release-notes"></a>Sürüm notları

### <a name="302"></a><a name="3.0.2"/>3.0.2
* MultiPolygon veri türü için destek eklendi
* Oturumdaki hata düzeltmesi yeniden deneme ilkesini okuyun
* Temel 64 dizeleri çözerken yanlış doldurma sorunları için hata düzeltmesi

### <a name="301"></a><a name="3.0.1"/>3.0.1
* LocationCache'de hata düzeltmesi
* Hata düzeltme uç nokta yeniden deneme mantığı
* Sabit belgeler

### <a name="300"></a><a name="3.0.0"/>3.0.0
* Çok bölgeli için destek yazıyor.
* Ad alanı azure.cosmos olarak değiştirildi.
* Kapsayıcı ve öğe olarak yeniden adlandırılmış, document_client cosmos_client olarak yeniden adlandırılmış koleksiyon ve belge kavramları. 

### <a name="233"></a><a name="2.3.3"/>2.3.3
* Proxy için destek eklendi
* Okuma değişikliği akışı için destek eklendi
* Toplama kotası üstbilgisi için ek destek
* Büyük oturum belirteçleri sorunu için hata düzeltmesi
* ReadMedia API için Hata Düzeltmesi
* Bölüm anahtar aralığı önbelleğinde hata düzeltmesi

### <a name="232"></a><a name="2.3.2"/>2.3.2
* Bağlantı sorunları yla ilgili varsayılan yeniden denemeler için destek eklendi.

### <a name="231"></a><a name="2.3.1"/>2.3.1
* Azure DocumentDB yerine Azure Cosmos DB'ye başvurmak için güncelleştirilmiş belgeler.

### <a name="230"></a><a name="2.3.0"/>2.3.0
* Bu SDK sürümü, Azure Cosmos DB Emulator'un https://aka.ms/cosmosdb-emulatoren son sürümünü n için gerekli.

### <a name="221"></a><a name="2.2.1"/>2.2.1
* Toplu sözlük için hata düzeltmesi.
* Kaynak bağlantısındaki kesikleri kırpmak için hata düzeltmesi.
* Unicode kodlama için testler eklendi.

### <a name="220"></a><a name="2.2.0"/>2.2.0
* Consistent Prefix adlı yeni bir tutarlılık düzeyi için destek eklendi.


### <a name="210"></a><a name="2.1.0"/>2.1.0
* Toplama sorguları (COUNT, MIN, MAX, SUM ve AVG) için destek eklendi.
* Cosmos DB Emulator'a karşı çalışırken SSL doğrulamasını devre dışı bırakma seçeneği eklendi.
* Bağımlı istek modülünün tam olarak 2.10.0 olması kısıtlaması kaldırıldı.
* 10.100 RU/s'den 2500 RU/s'ye kadar bölümlenmiş koleksiyonlarda minimum iş belini düşürür.
* Depolanan yordam yürütme sırasında komut dosyası günlüğe kaydetmeyi etkinleştirmek için destek eklendi.
* REST API sürümü bu sürümle '2017-01-19'a yükseltildi.

### <a name="201"></a><a name="2.0.1"/>2.0.1
* Dokümantasyon yorumlarına editoryal değişiklikler yaptı.

### <a name="200"></a><a name="2.0.0"/>2.0.0
* Python 3.5 için destek eklendi.
* İstekmodülü kullanılarak bağlantı havuzu desteği eklendi.
* Oturum tutarlılığı için destek eklendi.
* Bölümlenmiş koleksiyonlar için TOP/ORDERBY sorguları için destek eklendi.

### <a name="190"></a><a name="1.9.0"/>1.9.0
* Daraltılmış istekler için yeniden deneme ilkesi desteği eklendi. (Daraltılmış istekler çok büyük bir özel durum, hata kodu 429 bir istek oranı alır.) Varsayılan olarak, Azure Cosmos DB hata kodu 429 ile karşılaşıldığında her istek için dokuz kez yeniden çalışır ve yanıt üstbilgisinde yeniden denemeyi onurlandırır. Yeniden deneme aralığı, yeniden denemeler arasında sunucu tarafından döndürülen yeniden denemesüresini göz ardı etmek istiyorsanız, ConnectionPolicy nesnesi üzerindeki RetryOptions özelliğinin bir parçası olarak ayarlanabilir. Azure Cosmos DB artık daraltılmış olan her istek için en fazla 30 saniye bekler (yeniden deneme sayısına bakılmaksızın) ve yanıtı hata kodu 429 ile döndürür. Bu süre ConnectionPolicy nesnesi üzerindeki RetryOptions özelliğinde de geçersiz kılınabilir.
* Cosmos DB şimdi x-ms-throttle-retry-count ve x-ms-throttle-retry-wait-time-ms'i, gaz yeniden deneme sayısını ve isteğin yeniden denemeler arasında beklediği kümülatif süreyi belirtmek için her istekteki yanıt başlıkları olarak döndürür.
* Document_client sınıfında açığa çıkarılan RetryPolicy sınıfı ve ilgili özellik (retry_policy) kaldırıldı ve bunun yerine ConnectionPolicy sınıfında bazı geçersiz kılmak için kullanılabilecek RetryOptions özelliğini ortaya çıkaran bir RetryOptions sınıfı tanıtıldı varsayılan yeniden deneme seçenekleri.

### <a name="180"></a><a name="1.8.0"/>1.8.0
* Çok bölgeli veritabanı hesapları için destek eklendi.

### <a name="170"></a><a name="1.7.0"/>1.7.0
* Belgeler için Time To Live(TTL) özelliği için destek eklendi.

### <a name="161"></a><a name="1.6.1"/>1.6.1
* Hata bölüm anahtar yolunda özel karakterler izin vermek için sunucu tarafı bölümleme ile ilgili düzeltmeler.

### <a name="160"></a><a name="1.6.0"/>1.6.0
* Uygulanan [bölümlenmiş koleksiyonlar](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri.](performance-levels.md) 

### <a name="150"></a><a name="1.5.0"/>1.5.0
* Birden çok bölüm deki parçaları niçin parçalama uygulamalarına yardımcı olmak için Karma & Aralığı bölüm çözümleyicileri ekleyin.

### <a name="142"></a><a name="1.4.2"/>1.4.2
* Upsert'i uygulayın. Upsert özelliğini desteklemek için yeni UpsertXXX yöntemleri eklendi.
* Kimlik Tabanlı Yönlendirme'yi uygulayın. Genel API değişikliği yok, tüm değişiklikler dahili.

### <a name="120"></a><a name="1.2.0"/>1.2.0
* GeoSpatial dizinini destekler.
* Tüm kaynaklar için kimlik özelliğini doğrular. Kaynaklar için ids ?, /, \, #, karakterler veya bir boşluk ile biten içeremez.
* ResourceResponse'a yeni üstbilgi "dizin dönüştürme ilerlemesi" ekler.

### <a name="110"></a><a name="1.1.0"/>1.1.0
* V2 dizin oluşturma ilkesini uygular.

### <a name="101"></a><a name="1.0.1"/>1.0.1
* Proxy bağlantısını destekler.

### <a name="100"></a><a name="1.0.0"/>1.0.0
* GA SDK.

## <a name="release--retirement-dates"></a>Emeklilik tarihleri & serbest bırakma
Microsoft, daha yeni/desteklenen bir sürüme geçişi kolaylaştırmak için SDK'nın emekliye geçmesinden en az **12 ay** önce bildirim sağlar.

Yeni özellikler, işlevsellik ve optimizasyonlar yalnızca geçerli SDK'ya eklenir, bu nedenle her zaman en son SDK sürümüne mümkün olduğunca erken yükseltmeniz tavsiye edilir. 

Cosmos DB'ye emekli bir SDK kullanarak yapılan tüm istekler hizmet tarafından reddedilir.

> [!WARNING]
> Sürüm **1.0.0'dan** önce SQL API için Python SDK'nın tüm sürümleri **29 Şubat 2016'da**kullanımdan kaldırılmıştır. 
> 
> 

> [!WARNING]
> SQL API için Python SDK'nın 1.x ve 2.x tüm sürümleri **30 Ağustos 2020'de**kullanımdan kaldırılacaktır. 
> 
> 

<br/>

| Sürüm | Yayın Tarihi | Emeklilik Tarihi |
| --- | --- | --- |
| [3.0.2](#3.0.2) |Kasım 15, 2018 |--- |
| [3.0.1](#3.0.1) |Eki 04, 2018 |--- |
| [2.3.3](#2.3.3) |08 Eylül 2018 |Ağustos 30, 2020 |
| [2.3.2](#2.3.2) |08 Mayıs 2018 |Ağustos 30, 2020 |
| [2.3.1](#2.3.1) |21 Aralık 2017 |Ağustos 30, 2020 |
| [2.3.0](#2.3.0) |10 Kasım 2017 |Ağustos 30, 2020 |
| [2.2.1](#2.2.1) |29 Eylül 2017 |Ağustos 30, 2020 |
| [2.2.0](#2.2.0) |10 Mayıs 2017 |Ağustos 30, 2020 |
| [2.1.0](#2.1.0) |01 Mayıs 2017 |Ağustos 30, 2020 |
| [2.0.1](#2.0.1) |Ekim 30, 2016 |Ağustos 30, 2020 |
| [2.0.0](#2.0.0) |Eylül 29, 2016 |Ağustos 30, 2020 |
| [1.9.0](#1.9.0) |Temmuz 07, 2016 |Ağustos 30, 2020 |
| [1.8.0](#1.8.0) |Haziran 14, 2016 |Ağustos 30, 2020 |
| [1.7.0](#1.7.0) |26 Nisan 2016 |Ağustos 30, 2020 |
| [1.6.1](#1.6.1) |08 Nisan 2016 |Ağustos 30, 2020 |
| [1.6.0](#1.6.0) |29 Mart 2016 |Ağustos 30, 2020 |
| [1.5.0](#1.5.0) |03 Ocak 2016 |Ağustos 30, 2020 |
| [1.4.2](#1.4.2) |06 Ekim 2015 |Ağustos 30, 2020 |
| 1.4.1 |06 Ekim 2015 |Ağustos 30, 2020 |
| [1.2.0](#1.2.0) |06 Ağustos 2015 |Ağustos 30, 2020 |
| [1.1.0](#1.1.0) |Temmuz 09, 2015 |Ağustos 30, 2020 |
| [1.0.1](#1.0.1) |Mayıs 25, 2015 |Ağustos 30, 2020 |
| [1.0.0](#1.0.0) |07 Nisan 2015 |Ağustos 30, 2020 |
| 0.9.4-prelease |14 Ocak 2015 |Şubat 29, 2016 |
| 0.9.3-prelease |09 Aralık 2014 |Şubat 29, 2016 |
| 0.9.2-prelease |Kasım 25, 2014 |Şubat 29, 2016 |
| 0.9.1-prelease |Eylül 23, 2014 |Şubat 29, 2016 |
| 0.9.0-prelease |Ağustos 21, 2014 |Şubat 29, 2016 |

## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi edinmek için [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfasına bakın. 

