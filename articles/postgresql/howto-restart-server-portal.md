---
title: Sunucuyı yeniden başlat - Azure portalı - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, Azure portalını kullanarak PostgreSQL - Single Server için bir Azure Veritabanını nasıl yeniden başlatabileceğiniz açıklanmaktadır.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 52ffb3943e6e3f209fd236216cc44026dff59dad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74770093"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>PostgreSQL için Azure Veritabanını Yeniden Başlat - Azure portalını kullanarak Tek Sunucu
Bu konu, PostgreSQL sunucusu için bir Azure Veritabanını nasıl yeniden başlatabileceğinizi açıklar. Sunucu işlemi gerçekleştirirken kısa bir kesintiye neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatılır. Örneğin, hizmet vCores ölçekleme gibi daha önce istenen bir işlem işliyor olabilir.
 
Yeniden başlatmayı tamamlamak için gereken süre PostgreSQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda oluşan etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [PostgreSQL sunucusu için Bir Azure Veritabanı](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>Sunucu yeniden başlatmayı gerçekleştirin

Aşağıdaki adımlar PostgreSQL sunucusunu yeniden başlatın:

1. Azure [portalında,](https://portal.azure.com/)PostgreSQL sunucusu için Azure Veritabanınızı seçin.

2. Sunucunun **Genel Bakış** sayfasının araç çubuğunda **Yeniden Başlat'ı**tıklatın.

   ![PostgreSQL için Azure Veritabanı - Genel Bakış - Yeniden Başlat düğmesi](./media/howto-restart-server-portal/2-server.png)

3. Sunucunun yeniden başlatılmasını onaylamak için **Evet'i** tıklatın.

   ![PostgreSQL için Azure Veritabanı - Yeniden başlat onaylayın](./media/howto-restart-server-portal/3-restart-confirm.png)

4. Sunucu durumunun "Yeniden Başlatma" olarak değiştiğini gözlemleyin.

   ![PostgreSQL için Azure Veritabanı - Yeniden Başlatma durumu](./media/howto-restart-server-portal/4-restarting-status.png)

5. Sunucu yeniden başlatmanın başarılı olduğunu onaylayın.

   ![PostgreSQL için Azure Veritabanı - Başarıyı yeniden başlat](./media/howto-restart-server-portal/5-restart-success.png)

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL için Azure Veritabanı'nda parametrelerin nasıl ayarlanması](howto-configure-server-parameters-using-portal.md) hakkında bilgi edinin