---
title: Azure API Management için güvenlik denetimleri
description: API Management değerlendirmek için güvenlik denetimlerinin denetim listesini gözden geçirin. Bunlar arasında ağ, kimlik ve veri koruma denetimleri bulunur.
services: api-management
author: vladvino
ms.service: api-management
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: vlvinogr
ms.openlocfilehash: a147179f7b55e43379b3c3fa3a7a0767cc97b198
ms.sourcegitcommit: 4e5560887b8f10539d7564eedaff4316adb27e2c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87902584"
---
# <a name="security-controls-for-api-management"></a>API Management için güvenlik denetimleri

Bu makalede, API Management yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet uç noktası desteği| Hayır | |  |
| VNet ekleme desteği| Evet | |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Sırasıyla ağ güvenlik grupları (NSG) ve Azure Application Gateway (veya diğer yazılım gereci) kullanma. |  |
| Zorlamalı tünel desteği| Evet | Azure ağı Zorlamalı tünel sağlar. |  |

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | | |
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | [Azure Izleyici etkinlik günlükleri](../azure-monitor/platform/platform-logs-overview.md) | |
| Veri düzlemi günlüğü ve denetimi| Evet | [Azure izleyici tanılama günlükleri](../azure-monitor/platform/platform-logs-overview.md) ve (isteğe bağlı olarak) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  | |


## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik Doğrulaması| Evet | |  |
| Yetkilendirme| Evet | |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Evet | Sertifikalar, anahtarlar ve gizli-adlandırılmış değerler gibi hassas veriler hizmet tarafından yönetilen hizmet örneği anahtarları ile şifrelenir. |  |
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır | Tüm şifreleme anahtarları hizmet örneği başına alınır ve hizmet yönetilir. |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | [Express Route](../expressroute/index.yml) ve VNET şifrelemesi, [Azure ağı](../virtual-network/index.yml)tarafından sağlanır. |  |
| Şifrelenmiş API çağrıları| Evet | Yönetim düzlemi çağrıları TLS üzerinden [Azure Resource Manager](../azure-resource-manager/index.yml) üzerinden yapılır. Geçerli bir JSON Web belirteci (JWT) gerekiyor.  Veri düzlemi çağrıları TLS ve desteklenen kimlik doğrulama mekanizmalarından biri (örneğin, istemci sertifikası veya JWT) ile güvenli hale getirilmiş olabilir. |   |
 |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure API Management DevOps kaynak setini](https://aka.ms/apimdevops) kullanma |  |

## <a name="vulnerability-scans-false-positives"></a>Güvenlik açığı yanlış pozitifleri tarar

Bu bölüm, Azure API Management etkilemeyen yaygın güvenlik açıklarını belgelemektedir.

| Güvenlik açığı               | Açıklama                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bilet taşma payı (CVE-2016-9244) | Bilet taşma payı, bazı F5 ürünlerinde bulunan TLS Sessionbilet uzantısının uygulanmasında güvenlik açığıdır. Başlatılmamış bellekten 31 baytlık verilerin sızıntısını ("bleden") sağlar. Bunun nedeni, istemciden geçirilen bir oturum kimliği olan bir oturum kimliğini doldurmasından kaynaklanır ve bu da BT 32 bit uzunluğunda hale gelir. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.