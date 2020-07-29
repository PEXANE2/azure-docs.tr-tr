---
title: SQL yönetilen örnek geçişleri için ağ topolojileri
titleSuffix: Azure Database Migration Service
description: Azure veritabanı geçiş hizmeti 'ni kullanarak Azure SQL yönetilen örnek geçişleri için kaynak ve hedef konfigürasyonları öğrenin.
services: database-migration
author: pochiraju
ms.author: rajpo
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: seo-lt-2019
ms.topic: article
ms.date: 01/08/2020
ms.openlocfilehash: 31dfae60b1967e221e294195f66bb7fe59a15e64
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84187517"
---
# <a name="network-topologies-for-azure-sql-managed-instance-migrations-using-azure-database-migration-service"></a>Azure veritabanı geçiş hizmeti kullanılarak Azure SQL yönetilen örnek geçişleri için ağ topolojileri

Bu makalede, Azure veritabanı geçiş hizmeti 'nin SQL Server 'dan Azure SQL yönetilen örneği 'ne kapsamlı bir geçiş deneyimi sunmak üzere ile birlikte çalışabileceği çeşitli ağ topolojileri açıklanmaktadır.

## <a name="azure-sql-managed-instance-configured-for-hybrid-workloads"></a>Karma iş yükleri için yapılandırılmış Azure SQL yönetilen örneği 

Azure SQL yönetilen örneğiniz şirket içi ağınıza bağlıysa bu topolojiyi kullanın. Bu yaklaşım en basit ağ yönlendirmeyi sağlar ve geçiş sırasında en fazla veri üretilen işi verir.

![Karma Iş yükleri için ağ topolojisi](media/resource-network-topologies/hybrid-workloads.png)

**Gereksinimler**

- Bu senaryoda, SQL yönetilen örneği ve Azure veritabanı geçiş hizmeti örneği aynı Microsoft Azure Sanal Ağ oluşturulur, ancak farklı alt ağlar kullanır.  
- Bu senaryoda kullanılan sanal ağ aynı zamanda [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanılarak şirket içi ağa bağlanır.

## <a name="sql-managed-instance-isolated-from-the-on-premises-network"></a>Şirket içi ağdan yalıtılmış SQL yönetilen örneği

Ortamınız aşağıdaki senaryolardan birini veya birkaçını gerektiriyorsa, bu ağ topolojisini kullanın:

- SQL yönetilen örneği şirket içi bağlantıdan yalıtılmıştır, ancak Azure veritabanı geçiş hizmeti örneğiniz şirket içi ağa bağlanır.
- Rol tabanlı Access Control (RBAC) ilkeleri yerleşiyorsa ve kullanıcıları SQL yönetilen örneği barındıran aynı aboneliğe erişmek üzere sınırlandırdıysanız.
- SQL yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağlar farklı aboneliklerde.

![Şirket içi ağdan yalıtılmış yönetilen örnek için ağ topolojisi](media/resource-network-topologies/mi-isolated-workload.png)

**Gereksinimler**

- Azure veritabanı geçiş hizmeti 'nin bu senaryo için kullandığı sanal ağ, (veya VPN kullanarak şirket içi ağa da bağlanmalıdır https://docs.microsoft.com/azure/expressroute/expressroute-introduction) . [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)
- SQL yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağ arasında [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) ayarlayın.

## <a name="cloud-to-cloud-migrations-shared-virtual-network"></a>Buluttan buluta geçişler: paylaşılan sanal ağ

Kaynak SQL Server bir Azure VM 'de barındırılıyorsa ve SQL yönetilen örneği ve Azure veritabanı geçiş hizmeti ile aynı sanal ağı paylaşıyorsa, bu topolojiyi kullanın.

![Paylaşılan VNet ile buluttan buluta geçişler için ağ topolojisi](media/resource-network-topologies/cloud-to-cloud.png)

**Gereksinimler**

- Ek gereksinim yok.

## <a name="cloud-to-cloud-migrations-isolated-virtual-network"></a>Buluttan buluta geçişler: yalıtılmış sanal ağ

Ortamınız aşağıdaki senaryolardan birini veya birkaçını gerektiriyorsa, bu ağ topolojisini kullanın:

- SQL yönetilen örneği yalıtılmış bir sanal ağda sağlanır.
- Rol tabanlı Access Control (RBAC) ilkeleri yerleşiyorsa ve kullanıcıları SQL yönetilen örneği barındıran aynı aboneliğe erişmek üzere sınırlandırdıysanız.
- SQL yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağlar farklı aboneliklerde.

![Yalıtılmış VNet ile buluttan buluta geçişler için ağ topolojisi](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Gereksinimler**

- SQL yönetilen örneği ve Azure veritabanı geçiş hizmeti için kullanılan sanal ağ arasında [VNET ağ eşlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) ayarlayın.

## <a name="inbound-security-rules"></a>Gelen güvenlik kuralları

| **AD**   | **BAĞ** | **PROTOCOL** | **KAYNAKTAKI** | **HEDEFINE** | **ÖN** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Herhangi biri      | Herhangi biri          | DMS ALT AĞı | Herhangi biri             | İzin Ver      |

## <a name="outbound-security-rules"></a>Giden güvenlik kuralları

| **AD**                  | **BAĞ**                                              | **PROTOCOL** | **KAYNAKTAKI** | **HEDEFINE**           | **ÖN** | **Kural nedeni**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| yönetim                | 443, 9354                                              | TCP          | Herhangi biri        | Herhangi biri                       | İzin Ver      | Service Bus ve Azure Blob depolama aracılığıyla yönetim düzlemi iletişimi. <br/>(Microsoft eşlemesi etkinse, bu kurala ihtiyaç duymayabilir.)                                                             |
| Tanılama               | 12000                                                 | TCP          | Herhangi biri        | Herhangi biri                       | İzin Ver      | DMS, sorun giderme amacıyla tanılama bilgilerini toplamak için bu kuralı kullanır.                                                                                                                      |
| SQL kaynak sunucusu         | 1433 (veya SQL Server dinlediği TCP IP bağlantı noktası) | TCP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | DMS 'ten kaynak bağlantısı SQL Server <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyaç duymayabilir.)                                                                                       |
| SQL Server adlandırılmış örnek | 1434                                                  | UDP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | DMS 'ten adlandırılmış örnek kaynak bağlantısı SQL Server <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyaç duymayabilir.)                                                                        |
| SMB paylaşma                 | 445                                                   | TCP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | Azure SQL veritabanı mı ve Azure VM 'de SQL Server 'a geçiş için veritabanı yedekleme dosyalarını depolamak üzere DMS için SMB ağ paylaşımının <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyaç duymayabilir). |
| DMS_subnet                | Herhangi biri                                                   | Herhangi biri          | Herhangi biri        | DMS_Subnet                | İzin Ver      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Ayrıca bkz.

- [SQL Server SQL yönetilen örneğine geçir](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure veritabanı geçiş hizmeti 'ni kullanmaya yönelik önkoşullara genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure portalını kullanarak bir sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Sonraki adımlar

- Azure veritabanı geçiş hizmeti 'ne genel bakış için [Azure veritabanı geçiş hizmeti nedir?](dms-overview.md)makalesine bakın.
- Azure veritabanı geçiş hizmeti 'nin bölgesel kullanılabilirliği hakkında güncel bilgiler için [bölgeye göre kullanılabilir ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration) sayfasına bakın.
