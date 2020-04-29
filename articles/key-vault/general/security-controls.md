---
title: Azure Key Vault için güvenlik denetimleri
description: Azure Key Vault değerlendirmek için güvenlik denetimlerinin denetim listesi
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: cd6602f68b63e2c236e7f3905d33b88fbda36ed2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81429869"
---
# <a name="security-controls-for-azure-key-vault"></a>Azure Key Vault için güvenlik denetimleri

Bu makalede, Azure Key Vault yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir. 

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yes | Sanal ağ (VNet) hizmet uç noktalarını kullanma. |
| VNet ekleme desteği| Hayır |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes | VNet güvenlik duvarı kurallarını kullanma. |
| Zorlamalı tünel desteği| Hayır |  |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Yes | Log Analytics kullanma. |
| Denetim/Yönetim düzlemi günlüğü ve denetimi| Yes | Log Analytics kullanma. |
| Veri düzlemi günlüğü ve denetimi| Yes | Log Analytics kullanma. |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Yes | Kimlik doğrulaması Azure Active Directory. |
| Yetkilendirme| Yes | Key Vault erişim Ilkesini kullanma. |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes | Tüm nesneler şifrelenir. |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yes | Müşteri, Key Vault tüm anahtarları denetler. Donanım güvenlik modülü (HSM) tarafından desteklenen anahtarlar belirtildiğinde, FIPS düzey 2 HSM anahtarı, sertifikayı veya parolayı korur. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yes | Tüm iletişimler şifreli API çağrıları aracılığıyla yapılır |
| Şifrelenmiş API çağrıları| Yes | HTTPS kullanma. |

## <a name="access-controls"></a>Erişim denetimleri

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi erişim denetimleri | Yes | Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi erişim denetimleri (her hizmet düzeyinde) | Yes | Key Vault erişim Ilkesi |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.