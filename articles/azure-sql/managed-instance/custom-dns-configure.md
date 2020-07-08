---
title: Özel DNS
titleSuffix: Azure SQL Managed Instance
description: Bu konuda, Azure SQL yönetilen örneği ile özel bir DNS için yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 2ba5794ba647c28cde3b54a1afdfbd0201b23e8e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84706163"
---
# <a name="configure-a-custom-dns-for-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği için özel bir DNS yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneğinin bir Azure [sanal ağı (VNet)](../../virtual-network/virtual-networks-overview.md)içinde dağıtılması gerekir. Özel ana bilgisayar adlarının SQL Yönetilen Örneğinden çözümlenmesini gerektiren birkaç senaryo (örneğin veritabanı postası, bulutunuzda veya hibrit ortamınızda bulunan diğer SQL Server örnekleriyle bağlantılı sunucular) vardır. Böyle bir durumda Azure içinde özel DNS yapılandırmanız gerekir. 

SQL yönetilen örneği, iç işleyişi için aynı DNS 'i kullandığından, özel DNS sunucusunu ortak etki alanı adlarını çözümleyebilecek şekilde yapılandırın.

> [!IMPORTANT]
> Her zaman posta sunucusu için tam etki alanı adı (FQDN), SQL Server örneği ve diğer hizmetler için ise özel DNS bölgeniz içinde olsalar bile kullanın. Örneğin, `smtp.contoso.com` doğru çözümlenmeyeceği için posta sunucunuz için kullanın `smtp` . Aynı sanal ağ içinde SQL Server VM 'lerine başvuran bağlı sunucu veya çoğaltma oluşturmak için de bir FQDN ve varsayılan DNS soneki gerekir. Örneğin, `SQLVM.internal.cloudapp.net`. Daha fazla bilgi için, bkz. [kendı DNS sunucunuzu kullanan ad çözümlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

> [!IMPORTANT]
> Sanal ağ DNS sunucularının güncelleştirilmesi, SQL yönetilen örneğini hemen etkilemez. SQL yönetilen örneği DNS yapılandırması, DHCP kira süresi dolduktan sonra veya platform yükseltildikten sonra, hangisi önce gerçekleşirse güncellenir. **Kullanıcıların, ilk yönetilen örneğini oluşturmadan önce sanal ağ DNS yapılandırmalarını ayarlaması önerilir.**

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Azure SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
- Yeni bir yönetilen örnek oluşturma hakkında bir öğretici için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).
- Bir sanal ağı yönetilen bir örnek için yapılandırma hakkında bilgi için bkz. [yönetilen örnekler Için VNET yapılandırması](connectivity-architecture-overview.md).
