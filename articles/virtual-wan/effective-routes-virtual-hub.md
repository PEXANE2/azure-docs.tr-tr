---
title: "Sanal hub 'ın geçerli yollarını görüntüleme: Azure sanal WAN | Microsoft Docs"
description: Azure sanal WAN 'da bir sanal hub için geçerli rotaları görüntüleme
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: how-to
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 0f5481531d23eeb579dcabe80e028ed7b482b09f
ms.sourcegitcommit: e2b36c60a53904ecf3b99b3f1d36be00fbde24fb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/24/2020
ms.locfileid: "88762275"
---
# <a name="view-virtual-hub-effective-routes"></a>Sanal merkezin geçerli yollarını görüntüleme

Sanal WAN hub 'ınızın tüm yollarını Azure portal görüntüleyebilirsiniz. Bu makalede, geçerli yolları görüntüleme adımlarında adım adım gösterilmektedir. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="select-connections-or-route-tables"></a><a name="routing"></a>Bağlantıları veya yol tablolarını seçme

1. Sanal hub 'ınıza gidip **yönlendirme**' yi seçin. Yönlendirme sayfasında, **etkin yollar**' ı seçin.
1. Açılan listeden **rota tablosu**' nu seçebilirsiniz. Yol tablosu seçeneğini görmüyorsanız, bu sanal hub 'da ayarlanmış özel veya varsayılan bir rota tablosu olmadığı anlamına gelir.

## <a name="view-output"></a><a name="output"></a>Çıktıyı görüntüle

Sayfa çıktısı aşağıdaki alanları gösterir:

* **Ön ek**: geçerli varlık tarafından bilinen adres öneki (sanal hub yönlendiricisinden öğreni)
* **Sonraki atlama türü**: sanal ağ bağlantısı, VPN_S2S_Gateway, ExpressRouteGateway, uzak hub veya Azure Güvenlik duvarı olabilir.
* **Sonraki atlama**: Bu, bir sonraki atlamanın kaynak kimliği bağlantıdır veya yalnızca geçerli hub 'ın olduğunu gösteren bağlantıyı gösterir.
* **Kaynak**: yönlendirme KAYNAĞıNıN kaynak kimliği.
* **As yolu**: BGP özniteliği as (özerk sistem) yolu, yolun eklendiği öneki bulunduğu konuma ulaşmak için geçmesi gereken her türlü as sayısını listeler.

### <a name="example"></a><a name="example"></a>Örneğinde

Aşağıdaki örnek tablodaki değerler, sanal hub bağlantısı veya yol tablosunun 10.2.0.0/24 yolunu (bir dal öneki) öğrendiklerini göstermez. **Sonraki atlama** VPN Gateway kaynak kimliği Ile VPN_S2S_Gateway **VPN sonraki atlama türü** nedeniyle yolu öğrenmiştir. **Yol kaynağı** , kaynak VPN ağ geçidi/yol tablosu/BAĞLANTıSıNıN kaynak kimliğine işaret eder. **As yolu** , dalın as yolunu gösterir.

"Yol olarak" görüntülemek için tablonun altındaki kaydırma çubuğunu kullanın.

| **Ön ek** |  **Sonraki atlama türü** | **Sonraki atlama** |  **Yol kaynağı** |**AS yolu** |
| ---        | ---                | ---          | ---               | ---         |
| 10.2.0.0/24| VPN_S2S_Gateway |/Subscriptions/ `<sub id>` /ResourceGroups/ `<resource group name>` /providers/Microsoft.Network/vpnGateways/vpngw|/Subscriptions/ `<sub id>` /ResourceGroups/ `<resource group name>` /providers/Microsoft.Network/vpnGateways/vpngw| 20000|

**Konuları**

* **Etkin yollar al** çıktısında 0.0.0.0/0 görürseniz yol, yol tablolarından birinde mevcut olur. Ancak, bu yol internet için ayarlandıysa, bağlantıda **"Enableınternetsecurity"** adlı ek bir bayrak gerekir. Bağlantı üzerinde "Enableınternetsecurity" bayrağı "false" ise, VM NIC üzerindeki etkin yol yolu göstermez.

* Bir sanal ağ bağlantısını, VPN bağlantısını veya ExpressRoute bağlantısını düzenlerken, **varsayılan yolu yay** alanı Azure sanal WAN portalında görülür. Bu alan, her zaman varsayılan olarak ExpressRoute ve VPN bağlantıları için "false" olan ve sanal ağ bağlantıları için "true" olan **Enableınternetsecurity** bayrağını gösterir.

* VM NIC üzerinde etkin yollar görüntülenirken, VM sanal bir WAN hub 'ına bağlı bir bağlı ağ içinde olduğunda sanal hub yönlendiricisini belirten bir sonraki atlamayı "sanal ağ geçidi" olarak görürseniz.

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [Sanal WAN'a Genel Bakış](virtual-wan-about.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
