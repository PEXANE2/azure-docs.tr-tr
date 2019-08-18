---
title: ExpressRoute kullanarak CloudSimple-on-şirket içi bağlantı tarafından Azure VMware çözümü
description: CloudSimple bölgesi ağından ExpressRoute kullanarak şirket içi bir bağlantının nasıl isteneceğini açıklar.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/14/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 3f1772384621978524539514a0d647a4921a7b20
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69564114"
---
# <a name="connect-from-on-premises-to-cloudsimple-using-expressroute"></a>ExpressRoute kullanarak şirket içinden CloudSimple 'a bağlanma

Zaten bir dış konumdan (örneğin, şirket içi) Azure 'da Azure ExpressRoute bağlantınız varsa, bunu CloudSimple ortamınıza bağlayabilirsiniz. Bunu, iki ExpressRoute devresine bağlanmasına izin veren bir Azure özelliği aracılığıyla yapabilirsiniz. Bu yöntem, iki ortam arasında güvenli, özel, yüksek bant genişliği, düşük gecikme süreli bir bağlantı oluşturur.

[![Şirket içi ExpressRoute bağlantısı-Global Reach](media/cloudsimple-global-reach-connection.png)](media/cloudsimple-global-reach-connection.png)

## <a name="prerequisites"></a>Önkoşullar

* Devre ve CloudSimple özel bulut ağları arasında bağlantı kurmadan önce bir Azure ExpressRoute devresi gerekir.
* Bir ExpressRoute devresinde yetkilendirme anahtarları oluşturma ayrıcalıklarına sahip bir kullanıcı gereklidir.

## <a name="scenarios"></a>Senaryolar

Şirket içi ağınızı özel bulut ağınıza bağlamak, özel bulutu aşağıdakiler dahil çeşitli yollarla kullanmanıza olanak sağlar:

* Özel bulut ağınıza, siteden siteye VPN bağlantısı oluşturmadan erişin.
* Şirket içi Active Directory özel bulutunuzda kimlik kaynağı olarak kullanın.
* Şirket içinde çalışan sanal makineleri özel bulutunuzda geçirin.
* Özel bulutunuzu olağanüstü durum kurtarma çözümünün bir parçası olarak kullanın.
* Özel bulut iş yükü sanal makinelerinizdeki şirket içi kaynakları kullanın.

## <a name="connecting-expressroute-circuits"></a>ExpressRoute devrelerini bağlama

ExpressRoute bağlantısı kurmak için ExpressRoute bağlantı hattınızı bir yetkilendirme oluşturmanız ve CloudSimple yetkilendirme bilgilerini sağlamanız gerekir.

### <a name="create-expressroute-authorization"></a>ExpressRoute yetkilendirmesi oluşturma

1. Azure Portal’da oturum açın.

2. Üst arama çubuğundan **ExpressRoute** bağlantı hattını arayın ve **Hizmetler**altında **ExpressRoute devreleri** ' na tıklayın.
    [![ExpressRoute devreleri](media/azure-expressroute-transit-search.png)](media/azure-expressroute-transit-search.png)

3. CloudSimple ağınıza bağlanmayı düşündüğünüz ExpressRoute bağlantı hattını seçin.

4. ExpressRoute sayfasında, **yetkilendirmeler**' e tıklayın, yetkilendirme için bir ad girin ve **Kaydet**' e tıklayın.
    [![ExpressRoute devre yetkilendirmesi](media/azure-expressroute-transit-authorizations.png)](media/azure-expressroute-transit-authorizations.png)

5. Kopyala simgesine tıklayarak kaynak KIMLIĞI ve yetkilendirme anahtarı ' nı kopyalayın. KIMLIĞI ve anahtarı bir metin dosyasına yapıştırın.
    [![ExpressRoute bağlantı hattı yetkilendirme kopyası](media/azure-expressroute-transit-authorization-copy.png)](media/azure-expressroute-transit-authorization-copy.png)

    > [!IMPORTANT]
    > **Kaynak kimliği** , kullanıcı arabiriminden kopyalanmalı ve desteklemek için sağladığınız biçimde ```/subscriptions/<subscription-ID>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/expressRouteCircuits/<express-route-circuit-name>``` olmalıdır.

6. Oluşturulacak bağlantı <a href="https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest" target="_blank">desteğiyle</a> bir bilet dosyası oluşturma.
    * Sorun türü: **Teknik**
    * Abonelik: **CloudSimple hizmetinin dağıtıldığı abonelik**
    * Hizmet: **CloudSimple tarafından VMware çözümü**
    * Sorun türü: **Hizmet isteği**
    * Sorun alt türü: **Şirket içinde ExpressRoute bağlantısı oluşturma**
    * Ayrıntılar bölmesinde kopyaladığınız ve kaydettiğiniz kaynak KIMLIĞI ve yetkilendirme anahtarını sağlayın.
