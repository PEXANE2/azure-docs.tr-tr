---
title: include dosyası
description: include dosyası
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.topic: include
ms.date: 05/09/2019
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: 148ec3eccce71ab7a4a6c1391c0fa4753c248bd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80335086"
---
| Kaynak | Sınır |
| --- | --- |
| Abonelik başına Azure Ön Kapı kaynakları | 100 |
| Kaynak başına özel etki alanlarını içeren ön uç ana bilgisayarlar | 500 |
| Kaynak başına yönlendirme kuralları | 500 |
| Kaynak başına arka uç havuzları | 50 |
| Arka uç başına arka uçlar | 100 |
| Yönlendirme kuralıyla eşleşecek yol desenleri | 25 |
| Tek bir önbellek temizleme çağrısındaURL'ler | 100 |
| İlke başına özel web uygulaması güvenlik duvarı kuralları | 100 |
| Abonelik başına Web uygulaması güvenlik duvarı ilkesi | 100 |
| Web uygulaması güvenlik duvarı maç koşulları özel kural başına | 10 |
| Web uygulaması güvenlik duvarı IP adresi aralıkları maç durumu başına | 600 |
| Web uygulaması güvenlik duvarı dize maç durumu başına değerleri | 10 |
| Web uygulaması güvenlik duvarı dize maç değeri uzunluğu | 256 |
| Web uygulaması güvenlik duvarı POST gövde parametre adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı HTTP üstbilgi adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı çerez adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı HTTP istek gövde boyutu denetlenir | 128 KB |
| Web uygulaması güvenlik duvarı özel tepki gövde uzunluğu | 2 KB |

### <a name="timeout-values"></a>Zaman ara değerleri
#### <a name="client-to-front-door"></a>Müşteriden Ön Kapıya
* Ön Kapı 61 saniye boşta TCP bağlantı zaman adabı vardır.

#### <a name="front-door-to-application-back-end"></a>Uygulama arka uç için Ön Kapı
* Yanıt tıklım tıklım bir yanıtsa, ilk öbek alınırsa veya aldığında 200 döndürülür.
* HTTP isteği arka uca iletildikten sonra, Ön Kapı arka uçtan gelen ilk paket için 30 saniye bekler. Sonra istemciye bir 503 hatası döndürür. Bu değer API'deki SendRecvTimeoutSeconds alanı üzerinden yapılandırılabilir.
    * Önbelleğe alma senaryoları için, bu zaman sonu yapılandırılamaz ve bu nedenle, bir istek önbelleğe alınmışsa ve ön kapıdan veya arka uçtan ilk paket için 30 saniyeden fazla sürerse, istemciye 504 hatası döndürülür. 
* İlk paket arka uçtan alındıktan sonra, Ön Kapı boşta bir zaman diliminde 30 saniye bekler. Sonra istemciye bir 503 hatası döndürür. Bu zaman dışı değer yapılandırılamaz.
* Arka uç TCP oturum zaman sonuna Ön Kapı 90 saniyedir.

### <a name="upload-and-download-data-limit"></a>Veri yükleme ve indirme sınırı

|  | Öbeklenmiş aktarım kodlaması (CTE) ile | HTTP yığın olmadan |
| ---- | ------- | ------- |
| **İndir** | İndirme boyutunda bir sınır yoktur. | İndirme boyutunda bir sınır yoktur. |
| **Karşıya yükle** |    Her CTE yüklemesi 2 GB'dan az olduğu sürece sınır yoktur. | Boyutu 2 GB'tan büyük olamaz. |

### <a name="other-limits"></a>Diğer sınırlar
* Maksimum URL boyutu - 8.192 bayt - Ham URL'nin maksimum uzunluğunu belirtir (şema + ana bilgisayar adı + bağlantı noktası + yol + URL sorgu dizesi)
* Maksimum Sorgu Dizesi boyutu - 4.096 bayt - sorgu dizesinin en büyük uzunluğunu baytolarak belirtir.
* Sağlık sondası URL'sinden maksimum HTTP yanıt üstbilgi boyutu - 4.096 bayt - Sağlık sondalarının tüm yanıt başlıklarının maksimum uzunluğu belirtilir. 
