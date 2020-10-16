---
title: Ekiplerden Azure Lab Services Laboratuvarı silme
description: Ekiplerden Azure Lab Services Laboratuvarı silmeyi öğrenin.
ms.topic: article
ms.date: 10/12/2020
ms.openlocfilehash: 1d2fe73d33a88e595f42b47c1e7122dcbdfe1063
ms.sourcegitcommit: 30505c01d43ef71dac08138a960903c2b53f2499
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2020
ms.locfileid: "92094565"
---
# <a name="delete-labs-within-teams"></a>Takımlar içindeki laboratuvarları silme

Bu makalede, **Azure Lab Services** uygulamasından bir laboratuvarın nasıl silineceği gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* Azure portal [bir laboratuvar hizmetleri hesabı oluşturun](tutorial-setup-lab-account.md#create-a-lab-account) .
* [Kullanmaya başlayın ve takımlar Içinde Laboratuvar Hizmetleri Laboratuvarı oluşturun](how-to-get-started-create-lab-within-teams.md).

## <a name="delete-labs"></a>Laboratuvarları Sil

Takımlar içinde oluşturulan bir laboratuvar, laboratuvar [Hizmetleri Web sitesinde](https://labs.azure.com) , [Azure Lab Services yerleşik sınıf laboratuvarları yönetme](how-to-manage-classroom-labs.md)bölümünde açıklandığı gibi doğrudan laboratuvar silinerek silinebilir. 

Laboratuvar silme, takım silindiğinde de tetiklenir. Laboratuvarın oluşturulduğu takım silinirse, otomatik kullanıcı listesi eşitleme tetiklendikten sonra laboratuvar 24 saat sonra otomatik olarak silinir. 

> [!IMPORTANT]
> Sekmeyi silme veya uygulamayı kaldırma, laboratuvarın silinmesine neden olmaz. 

Sekme silinirse, takım üyelik listesindeki kullanıcılar, laboratuvar silme, Web sitesinde laboratuvar silinerek veya takım silinirken açıkça tetiklenmediği takdirde [Laboratuvar Hizmetleri Web sitesindeki](https://labs.azure.com) VM 'lere erişebilir. 

## <a name="next-steps"></a>Sonraki adımlar

- [Takımlara genel bakış içinde Azure Lab Services kullanma](lab-services-within-teams-overview.md)
- [Takımlar içindeki laboratuvar kullanıcı listelerini yönetme](how-to-manage-user-lists-within-teams.md)
- [Laboratuvarın takımlar içindeki VM havuzunu yönetme](how-to-manage-vm-pool-within-teams.md)
- [Takımlar içinde laboratuvar zamanlamaları oluşturma ve yönetme](how-to-create-schedules-within-teams.md)
- [Takımlar içindeki bir VM 'ye erişme – öğrenci görünümü](how-to-access-vm-for-students-within-teams.md)

