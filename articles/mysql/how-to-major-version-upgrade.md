---
title: Ana sürüm yükseltme-Azure portal-MySQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı 'nın ana sürümünü nasıl yükselteceğiniz açıklanmaktadır
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 11/16/2020
ms.openlocfilehash: 4dd4729589e429cb1b028b183fdfd144617d1d1b
ms.sourcegitcommit: 03c0a713f602e671b278f5a6101c54c75d87658d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/19/2020
ms.locfileid: "94920653"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda ana sürüm yükseltmesi

> [!IMPORTANT]
> MySQL için Azure veritabanı tek sunucu için ana sürüm yükseltmesi genel önizlemede.

Bu makalede MySQL ana sürümünüzü, MySQL için Azure veritabanı tek sunucu 'da nasıl yükseltebileceğinizi açıklamaktadır.

Bu özellik, müşterilerin MySQL 5,6 sunucularının herhangi bir veri hareketi olmadan veya herhangi bir uygulama bağlantı dizesi değişikliğine gerek kalmadan bir tıklama düğmesine sahip MySQL 5,7 ' e yerinde yükseltmelerini gerçekleştirmesini sağlayacaktır.

> [!Note]
> * Ana sürüm yükseltmesi yalnızca MySQL 5,6 ' den MySQL 5,7 ' ye ana sürüm yükseltmesi için kullanılabilir<br>
> * Ana sürüm yükseltmesi henüz Çoğaltma sunucusunda desteklenmiyor.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı tek sunucu](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57"></a>MySQL 5,6 ' den MySQL 5,7 ' e büyük sürüm yükseltmesi gerçekleştirin

MySQL 5,6 Server Azure veritabanınız için ana sürüm yükseltmesini gerçekleştirmek üzere bu adımları izleyin

> [!IMPORTANT]
> Üretimi doğrudan yükseltmek yerine, ilk olarak sunucunun geri yüklenen kopyasında yükseltmeyi gerçekleştirmenizi öneririz. Bkz. [Noktadan noktaya geri yükleme gerçekleştirme](howto-restore-server-portal.md#point-in-time-restore).

1. [Azure Portal](https://portal.azure.com/), var olan MySQL 5,6 Server Için Azure veritabanınızı seçin.

2. **Genel bakış** sayfasında, araç çubuğundaki **Yükselt** düğmesine tıklayın.

3. **Yükseltme** bölümünde MySQL 5,6 Server için Azure veritabanı 'nı 5,7 sunucusuna yükseltmek üzere **Tamam** ' ı seçin.

    :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="MySQL için Azure veritabanı-genel bakış-yükseltme":::

4. Bir bildirim, yükseltmenin başarılı olduğunu doğrulayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[MySQL Için Azure veritabanı sürüm oluşturma ilkesi](concepts-version-policy.md)hakkında bilgi edinin.