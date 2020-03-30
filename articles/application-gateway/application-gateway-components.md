---
title: Uygulama ağ geçidi bileşenleri
description: Bu makalede, bir uygulama ağ geçidindeki çeşitli bileşenler hakkında bilgi sağlar
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/20/2019
ms.author: absha
ms.openlocfilehash: 90b3c3fd18bc9211c731ccf16dd646a64a4a1116
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80133104"
---
# <a name="application-gateway-components"></a>Uygulama ağ geçidi bileşenleri

 Uygulama ağ geçidi, istemciler için tek bir iletişim noktası olarak hizmet vermektedir. Gelen uygulama trafiğini Azure Sanal Setleri, sanal makine ölçek kümeleri, Azure Uygulama Hizmeti ve şirket içi/harici sunucular gibi birden çok arka uç havuzuna dağıtır. Trafiği dağıtmak için, bir uygulama ağ geçidi bu makalede açıklanan birkaç bileşen kullanır.

![Bir uygulama ağ geçidinde kullanılan bileşenler](./media/application-gateway-components/application-gateway-components.png)

## <a name="frontend-ip-addresses"></a>Frontend IP adresleri

Ön uç IP adresi, bir uygulama ağ geçidiyle ilişkili IP adresidir. Bir uygulama ağ geçidini ortak bir IP adresine, özel bir IP adresine veya her ikisine sahip olacak şekilde yapılandırabilirsiniz. Uygulama ağ geçidi, bir ortak veya bir özel IP adresini destekler. Sanal ağınız ve genel IP adresiniz uygulama ağ geçidinizle aynı konumda olmalıdır. Oluşturulduktan sonra, bir ön uç IP adresi bir dinleyiciyle ilişkilendirilir.

### <a name="static-versus-dynamic-public-ip-address"></a>Statik karşı dinamik genel IP adresi

Azure Application Gateway V2 SKU, hem statik iç IP adresini hem de statik genel genel IP adresini veya yalnızca statik genel IP adresini destekleyecek şekilde yapılandırılabilir. Yalnızca statik iç IP adresini destekleyecek şekilde yapılandırılamaz.

V1 SKU statik veya dinamik dahili IP adresini ve dinamik genel IP adresini destekleyecek şekilde yapılandırılabilir. Uygulama Ağ Geçidi'nin dinamik IP adresi çalışan bir ağ geçidinde değişmez. Yalnızca Ağ Geçidi'ni durdurup başlattığınızda değişebilir. Sistem hatalarında, güncelleştirmelerde, Azure ana bilgisayar güncelleştirmelerinde vb. değişiklik yapmaz. 

Bir uygulama ağ geçidiyle ilişkili DNS adı ağ geçidinin yaşam döngüsü nde değişmez. Sonuç olarak, bir CNAME diğer adı kullanmalı ve uygulama ağ geçidinin DNS adresine yönlendirmelisiniz.

## <a name="listeners"></a>Dinleyiciler

Dinleyici, gelen bağlantı isteklerini denetleyen mantıksal bir varlıktır. İstekle ilişkili iletişim noktası, bağlantı noktası, ana bilgisayar adı ve IP adresi dinleyici yapılandırmasıyla ilişkili öğelerle eşleşirse, dinleyici isteği kabul eder.

Bir uygulama ağ geçidi kullanmadan önce en az bir dinleyici eklemeniz gerekir. Bir uygulama ağ geçidine bağlı birden çok dinleyici olabilir ve bunlar aynı protokol için kullanılabilir.

Dinleyici istemcilerden gelen istekleri algıladıktan sonra, uygulama ağ geçidi bu istekleri kuralda yapılandırılan arka uç havuzundaki üyelere yönlendirir.

Dinleyiciler aşağıdaki bağlantı noktalarını ve protokolleri destekler.

### <a name="ports"></a>Bağlantı Noktaları

Bağlantı noktası, dinleyicinin istemci isteğini dinlediği yerdir. V1 SKU için 1 ile 65502, v2 SKU için 1 ila 65199 arasında değişen bağlantı noktalarını yapılandırabilirsiniz.

### <a name="protocols"></a>Protokoller

Uygulama Ağ Geçidi dört protokolü destekler: HTTP, HTTPS, HTTP/2 ve WebSocket:
>[!NOTE]
>HTTP/2 protokol desteği yalnızca uygulama ağ geçidi dinleyicilerine bağlanan istemciler için kullanılabilir. Arka uç sunucu havuzlarına iletişim her zaman HTTP/1.1'in üzerindedir. Varsayılan olarak, HTTP/2 desteği devre dışı bırakılır. Etkinleştirmeyi seçebilirsiniz.

- Dinleyici yapılandırmasında HTTP ve HTTPS protokolleri arasında belirtin.
- [WebSockets ve HTTP/2 protokolleri](features.md#websocket-and-http2-traffic) için destek yerel olarak sağlanır ve [WebSocket desteği](application-gateway-websocket.md) varsayılan olarak etkinleştirilir. WebSocket desteğini isteğe bağlı olarak etkinleştirmek veya devre dışı bırakmak için kullanıcı tarafından yapılandırılabilen bir ayar yoktur. Hem HTTP hem de HTTPS dinleyicileriyle WebSockets'i kullanın.

TLS sonlandırma için bir HTTPS dinleyicisi kullanın. Bir HTTPS dinleyicisi şifreleme ve şifre çözme çalışmalarını uygulama ağ geçidinize boşaltır, böylece web sunucularınız genel merkezden etkilenmez.

### <a name="custom-error-pages"></a>Özel hata sayfaları

Uygulama Ağ Geçidi, varsayılan hata sayfalarını görüntülemek yerine özel hata sayfaları oluşturmanıza olanak tanır. Özel hata sayfası sayesinde kendi logonuzu ve sayfa düzeninizi kullanabilirsiniz. Uygulama Ağ Geçidi, istek arka uca ulaşadığında özel bir hata sayfası görüntüler.

Daha fazla bilgi [için, uygulama ağ geçidiniz için Özel hata sayfalarına](custom-error.md)bakın.

### <a name="types-of-listeners"></a>Dinleyici türleri

İki tür dinleyici vardır:

- **Temel**. Bu tür dinleyici, uygulama ağ geçidinin IP adresine tek bir DNS eşleciliği olan tek bir etki alanı sitesini dinler. Bu dinleyici yapılandırması, bir uygulama ağ geçidinin arkasında tek bir site barındırdığınızda gereklidir.

- **Çok siteli**. Bu dinleyici yapılandırması, aynı uygulama ağ geçidi örneğinde birden fazla web uygulamasını yapılandırırken gereklidir. Bir uygulama ağ geçidine en fazla 100 web sitesi ekleyerek dağıtımlarınız için daha verimli bir topoloji yapılandırmanıza olanak tanır. Her web sitesi, kendi arka uç havuzuna yönlendirilebilir. Örneğin, abc.contoso.com, xyz.contoso.com ve pqr.contoso.com olmak üzere üç alt etki alanı, uygulama ağ geçidinin IP adresini işaret eder. Üç çok siteli dinleyici oluşturur ve her dinleyiciyi ilgili bağlantı noktası ve protokol ayarı için yapılandırabilirsiniz.

    Daha fazla bilgi için [Çoklu site barındırma'ya](application-gateway-web-app-overview.md)bakın.

Bir dinleyici oluşturduktan sonra, bunu bir istek yönlendirme kuralıyla ilişkilendirin. Bu kural, dinleyiciden alınan isteğin arka uca nasıl yönlendirilmesi gerektiğini belirler.

Uygulama Ağ Geçidi, dinleyicileri [gösterilen sırada](configuration-overview.md#order-of-processing-listeners)işler.

## <a name="request-routing-rules"></a>Yönlendirme kuralları isteme

İstek yönlendirme kuralı, dinleyiciüzerindeki trafiğin nasıl yönlendirilenin ibelirleyen bir uygulama ağ geçidinin önemli bir bileşenidir. Kural dinleyiciyi, arka uç sunucu havuzunu ve arka uç HTTP ayarlarını bağlar.

Bir dinleyici bir isteği kabul ettiğinde, istek yönlendirme kuralı isteği arka uca iletir veya başka bir yere yönlendirir. İstek arka uca iletilirse, istek yönlendirme kuralı, hangi arka uç sunucu havuzuna iletilmesi gerektiğini tanımlar. İstek yönlendirme kuralı, istekteki üstbilginin yeniden yazılıp yazılmayacağına da karar vetir. Bir dinleyici bir kurala eklenebilir.

İki tür istek yönlendirme kuralı vardır:

- **Temel**. İlişkili dinleyicideki tüm istekler (örneğin, blog.contoso.com/*) ilişkili HTTP ayarı kullanılarak ilişkili arka uç havuzuna iletilir.

- **Yol tabanlı.** Bu yönlendirme kuralı, ilişkili dinleyicideki istekleri istekteki URL'ye göre belirli bir arka uç havuzuna yönlendirmenize olanak tanır. İstekteki URL'nin yolu yol tabanlı bir kuraldaki yol deseniyle eşleşiyorsa, kural isteği yönlendirir. Yol deseni yalnızca URL yoluna uygular, sorgu parametrelerine değil. Dinleyici isteğindeki URL yolu yol tabanlı kuralların hiçbirinde eşleşmiyorsa, isteği varsayılan arka uç havuzuna ve HTTP ayarlarına yönlendirir.

Daha fazla bilgi için [URL tabanlı yönlendirmeye](url-route-overview.md)bakın.

### <a name="redirection-support"></a>Yeniden yönlendirme desteği

İstek yönlendirme kuralı, uygulama ağ geçidindeki trafiği yeniden yönlendirmenize de olanak tanır. Bu genel bir yeniden yönlendirme mekanizmasıdır, böylece kuralları kullanarak tanımladığınız herhangi bir bağlantı noktasına yönlendirebilirsiniz.

Yeniden yönlendirme hedefini başka bir dinleyici (otomatik HTTP'den HTTPS yeniden yönlendirmesine etkinleştirmeye yardımcı olabilir) veya harici bir site olarak seçebilirsiniz. Ayrıca yeniden yönlendirmenin geçici veya kalıcı olmasını veya URI yolunu ve sorgu dizesini yeniden yönlendirilen URL'ye eklemeyi de seçebilirsiniz.

Daha fazla bilgi için, [uygulama ağ geçidinizdeki Trafiği Yeniden Yönlendir'e](redirect-overview.md)bakın.

### <a name="rewrite-http-headers"></a>HTTP üst bilgilerini yeniden üretme

İstek yönlendirme kurallarını kullanarak, istek ve yanıt paketleri uygulama ağ geçidi üzerinden istemci ve arka uç havuzları arasında hareket ettikçe HTTP(S) istek ve yanıt üstbilgilerini ekleyebilir, kaldırabilir veya güncelleyebilirsiniz.

Üstbilgi statik değerlere veya diğer üstbilgi ve sunucu değişkenlerine ayarlanabilir. Bu, istemci IP adreslerini ayıklama, arka uç la ilgili hassas bilgileri kaldırma, daha fazla güvenlik ekleme gibi önemli kullanım durumlarına yardımcı olur.

Daha fazla bilgi için, [uygulama ağ geçidinizde HTTP üstbilgilerini yeniden yazın'](rewrite-http-headers.md)a bakın.

## <a name="http-settings"></a>HTTP ayarları

Uygulama ağ geçidi, bu bileşende ayrıntılı olan bağlantı noktası numarasını, protokolü ve diğer ayarları kullanarak trafiği arka uç sunucularına (HTTP ayarlarını içeren istek yönlendirme kuralında belirtilir) yönlendirir.

HTTP ayarlarında kullanılan bağlantı noktası ve protokolü, uygulama ağ geçidi ve arka uç sunucuları arasındaki trafiğin şifrelenip şifrelenmediğini (uçlardan uca TLS sağlayıp sağlamadığını) veya şifrelenmemiş olarak belirler.

Bu bileşen aynı zamanda şu

- [Çerez tabanlı oturum afinitesini](features.md#session-affinity)kullanarak kullanıcı oturumunun aynı sunucuda tutulup tutulmayacağını belirleyin.

- [Bağlantı boşaltma](features.md#connection-draining)kullanarak arka uç havuzu üyelerini zarif bir şekilde kaldırın.

- Arka uç durumunu izlemek için özel bir sondayı ilişkilendirin, istek zaman aralığını ayarlayın, istekte ana bilgisayar adını ve yolunu geçersiz kılın ve Uygulama Hizmeti arka uç ayarlarını belirtmek için tek tıklatma kolaylığı sağlayın.

## <a name="backend-pools"></a>Arka uç havuzları

Arka uç havuzu, isteğe hizmet veren arka uç sunucularına isteği yönlendirir. Arka uç havuzları şunları içerebilir:

- NIC’ler
- Sanal makine ölçek kümeleri
- Genel IP adresleri
- Dahili IP adresleri
- FQDN
- Çok kiracılı arka uçlar (Uygulama Hizmeti gibi)

Uygulama Ağ Geçidi arka uç havuzu üyeleri kullanılabilirlik kümesine bağlı değildir. Bir uygulama ağ geçidi, içinde olduğu sanal ağın dışındaki örneklerle iletişim kurabilir. Sonuç olarak, ip bağlantısı olduğu sürece arka uç havuzlarının üyeleri kümeler arasında, veri merkezleri arasında veya Azure dışında olabilir.

Dahili IP'leri arka uç havuzu üyesi olarak kullanıyorsanız, [sanal ağ eşleme](../virtual-network/virtual-network-peering-overview.md) veya [VPN ağ geçidi](../vpn-gateway/vpn-gateway-about-vpngateways.md)kullanmanız gerekir. Sanal ağ eşleme, diğer sanal ağlardaki yük dengeleme trafiği için desteklenir ve yararlıdır.

Bir uygulama ağ geçidi, trafiğe izin verildiğinde Azure ExpressRoute veya VPN tünelleriyle bağlandığında şirket içi sunucularla da iletişim kurabilir.

Farklı istek türleri için farklı arka uç havuzları oluşturabilirsiniz. Örneğin, genel istekler için bir arka uç havuzu ve ardından uygulamanız için mikro hizmetlere gelen istekler için başka bir arka uç havuzu oluşturun.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Varsayılan olarak, bir uygulama ağ geçidi arka uç havuzundaki tüm kaynakların sistem durumunu izler ve sağlıksız olanları otomatik olarak kaldırır. Daha sonra sağlıksız örnekleri izler ve kullanılabilir olduklarında ve sağlık sondalarına yanıt verdiklerinde bunları sağlıklı arka uç havuzuna geri ekler.

Varsayılan sistem durumu sondası izlemenin yanı sıra, sistem durumu sondasını uygulamanızın gereksinimlerine uyacak şekilde özelleştirebilirsiniz. Özel sondalar, sistem durumu izleme üzerinde daha ayrıntılı denetim sağlar. Özel sondalar kullanırken, sonda aralığını, URL'yi ve sınanacak yolu ve arka uç havuzu örneği sağlıksız olarak işaretlenmeden önce kabul edilebilen kaç başarısız yanıtı yapılandırabilirsiniz. Her arka uç havuzunun durumunu izlemek için özel sondaları yapılandırmanızı öneririz.

Daha fazla bilgi için [bkz.](../application-gateway/application-gateway-probe-overview.md)

## <a name="next-steps"></a>Sonraki adımlar

Bir uygulama ağ geçidi oluşturun:

* [Azure portalında](quick-create-portal.md)
* [Azure PowerShell'i kullanarak](quick-create-powershell.md)
* [Azure CLI'yi kullanarak](quick-create-cli.md)
