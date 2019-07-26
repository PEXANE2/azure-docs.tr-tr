---
title: Azure depolama için güvenlik öznitelikleri
description: Azure Storage 'ı değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: storage
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: storage
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: mbaldwin
ms.openlocfilehash: 16ec2757955b53a8bfa73ba724100f7fa61d2867
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68444310"
---
# <a name="security-attributes-for-azure-storage"></a>Azure depolama için güvenlik öznitelikleri

Bu makale, Azure depolama 'da yerleşik olarak bulunan güvenlik özniteliklerini belgeler. 

[!INCLUDE [Security Attributes Header](../../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)| Evet |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Standart HTTPS/TLS mekanizmalarını destekler.  Kullanıcılar, hizmete iletilmeden önce verileri de şifreleyebilir. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Evet | Depolama Hizmeti Şifrelemesi bkz. [Azure Key Vault müşteri tarafından yönetilen anahtarları kullanma](storage-service-encryption-customer-managed-keys.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Yok |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | |
| Zorlamalı tünel desteği| Yok |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure Izleyici ölçümleri Şu anda kullanılabilir, günlük önizlemesi başlatılıyor |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Azure Active Directory, paylaşılan anahtar, paylaşılan erişim belirteci. |
| Authorization| Evet | RBAC, POSIX ACL 'Leri ve SAS belirteçleri aracılığıyla yetkilendirme desteği |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi | Evet | Etkinlik günlüğünü Azure Resource Manager |
| Veri düzlemi günlüğü ve denetimi| Evet | Hizmet tanılama günlükleri ve Azure Izleyici günlüğü başlangıç önizlemesi  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Azure Resource Manager API 'Leri aracılığıyla kaynak sağlayıcısı sürümlendirmeyi destekleme |