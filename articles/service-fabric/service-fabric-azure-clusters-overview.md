---
title: Windows Server ve Linux'ta kümeler oluşturma
description: Service Fabric kümeleri Windows Server ve Linux'ta çalışır, bu da Service Fabric uygulamalarını Windows Server veya Linux çalıştırabileceğiniz her yerde dağıtabileceğiniz ve barındırabileceğiniz anlamına gelir.
services: service-fabric
documentationcenter: .net
author: dkkapur
ms.topic: conceptual
ms.date: 02/01/2019
ms.author: dekapur
ms.openlocfilehash: b6942c2a0647401df0d88b83e1b144ca3207a6db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614681"
---
# <a name="overview-of-service-fabric-clusters-on-azure"></a>Azure'da Hizmet Kumaşı kümelerine genel bakış
Service Fabric kümesi, mikro hizmetlerinizin dağıtıldığı ve yönetildiği ağa bağlı sanal veya fiziksel makineler kümesidir. Kümenin bir parçası olan bir makine veya VM'ye küme düğümü denir. Kümeler binlerce düğüme ölçeklenebilir. Kümeye yeni düğümler eklerseniz, Service Fabric artan düğüm sayısı boyunca hizmet bölümü yinelemelerini ve örneklerini yeniden dengeler. Genel uygulama performansı artırır ve belleğe erişim için çekişme azalır. Kümedeki düğümler verimli bir şekilde kullanılıyorsa, kümedeki düğüm sayısını azaltabilirsiniz. Service Fabric, her düğümdeki donanımı daha iyi kullanmak için bölüm yinelemelerini ve örnekleri azalan düğüm sayısı boyunca yeniden dengeler.

Düğüm türü kümedeki bir düğüm kümesinin (sanal makineler) boyutunu, sayısını ve özelliklerini tanımlar. Daha sonra, her düğüm türünün ölçeği birbirinden bağımsız olarak artırılabilir veya azaltılabilir, her düğüm türünde farklı bağlantı noktası kümeleri açık olabilir ve farklı kapasite ölçümleri yapılabilir. Düğüm türleri, bir düğüm kümesinin "ön uç" veya "arka uç" şeklindeki rolünün tanımlanması için kullanılır. Kümenizde birden çok düğüm türü olabilir, ancak üretim kümeleri için birincil düğüm türünde en az beş VM (veya test kümeleri için en az üç VM) olmalıdır. [Service Fabric sistem hizmetleri](service-fabric-technical-overview.md#system-services), birincil düğüm türündeki düğümlere yerleştirilir. 

## <a name="cluster-components-and-resources"></a>Bileşenleri ve kaynakları kümele
Azure'daki Hizmet Dokusu kümesi, diğer Azure kaynaklarını kullanan ve bunlarla etkileşimde olan bir Azure kaynağıdır:
* Sanal M'ler ve sanal ağ kartları
* sanal makine ölçek kümeleri
* sanal ağlar
* yük dengeleyiciler
* depolama hesapları
* genel IP adresleri

![Servis Kumaş Kümesi][Image]

### <a name="virtual-machine"></a>Sanal makine
Bir kümenin parçası olan [sanal makineye](/azure/virtual-machines/) düğüm adı verilir, ancak teknik olarak küme düğümü Hizmet Kumaşı çalışma zamanı işlemidir. Her düğüme bir düğüm adı (bir dize) atanır. Düğümlerin [yerleşim özellikleri](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)gibi özellikleri vardır. Her makine veya VM bir otomatik başlangıç hizmeti vardır, *FabricHost.exe*, önyükleme zamanda çalışan başlar ve daha sonra iki çalıştırılabilir başlar, *Fabric.exe* ve *FabricGateway.exe*, düğüm oluşturan. Üretim dağıtımı, fiziksel veya sanal makine başına bir düğümdür. Test senaryoları için, *Fabric.exe* ve *FabricGateway.exe*birden fazla örnekleri çalıştırarak tek bir makine veya VM üzerinde birden fazla düğüm barındırabilir.

Her VM bir sanal ağ arabirim kartı (NIC) ile ilişkilidir ve her NIC'ye özel bir IP adresi atanır.  Bir VM NIC üzerinden sanal ağa ve yerel dengeleyiciye atanır.

Kümedeki tüm VM'ler sanal ağa yerleştirilir.  Aynı düğüm türü/ölçek kümesindeki tüm düğümler sanal ağdaki aynı alt ağa yerleştirilir.  Bu düğümler yalnızca özel IP adreslerine sahiptir ve sanal ağ dışında doğrudan ele alınmaz.  İstemciler, Azure yük bakiyesi aracılığıyla düğümler üzerindeki hizmetlere erişebilir.

### <a name="scale-setnode-type"></a>Ölçek kümesi/düğüm türü
Bir küme oluşturduğunuzda, bir veya daha fazla düğüm türü tanımlarsınız.  Düğüm türündeki düğümler veya VM'ler, CPU sayısı, bellek, disk sayısı ve disk G/Ç gibi aynı boyut ve özelliklere sahiptir.  Örneğin, bir düğüm türü, bağlantı noktaları internete açık olan küçük, ön uç VM'ler için olabilirken, başka bir düğüm türü verileri işleyen büyük, arka uç VM'ler için olabilir. Azure kümelerinde, her düğüm türü [sanal makine ölçeği kümesine](/azure/virtual-machine-scale-sets/)eşlenir.

Sanal makineler koleksiyonunu bir küme olarak dağıtmak ve yönetmek için ölçek kümelerini kullanabilirsiniz. Azure Hizmet Kumaşı kümesinde tanımladığınız her düğüm türü ayrı bir ölçek kümesi ayarlar. Hizmet Kumaşı çalışma süresi, Azure VM uzantıları kullanılarak ölçek kümesindeki her sanal makineye bağlanır. Bağımsız olarak her düğüm türünü yukarı veya aşağı ölçeklendirebilir, her küme düğümünde çalışan OS SKU'yu değiştirebilir, farklı bağlantı noktaları kümelerini açabilir ve farklı kapasite ölçümleri kullanabilirsiniz. Bir ölçek kümesinin beş [yükseltme etki alanı](service-fabric-cluster-resource-manager-cluster-description.md#upgrade-domains) ve beş hata etki [alanı](service-fabric-cluster-resource-manager-cluster-description.md#fault-domains) vardır ve en fazla 100 VM'ye sahip olabilir.  Birden çok ölçek kümesi/düğüm türü oluşturarak 100'den fazla düğümden oluşan kümeler oluşturursunuz.

> [!IMPORTANT]
> Kümeniz için düğüm türlerinin sayısını ve düğüm türünün her birinin özelliklerini (boyut, birincil, internete dönük, VM sayısı vb.) seçmek önemli bir görevdir.  Daha fazla bilgi için [küme kapasite planlama hususlarını](service-fabric-cluster-capacity.md)okuyun.

Daha fazla bilgi için [Service Fabric düğüm türlerini ve sanal makine ölçek kümelerini](service-fabric-cluster-nodetypes.md)okuyun.

### <a name="azure-load-balancer"></a>Azure Load Balancer
VM örnekleri, ortak bir [IP adresi](/azure/virtual-network/virtual-network-ip-addresses-overview-arm#public-ip-addresses) ve DNS etiketiyle ilişkili bir Azure [yük dengeleyicisi](/azure/load-balancer/load-balancer-overview)arkasında birleştirilir.  * &lt;Küme&gt;adı*, DNS adı, * &lt;küme adı&gt;&lt; ile bir küme sağlama. location&gt;.cloudapp.azure.com,* ölçek kümesinin önündeki yük dengeleyicisiyle ilişkili DNS etiketidir.

Kümedeki VM'lerin yalnızca [özel IP adresleri](/azure/virtual-network/virtual-network-ip-addresses-overview-arm#private-ip-addresses)vardır.  Yönetim trafiği ve servis trafiği, kamunun karşı karşıya olduğu yük dengeleyicisi üzerinden yönlendirilir.  Ağ trafiği bu makinelere NAT kuralları (istemciler belirli düğümlere/örneklere bağlanır) veya yük dengeleme kuralları (trafik VM'lere döner yuvarlak robin'e gider) ile yönlendirilir.  Yük dengeleyicisi biçiminde bir DNS adı ile ilişkili bir ortak IP vardır: * &lt;küme adı&gt;.&lt; yer&gt;.cloudapp.azure.com*.  Ortak IP, kaynak grubundaki başka bir Azure kaynağıdır.  Bir kümede birden çok düğüm türü tanımlarsanız, her düğüm türü/ölçek kümesi için bir yük dengeleyici si oluşturulur. Veya birden çok düğüm türü için tek bir yük dengeleyicisi kurabilirsiniz.  Birincil düğüm türü DNS etiket * &lt;küme&gt;&lt; adı vardır. location&gt;.cloudapp.azure.com*, diğer düğüm türleri DNS etiket * &lt;&gt;-&lt;küme adı&gt;nodetype var.&lt; yer&gt;.cloudapp.azure.com*.

### <a name="storage-accounts"></a>Depolama hesapları
Her küme düğümü türü bir [Azure depolama hesabı](/azure/storage/common/storage-introduction) ve yönetilen diskler tarafından desteklenir.

## <a name="cluster-security"></a>Küme güvenliği
Hizmet Kumaşı kümesi, sahip olduğunuz bir kaynaktır.  Yetkisiz kullanıcıların bu kümelere bağlanmasını önlemeye yardımcı olmak için kümelerinizi güvenli hale getirmek sizin sorumluluğunuzdadır. Kümede üretim iş yüklerini çalıştırırken güvenli küme özellikle önemlidir. 

### <a name="node-to-node-security"></a>Düğümden düğüme güvenlik
Düğümden düğüme güvenlik, bir kümedeki VM'ler veya bilgisayarlar arasındaki iletişimi güvence altına alar. Bu güvenlik senaryosu, kümeye yalnızca kümeye katılmaya yetkili bilgisayarların kümedeki uygulamaları ve hizmetleri barındırmaya katılmasını sağlar. Service Fabric, bir kümeyi güvenli hale getirmek ve uygulama güvenliği özellikleri sağlamak için X.509 sertifikalarını kullanır.  Küme trafiğini güvence altına almak ve küme ve sunucu kimlik doğrulaması sağlamak için küme sertifikası gereklidir.  Test kümeleri için kendi imzalı sertifikalar kullanılabilir, ancak güvenilir bir sertifika yetkilisinden gelen bir sertifika üretim kümelerini güvence altına almak için kullanılmalıdır.

Daha fazla bilgi için [Düğümden düğüme güvenlik](service-fabric-cluster-security.md#node-to-node-security)

### <a name="client-to-node-security"></a>İstemciden düğüme güvenlik
İstemciden düğüme güvenlik istemcilerin kimliğini doğrular ve kümedeki istemci ve tek tek düğümler arasındaki iletişimin güvenli hale ilmesine yardımcı olur. Bu tür güvenlik, kümeye yalnızca yetkili kullanıcıların kümeye ve kümede dağıtılan uygulamalara erişebilmesini sağlamaya yardımcı olur. İstemciler, X.509 sertifika güvenlik kimlik bilgileri yle benzersiz bir şekilde tanımlanır. Herhangi bir sayıda isteğe bağlı istemci sertifikası, yöneticinin veya kümedeki kullanıcı istemcilerinin kimliğini doğrulamak için kullanılabilir.

İstemci sertifikalarına ek olarak, Azure Etkin Dizin kümeile istemcilerin kimliğini doğrulamak için de yapılandırılabilir.

Daha fazla bilgi için [İstemci den düğüme güvenliği](service-fabric-cluster-security.md#client-to-node-security) okuyun

### <a name="role-based-access-control"></a>Rol Tabanlı Access Control
Role Tabanlı Erişim Denetimi (RBAC), Azure kaynaklarında ince taneli erişim denetimleri atamanıza olanak tanır.  Aboneliklere, kaynak gruplarına ve kaynaklara farklı erişim kuralları atayabilirsiniz.  RBAC kuralları, daha düşük bir düzeyde geçersiz kılınmadığı sürece kaynak hiyerarşisi boyunca devralınır.  AAD'nizdeki herhangi bir kullanıcı veya kullanıcı grubu, belirlenmiş kullanıcıların ve grupların kümenizi değiştirebilmeleri için RBAC kurallarıyla atayabilirsiniz.  Daha fazla bilgi için [Azure RBAC genel görünümünü](/azure/role-based-access-control/overview)okuyun.

Service Fabric, farklı kullanıcı grupları için belirli küme işlemlerine erişimi sınırlamak için erişim denetimini de destekler. Bu, kümenin daha güvenli hale getirmeye yardımcı olur. Bir kümeye bağlanan istemciler için iki erişim denetimi türü desteklenir: Yönetici rolü ve Kullanıcı rolü.  

Daha fazla bilgi için [Service Fabric Role-Based Access Control (RBAC)](service-fabric-cluster-security.md#role-based-access-control-rbac)okuyun.

### <a name="network-security-groups"></a>Ağ güvenlik grupları 
Ağ güvenlik grupları (NSG'ler), bir alt ağ, VM veya belirli NIC'nin gelen ve giden trafiğini denetler.  Varsayılan olarak, aynı sanal ağa birden çok VM konulduğunda, herhangi bir bağlantı noktası üzerinden birbirleriyle iletişim kurabilirler.  Makineler arasındaki iletişimi kısıtlamak istiyorsanız, ağı bölmek veya VM'leri birbirinden yalıtmak için NSG'leri tanımlayabilirsiniz.  Bir kümede birden çok düğüm türü varsa, farklı düğüm türlerine ait makinelerin birbiriyle iletişim kurmasını önlemek için alt ağlara NSG uygulayabilirsiniz.  

Daha fazla bilgi için [güvenlik grupları](/azure/virtual-network/security-overview) hakkında bilgi edinin

## <a name="scaling"></a>Ölçeklendirme

Uygulama talepleri zaman içinde değişir. Artan uygulama iş yükünü veya ağ trafiğini karşılamak veya talep düştüğünde küme kaynaklarını azaltmak için küme kaynaklarını artırmanız gerekebilir. Hizmet Kumaşı kümesi oluşturduktan sonra, kümeyi yatay olarak ölçeklendirebilir (düğüm sayısını değiştirebilir) veya dikey olarak (düğümlerin kaynaklarını değiştirebilirsiniz). Kümede iş yükleri çalışıyor olsa bile, kümeyi istediğiniz zaman ölçeklendirebilirsiniz. Küme ölçeklendikçe, uygulamalarınız da otomatik olarak ölçeklenir.

Daha fazla bilgi için [Azure kümelerini ölçekleme'yi](service-fabric-cluster-scaling.md)okuyun.

## <a name="upgrading"></a>Yükseltme
Azure Hizmet Kumaşı kümesi, sahip olduğunuz ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Microsoft, kümenizde temel işletim sistemi yama ve Hizmet Kumaş ı yükseltmeleri gerçekleştirmekten sorumludur. Kümenizi, Microsoft yeni bir sürüm yayımladığında otomatik çalışma zamanı yükseltmeleri alacak şekilde ayarlayabilir veya istediğiniz desteklenen çalışma zamanı sürümünü seçmeyi seçebilirsiniz. Çalışma zamanı yükseltmelerine ek olarak, sertifikalar veya uygulama bağlantı noktaları gibi küme yapılandırmasını da güncelleştirebilirsiniz.

Daha fazla bilgi için [kümeleri Yükseltme'yi](service-fabric-cluster-upgrade.md)okuyun.

## <a name="supported-operating-systems"></a>Desteklenen işletim sistemleri
Bu işletim sistemlerini çalıştıran sanal makinelerde kümeler oluşturabilirsiniz:

| İşletim sistemi | En erken desteklenen Service Fabric sürümü |
| --- | --- |
| Windows Server 2012 R2 | Tüm sürümler |
| Windows Server 2016 | Tüm sürümler |
| Windows Server 1709 | 6.0 |
| Windows Server 1803 | 6.4 |
| Windows Server 1809 | 6.4.654.9590 |
| Windows Server 2019 | 6.4.654.9590 |
| Linux Ubuntu 16.04 | 6.0 |

Ek bilgi için [Azure'da Desteklenen Küme Sürümlerine](https://docs.microsoft.com/azure/service-fabric/service-fabric-versions#supported-operating-systems) bakın

> [!NOTE]
> Service Fabric'i Windows Server 1709'a dağıtmaya karar verirseniz, (1) uzun vadeli bir servis dalı olmadığını, bu nedenle sürümleri gelecekte taşımanız gerekebileceğini ve (2) kapsayıcıları dağıtırsanız, Windows Server 2016'da yerleşik kapsayıcıların Windows Server'da çalışmadığını lütfen unutmayın 1709, ve tersi (bunları dağıtmak için yeniden inşa etmek zorunda kalacak).
>


## <a name="next-steps"></a>Sonraki adımlar
Azure kümelerini [güvence altına alma,](service-fabric-cluster-security.md) [ölçekleme](service-fabric-cluster-scaling.md)ve [yükseltme](service-fabric-cluster-upgrade.md) hakkında daha fazla bilgi edinin.

Service [Fabric destek seçenekleri](service-fabric-support.md)hakkında bilgi edinin.

[Image]: media/service-fabric-azure-clusters-overview/Cluster.PNG
