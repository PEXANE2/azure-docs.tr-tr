---
title: CloudSimple ile ExpressRoute - Azure VMware Solution kullanarak Azure sanal ağını CloudSimple'a bağlayın
description: Azure sanal ağı ile CloudSimple ortamınız arasındaki bağlantı için eşleme bilgisi nin nasıl elde edilebildiğini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 6b20ee4e04a4443529ecceca8c6fc2206f7df39d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77563982"
---
# <a name="connect-azure-virtual-network-to-cloudsimple-using-expressroute"></a>ExpressRoute kullanarak Azure sanal ağını CloudSimple'a bağlayın

Özel Bulut ağınızı Azure sanal ağınıza ve Azure kaynaklarına genişletebilirsiniz. ExpressRoute bağlantısı, Azure aboneliğinizde çalışan kaynaklara Özel Bulut'unuzdan erişmenizi sağlar.

## <a name="request-authorization-key"></a>Yetkilendirme anahtarı isteyin

Özel Bulut'unuz ile Azure sanal ağı arasındaki ExpressRoute bağlantısı için yetkilendirme anahtarı gereklidir. Bir anahtar almak için <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">Destek</a>ile bir bilet dosya.  İstekte aşağıdaki bilgileri kullanın:

* Sorun türü: **Teknik**
* Abonelik: **CloudSimple hizmetinin dağıtıldığı aboneliği seçin**
* Hizmet: **CloudSimple tarafından VMware Çözümü**
* Sorun türü: **Hizmet isteği**
* Sorun alt türü: **Azure VNET bağlantısı için yetkilendirme anahtarı**
* Konu: **Azure VNET bağlantısı için yetkilendirme anahtarı isteği**

## <a name="get-peering-information-from-cloudsimple-portal"></a>CloudSimple portalından göz arama bilgileri alın

Bağlantıyı kurmak için Azure sanal ağınızla CloudSimple ortamınız arasında bir bağlantı kurmanız gerekir.  Yordamın bir parçası olarak, eş devresi URI ve yetkilendirme anahtarı sağlamanız gerekir. [CloudSimple portalından](access-cloudsimple-portal.md)URI ve yetkilendirme anahtarını edinin.  Yan menüde **Ağ'ı** seçin ve ardından **Azure Ağ Bağlantısı'nı**seçin. Veya yan menüde **Hesap'ı** seçin ve ardından **Azure ağ bağlantısını**seçin.

Kopya simgesini kullanarak eş devresi URI'yi ve her bölge için yetkilendirme anahtarını *kopyalayın.* Bağlanmak istediğiniz her CloudSimple bölgesi için:

1. URI'yi kopyalamak için **Kopyala'yı** tıklatın. Azure portalına eklemek için kullanılabilen bir dosyaya yapıştırın.  
2. Yetkilendirme **Copy** anahtarını kopyalamak ve dosyaya yapıştırmak için Kopyala'yı tıklatın.

**Kullanılabilir** durumda olan yetkilendirme anahtarını ve eş devresi URI'yi kopyalayın.  **Kullanılan** durum, anahtarın sanal ağ bağlantısı oluşturmak için zaten kullanıldığını gösterir.

![Sanal Ağ Bağlantısı sayfası](media/virtual-network-connection.png)

Azure sanal ağını CloudSimple bağlantısına ayarlama hakkında ayrıntılı bilgi için [CloudSimple Private Cloud ortamınızı ExpressRoute kullanarak Azure sanal ağına bağlayın.](azure-expressroute-connection.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure sanal ağ bağlantısı Yla Özel Bulut](azure-expressroute-connection.md)
* [Azure ExpressRoute'u kullanarak şirket içi ağa bağlanma](on-premises-connection.md)
