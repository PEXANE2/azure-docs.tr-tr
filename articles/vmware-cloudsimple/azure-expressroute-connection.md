---
title: Azure VMware çözümleri (AVS)-ExpressRoute kullanarak AVS özel bulutunu Azure ağı 'na bağlama
description: ExpressRoute kullanarak AVS özel bulut ortamınızı Azure sanal ağına nasıl bağlayabileceğinizi açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3d487794e219f63150142db8df4b0c1abf112947
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77015244"
---
# <a name="connect-your-avs-private-cloud-environment-to-the-azure-virtual-network-using-expressroute"></a>ExpressRoute kullanarak AVS özel bulut ortamınızı Azure sanal ağına bağlama

AVS özel bulutunuz Azure ExpressRoute kullanarak Azure sanal ağınıza bağlanabilir. Bu yüksek bant genişliği, düşük gecikmeli bağlantı, AVS özel bulut ortamınızdan Azure aboneliğinizde çalışan hizmetlere erişmenizi sağlar.

Sanal ağ bağlantısı şunları yapmanıza olanak sağlar:

* Azure 'u, AVS özel bulutunuzda sanal makineler için yedekleme hedefi olarak kullanın.
* AVS özel bulut vSAN veri deposunu şifrelemek için Azure aboneliğinizde KMS sunucuları dağıtın.
* Uygulama ve veritabanı katmanları AVS özel bulutunuzda çalışırken uygulamanın web katmanının genel bulutta çalıştığı karma uygulamaları kullanın.

![Sanal ağa Azure ExpressRoute bağlantısı](media/cloudsimple-azure-network-connection.png)

## <a name="set-up-a-virtual-network-connection"></a>Sanal ağ bağlantısı kurma

Sanal ağ bağlantısını AVS özel bulutuna ayarlamak için yetkilendirme anahtarınız, eş bağlantı devrenizin URI 'niz ve Azure aboneliğinize erişmeniz gerekir. Bu bilgiler, AVS portalındaki sanal ağ bağlantısı sayfasında bulunur. Yönergeler için bkz. [Azure sanal ağı IÇIN AVS bağlantısı için eşleme bilgileri alma](virtual-network-connection.md). Bilgileri elde etmek için bir sorun yaşıyorsanız <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">destek isteği</a>gönderebilirsiniz.

> [!TIP]
> Zaten bir Azure sanal ağınız, ağ geçidi alt ağınız ve sanal ağ geçidiniz varsa 4. adıma geçebilirsiniz.

1. Azure aboneliğinizde bir sanal ağ oluşturun ve seçtiğiniz adres alanının, AVS özel bulutunuzun adres alanından farklı olduğunu doğrulayın. Zaten bir Azure sanal ağınız varsa mevcut olanı kullanabilirsiniz. Ayrıntılar için bkz. [Azure Portal kullanarak sanal ağ oluşturma](../virtual-network/quick-create-portal.md).
2. Azure sanal ağınızda ağ geçidi alt ağını oluşturun. Azure sanal ağınızda zaten bir ağ geçidi alt ağınız varsa, mevcut olanı kullanabilirsiniz. Ayrıntılar için bkz. [ağ geçidi alt ağını oluşturma](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-gateway-subnet).
3. Sanal ağınızda sanal ağ geçidini oluşturun. Var olan bir sanal ağ geçidiniz varsa var olanı kullanabilirsiniz. Ayrıntılar için bkz. [sanal ağ geçidini oluşturma](../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md#create-the-virtual-network-gateway).
4. [Bir sanal ağı devre dışı bir aboneliğe bağlama](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#connect-a-vnet-to-a-circuit---different-subscription)bölümünde açıklandığı gibi yetkilendirme anahtarını seçerek sanal ağınız ile AVS özel bulutunuz arasında bağlantı oluşturun.

> [!WARNING]
> Var olan bir sanal ağ geçidi kullanıyorsanız ve AVS ExpressRoute bağlantı hattı ile aynı konuma bir ExpressRoute bağlantısı varsa, bağlantı kurulamayan olur. Yeni bir sanal ağ oluşturun ve önceki adımları izleyin.

## <a name="test-the-virtual-network-connection"></a>Sanal ağ bağlantısını test etme

Bağlantı oluşturulduktan sonra **Ayarlar**' ın altında **Özellikler** ' i seçerek bağlantının durumunu kontrol edebilirsiniz. Durum ve sağlama durumu **başarılı**olarak gösterilmelidir.

![Bağlantı durumu](media/azure-expressroute-connection.png)

Sanal ağ bağlantısını test etmek için:

1. Azure aboneliğinizde bir sanal makine oluşturun.
2. AVS özel bulut vCenter 'nizin IP adresini bulun (hoş geldiniz e-postanıza bakın).
3. Azure sanal ağınızda oluşturulan sanal makineden bulut vCenter 'nize ping gönderin.
4. AVS özel bulut vCenter 'unuzda çalışan bir sanal makineden Azure sanal makinenize ping gönderin.

Bağlantıyı kurarken herhangi bir sorununuz varsa, bir <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">destek isteği</a>gönderebilirsiniz.
