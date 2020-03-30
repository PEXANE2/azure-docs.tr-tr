---
title: Bakım bildirimleri için portalı kullanma
description: Azure'da çalışan sanal makineler için bakım bildirimlerini görüntüleyin ve portalı kullanarak self servis bakıma başlayın.
author: shants123
ms.service: virtual-machines
ms.workload: infrastructure-services
ms.topic: article
ms.date: 11/19/2019
ms.author: shants
ms.openlocfilehash: 46fcc825ac49f0181ac74e9c3e2deaea577f3329
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77115730"
---
# <a name="handling-planned-maintenance-notifications-using-the-portal"></a>Portalı kullanarak planlı bakım bildirimlerinin işlenmesi

**Bu makale, hem Linux hem de Windows çalıştıran sanal makineler için geçerlidir.**

Planlı bir [bakım](maintenance-notifications.md) dalgası zamanlandıktan sonra, etkilenen sanal makinelerin listesini kontrol edebilirsiniz. 

Azure portalını kullanabilir ve bakım için zamanlanmış VM'lere bakabilirsiniz.

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. Sol navigasyonda **Sanal Makineler'i**tıklatın.

3. Sanal Makineler bölmesinde, kullanılabilir sütunların listesini açmak için **sütunları edit** düğmesini seçin.

4. Aşağıdaki sütunları seçin ve ekleyin:

   **Bakım durumu**: VM'nin bakım durumunu gösterir. Olası değerler şunlardır:
      
      | Değer | Açıklama |
      |-------|-------------|
      | Şimdi başlat | VM, bakımı kendiniz başlatmanızı sağlayan self servis bakım penceresinde yer nizdedir. VM'nizi nasıl çalıştırabilirsiniz: Aşağıya bakın. | 
      | Zamanlanan | VM, size başlatma seçeneği sunulmayan bir bakım için zamanlanır. Bu görünümde Bakım - Zamanlanmış pencereyi seçerek veya VM'ye tıklayarak bakım penceresini öğrenebilirsiniz. | 
      | Zaten güncellendi | VM'niz zaten güncelleştirildi ve şu anda başka bir işlem yapılması gerekmez. | 
      | Daha sonra yeniden deneyin | Hiçbir başarı ile bakım başlattı. Self servis bakım seçeneğini daha sonra kullanabileceksiniz. | 
      | Şimdi yeniden deneyin | Daha önce başarısız olan kendi kendine başlatılan bir bakımı yeniden deneyebilirsiniz. | 
      | - | VM'iniz planlı bir bakım dalgasının parçası değil. |
      

   **Bakım - Self servis penceresi**: VM'lerinizde kendi kendine bakım başlatabileceğiniz zaman penceresini gösterir.
   
   **Bakım - Zamanlanan pencere**: Bakımı tamamlamak için Azure'un VM'nizi ne zaman koruyacağı zaman penceresini gösterir. 



## <a name="notification-and-alerts-in-the-portal"></a>Portaldaki bildirim ve uyarılar

Azure, abonelik sahibine ve ortak sahiplerine bir e-posta göndererek planlı bakım için bir zamanlama iletir. Azure etkinlik günlüğü uyarıları oluşturarak bu iletişime ek alıcılar ve kanallar ekleyebilirsiniz. Daha fazla bilgi için [bkz.](../azure-monitor/platform/alerts-activity-log-service-notifications.md)

**Etkinlik türünü** **Planlı bakım**ve **Hizmetleri** Sanal **Makine Ölçeği Kümeleri** ve/veya **Sanal Makineler**olarak ayarladığınızdan emin olun.

## <a name="start-maintenance-on-your-vm-from-the-portal"></a>VM'nizde Bakımı portaldan başlatın

VM ayrıntılarına bakarken, bakımla ilgili daha fazla ayrıntı görebilirsiniz.  
VM'niz planlanan bir bakım dalgasına dahil edilirse, VM ayrıntıları görünümünün üst kısmında yeni bir bildirim şeridi eklenir. Ayrıca, mümkün olduğunda bakıma başlamak için yeni bir seçenek eklenir. 


Planlanan bakım hakkında daha fazla ayrıntı içeren bakım sayfasını görmek için bakım bildirimine tıklayın. Buradan VM'nizi **bakıma başlatabileceksiniz.**

Bakıma başladıktan sonra, sanal makineniz korunacak ve bakım durumu birkaç dakika içinde sonucu yansıtacak şekilde güncellenecektir.

Self servis penceresini kaçırdıysanız, VM'niz Azure tarafından korunacağı pencereyi görmeye devam edebilirsiniz. 


## <a name="next-steps"></a>Sonraki adımlar

Ayrıca, [Azure CLI](maintenance-notifications-cli.md) veya [PowerShell'i](maintenance-notifications-powershell.md)kullanarak planlı bakımı da işleyebilirsiniz.