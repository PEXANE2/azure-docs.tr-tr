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
ms.date: 05/11/2020
ms.author: memildin
ms.openlocfilehash: bfe1e5d6a0c4171a262b36387f02be356fb1d72d
ms.sourcegitcommit: a8ee9717531050115916dfe427f84bd531a92341
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83210902"
---
# <a name="whats-new-in-azure-security-center"></a>Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?

Azure güvenliği etkin bir geliştirme aşamasındadır ve gelişmede geliştirmeler sunar. En son gelişmelerden haberdar olmak için, bu sayfada hakkında bilgi verilmektedir:

- Yeni özellikler
- Hata düzeltmeleri
- Kullanım dışı işlevsellik

Bu sayfa düzenli olarak güncelleştirildiğinden, sık sık ziyaret edin. Altı aydan eski olan öğeleri arıyorsanız, [Azure Güvenlik Merkezi 'ndeki yenilikler Için arşivde](release-notes-archive.md)bulabilirsiniz.


## <a name="may-2020"></a>Mayıs 2020

### <a name="changes-to-just-in-time-jit-virtual-machine-vm-access"></a>Tam zamanında (JıT) sanal makine (VM) erişiminde yapılan değişiklikler

Güvenlik Merkezi, sanal makinelerinizin yönetim bağlantı noktalarını korumak için isteğe bağlı bir özellik içerir. Bu, deneme yanılma saldırılarına karşı en yaygın bir savunma biçimine karşı savunma sağlar.

Bu güncelleştirme, bu özelliğe aşağıdaki değişiklikleri getirir:

- Bir VM üzerinde JıT 'i etkinleştirmenizi öneren öneri yeniden adlandırıldı. Eskiden, "tam zamanında ağ erişim denetimi sanal makinelere uygulanmalıdır": "sanal makinelerin yönetim bağlantı noktaları tam zamanında ağ erişim denetimiyle korunmalıdır".

- Öneri yalnızca açık yönetim bağlantı noktaları varsa tetiklenecek şekilde ayarlanmıştır.

[JIT erişim özelliği hakkında daha fazla bilgi edinin](security-center-just-in-time.md).


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


### <a name="account-security-recommendations-moved-to-security-best-practices-security-control"></a>Hesap güvenlik önerileri "en iyi güvenlik uygulamaları" güvenlik denetimine taşındı

Gelişmiş güvenli puanla sunulan güvenlik denetimlerinden biri "en iyi güvenlik uygulamaları" ' dır. Bu denetimde bir öneri olduğunda, güvenli puanı etkilemez. 

Bu güncelleştirmeyle, üç öneri özgün olarak yerleştirildiği denetimlerden ve bu en iyi yöntemler denetimine taşınmıştır. Bu üç önerinin riskini başlangıçta düşündükten daha düşük olduğunu belirlediğimiz için bu adımı aldık.

Öneriler şunlardır:

- MFA, aboneliğinizde okuma izinleri olan hesaplarda etkinleştirilmelidir (başlangıçta "MFA 'yı etkinleştir" denetiminde)
- Okuma izinlerine sahip dış hesapların aboneliğinizden kaldırılması gerekir (başlangıçta "erişim ve izinleri yönetme" denetiminde)
- Aboneliğiniz için en fazla 3 sahip belirtilmelidir (başlangıçta "erişim ve izinleri yönetme" denetiminde)

[Azure Güvenlik Merkezi 'Nde gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)bölümünde güvenlik denetimleri hakkında daha fazla bilgi edinin.


### <a name="custom-policies-with-custom-metadata-generally-available"></a>Özel meta verileri olan özel ilkeler genel kullanıma sunuldu

Özel ilkeler artık Güvenlik Merkezi öneri deneyiminin, güvenli puanın ve mevzuat uyumluluk standartları panosunun bir parçasıdır. Bu özellik genel kullanıma sunulmuştur ve kuruluşunuzun güvenlik değerlendirmesi kapsamını güvenlik merkezi 'nde genişletmenizi sağlar. 

Azure ilkesinde özel bir girişim oluşturun, buna ilke ekleyin ve Azure Güvenlik Merkezi 'ne ekleyin ve öneriler olarak görselleştirin.

Artık özel öneri meta verilerini düzenleme seçeneğini de ekledik. Meta veri seçenekleri önem derecesi, düzeltme adımları, tehdit bilgileri ve daha fazlasını içerir.  

[Ayrıntılı bilgilerle özel önerilerinizi geliştirme hakkında daha fazla bilgi edinin](custom-security-policies.md#enhancing-your-custom-recommendations-with-detailed-information).


## <a name="april-2020"></a>Nisan 2020

### <a name="dynamic-compliance-packages-now-generally-available"></a>Dinamik uyumluluk paketleri artık genel kullanıma sunuldu

Azure Güvenlik Merkezi mevzuat uyumluluk panosu artık ek sektör ve yasal standartları izlemek için **dinamik uyumluluk paketleri** (genel kullanıma sunuldu) içerir.

Dinamik uyumluluk paketleri, Güvenlik Merkezi güvenlik ilkesi sayfasından aboneliğinize veya yönetim grubunuza eklenebilir. Bir standart veya kıyaslama eklendi olduğunuzda, değerlendirme olarak eşlenen tüm ilişkili uyumluluk verileri ile yasal uyumluluk panonuzda standart görüntülenir. Eklendi olan standartlardan herhangi biri için bir özet raporu indirilebilir.

Şimdi, şöyle bir standartlar ekleyebilirsiniz:

- **NIST SP 800-53 R4**
- **SWIFT CSP CSCF-V2020**
- **UK resmi ve UK NHS**
- **Kanada Federal PBMM**
- **Azure CIS 1.1.0 (yeni)** (Azure CIS 1.1.0 'ın daha kapsamlı bir gösterimi)

Ayrıca, Azure **güvenlik kıyaslaması**' nı son zamanlarda, genel uyumluluk çerçevelerine göre güvenlik ve uyum en iyi uygulamaları için Microsoft tarafından yazılan Azure 'a özgü yönergeleri ekledik. Panoda kullanılabilir hale geldiğinde ek standartlar desteklenecektir.  
 
[Yasal uyumluluk panonuzda standartlar kümesini özelleştirme hakkında daha fazla bilgi edinin](update-regulatory-compliance-packages.md).


### <a name="identity-recommendations-now-included-in-azure-security-center-free-tier"></a>Kimlik önerileri artık Azure Güvenlik Merkezi Ücretsiz katmanına eklenmiştir

Azure Güvenlik Merkezi Ücretsiz katmanında kimlik ve erişime yönelik güvenlik önerileri artık genel kullanıma sunulmuştur. Bu, bulut güvenlik durure yönetimi (CSPM) özelliklerini ücretsiz hale getirme çabalarının bir parçasıdır. Şu anda bu öneriler yalnızca standart fiyatlandırma katmanında kullanılabilir.

Kimlik ve erişim önerilerine örnek olarak şunlar verilebilir:

- "Çok faktörlü kimlik doğrulaması, aboneliğiniz üzerinde sahip izinleri olan hesaplarda etkinleştirilmelidir."
- "Aboneliğiniz için en fazla üç sahip belirtilmelidir."
- "Kullanımdan kaldırılan hesaplar aboneliğinizden kaldırılmalıdır."

Ücretsiz fiyatlandırma katmanında abonelikleriniz varsa, bu değişiklik, kimlik ve erişim güvenliği için hiçbir şekilde değerlendirilmediğinden, bu değişiklikten etkilenmez.

[Kimlik ve erişim önerileri hakkında daha fazla bilgi edinin](recommendations-reference.md#recs-identity).
[Kimlik ve erişim izleme hakkında daha fazla bilgi edinin](security-center-identity-access.md).


## <a name="march-2020"></a>Mart 2020

### <a name="workflow-automation-is-now-generally-available"></a>İş akışı Otomasyonu artık genel olarak kullanılabilir

Azure Güvenlik Merkezi 'nin iş akışı Otomasyonu özelliği genel kullanıma sunuldu. Güvenlik uyarılarında ve önerilerinde Logic Apps otomatik olarak tetiklemek için bu uygulamayı kullanın. Ayrıca, uyarılar ve hızlı düzelme seçeneği bulunan tüm öneriler için el ile Tetikleyiciler kullanılabilir.

Her güvenlik programı, olay yanıtı için birden çok iş akışı içerir. Bu işlemler ilgili hissedarları bildirmeyi, bir değişiklik yönetimi işlemini başlatmayı ve belirli düzeltme adımlarını uygulamayı içerebilir. Güvenlik uzmanları, yaptığınız gibi bu yordamların pek çok adımını otomatikleştirmenizi önerir. Otomasyon, yükü azaltır ve işlem adımlarının hızlı, tutarlı bir şekilde ve önceden tanımlanmış gereksinimlerinize göre hızla yapılmasını sağlayarak güvenliği geliştirebilir.

İş akışlarınızı çalıştırmaya yönelik otomatik ve el ile güvenlik merkezi özellikleri hakkında daha fazla bilgi için bkz. [iş akışı Otomasyonu](workflow-automation.md).

Logic Apps oluşturma hakkında bilgi edinmek için bkz. [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).


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

[Güvenlik Merkezi Ile Azure Kubernetes Hizmetleri Tümleştirmesi hakkında daha fazla bilgi edinin](azure-kubernetes-service-integration.md).
[Güvenlik Merkezi 'ndeki kapsayıcı güvenliği özellikleri hakkında daha fazla bilgi edinin](container-security.md).


### <a name="improved-just-in-time-experience"></a>Anında iyileştirilmiş deneyim

Azure Güvenlik Merkezi 'nin Yönetim bağlantı noktalarınızı güvenli hale getirmeye yönelik tam zamanında araçlar, işlem ve Kullanıcı arabirimi aşağıdaki şekilde geliştirilmiştir: 

- **Gerekçe alanı** -Azure Portal tam zamanında sayfasında bir sanal MAKINEYE (VM) erişim isteğinde bulunduğunda, istek için bir gerekçe girmek üzere yeni bir isteğe bağlı alan kullanılabilir. Bu alana girilen bilgiler etkinlik günlüğünde izlenebilir. 
- **Yedekli tam zamanında (JIT) kuralları otomatik temizleme** -bir JIT ilkesini her güncelleştirdiğinizde, tüm RuleSet 'nizin geçerliliğini denetlemek için otomatik olarak bir temizleme aracı çalışır. Araç, ilkenizin kuralları ile NSG 'deki kurallar arasında uyuşmazlıkları arar. Temizleme Aracı bir uyumsuzluk bulursa, nedeni belirler ve bunu yapmak güvenli olduğunda, artık gerekmeyen yerleşik kuralları kaldırır. Temizleyici, oluşturduğunuz kuralları hiçbir şekilde silmez. 

[JIT erişim özelliği hakkında daha fazla bilgi edinin](security-center-just-in-time.md).


### <a name="two-security-recommendations-for-web-applications-deprecated"></a>Web uygulamaları için iki güvenlik önerisi kullanım dışı

Web uygulamalarıyla ilgili iki güvenlik önerisi kullanım dışı bırakılmıştır: 

- IaaS NSG 'lerdeki Web uygulamalarına yönelik kurallar sağlamlaştırılmış olmalıdır.
    (İlgili ilke: IaaS üzerinde Web uygulamalarına yönelik NSG kuralları sağlamlaştırılmış olmalıdır)

- Uygulama hizmetlerine erişim kısıtlı olmalıdır.
    (İlgili ilke: uygulama hizmetlerine erişim kısıtlı olmalıdır [Önizleme])

Bu öneriler artık Güvenlik Merkezi öneri listesinde görünmez. İlgili ilkeler artık "Güvenlik Merkezi varsayılanı" adlı girişimde yer alınmaz.

[Güvenlik önerileri hakkında daha fazla bilgi edinin](recommendations-reference.md).

## <a name="february-2020"></a>Şubat 2020

### <a name="fileless-attack-detection-for-linux-is-now-in-preview"></a>Linux için dosya daha az saldırı algılama şimdi önizlemededir

Saldırganlar, algılamayı önlemek için stealthier yöntemleri kullanmayı artırarak, Azure Güvenlik Merkezi, Windows 'un yanı sıra Linux için dosya daha az saldırı algılamasını genişletmez. Dosya sistemi saldırıları Yazılım açıklarına karşı yararlanma, kötü amaçlı yükleri zararsız sistem işlemlerine ekleme ve bellekte gizleme. Bu teknikler:

- disk üzerinde kötü amaçlı yazılım izlemelerini en aza indirin veya kaldırın
- disk tabanlı kötü amaçlı yazılım tarama çözümlerine algılama olasılığını önemli ölçüde azaltma

Azure Güvenlik Merkezi, bu tehdidi ortadan silmek için, 2018 Ekim 'de Windows için dosya daha az saldırı algılaması yayımlamıştır ve artık Linux üzerinde dosya daha az saldırı algılama işlemi de genişletmiştir. 


## <a name="january-2020"></a>Ocak 2020

### <a name="enhanced-secure-score"></a>Gelişmiş güvenli puan

Azure Güvenlik Merkezi 'nin güvenli Puanlama özelliğinin gelişmiş bir sürümü artık önizlemede sunulmaktadır. Bu sürümde, çok sayıda öneri, güvenlik açıklarına karşı savunmasız saldırı yüzeylerinizi daha iyi yansıtarak (örneğin, yönetim bağlantı noktalarına erişimi kısıtla) güvenlik denetimlerinde gruplandırılır.

Önizleme aşamasında güvenli puan değişiklikleri hakkında bilgi edinin ve ortamınızı daha da güvenli hale getirmenize yardımcı olacak diğer düzeltmeleri öğrenin.

[Azure Güvenlik Merkezi 'Nde gelişmiş güvenli Puanlama (Önizleme)](secure-score-security-controls.md)hakkında daha fazla bilgi edinin.