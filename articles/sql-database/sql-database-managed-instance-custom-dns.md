---
title: Yönetilen örnek özel DNS
description: Bu konu, Azure SQL veritabanı yönetilen örneği ile özel bir DNS için yapılandırma seçeneklerini açıklamaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 1b27d316114b84434a84fc8226eba9328a803dc8
ms.sourcegitcommit: 318d1bafa70510ea6cdcfa1c3d698b843385c0f6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83774376"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği için Özel DNS yapılandırma

Azure SQL veritabanı yönetilen örneği bir Azure [sanal ağı (VNet)](../virtual-network/virtual-networks-overview.md)içinde dağıtılmalıdır. Yönetilen örnekten özel ana bilgisayar adlarının çözümlenemesinin gerekli olduğu birkaç senaryo (örneğin, DB posta, bağlı sunucular, bulut veya karma ortamınızdaki diğer SQL örneklerine) vardır. Bu durumda, Azure içinde özel bir DNS yapılandırmanız gerekir. 

Yönetilen örnek, iç işleyişi için aynı DNS 'yi kullandığından, özel DNS sunucusunu ortak etki alanı adlarını çözümleyebilecek şekilde yapılandırın.

> [!IMPORTANT]
> Her zaman posta sunucusu için tam etki alanı adı (FQDN), SQL Server örneği ve diğer hizmetler için, özel DNS bölgeniz içinde olsalar bile kullanın. Örneğin, `smtp.contoso.com` doğru çözümlenmeyeceği için posta sunucunuz için kullanın `smtp` . Aynı sanal ağ içinde SQL VM 'lerine bağlı sunucu veya çoğaltma oluşturmak için Ayrıca bir FQDN ve varsayılan DNS soneki gerekir. Örneğin, `SQLVM.internal.cloudapp.net`. Daha fazla bilgi için, bkz. [kendı DNS sunucunuzu kullanan ad çözümlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

> [!IMPORTANT]
> Sanal ağ DNS sunucularının güncelleştirilmesi, yönetilen örneği hemen etkilemez. Yönetilen örnek DNS yapılandırması, DHCP kira süresi dolduktan veya platform yükseltildikten sonra, hangisi önce gerçekleşirse güncellenir. **Kullanıcıların, ilk yönetilen örneğini oluşturmadan önce sanal ağ DNS yapılandırmalarını ayarlaması önerilir.**

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md)
- Yeni bir yönetilen örnek oluşturma hakkında bir öğretici için bkz. [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
- Bir sanal ağı yönetilen bir örnek için yapılandırma hakkında bilgi için bkz. [yönetilen örnekler Için VNET yapılandırması](sql-database-managed-instance-connectivity-architecture.md)
