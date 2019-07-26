---
title: Azure Service Bus mesajlaşma için güvenlik öznitelikleri
description: Azure Service Bus mesajlaşma değerlendirmesi için güvenlik özniteliklerinin denetim listesi
services: service-bus-messaging
ms.service: service-bus-messaging
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: 0e1d6e041b47a261b549fb8b608cf09d0d6362dd
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443890"
---
# <a name="security-attributes-for-azure-service-bus-messaging"></a>Azure Service Bus mesajlaşma için güvenlik öznitelikleri

Bu makale, Azure Service Bus mesajlaşma 'da yerleşik olarak bulunan güvenlik özniteliklerini belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)|  Varsayılan olarak sunucu tarafı şifreleme için Evet. | Müşteri tarafından yönetilen anahtarlar ve BYOK henüz desteklenmiyor. İstemci tarafı şifrelemesi, istemcinin sorumluluğundadır |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Standart HTTPS/TLS mekanizmasını destekler. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır |   |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | API çağrıları [Azure Resource Manager](../azure-resource-manager/index.yml) ve HTTPS aracılığıyla yapılır. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet (yalnızca Premium katman) | Sanal ağ hizmeti uç noktaları yalnızca [Service Bus Premium katmanı](service-bus-premium-messaging.md) için desteklenir. |
| VNet ekleme desteği| Hayır | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet (yalnızca Premium katman) |  |
| Zorlamalı tünel desteği| Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | [Azure izleyici ve uyarılar](service-bus-metrics-azure-monitor.md)aracılığıyla desteklenir. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | [Azure Active Directory yönetilen hizmet kimliği](service-bus-managed-service-identity.md)ile yönetilir; bkz. [Service Bus kimlik doğrulaması ve yetkilendirme](service-bus-authentication-and-authorization.md).|
| Authorization| Evet | [RBAC](service-bus-role-based-access-control.md) (Önizleme) ve SAS belirteci aracılığıyla yetkilendirmeyi destekler; bkz. [Service Bus kimlik doğrulaması ve yetkilendirme](service-bus-authentication-and-authorization.md). |



## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | İşlem günlükleri kullanılabilir; bkz. [Service Bus tanılama günlükleri](service-bus-diagnostic-logs.md).  |
| Veri düzlemi günlüğü ve denetimi| Hayır |  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure Resource Manager API](/rest/api/resources/)aracılığıyla kaynak sağlayıcısı sürümü oluşturmayı destekler.|
