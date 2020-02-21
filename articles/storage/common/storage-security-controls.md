---
title: Azure depolama için güvenlik denetimleri
description: Azure Storage 'ı değerlendirmek için güvenlik denetimlerinin denetim listesi
services: storage
documentationcenter: ''
author: msmbaldwin
manager: rkarlin
ms.service: storage
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 2cc54077456fce1e7e0f47843a762beee8e715f7
ms.sourcegitcommit: 3c8fbce6989174b6c3cdbb6fea38974b46197ebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/21/2020
ms.locfileid: "77526765"
---
# <a name="security-controls-for-azure-storage"></a>Azure depolama için güvenlik denetimleri

Bu makale, Azure depolama 'da yerleşik olarak bulunan güvenlik denetimlerini belgeler. 

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Evet |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Depolama Hizmeti Şifrelemesi bkz. [Azure Key Vault müşteri tarafından yönetilen anahtarları kullanma](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Standart HTTPS/TLS mekanizmalarını destekler.  Kullanıcılar, hizmete iletilmeden önce verileri de şifreleyebilir. |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Yok |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | |
| Zorlamalı tünel desteği| Yok |  |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure Izleyici ölçümleri|
| Denetim ve yönetim düzlemi günlüğü ve denetimi | Evet | Etkinlik günlüğünü Azure Resource Manager |
| Veri düzlemi günlüğü ve denetimi| Evet | Hizmet tanılama günlükleri.|

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Active Directory, paylaşılan anahtar, paylaşılan erişim belirteci. |
| Yetkilendirme| Evet | RBAC, POSIX ACL 'Leri ve SAS belirteçleri aracılığıyla yetkilendirme desteği |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Azure Resource Manager API 'Leri aracılığıyla kaynak sağlayıcısı sürümlendirmeyi destekleme |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.