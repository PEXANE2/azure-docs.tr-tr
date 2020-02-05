---
title: Azure VMware çözümleri (AVS)-ExpressRoute kullanarak şirket içi bağlantı
description: AVS bölge ağından ExpressRoute kullanarak şirket içi bir bağlantının nasıl isteneceğini açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 10a21faf2790b4c7a26d80e46bf44c8bffabf27f
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77019630"
---
# <a name="connect-from-on-premises-to-avs-using-expressroute"></a>ExpressRoute kullanarak şirket içinden AVS 'ye bağlanma

Zaten bir dış konumdan (örneğin, şirket içi) Azure 'da Azure ExpressRoute bağlantınız varsa, bunu AVS ortamınıza bağlayabilirsiniz. Bunu, iki ExpressRoute devresine bağlanmasına izin veren bir Azure özelliği aracılığıyla yapabilirsiniz. Bu yöntem, iki ortam arasında güvenli, özel, yüksek bant genişliği, düşük gecikme süreli bir bağlantı oluşturur.

[![şirket içi ExpressRoute bağlantısı Global Reach](media/cloudsimple-global-reach-connection.png)](media/cloudsimple-global-reach-connection.png)

## <a name="before-you-begin"></a>Başlamadan önce

Şirket içinden Global Reach bağlantı kurmak için bir **/29** ağ adresi bloğu gerekir. /29 adres alanı, ExpressRoute devreleri arasında transit ağı için kullanılır. Transit ağı, Azure sanal ağlarınızdan, şirket içi ağlardan veya AVS özel bulut ağlarınızla çakışmamalıdır.

## <a name="prerequisites"></a>Ön koşullar

* Devre ve AVS özel bulut ağları arasında bağlantı kurmadan önce bir Azure ExpressRoute devresi gerekir.
* Bir ExpressRoute devresinde yetkilendirme anahtarları oluşturma ayrıcalıklarına sahip bir kullanıcı gereklidir.

## <a name="scenarios"></a>Senaryolar

Şirket içi ağınızı AVS özel bulut ağınıza bağlamak, aşağıdaki senaryolar da dahil olmak üzere çeşitli yollarla AVS özel bulutunu kullanmanıza olanak sağlar:

* Siteden siteye VPN bağlantısı oluşturmadan AVS özel bulut ağınıza erişin.
* Şirket içi Active Directory, AVS özel bulutunuzda bir kimlik kaynağı olarak kullanın.
* Şirket içinde çalışan sanal makineleri AVS özel bulutuna geçirin.
* Bir olağanüstü durum kurtarma çözümünün bir parçası olarak AVS özel bulutunuzu kullanın.
* AVS özel bulut iş yükü sanal makinelerinizdeki şirket içi kaynakları kullanın.

## <a name="connecting-expressroute-circuits"></a>ExpressRoute devrelerini bağlama

ExpressRoute bağlantısı kurmak için ExpressRoute bağlantı hattınızı bir yetkilendirme oluşturmanız ve AVS 'ye yetkilendirme bilgilerini sağlamanız gerekir.


### <a name="create-expressroute-authorization"></a>ExpressRoute yetkilendirmesi oluşturma

1. Azure Portal’da oturum açın.

2. Üst arama çubuğundan **ExpressRoute** bağlantı hattını arayın ve **Hizmetler**altında **ExpressRoute devreleri** ' na tıklayın.
    [ExpressRoute devreleri ![](media/azure-expressroute-transit-search.png)](media/azure-expressroute-transit-search.png)

3. AVS ağınıza bağlanmayı düşündüğünüz ExpressRoute bağlantı hattını seçin.

4. ExpressRoute sayfasında, **yetkilendirmeler**' e tıklayın, yetkilendirme için bir ad girin ve **Kaydet**' e tıklayın.
    [ExpressRoute bağlantı hattı yetkilendirme ![](media/azure-expressroute-transit-authorizations.png)](media/azure-expressroute-transit-authorizations.png)

5. Kopyala simgesine tıklayarak kaynak KIMLIĞI ve yetkilendirme anahtarı ' nı kopyalayın. KIMLIĞI ve anahtarı bir metin dosyasına yapıştırın.
    [ExpressRoute bağlantı hattı yetkilendirme kopyasını ![](media/azure-expressroute-transit-authorization-copy.png)](media/azure-expressroute-transit-authorization-copy.png)

    > [!IMPORTANT]
    > **Kaynak kimliği** , kullanıcı arabiriminden kopyalanmalı ve desteklemek için sağladığınız biçimde ```/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/expressRouteCircuits/<express-route-circuit-name>``` biçiminde olmalıdır.

6. Oluşturulacak bağlantı <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">desteğiyle</a> bir bilet dosyası oluşturma.
    * Sorun türü: **Teknik**
    * Abonelik: **AVS hizmetinin dağıtıldığı abonelik**
    * Service: **VMware çözümleri (AVS)**
    * Sorun türü: **hizmet isteği**
    * Sorun alt türü: **Şirket içi Için ExpressRoute bağlantısı oluşturma**
    * Ayrıntılar bölmesinde kopyaladığınız ve kaydettiğiniz kaynak KIMLIĞI ve yetkilendirme anahtarını sağlayın.
    * Transit ağı için bir/29 ağ adresi alanı belirtin.
    * ExpressRoute aracılığıyla varsayılan yol gönderiyor musunuz?
    * AVS özel bulut trafiği ExpressRoute üzerinden gönderilen varsayılan yolu kullanmalıdır mi?

    > [!IMPORTANT]
    > Varsayılan yol gönderme, şirket içi internet bağlantınızı kullanarak AVS özel buluttan tüm internet trafiğini göndermenizi sağlar. AVS özel bulutu 'nda yapılandırılmış varsayılan yolu devre dışı bırakmak ve şirket içi bağlantı varsayılan yolunu kullanmak için destek bileti içindeki ayrıntıları sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure ağ bağlantıları hakkında daha fazla bilgi](cloudsimple-azure-network-connection.md)  
