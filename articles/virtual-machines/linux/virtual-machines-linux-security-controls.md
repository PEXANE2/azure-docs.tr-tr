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
ms.openlocfilehash: 300acaf4a9d2a11ef107e19df99452c909257d54
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190565"
---
# <a name="security-controls-for-linux-virtual-machines"></a>Linux Sanal Makineleri için güvenlik denetimleri

Bu makalede, Linux Sanal Makineleri yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls header](../../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet |  |
| Zorlamalı tünel desteği| Evet | Bkz. [Azure Resource Manager dağıtım modelini kullanarak Zorlamalı tünel yapılandırma](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [Azure 'Da Linux sanal makinesini izleme ve güncelleştirme](/azure/virtual-machines/linux/tutorial-monitoring). |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi | Hayır |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulama| Evet |  |
| Yetkilendirme| Evet |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Evet | Bkz. [Linux VM 'leri Için Azure disk şifrelemesi](disk-encryption-overview.md). |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Azure sanal makineleri [ExpressRoute](/azure/expressroute) ve VNET şifrelemesini destekler. Bkz. [VM 'lerde geçiş içi şifreleme](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Müşteri tarafından yönetilen anahtarlar desteklenen bir Azure şifreleme senaryosudur; bkz. [Azure şifrelemesi 'ne genel bakış](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| YOK | |
| Şifrelenmiş API çağrıları| Evet | HTTPS ve TLS aracılığıyla. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet |  | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
