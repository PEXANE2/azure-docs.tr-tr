---
title: Azure Uygulama Ağ Geçidi yapılandırmaya genel bakış
description: Bu makalede, Azure Uygulama Ağ Geçidi bileşenlerinin nasıl yapılandırılabildiğini açıklamaktadır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 03/24/2020
ms.author: absha
ms.openlocfilehash: 89d894a5125a16f95e6ef8a15c2503d48f3a8e55
ms.sourcegitcommit: d597800237783fc384875123ba47aab5671ceb88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80632183"
---
# <a name="application-gateway-configuration-overview"></a>Uygulama Ağ Geçidi yapılandırmagenel bakış

Azure Application Gateway, farklı senaryolar için çeşitli şekillerde yapılandırabileceğiniz birkaç bileşenden oluşur. Bu makalede, her bileşeni nasıl yapılandırabileceğinizgösterilmektedir.

![Uygulama Ağ Geçidi bileşenleri akış şeması](./media/configuration-overview/configuration-overview1.png)

Bu resim, üç dinleyicisi olan bir uygulamayı göstermektedir. İlk iki için çok sitedinleyici `http://acme.com/*` `http://fabrikam.com/*`ve , sırasıyla. İkisi de 80. Üçüncüsü, daha önce Güvenli Soketkatmanı (SSL) sonlandırma olarak bilinen uçtan uca Aktarım Katmanı Güvenliği (TLS) sonlandırmasına sahip temel bir dinleyicidir.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

### <a name="azure-virtual-network-and-dedicated-subnet"></a>Azure sanal ağı ve özel alt ağ

Uygulama ağ geçidi, sanal ağınızda özel bir dağıtımdır. Sanal ağınızda, uygulama ağ geçidi için özel bir alt ağ gereklidir. Bir alt ağda belirli bir uygulama ağ geçidi dağıtımının birden çok örneğine sahip olabilirsiniz. Alt ağdaki diğer uygulama ağ geçitlerini de dağıtabilirsiniz. Ancak uygulama ağ geçidi alt ağına başka bir kaynak dağıtamazsınız.

> [!NOTE]
> Standard_v2 ve Standart Azure Uygulama Ağ Geçidi'ni aynı alt ağda karıştıramazsınız.

#### <a name="size-of-the-subnet"></a>Alt netin boyutu

Uygulama Ağ Geçidi, özel bir ön uç IP'si yapılandırılırsa, örnek başına bir özel IP adresi ve başka bir özel IP adresi kullanır.

Azure ayrıca her alt ağda dahili kullanım için beş IP adresi ayırır: ilk dört ve son IP adresleri. Örneğin, özel ön uç IP'si olmayan 15 uygulama ağ geçidi örneğini göz önünde bulundurun. Bu alt ağ için en az 20 IP adresine ihtiyacınız vardır: beşi dahili kullanım için, 15'i de uygulama ağ geçidi örnekleri için. Yani, bir / 27 subnet boyutu veya daha büyük gerekir.

27 uygulama ağ geçidi örneği olan bir alt ağ ve özel bir ön uç IP'si için bir IP adresi düşünün. Bu durumda, 33 IP adresine ihtiyacınız vardır: uygulama ağ geçidi örnekleri için 27, özel ön uç için bir ve dahili kullanım için beş. Yani, bir / 26 subnet boyutu veya daha büyük gerekir.

En az /28 alt net boyutu kullanmanızı öneririz. Bu boyut size kullanılabilir 11 IP adresi verir. Uygulama yükünüz 10'dan fazla Uygulama Ağ Geçidi örneği gerektiriyorsa, bir /27 veya /26 alt net boyutunu düşünün.

#### <a name="network-security-groups-on-the-application-gateway-subnet"></a>Uygulama Ağ Geçidi alt ağındaki ağ güvenlik grupları

Ağ güvenlik grupları (NSG'ler) Uygulama Ağ Geçidi'nde desteklenir. Ama bazı kısıtlamalar vardır:

- Uygulama Ağ Geçidi v1 SKU için TCP bağlantı noktalarında 65503-65534 ve V2 SKU için 65200-65535 arasında gelen **Any** Internet trafiğine izin vermelisiniz. **GatewayManager** Bu bağlantı noktası aralığı Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları Azure sertifikaları tarafından korunur (kilitlenir). Bu ağ geçitlerinin müşterileri de dahil olmak üzere dış varlıklar bu uç noktalarda iletişim kuramıyor.

- Giden internet bağlantısı engellenemez. NSG'deki varsayılan giden kurallar internet bağlantısına izin verir. Şunları yapmanızı öneririz:

  - Varsayılan giden kuralları kaldırmayın.
  - Giden bağlantının reddedilmesini reddeden başka giden kurallar oluşturmayın.

- **AzureLoadBalancer** etiketinden gelen trafiğe izin verilmelidir.

#### <a name="allow-application-gateway-access-to-a-few-source-ips"></a>Uygulama Ağ Geçidi'nin birkaç kaynak IP'ye erişmesine izin ver

Bu senaryo için, Uygulama Ağ Geçidi alt netinde NSG'leri kullanın. Bu öncelik sırasına göre alt ağa aşağıdaki kısıtlamaları koyun:

1. Tüm Application Gateway alt net adresi aralığı ve gelen erişim bağlantı noktası olarak hedef bağlantı noktası olarak hedef ile bir kaynak IP veya IP aralığından gelen trafik izin (örneğin, http erişim için bağlantı noktası 80.
2. Uygulama **Ağ Geçidi** v1 SKU için 65503-65534 olarak **GatewayManager** servis etiketi ve hedef olarak kaynaktan gelen isteklere izin verin ve arka [uç sağlık durumu iletişimi](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)için v2 SKU için 65200-65535 bağlantı noktaları . Bu bağlantı noktası aralığı Azure altyapı iletişimi için gereklidir. Bu bağlantı noktaları Azure sertifikaları tarafından korunur (kilitlenir). Uygun sertifikalar yerinde olmadan, dış varlıklar bu uç noktalarda değişiklik başlatamaz.
3. [Ağ güvenlik grubunda](https://docs.microsoft.com/azure/virtual-network/security-overview)gelen Azure Load Balancer sondalarına *(AzureLoadBalancer* etiketi) ve gelen sanal ağ trafiğine *(VirtualNetwork* etiketi) izin verin.
4. Tümkuralları reddet kuralını kullanarak diğer tüm gelen trafiği engelleyin.
5. Tüm varış noktaları için internete giden trafiğe izin verin.

#### <a name="user-defined-routes-supported-on-the-application-gateway-subnet"></a>Application Gateway alt ağında desteklenen kullanıcı tanımlı yollar

> [!IMPORTANT]
> Uygulama Ağ Geçidi alt netinde ÜD'ler [kullanmak, arka uç sistem](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics#back-end-health) durumu görünümündeki sistem durumu bilinmiyor olarak görünmesine neden olabilir. **Unknown** Ayrıca, Uygulama Ağ Geçidi günlüklerinin ve ölçümlerinin oluşturma başarısız olmasına neden olabilir. Arka uç durumunu, günlükleri ve ölçümleri görüntüleyebilmeniz için Uygulama Ağ Geçidi alt netinde ÜD kullanmamanızı öneririz.

- **v1**

   v1 SKU için, uçtan uca istek/yanıt iletişimini değiştirmedikleri sürece, kullanıcı tanımlı rotalar (ÜDR) Uygulama Ağ Geçidi alt netinde desteklenir. Örneğin, paket denetimi için bir güvenlik duvarı cihazını işaret edecek şekilde Uygulama Ağ Geçidi alt ağına bir UDR ayarlayabilirsiniz. Ancak, paketin denetimden sonra hedeflenen hedefe ulaşabilmesini sağlamalısınız. Aksi takdirde yanlış sistem durumu sondası veya trafik yönlendirme davranışı na neden olabilir. Buna, sanal ağdaki Azure ExpressRoute veya VPN ağ geçitleri tarafından yayılan öğrenilen yolları veya varsayılan 0.0.0.0/0 rotalarını içerir.

- **v2**

   v2 SKU için desteklenen ve desteklenmeyen senaryolar vardır:

   **v2 desteklenen senaryolar**
   > [!WARNING]
   > Rota tablosunun yanlış bir yapılandırması, Application Gateway v2'de asimetrik yönlendirmeye neden olabilir. Tüm yönetim/kontrol uçağı trafiğinin sanal bir cihaz aracılığıyla değil, doğrudan Internet'e gönderilmesini sağlayın. Günlüğe kaydetme ve ölçümler de etkilenebilir.


  **Senaryo 1**: UDR, Uygulama Ağ Geçidi alt ağına Geçiş Ağ Geçidi Protokolü (BGP) Rota Yayılımı'nı devre dışı bırakma

   Bazen varsayılan ağ geçidi rotası (0.0.0.0/0) Application Gateway sanal ağıyla ilişkili ExpressRoute veya VPN ağ geçitleri aracılığıyla duyurulur. Bu, Internet'e doğrudan bir yol gerektiren yönetim düzlemtrafiğini kırar. Bu tür senaryolarda, BGP rota yayılmasını devre dışı kılabilir. 

   BGP rota yayılmasını devre dışı katmak için aşağıdaki adımları kullanın:

   1. Azure'da Bir Rota Tablosu kaynağı oluşturun.
   2. Sanal ağ **ağ geçidi rota yayılma** parametresini devre dışı bırak. 
   3. Rota Tablosunu uygun alt ağla ilişkilendirin. 

   Bu senaryo için UDR'yi etkinleştirmek varolan kurulumları bozmamalıdır.

  **Senaryo 2**: 0.0.0.0/0'ı Internet'e yönlendirmek için UDR

   0.0.0.0/0 trafiğini doğrudan Internet'e göndermek için bir UDR oluşturabilirsiniz. 

  **Senaryo 3**: Azure Kubernetes Servisi kubenet için UDR

  Kubenet'i Azure Kubernetes Hizmeti (AKS) ve Uygulama Ağ Geçidi Giriş Denetleyicisi (AGIC) ile kullanıyorsanız, bölmelere gönderilen trafiğin doğru düğüme yönlendirilmesine izin verecek bir rota tablosu ayarlamanız gerekir. Azure CNI kullanıyorsanız bu gerekli olmayacaktır. 

   Kubenet'in çalışmasına izin verecek rota tablosunu ayarlamak için aşağıdaki adımları kullanın:

  1. Azure'da Bir Rota Tablosu kaynağı oluşturun. 
  2. Oluşturulduktan sonra **Rotalar** sayfasına gidin. 
  3. Yeni bir rota ekleyin:
     - Adres öneki, AKS'de ulaşmak istediğiniz bölmelerin IP aralığı olmalıdır. 
     - Sonraki hop türü **Sanal Cihaz**olmalıdır. 
     - Sonraki atlama adresi, adres öneki alanında tanımlanan IP aralığındaki bölmeleri barındıran düğümün IP adresi olmalıdır. 
    
  **v2 desteklenmeyen senaryolar**

  **Senaryo 1**: Sanal Cihazlar için UDR

  0.0.0.0/0'ın herhangi bir sanal cihaz, hub/spoke sanal ağ veya şirket içi (zorunlu tünelleme) üzerinden yönlendirilmesi gereken tüm senaryolar V2 için desteklenmez.

## <a name="front-end-ip"></a>Ön uç IP

Uygulama ağ geçidini ortak bir IP adresine, özel bir IP adresine veya her ikisine sahip olacak şekilde yapılandırabilirsiniz. Müşterilerin internete bakan sanal IP (VIP) üzerinden internet üzerinden erişmesi gereken bir arka uç barındırdığınızda genel bir IP gereklidir. 

Kullanıma açık bir IP, internete maruz olmayan bir dahili uç nokta için gerekli değildir. Bu bir iç *yük dengeleyici* (ILB) uç noktası veya özel ön uç IP olarak bilinir. Bir uygulama ağ geçidi ILB, internete maruz olmayan iş yeri uygulamaları için yararlıdır. Ayrıca, internete maruz kalmayan ancak round-robin yük dağıtımı, oturum yapışkanlığı veya TLS sonlandırma gerektiren bir güvenlik sınırı içindeki çok katmanlı bir uygulamadaki hizmetler ve katmanlar için de yararlıdır.

Yalnızca 1 genel IP adresi veya bir özel IP adresi desteklenir. Uygulama ağ geçidini oluştururken ön uç IP'yi seçersiniz.

- Genel bir IP için, yeni bir genel IP adresi oluşturabilir veya uygulama ağ geçidiyle aynı konumda varolan bir genel IP'yi kullanabilirsiniz. Daha fazla bilgi için [statik ve dinamik genel IP adresine](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#static-versus-dynamic-public-ip-address)bakın.

- Özel bir IP için, uygulama ağ geçidinin oluşturulduğu alt ağdan özel bir IP adresi belirtebilirsiniz. Bir tane belirtmezseniz, alt ağdan otomatik olarak rasgele bir IP adresi seçilir. Seçtiğiniz IP adresi türü (statik veya dinamik) daha sonra değiştirilemez. Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/application-gateway/application-gateway-ilb-arm)

Ön uç IP adresi, ön uç IP'deki gelen istekleri denetleyen bir *dinleyiciyle*ilişkilidir.

## <a name="listeners"></a>Dinleyiciler

Dinleyici, gelen bağlantı isteklerini bağlantı noktası, protokol, ana bilgisayar ve IP adresini kullanarak denetleyen mantıksal bir varlıktır. Dinleyiciyi yapılandırdığınızda, ağ geçidindeki gelen istekte karşılık gelen değerlerle eşleşen değerler girmeniz gerekir.

Azure portalını kullanarak bir uygulama ağ geçidi oluşturduğunuzda, dinleyici için protokol ve bağlantı noktasını seçerek varsayılan bir dinleyici de oluşturursunuz. Dinleyicide HTTP2 desteğini etkinleştirip etkinleştirmeyeceğinizi seçebilirsiniz. Uygulama ağ geçidini oluşturduktan sonra, varsayılan dinleyicinin *(appGatewayHttpListener)* ayarlarını veya yeni dinleyiciler oluşturabilirsiniz.

### <a name="listener-type"></a>Dinleyici türü

Yeni bir dinleyici oluşturduğunuzda, [ *temel* ve *çok site arasında*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#types-of-listeners)seçim yapabilirsiniz.

- Tüm isteklerinizin (herhangi bir etki alanı için) kabul edilip arka uç havuzlarına iletilmesini istiyorsanız, temel i seçin. [Temel bir dinleyiciyle nasıl bir uygulama ağ geçidi oluşturabilirsiniz](https://docs.microsoft.com/azure/application-gateway/quick-create-portal)öğrenin.

- İstekleri *ana bilgisayar* üstbilgisini veya ana bilgisayar adına göre farklı arka uç havuzlarına iletmek istiyorsanız, gelen istekle eşleşen bir ana bilgisayar adı da belirtmeniz gereken çok siteli dinleyiciyi seçin. Bunun nedeni, Uygulama Ağ Geçidi'nin aynı genel IP adresi ve bağlantı noktasında birden fazla web sitesini barındırmak için HTTP 1.1 ana bilgisayar üstbilgilerine dayanmasın.

#### <a name="order-of-processing-listeners"></a>İşleme dinleyicisırası

v1 SKU için istekler kuralların sırasına ve dinleyici türüne göre eşleşir. Temel dinleyiciile bir kural sırayla ilk gelirse, önce işlenir ve bu bağlantı noktası ve IP kombinasyonu için herhangi bir isteği kabul eder. Bunu önlemek için, önce çok siteli dinleyicilerle kuralları yapılandırın ve temel dinleyiciyle kuralı listedeki sonuncuya doğru itin.

v2 SKU için, çok siteli dinleyiciler temel dinleyiciler önce işlenir.

### <a name="front-end-ip"></a>Ön uç IP

Bu dinleyiciyle ilişkilendirmeyi planladığınız ön uç IP adresini seçin. Dinleyici bu IP'de gelen istekleri dinleyecek.

### <a name="front-end-port"></a>Ön uç bağlantı noktası

Ön uç bağlantı noktasını seçin. Varolan bir bağlantı noktası seçin veya yeni bir bağlantı noktası oluşturun. [İzin verilen bağlantı noktaları aralığından](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#ports)herhangi bir değer seçin. Yalnızca 80 ve 443 gibi iyi bilinen bağlantı noktalarını değil, uygun olan özel bağlantı noktalarını da kullanabilirsiniz. Bağlantı noktası, genel kullanıma açık dinleyiciler veya özel olarak bakan dinleyiciler için kullanılabilir.

### <a name="protocol"></a>Protokol

HTTP veya HTTPS'yi seçin:

- HTTP'yi seçerseniz, istemci ve uygulama ağ geçidi arasındaki trafik şifrelenmemiş olur.

- [TLS sonlandırma](features.md#secure-sockets-layer-ssltls-termination) veya [uçlardan uca TLS şifrelemeistiyorsanız](https://docs.microsoft.com/azure/application-gateway/ssl-overview)HTTPS'yi seçin. İstemci ve uygulama ağ geçidi arasındaki trafik şifrelenir. Ve TLS bağlantısı uygulama ağ geçidinde sonlandırır. Uçlardan uca TLS şifreleme istiyorsanız, HTTPS'yi seçmeniz ve **arka uç HTTP** ayarını yapılandırmanız gerekir. Bu, uygulama ağ geçidinden arka uca geçerken trafiğin yeniden şifrelenmesini sağlar.


TLS sonlandırma ve uçuça TLS şifrelemesini yapılandırmak için, uygulama ağ geçidinin simetrik bir anahtar elde etmesini sağlamak için dinleyiciye bir sertifika eklemeniz gerekir. Bu, TLS protokol belirtimi tarafından dikte edilir. Simetrik anahtar, ağ geçidine gönderilen trafiği şifrelemek ve çözmek için kullanılır. Ağ geçidi sertifikası Kişisel Bilgi Alışverişi (PFX) biçiminde olmalıdır. Bu biçim, ağ geçidinin trafiği şifrelemek ve şifresini çözmek için kullandığı özel anahtarı dışa aktarmanızı sağlar.

#### <a name="supported-certificates"></a>Desteklenen sertifikalar

[TLS sonlandırma için desteklenen sertifikalara](https://docs.microsoft.com/azure/application-gateway/ssl-overview#certificates-supported-for-ssl-termination)bakın.

### <a name="additional-protocol-support"></a>Ek protokol desteği

#### <a name="http2-support"></a>HTTP2 desteği

HTTP/2 protokol desteği yalnızca uygulama ağ geçidi dinleyicilerine bağlanan istemciler tarafından kullanılabilir. Arka uç sunucu havuzlarına iletişim HTTP/1.1 üzerindedir. Varsayılan olarak, HTTP/2 desteği devre dışı bırakılır. Aşağıdaki Azure PowerShell kod snippet'i bunu nasıl etkinleştirin gösterir:

```azurepowershell
$gw = Get-AzApplicationGateway -Name test -ResourceGroupName hm

$gw.EnableHttp2 = $true

Set-AzApplicationGateway -ApplicationGateway $gw
```

#### <a name="websocket-support"></a>WebSocket desteği

WebSocket desteği varsayılan olarak etkinleştirilir. Etkinleştirmek veya devre dışı kalım getirmek için kullanıcı tarafından yapılandırılabilir ayar yoktur. Hem HTTP hem de HTTPS dinleyicileriyle WebSockets'i kullanabilirsiniz.

### <a name="custom-error-pages"></a>Özel hata sayfaları

Genel düzeyde veya dinleyici düzeyinde özel hata tanımlayabilirsiniz. Ancak Azure portalından genel düzeyde özel hata sayfaları oluşturmak şu anda desteklenmez. Dinleyici düzeyinde 403 web uygulaması güvenlik duvarı hatası veya 502 bakım sayfası için özel bir hata sayfası yapılandırabilirsiniz. Ayrıca, verilen hata durum kodu için herkese açık bir blob URL belirtmeniz gerekir. Daha fazla bilgi için bkz. [Application Gateway özel hata sayfası oluşturma](https://docs.microsoft.com/azure/application-gateway/custom-error).

![Uygulama Ağ Geçidi hata kodları](https://docs.microsoft.com/azure/application-gateway/media/custom-error/ag-error-codes.png)

Genel bir özel hata sayfasını yapılandırmak için [Azure PowerShell yapılandırmasına](https://docs.microsoft.com/azure/application-gateway/custom-error#azure-powershell-configuration)bakın.

### <a name="tls-policy"></a>TLS ilkesi

TLS/SSL sertifika yönetimini merkezileştirebilir ve arka uçtaki bir sunucu çiftliği için şifreleme çözme yükünü azaltabilirsiniz. Merkezi TLS işleme, güvenlik gereksinimlerinize uygun merkezi bir TLS ilkesi belirtmenize de olanak tanır. *Varsayılan,* önceden *tanımlanmış*veya *özel* TLS ilkesini seçebilirsiniz.

TLS iletişim kuralı sürümlerini denetlemek için TLS ilkesini yapılandırırsınız. TLS1.0, TLS1.1 ve TLS1.2'den tls el sıkışmaları için minimum protokol sürümünü kullanmak için bir uygulama ağ geçidi ni yapıyabilirsiniz. Varsayılan olarak, SSL 2.0 ve 3.0 devre dışı bırakılır ve yapılandırılamaz. Daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi TLS ilkesine genel bakış.](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-policy-overview)

Bir dinleyici oluşturduktan sonra, bunu bir istek yönlendirme kuralıyla ilişkilendirin. Bu kural, dinleyiciden alınan isteklerin arka uca nasıl yönlendirildiğini belirler.

## <a name="request-routing-rules"></a>Yönlendirme kuralları isteme

Azure portalını kullanarak bir uygulama ağ geçidi oluşturduğunuzda, varsayılan bir kural *(kural1)* oluşturursunuz. Bu kural varsayılan dinleyiciyi *(appGatewayHttpListener)* varsayılan arka uç havuzu *(appGatewayBackendPool)* ve varsayılan arka uç HTTP ayarları *(appGatewayBackendHttpSettings)* ile bağlar. Ağ geçidini oluşturduktan sonra, varsayılan kuralın ayarlarını ayarlayabilir veya yeni kurallar oluşturabilirsiniz.

### <a name="rule-type"></a>Kural türü

Bir kural oluşturduğunuzda, [ *temel* ve *yol tabanlı*](https://docs.microsoft.com/azure/application-gateway/application-gateway-components#request-routing-rules)arasında seçim yapabilirsiniz.

- İlişkili dinleyicideki tüm istekleri (örneğin, *blog<i></i>.contoso.com/)\** tek bir arka uç havuzuna iletmek istiyorsanız temel i seçin.
- İstekleri belirli URL yollarından belirli arka uç havuzlarına yönlendirmek istiyorsanız yol tabanlı seçin. Yol deseni, sorgu parametrelerine değil, yalnızca URL'nin yoluna uygulanır.

#### <a name="order-of-processing-rules"></a>İşlemkuralları sırası

v1 SKU için, gelen isteklerin desen eşleştirilmesi, yolların yol tabanlı kuralın URL yol haritasında listelenmesi sırasına göre işlenir. Bir istek, yol haritasındaki iki veya daha fazla yoldaki desenle eşleşirse, ilk listelenen yol eşleşir. Ve istek bu yol ile ilişkili arka uça iletilir.

v2 SKU için, tam eşleşme, URL yol haritasındaki yol sırasıile daha yüksek önceliktir. Bir istek desenle iki veya daha fazla yoldaki eşleşirse, istek, istekle tam olarak eşleşen yolla ilişkili arka uca iletilir. Gelen istekteki yol haritadaki herhangi bir yolla tam olarak eşleşmiyorsa, isteğin desen eşleşmesi yol tabanlı kural için yol haritası sıra listesinde işlenir.

### <a name="associated-listener"></a>İlişkili dinleyici

Dinleyiciyle ilişkili *istek yönlendirme kuralının,* isteği yönlendirmek için arka uç havuzunu belirlemek üzere değerlendirilecek şekilde bir dinleyiciyi kurala ilişkilendirin.

### <a name="associated-back-end-pool"></a>İlişkili arka uç havuzu

Kuralı, dinleyicinin aldığı isteklere hizmet veren arka uç hedeflerini içeren arka uç havuzunu ilişkilendirin.

 - Temel bir kural için, yalnızca bir arka uç havuzuna izin verilir. İlişkili dinleyicideki tüm istekler bu arka uç havuzuna iletilir.

 - Yol tabanlı bir kural için, her URL yoluna karşılık gelen birden çok arka uç havuzu ekleyin. Girilen URL yolu ile eşleşen istekler ilgili arka uç havuzuna iletilir. Ayrıca, varsayılan bir arka uç havuzu ekleyin. Kuraldaki herhangi bir URL yoluyla eşleşmeyan istekler bu havuza iletilir.

### <a name="associated-back-end-http-setting"></a>İlişkili arka uç HTTP ayarı

Her kural için bir arka uç HTTP ayarı ekleyin. İstekler, bağlantı noktası numarası, protokol ve bu ayarda belirtilen diğer bilgiler kullanılarak uygulama ağ geçidinden arka uç hedeflerine yönlendirilir.

Temel bir kural için, yalnızca bir arka uç HTTP ayarına izin verilir. İlişkili dinleyicideki tüm istekler bu HTTP ayarını kullanarak ilgili arka uç hedeflerine iletilir.

Yol tabanlı bir kural için, her URL yoluna karşılık gelen birden çok arka uç HTTP ayarları ekleyin. Bu ayarda URL yolu eşleşen istekler, her URL yoluna karşılık gelen HTTP ayarları kullanılarak ilgili arka uç hedeflerine iletilir. Ayrıca, varsayılan bir HTTP ayarı ekleyin. Bu kuraldaki herhangi bir URL yoluyla eşleşmeyan istekler varsayılan HTTP ayarı kullanılarak varsayılan arka uç havuzuna iletilir.

### <a name="redirection-setting"></a>Yeniden yönlendirme ayarı

Yeniden yönlendirme temel bir kural için yapılandırılırsa, ilişkili dinleyicideki tüm istekler hedefe yönlendirilir. Bu *küresel* yönlendirmedir. Yeniden yönlendirme yol tabanlı bir kural için yapılandırılırsa, yalnızca belirli bir site alanındaki istekler yeniden yönlendirilir. Buna örnek olarak */cart/\** ile gösterilen bir alışveriş sepeti alanı verilmiştir. Bu *yol tabanlı* yeniden yönlendirmedir.

Yönlendirmeler hakkında daha fazla bilgi için Bkz. [Uygulama Ağ Geçidi yeniden yönlendirme genel bakış.](redirect-overview.md)

#### <a name="redirection-type"></a>Yeniden yönlendirme türü

Gerekli yönlendirme türünü seçin: *Kalıcı(301)*, *Geçici(307)*, *Bulundu(302)* veya *Diğer (303) bakın.*

#### <a name="redirection-target"></a>Yeniden yönlendirme hedefi

Yeniden yönlendirme hedefi olarak başka bir dinleyici veya harici bir site seçin.

##### <a name="listener"></a>Dinleyici

Trafiği bir dinleyiciden diğerine ağ geçidinde yönlendirmek için yeniden yönlendirme hedefi olarak dinleyiciyi seçin. HTTP-to-HTTPS yeniden yönlendirmesini etkinleştirmek istediğinizde bu ayar gereklidir. Gelen HTTP isteklerini denetleyen kaynak dinleyiciden gelen HTTPS isteklerini denetleyen hedef dinleyiciye trafiği yönlendirir. Ayrıca, yeniden yönlendirme hedefine iletilen isteğe özgün istekten sorgu dizesi ve yolu eklemeyi de seçebilirsiniz.

![Uygulama Ağ Geçidi bileşenleri iletişim kutusu](./media/configuration-overview/configure-redirection.png)

HTTP-to-HTTPS yeniden yönlendirmesi hakkında daha fazla bilgi için bkz:
- [Azure portalını kullanarak HTTP-to-HTTPS yönlendirmesi](redirect-http-to-https-portal.md)
- [PowerShell kullanarak HTTP-to-HTTPS yönlendirmesi](redirect-http-to-https-powershell.md)
- [Azure CLI'yi kullanarak HTTP-to-HTTPS yönlendirmesi](redirect-http-to-https-cli.md)

##### <a name="external-site"></a>Dış site

Bu kuralla ilişkili dinleyiciüzerindeki trafiği harici bir siteye yönlendirmek istediğinizde harici site seçin. Yeniden yönlendirme hedefine iletilen isteğe özgün istekteki sorgu dizesini eklemeyi seçebilirsiniz. Yolu özgün istekte bulunan dış siteye iletemezsiniz.

Yeniden yönlendirme hakkında daha fazla bilgi için bkz:
- [PowerShell'i kullanarak trafiği harici bir siteye yönlendirme](redirect-external-site-powershell.md)
- [CLI'yi kullanarak trafiği harici bir siteye yönlendirme](redirect-external-site-cli.md)

#### <a name="rewrite-the-http-header-setting"></a>HTTP üstbilgi ayarını yeniden yazma

Bu ayar, istek ve yanıt paketleri istemci ve arka uç havuzları arasında hareket ederken HTTP istek ve yanıt üstbilgilerini ekler, kaldırır veya güncelleştirir. Daha fazla bilgi için bkz.

 - [HTTP üstbilgi genel görünümünü yeniden yazın](rewrite-http-headers.md)
 - [HTTP üstbilgi yeniden yapılandırma](rewrite-http-headers-portal.md)

## <a name="http-settings"></a>HTTP ayarları

Uygulama ağ geçidi, burada belirttiğiniz yapılandırmayı kullanarak trafiği arka uç sunucularına yönlendirir. Bir HTTP ayarı oluşturduktan sonra, bir veya daha fazla istek yönlendirme kuralıyla ilişkilendirmeniz gerekir.

### <a name="cookie-based-affinity"></a>Tanımlama bilgisi tabanlı benzeşim

Azure Application Gateway, kullanıcı oturumlarını korumak için ağ geçidi tarafından yönetilen tanımlama bilgileri kullanır. Bir kullanıcı ilk isteği Uygulama Ağ Geçidi'ne gönderdiğinde, yanıtta oturum ayrıntılarını içeren karma bir değerle bir yakınlık çerezi ayarlar, böylece yakınlık çerezini taşıyan sonraki istekler yapışkanlığı korumak için aynı arka uç sunucusuna yönlendirilecektir. 

Bu özellik, bir kullanıcı oturumunu aynı sunucuda tutmak istediğinizde ve oturum durumu bir kullanıcı oturumu için sunucuda yerel olarak kaydedildiğinde yararlıdır. Uygulama çerez tabanlı yakınlığı işleyebilir, bu özelliği kullanamazsınız. Kullanmak için istemcilerin tanımlama bilgilerini desteklediğinden emin olun.

[Krom tarayıcı](https://www.chromium.org/Home) [v80 güncellemesi,](https://chromiumdash.appspot.com/schedule) [SameSite](https://tools.ietf.org/id/draft-ietf-httpbis-rfc6265bis-03.html#rfc.section.5.3.7) özelliği olmayan HTTP tanımlama bilgilerinin SameSite=Lax olarak ele alınması gereken bir görev getirdi. CORS (Cross-Origin Kaynak Paylaşımı) istekleri durumunda, çerezüçüncü taraf bağlamında gönderilmesi gerekiyorsa, *SameSite=None kullanmak zorundadır; Güvenli* öznitelikler ve yalnızca HTTPS üzerinden gönderilmelidir. Aksi takdirde, bir HTTP yalnızca senaryoda, tarayıcı çerezleri üçüncü taraf bağlamında göndermez. Chrome'dan gelen bu güncelleştirmenin amacı güvenliği artırmak ve Siteler Arası İstek Sahteciliği (CSRF) saldırılarını önlemektir. 

Bu değişikliği desteklemek için, 17 Şubat 2020'den itibaren, Application Gateway (tüm SKU türleri) mevcut *ApplicationGatewayAffinity* çerezine ek olarak *ApplicationGatewayAffinityCORS* adlı başka bir çerez enjekte edecektir. *ApplicationGatewayAffinityCORS* çerezine iki özniteliği daha eklendi (*"SameSite=None; Güvenli"*) böylece yapışkan oturum çapraz orijin istekleri için bile korunur.

Varsayılan yakınlık çerez adı *ApplicationGatewayAffinity* olduğunu unutmayın ve değiştirebilirsiniz. Özel bir benzerlik çerez adı kullanıyorsanız, cors ile ek bir çerez eklenir. Örneğin, *CustomCookieNameCORS*.

> [!NOTE]
> Öznitelik *SameSite=None* ayarlanmışsa, çerezin *Güvenli* bayrak da içermesi ve HTTPS üzerinden gönderilmesi zorunludur.  CORS üzerinden oturum yakınlığı gerekiyorsa, iş yükünüzü HTTPS'ye geçirmeniz gerekir. Lütfen burada Uygulama Ağ Geçidi için TLS boşaltma ve Uçtan Uca TLS belgelerine bakın – [Genel Bakış](ssl-overview.md), [Azure portalını kullanarak TLS sonlandırma ile bir uygulama ağ geçidini yapılandırın](create-ssl-portal.md), [Portalla Birlikte Uygulama Ağ Geçidi'ni kullanarak uçtan uca TLS yapılandırın.](end-to-end-ssl-portal.md)

### <a name="connection-draining"></a>Bağlantı boşaltma

Bağlantı boşaltma, planlanan hizmet güncelleştirmeleri sırasında arka uç havuzu üyelerini zarif bir şekilde kaldırmanıza yardımcı olur. Bu ayarı kural oluşturma sırasında bir arka uç havuzunun tüm üyelerine uygulayabilirsiniz. Bir arka uç havuzunun tüm kayıt dışı bırakma örneklerinin varolan bağlantıları korumaya devam etmesini ve yapılandırılabilir bir zaman sonu için devam eden isteklere hizmet etmesini ve yeni istekler veya bağlantılar almamasını sağlar. Bunun tek istisnası, ağ geçidi tarafından yönetilen oturum afiyeti nedeniyle örnekleri niçin silmeye bağlı isteklerdir ve kayıt dışı lık örneklerine iletilmeye devam edecektir. Bağlantı boşaltma, arka uç havuzundan açıkça kaldırılan arka uç örnekleri için geçerlidir.

### <a name="protocol"></a>Protokol

Uygulama Ağ Geçidi, istekleri arka uç sunucularına yönlendirmek için hem HTTP hem de HTTPS'yi destekler. HTTP'yi seçerseniz, arka uç sunucularına gelen trafik şifrelenmemiş tir. Şifrelenmemiş iletişim kabul edilemezse HTTPS'yi seçin.

Dinleyicideki HTTPS ile birleştirilmiş bu ayar [uçtan uca TLS'yi](ssl-overview.md)destekler. Bu, şifrelenmiş hassas verileri güvenli bir şekilde arka uca iletmenizi sağlar. Uçlardan uca TLS etkin olan arka uç havuzundaki her arka uç sunucusu, güvenli iletişim sağlamak için bir sertifikaile yapılandırılmalıdır.

### <a name="port"></a>Bağlantı noktası

Bu ayar, arka uç sunucularının uygulama ağ geçidinden trafiği dinlediği bağlantı noktasını belirtir. 1 ile 65535 arasında değişen bağlantı noktalarını yapılandırabilirsiniz.

### <a name="request-timeout"></a>İstek zaman

Bu ayar, uygulama ağ geçidinin arka uç sunucusundan yanıt almak için beklediği saniye sayısıdır.

### <a name="override-back-end-path"></a>Arka uç yolunu geçersiz kılma

Bu ayar, istek arka uca iletildiğinde kullanmak üzere isteğe bağlı özel bir iletme yolunu yapılandırmanızı sağlar. Gelen yolun **geçersiz kılınan arka uç yolundaki** özel yolla eşleşen herhangi bir bölümü iletilen yola kopyalanır. Aşağıdaki tablo, bu özelliğin nasıl çalıştığını gösterir:

- HTTP ayarı temel bir istek yönlendirme kuralına eklendiğinde:

  | Orijinal istek  | Arka uç yolunu geçersiz kılma | İstem arka uca iletildi |
  | ----------------- | --------------------- | ---------------------------- |
  | /home/            | /geçersiz kılma/            | /override/home/              |
  | /home/secondhome/ | /geçersiz kılma/            | /override/home/secondhome/   |

- HTTP ayarı yol tabanlı istek yönlendirme kuralına eklendiğinde:

  | Orijinal istek           | Yol kuralı       | Arka uç yolunu geçersiz kılma | İstem arka uca iletildi |
  | -------------------------- | --------------- | --------------------- | ---------------------------- |
  | /pathrule/home/            | /pathrule*      | /geçersiz kılma/            | /override/home/              |
  | /pathrule/home/secondhome/ | /pathrule*      | /geçersiz kılma/            | /override/home/secondhome/   |
  | /home/                     | /pathrule*      | /geçersiz kılma/            | /override/home/              |
  | /home/secondhome/          | /pathrule*      | /geçersiz kılma/            | /override/home/secondhome/   |
  | /pathrule/home/            | /pathrule/home* | /geçersiz kılma/            | /geçersiz kılma/                   |
  | /pathrule/home/secondhome/ | /pathrule/home* | /geçersiz kılma/            | /geçersiz kılma/secondhome/        |
  | /pathrule/                 | /pathrule/      | /geçersiz kılma/            | /geçersiz kılma/                   |

### <a name="use-for-app-service"></a>Uygulama hizmeti için kullanın

Bu, Azure Uygulama Hizmeti için gerekli iki ayarı seçen bir UI yalnızca kısayoldur. **Arka uç adresinden ana bilgisayar adını seçmeyi**sağlar ve zaten bir tane yoksa yeni bir özel sonda oluşturur. (Daha fazla bilgi için, bu makalenin arka uç adresi ayarı [bölümünden ana bilgisayar adını seç](#pick) bölümüne bakın.) Yeni bir sonda oluşturulur ve sonda başlığı arka uç üyesinin adresinden seçilir.

### <a name="use-custom-probe"></a>Özel sonda kullanma

Bu ayar, özel bir [sondayı](application-gateway-probe-overview.md#custom-health-probe) bir HTTP ayarı ile ilişkilendirer. Yalnızca bir özel sondayı bir HTTP ayarı ile ilişkilendirebilirsiniz. Özel bir [sondayı](application-gateway-probe-overview.md#default-health-probe-settings) açıkça ilişkilendirmiyorsanız, varsayılan sonda arka uç durumunu izlemek için kullanılır. Arka uçlarınızın sistem durumu izleme üzerinde daha fazla denetim için özel bir sonda oluşturmanızı öneririz.

> [!NOTE]
> Özel sonda, ilgili HTTP ayarı açıkça bir dinleyiciyle ilişkilendirilmedikçe arka uç havuzunun durumunu izlemez.

### <a name="pick-host-name-from-back-end-address"></a><a id="pick"/></a>Arka uç adresinden ana bilgisayar adını seçme

Bu özellik, arka uç havuzunun ana bilgisayar adına istek ana *bilgisayar* üstbilgisini dinamik olarak ayarlar. Bir IP adresi veya FQDN kullanır.

Bu özellik, arka uç etki alanı adı uygulama ağ geçidinin DNS adından farklı olduğunda yardımcı olur ve arka uç doğru bitiş noktasına çözmek için belirli bir ana bilgisayar üstbilgigüvenir.

Örnek bir örnek arka uç olarak çok kiracı hizmetleridir. Uygulama hizmeti, tek bir IP adresiolan paylaşılan bir alanı kullanan çok kiracılı bir hizmettir. Bu nedenle, bir uygulama hizmetine yalnızca özel etki alanı ayarlarında yapılandırılan ana bilgisayar adları üzerinden erişilebilir.

Varsayılan olarak, özel etki alanı adı *example.azurewebsites.net.* Uygulama hizmetinde açıkça kayıtlı olmayan bir ana bilgisayar adı veya uygulama ağ geçidinin FQDN'si aracılığıyla bir uygulama ağ geçidi kullanarak uygulama hizmetinize erişmek için, uygulama hizmetinin ana bilgisayarına orijinal istekteki ana bilgisayarı geçersiz kılarsınız. Bunu yapmak için, **arka uç adres ayarından ana bilgisayar adını seçin.**

Varolan özel DNS adı uygulama hizmetine eşlenen özel bir etki alanı için bu ayarı etkinleştirmeniz gerekmez.

> [!NOTE]
> Bu ayar, özel bir dağıtım olan Uygulama Hizmet Ortamı için gerekli değildir.

### <a name="host-name-override"></a>Ana bilgisayar adı geçersiz kılma

Bu özellik, uygulama ağ geçidinde gelen istekteki *ana bilgisayar* üstbilgisini belirttiğiniz ana bilgisayar adı ile değiştirir.

Örneğin, ana **bilgisayar adı** ayarında *www.contoso.com* belirtilirse, istek`https://www.contoso.com/path1` arka uç sunucusuna iletildiğinde özgün istek *`https://appgw.eastus.cloudapp.azure.com/path1` * olarak değiştirilir.

## <a name="back-end-pool"></a>Arka uç havuzu

Bir arka uç havuzudört tür arka uç üyesine işaret edebilirsiniz: belirli bir sanal makine, sanal makine ölçek kümesi, IP adresi/FQDN veya bir uygulama hizmeti. 

Bir arka uç havuzu oluşturduktan sonra, bunu bir veya daha fazla istek yönlendirme kuralıyla ilişkilendirmeniz gerekir. Ayrıca, uygulama ağ geçidinizdeki her arka uç havuzu için sistem durumu sondalarını yapılandırmanız gerekir. İstek yönlendirme kuralı koşulu karşılandığında, uygulama ağ geçidi trafiği ilgili arka uç havuzunda (sistem durumu sondaları tarafından belirlendiği gibi) sağlıklı sunuculara iletir.

## <a name="health-probes"></a>Sistem durumu araştırmaları

Uygulama ağ geçidi varsayılan olarak arka ucundaki tüm kaynakların durumunu izler. Ancak, sistem durumu izleme üzerinde daha fazla denetim elde etmek için her arka uç HTTP ayarı için özel bir sonda oluşturmanızı şiddetle öneririz. Özel bir sondayı nasıl yapılandırılabildiğini öğrenmek için [Özel sistem durumu sondası ayarlarına](application-gateway-probe-overview.md#custom-health-probe-settings)bakın.

> [!NOTE]
> Özel bir sistem durumu sondası oluşturduktan sonra, bunu bir arka uç HTTP ayarına ilişkilendirmeniz gerekir. Özel bir sonda, ilgili HTTP ayarı açıkça bir kural kullanarak bir dinleyici ile ilişkili olmadığı sürece arka uç havuzunun durumunu izlemez.

## <a name="next-steps"></a>Sonraki adımlar

Artık Uygulama Ağ Geçidi bileşenlerini bildiğinize göre şunları yapabilirsiniz:

- [Azure portalında bir uygulama ağ geçidi oluşturma](quick-create-portal.md)
- [PowerShell'i kullanarak bir uygulama ağ geçidi oluşturma](quick-create-powershell.md)
- [Azure CLI'yi kullanarak bir uygulama ağ geçidi oluşturma](quick-create-cli.md)
