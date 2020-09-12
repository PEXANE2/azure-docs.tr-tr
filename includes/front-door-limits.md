---
title: dosya dahil etme
description: dosya dahil etme
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: include
ms.date: 05/09/2019
ms.author: sharadag
ms.custom: include file
ms.openlocfilehash: 361fb18574e90fc46e45bff8914c51ee1afa2f96
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89411647"
---
| Kaynak | Sınır |
| --- | --- |
| Abonelik başına Azure ön kapı kaynakları | 100 |
| Kaynak başına özel etki alanları içeren ön uç Konakları | 500 |
| Kaynak başına yönlendirme kuralları | 500 |
| Kaynak başına arka uç havuzları | 50 |
| Arka uç havuzu başına arka uçlar | 100 |
| Yönlendirme kuralı için eşleştirilecek yol desenleri | 25 |
| Tek bir önbellek temizleme çağrısında URL 'Ler | 100 |
| İlke başına özel Web uygulaması güvenlik duvarı kuralları | 100 |
| Abonelik başına Web uygulaması güvenlik duvarı ilkesi | 100 |
| Web uygulaması güvenlik duvarı özel kural başına eşleştirme koşulları | 10 |
| Web uygulaması güvenlik duvarı IP adresi aralıklarını eşleşme koşulu başına | 600 |
| Web uygulaması güvenlik duvarı dizesi eşleşme koşulu başına değerleri | 10 |
| Web uygulaması güvenlik duvarı dizesi eşleşme değeri uzunluğu | 256 |
| Web uygulaması güvenlik duvarı GÖNDERI gövdesi parametre adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı HTTP üst bilgi adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı tanımlama bilgisi adı uzunluğu | 256 |
| Web uygulaması güvenlik duvarı HTTP istek gövdesi boyutu inceledi | 128 KB |
| Web uygulaması güvenlik duvarı özel yanıt gövdesi uzunluğu | 2 KB |

### <a name="timeout-values"></a>Zaman aşımı değerleri
#### <a name="client-to-front-door"></a>İstemciden ön kapıya
* Ön kapıda 61 saniyelik boş bir TCP bağlantısı zaman aşımı vardır.

#### <a name="front-door-to-application-back-end"></a>Uygulama arka ucuna ön kapı
* Yanıt öbekli bir yanıt ise, ilk öbek alındığında bir 200 döndürülür.
* HTTP isteği arka uca iletilene sonra, ön kapı arka uçta ilk paket için 30 saniye bekler. Ardından istemciye 503 hatası döndürür. Bu değer, API 'de sendRecvTimeoutSeconds alanı aracılığıyla yapılandırılabilir.
    * Önbelleğe alma senaryolarında, bu zaman aşımı yapılandırılamaz ve bu nedenle, bir istek önbelleğe alınmışsa ve ilk paketin ön kapıdan ya da arka uca en fazla 30 saniye sürerse, istemciye bir 504 hatası döndürülür. 
* Arka uçtan ilk paket alındıktan sonra, ön kapı boşta kalma zaman aşımında 30 saniye bekler. Ardından istemciye 503 hatası döndürür. Bu zaman aşımı değeri yapılandırılamaz.
* Arka uç TCP oturumu zaman aşımının ön kapısı 90 saniyedir.

### <a name="upload-and-download-data-limit"></a>Veri sınırını karşıya yükleme ve indirme

|  | Öbekli aktarım kodlaması (CTE) ile | HTTP parçalama olmadan |
| ---- | ------- | ------- |
| **İndir** | İndirme boyutunda sınır yoktur. | İndirme boyutunda sınır yoktur. |
| **Karşıya yükle** |    Her CTE yüklemesi 2 GB 'den az olduğu sürece sınır yoktur. | Boyut 2 GB 'tan büyük olamaz. |

### <a name="other-limits"></a>Diğer sınırlar
* En büyük URL boyutu-8.192 bayt-ham URL 'nin uzunluk üst sınırını belirtir (Düzen + ana bilgisayar + bağlantı noktası + yol + URL 'nin sorgu dizesi)
* En fazla sorgu dizesi boyutu-4.096 bayt-sorgu dizesinin uzunluk üst sınırını bayt cinsinden belirtir.
* Durum araştırma URL 'sindeki en büyük HTTP yanıt üst bilgisi boyutu-4.096 bayt-sistem durumu araştırmalarının tüm yanıt üst bilgilerinin en fazla uzunluğu belirtildi. 
