---
title: Azure veri paylaşımının önizlemesini izleme
description: Azure veri paylaşımının önizlemesini izleme
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: fbc6847dd86b7687d477b2bae0deab1389dc8491
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827072"
---
# <a name="monitor-azure-data-share-preview"></a>Azure veri paylaşımının önizlemesini izleme 

Bu makalede, Azure veri paylaşımı önizleme kullanarak veri paylaşımlarınızı nasıl izleyebileceğiniz açıklanır. Veri sağlayıcı olarak, veri paylaşım ilişkilerinizin çeşitli yönlerini izleyebilirsiniz. Veri Tüketicileriniz veri paylaşımında davetinizi kabul etmiş olup olmadıkları ve bir paylaşma aboneliği oluşturulup oluşturulmayacağını ve verilerinizi kullanmaya başlamanızı sağlamak gibi ayrıntılar, izlemek için kullanılabilir. 

Bir veri tüketicisi olarak, Azure aboneliğinize tetiklenen anlık görüntüleri izleyebilirsiniz. 

## <a name="monitor-invitation-status"></a>Davet durumunu izleme

Gönderilen paylaşımlar-> davetlere giderek veri paylaşımı davetlerinizin durumunu görüntüleyin. 

![Davetiye durumu](./media/invitation-status.png "davetiye durumu") 

Davetinizin bulunabileceği üç durum vardır:

* Bekleyen-veri paylaşma alıcısı daveti henüz kabul etmedi.
* Kabul edilen-veri paylaşma alıcısı daveti kabul etti.
* Reddedildi-veri paylaşma alıcısı daveti reddetti.

> [!IMPORTANT]
> Zaten kabul edildikten sonra bir daveti silerseniz, erişimi iptal etme eşdeğeri değildir. Gelecekteki anlık görüntülerin veri tüketicileri depolama hesabınıza kopyalanmasını durdurmak isterseniz, *abonelikleri paylaşma* sekmesinden erişimi iptal etmeniz gerekir. 

## <a name="monitor-share-subscriptions"></a>Paylaşma aboneliklerini izleme

Gönderilen paylaşımlar-> paylaşma aboneliklerine giderek paylaşım aboneliklerinizin durumunu görüntüleyin. Bu, davetinizi kabul ettikten sonra veri Tüketicileriniz tarafından oluşturulan etkin abonelikler hakkındaki ayrıntıları verir. Paylaşma aboneliğini seçerek ve *Iptal et*' i seçerek veri tüketicinizin gelecekteki güncelleştirmelerini durdurabilirsiniz. 

## <a name="snapshot-history"></a>Anlık görüntü geçmişi 

Geçmiş sekmesinde, veri tüketicisinin kiracısına kopyalanmış olan anlık görüntüleri görüntüleyebilirsiniz. Her bir anlık görüntü aralığının sıklığını ve süresini izleyebilirsiniz. 

![Anlık görüntü geçmiş](./media/sent-shares.png "anlık görüntü geçmişi") 

Çalıştırma başlangıç tarihi ' ne tıklayarak her bir anlık görüntü çalıştırması hakkında daha fazla ayrıntı görüntüleyebilirsiniz. 

Varsayılan olarak en fazla 30 günlük anlık görüntü geçmişi görüntülenir. 30 günden fazla geçmişi görmeniz gerekiyorsa Izleme-> Tanılama Ayarları ' na gidin ve **Tanılama ayarı Ekle**' yi seçin. Bu günlükleri depolamak için bir depolama hesabı seçmeniz gerekecektir. 

![Anlık görüntü geçmişi](./media/diagnostic-settings.png "Tanılama ayarları") 

## <a name="next-steps"></a>Sonraki Adımlar 

[Azure veri paylaşma terminolojisi](terminology.md) hakkında daha fazla bilgi edinin