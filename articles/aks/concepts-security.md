---
title: Kavramlar - Azure Kubernetes Hizmetlerinde Güvenlik (AKS)
description: Ana ve düğüm iletişimi, ağ ilkeleri ve Kubernetes sırları da dahil olmak üzere Azure Kubernetes Hizmeti'nde (AKS) güvenlik hakkında bilgi edinin.
services: container-service
ms.topic: conceptual
ms.date: 03/01/2019
ms.openlocfilehash: 7238e6cd7ab3625e2953a4408c82802d43372256
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77595952"
---
# <a name="security-concepts-for-applications-and-clusters-in-azure-kubernetes-service-aks"></a>Azure Kubernetes Hizmeti'ndeki (AKS) uygulamalar ve kümeler için güvenlik kavramları

Azure Kubernetes Hizmeti'nde (AKS) uygulama iş yüklerini çalıştırırken müşteri verilerinizi korumak için kümenizin güvenliği önemli bir husustur. Kubernetes *ağ ilkeleri* ve *Sırlar*gibi güvenlik bileşenleri içerir. Azure daha sonra ağ güvenlik grupları ve düzenlenmiş küme yükseltmeleri gibi bileşenler ekler. Bu güvenlik bileşenleri, AKS kümenizin en son işletim sistemi güvenlik güncelleştirmelerini ve Kubernetes sürümlerini çalıştırmasını ve güvenli bölme trafiği ve hassas kimlik bilgilerine erişimi sağlamak için birleştirilir.

Bu makalede, AKS uygulamalarınızı güvenli temel kavramları tanıtır:

- [Ana bileşenler güvenliği](#master-security)
- [Düğüm güvenliği](#node-security)
- [Küme yükseltmeleri](#cluster-upgrades)
- [Ağ güvenliği](#network-security)
- [Kubernetes Gizli Dizileri](#kubernetes-secrets)

## <a name="master-security"></a>Ana güvenlik

AKS'de, Kubernetes ana bileşenleri Microsoft tarafından sağlanan yönetilen hizmetin bir parçasıdır. Her AKS kümeapi Server, Scheduler, vb sağlamak için kendi tek kiracılı, özel Kubernetes ana vardır. Bu ana microsoft tarafından yönetilir ve korunur.

Varsayılan olarak, Kubernetes API sunucusu ortak bir IP adresi ve tam nitelikli etki alanı adı (FQDN) kullanır. Kubernetes rol tabanlı erişim denetimlerini ve Azure Etkin Dizini'ni kullanarak API sunucusuna erişimi denetleyebilirsiniz. Daha fazla bilgi için [AKS ile Azure AD tümleştirmesi'ne][aks-aad]bakın.

## <a name="node-security"></a>Düğüm güvenliği

AKS düğümleri, yönettiğiniz ve koruduğunuz Azure sanal makineleridir. Linux düğümleri Moby kapsayıcı çalışma süresini kullanarak optimize edilmiş bir Ubuntu dağıtımı çalıştırın. Windows Server düğümleri (şu anda AKS önizlemede) en iyi duruma getirilmiş bir Windows Server 2019 sürümü çalıştırın ve Moby kapsayıcı çalışma süresini de kullanır. Bir AKS kümesi oluşturulduğunda veya büyütüldüğünde, düğümler en son işletim sistemi güvenlik güncelleştirmeleri ve yapılandırmalarıyla otomatik olarak dağıtılır.

Azure platformu, her gece Linux düğümlerine os güvenlik yamaları otomatik olarak uygular. Linux OS güvenlik güncelleştirmesi ana bilgisayar yeniden başlatma gerektiriyorsa, bu yeniden başlatma otomatik olarak gerçekleştirilmez. Linux düğümlerini el ile yeniden başlatabilirsiniz veya ortak bir yaklaşım Kubernetes için açık kaynak kodlu bir yeniden başlatma daemonu olan [Kured'i][kured]kullanmaktır. Kured, [DaemonSet][aks-daemonsets] olarak çalışır ve yeniden başlatma nın gerekli olduğunu belirten bir dosyanın varlığı için her düğümü izler. Yeniden başlatmalar küme yükseltmesi ile aynı [kordon ve boşaltma işlemi](#cordon-and-drain) kullanılarak küme genelinde yönetilir.

Windows Server düğümleri (şu anda AKS önizlemede), Windows Update otomatik olarak çalışmaz ve en son güncelleştirmeleri uygulamaz. Windows Update sürüm döngüsü ve kendi doğrulama işleminiz etrafında düzenli bir zamanlamada, AKS kümenizde Windows Server düğüm havuzunda(lar) yükseltme gerçekleştirmeniz gerekir. Bu yükseltme işlemi, en son Windows Server görüntüsünü çalıştıran düğümler ve düzeltme etekleri oluşturur ve eski düğümleri kaldırır. Bu işlem hakkında daha fazla bilgi için [AKS'deki düğüm havuzuna yükseltme bölümüne][nodepool-upgrade]bakın.

Düğümler, ortak IP adresleri atanmamış özel bir sanal ağ alt ağına dağıtılır. Sorun giderme ve yönetim amacıyla, SSH varsayılan olarak etkinleştirilir. Bu SSH erişimi yalnızca dahili IP adresi kullanılarak kullanılabilir.

Depolama sağlamak için düğümler Azure Yönetilen Diskler'i kullanır. Çoğu VM düğümü boyutu için, bunlar yüksek performanslı SSD'ler tarafından desteklenen Premium disklerdir. Yönetilen disklerde depolanan veriler, Azure platformunda dinlenime de otomatik olarak şifrelenir. Artıklığı artırmak için bu diskler Azure veri merkezi içinde de güvenli bir şekilde çoğaltılır.

Kubernetes ortamları, AKS veya başka bir yerde, şu anda düşmanca çok kiracı kullanımı için tamamen güvenli değildir. *Pod Güvenlik İlkeleri* veya düğümler için daha ince taneli rol tabanlı erişim denetimleri (RBAC) gibi ek güvenlik özellikleri, yararlanmaları daha da zorlaştırır. Ancak, düşmanca çok kiracılı iş yüklerini çalıştırırken gerçek güvenlik için, bir hipervizör güvenmeniz gereken tek güvenlik düzeyidir. Kubernetes için güvenlik etki alanı tek bir düğüm değil, tüm küme olur. Bu tür çok kiracılı çok kiracılı iş yükleri için fiziksel olarak yalıtılmış kümeler kullanmalısınız. İş yüklerini yalıtmanın yolları hakkında daha fazla bilgi için [AKS'de küme yalıtımı için en iyi uygulamalara][cluster-isolation]bakın,

## <a name="cluster-upgrades"></a>Küme yükseltmeleri

Azure, güvenlik ve uyumluluk veya en son özellikleri kullanmak için bir AKS kümesinin ve bileşenlerinin yükseltinmesini düzenlemek için araçlar sağlar. Bu yükseltme orkestrasyonu hem Kubernetes ana sını hem de aracı bileşenlerini içerir. AKS kümeniz için [kullanılabilir Kubernetes sürümlerinin listesini](supported-kubernetes-versions.md) görüntüleyebilirsiniz. Yükseltme işlemini başlatmak için, bu kullanılabilir sürümlerden birini belirtirsiniz. Azure daha sonra her AKS düğümlerini güvenli bir şekilde kordon altına alıp boşaltır ve yükseltmeyi gerçekleştirir.

### <a name="cordon-and-drain"></a>Kordon ve drenaj

Yükseltme işlemi sırasında AKS düğümleri kümeden ayrı ayrı kordon altına alınır, böylece yeni bölmeler zamanlanmaz. Düğümler daha sonra boşaltılır ve aşağıdaki gibi yükseltilir:

- Düğüm havuzuna yeni bir düğüm dağıtılır. Bu düğüm en son işletim sistemi görüntüsünü ve düzeltme emlelerini çalıştırır.
- Varolan düğümlerden biri yükseltme için tanımlanır. Bu düğümdeki bölmeler incelikle sonlandırılır ve düğüm havuzundaki diğer düğümlerde zamanlanır.
- Varolan bu düğüm AKS kümesinden silinir.
- Kümedeki bir sonraki düğüm, yükseltme işleminin bir parçası olarak tüm düğümler başarıyla değiştirilene kadar aynı işlem kullanılarak kordon altına alınır ve boşaltılır.

Daha fazla bilgi için [bir AKS kümesini yükseltme'ye][aks-upgrade-cluster]bakın.

## <a name="network-security"></a>Ağ güvenliği

Şirket içi ağlarla bağlantı ve güvenlik için AKS kümenizi mevcut Azure sanal ağ alt ağlarına dağıtabilirsiniz. Bu sanal ağlarda, şirket içi ağınıza azure siteden siteye VPN veya Ekspres Rota bağlantısı olabilir. Kubernetes giriş denetleyicileri özel, dahili IP adresleri ile tanımlanabilir, böylece hizmetlere yalnızca bu dahili ağ bağlantısı üzerinden erişilebilir.

### <a name="azure-network-security-groups"></a>Azure ağ güvenlik grupları

Azure, sanal ağlardaki trafik akışını filtrelemek için ağ güvenliği grubu kurallarını kullanır. Bu kurallar, kaynaklara erişime izin verilen veya reddedilen kaynak ve hedef IP aralıklarını, bağlantı noktalarını ve protokolleri tanımlar. Varsayılan kurallar, TlS trafiğinin Kubernetes API sunucusuna girmesine izin vermek için oluşturulur. Yük dengeleyicileri, bağlantı noktası eşlemeleri veya giriş yolları içeren hizmetler oluştururken, AKS trafiğin uygun şekilde akması için ağ güvenlik grubunu otomatik olarak değiştirir.

## <a name="kubernetes-secrets"></a>Kubernetes Gizli Dizileri

Kubernetes *Secret,* erişim kimlik bilgileri veya anahtarlar gibi hassas verileri bölmelere enjekte etmek için kullanılır. Önce Kubernetes API'sini kullanarak bir Gizli oluşturursunuz. Bölmenizi veya dağıtımınızı tanımladığınızda, belirli bir Gizli istenebilir. Sırlar yalnızca bunu gerektiren zamanlanmış bir bölmeye sahip düğümlere sağlanır ve Gizli, diske yazılmayan *tmpfs'de*depolanır. Gizli gerektiren bir düğümdeki son bölme silindiğinde, Gizli düğümün tmpfs'sinden silinir. Sırlar belirli bir ad alanı içinde depolanır ve yalnızca aynı ad alanı içindeki bölmeler tarafından erişilebilir.

Sırlar kullanımı bölme veya hizmet YAML bildiriminde tanımlanan hassas bilgileri azaltır. Bunun yerine, YAML manifestonuzun bir parçası olarak Kubernetes API Server'da depolanan Gizli'yi talep edersiniz. Bu yaklaşım yalnızca Gizli belirli pod erişim sağlar. Lütfen dikkat: ham gizli manifesto dosyaları base64 formatında gizli verileri içerir (daha fazla ayrıntı için [resmi belgelere][secret-risks] bakın). Bu nedenle, bu dosya hassas bilgiler olarak ele alınmalıdır ve kaynak denetimine asla taahhüt edilmemelidir.

## <a name="next-steps"></a>Sonraki adımlar

AKS kümelerinizi güvence altına almaya başlamak için [bir AKS kümesini Yükselt'e][aks-upgrade-cluster]bakın.

İlişkili en iyi uygulamalar [için, aks küme güvenliği ve yükseltmeleri için en iyi uygulamalar][operator-best-practices-cluster-security] ve [AKS'de pod güvenliği için en iyi uygulamalara][developer-best-practices-pod-security]bakın.

Çekirdek Kubernetes ve AKS kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Kubernetes / AKS kümeleri ve iş yükleri][aks-concepts-clusters-workloads]
- [Kubernetes / AKS kimliği][aks-concepts-identity]
- [Kubernetes / AKS sanal ağlar][aks-concepts-network]
- [Kubernetes / AKS depolama][aks-concepts-storage]
- [Kubernetes / AKS ölçeği][aks-concepts-scale]

<!-- LINKS - External -->
[kured]: https://github.com/weaveworks/kured
[kubernetes-network-policies]: https://kubernetes.io/docs/concepts/services-networking/network-policies/
[secret-risks]: https://kubernetes.io/docs/concepts/configuration/secret/#risks

<!-- LINKS - Internal -->
[aks-daemonsets]: concepts-clusters-workloads.md#daemonsets
[aks-upgrade-cluster]: upgrade-cluster.md
[aks-aad]: azure-ad-integration.md
[aks-concepts-clusters-workloads]: concepts-clusters-workloads.md
[aks-concepts-identity]: concepts-identity.md
[aks-concepts-scale]: concepts-scale.md
[aks-concepts-storage]: concepts-storage.md
[aks-concepts-network]: concepts-network.md
[cluster-isolation]: operator-best-practices-cluster-isolation.md
[operator-best-practices-cluster-security]: operator-best-practices-cluster-security.md
[developer-best-practices-pod-security]:developer-best-practices-pod-security.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
