---
title: Azure NetApp Files için el ile QoS kapasite havuzunu yönetme | Microsoft Docs
description: El ile QoS kapasite havuzu ayarlama ve bir kapasite havuzunu el ile QoS kullanacak şekilde değiştirme dahil olmak üzere, el ile QoS türü kullanan bir kapasite havuzunun nasıl yönetileceğini açıklar.
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
ms.date: 02/04/2021
ms.author: b-juche
ms.openlocfilehash: 566cc3b1192d632bbffb8f9ef091f291b4bcc6e6
ms.sourcegitcommit: 2817d7e0ab8d9354338d860de878dd6024e93c66
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99581166"
---
# <a name="manage-a-manual-qos-capacity-pool"></a>El ile QoS kapasite havuzu yönetme

Bu makalede, el ile QoS türünü kullanan bir kapasite havuzunun nasıl yönetileceği açıklanır.  

QoS türleriyle ilgili dikkat edilmesi gereken noktaları anlamak için, Azure NetApp Files için [Azure NetApp Files depolama hiyerarşisine](azure-netapp-files-understand-storage-hierarchy.md) ve [performans konularına](azure-netapp-files-performance-considerations.md) bakın.  

## <a name="register-the-feature"></a>Özelliği kaydetme
El ile QoS türü özelliği şu anda önizlemededir. Bu özelliği ilk kez kullanıyorsanız önce özelliği kaydetmeniz gerekir.
  
1.  Özelliği kaydedin:

    ```azurepowershell-interactive
    Register-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFFlexPool
    ```

2. Özellik kaydının durumunu denetleyin: 

    > [!NOTE]
    > **Registrationstate** , ' a `Registering` değiştirilmeden önce 60 dakikaya kadar bir durumda olabilir `Registered` . Devam etmeden önce durum **kaydoluncaya** kadar bekleyin.

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.NetApp -FeatureName ANFFlexPool
    ```
Ayrıca, [Azure CLI komutlarını](/cli/azure/feature?preserve-view=true&view=azure-cli-latest) kullanarak `az feature register` `az feature show` özelliği kaydedebilir ve kayıt durumunu görüntüleyebilirsiniz. 

## <a name="set-up-a-new-manual-qos-capacity-pool"></a>Yeni bir el ile QoS kapasite havuzu ayarlama 

El ile QoS türünü kullanarak yeni bir kapasite havuzu oluşturmak için:

1. [Kapasite havuzu ayarlama](azure-netapp-files-set-up-capacity-pool.md)bölümündeki adımları izleyin.  

2. Yeni kapasite havuzu penceresinde **El Ile QoS** türünü seçin.  

## <a name="change-a-capacity-pool-to-use-manual-qos"></a><a name="change-to-qos"></a>Bir kapasite havuzunu el ile QoS kullanacak şekilde değiştirme

El ile QoS türünü kullanmak için, şu anda otomatik QoS türünü kullanan bir kapasite havuzunu değiştirebilirsiniz.  

> [!IMPORTANT]
> Kapasite türünün el ile QoS olarak ayarlanması kalıcı bir değişiklikdir. El ile QoS tür kapasitesi aracını otomatik QoS kapasite havuzuna dönüştüremezsiniz.  
> Dönüştürme sırasında, işleme seviyeleri el ile QoS türünün birimleri için üretilen iş limitleriyle uyumlu olabilir. [Azure NetApp Files Için kaynak sınırlarına](azure-netapp-files-resource-limits.md#resource-limits)bakın.

1. NetApp hesabınız için yönetim dikey penceresinde, mevcut kapasite havuzlarını göstermek için **Kapasite havuzları** ' na tıklayın.   
 
2.  El ile QoS kullanarak değiştirmek istediğiniz kapasite havuzuna tıklayın.

3.  **QoS türünü değiştir**' e tıklayın. Ardından, **Yeni QoS türünü** **el ile** olarak ayarlayın. **Tamam**'a tıklayın. 

![QoS türünü değiştir](../media/azure-netapp-files/change-qos-type.png)


## <a name="monitor-the-throughput-of-a-manual-qos-capacity-pool"></a>El ile QoS kapasite havuzunun verimini izleme  

Ölçüm, bir birimin okuma ve yazma verimini izlemenize yardımcı olmak için kullanılabilir.  [Azure NetApp Files Için ölçümleri](azure-netapp-files-metrics.md)görüntüleyin.  

## <a name="modify-the-allotted-throughput-of-a-manual-qos-volume"></a>El ile QoS biriminin ayrılan verimini değiştirme 

Bir birim el ile QoS kapasite havuzunda yer alıyorsa, ayrılan birim aktarım hızını gerektiği gibi değiştirebilirsiniz.

1. **Birimler** sayfasında, aktarım hızını değiştirmek istediğiniz birimi seçin.   

2. **Aktarım hızını değiştir**' e tıklayın. İstediğiniz **aktarım hızını (MIB/S)** belirtin. **Tamam**'a tıklayın. 

    ![QoS aktarım hızını değiştirme](../media/azure-netapp-files/change-qos-throughput.png)

## <a name="next-steps"></a>Sonraki adımlar  

* [Kapasite havuzunu ayarlama](azure-netapp-files-set-up-capacity-pool.md)
* [Azure NetApp Files için ölçümler](azure-netapp-files-metrics.md)
* [Azure NetApp Files için performansla ilgili önemli noktalar](azure-netapp-files-performance-considerations.md)
* [Kapasite havuzu sorunlarını giderme](troubleshoot-capacity-pools.md)
* [Azure NetApp Files’ın depolama hiyerarşisi](azure-netapp-files-understand-storage-hierarchy.md)
* [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
* [Azure NetApp Files için maliyet modeli](azure-netapp-files-cost-model.md)
* [Azure NetApp Files için kaynak sınırları](azure-netapp-files-resource-limits.md)
* [NFS birimi oluşturma](azure-netapp-files-create-volumes.md)
* [SMB birimi oluşturma](azure-netapp-files-create-volumes-smb.md)
* [Çift protokollü birim oluşturma](create-volumes-dual-protocol.md)
