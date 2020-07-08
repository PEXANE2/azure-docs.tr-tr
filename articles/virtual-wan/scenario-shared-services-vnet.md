---
title: 'Senaryo: paylaşılan hizmetler sanal ağlarına yönlendirme'
titleSuffix: Azure Virtual WAN
description: Yönlendirme senaryoları-paylaşılan bir hizmet VNet 'e, her VNet ve dalın erişmesini istediğiniz bir iş yüküne erişmek için yolları ayarlayın.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 06/29/2020
ms.author: cherylmc
ms.openlocfilehash: 64bb4c85399f811c0ab7ff84b297b64734efc491
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85569258"
---
# <a name="scenario-route-to-shared-services-vnets"></a>Senaryo: paylaşılan hizmetler sanal ağlarına yönlendirme

Sanal WAN sanal hub 'ı yönlendirme ile çalışırken, kullanılabilecek oldukça az sayıda senaryo vardır. Bu senaryoda, hedef, her VNet ve dalın (VPN/ER/P2S) erişmesini istediğiniz iş yüküne sahip bir **paylaşılan hizmet** VNET 'ine erişmek için yollar ayarlamaya yönelik olur. Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).

## <a name="scenario-workflow"></a><a name="workflow"></a>Senaryo iş akışı

Bu senaryoyu yapılandırmak için aşağıdaki adımları göz önünde bulundurun:

1. **Paylaşılan hizmetler** VNET 'i tanımla.
2. Özel bir yol tablosu oluşturun. Örnekte, bu yol tablosuna **RT_SHARED**olarak başvurduk. Yol tablosu oluşturma adımları için bkz. [sanal hub yönlendirmeyi yapılandırma](how-to-virtual-hub-routing.md). Aşağıdaki değerleri bir kılavuz olarak kullanın:

   * **Kaldırma**
     * **Paylaşılan hizmetler VNET *hariç* sanal**ağlar Için, yalıtmak için sanal ağları seçin. Bu, tüm bu sanal ağların (paylaşılan hizmetler VNet hariç), RT_SHARED yol tablosunun yollarına göre hedefe ulaşabilmesi anlamına gelmez.

   * **Yayma**
      * **Dallar**için, daha önce seçmiş olduğunuz diğer yol tablolarının yanı sıra rotaları bu yol tablosuna yayın. Bu adım nedeniyle, yol tablosu RT_SHARED tüm dal bağlantılarından (VPN/ER/Kullanıcı VPN) yolları öğrenmeyecektir.
      * **Sanal**ağlar Için, **paylaşılan hizmetler VNET**' i seçin. Bu adım nedeniyle RT_SHARED yol tablosu, paylaşılan hizmetler VNet bağlantısından yolları öğrenmeyecektir.

     Bu, aşağıdaki şekilde görüldüğü gibi yönlendirme yapılandırması değişikliklerine neden olur

   :::image type="content" source="./media/routing-scenarios/shared-service-vnet/shared-services.png" alt-text="Paylaşılan hizmetler VNet":::

## <a name="next-steps"></a>Sonraki adımlar

* Sanal WAN hakkında daha fazla bilgi için bkz. [SSS](virtual-wan-faq.md).
* Sanal hub yönlendirmesi hakkında daha fazla bilgi için bkz. [sanal hub yönlendirmesi hakkında](about-virtual-hub-routing.md).
