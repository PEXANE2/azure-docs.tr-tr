---
title: HIPAA HiTRUST şema örnek denetimleri
description: HIPAA itrust şema örneklerinin denetim eşlemesi. Her denetim, değerlendirmenize yardımcı olan bir veya daha fazla Azure Ilkesiyle eşleştirilir.
ms.date: 11/26/2019
ms.topic: sample
ms.openlocfilehash: c6680055cfe41a2f893d46cba797783913d880ce
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75472313"
---
# <a name="control-mapping-of-the-hipaa-hitrust-blueprint-sample"></a>HIPAA zrust şema örneğinin denetim eşlemesi

Aşağıdaki makalede, Azure şemaları HIPAA, bluebaskı örneği, HIPAA ile HIPAA denetimleri için nasıl eşlenir. Denetimler hakkında daha fazla bilgi için bkz. [HIPAA HITRUST](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html).

Aşağıdaki eşlemeler **HIPAA ZRUST** denetimlerine göre yapılır. Sağ taraftaki gezinmeyi kullanarak doğrudan belirli bir denetim eşlemesine atlayın. Eşlenmiş denetimlerin birçoğu bir [Azure Policy](../../../policy/overview.md) girişimi ile uygulanır. Tüm girişimi gözden geçirmek için Azure portal **ilkeyi** açın ve **tanımlar** sayfasını seçin. Ardından, ** \[önizlemeyi\]bulun ve seçin: HIPAA HiTRUST denetimleri denetim** yerleşik ilke girişim.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure ilke](../../../policy/overview.md) tanımı ile ilişkilidir. Bu ilkeler, denetimiyle [uyumluluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; Ancak, bir denetim ve bir veya daha fazla ilke arasında genellikle bir 1:1 veya bir eşleşme yoktur. Bu nedenle, Azure Ilkesi ile **uyumlu** , yalnızca ilkelerin kendilerine başvurur; Bu, bir denetimin tüm gereksinimleriyle tamamen uyumlu olduğunuzdan emin değildir. Buna ek olarak, uyumluluk standardı şu anda herhangi bir Azure Ilke tanımı tarafından açıklanmayan denetimler içerir. Bu nedenle, Azure Ilkesinde uyumluluk, genel uyumluluk durumunuzu yalnızca kısmi görünümüdür. Bu uyumluluk şeması örneği için denetimler ve Azure Ilke tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub kayıt geçmişine](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/HIPAA-HITRUST/control-mapping.md)bakın.

## <a name="control-against-malicious-code"></a>Kötü amaçlı koda karşı denetim

Bu şema, Azure Güvenlik Merkezi 'nde sanal makinelerde eksik uç nokta koruması için izleme yapan [Azure ilke](../../../policy/overview.md) tanımlarını atayarak ve Windows sanal makinelerinde Microsoft kötü amaçlı yazılımdan koruma çözümünü zorlayarak, kötü amaçlı kod koruma dahil olmak üzere Endpoint Protection 'ı yönetmenize yardımcı olur.

- Windows Server için varsayılan Microsoft IaaS kötü amaçlı yazılımdan koruma uzantısını dağıt
- Batch hesaplarındaki tanılama günlükleri etkinleştirilmelidir
- Makinelerinize sistem güncelleştirmeleri yüklenmelidir


## <a name="management-of-removable-media"></a>Çıkarılabilir medya yönetimi

Veri sınıflandırma düzeyine bağlı olarak, medyayı kullanımdan önce (dizüstü bilgisayarlar dahil) kaydeder, bu tür ortamların nasıl kullanılacağına dair makul kısıtlamalar getirir ve uygun bir fiziksel ve mantıksal koruma düzeyi (şifreleme dahil), doğru bir şekilde yok edilir veya yok edilebilir.

- Data Lake Store hesaplarında şifreleme gerektir
- SQL yönetilen örnek TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- Disk şifrelemesi sanal makinelere uygulanmalıdır
- SQL veritabanlarındaki Saydam Veri Şifrelemesi etkinleştirilmelidir


## <a name="information-exchange-policies-and-procedures"></a>Bilgi değişimi Ilkeleri ve yordamları

Bulut hizmeti sağlayıcıları, birlikte çalışabilirlik sağlamaya yardımcı olmak için sektör tarafından tanınan bir sanallaştırma platformu ve standart sanallaştırma biçimleri (örn., Open Virtualization Format, OVF) kullanır ve kullanımda olan herhangi bir Hiper yöneticide yapılan özel değişiklikler ve müşteri incelemesi için tüm çözüme özgü sanallaştırma kancalarını belgelemiştir.

- Belirtilen uygulamaların yüklü olmadığı Windows sanal makinelerini denetlemek için önkoşulları dağıtın

## <a name="control-of-operational-software"></a>Işletimsel yazılımın denetimi 

Kuruluş, bilgi sisteminde sunucular, iş istasyonları ve dizüstü bilgisayarlar da dahil olmak üzere yetkisiz yazılımları tanımlar, bilgi sisteminde bilinen yetkisiz yazılımların yürütülmesini önlemek için bir izin verme, özel durum engelleme ilkesi kullanır ve yetkisiz yazılım listesini düzenli olarak gözden geçirir, ancak yılda bir günden az değildir.

- \[Önizleme\] "güvenlik seçenekleri-denetim" Içinde Windows VM yapılandırmalarından denetim sonuçlarını göster
- \[Önizleme\] "Sistem Denetim Ilkeleri-hesap yönetimi" Içindeki Windows VM yapılandırmalarından denetim sonuçlarını gösterme

## <a name="change-control-procedures"></a>Denetim yordamlarını değiştirme

Tüm sanal makine görüntülerinin bütünlüğü, sanal makine görüntülerine yapılan değişiklikler için bir uyarı kaydederek ve bir uyarı oluşturarak ve/veya müşteri (örneğin, portallar veya uyarılar) bir değişiklik veya taşıma sonuçları ve görüntünün bütünlüğünden sonraki doğrulanmasıyla ilgili bir uyarı kaydederek her zaman bir kez daha kırmızı hale getirilir.

- \[Önizleme\] ' sistem denetim Ilkeleri-ayrıntılı izleme ' Içindeki Windows VM Yapılandırması Denetim sonuçlarını göster

## <a name="inventory-of-assets"></a>Varlıkların envanteri 

Bir varlık ve hizmet envanteri tutulur

- Arama hizmetlerindeki tanılama günlükleri etkinleştirilmelidir.
- \["\] Güvenlik seçenekleri-Microsoft ağ sunucusu" 'Ndaki Windows VM yapılandırmasını denetlemek için dağıtım gereksinimlerini Önizleme
- \["\] Yönetim Şablonları-ağ" Içindeki Windows VM 'leri yapılandırmasını denetlemek için dağıtım gereksinimlerini Önizleme

## <a name="control-of-technical-vulnerabilities"></a>Teknik güvenlik açıklarının denetimi 

Tüm sistem ve ağ bileşenleri için sağlamlaştırılmış bir yapılandırma standardı vardır.

- Olağanüstü durum kurtarma yapılandırması olmadan sanal makineleri denetleme
- SQL yönetilen örneklerinizin üzerinde güvenlik açığı değerlendirmesi etkinleştirilmelidir
- Güvenlik açığı bir güvenlik açığı değerlendirme çözümü tarafından düzeltilmelidir

## <a name="segregation-in-networks"></a>Ağlarda ayrım yapma

Kuruluşun güvenlik ağ geçitleri (ör. güvenlik duvarları) güvenlik ilkelerini uygular ve etki alanları arasındaki trafiği filtrelemek, yetkisiz erişimi engellemek ve etki alanlarının her biri için, DMZs dahil olmak üzere iç kablolu, iç kablosuz ve dış ağ kesimleri (örn. Internet) arasında ayrımı sürdürmek için kullanılır.

- Güvenlik Izleme aracısının otomatik sağlaması
- Sanal ağlar oluşturulduğunda Ağ İzleyicisi dağıt

## <a name="input-data-validation"></a>Giriş verileri doğrulaması

Herkese yönelik tüm Web uygulamaları için uygulama düzeyi güvenlik duvarları trafiği denetlemek için uygulanır. Web tabanlı olmayan genel kullanıma yönelik uygulamalar için kuruluş, uygulama türüne özgü bir ağ tabanlı güvenlik duvarı uygulamıştır. Herkese açık uygulamanın trafiği şifrelenirse, cihaz şifrelemenin arkasında yer alır veya analizden önce trafiğin şifresini çözmezler.

- \[Önizleme\] "Windows Güvenlik Duvarı Özellikleri" Içindeki Windows VM yapılandırmasından denetim sonuçlarını göster


## <a name="network-connection-control"></a>Ağ bağlantısı denetimi

Ağ trafiği, kuruluşun erişim denetimi ilkesine ve her bir ağ erişim noktası ya da dış iletişim hizmeti 'nin yönetilen arabirimine yönelik güvenlik duvarı ve ağla ilgili diğer kısıtlamalar doğrultusunda denetlenir.

- Internet 'e yönelik uç noktalar üzerinden erişim kısıtlı olmalıdır
- SQL yönetilen örnek TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- API Apps için uzaktan hata ayıklama kapatılmalıdır

## <a name="network-controls"></a>Ağ denetimleri

Kuruluş, fiziksel sunucuları, uygulamaları veya verileri sanallaştırılmış sunuculara geçirirken güvenli ve şifreli iletişim kanalları kullanır.

- Disk şifrelemesi VM 'Lere uygulanmalıdır
- SQL Server TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- \[Önizleme\] , "güvenlik seçenekleri-ağ erişimi" Içindeki Windows VM yapılandırmalarının denetim sonuçlarını gösterir
- Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme
- \[Önizleme\] "Windows Güvenlik Duvarı Özellikleri" Içindeki Windows VM yapılandırması 'ndaki denetimlerin sonuçlarını göster
- Tanılama ayarlarını ağ güvenlik gruplarından dağıt
- Internet 'e yönelik uç noktalar üzerinden erişim kısıtlı olmalıdır

## <a name="sensitive-system-isolation"></a>Hassas sistem yalıtımı

Paylaşılan sistem kaynakları (örneğin, Yazmaçları, ana bellek, ikincil depolama) sisteme, diğer sistemlere/uygulamalara/kullanıcılara açıklanmadan korunur ve kullanıcılar bilinçli olarak veya istemeden bilgilere yeniden erişebilir.

- Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir

## <a name="security-of-network-services"></a>Ağ hizmetlerinin güvenliği

Bir ağ hizmeti sağlayıcısı/Yöneticisi tarafından sunulan kabul edilen hizmetler, güvenli bir şekilde sağlandıklarından emin olmak için resmi olarak yönetilir ve izlenir.

- Sanal makinelerin yeni Azure Resource Manager kaynaklara geçirilmesi gerekir

## <a name="network-routing-control"></a>Ağ yönlendirme denetimi

Yönlendirme denetimleri, iç ve dış ağlar (örneğin, Internet ve üçüncü taraf ağları) arasında kullanılan güvenlik ağ geçitleri (ör. güvenlik duvarları) aracılığıyla uygulanır.

- Uyarlamalı uygulama denetimleri sanal makinelerde etkinleştirilmelidir

## <a name="information-exchange-policies-and-procedures"></a>Bilgi değişimi Ilkeleri ve yordamları

Kuruluş, şirket tarafından denetlenen taşınabilir depolama ortamının kullanımını, dış bilgi sistemlerindeki yetkili kişiler tarafından sınırlandırır.

- Depolama hesaplarına Kısıtlanmamış ağ erişimini denetleme
- Web uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- API uygulaması yalnızca HTTPS üzerinden erişilebilir olmalıdır

## <a name="electronic-messaging"></a>Elektronik mesajlaşma

Onaylar, anlık mesajlaşma veya dosya paylaşımı dahil olmak üzere dış genel hizmetler kullanılmadan önce alınır.

- \[Önizleme\] , parola dosyası izinleri 0644 olarak ayarlanan Linux VM 'lerinden denetim sonuçlarını gösterir

## <a name="on-line-transactions"></a>Satır içi Işlemler

Kuruluş, işlem içinde yer alan tarafların her biri ile elektronik imzaların kullanımı arasında şifreleme kullanımını gerektirir. Kuruluş, işlem ayrıntılarının depolamanın, herkese açık olarak erişilebilen tüm ortamların dışında konumlandırılmasını sağlar (ör. kuruluşun intraneti üzerinde var olan bir depolama platformunda) ve artık Internet 'ten erişilebilen bir depolama ortamında korunmaz. (örn. dijital imzaları ve/veya dijital sertifikaları verme ve sürdürme amacıyla), güvenlik, uçtan uca sertifika/imza yönetimi işleminin tamamında tümleşiktir ve katıştırılır.

- Disk şifrelemesi VM 'Lere uygulanmalıdır
- \[Önizleme\] güvenilen kökte belirtilen sertifikaları Içermeyen Windows VM 'lerinden denetim sonuçlarını göster

## <a name="password-management"></a>Parola Yönetimi

Parolalar, tüm sistem bileşenlerinde iletim ve depolama sırasında şifrelenir.

- \[Önizleme\] parola karmaşıklığı ayarı etkinleştirilmemiş Windows VM 'lerinden denetim sonuçlarını göster

## <a name="user-authentication-for-external-connections"></a>Dış bağlantılar için Kullanıcı kimlik doğrulaması

Kuruluşlar ağına yönelik tüm dış bağlantılar için çok faktörlü, radius veya Kerberos (ayrıcalıklı erişim için) ve CHAP (çevirmeli yöntemlere yönelik kimlik bilgilerini şifreleme) gibi güçlü kimlik doğrulama yöntemleri uygulanır.

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- Tam zamanında ağ erişim denetimi VM 'Lere uygulanmalıdır

## <a name="user-identification-and-authentication"></a>Kullanıcı kimliği ve kimlik doğrulaması

Ayrıcalıklı işlevleri (örn. Sistem Yönetimi) gerçekleştiren kullanıcılar, bu ayrıcalıklı işlevleri gerçekleştirirken ayrı hesaplar kullanır. Multi-Factor Authentication yöntemleri kuruluş ilkesine uygun olarak kullanılır (örneğin, uzaktan ağ erişimi için).

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- Tam zamanında ağ erişim denetimi VM 'Lere uygulanmalıdır

## <a name="privilege-management"></a>Ayrıcalık yönetimi

Sanallaştırılmış sistemleri barındıran sistemler için yönetim işlevlerine veya yönetim konsollarına erişim, en az ayrıcalık ilkesine ve teknik denetimler aracılığıyla desteklenlerine bağlı olarak personelle kısıtlıdır.

- Tam zamanında ağ erişim denetimi VM 'Lere uygulanmalıdır
- \[Önizleme\] rol tabanlı Access Control (RBAC), Kubernetes hizmetlerinde kullanılmalıdır

## <a name="review-of-user-access-rights"></a>Kullanıcı erişim haklarının incelenmesi

Kuruluş, bilgi varlıklarının yetkili kullanıcılarının belgelenmiş bir listesini tutar.

- \[Önizleme\] , "güvenlik seçenekleri-hesaplar" bölümünde Windows VM yapılandırmalarının denetim sonuçlarını gösterir

## <a name="remote-diagnostic-and-configuration-port-protection"></a>Uzaktan tanılama ve yapılandırma bağlantı noktası koruması

Bir bilgisayarda veya ağ sistemlerinde yüklü olan bağlantı noktaları, hizmetler ve benzer uygulamalar, özellikle de iş işlevselliği için gerekli değildir, devre dışıdır veya kaldırılır.

- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır
- Sanal makinelerinizdeki güvenlik yapılandırmasındaki güvenlik açıklarına yönelik ölçek kümeleri düzeltilmelidir

## <a name="audit-logging"></a>Denetim günlüğü

Gönderilen ve alınan ileti günlükleri, iletinin tarihi, saati, kaynağı ve hedefi de dahil olmak üzere saklanır, ancak içeriğini değil. Sistem etkin durumdayken denetim her zaman kullanılabilir ve önemli olayları, başarı/başarısız veri erişimi, sistem güvenlik yapılandırması değişiklikleri, ayrıcalıklı veya yardımcı program kullanımı, tüm alarmlar (örneğin, A/V ve KIMLIKLER), kimlik ve kimlik doğrulama mekanizmalarının etkinleştirilmesi ve devre dışı bırakılması, sistem düzeyindeki nesnelerin oluşturulması ve silinmesi.

- Olay Hub 'ındaki tanılama günlükleri etkinleştirilmelidir
- Sanal makine ölçek kümelerindeki sistem güncelleştirmeleri yüklenmelidir

## <a name="monitoring-system-use"></a>İzleme sistem kullanımı

Kuruluşun ortamında dağıtılan otomatikleştirilmiş sistemler, önemli olayları ve anormal etkinlikleri izlemek ve sistem günlüklerini analiz etmek için kullanılır. bunun sonuçları düzenli olarak gözden geçirilir. İzleme, devre dışı bırakılmış hesaplara erişim girişimleri ve sistem uyarıları ya da arızalar dahil ayrıcalıklı işlemler, yetkili erişim veya yetkisiz erişim girişimleri içerir.

- Sanal makine ölçek kümelerindeki tanılama günlükleri etkinleştirilmelidir

## <a name="segregation-of-duties"></a>Görevlerin ayrımı

Görevlerin ayrımı, bilgi ve sistemlerin yetkisiz veya istenmeden değiştirilmesi riskini sınırlamak için kullanılır. Tek bir kişi yetkilendirme veya algılama olmadan bilgi sistemlerine erişemez, bunları değiştirebilir veya kullanamaz. Denetimleri yönetme ve erişim denetimlerinden sorumlu olan kişiler, her kullanıcının rolüne ve sorumluluklarına göre gerekli olan en düşük gerekliliktir ve bu kişiler, bu denetimlerle ilgili denetim işlevlerine erişemez.

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir

## <a name="administrator-and-operator-logs"></a>Yönetici ve operatör günlükleri

Kuruluş, yönetici etkinliklerini denetlemek için doğru günlük kaydının etkinleştirilmesini sağlar; ve sistem yöneticisi ve işletmen günlüklerini düzenli olarak inceler.

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir

## <a name="identification-of-risks-related-to-external-parties"></a>Dış taraflarla Ilgili risklerin tanımlanması

Kuruluş ve dış taraflar arasındaki uzaktan erişim bağlantıları şifrelenir

- Disk şifrelemesi sanal makinelere uygulanmalıdır

## <a name="business-continuity-and-risk-assessment"></a>İş sürekliliği ve risk değerlendirmesi

Kuruluş, kritik iş süreçlerini tanımlar ve iş sürekliliği 'nin bilgi güvenliği yönetimi gereksinimlerini, bu gibi yönlerle ilgili işlemler, ekip, malzemeler, taşıma ve tesisler ile ilgili diğer sürekliliği gereksinimleriyle tümleştirir.

- \[Önizleme\] "güvenlik seçenekleri-Kurtarma Konsolu" nda Windows VM yapılandırmasından denetim sonuçlarını göster

> [!NOTE]
> Belirli Azure Ilke tanımlarının kullanılabilirliği, Azure Kamu ve diğer ulusal bulutlarda farklılık gösterebilir. 

## <a name="next-steps"></a>Sonraki adımlar

HIPAA HiTRUST şema örneğinin denetim eşlemesini gözden geçirdiniz. Daha sonra, genel bakış ve bu örneği dağıtma hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="next step action"]
> [HIPAA HiTRUST şema-genel bakış](./control-mapping.md)
> [HIPAA HiTRUST şema-Deploy adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Şema yaşam döngüsü](../../concepts/lifecycle.md) hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- [Mevcut atamaları güncelleştirmeyi](../../how-to/update-existing-assignments.md) öğrenin.
