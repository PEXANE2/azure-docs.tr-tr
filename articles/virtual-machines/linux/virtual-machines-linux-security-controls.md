---
title: Azure Linux Sanal Makineleri için güvenlik denetimleri-Linux
description: Azure Linux Sanal Makineleri değerlendirmek için güvenlik denetimlerinin denetim listesi
services: virtual-machines
ms.service: virtual-machines
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 132f696dd1298384c302eeadc264e857d1edff96
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87080072"
---
# <a name="security-controls-for-linux-virtual-machines"></a>Linux Sanal Makineleri için güvenlik denetimleri

Bu makalede, Linux Sanal Makineleri yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls header](../../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yes | |
| VNet ekleme desteği| Yes | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes |  |
| Zorlamalı tünel desteği| Yes | Bkz. [Azure Resource Manager dağıtım modelini kullanarak Zorlamalı tünel yapılandırma](../../vpn-gateway/vpn-gateway-forced-tunneling-rm.md). |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Yes | Bkz. [Azure 'Da Linux sanal makinesini izleme ve güncelleştirme](./tutorial-monitor.md). |
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
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes | Bkz. [Linux VM 'leri Için Azure disk şifrelemesi](disk-encryption-overview.md). |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yes | Azure sanal makineleri [ExpressRoute](../../expressroute/index.yml) ve VNET şifrelemesini destekler. Bkz. [VM 'lerde geçiş içi şifreleme](../../security/fundamentals/encryption-overview.md#in-transit-encryption-in-vms). |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yes | Müşteri tarafından yönetilen anahtarlar desteklenen bir Azure şifreleme senaryosudur; bkz. [Azure şifrelemesi 'ne genel bakış](../../security/fundamentals/encryption-overview.md#in-transit-encryption-in-vms).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Yes | HTTPS ve TLS aracılığıyla. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
