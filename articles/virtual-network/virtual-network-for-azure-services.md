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
ms.openlocfilehash: 70266a1280b90b4573073d633a918f701f9ee8c2
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878281"
---
# <a name="deploy-dedicated-azure-services-into-virtual-networks"></a>Özel Azure hizmetlerini sanal ağlara dağıtma

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

|Kategori|Hizmet| Özel<sup>1</sup>sup</sup>>1 Subnet
|-|-|-|
| İşlem | Sanal makineler: [Linux](../virtual-machines/linux/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) veya [Windows](../virtual-machines/windows/infrastructure-networking-guidelines.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Sanal makine ölçek setleri](../virtual-machine-scale-sets/virtual-machine-scale-sets-mvss-existing-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Bulut Hizmeti](https://msdn.microsoft.com/library/azure/jj156091): Yalnızca sanal ağ (klasik)<br/> [Azure Batch](../batch/batch-api-basics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-network-vnet-and-firewall-configuration)| Hayır <br/> Hayır <br/> Hayır <br/> No<sup>2</sup>sup>2</sup>
| Ağ | [Uygulama Ağ Geçidi - WAF](../application-gateway/application-gateway-ilb-arm.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Güvenlik Duvarı](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) <br/>[Ağ Sanal Cihazlar](/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn) | Evet <br/> Evet <br/> Evet <br/> Hayır
|Veriler|[RedisCache](../azure-cache-for-redis/cache-how-to-premium-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure SQL Veritabanı Yönetilen Örneği](../sql-database/sql-database-managed-instance-connectivity-architecture.md?toc=%2fazure%2fvirtual-network%2ftoc.json)| Evet <br/> Evet <br/> 
|Analiz | [Azure HDInsight](../hdinsight/hdinsight-extend-hadoop-virtual-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Databricks](../azure-databricks/what-is-azure-databricks.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |No<sup>2</sup>sup>2</su<sup>2</sup>> <br/> Hayır<sup>2</sup> <br/> 
| Kimlik | [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-getting-started-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json) |Hayır <br/>
| Kapsayıcılar | [Azure Kubernetes Hizmeti (AKS)](../aks/concepts-network.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Container Instance (ACI)](https://www.aka.ms/acivnet)<br/>Azure Sanal Ağ CNI [eklentili](https://github.com/Azure/acs-engine/tree/master/examples/vnet) [Azure Konteyner Servis Motoru](https://github.com/Azure/acs-engine)<br/>[Azure İşlevleri](../azure-functions/functions-networking-options.md#virtual-network-integration) |No<sup>2</sup>sup>2</sup><br/> Evet <br/><br/> Hayır <br/> Evet
| Web | [API Management](../api-management/api-management-using-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Web Apps](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[App Service Ortamı](../app-service/web-sites-integrate-with-vnet.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure Logic Apps](../logic-apps/connect-virtual-network-vnet-isolated-environment-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Evet <br/> Evet <br/> Evet <br/> Evet
| Barındırılan | [Azure Ayrılmış HSM](../dedicated-hsm/index.yml?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>[Azure NetApp Files](../azure-netapp-files/azure-netapp-files-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json)<br/>|Evet <br/> Evet <br/>
| | |

<sup>1</sup> 'Adanmış', yalnızca hizmete özgü kaynakların bu alt ağda dağıtılabildiği ve müşteri VM/VMSS'leri ile birleştirilemeyeceği anlamına gelir <br/> 
<sup>2</sup> Bu hizmetlerin özel bir alt ağda olması en iyi uygulama olarak önerilir, ancak hizmet tarafından dayatılan zorunlu bir gereklilik değildir.
