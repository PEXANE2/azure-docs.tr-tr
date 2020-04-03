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
ms.topic: conceptual
ms.date: 04/02/2020
ms.author: b-juche
ms.openlocfilehash: b21db3e842898e8ce11b560714888b946373300e
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80616428"
---
# <a name="set-up-a-capacity-pool"></a>Kapasite havuzunu ayarlama

Kapasite havuzu ayarlamak, içinde birim oluşturmanıza olanak tanır.  

## <a name="before-you-begin"></a>Başlamadan önce 

Önceden bir NetApp hesabınız olması gerekir.   

[NetApp hesabı oluşturma](azure-netapp-files-create-netapp-account.md)

## <a name="steps"></a>Adımlar 

1. NetApp hesabınız için yönetim bıçağına gidin ve ardından gezinti bölmesinden **Kapasite havuzlarını**tıklatın.  
    
    ![Kapasite havuzuna gidin](../media/azure-netapp-files/azure-netapp-files-navigate-to-capacity-pool.png)

2. Yeni kapasite havuzu oluşturmak için **+ Havuz ekle**’ye tıklayın.   
    Yeni Kapasite Havuzu penceresi görüntülenir.

3. Yeni kapasite havuzuyla ilgili aşağıdaki bilgileri sağlayın:  
   * **Adı**  
     Kapasite havuzunun adını belirtin.  
     Kapasite havuzu adı her NetApp hesabı için benzersiz olmalıdır.

   * **Hizmet düzeyi**   
     Bu alan, kapasite havuzunun hedef performansını gösterir.  
     Kapasite havuzu için servis düzeyini belirtin: [**Ultra,**](azure-netapp-files-service-levels.md#Ultra) [**Premium**](azure-netapp-files-service-levels.md#Premium)veya [**Standart**](azure-netapp-files-service-levels.md#Standard).

   * **Boyutu**     
     Satın aldığınız kapasite havuzunun boyutunu belirtin.        
     Kapasite havuzunun boyutu en az 4 TiB’dir. Havuzunuzu 4 TiB’nin katları olan büyüklüklerde oluşturabilirsiniz.   
      
     ![Yeni kapasite havuzu](../media/azure-netapp-files/azure-netapp-files-new-capacity-pool.png)

4. **Tamam**'a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar 

- [Azure NetApp Files için hizmet düzeyleri](azure-netapp-files-service-levels.md)
- Farklı hizmet düzeylerinin fiyatı için [Azure NetApp Dosyaları fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/storage/netapp/) bakın
- [Azure NetApp Files için bir alt ağı temsilci olarak belirleme](azure-netapp-files-delegate-subnet.md)
