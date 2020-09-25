---
title: Azure NetApp Files için kapasite havuzunu ayarlama | Microsoft Docs
description: İçinde birimleri oluşturabileceğiniz bir kapasite havuzunu ayarlama işlemi açıklanır.
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
ms.date: 09/22/2020
ms.author: b-juche
ms.openlocfilehash: 2b52ad50854092cddd7b9e79cbeebd4a83017081
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91325428"
---
# <a name="set-up-a-capacity-pool"></a>Kapasite havuzunu ayarlama

Kapasite havuzu ayarlamak, içinde birim oluşturmanıza olanak tanır.  

## <a name="before-you-begin"></a>Başlamadan önce 

Önceden bir NetApp hesabınız olması gerekir.   

[NetApp hesabı oluşturma](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Adımlar 

1. NetApp hesabınız için yönetim dikey penceresine gidin ve ardından Gezinti bölmesinde **Kapasite havuzları**' na tıklayın.  
    
    ![Kapasite havuzuna git](../media/azure-netapp-files/azure-netapp-files-navigate-to-capacity-pool.png)

2. Yeni kapasite havuzu oluşturmak için **+ Havuz ekle**’ye tıklayın.   
    Yeni Kapasite Havuzu penceresi görüntülenir.

3. Yeni kapasite havuzuyla ilgili aşağıdaki bilgileri sağlayın:  
   * **Ad**  
     Kapasite havuzunun adını belirtin.  
     Kapasite havuzu adı her NetApp hesabı için benzersiz olmalıdır.

   * **Hizmet düzeyi**   
     Bu alan, kapasite havuzunun hedef performansını gösterir.  
     Kapasite havuzu için hizmet düzeyini belirtin: [**Ultra**](azure-netapp-files-service-levels.md#Ultra), [**Premium**](azure-netapp-files-service-levels.md#Premium)veya [**Standart**](azure-netapp-files-service-levels.md#Standard).

    * **Boyutla**     
     Satın aldığınız kapasite havuzunun boyutunu belirtin.        
     Kapasite havuzunun boyutu en az 4 TiB’dir. Havuzunuzu 4 TiB’nin katları olan büyüklüklerde oluşturabilirsiniz.   

   * **QoS**   
     Kapasite havuzunun **el ile** veya **Otomatik** QoS türünü kullanıp kullanmayacağını belirtin.  

     QoS türlerini anlamak için bkz. [depolama hiyerarşisi](azure-netapp-files-understand-storage-hierarchy.md) ve [performans konuları](azure-netapp-files-performance-considerations.md) .  

     > [!IMPORTANT] 
     > **QoS türü** **el ile** olarak ayarlanıyor kalıcı olarak ayarlanır. El ile QoS kapasite havuzunu otomatik QoS kullanacak şekilde dönüştüremezsiniz. Ancak, bir otomatik QoS kapasite havuzunu el ile QoS kullanacak şekilde dönüştürebilirsiniz. Bkz. [bir kapasite havuzunu el Ile QoS kullanacak şekilde değiştirme](manage-manual-qos-capacity-pool.md#change-to-qos).   
     > Bir kapasite havuzu için el ile QoS türü kullanmak kayıt gerektirir. Bkz. [El Ile QoS kapasite havuzunu yönetme](manage-manual-qos-capacity-pool.md#register-the-feature). 

    ![Yeni kapasite havuzu](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. **Oluştur**’a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar 

- [Depolama hiyerarşisi](azure-netapp-files-understand-storage-hierarchy.md) 
- [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- [Azure NetApp Files fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/netapp/)
- [El ile QoS kapasite havuzunu yönetme](manage-manual-qos-capacity-pool.md)
- [Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)
