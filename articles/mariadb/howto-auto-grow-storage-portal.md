---
title: Otomatik büyütme depolama - Azure portalı - MariaDB için Azure Veritabanı
description: Bu makalede, Azure portalLarını kullanarak MariaDB için Azure Veritabanı için otomatik büyütme depolamasını nasıl etkinleştirebileceğiniz açıklanmaktadır
author: ambhatna
ms.author: ambhatna
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 7ec4c7a61725fa02f00bf38048182d640666ae8f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528906"
---
# <a name="auto-grow-storage-in-azure-database-for-mariadb-using-the-azure-portal"></a>Azure portalını kullanarak MariaDB için Azure Veritabanı'nda otomatik depolama büyütün
Bu makalede, MariaDB sunucu depolama için bir Azure Veritabanını iş yükünü etkilemeden büyüyecek şekilde nasıl yapılandırabileceğiniz açıklanmaktadır.

Bir sunucu ayrılan depolama sınırına ulaştığında, sunucu salt okunur olarak işaretlenir. Ancak, depolama otomatik büyümesini etkinleştirirseniz, sunucu depolama büyüyen verileri karşılamak için artar. 100 GB'dan az depoya sahip sunucular için, ücretsiz depolama alanı 1 GB'ın altında veya sağlanan depolama alanının %10'unun altında olduğu anda sağlanan depolama boyutu 5 GB artırılır. 100 GB'dan fazla depoya sahip sunucular için, ücretsiz depolama alanı sağlanan depolama alanının %5'in altında olduğunda, sağlanan depolama boyutu %5 artırılır. [Burada](https://docs.microsoft.com/azure/mariadb/concepts-pricing-tiers#storage) belirtildiği gibi maksimum depolama limitleri geçerlidir.

## <a name="prerequisites"></a>Ön koşullar
Bu nasıl yapılacağını kılavuzunu tamamlamak için şunları yapmanız gerekir:
- [MariaDB sunucusu için bir Azure Veritabanı](./quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="enable-storage-auto-grow"></a>Depolama otomatik büyümesini etkinleştirin 

MariaDB sunucu depolama otomatik büyümek ayarlamak için aşağıdaki adımları izleyin:

1. Azure [portalında,](https://portal.azure.com/)MariaDB sunucusu için mevcut Azure Veritabanınızı seçin.

2. MariaDB sunucu sayfasında, **Ayarlar** başlığı altında, fiyatlandırma katmanı sayfasını açmak için **Fiyatlandırma katmanını** tıklatın.

3. Otomatik büyüme bölümünde, depolama otomatik büyümesini etkinleştirmek için **Evet'i** seçin.

    ![MariaDB için Azure Veritabanı - Settings_Pricing_tier - Otomatik büyüme](./media/howto-auto-grow-storage-portal/3-auto-grow.png)

4. Değişiklikleri kaydetmek için **Tamam**’a tıklayın.

5. Bir bildirim, otomatik büyümenin başarıyla etkinleştirildiğini doğrular.

    ![MariaDB için Azure Veritabanı - otomatik büyüme başarısı](./media/howto-auto-grow-storage-portal/5-auto-grow-successful.png)

## <a name="next-steps"></a>Sonraki adımlar

Ölçümler [de nasıl uyarı oluşturacağınız](howto-alert-metric.md)hakkında bilgi edinin.
