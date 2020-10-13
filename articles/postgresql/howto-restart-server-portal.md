---
title: Sunucuyu yeniden Başlat-Azure portal-PostgreSQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure portal kullanarak bir PostgreSQL için Azure veritabanı 'nı nasıl yeniden başlatabileceğinizi açıklar.
author: ajlam
ms.author: andrela
ms.service: postgresql
ms.topic: how-to
ms.date: 5/6/2019
ms.openlocfilehash: d25c8908b46608ac64bcf135404a070740413fe0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90907377"
---
# <a name="restart-azure-database-for-postgresql---single-server-using-the-azure-portal"></a>PostgreSQL için Azure veritabanı 'nı yeniden başlatın-Azure portal kullanarak tek bir sunucu
Bu konuda, PostgreSQL için Azure veritabanı sunucusunu nasıl yeniden başlatabileceğinizi açıklar. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.
 
Yeniden başlatmayı tamamlaması için gereken süre, PostgreSQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [PostgreSQL Için Azure veritabanı sunucusu](quickstart-create-server-database-portal.md)

## <a name="perform-server-restart"></a>Sunucu yeniden başlatması gerçekleştir

Aşağıdaki adımlar PostgreSQL sunucusunu yeniden başlatır:

1. [Azure Portal](https://portal.azure.com/), PostgreSQL Için Azure veritabanı sunucunuzu seçin.

2. Sunucunun **genel bakış** sayfasının araç çubuğunda **Yeniden Başlat**' a tıklayın.

   :::image type="content" source="./media/howto-restart-server-portal/2-server.png" alt-text="PostgreSQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi":::

3. Sunucuyu yeniden başlatmayı onaylamak için **Evet** ' e tıklayın.

   :::image type="content" source="./media/howto-restart-server-portal/3-restart-confirm.png" alt-text="PostgreSQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi" olarak değiştiğini gözlemleyin.

   :::image type="content" source="./media/howto-restart-server-portal/4-restarting-status.png" alt-text="PostgreSQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi":::

5. Sunucu yeniden başlatma işleminin başarılı olduğunu onaylayın.

   :::image type="content" source="./media/howto-restart-server-portal/5-restart-success.png" alt-text="PostgreSQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi":::

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL Için Azure veritabanı 'nda parametrelerin nasıl ayarlanacağı](howto-configure-server-parameters-using-portal.md) hakkında bilgi edinin