---
title: HIPAA HiTRUST şema örnek denetimleri
description: HIPAA itrust şema örneklerinin denetim eşlemesi. Her denetim, değerlendirmenize yardımcı olan bir veya daha fazla Azure Ilkesiyle eşleştirilir.
ms.date: 08/03/2020
ms.topic: sample
ms.openlocfilehash: 10b771e3cfb18a28bd720332a26e13bb1d1f6022
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88209416"
---
# <a name="control-mapping-of-the-hipaa-hitrust-blueprint-sample"></a>HIPAA zrust şema örneğinin denetim eşlemesi

Aşağıdaki makalede, Azure şemaları HIPAA, bluebaskı örneği, HIPAA ile HIPAA denetimleri için nasıl eşlenir. Denetimler hakkında daha fazla bilgi için bkz. [HIPAA HITRUST](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

Aşağıdaki eşlemeler **HIPAA ZRUST** denetimlerine göre yapılır. Sağ taraftaki gezinmeyi kullanarak doğrudan belirli bir denetim eşlemesine atlayın. Eşlenmiş denetimlerin birçoğu bir [Azure Policy](../../../policy/overview.md) girişimi ile uygulanır. Tüm girişimi gözden geçirmek için Azure portal **ilkeyi** açın ve **tanımlar** sayfasını seçin. Ardından, önizlemeyi bulun ve seçin ** \[ \] : HIPAA HiTRUST denetimleri denetim** yerleşik ilke girişim.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure ilke](../../../policy/overview.md) tanımı ile ilişkilidir. Bu ilkeler, denetimiyle [uyumluluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; Ancak, bir denetim ve bir veya daha fazla ilke arasında genellikle bir 1:1 veya bir eşleşme yoktur. Bu nedenle, Azure Ilkesi ile **uyumlu** , yalnızca ilkelerin kendilerine başvurur; Bu, bir denetimin tüm gereksinimleriyle tamamen uyumlu olduğunuzdan emin değildir. Buna ek olarak, uyumluluk standardı şu anda herhangi bir Azure Ilke tanımı tarafından açıklanmayan denetimler içerir. Bu nedenle, Azure Ilkesinde uyumluluk, genel uyumluluk durumunuzu yalnızca kısmi görünümüdür. Bu uyumluluk şeması örneği için denetimler ve Azure Ilke tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub kayıt geçmişine](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/HIPAA-HITRUST/control-mapping.md)bakın.

## <a name="control-against-malicious-code"></a>Kötü amaçlı koda karşı denetim

Bu şema, Azure Güvenlik Merkezi 'nde sanal makinelerde eksik uç nokta koruması için izleme yapan [Azure ilke](../../../policy/overview.md) tanımlarını atayarak ve Windows sanal makinelerinde Microsoft kötü amaçlı yazılımdan koruma çözümünü zorlayarak, kötü amaçlı kod koruma dahil olmak üzere Endpoint Protection 'ı yönetmenize yardımcı olur.

- Azure için Microsoft kötü amaçlı yazılımdan koruma imzaları otomatik olarak güncelleştirilecek şekilde yapılandırılmalıdır
- Azure Güvenlik Merkezi 'nde eksik Endpoint Protection izleme
- Uç nokta koruma çözümü, sanal makine ölçek kümelerine yüklenmelidir
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir


## <a name="management-of-removable-media"></a>Çıkarılabilir medya yönetimi

Veri sınıflandırma düzeyine bağlı olarak, medyayı kullanımdan önce (dizüstü bilgisayarlar dahil) kaydeder, bu tür ortamların nasıl kullanılacağına dair makul kısıtlamalar getirir ve uygun bir fiziksel ve mantıksal koruma düzeyi (şifreleme dahil), doğru bir şekilde yok edilir veya yok edilebilir.

- SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir
- Disk şifrelemesi sanal makinelere uygulanmalıdır
- Eklenmemiş diskler şifrelenmelidir
- Data Lake Store hesaplarında şifreleme gerektir
- SQL Server TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- SQL yönetilen örnek TDE koruyucusu kendi anahtarınızla şifrelenmelidir

## <a name="control-of-operational-software"></a>Işletimsel yazılımın denetimi 

Kuruluş, bilgi sisteminde sunucular, iş istasyonları ve dizüstü bilgisayarlar da dahil olmak üzere yetkisiz yazılımları tanımlar, bilgi sisteminde bilinen yetkisiz yazılımların yürütülmesini önlemek için bir izin verme, özel durum engelleme ilkesi kullanır ve yetkisiz yazılım listesini düzenli olarak gözden geçirir, ancak yılda bir günden az değildir.

- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir
- Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="change-control-procedures"></a>Denetim yordamlarını değiştirme

Tüm sanal makine görüntülerinin bütünlüğü, sanal makine görüntülerine yapılan değişiklikler için bir uyarı kaydederek ve bir uyarı oluşturarak ve/veya müşteri (örneğin, portallar veya uyarılar) bir değişiklik veya taşıma sonuçları ve görüntünün bütünlüğünden sonraki doğrulanmasıyla ilgili bir uyarı kaydederek her zaman bir kez daha kırmızı hale getirilir.

- \[Önizleme \] ' sistem denetim ilkeleri-ayrıntılı izleme ' içindeki Windows VM Yapılandırması Denetim sonuçlarını göster
- \[Önizleme \] : ' sistem denetim ilkeleri-ayrıntılı izleme ' Içindeki Windows VM yapılandırmalarından denetim sonuçlarını gösterme

## <a name="control-of-technical-vulnerabilities"></a>Teknik güvenlik açıklarının denetimi 

Tüm sistem ve ağ bileşenleri için sağlamlaştırılmış bir yapılandırma standardı vardır.

- Güvenlik açığı değerlendirmesi SQL sunucularınızda etkinleştirilmelidir
- SQL yönetilen örneklerinizin üzerinde güvenlik açığı değerlendirmesi etkinleştirilmelidir
- \[Önizleme \] güvenlik açığı değerlendirmesi sanal makinelerde etkinleştirilmelidir
- Güvenlik açıkları bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir
- Makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Sanal makine ölçek kümelerinizin güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir
- Kapsayıcı güvenlik yapılandırmalarında güvenlik açıkları düzeltilmelidir
- SQL veritabanlarınızdaki güvenlik açıkları düzeltilmelidir
- \[Önizleme \] : Kubernetes hizmetlerinde Pod güvenlik ilkeleri tanımlanmalıdır

## <a name="segregation-in-networks"></a>Ağlarda ayrım yapma

Kuruluşun güvenlik ağ geçitleri (ör. güvenlik duvarları) güvenlik ilkelerini uygular ve etki alanları arasındaki trafiği filtrelemek, yetkisiz erişimi engellemek ve etki alanlarının her biri için, DMZs dahil olmak üzere iç kablolu, iç kablosuz ve dış ağ kesimleri (örn. Internet) arasında ayrımı sürdürmek için kullanılır.

- Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir
- Sanal makinelerin onaylanan bir sanal ağa bağlanması gerekir
- Sanal makineler bir ağ güvenlik grubuyla ilişkilendirilmelidir
- Service Bus bir sanal ağ hizmeti uç noktası kullanmalıdır
- App Service bir sanal ağ hizmeti uç noktası kullanmalıdır
- SQL Server bir sanal ağ hizmeti uç noktası kullanmalıdır
- Olay Hub 'ı bir sanal ağ hizmeti uç noktası kullanmalıdır
- Cosmos DB bir sanal ağ hizmeti uç noktası kullanmalıdır
- Key Vault bir sanal ağ hizmeti uç noktası kullanmalıdır
- Ağ geçidi alt ağları bir ağ güvenlik grubuyla yapılandırılmamalıdır
- Depolama hesapları bir sanal ağ hizmeti uç noktası kullanmalıdır
- \[Önizleme \] : Container Registry sanal ağ hizmeti uç noktası kullanmalıdır
- Uyarlamalı ağ sağlamlaştırma önerileri, iç karşılıklı sanal makinelere uygulanmalıdır

## <a name="network-connection-control"></a>Ağ bağlantısı denetimi

Ağ trafiği, kuruluşun erişim denetimi ilkesine ve her bir ağ erişim noktası ya da dış iletişim hizmeti 'nin yönetilen arabirimine yönelik güvenlik duvarı ve ağla ilgili diğer kısıtlamalar doğrultusunda denetlenir.

- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- API AP 'niz içinde en son TLS sürümü kullanılmalıdır
- Web uygulamanızda en son TLS sürümü kullanılmalıdır
- İşlev Uygulaması en son TLS sürümü kullanılmalıdır
- İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir
- Alt ağlar bir ağ güvenlik grubuyla ilişkilendirilmelidir
- IaaS üzerinde Web uygulamalarına yönelik NSG kuralları sağlamlaştırılmış olmalıdır
- Internet 'e yönelik sanal makineler için ağ güvenlik grubu kuralları sağlamlaştırılmış olmalıdır
- Sanal makinelerin onaylanan bir sanal ağa bağlanması gerekir
- Sanal makineler bir ağ güvenlik grubuyla ilişkilendirilmelidir

## <a name="network-controls"></a>Ağ denetimleri

Kuruluş, fiziksel sunucuları, uygulamaları veya verileri sanallaştırılmış sunuculara geçirirken güvenli ve şifreli iletişim kanalları kullanır.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır
- Uyarlamalı ağ sağlamlaştırma önerileri, iç karşılıklı sanal makinelere uygulanmalıdır
- Service Bus bir sanal ağ hizmeti uç noktası kullanmalıdır
- App Service bir sanal ağ hizmeti uç noktası kullanmalıdır
- SQL Server bir sanal ağ hizmeti uç noktası kullanmalıdır
- Olay Hub 'ı bir sanal ağ hizmeti uç noktası kullanmalıdır
- Cosmos DB bir sanal ağ hizmeti uç noktası kullanmalıdır
- Key Vault bir sanal ağ hizmeti uç noktası kullanmalıdır
- Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme
- Depolama hesapları bir sanal ağ hizmeti uç noktası kullanmalıdır
- \[Önizleme \] : Container Registry sanal ağ hizmeti uç noktası kullanmalıdır

## <a name="security-of-network-services"></a>Ağ hizmetlerinin güvenliği

Bir ağ hizmeti sağlayıcısı/Yöneticisi tarafından sunulan kabul edilen hizmetler, güvenli bir şekilde sağlandıklarından emin olmak için resmi olarak yönetilir ve izlenir.

- \[Önizleme \] : ağ trafiği veri toplama Aracısı Windows sanal makinelerine yüklenmelidir
- \[Önizleme \] : ağ trafiği veri toplama Aracısı Linux sanal makinelerinde yüklü olmalıdır Ağ İzleyicisi etkinleştirilmelidir

## <a name="information-exchange-policies-and-procedures"></a>Bilgi değişimi Ilkeleri ve yordamları

Kuruluş, şirket tarafından denetlenen taşınabilir depolama ortamının kullanımını, dış bilgi sistemlerindeki yetkili kişiler tarafından sınırlandırır.

- WEB uygulamasının ' Istemci sertifikaları (gelen istemci sertifikaları) ' olarak ' on ' olarak ayarlandığından emin olun
- CORS, her kaynağın Web uygulamalarınıza erişmesine izin vermemelidir
- CORS, her kaynağın Işlev uygulamalarınıza erişmesine izin vermemelidir
- CORS, her kaynağın API uygulamanıza erişmesine izin vermemelidir
- Web uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- Işlev uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- API Apps için uzaktan hata ayıklama kapatılmalıdır

## <a name="on-line-transactions"></a>Satır içi Işlemler

Kuruluş, işlem içinde yer alan tarafların her biri ile elektronik imzaların kullanımı arasında şifreleme kullanımını gerektirir. Kuruluş, işlem ayrıntılarının depolamanın, herkese açık olarak erişilebilen tüm ortamların dışında konumlandırılmasını sağlar (ör. kuruluşun intraneti üzerinde var olan bir depolama platformunda) ve artık Internet 'ten erişilebilen bir depolama ortamında korunmaz. (örn. dijital imzaları ve/veya dijital sertifikaları verme ve sürdürme amacıyla), güvenlik, uçtan uca sertifika/imza yönetimi işleminin tamamında tümleşiktir ve katıştırılır.

- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- API AP 'niz içinde en son TLS sürümü kullanılmalıdır
- Web uygulamanızda en son TLS sürümü kullanılmalıdır
- İşlev Uygulaması en son TLS sürümü kullanılmalıdır
- İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir
- \[Önizleme \] : güvenilen kökte belirtilen sertifikaları Içermeyen Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- \[Önizleme \] : güvenilen kökte belirtilen sertifikaları Içermeyen Windows VM 'lerinden denetim sonuçlarını göster

## <a name="user-password-management"></a>Kullanıcı parolası yönetimi

Parolalar, tüm sistem bileşenlerinde iletim ve depolama sırasında şifrelenir.

- \[\]Güvenli olmayan parola güvenlik ayarlarıyla denetim VM 'lerinin önizlemesi

## <a name="user-authentication-for-external-connections"></a>Dış bağlantılar için Kullanıcı kimlik doğrulaması

Kuruluşlar ağına yönelik tüm dış bağlantılar için çok faktörlü, radius veya Kerberos (ayrıcalıklı erişim için) ve CHAP (çevirmeli yöntemlere yönelik kimlik bilgilerini şifreleme) gibi güçlü kimlik doğrulama yöntemleri uygulanır.

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir
- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır

## <a name="user-identification-and-authentication"></a>Kullanıcı kimliği ve kimlik doğrulaması

Ayrıcalıklı işlevleri (örn. Sistem Yönetimi) gerçekleştiren kullanıcılar, bu ayrıcalıklı işlevleri gerçekleştirirken ayrı hesaplar kullanır. Multi-Factor Authentication yöntemleri kuruluş ilkesine uygun olarak kullanılır (örneğin, uzaktan ağ erişimi için).

- MFA, aboneliğinizde sahip izinleri olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde yazma izinleri olan hesaplarda etkinleştirilmelidir
- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir
- Aboneliğiniz için en fazla 3 sahip belirtilmelidir
- Aboneliğinize birden fazla sahip atanmalıdır
- Yöneticiler grubunun belirtilen üyelerden birini içerdiği Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- Yöneticiler grubunun belirtilen üyelerden birini içerdiği Windows VM 'lerinden denetim sonuçlarını göster
- Yöneticiler grubunun belirtilen tüm üyeleri içermediği Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- Yöneticiler grubunun belirtilen tüm üyeleri içermediği Windows VM 'lerinden denetim sonuçlarını göster
- Yöneticiler grubunun yalnızca belirtilen üyeleri içermediği Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- Yöneticiler grubunun yalnızca belirtilen üyeleri içermediği Windows VM 'lerinden denetim sonuçlarını göster

## <a name="privilege-management"></a>Ayrıcalık yönetimi

Sanallaştırılmış sistemleri barındıran sistemler için yönetim işlevlerine veya yönetim konsollarına erişim, en az ayrıcalık ilkesine ve teknik denetimler aracılığıyla desteklenlerine bağlı olarak personelle kısıtlıdır.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır
- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır
- Aboneliğiniz için en fazla 3 sahip belirtilmelidir
- Aboneliğinize birden fazla sahip atanmalıdır
- Sahip izinleri olan dış hesaplar aboneliğinizden kaldırılmalıdır
- Sahip izinleri olan kullanım dışı hesaplar aboneliğinizden kaldırılmalıdır
- Özel RBAC kurallarının kullanımını denetleme
- Rol tabanlı Access Control (RBAC), Kubernetes hizmetlerinde kullanılmalıdır

## <a name="review-of-user-access-rights"></a>Kullanıcı erişim haklarının incelenmesi

Kuruluş, bilgi varlıklarının yetkili kullanıcılarının belgelenmiş bir listesini tutar.

- Özel RBAC kurallarının kullanımını denetleme

## <a name="remote-diagnostic-and-configuration-port-protection"></a>Uzaktan tanılama ve yapılandırma bağlantı noktası koruması

Bir bilgisayarda veya ağ sistemlerinde yüklü olan bağlantı noktaları, hizmetler ve benzer uygulamalar, özellikle de iş işlevselliği için gerekli değildir, devre dışıdır veya kaldırılır.

- Sanal makinelere anlık ağ erişim denetimi uygulanmalıdır
- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır
- Web uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- Işlev uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- API Apps için uzaktan hata ayıklama kapatılmalıdır
- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="audit-logging"></a>Denetim günlüğü

Gönderilen ve alınan ileti günlükleri, iletinin tarihi, saati, kaynağı ve hedefi de dahil olmak üzere saklanır, ancak içeriğini değil. Sistem etkin durumdayken denetim her zaman kullanılabilir ve önemli olayları, başarı/başarısız veri erişimi, sistem güvenlik yapılandırması değişiklikleri, ayrıcalıklı veya yardımcı program kullanımı, tüm alarmlar (örneğin, A/V ve KIMLIKLER), kimlik ve kimlik doğrulama mekanizmalarının etkinleştirilmesi ve devre dışı bırakılması, sistem düzeyindeki nesnelerin oluşturulması ve silinmesi.

- Azure Data Lake Store tanılama günlükleri etkinleştirilmelidir
- Logic Apps tanılama günlükleri etkinleştirilmelidir 
- IoT Hub tanılama günlükleri etkinleştirilmelidir 
- Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir 
- Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir 
- Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir 
- Arama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir 
- Uygulama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir 
- Data Lake Analytics tanılama günlükleri etkinleştirilmelidir 
- Key Vault tanılama günlükleri etkinleştirilmelidir 
- Service Bus tanılama günlükleri etkinleştirilmelidir
- Azure Stream Analytics tanılama günlükleri etkinleştirilmelidir
- SQL Server üzerinde denetim etkinleştirilmelidir
- Tanılama ayarını denetle
- Azure Izleyici, tüm bölgelerdeki etkinlik günlüklerini toplamalıdır

## <a name="monitoring-system-use"></a>İzleme sistem kullanımı

Kuruluşun ortamında dağıtılan otomatikleştirilmiş sistemler, önemli olayları ve anormal etkinlikleri izlemek ve sistem günlüklerini analiz etmek için kullanılır. bunun sonuçları düzenli olarak gözden geçirilir. İzleme, devre dışı bırakılmış hesaplara erişim girişimleri ve sistem uyarıları ya da arızalar dahil ayrıcalıklı işlemler, yetkili erişim veya yetkisiz erişim girişimleri içerir.

- Azure Izleyici, tüm bölgelerdeki etkinlik günlüklerini toplamalıdır
- Log Analytics Aracısı sanal makinelere yüklenmelidir
- Log Analytics Aracısı sanal makine ölçek kümelerine yüklenmelidir
- \[Önizleme \] : Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows sanal makinelerini denetlemek için önkoşulları dağıtın
- \[Önizleme \] : Log Analytics aracısının beklendiği gibi bağlı olmadığı Windows VM 'lerinden denetim sonuçlarını göster
- Azure Izleyici günlük profili, ' Write, ' ' DELETE, ' ve ' Action ' kategorilerinin günlüklerini toplamalıdır
- Log Analytics izleme aracısının otomatik olarak sağlanması aboneliğinizde etkinleştirilmelidir

## <a name="segregation-of-duties"></a>Görevlerin ayrımı

Görevlerin ayrımı, bilgi ve sistemlerin yetkisiz veya istenmeden değiştirilmesi riskini sınırlamak için kullanılır. Tek bir kişi yetkilendirme veya algılama olmadan bilgi sistemlerine erişemez, bunları değiştirebilir veya kullanamaz. Denetimleri yönetme ve erişim denetimlerinden sorumlu olan kişiler, her kullanıcının rolüne ve sorumluluklarına göre gerekli olan en düşük gerekliliktir ve bu kişiler, bu denetimlerle ilgili denetim işlevlerine erişemez.

- Rol tabanlı Access Control (RBAC), Kubernetes hizmetlerinde kullanılmalıdır
- Özel RBAC kurallarının kullanımını denetleme
- \[Önizleme \] : ' Kullanıcı hakları ataması ' Içinde Windows VM 'leri yapılandırmasını denetlemek için önkoşulları dağıtın
- \[Önizleme \] : ' Kullanıcı hakları ataması ' Içindeki Windows VM yapılandırmalarından denetim sonuçlarını göster
- \[Önizleme \] : ' güvenlik seçenekleri-Kullanıcı hesabı denetimi 'Nde Windows VM 'leri yapılandırmasını denetlemek için önkoşulları dağıtın
- \[Önizleme \] : Windows VM yapılandırmalarından denetim sonuçlarını ' güvenlik seçenekleri-Kullanıcı hesabı denetimi ' içinde göster
- Özel abonelik sahibi rolleri mevcut olmamalıdır

## <a name="administrator-and-operator-logs"></a>Yönetici ve operatör günlükleri

Kuruluş, yönetici etkinliklerini denetlemek için doğru günlük kaydının etkinleştirilmesini sağlar; ve sistem yöneticisi ve işletmen günlüklerini düzenli olarak inceler.

- Belirli yönetim işlemleri için bir etkinlik günlüğü uyarısı bulunmalıdır

## <a name="identification-of-risks-related-to-external-parties"></a>Dış taraflarla Ilgili risklerin tanımlanması

Kuruluş ve dış taraflar arasındaki uzaktan erişim bağlantıları şifrelenir

- Depolama hesaplarına güvenli aktarım etkinleştirilmelidir
- İşlev Uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- Web uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır
- MySQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- PostgreSQL veritabanı sunucuları için SSL bağlantısını zorla etkinleştirilmelidir
- Yalnızca Redis Cache güvenli bağlantılar etkinleştirilmelidir

## <a name="business-continuity-and-risk-assessment"></a>İş sürekliliği ve risk değerlendirmesi

Kuruluş, kritik iş süreçlerini tanımlar ve iş sürekliliği 'nin bilgi güvenliği yönetimi gereksinimlerini, bu gibi yönlerle ilgili işlemler, ekip, malzemeler, taşıma ve tesisler ile ilgili diğer sürekliliği gereksinimleriyle tümleştirir.

- Olağanüstü durum kurtarma yapılandırması olmadan sanal makineleri denetleme
- Key Vault nesneler kurtarılabilir olmalıdır
- \[Önizleme \] : ' güvenlik seçenekleri-Kurtarma Konsolu ' nda Windows VM 'leri yapılandırmasını denetlemek için önkoşulları dağıtın
- \[Önizleme \] "güvenlik seçenekleri-Kurtarma Konsolu" nda Windows VM yapılandırmasından denetim sonuçlarını göster

## <a name="back-up"></a>Yedekle

Bu şema, kuruluşun sistem yedekleme bilgilerini alternatif depolama sitesine elektronik olarak denetleyen Azure ilke tanımlarını atar. Depolama meta verilerinin fiziksel olarak sevkiyatı için Azure Data Box kullanmayı düşünün.

- Azure SQL veritabanları için uzun vadeli coğrafi olarak yedekli yedeklemenin etkinleştirilmesi gerekir
- MySQL için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- PostgreSQL için Azure veritabanı için coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- MariaDB için Azure veritabanı 'nda coğrafi olarak yedekli yedekleme etkinleştirilmelidir
- Sanal makineler için Azure Backup etkinleştirilmelidir

> [!NOTE]
> Belirli Azure Ilke tanımlarının kullanılabilirliği, Azure Kamu ve diğer ulusal bulutlarda farklılık gösterebilir. 

## <a name="next-steps"></a>Sonraki adımlar

HIPAA HiTRUST şema örneğinin denetim eşlemesini gözden geçirdiniz. Daha sonra, genel bakış ve bu örneği dağıtma hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="next step action"]
> [HIPAA HiTRUST şema-genel bakış](./control-mapping.md) 
>  [HIPAA HiTRUST şema-Deploy adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
