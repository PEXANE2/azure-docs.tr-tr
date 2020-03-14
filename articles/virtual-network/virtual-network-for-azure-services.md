---
title: Azure hizmetleri için sanal ağ
titlesuffix: Azure Virtual Network
description: Bir sanal ağa kaynak dağıtmanın avantajları hakkında bilgi edinin. Sanal ağlardaki kaynaklar, Internet 'Ten geçen trafik olmadan birbirleriyle ve şirket içi kaynaklarla iletişim kurabilir.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: malop
ms.reviewer: kumud
ms.openlocfilehash: 24bcc7e698527cd39958c53b48a0b36404c36bb4
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79279669"
---
# <a name="virtual-network-integration-for-azure-services"></a>Azure hizmetleri için sanal ağ tümleştirmesi

Azure hizmetleri 'ni bir Azure sanal ağı ile tümleştirmek, sanal makinelerden hizmete özel erişim sağlar veya sanal ağdaki işlem kaynaklarını sağlar.
Sanal ağınızdaki Azure hizmetlerini aşağıdaki seçeneklerle tümleştirebilirsiniz:
- Hizmetin adanmış örneklerini bir sanal ağa dağıtma. Daha sonra hizmetlere sanal ağ ve şirket içi ağlardan özel olarak erişilebilir.
- [Özel bağlantıyı](../private-link/private-link-overview.md) kullanarak sanal ağınızdan ve şirket içi ağlardan gelen hizmetin belirli bir örneğine özel olarak erişin.

Hizmet [uç noktaları](virtual-network-service-endpoints-overview.md)aracılığıyla bir sanal ağı hizmete genişleterek genel uç noktaları kullanarak hizmete erişebilirsiniz. Hizmet uç noktaları, hizmet kaynaklarının sanal ağla güvenliğini sağlar.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure hizmetlerini sanal ağlara dağıtma

Adanmış Azure hizmetlerini bir [Sanal ağda](virtual-networks-overview.md)dağıtırken, özel IP adresleri aracılığıyla hizmet kaynaklarıyla özel olarak iletişim kurabilirsiniz.

![Bir sanal ağda dağıtılan hizmetler](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Bir sanal ağ içinde hizmetleri dağıtmak aşağıdaki özellikleri sağlar:

- Sanal ağ içindeki kaynaklar özel IP adresleri aracılığıyla birbirleriyle özel olarak iletişim kurabilir. Örnek olarak, sanal ağdaki HDInsight ve bir sanal makinede çalışan SQL Server arasında doğrudan veri aktarımı.
- Şirket içi kaynaklar, bir [siteden sıteye VPN (VPN Gateway)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) veya [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)üzerinden özel IP adresleri kullanarak bir sanal ağdaki kaynaklara erişebilir.
- Sanal ağlar, sanal ağlardaki kaynakların birbirleriyle iletişim kurmasını ve özel IP adreslerini kullanmasını [sağlamak için eşlenebilir](virtual-network-peering-overview.md) .
- Bir sanal ağdaki hizmet örnekleri genellikle Azure hizmeti tarafından tam olarak yönetilir. Bu, kaynakların sistem durumunu izlemeyi ve yükleme ile ölçeklendirmeyi içerir.
- Hizmet örnekleri, bir sanal ağ içindeki bir alt ağa dağıtılır. Alt ağ için gelen ve giden ağ erişimi, hizmet tarafından sunulan bir kılavuza göre [ağ güvenlik grupları](security-overview.md#network-security-groups)aracılığıyla açılmalıdır.
- Ayrıca, bazı hizmetler dağıtılan alt ağa kısıtlamalar getirerek ilkelerin uygulamasını kısıtlar, VM 'Leri ve hizmet kaynaklarını aynı alt ağ içinde yönlendirir. Belirli kısıtlamaların zaman içinde değiştirebilecekleri her bir hizmete sahip olup olmadığını denetleyin. Bu hizmetlere örnek olarak Azure NetApp Files, adanmış HSM, Azure Container Instances, App Service verilebilir. 
- İsteğe bağlı olarak, hizmetler bir alt ağın belirli bir hizmeti barındırabilecek açık bir tanımlayıcı olarak bir [temsilci alt ağı](virtual-network-manage-subnet.md#add-a-subnet) gerektirebilir. Temsilci seçerek hizmetler, temsilci alt ağında hizmete özel kaynaklar oluşturmak için açık izinler alır.
- [Temsil edilen alt ağa sahip bir sanal ağ](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/get#get-virtual-network-with-a-delegated-subnet)üzerinde REST API yanıtı örneğine bir örnek bakın. Temsil edilen alt ağ modelini kullanan hizmetlerin kapsamlı bir listesi, [kullanılabilir temsilciler](https://docs.microsoft.com/rest/api/virtualnetwork/availabledelegations/list) API 'si aracılığıyla elde edilebilir.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Bir sanal ağa dağıtılabilecek hizmetler

|Kategori|Hizmet| Adanmış ¹ alt ağı
|-|-|-|
| İşlem | Sanal makineler: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Bulut hizmeti](https://msdn.microsoft.com/library/azure/jj156091): yalnızca sanal ağ (klasik)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)| Hayır <br/> Hayır <br/> Hayır <br/> ² yok
| Ağ | [Application Gateway-WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Güvenlik Duvarı](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Ağ sanal cihazları](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) | Yes <br/> Yes <br/> Yes <br/> Hayır
|Veriler|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure SQL Veritabanı Yönetilen Örneği](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)| Yes <br/> Yes <br/> 
|Analiz | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |² yok <br/> ² yok <br/> 
| Kimlik | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |Hayır <br/>
| Kapsayıcılar | [Azure Kubernetes Service (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Container Instance (acı)](https://www.aka.ms/acivnet)<br/>Azure sanal ağ CNı [eklentisiyle](https://github.com/Azure/acs-engine/tree/master/examples/vnet) [Azure Container Service altyapısı](https://github.com/Azure/acs-engine)|² yok<br/> Yes <br/><br/> Hayır
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[App Service Ortamı](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Yes <br/> Yes <br/> Yes
| Ilırken | [Azure ayrılmış HSM](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Yes <br/> Yes <br/>
| | |

¹ ' adanmış ', bu alt ağda yalnızca hizmete özgü kaynakların dağıtılabilecek olduğunu ve müşteri VM/VMSSs ile birleştirilemeyeceği anlamına gelir <br/> ², bu hizmetlerin özel bir alt ağda olması, ancak hizmet tarafından uygulanan zorunlu bir gereksinim olmaması için en iyi uygulama olarak önerilir.
