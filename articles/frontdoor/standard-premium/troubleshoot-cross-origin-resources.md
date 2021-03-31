---
title: Azure ön kapısı Standart/Premium 'u, çıkış noktaları arası kaynak paylaşımıyla kullanma
description: Azure ön kapısını (AFD), çıkış noktaları arası kaynak paylaşımı (CORS) ile nasıl kullanacağınızı öğrenin.
services: frontdoor
author: duongau
ms.service: frontdoor
ms.topic: how-to
ms.date: 02/18/2021
ms.author: qixwang
ms.openlocfilehash: ee8f19aca62d2e331fcf59551d47c2dac93783b1
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101100361"
---
# <a name="using-azure-front-door-standardpremium-with-cross-origin-resource-sharing-cors"></a>Azure ön kapısı Standart/Premium 'u, çıkış noktaları arası kaynak paylaşımı (CORS) ile kullanma

> [!Note]
> Bu belge, Azure ön kapısının Standart/Premium (Önizleme) içindir. Azure ön kapısı hakkında bilgi mi arıyorsunuz? [Burada](../front-door-overview.md)görüntüleyin.

## <a name="what-is-cors"></a>CORS nedir?

CORS (Çıkış Noktaları Arası Kaynak Paylaşımı), bir etki alanında çalışan web uygulamasının başka bir etki alanındaki kaynaklara erişmesini sağlayan bir HTTP özelliğidir. Siteler arası komut dosyası saldırıları olasılığını azaltmak için, tüm modern web tarayıcıları [aynı kaynak ilkesi](https://www.w3.org/Security/wiki/Same_Origin_Policy)olarak bilinen bir güvenlik kısıtlaması uygular. Bu, bir Web sayfasının farklı bir etki alanındaki API 'Leri aramasını engeller.  CORS, farklı bir kaynaktan API 'Leri çağırmak için bir kaynağa (kaynak etki alanı) izin vermek için güvenli bir yol sağlar.

> [!IMPORTANT]
> Azure ön kapı Standart/Premium (Önizleme) Şu anda genel önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="how-it-works"></a>Nasıl çalışır?

İki tür CORS isteği, *basit istekler* ve *karmaşık istekler vardır.*

### <a name="for-simple-requests"></a>Basit istekler için:

1. Tarayıcı, CORS isteğini başka bir **kaynak** http istek üstbilgisiyle gönderir. Bu üstbilginin değeri, üst sayfayı sunan, *protokol,* *etki alanı* ve *bağlantı noktası* birleşimi olarak tanımlanan başlangıç noktasıdır.  Https//www.contoso.com 'deki bir sayfa, \: fabrikam.com kaynağından bir kullanıcının verilerine erişmeyi denediğinde, aşağıdaki istek üst bilgisi fabrikam.com 'e gönderilir:

   `Origin: https://www.contoso.com`

2. Sunucu, aşağıdakilerden biriyle yanıt verebilir:

   * Yanıtında bir **erişim-denetim-Izin verme-kaynak** üst bilgisi, hangi kaynak siteye izin verildiğini belirtir. Örnek:

     `Access-Control-Allow-Origin: https://www.contoso.com`

   * Sunucu, kaynak üstbilgisini denetledikten sonra çıkış noktaları arası isteğe izin vermezse 403 gibi bir HTTP hata kodu

   * Tüm kaynakları izin veren bir joker karakter içeren bir **erişim-denetim-izin-kaynak** üst bilgisi:

     `Access-Control-Allow-Origin: *`

### <a name="for-complex-requests"></a>Karmaşık istekler için:

Karmaşık bir istek, tarayıcının gerçek CORS isteğini göndermeden önce bir *ön kontrol isteği* (yani bir ön araştırma) göndermesi için gereken bir CORS isteğidir. Özgün CORS isteği devam edebiliyorsanız ve aynı URL 'ye bir istek ise, ön kontrol isteği sunucu iznini ister `OPTIONS` .

> [!TIP]
> CORS akışları ve genel bilgiler hakkında daha fazla bilgi için, [REST API 'leri IÇIN CORS Kılavuzu](https://www.moesif.com/blog/technical/cors/Authoritative-Guide-to-CORS-Cross-Origin-Resource-Sharing-for-REST-APIs/)' na bakın.
>
>

## <a name="wildcard-or-single-origin-scenarios"></a>Joker karakter veya tek kaynak senaryoları
Azure ön kapısına ilişkin CORS, **erişim-denetimi-izin-Origin** üstbilgisi joker (*) veya tek bir Origin olarak ayarlandığında ek yapılandırma olmadan otomatik olarak çalışacaktır.  Azure ön kapısının ilk yanıtı önbelleğe alacak ve isteklerin aynı üstbilgiyi kullanması gerekir.

Azure ön kapısına CORS 'nin kaynak üzerinde ayarlanmadan önce istekleri zaten yapılmışsa, içeriği **Access-Control-Allow-Origin** üstbilgisiyle yeniden yüklemek için uç nokta içeriklerindeki içeriği temizlemeniz gerekir.

## <a name="multiple-origin-scenarios"></a>Birden çok kaynak senaryosu
CORS için belirli bir çıkış listesine izin verilmesi gerekiyorsa, bu şeyler biraz daha karmaşıktır. Bu sorun, CDN ilk CORS kaynağı için **Access-Control-Allow-Origin** üst bilgisini önbelleğe alırken oluşur.  Farklı bir CORS kaynağı başka bir istek yaptığında CDN, önbelleğe alınan **Access-Control-Allow-Origin** üst bilgisine sahip olur ve bu eşleşmez. Bu sorunu düzeltmek için birkaç yol vardır.

### <a name="azure-front-door-rule-set"></a>Azure ön kapı kural kümesi

Azure ön kapılarında, istekteki **kaynak** üst bilgisini denetlemek Için Azure ön kapı [kuralları](concept-rule-set.md) ' nda bir kural oluşturabilirsiniz. Geçerli bir Origin ise, kuralınız, **erişim-Control-Allow-Origin** üst bilgisini doğru değere ayarlar. Bu durumda, dosyanın kaynak sunucusundan **erişim-denetim-izin-kaynak** üst bilgisi yok SAYıLıR ve AFD 'ın Rules altyapısı ızın verilen CORS kaynakları 'nı tamamen yönetir.

:::image type="content" source="../media/troubleshooting-cross-origin-resource-sharing/cross-origin-resource.png" alt-text="Kural kümesine sahip kurallar örneği ekran görüntüsü.":::

> [!TIP]
> **Erişim-denetimi-Izin verme yöntemleri** gibi ek yanıt üstbilgilerini değiştirmek için kuralınıza ek eylemler ekleyebilirsiniz.
> 

## <a name="next-steps"></a>Sonraki adımlar

* [Front Door oluşturmayı](create-front-door-portal.md) öğrenin.
