---
title: Azure Data Lake Storage 1. 'de güvenliğe genel bakış | Microsoft Docs
description: Azure Data Lake Storage 1. daha güvenli bir büyük veri deposu olduğunu anlayın
services: data-lake-store
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: twooley
ms.openlocfilehash: 7e987c56c3a125a03e3a90540313ace1f8adf47a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82086581"
---
# <a name="security-in-azure-data-lake-storage-gen1"></a>Azure Data Lake Storage 1. güvenlik

Birçok kuruluş, iş öngörüleri için büyük veri analizinden yararlanarak akıllı kararlar almasına yardımcı olur. Bir kuruluş karmaşık ve düzenlenmiş bir ortama sahip olabilir ve birçok farklı kullanıcı sayısını artırır. Bir kuruluş için kritik iş verilerinin bireysel kullanıcılara verilen doğru erişim düzeyiyle daha güvenli bir şekilde saklandığından emin olması çok önemlidir. Azure Data Lake Storage 1., bu güvenlik gereksinimlerinin karşılamasına yardımcı olmak için tasarlanmıştır. Bu makalede, aşağıdakiler de dahil olmak üzere Data Lake Storage 1. güvenlik özellikleri hakkında bilgi edinin:

* Kimlik Doğrulaması
* Yetkilendirme
* Ağ yalıtımı
* Veri koruma
* Denetim

## <a name="authentication-and-identity-management"></a>Kimlik doğrulama ve kimlik yönetimi

Kimlik doğrulaması, Kullanıcı Data Lake Storage 1. veya Data Lake Storage 1. bağlanan herhangi bir hizmetle etkileşime geçtiğinde bir kullanıcının kimliğinin doğrulanması işlemidir. Kimlik yönetimi ve kimlik doğrulaması için Data Lake Storage 1., kullanıcıların ve grupların yönetimini kolaylaştıran kapsamlı bir kimlik ve erişim yönetimi bulut çözümü olan [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md)kullanır.

Her Azure aboneliği, bir Azure Active Directory örneğiyle ilişkilendirilebilir. Yalnızca Azure Active Directory hizmetinizde tanımlanan kullanıcılar ve hizmet kimlikleri Data Lake Storage 1. hesabınıza Azure portal, komut satırı araçlarını veya kuruluşunuzun Data Lake Storage 1. SDK kullanarak oluşturduğu istemci uygulamalarını kullanarak erişebilir. Merkezi erişim denetimi mekanizması olarak Azure Active Directory kullanmanın temel avantajları şunlardır:

* Basitleştirilmiş kimlik yaşam döngüsü yönetimi. Bir kullanıcının veya hizmetin kimliği (hizmet sorumlusu kimliği), dizindeki hesabı silerek veya devre dışı bırakarak hızlı bir şekilde oluşturulabilir ve hızlı bir şekilde iptal edilebilir.
* Multi-Factor Authentication. [Multi-Factor Authentication](../active-directory/authentication/multi-factor-authentication.md) , Kullanıcı oturum açma işlemleri ve işlemler için ek bir güvenlik katmanı sağlar.
* OAuth veya OpenID gibi standart bir açık protokol aracılığıyla herhangi bir istemciden kimlik doğrulaması.
* Kurumsal Dizin Hizmetleri ve bulut kimlik sağlayıcılarıyla Federasyon.

## <a name="authorization-and-access-control"></a>Yetkilendirme ve erişim denetimi

Azure Active Directory bir kullanıcının kimliğini doğruladıktan sonra, kullanıcının Data Lake Storage 1. erişebilmeleri için yetkilendirme denetimleri erişim izinleri Data Lake Storage 1.. Data Lake Storage 1. hesapla ilgili ve verilerle ilgili etkinliklere yönelik yetkilendirmeyi aşağıdaki şekilde ayırır:

* Hesap yönetimi için Azure tarafından sunulan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC)
* Depodaki verilere erişmek için POSIX ACL 'SI

### <a name="rbac-for-account-management"></a>Hesap yönetimi için RBAC

Dört temel rol, varsayılan olarak Data Lake Storage 1. için tanımlanmıştır. Roller, Azure portal, PowerShell cmdlet 'leri ve REST API 'Leri aracılığıyla Data Lake Storage 1. hesapta farklı işlemlere izin verir. Sahip ve katkıda bulunan rolleri, hesapta çeşitli yönetim işlevleri gerçekleştirebilir. Okuyucu rolünü yalnızca hesap yönetimi verilerini görüntüleyen kullanıcılara atayabilirsiniz.

![RBAC rolleri](./media/data-lake-store-security-overview/rbac-roles.png "RBAC rolleri")

Roller hesap yönetimi için atanmış olsa da, bazı rollerin verilere erişimi etkilediği unutulmamalıdır. Bir kullanıcının dosya sisteminde gerçekleştirebileceği işlemlere erişimi denetlemek için ACL 'Leri kullanmanız gerekir. Aşağıdaki tabloda, varsayılan roller için yönetim haklarının ve veri erişim haklarının bir özeti gösterilmektedir.

| Roller | Yönetim hakları | Veri erişim hakları | Açıklama |
| --- | --- | --- | --- |
| Atanan rol yok |Hiçbiri |ACL 'ye tabidir |Kullanıcı Data Lake Storage 1. taramak için Azure portal veya Azure PowerShell cmdlet 'lerini kullanamaz. Kullanıcı yalnızca komut satırı araçlarını kullanabilir. |
| Sahip |Tümü |Tümü |Sahip rolü bir süper kullanıcı. Bu rol her şeyi yönetebilir ve verilere tam erişim sağlayabilir. |
| Okuyucu |Salt okunur |ACL 'ye tabidir |Okuyucu rolü, hesap yönetimiyle ilgili her şeyi görüntüleyebilir, örneğin hangi rolün atandığı kullanıcı. Okuyucu rolü herhangi bir değişiklik yapamaz. |
| Katılımcı |Rol Ekle ve Kaldır dışında tümü |ACL 'ye tabidir |Katkıda bulunan rolü, bir hesabın dağıtım ve uyarı oluşturma ve yönetme gibi bazı yönlerini yönetebilir. Katkıda bulunan rolü rol ekleyemez veya kaldıramaz. |
| Kullanıcı Erişimi Yöneticisi |Rol Ekleme ve kaldırma |ACL 'ye tabidir |Kullanıcı erişimi Yöneticisi rolü, hesaplara Kullanıcı erişimini yönetebilir. |

Yönergeler için bkz. [Data Lake Storage 1. hesaplara Kullanıcı veya güvenlik grupları atama](data-lake-store-secure-data.md#assign-users-or-security-groups-to-data-lake-storage-gen1-accounts).

### <a name="using-acls-for-operations-on-file-systems"></a>Dosya sistemlerindeki işlemler için ACL 'Leri kullanma

Data Lake Storage 1., Hadoop Dağıtılmış Dosya Sistemi (bir) gibi hiyerarşik bir dosya sistemidir ve [POSIX ACL 'lerini](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)destekler. Sahip rolü, sahipler grubu ve diğer kullanıcılar ve gruplar için kaynak için okuma (r), yazma (w) ve yürütme (x) izinlerini denetler. Data Lake Storage 1., ACL 'Ler kök klasörde, alt klasörlerde ve tek tek dosyalarda etkinleştirilebilir. ACL 'Lerin Data Lake Storage 1. bağlamında nasıl çalıştığı hakkında daha fazla bilgi için, bkz. [Data Lake Storage 1. Access Control](data-lake-store-access-control.md).

[Güvenlik gruplarını](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)kullanarak birden çok kullanıcı için ACL tanımlamanızı öneririz. Bir güvenlik grubuna kullanıcı ekleyin ve ardından bir dosya veya klasörün ACL 'Lerini bu güvenlik grubuna atayın. Atanan izinler için en fazla 28 girişe sınırlı olduğunuzdan, atanan izinler sağlamak istediğinizde bu kullanışlıdır. Azure Active Directory güvenlik grupları kullanarak Data Lake Storage 1. depolanan verilerin daha iyi bir şekilde güvenliğini sağlama hakkında daha fazla bilgi için, bkz. [Data Lake Storage 1. dosya sistemine Kullanıcı veya güvenlik grubu ACL olarak atama](data-lake-store-secure-data.md#filepermissions).

![Erişim izinlerini listeleme](./media/data-lake-store-security-overview/adl.acl.2.png "Erişim izinlerini listeleme")

## <a name="network-isolation"></a>Ağ yalıtımı

Ağ düzeyinde veri deponuza erişimi denetlemeye yardımcı olması için Data Lake Storage 1. kullanın. Güvenlik duvarları oluşturabilir ve güvenilen istemcileriniz için bir IP adresi aralığı tanımlayabilirsiniz. Bir IP adresi aralığı ile yalnızca tanımlı aralıkta bir IP adresi olan istemciler Data Lake Storage 1. bağlanabilir.

![Güvenlik Duvarı ayarları ve IP erişimi](./media/data-lake-store-security-overview/firewall-ip-access.png "Güvenlik Duvarı ayarları ve IP adresi")

Azure sanal ağları (VNet) Data Lake Gen 1 için hizmet etiketlerini destekler. Hizmet etiketi, belirli bir Azure hizmetinden bir IP adresi önekleri grubunu temsil eder. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir. Daha fazla bilgi için bkz. [Azure hizmet etiketlerine genel bakış](../virtual-network/service-tags-overview.md).

## <a name="data-protection"></a>Veri koruma

Data Lake Storage 1. verilerinizi yaşam döngüsü boyunca korur. Data Lake Storage 1. veriler için, ağ üzerinden verileri güvenli hale getirmek üzere sektör standardı Aktarım Katmanı Güvenliği (TLS 1,2) protokolünü kullanır.

![Data Lake Storage 1. şifreleme](./media/data-lake-store-security-overview/adls-encryption.png "Data Lake Storage 1. şifreleme")

Data Lake Storage 1., hesapta depolanan veriler için de şifreleme sağlar. Verilerinizin şifrelenmesini tercih edebilir ya da şifrelemeyi kabul etmeyebilirsiniz. Şifrelemeyi tercih ediyorsanız, Data Lake Storage 1. depolanan veriler kalıcı medyada depolanmadan önce şifrelenir. Böyle bir durumda Data Lake Storage 1., verileri kalıcı hale getirmeyi ve verileri çözmeden önce otomatik olarak şifreler, bu nedenle verilere erişen istemciye tamamen saydamdır. İstemci tarafında verileri şifrelemek/şifrelerini çözmek için gereken kod değişikliği yoktur.

Anahtar yönetimi için Data Lake Storage 1., ana şifreleme anahtarlarınızı (MEKs) yönetmek için, Data Lake Storage 1. depolanan verilerin şifresini çözmek için gereken iki mod sağlar. Sizin için, MEKs 'leri Data Lake Storage 1. yönetebilir veya Azure Key Vault hesabınızı kullanarak MEKs 'in sahipliğini tutmayı seçebilirsiniz. Data Lake Storage 1. bir hesap oluştururken anahtar yönetiminin modunu belirtirsiniz. Şifreleme ile ilgili yapılandırma sağlama hakkında daha fazla bilgi için bkz. [Azure portalını kullanarak Azure Data Lake Storage 1. kullanmaya başlama](data-lake-store-get-started-portal.md).

## <a name="activity-and-diagnostic-logs"></a>Etkinlik ve tanılama günlükleri

Hesap yönetimiyle ilgili etkinliklere veya verilerle ilgili etkinliklere yönelik günlükleri arayıp aradığınıza bağlı olarak etkinlik veya tanılama günlüklerini kullanabilirsiniz.

* Hesap yönetimi ile ilgili etkinlikler Azure Resource Manager API 'Leri kullanır ve Azure portal etkinlik günlükleri aracılığıyla ortaya çıkmış.
* Verilerle ilgili etkinlikler weblerrest API 'Lerini kullanır ve tanılama günlükleri aracılığıyla Azure portal ortaya çıkmış.

### <a name="activity-log"></a>Etkinlik günlüğü

Yönetmeliklerle uyum sağlamak için, bir kuruluş belirli olaylara uyaması gerekiyorsa hesap yönetimi etkinliklerinin yeterli denetim izlerini gerektirebilir. Data Lake Storage 1. yerleşik izlemeye sahiptir ve tüm hesap yönetimi etkinliklerini günlüğe kaydeder.

Hesap yönetimi denetim izleri için, günlüğe kaydetmek istediğiniz sütunları görüntüleyin ve seçin. Etkinlik günlüklerini Azure depolama 'ya de aktarabilirsiniz.

![Etkinlik günlüğü](./media/data-lake-store-security-overview/activity-logs.png "Etkinlik günlüğü")

Etkinlik günlükleriyle çalışma hakkında daha fazla bilgi için bkz. [kaynaklardaki eylemleri denetlemek için etkinlik günlüklerini görüntüleme](../azure-resource-manager/management/view-activity-logs.md).

### <a name="diagnostics-logs"></a>Tanılama günlükleri

Azure portal veri erişimi denetimi ve tanılama günlüğünü etkinleştirebilir ve günlükleri bir Azure Blob depolama hesabına, bir olay hub 'ına veya Azure Izleyici günlüklerine gönderebilirsiniz.

![Tanılama günlükleri](./media/data-lake-store-security-overview/diagnostic-logs.png "Tanılama günlükleri")

Data Lake Storage 1. ile tanılama günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Data Lake Storage 1. için tanılama günlüklerine erişme](data-lake-store-diagnostic-logs.md).

## <a name="summary"></a>Özet

Kurumsal müşteriler, güvenli ve kullanımı kolay bir veri analizi bulut platformu talep talebinde bulunur. Data Lake Storage 1., bu gereksinimlerin kimlik yönetimi ve kimlik doğrulaması aracılığıyla Azure Active Directory tümleştirme, ACL tabanlı yetkilendirme, ağ yalıtımı, aktarım sırasında veri şifrelemesi ve denetim aracılığıyla ele alınmasına yardımcı olmak için tasarlanmıştır.

Data Lake Storage 1. yeni özellikleri görmek isterseniz, görüşlerinizi [Data Lake Storage 1. UserVoice forumundaki](https://feedback.azure.com/forums/327234-data-lake)bize gönderin.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure Data Lake Storage 1. genel bakış](data-lake-store-overview.md)
* [Data Lake Storage 1. kullanmaya başlayın](data-lake-store-get-started-portal.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
