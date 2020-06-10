---
title: Azure Güvenlik Merkezi için sürüm notları
description: Azure Güvenlik Merkezi 'nde nelerin yeni ve değiştirilmiş olduğuna ilişkin bir açıklama.
services: security-center
documentationcenter: na
author: memildin
manager: rkarlin
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/21/2020
ms.author: memildin
ms.openlocfilehash: 688f04ecf951265b9015b9516d63cf3399adcdc1
ms.sourcegitcommit: 5a8c8ac84c36859611158892422fc66395f808dc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84655462"
---
# <a name="whats-new-in-azure-security-center"></a>Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?

Azure güvenliği etkin bir geliştirme aşamasındadır ve gelişmede geliştirmeler sunar. En son gelişmelerden haberdar olmak için, bu sayfada hakkında bilgi verilmektedir:

- Yeni özellikler
- Hata düzeltmeleri
- Kullanım dışı işlevsellik

Bu sayfa düzenli olarak güncelleştirildiğinden, sık sık ziyaret edin. Altı aydan eski olan öğeleri arıyorsanız, [Azure Güvenlik Merkezi 'ndeki yenilikler Için arşivde](release-notes-archive.md)bulabilirsiniz.


## <a name="may-2020"></a>Mayıs 2020


### <a name="alert-suppression-rules-preview"></a>Uyarı gizleme kuralları (Önizleme)

Bu yeni Özellik (Şu anda önizlemede), uyarı fatıg azaltmaya yardımcı olur. Zararsız olarak bilinen veya kuruluşunuzdaki normal etkinliklerle ilgili olan uyarıları otomatik olarak gizlemek için kuralları kullanın. Bu, en ilgili tehditlere odaklanmanızı sağlar. 

Etkin gizleme kuralları ile eşleşen uyarılar üretilmeye devam eder, ancak durumları kapatıldı olarak ayarlanır. Durumu Azure portal görebilirsiniz, ancak Güvenlik Merkezi güvenlik uyarılarınıza erişebilirsiniz.

Gizleme kuralları, uyarıların otomatik olarak hangi ölçütlere göre kapatılacağını tanımlar. Genellikle bir gizleme kuralı kullanarak şunları yapabilirsiniz:

- yanlış pozitif sonuçlar olarak belirlediğiniz uyarıları gösterme

- çok sık tetiklenen uyarıları, faydalı olması için gösterme

[Azure Güvenlik Merkezi 'nin tehdit korumasından gelen uyarıları gizleme](alerts-suppression-rules.md)hakkında daha fazla bilgi edinin.


### <a name="virtual-machine-vulnerability-assessment-is-now-generally-available"></a>Sanal makine güvenlik açığı değerlendirmesi genel kullanıma sunuldu

Güvenlik Merkezi 'nin standart katmanı artık, ek ücret ödemeden sanal makinelere yönelik tümleşik bir güvenlik açığı değerlendirmesi içerir. Bu uzantı Qualys tarafından desteklenmektedir ancak bulgularını doğrudan güvenlik merkezi 'ne geri bildirir. Bir Qualys lisansına veya hatta bir Qualys hesabına ihtiyacınız yoktur. her şey güvenlik merkezi 'nin içinde sorunsuz bir şekilde işlenir.

Yeni çözüm Güvenlik Merkezi 'nde güvenlik açıklarını bulmak ve bulguları sunmak için sanal makinelerinizi sürekli tarayabilir. 

Çözümü dağıtmak için yeni güvenlik önerisini kullanın:

"Yerleşik güvenlik açığı değerlendirme çözümünü sanal makinelerde etkinleştir (Qualys tarafından desteklenir)"

[Güvenlik Merkezi 'nin sanal makineler için tümleşik güvenlik açığı değerlendirmesi](built-in-vulnerability-assessment.md)hakkında daha fazla bilgi edinin.



### <a name="changes-to-just-in-time-jit-virtual-machine-vm-access"></a>Tam zamanında (JıT) sanal makine (VM) erişiminde yapılan değişiklikler

Güvenlik Merkezi, sanal makinelerinizin yönetim bağlantı noktalarını korumak için isteğe bağlı bir özellik içerir. Bu, deneme yanılma saldırılarına karşı en yaygın bir savunma biçimine karşı savunma sağlar.

Bu güncelleştirme, bu özelliğe aşağıdaki değişiklikleri getirir:

- Bir VM üzerinde JıT 'i etkinleştirmenizi öneren öneri yeniden adlandırıldı. Eskiden, "tam zamanında ağ erişim denetimi sanal makinelere uygulanmalıdır": "sanal makinelerin yönetim bağlantı noktaları tam zamanında ağ erişim denetimiyle korunmalıdır".

- Öneri yalnızca açık yönetim bağlantı noktaları varsa tetiklenecek şekilde ayarlanmıştır.

[JIT erişim özelliği](security-center-just-in-time.md)hakkında daha fazla bilgi edinin.


### <a name="custom-recommendations-have-been-moved-to-a-separate-security-control"></a>Özel öneriler ayrı bir güvenlik denetimine taşındı

Gelişmiş güvenli puanla birlikte sunulan güvenlik denetimlerinden biri "en iyi güvenlik uygulamalarını uygulama" idi. Abonelikleriniz için oluşturulan tüm özel öneriler otomatik olarak bu denetime yerleştirildi. 

Özel önerilerinizi bulmayı kolaylaştırmak için, bunları "özel öneriler" olarak ayrılmış bir güvenlik denetimine taşıdık. Bu denetim, güvenli puanınızı etkilemez.

[Azure Güvenlik Merkezi 'Nde gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)bölümünde güvenlik denetimleri hakkında daha fazla bilgi edinin.


### <a name="toggle-added-to-view-recommendations-in-controls-or-as-a-flat-list"></a>Denetimlere veya düz liste olarak önerilere bakmak için eklenen değiştirme

Güvenlik denetimleri, ilgili güvenlik önerilerinin mantıksal gruplarıdır. Bunlar, savunmasız saldırı yüzeylerinizi yansıtır. Denetim, bu önerileri uygulamanıza yardımcı olan yönergelerden oluşan bir güvenlik önerileri kümesidir.

Kuruluşunuzun her bir saldırı yüzeyini ne kadar iyi güvenlik altına almak için her güvenlik denetiminin puanlarını gözden geçirin.

Varsayılan olarak, önerileriniz güvenlik denetimlerinde gösterilmiştir. Bu güncelleştirmeden, bunları bir liste olarak da görüntüleyebilirsiniz. Etkilenen kaynakların sistem durumuna göre sıralanmış basit liste olarak görüntülemek için, yeni ' denetimleri grupla ' seçeneğini kullanın. Geçiş, portaldaki listenin üzerinde bulunur.

Güvenlik denetimleri ve bu geçiş, yeni güvenli puan deneyiminin bir parçasıdır. Portalın içinden görüşlerinizi bize göndermeyi unutmayın.

[Azure Güvenlik Merkezi 'Nde gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)bölümünde güvenlik denetimleri hakkında daha fazla bilgi edinin.


### <a name="expanded-security-control-implement-security-best-practices"></a>Genişletilmiş Güvenlik denetimi "en iyi güvenlik uygulamalarını uygulayın" 

Gelişmiş güvenli puanla birlikte sunulan güvenlik denetimlerinden biri "en iyi güvenlik uygulamalarını uygulama" dir. Bu denetimde bir öneri olduğunda, güvenli puanı etkilemez. 

Bu güncelleştirmeyle, üç öneri özgün olarak yerleştirildiği denetimlerden ve bu en iyi yöntemler denetimine taşınmıştır. Bu üç önerinin riskini başlangıçta düşündükten daha düşük olduğunu belirlediğimiz için bu adımı aldık.

Ayrıca, bu denetime iki yeni öneri eklenmiştir ve eklendi.

Taşınan üç öneri şunlardır:

- **MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir** (BAŞLANGıÇTA "MFA 'yı etkinleştir" denetiminde)
- **Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir** (başlangıçta "erişim ve izinleri yönetme" denetiminde)
- **Aboneliğiniz için en fazla 3 sahip** belirtilmelidir (başlangıçta "erişim ve izinleri yönetme" denetiminde)

Denetime eklenen iki yeni öneri şunlardır:

- **Konuk yapılandırma uzantısının Windows sanal makinelerinde (Önizleme) yüklü olması gerekir** . [Azure ilkesi Konuk yapılandırması](https://docs.microsoft.com/azure/governance/policy/concepts/guest-configuration) kullanarak, sanal makinelerin içinde sunucu ve uygulama ayarlarına (yalnızca Windows) yönelik görünürlük sağlar.

- **Makinelerinizde Windows Defender Exploit Guard etkinleştirilmelidir (Önizleme)** -Windows Defender Exploit Guard, Azure Ilke Konuk yapılandırma aracısından yararlanır. Exploit Guard, kuruluşların güvenlik riskini ve üretkenlik gereksinimlerini (yalnızca Windows) dengeleyebilmesini sağlarken, cihazları birçok farklı saldırı vektörü ve çok sayıda kötü amaçlı yazılım saldırılarında yaygın olarak kullanılan blok davranışlarına karşı kilitlemek için tasarlanan dört bileşene sahiptir.

[Exploit Guard Ilkesi oluşturma ve dağıtma](https://docs.microsoft.com/mem/configmgr/protect/deploy-use/create-deploy-exploit-guard-policy)bölümünde Windows Defender Exploit Guard hakkında daha fazla bilgi edinin.

[Azure Güvenlik Merkezi 'Nde gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)bölümünde güvenlik denetimleri hakkında daha fazla bilgi edinin.



### <a name="custom-policies-with-custom-metadata-are-now-generally-available"></a>Özel meta verileri olan özel ilkeler artık genel kullanıma sunuldu

Özel ilkeler artık Güvenlik Merkezi öneri deneyiminin, güvenli puanın ve mevzuat uyumluluk standartları panosunun bir parçasıdır. Bu özellik genel kullanıma sunulmuştur ve kuruluşunuzun güvenlik değerlendirmesi kapsamını güvenlik merkezi 'nde genişletmenizi sağlar. 

Azure ilkesinde özel bir girişim oluşturun, buna ilke ekleyin ve Azure Güvenlik Merkezi 'ne ekleyin ve öneriler olarak görselleştirin.

Artık özel öneri meta verilerini düzenleme seçeneğini de ekledik. Meta veri seçenekleri önem derecesi, düzeltme adımları, tehdit bilgileri ve daha fazlasını içerir.  

[Ayrıntılı bilgilerle özel önerilerinizi geliştirme](custom-security-policies.md#enhancing-your-custom-recommendations-with-detailed-information)hakkında daha fazla bilgi edinin.



### <a name="crash-dump-analysis-capabilities-migrating-to-fileless-attack-detection"></a>Kilitlenme bilgi döküm çözümleme özellikleri, dosya daha az saldırı algılamasına geçiriliyor 

Windows kilitlenme dökümü Analizi (CDA) algılama yeteneklerini [dosya daha az saldırı algılamasında](https://docs.microsoft.com/azure/security-center/threat-protection#windows-fileless)tümleştireceğiz. Dosya daha az saldırı algılama analizi, Windows makineleri için aşağıdaki güvenlik uyarılarının gelişmiş sürümlerini getirir: kod ekleme keşfedilen, kendini gizleyen Windows modülü algılanan, shellcode keşfedilen ve şüpheli kod segmenti tespit etti.

Bu geçişin avantajlarından bazıları:

- **Proaktif ve zamanında kötü amaçlı yazılım algılama** -bir kilitlenme oluşması ve sonra kötü amaçlı yapıtları bulmak için analiz çalıştırma gibi CDA yaklaşım. Dosya kullanmayan saldırı algılama özelliğinin kullanılması, çalışırken bellek içi tehditlere yönelik proaktif kimlik tanımlamayı sağlar. 

- **Zenginleştirilmiş uyarılar** -dosya daha az saldırı algılamasında bulunan güvenlik uyarıları,, etkin ağ bağlantısı bilgileri gıbı, CDA ' den kullanılamayan zenginler içerir. 

- **Uyarı toplama** -CDA, tek çökme dökümü içinde birden çok saldırı deseni algıladığında birden çok güvenlik uyarısı tetikledi. Dosya Less saldırı algılaması, aynı işlemden alınan tüm saldırı düzenlerini tek bir uyarıya birleştirerek birden çok uyarıyı ilişkilendirme gereksinimini ortadan kaldırır.

- **Log Analytics çalışma alanınızda daha az gereksinim** vardır; potansiyel olarak hassas veriler içeren kilitlenme dökümleri artık Log Analytics çalışma alanınıza yüklenmeyecektir.



## <a name="april-2020"></a>Nisan 2020

### <a name="dynamic-compliance-packages-are-now-generally-available"></a>Dinamik uyumluluk paketleri artık genel kullanıma sunuldu

Azure Güvenlik Merkezi mevzuat uyumluluk panosu artık ek sektör ve yasal standartları izlemek için **dinamik uyumluluk paketleri** (genel kullanıma sunuldu) içerir.

Dinamik uyumluluk paketleri, Güvenlik Merkezi güvenlik ilkesi sayfasından aboneliğinize veya yönetim grubunuza eklenebilir. Bir standart veya kıyaslama eklendi olduğunuzda, değerlendirme olarak eşlenen tüm ilişkili uyumluluk verileri ile yasal uyumluluk panonuzda standart görüntülenir. Eklendi olan standartlardan herhangi biri için bir özet raporu indirilebilir.

Şimdi, şöyle bir standartlar ekleyebilirsiniz:

- **NIST SP 800-53 R4**
- **SWIFT CSP CSCF-V2020**
- **UK resmi ve UK NHS**
- **Kanada Federal PBMM**
- **Azure CIS 1.1.0 (yeni)** (Azure CIS 1.1.0 'ın daha kapsamlı bir gösterimi)

Ayrıca, Azure **güvenlik kıyaslaması**' nı son zamanlarda, genel uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan Azure 'a özgü yönergeleri ekledik. Panoda kullanılabilir hale geldiğinde ek standartlar desteklenecektir.  
 
[Yasal uyumluluk panonuzda standartlar kümesini özelleştirme](update-regulatory-compliance-packages.md)hakkında daha fazla bilgi edinin.


### <a name="identity-recommendations-now-included-in-azure-security-center-free-tier"></a>Kimlik önerileri artık Azure Güvenlik Merkezi Ücretsiz katmanına eklenmiştir

Azure Güvenlik Merkezi Ücretsiz katmanında kimlik ve erişime yönelik güvenlik önerileri artık genel kullanıma sunulmuştur. Bu, bulut güvenlik durure yönetimi (CSPM) özelliklerini ücretsiz hale getirme çabalarının bir parçasıdır. Şu anda bu öneriler yalnızca standart fiyatlandırma katmanında kullanılabilir.

Kimlik ve erişim önerilerine örnek olarak şunlar verilebilir:

- "Çok faktörlü kimlik doğrulaması, aboneliğiniz üzerinde sahip izinleri olan hesaplarda etkinleştirilmelidir."
- "Aboneliğiniz için en fazla üç sahip belirtilmelidir."
- "Kullanımdan kaldırılan hesaplar aboneliğinizden kaldırılmalıdır."

Ücretsiz fiyatlandırma katmanında abonelikleriniz varsa, bu değişiklik, kimlik ve erişim güvenliği için hiçbir şekilde değerlendirilmediğinden, bu değişiklikten etkilenmez.

[Kimlik ve erişim önerileri](recommendations-reference.md#recs-identity)hakkında daha fazla bilgi edinin.

[Kimlik ve erişim izleme](security-center-identity-access.md)hakkında daha fazla bilgi edinin.


## <a name="march-2020"></a>Mart 2020

### <a name="workflow-automation-is-now-generally-available"></a>İş akışı Otomasyonu artık genel olarak kullanılabilir

Azure Güvenlik Merkezi 'nin iş akışı Otomasyonu özelliği genel kullanıma sunuldu. Güvenlik uyarılarında ve önerilerinde Logic Apps otomatik olarak tetiklemek için bu uygulamayı kullanın. Ayrıca, uyarılar ve hızlı düzelme seçeneği bulunan tüm öneriler için el ile Tetikleyiciler kullanılabilir.

Her güvenlik programı, olay yanıtı için birden çok iş akışı içerir. Bu işlemler ilgili hissedarları bildirmeyi, bir değişiklik yönetimi işlemini başlatmayı ve belirli düzeltme adımlarını uygulamayı içerebilir. Güvenlik uzmanları, yaptığınız gibi bu yordamların pek çok adımını otomatikleştirmenizi önerir. Otomasyon, yükü azaltır ve işlem adımlarının hızlı, tutarlı bir şekilde ve önceden tanımlanmış gereksinimlerinize göre hızla yapılmasını sağlayarak güvenliği geliştirebilir.

İş akışlarınızı çalıştırmaya yönelik otomatik ve el ile güvenlik merkezi özellikleri hakkında daha fazla bilgi için bkz. [iş akışı Otomasyonu](workflow-automation.md).

[Logic Apps oluşturma](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview)hakkında daha fazla bilgi edinin.


### <a name="integration-of-azure-security-center-with-windows-admin-center"></a>Azure Güvenlik Merkezi 'Ni Windows Yönetim Merkezi ile tümleştirme

Artık şirket içi Windows Server 'larınızı Windows Yönetim merkezinden doğrudan Azure Güvenlik Merkezi 'ne taşımak mümkündür. Güvenlik Merkezi, şirket içi sunucular, sanal makineler ve ek PaaS iş yükleri dahil olmak üzere tüm Windows Yönetim Merkezi kaynaklarınızın güvenlik bilgilerini görüntülemek için tek bir cam bölmesinizde olur.

Bir sunucuyu Windows Yönetim merkezinden Azure Güvenlik Merkezi 'ne taşıdıktan sonra şunları yapabilirsiniz:

- Windows Yönetim Merkezi 'nin Güvenlik Merkezi uzantısında güvenlik uyarılarını ve önerilerini görüntüleyin.
- Güvenlik duruşunu görüntüleyin ve Azure portal (veya bir API aracılığıyla) Güvenlik Merkezi 'nde Windows Yönetim Merkezi tarafından yönetilen sunucularınızın ek ayrıntılı bilgilerini alın.

[Azure Güvenlik Merkezi 'Ni Windows Yönetim Merkezi ile tümleştirme](windows-admin-center-integration.md)hakkında daha fazla bilgi edinin.


### <a name="protection-for-azure-kubernetes-service"></a>Azure Kubernetes hizmeti için koruma

Azure Güvenlik Merkezi, Azure Kubernetes hizmetini (AKS) korumak için kapsayıcı güvenlik özelliklerini genişletiyor.

Popüler, açık kaynaklı platform Kubernetes, artık kapsayıcı düzenlemesi için bir sektör standardı olduğunu büyük ölçüde benimsemiştir. Bu yaygın uygulamaya rağmen, bir Kubernetes ortamının güvenliğini sağlama hakkında daha fazla bilgiye sahip olmaya devam edebilirsiniz. Kapsayıcılı bir uygulamanın saldırı yüzeylerini erteleme, altyapının güvenli bir şekilde yapılandırılmasını sağlamak ve olası tehditlere karşı sürekli izlenmeleri için uzmanlık gerektirir.

Güvenlik Merkezi savunması şunları içerir:

- Güvenlik Merkezi 'ne kayıtlı abonelikler içinde yönetilen AKS örneklerinin **sürekli bulunması.**
- **Güvenlik önerileri** -aks için en iyi güvenlik uygulamalarıyla uyum sağlamanıza yardımcı olacak öneriler. Bu öneriler, kuruluşunuzun güvenlik duruşunuzun bir parçası olarak görüntülendiklerinden emin olmak için güvenli puanına dahil edilmiştir. "Bir Kubernetes hizmet kümesine erişimi kısıtlamak için rol tabanlı erişim denetimi kullanılmalıdır" ifadesini görebilirsiniz.
- **Tehdit koruması** -aks dağıtımınızın sürekli analizi sayesinde, Güvenlik Merkezi, ana bilgisayar ve aks kümesi düzeyinde algılanan tehditler ve kötü amaçlı etkinlikler hakkında sizi uyarır.

[Azure Kubernetes Hizmetleri ' ni Güvenlik Merkezi ile tümleştirme](azure-kubernetes-service-integration.md)hakkında daha fazla bilgi edinin.

[Güvenlik Merkezi 'ndeki kapsayıcı güvenliği özellikleri](container-security.md)hakkında daha fazla bilgi edinin.


### <a name="improved-just-in-time-experience"></a>Anında iyileştirilmiş deneyim

Azure Güvenlik Merkezi 'nin Yönetim bağlantı noktalarınızı güvenli hale getirmeye yönelik tam zamanında araçlar, işlem ve Kullanıcı arabirimi aşağıdaki şekilde geliştirilmiştir: 

- **Gerekçe alanı** -Azure Portal tam zamanında sayfasında bir sanal MAKINEYE (VM) erişim isteğinde bulunduğunda, istek için bir gerekçe girmek üzere yeni bir isteğe bağlı alan kullanılabilir. Bu alana girilen bilgiler etkinlik günlüğünde izlenebilir. 
- **Yedekli tam zamanında (JIT) kuralları otomatik temizleme** -bir JIT ilkesini her güncelleştirdiğinizde, tüm RuleSet 'nizin geçerliliğini denetlemek için otomatik olarak bir temizleme aracı çalışır. Araç, ilkenizin kuralları ile NSG 'deki kurallar arasında uyuşmazlıkları arar. Temizleme Aracı bir uyumsuzluk bulursa, nedeni belirler ve bunu yapmak güvenli olduğunda, artık gerekmeyen yerleşik kuralları kaldırır. Temizleyici, oluşturduğunuz kuralları hiçbir şekilde silmez. 

[JIT erişim özelliği](security-center-just-in-time.md)hakkında daha fazla bilgi edinin.


### <a name="two-security-recommendations-for-web-applications-deprecated"></a>Web uygulamaları için iki güvenlik önerisi kullanım dışı

Web uygulamalarıyla ilgili iki güvenlik önerisi kullanım dışı bırakılmıştır: 

- IaaS NSG 'lerdeki Web uygulamalarına yönelik kurallar sağlamlaştırılmış olmalıdır.
    (İlgili ilke: IaaS üzerinde Web uygulamalarına yönelik NSG kuralları sağlamlaştırılmış olmalıdır)

- Uygulama hizmetlerine erişim kısıtlı olmalıdır.
    (İlgili ilke: uygulama hizmetlerine erişim kısıtlı olmalıdır [Önizleme])

Bu öneriler artık Güvenlik Merkezi öneri listesinde görünmez. İlgili ilkeler artık "Güvenlik Merkezi varsayılanı" adlı girişimde yer alınmaz.

[Güvenlik önerileri](recommendations-reference.md)hakkında daha fazla bilgi edinin.



## <a name="february-2020"></a>Şubat 2020

### <a name="fileless-attack-detection-for-linux-preview"></a>Linux için dosya daha az saldırı algılama (Önizleme)

Saldırganlar, algılamayı önlemek için stealthier yöntemleri kullanmayı artırarak, Azure Güvenlik Merkezi, Windows 'un yanı sıra Linux için dosya daha az saldırı algılamasını genişletmez. Dosya sistemi saldırıları Yazılım açıklarına karşı yararlanma, kötü amaçlı yükleri zararsız sistem işlemlerine ekleme ve bellekte gizleme. Bu teknikler:

- disk üzerinde kötü amaçlı yazılım izlemelerini en aza indirin veya kaldırın
- disk tabanlı kötü amaçlı yazılım tarama çözümlerine algılama olasılığını önemli ölçüde azaltma

Azure Güvenlik Merkezi, bu tehdidi ortadan silmek için, 2018 Ekim 'de Windows için dosya daha az saldırı algılaması yayımlamıştır ve artık Linux üzerinde dosya daha az saldırı algılama işlemi de genişletmiştir. 


## <a name="january-2020"></a>Ocak 2020

### <a name="enhanced-secure-score-preview"></a>Gelişmiş güvenli skor (Önizleme)

Azure Güvenlik Merkezi 'nin güvenli Puanlama özelliğinin gelişmiş bir sürümü artık önizlemede sunulmaktadır. Bu sürümde, çok sayıda öneri, güvenlik açıklarına karşı savunmasız saldırı yüzeylerinizi daha iyi yansıtarak (örneğin, yönetim bağlantı noktalarına erişimi kısıtla) güvenlik denetimlerinde gruplandırılır.

Önizleme aşamasında güvenli puan değişiklikleri hakkında bilgi edinin ve ortamınızı daha da güvenli hale getirmenize yardımcı olacak diğer düzeltmeleri öğrenin.

[Azure Güvenlik Merkezi 'Nde gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)hakkında daha fazla bilgi edinin.