---
title: Operatör en iyi uygulamaları - Azure Kubernetes Hizmetlerinde ağ bağlantısı (AKS)
description: Azure Kubernetes Hizmeti'nde (AKS) sanal ağ kaynakları ve bağlantı için küme operatörünün en iyi uygulamaları öğrenin
services: container-service
ms.topic: conceptual
ms.date: 12/10/2018
ms.openlocfilehash: 93659a0891b09c83db9f63fe0756fcf4d7e87f6a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77594694"
---
# <a name="best-practices-for-network-connectivity-and-security-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) hizmetinde ağ bağlantısı ve güvenlik için en iyi yöntemler

Azure Kubernetes Hizmeti'nde (AKS) kümeler oluşturur ken ve yönetirken, düğümleriniz ve uygulamalarınız için ağ bağlantısı sağlarsınız. Bu ağ kaynakları IP adres aralıklarını, yük dengeleyicilerini ve giriş denetleyicilerini içerir. Uygulamalarınız için yüksek hizmet kalitesini korumak için, bu kaynakları planlamanız ve sonra yapılandırmanız gerekir.

Bu en iyi uygulamalar makalesi, küme işleçleri için ağ bağlantısı ve güvenliğine odaklanır. Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * AKS'de kubenet ve Azure CNI ağ modlarını karşılaştırın
> * Gerekli IP adresleme ve bağlantı için plan
> * Yük dengeleyicileri, giriş denetleyicileri veya bir web uygulaması güvenlik duvarı (WAF) kullanarak trafiği dağıtma
> * Küme düğümlerine güvenli bir şekilde bağlanın

## <a name="choose-the-appropriate-network-model"></a>Uygun ağ modelini seçin

**En iyi uygulama kılavuzu** - Mevcut sanal ağlarla veya şirket içi ağlarla tümleştirme için AKS'de Azure CNI ağlarını kullanın. Bu ağ modeli, kurumsal bir ortamda kaynakların ve denetimlerin daha fazla ayrılmasına da olanak tanır.

Sanal ağlar, AKS düğümlerinin ve müşterilerinin uygulamalarınıza erişmeleri için temel bağlantı sağlar. AKS kümelerini sanal ağlara dağıtmanın iki farklı yolu vardır:

* **Kubenet ağı** - Azure, küme dağıtılırken sanal ağ kaynaklarını yönetir ve [kubenet][kubenet] Kubernetes eklentisini kullanır.
* **Azure CNI ağı** - Varolan bir sanal ağa dağıtır ve [Azure Kapsayıcı Ağ Arabirimi (CNI)][cni-networking] Kubernetes eklentisini kullanır. Podlar, diğer ağ hizmetlerine veya şirket içi kaynaklara yönlendirebilen tek tek IP'ler alır.

Kapsayıcı Ağ Arabirimi (CNI), kapsayıcı çalışma zamanının bir ağ sağlayıcısına istekte bulunmasını sağlayan satıcıdan bağımsız bir iletişim kuralıdır. Azure CNI, podlara ve düğümlere IP adresleri atar ve mevcut Azure sanal ağlarına bağlandığınızda IP adresi yönetimi (IPAM) özellikleri sağlar. Her düğüm ve bölme kaynağı Azure sanal ağında bir IP adresi alır ve diğer kaynaklar veya hizmetlerle iletişim kurmak için ek yönlendirme gerekmez.

![Her biri tek bir Azure VNet'e bağlanan köprüleri olan iki düğümü gösteren diyagram](media/operator-best-practices-network/advanced-networking-diagram.png)

Çoğu üretim dağıtımında Azure CNI ağlarını kullanmanız gerekir. Bu ağ modeli, kaynakların denetiminin ve yönetiminin ayrılmasına olanak tanır. Güvenlik açısından bakıldığında, genellikle farklı ekiplerin bu kaynakları yönetmesini ve güvenliğini sağlamasını istersiniz. Azure CNI ağı, mevcut Azure kaynaklarına, şirket içi kaynaklara veya diğer hizmetlere doğrudan her bir bölmeye atanan IP adresleri aracılığıyla bağlanmanıza olanak tanır.

Azure CNI ağını kullandığınızda, sanal ağ kaynağı AKS kümesine ayrı bir kaynak grubundadır. AKS hizmet sorumlusunun bu kaynaklara erişme ve yönetme izinlerini devredin. AKS kümesi tarafından kullanılan hizmet sorumlusunun, sanal ağınızdaki alt ağda en az [Ağ Katılımcısı](../role-based-access-control/built-in-roles.md#network-contributor) izinleri olmalıdır. Yerleşik Ağ Katılımcısı rolünü kullanmak yerine özel bir [rol](../role-based-access-control/custom-roles.md) tanımlamak istiyorsanız, aşağıdaki izinler gereklidir:
  * `Microsoft.Network/virtualNetworks/subnets/join/action`
  * `Microsoft.Network/virtualNetworks/subnets/read`

AKS hizmet ana delegasyonu hakkında daha fazla bilgi için, [diğer Azure kaynaklarına Temsilci erişimine][sp-delegation]bakın.

Her düğüm ve bölme kendi IP adresini aldığında, AKS alt ağlarının adres aralıklarını planlayın. Alt ağ, dağıttığınız her düğüm, bölme ve ağ kaynakları için IP adresleri sağlayacak kadar büyük olmalıdır. Her AKS kümesi kendi alt ağına yerleştirilmelidir. Azure'da şirket içi veya denetlenen ağlara bağlantı sağlamak için, varolan ağ kaynaklarıyla çakışan IP adresi aralıklarını kullanmayın. Her düğümün hem kubenet hem de Azure CNI ağlarıyla çalıştığı bölme sayısı için varsayılan sınırlar vardır. Olayları veya küme yükseltmelerini ölçeklendirmek için, atanan alt ağda kullanılmak üzere ek IP adreslerine de ihtiyacınız vardır. Bu düğüm havuzları en son güvenlik düzeltme emüleklerini uygulamak için yükseltme gerektirdiğinden, Windows Server kapsayıcılarını (şu anda AKS'de önizlemede) kullanıyorsanız, bu ek adres alanı özellikle önemlidir. Windows Server düğümleri hakkında daha fazla bilgi için [AKS'deki düğüm havuzunun yükselti'ne][nodepool-upgrade]bakın.

Gerekli IP adresini hesaplamak için [AKS'de Azure CNI ağyapılandırma'ya][advanced-networking]bakın.

### <a name="kubenet-networking"></a>Kubenet ağ

Kubenet küme dağıtılmadan önce sanal ağları kurmanızı gerektirmese de, dezavantajları vardır:

* Düğümler ve bölmeler farklı IP alt ağlarına yerleştirilir. Podlar ve düğümler arasındaki trafiği yönlendirmek için Kullanıcı Tanımlı Yönlendirme (UDR) ve IP yönlendirme kullanılır. Bu ek yönlendirme ağ performansını azaltabilir.
* Varolan şirket içi ağlara bağlantılar veya diğer Azure sanal ağlarına bakma karmaşık olabilir.

Sanal ağı ve alt ağları AKS kümesinden ayrı olarak oluşturmanız gerekmeden Kubenet küçük geliştirme veya test iş yükleri için uygundur. Düşük trafiğe sahip veya iş yüklerini konteynerlere kaldırmak ve kaydırmak için basit web siteleri, kubenet ağ ile dağıtılan AKS kümelerinin basitliğinden de yararlanabilir. Çoğu üretim dağıtımı için Azure CNI ağını planlamalı ve kullanmalısınız. Ayrıca [kubenet kullanarak kendi IP adresi aralıkları ve sanal ağlar yapılandırabilirsiniz.][aks-configure-kubenet-networking]

## <a name="distribute-ingress-traffic"></a>Giriş trafiğini dağıtma

**En iyi uygulama kılavuzu** - HTTP veya HTTPS trafiğini uygulamalarınız için dağıtmak için giriş kaynaklarını ve denetleyicilerini kullanın. Giriş denetleyicileri normal bir Azure yük dengeleyicisi üzerinde ek özellikler sağlar ve yerel Kubernetes kaynakları olarak yönetilebilir.

Azure yük dengeleyicisi, aks kümenizdeki uygulamalara müşteri trafiğini dağıtabilir, ancak bu trafikhakkında ne anladığıyla sınırlıdır. Yük dengeleyici kaynağı katman 4'te çalışır ve trafiği protokol veya bağlantı noktalarına göre dağıtır. HTTP veya HTTPS kullanan çoğu web uygulaması, katman 7'de çalışan Kuberenetes giriş kaynaklarını ve denetleyicilerini kullanmalıdır. Giriş, trafiği uygulamanın URL'sine göre dağıtabilir ve TLS/SSL sonlandırma işlemini işleyebilir. Bu yetenek, ortaya çıkardığınız ve haritaladığınız IP adreslerinin sayısını da azaltır. Bir yük dengeleyicisi ile, her uygulama genellikle aks kümesinde hizmete atanmış ve eşlenen bir genel IP adresi ne ihtiyaç duyar. Giriş kaynağıyla, tek bir IP adresi trafiği birden çok uygulamaya dağıtabilir.

![AKS kümesindeki Giriş trafik akışını gösteren diyagram](media/operator-best-practices-network/aks-ingress.png)

 Giriş için iki bileşen vardır:

 * Bir giriş *kaynağı*ve
 * Giriş *denetleyicisi*

Giriş kaynağı, aks kümenizde çalışan hizmetlere trafiği yönlendirmek için ana bilgisayarı, sertifikaları ve kuralları tanımlayan bir YAML bildirimidir. `kind: Ingress` Aşağıdaki örnek YAML bildirimi iki hizmet, *blogservice* veya *storeservice*birine *myapp.com* için trafik dağıtacak. Müşteri, eriştisi olduğu URL'ye bağlı olarak bir hizmete veya diğerine yönlendirilir.

```yaml
kind: Ingress
metadata:
 name: myapp-ingress
   annotations: kubernetes.io/ingress.class: "PublicIngress"
spec:
 tls:
 - hosts:
   - myapp.com
   secretName: myapp-secret
 rules:
   - host: myapp.com
     http:
      paths:
      - path: /blog
        backend:
         serviceName: blogservice
         servicePort: 80
      - path: /store
        backend:
         serviceName: storeservice
         servicePort: 80
```

Giriş denetleyicisi, AKS düğümünde çalışan ve gelen istekleri izleyen bir daemondur. Trafik daha sonra giriş kaynağında tanımlanan kurallara göre dağıtılır. En yaygın giriş [denetleyicisi NGINX'e]dayanır. AKS belirli bir denetleyici ile kısıtlamaz, bu nedenle [Kontur,][contour] [HAProxy][haproxy]veya [Traefik][traefik]gibi diğer denetleyicileri kullanabilirsiniz.

Giriş denetleyicileri bir Linux düğümü üzerinde zamanlanmalıdır. Windows Server düğümleri (şu anda AKS önizlemede) giriş denetleyicisini çalıştırmamalıdır. Kaynağın Linux tabanlı bir düğümüzerinde çalışması gerektiğini belirtmek için YAML bildiriminizde veya Miğfer grafiği dağıtımınızda bir düğüm seçici kullanın. Daha fazla bilgi için [bakınız AKS'de bölmelerin nerede zamanlandığını denetlemek için düğüm seçicileri kullanın.][concepts-node-selectors]

Giriş için aşağıdaki nasıl yapılır kılavuzları da dahil olmak üzere birçok senaryo vardır:

* [Harici ağ bağlantısı na sahip temel bir giriş denetleyicisi oluşturma][aks-ingress-basic]
* [Dahili, özel ağ ve IP adresi kullanan bir giriş denetleyicisi oluşturma][aks-ingress-internal]
* [Kendi TLS sertifikalarınızı kullanan bir giriş denetleyicisi oluşturun][aks-ingress-own-tls]
* [Dinamik bir genel IP adresiyle][aks-ingress-tls] veya statik bir genel IP [adresiyle][aks-ingress-static-tls] TLS sertifikaları otomatik olarak oluşturmak için Let's Encrypt'i kullanan bir giriş denetleyicisi oluşturun

## <a name="secure-traffic-with-a-web-application-firewall-waf"></a>Bir web uygulaması güvenlik duvarı (WAF) ile güvenli trafik

**En iyi uygulama kılavuzu** - Olası saldırılar için gelen trafiği tarayan için Azure veya Azure Uygulama Ağ Geçidi [için Barracuda WAF][barracuda-waf] gibi bir web uygulaması güvenlik duvarı (WAF) kullanın. Bu daha gelişmiş ağ kaynakları trafiği sadece HTTP ve HTTPS bağlantılarının veya temel SSL sonlandırmanın ötesine yönlendirebilir.

Trafiği hizmetlere ve uygulamalara dağıtan bir giriş denetleyicisi genellikle AKS kümenizdeki bir Kubernetes kaynağıdır. Denetleyici, AKS düğümünde daemon olarak çalışır ve düğümün CPU, bellek ve ağ bant genişliği gibi bazı kaynaklarını tüketir. Daha büyük ortamlarda, genellikle bu trafik yönlendirmesinin veya TLS sonlandırmasının bir kısmını AKS kümesi dışındaki bir ağ kaynağına boşaltmak istersiniz. Ayrıca, gelen trafiği olası saldırılar için taramaya da istiyorsunuz.

![Azure Uygulama Ağ Geçidi gibi bir web uygulaması güvenlik duvarı (WAF), AKS kümeniz için trafiği koruyabilir ve dağıtabilir](media/operator-best-practices-network/web-application-firewall-app-gateway.png)

Bir web uygulaması güvenlik duvarı (WAF), gelen trafiği filtreleyerek ek bir güvenlik katmanı sağlar. Açık Web Uygulama Güvenliği Projesi (OWASP), site ler arası komut dosyası açma veya çerez zehirlenmesi gibi saldırılara karşı izlenmesi gereken bir dizi kural sağlar. [Azure Uygulama Ağ Geçidi][app-gateway] (şu anda AKS'de önizlemede) trafik AKS kümenize ve uygulamalarınıza ulaşmadan önce bu güvenlik özelliklerini sağlamak için AKS kümeleriyle tümleştirebilen bir WAF'dır. Diğer üçüncü taraf çözümleri de bu işlevleri gerçekleştirir, böylece belirli bir üründeki varolan yatırımları veya uzmanlığı kullanmaya devam edebilirsiniz.

Yük dengeleyici veya giriş kaynakları, trafik dağılımını daha da hassaslaştırmak için AKS kümenizde çalışmaya devam eder. App Gateway merkezi bir kaynak tanımı ile bir giriş denetleyicisi olarak yönetilebilir. Başlamak için [bir Uygulama Ağ Geçidi Giriş denetleyicisi oluşturun.][app-gateway-ingress]

## <a name="control-traffic-flow-with-network-policies"></a>Ağ ilkeleriyle trafik akışını denetleme

**En iyi uygulama kılavuzu** - Bölmelere trafik izni vermek veya reddetmek için ağ ilkelerini kullanın. Varsayılan olarak, küme içindeki bölmeler arasında tüm trafiğe izin verilir. Geliştirilmiş güvenlik için, pod iletişimini sınırlayan kuralları tanımlayın.

Ağ ilkesi, bölmeler arasındaki trafik akışını denetlemenize olanak tanıyan bir Kubernetes özelliğidir. Atanan etiketler, ad alanı veya trafik bağlantı noktası gibi ayarlara göre trafiğe izin vermeyi veya reddetmeyi seçebilirsiniz. Ağ ilkelerinin kullanımı, trafik akışını denetlemek için buluta özgü bir yol sağlar. Bir AKS kümesinde bölmedinamik olarak oluşturulduğundan, gerekli ağ ilkeleri otomatik olarak uygulanabilir. Pod-pod trafiğini denetlemek için Azure ağ güvenlik gruplarını kullanmayın, ağ ilkelerini kullanın.

Ağ ilkesini kullanmak için, bir AKS kümesi oluştururken özelliğin etkinleştirilmesi gerekir. Varolan bir AKS kümesinde ağ ilkesini etkinleştiremezsiniz. Kümeler üzerinde ağ ilkesini etkinleştirdiğinizden ve gerektiğinde kullanabileceğinden emin olmak için önceden plan yapın. Ağ ilkesi yalnızca AKS'deki Linux tabanlı düğümler ve bölmeler için kullanılmalıdır.

Bir ağ ilkesi, YAML bildirimi kullanılarak Kubernetes kaynağı olarak oluşturulur. İlkeler tanımlı bölmelere uygulanır, ardından giriş veya çıkış kuralları trafiğin nasıl akabileceğini tanımlar. Aşağıdaki örnek, uygulamayla birlikte bölmelere bir ağ ilkesi *uygular:* bunlara uygulanan arka uç etiketi. Giriş kuralı daha sonra yalnızca uygulama ile bölmelerden trafiğe izin *verir: ön yüz* etiketi:

```yaml
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  name: backend-policy
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: frontend
```

İlkelere başlamak için Azure [Kubernetes Hizmeti'nde (AKS) ağ ilkelerini kullanan bölmeler arasındaki güvenli trafiği][use-network-policies]görün.

## <a name="securely-connect-to-nodes-through-a-bastion-host"></a>Burç ana bilgisayar aracılığıyla düğümlere güvenli bir şekilde bağlanın

**En iyi uygulama kılavuzu** - AKS düğümlerinize uzaktan bağlantı göstermeyin. Bir yönetim sanal ağında bir burç ana bilgisayar veya atlama kutusu oluşturun. Trafiği AKS kümenize güvenli bir şekilde uzaktan yönetim görevlerine yönlendirmek için burç ana bilgisayarını kullanın.

AKS'deki işlemlerin çoğu Azure yönetim araçları kullanılarak veya Kubernetes API sunucusu aracılığıyla tamamlanabilir. AKS düğümleri genel internete bağlı değildir ve yalnızca özel bir ağda kullanılabilir. Düğümlere bağlanmak ve bakım veya sorun giderme sorunlarını gerçekleştirmek için bağlantılarınızı bir burç ana bilgisayarı veya atlama kutusu üzerinden yönlendirin. Bu ana bilgisayar, AKS küme sanal ağına güvenli bir şekilde bakan ayrı bir yönetim sanal ağında olmalıdır.

![Bir burç ana bilgisayar veya atlama kutusu kullanarak AKS düğümlerine bağlanma](media/operator-best-practices-network/connect-using-bastion-host-simplified.png)

Burç ana bilgisayarının yönetim ağı da güvence altına alınmalıdır. Şirket içi bir ağa bağlanmak için [Azure ExpressRoute][expressroute] veya [VPN ağ geçidi][vpn-gateway] kullanın ve ağ güvenlik gruplarını kullanarak erişimi denetle.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede ağ bağlantısı ve güvenlik üzerinde duruldu. Kubernetes'teki ağ temelleri hakkında daha fazla bilgi için Azure [Kubernetes Hizmeti'ndeki (AKS) uygulamalar için Ağ kavramlarına][aks-concepts-network] bakın

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[app-gateway-ingress]: https://github.com/Azure/application-gateway-kubernetes-ingress
[nginx]: https://www.nginx.com/products/nginx/kubernetes-ingress-controller
[contour]: https://github.com/heptio/contour
[haproxy]: https://www.haproxy.org
[traefik]: https://github.com/containous/traefik
[barracuda-waf]: https://www.barracuda.com/products/webapplicationfirewall/models/5

<!-- INTERNAL LINKS -->
[aks-concepts-network]: concepts-network.md
[sp-delegation]: kubernetes-service-principal.md#delegate-access-to-other-azure-resources
[expressroute]: ../expressroute/expressroute-introduction.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[aks-ingress-internal]: ingress-internal-ip.md
[aks-ingress-static-tls]: ingress-static-ip.md
[aks-ingress-basic]: ingress-basic.md
[aks-ingress-tls]: ingress-tls.md
[aks-ingress-own-tls]: ingress-own-tls.md
[app-gateway]: ../application-gateway/overview.md
[use-network-policies]: use-network-policies.md
[advanced-networking]: configure-azure-cni.md
[aks-configure-kubenet-networking]: configure-kubenet.md
[concepts-node-selectors]: concepts-clusters-workloads.md#node-selectors
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool