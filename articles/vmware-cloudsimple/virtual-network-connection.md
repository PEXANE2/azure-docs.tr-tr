---
title: ExpressRoute kullanarak Azure sanal ağını AVS 'ye bağlama
description: Azure sanal ağı ile AVS ortamınız arasındaki bir bağlantı için eşleme bilgilerinin nasıl alınacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 1ff11bbafe6f9057ce70c799e0437691d89ccb40
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014411"
---
# <a name="connect-azure-virtual-network-to-avs-using-expressroute"></a>ExpressRoute kullanarak Azure sanal ağını AVS 'ye bağlama

AVS özel bulut ağınızı Azure sanal ağınıza ve Azure kaynaklarınıza genişletebilirsiniz. ExpressRoute bağlantısı, AVS özel bulutunuzda Azure aboneliğinizde çalışan kaynaklara erişmenize olanak tanır.

## <a name="request-authorization-key"></a>İstek yetkilendirme anahtarı

AVS özel bulutunuz ile Azure sanal ağı arasındaki ExpressRoute bağlantısı için bir yetkilendirme anahtarı gereklidir. Anahtar almak için, <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">destek</a>ile bir bilet dosyası yapın. İstekte aşağıdaki bilgileri kullanın:

* Sorun türü: **Teknik**
* Abonelik: **AVS hizmetinin dağıtıldığı aboneliği seçin**
* Service: **VMware çözümleri (AVS)**
* Sorun türü: **hizmet isteği**
* Sorun alt türü: **Azure VNet bağlantısı Için yetkilendirme anahtarı**
* Subject: **Azure VNet bağlantısı için yetkilendirme anahtarı isteği**

## <a name="get-peering-information-from-avs-portal"></a>AVS portalından eşleme bilgilerini al

Bağlantıyı kurmak için, Azure sanal ağı ile AVS ortamınız arasında bir bağlantı kurmanız gerekir. Yordamın bir parçası olarak, eşdüzey devre URI 'sini ve yetkilendirme anahtarını sağlamalısınız. [AVS PORTALıNDAN](access-cloudsimple-portal.md)URI ve yetkilendirme anahtarını edinin. Yan menüden **ağ** ' ı seçin ve ardından **Azure ağ bağlantısı**' nı seçin. Veya yan menüden **Hesap** ' ı seçin ve ardından **Azure ağ bağlantısı**' nı seçin.

*Kopya* simgesini kullanarak her bir bölgenin yetkilendirme anahtarını ve eş devre URI 'sini kopyalayın. Bağlamak istediğiniz her AVS bölgesi için:

1. URI 'yi kopyalamak için **Kopyala** ' ya tıklayın. Azure portal eklemek için kullanılabilecek bir dosyaya yapıştırın. 
2. Yetkilendirme anahtarını kopyalamak için **Kopyala** ' ya tıklayın ve dosyayı da dosyaya yapıştırın.

**Kullanılabilir** durumda olan yetkilendirme anahtarını ve eş devre dışı URI 'yi kopyalayın. **Kullanılan** durum, anahtarın zaten bir sanal ağ bağlantısı oluşturmak için kullanıldığını gösterir.

![Sanal ağ bağlantısı sayfası](media/virtual-network-connection.png)

Azure sanal ağını AVS bağlantısına ayarlama hakkında daha fazla bilgi için bkz. [ExpressRoute kullanarak AVS özel bulut ortamınızı Azure sanal ağına bağlama](azure-expressroute-connection.md).

## <a name="next-steps"></a>Sonraki adımlar

* [AVS özel bulutuna Azure sanal ağ bağlantısı](azure-expressroute-connection.md)
* [Azure ExpressRoute kullanarak şirket içi ağa bağlanma](on-premises-connection.md)
