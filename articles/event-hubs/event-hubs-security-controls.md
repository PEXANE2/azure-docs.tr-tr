---
title: Azure Event Hubs için güvenlik denetimleri
description: Azure Event Hubs değerlendirmek için güvenlik denetimlerinin denetim listesi
services: event-hubs
ms.service: event-hubs
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: ae357d25a37e188ed043aaa0ca750bb0e52d58da
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903542"
---
# <a name="security-controls-for-azure-event-hubs"></a>Azure Event Hubs için güvenlik denetimleri

Bu makale, Azure Event Hubs yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet uç noktası desteği| Evet |  |  |
| VNet ekleme desteği| Hayır | |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet |  |  |
| Zorlamalı tünel desteği| Hayır |  |  |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | |  |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet |  |  |
| Veri düzlemi günlüğü ve denetimi| Evet |   |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik Doğrulaması| Evet | | [Azure Event Hubs erişimi yetkilendirme](authorize-access-event-hubs.md), [Azure Active Directory kullanarak Event Hubs kaynaklarına](authorize-access-azure-active-directory.md)erişimi yetkilendirme, [paylaşılan erişim imzalarını kullanarak Event Hubs kaynaklarına erişimi yetkilendirme](authorize-access-shared-access-signature.md) |
| Yetkilendirme|  Evet | | [Event Hubs kaynaklara erişmek için Azure Active Directory ile yönetilen bir kimliğin kimliğini doğrulama](authenticate-managed-identity.md), [Event Hubs kaynaklarına erişmek için Azure Active Directory ile bir uygulamanın kimliğini](authenticate-application.md)doğrulama, [paylaşılan erişim imzalarını (SAS) kullanarak Event Hubs kaynaklarına erişimin kimliğini doğrulama](authenticate-shared-access-signature.md) |

## <a name="data-protection"></a>Veri koruması

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar |  Evet | |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet. Adanmış kümeler için kullanılabilir. | Azure Anahtar Kasası 'nda müşteri tarafından yönetilen anahtar, bekleyen bir olay hub 'ında verileri şifrelemek için kullanılabilir. | [Azure Event Hubs verilerini Rest 'te şifrelemek için müşteri tarafından yönetilen anahtarları Azure portal kullanarak yapılandırın](configure-customer-managed-key.md) |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | |  |
| Şifrelenmiş API çağrıları| Evet |  |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | |  |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
