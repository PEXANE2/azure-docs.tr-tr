---
title: Azure Service Fabric için güvenlik öznitelikleri
description: Azure Service Fabric değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: service-fabric
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: service-fabric
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 23c7f8bdcf67d59ccdd5cd0b00bc0e0960ba1d8f
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68443866"
---
# <a name="security-attributes-for-azure-service-fabric"></a>Azure Service Fabric için güvenlik öznitelikleri

Bu makalede, Azure Service Fabric 'da yerleşik olarak bulunan güvenlik öznitelikleri Belgelenebilir. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)| Evet | Müşteri, kümeye ve kümenin oluşturulduğu sanal makine ölçek kümesine sahip. Azure disk şifrelemesi, sanal makine ölçek kümesi üzerinde etkinleştirilebilir. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet |  |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Evet | Müşteri, kümeye ve kümenin oluşturulduğu sanal makine ölçek kümesine sahip. Azure disk şifrelemesi, sanal makine ölçek kümesi üzerinde etkinleştirilebilir. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | Service Fabric API çağrıları Azure Resource Manager aracılığıyla yapılır. Geçerli bir JSON Web belirteci (JWT) gerekiyor. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Evet |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Ağ güvenlik grupları (NSG) kullanma. |
| Zorlamalı tünel desteği| Evet | Azure ağı Zorlamalı tünel sağlar. |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure izleme desteğini ve üçüncü taraf desteğini kullanma. |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Kimlik doğrulaması Azure Active Directory. |
| Authorization| Evet | SFRP aracılığıyla çağrılar için kimlik ve erişim yönetimi (ıAM). Doğrudan küme uç noktasına yapılan çağrılar iki rolü destekler: Kullanıcı ve yönetici. Müşteri, API 'Leri her iki role de eşleyebilir. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Tüm denetim düzlemi işlemleri, denetim ve onaylar için işlemler aracılığıyla çalışır. |
| Veri düzlemi günlüğü ve denetimi| Yok | Müşteri, kümeye sahip.  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | |