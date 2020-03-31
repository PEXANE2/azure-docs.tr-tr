---
title: include dosyası
description: include dosyası
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 11/12/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 8087025810214f3edbb74e628698eb69558f3500
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "74085282"
---
Bir sanal ağ geçidi oluşturduğunuzda, kullanmak istediğiniz ağ geçidi SKU’sunu belirtmeniz gerekir. İş yükü, aktarım hızı, özellik ve SLA türlerine bağlı olarak gereksinimlerinize uyan SKU’ları seçin. Azure Kullanılabilirlik Bölgelerindeki sanal ağ ağ geçidi SK'leri için [Azure Kullanılabilirlik Bölgeleri ağ geçidi SDA'larına](../articles/vpn-gateway/about-zone-redundant-vnet-gateways.md)bakın.

###  <a name="gateway-skus-by-tunnel-connection-and-throughput"></a><a name="benchmark"></a>Tünele, bağlantıya ve performansa göre Ağ Geçidi SKU’ları

[!INCLUDE [Aggregated throughput by SKU](./vpn-gateway-table-gwtype-aggtput-include.md)]

> [!NOTE]
> VpnGw SKUs (VpnGw1, VpnGw1AZ, VpnGw2, VpnGw2, VpnGw3, VpnGw3AZ, VpnGw4, VpnGw4AZ, VpnGw5 ve VpnGw5AZ) kaynak yöneticisi dağıtım modeli için desteklenir. Klasik sanal ağlar eski (eski) SNU'ları kullanmaya devam etmelidir.
>  * Eski ağ geçidi SNU'larla (Temel, Standart ve Yüksek Performans) çalışma hakkında daha fazla bilgi için VPN [ağ geçidi SNU'ları (eski SUS'lar) ile çalışma](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md)bilgisine bakın.
>  * ExpressRoute ağ geçidi SK'leri [için ExpressRoute için Sanal Ağ Ağ Geçitleri'ne](../articles/expressroute/expressroute-about-virtual-network-gateways.md)bakın.
>

###  <a name="gateway-skus-by-feature-set"></a><a name="feature"></a>Özellik kümesine göre Ağ Geçidi SUS'ları

Yeni VPN ağ geçidi STU'lar ağ geçitlerinde sunulan özellik kümelerini kolaylaştırın:

| **Sku**| **Özellik**|
| ---    | ---         |
|**Temel** (**)   | **Rota tabanlı VPN**: S2S/bağlantılar için 10 tünel; P2S için RADIUS kimlik doğrulaması yok; P2S için IKEv2 yok<br>**Politika tabanlı VPN**: (IKEv1): 1 S2S/bağlantı tüneli; P2S yok|
| **Temel Hariç Tüm Nesil1 ve Generation2 SK'lar** | **Rota tabanlı VPN**: en fazla 30 tünel (*), P2S, BGP, aktif-aktif, özel IPsec/IKE ilkesi, ExpressRoute/VPN birlikteliği |
|        |             |

(*) "PolicyBasedTrafficSelectors"u, rota tabanlı bir VPN ağ geçidini şirket içi ilke tabanlı birden çok güvenlik duvarı aygıtına bağlamak için yapılandırabilirsiniz. Ayrıntılı bilgi için bkz. [PowerShell kullanarak VPN ağ geçitlerini şirket içi ilke tabanlı birden fazla VPN cihazına bağlama](../articles/vpn-gateway/vpn-gateway-connect-multiple-policybased-rm-ps.md).

(\*\*) Temel SKU bir miras SKU olarak kabul edilir. Temel SKU'nun belirli özellik sınırlamaları vardır. Temel SKU kullanan bir ağ geçidini yeni ağ geçidi SKU'larından birine yeniden boyutlandıramaz, bunun yerine VPN ağ geçidinizi silmeyi ve yeniden oluşturmayı içeren yeni bir SKU'ya geçiş yapmalısınız.

###  <a name="gateway-skus---production-vs-dev-test-workloads"></a><a name="workloads"></a>Ağ Geçidi SKUs 'ları - Üretim ve Dev-Test İş Yükleri

SLA'lardaki ve özellik kümelerindeki farklılıklar nedeniyle üretim ve geliştirme-test iş yükleri için aşağıdaki farklı  SKU'ları öneririz:

| **İş yükü**                       | **SKU'lar**               |
| ---                                | ---                    |
| **Üretim, kritik iş yükleri** | Temel Hariç Tüm Nesil1 ve Generation2 SK'lar |
| **Geliştirme-test veya kavram kanıtı**   | Temel (**)                 |
|                                    |                        |

(\*\*) Temel SKU bir eski SKU olarak kabul edilir ve özellik sınırlamaları vardır. Temel SKU'yu kullanmadan önce gereksinim duyduğunuz özelliğin desteklendiğini doğrulayın.

Eski SKU'ları (eski) kullanıyorsanız, üretim SKU önerileri Standart ve Yüksek Performans'tir. Eski SK'ler hakkında bilgi ve talimatlar için [ağ geçidi SK'leri (eski)](../articles/vpn-gateway/vpn-gateway-about-skus-legacy.md)bakın.
