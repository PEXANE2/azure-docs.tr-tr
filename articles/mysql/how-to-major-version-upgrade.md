---
title: Ana sürüm yükseltme-Azure portal-MySQL için Azure veritabanı-tek sunucu
description: Bu makalede, Azure portal kullanarak MySQL için Azure veritabanı 'nın ana sürümünü nasıl yükselteceğiniz açıklanmaktadır
author: ambhatna
ms.author: ambhatna
ms.service: mysql
ms.topic: how-to
ms.date: 11/16/2020
ms.openlocfilehash: 78c35e42cefa8897d9f93c3a941b4c0e8b81e5f9
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94687145"
---
# <a name="major-version-upgrade-in-azure-database-for-mysql-single-server-using-the-azure-portal"></a>Azure portal kullanarak MySQL için Azure veritabanı 'nda ana sürüm yükseltmesi

> [!IMPORTANT]
> MySQL için Azure veritabanı tek sunucu için ana sürüm yükseltmesi genel önizlemede.

Bu makalede MySQL ana sürümünüzü, MySQL için Azure veritabanı tek sunucu 'da nasıl yükseltebileceğinizi açıklamaktadır.

Bu özellik, müşterilerin MySQL 5,6 sunucularının herhangi bir veri hareketi olmadan veya herhangi bir uygulama bağlantı dizesi değişikliğine gerek kalmadan bir tıklama düğmesine sahip MySQL 5,7 ' e yerinde yükseltmelerini gerçekleştirmesini sağlayacaktır.

> [!Note]
> * Ana sürüm yükseltmesi yalnızca MySQL 5,6 ' den MySQL 5,7 ' ye ana sürüm yükseltmesi için kullanılabilir<br>
> * Ana sürüm yükseltmesi henüz Çoğaltma sunucusunda desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar
Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:
- [MySQL Için Azure veritabanı tek sunucu](quickstart-create-mysql-server-database-using-azure-portal.md)

## <a name="perform-major-version-upgrade-from-mysql-56-to-mysql-57"></a>MySQL 5,6 ' den MySQL 5,7 ' e büyük sürüm yükseltmesi gerçekleştirin

MySQL 5,6 Server Azure veritabanınız için ana sürüm yükseltmesini gerçekleştirmek üzere bu adımları izleyin

1. [Azure Portal](https://portal.azure.com/), var olan MySQL 5,6 Server Için Azure veritabanınızı seçin.

2. **Genel bakış** sayfasında, araç çubuğundaki **Yükselt** düğmesine tıklayın.

3. **Yükseltme** bölümünde MySQL 5,6 Server için Azure veritabanı 'nı 5,7 sunucusuna yükseltmek üzere **Tamam** ' ı seçin.

    :::image type="content" source="./media/how-to-major-version-upgrade-portal/upgrade.png" alt-text="MySQL için Azure veritabanı-genel bakış-yükseltme":::

4. Bir bildirim, yükseltmenin başarılı olduğunu doğrulayacaktır.

## <a name="next-steps"></a>Sonraki adımlar

[MySQL Için Azure veritabanı sürüm oluşturma ilkesi](concepts-version-policy.md)hakkında bilgi edinin.