---
title: Güvenlik denetimleri
titleSuffix: Azure Storage
description: Azure Storage 'ı değerlendirmek için güvenlik denetimi denetim listelerini görüntüleyin. Veri koruma, ağ, izleme ve günlüğe kaydetme, kimlik ve yapılandırma kapsamındaki bölgeler.
services: storage
author: msmbaldwin
ms.author: mbaldwin
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 03/11/2020
ms.openlocfilehash: 3b4d74e7ba869e0438a936817d824e841823d472
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91715719"
---
# <a name="security-controls-for-azure-storage"></a>Azure depolama için güvenlik denetimleri

Bu makale, Azure depolama 'da yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yes | Depolama Hizmeti Şifrelemesi bkz. [Azure Key Vault müşteri tarafından yönetilen anahtarları kullanma](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yes | Standart HTTPS/TLS mekanizmalarını destekler.  Kullanıcılar, hizmete iletilmeden önce verileri de şifreleyebilir. |
| Şifrelenmiş API çağrıları| Yes |  |

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yes |  |
| Hizmet etiketleri desteği| Yes | Azure depolama tarafından desteklenen hizmet etiketleri hakkında daha fazla bilgi için bkz. [Azure hizmet etiketlerine genel bakış](../../virtual-network/service-tags-overview.md) . |
| VNet ekleme desteği| Yok |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes | |
| Zorlamalı tünel desteği| Yok |  |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Yes | Azure Izleyici ölçümleri|
| Denetim ve yönetim düzlemi günlüğü ve denetimi | Yes | Azure Etkinlik Günlüğü |
| Veri düzlemi günlüğü ve denetimi| Yes | Azure Izleyici kaynak günlükleri |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Yes | Azure Active Directory, paylaşılan anahtar, paylaşılan erişim belirteci. |
| Yetkilendirme| Yes | Azure RBAC, POSIX ACL 'Leri ve SAS belirteçleri aracılığıyla yetkilendirme desteği |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yes | Azure Resource Manager API 'Leri aracılığıyla kaynak sağlayıcısı sürümlendirmeyi destekleme |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.