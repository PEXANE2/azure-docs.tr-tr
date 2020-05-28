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
ms.openlocfilehash: 5b9d18d47c1430023dc9d892ce25389810690a4d
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84021645"
---
# <a name="deploy-dedicated-azure-services-into-virtual-networks"></a>Adanmış Azure hizmetlerini sanal ağlara dağıtma

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

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Sanal ağa dağıtılabilecek hizmetler

|Kategori|Hizmet| Adanmış<sup>1</sup>SUP>1 </sup> alt ağı
|-|-|-|
| İşlem | Sanal makineler: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Sanal Makine Ölçek Kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Bulut hizmeti](https://msdn.microsoft.com/library/azure/jj156091): yalnızca sanal ağ (klasik)<br/> [Azure Batch](../batch/nodes-and-pools.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)| Hayır <br/> Hayır <br/> Hayır <br/> 2<sup>sup></sup>2</sup>
| Ağ | [Application Gateway-WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Güvenlik Duvarı](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Ağ sanal cihazları](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) | Yes <br/> Yes <br/> Yes <br/> No
|Veriler|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure SQL Veritabanı Yönetilen Örneği](../azure-sql/managed-instance/connectivity-architecture-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)| Yes <br/> Yes <br/> 
|Analiz | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |<sup>2</sup>SUP>2</su<sup>2</sup>> <br/> <sup>2</sup> yok <br/> 
| Kimlik | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |No <br/>
| Kapsayıcılar | [Azure Kubernetes Hizmeti (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Container Instance (ACI)](https://www.aka.ms/acivnet)<br/>Azure sanal ağ CNı [eklentisiyle](https://github.com/Azure/acs-engine/tree/master/examples/vnet) [Azure Container Service altyapısı](https://github.com/Azure/acs-engine)<br/>[Azure İşlevleri](../azure-functions/functions-networking-options.md#virtual-network-integration) |2<sup>sup></sup>2</sup><br/> Yes <br/><br/> No <br/> Yes
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Web Apps](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[App Service Ortamı](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Yes <br/> Yes <br/> Yes <br/> Yes
| Barındırılan | [Azure Ayrılmış HSM](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Yes <br/> Yes <br/>
| | |

<sup>1</sup> ' adanmış ', bu alt ağda yalnızca hizmete özgü kaynakların dağıtılabilmesi ve müşteri VM/VMSSs ile birleştirilemez <br/> 
<sup>2</sup> özel bir alt ağda bu hizmetlerin olması için en iyi uygulama olarak kullanılması önerilir, ancak hizmet tarafından uygulanan zorunlu bir gereksinim değildir.
