---
title: ExpressRoute kullanarak Azure sanal ağını CloudSimple 'a bağlama
description: Azure sanal ağı ile CloudSimple ortamınız arasındaki bir bağlantı için eşleme bilgilerinin nasıl alınacağını açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 8ba2d2fd9c943fe55e82956d022f6ba9840a550f
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69536605"
---
# <a name="connect-azure-virtual-network-to-cloudsimple-using-expressroute"></a>ExpressRoute kullanarak Azure sanal ağını CloudSimple 'a bağlama

Özel bulut ağınızı Azure sanal ağınıza ve Azure kaynaklarınıza genişletebilirsiniz. ExpressRoute bağlantısı, özel bulutunuzda Azure aboneliğinizde çalışan kaynaklara erişmenize olanak tanır.

## <a name="request-authorization-key"></a>İstek yetkilendirme anahtarı

Özel bulutunuz ve Azure sanal ağı arasındaki ExpressRoute bağlantısı için bir yetkilendirme anahtarı gereklidir. Anahtar almak için, <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">destek</a>ile bir bilet dosyası yapın.  İstekte aşağıdaki bilgileri kullanın:

* Sorun türü: **Teknik**
* Abonelik: CloudSimple hizmetinin dağıtıldığı aboneliği seçin * *
* Hizmet: **CloudSimple tarafından VMware çözümü**
* Sorun türü: **Hizmet isteği**
* Sorun alt türü: **Azure VNET bağlantısı için yetkilendirme anahtarı**
* Konu: **Azure VNET bağlantısı için yetkilendirme anahtarı isteği**

## <a name="obtain-peering-information-for-azure-virtual-network-to-cloudsimple-connection"></a>Azure sanal ağı ile CloudSimple bağlantısı arasındaki eşleme bilgilerini alın

Bağlantıyı kurmak için, Azure sanal ağı ile CloudSimple ortamınız arasında bir bağlantı kurmanız gerekir.  Yordamın bir parçası olarak, eşdüzey devre URI 'sini ve yetkilendirme anahtarını sağlamalısınız. [Cloudsimple PORTALıNDAN](access-cloudsimple-portal.md)URI ve yetkilendirme anahtarını edinin.  Yan menüden **ağ** ' ı seçin ve ardından **Azure ağ bağlantısı**' nı seçin. Veya yan menüden **Hesap** ' ı seçin ve ardından **Azure ağ bağlantısı**' nı seçin.

Eş devre URI 'SI ve her bir bölgenin yetkilendirme anahtarı için kopyalama simgeleri olduğuna dikkat edin. Bağlanmak istediğiniz her bir özel bulut için:

* URI 'yi kopyalamak için **Kopyala** ' ya tıklayın. Azure portal eklemek için kullanılabilecek bir dosyaya yapıştırın.  
* Yetkilendirme anahtarını kopyalamak için **Kopyala** ' ya tıklayın ve dosyayı da dosyaya yapıştırın.

![Sanal ağ bağlantısı sayfası](media/network-virt-conn-page.png)

Azure sanal ağını CloudSimple bağlantısına ayarlama hakkında daha fazla bilgi için bkz. [ExpressRoute kullanarak Cloudsimple özel bulut ortamınızı Azure sanal ağına bağlama](azure-expressroute-connection.md).
