---
title: Güvenlik denetimleri
titleSuffix: Azure Storage
description: Azure Depolama'yı değerlendirmek için güvenlik denetimleri listesi.
services: storage
author: msmbaldwin
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: mbaldwin
ms.openlocfilehash: f03f497051367d36bd229a3f358d28a1130ec620
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82082365"
---
# <a name="security-controls-for-azure-storage"></a>Azure Depolama için güvenlik denetimleri

Bu makalede, Azure Depolama'da yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls Header](../../../includes/security-controls-header.md)]

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet |  |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | [Azure Key Vault'ta müşteri tarafından yönetilen anahtarları kullanarak Depolama Hizmeti Şifrelemesi'ne](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)bakın.|
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok |  |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Standart HTTPS/TLS mekanizmalarını destekleyin.  Kullanıcılar, hizmete aktarılmadan önce verileri de şifreleyebilir. |
| API şifreli aramalar| Evet |  |

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Evet |  |
| Hizmet etiketleri desteği| Evet | Azure Depolama tarafından desteklenen hizmet etiketleri hakkında daha fazla bilgi için [Azure hizmet etiketleriyle ilgili genel bakışa](../../virtual-network/service-tags-overview.md) bakın. |
| VNet enjeksiyon desteği| Yok |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | |
| Zorunlu tünel desteği| Yok |  |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | Azure Monitör Ölçümleri|
| Kontrol ve yönetim düzlemi günlüğü ve denetimi | Evet | Azure Kaynak Yöneticisi Etkinlik Günlüğü |
| Veri düzlemi günlüğü ve denetimi| Evet | Hizmet Tanılama Günlükleri.|

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Azure Etkin Dizin, Paylaşılan anahtar, Paylaşılan erişim jetonu. |
| Yetkilendirme| Evet | RBAC, POSIX AKLAR ve SAS Jetonları ile Destek Yetkilendirmesi |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | Azure Kaynak Yöneticisi API'leri aracılığıyla Kaynak Sağlayıcı sürümüne destek |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.