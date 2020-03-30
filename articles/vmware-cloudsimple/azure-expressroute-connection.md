---
title: CloudSimple'a göre Azure VMware Çözümü - ExpressRoute kullanarak Özel Bulut'u Azure ağına bağlayın
description: CloudSimple Private Cloud ortamınızı ExpressRoute kullanarak Azure sanal ağına nasıl bağlayabilirsiniz
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 9bb68ec68f4de646239477ceeaac50a7a33989fc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77015244"
---
# <a name="connect-your-cloudsimple-private-cloud-environment-to-the-azure-virtual-network-using-expressroute"></a>CloudSimple Private Cloud ortamınızı ExpressRoute'u kullanarak Azure sanal ağına bağlayın

CloudSimple Private Cloud'unuz Azure ExpressRoute kullanarak Azure sanal ağınıza bağlanabilir.  Bu yüksek bant genişliği, düşük gecikme sonu bağlantısı, Azure aboneliğinizde çalışan hizmetlere Özel Bulut ortamınızdan erişmenizi sağlar.

Sanal ağ bağlantısı şunları yapmanızı sağlar:

* Azure'u Özel Bulut'unuzdaki sanal makineler için yedek hedef olarak kullanın.
* Özel Bulut vSAN veri mağazanızı şifrelemek için Azure aboneliğinizde KMS sunucularını dağıtın.
* Uygulama ve veritabanı katmanları Özel Bulut'unuzda çalışırken uygulamanın web katmanının genel bulutta çalıştığı karma uygulamaları kullanın.

![Azure ExpressRoute Sanal Ağa Bağlantı](media/cloudsimple-azure-network-connection.png)

## <a name="set-up-a-virtual-network-connection"></a>Sanal ağ bağlantısı kurma

Özel Bulut'unuza sanal ağ bağlantısını kurmak için yetkilendirme anahtarınız, eş devresi URI'nize ve Azure aboneliğinize erişmeniz gerekir. Bu bilgiler CloudSimple portalındaki Sanal Ağ Bağlantısı sayfasında mevcuttur. Talimatlar için bkz. Azure [sanal ağı için CloudSimple bağlantısıiçin eşleme bilgileri edinin.](virtual-network-connection.md) Bilgileri almakta sorun yaşıyorsanız, bir <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">destek isteği</a>gönderin.

> [!TIP]
> Zaten bir Azure sanal ağınız, ağ geçidiniz ve sanal ağ ağ geçidiniz varsa, adım 4'e atlayabilirsiniz.

1. Azure aboneliğinizde bir sanal ağ oluşturun ve seçtiğiniz adres alanının Özel Bulut'un uzun adres alanından farklı olduğunu doğrulayın.  Zaten bir Azure sanal ağınız varsa, varolan ağı kullanabilirsiniz.  Ayrıntılar için Azure [portalını kullanarak sanal ağ oluşturma 'na](../virtual-network/quick-create-portal.md)bakın.
2. Azure sanal ağınızda ağ geçidi alt netini oluşturun.  Azure sanal ağınızda zaten bir ağ geçidi alt ağı varsa, varolan ağı kullanabilirsiniz. Ayrıntılar için [bkz.](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet)
3. Sanal ağınızdaki sanal ağ ağ geçidini oluşturun.  Varolan bir sanal ağ ağ ağ geçidiniz varsa, varolan ağı kullanabilirsiniz. Ayrıntılar için bkz. [sanal ağ ağ geçidi oluştur.](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-virtual-network-gateway)
4. Sanal ağı bir [devreye bağlayın'da](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#connect-a-vnet-to-a-circuit---different-subscription)açıklandığı gibi yetkilendirme anahtarını kullanabilirsiniz - farklı abonelik .

> [!WARNING]
> Varolan bir sanal ağ ağ geçidi kullanıyorsanız ve CloudSimple ExpressRoute devresi ile aynı konuma expressroute bağlantısı varsa, bağlantı kurulmaz.  Yeni bir sanal ağ oluşturun ve önceki adımları izleyin.

## <a name="test-the-virtual-network-connection"></a>Sanal ağ bağlantısını test edin

Bağlantı oluşturulduktan sonra Ayarlar **altında** **Özellikler'i** seçerek bağlantının durumunu denetleyebilirsiniz.  Durum ve Provizyon Durumu **Başarılı**göstermelidir.

![Bağlantı Durumu](media/azure-expressroute-connection.png)

Sanal ağ bağlantısını sınamak için:

1. Azure aboneliğinizde sanal bir makine oluşturun.
2. Private Cloud vCenter'ınızın IP adresini bulun (hoş geldiniz e-postanıza bakın).
3. Azure sanal ağınızda oluşturulan sanal makineden Cloud vCenter'ınızı pingleme.
4. Azure sanal makinenizi Özel Bulut vCenter'ınızda çalışan sanal bir makineden pingatın.

Bağlantıyı kurarken herhangi bir sorun varsa, bir <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">destek isteği</a>gönderin.
