---
title: Azure ExpressRoute için güvenlik öznitelikleri
description: Azure ExpressRoute 'ı değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: expressroute
ms.service: expressroute
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.topic: conceptual
ms.date: 06/05/2019
ms.author: mbaldwin
ms.openlocfilehash: c9a46497c18b99ad7774036fd92e63d024b47045
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442195"
---
# <a name="security-attributes-for-azure-expressroute"></a>Azure ExpressRoute için güvenlik öznitelikleri

Bu makale, Azure ExpressRoute 'da yerleşik olarak bulunan güvenlik özniteliklerini belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)|  Yok | ExpressRoute müşteri verilerini depolamaz. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Hayır | |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Yok |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | [Azure Resource Manager](../azure-resource-manager/index.yml) ve https üzerinden. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok |  |
| VNet ekleme desteği| Yok | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Her müşteri kendi yönlendirme etki alanında bulunur ve kendi VNet 'ine tünel |
| Zorlamalı tünel desteği| Yok | Sınır Ağ Geçidi Protokolü (BGP) üzerinden. |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [ExpressRoute izleme, ölçümler ve uyarılar](expressroute-monitoring-metrics-alerts.md).|

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Microsoft için ağ geçidi için hizmet hesabı (GWM) (denetleyici); Geliştirme ve OP için tam zamanında (JıT) erişim. |
| Authorization|  Evet |Microsoft için ağ geçidi için hizmet hesabı (GWM) (denetleyici); Geliştirme ve OP için tam zamanında (JıT) erişim. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar| 
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi| Hayır |   |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Ağ kaynak sağlayıcısı (NRP) aracılığıyla. |
