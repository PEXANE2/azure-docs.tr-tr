---
title: Azure Key Vault için güvenlik öznitelikleri
description: Azure Key Vault değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: key-vault
author: msmbaldwin
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 836d025c5bc69da9606c9a6172ac6a43caaaf29b
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444013"
---
# <a name="security-attributes-for-azure-key-vault"></a>Azure Key Vault için güvenlik öznitelikleri

Bu makale, Azure Key Vault yerleşik güvenlik özniteliklerini belgeler. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)| Evet | Tüm nesneler şifrelenir. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Tüm iletişimler şifreli API çağrıları aracılığıyla yapılır |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Evet | Müşteri, Key Vault tüm anahtarları denetler. Donanım güvenlik modülü (HSM) tarafından desteklenen anahtarlar belirtildiğinde, FIPS düzey 2 HSM anahtarı, sertifikayı veya parolayı korur. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | HTTPS kullanma. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Sanal ağ (VNet) hizmet uç noktalarını kullanma. |
| VNet ekleme desteği| Hayır |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | VNet güvenlik duvarı kurallarını kullanma. |
| Zorlamalı tünel desteği| Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Log Analytics kullanma. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Kimlik doğrulaması Azure Active Directory. |
| Authorization| Evet | Key Vault erişim Ilkesini kullanma. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi günlüğü ve denetimi| Evet | Log Analytics kullanma. |
| Veri düzlemi günlüğü ve denetimi| Evet | Log Analytics kullanma. |

## <a name="access-controls"></a>Erişim denetimleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi erişim denetimleri | Evet | Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi erişim denetimleri (her hizmet düzeyinde) | Evet | Key Vault erişim Ilkesi |