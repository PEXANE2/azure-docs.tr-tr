---
title: 'Senaryo: trafiği bir NVA üzerinden yönlendirme'
titleSuffix: Azure Virtual WAN
description: Trafiği NVA üzerinden yönlendirme
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 0716ca8f0457ca801098c97dd7a5e68751822d4d
ms.sourcegitcommit: cec9676ec235ff798d2a5cad6ee45f98a421837b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85848095"
---
# <a name="scenario-route-traffic-through-an-nva"></a>Senaryo: trafiği bir NVA üzerinden yönlendirme

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu NVA senaryosunda, hedef, trafiği bir NVA (ağ sanal gereci) yoluyla VNet 'e ve VNet 'e Dala Dala yönlendirmektir. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="scenario-architecture"></a><a name="architecture"></a>Senaryo mimarisi

**Şekil 1**' de iki hub vardır; **Merkez 1** ve **Merkez 2**.

* **Merkez 1** ve **Merkez 2** , NVA sanal ağları **VNET 2** ve **VNET 4**' e doğrudan bağlanır.

* VNET **5** ve **VNET 6** VNET **2**ile eşlenmez.

* VNET **7** ve VNET **8** , **VNET 4**ile eşlenmez.

* **Vnetme 5, 6, 7, 8** , bir sanal hub 'a doğrudan bağlı değil dolaylı ışınsal

**Şekil 1**

:::image type="content" source="./media/routing-scenarios/nva/nva.png" alt-text="Şekil 1":::

## <a name="scenario-workflow"></a><a name="workflow"></a>Senaryo iş akışı

NVA aracılığıyla yönlendirmeyi ayarlamak için şunları göz önünde bulundurmanız gereken adımlar şunlardır:

1. NVA bağlı VNet bağlantısını tanımla. **Şekil 1**' de, **VNET 2 bağlantısı (Eastusconn)** ve **VNET 4 bağlantısı (weconn)**.

   UDRs 'nin ayarlanmış olduğundan emin olun:
   * VNET 5 ve 6-VNET 2 NVA IP 'sinden
   * VNET 7 ve 8 ile VNET 4 NVA IP 
   
   VNET 5, 6, 7, 8 ' i doğrudan sanal hub 'lara bağlamanız gerekmez. Vnetme 5, 6, 7, 8 ' deki NSG 'lerde, uzak sanal ağlara bağlı dal (VPN/ER/P2S) veya sanal ağlar için trafiğe izin verildiğinden emin olun. Örneğin, VNET 5, NSG 'lerin şirket içi adres önekleri ve sanal ağlar 7, uzak hub 2 ' ye bağlı trafik için trafiğe izin vermesinden emin olmalıdır. 

2. Vnetme 2, 5, 6 ' dan hub 1 ' in varsayılan yol tablosuna toplanan bir statik yol girişi ekleyin. 

   :::image type="content" source="./media/routing-scenarios/nva/nva-static-expand.png" alt-text="Örnek":::

3. VNET 2 ' in sanal ağ bağlantısı içinde Vnetme 5, 6 için statik bir yol yapılandırın. Bir sanal ağ bağlantısı için yönlendirme yapılandırmasını ayarlamak için bkz. [sanal hub yönlendirme](how-to-virtual-hub-routing.md#routing-configuration).

4. Vnetme 4, 7, 8 ' in hub 1 ' in varsayılan yol tablosuna bir toplu statik yol girişi ekleyin.

5. Merkez 2 ' nin varsayılan yol tablosu için 2, 3 ve 4. adımları yineleyin.

Bu, aşağıdaki şekilde görüldüğü gibi yönlendirme yapılandırması değişikliklerine neden olur

   :::image type="content" source="./media/routing-scenarios/nva/nva-result.png" alt-text="Sonuç":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
