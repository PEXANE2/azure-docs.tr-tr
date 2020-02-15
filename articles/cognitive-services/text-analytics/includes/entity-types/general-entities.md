---
title: Genel adlandırılmış varlıklar
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 02/06/2020
ms.author: aahi
ms.openlocfilehash: ba516a548fc8385ca86526a7f2dd082b27e53208
ms.sourcegitcommit: 2823677304c10763c21bcb047df90f86339e476a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2020
ms.locfileid: "77211406"
---
## <a name="general-entity-types"></a>Genel varlık türleri:

### <a name="person"></a>Kişi

Metindeki kişi adlarını tanır.

Diller:
* Genel Önizleme: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` ve `Turkish`

| Alt tür adı | Açıklama                                                      | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------------------------------|---------------------------------------|
| Yok          | Tanınan kişi adları, örneğin `Bill Gates``Marie Curie` | `2019-10-01`                          | 

### <a name="persontype"></a>Kişilik türü
Bir kişi tarafından tutulan iş türü veya rol.

Diller:
* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                                                                | Model sürümünden itibaren kullanılabilir |
|--------------|--------------------------------------------------------------------------------------------|----------------------------------------|
| Yok          | İş türleri örneğin `civil engineer`, `salesperson`, `chef`, `librarian`, `nursing aide` | `2020-02-01`                           |

### <a name="location"></a>Konum

Doğal ve insan tarafından oluşturulan yer işaretleri, yapılar, coğrafi özellikler ve geopolitik varlıklar.

Diller:

* Genel Önizleme: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` ve `Turkish`

| Alt tür adı              | Açıklama                                                                              | Model sürümünden itibaren kullanılabilir |
|---------------------------|------------------------------------------------------------------------------------------|----------------------------------------|
| Yok                       | konumlar, örneğin `Atlantic Ocean`, `library`, `Eiffel Tower`, `Statue of Liberty`  | `2019-10-01`                           |
| Geopolitik varlık (GPE)-yalnızca Ingilizce| Şehirlerin, ülkelerin, örneğin `Seattle`, `Pennsylvania`, `South Africa`, `Tokyo` | `2020-02-01`                           |

### <a name="organization"></a>Kuruluş  

Tanınan kuruluşlar, şirketler, kurumlar ve diğer kişi grupları. Örneğin: şirketler, siyatik gruplar, müzik bantları, spor sinek, kamu gövdeleri ve kamu kuruluşları. Bu varlık türünde ülke almallikleri ve dini dahil değildir. 

Diller: 

* Genel Önizleme: `Arabic`, `Czech`, `Chinese-Simplified`, `Danish`, `Dutch`, `English`, `Finnish`, `French`, `German`, `Hungarian`, `Italian`, `Japanese`, `Korean`, `Norwegian (Bokmål)`, `Polish`, `Portuguese (Portugal)`, `Portuguese (Brazil)`, `Russian`, `Spanish`, `Swedish` ve `Turkish`

| Alt tür adı | Açıklama                                                                                             | Model sürümünden itibaren kullanılabilir |
|--------------|---------------------------------------------------------------------------------------------------------|----------------------------------------|
| Yok          | kuruluşlar, örneğin `Microsoft`, `NASA`, `National Oceanic and Atmospheric Administration`,`VOA` | `2019-10-01`                           |

### <a name="event"></a>Olay  

Geçmiş, sosyal ve doğal ortaya çıkma olayları.  

Diller: 

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                                            | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------------------------------------|----------------------------------------|
| Yok          | `wedding`, `hurricane`, `car accident`, `solar eclipse`gibi olaylar `American Revolution` | `2020-02-01`                           |

### <a name="product"></a>Ürün  

Çeşitli kategorilerin fiziksel nesneleri.  

Diller: 

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                                                        | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------------------------------------------------|----------------------------------------|
| Yok          | Örneğin, `Microsoft Surface laptop`, `sunglasses`, `motorcycle`, `bag`, `Xbox` | `2020-02-01`                           |
| Bilgi işlem    | `Azure Cosmos DB`, `Azure Kubernetes Service`                                     | `2020-02-01`                           |

### <a name="skill"></a>İmde  

Bir özelliği veya uzmanlığı açıklayan bir varlık.  

Diller: 

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                                                 | Model sürümünden itibaren kullanılabilir |
|--------------|-----------------------------------------------------------------------------|----------------------------------------|
| Yok          | `nursing`, `data mining`, `linguistics`, `critical thinking`, `photography` | `2020-02-01`                           |

### <a name="phone-number"></a>Telefon numarası

Telefon numaraları (yalnızca ABD telefon numaraları). 

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                    | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------------|----------------------------------------|
| Yok          | ABD telefon numaraları, örneğin `(312) 555-0176` | `2019-10-01`                           |

### <a name="email"></a>E-posta

E-posta adresi. 

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                      | Model sürümünden itibaren kullanılabilir |
|--------------|--------------------------------------------------|----------------------------------------|
| Yok          | E-posta adresi, örneğin `support@contoso.com` | `2019-10-01`                           |

### <a name="url"></a>URL'si

Internet URL 'Leri.

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                                          | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------------------|----------------------------------------|
| Yok          | Web sitelerine yönelik URL 'Ler, örneğin `https://www.bing.com` | `2019-10-01`                           |

### <a name="ip-address"></a>IP Adresi

Internet Protokolü adresi

Diller:

* Genel Önizleme: `English`

| Alt tür adı | Açıklama                              | Model sürümünden itibaren kullanılabilir |
|--------------|------------------------------------------|----------------------------------------|
| Yok          | Örneğin, ağ adresi `10.0.0.101` | `2019-10-01`                           |

###  <a name="datetime"></a>DateTime

Tarih ve saat varlıkları. 

* Model sürümünden itibaren kullanılabilir `2019-10-01`

Diller:

* Genel Önizleme: `Chinese-Simplified`, `English`, `French`, `German` ve `Spanish`

| Alt tür adı    | Örnekler                     |
|-------------|------------------------------|
| Yok         | `6:30PM February 4, 2012`, `4/1/2011 2:45`                   |
| Tarih  | `May 2nd, 2017`, `05/02/2017`       |
| Zaman     | `8:15`, `6AM`              |
| DateRange    | `August 2nd to August 5th`         |
| TimeRange   | `4-6PM`, `10:00AM to Noon`          |
| Süre | `2.5 minutes`, `one and a half hours`         |
| Ayarla | `every Saturday`         |

###  <a name="quantity"></a>Miktar

Sayılar ve sayısal miktarlar. 

* Model sürümünden itibaren kullanılabilir `2019-10-01`

Diller:

* Genel Önizleme: `Chinese-Simplified`, `English`, `French`, `German` ve `Spanish`

| Alt tür adı    | Örnekler                     |
|-------------|------------------------------|
| Sayı         | `6`, `six`                   |
| Yüzde  | `50%`, `fifty percent`       |
| Numarasını     | `2nd`, `second`              |
| Yaş         | `90 day old`, `30 years old` |
| Para birimi    | `$10.99`, `€30.00`           |
| Boyut   | `10 miles`, `40 cm`          |
| Sıcaklık | `32 degrees`, `10°C`         |
