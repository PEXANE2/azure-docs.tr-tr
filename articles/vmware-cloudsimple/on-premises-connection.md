---
title: CloudSimple ile Azure VMware Çözümü - ExpressRoute kullanarak şirket içi bağlantı
description: CloudSimple bölge ağındaki ExpressRoute'u kullanarak şirket içi bağlantı isteğinde bulunmayı açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0dd5ede110255b6e53bbc397e683e66b3beffc65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77019630"
---
# <a name="connect-from-on-premises-to-cloudsimple-using-expressroute"></a>ExpressRoute'u kullanarak şirket içinde CloudSimple'a bağlanın

Harici bir konumdan (şirket içi gibi) Azure'a zaten bir Azure ExpressRoute bağlantınız varsa, bu bağlantıyı CloudSimple ortamınıza bağlayabilirsiniz. Bunu, iki ExpressRoute devresi'nin birbiriyle bağlanmasına olanak tanıyan bir Azure özelliği aracılığıyla yapabilirsiniz. Bu yöntem, iki ortam arasında güvenli, özel, yüksek bant genişliği, düşük gecikme süresi bağlantısı kurar.

[![Şirket Içi ExpressRoute Bağlantısı - Global Reach](media/cloudsimple-global-reach-connection.png)](media/cloudsimple-global-reach-connection.png)

## <a name="before-you-begin"></a>Başlamadan önce

Şirket içinde Global Reach bağlantısı kurmak için **bir /29** ağ adresi bloğu gereklidir.  /29 adres alanı ExpressRoute devreleri arasındaki geçiş ağı için kullanılır.  Geçiş ağı, Azure sanal ağlarınız, şirket içi ağlarınız veya CloudSimple Private Cloud ağlarınızla örtüşmemelidir.

## <a name="prerequisites"></a>Ön koşullar

* Devre ile CloudSimple Private Cloud ağları arasındaki bağlantıyı kuradan önce bir Azure ExpressRoute devresi gereklidir.
* Bir kullanıcının ExpressRoute devresi üzerinde yetkilendirme anahtarları oluşturmak için ayrıcalıkları vardır.

## <a name="scenarios"></a>Senaryolar

Şirket içi ağınızı Özel Bulut ağınıza bağlamak, Özel Bulut'u aşağıdaki senaryolar da dahil olmak üzere çeşitli şekillerde kullanmanıza olanak tanır:

* Siteden Siteye VPN bağlantısı oluşturmadan Özel Bulut ağınıza erişin.
* Şirket içi Active Directory'nizi Özel Bulut'unuzda kimlik kaynağı olarak kullanın.
* Şirket içinde çalışan sanal makineleri Özel Bulut'unuza geçirin.
* Özel Bulut'unuzu olağanüstü durum kurtarma çözümünün bir parçası olarak kullanın.
* Özel Bulut iş yükü VM'lerinizde şirket içi kaynakları tüketin.

## <a name="connecting-expressroute-circuits"></a>ExpressRoute devrelerini bağlama

ExpressRoute bağlantısını kurmak için, ExpressRoute devrenizde bir yetkilendirme oluşturmanız ve yetkilendirme bilgilerini CloudSimple'a sağlamanız gerekir.


### <a name="create-expressroute-authorization"></a>ExpressRoute yetkilendirmesi oluşturma

1. Azure Portal’da oturum açın.

2. Üst arama çubuğundan **ExpressRoute devresini** arayın ve **Hizmetler**altındaki **ExpressRoute devrelerini** tıklatın.
    [![ExpressRoute Devreleri](media/azure-expressroute-transit-search.png)](media/azure-expressroute-transit-search.png)

3. CloudSimple ağınıza bağlanmak istediğiniz ExpressRoute devresini seçin.

4. ExpressRoute **sayfasında, Yetkilendirmeler'i**tıklatın, yetkilendirme için bir ad girin ve **Kaydet'i**tıklatın.
    [![ExpressRoute Devre Yetkilendirmesi](media/azure-expressroute-transit-authorizations.png)](media/azure-expressroute-transit-authorizations.png)

5. Kopya simgesini tıklatarak kaynak kimliğini ve yetkilendirme anahtarını kopyalayın. Kimliği ve anahtarı bir metin dosyasına yapıştırın.
    [![ExpressRoute Devre Yetkilendirme Fotokopisi](media/azure-expressroute-transit-authorization-copy.png)](media/azure-expressroute-transit-authorization-copy.png)

    > [!IMPORTANT]
    > **Kaynak kimliği** UI'dan kopyalanmalıdır ve destek ```/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/expressRouteCircuits/<express-route-circuit-name>``` sağlamak için sağladığınızda biçimde olmalıdır.

6. Oluşturulacak bağlantı için <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">Destek</a> ile bir bilet dosya.
    * Sorun türü: **Teknik**
    * Abonelik: **CloudSimple hizmetinin dağıtıldığı abonelik**
    * Hizmet: **CloudSimple tarafından VMware Çözümü**
    * Sorun türü: **Hizmet isteği**
    * Sorun alt türü: **Şirket içi ExpressRoute bağlantısı oluşturma**
    * Kopyaladığınız ve ayrıntılar bölmesine kaydettiğiniz kaynak kimliği ve yetkilendirme anahtarını sağlayın.
    * Transit ağ için bir /29 ağ adresi alanı sağlayın.
    * ExpressRoute üzerinden varsayılan rota mı gönderiyorsunuz?
    * Özel Bulut trafiği ExpressRoute üzerinden gönderilen varsayılan rotayı kullanmalı mı?

    > [!IMPORTANT]
    > Varsayılan rota göndermek, şirket içi internet bağlantınızı kullanarak Tüm Internet trafiğini Private Cloud'dan göndermenize olanak tanır.  Özel Bulut'ta yapılandırılan varsayılan rotayı devre dışı bırakıp şirket içi bağlantı varsayılan rotasını kullanmak için destek biletindeki ayrıntıları sağlayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure ağ bağlantıları hakkında daha fazla bilgi edinin](cloudsimple-azure-network-connection.md)  
