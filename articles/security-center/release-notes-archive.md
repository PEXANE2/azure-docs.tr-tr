---
title: Azure Güvenlik Merkezi 'ndeki yenilikleri Arşivle
description: Azure Güvenlik Merkezi 'nde altı aydan önce ve önceki sürümlerde nelerin yeni ve değiştirilmiş olduğuna ilişkin bir açıklama.
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
ms.openlocfilehash: 2012f590aa833bea1b49955958095917a6479665
ms.sourcegitcommit: bf1340bb706cf31bb002128e272b8322f37d53dd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/03/2020
ms.locfileid: "89438577"
---
# <a name="archive-for-whats-new-in-azure-security-center"></a>Azure Güvenlik Merkezi 'ndeki yenilikler için Arşiv mi var?

[Azure Güvenlik Merkezi 'ndeki yenilikler nelerdir?](release-notes.md) sürüm notları sayfasında son altı aya yönelik güncelleştirmeler bulunur, Bu sayfa eski öğeler içerir.

Bu sayfa hakkında bilgi sağlar:

- Yeni özellikler
- Hata düzeltmeleri
- Kullanım dışı işlevsellik


## <a name="march-2020"></a>Mart 2020

Mart 'taki güncelleştirmeler şunları içerir:

- [İş akışı Otomasyonu artık genel olarak kullanılabilir](#workflow-automation-is-now-generally-available)
- [Azure Güvenlik Merkezi 'Ni Windows Yönetim Merkezi ile tümleştirme](#integration-of-azure-security-center-with-windows-admin-center)
- [Azure Kubernetes hizmeti için koruma](#protection-for-azure-kubernetes-service)
- [Anında iyileştirilmiş deneyim](#improved-just-in-time-experience)
- [Web uygulamaları için iki güvenlik önerisi kullanım dışı](#two-security-recommendations-for-web-applications-deprecated)


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

[Gelişmiş güvenli skor (Önizleme)](secure-score-security-controls.md)hakkında daha fazla bilgi edinin.



## <a name="november-2019"></a>Kasım 2019

Kasım 'daki güncelleştirmeler şunlardır:
 - [Kuzey Amerika bölgelerinde Azure Key Vault için tehdit koruması (Önizleme)](#threat-protection-for-azure-key-vault-in-north-america-regions-preview)
 - [Azure depolama için tehdit koruması, kötü amaçlı yazılım sayını filtreleme içerir](#threat-protection-for-azure-storage-includes-malware-reputation-screening)
 - [Logic Apps iş akışı Otomasyonu (Önizleme)](#workflow-automation-with-logic-apps-preview)
 - [Toplu kaynaklar için hızlı düzelme genel kullanıma sunuldu](#quick-fix-for-bulk-resources-generally-available)
 - [Güvenlik açıkları için kapsayıcı görüntülerini Tara (Önizleme)](#scan-container-images-for-vulnerabilities-preview)
 - [Ek mevzuat uyumluluk standartları (Önizleme)](#additional-regulatory-compliance-standards-preview)
 - [Azure Kubernetes hizmeti (Önizleme) için tehdit koruması](#threat-protection-for-azure-kubernetes-service-preview)
 - [Sanal makine güvenlik açığı değerlendirmesi (Önizleme)](#virtual-machine-vulnerability-assessment-preview)
 - [Azure sanal makineler 'de SQL sunucuları için gelişmiş veri güvenliği (Önizleme)](#advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview)
 - [Özel ilkeler desteği (Önizleme)](#support-for-custom-policies-preview)
 - [Topluluk ve iş ortakları platformu ile Azure Güvenlik Merkezi kapsamını genişletme](#extending-azure-security-center-coverage-with-platform-for-community-and-partners)
 - [Öneriler ve uyarıların dışa aktarılması ile gelişmiş tümleştirmeler (Önizleme)](#advanced-integrations-with-export-of-recommendations-and-alerts-preview)
 - [Windows Yönetim Merkezi 'nden Güvenlik Merkezi 'ne şirket içi sunucular ekleme (Önizleme)](#onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview)

### <a name="threat-protection-for-azure-key-vault-in-north-america-regions-preview"></a>Kuzey Amerika bölgelerinde Azure Key Vault için tehdit koruması (Önizleme)

Azure Key Vault, bulutta anahtarlar, gizlilikler, şifreleme anahtarları ve ilkeleri merkezi olarak yönetme olanağı sunarak verileri korumak ve bulut uygulamalarının performansını geliştirmek için gerekli olan bir hizmettir. Azure Key Vault hassas ve iş açısından kritik verileri depoladığından, Anahtar kasaları ve bunlarda depolanan veriler için en fazla güvenlik gerektirir.

Azure Güvenlik Merkezi 'nin Azure Key Vault için tehdit koruması desteği, önemli kasaların ne kadar büyük ve olası olabilecek zararlı girişimleri algılayan ek bir güvenlik zekası katmanı sağlar. Bu yeni koruma katmanı, müşterilerin güvenlik uzmanı olmadan veya güvenlik izleme sistemlerini yönetmeksizin önemli kasalara karşı tehditleri ele almasına olanak tanır. Bu özellik Kuzey Amerika bölgelerinde genel önizlemede bulunur.


### <a name="threat-protection-for-azure-storage-includes-malware-reputation-screening"></a>Azure depolama için tehdit koruması, kötü amaçlı yazılım sayını filtreleme içerir

Azure depolama için tehdit koruması, Microsoft Threat Intelligence tarafından desteklenen yeni algılamalar sunarak, karma saygınlığı analizini ve bir etkin Tor çıkış düğümünden şüpheli erişimi (bir anonim yönetim proxy) kullanarak Azure depolama 'ya kötü amaçlı yazılım yüklemeleri tespit edin. Artık, Azure Güvenlik Merkezi 'ni kullanarak depolama hesaplarında algılanan kötü amaçlı yazılımları görüntüleyebilirsiniz.


### <a name="workflow-automation-with-logic-apps-preview"></a>Logic Apps iş akışı Otomasyonu (Önizleme)

Merkezi olarak yönetilen güvenlik ve BT/işlem kuruluşları, ortamlarında tutarsızlıklar bulunduğunda kuruluştaki gerekli eylemi gerçekleştirmek için iç iş akışı süreçlerini uygular. Çoğu durumda, bu iş akışları tekrarlanabilir süreçler ve otomasyon, kuruluştaki süreçler büyük ölçüde kolaylaştırabilir.

Günümüzde, güvenlik merkezi 'nde müşterilerin Azure Logic Apps kullanarak Otomasyon yapılandırması oluşturmalarına ve öneriler veya uyarılar gibi belirli ASC 'leri temel alarak otomatik olarak tetiklenecek ilkeler oluşturmalarına olanak tanıyan yeni bir özellik sunuyoruz. Azure Logic App, çok sayıda Logic App Bağlayıcısı tarafından desteklenen özel bir eylem yapmak veya bir e-posta gönderme ya da ServiceNow™ bileti açma gibi güvenlik merkezi tarafından sunulan şablonlardan birini kullanmak üzere yapılandırılabilir.

İş akışlarınızı çalıştırmaya yönelik otomatik ve el ile güvenlik merkezi özellikleri hakkında daha fazla bilgi için bkz. [iş akışı Otomasyonu](workflow-automation.md).

Logic Apps oluşturma hakkında bilgi edinmek için bkz. [Azure Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-overview).


### <a name="quick-fix-for-bulk-resources-generally-available"></a>Toplu kaynaklar için hızlı düzelme genel kullanıma sunuldu

Bir kullanıcının güvenli puanın bir parçası olarak verdiği çok sayıda görev olduğunda, büyük bir filo genelinde sorunları etkili bir şekilde düzeltme yeteneği zor olabilir.

Güvenlik yapılandırmalarını düzeltmeyi basitleştirmek ve kaynakların toplu olarak önerilerini hızlıca düzeltmek ve güvenli puanınızı iyileştirmek için hızlı düzeltme düzeltmesini kullanın.

Bu işlem, düzeltmeyi uygulamak istediğiniz kaynakları seçmenizi ve sizin adınıza yapılandırmayı sağlayacak bir düzeltme eylemi başlatmayı sağlayacaktır.

Hızlı düzeltme, günümüzde müşteriler güvenlik merkezi öneriler sayfasının bir parçası olarak sunulmaktadır.

[Güvenlik önerilerine yönelik başvuru kılavuzunda](recommendations-reference.md), hangi önerilerin hızlı düzeltme etkin olduğunu görün.


### <a name="scan-container-images-for-vulnerabilities-preview"></a>Güvenlik açıkları için kapsayıcı görüntülerini Tara (Önizleme)

Azure Güvenlik Merkezi, artık güvenlik açıkları için Azure Container Registry kapsayıcı görüntülerini tarayacaktır.

Görüntü taraması, kapsayıcı görüntü dosyasını ayrıştırarak ve ardından bilinen güvenlik açıkları olup olmadığını (Qualys tarafından desteklenir) kontrol ederek çalışır.

Azure Container Registry yeni kapsayıcı görüntüleri gönderilirken tarama kendisi otomatik olarak tetiklenir. Bulunan güvenlik açıkları güvenlik merkezi önerileri olarak yüzdedir ve izin verilen saldırı yüzeyini azaltmak için bunları düzeltme eki uygulama hakkında bilgi ile birlikte Azure güvenli puanına dahil edilir.


### <a name="additional-regulatory-compliance-standards-preview"></a>Ek mevzuat uyumluluk standartları (Önizleme)

Mevzuat uyumluluk panosu, güvenlik merkezi değerlendirmelerine bağlı olarak uyumluluk gönderinize yönelik öngörüler sağlar. Panoda, ortamınız belirli mevzuata standartları ve sektör kıyaslamaları tarafından belirlenen denetimler ve gereksinimler ile nasıl uyumlu olduğunu ve bu gereksinimlerin nasıl ele alınacağını gösteren öngörülere ilişkin öneriler sağlar.

Mevzuat uyumluluk panosu, bu nedenle dört yerleşik standart standardı destekler: Azure CIS 1.1.0, PCI-DSS, ISO 27001 ve SOC-TSP. Artık desteklenen ek standartların genel önizleme sürümünü duyuruyoruz: NıST SP 800-53 R4, SWIFT CSP CSCF V2020, Kanada Federal PBMM ve UK, UK NHS ile Birleşik Krallık resmi. Ayrıca, standart ve genişletilebilirlik geliştirmeyle daha fazla denetim kapsayan Azure CIS 1.1.0 'ın güncelleştirilmiş bir sürümünü yayınlarız.

[Yasal uyumluluk panonuzda standartlar kümesini özelleştirme hakkında daha fazla bilgi edinin](update-regulatory-compliance-packages.md).


### <a name="threat-protection-for-azure-kubernetes-service-preview"></a>Azure Kubernetes hizmeti (Önizleme) için tehdit koruması

Kubernetes, bulutta yazılım dağıtmak ve yönetmek için hızlı bir şekilde yeni standart haline geliyor. Birkaç kişinin Kubernetes ile kapsamlı bir deneyimi vardır ve çoğu yalnızca genel mühendislik ve yönetime odaklanır ve güvenlik üst kısmına göz atın. Kubernetes ortamının güvenli olması için dikkatli bir şekilde yapılandırılması gerekir. Bu, hiçbir kapsayıcının odaklanmış saldırı yüzeyi yüzeyinin açık olmadığından emin olmak saldırganlar için açıktır. Güvenlik Merkezi, Azure-Azure Kubernetes Service (AKS) ' deki en hızlı büyüyen hizmetlerden birine kapsayıcı alanında desteğini genişletiyor.

Bu genel önizleme sürümündeki yeni yetenekler şunlardır:

- **Keşif & görünürlüğü** -Güvenlik Merkezi 'nin kayıtlı aboneliklerinde YÖNETILEN aks örneklerinin sürekli keşfi.
- **Güvenli puan önerileri** -müşterilerin, bir Kubernetes hizmet kümesine erişimi kısıtlamak Için "rol tabanlı Access Control kullanılması gerekir" gibi, müşterinin güvenli puanının bir parçası olarak aks 'deki en iyi güvenlik uygulamalarına uyum sağlamak için işlem yapılabilir öğeler.
- **Tehdit algılama** -"ayrıcalıklı bir kapsayıcı algılandı" gibi konak ve küme tabanlı analiz.


### <a name="virtual-machine-vulnerability-assessment-preview"></a>Sanal makine güvenlik açığı değerlendirmesi (Önizleme)

Sanal makinelere yüklenen uygulamalar, genellikle sanal makine ihlaline neden olabilecek güvenlik açıklarına sahip olabilir. Güvenlik Merkezi Standart katmanının, hiçbir ek ücret ödemeden sanal makineler için yerleşik güvenlik açığı değerlendirmesi içerdiğini duyuruyoruz. Genel önizlemede Qualys tarafından desteklenen güvenlik açığı değerlendirmesi, güvenlik açığı bulunan uygulamaları bulmak ve Güvenlik Merkezi portalının deneyimindeki bulguları sunmak için bir sanal makinede yüklü olan tüm uygulamaları sürekli olarak taramanıza olanak sağlayacak. Güvenlik Merkezi, kullanıcıdan hiçbir ek iş gerekmemesi için tüm dağıtım işlemlerini gerçekleştirir. İleri ' ye giderek, müşterilerinizin benzersiz iş ihtiyaçlarını desteklemeye yönelik güvenlik açığı değerlendirmesi seçenekleri sağlamayı planlıyoruz.

[Azure sanal makineleriniz için güvenlik açığı değerlendirmeleri hakkında daha fazla bilgi edinin](deploy-vulnerability-assessment-vm.md).


### <a name="advanced-data-security-for-sql-servers-on-azure-virtual-machines-preview"></a>Azure sanal makineler 'de SQL sunucuları için gelişmiş veri güvenliği (Önizleme)

Azure Güvenlik Merkezi 'nin tehdit koruması ve IaaS VM 'lerinde çalıştırılan SQL DB değerlendirmesi için destek artık önizleme aşamasındadır.

[Güvenlik açığı değerlendirmesi](https://docs.microsoft.com/azure/sql-database/sql-vulnerability-assessment) olası veritabanı güvenlik açıklarını keşfetmenizi ve izlemenizi sağlamanın yanı sıra bunları gidermeye yardımcı olan yapılandırması kolay bir hizmettir. Azure güvenli puanınızın bir parçası olarak güvenlik duruşunuzu görünürlük sağlar ve güvenlik sorunlarını giderme ve veritabanınızın geliştirilmesine yönelik adımları içerir.

[Gelişmiş tehdit koruması](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-overview) , SQL Server 'a erişmeye veya yararlanmaya yönelik olağan dışı ve zararlı olabilecek girişimleri gösteren anormal etkinlikleri algılar. Veritabanınızı sürekli şüpheli etkinlikler için izler ve anormal veritabanı erişim desenlerinde eyleme dayalı güvenlik uyarıları sağlar. Bu uyarılar, tehdidi araştırmak ve azaltmak için şüpheli etkinlik ayrıntılarını ve önerilen eylemleri sağlar.


### <a name="support-for-custom-policies-preview"></a>Özel ilkeler desteği (Önizleme)

Azure Güvenlik Merkezi artık özel ilkeleri destekliyor (önizlemede).

Müşterilerimiz, güvenlik merkezi 'ndeki geçerli güvenlik değerlendirmesi kapsamını, Azure Ilkesinde oluşturdukları ilkelere bağlı olarak kendi güvenlik değerlendirmelerine göre genişletecek. Özel ilkeler desteğiyle, bu artık mümkündür.

Bu yeni ilkeler, güvenlik merkezi öneri deneyiminin, güvenli puanın ve mevzuat uyumluluk standartları panosunun bir parçası olacaktır. Özel ilkeler desteğiyle, artık Azure Ilkesinde özel bir girişim oluşturabilir, ardından Güvenlik Merkezi 'nde ilke olarak ekleyebilir ve bunu bir öneri olarak görselleştirebilirsiniz.


### <a name="extending-azure-security-center-coverage-with-platform-for-community-and-partners"></a>Topluluk ve iş ortakları platformu ile Azure Güvenlik Merkezi kapsamını genişletme

Yalnızca Microsoft 'tan değil, daha fazla tümleştirmelere sahip olan Check Point, Tenable ve Siark gibi iş ortaklarından gelen önerileri almak için Güvenlik Merkezi 'ni kullanın.  Güvenlik Merkezi 'nin basit ekleme akışı, mevcut çözümlerinizi Güvenlik Merkezi 'ne bağlanarak güvenlik sonrası önerilerinizi tek bir yerde görüntüleyebilir, Birleşik raporlar çalıştırabilir ve hem yerleşik hem de iş ortağı önerilerine karşı tüm güvenlik merkezi yeteneklerini kullanabilir. Ayrıca, güvenlik merkezi önerilerini iş ortağı ürünlerine de dışarı aktarabilirsiniz.

[Microsoft Akıllı güvenlik ilişkilendirmesi hakkında daha fazla bilgi edinin](https://www.microsoft.com/security/partnerships/intelligent-security-association).



### <a name="advanced-integrations-with-export-of-recommendations-and-alerts-preview"></a>Öneriler ve uyarıların dışa aktarılması ile gelişmiş tümleştirmeler (Önizleme)

Kurumsal düzey senaryoları Güvenlik Merkezi 'nin üzerinde etkinleştirmek için artık Güvenlik Merkezi uyarılarını ve Azure portal veya API hariç ek yerlerdeki önerileri kullanmak mümkündür. Bunlar, doğrudan bir olay hub 'ına ve Log Analytics çalışma alanlarına aktarılabilir. Bu yeni yetenekler çerçevesinde oluşturabileceğiniz birkaç iş akışı aşağıda verilmiştir:

- Log Analytics çalışma alanına aktar ile Power BI özel panolar oluşturabilirsiniz.
- Olay Hub 'ına dışa aktarma sayesinde, üçüncü taraf Sıems 'nize yönelik güvenlik merkezi uyarılarını ve önerilerini, gerçek zamanlı bir üçüncü taraf çözümüne veya Azure Veri Gezgini dışarı aktarabileceksiniz.


### <a name="onboard-on-prem-servers-to-security-center-from-windows-admin-center-preview"></a>Windows Yönetim Merkezi 'nden Güvenlik Merkezi 'ne şirket içi sunucular ekleme (Önizleme)

Windows Yönetim Merkezi, Azure 'da dağıtılmayan Windows sunucuları için, yedekleme ve sistem güncelleştirmeleri gibi çeşitli Azure yönetim özellikleri sunan bir yönetim portalıdır. Son zamanlarda, bu Azure dışı sunucuları, Windows Yönetim Merkezi deneyiminden ASC tarafından doğrudan korunabilecek şekilde ekleme olanağı ekledik.

Bu yeni deneyimle, kullanıcılar Azure Güvenlik Merkezi 'ne bir WAC sunucusu eklemek ve güvenlik uyarılarını ve önerilerini doğrudan Windows Yönetim Merkezi deneyiminde görüntülemeyi etkinleştirmek olacaktır.


## <a name="september-2019"></a>Eylül 2019

Eylül ayında güncelleştirmeler şunları içerir:

 - [Uyarlamalı uygulama denetimleri ile kuralları yönetme iyileştirmeleri](#managing-rules-with-adaptive-application-controls-improvements)
 - [Azure Ilkesi kullanarak kapsayıcı güvenliği önerisi denetimi](#control-container-security-recommendation-using-azure-policy)

### <a name="managing-rules-with-adaptive-application-controls-improvements"></a>Uyarlamalı uygulama denetimleri ile kuralları yönetme iyileştirmeleri

Uyarlamalı uygulama denetimleri kullanan sanal makineler için kuralları yönetme deneyimi geliştirilmiştir. Azure Güvenlik Merkezi 'nin Uyarlamalı uygulama denetimleri, sanal makinelerinizde hangi uygulamaların çalıştırılacağını denetlemenize yardımcı olur. Kural yönetimine genel bir iyileştirmenin yanı sıra yeni bir avantaj de yeni bir kural eklediğinizde hangi dosya türlerinin korunacağını denetlemenize olanak sağlar.

[Uyarlamalı uygulama denetimleri hakkında daha fazla bilgi edinin](security-center-adaptive-application.md).


### <a name="control-container-security-recommendation-using-azure-policy"></a>Azure Ilkesi kullanarak kapsayıcı güvenliği önerisi denetimi

Azure Güvenlik Merkezi 'nin kapsayıcı güvenliğine yönelik güvenlik açıklarını düzeltme önerisi artık Azure Ilkesi aracılığıyla etkinleştirilebilir veya devre dışı bırakılabilir.

Etkin güvenlik ilkelerinizi görüntülemek için Güvenlik Merkezi 'nden Güvenlik Ilkesi sayfasını açın.


## <a name="august-2019"></a>Ağustos 2019

Ağustos ayında güncelleştirmeler şunlardır:

 - [Azure Güvenlik Duvarı için tam zamanında (JıT) VM erişimi](#just-in-time-jit-vm-access-for-azure-firewall)
 - [Güvenlik duruşunuzu (Önizleme) artırmak için tek tıklama düzeltmesi](#single-click-remediation-to-boost-your-security-posture-preview)
 - [Kiracılar arası yönetim](#cross-tenant-management)

### <a name="just-in-time-jit-vm-access-for-azure-firewall"></a>Azure Güvenlik Duvarı için tam zamanında (JıT) VM erişimi 

Azure Güvenlik Duvarı için tam zamanında (JıT) VM erişimi artık genel kullanıma sunuldu. NSG korumalı ortamlarınıza ek olarak Azure Güvenlik Duvarı korumalı ortamlarınızın güvenliğini sağlamak için bunu kullanın.

JıT VM erişimi, VM 'lere yalnızca gerektiğinde, NSG ve Azure Güvenlik Duvarı kurallarınızı kullanarak denetimli erişim sağlayarak ağ Volumetric saldırılarına maruz kalmayı azaltır.

VM 'niz için JıT 'i etkinleştirdiğinizde, korunacak bağlantı noktalarını, bağlantı noktalarının açık kalacağı süreyi ve bu bağlantı noktalarından erişilebilen onaylanan IP adreslerini belirleyen bir ilke oluşturursunuz. Bu ilke, kullanıcıların erişim istediklerinde neler yapabileceğini denetlemenize yardımcı olur.

İstekleri Azure etkinlik günlüğünde günlüğe kaydedilir, böylelikle erişimi kolayca izleyebilir ve denetleyebilirsiniz. Tam zamanında sayfası, JIT etkin olan VM 'Leri ve JıT 'in önerildiği VM 'Leri hızlı bir şekilde belirlemenize de yardımcı olur.

[Azure Güvenlik Duvarı hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/firewall/overview).


### <a name="single-click-remediation-to-boost-your-security-posture-preview"></a>Güvenlik duruşunuzu (Önizleme) artırmak için tek tıklama düzeltmesi

Güvenli puan, iş yükü güvenlik duruşunuzu değerlendirmenize yardımcı olan bir araçtır. Güvenlik önerilerinizi gözden geçirir ve bunları sizin için önceliklendirir, bu sayede öncelikle hangi önerilerin gerçekleştirileceğini öğrenin. Bu, araştırmaya öncelik vermek için en ciddi güvenlik açıklarını bulmanıza yardımcı olur.

Güvenlik Yapılandırması yapılandırmalarını basitleştirecek ve güvenli puanınızı hızlı bir şekilde iyileştirmenize yardımcı olmak için, tek bir tıklama içindeki kaynakların toplu olarak bir önerisini düzeltmenizi sağlayan yeni bir özellik ekledik.

Bu işlem, düzeltmeyi uygulamak istediğiniz kaynakları seçmenizi ve sizin adınıza yapılandırmayı sağlayacak bir düzeltme eylemi başlatmayı sağlayacaktır.

[Güvenlik önerilerine yönelik başvuru kılavuzunda](recommendations-reference.md), hangi önerilerin hızlı düzeltme etkin olduğunu görün.


### <a name="cross-tenant-management"></a>Kiracılar arası yönetim

Güvenlik Merkezi artık Azure açık Thouse 'ın bir parçası olarak platformlar arası yönetim senaryolarını desteklemektedir. Bu, güvenlik merkezi 'nde birden çok kiracının güvenlik duruşunu görünürlüğe ve yönetmenize olanak sağlar. 

[Çapraz kiracı yönetim deneyimleri hakkında daha fazla bilgi edinin](security-center-cross-tenant-management.md).


## <a name="july-2019"></a>Temmuz 2019

### <a name="updates-to-network-recommendations"></a>Ağ önerilerini güncelleştirme

Azure Güvenlik Merkezi (ASC) yeni ağ önerilerini başlattı ve bazı mevcut olanları iyileştirmiştir. Artık, güvenlik merkezi 'nin kullanılması kaynaklarınız için daha fazla ağ koruması sağlar. 

[Ağ önerileri hakkında daha fazla bilgi edinin](recommendations-reference.md#recs-network).


## <a name="june-2019"></a>Haziran 2019

### <a name="adaptive-network-hardening---generally-available"></a>Uyarlamalı ağ sağlamlaştırma-genel kullanıma sunuldu

Genel bulutta çalışan iş yükleri için en büyük saldırı yüzeylerinden biri, genel Internet 'e ve bu bilgisayardan bağlantı sağlar. Müşterilerimiz, Azure iş yüklerinin yalnızca gerekli kaynak aralıklarında kullanılabilir olduğundan emin olmak için hangi ağ güvenlik grubu (NSG) kurallarının yerinde olduğunu öğrenmelidir. Bu özellik ile, Güvenlik Merkezi, Azure iş yüklerinin ağ trafiğini ve bağlantı düzenlerini öğrenir ve Internet 'e yönelik sanal makineler için NSG kural önerileri sağlar. Bu, müşterimizin ağ erişim ilkelerini daha iyi yapılandırmasına ve saldırılardan etkilenme olasılığını sınırlamaya yardımcı olur. 

[Uyarlamalı ağ sağlamlaştırma hakkında daha fazla bilgi edinin](security-center-adaptive-network-hardening.md).
