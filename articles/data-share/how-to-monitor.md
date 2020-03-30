---
title: Azure Veri Paylaşımı nasıl izlenir?
description: Azure Veri Paylaşımı'nda davet durumunu, abonelikleri paylaşmayı ve anlık görüntü geçmişini nasıl izleyeceğinizi öğrenin
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 15089dd99de0471c244a6c0d93931438442599a7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73490509"
---
# <a name="monitor-azure-data-share"></a>Azure Veri Paylaşımı’nı izleme  

Bu makalede, Azure Veri Paylaşımı'nı kullanarak veri paylaşımlarınızı nasıl izleyebilirsiniz. Bir veri sağlayıcısı olarak, veri paylaşımı ilişkilerinizin çeşitli yönlerini izleyebilirsiniz. Veri tüketicilerinizin veri paylaşımı davetinizi kabul edip etmediği, ayrıca bir paylaşım aboneliği oluşturup kullanmaya başlamadıkları gibi ayrıntılar izlenebilir. 

Veri tüketicisi olarak, Azure aboneliğinizde tetiklenen anlık görüntüleri izleyebilirsiniz. 

## <a name="monitor-invitation-status"></a>Davet durumunu izleme

Gönderilen paylaşımlar -> Davetleri'nde gezinerek veri paylaşım davetlerinizin durumunu görüntüleyin. 

![Davet durumu](./media/invitation-status.png "Davet durumu") 

Davetinizin şu üç durumda olabileceği vardır:

* Beklemede - Veri paylaşımı alıcısı henüz daveti kabul etmedi.
* Kabul edildi - Veri paylaşımı alıcısı daveti kabul etti.
* Reddedildi - Veri paylaşımı alıcısı daveti reddetti.

> [!IMPORTANT]
> Bir daveti kabul edildikten sonra silerseniz, bu davet erişimi iptal etmekle eşdeğer değildir. Gelecekteki anlık görüntünün veri tüketicileri depolama hesabınıza kopyalanmasını engellemek istiyorsanız, *Abonelikleri Paylaş* sekmesi aracılığıyla erişimi iptal etmelisiniz. 

## <a name="monitor-share-subscriptions"></a>Paylaşım aboneliklerini izleme

Gönderilen Paylaşımlar -> Paylaşım Abonelikleri'ne göz atarak paylaşım aboneliklerinizin durumunu görüntüleyin. Bu, davetinizi kabul ettikten sonra veri tüketicileriniz tarafından oluşturulan etkin abonelikler hakkında ayrıntılı bilgi verecektir. Paylaşım aboneliğini seçerek ve *İptal'i*seçerek veri tüketicinize gelecek güncelleştirmeleri durdurabilirsiniz. 

## <a name="snapshot-history"></a>Anlık görüntü geçmişi 

Geçmiş sekmesinde, veri tüketicinizin kiracısına kopyalanan anlık görüntüleri görüntüleyebilirsiniz. Her anlık görüntü aralığının sıklığını ve süresini izleyebilirsiniz. 

![Anlık görüntü geçmişi](./media/sent-shares.png "Anlık görüntü geçmişi") 

Çalıştırma başlangıç tarihine tıklayarak çalıştırılabilen her anlık görüntü hakkında daha fazla ayrıntı görüntüleyebilirsiniz. 

Varsayılan olarak 30 güne kadar anlık görüntü geçmişi görüntülenir. 30 günden fazla bir geçmişe sahip olmanız gerekiyorsa, İzleme -> Tanılama ayarlarına gidin ve **tanıayar ını ekle'yi**seçin. Bu günlükleri depolamak için bir depolama hesabı seçmeniz gerekir. 

![Anlık görüntü geçmişi](./media/diagnostic-settings.png "Tanılama ayarları") 

## <a name="next-steps"></a>Sonraki Adımlar 

[Azure Veri Paylaşımı terminolojisi](terminology.md) hakkında daha fazla bilgi edinin