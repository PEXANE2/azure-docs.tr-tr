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
ms.openlocfilehash: 6a6cca1228c9891c023ce96532dc89b2f527e9a3
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655518"
---
# <a name="configure-a-custom-dns-for-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği için özel bir DNS yapılandırma
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

Azure SQL yönetilen örneğinin bir Azure [sanal ağı (VNet)](../../virtual-network/virtual-networks-overview.md)içinde dağıtılması gerekir. Özel ana bilgisayar adlarının SQL yönetilen örneğinden çözümlenmesi gereken birkaç senaryo (örneğin, DB posta, bağlı sunucular, bulut veya karma ortamınızda bulunan diğer SQL Server örneklerine) vardır. Bu durumda, Azure içinde özel bir DNS yapılandırmanız gerekir. 

SQL yönetilen örneği, iç işleyişi için aynı DNS 'i kullandığından, özel DNS sunucusunu ortak etki alanı adlarını çözümleyebilecek şekilde yapılandırın.

> [!IMPORTANT]
> Her zaman posta sunucusu için tam etki alanı adı (FQDN), SQL Server örneği ve diğer hizmetler için ise özel DNS bölgeniz içinde olsalar bile kullanın. Örneğin, `smtp.contoso.com` doğru çözümlenmeyeceği için posta sunucunuz için kullanın `smtp` . Aynı sanal ağ içinde SQL Server VM 'lerine başvuran bağlı sunucu veya çoğaltma oluşturmak için de bir FQDN ve varsayılan DNS soneki gerekir. Örneğin, `SQLVM.internal.cloudapp.net`. Daha fazla bilgi için, bkz. [kendı DNS sunucunuzu kullanan ad çözümlemesi](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server).

> [!IMPORTANT]
> Sanal ağ DNS sunucularının güncelleştirilmesi, SQL yönetilen örneğini hemen etkilemez. SQL yönetilen örneği DNS yapılandırması, DHCP kira süresi dolduktan sonra veya platform yükseltildikten sonra, hangisi önce gerçekleşirse güncellenir. **Kullanıcıların, ilk yönetilen örneğini oluşturmadan önce sanal ağ DNS yapılandırmalarını ayarlaması önerilir.**

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Azure SQL yönetilen örneği nedir?](sql-managed-instance-paas-overview.md).
- Yeni bir yönetilen örnek oluşturma hakkında bir öğretici için bkz. [yönetilen örnek oluşturma](instance-create-quickstart.md).
- Bir sanal ağı yönetilen bir örnek için yapılandırma hakkında bilgi için bkz. [yönetilen örnekler Için VNET yapılandırması](connectivity-architecture-overview.md).
