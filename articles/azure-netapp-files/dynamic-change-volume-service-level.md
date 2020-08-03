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
ms.openlocfilehash: 5097a5dfa6dd9b8fd46e4bcbcee72319af51f86f
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87499373"
---
# <a name="dynamically-change-the-service-level-of-a-volume"></a>Birimin hizmet düzeyini dinamik olarak değiştirme

Birimi, birimi için istediğiniz [hizmet düzeyini](azure-netapp-files-service-levels.md) kullanan başka bir kapasite havuzuna taşıyarak mevcut bir birimin hizmet düzeyini değiştirebilirsiniz. Birime yönelik bu yerinde hizmet düzeyi değişikliği, veri geçişini gerektirmez. Ayrıca, birime erişimi etkilemez.  

Bu işlevsellik, istek üzerine iş yükü ihtiyaçlarını karşılamanıza olanak sağlar.  Daha iyi performans için, mevcut bir birimi daha yüksek bir hizmet düzeyi kullanacak şekilde değiştirebilir veya maliyet iyileştirmesi için daha düşük bir hizmet düzeyi kullanabilirsiniz. Örneğin, birim Şu anda *Standart* hizmet düzeyini kullanan bir kapasite havuzunadeyse ve birimin *Premium* hizmet düzeyini kullanmasını istiyorsanız, birimi dinamik olarak *Premium* hizmet düzeyini kullanan bir kapasite havuzuna taşıyabilirsiniz.  

Birimi taşımak istediğiniz kapasite havuzu zaten var olmalıdır. Kapasite havuzu diğer birimleri içerebilir.  Birimi yeni bir kapasite havuzuna taşımak istiyorsanız, birimi Taşımadan önce [Kapasite havuzunu oluşturmanız](azure-netapp-files-set-up-capacity-pool.md) gerekir.  

## <a name="considerations"></a>Dikkat edilmesi gerekenler

* Birim başka bir kapasite havuzuna taşındıktan sonra, önceki toplu etkinlik günlüklerine ve birim ölçümlerine artık erişemeyecektir. Birim yeni kapasite havuzu altında yeni etkinlik günlükleri ve ölçümleri ile başlatılır.

* Bir birimi daha yüksek bir hizmet düzeyinin kapasite havuzuna taşırsanız (örneğin, *Standart* düzeyinden *Premium* veya *Ultra* hizmet düzeyine geçme), birimi daha düşük bir hizmet düzeyindeki bir kapasite havuzuna yeniden taşıyabilmeniz için en az yedi gün beklemeniz gerekir (örneğin, *Ultra* *Premium* veya *Standart*arasında geçiş).  
Birimi aynı hizmet düzeyine veya daha düşük bir hizmet düzeyine sahip bir kapasite havuzuna taşırsanız, bu bekleme süresi uygulanmaz.

## <a name="register-the-feature"></a>Özelliği kaydetme

1. Bir birimi başka bir kapasite havuzuna taşıma özelliği şu anda önizleme aşamasındadır. Bu özelliği ilk kez kullanıyorsanız, özelliği kullanmadan önce kaydedin: 

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

2. Özellik kaydının durumunu denetleyin: 

    > [!NOTE]
    > **Registrationstate** , üzerinde `Registering` değişiklik yapmadan önce birkaç dakika içinde olabilir `Registered` . Devam etmeden önce durum **kaydoluncaya** kadar bekleyin.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFTierChange
    ```

## <a name="move-a-volume-to-another-capacity-pool"></a>Bir birimi başka bir kapasite havuzuna taşıma

1.  Birimler sayfasında, hizmet düzeyini değiştirmek istediğiniz birime sağ tıklayın. **Havuzu Değiştir**' i seçin.

    ![Birim ' e sağ tıklayın](../media/azure-netapp-files/right-click-volume.png)

2. Havuzu Değiştir penceresinde, birimi taşımak istediğiniz kapasite havuzunu seçin. 

    ![Havuzu Değiştir](../media/azure-netapp-files/change-pool.png)

3.  **Tamam**'a tıklayın.


## <a name="next-steps"></a>Sonraki adımlar  

* [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
