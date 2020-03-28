---
title: HIPAA HITRUST plan örnek kontrolleri
description: HIPAA HITRUST plan örneklerinin kontrol eşlemi. Her denetim, değerlendirmeye yardımcı olan bir veya daha fazla Azure İlkesiyle eşlenir.
ms.date: 11/26/2019
ms.topic: sample
ms.openlocfilehash: c6680055cfe41a2f893d46cba797783913d880ce
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75472313"
---
# <a name="control-mapping-of-the-hipaa-hitrust-blueprint-sample"></a>HIPAA HITRUST plan örneğinin kontrol eşlemi

Aşağıdaki makalede, Azure Planları HIPAA HITRUST planı örnek haritalarının HIPAA HITRUST denetimleri için nasıl açıklanmaktadır. Denetimler hakkında daha fazla bilgi için [HIPAA HITRUST'a](https://www.hhs.gov/hipaa/for-professionals/security/laws-regulations/index.html)bakın.

Aşağıdaki eşlemeler **HIPAA HITRUST** denetimleri içindir. Doğrudan belirli bir denetim eşleleme sine atlamak için sağdaki gezinmeyi kullanın. Eşlenen denetimlerin çoğu bir Azure [İlkesi](../../../policy/overview.md) girişimiyle uygulanır. Girişimin tamamını incelemek için Azure portalında **İlke'yi** açın ve **Tanımlar** sayfasını seçin. Ardından Önizleme'yi ** \[bulun\]ve seçin: Audit HIPAA HITRUST denetimleri** yerleşik ilke girişimi.

> [!IMPORTANT]
> Aşağıdaki her denetim bir veya daha fazla [Azure İlkesi](../../../policy/overview.md) tanımıyla ilişkilidir. Bu ilkeler, denetime [uygunluğu değerlendirmenize](../../../policy/how-to/get-compliance-data.md) yardımcı olabilir; ancak, genellikle bir denetim ve bir veya daha fazla ilke arasında 1:1 veya tam bir eşleşme yoktur. Bu nedenle, Azure İlkesinde **Uyumlu olmak** yalnızca ilkelerin kendileri için başvurur; bu, bir denetimin tüm gereksinimlerine tam olarak uymanızı sağlamaz. Ayrıca, uyumluluk standardı şu anda herhangi bir Azure İlkesi tanımı tarafından ele alınmayan denetimleri içerir. Bu nedenle, Azure İlkesi'ndeki uyumluluk, genel uyumluluk durumunuza yalnızca kısmi bir görünümdür. Bu uyumluluk planı örneği için denetimler ve Azure İlkesi tanımları arasındaki ilişkilendirmeler zaman içinde değişebilir. Değişiklik geçmişini görüntülemek için [GitHub Commit History'ye](https://github.com/MicrosoftDocs/azure-docs/commits/master/articles/governance/blueprints/samples/HIPAA-HITRUST/control-mapping.md)bakın.

## <a name="control-against-malicious-code"></a>Kötü Amaçlı Koda Karşı Denetim

Bu plan, Azure Güvenlik Merkezi'ndeki sanal makinelerde eksik uç nokta korumasını izleyen ve Windows sanal makinelerde Microsoft kötü amaçlı yazılımdan koruma çözümünü uygulayan [Azure İlkesi](../../../policy/overview.md) tanımları atayarak kötü amaçlı kod koruması da dahil olmak üzere uç nokta korumayı yönetmenize yardımcı olur.

- Windows sunucusu için varsayılan Microsoft IaaS Kötü Amaçlı Yazılımdan Koruma uzantısını dağıtma
- Toplu iş hesaplarındaki tanılama günlükleri etkinleştirilmeli
- Makinelerinize sistem güncelleştirmeleri yüklenmelidir


## <a name="management-of-removable-media"></a>Çıkarılabilir Medya Yönetimi

Kuruluş, veri sınıflandırma düzeyine bağlı olarak, kullanımdan önce medyayı (dizüstü bilgisayarlar dahil) kaydeder, bu ortamın nasıl kullanılacağına ilişkin makul kısıtlamalar koyar ve uygun düzeyde fiziksel ve mantıksal koruma (şifreleme dahil) sağlar. düzgün bir şekilde yok edilene veya dezenfekte edilene kadar kapalı bilgiler içeren ortamlar.

- Data Lake Store hesaplarında şifreleme gerektirme
- SQL yönetilen örnek TDE koruyucukendi anahtarı ile şifrelenmelidir
- Disk şifreleme sanal makinelerde uygulanmalıdır
- SQL veritabanlarında Saydam Veri Şifrelemesi etkinleştirilmelidir


## <a name="information-exchange-policies-and-procedures"></a>Bilgi Alışverişi Politikaları ve Prosedürleri

Bulut hizmeti sağlayıcıları, birlikte çalışabilirliği sağlamaya yardımcı olmak için endüstri tarafından tanınan bir sanallaştırma platformu ve standart sanallaştırma biçimleri (örneğin, Açık Sanallaştırma Biçimi, OVF) kullanır ve kullanımdaki herhangi bir hipervizörde yapılan özel değişiklikleri belgelemiştir ve tüm çözüme özel sanallaştırma kancaları müşteri incelemesi için kullanılabilir.

- Belirtilen uygulamaları yüklü olmayan Windows VM'lerini denetlemek için ön koşulları dağıtma

## <a name="control-of-operational-software"></a>Operasyonel Yazılımın Kontrolü 

Kuruluş, sunucular, iş istasyonları ve dizüstü bilgisayarlar da dahil olmak üzere bilgi sisteminde ki yetkisiz yazılımları tanımlar, bilgi sisteminde bilinen yetkisiz yazılımların yürütülmesini yasaklamak için her şeye izin veren, istisnai bir politika uygular ve yetkisiz yazılım listesini periyodik olarak ancak yıllık olarak en az olarak inceler ve günceller.

- \[Önizleme\] "Güvenlik Seçenekleri-Denetim" içinde Windows VM yapılandırmalarından denetim sonuçlarını göster
- \[Önizleme\] "Sistem Denetim İlkeleri- Hesap Yönetimi"nde Windows VM yapılandırmalarından gelen denetim sonuçlarını göster

## <a name="change-control-procedures"></a>Denetim Prosedürlerini Değiştir

Tüm sanal makine görüntülerinin bütünlüğü, sanal makine görüntülerinde yapılan değişiklikler için her zaman bir uyarı oluşturarak ve işletme sahibinin(ler) ve/veya müşteri(ler) elektronik yöntemlerle (örn. portallar veya uyarılar) bir değişikliğin veya hareketin sonuçlarını ve görüntünün bütünlüğünün sonraki doğrulaması yoluyla sağlanır.

- \[Önizleme\] Windows VM yapılandırmasından gelen denetim sonuçlarını 'Sistem Denetim İlkeleri -Ayrıntılı İzleme' adlı yapıda göster

## <a name="inventory-of-assets"></a>Varlıkların Envanteri 

Varlıkların ve hizmetlerin envanteri korunur

- Arama hizmetlerinde tanılama günlükleri etkinleştirilmelidir.
- \["Güvenlik Seçenekleri- Microsoft Network Server"da Windows VM yapılandırmalarını denetlemek için dağıtma gereksinimlerini önizleme\]
- \["Yönetimşablonları- Ağ"da Windows VM yapılandırmalarını denetlemek için dağıtma gereksinimlerini önizleme\]

## <a name="control-of-technical-vulnerabilities"></a>Teknik Güvenlik Açıklarının Kontrolü 

Tüm sistem ve ağ bileşenleri için sertleştirilmiş bir yapılandırma standardı vardır.

- Olağanüstü durum kurtarma yapılandırması yapılmadan Sanal Makineleri Denetleme
- SQL yönetilen örneklerinizde güvenlik açığı değerlendirmesi etkinleştirilmelidir
- Güvenlik Açığı, Güvenlik Açığı Değerlendirmesi çözümü yle düzeltilmelidir

## <a name="segregation-in-networks"></a>Ağlarda Ayrımcılık

Kuruluşun güvenlik ağ geçitleri (örn. güvenlik duvarları) güvenlik ilkeleri uygular ve etki alanları arasındaki trafiği filtreleyecek, yetkisiz erişimi engelleyecek şekilde yapılandırılır ve dahili kablolu, dahili kablosuz ve harici bağlantı ayrımını korumak için kullanılır DMZ'ler de dahil olmak üzere ağ segmentleri (örneğin, Internet) ve etki alanlarının her biri için erişim denetimi ilkelerini uygulayın.

- Güvenlik İzleme aracısının Otomatik Olarak Sağlanması
- Sanal ağlar oluşturulduğunda ağ izleyicisini dağıtma

## <a name="input-data-validation"></a>Giriş Veri Doğrulama

Kamuya açık web uygulamaları için, trafiği denetlemek için uygulama düzeyinde güvenlik duvarları uygulanır. Web tabanlı olmayan genel kullanıma dönük uygulamalar için kuruluş, uygulama türüne özgü ağ tabanlı bir güvenlik duvarı uygulamıştır. Genel kullanıma açık uygulamaya yönelik trafik şifrelenirse, aygıt şifrelemenin arkasında oturur veya çözümlemeden önce trafiğin şifresini çözebiliyor.

- \[Önizleme\] "Windows Güvenlik Duvarı Özellikleri"nde Windows VM yapılandırmasından gelen denetim sonuçlarını göster


## <a name="network-connection-control"></a>Ağ Bağlantı Kontrolü

Ağ trafiği, her ağ erişim noktası veya harici telekomünikasyon hizmetinin yönetilen arabirimi için güvenlik duvarı ve ağla ilgili diğer kısıtlamalar aracılığıyla kuruluşun erişim kontrol ilkesine uygun olarak denetlenir.

- Internet'e bakan uç noktalarüzerinden erişim kısıtlanmalıdır
- SQL yönetilen örnek TDE koruyucukendi anahtarı ile şifrelenmelidir
- API uygulamaları için uzaktan hata ayıklama kapatılmalıdır

## <a name="network-controls"></a>Ağ Denetimleri

Kuruluş, fiziksel sunucuları, uygulamaları veya verileri sanallaştırılmış sunuculara aktarırken güvenli ve şifreli iletişim kanallarını kullanır.

- VM'lerde disk şifreleme uygulanmalıdır
- SQL server TDE koruyucusu kendi anahtarınızla şifrelenmelidir
- \[Önizleme\] "Güvenlik Seçenekleri- Ağ Erişimi"nde Windows VM yapılandırmalarından gelen denetim sonuçlarını göster
- Depolama hesaplarına sınırsız ağ erişimini denetleme
- \[Önizleme\] "Windows Güvenlik Duvarı Özellikleri"nde Windows VM Yapılandırmasından gelen denetim sonuçlarını göster
- Ağ Güvenlik Gruplarından Tanılama ayarlarını dağıtma
- Internet'e bakan uç noktalarüzerinden erişim kısıtlanmalıdır

## <a name="sensitive-system-isolation"></a>Hassas Sistem Yalıtımı

Paylaşılan sistem kaynakları (örn. kayıtlar, ana bellek, ikincil depolama) sisteme geri salınır, diğer sistemlere/uygulamalara/kullanıcılara ifşa olmaktan korunur ve kullanıcılar bilgi kalıntılarına kasıtlı veya kasıtsız olarak erişemez.

- Sanal Makineler yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir

## <a name="security-of-network-services"></a>Ağ Hizmetlerinin Güvenliği

Bir ağ servis sağlayıcısı/yöneticisi tarafından sağlanan kabul edilen hizmetler, güvenli bir şekilde sağlandığından emin olmak için resmi olarak yönetilir ve izlenir.

- Sanal Makineler yeni Azure Kaynak Yöneticisi kaynaklarına geçirilmelidir

## <a name="network-routing-control"></a>Ağ Yönlendirme Kontrolü

Yönlendirme denetimleri, iç ve dış ağlar (örn. Internet ve üçüncü taraf ağları) arasında kullanılan güvenlik ağ geçitleri (örn. güvenlik duvarları) aracılığıyla uygulanır.

- Sanal Makinelerde Uyarlanabilir Uygulama Denetimleri etkinleştirilmelidir

## <a name="information-exchange-policies-and-procedures"></a>Bilgi Alışverişi Politikaları ve Prosedürleri

Kuruluş, dış bilgi sistemlerinde yetkili kişiler tarafından organizasyon kontrollü taşınabilir depolama ortamının kullanımını sınırlar.

- Depolama hesaplarına sınırsız ağ erişimini denetleme
- Web uygulamaları için uzaktan hata ayıklama kapatılmalıdır
- APi Uygulamasına yalnızca HTTPS üzerinden erişilebilmeli

## <a name="electronic-messaging"></a>Elektronik Mesajlaşma

Onaylar, anlık ileti veya dosya paylaşımı da dahil olmak üzere harici kamu hizmetlerini kullanmadan önce alınır.

- \[Önizleme\] 0644 olarak ayarlanmış parola dosyası izinleri olmayan Linux VM'lerden gelen denetim sonuçlarını göster

## <a name="on-line-transactions"></a>On-Line İşlemler

Kuruluş, işlemde yer alan tarafların her biri tarafından elektronik imzalar arasında şifreleme kullanımını ve elektronik imzaların kullanılmasını gerektirir. Kuruluş, işlem ayrıntılarının kamuya açık ortamların dışında (örneğin, kuruluşun intranetinde bulunan bir depolama platformunda) ve Internet'ten doğrudan erişilebilen bir depolama ortamında tutulmamasını ve maruz kalmamasını sağlar.Güvenilir bir yetkilinin kullanıldığı yerde (örn. dijital imza ve/veya dijital imza ların verilmesi ve saklanması amacıyla), güvenlik tüm uçtan uca sertifika/yönetim boyunca entegre edilmiştir ve Işlem.

- VM'lerde disk şifreleme uygulanmalıdır
- \[Önizleme,\] belirtilen sertifikaları güvenilir kökte içermeyen Windows VM'lerinden gelen denetim sonuçlarını göster

## <a name="password-management"></a>Parola Yönetimi

Parolalar tüm sistem bileşenlerinde iletim ve depolama sırasında şifrelenir.

- \[Önizleme\] Parola karmaşıklığı ayarını etkinleştirmeyen windows VM'lerinden denetim sonuçlarını göster

## <a name="user-authentication-for-external-connections"></a>Harici Bağlantılar için Kullanıcı Kimlik Doğrulaması

Çok faktörlü, Yarıçap veya Kerberos (ayrıcalıklı erişim için) ve CHAP (çevirmeli bağlantı yöntemleri için kimlik bilgilerinin şifrelemesi için) gibi güçlü kimlik doğrulama yöntemleri, kuruluşlar ağına yapılan tüm dış bağlantılar için uygulanır.

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- Just In Time ağ erişim kontrolü VM'lerde uygulanmalıdır

## <a name="user-identification-and-authentication"></a>Kullanıcı Kimliği ve Kimlik Doğrulama

Ayrıcalıklı işlevleri gerçekleştiren kullanıcılar (örneğin, sistem yönetimi) bu ayrıcalıklı işlevleri gerçekleştirirken ayrı hesaplar kullanır. Çok faktörlü kimlik doğrulama yöntemleri, kuruluş ilkesine (örn. uzak ağ erişimi için) uygun olarak kullanılır.

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir
- Just In Time ağ erişim kontrolü VM'lerde uygulanmalıdır

## <a name="privilege-management"></a>Ayrıcalık Yönetimi

Sanallaştırılmış sistemlere ev sahipliği yapan sistemler için yönetim işlevlerine veya yönetim konsollarına erişim, en az ayrıcalık ilkesine dayalı olarak personelle sınırlıdır ve teknik kontroller yoluyla desteklenir.

- Just In Time ağ erişim kontrolü VM'lerde uygulanmalıdır
- \[Önizleme\] Rolü Tabanlı Erişim Kontrolü (RBAC) Kubernetes hizmetlerinde kullanılmalıdır

## <a name="review-of-user-access-rights"></a>Kullanıcı Erişim Haklarının Gözden Geçirilmesi

Kuruluş, bilgi varlıklarının yetkili kullanıcılarının belgelenmiş bir listesini tutar.

- \[Önizleme\] "Güvenlik Seçenekleri- Hesaplar"daki Windows VM yapılandırmalarından gelen denetim sonuçlarını göster

## <a name="remote-diagnostic-and-configuration-port-protection"></a>Uzaktan Tanılama ve Yapılandırma Bağlantı Noktası Koruması

İş işlevselliği için özel olarak gerekli olmayan bilgisayar veya ağ sistemlerine yüklenen bağlantı noktaları, hizmetler ve benzeri uygulamalar devre dışı bırakılır veya kaldırılır.

- Yönetim bağlantı noktaları sanal makinelerinizde kapatılmalıdır
- Sanal makineler ölçek kümelerinizdeki güvenlik yapılandırmasındaki güvenlik açıkları düzeltilmelidir

## <a name="audit-logging"></a>Denetim Günlüğü

Gönderilen ve alınan iletilerin günlükleri, iletinin tarihi, saati, kaynağı ve hedefi dahil olmak üzere tutulur, ancak içeriğini içermez. Sistem etkinken ve önemli olayları, başarı/başarısız veri erişimini, sistem güvenliği yapılandırma değişikliklerini, ayrıcalıklı veya yardımcı program kullanımını, yükseltilen alarmları, koruma sistemlerinin aktivasyonunu ve devre dışı bırakılmasını (örn. A/V ve IDS), tanımlama ve kimlik doğrulama mekanizmalarının aktivasyonu ve devre dışı bırakılması ve sistem düzeyindeki nesnelerin oluşturulması ve silinmesini izlerken denetim her zaman kullanılabilir.

- Event Hub'daki tanılama günlükleri etkinleştirilmeli
- Sanal makine ölçek kümelerinde sistem güncellemeleri yüklenmelidir

## <a name="monitoring-system-use"></a>İzleme Sistemi Kullanımı

Kuruluşun ortamında dağıtılan otomatik sistemler, önemli olayları ve anormal etkinlikleri izlemek ve sonuçları düzenli olarak gözden geçirilen sistem günlüklerini analiz etmek için kullanılır. İzleme, devre dışı bırakılmış hesaplara erişim girişimleri ve sistem uyarıları veya hataları da dahil olmak üzere ayrıcalıklı işlemleri, yetkili erişimi veya yetkisiz erişim girişimlerini içerir.

- Sanal Makine ölçek kümelerinde tanılama günlükleri etkinleştirilmelidir

## <a name="segregation-of-duties"></a>Görevlerin Ayrılması

Görev ayrılığı, bilgi ve sistemlerin yetkisiz veya kasıtsız olarak değiştirilmesi riskini sınırlamak için kullanılır. Hiçbir kişi yetkilendirme veya algılama olmadan bilgi sistemlerine erişemez, değiştiremez veya kullanamaz. Denetimleri yönetmek ve erişimden sorumlu kişilerin erişimi, her kullanıcının rolü ve sorumluluklarına bağlı olarak gereken minimum la sınırlıdır ve bu kişiler bu denetimlerle ilgili denetim işlevlerine erişemez.

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir

## <a name="administrator-and-operator-logs"></a>Yönetici ve Operatör Günlükleri

Kuruluş, yönetici etkinliklerini denetlemek için uygun günlüğe kaydetmenin etkin olmasını sağlar; ve sistem yöneticisi ve operatör günlüklerini düzenli olarak inceler.

- MFA, aboneliğinizde yazma izinleri olan hesaplar etkinleştirilmelidir

## <a name="identification-of-risks-related-to-external-parties"></a>Dış Taraflarla İlgili Risklerin Belirlenmesi

Kuruluş ve dış taraflar arasındaki uzaktan erişim bağlantıları şifrelenir

- Disk şifreleme Sanal makinelerde uygulanmalıdır

## <a name="business-continuity-and-risk-assessment"></a>İş Sürekliliği ve Risk Değerlendirmesi

Kuruluş, kritik iş süreçlerini belirler ve iş sürekliliğinin bilgi güvenliği yönetimi gereksinimlerini operasyonlar, personel, malzeme, ulaşım gibi diğer süreklilik gereksinimleriyle bütünleştirir. ve tesisler.

- \[Önizleme\] "Güvenlik Seçenekleri- Kurtarma Konsolu"nda Windows VM yapılandırmasından gelen denetim sonuçlarını göster

> [!NOTE]
> Azure İlkesi tanımlarının kullanılabilirliği Azure Kamu ve diğer ulusal bulutlarda farklılık gösterebilir. 

## <a name="next-steps"></a>Sonraki adımlar

HIPAA HITRUST plan örneğinin kontrol eşlemi gözden geçirdiniz. Ardından, genel bakış ve bu örneğin nasıl dağıtılacak hakkında bilgi edinmek için aşağıdaki makaleleri ziyaret edin:

> [!div class="next step action"]
> [HIPAA HITRUST planı - Genel Bakış](./control-mapping.md)
> [HIPAA HITRUST planı - Dağıtım adımları](./deploy.md)

Şemalar ve bunların kullanımı hakkındaki diğer makaleler:

- [Plan yaşam döngüsü](../../concepts/lifecycle.md)hakkında bilgi edinin.
- [Statik ve dinamik parametrelerin](../../concepts/parameters.md) kullanımını anlayın.
- [Şema sıralama düzenini](../../concepts/sequencing-order.md) özelleştirmeyi öğrenin.
- [Şema kaynak kilitleme](../../concepts/resource-locking.md) özelliğini kullanmayı öğrenin.
- Varolan atamaları nasıl [güncelleştirini](../../how-to/update-existing-assignments.md)öğrenin.
