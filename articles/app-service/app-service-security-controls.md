---
title: Güvenlik denetimleri
description: Kuruluşunuz için Azure App Service değerlendirmek üzere güvenlik denetimlerinin denetim listesini bulun.
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 2586821c4c48f809efb5408c3cdae5e42e3b3fcf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74671444"
---
# <a name="security-controls-for-azure-app-service"></a>Azure App Service için güvenlik denetimleri

Bu makalede, Azure App Service yerleşik olarak bulunan güvenlik denetimleri Belgelenebilir.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Hizmet uç noktası desteği| Yes | App Service için kullanılabilir.| [Azure App Service erişim kısıtlamaları](app-service-ip-restrictions.md)
| VNet ekleme desteği| Yes | App Service ortamlar, müşterinin sanal ağına eklenen tek bir müşteriye adanmış App Service özel uygulamalarıdır. | [App Service Ortamlarına giriş](environment/intro.md)
| Ağ yalıtımı ve güvenlik duvarı desteği| Yes | App Service Genel çok kiracılı bir çeşitlemesi için, müşteriler izin verilen gelen trafiği kilitlemek üzere ağ ACL 'Lerini (IP kısıtlamaları) yapılandırabilir.  App Service ortamlar doğrudan sanal ağlara dağıtılır ve bu nedenle NSG 'ler ile güvenliği sağlanmış olabilir. | [Azure App Service erişim kısıtlamaları](app-service-ip-restrictions.md)
| Zorlamalı tünel desteği| Yes | App Service ortamlar, zorlamalı tünelin yapılandırıldığı bir müşterinin sanal ağına dağıtılabilir. | [App Service Ortamınızı zorlamalı tünelle yapılandırma](environment/forced-tunnel-support.md)

## <a name="monitoring--logging"></a>& günlüğü izleme

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Yes | App Service, Application Insights destekleyen diller için Application Insights tümleştirilir (tam .NET Framework, .NET Core, Java ve Node. JS).  Bkz. [izleyici Azure App Service performansı](../azure-monitor/app/azure-web-apps.md). App Service Ayrıca, Azure Izleyici 'ye uygulama ölçümleri de gönderir. | [Azure App Service uygulamaları izleme](web-sites-monitor.md)
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Yes | App Service nesneler üzerinde gerçekleştirilen tüm yönetim işlemleri [Azure Resource Manager](../azure-resource-manager/index.yml)aracılığıyla oluşur. Bu işlemlerin geçmiş günlükleri hem portalda hem de CLı aracılığıyla kullanılabilir. | [Kaynak sağlayıcısı işlemlerini Azure Resource Manager](../role-based-access-control/resource-provider-operations.md#microsoftweb), [az Monitor Activity-Log](/cli/azure/monitor/activity-log)
| Veri düzlemi günlüğü ve denetimi | Hayır | App Service için veri düzlemi, müşterinin dağıtılmış web sitesi içeriğini içeren bir uzak dosya paylaşımıdır.  Uzak dosya paylaşımının denetimi yoktur. |

## <a name="identity"></a>Kimlik

| Güvenlik denetimi | Evet/Hayır | Notlar |  Belgeler
|---|---|--|
| Kimlik Doğrulaması| Yes | Müşteriler, [Azure Active Directory (Azure AD)](../active-directory/index.yml) ile otomatik olarak tümleştirilen ve App Service varlıklara yönetim erişimi Için diğer OAuth uyumlu kimlik sağlayıcılarının App Service uygulamalar oluşturabilir; tüm erişim, Azure ad kimliği doğrulanmış sorumlu ve Azure Resource Manager RBAC rollerinin bir birleşimiyle denetlenir. | [Azure App Service’de kimlik doğrulaması ve yetkilendirme](overview-authentication-authorization.md)
| Yetkilendirme| Yes | App Service varlıklara yönetim erişimi için tüm erişim, Azure AD kimlik doğrulamalı sorumlu ve Azure Resource Manager RBAC rollerinin bir birleşimiyle denetlenir.  | [Azure App Service’de kimlik doğrulaması ve yetkilendirme](overview-authentication-authorization.md)

## <a name="data-protection"></a>Veri koruma

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Rest 'te sunucu tarafı şifreleme: Microsoft tarafından yönetilen anahtarlar | Yes | Web sitesi dosya içeriği, bekleyen içeriği otomatik olarak şifreleyen Azure Storage 'da depolanır. <br><br>Müşteri tarafından sağlanan gizlilikler bekleyen bir şekilde şifrelenir. Gizlilikler, App Service yapılandırma veritabanlarında depolanırken Rest 'te şifrelenir.<br><br>Yerel olarak bağlı diskler isteğe bağlı olarak, Web siteleri tarafından geçici depolama olarak kullanılabilir (D:\Local ve% TMP%). Yerel olarak bağlı diskler, bekleyen olarak şifrelenmez. | [Bekleyen veriler için Azure depolama şifrelemesi](../storage/common/storage-service-encryption.md)
| Bekleyen sunucu tarafı şifreleme: müşteri tarafından yönetilen anahtarlar (BYOK) | Yes | Müşteriler, Key Vault içinde uygulama gizli dizilerini depolamayı ve çalışma zamanında bunları almayı seçebilir. | [App Service ve Azure Işlevleri için Key Vault başvurularını kullanma (Önizleme)](app-service-key-vault-references.md)
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yes | Müşteriler, Web sitelerini gelen trafik için HTTPS 'yi gerektirecek ve kullanacak şekilde yapılandırabilir.  | [Yalnızca HTTPS Azure App Service oluşturma](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/) (blog gönderisi)
| Şifrelenmiş API çağrıları| Yes | App Service yapılandırmak için yönetim çağrıları, HTTPS üzerinden [Azure Resource Manager](../azure-resource-manager/index.yml) çağrıları aracılığıyla gerçekleşir. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik denetimi | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yes | Yönetim işlemleri için bir App Service yapılandırmanın durumu bir Azure Resource Manager şablonu olarak ve zaman içinde sürümlü şekilde aktarılabilir. Çalışma zamanı işlemlerinde, müşteriler App Service dağıtım yuvaları özelliğini kullanarak bir uygulamanın birden çok farklı canlı sürümünü koruyabilir. | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetleri genelinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
