---
title: Azure Event Hubs için güvenlik denetimleri
description: Bu makalede, Azure Event Hubs (ağ, kimlik, veri koruma vb.) değerlendirmesi için güvenlik denetimlerinin bir denetim listesi sunulmaktadır.
services: event-hubs
ms.service: event-hubs
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: 0769e88eb72b5b347dd9ebf4b1634501ca54098e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76309515"
---
# <a name="security-controls-for-azure-event-hubs"></a>Azure Event Hubs için güvenlik denetimleri

Bu makale, Azure Event Hubs yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet uç noktası desteği| Yes |  |  |
| VNet ekleme desteği| Hayır | |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes |  |  |
| Zorlamalı tünel desteği| Hayır |  |  |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Yes | |  |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Yes |  |  |
| Veri düzlemi günlüğü ve denetimi| Yes |   |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik Doğrulaması| Yes | | [Azure Event Hubs erişimi yetkilendirme](authorize-access-event-hubs.md), [Azure Active Directory kullanarak Event Hubs kaynaklarına](authorize-access-azure-active-directory.md)erişimi yetkilendirme, [paylaşılan erişim imzalarını kullanarak Event Hubs kaynaklarına erişimi yetkilendirme](authorize-access-shared-access-signature.md) |
| Yetkilendirme|  Yes | | [Event Hubs kaynaklara erişmek için Azure Active Directory ile yönetilen bir kimliğin kimliğini doğrulama](authenticate-managed-identity.md), [Event Hubs kaynaklarına erişmek için Azure Active Directory ile bir uygulamanın kimliğini](authenticate-application.md)doğrulama, [paylaşılan erişim imzalarını (SAS) kullanarak Event Hubs kaynaklarına erişimin kimliğini doğrulama](authenticate-shared-access-signature.md) |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar |  Yes | |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet. Adanmış kümeler için kullanılabilir. | Azure Anahtar Kasası 'nda müşteri tarafından yönetilen anahtar, bekleyen bir olay hub 'ında verileri şifrelemek için kullanılabilir. | [Azure Event Hubs verilerini Rest 'te şifrelemek için müşteri tarafından yönetilen anahtarları Azure portal kullanarak yapılandırın](configure-customer-managed-key.md) |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yes | |  |
| Şifrelenmiş API çağrıları| Yes |  |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yes | |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
