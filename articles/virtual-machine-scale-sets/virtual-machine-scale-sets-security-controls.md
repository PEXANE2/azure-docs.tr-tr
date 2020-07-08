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
ms.openlocfilehash: 030e2c23d68a3fbbc96dd7591583cb27b650d011
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83200004"
---
# <a name="security-controls-for-azure-virtual-machine-scale-sets"></a>Azure sanal makine ölçek kümeleri için güvenlik denetimleri

Bu makale, Azure sanal makine ölçek kümelerine yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet |  |
| Zorlamalı tünel desteği| Evet | Bkz. [Azure Resource Manager dağıtım modelini kullanarak Zorlamalı tünel yapılandırma](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. Azure ['da bir Linux sanal makinesini izleme ve güncelleştirme](/azure/virtual-machines/linux/tutorial-monitoring) , [Azure 'da bir Windows sanal makinesini izleme ve güncelleştirme](/azure/virtual-machines/windows/tutorial-monitoring). |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi | Hayır |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet |  |
| Yetkilendirme| Evet |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Evet | Bkz. [Sanal Makine Ölçek Kümeleri Için Azure disk şifrelemesi](disk-encryption-overview.md). |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Azure sanal makineleri [ExpressRoute](/azure/expressroute) ve VNET şifrelemesini destekler. Bkz. [VM 'lerde geçiş içi şifreleme](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Müşteri tarafından yönetilen anahtarlar desteklenen bir Azure şifreleme senaryosudur; bkz. [Sanal Makine Ölçek Kümeleri Için bkz. Azure disk şifrelemesi](disk-encryption-overview.md)|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| YOK | |
| Şifrelenmiş API çağrıları| Evet | HTTPS ve TLS aracılığıyla. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
