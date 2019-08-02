---
title: Azure hizmetleri için güvenlik öznitelikleri
description: Azure hizmetlerini değerlendirmek için güvenlik özniteliklerinin denetim listesi
services: security
documentationcenter: ''
author: msmbaldwin
manager: barbkess
ms.service: security
ms.subservice: security-fundamentals
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: mbaldwin
ms.openlocfilehash: be39a623dc5dac09dcfe47de67e025191b5c7de3
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727496"
---
# <a name="security-attributes-for-azure-services"></a>Azure hizmetleri için güvenlik öznitelikleri

Bu makale, seçili Azure hizmetleri için güvenlik özniteliklerini toplar. Güvenlik özniteliği bir Azure hizmeti 'nin kalitesi veya özelliğidir. Hizmetin güvenlik açıklarını önleme, algılama ve yanıtlama yeteneğine katkıda bulunur.

Güvenlik öznitelikleri şöyle kategorize edilir:
* Önleyici
* Ağ kesimleme
* Algılama
* Kimlik ve erişim yönetimi desteği
* Denetim izi
* Erişim denetimleri (kullanılıyorsa)
* Yapılandırma yönetimi (kullanılıyorsa)

Her kategoride, bir özniteliğin kullanılıp kullanılmadığını göstermek için "Evet" veya "Hayır" görüntülenir. Bazı hizmetlerde, geçerli olmayan bir öznitelik için "N/A" göstereceğiz. Ayrıca, bir öznitelik hakkında daha fazla bilgi için bir de veya bir bağlantı sağlayabiliriz.

## <a name="api-managementazureapi-managementapi-management-security-attributes"></a>[API Management](/azure/api-management/api-management-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Sertifikalar, anahtarlar ve gizli-adlandırılmış değerler gibi hassas veriler hizmet tarafından yönetilen hizmet örneği anahtarları ile şifrelenir. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | [Express Route](/azure/expressroute/index) ve VNET şifrelemesi, [Azure ağı](/azure/virtual-network/index)tarafından sağlanır. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır | Tüm şifreleme anahtarları hizmet örneği başına alınır ve hizmet yönetilir. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | Yönetim düzlemi çağrıları TLS üzerinden [Azure Resource Manager](/azure/azure-resource-manager/index) üzerinden yapılır. Geçerli bir JSON Web belirteci (JWT) gerekiyor.  Veri düzlemi çağrıları TLS ve desteklenen kimlik doğrulama mekanizmalarından biri (örneğin, istemci sertifikası veya JWT) ile güvenli hale getirilmiş olabilir.
 |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Sırasıyla ağ güvenlik grupları (NSG) ve Azure Application Gateway (veya diğer yazılım gereci) kullanma. |
| Zorlamalı tünel desteği| Evet | Azure ağı Zorlamalı tünel sağlar. |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | |
| Authorization| Evet | |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | [Azure Izleyici etkinlik günlükleri](/azure/azure-monitor/platform/activity-logs-overview) |
| Veri düzlemi günlüğü ve denetimi| Evet | [Azure izleyici tanılama günlükleri](/azure/azure-monitor/platform/diagnostic-logs-overview) ve (isteğe bağlı olarak) [Azure Application Insights](/azure/azure-monitor/app/app-insights-overview).|

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure API Management DevOps kaynak setini](https://aka.ms/apimdevops) kullanma |

### <a name="vulnerability-scans-false-positives"></a>Güvenlik açığı yanlış pozitifleri tarar

Bu bölüm, Azure API Management etkilemeyen yaygın güvenlik açıklarını belgelemektedir.

| Güvenlik açığı               | Açıklama                                                                                                                                                                                                                                                                                                               |
|-----------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Bilet taşma payı (CVE-2016-9244) | Bilet taşma payı, bazı F5 ürünlerinde bulunan TLS Sessionbilet uzantısının uygulanmasında güvenlik açığıdır. Başlatılmamış bellekten 31 baytlık verilerin sızıntısını ("bleden") sağlar. Bunun nedeni, istemciden geçirilen bir oturum kimliği olan bir oturum kimliğini doldurmasından kaynaklanır ve bu da BT 32 bit uzunluğunda hale gelir. |


## <a name="app-serviceazureapp-serviceapp-service-security-attributes"></a>[App Service](/azure/app-service/app-service-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet | Web sitesi dosya içeriği, bekleyen içeriği otomatik olarak şifreleyen Azure Storage 'da depolanır. [Bekleyen veriler için bkz. Azure depolama şifrelemesi](/azure/storage/common/storage-service-encryption).<br><br>Müşteri tarafından sağlanan gizlilikler bekleyen bir şekilde şifrelenir. Gizlilikler, App Service yapılandırma veritabanlarında depolanırken Rest 'te şifrelenir.<br><br>Yerel olarak bağlı diskler isteğe bağlı olarak, Web siteleri tarafından geçici depolama olarak kullanılabilir (D:\Local ve% TMP%). Yerel olarak bağlı diskler, bekleyen olarak şifrelenmez. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | Müşteriler, Web sitelerini gelen trafik için HTTPS 'yi gerektirecek ve kullanacak şekilde yapılandırabilir. Bkz. blog gönderisi [yalnızca HTTPS Azure App Service oluşturma](https://blogs.msdn.microsoft.com/benjaminperkins/2017/11/30/how-to-make-an-azure-app-service-https-only/). |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Evet | Müşteriler, Key Vault içinde uygulama gizli dizilerini depolamayı ve çalışma zamanında bunları almayı seçebilir. Bkz. [App Service ve Azure işlevleri için Key Vault başvurularını kullanma (Önizleme)](/azure/app-service/app-service-key-vault-references).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | App Service yapılandırmak için yönetim çağrıları, HTTPS üzerinden [Azure Resource Manager](/azure/azure-resource-manager/index) çağrıları aracılığıyla gerçekleşir. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Şu anda App Service için önizlemede kullanılabilir. Bkz. [Azure App Service erişim kısıtlamaları](/azure/app-service/app-service-ip-restrictions). |
| VNet ekleme desteği| Evet | App Service ortamlar, müşterinin sanal ağına eklenen tek bir müşteriye adanmış App Service özel uygulamalarıdır. Bkz. [App Service ortamlarına giriş](/azure/app-service/environment/intro). |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | App Service Genel çok kiracılı bir çeşitlemesi için, müşteriler izin verilen gelen trafiği kilitlemek üzere ağ ACL 'Lerini (IP kısıtlamaları) yapılandırabilir.  Bkz. [Azure App Service erişim kısıtlamaları](/azure/app-service/app-service-ip-restrictions).  App Service ortamlar doğrudan sanal ağlara dağıtılır ve bu nedenle NSG 'ler ile güvenliği sağlanmış olabilir. |
| Zorlamalı tünel desteği| Evet | App Service ortamlar, zorlamalı tünelin yapılandırıldığı bir müşterinin sanal ağına dağıtılabilir. Müşterilerin [App Service ortamı zorlamalı tünelleme Ile yapılandırma](/azure/app-service/environment/forced-tunnel-support)yönergelerini izlemesi gerekir. |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | App Service, Application Insights destekleyen diller için Application Insights tümleştirilir (tam .NET Framework, .NET Core, Java ve Node. JS).  Bkz. [izleyici Azure App Service performansı](/azure/azure-monitor/app/azure-web-apps). App Service Ayrıca, Azure Izleyici 'ye uygulama ölçümleri de gönderir. Bkz. [Azure App Service uygulamaları izleme](/azure/app-service/web-sites-monitor). |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Müşteriler, otomatik olarak [Azure Active Directory (Azure AD)](/azure/active-directory/index) ile tümleştirilen ve diğer OAuth uyumlu kimlik sağlayıcılarının yanı sıra App Service uygulamalar oluşturabilir. bkz. [Azure App Service kimlik doğrulaması ve yetkilendirme](/azure/app-service/overview-authentication-authorization). App Service varlıklara yönetim erişimi için tüm erişim, Azure AD kimlik doğrulamalı sorumlu ve Azure Resource Manager RBAC rollerinin bir birleşimiyle denetlenir. |
| Authorization| Evet | App Service varlıklara yönetim erişimi için tüm erişim, Azure AD kimlik doğrulamalı sorumlu ve Azure Resource Manager RBAC rollerinin bir birleşimiyle denetlenir.  |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | App Service nesneler üzerinde gerçekleştirilen tüm yönetim işlemleri [Azure Resource Manager](/azure/azure-resource-manager/index)aracılığıyla oluşur. Bu işlemlerin geçmiş günlükleri hem portalda hem de CLı aracılığıyla kullanılabilir. bkz. [Azure Resource Manager kaynak sağlayıcısı işlemleri](/azure/role-based-access-control/resource-provider-operations#microsoftweb) ve [az Monitor Activity-Log](/cli/azure/monitor/activity-log). |
| Veri düzlemi günlüğü ve denetimi | Hayır | App Service için veri düzlemi, müşterinin dağıtılmış web sitesi içeriğini içeren bir uzak dosya paylaşımıdır.  Uzak dosya paylaşımının denetimi yoktur. |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Yönetim işlemleri için bir App Service yapılandırmanın durumu bir Azure Resource Manager şablonu olarak ve zaman içinde sürümlü şekilde aktarılabilir. Çalışma zamanı işlemlerinde, müşteriler App Service dağıtım yuvaları özelliğini kullanarak bir uygulamanın birden çok farklı canlı sürümünü koruyabilir. | 



## <a name="azure-resource-managerazureazure-resource-managerazure-resource-manager-security-attributes"></a>[Azure Resource Manager](/azure/azure-resource-manager/azure-resource-manager-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | HTTPS/TLS. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Yok | Azure Resource Manager hiçbir müşteri içeriği depolar, yalnızca denetim verileri. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Evet | |
| Şifrelenmiş API çağrıları| Evet | |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır | |
| VNet ekleme desteği| Evet | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |
| Zorlamalı tünel desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Hayır | |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | [Azure Active Directory](/azure/active-directory) tabanlı.|
| Authorization| Evet | |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Etkinlik günlükleri, kaynaklarınız üzerinde gerçekleştirilen tüm yazma işlemlerini (PUT, POST, SILME) kullanıma sunar; bkz. [kaynaklardaki eylemleri denetlemek için etkinlik günlüklerini görüntüleme](/azure/azure-resource-manager/resource-group-audit). |
| Veri düzlemi günlüğü ve denetimi| Yok | |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet |  |


## <a name="azure-backupazurebackupbackup-security-attributes"></a>[Azure Backup](/azure/backup/backup-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri)| Evet | Depolama hesapları için depolama hizmeti şifrelemesini kullanma. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Hayır | HTTPS kullanma. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Hayır |  |
| Şifrelenmiş API çağrıları| Evet |  |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| VNet ekleme desteği| Hayır |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Zorlamalı tünel VM yedeklemesi için desteklenir. Zorlamalı tünel, VM 'Ler içinde çalışan iş yükleri için desteklenmez. |
| Zorlamalı tünel desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Log Analytics tanılama günlükleri aracılığıyla desteklenir. Daha fazla bilgi için bkz. [Log Analytics kullanarak korumalı iş yüklerini izleme Azure Backup](https://azure.microsoft.com/blog/monitor-all-azure-backup-protected-workloads-using-log-analytics/) . |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Kimlik doğrulaması Azure Active Directory. |
| Authorization| Evet | Oluşturulan müşteri ve yerleşik RBAC rolleri kullanılır. Daha fazla bilgi için [Azure Backup kurtarma noktalarını yönetmek üzere rol tabanlı Access Control kullanma](/azure/backup/backup-rbac-rs-vault) konusuna bakın. |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Azure portal müşterinin tetiklediği tüm eylemler etkinlik günlüklerine kaydedilir. |
| Veri düzlemi günlüğü ve denetimi| Hayır | Azure Backup veri düzlemine doğrudan ulaşılamıyor.  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet|  |

## <a name="cosmos-dbazurecosmos-dbcosmos-db-security-attributes"></a>[Cosmos DB](/azure/cosmos-db/cosmos-db-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet | Tüm Cosmos DB veritabanları ve yedeklemeler varsayılan olarak şifrelenir; [Azure Cosmos DB veri şifrelemeyi](/azure/cosmos-db/database-encryption-at-rest)inceleyin. Müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme desteklenmez. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Tüm Azure Cosmos DB verileri aktarım sırasında şifrelenir. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Evet | Yalnızca API Premium tablolarında. Tüm API 'Ler bu özelliği desteklemez. Bkz [. Azure Cosmos DB giriş: Tablo API'si](/azure/cosmos-db/table-introduction). |
| Şifrelenmiş API çağrıları| Evet | Azure Cosmos DB tüm bağlantıları HTTPS 'yi destekler. Azure Cosmos DB TLS 1,2 bağlantılarını da destekler, ancak bu henüz zorlanmaz. Müşteriler, son ucunda daha düşük düzey TLS 'yi devre dışı bırakır. Cosmos DB bağlandıklarından emin olabilirler.  |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Evet | VNet hizmet uç noktası ile, bir Azure Cosmos DB hesabını yalnızca bir sanal ağın (VNet) belirli bir alt ağından erişime izin verecek şekilde yapılandırabilirsiniz. Ayrıca, VNet erişimini güvenlik duvarı kurallarıyla birleştirebilirsiniz.  Bkz. [sanal ağlardan erişim Azure Cosmos DB](/azure/cosmos-db/vnet-service-endpoint). |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Güvenlik Duvarı desteğiyle, Azure Cosmos hesabınızı yalnızca onaylanan bir IP adresi kümesinden, IP adresi aralığına ve/veya bulut hizmetlerinden erişime izin verecek şekilde yapılandırabilirsiniz. Bkz. [Azure Cosmos DB IP güvenlik duvarını yapılandırma](/azure/cosmos-db/how-to-configure-firewall).|
| Zorlamalı tünel desteği| Evet | , Sanal makinelerin bulunduğu VNet üzerinde istemci tarafında yapılandırılabilir.   |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure Cosmos DB gönderilen tüm istekler günlüğe kaydedilir. Azure [izleme](/azure/azure-monitor/overview), Azure ölçümleri, Azure denetim günlüğü desteklenir.  Veri düzlemi isteklerine karşılık gelen bilgileri, sorgu çalışma zamanı istatistiklerini, sorgu metnini, MongoDB isteklerini günlüğe kaydedebilirsiniz. Ayrıca, uyarıları da ayarlayabilirsiniz. |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Evet, veritabanı hesabı düzeyinde; veri düzlemi düzeyinde Cosmos DB, kaynak belirteçlerini ve anahtar erişimini kullanır. |
| Authorization| Evet | Azure Cosmos hesabında ana anahtarlar (birincil ve ikincil) ve kaynak belirteçleri ile desteklenir. Ana anahtarlarla verilere okuma/yazma veya salt okuma erişimi alabilirsiniz. Kaynak belirteçleri, belgeler ve kapsayıcılar gibi kaynaklara sınırlı zamanlı erişime izin verir. |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Güvenlik duvarları, VNET 'ler, anahtar erişimi ve ıAM gibi hesap düzeyindeki işlemler için Azure etkinlik günlüğü. |
| Veri düzlemi günlüğü ve denetimi | Evet | Kapsayıcı oluşturma, sağlama işleme, dizin oluşturma ilkeleri ve belgelerde CRUD işlemleri gibi kapsayıcı düzeyindeki işlemler için Tanılama izleme günlüğü. |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Hayır  | | 

### <a name="additional-security-attributes-for-cosmos-db"></a>Cosmos DB için ek güvenlik öznitelikleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Çapraz kaynak kaynak paylaşımı (CORS) | Evet | Bkz. [çıkış noktaları arası kaynak paylaşımı 'nı (CORS) yapılandırma](/azure/cosmos-db/how-to-configure-cross-origin-resource-sharing). |


## <a name="event-hubsazureevent-hubsevent-hubs-security-attributes"></a>[Event Hubs](/azure/event-hubs/event-hubs-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) |  Evet | |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet |  |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Hayır | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet |  |
| Zorlamalı tünel desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | |
| Authorization|  Evet | |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi| Evet |   |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | |


## <a name="expressrouteazureexpressrouteexpressroute-security-attributes"></a>[ExpressRoute](/azure/expressroute/expressroute-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) |  Yok | ExpressRoute müşteri verilerini depolamaz. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Hayır | |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Yok |  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | [Azure Resource Manager](/azure/azure-resource-manager/index) ve https üzerinden. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok |  |
| VNet ekleme desteği| Yok | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Her müşteri kendi yönlendirme etki alanında bulunur ve kendi VNet 'ine tünel |
| Zorlamalı tünel desteği| Yok | Sınır Ağ Geçidi Protokolü (BGP) üzerinden. |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [ExpressRoute izleme, ölçümler ve uyarılar](/azure/expressroute/expressroute-monitoring-metrics-alerts).|

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Microsoft için ağ geçidi için hizmet hesabı (GWM) (denetleyici); Geliştirme ve OP için tam zamanında (JıT) erişim. |
| Authorization|  Evet |Microsoft için ağ geçidi için hizmet hesabı (GWM) (denetleyici); Geliştirme ve OP için tam zamanında (JıT) erişim. |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar| 
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi| Hayır |   |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Ağ kaynak sağlayıcısı (NRP) aracılığıyla. |


## <a name="key-vaultkey-vaultkey-vault-security-attributesmd"></a>[Anahtar Kasası](../../key-vault/key-vault-security-attributes.md)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet | Tüm nesneler şifrelenir. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | Tüm iletişimler şifreli API çağrıları aracılığıyla yapılır |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Evet | Müşteri, Key Vault tüm anahtarları denetler. Donanım güvenlik modülü (HSM) tarafından desteklenen anahtarlar belirtildiğinde, FIPS düzey 2 HSM anahtarı, sertifikayı veya parolayı korur. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | HTTPS kullanma. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Sanal ağ (VNet) hizmet uç noktalarını kullanma. |
| VNet ekleme desteği| Hayır |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | VNet güvenlik duvarı kurallarını kullanma. |
| Zorlamalı tünel desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Log Analytics kullanma. |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Kimlik doğrulaması Azure Active Directory. |
| Authorization| Evet | Key Vault erişim Ilkesini kullanma. |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi günlüğü ve denetimi| Evet | Log Analytics kullanma. |
| Veri düzlemi günlüğü ve denetimi| Evet | Log Analytics kullanma. |

### <a name="access-controls"></a>Erişim denetimleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim/Yönetim düzlemi erişim denetimleri | Evet | Azure Resource Manager Rol Tabanlı Access Control (RBAC) |
| Veri düzlemi erişim denetimleri (her hizmet düzeyinde) | Evet | Key Vault erişim Ilkesi |

## <a name="load-balancerload-balancerload-balancer-security-attributesmd"></a>[Yük Dengeleyici](../../load-balancer/load-balancer-security-attributes.md)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Yok | |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Yok | |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Yok | |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | [Azure Resource Manager](/azure/azure-resource-manager/index)üzerinden. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok | |
| VNet ekleme desteği| Yok | . |
| Ağ yalıtımı ve güvenlik duvarı desteği| Yok |  |
| Zorlamalı tünel desteği| Yok | |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [Genel temel Load Balancer Için Azure izleyici günlükleri](/azure/load-balancer/load-balancer-monitor-log). |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Yok |  |
| Authorization| Yok |  |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Bkz. [Genel temel Load Balancer Için Azure izleyici günlükleri](/azure/load-balancer/load-balancer-monitor-log). |
| Veri düzlemi günlüğü ve denetimi | Yok |  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Yok |  | 

## <a name="service-bus-messagingazureservice-bus-messagingservice-bus-messaging-security-attributes"></a>[Service Bus mesajlaşma](/azure/service-bus-messaging/service-bus-messaging-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) |  Varsayılan olarak sunucu tarafı şifreleme için Evet. | Müşteri tarafından yönetilen anahtarlar ve BYOK henüz desteklenmiyor. İstemci tarafı şifrelemesi, istemcinin sorumluluğundadır |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | Standart HTTPS/TLS mekanizmasını destekler. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır |   |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | API çağrıları [Azure Resource Manager](/azure/azure-resource-manager/index) ve HTTPS aracılığıyla yapılır. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet (yalnızca Premium katman) | Sanal ağ hizmeti uç noktaları yalnızca [Service Bus Premium katmanı](/azure/service-bus-messaging/service-bus-premium-messaging) için desteklenir. |
| VNet ekleme desteği| Hayır | |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet (yalnızca Premium katman) |  |
| Zorlamalı tünel desteği| Hayır |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | [Azure izleyici ve uyarılar](/azure/service-bus-messaging/service-bus-metrics-azure-monitor)aracılığıyla desteklenir. |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | [Azure Active Directory yönetilen hizmet kimliği](/azure/service-bus-messaging/service-bus-managed-service-identity)ile yönetilir; bkz. [Service Bus kimlik doğrulaması ve yetkilendirme](/azure/service-bus-messaging/service-bus-authentication-and-authorization).|
| Authorization| Evet | [RBAC](/azure/service-bus-messaging/service-bus-role-based-access-control) (Önizleme) ve SAS belirteci aracılığıyla yetkilendirmeyi destekler; bkz. [Service Bus kimlik doğrulaması ve yetkilendirme](/azure/service-bus-messaging/service-bus-authentication-and-authorization). |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | İşlem günlükleri kullanılabilir; bkz. [Service Bus tanılama günlükleri](/azure/service-bus-messaging/service-bus-diagnostic-logs).  |
| Veri düzlemi günlüğü ve denetimi| Hayır |  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure Resource Manager API](/rest/api/resources/)aracılığıyla kaynak sağlayıcısı sürümü oluşturmayı destekler.|


## <a name="service-bus-relayazureservice-bus-relayservice-bus-relay-security-attributes"></a>[Service Bus Geçişi](/azure/service-bus-relay/service-bus-relay-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) |  Yok | Geçiş bir Web soketi ve verileri kalıcı yapmaz. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | Hizmet için TLS gerekiyor. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır | Yalnızca Microsoft TLS sertifikalarını kullanır.  |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | 'DİR. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Hayır |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Hayır |  |
| Zorlamalı tünel desteği| Yok | Geçiş, TLS tünelidir  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | SAS aracılığıyla. |
| Authorization|  Evet | SAS aracılığıyla. |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | [Azure Resource Manager](/azure/azure-resource-manager/index)üzerinden. |
| Veri düzlemi günlüğü ve denetimi| Evet | Bağlantı başarısı/hatası ve hataları ve günlüğe kaydedildi.  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | [Azure Resource Manager](/azure/azure-resource-manager/index)üzerinden.|

## <a name="service-fabricservice-fabricservice-fabric-security-attributesmd"></a>[Service Fabric](../../service-fabric/service-fabric-security-attributes.md)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet | Müşteri, kümeye ve kümenin oluşturulduğu sanal makine ölçek kümesine sahip. Azure disk şifrelemesi, sanal makine ölçek kümesi üzerinde etkinleştirilebilir. |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | Müşteri, kümeye ve kümenin oluşturulduğu sanal makine ölçek kümesine sahip. Azure disk şifrelemesi, sanal makine ölçek kümesi üzerinde etkinleştirilebilir. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet | Service Fabric API çağrıları Azure Resource Manager aracılığıyla yapılır. Geçerli bir JSON Web belirteci (JWT) gerekiyor. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Evet |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Ağ güvenlik grupları (NSG) kullanma. |
| Zorlamalı tünel desteği| Evet | Azure ağı Zorlamalı tünel sağlar. |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure izleme desteğini ve üçüncü taraf desteğini kullanma. |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Kimlik doğrulaması Azure Active Directory. |
| Authorization| Evet | SFRP aracılığıyla çağrılar için kimlik ve erişim yönetimi (ıAM). Doğrudan küme uç noktasına yapılan çağrılar iki rolü destekler: Kullanıcı ve yönetici. Müşteri, API 'Leri her iki role de eşleyebilir. |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Tüm denetim düzlemi işlemleri, denetim ve onaylar için işlemler aracılığıyla çalışır. |
| Veri düzlemi günlüğü ve denetimi| Yok | Müşteri, kümeye sahip.  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | |

## <a name="sql-databasesql-databasesql-database-security-attributesmd"></a>[SQL Veritabanı](../../sql-database/sql-database-security-attributes.md)

SQL veritabanı hem [tek veritabanı](/azure/sql-database/sql-database-single-index) hem de [yönetilen örnek](/azure/sql-database/sql-database-managed-instance)içerir. Aşağıdaki girişler, aksi belirtilmedikçe her iki teklife de uygulanır.

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet | [Always Encrypted](/azure/sql-database/sql-database-always-encrypted)makalesinde açıklandığı gibi "kullanımda olan şifreleme" olarak adlandırılır. Sunucu tarafı şifreleme, [Saydam veri şifrelemesi](/azure/sql-database/transparent-data-encryption-azure-sql)kullanır.|
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | HTTPS kullanma. |
| CMK veya BYOK gibi şifreleme anahtarı işleme| Evet | Hem hizmet tarafından yönetilen hem de müşteri tarafından yönetilen anahtar işleme sunulur. İkincisi [Azure Key Vault](/azure/key-vault/index)aracılığıyla sunulur. |
| Azure veri Hizmetleri tarafından sunulan sütun düzeyinde şifreleme| Evet | [Always Encrypted](/azure/sql-database/sql-database-always-encrypted)üzerinden. |
| Şifrelenmiş API çağrıları| Evet | HTTPS/SSL kullanma. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | Yalnızca [tek veritabanı](/azure/sql-database/sql-database-single-index) için geçerlidir. |
| Azure sanal ağ ekleme desteği| Evet | Yalnızca [yönetilen örnek](/azure/sql-database/sql-database-managed-instance) için geçerlidir. |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | Hem veritabanı düzeyinde hem de sunucu düzeyinde güvenlik duvarı. Ağ yalıtımı yalnızca [yönetilen örnek](/azure/sql-database/sql-database-managed-instance) içindir. |
| Zorlamalı tünel desteği| Evet | [ExpressRoute](/azure/expressroute/index.yml) VPN aracılığıyla [yönetilen örnek](/azure/sql-database/sql-database-managed-instance) . |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Log Analytics veya Application Insights gibi Azure izleme desteği| Evet | EMVA 'dan SıEM çözümü olan SecureSphere, [SQL denetimi](/azure/sql-database/sql-database-auditing)aracılığıyla [Azure Event Hubs](/azure/event-hubs/index) tümleştirme aracılığıyla da desteklenir. |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Azure Active Directory (Azure AD) |
| Authorization| Evet | None |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim düzlemi ve yönetimi-düzlem günlüğü ve denetimi| Evet | Yalnızca bazı olaylar için Evet |
| Veri düzlemi günlüğü ve denetimi | Evet | [SQL denetimi](/azure/sql-database/sql-database-auditing) aracılığıyla |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma sürümü oluşturma gibi yapılandırma yönetimi desteği| Hayır  | Yok. |

### <a name="additional-security-attributes-for-sql-database"></a>SQL veritabanı için ek güvenlik öznitelikleri

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Koruyucu: güvenlik açığı değerlendirmesi | Evet | Bkz. [SQL güvenlik açığı değerlendirmesi hizmeti, veritabanı güvenlik açıklarını belirlemenize yardımcı olur](/azure/sql-database/sql-vulnerability-assessment). |
| Koruyucu: veri bulma ve sınıflandırma  | Evet | Bkz. [Azure SQL veritabanı ve SQL veri ambarı veri bulma & sınıflandırması](/azure/sql-database/sql-database-data-discovery-and-classification). |
| Algılama: tehdit algılama | Evet | Bkz. [Azure SQL veritabanı Için Gelişmiş tehdit koruması](/azure/sql-database/sql-database-threat-detection-overview). |


## <a name="storageazurestoragecommonstorage-security-attributes"></a>[Depolama](/azure/storage/common/storage-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet |  |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi) | Evet | Standart HTTPS/TLS mekanizmalarını destekler.  Kullanıcılar, hizmete iletilmeden önce verileri de şifreleyebilir. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Evet | Depolama Hizmeti Şifrelemesi bkz. [Azure Key Vault müşteri tarafından yönetilen anahtarları kullanma](/azure/storage/common/storage-service-encryption-customer-managed-keys?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok |  |
| Şifrelenmiş API çağrıları| Evet |  |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet |  |
| VNet ekleme desteği| Yok |  |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | |
| Zorlamalı tünel desteği| Yok |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Azure Izleyici ölçümleri Şu anda kullanılabilir, günlük önizlemesi başlatılıyor |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Azure Active Directory, paylaşılan anahtar, paylaşılan erişim belirteci. |
| Authorization| Evet | RBAC, POSIX ACL 'Leri ve SAS belirteçleri aracılığıyla yetkilendirme desteği |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi | Evet | Etkinlik günlüğünü Azure Resource Manager |
| Veri düzlemi günlüğü ve denetimi| Evet | Hizmet tanılama günlükleri ve Azure Izleyici günlüğü başlangıç önizlemesi  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Azure Resource Manager API 'Leri aracılığıyla kaynak sağlayıcısı sürümlendirmeyi destekleme |

## <a name="virtual-machines-and-virtual-machine-scale-sets"></a>Sanal makineler ve sanal makine ölçek kümeleri

[Linux VM 'leri](/azure/virtual-machines/windows/virtual-machines-windows-security-attributes) | [Windows VM 'leri](/azure/virtual-machines/windows/virtual-machines-windows-security-attributes) | [Sanal Makine Ölçek Kümeleri](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-security-attributes)


### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Evet | Bkz. [Azure 'Da Linux sanal makinesini şifreleme](/azure/virtual-machines/linux/encrypt-disks) ve [Windows VM 'de sanal diskleri şifreleme](/azure/virtual-machines/windows/encrypt-disks). |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | Azure sanal makineleri [ExpressRoute](/azure/expressroute) ve VNET şifrelemesini destekler. Bkz. [VM 'lerde geçiş içi şifreleme](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms). |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Evet | Müşteri tarafından yönetilen anahtarlar desteklenen bir Azure şifreleme senaryosudur; bkz. [Azure şifrelemesi 'ne genel bakış](/azure/security/security-azure-encryption-overview#in-transit-encryption-in-vms).|
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | HTTPS ve SSL aracılığıyla. |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Evet | |
| VNet ekleme desteği| Evet | . |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet |  |
| Zorlamalı tünel desteği| Evet | Bkz. [Azure Resource Manager dağıtım modelini kullanarak Zorlamalı tünel yapılandırma](/azure/vpn-gateway/vpn-gateway-forced-tunneling-rm). |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. Azure ['da bir Linux sanal makinesini izleme ve güncelleştirme](/azure/virtual-machines/linux/tutorial-monitoring) , [Azure 'da bir Windows sanal makinesini izleme ve güncelleştirme](/azure/virtual-machines/windows/tutorial-monitoring). |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet |  |
| Authorization| Evet |  |


### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet |  |
| Veri düzlemi günlüğü ve denetimi | Hayır |  |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet |  | 


## <a name="vpn-gatewayazurevpn-gatewayvpn-gateway-security-attributes"></a>[VPN Gateway](/azure/vpn-gateway/vpn-gateway-security-attributes)

### <a name="preventative"></a>Önleyici

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Bekleyen şifreleme (sunucu tarafı şifreleme, müşteri tarafından yönetilen anahtarlarla sunucu tarafı şifreleme ve diğer şifreleme özellikleri) | Yok | VPN Gateway transit müşteri verileri, müşteri verilerini depolamaz |
| Aktarım sırasında şifreleme (ExpressRoute şifrelemesi, VNet şifreleme ve VNet-VNet şifreleme gibi)| Evet | VPN Gateway, Azure VPN ağ geçitleri ile müşteri şirket içi VPN cihazları (S2S) veya VPN istemcileri (P2S) arasında müşteri paketlerini şifreleyin. VPN ağ geçitleri Ayrıca VNet 'ten VNet 'e şifrelemeyi destekler. |
| Şifreleme anahtarı işleme (CMK, BYOK vb.)| Hayır | Müşteri tarafından belirtilen önceden paylaşılan anahtarlar bekleyen olarak şifrelenir; Ancak henüz CMK ile tümleştirmemelidir. |
| Sütun düzeyinde şifreleme (Azure veri Hizmetleri)| Yok | |
| Şifrelenmiş API çağrıları| Evet | [Azure Resource Manager](/azure/azure-resource-manager/index) ve https üzerinden  |

### <a name="network-segmentation"></a>Ağ kesimleme

| Güvenlik özniteliği | Evet/Hayır | Notlar |
|---|---|--|
| Hizmet uç noktası desteği| Yok | |
| VNet ekleme desteği| Yok | . |
| Ağ yalıtımı ve güvenlik duvarı desteği| Evet | VPN ağ geçitleri her müşteri sanal ağı için ayrılmış sanal makine örnekleridir  |
| Zorlamalı tünel desteği| Evet |  |

### <a name="detection"></a>Algılama

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Azure izleme desteği (Log Analytics, App Insights vb.)| Evet | Bkz. [Azure izleyici tanılama günlükleri/uyarı](/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-log) & [Azure izleyici ölçümleri/uyarısı](/azure/vpn-gateway/vpn-gateway-howto-setup-alerts-virtual-network-gateway-metric).  |

### <a name="identity-and-access-management"></a>Kimlik ve erişim yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Authentication| Evet | Hizmeti yönetmek ve Azure VPN ağ geçidini yapılandırmak için [Azure Active Directory](/azure/active-directory/fundamentals/active-directory-whatis) . |
| Authorization| Evet | [RBAC](/azure/role-based-access-control/overview)aracılığıyla yetkilendirmeyi destekler. |

### <a name="audit-trail"></a>Denetim izi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Denetim ve yönetim düzlemi günlüğü ve denetimi| Evet | Etkinlik günlüğünü Azure Resource Manager. |
| Veri düzlemi günlüğü ve denetimi | Evet | Azure, VPN bağlantısı günlüğü ve denetimi için [tanılama günlüklerini izler](/azure/azure-resource-manager/resource-group-audit) . |

### <a name="configuration-management"></a>Yapılandırma yönetimi

| Güvenlik özniteliği | Evet/Hayır | Notlar|
|---|---|--|
| Yapılandırma yönetimi desteği (yapılandırmanın sürümü oluşturma, vb.)| Evet | Yönetim işlemleri için bir Azure VPN ağ geçidi yapılandırmasının durumu, zaman içinde bir Azure Resource Manager şablonu ve sürümü oluşturulmuş olarak aktarılabilir. | 

