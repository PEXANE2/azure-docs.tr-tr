---
title: Sunucuyı yeniden başlat - Azure portalı - MariaDB için Azure Veritabanı
description: Bu makalede, Azure Portalı'nı kullanarak MariaDB sunucusu için bir Azure Veritabanını nasıl yeniden başlatabileceğiniz açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: ed95304807a1a03880cc824c1a58f010203d418d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534720"
---
# <a name="restart-azure-database-for-mariadb-server-using-azure-portal"></a>Azure portalLarını kullanarak MariaDB sunucusu için Azure Veritabanını yeniden başlatın
Bu konu, MariaDB sunucusu için bir Azure Veritabanını nasıl yeniden başlatabileceğinizi açıklar. Sunucu işlemi gerçekleştirirken kısa bir kesintiye neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatılır. Örneğin, hizmet vCores ölçekleme gibi daha önce istenen bir işlem işliyor olabilir.

Yeniden başlatmayı tamamlamak için gereken süre MariaDB kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda oluşan etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [MariaDB sunucusu için bir Azure Veritabanı](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Sunucu yeniden başlatmayı gerçekleştirin

Aşağıdaki adımlar MariaDB sunucusunu yeniden başlatın:

1. Azure portalında, MariaDB sunucusu için Azure Veritabanınızı seçin.

2. Sunucunun **Genel Bakış** sayfasının araç çubuğunda **Yeniden Başlat'ı**tıklatın.

   ![MariaDB için Azure Veritabanı - Genel Bakış - Yeniden Başlat düğmesi](./media/howto-restart-server-portal/2-server.png)

3. Sunucunun yeniden başlatılmasını onaylamak için **Evet'i** tıklatın.

   ![MariaDB için Azure Veritabanı - Yeniden başlat onaylayın](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Sunucu durumunun "Yeniden Başlatma" olarak değiştiğini gözlemleyin.

   ![MariaDB için Azure Veritabanı - Yeniden Başlatma durumu](./media/howto-restart-server-portal/4-restarting-status.png)

5. Sunucu yeniden başlatmanın başarılı olduğunu onaylayın.

   ![MariaDB için Azure Veritabanı - Başarıyı yeniden başlatın](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Quickstart: Azure portalLarını kullanarak MariaDB sunucusu için Azure Veritabanı oluşturma](./quickstart-create-mariadb-server-database-using-azure-portal.md)