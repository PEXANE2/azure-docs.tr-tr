---
title: SQL yönetilen örnek geçişleri için ağ topolojileri
titleSuffix: Azure Database Migration Service
description: Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure SQL Veritabanı yönetilen örnek geçişlerinin kaynak ve hedef yapılandırmalarını öğrenin.
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
ms.openlocfilehash: 48485b7ba0f846afa737454b092a6c1ee986b737
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78254950"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-azure-database-migration-service"></a>Azure Veritabanı Geçiş Hizmeti'ni kullanarak Azure SQL DB Yönetilen Örnek geçişleri için ağ topolojileri

Bu makalede, Azure Veritabanı Geçiş Hizmeti'nin şirket içi SQL Sunucularından Azure SQL Veritabanı Yönetilen Örneği'ne kapsamlı bir geçiş deneyimi sağlamak için çalışabileceği çeşitli ağ topolojileri açıklanmaktadır.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Karma iş yükleri için yapılandırılan Azure SQL Veritabanı Yönetilen Örnek 

Azure SQL Veritabanı Yönetilen Örneğiniz şirket içi ağınıza bağlıysa bu topolojiyi kullanın. Bu yaklaşım, en basitleştirilmiş ağ yönlendirmesini sağlar ve geçiş sırasında maksimum veri veri verimi sağlar.

![Karma İş Yükleri için Ağ Topolojisi](media/resource-network-topologies/hybrid-workloads.png)

**Gereksinimler**

- Bu senaryoda, Azure SQL Veritabanı yönetilen örnek ve Azure Veritabanı Geçiş Hizmeti örneği aynı Microsoft Azure Sanal Ağ'da oluşturulur, ancak farklı alt ağlar kullanırlar.  
- Bu senaryoda kullanılan sanal ağ da [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) veya [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanılarak şirket içi ağa bağlanır.

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Azure SQL Veritabanı Yönetilen Örnek şirket içi ağdan yalıtılmış

Ortamınız aşağıdaki senaryolardan birini veya birkaçını gerektiriyorsa, bu ağ topolojisini kullanın:

- Azure SQL Veritabanı yönetilen örnek şirket içi bağlantıdan yalıtılmış, ancak Azure Veritabanı Geçiş Hizmeti örneğiniz şirket içi ağa bağlıdır.
- Role Based Access Control (RBAC) ilkeleri varsa ve kullanıcıları Azure SQL Veritabanı yönetilen örneğini barındıran aynı aboneliğe erişmekle sınırlamanız gerekir.
- Azure SQL Veritabanı Yönetilen Örneği ve Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağlar farklı aboneliklerdedir.

![Şirket içi ağdan yalıtılmış Yönetilen Örnek için Ağ Topolojisi](media/resource-network-topologies/mi-isolated-workload.png)

**Gereksinimler**

- Azure Veritabanı Geçiş Hizmeti'nin bu senaryo için kullandığı sanal ağ, şirket içihttps://docs.microsoft.com/azure/expressroute/expressroute-introduction) ağa (veya [VPN)](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)kullanılarak da bağlanmalıdır.
- Azure SQL Veritabanı yönetilen örnek için kullanılan sanal ağ ile Azure Veritabanı Geçiş Hizmeti arasında [VNet ağı eşlemeyi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) ayarlayın.

## <a name="cloud-to-cloud-migrations-shared-virtual-network"></a>Bulut-bulut geçişleri: Paylaşılan sanal ağ

Kaynak SQL Server bir Azure VM'de barındırılıyorsa ve aynı sanal ağı Azure SQL Veritabanı yönetilen örneği ve Azure Veritabanı Geçiş Hizmeti ile paylaşıyorsa bu topolojiyi kullanın.

![Paylaşılan bir VNet ile Bulut'a Buluta Geçişler için Ağ Topolojisi](media/resource-network-topologies/cloud-to-cloud.png)

**Gereksinimler**

- Ek gereksinim yok.

## <a name="cloud-to-cloud-migrations-isolated-virtual-network"></a>Bulutlardan buluta geçişler: Yalıtılmış sanal ağ

Ortamınız aşağıdaki senaryolardan birini veya birkaçını gerektiriyorsa, bu ağ topolojisini kullanın:

- Azure SQL Veritabanı yönetilen örneği yalıtılmış bir sanal ağda sağlanmıştır.
- Role Based Access Control (RBAC) ilkeleri varsa ve kullanıcıları Azure SQL Veritabanı yönetilen örneğini barındıran aynı aboneliğe erişmekle sınırlamanız gerekir.
- Azure SQL Veritabanı Yönetilen Örneği ve Azure Veritabanı Geçiş Hizmeti için kullanılan sanal ağlar farklı aboneliklerdedir.

![Yalıtılmış bir VNet ile Bulut-Bulut geçişleri için Ağ Topolojisi](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Gereksinimler**

- Azure SQL Veritabanı yönetilen örnek için kullanılan sanal ağ ile Azure Veritabanı Geçiş Hizmeti arasında [VNet ağı eşlemeyi](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) ayarlayın.

## <a name="inbound-security-rules"></a>Gelen güvenlik kuralları

| **AD**   | **Bağlantı noktası** | **Protokolü** | **Kaynak** | **Hedef** | **Eylem** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Herhangi biri      | Herhangi biri          | DMS ALT AĞ | Herhangi biri             | İzin Ver      |

## <a name="outbound-security-rules"></a>Giden güvenlik kuralları

| **AD**                  | **Bağlantı noktası**                                              | **Protokolü** | **Kaynak** | **Hedef**           | **Eylem** | **Kural nedeni**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| yönetim                | 443,9354                                              | TCP          | Herhangi biri        | Herhangi biri                       | İzin Ver      | Servis Veri Servisi ve Azure blob depolama yoluyla yönetim düzlemi iletişimi. <br/>(Microsoft'un bakışları etkinse, bu kurala ihtiyacınız olmayabilir.)                                                             |
| Tanılama               | 12000                                                 | TCP          | Herhangi biri        | Herhangi biri                       | İzin Ver      | DMS, sorun giderme amacıyla tanılama bilgilerini toplamak için bu kuralı kullanır.                                                                                                                      |
| SQL Kaynak sunucusu         | 1433 (veya SQL Server'ın dinlediği TCP IP bağlantı noktası) | TCP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | DMS'den SQL Server kaynak bağlantısı <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyacınız olmayabilir.)                                                                                       |
| SQL Server adlı örnek | 1434                                                  | UDP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | DMS'den örnek kaynak bağlantısı adlı SQL Server <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyacınız olmayabilir.)                                                                        |
| SMB payı                 | 445                                                   | TCP          | Herhangi biri        | Şirket içi adres alanı | İzin Ver      | Azure SQL Veritabanı MI ve SQL Sunucularına Azure VM'de geçişler için veritabanı yedekleme dosyalarını depolamak için DMS için SMB ağ paylaşımı <br/>(Siteden siteye bağlantınız varsa, bu kurala ihtiyacınız olmayabilir). |
| DMS_subnet                | Herhangi biri                                                   | Herhangi biri          | Herhangi biri        | DMS_Subnet                | İzin Ver      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Ayrıca bkz.

- [SQL Server'ı Azure SQL Veritabanı Yönetilen Örneği'ne geçirin](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Azure Veritabanı Geçiş Hizmeti'ni kullanmak için ön koşullara genel bakış](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Azure portalını kullanarak bir sanal ağ oluşturma](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Sonraki adımlar

- Azure Veritabanı Geçiş Hizmeti'ne genel bir bakış için, [bkz.](dms-overview.md)
- Azure Veritabanı Geçiş Hizmeti'nin bölgesel kullanılabilirliği hakkında güncel bilgiler [için, bölgeye göre kullanılabilen Ürünler](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration) sayfasına bakın.
