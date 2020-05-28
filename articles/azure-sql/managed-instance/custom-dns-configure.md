---
title: Özel DNS
titleSuffix: Azure SQL Managed Instance
description: Bu konuda, Azure SQL yönetilen örneği ile özel bir DNS için yapılandırma seçenekleri açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 5f442eb5b48611d39e9a123a495f8f42095c8017
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84045175"
---
# <a name="configuring-a-custom-dns-for-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği için özel DNS yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneğinin bir Azure [sanal ağı (VNet)](../../virtual-network/virtual-networks-overview.md)içinde dağıtılması gerekir. Özel ana bilgisayar adlarının SQL yönetilen örneğinden çözümlenmesi gereken birkaç senaryo (örneğin, DB posta, bağlı sunucular, bulut veya karma ortamınızda bulunan diğer SQL örneklerine) vardır. Bu durumda, Azure içinde özel bir DNS yapılandırmanız gerekir. 

SQL yönetilen örneği, iç işleyişi için aynı DNS 'i kullandığından, özel DNS sunucusunu ortak etki alanı adlarını çözümleyebilecek şekilde yapılandırın.

> [!IMPORTANT]
> Her zaman posta sunucusu için tam etki alanı adı (FQDN), SQL Server örneği ve diğer hizmetler için, özel DNS bölgeniz içinde olsalar bile kullanın. Örneğin, `smtp.contoso.com` doğru çözümlenmeyeceği için posta sunucunuz için kullanın `smtp` . Aynı sanal ağ içinde SQL VM 'lerine bağlı sunucu veya çoğaltma oluşturmak için Ayrıca bir FQDN ve varsayılan DNS soneki gerekir. Örneğin, `SQLVM.internal.cloudapp.net`. Daha fazla bilgi için, bkz. [kendı DNS sunucunuzu kullanan ad çözümlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

> [!IMPORTANT]
> Sanal ağ DNS sunucularının güncelleştirilmesi, SQL yönetilen örneğini hemen etkilemez. Yönetilen örnek DNS yapılandırması, DHCP kira süresi dolduktan veya platform yükseltildikten sonra, hangisi önce gerçekleşirse güncellenir. **Kullanıcıların, ilk yönetilen örneğini oluşturmadan önce sanal ağ DNS yapılandırmalarını ayarlaması önerilir.**

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-managed-instance-paas-overview.md)
- Yeni bir yönetilen örnek oluşturma hakkında bir öğretici için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).
- Bir sanal ağı yönetilen bir örnek için yapılandırma hakkında bilgi için bkz. [yönetilen örnekler Için VNET yapılandırması](connectivity-architecture-overview.md)
