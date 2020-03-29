---
title: Azure Etkinlik Hub'ları için güvenlik denetimleri
description: Bu makalede, Azure Olay Hub'larını (ağ, kimlik, veri koruma, vb.) değerlendirmek için güvenlik denetimleri bir denetim listesi bulunur.
services: event-hubs
ms.service: event-hubs
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: 0769e88eb72b5b347dd9ebf4b1634501ca54098e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76309515"
---
# <a name="security-controls-for-azure-event-hubs"></a>Azure Etkinlik Hub'ları için güvenlik denetimleri

Bu makalede, Azure Olay Hub'larında yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet bitiş noktası desteği| Evet |  |  |
| VNet enjeksiyon desteği| Hayır | |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet |  |  |
| Zorunlu tünel desteği| Hayır |  |  |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | |  |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet |  |  |
| Veri düzlemi günlüğü ve denetimi| Evet |   |  |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik doğrulaması| Evet | | [Azure Etkinlik Hub'larına erişimi yetkilendirme](authorize-access-event-hubs.md), [Azure Etkin Dizinini kullanarak Etkinlik Hub'ları kaynaklarına erişimi yetkilendirme](authorize-access-azure-active-directory.md), Paylaşılan Erişim [İmzalarını kullanarak Etkinlik Hub'ları kaynaklarına erişim](authorize-access-shared-access-signature.md) yetkisi verme |
| Yetkilendirme|  Evet | | [Etkinlik Hub'ları Kaynaklarına erişmek için Azure Active Directory ile yönetilen bir kimliğin kimliğini doğrula,](authenticate-managed-identity.md) [Etkinlik Hub'ları kaynaklarına erişmek için Azure Active Directory ile bir uygulamayı](authenticate-application.md)kimlik doğrulama , paylaşılan erişim [imzalarını (SAS) kullanarak Etkinlik Hub'ları kaynaklarına erişimi doğrulama](authenticate-shared-access-signature.md) |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar |  Evet | |  |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet. Özel kümeler için kullanılabilir. | Azure KeyVault'taki müşteri yönetilen bir anahtar, bir Etkinlik Hub'ındaki verileri istirahathalinde şifrelemek için kullanılabilir. | [Azure portalını kullanarak Azure Event Hub'ları verilerini yeniden şifrelemek için müşteri tarafından yönetilen anahtarları yapılandırın](configure-customer-managed-key.md) |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |  |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | |  |
| API şifreli aramalar| Evet |  |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
