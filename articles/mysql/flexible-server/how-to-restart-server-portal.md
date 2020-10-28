---
title: Sunucuyu yeniden Başlat-Azure portal-MySQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı esnek sunucusunu nasıl yeniden başlatabileceğinizi açıklar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: how-to
ms.date: 10/26/2020
ms.openlocfilehash: c44e1da46b969b2d359a225e9d310160ce2092ce
ms.sourcegitcommit: 4cb89d880be26a2a4531fedcc59317471fe729cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92681681"
---
# <a name="restart-azure-database-for-mysql-flexible-server-using-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı esnek sunucusunu yeniden başlatın
Bu konuda, MySQL için Azure veritabanı esnek sunucusunu nasıl yeniden başlatabileceğinizi açıklanmaktadır. Sunucu işlemi gerçekleştirdiğinden kısa süreli bir kesinti oluşmasına neden olan bakım nedenleriyle sunucunuzu yeniden başlatmanız gerekebilir.

Hizmet meşgulse sunucu yeniden başlatması engellenir. Örneğin, hizmet, sanal çekirdekleri ölçeklendirirken daha önce istenen bir işlemi işliyor olabilir.

Yeniden başlatmayı tamamlaması için gereken süre MySQL kurtarma işlemine bağlıdır. Yeniden başlatma süresini azaltmak için, yeniden başlatmadan önce sunucuda gerçekleşen etkinlik miktarını en aza indirmenizi öneririz.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı esnek sunucusu](quickstart-create-server-portal.md)

## <a name="perform-server-restart"></a>Sunucu yeniden başlatması gerçekleştir

Aşağıdaki adımlar MySQL sunucusunu yeniden başlatır:

1. Azure portal, MySQL için Azure veritabanı esnek sunucusu ' nu seçin.

2. Sunucunun **genel bakış** sayfasının araç çubuğunda **Yeniden Başlat** ' a tıklayın.

   :::image type="content" source="./media/how-to-restart-server-portal/2-server.png" alt-text="MySQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi":::

3. Sunucuyu yeniden başlatmayı onaylamak için **Evet** ' e tıklayın.

   :::image type="content" source="./media/how-to-restart-server-portal/3-restart-confirm.png" alt-text="MySQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi" olarak değiştiğini gözlemleyin.

   :::image type="content" source="./media/how-to-restart-server-portal/4-restarting-status.png" alt-text="MySQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi":::

5. Sunucu yeniden başlatma işleminin başarılı olduğunu onaylayın.

   :::image type="content" source="./media/how-to-restart-server-portal/5-restart-success.png" alt-text="MySQL için Azure veritabanı-genel bakış-yeniden Başlat düğmesi":::

## <a name="next-steps"></a>Sonraki adımlar

[Hızlı başlangıç: Azure portal kullanarak MySQL için Azure veritabanı esnek sunucusu oluşturma](quickstart-create-server-portal.md)
