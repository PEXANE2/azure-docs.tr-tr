---
title: Azure sanal ağ hizmeti bitiş noktaları
titlesuffix: Azure Virtual Network
description: Hizmet uç noktalarını kullanarak bir sanal ağdan Azure kaynaklarına doğrudan erişimi etkinleştirmeyi öğrenin.
services: virtual-network
documentationcenter: na
author: sumeetmittal
ms.service: virtual-network
ms.devlang: NA
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/08/2019
ms.author: sumi
ms.custom: ''
ms.openlocfilehash: 72c2c90f7a71bd9bf251adb492168fa5d2baa60a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79244790"
---
# <a name="virtual-network-service-endpoints"></a>Sanal Ağ hizmet uç noktaları

Sanal Ağ (VNet) hizmet bitiş noktaları sanal ağ özel adres alanı nızı genişletir. Uç noktalar, doğrudan bağlantı üzerinden VNet'inizin kimliğini Azure hizmetlerine de genişletir. Uç noktalar kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla sınırlayarak güvenliğini sağlamanıza imkan verir. Sanal ağınızdan Azure hizmetine giden trafik her zaman Microsoft Azure omurga ağında kalır.

Bu özellik, aşağıdaki Azure hizmetleri ve bölgeleri için kullanılabilir. *Microsoft.\* * kaynak parantez içindedir. Hizmetinizin hizmet uç noktalarını yapılandırırken bu kaynağı alt ağ tarafından etkinleştirin:

**Genel kullanıma sunuldu**

- **[Azure Depolama](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network)** (*Microsoft.Storage*): Genellikle tüm Azure bölgelerinde kullanılabilir.
- **[Azure SQL Veritabanı](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.Sql*): Genellikle tüm Azure bölgelerinde kullanılabilir.
- **[Azure SQL Veri Ambarı](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.Sql*): Genellikle tüm Azure bölgelerinde kullanılabilir.
- **[PostgreSQL sunucusu için Azure Veritabanı](../postgresql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.Sql*): Genellikle veritabanı hizmetinin kullanılabildiği Azure bölgelerinde kullanılabilir.
- **[MySQL sunucusu için Azure Veritabanı](../mysql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.Sql*): Genellikle veritabanı hizmetinin kullanılabildiği Azure bölgelerinde kullanılabilir.
- **[MariaDB için Azure Veritabanı](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-vnet)** (*Microsoft.Sql*): Genellikle veritabanı hizmetinin kullanılabildiği Azure bölgelerinde kullanılabilir.
- **[Azure Cosmos DB](../cosmos-db/vnet-service-endpoint.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.AzureCosmosDB*): Genellikle tüm Azure bölgelerinde kullanılabilir.
- **[Azure Key Vault](../key-vault/key-vault-overview-vnet-service-endpoints.md)** (*Microsoft.KeyVault*): Genellikle tüm Azure bölgelerinde kullanılabilir.
- **[Azure Hizmet Veri Servisi](../service-bus-messaging/service-bus-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.ServiceBus*): Genellikle tüm Azure bölgelerinde kullanılabilir.
- **[Azure Etkinlik Hub'ları](../event-hubs/event-hubs-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.EventHub*): Genellikle tüm Azure bölgelerinde kullanılabilir.
- **[Azure Veri Gölü Deposu Gen 1](../data-lake-store/data-lake-store-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.AzureActiveDirectory*): Genellikle ADLS Gen1'in kullanılabildiği tüm Azure bölgelerinde kullanılabilir.
- **[Azure Uygulama Hizmeti](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)**: Genellikle Uygulama hizmetinin kullanılabildiği tüm Azure bölgelerinde kullanılabilir.

**Genel Önizleme**

- **[Azure Kapsayıcı Kayıt Defteri](../container-registry/container-registry-vnet.md)** (*Microsoft.Container Registry*): Azure Kapsayıcı Kayıt Defteri'nin kullanılabildiği tüm Azure bölgelerinde kullanılabilir önizleme.

En güncel bildirimler için [Azure Sanal Ağ güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasını inceleyin.

## <a name="key-benefits"></a>Önemli avantajlar

Hizmet uç noktaları aşağıdaki avantajları sağlar:

- **Azure hizmet kaynaklarınız için geliştirilmiş güvenlik:** VNet özel adres alanları çakışabilir. VNet'inizden kaynaklanan trafiği benzersiz olarak tanımlamak için çakışan alanları kullanamazsınız. Hizmet bitiş noktaları, VNet kimliğini hizmete genişleterek Azure hizmet kaynaklarını sanal ağınıza güvenli hale getirmek için idealdir. Sanal ağınızdaki hizmet uç noktalarını etkinleştirdikten sonra, Sanal ağınıza Azure hizmet kaynaklarını güvence altına almak için sanal ağ kuralı ekleyebilirsiniz. Kural ekinde, genel internet kaynakları erişimini tamamen kaldırarak ve yalnızca sanal aağınızdan trafiğe izin vererek daha iyi güvenlik sağlar.
- **Sanal ağınızdan Azure servis trafiği için en uygun yönlendirme**: Bugün, sanal ağınızdaki internet trafiğini şirket içi ve/veya sanal cihazlarınız için zorlayan tüm rotalar, Azure hizmet trafiğini de Internet trafiğiyle aynı rotayı almaya zorlar. Hizmet uç noktaları Azure trafiği için en uygun rotayı sunar. 

  Uç noktalar her zaman hizmet trafiğini sanal ağınızdan doğrudan Microsoft Azure omurga ağındaki hizmete yönlendirir. Trafiğin Azure omurga ağında tutulması, zorlamalı tünel aracılığıyla hizmet trafiğini etkilemeden, giden İnternet trafiğini sanal ağlarınızdan denetlemeye ve izlemeye devam etmenize olanak sağlar. Kullanıcı tanımlı rotalar ve zorunlu tünelleme hakkında daha fazla bilgi için [Azure sanal ağ trafiğini yönlendirme](virtual-networks-udr-overview.md)bölümüne bakın.
- **Kolay kurulum sayesinde daha az yönetim yükü**: Azure kaynaklarını IP güvenlik duvarı aracılığıyla güvenli hale getirmek için artık sanal ağınızda ayrılmış, ortak IP adresleri olması gerekmez. Hizmet bitiş noktalarını ayarlamak için Ağ Adresi Çevirisi (NAT) veya ağ geçidi aygıtı gerekmez. Hizmet uç noktalarını bir alt ağa basit bir tıklamayla yapılandırabilirsiniz. Uç noktaları korumak için ek bir ek yükü yoktur.

## <a name="limitations"></a>Sınırlamalar

- Bu özellik yalnızca Azure Resource Manager dağıtım modeli üzerinden dağıtılmış olan sanal ağlarda kullanılabilir.
- Uç noktalar Azure sanal ağlarında yapılandırılmış olan alt ağlarda etkindir. Bitiş noktaları, binanızdan Azure hizmetlerine olan trafik için kullanılamaz. Daha fazla bilgi için [şirket için güvenli Azure hizmeti erişimine](#secure-azure-services-to-virtual-networks) bakın
- Azure SQL için, hizmet uç noktası yalnızca sanal ağ ile aynı bölgedeki Azure hizmeti trafiği için geçerlidir. Azure Depolama için uç noktalar, Oku-Erişim Coğrafi Yedekli Depolama (RA-GRS) ve Coğrafi Yedekli Depolama (GRS) trafiğini desteklemek için sanal ağı dağıttığınız eşleştirilmiş bölgeleri de kapsayacak şekilde genişletilir. Daha fazla bilgi için Azure [eşleştirilmiş bölgelere](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions)bakın.
- Azure Veri Gölü Depolama (ADLS) Gen 1 için VNet Tümleştirme özelliği yalnızca aynı bölgedeki sanal ağlar için kullanılabilir. ADLS Gen1 için sanal ağ tümleştirmesinin, erişim belirtecinde ek güvenlik talepleri oluşturmak için sanal ağınız ile Azure Etkin Dizin (Azure AD) arasındaki sanal ağ hizmeti uç noktası güvenliğini kullandığını da unutmayın. Ardından bu talepler sanal ağınız için Data Lake Storage 1. Nesil hesabınızda kimlik doğrulaması gerçekleştirme ve erişim izni verme amacıyla kullanılır. Hizmetleri destekleyen hizmet bitiş noktaları altında listelenen *Microsoft.AzureActiveDirectory* etiketi yalnızca ADLS Gen 1'in hizmet uç noktalarını desteklemek için kullanılır. Azure AD, hizmet bitiş noktalarını yerel olarak desteklemez. Azure Veri Gölü Deposu Gen 1 VNet entegrasyonu hakkında daha fazla bilgi için [Azure Veri Gölü Depolama Gen1'deki Ağ güvenliğine](../data-lake-store/data-lake-store-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın.

## <a name="secure-azure-services-to-virtual-networks"></a>Sanal ağlara Azure hizmetlerini güvenli hale

- Sanal ağ hizmet uç noktası, Azure hizmetine sanal ağınızın kimliğini sağlar. Sanal ağınızdaki hizmet uç noktalarını etkinleştirdikten sonra, Sanal ağınıza Azure hizmet kaynaklarını güvence altına almak için sanal ağ kuralı ekleyebilirsiniz.
- Günümüzde, bir sanal ağdan gelen Azure hizmet trafiği, kaynak IP adresleri olarak ortak IP adreslerini kullanır. Hizmet uç noktaları ile, hizmet trafiği sanal ağınızdan Azure hizmetinize erişim sırasında kaynak IP adresleri olarak sanal ağ özel adreslerini kullanır. Bu anahtar IP güvenlik duvarlarında kullanılan ayrılmış ve genel IP adreslerini kullanmadan hizmetlere erişmenizi sağlar.

   >[!NOTE]
   > Hizmet uç noktaları ile alt ağdaki sanal makinelerin kaynak IP adresleri hizmet trafiği için genel IPv4 adresleri yerine özel IPv4 adreslerini kullanmaya başlar. Bu değişikliğin ardından Azure genel IP adreslerini kullanan mevcut Azure hizmeti güvenlik duvarı kuralları çalışmamaya başlar. Hizmet uç noktalarını ayarlamadan önce Azure hizmeti güvenlik duvarı kurallarının bu değişikliğe uygun olduğundan emin olun. Hizmet uç noktalarını yapılandırırken bu alt ağdan gelen hizmet trafiğinde geçici kesintiler de yaşayabilirsiniz. 
 
## <a name="secure-azure-service-access-from-on-premises"></a>Şirket içinden Azure hizmet erişimini güvenli hale

  Varsayılan olarak, sanal ağlara güvenli Azure hizmet kaynaklarına şirket içi ağlardan erişilemez. Şirket içi trafiğe izin vermek istiyorsanız, şirket içi veya ExpressRoute'unuzdan herkese açık (genellikle NAT) IP adreslerine de izin vermelisiniz. Bu IP adreslerini Azure hizmet kaynakları için IP güvenlik duvarı yapılandırması aracılığıyla ekleyebilirsiniz.

  ExpressRoute: [ExpressRoute'u](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) genel olarak bakmak için veya binalarınızdan Microsoft'a bakmak için kullanıyorsanız, kullanmakta olduğunuz NAT IP adreslerini tanımlamanız gerekir. Genel bakış için her ExpressRoute devresi, trafik Microsoft Azure ağ omurgasına girdiğinde Varsayılan olarak Azure hizmet trafiğine uygulanan iki NAT IP adresi kullanır. Microsoft'un bakışları için NAT IP adresleri, hizmet sağlayıcısı tarafından sağlanan veya sağlanan müşteridir.Hizmet kaynaklarınıza erişime izin vermek için, bu genel IP adreslerine kaynak IP güvenlik duvarı ayarında izin vermeniz gerekir.Ortak eşleme ExpressRoute bağlantı hattı IP adreslerinizi bulmak için Azure portalında [ExpressRoute ile bir destek bileti açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview). ExpressRoute public ve Microsoft'un bakışları için NAT hakkında daha fazla bilgi için [ExpressRoute NAT gereksinimlerine](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering)bakın.

![Azure hizmetlerini sanal ağlar ile sınırlama](./media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Yapılandırma

- Sanal ağdaki bir alt ağdaki hizmet uç noktalarını yapılandırın. Uç noktalar ilgili alt ağ içinde çalışan tüm işlem örneği türleriyle birlikte çalışabilir.
- Desteklenen tüm Azure hizmetleri (örneğin Azure Depolama veya Azure SQL Veritabanı) için birden çok hizmet uç noktasını bir alt ağda yapılandırabilirsiniz.
- Azure SQL Veritabanı için, sanal ağların Azure hizmet kaynağıyla aynı bölgede bulunması gerekir. GRS ve RA-GRS Azure Depolama hesapları kullanılıyorsa, birincil hesap sanal ağ ile aynı bölgede olmalıdır. Diğer tüm hizmetlerde Azure hizmet kaynaklarını herhangi bir bölgedeki sanal ağlara güvenebilirsiniz. 
- Uç noktanın yapılandırıldığı sanal ağ, Azure hizmet kaynağıyla aynı veya ondan farklı abonelikte olabilir. Uç noktaları ayarlamak ve Azure hizmetlerinin güvenliğini sağlamak için gerekli olan izinler hakkında daha fazla bilgi için [Sağlama](#provisioning) bölümüne bakın.
- Desteklenen hizmetler için yeni veya mevcut kaynaklar ile sanal ağlar arasındaki güvenliği hizmet uç noktaları kullanarak sağlayabilirsiniz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

- Bir hizmet bitiş noktasını etkinleştirdikten sonra, alt ağ anahtarındaki sanal makinelerin kaynak IP adresleri. Kaynak IP adresleri, genel IPv4 adreslerini kullanmaktan, hizmetle bu alt ağdan iletişim kurarken özel IPv4 adreslerini kullanmaya geçiş eder. Hizmete giden mevcut açık TCP bağlantıları bu geçiş sırasında kapatılır. Bir alt ağ için hizmete yönelik hizmet uç noktasını etkinleştirmeden veya devre dışı bırakmadan önce çalışan kritik görev olmadığından emin olun. Ayrıca uygulamalarınızın IP adresi değişikliğinin ardından Azure hizmetlerine otomatik olarak bağlanabildiğinden emin olun.

  IP adresi değişiklikleri yalnızca sanal ağınızdan giden hizmet trafiğini etkiler. Sanal makinelerinize atanan genel IPv4 adreslerine veya adresinden gönderilen diğer trafiklere herhangi bir etki yoktur. Azure hizmetleri açısından, Azure genel IP adreslerini kullanan mevcut güvenlik duvarı kurallarınız varsa bu kurallar sanal ağ özel adresine geçiş yapıldığında çalışmaz.
- Hizmet bitiş noktalarıyla, Azure hizmetleri için DNS girişleri bugün olduğu gibi kalır ve Azure hizmetine atanan genel IP adreslerine çözümmeye devam eder.

- Hizmet uç noktasına sahip ağ güvenlik grupları (NSG):
  - Varsayılan olarak, NSG'ler giden internet trafiğine ve VNet'inizden Azure hizmetlerine giden trafiğe izin verir. Bu trafik olduğu gibi hizmet bitiş noktaları ile çalışmaya devam ediyor. 
  - Tüm giden Internet trafiğini reddetmek ve yalnızca belirli Azure hizmetlerine giden trafiğe izin vermek istiyorsanız, bunu NSG'lerinizdeki [hizmet etiketlerini](security-overview.md#service-tags) kullanarak yapabilirsiniz. Desteklenen Azure hizmetlerini NSG kurallarınızda hedef olarak belirtebilirsiniz ve Azure da her etiketin altında yatan IP adreslerinin bakımını sağlar. Daha fazla bilgi için bkz. [NSG'ler için Azure Hizmet etiketleri.](security-overview.md#service-tags) 

### <a name="scenarios"></a>Senaryolar

- **Eşlenmiş, bağlı veya birden çok sanal ağ**: Bir sanal ağ içindeki veya birden fazla sanal ağ üzerinde bulunan birden fazla alt ağdaki Azure hizmetlerinin güvenliğini sağlamak için, her bir alt ağdaki hizmet uç noktasını ayrı ayrı etkinleştirebilir ve bu alt ağlara giden Azure hizmet kaynaklarının güvenliğini sağlayabilirsiniz.
- **Giden trafiği sanal ağdan Azure hizmetlerine filtreleme**: Bir Sanal ağdan Bir Azure hizmetine gönderilen trafiği incelemek veya filtrelemek istiyorsanız, sanal ağ içinde bir ağ sanal cihazı dağıtabilirsiniz. Ardından hizmet uç noktalarını ağ sanal gerecinin dağıtılmış olduğu alt ağa uygulayabilir ve Azure hizmet kaynağını yalnızca bu alt ağ ile sınırlayabilirsiniz. Bu senaryo, sanal ağınızdan Azure hizmet erişimini yalnızca belirli Azure kaynaklarına kısıtlamak için ağ sanal cihaz filtrelemesini kullanmak istiyorsanız yararlı olabilir. Daha fazla bilgi için bkz. [Ağ sanal gereçleri ile çıkış](/azure/architecture/reference-architectures/dmz/nva-ha).
- **Azure kaynaklarını doğrudan sanal ağlara dağıtılan hizmetlere securing**: Çeşitli Azure hizmetlerini sanal ağdaki belirli alt ağlara doğrudan dağıtabilirsiniz. Yönetilen hizmet alt ağında bir hizmet uç noktası kurarak Azure hizmet kaynaklarını [yönetilen hizmet](virtual-network-for-azure-services.md) alt ağlarına ayırabilirsiniz.
- **Azure sanal makinesinden gelen disk trafiği**: Yönetilen ve yönetilmeyen diskler için Sanal Makine Disk trafiği, Azure Depolama için hizmet uç noktaları yönlendirme değişikliklerinden etkilenmez. Bu trafik diskIO yanı sıra montaj ve unmount içerir. Hizmet bitiş noktaları ve [Azure Depolama ağ kuralları](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)üzerinden ağları seçmek için SAYFA bloblarına REST erişimini sınırlandırabilirsiniz. 

### <a name="logging-and-troubleshooting"></a>Günlüğe kaydetme ve sorun giderme

Hizmet uç noktalarını belirli bir hizmete yapılandırdıktan sonra, hizmet bitiş noktası rotasının geçerli olduğunu doğrulayın: 
 
- Hizmet tanılamada herhangi bir hizmet isteğinin kaynak IP adresini doğrulama. Hizmet uç noktalarına sahip tüm yeni isteklerin kaynak IP adresi değerinde isteği sanal ağınızdan gönderen istemciye atanmış olan sanal ağ özel IP adresi gösterilir. Uç noktası olmadığında bu adres Azure genel IP adreslerinden biri olur.
- Bir alt ağdaki herhangi bir ağ arabiriminin etkin yollarını görüntüleme. Hizmet yolu:
  - Her hizmetin adres ön eki aralıklarına daha belirli bir varsayılan yolu gösterir
  - nextHopType değeri *VirtualNetworkServiceEndpoint* olarak belirlenmiştir
  - Herhangi bir zorunlu tünelleme rotasıile karşılaştırıldığında hizmete daha doğrudan bağlantının etkin olduğunu gösterir

>[!NOTE]
> Hizmet uç noktası rotaları, Azure hizmeti olarak adres ön eki eşleşmesi için BGP veya UDR rotasını geçersiz kılar. Daha fazla bilgi [için, etkili rotalarla sorun giderme olayına](diagnose-network-routing-problem.md)bakın.

## <a name="provisioning"></a>Sağlama

Hizmet bitiş noktaları, sanal ağa yazma erişimi olan bir kullanıcı tarafından sanal ağlarda bağımsız olarak yapılandırılabilir. Azure hizmet kaynaklarını bir VNet'e sabitlemek için, kullanıcının eklenen alt ağlar için *Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action* izni ne kadar olmalıdır. Yerleşik hizmet yöneticisi rolleri varsayılan olarak bu izni içerir. Özel roller oluşturarak izni değiştirebilirsiniz.

Yerleşik roller hakkında daha fazla bilgi için Azure [kaynakları için Yerleşik rollere](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json)bakın. Özel rollere belirli izinler atama hakkında daha fazla bilgi için [Azure kaynakları için Özel rolleri](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json)görün.

Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. Sanal ağ ve Azure hizmet kaynaklarının farklı aboneliklerde olması halinde kaynakların aynı Active Directory (AD) kiracısı altında bulunması gerekir. 

## <a name="pricing-and-limits"></a>Fiyatlandırma ve limitler

Hizmet bitiş noktalarını kullanmak için ek ücret alınmaz. Azure hizmetleri için geçerli fiyatlandırma modeli (Azure Depolama, Azure SQL Veritabanı vb.) bugün olduğu gibi geçerlidir.

Sanal ağdaki toplam hizmet bitiş noktası sayısında bir sınır yoktur.

Azure Depolama Hesapları gibi belirli Azure hizmetleri, kaynağın güvenliğini sağlamak için kullanılan alt ağ sayısında sınırlamalar getirebilir. Ayrıntılar için [Sonraki adımlar](#next-steps) bölümündeçeşitli hizmetler için belgelere bakın.

## <a name="vnet-service-endpoint-policies"></a>VNet hizmeti bitiş noktası ilkeleri 

VNet hizmet bitiş noktası ilkeleri, sanal ağ trafiğini Azure hizmetlerine filtrelemenize olanak tanır. Bu filtre, hizmet bitiş noktaları üzerinden yalnızca belirli Azure hizmet kaynaklarına izin verir. Hizmet bitiş noktası ilkeleri, Azure hizmetlerine sanal ağ trafiği için parçalı erişim denetimi sağlar. Daha fazla bilgi için [Sanal Ağ Hizmeti Bitiş Noktası İlkeleri'ne](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview)bakın.

## <a name="faqs"></a>SSS

SSS'ler için [Sanal Ağ Hizmeti Bitiş Noktası SSS'lerine](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#virtual-network-service-endpoints)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Sanal ağ hizmeti uç noktalarını yapılandırma](tutorial-restrict-network-access-to-resources.md)
- [Azure Depolama hesabını sanal ağa sabitleme](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure SQL Veritabanını sanal ağa sabitleme](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure SQL Veri Ambarı'nı sanal ağa sabitleme](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [Sanal ağlarda Azure hizmet entegrasyonu](virtual-network-for-azure-services.md)
- [Sanal Ağ Hizmeti Bitiş Noktası İlkeleri](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview)
- [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration)

