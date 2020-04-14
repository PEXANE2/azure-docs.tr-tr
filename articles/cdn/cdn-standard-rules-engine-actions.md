---
title: Azure CDN için Standart kurallar altyapısındaki eylemler | Microsoft Dokümanlar
description: Azure İçerik Dağıtım Ağı (Azure CDN) için Standart kurallar altyapısındaki eylemler için başvuru belgeleri.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: allensu
ms.openlocfilehash: 29138b4fc6716ae5361cc4d7f97ceba41b90c2da
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81259961"
---
# <a name="actions-in-the-standard-rules-engine-for-azure-cdn"></a>Azure CDN için Standart kurallar altyapısındaki eylemler

Azure İçerik Teslim Ağı (Azure CDN) için [Standart kurallar altyapısında,](cdn-standard-rules-engine.md) bir kural bir veya daha fazla eşleşme koşulu ve bir eylemden oluşur. Bu makalede, Azure CDN için Standart kurallar altyapısında kullanabileceğiniz eylemlerin ayrıntılı açıklamaları sağlanmaktadır.

Kuralın ikinci bölümü bir eylemdir. Eylem, eşleşme koşulunun veya eşleşme koşulları kümesinin tanımladığı istek türüne uygulanan davranışı tanımlar.

## <a name="actions"></a>Eylemler

Aşağıdaki eylemler Azure CDN için Standart kurallar altyapısında kullanılabilir. 

### <a name="cache-expiration"></a>Önbellek süresi

Kuralların koşullarıyla eşleşen istekleri için bitiş noktasının yaşam (TTL) değerinin üzerine yazmak için bu eylemi kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Önbellek davranışı |  Açıklama              
---------------|----------------
Önbelleği atla | Bu seçenek seçildiğinde ve kural eşleştiğinde, içerik önbelleğe alınmaz.
Geçersiz kıl | Bu seçenek seçildiğinde ve kural eşleştiğinde, kaynağınızdan döndürülen TTL değeri eylemde belirtilen değerle birlikte üzerine yazılır.
Eksikse ayarla | Bu seçenek seçildiğinde ve kural eşleştiğinde, kaynağınızdan TTL değeri döndürülmediyse, kural TTL'yi eylemde belirtilen değere ayarlar.

#### <a name="additional-fields"></a>Ek alanlar

Gün | Saat | Dakika | Saniye
-----|-------|---------|--------
int | int | int | int 

### <a name="cache-key-query-string"></a>Önbellek anahtarı sorgu dizesi

Sorgu dizelerini temel alan önbellek anahtarını değiştirmek için bu eylemi kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Davranış | Açıklama
---------|------------
Şunları Dahil Et: | Bu seçenek seçildiğinde ve kural eşleştiğinde, önbellek anahtarı oluşturulduğunda parametrelerde belirtilen sorgu dizeleri dahil edilir. 
Her benzersiz URL'yi önbelleğe al | Bu seçenek seçildiğinde ve kural eşleştiğinde, her benzersiz URL'nin kendi önbellek anahtarı vardır. 
Exclude | Bu seçenek seçildiğinde ve kural eşleştiğinde, önbellek anahtarı oluşturulduğunda parametrelerde belirtilen sorgu dizeleri dışlanır.
Sorgu dizelerini yoksay | Bu seçenek seçildiğinde ve kural eşleştiğinde, önbellek anahtarı oluşturulduğunda sorgu dizeleri dikkate alınmaz. 

### <a name="modify-request-header"></a>İstek üstbilgisini değiştirme

Kaynağınıza gönderilen isteklerde bulunan üstbilgileri değiştirmek için bu eylemi kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Eylem | HTTP üstbilgi adı | Değer
-------|------------------|------
Ekle | Bu seçenek seçildiğinde ve kural eşleştiğinde, **üstbilgi adında** belirtilen üstbilgi belirtilen değerle isteğe eklenir. Üstbilgi zaten mevcutsa, değer varolan değere eklenir. | Dize
Üzerine yaz | Bu seçenek seçildiğinde ve kural eşleştiğinde, **üstbilgi adında** belirtilen üstbilgi belirtilen değerle isteğe eklenir. Üstbilgi zaten mevcutsa, belirtilen değer varolan değerin üzerine yazar. | Dize
Sil | Bu seçenek seçildiğinde, kural eşleşir ve kuralda belirtilen üstbilgi bulunursa, üstbilgi istekten silinir. | Dize

### <a name="modify-response-header"></a>Yanıt üstbilgisini değiştirme

Müşterilerinize döndürülen yanıtlarda bulunan üstbilgileri değiştirmek için bu eylemi kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Eylem | HTTP Üstbilgi adı | Değer
-------|------------------|------
Ekle | Bu seçenek seçildiğinde ve kural eşleştiğinde, **üstbilgi adında** belirtilen üstbilgi belirtilen **Değer**kullanılarak yanıta eklenir. Üstbilgi zaten mevcutsa, **Değer** varolan değere eklenir. | Dize
Üzerine yaz | Bu seçenek seçildiğinde ve kural eşleştiğinde, **üstbilgi adında** belirtilen üstbilgi belirtilen **Değer**kullanılarak yanıta eklenir. Üstbilgi zaten mevcutsa, **Değer** varolan değerin üzerine yazar. | Dize
Sil | Bu seçenek seçildiğinde, kural eşleşir ve kuralda belirtilen üstbilgi bulunursa, üstbilgi yanıttan silinir. | Dize

### <a name="url-redirect"></a>URL yeniden yönlendirme

İstemcileri yeni bir URL'ye yönlendirmek için bu eylemi kullanın. 

#### <a name="required-fields"></a>Gerekli alanlar

Alan | Açıklama 
------|------------
Tür | İstekçiye dönmek için yanıt türünü seçin: Bulundu (302), Moved (301), Temporary redirect (307) ve Kalıcı yeniden yönlendirme (308).
Protokol | Maç İsteği, HTTP, HTTPS.
Ana Bilgisayar Adı | İsteğin yeniden yönlendirilmesini istediğiniz ana bilgisayar adını seçin. Gelen ana bilgisayarı korumak için boş bırakın.
Yol | Yönlendirmede kullanılacak yolu tanımlayın. Gelen yolu korumak için boş bırakın.  
Sorgu dizesi | Yönlendirmede kullanılan sorgu dizesini tanımlayın. Gelen sorgu dizesini korumak için boş bırakın. 
Parça | Yeniden yönlendirmede kullanılacak parçayı tanımlayın. Gelen parçayı korumak için boş bırakın. 

Mutlak bir URL kullanmanızı şiddetle öneririz. Göreli bir URL kullanmak Azure CDN URL'lerini geçersiz bir yola yönlendirebilir. 

### <a name="url-rewrite"></a>URL yeniden yazma

Kaynağınıza giden bir isteğin yolunu yeniden yazmak için bu eylemi kullanın.

#### <a name="required-fields"></a>Gerekli alanlar

Alan | Açıklama 
------|------------
Kaynak deseni | Değiştirmek için URL yolunda kaynak deseni tanımlayın. Şu anda, kaynak deseni önek tabanlı bir eşleşme kullanır. Tüm URL yollarını eşleştirmek için**/** kaynak desen değeri olarak ileri eğik çizgi ( ) kullanın.
Hedef | Yeniden yazmada kullanılacak hedef yolu tanımlayın. Hedef yol kaynak deseninin üzerine yazar.
Eşleşmemiş yolu koru | **Evet**olarak ayarlanırsa, kaynak desenden sonra kalan yol yeni hedef yola eklenir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure CDN'ye genel bakış](cdn-overview.md)
- [Standart kural altyapısı başvurusu](cdn-standard-rules-engine-reference.md)
- [Standart kurallar motorundaki koşulları eşleştirme](cdn-standard-rules-engine-match-conditions.md)
- [Standart kural altyapısını kullanarak HTTPS'yi zorlama](cdn-standard-rules-engine.md)
