---
title: Otomatik büyütme depolama - Azure portalı - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL için Azure Veritabanı'ndaki Azure portalını kullanarak depolama otomatik büyümesini nasıl yapılandırabileceğiniz açıklanmaktadır - Tek Sunucu
author: ambhatna
ms.author: ambhatna
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: 5e4f9d68d02edf456394d4ce10b7b6af5f8643d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74769243"
---
# <a name="auto-grow-storage-using-the-azure-portal-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı'ndaki Azure portalını kullanarak otomatik büyütme depolama - Single Server
Bu makalede, iş yükünü etkilemeden büyümek üzere PostgreSQL sunucu depolama için bir Azure Veritabanını nasıl yapılandırabileceğiniz açıklanmaktadır.

Bir sunucu ayrılan depolama sınırına ulaştığında, sunucu salt okunur olarak işaretlenir. Ancak, depolama otomatik büyümesini etkinleştirirseniz, sunucu depolama büyüyen verileri karşılamak için artar. 100 GB'dan az depoya sahip sunucular için, ücretsiz depolama alanı 1 GB'ın altında veya sağlanan depolama alanının %10'unun altında olduğu anda sağlanan depolama boyutu 5 GB artırılır. 100 GB'dan fazla depoya sahip sunucular için, ücretsiz depolama alanı sağlanan depolama alanının %5'in altında olduğunda, sağlanan depolama boyutu %5 artırılır. [Burada](https://docs.microsoft.com/azure/postgresql/concepts-pricing-tiers#storage) belirtildiği gibi maksimum depolama limitleri geçerlidir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [PostgreSQL sunucusu için Bir Azure Veritabanı](quickstart-create-server-database-portal.md)

## <a name="enable-storage-auto-grow"></a>Depolama otomatik büyümesini etkinleştirin 

PostgreSQL sunucu depolama otomatik büyümek ayarlamak için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/)PostgreSQL sunucusu için mevcut Azure Veritabanınızı seçin.

2. PostgreSQL sunucu sayfasında, **Ayarlar**altında, fiyatlandırma katmanı sayfasını açmak için **Fiyatlandırma katmanını** tıklatın.

3. Otomatik **büyüme** bölümünde, depolama otomatik büyümesini etkinleştirmek için **Evet'i** seçin.

    ![PostgreSQL için Azure Veritabanı - Settings_Pricing_tier - Otomatik büyüme](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

5. Bir bildirim, otomatik büyümenin başarıyla etkinleştirildiğini doğrular.

    ![PostgreSQL için Azure Veritabanı - otomatik büyüme başarısı](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Sonraki adımlar

Ölçümler [de nasıl uyarı oluşturacağınız](howto-alert-on-metric.md)hakkında bilgi edinin.
