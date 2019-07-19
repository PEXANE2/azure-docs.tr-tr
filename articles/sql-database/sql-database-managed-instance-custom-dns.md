---
title: Azure SQL veritabanı yönetilen örnek özel DNS | Microsoft Docs
description: Bu konu, Azure SQL veritabanı yönetilen örneği ile özel bir DNS için yapılandırma seçeneklerini açıklamaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 07/17/2019
ms.openlocfilehash: 674c5d48dad5d3cfd138853d7ea38ae4a216c93d
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68309876"
---
# <a name="configuring-a-custom-dns-for-azure-sql-database-managed-instance"></a>Azure SQL veritabanı yönetilen örneği için özel DNS yapılandırma

Azure SQL veritabanı yönetilen örneği bir Azure [sanal ağı (VNet)](../virtual-network/virtual-networks-overview.md)içinde dağıtılmalıdır. Yönetilen örnekten özel ana bilgisayar adlarının çözümlenemesinin gerekli olduğu birkaç senaryo (örneğin, DB posta, bağlı sunucular, bulut veya karma ortamınızdaki diğer SQL örneklerine) vardır. Bu durumda, Azure içinde özel bir DNS yapılandırmanız gerekir. 

Yönetilen örnek, iç işleyişi için aynı DNS 'yi kullandığından, özel DNS sunucusunu ortak etki alanı adlarını çözümleyebilecek şekilde yapılandırmanız gerekir.

   > [!IMPORTANT]
   > Posta sunucuları, SQL sunucuları ve diğer hizmetler için, özel DNS bölgeniz içinde olsalar bile her zaman tam etki alanı adlarını (FQDN) kullanın. Örneğin, posta `smtp.contoso.com` sunucusu için kullanımı, basit `smtp` olarak düzgün çözümlenemeyecek.

   > [!IMPORTANT]
   > Sanal ağ DNS sunucularının güncelleştirilmesi, yönetilen örneği hemen etkilemez. Yönetilen örnek DNS yapılandırması, DHCP kira süresi dolduktan sonra veya platform upgarade sonrasında, ne olursa olsun güncelleştirilir. **Kullanıcıların, ilk yönetilen örneğini oluşturmadan önce sanal ağ DNS yapılandırmalarını ayarlaması önerilir.**

## <a name="next-steps"></a>Sonraki adımlar

- Genel bakış için bkz. [yönetilen örnek nedir?](sql-database-managed-instance.md)
- Yeni bir yönetilen örnek oluşturma hakkında bir öğretici için bkz. [yönetilen örnek oluşturma](sql-database-managed-instance-get-started.md).
- Bir sanal ağı yönetilen bir örnek için yapılandırma hakkında bilgi için bkz. [yönetilen örnekler Için VNET yapılandırması](sql-database-managed-instance-connectivity-architecture.md)
