---
title: Azure Application Gateway istek yönlendirme kuralları yapılandırması
description: Bu makalede, Azure Application Gateway isteği yönlendirme kurallarının nasıl yapılandırılacağı açıklanır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: 20a665eefbb73f062f1f036e17b16da891a43eef
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653213"
---
# <a name="application-gateway-request-routing-rules"></a>Application Gateway isteği yönlendirme kuralları

Azure portal kullanarak bir uygulama ağ geçidi oluşturduğunuzda, varsayılan bir kural (*rule1*) oluşturursunuz. Bu kural varsayılan dinleyiciyi (*Appgatewayhttplistener*) varsayılan arka uç Havuzu (*appgatewaybackendpool*) ve varsayılan arka uç http ayarları (*Appgatewaybackendhttpsettings*) ile bağlar. Ağ geçidini oluşturduktan sonra, varsayılan kuralın ayarlarını düzenleyebilir veya yeni kurallar oluşturabilirsiniz.

## <a name="rule-type"></a>Kural türü

Bir kural oluşturduğunuzda [ *temel* ve *yol tabanlı*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rules)arasında seçim yapabilirsiniz.

- İlişkili dinleyicide (örneğin, *Blog <i></i> . contoso.com/ \* )* tüm istekleri tek bir arka uç havuzuna iletmek istiyorsanız temel ' yı seçin.
- Belirli URL yollarındaki istekleri belirli arka uç havuzlarına yönlendirmek istiyorsanız yol tabanlı ' i seçin. Yol deseninin Sorgu parametrelerine değil yalnızca URL 'nin yoluna uygulanması.

### <a name="order-of-processing-rules"></a>İşleme kuralları sırası

V1 ve v2 SKU 'SU için, gelen isteklerin düzen eşleştirmesi, yolların yol tabanlı kuralın URL yol haritasında listelendiği sırada işlenir. Bir istek, yol eşlemesindeki iki veya daha fazla yoldaki Düzenle eşleşiyorsa, önce listelenen yol eşleştirilir. Ve istek bu yol ile ilişkili arka uca iletilir.

## <a name="associated-listener"></a>İlişkili dinleyici

Dinleyiciyle ilişkilendirilen *istek yönlendirme kuralı* , isteği yönlendiren arka uç havuzunu belirleyecek şekilde değerlendirilmek için bir dinleyiciyi kuralla ilişkilendirin.

## <a name="associated-back-end-pool"></a>İlişkili arka uç havuzu

Dinleyicinin aldığı isteklere sunan arka uç hedeflerini içeren arka uç havuzunun bulunduğu kuralla ilişkilendirin.

 - Temel bir kural için yalnızca bir arka uç havuzuna izin verilir. İlişkili dinleyicide bulunan tüm istekler bu arka uç havuzuna iletilir.

 - Yol tabanlı bir kural için, her bir URL yoluna karşılık gelen birden fazla arka uç havuzu ekleyin. Girilen URL yoluyla eşleşen istekler karşılık gelen arka uç havuzuna iletilir. Ayrıca, bir varsayılan arka uç havuzu ekleyin. Kuraldaki hiçbir URL yoluyla eşleşmeyen istekler bu havuza iletilir.

## <a name="associated-back-end-http-setting"></a>İlişkili arka uç HTTP ayarı

Her kural için bir arka uç HTTP ayarı ekleyin. İstekler, bu ayarda belirtilen bağlantı noktası numarası, protokol ve diğer bilgileri kullanarak uygulama ağ geçidinden arka uç hedeflerine yönlendirilir.

Temel bir kural için yalnızca bir arka uç HTTP ayarına izin verilir. İlişkili dinleyicide bulunan tüm istekler, bu HTTP ayarı kullanılarak karşılık gelen arka uç hedeflerine iletilir.

Yol tabanlı bir kural için, her bir URL yoluna karşılık gelen birden fazla arka uç HTTP ayarı ekleyin. Bu ayarda URL yoluyla eşleşen istekler, her bir URL yoluna karşılık gelen HTTP ayarları kullanılarak ilgili arka uç hedeflerine iletilir. Ayrıca, varsayılan bir HTTP ayarı ekleyin. Bu kuraldaki hiçbir URL yoluyla eşleşmeyen istekler varsayılan HTTP ayarı kullanılarak varsayılan arka uç havuzuna iletilir.

## <a name="redirection-setting"></a>Yeniden yönlendirme ayarı

Bir temel kural için yeniden yönlendirme yapılandırılırsa, ilişkili dinleyicinin tüm istekleri hedefe yeniden yönlendirilir. Bu, *genel* yeniden yönlendirme 'dir. Yol tabanlı bir kural için yeniden yönlendirme yapılandırılırsa, yalnızca belirli bir site alanındaki istekler yeniden yönlendirilir. Örnek, */cart/ \* *tarafından belirtilen bir alışveriş sepeti alanıdır. Bu, *yol tabanlı* yeniden yönlendirme 'dir.

Yeniden yönlendirmeler hakkında daha fazla bilgi için bkz. [Application Gateway yönlendirmeye genel bakış](redirect-overview.md).

### <a name="redirection-type"></a>Yeniden yönlendirme türü

Gereken yeniden yönlendirme türünü seçin: *kalıcı (301)*, *geçici (.)*, *bulunan (302*) veya *diğer (303*).

### <a name="redirection-target"></a>Yeniden yönlendirme hedefi

Yeniden yönlendirme hedefi olarak başka bir dinleyici veya dış site seçin.

#### <a name="listener"></a>Dinleyici

Trafiği bir dinleyicisinden ağ geçidinde diğerine yeniden yönlendirmek için yeniden yönlendirme hedefi olarak dinleyici ' i seçin. HTTP-HTTPS yeniden yönlendirmeyi etkinleştirmek istediğinizde bu ayar gereklidir. Gelen HTTP isteklerini, gelen HTTPS isteklerini denetleyen hedef dinleyiciye denetleyen kaynak dinleyicisinden trafiği yeniden yönlendirir. Ayrıca, yeniden yönlendirme hedefine iletilen istekteki özgün istekten sorgu dizesini ve yolunu eklemeyi de tercih edebilirsiniz.

![Application Gateway bileşenleri iletişim kutusu](./media/configuration-overview/configure-redirection.png)

HTTP-HTTPS yeniden yönlendirmesi hakkında daha fazla bilgi için bkz.:
- [Azure portal kullanarak HTTP-HTTPS yönlendirmesi](redirect-http-to-https-portal.md)
- [PowerShell kullanarak HTTP-to-HTTPS yönlendirmesi](redirect-http-to-https-powershell.md)
- [Azure CLı kullanarak HTTP-HTTPS yönlendirmesi](redirect-http-to-https-cli.md)

#### <a name="external-site"></a>Dış site

Bu kuralla ilişkilendirilen dinleyicide trafiği bir dış siteye yönlendirmek istediğinizde dış site ' yi seçin. Sorgu dizesini, yeniden yönlendirme hedefine iletilen istekteki özgün istekten eklemeyi seçebilirsiniz. Yolu özgün istekte bulunan dış siteye iletmeyin.

Yeniden yönlendirme hakkında daha fazla bilgi için bkz.
- [PowerShell kullanarak trafiği dış siteye yönlendirme](redirect-external-site-powershell.md)
- [CLı kullanarak trafiği dış siteye yönlendirme](redirect-external-site-cli.md)

## <a name="rewrite-http-headers-and-url"></a>HTTP üst bilgilerini ve URL’sini yeniden yazma

Yeniden yazma kurallarını kullanarak, HTTP (S) isteği ve yanıt üst bilgilerini ekleyebilir, kaldırabilir veya güncelleştirebilir, istek ve yanıt paketleri uygulama ağ geçidi aracılığıyla istemci ile arka uç havuzları arasında hareket edebilir.

Üst bilgiler ve URL parametreleri statik değerlere veya diğer üst bilgilere ve sunucu değişkenlerine ayarlanabilir. Bu, istemci IP adreslerini ayıklama, arka uç hakkındaki hassas bilgileri kaldırma, daha fazla güvenlik ekleme vb. gibi önemli kullanım durumlarının sağlanmasına yardımcı olur.
Daha fazla bilgi için bkz.

 - [HTTP üstbilgilerini ve URL genel bakış 'ı yeniden yazma](rewrite-http-headers-url.md)
 - [HTTP üstbilgisini yeniden yazmayı yapılandırma](rewrite-http-headers-portal.md)
 - [URL yeniden yazmayı yapılandırma](rewrite-url-portal.md)

## <a name="next-steps"></a>Sonraki adımlar

- [HTTP ayarları hakkında bilgi edinin](configuration-http-settings.md)