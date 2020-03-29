---
title: Azure Kubernetes Hizmeti'nde (AKS) çıkış trafiğini kısıtlama
description: Azure Kubernetes Hizmeti'nde (AKS) çıkış trafiğini denetlemek için hangi bağlantı noktalarının ve adreslerin gerekli olduğunu öğrenin
services: container-service
ms.topic: article
ms.date: 03/10/2020
ms.openlocfilehash: 2cd7aeea272d22615d3ba3d3db6acc2c84d22cca
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79080182"
---
# <a name="control-egress-traffic-for-cluster-nodes-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'nde (AKS) küme düğümleri için çıkış trafiğini denetleme

Varsayılan olarak, AKS kümeleri sınırsız giden (çıkış) internet erişimine sahiptir. Bu ağ erişim düzeyi, çalıştırdığınız düğümlerin ve hizmetlerin gerektiğinde dış kaynaklara erişmesine olanak tanır. Çıkış trafiğini kısıtlamak istiyorsanız, sağlıklı küme bakım görevlerini sürdürmek için sınırlı sayıda bağlantı noktası ve adresine erişilmesi gerekir. Kümeniz varsayılan olarak yalnızca Microsoft Kapsayıcı Kayıt Defteri 'nden (MCR) veya Azure Kapsayıcı Kayıt Defteri'nden (ACR) temel sistem kapsayıcı görüntülerini kullanacak şekilde yapılandırılır. Bu gerekli bağlantı noktalarına ve adreslere izin verecek şekilde tercih ettiğiniz güvenlik duvarı ve güvenlik kurallarını yapılandırın.

Bu makalede, aks kümesindeki çıkış trafiğini kısıtlarsanız, hangi ağ bağlantı noktalarının ve tam nitelikli alan adlarının (FQDN'ler) gerekli olduğu ve isteğe bağlı olduğu açıklanır.

> [!IMPORTANT]
> Bu belge yalnızca AKS alt netinden ayrılan trafiğin nasıl kilitlenir olduğunu kapsar. AKS'nin giriş gereksinimleri yoktur.  Ağ güvenlik grupları (NSG' ler) ve güvenlik duvarları kullanılarak dahili alt ağ trafiğinin engellenmesi desteklenmez. Küme içindeki trafiği denetlemek ve engellemek için [Ağ İlkeleri'ni][network-policy]kullanın.

## <a name="before-you-begin"></a>Başlamadan önce

Azure CLI sürümü 2.0.66 veya daha sonra yüklenmiş ve yapılandırılan gerekir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][install-azure-cli].

## <a name="egress-traffic-overview"></a>Çıkış trafiğine genel bakış

Yönetim ve operasyonel amaçlar için, AKS kümesindeki düğümlerin belirli bağlantı noktalarına ve tam nitelikli alan adlarına (FQDNs) erişmeleri gerekir. Bu eylemler API sunucusuyla iletişim kurmak veya çekirdek Kubernetes küme bileşenlerini ve düğüm güvenlik güncelleştirmelerini karşıdan yüklemek ve yüklemek olabilir. Varsayılan olarak, çıkış (giden) internet trafiği bir AKS kümesindeki düğümler için sınırlandırılmamıştır. Küme, taban sistemi kapsayıcı görüntülerini dış depolardan çekebilir.

AKS kümenizin güvenliğini artırmak için çıkış trafiğini kısıtlamak isteyebilirsiniz. Küme, taban sistemi kapsayıcı görüntülerini MCR veya ACR'den çekecek şekilde yapılandırılmıştır. Çıkış trafiğini bu şekilde kilitlerseniz, AKS düğümlerinin gerekli dış hizmetlerle doğru şekilde iletişim kurmasına izin vermek için belirli bağlantı noktalarını ve FQDN'leri tanımlayın. Bu yetkili bağlantı noktaları ve FQDN'ler olmadan AKS düğümleriniz API sunucusuyla iletişim kuramaz veya temel bileşenleri yükleyemez.

Çıkış trafiğinizi güvence altına almak ve gerekli bağlantı noktalarını ve adreslerini tanımlamak için [Azure Güvenlik Duvarı'nı][azure-firewall] veya üçüncü taraf güvenlik duvarı aygıtını kullanabilirsiniz. AKS bu kuralları sizin için otomatik olarak oluşturmaz. Ağ güvenlik duvarınızda uygun kuralları oluştururken aşağıdaki bağlantı noktaları ve adresler başvuru içindir.

> [!IMPORTANT]
> Çıkış trafiğini kısıtlamak ve tüm çıkış trafiğini zorlamak için kullanıcı tanımlı bir rota (UDR) oluşturmak için Azure Güvenlik Duvarı'nı kullandığınızda, Giriş trafiğine doğru şekilde izin vermek için Güvenlik Duvarı'nda uygun bir DNAT kuralı oluşturduğunuzdan emin olun. UDR ile Azure Güvenlik Duvarı'nı kullanmak, asimetrik yönlendirme nedeniyle giriş kurulumlarını bozar. (Aks alt netinin güvenlik duvarının özel IP adresine giden varsayılan bir rotası varsa, ancak bir ortak yük dengeleyicisi kullanıyorsanız sorun oluşur - giriş veya Kubernetes türünden hizmet: LoadBalancer). Bu durumda, gelen yük dengeleyici trafiği genel IP adresi üzerinden alınır, ancak dönüş yolu güvenlik duvarının özel IP adresinden geçer. Güvenlik duvarı durum lu olduğundan, güvenlik duvarı belirlenmiş bir oturumdan haberdar olmadığından dönen paketi düşürür. Azure Güvenlik Duvarı'nı giriş veya hizmet yük bakiyecinizle nasıl entegre edebilirsiniz öğrenmek için [bkz.](https://docs.microsoft.com/azure/firewall/integrate-lb)
> Çıkış işçi düğümü IP(ler) ve API sunucusu nun IP'si arasındaki ağ kuralını kullanarak TCP portu 9000, TCP portu 22 ve UDP portu 1194'ün trafiğini kilitleyebilirsiniz.

AKS'de iki bağlantı noktası ve adresi vardır:

* [AKS kümeleri için gerekli bağlantı noktaları ve adres,](#required-ports-and-addresses-for-aks-clusters) yetkili çıkış trafiği için minimum gereksinimleri ayrıntılarıyla anlatır.
* [AKS kümeleri için isteğe bağlı önerilen adresler ve bağlantı noktaları](#optional-recommended-addresses-and-ports-for-aks-clusters) tüm senaryolar için gerekli değildir, ancak Azure Monitor gibi diğer hizmetlerle tümleştirme düzgün çalışmaz. İsteğe bağlı bağlantı noktaları ve FQDN'ler listesini gözden geçirin ve AKS kümenizde kullanılan hizmetlerden ve bileşenlerden herhangi birini yetkilendirmeyin.

> [!NOTE]
> Çıkış trafiğini sınırlamak yalnızca yeni AKS kümelerinde çalışır. Varolan kümeler için, çıkış trafiğini sınırlamadan önce komutu `az aks upgrade` kullanarak bir küme yükseltme işlemi [gerçekleştirin.][aks-upgrade]

## <a name="required-ports-and-addresses-for-aks-clusters"></a>AKS kümeleri için gerekli bağlantı noktaları ve adresler

Bir AKS kümesi için aşağıdaki giden bağlantı noktaları / ağ kuralları gereklidir:

* TCP bağlantı noktası *443*
* TCP [IPAddrOfYourAPIServer]:443 API sunucusuyla konuşmak için gereken bir uygulama varsa gereklidir.  Bu değişiklik küme oluşturulduktan sonra ayarlanabilir.
* TCP portu *9000*, TCP *portu 22* ve UDP *portu 1194* tünel ön bölmesi için API sunucusunda tünel sonu ile iletişim kurmak için.
    * Daha spesifik olmak için **.hcp\< adresine bakın. yer\>.azmk8s.io* ve **.tun.\< yer\>.azmk8s.io* adresleri aşağıdaki tabloda.
* AĞ ZAMAN Protokolü (NTP) zaman senkronizasyonu (Linux düğümleri) için UDP bağlantı noktası *123.*
* DOĞRUDAN API sunucusuna erişen bölmeleriniz varsa, DNS için UDP bağlantı noktası *53* de gereklidir.

Aşağıdaki FQDN / uygulama kuralları gereklidir:

> [!IMPORTANT]
> ***.blob.core.windows.net ve aksrepos.azurecr.io** artık çıkış kilitleme için FQDN kuralları gerekli değildir.  Varolan kümeler için, bu kuralları `az aks upgrade` kaldırmak için komutu kullanarak [bir küme yükseltme işlemi gerçekleştirin.][aks-upgrade]

> [!IMPORTANT]
> *.cdn.mscr.io, Azure genel bulut bölgeleri için *.data.mcr.microsoft.com ile değiştirilmiştir. Değişikliklerin etkili olması için lütfen mevcut güvenlik duvarı kurallarınızı yükseltin.

- Azure Global

| FQDN                       | Bağlantı noktası      | Kullanım      |
|----------------------------|-----------|----------|
| *.hcp. \<yer\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Bu adres Düğüm < > API sunucu iletişimi için gereklidir. * \<Konumu\> * AKS kümenizin dağıtıldığı bölgeyle değiştirin. |
| *.tun. \<yer\>.azmk8s.io | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Bu adres Düğüm < > API sunucu iletişimi için gereklidir. * \<Konumu\> * AKS kümenizin dağıtıldığı bölgeyle değiştirin. |
| *.cdn.mscr.io       | HTTPS:443 SAYıLı | Bu adres, Azure İçerik Dağıtım Ağı (CDN) tarafından desteklenen MCR depolama alanı için gereklidir. |
| mcr.microsoft.com          | HTTPS:443 SAYıLı | Bu adres, Microsoft Kapsayıcı Kayıt Defteri'ndeki (MCR) resimlere erişmek için gereklidir. Bu kayıt defteri, kümenin yükseltilmesi ve ölçeği sırasında kümenin çalışması için gerekli birinci taraf görüntüleri/grafikleri (örneğin, moby, vb.) içerir |
| *.data.mcr.microsoft.com             | HTTPS:443 SAYıLı | Bu adres, Azure içerik dağıtım ağı (CDN) tarafından desteklenen MCR depolama alanı için gereklidir. |
| management.azure.com       | HTTPS:443 SAYıLı | Bu adres Kubernetes GET/PUT işlemleri için gereklidir. |
| login.microsoftonline.com  | HTTPS:443 SAYıLı | Bu adres Azure Etkin Dizin kimlik doğrulaması için gereklidir. |
| ntp.ubuntu.com             | UDP:123   | Bu adres, Linux düğümlerinde NTP zaman eşitlemesi için gereklidir. |
| packages.microsoft.com     | HTTPS:443 SAYıLı | Bu adres, önbelleğe alınmış *apt-get* işlemleri için kullanılan Microsoft paketleri deposudur.  Örnek paketler Moby, PowerShell ve Azure CLI'yi içerir. |
| acs-mirror.azureedge.net      | HTTPS:443 SAYıLı | Bu adres, kubenet ve Azure CNI gibi gerekli ikilileri yüklemek için gereken depo içindir. |

- Azure Çin 21Vianet

| FQDN                       | Bağlantı noktası      | Kullanım      |
|----------------------------|-----------|----------|
| *.hcp. \<yer\>.cx.prod.service.azk8s.cn | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Bu adres Düğüm < > API sunucu iletişimi için gereklidir. * \<Konumu\> * AKS kümenizin dağıtıldığı bölgeyle değiştirin. |
| *.tun. \<yer\>.cx.prod.service.azk8s.cn | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Bu adres Düğüm < > API sunucu iletişimi için gereklidir. * \<Konumu\> * AKS kümenizin dağıtıldığı bölgeyle değiştirin. |
| *.azk8s.cn        | HTTPS:443 SAYıLı | Bu adres gerekli ikili leri ve resimleri indirmek için gereklidir|
| mcr.microsoft.com          | HTTPS:443 SAYıLı | Bu adres, Microsoft Kapsayıcı Kayıt Defteri'ndeki (MCR) resimlere erişmek için gereklidir. Bu kayıt defteri, kümenin yükseltilmesi ve ölçeği sırasında kümenin çalışması için gerekli birinci taraf görüntüleri/grafikleri (örneğin, moby, vb.) içerir |
| *.cdn.mscr.io       | HTTPS:443 SAYıLı | Bu adres, Azure İçerik Dağıtım Ağı (CDN) tarafından desteklenen MCR depolama alanı için gereklidir. |
| *.data.mcr.microsoft.com             | HTTPS:443 SAYıLı | Bu adres, Azure içerik dağıtım ağı (CDN) tarafından desteklenen MCR depolama alanı için gereklidir. |
| management.chinacloudapi.cn       | HTTPS:443 SAYıLı | Bu adres Kubernetes GET/PUT işlemleri için gereklidir. |
| login.chinacloudapi.cn  | HTTPS:443 SAYıLı | Bu adres Azure Etkin Dizin kimlik doğrulaması için gereklidir. |
| ntp.ubuntu.com             | UDP:123   | Bu adres, Linux düğümlerinde NTP zaman eşitlemesi için gereklidir. |
| packages.microsoft.com     | HTTPS:443 SAYıLı | Bu adres, önbelleğe alınmış *apt-get* işlemleri için kullanılan Microsoft paketleri deposudur.  Örnek paketler Moby, PowerShell ve Azure CLI'yi içerir. |

- Azure Kamu

| FQDN                       | Bağlantı noktası      | Kullanım      |
|----------------------------|-----------|----------|
| *.hcp. \<yer\>.cx.aks.containerservice.azure.us | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Bu adres Düğüm < > API sunucu iletişimi için gereklidir. * \<Konumu\> * AKS kümenizin dağıtıldığı bölgeyle değiştirin. |
| *.tun. \<yer\>.cx.aks.containerservice.azure.us | HTTPS:443, TCP:22, TCP:9000, UDP:1194 | Bu adres Düğüm < > API sunucu iletişimi için gereklidir. * \<Konumu\> * AKS kümenizin dağıtıldığı bölgeyle değiştirin. |
| mcr.microsoft.com          | HTTPS:443 SAYıLı | Bu adres, Microsoft Kapsayıcı Kayıt Defteri'ndeki (MCR) resimlere erişmek için gereklidir. Bu kayıt defteri, kümenin yükseltilmesi ve ölçeği sırasında kümenin çalışması için gerekli birinci taraf görüntüleri/grafikleri (örneğin, moby, vb.) içerir |
|*.cdn.mscr.io              | HTTPS:443 SAYıLı | Bu adres, Azure İçerik Dağıtım Ağı (CDN) tarafından desteklenen MCR depolama alanı için gereklidir. |
| *.data.mcr.microsoft.com             | HTTPS:443 SAYıLı | Bu adres, Azure içerik dağıtım ağı (CDN) tarafından desteklenen MCR depolama alanı için gereklidir. |
| management.usgovcloudapi.net       | HTTPS:443 SAYıLı | Bu adres Kubernetes GET/PUT işlemleri için gereklidir. |
| login.microsoftonline.us  | HTTPS:443 SAYıLı | Bu adres Azure Etkin Dizin kimlik doğrulaması için gereklidir. |
| ntp.ubuntu.com             | UDP:123   | Bu adres, Linux düğümlerinde NTP zaman eşitlemesi için gereklidir. |
| packages.microsoft.com     | HTTPS:443 SAYıLı | Bu adres, önbelleğe alınmış *apt-get* işlemleri için kullanılan Microsoft paketleri deposudur.  Örnek paketler Moby, PowerShell ve Azure CLI'yi içerir. |
| acs-mirror.azureedge.net      | HTTPS:443 SAYıLı | Bu adres, kubenet ve Azure CNI gibi gerekli ikilileri yüklemek için gereken depo içindir. |

## <a name="optional-recommended-addresses-and-ports-for-aks-clusters"></a>AKS kümeleri için isteğe bağlı önerilen adresler ve bağlantı noktaları

Aşağıdaki giden bağlantı noktaları / ağ kuralları bir AKS kümesi için isteğe bağlıdır:

AKS kümelerinin düzgün çalışması için aşağıdaki FQDN / uygulama kuralları önerilir:

| FQDN                                    | Bağlantı noktası      | Kullanım      |
|-----------------------------------------|-----------|----------|
| security.ubuntu.com, azure.archive.ubuntu.com, changelogs.ubuntu.com | HTTP:80   | Bu adres, Linux küme düğümlerinin gerekli güvenlik düzeltme emlelerini ve güncelleştirmelerini indirmesini sağlar. |

## <a name="required-addresses-and-ports-for-gpu-enabled-aks-clusters"></a>GPU etkin AKS kümeleri için gerekli adresler ve bağlantı noktaları

GPU etkin aks kümeleri için aşağıdaki FQDN / uygulama kuralları gereklidir:

| FQDN                                    | Bağlantı noktası      | Kullanım      |
|-----------------------------------------|-----------|----------|
| nvidia.github.io | HTTPS:443 SAYıLı | Bu adres, GPU tabanlı düğümlerde doğru sürücü yüklemesi ve çalışması için kullanılır. |
| us.download.nvidia.com | HTTPS:443 SAYıLı | Bu adres, GPU tabanlı düğümlerde doğru sürücü yüklemesi ve çalışması için kullanılır. |
| apt.dockerproject.org | HTTPS:443 SAYıLı | Bu adres, GPU tabanlı düğümlerde doğru sürücü yüklemesi ve çalışması için kullanılır. |

## <a name="required-addresses-and-ports-with-azure-monitor-for-containers-enabled"></a>Etkin kapsayıcılar için Azure Monitor ile gerekli adresler ve bağlantı noktaları

Kapsayıcılar için Azure Monitörü etkinleştirilmiş AKS kümeleri için aşağıdaki FQDN / uygulama kuralları gereklidir:

| FQDN                                    | Bağlantı noktası      | Kullanım      |
|-----------------------------------------|-----------|----------|
| dc.services.visualstudio.com | HTTPS:443 SAYıLı    | Bu, Azure Monitor kullanarak doğru ölçümler ve izleme telemetrisi içindir. |
| *.ods.opinsights.azure.com    | HTTPS:443 SAYıLı    | Bu, azure monitor tarafından günlük analizi verilerini sindiren ler için kullanılır. |
| *.oms.opinsights.azure.com | HTTPS:443 SAYıLı | Bu adres, günlük analizi hizmetini doğrulamak için kullanılan omsagent tarafından kullanılır. |
|*.microsoftonline.com | HTTPS:443 SAYıLı | Bu, azure monitor ölçümlerinin doğruluğunu doğrulamak ve göndermek için kullanılır. |
|*.monitoring.azure.com | HTTPS:443 SAYıLı | Bu, azure monitor'a metrik verileri göndermek için kullanılır. |

## <a name="required-addresses-and-ports-with-azure-dev-spaces-enabled"></a>Azure Geliştirme Alanları etkinleştirilmiş gerekli adresler ve bağlantı noktaları

Azure Dev Spaces etkinleştirilmiş AKS kümeleri için aşağıdaki FQDN / uygulama kuralları gereklidir:

| FQDN                                    | Bağlantı noktası      | Kullanım      |
|-----------------------------------------|-----------|----------|
| cloudflare.docker.com | HTTPS:443 SAYıLı | Bu adres, linux alp ve diğer Azure Dev Spaces görüntülerini çekmek için kullanılır |
| gcr.io | HTTP:443 SAYıLı | Bu adres dümen/çapa makinesi görüntülerini çekmek için kullanılır |
| storage.googleapis.com | HTTP:443 SAYıLı | Bu adres dümen/çapa makinesi görüntülerini çekmek için kullanılır |
| azds-\<\>rehberlik . \<yer\>.azds.io | HTTPS:443 SAYıLı | Denetleyiciniz için Azure Dev Spaces arka uç hizmetleriyle iletişim kurmak için. Tam FQDN bulunabilir "dataplaneFqdn" içinde %USERPROFILE%\.azds\settings.json |

## <a name="required-addresses-and-ports-for-aks-clusters-with-azure-policy-in-public-preview-enabled"></a>Azure İlkesi (genel önizlemede) etkin olan AKS kümeleri için gerekli adresler ve bağlantı noktaları

> [!CAUTION]
> Aşağıdaki özelliklerden bazıları önizlemededir.  Özellik genel önizleme ve gelecekteki sürüm aşamalarına taşındığında, bu makaledeki öneriler değişebilir.

Azure İlkesi etkinleştirilmiş AKS kümeleri için aşağıdaki FQDN / uygulama kuralları gereklidir.

| FQDN                                    | Bağlantı noktası      | Kullanım      |
|-----------------------------------------|-----------|----------|
| gov-prod-policy-data.trafficmanager.net | HTTPS:443 SAYıLı | Bu adres, Azure İlkesi'nin doğru çalışması için kullanılır. (şu anda AKS önizleme) |
| raw.githubusercontent.com | HTTPS:443 SAYıLı | Bu adres, Azure İlkesi'nin doğru çalışmasını sağlamak için yerleşik ilkeleri GitHub'dan çekmek için kullanılır. (şu anda AKS önizleme) |
| *.gk. \<yer\>.azmk8s.io | HTTPS:443 SAYıLı    | Denetim sonuçlarını almak için ana sunucuda çalışan Gatekeeper denetim bitiş noktasıyla konuşan Azure ilke eklentisi. |
| dc.services.visualstudio.com | HTTPS:443 SAYıLı | Uygulama öngörüleri bitiş noktasına telemetri verileri gönderen Azure ilke eklentisi. |

## <a name="required-by-windows-server-based-nodes-in-public-preview-enabled"></a>Windows Server tabanlı düğümler (genel önizlemede) etkin

> [!CAUTION]
> Aşağıdaki özelliklerden bazıları önizlemededir.  Özellik genel önizleme ve gelecekteki sürüm aşamalarına taşındığında, bu makaledeki öneriler değişebilir.

Windows Server tabanlı AKS kümeleri için aşağıdaki FQDN / uygulama kuralları gereklidir:

| FQDN                                    | Bağlantı noktası      | Kullanım      |
|-----------------------------------------|-----------|----------|
| onegetcdn.azureedge.net, winlayers.blob.core.windows.net, winlayers.cdn.mscr.io, go.microsoft.com | HTTPS:443 SAYıLı | Windows ile ilgili ikilileri yüklemek için |
| mp.microsoft.com,<span></span>www .msftconnecttest.com, ctldl.windowsupdate.com | HTTP:80 | Windows ile ilgili ikilileri yüklemek için |
| kms.core.windows.net | TCP:1688 | Windows ile ilgili ikilileri yüklemek için |


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, kümenin çıkış trafiğini kısıtlarsanız hangi bağlantı noktalarına ve adreslerine izin verileceği öğrenildi. Ayrıca, bölmelerin nasıl iletişim kurabileceğini ve küme içinde ne gibi kısıtlamaları olduğunu da tanımlayabilirsiniz. Daha fazla bilgi için [aks ağ ilkelerini kullanarak bölmeler arasındaki güvenli trafiği][network-policy]görün.

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
