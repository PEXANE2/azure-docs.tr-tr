---
title: Ölçeklendirme işlemleri-Azure portal-PostgreSQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure portal aracılığıyla PostgreSQL için Azure veritabanı 'nda Ölçek işlemlerinin nasıl gerçekleştirileceği açıklanır.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: 1542bba53b51ffdf2129953a81e5d13975ade434
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "90941611"
---
# <a name="scale-operations-in-flexible-server"></a>Esnek sunucu 'da ölçeklendirme işlemleri

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

Bu makalede, işlem ve depolama için ölçeklendirme işlemleri gerçekleştirme adımları sağlanmaktadır. , Uygulamanızı çalıştırmak için uygun olan Vcore sayısını seçmek dahil olmak üzere, Burstable, genel amaçlı ve bellek için iyileştirilmiş SKU 'Lar arasında işlem Katmanlarınızı değiştirebileceksiniz. Ayrıca, depolama alanınızı da ölçeklendirebilirsiniz. Beklenen ıOPS, işlem katmanına, sanal çekirdekleri ve depolama kapasitesine göre gösterilir. Maliyet tahmini, seçiminize göre de gösterilir.

> [!IMPORTANT]
> Depolama alanını ölçeklendiremezsiniz.

## <a name="pre-requisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzunu tamamlayabilmeniz için şunlar gerekir:

-   PostgreSQL için Azure veritabanı-esnek sunucu olmalıdır. Aynı yordam, bölge yedekliliği ile yapılandırılmış esnek sunucu için de geçerlidir.
> [!IMPORTANT]
> Yüksek kullanılabilirliğiyle yapılandırıldığında, Burstable SKU 'YU seçebilirsiniz. Ölçeklendirme işlemi sırasında, bekleme öncelikle istenen boyuta ölçeklendirilir, birincil sunucunun yük devri yapılır ve birincil ölçeklendirilir. 

## <a name="scaling-the-compute-tier-and-size"></a>İşlem katmanını ve boyutunu ölçekleme

İşlem katmanını seçmek için aşağıdaki adımları izleyin.
 
1.  [Azure Portal](https://portal.azure.com/), yedeği geri yüklemek istediğiniz esnek sunucunuzu seçin.

2.  **İşlem + depolama**' ya tıklayın.

3.  Geçerli ayarları içeren bir sayfa görüntülenir.
 :::image type="content" source="./media/how-to-scale-compute-storage-portal/click-compute-storage.png" alt-text="işlem + depolama görünümü":::

4.  Burstable, genel amaçlı ve bellek için iyileştirilmiş katmanlar arasında işlem sınıfını seçebilirsiniz.
   :::image type="content" source="./media/how-to-scale-compute-storage-portal/list-compute-tiers.png" alt-text="işlem katmanlarını listeleme":::


5.  Varsayılan sanal çekirdekler ve bellek boyutları ile karşılaşırsanız, bir sonraki adımı atlayabilirsiniz.

6.  Vçekirdekleri sayısını değiştirmek istiyorsanız, **işlem boyutu** açılır listesine tıklayabilir ve listeden Istenen sayıda sanal çekirdek/bellek öğesine tıklayabilirsiniz.
    
    - Burstable işlem katmanı: :::image type="content" source="./media/how-to-scale-compute-storage-portal/compute-burstable-dropdown.png" alt-text="Burstable işlem":::

    - Genel amaçlı işlem katmanı: :::image type="content" source="./media/how-to-scale-compute-storage-portal/compute-general-purpose-dropdown.png" alt-text="genel amaçlı işlem":::

    - Bellek için iyileştirilmiş işlem katmanı: :::image type="content" source="./media/how-to-scale-compute-storage-portal/compute-memory-optimized-dropdown.png" alt-text="bellek için iyileştirilmiş işlem":::

7.  **Kaydet**’e tıklayın. 
8.  Bir onay iletisi görürsünüz. Devam etmek istiyorsanız **Tamam** ' ı tıklatın. 
9.  Devam eden ölçeklendirme işlemi hakkında bir bildirim.


## <a name="scaling-storage-size"></a>Ölçek depolama boyutu

Depolama boyutunuzu artırmak için bu adımları izleyin.

1.  [Azure Portal](https://portal.azure.com/), depolama boyutunu artırmak istediğiniz esnek sunucunuzu seçin.
2.  **İşlem + depolama**' ya tıklayın.

3.  Geçerli ayarları içeren bir sayfa görüntülenir.
   
:::image type="content" source="./media/how-to-scale-compute-storage-portal/click-compute-storage.png" alt-text="işlem + depolama ' ya tıklayın":::
4.  **GiB 'deki** bir slayt çubuğuyla alan depolama boyutu geçerli boyutla gösterilir.

5.  Çubuğu istediğiniz boyuta kaydırın. Karşılık gelen ıOPS numarası gösterilir. IOPS, işlem katmanına ve boyutuna bağlıdır. Maliyet bilgileri de gösterilir. 

 :::image type="content" source="./media/how-to-scale-compute-storage-portal/storage-scaleup.png" alt-text="depolama ölçeği artırma":::

6.  Depolama boyutuyla ilgili bir sorun yaşıyorsanız **Kaydet**' e tıklayın. 
7.  Bir onay iletisi görürsünüz. Devam etmek istiyorsanız **Tamam** ' ı tıklatın. 
8.  Devam eden ölçeklendirme işlemi hakkında bir bildirim.

## <a name="next-steps"></a>Sonraki adımlar

-   [İş sürekliliği](./concepts-business-continuity.md) hakkında bilgi edinin
-   [Yüksek kullanılabilirlik](./concepts-high-availability.md) hakkında bilgi edinin
-   [Yedekleme ve kurtarma](./concepts-backup-restore.md) hakkında bilgi edinin
