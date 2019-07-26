---
title: Azure Service Bus geçişi için güvenlik öznitelikleri
description: Azure Service Bus geçişini değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: service-bus-relay
ms.service: service-bus-relay
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: mbaldwin
ms.openlocfilehash: 2047f64914d4a286e6de38b7b2c8524d98eba562
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443877"
---
# <a name="security-attributes-for-azure-service-bus-relay"></a>Azure Service Bus geçişi için güvenlik öznitelikleri

Bu makale Azure Service Bus geçişine yerleşik olarak bulunan güvenlik özniteliklerini belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)|  Yok | Geçiş bir Web soketi ve verileri kalıcı yapmaz. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Hizmet için TLS gerekiyor. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır | Yalnızca Microsoft TLS sertifikalarını kullanır.  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | 'DİR. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |
| Zorlamalı tünel desteği| Yok | Geçiş, TLS tünelidir  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | SAS aracılığıyla. |
| Authorization|  Evet | SAS aracılığıyla. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden. |
| Veri düzlemi günlüğü ve denetimi| Evet | Bağlantı başarısı/hatası ve hataları ve günlüğe kaydedildi.  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml)üzerinden.|
