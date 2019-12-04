---
title: Sunucuyu yeniden Başlat-Azure portal-MySQL için Azure veritabanı
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 1857c67a77b9600f3d8f7c222a8e06f899fba728
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74774060"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı sunucusunu yeniden başlatma
Bu konuda, MySQL sunucusu için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi açıklanmaktadır. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.

Yeniden başlatmayı tamamlaması için gereken süre MySQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı sunucusu](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Sunucu yeniden başlatması gerçekleştir

Aşağıdaki adımlar MySQL sunucusunu yeniden başlatır:

1. Azure portal, MySQL için Azure veritabanı sunucunuzu seçin.

2. Sunucunun **genel bakış** sayfasının araç çubuğunda **Yeniden Başlat**' a tıklayın.

   ![MySQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi](./media/howto-restart-server-portal/2-server.png)

3. Sunucuyu yeniden başlatmayı onaylamak için **Evet** ' e tıklayın.

   ![MySQL için Azure veritabanı-yeniden başlatma Onayla](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Sunucu durumunun "yeniden başlatılıyor" olarak değiştiğini gözlemleyin.

   ![MySQL için Azure veritabanı-yeniden başlatma durumu](./media/howto-restart-server-portal/4-restarting-status.png)

5. Sunucu yeniden başlatma işleminin başarılı olduğunu onaylayın.

   ![MySQL için Azure veritabanı-yeniden başlatma başarılı](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: Azure portal kullanarak MySQL için Azure veritabanı sunucusu oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)
