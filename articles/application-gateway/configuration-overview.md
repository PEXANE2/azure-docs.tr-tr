---
title: Azure Application Gateway yapılandırmasına genel bakış
description: Bu makalede, Azure Application Gateway bileşenlerini yapılandırma açıklanmaktadır
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: conceptual
ms.date: 07/20/2020
ms.author: absha
ms.openlocfilehash: 20d1dfea251fdfd0bd6e8432d1ea0c7af7284cb5
ms.sourcegitcommit: 0b8320ae0d3455344ec8855b5c2d0ab3faa974a3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87428184"
---
# <a name="application-gateway-configuration-overview"></a>Application Gateway yapılandırmaya genel bakış

Azure Application Gateway, farklı senaryolar için çeşitli şekillerde yapılandırabileceğiniz çeşitli bileşenlerden oluşur. Bu makalede, her bir bileşenin nasıl yapılandırılacağı gösterilir.

![Application Gateway bileşenleri akış grafiği](./media/configuration-overview/configuration-overview1.png)

Bu görüntüde, üç dinleyici içeren bir uygulama gösterilmektedir. İlk ikisi sırasıyla ve için çok siteli dinleyiclardır `http://acme.com/*` `http://fabrikam.com/*` . Her ikisi de 80 numaralı bağlantı noktasını dinler. Üçüncü, daha önce Güvenli Yuva Katmanı (SSL) sonlandırma olarak bilinen uçtan uca Aktarım Katmanı Güvenliği (TLS) sonlandırmasına sahip temel bir dinleyiciye sahiptir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure sanal ağı ve adanmış alt ağ

Uygulama ağ geçidi, sanal ağınızdaki adanmış bir dağıtımdır. Sanal ağınızda, uygulama ağ geçidi için adanmış bir alt ağ gerekir. Bir alt ağda belirli bir uygulama ağ geçidi dağıtımının birden fazla örneğine sahip olabilirsiniz. Ayrıca, alt ağdaki diğer uygulama ağ geçitlerini da dağıtabilirsiniz. Ancak uygulama ağ geçidi alt ağında başka herhangi bir kaynak dağıtamazsınız.

> [!NOTE]
> Aynı alt ağda Standard_v2 ve standart Azure Application Gateway karıştırılamaz.

#### <a name="size-of-the-subnet"></a>Alt ağın boyutu

Application Gateway, örnek başına bir özel IP adresi ve özel bir ön uç IP yapılandırılmışsa başka bir özel IP adresi kullanır.

Azure, iç kullanım için her alt ağda beş IP adresini de ayırır: ilk dört ve son IP adresi. Örneğin, özel ön uç IP 'si olmayan 15 Application Gateway örnekleri göz önünde bulundurun. Bu alt ağ için en az 20 IP adresiniz olması gerekir: iç kullanım için beş ve Application Gateway örnekleri için 15. Bu nedenle,/27 veya daha büyük bir alt ağ boyutu gerekir.

27 Application Gateway örnekleri ve bir özel ön uç IP 'si için IP adresi olan bir alt ağ düşünün. Bu durumda, uygulama ağ geçidi örnekleri için, biri özel ön uç için, diğeri de iç kullanım için beş IP adresi 33 gerekir: 27. Bu nedenle,/26 alt ağ boyutuna veya daha büyük bir ağa ihtiyacınız vardır.

En az/28 olan bir alt ağ boyutu kullanmanızı öneririz. Bu boyut size 11 kullanılabilir IP adresi sağlar. Uygulamanızın yüklemesi 10 ' dan fazla Application Gateway örneği gerektiriyorsa, bir/27 veya/26 alt ağ boyutu düşünün.

#### <a name="network-security-groups-on-the-application-gateway-subnet"></a>Application Gateway alt ağındaki ağ güvenlik grupları

Ağ güvenlik grupları (NSG 'ler) Application Gateway desteklenir. Ancak bazı kısıtlamalar vardır:

- Application Gateway v1 SKU 'SU için 65503-65534 TCP bağlantı noktalarında gelen Internet trafiğine ve **Gatewaymanager** hizmet **etiketi ile hedef** alt ağa sahip v2 SKU 'su için TCP bağlantı noktaları 65200-65535 ' ye izin vermelisiniz. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları Azure sertifikaları tarafından korunur (kilitlidir). Bu ağ geçitlerinin müşterileri de dahil olmak üzere dış varlıklar bu uç noktalar üzerinde iletişim kuramaz.

- Giden internet bağlantısı engellenmiyor. NSG 'deki varsayılan giden kurallar internet bağlantısına izin verir. Şunları yapmanızı öneririz:

  - Varsayılan giden kurallarını kaldırmayın.
  - Giden bağlantıları reddeden diğer çıkış kuralları oluşturmayın.

- Hedef alt ağa sahip **AzureLoadBalancer** etiketindeki trafiğe izin **verilmesi gerekir.**

#### <a name="allow-application-gateway-access-to-a-few-source-ips"></a>Birkaç kaynak IP 'ye Application Gateway erişime izin ver

Bu senaryo için Application Gateway alt ağında NSG 'leri kullanın. Aşağıdaki kısıtlamaları alt ağa bu öncelik sırasına göre yerleştirin:

1. Kaynak IP veya IP aralığından gelen trafiğe, gelen erişim bağlantı noktası (örneğin, HTTP erişimi için bağlantı noktası 80) olarak tüm Application Gateway alt ağ adres aralığı ve hedef bağlantı noktası olarak izin verin.
2. Application Gateway v1 SKU 'SU için 65503-65534 olarak **kaynak ve hedef** bağlantı **noktası olarak kaynak** kaynaklı gelen isteklere ve [arka uç sistem durumu iletişimi](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)için v2 SKU 'su için 65200-65535 bağlantı noktalarına izin verin. Bu bağlantı noktası aralığı, Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları Azure sertifikaları tarafından korunur (kilitlidir). Uygun sertifikalar yerine, dış varlıklar bu uç noktalar üzerinde değişiklik başlatamaz.
3. [Ağ güvenlik grubundaki](https://docs.microsoft.com/azure/virtual-network/security-overview)gelen Azure Load Balancer Araştırmaları (*AzureLoadBalancer* Tag) ve gelen sanal ağ trafiğine (*VirtualNetwork* etiketi) izin verin.
4. Engelle-All kuralını kullanarak diğer tüm gelen trafiği engelleyin.
5. Tüm hedefler için internet 'e giden trafiğe izin verin.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Application Gateway alt ağında desteklenen kullanıcı tanımlı yollar

> [!IMPORTANT]
> Application Gateway alt ağında UDRs kullanmak, [arka uç sistem durumu görünümündeki](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) sistem durumunun **Bilinmeyen**olarak görünmesine neden olabilir. Ayrıca, Application Gateway günlüklerin ve ölçümlerin oluşturulmasına neden olabilir. Arka uç sistem durumunu, günlükleri ve ölçümleri görüntüleyebilmeniz için Application Gateway alt ağında UDRs 'yi kullanmanızı öneririz.

- **v1**

   V1 SKU 'SU için, Kullanıcı tanımlı yollar (UDRs), uçtan uca istek/yanıt iletişimini değiştirmediği sürece Application Gateway alt ağında desteklenir. Örneğin, paket incelemesi için bir güvenlik duvarı gereci işaret etmek üzere Application Gateway alt ağında bir UDR ayarlayabilirsiniz. Ancak, inceleme sonrasında paketin amaçlanan hedefe ulaşabildiğinizden emin olmanız gerekir. Bunun başarısız olması, sistem durumu araştırmasına veya trafik yönlendirme davranışına yol açabilir. Bu, Azure ExpressRoute veya sanal ağdaki VPN ağ geçitleri tarafından yayılan, öğrenilen yolları veya varsayılan 0.0.0.0/0 yollarını içerir. 0.0.0.0/0 ' ın şirket içinde yeniden yönlendirilmesi gereken (Zorlamalı tünel) her senaryo v1 için desteklenmez.

- **v2**

   V2 SKU 'SU için desteklenen ve desteklenmeyen senaryolar vardır:

   **v2 desteklenen senaryolar**
   > [!WARNING]
   > Yol tablosunun yanlış yapılandırması, Application Gateway v2 'de simetrik yönlendirmeye neden olabilir. Tüm yönetim/denetim düzlemi trafiğinin bir Sanal Gereç üzerinden değil, doğrudan Internet 'e gönderildiğinden emin olun. Günlüğe kaydetme ve ölçümler de etkilenebilir.


  **Senaryo 1**: sınır ağ GEÇIDI Protokolü (BGP) yol yaymayı Application Gateway alt ağa devre dışı bırakmak için UDR

   Bazen varsayılan ağ geçidi yolu (0.0.0.0/0) Application Gateway sanal ağla ilişkili ExpressRoute veya VPN Gateway 'ler aracılığıyla tanıtılabilir. Bu, Internet 'e doğrudan yol gerektiren yönetim düzlemi trafiğini keser. Bu tür senaryolarda, BGP yol yaymayı devre dışı bırakmak için bir UDR kullanılabilir. 

   BGP yol yaymayı devre dışı bırakmak için aşağıdaki adımları kullanın:

   1. Azure 'da bir yol tablosu kaynağı oluşturun.
   2. **Sanal ağ geçidi yol yayma** parametresini devre dışı bırakın. 
   3. Yol tablosunu uygun alt ağla ilişkilendirin. 

   Bu senaryo için UDR 'nin etkinleştirilmesi, mevcut tüm kurulumları bozmamalıdır.

  **2. senaryo**: Internet 'e 0.0.0.0/0 öğesini yönlendirmek için UDR

   0.0.0.0/0 trafiğini doğrudan Internet 'e göndermek için bir UDR oluşturabilirsiniz. 

  **Senaryo 3**: Kubernetes kullanan ile Azure Kubernetes hizmeti için UDR

  Azure Kubernetes hizmeti (AKS) ve Application Gateway giriş denetleyicisi (AGIC) ile Kubernetes kullanan kullanıyorsanız, Application Gateway olan ve alt öğe için gönderilen trafiğin doğru düğüme yönlendirilmesini sağlamak için bir yol tablosu gerekir. Azure CNı kullanıyorsanız bu gerekli değildir. 

  Kubernetes kullanan 'in çalışmasına izin vermek için yol tablosunu kullanmak üzere aşağıdaki adımları izleyin:

  1. AKS tarafından oluşturulan kaynak grubuna gidin (kaynak grubunun adı "MC_" ile başlamalıdır)
  2. Bu kaynak grubunda AKS tarafından oluşturulan yol tablosunu bulun. Yol tablosu aşağıdaki bilgilerle doldurulmalıdır:
     - Adres ön eki, AKS 'te erişmek istediğiniz nesnelerin IP aralığı olmalıdır. 
     - Sonraki atlama türü Sanal Gereç olmalıdır. 
     - Sonraki atlama adresi, pods 'yi barındıran düğümün IP adresi olmalıdır.
  3. Bu yol tablosunu Application Gateway alt ağıyla ilişkilendirin. 
    
  **v2 desteklenmeyen senaryolar**

  **Senaryo 1**: sanal gereçler için UDR

  0.0.0.0/0 ' ın herhangi bir Sanal Gereç aracılığıyla yeniden yönlendirilmesi gereken herhangi bir senaryo, bir hub/bağlı sanal ağ veya şirket içi (Zorlamalı tünel) v2 için desteklenmez.

## <a name="front-end-ip"></a>Ön uç IP 'si

Uygulama ağ geçidini genel IP adresi, özel bir IP adresi veya her ikisine de sahip olacak şekilde yapılandırabilirsiniz. İstemcilerin internet 'e yönelik bir sanal IP (VIP) ile internet üzerinden erişmesi gereken bir arka ucu barındırdığınızda genel IP gereklidir. 

İnternet 'e açık olmayan bir iç uç nokta için genel IP gerekli değildir. Bu, *iç yük dengeleyici* (ILB) uç noktası veya özel ön uç IP 'si olarak bilinir. Bir Application Gateway ıLB, internet 'e açık olmayan iç iş kolu uygulamaları için yararlıdır. Ayrıca, internet 'e açık olmayan ancak hepsini bir kez deneme yük dağıtımı, oturum sürekliliği veya TLS sonlandırma gerektiren bir güvenlik sınırı içinde çok katmanlı bir uygulamadaki hizmetler ve katmanlar için de kullanışlıdır.

Yalnızca 1 genel IP adresi veya bir özel IP adresi desteklenir. Uygulama ağ geçidini oluştururken ön uç IP 'sini seçersiniz.

- Genel IP için, yeni bir genel IP adresi oluşturabilir veya uygulama ağ geçidi ile aynı konumda bulunan bir genel IP 'yi kullanabilirsiniz. Daha fazla bilgi için bkz. [statik ve dinamik genel IP adresi](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-versus-dynamic-public-ip-address).

- Özel bir IP için, uygulama ağ geçidinin oluşturulduğu alt ağdan özel bir IP adresi belirtebilirsiniz. Bir tane belirtmezseniz, alt ağdan rastgele bir IP adresi otomatik olarak seçilir. Seçtiğiniz IP adresi türü (statik veya dinamik) daha sonra değiştirilemez. Daha fazla bilgi için bkz. [iç yük dengeleyiciye sahip bir uygulama ağ geçidi oluşturma](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm).

Ön uç IP adresi, ön uç IP üzerindeki gelen istekleri denetleyen bir *dinleyiciyle*ilişkilendirilir.

## <a name="listeners"></a>Dinleyiciler

Dinleyici, bağlantı noktası, protokol, konak ve IP adresini kullanarak gelen bağlantı isteklerini denetleyen mantıksal bir varlıktır. Dinleyiciyi yapılandırdığınızda, bunlar için ağ geçidinde gelen istekteki karşılık gelen değerlerle eşleşen değerler girmeniz gerekir.

Azure portal kullanarak bir uygulama ağ geçidi oluşturduğunuzda, dinleyici için protokolü ve bağlantı noktasını seçerek de varsayılan bir dinleyici oluşturursunuz. HTTP2 desteğinin dinleyicide etkinleştirilip etkinleştirilmeyeceğini seçebilirsiniz. Uygulama ağ geçidini oluşturduktan sonra, bu varsayılan dinleyicinin (*Appgatewayhttplistener*) ayarlarını düzenleyebilir veya yeni dinleyiciler oluşturabilirsiniz.

### <a name="listener-type"></a>Dinleyici türü

Yeni bir dinleyici oluşturduğunuzda [ *temel* ve *Çoklu site*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners)arasında seçim yapabilirsiniz.

- Tüm isteklerinizin (herhangi bir etki alanı için) kabul edilmesini ve arka uç havuzlara iletilmesini istiyorsanız temel ' yı seçin. [Temel dinleyiciyle bir uygulama ağ geçidi oluşturmayı](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)öğrenin.

- İstekleri *konak* üstbilgisine veya ana bilgisayar adlarına göre farklı arka uç havuzlarıyla iletmek istiyorsanız, Ayrıca, gelen istekle eşleşen bir ana bilgisayar adı belirtmeniz gereken çok siteli dinleyici ' i seçin. Bunun nedeni, Application Gateway aynı genel IP adresi ve bağlantı noktasında birden fazla Web sitesini barındırmak için HTTP 1,1 ana bilgisayar üst bilgilerini temel alır. Daha fazla bilgi için bkz. [Application Gateway kullanarak birden çok site barındırma](multiple-site-overview.md).

#### <a name="order-of-processing-listeners"></a>İşleme dinleyicileri sırası

V1 SKU 'SU için, istekler kuralların sırasına ve dinleyici türüne göre eşleştirilir. Temel dinleyiciye sahip bir kural sırayla ilk kez geliyorsa, önce işlenir ve bu bağlantı noktası ve IP birleşimi için tüm istekleri kabul eder. Bunu önlemek için, kuralları çok siteli dinleyiciyle yapılandırın ve kuralı temel dinleyiciyle listenin en son bölümüne gönderin.

V2 SKU 'SU için, çoklu site dinleyicileri temel dinleyicilerine göre işlenir.

### <a name="front-end-ip"></a>Ön uç IP 'si

Bu dinleyiciyle ilişkilendirmeyi planladığınız ön uç IP adresini seçin. Dinleyici, bu IP üzerindeki gelen istekleri dinleyecektir.

### <a name="front-end-port"></a>Ön uç bağlantı noktası

Ön uç bağlantı noktasını seçin. Mevcut bir bağlantı noktasını seçin veya yeni bir bağlantı noktası oluşturun. [İzin verilen bağlantı noktası aralığından](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports)herhangi bir değer seçin. 80 ve 443 gibi yalnızca iyi bilinen bağlantı noktalarını, ancak uygun olan tüm özel bağlantı noktalarını kullanabilirsiniz. Bir bağlantı noktası, genel kullanıma yönelik dinleyiciler veya özel kullanıma yönelik dinleyiciler için kullanılabilir.

### <a name="protocol"></a>Protokol

HTTP veya HTTPS seçin:

- HTTP ' yi seçerseniz, istemci ve uygulama ağ geçidi arasındaki trafik şifrelenmemiş olur.

- [TLS sonlandırmasını](features.md#secure-sockets-layer-ssltls-termination) veya [uçtan uca TLS şifrelemesini](https://docs.microsoft.com/azure/application-gateway/ssl-overview)istiyorsanız https 'yi seçin. İstemci ile uygulama ağ geçidi arasındaki trafik şifrelenir. Ve TLS bağlantısı uygulama ağ geçidinde sonlanır. Uçtan uca TLS şifrelemeyi istiyorsanız, HTTPS 'yi seçmeniz ve **arka uç http** ayarını yapılandırmanız gerekir. Bu, uygulama ağ geçidinden arka uca seyahat edildiğinde trafiğin yeniden şifrelenmesini sağlar.


TLS sonlandırmasını ve uçtan uca TLS şifrelemesini yapılandırmak için, uygulama ağ geçidinin bir simetrik anahtar türetmesini sağlamak üzere dinleyiciye bir sertifika eklemeniz gerekir. Bu, TLS protokol belirtimi tarafından belirlenir. Simetrik anahtar, ağ geçidine gönderilen trafiği şifrelemek ve şifresini çözmek için kullanılır. Ağ Geçidi sertifikası kişisel bilgi değişimi (PFX) biçiminde olmalıdır. Bu biçim, ağ geçidinin trafiği şifrelemek ve şifresini çözmek için kullandığı özel anahtarı dışa aktarmanıza olanak tanır.

#### <a name="supported-certificates"></a>Desteklenen sertifikalar

Bkz. [TLS sonlandırma için desteklenen sertifikalar](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination).

### <a name="additional-protocol-support"></a>Ek protokol desteği

#### <a name="http2-support"></a>HTTP2 desteği

HTTP/2 protokol desteği yalnızca uygulama ağ geçidi dinleyicilerine bağlanan istemciler tarafından kullanılabilir. Arka uç sunucu havuzlarıyla iletişim HTTP/1.1 üzerinden yapılır. HTTP/2 desteği varsayılan olarak devre dışıdır. Aşağıdaki Azure PowerShell kod parçacığı bunun nasıl etkinleştirileceğini göstermektedir:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>WebSocket desteği

WebSocket desteği varsayılan olarak etkindir. Etkinleştirmek veya devre dışı bırakmak için Kullanıcı tarafından yapılandırılabilir bir ayar yoktur. WebSockets 'i hem HTTP hem de HTTPS dinleyicilerine sahip olarak kullanabilirsiniz.

### <a name="custom-error-pages"></a>Özel hata sayfaları

Genel düzeyde veya dinleyici düzeyinde özel hata tanımlayabilirsiniz. Ancak Azure portal genel düzey özel hata sayfaları oluşturma şu anda desteklenmiyor. Dinleyici düzeyinde 403 Web uygulaması güvenlik duvarı hatası veya 502 bakım sayfası için özel hata sayfası yapılandırabilirsiniz. Ayrıca, belirtilen hata durum kodu için genel olarak erişilebilir bir blob URL 'SI belirtmeniz gerekir. Daha fazla bilgi için bkz. [Application Gateway özel hata sayfası oluşturma](https://docs.microsoft.com/azure/application-gateway/custom-error).

![Application Gateway hata kodları](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Genel özel hata sayfasını yapılandırmak için, bkz. [Azure PowerShell Configuration](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration).

### <a name="tls-policy"></a>TLS ilkesi

Bir arka uç sunucu grubu için TLS/SSL sertifika yönetimini merkezileştirmek ve şifreleme şifre çözme ek yükünü azaltabilirsiniz. Merkezi TLS işleme, güvenlik gereksinimlerinize uygun bir merkezi TLS ilkesi belirtmenize de olanak tanır. *Varsayılan*, *önceden tanımlanmış*veya *özel* TLS ilkesi seçebilirsiniz.

TLS protokolü sürümlerini denetlemek için TLS ilkesi yapılandırırsınız. Bir uygulama ağ geçidini TLS 1.0, TLS 1.1 ve TLS 1.2 ile TLS el sıkışmaları için en düşük protokol sürümünü kullanacak şekilde yapılandırabilirsiniz. Varsayılan olarak, SSL 2,0 ve 3,0 devre dışıdır ve yapılandırılamaz. Daha fazla bilgi için bkz. [APPLICATION Gateway TLS ilkesine genel bakış](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview).

Bir dinleyici oluşturduktan sonra, bunu bir istek yönlendirme kuralıyla ilişkilendirirsiniz. Bu kural, dinleyicide alınan isteklerin arka uca nasıl yönlendirildiğini belirler.

## <a name="request-routing-rules"></a>İstek yönlendirme kuralları

Azure portal kullanarak bir uygulama ağ geçidi oluşturduğunuzda, varsayılan bir kural (*rule1*) oluşturursunuz. Bu kural varsayılan dinleyiciyi (*Appgatewayhttplistener*) varsayılan arka uç Havuzu (*appgatewaybackendpool*) ve varsayılan arka uç http ayarları (*Appgatewaybackendhttpsettings*) ile bağlar. Ağ geçidini oluşturduktan sonra, varsayılan kuralın ayarlarını düzenleyebilir veya yeni kurallar oluşturabilirsiniz.

### <a name="rule-type"></a>Kural türü

Bir kural oluşturduğunuzda [ *temel* ve *yol tabanlı*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rules)arasında seçim yapabilirsiniz.

- İlişkili dinleyicide (örneğin, *Blog <i></i> . contoso.com/ \* )* tüm istekleri tek bir arka uç havuzuna iletmek istiyorsanız temel ' yı seçin.
- Belirli URL yollarındaki istekleri belirli arka uç havuzlarına yönlendirmek istiyorsanız yol tabanlı ' i seçin. Yol deseninin Sorgu parametrelerine değil yalnızca URL 'nin yoluna uygulanması.

#### <a name="order-of-processing-rules"></a>İşleme kuralları sırası

V1 ve v2 SKU 'SU için, gelen isteklerin düzen eşleştirmesi, yolların yol tabanlı kuralın URL yol haritasında listelendiği sırada işlenir. Bir istek, yol eşlemesindeki iki veya daha fazla yoldaki Düzenle eşleşiyorsa, önce listelenen yol eşleştirilir. Ve istek bu yol ile ilişkili arka uca iletilir.

### <a name="associated-listener"></a>İlişkili dinleyici

Dinleyiciyle ilişkilendirilen *istek yönlendirme kuralı* , isteği yönlendiren arka uç havuzunu belirleyecek şekilde değerlendirilmek için bir dinleyiciyi kuralla ilişkilendirin.

### <a name="associated-back-end-pool"></a>İlişkili arka uç havuzu

Dinleyicinin aldığı isteklere sunan arka uç hedeflerini içeren arka uç havuzunun bulunduğu kuralla ilişkilendirin.

 - Temel bir kural için yalnızca bir arka uç havuzuna izin verilir. İlişkili dinleyicide bulunan tüm istekler bu arka uç havuzuna iletilir.

 - Yol tabanlı bir kural için, her bir URL yoluna karşılık gelen birden fazla arka uç havuzu ekleyin. Girilen URL yoluyla eşleşen istekler karşılık gelen arka uç havuzuna iletilir. Ayrıca, bir varsayılan arka uç havuzu ekleyin. Kuraldaki hiçbir URL yoluyla eşleşmeyen istekler bu havuza iletilir.

### <a name="associated-back-end-http-setting"></a>İlişkili arka uç HTTP ayarı

Her kural için bir arka uç HTTP ayarı ekleyin. İstekler, bu ayarda belirtilen bağlantı noktası numarası, protokol ve diğer bilgileri kullanarak uygulama ağ geçidinden arka uç hedeflerine yönlendirilir.

Temel bir kural için yalnızca bir arka uç HTTP ayarına izin verilir. İlişkili dinleyicide bulunan tüm istekler, bu HTTP ayarı kullanılarak karşılık gelen arka uç hedeflerine iletilir.

Yol tabanlı bir kural için, her bir URL yoluna karşılık gelen birden fazla arka uç HTTP ayarı ekleyin. Bu ayarda URL yoluyla eşleşen istekler, her bir URL yoluna karşılık gelen HTTP ayarları kullanılarak ilgili arka uç hedeflerine iletilir. Ayrıca, varsayılan bir HTTP ayarı ekleyin. Bu kuraldaki hiçbir URL yoluyla eşleşmeyen istekler varsayılan HTTP ayarı kullanılarak varsayılan arka uç havuzuna iletilir.

### <a name="redirection-setting"></a>Yeniden yönlendirme ayarı

Bir temel kural için yeniden yönlendirme yapılandırılırsa, ilişkili dinleyicinin tüm istekleri hedefe yeniden yönlendirilir. Bu, *genel* yeniden yönlendirme 'dir. Yol tabanlı bir kural için yeniden yönlendirme yapılandırılırsa, yalnızca belirli bir site alanındaki istekler yeniden yönlendirilir. Örnek, */cart/ \* *tarafından belirtilen bir alışveriş sepeti alanıdır. Bu, *yol tabanlı* yeniden yönlendirme 'dir.

Yeniden yönlendirmeler hakkında daha fazla bilgi için bkz. [Application Gateway yönlendirmeye genel bakış](redirect-overview.md).

#### <a name="redirection-type"></a>Yeniden yönlendirme türü

Gereken yeniden yönlendirme türünü seçin: *kalıcı (301)*, *geçici (.)*, *bulunan (302*) veya *diğer (303*).

#### <a name="redirection-target"></a>Yeniden yönlendirme hedefi

Yeniden yönlendirme hedefi olarak başka bir dinleyici veya dış site seçin.

##### <a name="listener"></a>Dinleyici

Trafiği bir dinleyicisinden ağ geçidinde diğerine yeniden yönlendirmek için yeniden yönlendirme hedefi olarak dinleyici ' i seçin. HTTP-HTTPS yeniden yönlendirmeyi etkinleştirmek istediğinizde bu ayar gereklidir. Gelen HTTP isteklerini, gelen HTTPS isteklerini denetleyen hedef dinleyiciye denetleyen kaynak dinleyicisinden trafiği yeniden yönlendirir. Ayrıca, yeniden yönlendirme hedefine iletilen istekteki özgün istekten sorgu dizesini ve yolunu eklemeyi de tercih edebilirsiniz.

![Application Gateway bileşenleri iletişim kutusu](./media/configuration-overview/configure-redirection.png)

HTTP-HTTPS yeniden yönlendirmesi hakkında daha fazla bilgi için bkz.:
- [Azure portal kullanarak HTTP-HTTPS yönlendirmesi](redirect-http-to-https-portal.md)
- [PowerShell kullanarak HTTP-to-HTTPS yönlendirmesi](redirect-http-to-https-powershell.md)
- [Azure CLı kullanarak HTTP-HTTPS yönlendirmesi](redirect-http-to-https-cli.md)

##### <a name="external-site"></a>Dış site

Bu kuralla ilişkilendirilen dinleyicide trafiği bir dış siteye yönlendirmek istediğinizde dış site ' yi seçin. Sorgu dizesini, yeniden yönlendirme hedefine iletilen istekteki özgün istekten eklemeyi seçebilirsiniz. Yolu özgün istekte bulunan dış siteye iletmeyin.

Yeniden yönlendirme hakkında daha fazla bilgi için bkz.
- [PowerShell kullanarak trafiği dış siteye yönlendirme](redirect-external-site-powershell.md)
- [CLı kullanarak trafiği dış siteye yönlendirme](redirect-external-site-cli.md)

### <a name="rewrite-http-headers-and-url"></a>HTTP üst bilgilerini ve URL’sini yeniden yazma

Yeniden yazma kurallarını kullanarak, HTTP (S) isteği ve yanıt üst bilgilerini ekleyebilir, kaldırabilir veya güncelleştirebilir, istek ve yanıt paketleri uygulama ağ geçidi aracılığıyla istemci ile arka uç havuzları arasında hareket edebilir.

Üst bilgiler ve URL parametreleri statik değerlere veya diğer üst bilgilere ve sunucu değişkenlerine ayarlanabilir. Bu, istemci IP adreslerini ayıklama, arka uç hakkındaki hassas bilgileri kaldırma, daha fazla güvenlik ekleme vb. gibi önemli kullanım durumlarının sağlanmasına yardımcı olur.
Daha fazla bilgi için bkz.

 - [HTTP üstbilgilerini ve URL genel bakış 'ı yeniden yazma](rewrite-http-headers-url.md)
 - [HTTP üstbilgisini yeniden yazmayı yapılandırma](rewrite-http-headers-portal.md)
 - [URL yeniden yazmayı yapılandırma](rewrite-url-portal.md)

## <a name="http-settings"></a>HTTP ayarları

Application Gateway, burada belirttiğiniz yapılandırmayı kullanarak trafiği arka uç sunucularına yönlendirir. Bir HTTP ayarı oluşturduktan sonra, bir veya daha fazla istek yönlendirme kuralıyla ilişkilendirmeniz gerekir.

### <a name="cookie-based-affinity"></a>Tanımlama bilgisi tabanlı benzeşim

Azure Application Gateway, kullanıcı oturumlarını sürdürmek için ağ geçidi ile yönetilen tanımlama bilgilerini kullanır. Bir Kullanıcı Application Gateway ilk isteği gönderdiğinde, yanıt ayrıntılarını içeren bir karma değer olan yanıtta benzeşim tanımlama bilgisini ayarlar. böylece, benzeşim tanımlama bilgisini taşıyan sonraki isteklerin, sürekliliği sürdürmek için aynı arka uç sunucusuna yönlendirilecektir. 

Bu özellik, bir kullanıcı oturumunu aynı sunucuda tutmak ve oturum durumunun bir Kullanıcı oturumu için sunucuda yerel olarak kaydedilmesi durumunda yararlıdır. Uygulama, tanımlama bilgisi tabanlı benzeşimi işleyemez, bu özelliği kullanamazsınız. Bunu kullanmak için, istemcilerin tanımlama bilgilerini desteklemesini sağlayın.

[Kmıum Browser](https://www.chromium.org/Home) [V80 Update](https://chromiumdash.appspot.com/schedule) , [SameSite](https://tools.ietf.org/id/draft-ietf-httpbis-rfc6265bis-03.html#rfc.section.5.3.7) özniteliği olmayan http tanımlama bilgilerinin SameSite = LAX olarak değerlendirilmesi gereken bir mantarih getirdi. CORS (çıkış noktaları arası kaynak paylaşımı) istekleri söz konusu olduğunda, tanımlama bilgisinin bir üçüncü taraf bağlamında gönderilmesi gerekiyorsa, *SameSite = None; kullanması gerekir. Güvenli* öznitelikler ve yalnızca https üzerinden gönderilmesi gerekir. Aksi halde, yalnızca HTTP senaryosunda tarayıcı tanımlama bilgilerini üçüncü taraf bağlamında göndermez. Bu güncelleştirmenin Chrome 'dan hedefi, güvenliği artırmaktır ve siteler arası Istek sahteciliği (CSRF) saldırılarından kaçınmaktır. 

Bu değişikliği desteklemek için, 17 2020 Şubat tarihinden itibaren Application Gateway (tüm SKU türleri), mevcut *Applicationgatewaybenzeşim* tanımlama bilgisine ek olarak *Applicationgatewayaffinitycors* adlı başka bir tanımlama bilgisi ekleyecektir. *Applicationgatewayaffinitycors* tanımlama bilgisinin kendisine eklenen iki özniteliği vardır (*"SameSite = None; Güvenli "*) böylece, geçici kaynak istekleri için de yapışkan oturumun korunmasını sağlayabilirsiniz.

Varsayılan benzeşim tanımlama bilgisi adının *Applicationgatewaybenzeşim* olduğunu ve bunu değiştirebileceğinizi unutmayın. Özel bir benzeşim tanımlama bilgisi adı kullanıyorsanız, CORS ile son ek tanımlama bilgisi eklenir. Örneğin, *Customtanımlama bilgisi Enamecors*.

> [!NOTE]
> *SameSite = None* özniteliği ayarlandıysa, tanımlama bilgisinin de *güvenli* bayrağı içermesi zorunludur ve https üzerinden gönderilmesi gerekir.  CORS üzerinden oturum benzeşimi gerekliyse, iş yükünüzü HTTPS 'ye geçirmeniz gerekir. Burada Application Gateway için TLS yük boşaltma ve uçtan uca TLS belgelerine bakın: [genel bakış](ssl-overview.md), [Azure Portal kullanarak bir uygulama ağ geçidini TLS sonlandırmasıyla yapılandırma](create-ssl-portal.md), [Portal Ile Application Gateway kullanarak uçtan uca TLS Yapılandırma](end-to-end-ssl-portal.md).

### <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlı hizmet güncelleştirmeleri sırasında arka uç havuz üyelerini dikkatlice kaldırmanıza yardımcı olur. HTTP ayarında bağlantı boşaltma etkinleştirerek, bu ayarı bir arka uç havuzunun tüm üyelerine uygulayabilirsiniz. Bir arka uç havuzunun tüm kaydını kaldırmak örneklerinin mevcut bağlantıları sürdürmeye devam etmesini ve yapılandırılabilir bir zaman aşımı için, yeni istek veya bağlantı almamasını sağlar. Bunun tek istisnası, ağ geçidi tarafından yönetilen oturum benzeşimi nedeniyle kaydını kaldırmak örneklerine yönelik isteklerdir ve kaydını kaldırmak örneklerine iletilmeye devam edecektir. Bağlantı boşaltma, arka uç havuzundan açıkça kaldırılan arka uç örnekleri için geçerlidir.

### <a name="protocol"></a>Protokol

Application Gateway, arka uç sunucularına yönlendirme istekleri için hem HTTP hem de HTTPS 'yi destekler. HTTP ' yi seçerseniz, arka uç sunucularının trafiği şifrelenmemiş olur. Şifrelenmemiş iletişim kabul edilebilir değilse, HTTPS seçeneğini belirleyin.

Dinleyicide HTTPS ile birleştirilmiş bu ayar [uçtan uca TLS](ssl-overview.md)'yi destekler. Bu, arka uca şifrelenmiş hassas verileri güvenli bir şekilde aktarmanıza olanak tanır. Arka uç havuzundaki uçtan uca TLS etkin olan her arka uç sunucusu, güvenli iletişime izin verecek bir sertifikayla yapılandırılmalıdır.

### <a name="port"></a>Bağlantı noktası

Bu ayar, arka uç sunucularının uygulama ağ geçidinden gelen trafiği dinleyebileceği bağlantı noktasını belirtir. 1 ile 65535 arasında bir bağlantı noktası yapılandırabilirsiniz.

### <a name="request-timeout"></a>İstek zaman aşımı

Bu ayar, uygulama ağ geçidinin arka uç sunucusundan yanıt almak için beklediği saniye sayısıdır.

### <a name="override-back-end-path"></a>Arka uç yolunu geçersiz kıl

Bu ayar, istek arka uca iletildiğinde kullanılacak isteğe bağlı bir özel iletme yolu yapılandırmanıza olanak tanır. Gelen yolun, **arka uç yolunu geçersiz kılma** alanındaki özel yol ile eşleşen herhangi bir bölümü, iletilen yola kopyalanır. Aşağıdaki tabloda bu özelliğin nasıl çalıştığı gösterilmektedir:

- HTTP ayarı temel bir istek yönlendirme kuralına eklendiğinde:

  | Özgün istek  | Arka uç yolunu geçersiz kıl | İstek arka uca iletildi |
  | ----------------- | --------------------- | ---------------------------- |
  | /Home            | /override seçeneğiyle birlikte            | /override/Home/              |
  | /Home/Secondhome/ | /override seçeneğiyle birlikte            | /override/Home/Secondhome/   |

- HTTP ayarı yol tabanlı bir istek yönlendirme kuralına eklendiğinde:

  | Özgün istek           | Yol kuralı       | Arka uç yolunu geçersiz kıl | İstek arka uca iletildi |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/Home/            | pathrule      | /override seçeneğiyle birlikte            | /override/Home/              |
  | /pathrule/Home/Secondhome/ | pathrule      | /override seçeneğiyle birlikte            | /override/Home/Secondhome/   |
  | /Home                     | pathrule      | /override seçeneğiyle birlikte            | /override/Home/              |
  | /Home/Secondhome/          | pathrule      | /override seçeneğiyle birlikte            | /override/Home/Secondhome/   |
  | /pathrule/Home/            | /pathrule/Home * | /override seçeneğiyle birlikte            | /override seçeneğiyle birlikte                   |
  | /pathrule/Home/Secondhome/ | /pathrule/Home * | /override seçeneğiyle birlikte            | /override/Secondhome/        |
  | pathrule                 | pathrule      | /override seçeneğiyle birlikte            | /override seçeneğiyle birlikte                   |

### <a name="use-for-app-service"></a>App Service için kullanın

Bu yalnızca Azure App Service arka ucu için gereken iki ayarı seçen bir UI kısayoludur. **Ana bilgisayar adının arka uç adresinden seçim**yapmasına izin verebilir ve henüz yoksa yeni bir özel araştırma oluşturur. (Daha fazla bilgi için bu makalenin [arka uç adres ayarından konak adını seçin](#pick) bölümüne bakın.) Yeni bir araştırma oluşturulur ve araştırma üst bilgisi arka uç üyesinin adresinden çekilir.

### <a name="use-custom-probe"></a>Özel araştırma kullan

Bu ayar özel bir [araştırmayı](application-gateway-probe-overview.md#custom-health-probe) bir http ayarıyla ilişkilendirir. HTTP ayarıyla yalnızca bir özel araştırma ilişkilendirebilirsiniz. Özel bir araştırmayı açık bir şekilde ilişkilendirmezseniz, arka ucun sistem durumunu izlemek için [varsayılan araştırma](application-gateway-probe-overview.md#default-health-probe-settings) kullanılır. Arka uçlarınızın sistem durumu izlemesi üzerinde daha fazla denetim için özel bir araştırma oluşturmanız önerilir.

> [!NOTE]
> Özel araştırma, karşılık gelen HTTP ayarı açıkça bir dinleyiciyle ilişkilendirilmediği takdirde arka uç havuzunun sistem durumunu izlemez.

### <a name="pick-host-name-from-back-end-address"></a><a name="pick"></a>Arka uç adresinden ana bilgisayar adını seçin

Bu yetenek, istekteki *ana bilgisayar* üst bilgisini arka uç havuzunun ana bilgisayar adına dinamik olarak ayarlar. Bir IP adresi veya FQDN kullanır.

Bu özellik, arka ucun etki alanı adının uygulama ağ geçidinin DNS adından farklı olması ve arka ucunun doğru uç noktaya çözülmesi için belirli bir ana bilgisayar üst bilgisine dayanmasına yardımcı olur.

Örnek olarak, arka uç olarak çok kiracılı hizmetler yer aldığı bir durumdur. App Service, tek bir IP adresi ile paylaşılan bir alan kullanan çok kiracılı bir hizmettir. Bu nedenle, bir App Service 'e yalnızca özel etki alanı ayarlarında yapılandırılmış ana bilgisayar adları aracılığıyla erişilebilir.

Varsayılan olarak, özel etki alanı adı *example.azurewebsites.net*' dir. App Service 'e açıkça kayıtlı olmayan bir ana bilgisayar adı aracılığıyla veya uygulama ağ geçidinin FQDN 'SI aracılığıyla uygulama hizmetinize erişmek için, özgün istekteki ana bilgisayar adını App Service 'in ana bilgisayar adına geçersiz kılarsınız. Bunu yapmak için, **arka uç adres ayarından seçim ana bilgisayar adını** etkinleştirin.

Var olan özel DNS adı App Service 'e eşlenmiş özel bir etki alanı için bu ayarı etkinleştirmeniz gerekmez.

> [!NOTE]
> Bu ayar, adanmış bir dağıtım olan App Service Ortamı için gerekli değildir.

### <a name="host-name-override"></a>Ana bilgisayar adı geçersiz kılma

Bu özellik, uygulama ağ geçidinde gelen istekteki *ana bilgisayar* üstbilgisinin yerine belirttiğiniz ana bilgisayar adını koyar.

Örneğin, *www.contoso.com* **konak adı** ayarında belirtilmişse, `https://appgw.eastus.cloudapp.azure.com/path1` `https://www.contoso.com/path1` istek arka uç sunucusuna iletildiğinde, * özgün istek * olarak değiştirilir.

## <a name="back-end-pool"></a>Arka uç havuzu

Arka uç havuzunu dört tür arka uç üyesine işaret edebilirsiniz: belirli bir sanal makine, bir sanal makine ölçek kümesi, IP adresi/FQDN veya App Service. 

Bir arka uç havuzu oluşturduktan sonra, bir veya daha fazla istek yönlendirme kuralıyla ilişkilendirmeniz gerekir. Ayrıca, uygulama ağ geçidinizdeki her bir arka uç havuzu için sistem durumu araştırmalarını da yapılandırmanız gerekir. İstek yönlendirme kuralı koşulu karşılandığında, Application Gateway trafiği ilgili arka uç havuzundaki sağlıklı sunuculara (sistem durumu araştırmaları tarafından belirlendiği şekilde) iletir.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Bir Application Gateway, varsayılan olarak arka uçtaki tüm kaynakların sistem durumunu izler. Ancak sistem durumu izleme üzerinde daha fazla denetim sağlamak için her bir arka uç HTTP ayarı için özel bir araştırma oluşturmanız önemle tavsiye ederiz. Özel bir araştırmanın nasıl yapılandırılacağını öğrenmek için bkz. [özel durum araştırma ayarları](application-gateway-probe-overview.md#custom-health-probe-settings).

> [!NOTE]
> Özel bir sistem durumu araştırması oluşturduktan sonra, bir arka uç HTTP ayarıyla ilişkilendirmeniz gerekir. Karşılık gelen HTTP ayarı bir kural kullanan bir dinleyiciyle açıkça ilişkilendirilmediği takdirde, özel bir araştırma arka uç havuzunun sistem durumunu izlemez.

## <a name="next-steps"></a>Sonraki adımlar

Artık Application Gateway bileşenleri hakkında bilgi edineceğimize göre şunları yapabilirsiniz:

- [Azure portal bir uygulama ağ geçidi oluşturun](quick-create-portal.md)
- [PowerShell kullanarak uygulama ağ geçidi oluşturma](quick-create-powershell.md)
- [Azure CLı kullanarak uygulama ağ geçidi oluşturma](quick-create-cli.md)
