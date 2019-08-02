---
title: Azure Backup için güvenlik öznitelikleri
description: Azure Backup değerlendirmek için güvenlik özniteliklerinin denetim listesi
ms.reviewer: mbaldwin
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 04/16/2019
ms.author: dacurwin
ms.openlocfilehash: e6ea701840b201c2794d81be5019d57ca531cd00
ms.sourcegitcommit: d585cdda2afcf729ed943cfd170b0b361e615fae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68688498"
---
# <a name="security-attributes-for-azure-backup"></a>Azure Backup için güvenlik öznitelikleri

Bu makale, Azure Backup yerleşik güvenlik özniteliklerini belgeler. 

[!INCLUDE [Security Attributes Header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)| Evet | Depolama hesapları için depolama hizmeti şifrelemesini kullanma. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Hayır | HTTPS kullanma. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Hayır |  |
| Şifrelenmiş API çağrıları| Evet |  |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| VNet ekleme desteği| Hayır |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Zorlamalı tünel VM yedeklemesi için desteklenir. Zorlamalı tünel, VM 'Ler içinde çalışan iş yükleri için desteklenmez. |
| Zorlamalı tünel desteği| Hayır |  |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Log Analytics tanılama günlükleri aracılığıyla desteklenir. Daha fazla bilgi için bkz. [Log Analytics kullanarak korumalı iş yüklerini izleme Azure Backup](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) . |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Kimlik doğrulaması Azure Active Directory. |
| Authorization| Evet | Oluşturulan müşteri ve yerleşik RBAC rolleri kullanılır. Daha fazla bilgi için [Azure Backup kurtarma noktalarını yönetmek üzere rol tabanlı Access Control kullanma](/azure/backup/backup-rbac-rs-vault) konusuna bakın. |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Azure portal müşterinin tetiklediği tüm eylemler etkinlik günlüklerine kaydedilir. |
| Veri düzlemi günlüğü ve denetimi| Hayır | Azure Backup veri düzlemine doğrudan ulaşılamıyor.  |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet|  |