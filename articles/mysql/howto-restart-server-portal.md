---
title: Sunucuyı yeniden başlat - Azure portalı - MySQL için Azure Veritabanı
description: Bu makalede, Azure portalını kullanarak MySQL sunucusu için bir Azure Veritabanını nasıl yeniden başlatabileceğiniz açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: d7e158124347b302492364df46ccf5b5e78e75c8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80063295"
---
# <a name="restart-azure-database-for-mysql-server-using-azure-portal"></a>Azure portalLarını kullanarak MySQL sunucusu için Azure Veritabanını yeniden başlatın
Bu konu, MySQL sunucusu için bir Azure Veritabanını nasıl yeniden başlatabileceğinizi açıklar. Sunucu işlemi gerçekleştirirken kısa bir kesintiye neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatılır. Örneğin, hizmet vCores ölçekleme gibi daha önce istenen bir işlem işliyor olabilir.

Yeniden başlatmayı tamamlamak için gereken süre MySQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda oluşan etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [MySQL sunucusu için bir Azure Veritabanı](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-server-restart"></a>Sunucu yeniden başlatmayı gerçekleştirin

Aşağıdaki adımlar MySQL sunucusunu yeniden başlatın:

1. Azure portalında MySQL sunucusu için Azure Veritabanınızı seçin.

2. Sunucunun **Genel Bakış** sayfasının araç çubuğunda **Yeniden Başlat'ı**tıklatın.

   ![MySQL için Azure Veritabanı - Genel Bakış - Yeniden Başlat düğmesi](./media/howto-restart-server-portal/2-server.png)

3. Sunucunun yeniden başlatılmasını onaylamak için **Evet'i** tıklatın.

   ![MySQL için Azure Veritabanı - Yeniden başlat onaylayın](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Sunucu durumunun "Yeniden Başlatma" olarak değiştiğini gözlemleyin.

   ![MySQL için Azure Veritabanı - Yeniden Başlatma durumu](./media/howto-restart-server-portal/4-restarting-status.png)

5. Sunucu yeniden başlatmanın başarılı olduğunu onaylayın.

   ![MySQL için Azure Veritabanı - Başarıyı yeniden başlat](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[Quickstart: Azure portalLarını kullanarak MySQL sunucusu için Azure Veritabanı oluşturma](./quickstart-create-mysql-server-database-using-azure-portal.md)
