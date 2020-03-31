---
title: Güvenlik denetimleri
description: Kuruluşunuz için Azure Uygulama Hizmetini değerlendirmek için güvenlik denetimleri bir denetim listesi bulun.
author: msmbaldwin
ms.topic: conceptual
ms.date: 09/04/2019
ms.author: mbaldwin
ms.openlocfilehash: 2586821c4c48f809efb5408c3cdae5e42e3b3fcf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74671444"
---
# <a name="security-controls-for-azure-app-service"></a>Azure Uygulama Hizmeti için güvenlik denetimleri

Bu makalede, Azure Uygulama Hizmeti'nde yerleşik güvenlik denetimleri belgeleilmiştir.

[!INCLUDE [Security controls header](../../includes/security-controls-header.md)]

## <a name="network"></a>Ağ

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Hizmet bitiş noktası desteği| Evet | Uygulama Hizmeti için kullanılabilir.| [Azure Uygulaması Hizmeti Erişim Kısıtlamaları](app-service-ip-restrictions.md)
| VNet enjeksiyon desteği| Evet | Uygulama Hizmet Ortamları, Uygulama Hizmeti'nin müşterinin sanal ağına enjekte edilen tek bir müşteriye adanmış özel uygulamalarıdır. | [App Service Ortamlarına giriş](environment/intro.md)
| Ağ Yalıtımı ve Güvenlik Duvarı desteği| Evet | App Service'in genel çok kiracılı varyasyonu için, müşteriler izin verilen gelen trafiği kilitlemek için ağ ABM'lerini (IP Kısıtlamaları) yapılandırabilir.  Uygulama Hizmet Ortamları doğrudan sanal ağlara dağıtılır ve bu nedenle NSG'lerle güvenli hale getirilebilir. | [Azure Uygulaması Hizmeti Erişim Kısıtlamaları](app-service-ip-restrictions.md)
| Zorunlu tünel desteği| Evet | Uygulama Hizmet Ortamları, zorunlu tünellemenin yapılandırıldığı bir müşterinin sanal ağına dağıtılabilir. | [App Service Ortamınızı zorlamalı tünelle yapılandırma](environment/forced-tunnel-support.md)

## <a name="monitoring--logging"></a>İzleme & günlüğe kaydetme

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Azure izleme desteği (Log analitiği, Uygulama öngörüleri, vb.)| Evet | Uygulama Hizmeti, Uygulama Öngörülerini destekleyen diller için Uygulama Öngörüleri (Full .NET Framework, .NET Core, Java ve Node.JS) ile tümleşir.  Bkz. [Azure Uygulama Hizmeti performansını izleyin.](../azure-monitor/app/azure-web-apps.md) Uygulama Hizmeti ayrıca Azure Monitor'a uygulama ölçümleri gönderir. | [Azure Uygulama Hizmeti'ndeki uygulamaları izleme](web-sites-monitor.md)
| Kontrol ve yönetim düzlemi günlüğü ve denetimi| Evet | Uygulama Hizmeti nesnelerinde gerçekleştirilen tüm yönetim işlemleri [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml)aracılığıyla gerçekleştirilir. Bu işlemlerin geçmiş günlükleri hem portalda hem de CLI üzerinden mevcuttur. | [Azure Kaynak Yöneticisi kaynak sağlayıcı işlemleri](../role-based-access-control/resource-provider-operations.md#microsoftweb), az monitör etkinlik [günlüğü](/cli/azure/monitor/activity-log)
| Veri düzlemi günlüğü ve denetimi | Hayır | App Service için veri düzlemi, müşterinin dağıtılmış web sitesi içeriğini içeren uzak bir dosya paylaşımıdır.  Uzak dosya paylaşımı denetlenir. |

## <a name="identity"></a>Kimlik

| Güvenlik kontrolü | Evet/Hayır | Notlar |  Belgeler
|---|---|--|
| Kimlik doğrulaması| Evet | Müşteriler, Uygulama Hizmeti'nde Azure Active [Directory (Azure AD) ile](../active-directory/index.yml) otomatik olarak entegre olan uygulamalar ve diğer OAuth uyumlu kimlik sağlayıcıları App Service varlıklarına yönetim erişimi için, tüm erişim, Azure AD kimlik doğrulaması ana para yöneticisi ve Azure Kaynak Yöneticisi RBAC rollerinin birleşimi tarafından denetlenir. | [Azure App Service’de kimlik doğrulaması ve yetkilendirme](overview-authentication-authorization.md)
| Yetkilendirme| Evet | Uygulama Hizmeti varlıklarına yönetim erişimi için tüm erişim, Azure AD kimlik doğrulaması asıl ve Azure Kaynak Yöneticisi RBAC rollerinin birleşimi tarafından denetlenir.  | [Azure App Service’de kimlik doğrulaması ve yetkilendirme](overview-authentication-authorization.md)

## <a name="data-protection"></a>Veri koruma

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Sunucu tarafında şifreleme istirahat: Microsoft tarafından yönetilen anahtarlar | Evet | Web sitesi dosya içeriği Azure Depolama'da depolanır ve bu da içeriği otomatik olarak yeniden şifreler. <br><br>Müşteri tarafından sağlanan sırlar istirahatte şifrelenir. Uygulamalar Servis yapılandırma veritabanlarında saklanırken, sırlar istirahatte şifrelenir.<br><br>Yerel olarak iliştirilen diskler isteğe bağlı olarak web siteleri tarafından geçici depolama alanı olarak kullanılabilir (D:\yerel ve %TMP%). Yerel olarak bağlı diskler istirahatte şifrelenmez. | [Veriler için Azure Depolama şifrelemesi](../storage/common/storage-service-encryption.md)
| Sunucu tarafında şifreleme istirahat: müşteri tarafından yönetilen anahtarlar (BYOK) | Evet | Müşteriler uygulama sırlarını Key Vault'ta depolamayı ve çalışma zamanında almayı seçebilirler. | [Uygulama Hizmeti ve Azure İşlevleri için Anahtar Kasa başvurularını kullanma (önizleme)](app-service-key-vault-references.md)
| Sütun düzeyi şifreleme (Azure Veri Hizmetleri)| Yok | |
| Aktarım sırasında şifreleme (ExpressRoute şifreleme, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Müşteriler web sitelerini gelen trafik için HTTPS'yi gerektirecek ve kullanacak şekilde yapılandırabilir.  | [Yalnızca Azure Uygulama Hizmeti HTTPS nasıl yapılır](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/) (blog gönderimi)
| API şifreli aramalar| Evet | Uygulama Hizmetini yapılandırmak için yönetim çağrıları, HTTPS üzerinden [Azure Kaynak Yöneticisi](../azure-resource-manager/index.yml) çağrıları aracılığıyla gerçekleşir. |

## <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik kontrolü | Evet/Hayır | Notlar | Belgeler
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü, vb.)| Evet | Yönetim işlemleri için, Bir Uygulama Hizmeti yapılandırmasının durumu Azure Kaynak Yöneticisi şablonu olarak dışa aktarılabilir ve zaman içinde sürümlendirilebilir. Çalışma zamanı işlemleri için müşteriler, Uygulama Hizmeti dağıtım yuvaları özelliğini kullanarak bir uygulamanın birden çok farklı canlı sürümü ne olabilir. | 

## <a name="next-steps"></a>Sonraki adımlar

- [Azure hizmetlerinde yerleşik güvenlik denetimleri](../security/fundamentals/security-controls.md)hakkında daha fazla bilgi edinin.
