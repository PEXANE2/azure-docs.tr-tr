---
title: Azure Kubernetes hizmeti 'nde (AKS) çıkış trafiğini kısıtlama
description: Azure Kubernetes Service (AKS) ' de çıkış trafiğini denetlemek için hangi bağlantı noktalarının ve adreslerin gerekli olduğunu öğrenin
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/06/2019
ms.author: mlearned
ms.openlocfilehash: 369729f10de4a55cd14bb866795ea1aa15b3d9da
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69639777"
---
# <a name="preview---limit-egress-traffic-for-cluster-nodes-and-control-access-to-required-ports-and-services-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS) içinde küme düğümleri için çıkış trafiğini önizleme ve gerekli bağlantı noktalarına ve hizmetlere erişimi denetleme

Varsayılan olarak, AKS kümelerinde sınırsız giden (çıkış) internet erişimi vardır. Bu ağ erişimi düzeyi, çalıştırdığınız düğüm ve hizmetlere, gerektiğinde dış kaynaklara erişmek için izin verir. Çıkış trafiğini kısıtlamak istiyorsanız, sağlıklı küme bakım görevlerini sürdürmek için sınırlı sayıda bağlantı noktasına ve adrese erişilebilir olması gerekir. Kümeniz daha sonra yalnızca Microsoft Container Registry (MCR) veya Azure Container Registry (ACR) olan temel sistem kapsayıcısı görüntülerini, dış genel depolardan değil kullanacak şekilde yapılandırılmıştır. Tercih ettiğiniz güvenlik duvarı ve güvenlik kurallarını bu gerekli bağlantı noktalarına ve adreslere izin verecek şekilde yapılandırmanız gerekir.

Bu makalede, bir AKS kümesindeki çıkış trafiğini kısıtladığınızda ağ bağlantı noktaları ve tam etki alanı adları (FQDN 'Ler) gerekli ve isteğe bağlı olarak ayrıntılı olarak açıklanmaktadır.  Bu özellik şu anda önizleme sürümündedir.

> [!IMPORTANT]
> AKS Önizleme özellikleri self servis kabul etme sürecindedir. Önizlemeler, "olduğu gibi" ve "kullanılabilir olarak" verilmiştir ve hizmet düzeyi sözleşmelerinden ve sınırlı garantiden çıkarılır. AKS önizlemeleri, müşteri desteğinin en iyi çaba temelinde kısmen ele alınmıştır. Bu nedenle, bu özellikler üretim kullanımı için tasarlanmamıştır. Ek bilgi için lütfen aşağıdaki destek makalelerine bakın:
>
> * [AKS destek Ilkeleri][aks-support-policies]
> * [Azure desteği SSS][aks-faq]

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLı sürüm 2.0.66 veya sonraki bir sürümün yüklü ve yapılandırılmış olması gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekiyorsa bkz. [Azure CLI'yı yükleme][install-azure-cli].

Çıkış trafiğini sınırlayan bir AKS kümesi oluşturmak için öncelikle aboneliğinizde bir özellik bayrağını etkinleştirin. Bu özellik kaydı, MCR veya ACR 'den temel sistem kapsayıcısı görüntülerini kullanmak için oluşturduğunuz AKS kümelerini yapılandırır. *Akslockingdownegresspreview* Özellik bayrağını kaydetmek için, aşağıdaki örnekte gösterildiği gibi [az Feature Register][az-feature-register] komutunu kullanın:

> [!CAUTION]
> Bir abonelik üzerinde bir özelliği kaydettiğinizde, o özelliği şu anda kaydedemezsiniz. Bazı Önizleme özelliklerini etkinleştirdikten sonra, daha sonra abonelikte oluşturulan tüm AKS kümeleri için varsayılanlar kullanılabilir. Üretim aboneliklerinde Önizleme özelliklerini etkinleştirmeyin. Önizleme özelliklerini test etmek ve geri bildirim toplamak için ayrı bir abonelik kullanın.

```azurecli-interactive
az feature register --name AKSLockingDownEgressPreview --namespace Microsoft.ContainerService
```

Durumun *kayıtlı*gösterilmesi birkaç dakika sürer. [Az Feature List][az-feature-list] komutunu kullanarak kayıt durumunu denetleyebilirsiniz:

```azurecli-interactive
az feature list -o table --query "[?contains(name, 'Microsoft.ContainerService/AKSLockingDownEgressPreview')].{Name:name,State:properties.state}"
```

Hazırlandığınızda, [az Provider Register][az-provider-register] komutunu kullanarak *Microsoft. Containerservice* kaynak sağlayıcısı kaydını yenileyin:

```azurecli-interactive
az provider register --namespace Microsoft.ContainerService
```

## <a name="egress-traffic-overview"></a>Çıkış trafiğine genel bakış

Yönetim ve operasyonel amaçlar için, bir AKS kümesindeki düğümlerin belirli bağlantı noktalarına ve tam etki alanı adlarına (FQDN) erişmesi gerekir. Bu eylemler, API sunucusuyla iletişim kurmak veya temel Kubernetes küme bileşenlerini ve düğüm güvenliği güncelleştirmelerini indirmek ve yüklemek olabilir. Varsayılan olarak, çıkış (giden) internet trafiği bir AKS kümesindeki düğümler için sınırlandırılmaz. Küme, dış depolardan temel sistem kapsayıcısı görüntülerini alabilir.

AKS kümenizin güvenliğini arttırmak için çıkış trafiğini kısıtlamak isteyebilirsiniz. Küme, MCR veya ACR 'den temel sistem kapsayıcısı görüntülerini çekmek üzere yapılandırılmıştır. Çıkış trafiğini bu şekilde kilitlerseniz, AKS düğümlerinin gerekli dış hizmetlerle doğru iletişim kurmasına izin vermek için belirli bağlantı noktalarını ve FQDN 'leri tanımlamanız gerekir. Bu yetkili bağlantı noktaları ve FQDN 'Ler olmadan AKS düğümleriniz API sunucusuyla iletişim kuramaz veya çekirdek bileşenleri yükleyemez.

Çıkış trafiğinizi güvenli hale getirmek ve bu gerekli bağlantı noktalarını ve adresleri tanımlamak için [Azure Güvenlik Duvarı][azure-firewall] 'nı veya üçüncü taraf güvenlik duvarı gereçini kullanabilirsiniz. AKS bu kuralları sizin için otomatik olarak oluşturmaz. Aşağıdaki bağlantı noktaları ve adresler, ağ güvenlik duvarınızdaki uygun kuralları oluştururken başvuru içindir.

> [!IMPORTANT]
> Çıkış trafiğini kısıtlamak ve tüm çıkış trafiğini zorlamak için Kullanıcı tanımlı yol (UDR) oluşturmak üzere Azure Güvenlik Duvarı 'nı kullandığınızda, giriş trafiğine doğru şekilde izin vermek için güvenlik duvarında uygun bir DNAT kuralı oluşturduğunuzdan emin olun. Azure Güvenlik Duvarı 'nı bir UDR ile kullanmak, asimetrik yönlendirme nedeniyle giriş kurulumunu keser. (Bu sorun, AKS alt ağının güvenlik duvarının özel IP adresine giden bir varsayılan yolu olduğu, ancak türünde ortak yük dengeleyici veya Kubernetes hizmeti kullandığınız için oluşur: LoadBalancer). Bu durumda, gelen yük dengeleyici trafiği genel IP adresi aracılığıyla alınır, ancak döndürülen yol güvenlik duvarının özel IP adresinden geçer. Güvenlik duvarı durum bilgisi olduğundan, güvenlik duvarı kurulu bir oturumun farkında olmadığından döndürülen paketi bırakır. Azure Güvenlik duvarını giriş veya hizmet yük dengeleyicinizle tümleştirmeyi öğrenmek için bkz. Azure [güvenlik duvarını azure standart Load Balancer tümleştirme](https://docs.microsoft.com/en-us/azure/firewall/integrate-lb).
>

AKS 'de, iki bağlantı noktası ve adres kümesi vardır:

* [AKS kümelerine yönelik gerekli bağlantı noktaları ve adresler](#required-ports-and-addresses-for-aks-clusters) , yetkili çıkış trafiği için en düşük gereksinimleri ayrıntılarıyla ayrıntılardır.
* [AKS kümeleri için isteğe bağlı önerilen adresler ve bağlantı noktaları](#optional-recommended-addresses-and-ports-for-aks-clusters) tüm senaryolar için gerekli değildir, ancak Azure izleyici gibi diğer hizmetlerle tümleştirme doğru çalışmaz. İsteğe bağlı bağlantı noktaları ve FQDN 'Ler listesini gözden geçirin ve AKS Kümenizde kullanılan hizmet ve bileşenlerden herhangi birini yetkilendirin.

> [!NOTE]
> Çıkış trafiğini sınırlandırma özelliği, yalnızca özellik bayrağı kaydını etkinleştirdikten sonra oluşturulan yeni AKS kümelerinde kullanılabilir. Mevcut kümeler için çıkış trafiğini sınırlandırmadan önce `az aks upgrade` komutunu kullanarak [bir küme yükseltme işlemi gerçekleştirin][aks-upgrade] .

## <a name="required-ports-and-addresses-for-aks-clusters"></a>AKS kümeleri için gerekli bağlantı noktaları ve adresler

AKS kümesi için aşağıdaki giden bağlantı noktaları/ağ kuralları gereklidir:

* TCP bağlantı noktası *443*
* Tünel ön pod için, API sunucusundaki tünel sonuyla iletişim kurmak üzere TCP bağlantı noktası *9000* ve TCP bağlantı noktası *22* .
    * Daha fazla bilgi almak için * *. HCP öğesine bakın.\< Location\>. azmk8s.io* ve * *. tun.\< Aşağıdaki\>tablodaki location. azmk8s.io* adresleri.

Aşağıdaki FQDN/uygulama kuralları gereklidir:

| FQDN                       | Port      | Bir yönetim grubuna bağlanmak veya bağlı bir yönetim grubunun özelliklerini düzenlemek için Yönetim çalışma alanında      |
|----------------------------|-----------|----------|
| *. HCP. \<Location\>. azmk8s.io | HTTPS: 443, TCP: 22, TCP: 9000 | Bu adres, API sunucusu uç noktasıdır. Konumu aks kümenizin dağıtıldığı bölge ile değiştirin. *\<\>* |
| *. tun. \<Location\>. azmk8s.io | HTTPS: 443, TCP: 22, TCP: 9000 | Bu adres, API sunucusu uç noktasıdır. Konumu aks kümenizin dağıtıldığı bölge ile değiştirin. *\<\>* |
| aksrepos.azurecr.io        | HTTPS: 443 | Bu adres Azure Container Registry (ACR) içindeki görüntülere erişmek için gereklidir. |
| *.blob.core.windows.net    | HTTPS: 443 | Bu adres, ACR 'de depolanan görüntülerin arka uç deposudur. |
| mcr.microsoft.com          | HTTPS: 443 | Bu adres, Microsoft Container Registry (MCR) içindeki görüntülere erişmek için gereklidir. |
| *.cdn.mscr.io              | HTTPS: 443 | Bu adres, Azure Content Delivery Network (CDN) tarafından desteklenen MCR depolama alanı için gereklidir. |
| Management.Azure.com       | HTTPS: 443 | Bu adres, Kubernetes GET/PUT işlemleri için gereklidir. |
| login.microsoftonline.com  | HTTPS: 443 | Bu adres Azure Active Directory kimlik doğrulaması için gereklidir. |
| api.snapcraft.io           | HTTPS: 443, HTTP: 80 | Bu adres, Linux düğümlerine ek paketler yüklemek için gereklidir. |
| ntp.ubuntu.com             | UDP: 123   | Bu adres, Linux düğümlerinde NTP zaman eşitlemesi için gereklidir. |
| *. docker.io                | HTTPS: 443 | Bu adres, tünel ön öğesine gereken kapsayıcı görüntülerini çekmek için gereklidir. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>AKS kümeleri için isteğe bağlı önerilen adresler ve bağlantı noktaları

* DNS için UDP bağlantı noktası *53*

AKS kümelerinin doğru çalışması için aşağıdaki FQDN/uygulama kuralları önerilir:

| FQDN                                    | Port      | Bir yönetim grubuna bağlanmak veya bağlı bir yönetim grubunun özelliklerini düzenlemek için Yönetim çalışma alanında      |
|-----------------------------------------|-----------|----------|
| *. ubuntu.com                            | HTTP: 80   | Bu adres, Linux küme düğümlerinin gerekli güvenlik düzeltme eklerini ve güncelleştirmelerini indirmesini sağlar. |
| packages.microsoft.com                  | HTTPS: 443 | Bu adres, önbelleğe alınmış *apt-get* işlemleri Için kullanılan Microsoft paketleri deposudur. |
| dc.services.visualstudio.com            | HTTPS: 443 | Azure Izleyici kullanarak doğru ölçümler ve izleme için önerilir. |
| *.opinsights.azure.com                  | HTTPS: 443 | Azure Izleyici kullanarak doğru ölçümler ve izleme için önerilir. |
| *. monitoring.azure.com                  | HTTPS: 443 | Azure Izleyici kullanarak doğru ölçümler ve izleme için önerilir. |
| gov-prod-policy-data.trafficmanager.net | HTTPS: 443 | Bu adres, Azure Ilkesi 'nin doğru çalışması için kullanılır (Şu anda AKS 'de önizlemededir). |
| apt.dockerproject.org                   | HTTPS: 443 | Bu adres, GPU tabanlı düğümlerde doğru sürücü yükleme ve işlem için kullanılır. |
| nvidia.github.io                        | HTTPS: 443 | Bu adres, GPU tabanlı düğümlerde doğru sürücü yükleme ve işlem için kullanılır. |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kümenin çıkış trafiğini kısıtladığınızda hangi bağlantı noktalarının ve adreslerin izin verdiklerini öğrendiniz. Ayrıca, ayırımların nasıl iletişim kurabildiğini ve küme içinde sahip oldukları kısıtlamaları tanımlayabilirsiniz. Daha fazla bilgi için bkz. [aks 'deki ağ ilkelerini kullanarak Pod arasındaki trafiği güvenli hale getirme][network-policy].

<!-- LINKS - internal -->
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[network-policy]: use-network-policies.md
[azure-firewall]: ../firewall/overview.md
[az-feature-register]: /cli/azure/feature#az-feature-register
[az-feature-list]: /cli/azure/feature#az-feature-list
[az-provider-register]: /cli/azure/provider#az-provider-register
[aks-upgrade]: upgrade-cluster.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
