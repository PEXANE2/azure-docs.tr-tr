---
title: Azure Service Fabric için güvenlik denetimleri
description: Azure Service Fabric için güvenlik denetimleri hakkında bilgi edinin. Yerleşik güvenlik denetimlerinin denetim listesini içerir.
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: a8bb49e20ec5812a4882966c6918cf2bd59f36a0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75645438"
---
# <a name="security-controls-for-azure-service-fabric"></a>Azure Service Fabric için güvenlik denetimleri

Bu makale, Azure Service Fabric yerleşik olarak bulunan güvenlik denetimlerini belgeler. 

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Evet |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Ağ güvenlik grupları (NSG) kullanma. |
| Zorlamalı tünel desteği| Evet | Azure ağı Zorlamalı tünel sağlar. |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure izleme desteğini ve üçüncü taraf desteğini kullanma. |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Tüm denetim düzlemi işlemleri, denetim ve onaylar için işlemler aracılığıyla çalışır. |
| Veri düzlemi günlüğü ve denetimi| YOK | Müşteri, kümeye sahip.  |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Kimlik Doğrulaması| Evet | Kimlik doğrulaması Azure Active Directory. |
| Yetkilendirme| Evet | SFRP aracılığıyla çağrılar için kimlik ve erişim yönetimi (ıAM). Doğrudan küme uç noktasına yapılan çağrılar iki rolü destekler: Kullanıcı ve yönetici. Müşteri, API 'Leri her iki role de eşleyebilir. |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar |
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Evet | Müşteri, kümeye ve kümenin oluşturulduğu sanal makine ölçek kümesine sahip. Azure disk şifrelemesi, sanal makine ölçek kümesi üzerinde etkinleştirilebilir. |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Müşteri, kümeye ve kümenin oluşturulduğu sanal makine ölçek kümesine sahip. Azure disk şifrelemesi, sanal makine ölçek kümesi üzerinde etkinleştirilebilir. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| YOK |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet |  |
| Şifrelenmiş API çağrıları| Evet | Service Fabric API çağrıları Azure Resource Manager aracılığıyla yapılır. Geçerli bir JSON Web belirteci (JWT) gerekiyor. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.