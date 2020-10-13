---
title: Genel adlandırılmış varlıklar
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: include
ms.date: 05/13/2020
ms.author: aahi
ms.openlocfilehash: 630f04bf2cc9e7de6331f9d25754a20fe2327d76
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91779740"
---
Uç noktaya istek gönderilirken aşağıdaki varlık kategorileri döndürülür `/entities/recognition/general` .

| Kategori   | Alt Kategori | Açıklama                          | Model sürümü başlatılıyor                                                    | Notlar |
|------------|-------------|--------------------------------------|-------------------------------------------------------------|--------------------------------------|
| Kişi     | Yok         | Kişilerin adları.  | `2019-10-01`  | Ayrıca, NER v 2.1 tarafından döndürülen |
| Kişilik türü | Yok         | Bir kişi tarafından tutulan iş türleri veya roller. | `2020-02-01` | |
|Konum    | Yok         | Doğal ve insan tarafından oluşturulan yer işaretleri, yapılar, coğrafi özellikler ve geopolitik varlıklar     |  `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
|Konum     | Geopolitik varlık (GPE)        | Şehir, ülke/bölge, eyalet.      | `2020-02-01` | |
|Konum     | Yapısal                       | Manın yapıları. | `2020-04-01` | |
|Konum     | Coğrafi       | Rivers, Oceans ve deserler gibi coğrafi ve doğal Özellikler. |  `2020-04-01` | |
|Kuruluş  | Yok | Şirketler, siyatik gruplar, müzik bantları, spor sinek, kamu gövdeleri ve kamu kuruluşları.  | `2019-10-01` | Bu varlık türünde ülke almallikleri ve dini dahil değildir. Ayrıca, NER v 2.1 tarafından döndürülen |
|Kuruluş | Birinin | Tıbbi şirketler ve gruplar. | `2020-04-01` |  |
|Kuruluş | Stok alışverişi | Kambiyo senedi grupları. | `2020-04-01` | |
| Kuruluş | Spor | Spor ile ilgili kuruluşlar. | `2020-04-01` |  |
| Olay  | Yok | Geçmiş, sosyal ve doğal olarak gerçekleşen olaylar. | `2020-02-01` |  |
| Olay  | Kültürel | Kültürel olaylar ve tatiller. | `2020-04-01` | |
| Olay  | Olmayan | Doğal olarak gerçekleşen olaylar. | `2020-04-01` |  |
| Olay  | Spor | Spor olayları.  | `2020-04-01` | |
| Ürün | Yok | Çeşitli kategorilerin fiziksel nesneleri. | `2020-02-01` | |
| Ürün | Ürünleri hesaplama | Ürünleri hesaplama. |  `2020-02-01 ` | |
| İmde | Yok | Yetenek, beceri veya uzmanlık. | `2020-02-01` |  |
| Adres | Yok | Tam posta adresleri.  | `2020-04-01` |  |
| PhoneNumber | Yok | Telefon numaraları (yalnızca ABD ve AB telefon numaraları). | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| E-posta | Yok | E-posta adresleri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| URL | Yok | Web sitelerinin URL 'Leri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen  |
| IP | Yok | Ağ IP adresleri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| DateTime | Yok | Günün tarihleri ve saatleri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen | 
| Tarih-Saat | Tarih | Takvim tarihleri. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Tarih-Saat | Süre | Günün saati | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Tarih-Saat | DateRange | Tarih aralıkları. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Tarih-Saat | TimeRange | Zaman aralıkları. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Tarih-Saat | Süre | Sürelerde. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Tarih-Saat | Sabit | Ayarlama, yinelenme süreleri. |  `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Yok | Sayılar ve sayısal miktarlar. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen  |
| Miktar | Sayı | Sayılarının. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Yüzde | Değerleri.| `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Sıralı | Sıra sayıları. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Yaş | Geçirir. | `2019-10-01` |  Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Para birimi | Ayarlarsanız. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Boyut | Boyutlar ve ölçümler. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
| Miktar | Sıcaklık | Sıcak. | `2019-10-01` | Ayrıca, NER v 2.1 tarafından döndürülen |
