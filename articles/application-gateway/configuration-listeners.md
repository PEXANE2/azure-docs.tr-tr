---
title: Azure Application Gateway dinleyicisi yapılandırması
description: Bu makalede Azure Application Gateway dinleyicilerinin nasıl yapılandırılacağı açıklanır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 09/09/2020
ms.author: surmb
ms.openlocfilehash: ef2ff8924cd8a92c5d2d2e5dd9da6bb74fad1a14
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653227"
---
# <a name="application-gateway-listener-configuration"></a>Application Gateway dinleyicisi yapılandırması

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Dinleyici, bağlantı noktası, protokol, konak ve IP adresini kullanarak gelen bağlantı isteklerini denetleyen mantıksal bir varlıktır. Dinleyiciyi yapılandırdığınızda, bunlar için ağ geçidinde gelen istekteki karşılık gelen değerlerle eşleşen değerler girmeniz gerekir.

Azure portal kullanarak bir uygulama ağ geçidi oluşturduğunuzda, dinleyici için protokolü ve bağlantı noktasını seçerek de varsayılan bir dinleyici oluşturursunuz. HTTP2 desteğinin dinleyicide etkinleştirilip etkinleştirilmeyeceğini seçebilirsiniz. Uygulama ağ geçidini oluşturduktan sonra, bu varsayılan dinleyicinin (*Appgatewayhttplistener*) ayarlarını düzenleyebilir veya yeni dinleyiciler oluşturabilirsiniz.

## <a name="listener-type"></a>Dinleyici türü

Yeni bir dinleyici oluşturduğunuzda [ *temel* ve *Çoklu site*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners)arasında seçim yapabilirsiniz.

- Tüm isteklerinizin (herhangi bir etki alanı için) kabul edilmesini ve arka uç havuzlara iletilmesini istiyorsanız temel ' yı seçin. [Temel dinleyiciyle bir uygulama ağ geçidi oluşturmayı](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)öğrenin.

- İstekleri *konak* üstbilgisine veya ana bilgisayar adlarına göre farklı arka uç havuzlarıyla iletmek istiyorsanız, Ayrıca, gelen istekle eşleşen bir ana bilgisayar adı belirtmeniz gereken çok siteli dinleyici ' i seçin. Bunun nedeni, Application Gateway aynı genel IP adresi ve bağlantı noktasında birden fazla Web sitesini barındırmak için HTTP 1,1 ana bilgisayar üst bilgilerini temel alır. Daha fazla bilgi için bkz. [Application Gateway kullanarak birden çok site barındırma](multiple-site-overview.md).

### <a name="order-of-processing-listeners"></a>İşleme dinleyicileri sırası

V1 SKU 'SU için, istekler kuralların sırasına ve dinleyici türüne göre eşleştirilir. Temel dinleyiciye sahip bir kural sırayla ilk kez geliyorsa, önce işlenir ve bu bağlantı noktası ve IP birleşimi için tüm istekleri kabul eder. Bunu önlemek için, kuralları çok siteli dinleyiciyle yapılandırın ve kuralı temel dinleyiciyle listenin en son bölümüne gönderin.

V2 SKU 'SU için, çoklu site dinleyicileri temel dinleyicilerine göre işlenir.

## <a name="front-end-ip-address"></a>Ön uç IP adresi

Bu dinleyiciyle ilişkilendirmeyi planladığınız ön uç IP adresini seçin. Dinleyici, bu IP üzerindeki gelen istekleri dinleyecektir.

## <a name="front-end-port"></a>Ön uç bağlantı noktası

Ön uç bağlantı noktasını seçin. Mevcut bir bağlantı noktasını seçin veya yeni bir bağlantı noktası oluşturun. [İzin verilen bağlantı noktası aralığından](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports)herhangi bir değer seçin. 80 ve 443 gibi yalnızca iyi bilinen bağlantı noktalarını, ancak uygun olan tüm özel bağlantı noktalarını kullanabilirsiniz. Bir bağlantı noktası, genel kullanıma yönelik dinleyiciler veya özel kullanıma yönelik dinleyiciler için kullanılabilir.

## <a name="protocol"></a>Protokol

HTTP veya HTTPS seçin:

- HTTP ' yi seçerseniz, istemci ve uygulama ağ geçidi arasındaki trafik şifrelenmemiş olur.

- [TLS sonlandırmasını](features.md#secure-sockets-layer-ssltls-termination) veya [uçtan uca TLS şifrelemesini](https://docs.microsoft.com/azure/application-gateway/ssl-overview)istiyorsanız https 'yi seçin. İstemci ile uygulama ağ geçidi arasındaki trafik şifrelenir. Ve TLS bağlantısı uygulama ağ geçidinde sonlanır. Uçtan uca TLS şifrelemeyi istiyorsanız, HTTPS 'yi seçmeniz ve **arka uç http** ayarını yapılandırmanız gerekir. Bu, uygulama ağ geçidinden arka uca seyahat edildiğinde trafiğin yeniden şifrelenmesini sağlar.


TLS sonlandırmasını ve uçtan uca TLS şifrelemesini yapılandırmak için, uygulama ağ geçidinin bir simetrik anahtar türetmesini sağlamak üzere dinleyiciye bir sertifika eklemeniz gerekir. Bu, TLS protokol belirtimi tarafından belirlenir. Simetrik anahtar, ağ geçidine gönderilen trafiği şifrelemek ve şifresini çözmek için kullanılır. Ağ Geçidi sertifikası kişisel bilgi değişimi (PFX) biçiminde olmalıdır. Bu biçim, ağ geçidinin trafiği şifrelemek ve şifresini çözmek için kullandığı özel anahtarı dışa aktarmanıza olanak tanır.

## <a name="supported-certificates"></a>Desteklenen sertifikalar

Bkz. [TLS sonlandırmasına genel bakış ve Application Gateway ile uçtan uca TLS](ssl-overview.md#certificates-supported-for-tls-termination)

## <a name="additional-protocol-support"></a>Ek protokol desteği

### <a name="http2-support"></a>HTTP2 desteği

HTTP/2 protokol desteği yalnızca uygulama ağ geçidi dinleyicilerine bağlanan istemciler tarafından kullanılabilir. Arka uç sunucu havuzlarıyla iletişim HTTP/1.1 üzerinden yapılır. HTTP/2 desteği varsayılan olarak devre dışıdır. Aşağıdaki Azure PowerShell kod parçacığı bunun nasıl etkinleştirileceğini göstermektedir:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

### <a name="websocket-support"></a>WebSocket desteği

WebSocket desteği varsayılan olarak etkindir. Etkinleştirmek veya devre dışı bırakmak için Kullanıcı tarafından yapılandırılabilir bir ayar yoktur. WebSockets 'i hem HTTP hem de HTTPS dinleyicilerine sahip olarak kullanabilirsiniz.

## <a name="custom-error-pages"></a>Özel hata sayfaları

Genel düzeyde veya dinleyici düzeyinde özel hata tanımlayabilirsiniz. Ancak Azure portal genel düzey özel hata sayfaları oluşturma şu anda desteklenmiyor. Dinleyici düzeyinde 403 Web uygulaması güvenlik duvarı hatası veya 502 bakım sayfası için özel hata sayfası yapılandırabilirsiniz. Ayrıca, belirtilen hata durum kodu için genel olarak erişilebilir bir blob URL 'SI belirtmeniz gerekir. Daha fazla bilgi için bkz. [Application Gateway özel hata sayfası oluşturma](https://docs.microsoft.com/azure/application-gateway/custom-error).

![Application Gateway hata kodları](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Genel özel hata sayfasını yapılandırmak için, bkz. [Azure PowerShell Configuration](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration).

## <a name="tls-policy"></a>TLS ilkesi

Bir arka uç sunucu grubu için TLS/SSL sertifika yönetimini merkezileştirmek ve şifreleme şifre çözme ek yükünü azaltabilirsiniz. Merkezi TLS işleme, güvenlik gereksinimlerinize uygun bir merkezi TLS ilkesi belirtmenize de olanak tanır. *Varsayılan*, *önceden tanımlanmış*veya *özel* TLS ilkesi seçebilirsiniz.

TLS protokolü sürümlerini denetlemek için TLS ilkesi yapılandırırsınız. Bir uygulama ağ geçidini TLS 1.0, TLS 1.1 ve TLS 1.2 ile TLS el sıkışmaları için en düşük protokol sürümünü kullanacak şekilde yapılandırabilirsiniz. Varsayılan olarak, SSL 2,0 ve 3,0 devre dışıdır ve yapılandırılamaz. Daha fazla bilgi için bkz. [APPLICATION Gateway TLS ilkesine genel bakış](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Bir dinleyici oluşturduktan sonra, bunu bir istek yönlendirme kuralıyla ilişkilendirirsiniz. Bu kural, dinleyicide alınan isteklerin arka uca nasıl yönlendirildiğini belirler.

## <a name="next-steps"></a>Sonraki adımlar

- [İstek yönlendirme kuralları hakkında bilgi edinin](configuration-request-routing-rules.md).