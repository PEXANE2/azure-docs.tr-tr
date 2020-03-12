---
title: Windows sanal masaüstü için hizmet uyarılarını ayarlama-Azure
description: Azure hizmet durumunu Windows sanal masaüstü için hizmet bildirimleri alacak şekilde ayarlama.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2834ba924fa9c29d955c38fbaeb45ab23e5c4e9b
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79127703"
---
# <a name="tutorial-set-up-service-alerts"></a>Öğretici: hizmet uyarılarını ayarlama

Windows sanal masaüstü için hizmet sorunlarını ve sistem durumu belgelerini izlemek üzere Azure hizmet durumu ' nu kullanabilirsiniz. Azure hizmet durumu, farklı Uyarı türleri (örneğin, e-posta veya SMS) ile size bildirimde bulunabilir, bir sorunun etkisini anlamanıza yardımcı olur ve sorun çözümlendiği sürece güncelleştirmiş olursunuz. Azure hizmet durumu, kapalı kalma süresini azaltmanıza ve kaynaklarınızın kullanılabilirliğini etkileyebilecek planlı bakım ve değişiklikler için hazırlık yapmanıza da yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet uyarıları oluşturun ve yapılandırın.

Azure hizmet durumu hakkında daha fazla bilgi edinmek için bkz. [Azure sistem durumu belgeleri](https://docs.microsoft.com/azure/service-health/).

## <a name="prerequisites"></a>Önkoşullar

- [Öğretici: Windows sanal masaüstünde kiracı oluşturma](tenant-setup-azure-active-directory.md)
- [Öğretici: PowerShell ile hizmet sorumluları ve rol atamaları oluşturma](create-service-principal-role-powershell.md)
- [Öğretici: Azure Marketi ile bir konak havuzu oluşturma](create-host-pools-azure-marketplace.md)

## <a name="create-service-alerts"></a>Hizmet uyarıları oluşturma

Bu bölümde, Azure hizmet durumunu yapılandırma ve Azure portal erişebileceğiniz bildirimlerin nasıl ayarlanacağı gösterilmektedir. Farklı Uyarı türleri ayarlayabilir ve bunları zamanında bilgilendirecek şekilde zamanlayabilirsiniz.

### <a name="recommended-service-alerts"></a>Önerilen hizmet uyarıları

Aşağıdaki sistem durumu olay türleri için hizmet uyarıları oluşturmanızı öneririz:

- **Hizmet sorunu:** Hizmet ile kullanıcılarınızın bağlantısını etkileyen veya Windows sanal masaüstü kiracınızı yönetme özelliği ile ilgili önemli sorunlar hakkında bildirim alın.
- **Durum danışmanlık:** Dikkat etmeniz gereken bildirimleri alın. Aşağıda bu tür bir bildirime örnek verilmiştir:
    - Sanal makineler (VM) açık bağlantı noktası 3389 olarak güvenli yapılandırılmamış
    - İşlevin kullanımdan kaldırılması

### <a name="configure-service-alerts"></a>Hizmet uyarılarını yapılandırma

Hizmet uyarılarını yapılandırmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Hizmet durumunu seçin.**
3. Uyarı ve bildirimlerinizi ayarlamak için [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturma](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal) ' daki yönergeleri kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Windows sanal masaüstü için hizmet sorunlarını ve sistem durumu belgelerini izlemek üzere Azure hizmet durumunu ayarlamayı ve kullanmayı öğrendiniz. Windows sanal masaüstü 'nde oturum açma hakkında bilgi edinmek için Windows sanal masaüstü nasıl yapılır-TOS ' a bağlanma konusuna geçin.

> [!div class="nextstepaction"]
> [Windows 7 ve Windows 10 ' da uzak masaüstü istemcisine bağlanma](./connect-windows-7-and-10.md)
