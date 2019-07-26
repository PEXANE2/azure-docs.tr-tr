---
title: Azure API Management için güvenlik öznitelikleri
description: API Management değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: api-management
author: msmbaldwin
manager: barbkess
ms.service: api-management
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 2049e2349e3a25ebd4d3f4db19ec47bbaeb067de
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442277"
---
# <a name="security-attributes-for-api-management"></a>API Management için güvenlik öznitelikleri

Bu makale, API Management yerleşik güvenlik özniteliklerini belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)| Evet (yalnızca hizmet tarafı şifreleme) | Sertifikalar, anahtarlar ve gizli-adlandırılmış değerler gibi hassas veriler hizmet tarafından yönetilen hizmet örneği anahtarları ile şifrelenir. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | [Express Route](../expressroute/index.yml) ve VNET şifrelemesi, [Azure ağı](../virtual-network/index.yml)tarafından sağlanır. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır | Tüm şifreleme anahtarları hizmet örneği başına alınır ve hizmet yönetilir. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | Yönetim düzlemi çağrıları TLS üzerinden [Azure Resource Manager](../azure-resource-manager/index.yml) üzerinden yapılır. Geçerli bir JSON Web belirteci (JWT) gerekiyor.  Veri düzlemi çağrıları TLS ve desteklenen kimlik doğrulama mekanizmalarından biri (örneğin, istemci sertifikası veya JWT) ile güvenli hale getirilmiş olabilir.
 |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Sırasıyla ağ güvenlik grupları (NSG) ve Azure Application Gateway (veya diğer yazılım gereci) kullanma. |
| Zorlamalı tünel desteği| Evet | Azure ağı Zorlamalı tünel sağlar. |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | |
| Authorization| Evet | |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | [Azure Izleyici etkinlik günlükleri](../azure-monitor/platform/activity-logs-overview.md) |
| Veri düzlemi günlüğü ve denetimi| Evet | [Azure izleyici tanılama günlükleri](../azure-monitor/platform/diagnostic-logs-overview.md) ve (isteğe bağlı olarak) [Azure Application Insights](../azure-monitor/app/app-insights-overview.md).  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure API Management DevOps kaynak setini](https://aka.ms/apimdevops) kullanma |

## <a name="vulnerability-scans-false-positives"></a>Güvenlik açığı yanlış pozitifleri tarar

Bu bölüm, Azure API Management etkilemeyen yaygın güvenlik açıklarını belgelemektedir.

| Güvenlik açığı               | Açıklama                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bilet taşma payı (CVE-2016-9244) | Bilet taşma payı, bazı F5 ürünlerinde bulunan TLS Sessionbilet uzantısının uygulanmasında güvenlik açığıdır. Başlatılmamış bellekten 31 baytlık verilerin sızıntısını ("bleden") sağlar. Bunun nedeni, istemciden geçirilen bir oturum kimliği olan bir oturum kimliğini doldurmasından kaynaklanır ve bu da BT 32 bit uzunluğunda hale gelir. |
