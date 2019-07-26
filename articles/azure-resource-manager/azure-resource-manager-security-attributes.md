---
title: Azure Resource Manager için güvenlik öznitelikleri
description: Azure Resource Manager değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: azure-resource-manager
author: msmbaldwin
manager: barbkess
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: mbaldwin
ms.openlocfilehash: e3bfb79c54ff57adfa947f2dd0100f6c05c7af9f
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444143"
---
# <a name="security-attributes-for-azure-resource-manager"></a>Azure Resource Manager için güvenlik öznitelikleri

Bu makale, Azure Resource Manager yerleşik güvenlik özniteliklerini belgeler.

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)| Evet |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | HTTPS/TLS. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Yok | Azure Resource Manager hiçbir müşteri içeriği depolar, yalnızca denetim verileri. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Evet | |
| Şifrelenmiş API çağrıları| Evet | |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |
| Zorlamalı tünel desteği| Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Hayır | |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | [Azure Active Directory](/azure/active-directory) tabanlı.|
| Authorization| Evet | |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Etkinlik günlükleri, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, SILME) kullanıma sunar; bkz. [kaynaklardaki eylemleri denetlemek için etkinlik günlüklerini görüntüleme](resource-group-audit.md). |
| Veri düzlemi günlüğü ve denetimi| Yok | |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet |  |
