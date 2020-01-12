---
title: Azure Service Bus mesajlaşma için güvenlik denetimleri
description: Azure Service Bus mesajlaşma değerlendirmesi için güvenlik denetimlerinin denetim listesi
services: service-bus-messaging
ms.service: service-bus-messaging
author: spelluru
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: spelluru
ms.openlocfilehash: af119ef026b70fcb4a56b4f823d20c0e9eddddc8
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903258"
---
# <a name="security-controls-for-azure-service-bus-messaging"></a>Azure Service Bus mesajlaşma için güvenlik denetimleri

Bu makale, Azure Service Bus mesajlaşma 'da yerleşik olarak bulunan güvenlik denetimlerini belgeler.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet uç noktası desteği| Evet (yalnızca Premium katman) | Sanal ağ hizmeti uç noktaları yalnızca [Service Bus Premium katmanı](service-bus-premium-messaging.md) için desteklenir. |  |
| VNet ekleme desteği| Hayır | |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet (yalnızca Premium katman) |  |  |
| Zorlamalı tünel desteği| Hayır |  |  |

## <a name="monitoring--logging"></a>& Günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | [Azure izleyici ve uyarılar](service-bus-metrics-azure-monitor.md)aracılığıyla desteklenir. |  |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | İşlem günlükleri kullanılabilir.  | [Tanılama günlüklerini Service Bus](service-bus-diagnostic-logs.md) |
| Veri düzlemi günlüğü ve denetimi| Hayır |  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik Doğrulaması| Evet | [Azure Active Directory yönetilen hizmet kimliği](service-bus-managed-service-identity.md)üzerinden yönetilir.| [Kimlik doğrulaması ve yetkilendirme Service Bus](service-bus-authentication-and-authorization.md). |
| Yetkilendirme| Evet | [RBAC](authenticate-application.md) ve SAS belirteci aracılığıyla yetkilendirmeyi destekler. | [Kimlik doğrulaması ve yetkilendirme Service Bus](service-bus-authentication-and-authorization.md). |

## <a name="data-protection"></a>Veri koruması

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar |  Varsayılan olarak sunucu tarafı şifreleme için Evet. |  |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet. | Azure Anahtar Kasası 'nda müşteri tarafından yönetilen bir anahtar, bekleyen Service Bus ad alanındaki verileri şifrelemek için kullanılabilir. | [Azure portal kullanarak geri kalan Azure Service Bus verileri şifrelemek için müşteri tarafından yönetilen anahtarları yapılandırın](configure-customer-managed-key.md)  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |   |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Standart HTTPS/TLS mekanizmasını destekler. |   |
| Şifrelenmiş API çağrıları| Evet | API çağrıları [Azure Resource Manager](../azure-resource-manager/index.yml) ve HTTPS aracılığıyla yapılır. |   |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure Resource Manager API](/rest/api/resources/)aracılığıyla kaynak sağlayıcısı sürümü oluşturmayı destekler.|   |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
