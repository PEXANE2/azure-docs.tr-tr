---
title: Microsoft standart kuralları altyapı eylemleriyle Azure CDN | Microsoft Docs
description: Microsoft standart kuralları altyapı eylemleriyle ilgili Azure CDN için başvuru belgeleri.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: dbde93cc7ffd21e341653407e6e4f910e4620974
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615994"
---
# <a name="azure-cdn-from-microsoft-standard-rules-engine-actions"></a>Microsoft standart kuralları altyapı eylemleriyle Azure CDN

Bu makalede, Microsoft [Standart kuralları altyapısından](cdn-standard-rules-engine.md)Azure Content DELIVERY Network (CDN) için kullanılabilir eylemlerin ayrıntılı açıklamaları listelenmektedir.

Bir kuralın ikinci bölümü bir eylemdir. Bir eylem, bir eşleşme koşulları kümesi tarafından tanımlanan istek türüne uygulanan davranışı tanımlar.

## <a name="actions"></a>Eylemler

Aşağıdaki eylemler kullanılabilir. 

## <a name="cache-expiration"></a>Önbellek süre sonu

Bu eylem, kural eşleştirme koşulları tarafından belirtilen istekler için uç noktanın TTL değerinin üzerine yazılmasını sağlar.

**Gerekli alanlar**

Önbellek davranışı |                
---------------|----------------
Atlama önbelleği | Bu seçenek belirlendiğinde ve kural eşleştiğinde, içerik önbelleğe alınmaz.
Manızı | Bu seçenek belirlendiğinde ve kural eşleştiğinde, kaynaktan döndürülen TTL değeri, eylemde belirtilen değerle geçersiz kılınır.
Eksikse ayarla | Bu seçenek belirlendiğinde ve kural eşleştiğinde, kaynaktan bir TTL değeri döndürülmezse, kural TTL 'yi eylemde belirtilen değere ayarlar.

**Ek alanlar**

Gün | Saat | Dakika | Saniye
-----|-------|---------|--------
'Tir | 'Tir | 'Tir | 'Tir 

## <a name="cache-key-query-string"></a>Önbellek anahtarı sorgu dizesi

Bu eylem, sorgu dizelerine göre önbellek anahtarını değiştirmenize olanak sağlar.

**Gerekli alanlar**

Davranış | Açıklama
---------|------------
içeriyor | Bu seçenek belirlendiğinde ve kural eşleştiğinde, parametrelerde belirtilen sorgu dizeleri önbellek anahtarı oluşturulurken dahil edilir. 
Her benzersiz URL'yi önbelleğe al | Bu seçenek belirlendiğinde ve kural eşleştiğinde, her benzersiz URL kendi önbellek anahtarına sahip olur. 
Amaz | Bu seçenek belirlendiğinde ve kural eşleştiğinde, parametrelerde belirtilen sorgu dizeleri önbellek anahtarı oluşturulurken dışarıda bırakılır.
Sorgu dizelerini yoksay | Bu seçenek belirlendiğinde ve kural eşleştiğinde, önbellek anahtarı oluşturulurken sorgu dizeleri göz önünde bulundurulmaz. 

## <a name="modify-request-header"></a>Istek üst bilgisini Değiştir

Bu eylem, kaynağınıza gönderilen isteklerde bulunan üst bilgileri değiştirmenize izin verir.

**Gerekli alanlar**

Eylem | HTTP üst bilgi adı | Değer
-------|------------------|------
Ekle | Bu seçenek belirlendiğinde ve kural eşleştiğinde, üstbilgi adı bölümünde belirtilen üstbilgi belirtilen değere sahip isteğe eklenir. Üst bilgi zaten mevcutsa, bu değer mevcut değere eklenir. | Dize
Yazılacak | Bu seçenek belirlendiğinde ve kural eşleştiğinde, üstbilgi adı bölümünde belirtilen üstbilgi belirtilen değere sahip isteğe eklenir. Üst bilgi zaten mevcutsa, değer varolan değerin üzerine yazar. | Dize
Sil | Bu seçenek belirlendiğinde ve kural eşleşiyorsa ve kuralda belirtilen üst bilgi varsa, istekten silinir. | Dize

## <a name="modify-response-header"></a>Yanıt üst bilgisini Değiştir

Bu eylem, son istemcilerinize döndürülen yanıtlarda bulunan üstbilgileri değiştirmenize izin verir

**Gerekli alanlar**

Eylem | HTTP üst bilgi adı | Değer
-------|------------------|------
Ekle | Bu seçenek belirlendiğinde ve kural eşleştiğinde, üstbilgi adı bölümünde belirtilen üstbilgi belirtilen değere sahip yanıta eklenir. Üst bilgi zaten mevcutsa, bu değer mevcut değere eklenir. | Dize
Yazılacak | Bu seçenek belirlendiğinde ve kural eşleştiğinde, üstbilgi adı bölümünde belirtilen üstbilgi belirtilen değere sahip yanıta eklenir. Üst bilgi zaten mevcutsa, değer varolan değerin üzerine yazar. | Dize
Sil | Bu seçenek belirlendiğinde ve kural eşleşiyorsa ve kuralda belirtilen üst bilgi varsa, yanıttan silinir. | Dize

## <a name="url-redirect"></a>URL yeniden yönlendirme

Bu eylem, son istemcileri yeni bir URL 'ye yönlendirmenizi sağlar. 

**Gerekli alanlar**

Alan | Açıklama 
------|------------
Tür | İstek sahibine döndürülecek yanıt türünü seçin. Seçenekler-302 bulundu, 301 taşındı, 307 geçici yeniden yönlendirme ve 308 kalıcı yeniden yönlendirme
Protokol | Match Isteği, HTTP veya HTTPS
Ana Bilgisayar Adı | İsteğin yönlendirileceği ana bilgisayar adını seçin. Gelen ana bilgisayarı korumak için boş bırakın.
Yol | Yeniden Yönlendirmede kullanılacak yolu tanımlayın. Gelen yolu korumak için boş bırakın.  
Sorgu Dizesi | Yeniden Yönlendirmede kullanılan sorgu dizesini tanımlayın. Gelen sorgu dizesini korumak için boş bırakın. 
Parçada | Yeniden Yönlendirmede kullanılacak parçayı tanımlayın. Gelen parçayı korumak için boş bırakın. 

Mutlak bir URL kullanmanız önemle önerilir. Göreli URL kullanımı, CDN URL 'Lerini geçersiz bir yola yönlendirebilir. 

## <a name="url-rewrite"></a>URL Yeniden Yazma

Bu eylem, bir isteğin yolunu, kaynağınıza yeniden yazmanız için izin verir.

**Gerekli alanlar**

Alan | Açıklama 
------|------------
Kaynak stili | Değiştirilecek URL yolundaki kaynak modelini tanımlayın. Şu anda, kaynak stili önek tabanlı eşleşme kullanıyor. Tüm URL yollarını eşleştirmek için kaynak model değeri olarak "/" kullanın.
Hedef | Yeniden yazma sırasında kullanılacak hedef yolunu tanımlayın. Bu, kaynak deseninin üzerine yazacak
Eşleşmeyen yolu koru | Yanıt Evet ise, kaynak örüntüden sonraki kalan yol yeni hedef yoluna eklenir. 


[Başa dön](#actions)

</br>

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Content Delivery Network genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-standard-rules-engine-reference.md)
- [Kural altyapısı eşleştirme koşulları](cdn-standard-rules-engine-match-conditions.md)
- [Standart kurallar altyapısını kullanarak HTTPS 'yi zorla](cdn-standard-rules-engine.md)
