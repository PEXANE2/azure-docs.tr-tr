---
title: Azure veri paylaşımının önizlemesini izleme
description: Azure veri paylaşımında Önizleme ' de davetiye durumunu izlemeyi, abonelikleri paylaşmayı ve anlık görüntü geçmişini nasıl izleyeceğinizi öğrenin.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 22c95f1ac541e1288494ed85cc9654d42780ea60
ms.sourcegitcommit: aef6040b1321881a7eb21348b4fd5cd6a5a1e8d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2019
ms.locfileid: "72169114"
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