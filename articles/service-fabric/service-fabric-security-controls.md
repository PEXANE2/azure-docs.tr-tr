---
title: Azure Hizmet Kumaşı için güvenlik denetimleri
description: Azure Hizmet Kumaşı için güvenlik denetimleri hakkında bilgi edinin. Yerleşik güvenlik denetimlerinin bir denetim listesini içerir.
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: a8bb49e20ec5812a4882966c6918cf2bd59f36a0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645438"
---
# <a name="security-controls-for-azure-service-fabric"></a>Azure Hizmet Kumaşı için güvenlik denetimleri

Bu makalede, Azure Hizmet Kumaşı'nda yerleşik güvenlik denetimleri belgeleilmiştir. 

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet bitiş noktası desteği| Evet |  |
| VNet enjeksiyon desteği| Evet |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Ağ güvenlik gruplarını (NSG) kullanma. |
| Zorunlu tünel desteği| Evet | Azure ağ zorunlu tünel sağlar. |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | Azure izleme desteği ve üçüncü taraf desteğini kullanma. |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | Tüm kontrol uçağı işlemleri, denetim ve onay işlemleri ile çalışır. |
| Veri düzlemi günlüğü ve denetimi| Yok | Müşteri kümenin sahibidir.  |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik doğrulaması| Evet | Kimlik doğrulama, Azure Etkin Dizini aracılığıyla yapılır. |
| Yetkilendirme| Evet | SFRP üzerinden yapılan aramalar için kimlik ve erişim yönetimi (IAM). Doğrudan küme bitiş noktasına yapılan çağrılar iki rolü destekler: Kullanıcı ve Yönetici. Müşteri API'leri her iki role de eşleyebilir. |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar |
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | Müşteri kümenin ve kümenin üzerine inşa edildiği sanal makine ölçeğine sahip. Azure disk şifrelemesanal makine ölçeği kümesinde etkinleştirilebilir. |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Müşteri kümenin ve kümenin üzerine inşa edildiği sanal makine ölçeğine sahip. Azure disk şifrelemesanal makine ölçeği kümesinde etkinleştirilebilir. |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok |  |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet |  |
| API şifreli aramalar| Evet | Hizmet Kumaş API aramaları Azure Kaynak Yöneticisi aracılığıyla yapılır. Geçerli bir JSON web belirteci (JWT) gereklidir. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.