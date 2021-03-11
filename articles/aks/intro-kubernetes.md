---
title: Azure Kubernetes Service'e Giriş
description: Azure’da kapsayıcı tabanlı uygulamaları dağıtmak ve yönetmek için Azure Kubernetes Hizmetinin özelliklerini ve avantajlarını öğrenin.
services: container-service
ms.topic: overview
ms.date: 02/09/2021
ms.custom: mvc
ms.openlocfilehash: 58a467d697e782b3e21e7b488b7db4c9b8951b2a
ms.sourcegitcommit: d135e9a267fe26fbb5be98d2b5fd4327d355fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/10/2021
ms.locfileid: "102616851"
---
# <a name="azure-kubernetes-service-aks"></a>Azure Kubernetes Service (AKS)

Azure Kubernetes hizmeti (AKS), Azure 'da karmaşıklık ve operasyonel ek yükün çoğunu boşaltarak Azure 'da yönetilen bir Kubernetes kümesinin dağıtılmasını basitleştirir. Barındırılan bir Kubernetes hizmeti olarak Azure, sistem durumu izleme ve bakım gibi kritik görevleri sizin için işler.  

Kubernetes yöneticileri Azure tarafından yönetildiğinden yalnızca aracı düğümlerini yönetebilir ve bakımını yapabilirsiniz. Bu nedenle, yönetilen bir Kubernetes hizmeti olarak AKS ücretsizdir; ana bilgisayarlar için değil, yalnızca kümelerinizdeki aracı düğümleri için ödeme yaparsınız.  

Azure portal, Azure CLı, Azure PowerShell veya Kaynak Yöneticisi şablonları ve Tertuform gibi şablon odaklı dağıtım seçeneklerini kullanarak bir AKS kümesi oluşturabilirsiniz. Bir AKS kümesini dağıttığınızda ana düğümler ve diğer tüm Kubernetes düğümleri sizin yerinize dağıtılır ve yapılandırılır. Gelişmiş ağ özellikleri, Azure Active Directory tümleştirmesi ve izleme gibi ek özellikler de dağıtım işlemi sırasında yapılandırılabilir. Windows Server kapsayıcıları AKS 'de desteklenir.

Kubernetes temelleri hakkında daha fazla bilgi için bkz. [AKS Için Kubernetes temel kavramları][concepts-clusters-workloads].

Başlamak için [Azure Portal][aks-portal] veya [Azure CLI Ile][aks-cli]aks hızlı başlangıcını doldurun.

[!INCLUDE [azure-lighthouse-supported-service](../../includes/azure-lighthouse-supported-service.md)]

## <a name="access-security-and-monitoring"></a>Erişim, güvenlik ve izleme

Gelişmiş güvenlik ve yönetim için, AKS Azure Active Directory (Azure AD) ile tümleştirmenize olanak tanır ve:
* Kubernetes rol tabanlı erişim denetimini (Kubernetes RBAC) kullanın. 
* Kümenizin ve kaynaklarınızın sistem durumunu izleyin.

### <a name="identity-and-security-management"></a>Kimlik ve güvenlik yönetimi

Küme kaynaklarına erişimi sınırlandırmak için AKS, [Kubernetes RBAC][kubernetes-rbac]'yi destekler. Kubernetes RBAC, Kubernetes kaynakları ve ad alanları için erişimi ve izinleri denetlemenize olanak tanır.  

Ayrıca, Azure AD ile tümleşecek bir AKS kümesi de yapılandırabilirsiniz. Azure AD tümleştirmesi ile Kubernetes erişimini mevcut kimliğe ve grup üyeliğine göre yapılandırabilirsiniz. Mevcut Azure AD kullanıcılarınız ve gruplarınız, tümleşik bir oturum açma deneyimi ve AKS kaynaklarına erişim ile birlikte sağlanmış olabilir.  

Kimlik hakkında daha fazla bilgi için bkz. [AKS Için erişim ve kimlik seçenekleri][concepts-identity].

AKS kümelerinizin güvenliğini sağlamak için, bkz. [Azure Active Directory’yi AKS ile Tümleştirme][aks-aad].

### <a name="integrated-logging-and-monitoring"></a>Tümleşik günlüğe kaydetme ve izleme

Kapsayıcı sistem durumu için Azure Izleyici, AKS kümenizde ve dağıtılan uygulamalarda bulunan kapsayıcılardan, düğümlerden ve denetleyicilerden bellek ve işlemci performans ölçümlerini toplar. Hem kapsayıcı günlüklerini hem de [Kubernetes ana günlüklerini][aks-master-logs]gözden geçirebilirsiniz. Bu izleme verileri bir Azure Log Analytics çalışma alanında depolanır ve Azure portal, Azure CLı veya REST uç noktası aracılığıyla kullanılabilir.

Daha fazla bilgi için bkz. [Azure Kubernetes Hizmeti kapsayıcısı sistem durumunu izleme][container-health].

## <a name="clusters-and-nodes"></a>Kümeler ve düğümler

AKS düğümleri Azure sanal makinelerinde (VM 'Ler) çalışır. AKS düğümleri sayesinde, depolama alanı düğümlerine ve yığınlara bağlanabilir, küme bileşenlerini yükseltebilir ve GPU 'Lar kullanabilirsiniz. AKS, karma işletim sistemlerini ve Windows Server kapsayıcılarını desteklemek için birden çok düğüm havuzu çalıştıran Kubernetes kümelerini destekler.  

Kubernetes kümesi, düğümü ve düğüm havuzu özellikleri hakkında daha fazla bilgi için bkz. [AKS Için Kubernetes temel kavramları][concepts-clusters-workloads].

### <a name="cluster-node-and-pod-scaling"></a>Küme düğümü ve pod ölçeklendirme

Kaynaklar için talep değiştikçe, hizmetlerinizi çalıştıran küme düğümleri veya pod’lar otomatik olarak büyüyebilir veya küçülebilir. Yatay pod otomatik ölçeklendiricisini veya küme otomatik ölçeklendiricisini kullanabilirsiniz. Bu ölçeklendirme yaklaşımı AKS kümesinin taleplere otomatik olarak ayarlanmasını ve yalnızca ihtiyaç duyulan kaynakları çalıştırmasını sağlar.

Daha fazla bilgi için bkz.[Azure Kubernetes Service (AKS) kümesini ölçeklendirme][aks-scale].

### <a name="cluster-node-upgrades"></a>Küme düğümü yükseltmeleri

AKS birden fazla Kubernetes sürümü sunmaktadır. Yeni sürümler AKS'de kullanılabilir duruma geldikçe Azure portal veya Azure CLI kullanarak kümenizi yükseltebilirsiniz. Yükseltme işlemi sırasında, çalışan uygulamaların kesintiye uğramasını azaltmak için düğümler dikkatli bir şekilde kordonlanır ve boşaltılır.  

Yaşam döngüsü sürümleri hakkında daha fazla bilgi edinmek için, bkz. [AKS’de desteklenen Kubernetes sürümleri][aks-supported versions]. Yükseltmeyle ilgili adımlar için bkz. [Azure Kubernetes Service (AKS) kümesini yükseltme][aks-upgrade].

### <a name="gpu-enabled-nodes"></a>GPU etkin düğümler

AKS, GPU etkin düğüm havuzlarının oluşturulmasını destekler. Azure Şu anda tek veya birden çok GPU özellikli VM sağlıyor. GPU özellikli VM 'Ler, işlem yoğunluğu, grafik yoğun ve görselleştirme iş yükleri için tasarlanmıştır.

Daha fazla bilgi için bkz. [AKS üzerinde GPU kullanma][aks-gpu].

### <a name="confidential-computing-nodes-public-preview"></a>Gizli bilgi işlem düğümleri (Genel Önizleme)

AKS, Intel SGX tabanlı, gizli bilgi işlem düğüm havuzlarının (DCSv2 VM 'Ler) oluşturulmasını destekler. Gizli bilgi işlem düğümleri, kapsayıcıların donanım tabanlı, güvenilir bir yürütme ortamında (şifreleme) çalışmasına izin verir. Kanıtlama ile kod bütünlüğü ile birleştirilmiş kapsayıcılar arasındaki yalıtım, derinlemesine savunma kapsayıcısı güvenlik stratejinize yardımcı olabilir. Gizli bilgi işlem düğümleri hem gizli kapsayıcıları (mevcut Docker uygulamaları) hem de şifreleme kullanan kapsayıcıları destekler.

Daha fazla bilgi için bkz. [AKS üzerinde gizli bilgi işlem düğümleri][conf-com-node].

### <a name="storage-volume-support"></a>Depolama birimi desteği

Uygulama iş yüklerini desteklemek isterseniz, kalıcı veriler için depolama birimleri bağlayabilirsiniz. Hem statik hem de dinamik birimleri kullanabilirsiniz. Depolama birimlerinin paylaşılması beklenen bağlı FID sayısına bağlı olarak, tek Pod erişimi için Azure diskleri veya birden çok eşzamanlı Pod erişimi için Azure dosyaları tarafından desteklenen depolama alanını kullanabilirsiniz.

Daha fazla bilgi için bkz. [AKS 'teki uygulamalar Için depolama seçenekleri][concepts-storage].

[Azure diskleri][azure-disk] veya [Azure dosyalarını][azure-files]kullanarak dinamik kalıcı birimler ile çalışmaya başlayın.

## <a name="virtual-networks-and-ingress"></a>Sanal ağlar ve giriş

AKS kümesi var olan bir sanal ağa dağıtılabilir. Bu yapılandırmada, kümedeki her Pod sanal ağda bir IP adresi atanır ve kümedeki diğer düğüm ve sanal ağdaki diğer düğümlerde doğrudan iletişim kurabilir. Pods Ayrıca, eşlenmiş bir sanal ağdaki diğer hizmetlere ve ExpressRoute veya siteden siteye (S2S) VPN bağlantıları üzerinden şirket içi ağlara bağlanabilir.  

Daha fazla bilgi için bkz. [AKS 'teki uygulamalar Için ağ kavramları][aks-networking].

### <a name="ingress-with-http-application-routing"></a>HTTP uygulama yönlendirmesiyle giriş

HTTP uygulama yönlendirmesi eklentisi, AKS kümenize dağıtılan uygulamalara daha kolay erişmenizi sağlar. HTTP uygulama yönlendirmesi çözümü etkinleştirildiğinde AKS kümenizde bir giriş denetleyicisi yapılandırır.  

Uygulamalar dağıtıldığında, genel olarak erişilebilen DNS adları otomatik olarak yapılandırılır. HTTP uygulama yönlendirmesi bir DNS bölgesi ayarlar ve bunu AKS kümesiyle tümleştirir. Ardından, Kubernetes giriş kaynaklarını normal olarak dağıtabilirsiniz.  

Giriş trafiği ile başlamak için, bkz. [HTTP uygulama yönlendirmesi][aks-http-routing].

## <a name="development-tooling-integration"></a>Geliştirme araçlarıyla tümleştirme

Kubernetes, AKS ile sorunsuz şekilde çalışan bir geliştirme ve yönetim araçları zengin ekosistemine sahiptir. Bu araçlar, Visual Studio Code için helk ve Kubernetes uzantısını içerir. Bu araçlar, AKS ile sorunsuz bir şekilde çalışır.  

Ayrıca, Azure, DevOps Starter gibi Kubernetes 'i kolaylaştırmaya yardımcı olan çeşitli araçlar sağlar.  

DevOps Starter, mevcut kod ve Git depolarını Azure 'a getirmek için basit bir çözüm sunar. DevOps başlangıç otomatik:
* Azure kaynakları (örneğin, AKS) oluşturur; 
* Azure DevOps Services, CI için derleme işlem hattı içeren bir yayın işlem hattını yapılandırır. 
* CD için bir yayın işlem hattı ayarlar; ' 
* İzleme için bir Azure Application Insights kaynağı oluşturur. 

Daha fazla bilgi için bkz. [DevOps Starter][azure-devops].

## <a name="docker-image-support-and-private-container-registry"></a>Docker görüntü desteği ve özel kapsayıcı kayıt defteri

AKS, Docker görüntü biçimini destekler. Docker görüntülerinizin özel olarak depolanması için, AKS’yi Azure Container Registry (ACR) ile tümleştirebilirsiniz.

Özel bir görüntü deposu oluşturmak için, bkz. [Azure Container Registry][acr-docs].

## <a name="kubernetes-certification"></a>Kubernetes sertifikası

AKS 'ler, Kubernetes uyumlu olarak onaylandı.

## <a name="regulatory-compliance"></a>Mevzuata uyumluluk

AKS, SOC, ISO, PCI DSS ve HIPAA ile uyumludur. Daha fazla bilgi için bkz. [Microsoft Azure uyumluluğuna genel bakış][compliance-doc].

## <a name="next-steps"></a>Sonraki adımlar

Azure CLı hızlı başlangıç ile AKS dağıtma ve yönetme hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [AKS hızlı başlangıç][aks-cli]

<!-- LINKS - external -->
[aks-engine]: https://github.com/Azure/aks-engine
[kubectl-overview]: https://kubernetes.io/docs/user-guide/kubectl-overview/
[compliance-doc]: https://azure.microsoft.com/en-us/overview/trusted-cloud/compliance/

<!-- LINKS - internal -->
[acr-docs]: ../container-registry/container-registry-intro.md
[aks-aad]: ./azure-ad-integration-cli.md
[aks-cli]: ./kubernetes-walkthrough.md
[aks-gpu]: ./gpu-cluster.md
[aks-http-routing]: ./http-application-routing.md
[aks-networking]: ./concepts-network.md
[aks-portal]: ./kubernetes-walkthrough-portal.md
[aks-scale]: ./tutorial-kubernetes-scale.md
[aks-upgrade]: ./upgrade-cluster.md
[azure-dev-spaces]: ../dev-spaces/index.yml
[azure-devops]: ../devops-project/overview.md
[azure-disk]: ./azure-disks-dynamic-pv.md
[azure-files]: ./azure-files-dynamic-pv.md
[container-health]: ../azure-monitor/containers/container-insights-overview.md
[aks-master-logs]: ./view-control-plane-logs.md
[aks-supported versions]: supported-kubernetes-versions.md
[concepts-clusters-workloads]: concepts-clusters-workloads.md
[kubernetes-rbac]: concepts-identity.md#kubernetes-role-based-access-control-kubernetes-rbac
[concepts-identity]: concepts-identity.md
[concepts-storage]: concepts-storage.md
[conf-com-node]: ../confidential-computing/confidential-nodes-aks-overview.md
