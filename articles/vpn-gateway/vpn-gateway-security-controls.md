---
title: Azure VPN Ağ Geçidi için güvenlik denetimleri
description: Azure VPN Ağ Geçidi'ni değerlendirmek için güvenlik denetimleri listesi
services: sql-database
author: msmbaldwin
manager: rkarlin
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/06/2019
ms.author: mbaldwin
ms.openlocfilehash: cdf616b29a93e786ef26af83b5d3b3541f94d67c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75972289"
---
# <a name="security-controls-for-azure-vpn-gateway"></a>Azure VPN Ağ Geçidi için güvenlik denetimleri

Bu makalede, Azure VPN Ağ Geçidi'nde yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Yok | |
| VNet enjeksiyon desteği| Yok | |
| Ağ Yalıtımı ve Güvenlik Duvarı desteği| Evet | VPN ağ geçitleri her müşteri sanal ağ için özel VM örnekleridir  |
| Zorunlu tünel desteği| Evet |  |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | Bkz. [Azure MonitörÜ Tanılama Günlükleri/uyarı](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure Monitör Ölçümleri/uyarısı.](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md)  |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | Azure Kaynak Yöneticisi Etkinlik Günlüğü. |
| Veri düzlemi günlüğü ve denetimi | Evet | VPN bağlantısı günlüğü ve denetimi için [Azure Monitör Tanı Günlükleri.](../azure-resource-manager/management/view-activity-logs.md) |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Evet | Hizmeti yönetmek ve Azure VPN ağ geçidini yapılandırmak için [Azure Active Directory.](../active-directory/fundamentals/active-directory-whatis.md) |
| Yetkilendirme| Evet | RBAC üzerinden Destek [Yetkilendirmesi.](../role-based-access-control/overview.md) |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Yok | VPN ağ geçidi geçiş müşteri verileri, değil müşteri verileri depolamak |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | VPN ağ geçidi, Azure VPN ağ geçitleri ile müşteri şirket içi VPN aygıtları (S2S) veya VPN istemcileri (P2S) arasındaki müşteri paketlerini şifreler. VPN ağ geçitleri VNet-to-VNet şifrelemesini de destekler. |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır | Müşteri tarafından önceden paylaşılan anahtarlar istirahatte şifrelenir; ancak henüz CMK ile entegre değil. |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |
| API şifreli aramalar| Evet | [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml) ve HTTPS aracılığıyla  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | Yönetim işlemleri için, Azure VPN ağ geçidi yapılandırmasının durumu Azure Kaynak Yöneticisi şablonu olarak dışa aktarılabilir ve zaman içinde sürümlendirilebilir. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
