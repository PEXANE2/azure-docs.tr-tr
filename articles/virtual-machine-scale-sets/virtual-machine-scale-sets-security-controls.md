---
title: Azure sanal makine ölçek kümeleri için güvenlik denetimleri
description: Azure sanal makine ölçek kümelerini değerlendirmek için güvenlik denetimlerinin denetim listesi
author: mimckitt
ms.author: mimckitt
ms.topic: conceptual
ms.service: virtual-machine-scale-sets
ms.subservice: security
ms.date: 09/05/2019
ms.reviewer: jushiman
ms.custom: mimckitt
ms.openlocfilehash: 4480ad425d9a3953fd5779f99d27b5b6b037e61e
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87029425"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleri için güvenlik denetimleri

Bu makale, Azure sanal makine ölçek kümelerine yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yes | |
| VNet ekleme desteği| Yes | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes |  |
| Zorlamalı tünel desteği| Yes | Bkz. [Azure Resource Manager dağıtım modelini kullanarak Zorlamalı tünel yapılandırma](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md). |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Yes | Bkz. Azure ['da bir Linux sanal makinesini izleme ve güncelleştirme](../virtual-machines/linux/tutorial-monitor.md) , [Azure 'da bir Windows sanal makinesini izleme ve güncelleştirme](../virtual-machines/windows/tutorial-monitor.md). |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Yes |  |
| Veri düzlemi günlüğü ve denetimi | Hayır |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Yes |  |
| Yetkilendirme| Yes |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes | Bkz. [Sanal Makine Ölçek Kümeleri Için Azure disk şifrelemesi](disk-encryption-overview.md). |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yes | Azure sanal makineleri [ExpressRoute](../expressroute/index.yml) ve VNET şifrelemesini destekler. Bkz. [VM 'lerde geçiş içi şifreleme](../security/fundamentals/encryption-overview.md#in-transit-encryption-in-vms). |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yes | Müşteri tarafından yönetilen anahtarlar desteklenen bir Azure şifreleme senaryosudur; bkz. [Sanal Makine Ölçek Kümeleri Için bkz. Azure disk şifrelemesi](disk-encryption-overview.md)|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Yes | HTTPS ve TLS aracılığıyla. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
