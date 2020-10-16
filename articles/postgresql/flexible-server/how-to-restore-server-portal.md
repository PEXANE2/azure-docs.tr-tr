---
title: Geri yükleme-Azure portal-PostgreSQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure portal aracılığıyla PostgreSQL için Azure veritabanı 'nda geri yükleme işlemlerinin nasıl gerçekleştirileceği açıklanır.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: e69bcb3d9e4dca4c45bf9a6fe8ed4d54e7f4a8cd
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90941459"
---
# <a name="point-in-time-restore-of-a-flexible-server"></a>Esnek bir sunucunun zaman içindeki bir noktaya geri yüklemesi

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

Bu makalede, yedeklemeleri kullanarak esnek sunucu 'da zaman içinde kurtarma gerçekleştirmek için adım adım yordamlar sunulmaktadır. Bekletme döneminizin içinde en erken geri yükleme noktası veya özel geri yükleme noktası gerçekleştirebilirsiniz.

## <a name="pre-requisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

-   PostgreSQL için Azure veritabanı-esnek sunucu olmalıdır. Aynı yordam, bölge yedekliliği ile yapılandırılmış esnek sunucu için de geçerlidir.

## <a name="restoring-to-the-earliest-restore-point"></a>En erken geri yükleme noktasına geri yükleme

En eski mevcut yedeği kullanarak esnek sunucunuzu geri yüklemek için bu adımları izleyin.

1.   [Azure Portal](https://portal.azure.com/), yedeği geri yüklemek istediğiniz esnek sunucunuzu seçin.

2.  Sol panelde **Genel Bakış ' a** tıklayın ve **geri yükle** ' ye tıklayın
   
   :::image type="content" source="./media/how-to-restore-server-portal/restore-overview.png" alt-text="Geri yüklemeye genel bakış":::

3.  Geri yükleme sayfası, en erken geri yükleme noktası ve özel geri yükleme noktası arasından seçim yapabileceğiniz bir seçenekle gösterilir.

4.  **En erken geri yükleme noktası ' nı** seçin ve **yeni sunucuya geri yükle** alanına yeni bir sunucu adı sağlayın. Geri yükleme işlemi için kullanabileceğiniz en erken zaman damgası görüntülenir. 
   
   :::image type="content" source="./media/how-to-restore-server-portal/restore-earliest.png" alt-text="Geri yüklemeye genel bakış":::

5.  **Tamam**'a tıklayın.

6.  Geri yükleme işleminin başlatıldığı bir bildirim gösterilir.

## <a name="restoring-to-a-custom-restore-point"></a>Özel bir geri yükleme noktasına geri yükleme

En eski mevcut yedeği kullanarak esnek sunucunuzu geri yüklemek için bu adımları izleyin.

1.   [Azure Portal](https://portal.azure.com/), yedeği geri yüklemek istediğiniz esnek sunucunuzu seçin.

2.  Genel Bakış sayfasında **geri yükle**' ye tıklayın.
 :::image type="content" source="./media/how-to-restore-server-portal/restore-overview.png" alt-text="Geri yüklemeye genel bakış":::
    
3.  Geri yükleme sayfası, en erken geri yükleme noktası ve özel geri yükleme noktası arasından seçim yapabileceğiniz bir seçenekle gösterilir.

4.  **Özel geri yükleme noktası**seçin.

5.  Tarih ve saat ' i seçin ve **yeni sunucuya geri yükle** alanına yeni bir sunucu adı sağlayın. 
   
:::image type="content" source="./media/how-to-restore-server-portal/restore-custom.png" alt-text="Geri yüklemeye genel bakış":::
 
6.  **Tamam**'a tıklayın.

7.  Geri yükleme işleminin başlatıldığı bir bildirim gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

-   [İş sürekliliği](./concepts-business-continuity.md) hakkında bilgi edinin
-    [Bölge yedekli yüksek kullanılabilirlik](./concepts-high-availability.md) hakkında bilgi edinin
-   [Yedekleme ve kurtarma](./concepts-backup-restore.md) hakkında bilgi edinin
