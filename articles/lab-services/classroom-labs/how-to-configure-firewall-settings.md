---
title: Azure Lab Services için güvenlik duvarı ayarları
description: Güvenlik duvarı kurallarına eklenmek üzere bir laboratuvardaki sanal makinelerin genel IP adresini ve bağlantı noktası numarası aralığını belirlemeyi öğrenin.
author: emaher
ms.author: enewman
ms.date: 12/12/2019
ms.topic: article
ms.service: lab-services
ms.openlocfilehash: da1614e4a3e02ed91ef2d3c59ac4eb3eac0dcc7c
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75692777"
---
# <a name="firewall-settings-for-azure-lab-services"></a>Azure Lab Services için güvenlik duvarı ayarları

Her bir kuruluş veya okul, kendi ağını gereksinimlerine en uygun şekilde ayarlar.  Bazen bu, kendi ağı dışındaki makinelere Uzak Masaüstü Protokolü (RDP) veya Secure Shell (SSH) bağlantılarını engelleyen güvenlik duvarı kurallarını ayarlamayı içerir.  Azure Lab Services ortak bulutta çalıştığından, öğrencilerin kampüs ağdan bağlanırken VM 'lerine erişmesine izin vermek için bazı ek yapılandırmalar gerekebilir.

Her laboratuvar tek genel IP adresi ve birden çok bağlantı noktası kullanır.  Hem şablon VM hem de öğrenci VM 'Leri olan tüm VM 'Ler, bu genel IP adresini kullanacaktır.  Genel IP adresi, laboratuvarın ömrü boyunca değişmeyecektir.  Ancak, her VM farklı bir bağlantı noktası numarasına sahip olur.  Bağlantı noktası numaraları 49152 ile 65535 arasında değişir.  Genel IP adresi ve bağlantı noktası numarası birleşimi, eğitmeni ve öğrencileri doğru VM 'ye bağlamak için kullanılır.  Bu makalede, bir laboratuvar tarafından kullanılan belirli genel IP adresinin nasıl bulunacağı ele alınacaktır.  Bu bilgiler, öğrencilerin VM 'lerine erişebilmesi için gelen ve giden güvenlik duvarı kurallarını güncelleştirmek üzere kullanılabilir.

>[!IMPORTANT]
>Her laboratuvarın farklı bir genel IP adresi olur.

## <a name="find-public-ip-for-a-lab"></a>Laboratuvar için genel IP bulma

Her bir laboratuvarın genel IP adresleri, Laboratuvar Hizmetleri Laboratuvarı hesabının **Tüm laboratuvarları** dikey penceresinde listelenir.  **Tüm Labs** dikey penceresini bulma hakkında yönergeler için bkz. [Laboratuvar hesabındaki laboratuvarları yönetme](how-to-manage-lab-accounts.md#view-and-manage-labs-in-the-lab-account).  

> [!div class="mx-imgBorder"]
> Tüm Labs dikey penceresi ![](../media/how-to-configure-firewall-settings/all-labs-properties.png)

>[!NOTE]
>Laboratuvarınızın şablon makinesi henüz yayınlanmamışsa genel IP adresini görmezsiniz.

## <a name="conclusion"></a>Sonuç

Artık laboratuvarın genel IP adresini biliyoruz.  Genel IP adresi ve 49152-65535 numaralı bağlantı noktası aralığı için kuruluşun güvenlik duvarı için gelen ve giden kuralları oluşturulabilir.  Kurallar güncelleştirildikten sonra, öğrenciler ağ güvenlik duvarı erişimi engellenmeksizin VM 'lerine erişebilir.
