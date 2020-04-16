---
title: Azure Anahtar Kasası için güvenlik denetimleri
description: Azure Anahtar Kasası'nı değerlendirmek için güvenlik denetimleri listesi
services: key-vault
author: msmbaldwin
manager: rkarlin
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: cd6602f68b63e2c236e7f3905d33b88fbda36ed2
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429869"
---
# <a name="security-controls-for-azure-key-vault"></a>Azure Anahtar Kasası için güvenlik denetimleri

Bu makalede, Azure Anahtar Kasası'nda yerleşik güvenlik denetimleri belgeleilmiştir. 

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Evet | Sanal Ağ (VNet) hizmet bitiş noktalarını kullanma. |
| VNet enjeksiyon desteği| Hayır |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | VNet güvenlik duvarı kurallarını kullanma. |
| Zorunlu tünel desteği| Hayır |  |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | Log Analytics'i kullanma. |
| Kontrol/Yönetim uçağı Günlüğü ve Denetimi| Evet | Log Analytics'i kullanma. |
| Veri düzlemi günlüğü ve denetimi| Evet | Log Analytics'i kullanma. |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Kimlik doğrulama, Azure Etkin Dizini aracılığıyla yapılır. |
| Yetkilendirme| Evet | Key Vault Erişim Politikası'nı kullanma. |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | Tüm nesneler şifrelenir. |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Müşteri, Key Vault'undaki tüm anahtarları kontrol eder. Donanım güvenlik modülü (HSM) destekli anahtarlar belirtildiğinde, FIPS Düzey 2 HSM anahtarı, sertifikayı veya gizliyi korur. |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok |  |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Tüm iletişim şifreli API aramaları üzerinden |
| API şifreli aramalar| Evet | HTTPS'yi kullanma. |

## <a name="access-controls"></a>Erişim denetimleri

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kontrol/Yönetim düzlemerişim denetimleri | Evet | Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi erişim denetimleri (Her hizmet düzeyinde) | Evet | Anahtar Vault Erişim Politikası |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.