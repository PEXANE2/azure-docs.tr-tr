---
title: 'Senaryo: any-any'
titleSuffix: Azure Virtual WAN
description: Any ile herhangi bir yönlendirme için senaryolar
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 08/03/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: 95fa7a8c6abd0ad65b367cacef15b8faa16da640
ms.sourcegitcommit: 1b2d1755b2bf85f97b27e8fbec2ffc2fcd345120
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87553449"
---
# <a name="scenario-any-to-any"></a>Senaryo: any-any

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Dilediğiniz bir senaryoda, herhangi bir bağlı bileşen başka bir bağlı bileşene erişebilir. Birden çok hub mevcut olduğunda, standart sanal WAN 'da hub-hub arası yönlendirme (hub 'lar olarak da bilinir) varsayılan olarak etkindir. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Tasarım

Bir sanal WAN senaryosunda kaç yol tablosunun gerekli olacağını anlamak için, her hücrenin bir kaynağın (satır) bir hedefle (sütun) iletişim kurup kuramayacağını temsil ettiği bir bağlantı matrisi oluşturabilirsiniz. Bu senaryodaki bağlantı matrisi önemsiz, ancak diğer senaryolarla tutarlı olması için bu senaryoyu sunuyoruz.

| Kaynak |   Amaç |  *Sanal ağlar* | *Dallar* |
| -------------- | -------- | ---------- | ---|
| Sanal ağlar     | &#8594;|      X     |     X    |
| Dallar   | &#8594;|    X     |     X    |

Önceki tabloda bulunan hücrelerden her biri, bir sanal WAN bağlantısının (akışın "Kimden" tarafı, tablodaki satır başlıkları), belirli bir trafik akışı için bir hedef ön eki (akışın "Kimden" tarafı, tablodaki sütun başlıkları) öğrenip öğrenmediğini açıklar.

Sanal ağların ve dalların (VPN, ExpressRoute ve kullanıcı VPN) tüm bağlantıları aynı bağlantı gereksinimlerine sahip olduğundan, tek bir yol tablosu gereklidir. Sonuç olarak, tüm bağlantılar ilişkilendirilir ve varsayılan yol tablosuna aynı rota tablosuna yayılır:

* Sanal ağlar:
  * İlişkili yol tablosu: **varsayılan**
  * Yol tablolarına yayma: **varsayılan**
* Dallar
  * İlişkili yol tablosu: **varsayılan**
  * Yol tablolarına yayma: **varsayılan**

Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="architecture"></a><a name="architecture"></a>Mimari

**Şekil 1**' de tüm sanal ağlar ve dallar (VPN, ExpressRoute, P2S) birbirine ulaşabilir. Bir sanal hub 'da bağlantılar aşağıdaki gibi çalışır:

* VPN bağlantısı VPN sitesini VPN ağ geçidine bağlar.
* Sanal ağ bağlantısı, sanal bir ağı bir sanal hub 'a bağlar. Sanal hub 'ın yönlendiricisi, sanal ağlar arasında aktarım işlevi sağlar.
* ExpressRoute bağlantısı, bir ExpressRoute bağlantı hattını ExpressRoute bağlantı hattına bağlar.

Bu bağlantılar (varsayılan olarak, oluşturma sırasında), bağlantının yönlendirme yapılandırmasını **hiçbiri**veya özel bir yol tablosu olarak ayarlamadığınız takdirde varsayılan yol tablosuyla ilişkilendirilir. Bu bağlantılar, varsayılan olarak varsayılan yol tablosuna aynı zamanda yolları da yayar. Bu, herhangi bir bağlı bileşen (VNet, VPN, ER, P2S) birbirlerine ulaşabulabildiği herhangi bir senaryoya olanak sağlar.

**Şekil 1**

:::image type="content" source="./media/routing-scenarios/any-any/figure-1.png" alt-text="Şekil 1":::

## <a name="workflow"></a><a name="workflow"></a>Akışıyla

Bu senaryo, standart sanal WAN için varsayılan olarak etkinleştirilmiştir. Şube dalı için ayarı WAN yapılandırmasında devre dışıysa, bu, dal arasında bağlantı yapılmasına izin vermez. VPN/ExpressRoute/Kullanıcı VPN, sanal WAN 'da dal olarak kabul edilir

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
