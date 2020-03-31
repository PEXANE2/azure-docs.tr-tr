---
title: Azure Sanal Makine Ölçek Kümeleri için güvenlik denetimleri
description: Azure Sanal Makine Ölçek Kümelerini değerlendirmek için güvenlik denetimleri denetim listesi
ms.service: virtual-machine-scale-sets
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: mbaldwin
ms.openlocfilehash: 4007f4adeee065fe32492d3bd16f3a06d24e7d96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190603"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Azure Sanal Makine Ölçek Kümeleri için güvenlik denetimleri

Bu makalede, Azure Sanal Makine Ölçek Kümeleri'nde yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Evet | |
| VNet enjeksiyon desteği| Evet | |
| Ağ Yalıtımı ve Güvenlik Duvarı desteği| Evet |  |
| Zorunlu tünel desteği| Evet | Bkz. [Azure Kaynak Yöneticisi dağıtım modelini kullanarak zorunlu tünel yapılandırma.](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm) |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | Azure [ve Azure'da bir Linux sanal makinesini izleyin](/azure/virtual-machines/linux/tutorial-monitoring) ve güncelleyin ve [Azure'da bir Windows sanal makinesini güncelleyin.](/azure/virtual-machines/windows/tutorial-monitoring) |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi | Hayır |  |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Evet |  |
| Yetkilendirme| Evet |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | [Sanal Makine Ölçek Kümeleri için Azure Disk Şifreleme'ye](disk-encryption-overview.md)bakın. |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Azure Sanal Makineler [ExpressRoute](/azure/expressroute) ve VNet şifrelemeyi destekler. [VM'lerde aktarım şifrelemesi](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)ne bakınız. |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Müşteri tarafından yönetilen anahtarlar desteklenen bir Azure şifreleme senaryosudur; Bkz. [Sanal Makine Ölçek Kümeleri için Azure Disk Şifrelemesi](disk-encryption-overview.md)|
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |
| API şifreli aramalar| Evet | HTTPS ve TLS üzerinden. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
