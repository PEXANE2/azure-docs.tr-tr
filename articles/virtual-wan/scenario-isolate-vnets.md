---
title: "Senaryo: VNET 'leri yalıtma"
titleSuffix: Azure Virtual WAN
description: Yönlendirme yalıtma sanal ağları için senaryolar
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 09/22/2020
ms.author: cherylmc
ms.custom: fasttrack-edit
ms.openlocfilehash: e5e2ce17be6d8a1fa82d8a92b9b788f0bd2a37b8
ms.sourcegitcommit: 6906980890a8321dec78dd174e6a7eb5f5fcc029
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/22/2020
ms.locfileid: "92424736"
---
# <a name="scenario-isolating-vnets"></a>Senaryo: VNET 'leri yalıtma

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu senaryoda, hedef, VNET 'lerin başka bir şekilde iletişime geçebilmesini engellemektir. Bu, sanal ağları yalıtma olarak bilinir. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="design"></a><a name="design"></a>Tasarım

Bu senaryoda, belirli bir sanal ağ içindeki iş yükü yalıtılmış kalır ve diğer sanal ağlarda iletişim kuramaz. Ancak, sanal ağlar tüm dallara (VPN, ER ve kullanıcı VPN) ulaşabilmesi için gereklidir. Kaç tane yol tablosunun gerekli olacağını anlamak için bir bağlantı matrisi oluşturabilirsiniz. Bu senaryo için, her hücrenin bir kaynağın (satır) bir hedefle (sütun) iletişim kurup kuramayacağını temsil ettiği aşağıdaki tablo gibi görünür:

| Kaynak |   Amaç |  *Sanal ağlar* | *Dallar* |
| -------------- | -------- | ---------- | ---|
| Sanal ağlar     | &#8594;| Direct |   Direct    |
| Dallar   | &#8594;|  Direct  |   Direct    |

Önceki tablodaki hücrelerden her biri, bir sanal WAN bağlantısının (akışın "Kimden" tarafı, satır başlıkları) bir hedef önekiyle (akışın "to" tarafı, italik olan sütun başlıkları) iletişim kuracağını açıklar. Bu senaryoda, güvenlik duvarı veya ağ sanal gereçleri yoktur, bu nedenle iletişimler doğrudan sanal WAN üzerinden akar (Bu nedenle tablodaki "doğrudan" sözcüğü).

Bu bağlantı matrisi, iki yol tablosuna çeviren iki farklı satır deseni sunar. Sanal WAN zaten varsayılan bir yol tablosuna sahiptir, bu nedenle başka bir yol tablosu gerekecektir. Bu örnekte, yol tablosu **RT_VNET**olarak adı vereceğiz.

VNET 'ler, bu **RT_VNET** yol tablosuyla ilişkilendirilir. Dallara bağlanmaları gerektiğinden, dalların **RT_VNET** yayılması gerekir (Aksi halde sanal ağlar dal öneklerini öğrenmez). Dallar her zaman varsayılan yol tablosuyla ilişkilendirildiğinden, sanal ağların varsayılan yol tablosuna yayılması gerekir. Sonuç olarak, bu son tasarımdır:

* Sanal ağlar:
  * İlişkili yol tablosu: **RT_VNET**
  * Yol tablolarına yayma: **varsayılan**
* Dallar
  * İlişkili yol tablosu: **varsayılan**
  * Yol tablolarına yayma: **RT_VNET** ve **varsayılan**

Yalnızca dalların yol tablosuna yayıldığından **RT_VNET**, bu, diğer VNET 'lerin öğrendiğine yönelik tek ön eklerin olduğunu fark eder.

Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="workflow"></a><a name="workflow"></a>İş akışı

Bu senaryoyu yapılandırmak için aşağıdaki adımları göz önünde bulundurun:

1. Her hub 'da özel bir yol tablosu oluşturun. Örnekte, yol tablosu **RT_VNet**. Bir yol tablosu oluşturmak için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md). Yol tabloları hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
2. **RT_VNet** yol tablosunu oluşturduğunuzda, aşağıdaki ayarları yapılandırın:

   * **İlişkilendirme**: yalıtmak istediğiniz sanal ağları seçin.
   * **Yayma**: dallara yönelik seçeneği belirleyin, diğer dal (VPN/er/P2S) bağlantıları bu yol tablosuna rotaları yayacaktır.

:::image type="content" source="./media/routing-scenarios/isolated/isolated-vnets.png" alt-text="Yalıtılmış VNET 'ler":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
