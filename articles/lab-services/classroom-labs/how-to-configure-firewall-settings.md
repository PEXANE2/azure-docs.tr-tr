---
title: Azure Lab Services için güvenlik duvarı ayarları
description: Bilgilerin güvenlik duvarı kurallarına eklenabilmesi için laboratuvardaki sanal makinelerin ortak IP adresini ve bağlantı noktası numarası aralığını nasıl belirleyeceğinizi öğrenin.
author: emaher
ms.author: enewman
ms.date: 02/14/2020
ms.topic: article
ms.service: lab-services
ms.openlocfilehash: fbd45af0c9b94f04fdaad9d9b5c8214a91a8db91
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77443473"
---
# <a name="firewall-settings-for-azure-lab-services"></a>Azure Lab Services için güvenlik duvarı ayarları

Her kuruluş veya okul kendi ağlarını kendi ihtiyaçlarına en uygun şekilde kuracak.  Bazen bu, Uzak Masaüstü Protokolü (rdp) veya Güvenli Kabuk (ssh) bağlantılarını kendi ağları dışındaki makinelere engelleyen güvenlik duvarı kurallarını ayarlamayı içerir.  Azure Lab Hizmetleri genel bulutta çalıştığından, öğrencilerin kampüs ağından bağlanırken VM'lerine erişmelerine izin vermek için bazı ekstra yapılandırmalar gerekebilir.

Her laboratuvar da tek bir genel IP adresi ve birden çok bağlantı noktası kullanır.  Tüm VM'ler, hem şablon VM hem de öğrenci VM'leri, bu genel IP adresini kullanır.  Ortak IP adresi, laboratuvar ömrü boyunca değişmez.  Ancak, her VM farklı bir bağlantı noktası numarasına sahip olacaktır.  Bağlantı noktası numaraları 49152 ile 65535 arasında değişmektedir.  Ortak IP adresi ve bağlantı noktası numarasının birleşimi, eğitmen ve öğrencileri doğru VM'ye bağlamak için kullanılır.  Bu makalede, bir laboratuvar tarafından kullanılan belirli genel IP adresi nin nasıl bulunacağı ele alınacaktır.  Bu bilgiler, öğrencilerin VM'lerine erişebilmeleri için gelen ve giden güvenlik duvarı kurallarını güncelleştirmek için kullanılabilir.

>[!IMPORTANT]
>Her laboratuvar da farklı bir genel IP adresine sahip olacaktır.

## <a name="find-public-ip-for-a-lab"></a>Laboratuvar için genel IP'yi bulma

Her laboratuvar için ortak IP adresleri, Laboratuvar Hizmetleri laboratuvar hesabının **Tüm laboratuvarlar** sayfasında listelenir.  **Tüm laboratuvarlar** sayfasını nasıl bulacağıyilgini öğrenmek için laboratuvar [hesabındaki laboratuvarları nasıl yöneteceğime](how-to-manage-lab-accounts.md#view-and-manage-labs-in-the-lab-account)bakın.  

> [!div class="mx-imgBorder"]
> ![Tüm laboratuvarlar sayfası](../media/how-to-configure-firewall-settings/all-labs-properties.png)

>[!NOTE]
>Laboratuvarınızın şablon makinesi henüz yayınlanmadıysa, herkese açık IP adresini görmezsiniz.

## <a name="conclusion"></a>Sonuç

Artık laboratuvarın halka açık IP adresini biliyoruz.  Gelen ve giden kurallar, kuruluşun genel ip adresi ve bağlantı noktası aralığı 49152-65535 için güvenlik duvarı için oluşturulabilir.  Kurallar güncelleştirildikten sonra, öğrenciler ağ güvenlik duvarı nın erişimini engellemeden VM'lerine erişebilirler.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Laboratuvar oluşturucunun laboratuvar konumunu seçmesine izin verme](allow-lab-creator-pick-lab-location.md)
- [Laboratuvarağınızı bir eş sanal ağıyla bağlayın](how-to-connect-peer-virtual-network.md)
- [Paylaşılan bir resim galerisini laboratuvara ekleme](how-to-attach-detach-shared-image-gallery.md)
- [Kullanıcıyı laboratuvar sahibi olarak ekleme](how-to-add-user-lab-owner.md)
- [Laboratuvar için güvenlik duvarı ayarlarını görüntüleme](how-to-configure-firewall-settings.md)
- [Laboratuvar için diğer ayarları yapılandırma](how-to-configure-lab-accounts.md)
