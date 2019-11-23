---
title: Azure virtual network service endpoints
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
ms.sourcegitcommit: f523c8a8557ade6c4db6be12d7a01e535ff32f32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74378686"
---
# <a name="virtual-network-service-endpoints"></a>Sanal Ağ hizmet uç noktaları

Virtual Network (VNet) service endpoints extend your virtual network private address space. The endpoints also extend the identity of your VNet to the Azure services over a direct connection. Uç noktalar kritik Azure hizmeti kaynaklarınızı sanal ağlarınızla sınırlayarak güvenliğini sağlamanıza imkan verir. Sanal ağınızdan Azure hizmetine giden trafik her zaman Microsoft Azure omurga ağında kalır.

This feature is available for the following Azure services and regions. The *Microsoft.\** resource is in parenthesis. Enable this resource from the subnet side while configuring service endpoints for your service:

**Genel kullanıma sunuldu**

- **[Azure Storage](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json#grant-access-from-a-virtual-network)** (*Microsoft.Storage*): Generally available in all Azure regions.
- **[Azure SQL Database](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.Sql*): Generally available in all Azure regions.
- **[Azure SQL Data Warehouse](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.Sql*): Generally available in all Azure regions.
- **[Azure Database for PostgreSQL server](../postgresql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.Sql*): Generally available in Azure regions where database service is available.
- **[Azure Database for MySQL server](../mysql/howto-manage-vnet-using-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.Sql*): Generally available in Azure regions where database service is available.
- **[Azure Database for MariaDB](https://docs.microsoft.com/azure/mariadb/concepts-data-access-security-vnet)** (*Microsoft.Sql*): Generally available in Azure regions where database service is available.
- **[Azure Cosmos DB](../cosmos-db/vnet-service-endpoint.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.AzureCosmosDB*): Generally available in all Azure regions.
- **[Azure Key Vault](../key-vault/key-vault-overview-vnet-service-endpoints.md)** (*Microsoft.KeyVault*): Generally available in all Azure regions.
- **[Azure Service Bus](../service-bus-messaging/service-bus-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.ServiceBus*): Generally available in all Azure regions.
- **[Azure Event Hubs](../event-hubs/event-hubs-service-endpoints.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.EventHub*): Generally available in all Azure regions.
- **[Azure Data Lake Store Gen 1](../data-lake-store/data-lake-store-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)** (*Microsoft.AzureActiveDirectory*): Generally available in all Azure regions where ADLS Gen1 is available.
- **[Azure App Service](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions)** : Generally available in all Azure regions where App service is available.

**Public Preview**

- **[Azure Container Registry](../container-registry/container-registry-vnet.md)** (*Microsoft.ContainerRegistry*): Preview available in all Azure regions where Azure Container Registry is available.

En güncel bildirimler için [Azure Sanal Ağ güncelleştirmeleri](https://azure.microsoft.com/updates/?product=virtual-network) sayfasını inceleyin.

## <a name="key-benefits"></a>Önemli avantajlar

Hizmet uç noktaları aşağıdaki avantajları sağlar:

- **Improved security for your Azure service resources**: VNet private address spaces can overlap. You can't use overlapping spaces to uniquely identify traffic that originates from your VNet. Service endpoints provide the ability to secure Azure service resources to your virtual network by extending VNet identity to the service. Once you enable service endpoints in your virtual network, you can add a virtual network rule to secure the Azure service resources to your virtual network. The rule addition provides improved security by fully removing public internet access to resources and allowing traffic only from your virtual network.
- **Optimal routing for Azure service traffic from your virtual network**: Today, any routes in your virtual network that force internet traffic to your on-premises and/or virtual appliances also force Azure service traffic to take the same route as the internet traffic. Hizmet uç noktaları Azure trafiği için en uygun rotayı sunar. 

  Uç noktalar her zaman hizmet trafiğini sanal ağınızdan doğrudan Microsoft Azure omurga ağındaki hizmete yönlendirir. Trafiğin Azure omurga ağında tutulması, zorlamalı tünel aracılığıyla hizmet trafiğini etkilemeden, giden İnternet trafiğini sanal ağlarınızdan denetlemeye ve izlemeye devam etmenize olanak sağlar. For more information about user-defined routes and forced-tunneling, see [Azure virtual network traffic routing](virtual-networks-udr-overview.md).
- **Kolay kurulum sayesinde daha az yönetim yükü**: Azure kaynaklarını IP güvenlik duvarı aracılığıyla güvenli hale getirmek için artık sanal ağınızda ayrılmış, ortak IP adresleri olması gerekmez. There are no Network Address Translation (NAT) or gateway devices required to set up the service endpoints. You can configure service endpoints through a simple click on a subnet. There's no additional overhead to maintaining the endpoints.

## <a name="limitations"></a>Sınırlamalar

- Bu özellik yalnızca Azure Resource Manager dağıtım modeli üzerinden dağıtılmış olan sanal ağlarda kullanılabilir.
- Uç noktalar Azure sanal ağlarında yapılandırılmış olan alt ağlarda etkindir. Endpoints can't be used for traffic from your premises to Azure services. For more information, see [Secure Azure service access from on-premises](#secure-azure-services-to-virtual-networks)
- Azure SQL için, hizmet uç noktası yalnızca sanal ağ ile aynı bölgedeki Azure hizmeti trafiği için geçerlidir. For Azure Storage, endpoints also extend to include paired regions where you deploy the virtual network to support Read-Access Geo-Redundant Storage (RA-GRS) and Geo-Redundant Storage (GRS) traffic. For more information, see [Azure paired regions](../best-practices-availability-paired-regions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-paired-regions).
- For Azure Data Lake Storage (ADLS) Gen 1, the VNet Integration capability is only available for virtual networks within the same region. Also note that virtual network integration for ADLS Gen1 uses the virtual network service endpoint security between your virtual network and Azure Active Directory (Azure AD) to generate additional security claims in the access token. Ardından bu talepler sanal ağınız için Data Lake Storage 1. Nesil hesabınızda kimlik doğrulaması gerçekleştirme ve erişim izni verme amacıyla kullanılır. The *Microsoft.AzureActiveDirectory* tag listed under services supporting service endpoints is used only for supporting service endpoints to ADLS Gen 1. Azure AD doesn't support service endpoints natively. For more information about Azure Data Lake Store Gen 1 VNet integration, see [Network security in Azure Data Lake Storage Gen1](../data-lake-store/data-lake-store-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="secure-azure-services-to-virtual-networks"></a>Secure Azure services to virtual networks

- Sanal ağ hizmet uç noktası, Azure hizmetine sanal ağınızın kimliğini sağlar. Once you enable service endpoints in your virtual network, you can add a virtual network rule to secure the Azure service resources to your virtual network.
- Günümüzde, bir sanal ağdan gelen Azure hizmet trafiği, kaynak IP adresleri olarak ortak IP adreslerini kullanır. Hizmet uç noktaları ile, hizmet trafiği sanal ağınızdan Azure hizmetinize erişim sırasında kaynak IP adresleri olarak sanal ağ özel adreslerini kullanır. Bu anahtar IP güvenlik duvarlarında kullanılan ayrılmış ve genel IP adreslerini kullanmadan hizmetlere erişmenizi sağlar.

   >[!NOTE]
   > Hizmet uç noktaları ile alt ağdaki sanal makinelerin kaynak IP adresleri hizmet trafiği için genel IPv4 adresleri yerine özel IPv4 adreslerini kullanmaya başlar. Bu değişikliğin ardından Azure genel IP adreslerini kullanan mevcut Azure hizmeti güvenlik duvarı kuralları çalışmamaya başlar. Hizmet uç noktalarını ayarlamadan önce Azure hizmeti güvenlik duvarı kurallarının bu değişikliğe uygun olduğundan emin olun. Hizmet uç noktalarını yapılandırırken bu alt ağdan gelen hizmet trafiğinde geçici kesintiler de yaşayabilirsiniz. 
 
## <a name="secure-azure-service-access-from-on-premises"></a>Secure Azure service access from on-premises

  By default, Azure service resources secured to virtual networks aren't reachable from on-premises networks. If you want to allow traffic from on-premises, you must also allow public (typically, NAT) IP addresses from your on-premises or ExpressRoute. You can add these IP addresses through the IP firewall configuration for Azure service resources.

  ExpressRoute: If you're using [ExpressRoute](../expressroute/expressroute-introduction.md?toc=%2fazure%2fvirtual-network%2ftoc.json) for public peering or Microsoft peering from your premises, you'll need to identify the NAT IP addresses that you're using. For public peering, each ExpressRoute circuit uses two NAT IP addresses, by default, applied to Azure service traffic when the traffic enters the Microsoft Azure network backbone. For Microsoft peering, the NAT IP addresses are either customer provided or provided by the service provider. To allow access to your service resources, you must allow these public IP addresses in the resource IP firewall setting. To find your public peering ExpressRoute circuit IP addresses, [open a support ticket with ExpressRoute](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) via the Azure portal. For more information about NAT for ExpressRoute public and Microsoft peering, see [ExpressRoute NAT requirements](../expressroute/expressroute-nat.md?toc=%2fazure%2fvirtual-network%2ftoc.json#nat-requirements-for-azure-public-peering).

![Azure hizmetlerini sanal ağlar ile sınırlama](./media/virtual-network-service-endpoints-overview/VNet_Service_Endpoints_Overview.png)

### <a name="configuration"></a>Yapılandırma

- Configure service endpoints on a subnet in a virtual network. Uç noktalar ilgili alt ağ içinde çalışan tüm işlem örneği türleriyle birlikte çalışabilir.
- You can configure multiple service endpoints for all supported Azure services (Azure Storage or Azure SQL Database, for example) on a subnet.
- Azure SQL Veritabanı için, sanal ağların Azure hizmet kaynağıyla aynı bölgede bulunması gerekir. GRS ve RA-GRS Azure Depolama hesapları kullanılıyorsa, birincil hesap sanal ağ ile aynı bölgede olmalıdır. For all other services, you can secure Azure service resources to virtual networks in any region. 
- Uç noktanın yapılandırıldığı sanal ağ, Azure hizmet kaynağıyla aynı veya ondan farklı abonelikte olabilir. Uç noktaları ayarlamak ve Azure hizmetlerinin güvenliğini sağlamak için gerekli olan izinler hakkında daha fazla bilgi için [Sağlama](#provisioning) bölümüne bakın.
- Desteklenen hizmetler için yeni veya mevcut kaynaklar ile sanal ağlar arasındaki güvenliği hizmet uç noktaları kullanarak sağlayabilirsiniz.

### <a name="considerations"></a>Dikkat edilmesi gerekenler

- After enabling a service endpoint, the source IP addresses of virtual machines in the subnet switch. The source IP addresses switch from using public IPv4 addresses to using their private IPv4 address when communicating with the service from that subnet. Hizmete giden mevcut açık TCP bağlantıları bu geçiş sırasında kapatılır. Bir alt ağ için hizmete yönelik hizmet uç noktasını etkinleştirmeden veya devre dışı bırakmadan önce çalışan kritik görev olmadığından emin olun. Ayrıca uygulamalarınızın IP adresi değişikliğinin ardından Azure hizmetlerine otomatik olarak bağlanabildiğinden emin olun.

  IP adresi değişiklikleri yalnızca sanal ağınızdan giden hizmet trafiğini etkiler. There's no impact to any other traffic addressed to or from the public IPv4 addresses assigned to your virtual machines. Azure hizmetleri açısından, Azure genel IP adreslerini kullanan mevcut güvenlik duvarı kurallarınız varsa bu kurallar sanal ağ özel adresine geçiş yapıldığında çalışmaz.
- With service endpoints, DNS entries for Azure services remain as-is today and continue to resolve to public IP addresses assigned to the Azure service.

- Hizmet uç noktasına sahip ağ güvenlik grupları (NSG):
  - By default, NSGs allow outbound internet traffic and also allow traffic from your VNet to Azure services. This traffic continues to work with service endpoints as is. 
  - If you want to deny all outbound internet traffic and allow only traffic to specific Azure services, you can do so using [service tags](security-overview.md#service-tags) in your NSGs. You can specify supported Azure services as destination in your NSG rules and Azure also provides the maintenance of IP addresses underlying each tag. Daha fazla bilgi için bkz. [NSG'ler için Azure Hizmet etiketleri.](security-overview.md#service-tags) 

### <a name="scenarios"></a>Senaryolar

- **Eşlenmiş, bağlı veya birden çok sanal ağ**: Bir sanal ağ içindeki veya birden fazla sanal ağ üzerinde bulunan birden fazla alt ağdaki Azure hizmetlerinin güvenliğini sağlamak için, her bir alt ağdaki hizmet uç noktasını ayrı ayrı etkinleştirebilir ve bu alt ağlara giden Azure hizmet kaynaklarının güvenliğini sağlayabilirsiniz.
- **Filtering outbound traffic from a virtual network to Azure services**: If you want to inspect or filter the traffic sent to an Azure service from a virtual network, you can deploy a network virtual appliance within the virtual network. Ardından hizmet uç noktalarını ağ sanal gerecinin dağıtılmış olduğu alt ağa uygulayabilir ve Azure hizmet kaynağını yalnızca bu alt ağ ile sınırlayabilirsiniz. This scenario might be helpful if you want use network virtual appliance filtering to restrict Azure service access from your virtual network only to specific Azure resources. Daha fazla bilgi için bkz. [Ağ sanal gereçleri ile çıkış](/azure/architecture/reference-architectures/dmz/nva-ha).
- **Securing Azure resources to services deployed directly into virtual networks**: You can directly deploy various Azure services into specific subnets in a virtual network. Yönetilen hizmet alt ağında bir hizmet uç noktası kurarak Azure hizmet kaynaklarını [yönetilen hizmet](virtual-network-for-azure-services.md) alt ağlarına ayırabilirsiniz.
- **Disk traffic from an Azure virtual machine**: Virtual Machine Disk traffic for managed and unmanaged disks isn't affected by service endpoints routing changes for Azure Storage. This traffic includes diskIO as well as mount and unmount. You can limit REST access to page blobs to select networks through service endpoints and [Azure Storage network rules](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

### <a name="logging-and-troubleshooting"></a>Günlüğe kaydetme ve sorun giderme

Once you configure service endpoints to a specific service, validate that the service endpoint route is in effect by: 
 
- Hizmet tanılamada herhangi bir hizmet isteğinin kaynak IP adresini doğrulama. Hizmet uç noktalarına sahip tüm yeni isteklerin kaynak IP adresi değerinde isteği sanal ağınızdan gönderen istemciye atanmış olan sanal ağ özel IP adresi gösterilir. Uç noktası olmadığında bu adres Azure genel IP adreslerinden biri olur.
- Bir alt ağdaki herhangi bir ağ arabiriminin etkin yollarını görüntüleme. Hizmet yolu:
  - Her hizmetin adres ön eki aralıklarına daha belirli bir varsayılan yolu gösterir
  - nextHopType değeri *VirtualNetworkServiceEndpoint* olarak belirlenmiştir
  - Indicates that a more direct connection to the service is in effect compared to any forced-tunneling routes

>[!NOTE]
> Hizmet uç noktası rotaları, Azure hizmeti olarak adres ön eki eşleşmesi için BGP veya UDR rotasını geçersiz kılar. For more information, see [troubleshooting with effective routes](diagnose-network-routing-problem.md).

## <a name="provisioning"></a>Sağlama

Service endpoints can be configured on virtual networks independently by a user with write access to a virtual network. To secure Azure service resources to a VNet, the user must have permission to *Microsoft.Network/virtualNetworks/subnets/joinViaServiceEndpoint/action* for the added subnets. The built-in service administrator roles include this permission by default. You can modify the permission by creating custom roles.

For more information about built-in roles, see [Built-in roles for Azure resources](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json). For more information about assigning specific permissions to custom roles, see [Custom roles for Azure resources](../role-based-access-control/custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

Sanal ağlar ve Azure hizmet kaynakları aynı ağda veya farklı aboneliklerde olabilir. Sanal ağ ve Azure hizmet kaynaklarının farklı aboneliklerde olması halinde kaynakların aynı Active Directory (AD) kiracısı altında bulunması gerekir. 

## <a name="pricing-and-limits"></a>Fiyatlandırma ve limitler

There's no additional charge for using service endpoints. The current pricing model for Azure services (Azure Storage, Azure SQL Database, etc.) applies as-is today.

There's no limit on the total number of service endpoints in a virtual network.

Certain Azure services, such as Azure Storage Accounts, may enforce limits on the number of subnets used for securing the resource. Refer to the documentation for various services in the [Next steps](#next-steps) section for details.

## <a name="vnet-service-endpoint-policies"></a>VNet service endpoint policies 

VNet service endpoint policies allow you to filter virtual network traffic to Azure services. This filter allows only specific Azure service resources over service endpoints. Service endpoint policies provide granular access control for virtual network traffic to Azure services. For more information, see [Virtual Network Service Endpoint Policies](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview).

## <a name="faqs"></a>SSS

For FAQs, see [Virtual Network Service Endpoint FAQs](https://docs.microsoft.com/azure/virtual-network/virtual-networks-faq#virtual-network-service-endpoints).

## <a name="next-steps"></a>Sonraki adımlar

- [Configure virtual network service endpoints](tutorial-restrict-network-access-to-resources.md)
- [Secure an Azure Storage account to a virtual network](../storage/common/storage-network-security.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Secure an Azure SQL Database to a virtual network](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Secure an Azure SQL Data Warehouse to a virtual network](../sql-database/sql-database-vnet-service-endpoint-rule-overview.md?toc=%2fazure%2fsql-data-warehouse%2ftoc.json)
- [Azure service integration in virtual networks](virtual-network-for-azure-services.md)
- [Virtual Network Service Endpoint Policies](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoint-policies-overview)
- [Azure Resource Manager şablonu](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration)

