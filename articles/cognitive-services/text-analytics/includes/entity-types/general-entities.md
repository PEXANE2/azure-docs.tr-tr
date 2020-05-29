---
title: Genel adlandırılmış varlıklar
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 05/13/2020
ms.author: aahi
ms.openlocfilehash: 32e80c50ff6f543679852cbd7e5ce9bda92d01e1
ms.sourcegitcommit: f0b206a6c6d51af096a4dc6887553d3de908abf3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140964"
---
Uç noktaya istek gönderilirken aşağıdaki varlık kategorileri döndürülür `/entities/recognition/general` .

| Kategori   | Subcategory | Açıklama                          | Model sürümü başlatılıyor                                                    | Notlar |
|------------|-------------|--------------------------------------|-------------------------------------------------------------|--------------------------------------|
| Kişi     | YOK         | Kişilerin adları.  | `2019-10-01`  | Ayrıca, NER v 2.1 tarafından döndürülen |
| Kişilik türü | YOK         | Bir kişi tarafından tutulan iş türleri veya roller. | `2020-02-01` | |
|Konum    | YOK         | Doğal ve insan tarafından oluşturulan yer işaretleri, yapılar, coğrafi özellikler ve geopolitik varlıklar     |  `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
|Konum     | Geopolitik varlık (GPE)        | Şehir, ülke/bölge, eyalet.      | `2020-02-01` | |
|Konum     | Yapısal                       | Manın yapıları. | `2020-04-01` | |
|Konum     | Coğrafi       | Rivers, Oceans ve deserler gibi coğrafi ve doğal Özellikler. |  `2020-04-01` | |
|Kuruluş  | YOK | Şirketler, siyatik gruplar, müzik bantları, spor sinek, kamu gövdeleri ve kamu kuruluşları.  | `2019-10-01` | Bu varlık türünde ülke almallikleri ve dini dahil değildir. Ayrıca, NER v 2.1 tarafından döndürülen |
|Kuruluş | Birinin | Tıbbi şirketler ve gruplar. | `2020-04-01` |  |
|Kuruluş | Stok alışverişi | Kambiyo senedi grupları. | `2020-04-01` | |
| Kuruluş | Spor | Spor ile ilgili kuruluşlar. | `2020-04-01` |  |
| Olay  | YOK | Geçmiş, sosyal ve doğal olarak gerçekleşen olaylar. | `2020-02-01` |  |
| Olay  | Kültürel | Kültürel olaylar ve tatiller. | `2020-04-01` | |
| Olay  | Olmayan | Doğal olarak gerçekleşen olaylar. | `2020-04-01` |  |
| Olay  | Spor | Spor olayları.  | `2020-04-01` | |
| Ürün | YOK | Çeşitli kategorilerin fiziksel nesneleri. | `2020-02-01` | |
| Ürün | Ürünleri hesaplama | Ürünleri hesaplama. |  `2020-02-01 ` | |
| İmde | YOK | Yetenek, beceri veya uzmanlık. | `2020-02-01` |  |
| Adres | YOK | Tam posta adresleri.  | `2020-04-01` |  |
| PhoneNumber | YOK | Telefon numaraları (yalnızca ABD ve AB telefon numaraları). | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| E-posta | YOK | E-posta adresleri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| URL | YOK | Web sitelerinin URL 'Leri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen  |
| IP | YOK | Ağ IP adresleri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| DateTime | YOK | Günün tarihleri ve saatleri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen | 
| DateTime | Tarih | Takvim tarihleri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| DateTime | Saat | Günün saati | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| DateTime | DateRange | Tarih aralıkları. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| DateTime | TimeRange | Zaman aralıkları. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| DateTime | Süre | Sürelerde. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| DateTime | Ayarla | Ayarlama, yinelenme süreleri. |  `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | YOK | Sayılar ve sayısal miktarlar. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen  |
| Miktar | Sayı | Sayılarının. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Yüzde | Değerleri.| `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Numarasını | Sıra sayıları. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Yaş | Geçirir. | `2019-10-01` |  Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Para Birimi | Ayarlarsanız. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Boyut | Boyutlar ve ölçümler. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Sıcaklık | Sıcak. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
