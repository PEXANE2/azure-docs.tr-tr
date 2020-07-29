---
title: Azure App Service için ağ yapılandırmasını eşitleyin
titleSuffix: Azure SQL Managed Instance
description: Bu makalede, Azure SQL yönetilen örneğiniz ile Azure App Service barındırma planı için ağ yapılandırmanızı nasıl eşitleyebileceğinizi ele alınmaktadır.
services: sql-database
ms.service: sql-managed-instance
ms.subservice: operations
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 12/13/2018
ms.openlocfilehash: a0192f204fdb0797d98947a174bf6d669034b666
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84695462"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan-with-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği ile Azure App Service barındırma planı için eşitleme ağı yapılandırması
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

[Uygulamanızı bir Azure sanal ağı ile tümleştirseniz](../../app-service/web-sites-integrate-with-vnet.md)de SQL yönetilen örneği ile bağlantı kuramıyoruz. Yenileme veya eşitleme, hizmet planınız için ağ yapılandırması bu sorunu çözebilir. 

## <a name="sync-network-configuration"></a>Eşitleme ağı yapılandırması 

Bunu yapmak için şu adımları uygulayın:  

1. Web Apps App Service planına gidin.

   ![App Service planının ekran görüntüsü](./media/azure-app-sync-network-configuration/app-service-plan.png)

2. **Ağ** ' ı seçin ve ardından **yönetmek Için buraya tıklayın ' ı**seçin.

   ![Hizmet planını yönetme ekran görüntüsü](./media/azure-app-sync-network-configuration/manage-plan.png)

3. **VNET** ' i seçin ve **ağa Eşitle**' ye tıklayın.

   ![Eşitleme ağının ekran görüntüsü](./media/azure-app-sync-network-configuration/sync.png)

4. Eşitleme işlemi tamamlanana kadar bekleyin.
  
   ![Eşitlemenin ekran görüntüsü tamamlandı](./media/azure-app-sync-network-configuration/sync-done.png)

Artık SQL yönetilen örneğinizin bağlantısını yeniden oluşturmaya başlamaya hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- VNet 'i SQL yönetilen örneği için yapılandırma hakkında daha fazla bilgi için bkz. [SQL yönetilen örnek VNET mimarisi](connectivity-architecture-overview.md) ve [var olan VNET 'i yapılandırma](vnet-existing-add-subnet.md).
