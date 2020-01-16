---
title: Azure VPN Gateway için güvenlik denetimleri
description: Azure VPN Gateway değerlendirmek için güvenlik denetimlerinin denetim listesi
services: sql-database
author: msmbaldwin
manager: rkarlin
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: mbaldwin
ms.openlocfilehash: cdf616b29a93e786ef26af83b5d3b3541f94d67c
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "75972289"
---
# <a name="security-controls-for-azure-vpn-gateway"></a>Azure VPN Gateway için güvenlik denetimleri

Bu makale, Azure VPN Gateway yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok | |
| VNet ekleme desteği| Yok | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | VPN ağ geçitleri her müşteri sanal ağı için ayrılmış sanal makine örnekleridir  |
| Zorlamalı tünel desteği| Evet |  |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [Azure Izleyici tanılama günlükleri/uyarı](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure izleyici ölçümleri/uyarısı](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Etkinlik günlüğünü Azure Resource Manager. |
| Veri düzlemi günlüğü ve denetimi | Evet | Azure, VPN bağlantısı günlüğü ve denetimi için [tanılama günlüklerini izler](../azure-resource-manager/management/view-activity-logs.md) . |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Hizmeti yönetmek ve Azure VPN ağ geçidini yapılandırmak için [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) . |
| Yetkilendirme| Evet | [RBAC](../role-based-access-control/overview.md)aracılığıyla yetkilendirmeyi destekler. |

## <a name="data-protection"></a>Veri koruması

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yok | VPN Gateway transit müşteri verileri, müşteri verilerini depolamaz |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | VPN Gateway, Azure VPN ağ geçitleri ile müşteri şirket içi VPN cihazları (S2S) veya VPN istemcileri (P2S) arasında müşteri paketlerini şifreleyin. VPN ağ geçitleri Ayrıca VNet 'ten VNet 'e şifrelemeyi destekler. |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır | Müşteri tarafından belirtilen önceden paylaşılan anahtarlar bekleyen olarak şifrelenir; Ancak henüz CMK ile tümleştirmemelidir. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml) ve https üzerinden  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Yönetim işlemleri için bir Azure VPN ağ geçidi yapılandırmasının durumu, zaman içinde bir Azure Resource Manager şablonu ve sürümü oluşturulmuş olarak aktarılabilir. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
