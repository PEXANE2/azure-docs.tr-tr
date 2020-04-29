---
title: Sunucuyu yeniden Başlat-Azure portal-MariaDB için Azure veritabanı
description: Bu makalede, Azure portalını kullanarak bir MariaDB sunucusu için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: ed95304807a1a03880cc824c1a58f010203d418d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79534720"
---
# <a name="restart-azure-database-for-mariadb-server-using-azure-portal"></a>Azure portal kullanarak MariaDB sunucusu için Azure veritabanı 'nı yeniden başlatın
Bu konuda, MariaDB sunucusu için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi anlatmaktadır. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.

Yeniden başlatmanın tamamlanması için gereken süre, MariaDB kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MariaDB sunucusu Için Azure veritabanı](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Sunucu yeniden başlatması gerçekleştir

Aşağıdaki adımlar MariaDB sunucusunu yeniden başlatır:

1. Azure portal, MariaDB sunucusu için Azure veritabanınızı seçin.

2. Sunucunun **genel bakış** sayfasının araç çubuğunda **Yeniden Başlat**' a tıklayın.

   ![MariaDB için Azure veritabanı-genel bakış-yeniden Başlat düğmesi](./media/howto-restart-server-portal/2-server.png)

3. Sunucuyu yeniden başlatmayı onaylamak için **Evet** ' e tıklayın.

   ![MariaDB için Azure veritabanı-yeniden başlatma Onayla](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Sunucu durumunun "yeniden başlatılıyor" olarak değiştiğini gözlemleyin.

   ![MariaDB için Azure veritabanı-yeniden başlatma durumu](./media/howto-restart-server-portal/4-restarting-status.png)

5. Sunucu yeniden başlatma işleminin başarılı olduğunu onaylayın.

   ![MariaDB için Azure veritabanı-yeniden başlatma başarılı](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: Azure portal kullanarak MariaDB sunucusu için Azure veritabanı oluşturma](./quickstart-create-mariadb-server-database-using-azure-portal.md)