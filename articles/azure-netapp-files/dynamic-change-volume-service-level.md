---
title: Azure NetApp Files için bir birimin hizmet düzeyini dinamik olarak değiştirme | Microsoft Docs
description: Bir birimin hizmet düzeyinin dinamik olarak nasıl değiştirileceğini açıklar.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: how-to
ms.date: 07/24/2020
ms.author: b-juche
ms.openlocfilehash: bd28f949d35d38c9e64af7ff4196aa1754fbc37a
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87172676"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Bir birimin hizmet düzeyini dinamik olarak değiştirme

Birimi, birimi için istediğiniz [hizmet düzeyini](azure-netapp-files-service-levels.md) kullanan başka bir kapasite havuzuna taşıyarak mevcut bir birimin hizmet düzeyini değiştirebilirsiniz. Birime yönelik bu yerinde hizmet düzeyi değişikliği, veri geçişini gerektirmez. Ayrıca, birime erişimi etkilemez.  

> [!IMPORTANT] 
> Bu özelliğin kullanılması için beyaz liste gerekir. anffeedback@microsoft.comBu özelliği istemek için ABONELIK Kimliğiniz ile e-posta gönderin.

Bu işlevsellik, istek üzerine iş yükü ihtiyaçlarını karşılamanıza olanak sağlar.  Daha iyi performans için, mevcut bir birimi daha yüksek bir hizmet düzeyi kullanacak şekilde değiştirebilir veya maliyet iyileştirmesi için daha düşük bir hizmet düzeyi kullanabilirsiniz. Örneğin, birim Şu anda *Standart* hizmet düzeyini kullanan bir kapasite havuzunadeyse ve birimin *Premium* hizmet düzeyini kullanmasını istiyorsanız, birimi dinamik olarak *Premium* hizmet düzeyini kullanan bir kapasite havuzuna taşıyabilirsiniz.  

Birimi taşımak istediğiniz kapasite havuzu zaten var olmalıdır. Kapasite havuzu diğer birimleri içerebilir.  Birimi yeni bir kapasite havuzuna taşımak istiyorsanız, birimi Taşımadan önce [Kapasite havuzunu oluşturmanız](azure-netapp-files-set-up-capacity-pool.md) gerekir.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Birim başka bir kapasite havuzuna taşındıktan sonra, önceki toplu etkinlik günlüklerine ve birim ölçümlerine artık erişemeyecektir. Birim yeni kapasite havuzu altında yeni etkinlik günlükleri ve ölçümleri ile başlatılır.

* Bir birimi daha yüksek bir hizmet düzeyinin kapasite havuzuna taşırsanız (örneğin, *Standart* düzeyinden *Premium* veya *Ultra* hizmet düzeyine geçme), birimi daha düşük bir hizmet düzeyindeki bir kapasite havuzuna yeniden taşıyabilmeniz için en az yedi gün beklemeniz gerekir (örneğin, *Ultra* *Premium* veya *Standart*arasında geçiş).  
Birimi aynı hizmet düzeyine veya daha düşük bir hizmet düzeyine sahip bir kapasite havuzuna taşırsanız, bu bekleme süresi uygulanmaz.

## <a name="steps"></a>Adımlar

1.  Birimler sayfasında, hizmet düzeyini değiştirmek istediğiniz birime sağ tıklayın. **Havuzu Değiştir**' i seçin.

    ![Birim ' e sağ tıklayın](../media/azure-netapp-files/right-click-volume.png)

2. Havuzu Değiştir penceresinde, birimi taşımak istediğiniz kapasite havuzunu seçin. 

    ![Havuzu Değiştir](../media/azure-netapp-files/change-pool.png)

3.  **Tamam** düğmesine tıklayın.


## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
