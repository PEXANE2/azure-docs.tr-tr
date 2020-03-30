---
title: Azure Veri Gölü Depolama Gen1'de güvenliğe genel bakış | Microsoft Dokümanlar
description: Azure Veri Gölü Depolama Gen1'in nasıl daha güvenli bir büyük veri deposu olduğunu anlayın
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
editor: cgronlun
ms.assetid: ebd5b2ac-c5cc-46d4-9cfd-1a1ee70024c2
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 03/26/2018
ms.author: twooley
ms.openlocfilehash: 4e640aa1cb02174c935c0f7c1d61ab2fca5ea046
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75974572"
---
# <a name="security-in-azure-data-lake-storage-gen1"></a>Azure Veri Gölü Depolama Gen1'de Güvenlik
Birçok işletme, akıllı kararlar almalarına yardımcı olmak için iş öngörüleri için büyük veri analitiğinden yararlanmaktadır. Bir kuruluşun karmaşık ve düzenli bir ortamı olabilir ve giderek artan sayıda farklı kullanıcı vardır. Bir kuruluş için kritik iş verilerinin tek tek kullanıcılara doğru erişim düzeyiyle daha güvenli bir şekilde depolanması için hayati önem taşır. Azure Veri Gölü Depolama Gen1, bu güvenlik gereksinimlerini karşılamaya yardımcı olmak üzere tasarlanmıştır. Bu makalede, Aşağıdakiler de dahil olmak üzere Veri Gölü Depolama Gen1'in güvenlik yetenekleri hakkında bilgi edinin:

* Kimlik doğrulaması
* Yetkilendirme
* Ağ yalıtımı
* Veri koruma
* Denetim

## <a name="authentication-and-identity-management"></a>Kimlik doğrulama ve kimlik yönetimi
Kimlik doğrulama, kullanıcı Veri Gölü Depolama Gen1 ile veya Veri Gölü Depolama Gen1'e bağlanan herhangi bir hizmetle etkileşimde bulunduğunda kullanıcının kimliğinin doğrulanması işlemidir. Kimlik yönetimi ve kimlik doğrulama için Data Lake Storage Gen1, kullanıcıların ve grupların yönetimini kolaylaştıran kapsamlı bir kimlik ve erişim yönetimi bulut çözümü olan [Azure Active Directory'yi](../active-directory/fundamentals/active-directory-whatis.md)kullanır.

Her Azure aboneliği, Azure Etkin Dizin örneğiyle ilişkilendirilebilir. Yalnızca Azure Etkin Dizin hizmetinizde tanımlanan kullanıcılar ve hizmet kimlikleri, Azure portalı, komut satırı araçlarını kullanarak veya Kuruluşunuzun Oluşturduğu Müşteri Uygulamaları aracılığıyla Veri Gölü Depolama Gen1 hesabınıza erişebilir Depolama Gen1 SDK. Azure Active Directory'yi merkezi bir erişim denetim mekanizması olarak kullanmanın başlıca avantajları şunlardır:

* Basitleştirilmiş kimlik yaşam döngüsü yönetimi. Bir kullanıcının veya hizmetin (hizmet temel kimliği) kimliği, dizindeki hesabı silerek veya devre dışı bırakarak hızlı bir şekilde oluşturulabilir ve hızlı bir şekilde iptal edilebilir.
* Çok faktörlü kimlik doğrulama. [Çok faktörlü kimlik doğrulama,](../active-directory/authentication/multi-factor-authentication.md) kullanıcı oturum açma ve hareketler için ek bir güvenlik katmanı sağlar.
* OAuth veya OpenID gibi standart bir açık protokol aracılığıyla herhangi bir istemciden kimlik doğrulaması.
* Kurumsal dizin hizmetleri ve bulut kimlik sağlayıcıları ile federasyon.

## <a name="authorization-and-access-control"></a>Yetkilendirme ve erişim kontrolü
Azure Active Directory, kullanıcının Veri Gölü Depolama Gen1'e erişebilmeleri için bir kullanıcının kimliğini doğruladıktan sonra, yetkilendirme Veri Gölü Depolama Gen1 için erişim izinlerini denetler. Data Lake Storage Gen1, hesapla ilgili ve verilerle ilgili faaliyetler için yetkilendirmeyi aşağıdaki şekilde ayırır:

* Azure tarafından hesap yönetimi için sağlanan [rol tabanlı erişim denetimi](../role-based-access-control/overview.md) (RBAC)
* Mağazadaki verilere erişmek için POSIX ACL

### <a name="rbac-for-account-management"></a>Hesap yönetimi için RBAC
Varsayılan olarak Veri Gölü Depolama Gen1 için dört temel rol tanımlanır. Roller, Azure portalı, PowerShell cmdlets ve REST API'leri aracılığıyla Veri Gölü Depolama Gen1 hesabında farklı işlemlere izin verir. Sahibi ve Katılımcı rolleri hesapta çeşitli yönetim işlevleri gerçekleştirebilir. Okuyucu rolünü yalnızca hesap yönetimi verilerini görüntüleyen kullanıcılara atayabilirsiniz.

![RBAC rolleri](./media/data-lake-store-security-overview/rbac-roles.png "RBAC rolleri")

Roller hesap yönetimi için atanmış olsa da, bazı rollerin verilere erişimi etkilediğini unutmayın. Bir kullanıcının dosya sisteminde gerçekleştirebileceği işlemlere erişimi denetlemek için ALA'ları kullanmanız gerekir. Aşağıdaki tablo, varsayılan roller için yönetim haklarının ve veri erişim haklarının bir özetini gösterir.

| Roller | Yönetim hakları | Veri erişim hakları | Açıklama |
| --- | --- | --- | --- |
| Atanmış rol yok |None |ACL tarafından yönetilir |Kullanıcı, Veri Gölü Depolama Gen1'e göz atmak için Azure portalını veya Azure PowerShell cmdlets'i kullanamaz. Kullanıcı yalnızca komut satırı araçlarını kullanabilir. |
| Sahip |Tümü |Tümü |Sahibi rolü bir süper kullanıcıdır. Bu rol her şeyi yönetebilir ve verilere tam erişime sahiptir. |
| Okuyucu |Salt okunur |ACL tarafından yönetilir |Reader rolü, hangi kullanıcının hangi role atandığı gibi hesap yönetimiyle ilgili her şeyi görüntüleyebilir. Okuyucu rolü herhangi bir değişiklik yapamaz. |
| Katılımcı |Roller ekleme ve kaldırma dışında hepsi |ACL tarafından yönetilir |Katılımcı rolü, bir hesabın dağıtımlar ve uyarı oluşturma ve yönetme gibi bazı yönlerini yönetebilir. Katılımcı rolü roller ekleyemez veya kaldıramaz. |
| Kullanıcı Erişimi Yöneticisi |Roller ekleme ve kaldırma |ACL tarafından yönetilir |Kullanıcı Erişim Yöneticisi rolü, hesaplara kullanıcı erişimini yönetebilir. |

Talimatlar için [bkz.](data-lake-store-secure-data.md#assign-users-or-security-groups-to-data-lake-storage-gen1-accounts)

### <a name="using-acls-for-operations-on-file-systems"></a>Dosya sistemlerindeki işlemler için ALA'ları kullanma
Data Lake Storage Gen1 Hadoop Distributed File System (HDFS) gibi hiyerarşik bir dosya sistemidir ve [POSIX ALIP'lerini](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists)destekler. Sahibi rolü, Sahipler grubu ve diğer kullanıcılar ve gruplar için kaynaklara okuma (r), yazma (w) ve (x) izinlerini uygular. Data Lake Storage Gen1'de, Aç'lar kök klasöründe, alt klasörlerde ve tek tek dosyalarda etkinleştirilebilir. ACD'lerin Veri Gölü Depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için Veri [Gölü Depolama Gen1'de Erişim denetimine](data-lake-store-access-control.md)bakın.

[Güvenlik gruplarını](../active-directory/fundamentals/active-directory-groups-create-azure-portal.md)kullanarak birden çok kullanıcı için ALA'ları tanımlamanızı öneririz. Kullanıcıları bir güvenlik grubuna ekleyin ve ardından bir dosya veya klasörün ALA'larını bu güvenlik grubuna atayın. Atanan izinler için en fazla 28 girişle sınırlı olduğunuziçin, atanmış izinleri sağlamak istediğinizde bu yararlıdır. Azure Active Directory güvenlik gruplarını kullanarak Veri Gölü Depolama Gen1'de depolanan verilerin nasıl daha iyi güvenli hale gelebildiğiniz hakkında daha fazla bilgi için, [kullanıcıları veya güvenlik grubunu Data Lake Storage Gen1 dosya sistemine ACD olarak atama'ya](data-lake-store-secure-data.md#filepermissions)bakın.

![Erişim izinlerini listele](./media/data-lake-store-security-overview/adl.acl.2.png "Erişim izinlerini listele")

## <a name="network-isolation"></a>Ağ yalıtımı
Ağ düzeyindeveri deponuza erişimi denetlemeye yardımcı olmak için Veri Gölü Depolama Gen1'i kullanın. Güvenlik duvarları oluşturup, güvenilir müşterileriniz için bir IP adresi aralığı tanımlayabilirsiniz. IP adres aralığıyla, yalnızca tanımlanan aralıkta IP adresi olan istemciler Veri Gölü Depolama Gen1'e bağlanabilir.

![Güvenlik duvarı ayarları ve IP erişimi](./media/data-lake-store-security-overview/firewall-ip-access.png "Güvenlik duvarı ayarları ve IP adresi")

## <a name="data-protection"></a>Veri koruma
Data Lake Storage Gen1, yaşam döngüsü boyunca verilerinizi korur. Veri Gölü Depolama Gen1, aktarım daki veriler için ağ üzerinden verileri güvence altına almak için endüstri standardı Taşıma Katmanı Güvenliği (TLS 1.2) protokolünü kullanır.

![Veri Gölü Depolama Gen1 Şifreleme](./media/data-lake-store-security-overview/adls-encryption.png "Veri Gölü Depolama Gen1 Şifreleme")

Veri Gölü Depolama Gen1 de hesapta depolanan veriler için şifreleme sağlar. Verilerinizin şifrelenmesini tercih edebilir ya da şifrelemeyi kabul etmeyebilirsiniz. Şifrelemeyi seçerseniz, Veri Gölü Depolama Gen1'de depolanan veriler kalıcı ortamlarda depolamadan önce şifrelenir. Böyle bir durumda, Data Lake Storage Gen1 kalıcı verilerden önce verileri otomatik olarak şifreler ve verileri almadan önce şifresini çözer, bu nedenle verilere erişen istemci için tamamen saydamdır. Verileri şifrelemek/çözmek için istemci tarafında herhangi bir kod değişikliği gerekmez.

Anahtar yönetimi için, Veri Gölü Depolama Gen1, Veri Gölü Depolama Gen1'de depolanan verilerin şifresini çözmek için gereken ana şifreleme anahtarlarınızı (MEKs) yönetmek için iki mod sağlar. Data Lake Storage Gen1'in MEK'leri sizin için yönetmesine izin verebilir veya Azure Key Vault hesabınızı kullanarak MEK'lerin sahipliğini korumayı seçebilirsiniz. Veri Gölü Depolama Gen1 hesabı oluştururken anahtar yönetim modunu belirtirsiniz. Şifrelemeyle ilgili yapılandırmanın nasıl sağlayabileceği hakkında daha fazla bilgi için [bkz.](data-lake-store-get-started-portal.md)

## <a name="activity-and-diagnostic-logs"></a>Etkinlik ve tanılama günlükleri
Hesap yönetimiyle ilgili etkinlikler veya veri yle ilgili etkinlikler için günlük ler aramanıza bağlı olarak etkinlik veya tanılama günlüklerini kullanabilirsiniz.

* Hesap yönetimiyle ilgili etkinlikler Azure Kaynak Yöneticisi API'lerini kullanır ve etkinlik günlükleri aracılığıyla Azure portalında su yüzüne çıkar.
* Verilerle ilgili etkinlikler WebHDFS REST API'lerini kullanır ve tanılama günlükleri aracılığıyla Azure portalında su yüzüne çıkar.

### <a name="activity-log"></a>Etkinlik günlüğü
Yönetmeliklere uymak için, bir kuruluş belirli olayları araştırmak için hesap yönetimi faaliyetlerinin yeterli denetim izlerini gerektirebilir. Data Lake Storage Gen1 yerleşik izleme vardır ve tüm hesap yönetimi faaliyetlerini kaydeder.

Hesap yönetimi denetim izleri için, günlüğe kaydetmek istediğiniz sütunları görüntüleyin ve seçin. Etkinlik günlüklerini Azure Depolama'ya da dışa aktarabilirsiniz.

![Etkinlik günlüğü](./media/data-lake-store-security-overview/activity-logs.png "Etkinlik günlüğü")

Etkinlik günlükleriyle çalışma hakkında daha fazla bilgi [için, kaynaklardaki eylemleri denetlemek için etkinlik günlüklerini görüntüle'ye](../azure-resource-manager/management/view-activity-logs.md)bakın.

### <a name="diagnostics-logs"></a>Tanılama günlükleri
Azure portalında veri erişimi denetimi ve tanılama günlüğünü etkinleştirebilir ve günlükleri bir Azure Blob depolama hesabına, bir etkinlik hub'ına veya Azure Monitor günlüklerine gönderebilirsiniz.

![Tanılama günlükleri](./media/data-lake-store-security-overview/diagnostic-logs.png "Tanılama günlükleri")

Data Lake Storage Gen1 ile tanılama günlükleriyle çalışma hakkında daha fazla bilgi için Bkz. [Veri Gölü Depolama Gen1 için tanı günlüklerine erişim.](data-lake-store-diagnostic-logs.md)

## <a name="summary"></a>Özet
Kurumsal müşteriler, güvenli ve kullanımı kolay bir veri analitiği bulut platformu talep eder. Data Lake Storage Gen1, Azure Active Directory entegrasyonu, ACL tabanlı yetkilendirme, ağ yalıtımı, aktarım ve istirahatte veri şifreleme ve denetim yoluyla kimlik yönetimi ve kimlik doğrulama yoluyla bu gereksinimlerin giderilmesine yardımcı olmak üzere tasarlanmıştır.

Data Lake Storage Gen1'deki yeni özellikleri görmek istiyorsanız, [Data Lake Storage Gen1 UserVoice forumunda](https://feedback.azure.com/forums/327234-data-lake)görüşlerinizi bize gönderin.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Veri Gölü Depolama Gen1'e Genel Bakış](data-lake-store-overview.md)
* [Data Lake Storage Gen1 ile başlayın](data-lake-store-get-started-portal.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
