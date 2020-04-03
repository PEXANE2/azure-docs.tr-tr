---
title: Azure hizmetleri için sanal ağ
titlesuffix: Azure Virtual Network
description: Kaynakları sanal ağa dağıtmanın yararları hakkında bilgi edinin. Sanal ağlardaki kaynaklar, Internet'te trafik geçmeden birbirleriyle ve şirket içi kaynaklarla iletişim kurabilir.
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
ms.openlocfilehash: b721857f2fa76dcee144521fb34b34ce48b7bd95
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80616917"
---
# <a name="virtual-network-integration-for-azure-services"></a>Azure hizmetleri için sanal ağ tümleştirmesi

Azure hizmetlerini bir Azure sanal ağına entegre etmek, sanal makinelerden hizmete özel erişim veya sanal ağdaki bilgi işlem kaynaklarına özel erişim sağlar.
Azure hizmetlerini sanal ağınızda aşağıdaki seçeneklerle tümleştirebilirsiniz:
- Hizmetin özel örneklerini sanal ağa dağıtma. Hizmetlere daha sonra sanal ağ içinde ve şirket içi ağlardan özel olarak erişilebilir.
- [Özel Bağlantı'yı](../private-link/private-link-overview.md) kullanarak sanal ağınızdan ve şirket içi ağlardan hizmetin belirli bir örneğine özel olarak erişin.

Ayrıca, [hizmet bitiş noktaları](virtual-network-service-endpoints-overview.md)aracılığıyla, hizmete sanal bir ağ genişleterek ortak uç noktaları kullanarak hizmete erişebilirsiniz. Hizmet bitiş noktaları, hizmet kaynaklarının sanal ağa güvenli hale alınmasına olanak tanır.
 
## <a name="deploy-azure-services-into-virtual-networks"></a>Azure hizmetlerini sanal ağlara dağıtma

Özel Azure hizmetlerini sanal [ağda](virtual-networks-overview.md)dağıttığınızda, özel IP adresleri aracılığıyla hizmet kaynaklarıyla özel olarak iletişim kurabilirsiniz.

![Sanal ağda dağıtılan hizmetler](./media/virtual-network-for-azure-services/deploy-service-into-vnet.png)

Hizmetleri sanal ağ içinde dağıtmak aşağıdaki özellikleri sağlar:

- Sanal ağ içindeki kaynaklar, özel IP adresleri aracılığıyla birbirleriyle özel olarak iletişim kurabilir. Örneğin, sanal ağda sanal bir makinede çalışan HDInsight ve SQL Server arasında doğrudan veri aktarımı.
- Şirket içi kaynaklar, [Siteden Siteye VPN (VPN Ağ Geçidi)](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti) veya [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)üzerinden özel IP adreslerini kullanarak sanal ağdaki kaynaklara erişebilir.
- Sanal ağlar, sanal ağlardaki kaynakların özel IP adreslerini kullanarak birbirleriyle iletişim kurmasını sağlamak için [eşlenebilir.](virtual-network-peering-overview.md)
- Sanal ağdaki hizmet örnekleri genellikle Azure hizmeti tarafından tamamen yönetilir. Bu, kaynakların durumunu izlemeyi ve yük ile ölçeklemeyi içerir.
- Hizmet örnekleri sanal ağdaki bir alt ağa dağıtılır. Alt ağ için gelen ve giden ağ erişimi, hizmet tarafından sağlanan kılavuza göre [ağ güvenlik grupları](security-overview.md#network-security-groups)aracılığıyla açılmalıdır.
- Bazı hizmetler, dağıtıldığı alt ağa kısıtlamalar getirerek, politikaların, yolların uygulanmasını sınırlandırabilir veya Aynı alt ağdaki VM'ler ve hizmet kaynaklarını birleştirir. Zaman içinde değişebileceğinden, her hizmete belirli kısıtlamalar üzerinde check-in edin. Bu tür hizmetlere örnek olarak Azure NetApp Dosyaları, Özel HSM, Azure Kapsayıcı Örnekleri, Uygulama Hizmeti verilebilir. 
- İsteğe bağlı olarak, hizmetler, bir alt ağ belirli bir hizmeti barındırabilir açık bir tanımlayıcı olarak [devredilen](virtual-network-manage-subnet.md#add-a-subnet) bir alt ağ gerektirebilir. Atama yla, hizmetler devredilen alt ağda hizmete özgü kaynaklar oluşturmak için açık izinler alır.
- [Temsilci alt ağı olan sanal ağdaki](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/get#get-virtual-network-with-a-delegated-subnet)REST API yanıtı örneğine bakın. Temsilci alt ağ modelini kullanan hizmetlerin kapsamlı bir listesi [Kullanılabilir Delegasyonlar](https://docs.microsoft.com/rest/api/virtualnetwork/availabledelegations/list) API'si aracılığıyla elde edilebilir.

### <a name="services-that-can-be-deployed-into-a-virtual-network"></a>Sanal ağa dağıtılabilecek hizmetler

|Kategori|Hizmet| Adanmış¹ Alt Ağ
|-|-|-|
| İşlem | Sanal makineler: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Sanal makine ölçek kümeleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Bulut Hizmeti](https://msdn.microsoft.com/library/azure/jj156091): Yalnızca sanal ağ (klasik)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)| Hayır <br/> Hayır <br/> Hayır <br/> No²
| Ağ | [Uygulama Ağ Geçidi - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Güvenlik Duvarı](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Ağ Sanal Cihazlar](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) | Evet <br/> Evet <br/> Evet <br/> Hayır
|Veriler|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure SQL Veritabanı Yönetilen Örneği](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)| Evet <br/> Evet <br/> 
|Analiz | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |No² <br/> No² <br/> 
| Kimlik | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |Hayır <br/>
| Kapsayıcılar | [Azure Kubernetes Hizmeti (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Container Instance (ACI)](https://www.aka.ms/acivnet)<br/>Azure Sanal Ağ CNI [eklentili](https://github.com/Azure/acs-engine/tree/master/examples/vnet) [Azure Konteyner Servis Motoru](https://github.com/Azure/acs-engine)<br/>[Azure İşlevleri](../azure-functions/functions-networking-options.md#virtual-network-integration) |No²<br/> Evet <br/><br/> Hayır <br/> Evet
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Web Apps](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[App Service Ortamı](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Evet <br/> Evet <br/> Evet <br/> Evet
| Barındırılan | [Azure Ayrılmış HSM](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Evet <br/> Evet <br/>
| | |

¹ 'Adanmış' yalnızca hizmete özgü kaynakların bu alt ağda dağıtılabildiği ve müşteri VM/VMSS'leri ile birleştirilemeyeceği anlamına gelir <br/> ² Bu hizmetlerin özel bir alt ağda olması en iyi uygulama olarak önerilir, ancak hizmet tarafından dayatılan zorunlu bir gereklilik değildir.
