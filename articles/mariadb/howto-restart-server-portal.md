---
title: Sunucuyu yeniden Başlat-Azure portal-MariaDB için Azure veritabanı
description: Bu makalede, Azure portalını kullanarak bir MariaDB sunucusu için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: fb4e56a3f40573a65b679ee026c22dfc5d6e5fa2
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74769430"
---
# <a name="restart-azure-database-for-mariadb-server-using-azure-portal"></a>Azure portal kullanarak MariaDB sunucusu için Azure veritabanı 'nı yeniden başlatın
Bu konuda, MariaDB sunucusu için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi anlatmaktadır. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.

Yeniden başlatmanın tamamlanması için gereken süre, MariaDB kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Önkoşullar
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