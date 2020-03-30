---
title: Azure Uygulama Hizmeti - eşitleme ağı yapılandırması
description: Bu makalede, Azure Uygulama Hizmeti barındırma planı için ağ yapılandırmanızın nasıl eşitlenilen bir şekilde eşitlenilen.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 12/13/2018
ms.openlocfilehash: e7263d6a7716caf9f53e8496c6fb02b6d17b5509
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73687904"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Azure App Service barındırma planı için eşitleme ağ yapılandırması

[Uygulamanızı bir Azure Sanal Ağıyla tümleştirseniz](../app-service/web-sites-integrate-with-vnet.md)de Yönetilen Örnek'e bağlantı kuramamanız olabilir. Deneyebileceğiniz tek şey, hizmet planınız için ağ yapılandırmasını yenilemektir.

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>App Service barındırma planı için eşitleme ağı yapılandırması

Bunu yapmak için şu adımları uygulayın:  

1. Web uygulamaları Uygulama Hizmeti planınıza gidin.

   ![uygulama hizmet planı](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. **Ağ'ı** tıklatın ve ardından **Yönetmek için burayı tıklatın.**

   ![hizmet planını yönetme](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. **VNet'inizi** seçin ve **Ağı Eşitle'yi**tıklatın.

   ![eşitleme ağı](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Eşitleme bitene kadar bekleyin.
  
   ![eşitleme yapıldı](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Artık Yönetilen Örneğiniz ile bağlantınızı yeniden kurmaya hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- Yönetilen Örnek için VNet yapılandırma hakkında bilgi için [bkz.](sql-database-managed-instance-connectivity-architecture.md) [How to configure existing VNet](sql-database-managed-instance-configure-vnet-subnet.md)
