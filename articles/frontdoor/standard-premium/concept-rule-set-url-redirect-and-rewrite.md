---
title: Azure ön kapı standardı/Premium ile URL yeniden yönlendirme ve URL yeniden yazma (Önizleme)
description: Bu makale Azure ön kapısının URL yeniden yönlendirmeyi ve Azure ön kapı kural kümesini kullanarak URL yeniden yazmayı nasıl desteklediğini anlamanıza yardımcı olur.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: article
ms.date: 02/18/2021
ms.author: yuajia
ms.openlocfilehash: 382a4c040c7a519462ee3e35119b9471031e0724
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101100502"
---
# <a name="url-redirect-and-url-rewrite-with-azure-front-door-standardpremium-preview"></a>Azure ön kapı standardı/Premium ile URL yeniden yönlendirme ve URL yeniden yazma (Önizleme)

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

Bu makale, Azure ön kapısının Standart/Premium 'un bir kural kümesinde kullanılan URL yeniden yönlendirmeyi ve URL yeniden yazmayı nasıl desteklediğini anlamanıza yardımcı olur.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="url-redirect"></a>URL yeniden yönlendirme

Azure ön kapısı şu düzeylerin her birinde trafiği yeniden yönlendirebilir: protokol, konak adı, yol, sorgu dizesi ve parça. Bu işlevler, yeniden yönlendirme yol tabanlı olduğundan tek bir mikro hizmet için yapılandırılabilir. URL yönlendirme ile, kaynak kullanımını iyileştirerek uygulama yapılandırmasını basitleştirebilir ve genel ve yol tabanlı yeniden yönlendirme dahil yeni yeniden yönlendirme senaryolarını destekler.

URL yeniden yönlendirmeyi, kural kümesi aracılığıyla yapılandırabilirsiniz.

:::image type="content" source="../media/concept-url-redirect-and-rewrite/front-door-url-redirect.png" alt-text="Kural kümesi ile URL yeniden yönlendirmesi oluşturma ekranının ekran görüntüsü." lightbox="../media/concept-url-redirect-and-rewrite/front-door-url-redirect-expanded.png":::

### <a name="redirection-types"></a>Yeniden yönlendirme türleri
Yeniden yönlendirme türü, istemcilerin yeniden yönlendirmenin amacını anlaması için yanıt durum kodunu ayarlar. Aşağıdaki yeniden yönlendirme türleri desteklenir:

* **301 (kalıcı olarak taşındı)**: hedef kaynağa yeni BIR kalıcı URI atandığını gösterir. Bu kaynağa gelecek tüm başvurular, eklenen URI 'lerden birini kullanacaktır. HTTP-HTTPS yönlendirmesi için 301 durum kodu kullanın.
* **302 (bulunan)**: hedef kaynağın geçici olarak farklı bir URI altında olduğunu gösterir. Yeniden yönlendirme bazen değişebileceğinizden, istemci gelecekteki istekler için geçerli istek URI 'sini kullanmaya devam etmelidir.
* **307 (geçici yeniden yönlendirme)**: hedef kaynağın geçici olarak farklı bir URI altında olduğunu gösterir. Kullanıcı Aracısı, bu URI 'ye otomatik yeniden yönlendirme yaparsanız istek yöntemini değiştirmemelidir. Yeniden yönlendirme zaman içinde değişebileceğinizden, istemci, gelecekteki istekler için özgün etkin istek URI 'sini kullanmaya devam etmeyi uygun şekilde gösterir.
* **308 (kalıcı yeniden yönlendirme)**: hedef kaynağa yeni BIR kalıcı URI atandığını gösterir. Bu kaynağa gelecek tüm başvurular, iliştirilmiş URI 'lerden birini kullanmalıdır.

### <a name="redirection-protocol"></a>Yeniden yönlendirme protokolü
Yönlendirme için kullanılacak protokolü ayarlayabilirsiniz. Yeniden yönlendirme özelliğinin en yaygın kullanım durumları HTTP olarak HTTPS yönlendirmesi olarak ayarlanmıştır.

* **Yalnızca https**: trafiği http 'den https 'ye yönlendirmek istiyorsanız, protokolü yalnızca https olarak ayarlayın. Azure ön kapısının her zaman yeniden yönlendirmeyi yalnızca HTTPS olarak ayarlamanız önerilir.
* **Yalnızca http**: gelen isteği http 'ye yönlendirir. Bu değeri yalnızca, trafik HTTP 'nizi, şifreli olmayan HTTP 'yi tutmak istiyorsanız kullanın.
* **Eşleştirme isteği**: Bu seçenek, gelen istek tarafından kullanılan protokolü korur. Bu nedenle, HTTP isteği HTTP olarak kalır ve HTTPS isteği HTTPS Post yeniden yönlendirmesi kalır.

### <a name="destination-host"></a>Hedef konak
Yeniden yönlendirme yönlendirmesi yapılandırmanın bir parçası olarak, yeniden yönlendirme isteği için ana bilgisayar adı veya etki alanını da değiştirebilirsiniz. Bu alanı, yeniden yönlendirmenin URL 'sindeki ana bilgisayar adını değiştirmek ya da başka bir şekilde gelen istekten ana bilgisayar adını korumak için ayarlayabilirsiniz. Bu nedenle, bu alanı kullanarak üzerinde gönderilen tüm istekleri öğesine yeniden `https://www.contoso.com/*` yönlendirebilirsiniz `https://www.fabrikam.com/*` .

### <a name="destination-path"></a>Hedef yol
Yeniden yönlendirmenin bir parçası olarak bir URL 'nin yol segmentini değiştirmek istediğiniz durumlarda, bu alanı yeni yol değeri ile ayarlayabilirsiniz. Aksi takdirde, yol değerini yeniden yönlendirmenin bir parçası olarak korumayı seçebilirsiniz. Bu nedenle, bu alanı kullanarak öğesine gönderilen tüm istekleri öğesine yeniden yönlendirebilirsiniz `https://www.contoso.com/\*`  `https://www.contoso.com/redirected-site` .

### <a name="query-string-parameters"></a>Sorgu dizesi parametreleri
Ayrıca, yeniden yönlendirilen URL 'deki sorgu dizesi parametrelerini de değiştirebilirsiniz. Gelen istek URL 'sindeki mevcut Sorgu dizelerini değiştirmek için, bu alanı ' replace ' olarak ayarlayın ve uygun değeri ayarlayın. Aksi halde, alanı ' preserve ' olarak ayarlayarak özgün sorgu dizeleri kümesini tutabilirsiniz. Örnek olarak, bu alanı kullanarak öğesine gönderilen tüm trafiği öğesine yeniden yönlendirebilirsiniz `https://www.contoso.com/foo/bar` `https://www.contoso.com/foo/bar?&utm_referrer=https%3A%2F%2Fwww.bing.com%2F` . 

### <a name="destination-fragment"></a>Hedef parça
Hedef parça, Web sayfasının belirli bir bölümüne gitmek için tarayıcı tarafından kullanılan ' # ' öğesinden sonra URL 'nin bölümüdür. Bu alanı yeniden yönlendirme URL 'sine bir parça eklemek için ayarlayabilirsiniz.

## <a name="url-rewrite"></a>URL yeniden yazma

Azure ön kapısı, kaynağına yönlendiren bir isteğin yolunu yeniden yazmak için URL yeniden yazmayı destekler. URL yeniden yazma, URL 'nin veya belirtilen üstbilgilerin yalnızca belirli koşullar karşılandığında yeniden yazılabilir olmasını sağlamak için koşullar eklemenize olanak tanır. Bu koşullar istek ve yanıt bilgilerine dayanır.

Bu özellikle, kullanıcıları senaryoya, cihaz türüne ve istenen dosya türüne göre farklı kaynaklarına yönlendirebilirsiniz.

URL yeniden yönlendirmeyi, kural kümesi aracılığıyla yapılandırabilirsiniz.

:::image type="content" source="../media/concept-url-redirect-and-rewrite/front-door-url-rewrite.png" alt-text="Kural kümesi ile URL yeniden yazma oluşturma ekranının ekran görüntüsü." lightbox="../media/concept-url-redirect-and-rewrite/front-door-url-rewrite-expanded.png":::

### <a name="source-pattern"></a>Kaynak stili

Kaynak stili, değiştirilecek kaynak istekteki URL yoludur. Şu anda, kaynak stili önek tabanlı eşleşme kullanıyor. Tüm URL yollarını eşleştirmek için, kaynak model değeri olarak bir eğik çizgi (/) kullanın.

### <a name="destination"></a>Hedef

Yeniden yazma sırasında kullanılacak hedef yolunu tanımlayabilirsiniz. Hedef yol, kaynak deseninin üzerine yazar.

### <a name="preserve-unmatched-path"></a>Eşleşmeyen yolu koru

Eşleşmeyen yolu koru, kaynak düzeninden sonra kalan yolu yeni yola eklemenizi sağlar.

Örneğin, **eşleşmeyen yolu Evet olarak** ayarla ' yı ayarlarsanız.
* Gelen istek ise, `www.contoso.com/sub/1.jpg` kaynak model olarak ayarlanır `/` , hedef al, olarak ayarlanır `/foo/` ve içerik `/foo/sub/1` . jpg kaynağından kaynaktan sunulur.

* Gelen istek ise, `www.contoso.com/sub/image/1.jpg` kaynak model olarak ayarlanır `/sub/` , hedef al olarak ayarlanır `/foo/` , içerik kaynağından alınır `/foo/image/1.jpg` .

Örneğin, **eşleşmeyen bir yolu Hayır olarak** ayarla ' yı ayarlarsanız.
* Gelen istek ise, `www.contoso.com/sub/image/1.jpg` kaynak model olarak ayarlanır `/sub/` , hedef Get olarak ayarlanır `/foo/2.jpg` , içerik her zaman `/foo/2.jpg` hangi yolların izlendiğine bakılmaksızın `wwww.contoso.com/sub/` kaynaktan alınır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure ön kapısının Standart/Premium kural kümesi](concept-rule-set.md)hakkında daha fazla bilgi edinin.
