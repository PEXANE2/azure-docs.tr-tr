---
title: Windows Sanal Masaüstü için hizmet uyarıları ayarlama - Azure
description: Windows Sanal Masaüstü için hizmet bildirimleri almak için Azure Hizmet Durumu nasıl ayarlayabilirsiniz?
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: tutorial
ms.date: 06/11/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 2834ba924fa9c29d955c38fbaeb45ab23e5c4e9b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79127703"
---
# <a name="tutorial-set-up-service-alerts"></a>Öğretici: Hizmet uyarıları ayarlama

Windows Sanal Masaüstü için hizmet sorunlarını ve sistem durumu danışma sorunlarını izlemek için Azure Hizmet Durumu'nü kullanabilirsiniz. Azure Hizmet Durumu sizi farklı türde uyarılarla (örneğin, e-posta veya SMS) bilgilendirebilir, sorunun etkisini anlamanıza yardımcı olabilir ve sorun giderirken sizi güncel tutabilir. Azure Hizmet Durumu, kapalı kalma süresini azaltmanıza ve kaynaklarınızın kullanılabilirliğini etkileyebilecek planlı bakım ve değişikliklere hazırlanmanıza da yardımcı olabilir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Hizmet uyarıları oluşturun ve yapılandırır.

Azure Hizmet Durumu hakkında daha fazla bilgi edinmek için [Azure Sistem Durumu Belgeleri'ne](https://docs.microsoft.com/azure/service-health/)bakın.

## <a name="prerequisites"></a>Ön koşullar

- [Öğretici: Windows Sanal Masaüstünde kiracı oluşturma](tenant-setup-azure-active-directory.md)
- [Öğretici: PowerShell ile hizmet ilkeleri ve rol atamaları oluşturun](create-service-principal-role-powershell.md)
- [Öğretici: Azure Marketi ile ana bilgisayar havuzu oluşturma](create-host-pools-azure-marketplace.md)

## <a name="create-service-alerts"></a>Hizmet uyarıları oluşturma

Bu bölümde, Azure Hizmet Durumu'nu nasıl yapılandırabileceğiniz ve Azure portalında erişebileceğiniz bildirimleri nasıl ayarladığınızı gösterir. Farklı türde uyarılar ayarlayabilir ve bunları sizi zamanında bildirmek üzere zamanlayabilirsiniz.

### <a name="recommended-service-alerts"></a>Önerilen hizmet uyarıları

Aşağıdaki sistem durumu olay türleri için hizmet uyarıları oluşturmanızı öneririz:

- **Hizmet sorunu:** Kullanıcılarınızın hizmetle bağlantılarını etkileyen veya Windows Sanal Masaüstü kiracınızı yönetebilme özelliğine sahip önemli konularda bildirimler alın.
- **Sağlık danışmanlığı:** Dikkatinizi gerektiren bildirimler alın. Bu bildirim türüne bazı örnekler aşağıda verilmiştir:
    - Sanal Makineler (VM' ler) güvenli bir şekilde açık bağlantı noktası 3389 olarak yapılandırılmamıştır
    - İşlevselliğin amortismanı

### <a name="configure-service-alerts"></a>Hizmet uyarılarını yapılandırma

Hizmet uyarılarını yapılandırmak için:

1. [Azure portalında](https://portal.azure.com/)oturum açın.
2. **Hizmet Durumu'nun** durumunu seçin.
3. Uyarılarınızı ve bildirimlerinizi ayarlamak için [hizmet bildirimlerinde etkinlik günlüğü uyarıları oluşturun'daki](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log-service-notifications?toc=%2Fazure%2Fservice-health%2Ftoc.json#alert-and-new-action-group-using-azure-portal) yönergeleri kullanın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Windows Sanal Masaüstü için hizmet sorunlarını ve sistem durumu danışma belgelerini izlemek için Azure Hizmet Durumu'nun nasıl ayarlanıp kullanılacağını öğrendiniz. Windows Sanal Masaüstü'nde nasıl oturum açabilirsiniz hakkında bilgi edinmek için Windows Sanal Masaüstü Nasıl Yap'a Bağlan'a devam edin.

> [!div class="nextstepaction"]
> [Windows 7 ve Windows 10'da Uzak Masaüstü istemcisine bağlanma](./connect-windows-7-and-10.md)
