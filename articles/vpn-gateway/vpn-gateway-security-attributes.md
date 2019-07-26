---
title: Azure VPN Gateway için güvenlik öznitelikleri
description: Azure VPN Gateway değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: sql-database
author: msmbaldwin
manager: barbkess
ms.service: load-balancer
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: mbaldwin
ms.openlocfilehash: c355f70975f441609304a4c9ee2ead75f0e0ce25
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444596"
---
# <a name="security-attributes-for-azure-vpn-gateway"></a>Azure VPN Gateway için güvenlik öznitelikleri

Bu makalede, Azure VPN Gateway 'da yerleşik olarak bulunan güvenlik öznitelikleri Belgelenebilir.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]


## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Yok | VPN Gateway transit müşteri verileri, müşteri verilerini depolamaz |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | VPN Gateway, Azure VPN ağ geçitleri ile müşteri şirket içi VPN cihazları (S2S) veya VPN istemcileri (P2S) arasında müşteri paketlerini şifreleyin. VPN ağ geçitleri Ayrıca VNet 'ten VNet 'e şifrelemeyi destekler. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır | Müşteri tarafından belirtilen önceden paylaşılan anahtarlar bekleyen olarak şifrelenir; Ancak henüz CMK ile tümleştirmemelidir. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml) ve https üzerinden  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok | |
| VNet ekleme desteği| Yok | . |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | VPN ağ geçitleri her müşteri sanal ağı için ayrılmış sanal makine örnekleridir  |
| Zorlamalı tünel desteği| Evet |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [Azure izleyici tanılama günlükleri/uyarı](vpn-gateway-howto-setup-alerts-virtual-network-gateway-log.md) & [Azure izleyici ölçümleri/uyarısı](vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric.md).  |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Hizmeti yönetmek ve Azure VPN ağ geçidini yapılandırmak için [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) . |
| Authorization| Evet | [RBAC](../role-based-access-control/overview.md)aracılığıyla yetkilendirmeyi destekler. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Etkinlik günlüğünü Azure Resource Manager. |
| Veri düzlemi günlüğü ve denetimi | Evet | Azure, VPN bağlantısı günlüğü ve denetimi için [tanılama günlüklerini izler](../azure-resource-manager/resource-group-audit.md) . |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Yönetim işlemleri için bir Azure VPN ağ geçidi yapılandırmasının durumu, zaman içinde bir Azure Resource Manager şablonu ve sürümü oluşturulmuş olarak aktarılabilir. | 