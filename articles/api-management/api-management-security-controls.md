---
title: Azure API Yönetimi için güvenlik denetimleri
description: API Yönetimini değerlendirmek için güvenlik denetimleri denetim listesi
services: api-management
author: vladvino
ms.service: api-management
ms.topic: conceptual
ms.date: 09/23/2019
ms.author: vlvinogr
ms.openlocfilehash: 670050efe01fb658fab52a43914f193e9798b828
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75751131"
---
# <a name="security-controls-for-api-management"></a>API Yönetimi için güvenlik denetimleri

Bu makalede, API Yönetimi yerleşik güvenlik denetimleri belgeler.

[!INCLUDE [Security controls Header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Hizmet bitiş noktası desteği| Hayır | |  |
| VNet enjeksiyon desteği| Evet | |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Ağ güvenlik gruplarını (NSG) ve Azure Application Gateway 'i (veya diğer yazılım cihazlarını) sırasıyla kullanma. |  |
| Zorunlu tünel desteği| Evet | Azure ağ zorunlu tünel sağlar. |  |

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | | |
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | [Azure Monitör etkinlik günlükleri](../azure-monitor/platform/platform-logs-overview.md) | |
| Veri düzlemi günlüğü ve denetimi| Evet | [Azure Tanı günlüklerini](../azure-monitor/platform/platform-logs-overview.md) ve (isteğe bağlı olarak) [Azure Uygulama Öngörülerini](../azure-monitor/app/app-insights-overview.md)izleyin.  | |


## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Kimlik doğrulaması| Evet | |  |
| Yetkilendirme| Evet | |  |

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler |
|---|---|--|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | Sertifikalar, anahtarlar ve gizli adlandırılmış değerler gibi hassas veriler, hizmet örneği anahtarları başına hizmet yönetimiyle şifrelenir. |  |
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Hayır | Tüm şifreleme anahtarları hizmet örneği başınadır ve hizmet tarafından yönetilir. |  |
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |  |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | [Express Route](../expressroute/index.yml) ve VNet şifrelemesi [Azure ağı](../virtual-network/index.yml)tarafından sağlanır. |  |
| API şifreli aramalar| Evet | Yönetim düzlemi aramaları TLS üzerinden [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml) üzerinden yapılır. Geçerli bir JSON web belirteci (JWT) gereklidir.  Veri düzlemi aramaları TLS ve desteklenen kimlik doğrulama mekanizmalarından biri (örneğin, istemci sertifikası veya JWT) ile güvence altına alınabilir. |   |
 |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar| Belgeler |
|---|---|--|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | Azure [API Yönetimi DevOps Kaynak Kitini](https://aka.ms/apimdevops) Kullanma |  |

## <a name="vulnerability-scans-false-positives"></a>Güvenlik açığı yanlış pozitif leri tarar

Bu bölümde, Azure API Yönetimi'ni etkilemeyen yaygın güvenlik açıkları belgelenemektedir.

| Güvenlik açığı               | Açıklama                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Biletli (CVE-2016-9244) | Ticketbleed, bazı F5 ürünlerinde bulunan TLS SessionTicket uzantısının uygulanmasında güvenlik açığıdır. Başlamayan bellekten 31 bayta kadar veri sızıntısına ("kanama") izin verir. Bunun nedeni, istemciden geçirilen ve 32 bit uzunluğunda veri içeren BIR Oturum Kimliği doldurma TLS yığınından kaynaklanır. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.