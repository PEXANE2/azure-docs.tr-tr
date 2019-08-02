---
title: Azure App Service-ağ yapılandırmasını Eşitle | Microsoft Docs
description: Bu makalede, Azure App Service barındırma planı için ağ yapılandırmanızı eşitleme konusu açıklanmaktadır.
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
ms.openlocfilehash: 920851e33255b0bcb5b085c541ae636c5988be72
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567320"
---
# <a name="sync-networking-configuration-for-azure-app-service-hosting-plan"></a>Azure App Service barındırma planı için eşitleme ağı yapılandırması

[Uygulamanızı bir Azure sanal ağı ile tümleştirseniz](../app-service/web-sites-integrate-with-vnet.md), yönetilen örnekle bağlantı kuramıyoruz. Deneyebileceğiniz bir şey, hizmet planınız için ağ yapılandırmasını yenilememiz olabilir.

## <a name="sync-network-configuration-for-app-service-hosting-plan"></a>App Service barındırma planı için ağ yapılandırmasını eşitleme

Bunu yapmak için şu adımları uygulayın:  

1. Web Apps App Service planına gidin.

   ![App Service planı](./media/sql-database-managed-instance-sync-networking/app-service-plan.png)

2. **Ağ** ' a ve ardından **yönetmek için buraya tıklayın**' a tıklayın.

   ![hizmet planını Yönet](./media/sql-database-managed-instance-sync-networking/manage-plan.png)

3. **VNET** ' i seçin ve **ağa Eşitle**' ye tıklayın.

   ![Ağ eşitleme](./media/sql-database-managed-instance-sync-networking/sync.png)

4. Eşitleme işlemi tamamlanana kadar bekleyin.
  
   ![Eşitleme tamamlandı](./media/sql-database-managed-instance-sync-networking/sync-done.png)

Artık yönetilen örneğiniz için bağlantınızı yeniden kurmaya başlamaya hazırsınız.

## <a name="next-steps"></a>Sonraki adımlar

- VNet 'i yönetilen örnek için yapılandırma hakkında daha fazla bilgi için bkz. [yönetilen örnek VNET mimarisi](sql-database-managed-instance-connectivity-architecture.md) ve [var olan VNET 'i yapılandırma](sql-database-managed-instance-configure-vnet-subnet.md).
