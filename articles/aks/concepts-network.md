---
title: Kavramlar - Azure Kubernetes Hizmetlerinde Ağ Oluşturma (AKS)
description: Kubenet ve Azure CNI ağ, giriş denetleyicileri, yük dengeleyicileri ve statik IP adresleri dahil olmak üzere Azure Kubernetes Hizmeti'nde (AKS) ağ ağı hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 02/28/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: 5800254ab44b5b0f1048ce2200f90c06a8d1666a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253942"
---
# <a name="network-concepts-for-applications-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti 'ndeki (AKS) uygulamalar için ağ kavramları

Uygulama geliştirmeye kapsayıcı tabanlı mikro hizmetler yaklaşımında, uygulama bileşenleri görevlerini işlemek için birlikte çalışmalıdır. Kubernetes bu uygulama iletişimi sağlayan çeşitli kaynaklar sağlar. Uygulamalara dahili veya harici olarak bağlanabilir ve bunları ortaya çıkarabilirsiniz. Yüksek kullanılabilir uygulamalar oluşturmak için, uygulamalarınızı dengeyükleyebilirsiniz. Daha karmaşık uygulamalar, Birden çok bileşenin SSL/TLS sonlandırılması veya yönlendirmesi için giriş trafiğinin yapılandırmasını gerektirebilir. Güvenlik nedenleriyle, ağ trafiğinin bölmelere ve düğümlere veya bunlar arasına akışını kısıtlamanız da gerekebilir.

Bu makalede, AKS uygulamalarınıza ağ sağlayan temel kavramlar tanıtışlar:

- [Hizmetler](#services)
- [Azure sanal ağlar](#azure-virtual-networks)
- [Giriş denetleyicileri](#ingress-controllers)
- [Ağ ilkeleri](#network-policies)

## <a name="kubernetes-basics"></a>Kubernetes hakkında temel bilgiler

Kubernetes, uygulamalarınız için erişime izin vermek veya uygulama bileşenlerinin birbiriyle iletişim kurabilmesi için sanal ağa bir soyutlama katmanı sağlar. Kubernetes düğümleri sanal bir ağa bağlıdır ve bölmeler için gelen ve giden bağlantı sağlayabilir. *Kube-proxy* bileşeni bu ağ özelliklerini sağlamak için her düğümüzerinde çalışır.

Kubernetes'te, *Hizmetler* bir IP adresi veya DNS adı üzerinden ve belirli bir bağlantı noktasında doğrudan erişime izin vermek için bölmeleri mantıksal olarak gruplandırır. Ayrıca *bir yük dengeleyici*kullanarak trafik dağıtabilirsiniz. Giriş Denetleyicileri ile uygulama trafiğinin daha karmaşık *yönlendirmesi*de sağlanabilir. Kubernetes *ağ ilkeleri*ile bölmeler için ağ trafiğinin güvenliği ve filtrelemi mümkündür.

Azure platformu, AKS kümeleri için sanal ağ landırmayı basitleştirmeye de yardımcı olur. Bir Kubernetes yük dengeleyicisi oluşturduğunuzda, temel Azure yük dengeleyici sayılsa da, temel Azure yük dengeleyicisi kaynağı oluşturulur ve yapılandırılır. Ağ bağlantı noktalarını bölmelere açtıkça, ilgili Azure ağ güvenlik grubu kuralları yapılandırılır. HTTP uygulama yönlendirmesi için Azure, yeni giriş yolları yapılandırıldıkça *harici DNS'yi* de yapılandırabilir.

## <a name="services"></a>Hizmetler

Uygulama iş yükleri için ağ yapılandırmasını kolaylaştırmak için Kubernetes, bir bölme kümesini mantıksal olarak gruplandırmak ve ağ bağlantısı sağlamak için *Hizmetleri* kullanır. Aşağıdaki Hizmet türleri kullanılabilir:

- **Cluster IP** - AKS kümesi içinde kullanılmak üzere dahili bir IP adresi oluşturur. Kümedeki diğer iş yüklerini destekleyen yalnızca dahili uygulamalar için iyidir.

    ![Aks kümesindeki Küme IP trafik akışını gösteren diyagram][aks-clusterip]

- **NodePort** - Uygulamanın doğrudan düğüm IP adresi ve bağlantı noktası ile erişilmesine olanak tanıyan temel düğüm üzerinde bir bağlantı noktası eşleme oluşturur.

    ![Aks kümesindeki NodePort trafik akışını gösteren diyagram][aks-nodeport]

- **LoadBalancer** - Bir Azure yük dengeleyici kaynağı oluşturur, harici bir IP adresi yapılandırır ve istenen bölmeleri yük dengeleyici arka uç havuzuna bağlar. Müşterilerin trafiğinin uygulamaya ulaşmasını sağlamak için, istenilen bağlantı noktalarında yük dengeleme kuralları oluşturulur. 

    ![AKS kümesindeki Yük Dengeleyici trafik akışını gösteren diyagram][aks-loadbalancer]

    Ek denetim ve gelen trafiğin yönlendirmesi için, bunun yerine bir [Giriş denetleyicisi](#ingress-controllers)kullanabilirsiniz.

- **ExternalName** - Daha kolay uygulama erişimi için belirli bir DNS girişi oluşturur.

Yük dengeleyicileri ve hizmetleri için IP adresi dinamik olarak atanabilir veya kullanılacak varolan statik bir IP adresi belirtebilirsiniz. Hem iç hem de dış statik IP adresleri atanabilir. Bu varolan statik IP adresi genellikle bir DNS girişine bağlıdır.

Hem *iç* hem de *dış* yük dengeleyicileri oluşturulabilir. Dahili yük dengeleyicilerine yalnızca özel bir IP adresi atanmıştır, bu nedenle Internet'ten erişilemezler.

## <a name="azure-virtual-networks"></a>Azure sanal ağları

AKS'de aşağıdaki iki ağ modelinden birini kullanan bir küme dağıtabilirsiniz:

- *Kubenet* ağ - Aks kümesi dağıtıldıkça ağ kaynakları genellikle oluşturulur ve yapılandırılır.
- *Azure Kapsayıcı Ağ Arabirimi (CNI)* ağ - AKS kümesi varolan sanal ağ kaynaklarına ve yapılandırmalarına bağlıdır.

### <a name="kubenet-basic-networking"></a>Kubenet (temel) ağ

*Kubenet* ağ seçeneği AKS küme oluşturma için varsayılan yapılandırmadır. *Kubenet*ile düğümler Azure sanal ağ alt netinden bir IP adresi alır. Podlar, düğümlerin Azure sanal ağ alt ağından mantıksal olarak farklı bir adres alanından IP adresi alır. Ardından podların Azure sanal ağındaki kaynaklara erişebilmesi için ağ adresi çevirisi (NAT) yapılandırması gerçekleştirilir. Trafiğin kaynak IP adresi düğümün birincil IP adresine NAT'd'dir.

Düğümler [kubenet][kubenet] Kubernetes eklentisini kullanır. Azure platformunun sizin için sanal ağları oluşturmasına ve yapılandırmasına izin verebilir veya AKS kümenizi varolan bir sanal ağ alt ağına dağıtmayı seçebilirsiniz. Yine, yalnızca düğümler bir routable IP adresi alırsınız ve bölmeler AKS kümesi dışındaki diğer kaynaklarla iletişim kurmak için NAT kullanır. Bu yaklaşım, bölmelerin kullanabilmesi için ağ alanınızda ayırmanız gereken IP adreslerinin sayısını büyük ölçüde azaltır.

Daha fazla bilgi için, [bir AKS kümesi için kubenet ağ yapılandırma'ya][aks-configure-kubenet-networking]bakın.

### <a name="azure-cni-advanced-networking"></a>Azure CNI (gelişmiş) ağ

Azure CNI seçeneğinde her pod, alt ağdan doğrudan erişilebilen bir IP adresi alır. Bu IP adresleri ağ alanınızda benzersiz olmalıdır ve önceden planlanmalıdır. Her düğüm, desteklediği en fazla bölme sayısı için bir yapılandırma parametresi vardır. Düğüm başına eşdeğer IP adresi sayısı, bu düğüm için ön tarafta ayrılmıştır. Bu yaklaşım, ip adresi tükenmesine veya uygulama talepleriniz arttıkça kümeleri daha büyük bir alt ağda yeniden oluşturma gereksinimine yol açabileceğinden, daha fazla planlama gerektirir.

Düğümler Azure [Kapsayıcı Ağ Arabirimi (CNI)][cni-networking] Kubernetes eklentisini kullanır.

![Her biri tek bir Azure VNet'e bağlanan köprüleri olan iki düğümü gösteren diyagram][advanced-networking-diagram]

Daha fazla bilgi için, [bir AKS kümesi için Azure CNI'yi Yapılandır' a][aks-configure-advanced-networking]bakın.

### <a name="compare-network-models"></a>Ağ modellerini karşılaştırın

Hem kubenet hem de Azure CNI, AKS kümeleriniz için ağ bağlantısı sağlar. Ancak, her biri için avantajları ve dezavantajları vardır. Yüksek düzeyde, aşağıdaki hususlar geçerlidir:

* **kubenet**
    * IP adresi alanını korur.
    * Kümenin dışından bölmelere ulaşmak için Kubernetes dahili veya harici yük dengeleyicisini kullanır.
    * Kullanıcı tanımlı rotaları (ÜDR) el ile yönetmeli ve korumalısınız.
    * Küme başına en fazla 400 düğüm.
* **Azure CNI**
    * Podlar tam sanal ağ bağlantısına sahip olur ve bağlı ağlardan özel IP adresleri üzerinden doğrudan ulaşılabilir.
    * Daha fazla IP adresi alanı gerektirir.

Kubenet ve Azure CNI arasında aşağıdaki davranış farklılıkları vardır:

| Özellik                                                                                   | Kubenet   | Azure CNI |
|----------------------------------------------------------------------------------------------|-----------|-----------|
| Varolan veya yeni sanal ağda küme dağıtma                                            | Desteklenen - El ile uygulanan ÜTR'ler | Destekleniyor |
| Pod-pod bağlantısı                                                                         | Destekleniyor | Destekleniyor |
| Pod-VM bağlantısı; Aynı sanal ağda VM                                          | Pod tarafından başlatıldığında çalışır | Her iki şekilde de çalışır |
| Pod-VM bağlantısı; Eşli sanal ağda VM                                            | Pod tarafından başlatıldığında çalışır | Her iki şekilde de çalışır |
| VPN veya Express Route kullanarak şirket içi erişim                                                | Pod tarafından başlatıldığında çalışır | Her iki şekilde de çalışır |
| Hizmet bitiş noktalarıtarafından güvence altına alınmıştır kaynaklara erişim                                             | Destekleniyor | Destekleniyor |
| Kubernetes hizmetlerini yük dengeleyici hizmeti, App Gateway veya giriş denetleyicisi kullanarak ortaya çıkarma | Destekleniyor | Destekleniyor |
| Varsayılan Azure DNS ve Özel Bölgeler                                                          | Destekleniyor | Destekleniyor |

DNS ile ilgili olarak, hem kubenet hem de Azure CNI eklentileri ile DNS, AKS'de çalışan bir daemon seti olan CoreDNS tarafından sunulmaktadır. Kubernetes coredns hakkında daha fazla bilgi için [DNS Hizmeti Özelleştirme](https://kubernetes.io/docs/tasks/administer-cluster/dns-custom-nameservers/)bakın. CoreDNS varsayılan başına, bilinmeyen etki alanlarını düğüm DNS sunucularına, başka bir deyişle AKS kümesinin dağıtıldığı Azure Sanal Ağı'nın DNS işlevine iletmek üzere yapılandırılır. Bu nedenle, Azure DNS ve Özel Bölgeler AKS'de çalışan bölmeler için çalışır.

### <a name="support-scope-between-network-models"></a>Ağ modelleri arasındaki destek kapsamı

Kullandığınız ağ modeline bakılmaksızın, hem kubenet hem de Azure CNI aşağıdaki yollardan biriyle dağıtılabilir:

* Azure platformu, bir AKS kümesi oluşturduğunuzda sanal ağ kaynaklarını otomatik olarak oluşturabilir ve yapılandırabilir.
* Aks kümenizi oluştururken sanal ağ kaynaklarını el ile oluşturabilir ve yapılandırabilir ve bu kaynaklara ekleyebilirsiniz.

Hizmet bitiş noktaları veya ÜD'ler gibi özellikler hem kubenet hem de Azure CNI ile desteklenmiş olsa da, [AKS için destek ilkeleri][support-policies] hangi değişiklikleri yapabileceğinizi tanımlar. Örnek:

* Bir AKS kümesi için sanal ağ kaynaklarını el ile oluşturursanız, kendi ÜTR'lerinizi veya hizmet uç noktalarınızı yapılandırırken desteklenirsiniz.
* Azure platformu AKS kümeniz için sanal ağ kaynaklarını otomatik olarak oluşturuyorsa, kendi ÜrKP'lerinizi veya hizmet bitiş noktalarınızı yapılandırmak için AKS tarafından yönetilen kaynakları el ile değiştirmek için desteklenmez.

## <a name="ingress-controllers"></a>Giriş denetleyicileri

LoadBalancer türünde bir Hizmet oluşturduğunuzda, altta yatan bir Azure yük dengeleyici kaynağı oluşturulur. Yük dengeleyicisi, belirli bir bağlantı noktasında Hizmetinizdeki bölmelere trafik dağıtmak üzere yapılandırılır. LoadBalancer yalnızca katman 4'te çalışır - Hizmet gerçek uygulamalardan habersizdir ve ek yönlendirme hususları yapamaz.

*Giriş denetleyicileri* katman 7'de çalışır ve uygulama trafiğini dağıtmak için daha akıllı kurallar kullanabilir. Giriş denetleyicisinin yaygın kullanımı, HTTP trafiğini gelen URL'ye göre farklı uygulamalara yönlendirmektir.

![AKS kümesindeki Giriş trafik akışını gösteren diyagram][aks-ingress]

AKS'de, NGINX gibi bir şeyi kullanarak bir Giriş kaynağı oluşturabilir veya AKS HTTP uygulama yönlendirme özelliğini kullanabilirsiniz. Bir AKS kümesi için HTTP uygulama yönlendirmesini etkinleştirdiğinizde, Azure platformu Giriş denetleyicisi ve *harici DNS* denetleyicisi oluşturur. Kubernetes'te yeni Giriş kaynakları oluşturuldukça, kümeye özgü bir DNS bölgesinde gerekli DNS A kayıtları oluşturulur. Daha fazla bilgi için [http uygulama yönlendirmesi dağıtma'ya][aks-http-routing]bakın.

Giriş'in bir diğer yaygın özelliği de SSL/TLS sonlandırmadır. HTTPS üzerinden erişilen büyük web uygulamalarında TLS sonlandırma, uygulamanın kendisi yerine Giriş kaynağı tarafından işlenebilir. Otomatik TLS sertifika oluşturma ve yapılandırma sağlamak için, Ingress kaynağını Let's Encrypt gibi sağlayıcıları kullanacak şekilde yapılandırabilirsiniz. Let's Encrypt ile NGINX Ingress denetleyicisi yapılandırma hakkında daha fazla bilgi için [Giriş ve TLS'ye][aks-ingress-tls]bakın.

Aks kümenizdeki kapsayıcılara gelen isteklerde istemci kaynak IP'yi korumak için giriş denetleyicinizi de yapılandırabilirsiniz. İstemci isteği giriş denetleyiciniz aracılığıyla AKS kümenizdeki bir kapsayıcıya yönlendirildiğinde, bu isteğin özgün kaynak IP'si hedef kapsayıcıda kullanılamaz. *İstemci kaynağı IP*korumaetkinleştirdiğinizde, istemci için kaynak IP *X-Forwarded-For*altında istek üstbilgisinde kullanılabilir. Giriş denetleyicinizde istemci kaynağı IP koruma kullanıyorsanız, SSL geçişini kullanamazsınız. İstemci kaynağı IP koruma ve SSL *geçişloadBalancer* türü gibi diğer hizmetlerile kullanılabilir.

## <a name="network-security-groups"></a>Ağ güvenlik grupları

Ağ güvenlik grubu, AKS düğümleri gibi VM'ler için trafiği filtreler. LoadBalancer gibi Hizmetler oluşturduğunuzda, Azure platformu gereken ağ güvenlik grubu kurallarını otomatik olarak yapılandırır. Aks kümesindeki bölmetrafiğini filtrelemek için ağ güvenlik grubu kurallarını el ile yapılandırmayın. Gerekli bağlantı noktalarını ve iletmeleri Kubernetes Hizmet bildirimlerinizin bir parçası olarak tanımlayın ve Azure platformunun uygun kuralları oluşturmasına veya güncelleştirmesine izin verin. Bir sonraki bölümde belirtildiği gibi ağ ilkelerini de kullanarak bölmelere trafik filtresi kurallarını otomatik olarak uygulayabilirsiniz.

## <a name="network-policies"></a>Ağ ilkeleri

Varsayılan olarak, bir AKS kümesindeki tüm bölmeler herhangi bir sınırlama olmaksızın trafik gönderebilir ve alabilir. Gelişmiş güvenlik için, trafik akışını kontrol eden kuralları tanımlamak isteyebilirsiniz. Arka uç uygulamaları genellikle yalnızca gerekli ön uç hizmetlerine maruz kalır veya veritabanı bileşenlerine yalnızca bunlara bağlanan uygulama katmanları tarafından erişilebilir.

Ağ ilkesi, AKS'de bulunan ve bölmeler arasındaki trafik akışını denetlemenize olanak tanıyan bir Kubernetes özelliğidir. Atanan etiketler, ad alanı veya trafik bağlantı noktası gibi ayarlara göre trafiğe izin vermeyi veya reddetmeyi seçebilirsiniz. Ağ güvenlik grupları daha çok AKS düğümleri içindir, bölmeler için değil. Ağ ilkelerinin kullanımı, trafik akışını denetlemek için daha uygun, buluta özgü bir yoldur. Bir AKS kümesinde bölmedinamik olarak oluşturulduğundan, gerekli ağ ilkeleri otomatik olarak uygulanabilir.

Daha fazla bilgi için bkz: [Azure Kubernetes Hizmeti'nde (AKS) ağ ilkelerini kullanarak bölmeler arasındaki güvenli trafiği.][use-network-policies]

## <a name="next-steps"></a>Sonraki adımlar

AKS ağ ile başlamak [için, kubenet][aks-configure-kubenet-networking] veya [Azure CNI][aks-configure-advanced-networking]kullanarak kendi IP adres aralıkları ile bir AKS kümesi oluşturun ve yapılandırın.

İlişkili en iyi uygulamalar [için AKS'de ağ bağlantısı ve güvenliği için en iyi uygulamalara][operator-best-practices-network]bakın.

Çekirdek Kubernetes ve AKS kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümeleri ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS erişim ve kimlik][aks-concepts-identity]
- [Kubernetes / AKS güvenlik][aks-concepts-security]
- [Kubernetes / AKS depolama][aks-concepts-storage]
- [Kubernetes / AKS ölçeği][aks-concepts-scale]

<!-- IMAGES -->
[aks-clusterip]: ./media/concepts-network/aks-clusterip.png
[aks-nodeport]: ./media/concepts-network/aks-nodeport.png
[aks-loadbalancer]: ./media/concepts-network/aks-loadbalancer.png
[advanced-networking-diagram]: ./media/concepts-network/advanced-networking-diagram.png
[aks-ingress]: ./media/concepts-network/aks-ingress.png

<!-- LINKS - External -->
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet

<!-- LINKS - Internal -->
[aks-http-routing]: http-application-routing.md
[aks-ingress-tls]: ingress.md
[aks-configure-kubenet-networking]: configure-kubenet.md
[aks-configure-advanced-networking]: configure-azure-cni.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-security]: concepts-security.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-identity]: concepts-identity.md
[use-network-policies]: use-network-policies.md
[operator-best-practices-network]: operator-best-practices-network.md
[support-policies]: support-policies.md
