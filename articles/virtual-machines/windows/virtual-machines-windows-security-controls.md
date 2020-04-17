---
title: Azure Windows Sanal Makineleri için güvenlik denetimleri
description: Azure Windows Sanal Makinelerini değerlendirmek için güvenlik denetimleri listesi
services: virtual-machines
ms.service: virtual-machines
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 09/05/2019
ms.author: mbaldwin
ms.openlocfilehash: 569c989e8876bb7213d1469c7a70da99aa0b1e9c
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81455351"
---
# <a name="security-controls-for-windows-virtual-machines"></a>Windows Sanal Makineler için güvenlik denetimleri

Bu makalede, Windows Sanal Makineler yerleşik güvenlik denetimleri belgeler.

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
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | [Azure'da bir Windows sanal makinesini izleyin ve güncelleyin.](tutorial-monitoring.md) |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi | Hayır |  |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet |  |
| Yetkilendirme| Evet |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | [Bkz. Windows VM'deki sanal diskleri şifreleme.](/azure/virtual-machines/windows/disk-encryption-overview) |
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
