---
title: Azure App Service için ağ yapılandırmasını eşitleyin
titleSuffix: Azure SQL Managed Instance
description: Bu makalede, Azure SQL yönetilen örneğiniz ile Azure App Service barındırma planı için ağ yapılandırmanızı nasıl eşitleyebileceğinizi ele alınmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: sqldbrb=1
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
ms.date: 12/13/2018
ms.openlocfilehash: 6e4f6789be2faa8d1af0fd75830541f8a9e27a7e
ms.sourcegitcommit: 053e5e7103ab666454faf26ed51b0dfcd7661996
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84044573"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan-with-azure-sql-managed-instance"></a>Azure SQL yönetilen örneği ile Azure App Service barındırma planı için eşitleme ağı yapılandırması
[!INCLUDE[appliesto-sqlmi](../includes/appliesto-sqlmi.md)]

[Uygulamanızı bir Azure sanal ağı ile tümleştirseniz](../../app-service/web-sites-integrate-with-vnet.md)de SQL yönetilen örneği ile bağlantı kuramıyoruz. Yenileme veya eşitleme, hizmet planınız için ağ yapılandırması bu sorunu çözebilir. 

## <a name="sync-network-configuration"></a>Eşitleme ağı yapılandırması 

Bunu yapmak için şu adımları uygulayın:  

1. Web Apps App Service planına gidin.

   ![App Service planı](./media/azure-app-sync-network-configuration/app-service-plan.png)

2. **Ağ** ' ı seçin ve ardından **yönetmek Için buraya tıklayın ' ı**seçin.

   ![hizmet planını Yönet](./media/azure-app-sync-network-configuration/manage-plan.png)

3. **VNET** ' i seçin ve **ağa Eşitle**' ye tıklayın.

   ![Ağ eşitleme](./media/azure-app-sync-network-configuration/sync.png)

4. Eşitleme işlemi tamamlanana kadar bekleyin.
  
   ![Eşitleme tamamlandı](./media/azure-app-sync-network-configuration/sync-done.png)

Artık SQL yönetilen örneğinizin bağlantısını yeniden oluşturmaya başlamaya hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- VNet 'i SQL yönetilen örneği için yapılandırma hakkında daha fazla bilgi için bkz. [SQL yönetilen örnek VNET mimarisi](connectivity-architecture-overview.md) ve [var olan VNET 'i yapılandırma](vnet-existing-add-subnet.md).
