---
title: SQL yönetilen örnek geçişleri için ağ topolojileri
titleSuffix: Azure Database Migration Service
description: Azure veritabanı geçiş hizmeti 'ni kullanarak Azure SQL veritabanı yönetilen örnek geçişleri için kaynak ve hedef konfigürasyonları öğrenin.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: 9a313ea798519273ce57961544ec5b37c4d9c5ca
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75749260"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti kullanılarak Azure SQL DB yönetilen örnek geçişleri için ağ topolojileri

Bu makalede, Azure veritabanı geçiş hizmeti 'nin şirket içi SQL Server 'dan Azure SQL veritabanı yönetilen örneği 'ne kapsamlı bir geçiş deneyimi sunmak üzere ile birlikte çalışabileceği çeşitli ağ topolojileri açıklanmaktadır.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Karma iş yükleri için yapılandırılmış Azure SQL veritabanı yönetilen örneği 

Azure SQL veritabanı yönetilen örneğiniz şirket içi ağınıza bağlıysa bu topolojiyi kullanın. Bu yaklaşım en basit ağ yönlendirmeyi sağlar ve geçiş sırasında en fazla veri üretilen işi verir.

![Karma Iş yükleri için ağ topolojisi](media/resource-network-topologies/hybrid-workloads.png)

**Gereksinimler**

- Bu senaryoda, Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti örneği aynı Microsoft Azure Sanal Ağ oluşturulur, ancak farklı alt ağlar kullanır.  
- Bu senaryoda kullanılan sanal ağ aynı zamanda [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanılarak şirket içi ağa bağlanır.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL veritabanı yönetilen örneği, şirket içi ağdan yalıtılmış

Ortamınız aşağıdaki senaryolardan birini veya birkaçını gerektiriyorsa, bu ağ topolojisini kullanın:

- Azure SQL veritabanı yönetilen örneği şirket içi bağlantıdan yalıtılmıştır, ancak Azure veritabanı geçiş hizmeti örneğiniz şirket içi ağa bağlanır.
- Rol tabanlı Access Control (RBAC) ilkeleri yerleşiyorsa ve kullanıcıları Azure SQL veritabanı yönetilen örneğini barındıran aynı aboneliğe erişmek üzere sınırlandırdıysanız.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağlar farklı aboneliklerde.

![Şirket içi ağdan yalıtılmış yönetilen örnek için ağ topolojisi](media/resource-network-topologies/mi-isolated-workload.png)

**Gereksinimler**

- Azure veritabanı geçiş hizmeti 'nin bu senaryo için kullandığı sanal ağın Ayrıca (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanılarak şirket içi ağa bağlanması gerekir.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağ arasında [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) ayarlayın.

## <a name="cloud-to-cloud-migrations-shared-virtual-network"></a>Buluttan buluta geçişler: paylaşılan sanal ağ

Kaynak SQL Server bir Azure VM 'de barındırılıyorsa ve Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti ile aynı sanal ağı paylaşıyorsa, bu topolojiyi kullanın.

![Paylaşılan VNet ile buluttan buluta geçişler için ağ topolojisi](media/resource-network-topologies/cloud-to-cloud.png)

**Gereksinimler**

- Ek gereksinim yok.

## <a name="cloud-to-cloud-migrations-isolated-virtual-network"></a>Buluttan buluta geçişler: yalıtılmış sanal ağ

Ortamınız aşağıdaki senaryolardan birini veya birkaçını gerektiriyorsa, bu ağ topolojisini kullanın:

- Azure SQL veritabanı yönetilen örneği yalıtılmış bir sanal ağda sağlanır.
- Rol tabanlı Access Control (RBAC) ilkeleri yerleşiyorsa ve kullanıcıları Azure SQL veritabanı yönetilen örneğini barındıran aynı aboneliğe erişmek üzere sınırlandırdıysanız.
- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağlar farklı aboneliklerde.

![Yalıtılmış VNet ile buluttan buluta geçişler için ağ topolojisi](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Gereksinimler**

- Azure SQL veritabanı yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağ arasında [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) ayarlayın.

## <a name="inbound-security-rules"></a>Gelen güvenlik kuralları

| **ADA**   | **BAĞ** | **PROTOCOL** | **KAYNAK** | **HEDEFINE** | **ÖN** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Herhangi biri      | Herhangi biri          | DMS ALT AĞı | Herhangi biri             | Allow      |

## <a name="outbound-security-rules"></a>Giden güvenlik kuralları

| **ADA**                  | **BAĞ**                                              | **PROTOCOL** | **KAYNAK** | **HEDEFINE**           | **ÖN** | **Kural nedeni**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| yönetim                | 443, 9354                                              | TCP          | Herhangi biri        | Herhangi biri                       | Allow      | Service Bus ve Azure Blob depolama aracılığıyla yönetim düzlemi iletişimi. <br/>(Microsoft eşlemesi etkinse, bu kurala ihtiyaç duymayabilir.)                                                             |
| Tanılama               | 12000                                                 | TCP          | Herhangi biri        | Herhangi biri                       | Allow      | DMS, sorun giderme amacıyla tanılama bilgilerini toplamak için bu kuralı kullanır.                                                                                                                      |
| SQL kaynak sunucusu         | 1433 (veya SQL Server dinlediği TCP IP bağlantı noktası) | TCP          | Herhangi biri        | Şirket içi adres alanı | Allow      | DMS 'ten kaynak bağlantısı SQL Server <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyaç duymayabilir.)                                                                                       |
| SQL Server adlandırılmış örnek | 1434                                                  | UDP          | Herhangi biri        | Şirket içi adres alanı | Allow      | DMS 'ten adlandırılmış örnek kaynak bağlantısı SQL Server <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyaç duymayabilir.)                                                                        |
| SMB paylaşma                 | 445                                                   | TCP          | Herhangi biri        | Şirket içi adres alanı | Allow      | Azure SQL veritabanı mı ve Azure VM 'de SQL Server 'a geçiş için veritabanı yedekleme dosyalarını depolamak üzere DMS için SMB ağ paylaşımının <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyaç duymayabilir). |
| DMS_subnet                | Herhangi biri                                                   | Herhangi biri          | Herhangi biri        | DMS_Subnet                | Allow      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Ayrıca bkz.

- [SQL Server Azure SQL veritabanı yönetilen örneği 'ne geçirme](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure veritabanı geçiş hizmeti 'ni kullanmaya yönelik önkoşullara genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure portalını kullanarak bir sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Sonraki adımlar

- Azure veritabanı geçiş hizmeti 'ne genel bakış için [Azure veritabanı geçiş hizmeti nedir?](dms-overview.md)makalesine bakın.
- Azure veritabanı geçiş hizmeti 'nin bölgesel kullanılabilirliği hakkında güncel bilgiler için [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration) sayfasına bakın.
