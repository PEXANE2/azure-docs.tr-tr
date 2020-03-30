---
title: Azure Linux Sanal Makineler için güvenlik kontrolleri - Linux
description: Azure Linux Sanal Makinelerini değerlendirmek için güvenlik denetimleri listesi
services: virtual-machines
ms.service: virtual-machines
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 300acaf4a9d2a11ef107e19df99452c909257d54
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77190565"
---
# <a name="security-controls-for-linux-virtual-machines"></a>Linux Sanal Makineler için güvenlik kontrolleri

Bu makalede, Linux Sanal Makineler yerleşik güvenlik kontrolleri belgeler.

[!INCLUDE [Security controls header](../../../includes/security-controls-header.md)]

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
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | [Azure'da bir Linux sanal makinesini izleyin ve güncelleyin.](/azure/virtual-machines/linux/tutorial-monitoring) |
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
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | [Linux VM'leri için Azure Disk Şifrelemesi'ne](disk-encryption-overview.md)bakın. |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Azure Sanal Makineler [ExpressRoute](/azure/expressroute) ve VNet şifrelemeyi destekler. [VM'lerde aktarım şifrelemesi](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)ne bakınız. |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Müşteri tarafından yönetilen anahtarlar desteklenen bir Azure şifreleme senaryosudur; bkz. [Azure şifrelemeye genel bakış.](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms)|
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |
| API şifreli aramalar| Evet | HTTPS ve TLS üzerinden. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
