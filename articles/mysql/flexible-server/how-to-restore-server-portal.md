---
title: Geri yükleme-Azure portal-MySQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure portal aracılığıyla MySQL için Azure veritabanı 'nda geri yükleme işlemlerinin nasıl gerçekleştirileceği açıklanır.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.openlocfilehash: 062d53fcb122ebacd004d7dca5e11f5a883354cd
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93241965"
---
# <a name="point-in-time-restore-of-a-azure-database-for-mysql---flexible-server-preview"></a>MySQL için Azure veritabanı-esnek sunucu (Önizleme) için bir noktadan noktaya geri yükleme


> [!IMPORTANT]
> MySQL için Azure veritabanı-esnek sunucu şu anda genel önizlemededir.

Bu makalede, yedeklemeleri kullanarak esnek sunucu 'da zaman içinde kurtarma gerçekleştirmek için adım adım yordamlar sunulmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

-   MySQL için Azure veritabanı esnek sunucusu olmalıdır.

## <a name="restore-to-the-latest-restore-point"></a>En son geri yükleme noktasına geri yükleme

En eski mevcut yedeği kullanarak esnek sunucunuzu geri yüklemek için bu adımları izleyin.

1.  [Azure Portal](https://portal.azure.com/), yedeği geri yüklemek istediğiniz esnek sunucunuzu seçin.

2.  Sol panelde **Genel Bakış ' a** tıklayın.

3.  Genel Bakış sayfasında **geri yükle**' ye tıklayın.

    Tutucu

4.  Geri yükleme sayfası, **en son geri yükleme noktası** ve özel geri yükleme noktası arasından seçim yapabileceğiniz bir seçenekle gösterilir.

5.  **En son geri yükleme noktası** seçin.


6.  **Yeni sunucuya geri yükle** alanına yeni bir sunucu adı sağlayın.

    :::image type="content" source="./media/concept-backup-restore/restore-blade-latest.png" alt-text="En erken geri yükleme süresi":::

8.  **Tamam**'a tıklayın.

9.  Geri yükleme işleminin başlatıldığı bir bildirim gösterilir.

## <a name="restoring-to-a-custom-restore-point"></a>Özel bir geri yükleme noktasına geri yükleme

En eski mevcut yedeği kullanarak esnek sunucunuzu geri yüklemek için bu adımları izleyin.

1.  [Azure Portal](https://portal.azure.com/), yedeği geri yüklemek istediğiniz esnek sunucunuzu seçin.

2.  Genel Bakış sayfasında **geri yükle**' ye tıklayın.

    Tutucu

3.  Geri yükleme sayfası, en erken geri yükleme noktası ve özel geri yükleme noktası arasından seçim yapabileceğiniz bir seçenekle gösterilir.

4.  **Özel geri yükleme noktası** seçin.

5.  Tarih ve saati seçin.

6.  **Yeni sunucuya geri yükle** alanına yeni bir sunucu adı sağlayın.

6.  **Yeni sunucuya geri yükle** alanına yeni bir sunucu adı sağlayın. 
   
    :::image type="content" source="./media/concept-backup-restore/restore-blade-custom.png" alt-text="genel bakışı görüntüle":::
 
7.  **Tamam**'a tıklayın.

8.  Geri yükleme işleminin başlatıldığı bir bildirim gösterilir.

## <a name="next-steps"></a>Sonraki adımlar

Yer tutucu
