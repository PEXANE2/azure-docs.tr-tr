---
title: Azure Kubernetes Hizmetinde Dağıtılan Mikro Hizmetlerle Azure API Yönetimini Kullanın | Microsoft Dokümanlar
description: Bu makalede, AKS ile API Yönetimi dağıtma seçenekleri açıklanmaktadır
services: api-management
documentationcenter: ''
author: miaojiang
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 12/14/2019
ms.author: apimpm
ms.openlocfilehash: 1d6773b4daac256234c33bf50fb3736d585ac505
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75481002"
---
# <a name="use-azure-api-management-with-microservices-deployed-in-azure-kubernetes-service"></a>Azure Kubernetes Hizmetinde dağıtılan mikro hizmetlerle Azure API Yönetimini kullanma

Mikro hizmetler API'ler oluşturmak için idealdir. [Azure Kubernetes Hizmeti](https://azure.microsoft.com/services/kubernetes-service/) (AKS) ile bulutta mikro hizmetler tabanlı bir [mimariyi](https://docs.microsoft.com/azure/architecture/guide/architecture-styles/microservices) hızla dağıtabilir ve çalıştırabilirsiniz. Daha sonra, mikro hizmetlerinizi dahili ve harici tüketim için API olarak yayımlamak için [Azure API Yönetimi'nden](https://aka.ms/apimrocks) (API Yönetimi) yararlanabilirsiniz. Bu makalede, AKS ile API Yönetimi dağıtma seçenekleri açıklanmaktadır. Kubernetes, API Yönetimi ve Azure ağı hakkında temel bilgileri varsayar. 

## <a name="background"></a>Arka plan

Mikro hizmetleri tüketim için API olarak yayımlarken, mikro hizmetler ve bunları tüketen istemciler arasındaki iletişimi yönetmek zor olabilir. Kimlik doğrulama, yetkilendirme, azaltma, önbelleğe alma, dönüştürme ve izleme gibi çok sayıda çapraz kesme endişesi vardır. Bu endişeler, mikro hizmetlerin dahili veya harici istemcilere maruz kalıp kalmadığı konusunda geçerlidir. 

[API Ağ Geçidi](https://docs.microsoft.com/dotnet/architecture/microservices/architect-microservice-container-applications/direct-client-to-microservice-communication-versus-the-api-gateway-pattern) deseni bu endişeleri gideriyor. Bir API ağ geçidi, mikro hizmetler için bir ön kapı görevi görmekte, müşterileri mikro hizmetlerinizden ayırıyor, ek bir güvenlik katmanı ekliyor ve çapraz kesme endişelerini giderme yükünü ortadan kaldırarak mikro hizmetlerinizin karmaşıklığını azaltır. 

[Azure API Yönetimi, API](https://aka.ms/apimrocks) ağ geçidi gereksinimlerinizi çözmek için anahtar teslim i Mikro hizmetleriniz için hızlı ve modern bir ağ geçidi oluşturabilir ve bunları API olarak yayınlayabilirsiniz. Tam yaşam döngüsü API yönetimi çözümü olarak, API bulma, API yaşam döngüsü yönetimi ve API analitiği için self servis geliştirici portalı da dahil olmak üzere ek özellikler sağlar.

AKS ve API Yönetimi birlikte kullanıldığında, mikro hizmetler tabanlı API'lerinizi dağıtmak, yayınlamak, güvence altına almak, izlemek ve yönetmek için bir platform sağlar. Bu makalede, API Yönetimi ile birlikte AKS dağıtma birkaç seçenek üzerinden gidecek. 

## <a name="kubernetes-services-and-apis"></a>Kubernetes Hizmetleri ve API'leri

Bir Kubernetes kümesinde, kapsayıcılar geçici ve yaşam döngüsüne sahip [Pods,.](https://kubernetes.io/docs/concepts/workloads/pods/pod/) Bir işçi düğümü öldüğünde, düğüm üzerinde çalışan Podlar kaybolur. Bu nedenle, pod IP adresi her zaman değişebilir. Kapsülle iletişim kurmasına güvenemeyiz. 

Bu sorunu çözmek için, Kubernetes [Hizmet](https://kubernetes.io/docs/concepts/services-networking/service/)kavramını tanıttı. Kubernetes Hizmeti, podların bir mantık grubunu tanımlayan ve bu Podlar için dış trafik maruziyeti, yük dengeleme ve hizmet bulma sağlayan bir soyutlama katmanıdır. 

Mikro hizmetlerimizi API Yönetimi aracılığıyla API olarak yayınlamaya hazır olduğumuzda, Kubernetes'teki Hizmetlerimizi API Yönetimi'ndeki API'lerle nasıl eşlediğimizi düşünmemiz gerekir. Belirlenmiş kurallar yok. Başlangıçta işletme yeteneklerinizi veya etki alanlarınızı nasıl tasarladığınıza ve bölümlere baldığınıza bağlıdır. Örneğin, Bir Hizmetin arkasındaki bölmeler belirli bir kaynaktaki tüm işlemlerden (örneğin, Müşteri) sorumluysa, Hizmet bir API'ye eşlenebilir. Bir kaynak üzerindeki işlemler birden çok mikro hizmete (örneğin, GetOrder, PlaceOrder) bölünürse, birden çok Hizmet mantıksal olarak API yönetiminde tek bir API'de toplanabilir (Bkz. Şekil 1). 

Haritalamalar da gelişebilir. API Yönetimi, mikro hizmetlerin önünde bir cephe oluşturduğundan, zaman içinde mikro hizmetlerimizi yeniden düzenlememizi ve sağ boyutumuzu oluşturmamızı sağlar. 

![API'lere harita hizmetleri](./media/api-management-aks/service-api-mapping.png)

## <a name="deploy-api-management-in-front-of-aks"></a>AKS önünde API Yönetimini Dağıtın

Bir AKS kümesinin önünde API Yönetimi'ni dağıtmak için birkaç seçenek vardır. 

Bir AKS kümesi her zaman sanal ağda (VNet) dağıtılsa da, Bir VNet'te dağıtılmak üzere bir API Yönetimi örneği gerekmez. API Yönetimi VNet kümesi içinde ikamet etmediğinde, AKS kümesinin BAĞLANILMASı için API Yönetimi için ortak uç noktaları yayımlamak zorundadır. Bu durumda, API Yönetimi ve AKS arasındaki bağlantıyı güvence altına almak gerekir. Başka bir deyişle, kümeye yalnızca API Yönetimi aracılığıyla erişilebilmesini sağlamamız gerekir. Seçenekleri gözden geçirelim. 

### <a name="option-1-expose-services-publicly"></a>Seçenek 1: Hizmetleri herkese açık olarak ortaya çıkarma

AKS kümesindeki hizmetler, NodePort, LoadBalancer veya ExternalName [Servis türleri](https://docs.microsoft.com/azure/aks/concepts-network) kullanılarak genel olarak açıklanabilir. Bu durumda, Hizmetlere doğrudan genel internetten erişilebilir. API Management'ı kümenin önünde dağıttıktan sonra, mikro hizmetlerde kimlik doğrulama uygulayarak tüm gelen trafiğin API Yönetimi'nden geçtiğini sağlamamız gerekir. Örneğin, API Yönetimi kümeye yapılan her istekte bir erişim belirteci içerebilir. Her microservice isteği işlemeden önce belirteci doğrulamak için sorumludur. 


Bu, ÖZELLIKLE mikro hizmetlerinizde zaten uygulanan kimlik doğrulama mantığınız varsa, API Yönetimini AKS'nin önünde dağıtmak için en kolay seçenek olabilir. 

![Hizmetleri doğrudan yayınlama](./media/api-management-aks/direct.png)

Profesyonel:
* Küme VNet içine enjekte edilmesi gerekmez çünkü API Yönetimi tarafında kolay yapılandırma
* Hizmetler zaten kamuya açıksa ve mikro hizmetlerde kimlik doğrulama mantığı zaten mevcutsa AKS tarafında değişiklik yok

Eksi -lerini:
* Hizmet bitiş noktalarının genel görünürlüğü nedeniyle olası güvenlik riski
* Gelen küme trafiği için tek giriş noktası yok
* Yinelenen kimlik doğrulama mantığıyla mikro hizmetleri karmaşıklaştırır

### <a name="option-2-install-an-ingress-controller"></a>Seçenek 2: Giriş Denetleyicisi Yükleme

Seçenek 1 daha kolay olsa da, yukarıda belirtildiği gibi önemli dezavantajları vardır. Bir API Yönetimi örneği VNet kümesinde yer almazsa, Karşılıklı TLS kimlik doğrulaması (mTLS), bir API Yönetimi örneği ile AKS kümesi arasında trafiğin güvenli ve her iki yönde de güvenilmesini sağlamanın sağlam bir yoludur. 

Karşılıklı TLS kimlik doğrulaması api yönetimi tarafından [yerel olarak desteklenir](https://docs.microsoft.com/azure/api-management/api-management-howto-mutual-certificates) ve Kubernetes'te [bir Giriş Denetleyicisi takılarak](https://docs.microsoft.com/azure/aks/ingress-own-tls) etkinleştirilebilir (Res. 3). Sonuç olarak, kimlik doğrulama, mikro hizmetleri basitleştiren Giriş Denetleyicisi'nde gerçekleştirilir. Ayrıca, yalnızca API Yönetimi kümeye erişebildiğinden emin olmak için, Giriş tarafından izin verilen listeye API Yönetimi'nin IP adreslerini ekleyebilirsiniz.  

 
![Giriş denetleyicisi aracılığıyla yayımlama](./media/api-management-aks/ingress-controller.png)


Profesyonel:
* Küme VNet içine enjekte edilmesi gerekmez ve mTLS yerel olarak desteklenen çünkü API Yönetimi tarafında kolay yapılandırma
* Giriş Denetleyicisi katmanında gelen küme trafiği için korumayı merkezileştirir
* Genel olarak görünür küme uç noktalarını en aza indirerek güvenlik riskini azaltır

Eksi -lerini:
* Giriş Denetleyicisini yüklemek, yapılandırmak ve korumak ve mTLS için kullanılan sertifikaları yönetmek için fazladan çalışma nedeniyle küme yapılandırmasının karmaşıklığını artırır
* Giriş Denetleyici uç noktasının genel görünürlüğü nedeniyle güvenlik riski


API Yönetimi aracılığıyla API'leri yayımladığınızda, abonelik anahtarlarını kullanarak bu API'lere erişimi güvence altına almak kolay ve yaygındır. Yayımlanmış API'leri tüketmesi gereken geliştiriciler, bu API'lere çağrı yaparken HTTP isteklerine geçerli bir abonelik anahtarı eklemelidir. Aksi takdirde, aramalar API Yönetimi ağ geçidi tarafından hemen reddedilir. Arka uç servislerine iletilirler.

API'lere erişmek için abonelik anahtarı almak için abonelik gerekir. Abonelik aslında bir çift abonelik anahtarı için adlandırılmış bir kapsayıcıdır. Yayımlanmış API'leri tüketen geliştiriciler abonelik alabilir. Ve API yayıncılarının onayına ihtiyaçları yok. API yayıncıları doğrudan API tüketicileri için abonelikler oluşturabilir.

### <a name="option-3-deploy-apim-inside-the-cluster-vnet"></a>Seçenek 3: VNet kümesinin içine APIM dağıtma

Bazı durumlarda, düzenleyici kısıtlamalara veya katı güvenlik gereksinimlerine sahip müşteriler, genel olarak açıkolan uç noktalar nedeniyle Seçenek 1 ve 2'yi uygun olmayan çözümler bulabilir. Diğerlerinde, AKS kümesi ve mikro hizmetleri tüketen uygulamalar aynı VNet içinde olabilir, bu nedenle tüm API trafiği VNet içinde kalacağından, kümeyi herkese açık olarak ortaya çıkarmak için bir neden yoktur. Bu senaryolar için, API Yönetimi'ni VNet kümesine dağıtabilirsiniz. [API Management Premium katmanı](https://aka.ms/apimpricing) VNet dağıtımını destekler. 

[API Yönetimini Bir VNet'e dağıtmanın](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet) iki modu vardır – Harici ve Dahili. 

API tüketicileri VNet kümesinde ikamet etmiyorsa, Dış mod (Res. 4) kullanılmalıdır. Bu modda, API Yönetimi ağ geçidi VNet kümesine enjekte edilir, ancak harici bir yük dengeleyicisi aracılığıyla herkese açık internetten erişilebilir. Dış istemcilerin mikro hizmetleri tüketmesine izin verirken kümeyi tamamen gizlemeye yardımcı olur. Ayrıca, ağ trafiğini kısıtlamak için Ağ Güvenlik Grupları (NSG) gibi Azure ağ özelliklerini kullanabilirsiniz.

![Harici VNet modu](./media/api-management-aks/vnet-external.png)

Tüm API tüketicileri VNet kümesi içinde ikamet ediyorsa, Dahili mod (Res. 5) kullanılabilir. Bu modda, API Yönetimi ağ geçidi VNET kümesine enjekte edilir ve yalnızca bu VNet'in içinden bir dahili yük dengeleyicisi aracılığıyla erişilebilir. GENEL Internet'ten API Yönetimi ağ geçidine veya AKS kümesine ulaşmanın bir yolu yoktur. 

![Dahili VNet modu](./media/api-management-aks/vnet-internal.png)

 Her iki durumda da, AKS kümesi genel olarak görünür değildir. Seçenek 2 ile karşılaştırıldığında, Giriş Denetleyicisi gerekli olmayabilir. Senaryonuza ve yapılandırmanıza bağlı olarak, API Yönetimi ile mikro hizmetleriniz arasında kimlik doğrulama gerekebilir. Örneğin, bir Hizmet Kafesi kabul edilirse, her zaman karşılıklı TLS kimlik doğrulaması gerektirir. 

Profesyonel:
* AKS kümesinin ortak bitiş noktası olmadığından en güvenli seçenek
* Ortak bitiş noktası olmadığı için küme yapılandırması basitleştirir
* İç modu kullanarak HEM API Yönetimini hem de AKS'yi VNet içinde gizleme becerisi
* Ağ Güvenlik Grupları (NSG) gibi Azure ağ özelliklerini kullanarak ağ trafiğini denetleme olanağı

Eksi -lerini:
* API Yönetimini VNet içinde çalışacak şekilde dağıtma ve yapılandırma karmaşıklığını artırır

## <a name="next-steps"></a>Sonraki adımlar

* [AKS'deki uygulamalar için Ağ kavramları](https://docs.microsoft.com/azure/aks/concepts-network) hakkında daha fazla bilgi edinin
* [SANAL AĞLARLA API Yönetimi'ni kullanma](https://docs.microsoft.com/azure/api-management/api-management-using-with-vnet) hakkında daha fazla bilgi edinin





