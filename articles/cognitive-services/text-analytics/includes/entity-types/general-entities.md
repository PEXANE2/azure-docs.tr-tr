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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77211406"
---
## <a name="general-entity-types"></a>Genel varlık türleri:

### <a name="person"></a>Kişi

Metindeki kişi adlarını tanıyın.

Diller:
* Genel `Arabic`önizleme: `Czech`, `Dutch`, `English` `Chinese-Simplified` `Danish`, `French` `German`, `Hungarian` `Italian`, `Japanese` `Korean` `Norwegian (Bokmål)` `Russian` `Spanish` `Swedish` , , , , , , , , , , , , `Finnish` `Polish` `Portuguese (Portugal)` `Portuguese (Brazil)``Turkish`

| Alt tür adı | Açıklama                                                      | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------------------------------|---------------------------------------|
| Yok          | Tanınan kişi adları, `Bill Gates`örneğin ,`Marie Curie` | `2019-10-01`                          | 

### <a name="persontype"></a>Kişi Tipi
Bir kişinin elindeki iş türü veya rolü.

Diller:
* Genel önizleme:`English`

| Alt tür adı | Açıklama                                                                                | Model sürümüyle başlayan kullanılabilir |
|--------------|--------------------------------------------------------------------------------------------|----------------------------------------|
| Yok          | İş türleri `civil engineer`örneğin `salesperson` `chef`, `librarian`, , ,`nursing aide` | `2020-02-01`                           |

### <a name="location"></a>Konum

Doğal ve insan yapımı yerler, yapılar, coğrafi özellikler ve jeopolitik varlıklar.

Diller:

* Genel `Arabic`önizleme: `Czech`, `Dutch`, `English` `Chinese-Simplified` `Danish`, `French` `German`, `Hungarian` `Italian`, `Japanese` `Korean` `Norwegian (Bokmål)` `Russian` `Spanish` `Swedish` , , , , , , , , , , , , `Finnish` `Polish` `Portuguese (Portugal)` `Portuguese (Brazil)``Turkish`

| Alt tür adı              | Açıklama                                                                              | Model sürümüyle başlayan kullanılabilir |
|---------------------------|------------------------------------------------------------------------------------------|----------------------------------------|
| Yok                       | yerleri, örneğin `Atlantic Ocean` `library`, `Eiffel Tower`, ,`Statue of Liberty`  | `2019-10-01`                           |
| Jeopolitik Varlık (GPE) - Yalnızca İngilizce| Şehirler, ülkeler, devletler `Seattle` `Pennsylvania`örneğin `South Africa`, , ,`Tokyo` | `2020-02-01`                           |

### <a name="organization"></a>Kuruluş  

Tanınan kuruluşlar, şirketler, ajanslar ve diğer insan grupları. Örneğin: şirketler, siyasi gruplar, müzik grupları, spor kulüpleri, devlet kurumları ve kamu kuruluşları. Milliyetler ve dinler bu varlık türüne dahil değildir. 

Diller: 

* Genel `Arabic`önizleme: `Czech`, `Dutch`, `English` `Chinese-Simplified` `Danish`, `French` `German`, `Hungarian` `Italian`, `Japanese` `Korean` `Norwegian (Bokmål)` `Russian` `Spanish` `Swedish` , , , , , , , , , , , , `Finnish` `Polish` `Portuguese (Portugal)` `Portuguese (Brazil)``Turkish`

| Alt tür adı | Açıklama                                                                                             | Model sürümüyle başlayan kullanılabilir |
|--------------|---------------------------------------------------------------------------------------------------------|----------------------------------------|
| Yok          | organizasyonlar, `Microsoft`örneğin `NASA` `National Oceanic and Atmospheric Administration`, , ,`VOA` | `2019-10-01`                           |

### <a name="event"></a>Olay  

Tarihsel, sosyal ve doğal olaylar.  

Diller: 

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                                            | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------------------------------------|----------------------------------------|
| Yok          | `wedding`Gibi olaylar `hurricane`, `car accident` `solar eclipse`, , ,`American Revolution` | `2020-02-01`                           |

### <a name="product"></a>Ürün  

Çeşitli kategorilerdeki fiziksel nesneler.  

Diller: 

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                                                        | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------------------------------------------------|----------------------------------------|
| Yok          | Örneğin, `Microsoft Surface laptop`, `sunglasses` `motorcycle`, `bag`, ,`Xbox` | `2020-02-01`                           |
| Bilgi işlem    | `Azure Cosmos DB`, `Azure Kubernetes Service`                                     | `2020-02-01`                           |

### <a name="skill"></a>Beceri  

Bir yeteneği veya uzmanlığı açıklayan bir varlık.  

Diller: 

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                                                 | Model sürümüyle başlayan kullanılabilir |
|--------------|-----------------------------------------------------------------------------|----------------------------------------|
| Yok          | `nursing`, `data mining`, `linguistics`, `critical thinking`, `photography` | `2020-02-01`                           |

### <a name="phone-number"></a>Telefon Numarası

Telefon numaraları (yalnızca ABD Telefon numaraları). 

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                    | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------------|----------------------------------------|
| Yok          | Örneğin ABD telefon numaraları`(312) 555-0176` | `2019-10-01`                           |

### <a name="email"></a>Email

E-posta adresi. 

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                      | Model sürümüyle başlayan kullanılabilir |
|--------------|--------------------------------------------------|----------------------------------------|
| Yok          | Örneğin, e-posta adresi`support@contoso.com` | `2019-10-01`                           |

### <a name="url"></a>URL'si

İnternet URL'leri.

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                                          | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------------------|----------------------------------------|
| Yok          | Örneğin, web sitelerine URL'ler`https://www.bing.com` | `2019-10-01`                           |

### <a name="ip-address"></a>IP Adresi

İnternet Protokol Adresi

Diller:

* Genel önizleme:`English`

| Alt tür adı | Açıklama                              | Model sürümüyle başlayan kullanılabilir |
|--------------|------------------------------------------|----------------------------------------|
| Yok          | Örneğin ağ adresi`10.0.0.101` | `2019-10-01`                           |

###  <a name="datetime"></a>DateTime

Tarih ve Saat varlıkları. 

* Model sürümüyle başlayan kullanılabilir`2019-10-01`

Diller:

* Genel `Chinese-Simplified`önizleme: `English` `French`, `German` , , ve`Spanish`

| Alt tür adı    | Örnekler                     |
|-------------|------------------------------|
| Yok         | `6:30PM February 4, 2012`, `4/1/2011 2:45`                   |
| Tarih  | `May 2nd, 2017`, `05/02/2017`       |
| Zaman     | `8:15`, `6AM`              |
| Tarih Aralığı    | `August 2nd to August 5th`         |
| Zaman Aralığı   | `4-6PM`, `10:00AM to Noon`          |
| Süre | `2.5 minutes`, `one and a half hours`         |
| Ayarla | `every Saturday`         |

###  <a name="quantity"></a>Miktar

Sayılar ve sayısal miktarlar. 

* Model sürümüyle başlayan kullanılabilir`2019-10-01`

Diller:

* Genel `Chinese-Simplified`önizleme: `English` `French`, `German` , , ve`Spanish`

| Alt tür adı    | Örnekler                     |
|-------------|------------------------------|
| Sayı         | `6`, `six`                   |
| Yüzde  | `50%`, `fifty percent`       |
| Sıralı     | `2nd`, `second`              |
| Yaş         | `90 day old`, `30 years old` |
| Para birimi    | `$10.99`, `€30.00`           |
| Boyut   | `10 miles`, `40 cm`          |
| Sıcaklık | `32 degrees`, `10°C`         |
