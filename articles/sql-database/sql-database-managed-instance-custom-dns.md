---
title: Yönetilen örnek özel DNS
description: Bu konu, Azure SQL Veritabanı Yönetilen Örneği olan özel bir DNS yapılandırma seçeneklerini açıklar.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 07/17/2019
ms.openlocfilehash: 9c98176413f858d236902c332553d47f749aa3c0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80247087"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Azure SQL Veritabanı Yönetilen Örneği için Özel DNS yapılandırma

Azure SQL Veritabanı Yönetilen Örneği, bir Azure [sanal ağında (VNet)](../virtual-network/virtual-networks-overview.md)dağıtılmalıdır. Yönetilen Örnek'ten özel ana bilgisayar adlarının çözülmesini gerektiren birkaç senaryo (örneğin, db posta, bulut veya karma ortamınızdaki diğer SQL örneklerine bağlı sunucular) vardır. Bu durumda, Azure içinde özel bir DNS yapılandırmanız gerekir. 

Yönetilen Örnek iç işleyişi için aynı DNS kullandığından, ortak alan adlarını çözebilmek için özel DNS sunucusunu yapılandırın.

> [!IMPORTANT]
> Posta sunucusu, SQL Server örneği ve diğer hizmetler için özel DNS bölgenizde olsalar bile her zaman tam nitelikli bir etki alanı adı (FQDN) kullanın. Örneğin, doğru `smtp.contoso.com` şekilde çözülmeyeceği `smtp` için posta sunucunuz için kullanın. Aynı sanal ağ içinde SQL VM'lere başvuran bağlantılı bir sunucu veya çoğaltma oluşturmak için bir FQDN ve varsayılan Bir DNS soneki de gerektirir. Örneğin, `SQLVM.internal.cloudapp.net`. Daha fazla bilgi için, [kendi DNS sunucunuzu kullanan Ad çözünürlüğüne](https://docs.microsoft.com/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)bakın.

> [!IMPORTANT]
> Sanal ağ DNS sunucularını güncelleştirmek Yönetilen Örnek'i hemen etkilemez. Yönetilen Örnek DNS yapılandırması, DHCP kira süresi dolduktan sonra veya platform yükseltmesi sonrasında (hangisi önce gerçekleşirse) güncelleştirilir. **Kullanıcıların ilk Yönetilen Örneğini oluşturmadan önce sanal ağ DNS yapılandırmalarını ayarlamaları önerilir.**

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [Yönetilen Örnek Nedir](sql-database-managed-instance.md)
- Yeni yönetilen örnek oluşturmayı gösteren [bir](sql-database-managed-instance-get-started.md)öğretici için bkz.
- Yönetilen Örnek için Bir VNet yapılandırma hakkında bilgi için Yönetilen [Örnekler için VNet yapılandırması na](sql-database-managed-instance-connectivity-architecture.md) bakın
