---
title: Azure App Service için güvenlik öznitelikleri
description: Azure App Service değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: app-service
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: app-service
ms.topic: conceptual
ms.date: 05/08/2019
ms.author: mbaldwin
ms.openlocfilehash: 299262610c027529749840720f46d6dc97a07b21
ms.sourcegitcommit: a874064e903f845d755abffdb5eac4868b390de7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68442265"
---
# <a name="security-attributes-for-azure-app-service"></a>Azure App Service için güvenlik öznitelikleri

Bu makale, Azure App Service yerleşik güvenlik özniteliklerini belgeler.

[!INCLUDE [Security attributes header](../../includes/security-attributes-header.md)]

## <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet | Web sitesi dosya içeriği, bekleyen içeriği otomatik olarak şifreleyen Azure Storage 'da depolanır. [Bekleyen veriler için bkz. Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md).<br><br>Müşteri tarafından sağlanan gizlilikler bekleyen bir şekilde şifrelenir. Gizlilikler, App Service yapılandırma veritabanlarında depolanırken Rest 'te şifrelenir.<br><br>Yerel olarak bağlı diskler isteğe bağlı olarak, Web siteleri tarafından geçici depolama olarak kullanılabilir (D:\Local ve% TMP%). Yerel olarak bağlı diskler, bekleyen olarak şifrelenmez. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Müşteriler, Web sitelerini gelen trafik için HTTPS 'yi gerektirecek ve kullanacak şekilde yapılandırabilir. Bkz. blog gönderisi [yalnızca HTTPS Azure App Service oluşturma](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Evet | Müşteriler, Key Vault içinde uygulama gizli dizilerini depolamayı ve çalışma zamanında bunları almayı seçebilir. Bkz. [App Service ve Azure işlevleri için Key Vault başvurularını kullanma (Önizleme)](app-service-key-vault-references.md).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | App Service yapılandırmak için yönetim çağrıları, HTTPS üzerinden [Azure Resource Manager](../azure-resource-manager/index.yml) çağrıları aracılığıyla gerçekleşir. |

## <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Şu anda App Service için önizlemede kullanılabilir. Bkz. [Azure App Service erişim kısıtlamaları](app-service-ip-restrictions.md). |
| VNet ekleme desteği| Evet | App Service ortamlar, müşterinin sanal ağına eklenen tek bir müşteriye adanmış App Service özel uygulamalarıdır. Bkz. [App Service ortamlarına giriş](environment/intro.md). |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | App Service Genel çok kiracılı bir çeşitlemesi için, müşteriler izin verilen gelen trafiği kilitlemek üzere ağ ACL 'Lerini (IP kısıtlamaları) yapılandırabilir.  Bkz. [Azure App Service erişim kısıtlamaları](app-service-ip-restrictions.md).  App Service ortamlar doğrudan sanal ağlara dağıtılır ve bu nedenle NSG 'ler ile güvenliği sağlanmış olabilir. |
| Zorlamalı tünel desteği| Evet | App Service ortamlar, zorlamalı tünelin yapılandırıldığı bir müşterinin sanal ağına dağıtılabilir. Müşterilerin [App Service ortamı zorlamalı tünelleme Ile yapılandırma](environment/forced-tunnel-support.md)yönergelerini izlemesi gerekir. |

## <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | App Service, Application Insights destekleyen diller için Application Insights tümleştirilir (tam .NET Framework, .NET Core, Java ve Node. JS).  Bkz. [izleyici Azure App Service performansı](../azure-monitor/app/azure-web-apps.md). App Service Ayrıca, Azure Izleyici 'ye uygulama ölçümleri de gönderir. Bkz. [Azure App Service uygulamaları izleme](web-sites-monitor.md). |

## <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Müşteriler, otomatik olarak [Azure Active Directory (Azure AD)](../active-directory/index.yml) ile tümleştirilen ve diğer OAuth uyumlu kimlik sağlayıcılarının yanı sıra App Service uygulamalar oluşturabilir. bkz. [Azure App Service kimlik doğrulaması ve yetkilendirme](overview-authentication-authorization.md). App Service varlıklara yönetim erişimi için tüm erişim, Azure AD kimlik doğrulamalı sorumlu ve Azure Resource Manager RBAC rollerinin bir birleşimiyle denetlenir. |
| Authorization| Evet | App Service varlıklara yönetim erişimi için tüm erişim, Azure AD kimlik doğrulamalı sorumlu ve Azure Resource Manager RBAC rollerinin bir birleşimiyle denetlenir.  |


## <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | App Service nesneler üzerinde gerçekleştirilen tüm yönetim işlemleri [Azure Resource Manager](../azure-resource-manager/index.yml)aracılığıyla oluşur. Bu işlemlerin geçmiş günlükleri hem portalda hem de CLı aracılığıyla kullanılabilir. bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftweb) ve [az Monitor Activity-Log](/cli/azure/monitor/activity-log). |
| Veri düzlemi günlüğü ve denetimi | Hayır | App Service için veri düzlemi, müşterinin dağıtılmış web sitesi içeriğini içeren bir uzak dosya paylaşımıdır.  Uzak dosya paylaşımının denetimi yoktur. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Yönetim işlemleri için bir App Service yapılandırmanın durumu bir Azure Resource Manager şablonu olarak ve zaman içinde sürümlü şekilde aktarılabilir. Çalışma zamanı işlemlerinde, müşteriler App Service dağıtım yuvaları özelliğini kullanarak bir uygulamanın birden çok farklı canlı sürümünü koruyabilir. | 

