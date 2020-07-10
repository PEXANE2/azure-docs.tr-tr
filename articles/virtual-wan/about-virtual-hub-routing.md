---
title: Sanal merkez yönlendirmesi hakkında
titleSuffix: Azure Virtual WAN
description: Bu makalede sanal hub yönlendirmeyi açıklanmaktadır
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 368440976558730bd7bb9600a0bf45c56d1ee300
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147387"
---
# <a name="about-virtual-hub-routing"></a>Sanal merkez yönlendirmesi hakkında

Bir sanal hub 'daki yönlendirme özellikleri, Sınır Ağ Geçidi Protokolü (BGP) kullanarak ağ geçitleri arasındaki tüm yönlendirmeyi yöneten bir yönlendirici tarafından sağlanır. Bir sanal hub, siteden siteye VPN ağ geçidi, ExpressRoute ağ geçidi, Noktadan siteye ağ geçidi, Azure Güvenlik Duvarı gibi birden çok ağ geçidi içerebilir. Bu yönlendirici Ayrıca, bir sanal hub 'a bağlanan sanal ağlar arasında geçiş bağlantısı sağlar ve 50 Gbps 'in toplam verimini destekleyebilir. Bu yönlendirme özellikleri standart sanal WAN müşterileri için geçerlidir.

Yönlendirmeyi yapılandırmak için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md).

## <a name="routing-concepts"></a><a name="concepts"></a>Yönlendirme kavramları

Aşağıdaki bölümlerde, sanal hub yönlendirmesinde temel kavramlar açıklanır.

> [!NOTE]
> Hub yol tablosu, ilişkilendirme, yayma ve bir sanal ağ bağlantısı içindeki statik yolların bu yeni kavramlarından bazıları yine de kullanıma sunulmakta ve 3. adımda tamamlanmayı beklenebilir.
>

### <a name="hub-route-table"></a><a name="hub-route"></a>Merkez yol tablosu

Bir sanal hub yol tablosu, bir veya daha fazla yol içerebilir. Rota, adını, etiketini, hedef türünü, hedef önekleri listesini ve bir paketin yönlendirileceği sonraki atlama bilgilerini içerir. Bir **bağlantı** genellikle bir rota tablosuna ilişkili veya yayan bir yönlendirme yapılandırmasına sahip olur

### <a name="connection"></a><a name="connection"></a>Bağlantı

Bağlantılar, yönlendirme yapılandırmasına sahip Kaynak Yöneticisi kaynaklardır. Dört bağlantı türü şunlardır:

* **VPN bağlantısı**: VPN sitesini bir sanal hub VPN Gateway 'e bağlar.
* **ExpressRoute bağlantısı**: bir ExpressRoute devresini bir sanal hub ExpressRoute ağ geçidine bağlar.
* **P2S yapılandırma bağlantısı**: BIR kullanıcı VPN (Noktadan siteye) yapılandırmasını bir sanal hub Kullanıcı VPN (Noktadan siteye) ağ geçidine bağlar.
* **Hub sanal ağ bağlantısı**: sanal ağları bir sanal hub 'a bağlar.

Kurulum sırasında bir sanal ağ bağlantısı için yönlendirme yapılandırmasını ayarlayabilirsiniz. Varsayılan olarak, tüm bağlantılar varsayılan yol tablosuna ilişkilendirir ve yayılır.

### <a name="association"></a><a name="association"></a>Kaldırma

Her bağlantı, bir yol tablosuyla ilişkilendirilir. Bir bağlantıyı bir yol tablosuyla ilişkilendirmek, trafiğin yol tablosundaki yollar olarak belirtilen hedefe gönderilmesine izin verir. Bağlantının Yönlendirme yapılandırması ilişkili yol tablosunu gösterir.  Aynı yol tablosuyla birden çok bağlantı ilişkilendirilebilir. Tüm VPN, ExpressRoute ve kullanıcı VPN bağlantıları aynı (varsayılan) yol tablosuyla ilişkilendirilir.

Varsayılan olarak, tüm bağlantılar bir sanal hub 'daki **varsayılan yol tablosuyla** ilişkilendirilir. Her sanal hub 'ın, statik yollar eklemek için düzenlenebilen kendi varsayılan yol tablosu vardır. Yollar, aynı öneklere göre dinamik olarak öğrenilen yolların önüne statik olarak eklendi.

:::image type="content" source="./media/about-virtual-hub-routing/concepts-association.png" alt-text="Kaldırma":::

### <a name="propagation"></a><a name="propagation"></a>Yayma

Bağlantılar yolları bir yol tablosuna dinamik olarak yayar. VPN bağlantısı, ExpressRoute bağlantısı veya P2S yapılandırma bağlantısıyla, rotalar sanal hub 'dan BGP kullanılarak şirket içi yönlendiriciye dağıtılır. Yollar bir veya birden çok yol tablosuna yayılamaz.

Her sanal hub için bir **yol tablosu** da kullanılabilir. None yol tablosuna yayılması, bağlantıdan yayılmaması için hiçbir yolun gerekli olmadığını gösterir. VPN, ExpressRoute ve kullanıcı VPN bağlantıları, rotaları aynı yol tabloları kümesine yayar.

:::image type="content" source="./media/about-virtual-hub-routing/concepts-propagation.png" alt-text="Yayma":::

### <a name="configuring-static-routes-in-a-virtual-network-connection"></a><a name="static"></a>Bir sanal ağ bağlantısında statik yolları yapılandırma

Statik yolları yapılandırmak, bir sonraki atlama IP 'si aracılığıyla trafiği bir sanal hub 'a bağlı olan bir bağlı ağ VNet 'te sağlanan bir ağ sanal gereci (NVA) olabilecek bir mekanizma sağlar. Statik yol, bir yol adından, hedef ön eklerin listesinden ve bir sonraki atlama IP 'si ile oluşur.

## <a name="route-tables-in-basic-and-standard-virtual-wans-prior-to-the-feature-set-of-association-and-propagation"></a><a name="route"></a>Temel ve standart sanal WAN 'Lara, Ilişkilendirme ve yayma özelliği kümesinden önce rota tabloları

Yol tablolarında ilişkilendirme ve yayma özellikleri artık vardır. Önceden var olan bir yol tablosu, bu özelliklere sahip olmayan bir yol tablosudur. Hub yönlendirmesinde önceden varolan yollar varsa ve yeni özellikleri kullanmak istiyorsanız, aşağıdakileri göz önünde bulundurun:

* **Sanal hub 'da önceden var olan yollara sahip standart sanal WAN müşterileri**:

Yeni yol tablosu yeteneklerini kullanmak için lütfen Azure 'daki dışarı aktarmak için 3. adımda bekleyin. Azure portal Hub için yönlendirme bölümünde önceden var olan yollara sahipseniz, önce bunları silmeniz ve ardından yeni rota tabloları oluşturmayı denemeniz gerekir (Azure portal Hub için yönlendirme tabloları bölümünde kullanılabilir)

* **Sanal hub 'da önceden var olan yollara sahip temel sanal WAN müşterileri**: yeni yol tablosu yeteneklerini kullanmak Için lütfen Azure 'daki dışarı aktarmak Için 3 Ağustos 'tan sonra tamamlanana kadar bekleyin. Azure portal Hub için yönlendirme bölümünde önceden mevcut olan yollar varsa öncelikle bunları silmeniz ve ardından temel sanal WAN 'ınızı standart sanal WAN 'a **yükseltmeniz** gerekir. Bkz. [bir sanal WAN 'ı temel 'Ten standart sürümüne yükseltme](upgrade-virtual-wan.md).

## <a name="next-steps"></a>Sonraki adımlar

Yönlendirmeyi yapılandırmak için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md).

Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
