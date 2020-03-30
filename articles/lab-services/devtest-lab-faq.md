---
title: Azure Devtest Labs SSS | Microsoft Dokümanlar
description: Bu makalede, Azure DevTest Labs hakkında sık sorulan bazı soruların (SSS) yanıtları verilmektedir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2020
ms.author: spelluru
ms.openlocfilehash: de99e9b1e4adceaf08beaf8ad3b5ea114b31a586
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79270790"
---
# <a name="azure-devtest-labs-faq"></a>Azure DevTest Labs SSS
Azure DevTest Labs hakkında en sık sorulan sorulardan bazılarına yanıt alın.

## <a name="blog-post"></a>Blog yazısı
DevTest Labs Team blogumuz 20 Mart 2019 itibariyle kullanımdan kaldırılmıştir. 

### <a name="where-can-i-track-feature-updates-from-now-on"></a>Bundan sonra özellik güncelleştirmelerini nereden izleyebilirim?
Şu andan itibaren Azure blogunda ve Azure güncellemelerinde özellik güncellemeleri ve bilgilendirici blog gönderileri yayınlayacağız. Bu blog gönderileri de gerekli olan her yerde belgelerimize bağlantı verecektir.

DevTest [Labs'daki](https://azure.microsoft.com/blog/tag/azure-devtest-labs/) yeni özelliklerden haberdar olmak için DevTest Labs Azure Blog'a ve [DevTest Labs Azure güncellemelerine](https://azure.microsoft.com/updates/?product=devtest-lab) abone olun.

### <a name="what-happens-to-the-existing-blog-posts"></a>Varolan blog gönderilerine ne olur?
Şu anda [DevTest Labs belgelerimize](devtest-lab-overview.md)mevcut blog gönderilerini (kesinti güncellemeleri hariç) geçirme üzerinde çalışıyoruz. MSDN blogu amortismana girdiğinde, DevTest Labs için belgelere genel bakışa yönlendirilir. Yeniden yönlendirildikten sonra, 'Filtrenize göre' başlığında aradığınız makaleyi arayabilirsiniz. Henüz tüm mesajları göç değil, ama bu ayın sonuna kadar yapılmalıdır. 


### <a name="where-do-i-see-outage-updates"></a>Kesinti güncellemelerini nerede görüyorum?
Şu andan itibaren Twitter kulpumuzu kullanarak kesinti güncellemeleri yayınlayacağız. Kesintiler ve bilinen hatalar hakkında en son güncellemeleri almak için Bizi Twitter'da takip edin.

### <a name="twitter"></a>Twitter
Bizim Twitter kolu:[@azlabservices](https://twitter.com/azlabservices)

## <a name="general"></a>Genel
### <a name="what-if-my-question-isnt-answered-here"></a>Ya sorum burada cevaplandırılmazsa?
Sorunuz burada listelenmemişse, bir yanıt bulmanıza yardımcı olalım.

- Bu SSS'nin sonunda bir soru gönderin.
- Daha geniş bir kitleye ulaşmak [için, Azure DevTest Labs MSDN forumunda](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs)bir soru yayınlayın. Azure DevTest Labs ekibi ve topluluğun diğer üyeleriyle iletişim kur.
- Özellik istekleri için isteklerinizi ve fikirlerinizi [Azure DevTest Labs Kullanıcı Sesi'ne](https://feedback.azure.com/forums/320373-azure-devtest-labs)gönderin.

### <a name="what-is-a-microsoft-account"></a>Microsoft hesabı nedir?
Microsoft hesabı, Microsoft aygıtları ve hizmetleriyle yaptığınız hemen hemen her şey için kullandığınız bir hesaptır. Skype, Outlook.com, OneDrive, Windows phone, Azure ve Xbox Live'da oturum açabilmek için kullandığınız bir e-posta adresi ve paroladır. Tek bir hesap, dosyalarınızın, kişilerinizin, kişilerinizin ve ayarlarınızın sizi herhangi bir cihazda takip edebileceği anlamına gelir.

> [!NOTE]
> Eskiden Windows Live Kimliği olarak adlandırılan bir Microsoft hesabı.

### <a name="why-should-i-use-azure-devtest-labs"></a>Azure DevTest Labs'ı neden kullanmalıyım?
Azure DevTest Labs ekibinize zaman ve para dan tasarruf edebilir. Geliştiriciler birkaç farklı temel kullanarak kendi ortamlarını oluşturabilir. Ayrıca, uygulamaları hızla dağıtmak ve yapılandırmak için yapıları da kullanabilirler. Özel görüntüler ve formüller kullanarak sanal makineleri (VM' ler) şablon olarak kaydedebilir ve bunları ekip genelinde kolayca yeniden oluşturabilirsiniz. DevTest Labs ayrıca, laboratuvar yöneticilerinin atıkları azaltmak ve ekibin ortamlarını yönetmek için kullanabilecekleri birkaç yapılandırılabilir ilke de sunar. Bu ilkeler otomatik kapatma, maliyet eşiği, kullanıcı başına maksimum VM ve maksimum VM boyutunu içerir. DevTest Labs daha derinlemesine bir açıklama için, [genel bakış](devtest-lab-overview.md) veya [tanıtım video](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs)bakın.

### <a name="what-does-worry-free-self-service-mean"></a>"Kaygısız self servis" ne demek?
Sorunsuz self servis, geliştiricilerin ve sınayıcıların gerektiğinde kendi ortamlarını oluşturmaları anlamına gelir. Yöneticiler, DevTest Labs'ın uygun erişimi ayarlamaya, atıkları en aza indirmeye ve maliyetleri kontrol etmesine yardımcı olabileceğini bilme güvenliğine sahiptir. Yöneticiler hangi VM boyutlarına, maksimum VM sayısını ve VM'lerin başlatılıp kapatıldığında belirtebilirler. DevTest Labs, laboratuvar kaynaklarının nasıl kullanıldığından haberdar olmanıza yardımcı olmak için maliyetleri izlemeyi ve uyarıları ayarlamayı da kolaylaştırır.

### <a name="how-can-i-use-devtest-labs"></a>DevTest Labs'ı nasıl kullanabilirim?
DevTest Labs, dev veya test ortamlarına ihtiyaç duyduğunuz ve bunları hızlı bir şekilde çoğaltmak veya maliyet tasarrufu ilkeleri kullanarak yönetmek istediğiniz her zaman kullanışlıdır.
Müşterilerimizin DevTest Labs'ı şu lar için kullandıkları bazı senaryolar şunlardır:

- Dev ve test ortamlarını tek bir yerde yönetin. Maliyetleri azaltmak ve yapılarını ekip genelinde paylaşmak için özel görüntüler oluşturmak için ilkeleri kullanın.
- Geliştirme aşamaları boyunca disk durumunu kaydetmek için özel görüntüler kullanarak bir uygulama geliştirin.
- İlerlemeyle ilgili maliyeti izleyin.
- Kalite güvence testi için toplu test ortamları oluşturun.
- Bir uygulamayı çeşitli ortamlarda kolayca yapılandırmak ve çoğaltmak için yapıları ve formülleri kullanın.
- Hackathons (ortak dev veya test çalışması) için VM dağıtın ve olay sona erdiğinde bunları kolayca deprovision.

### <a name="how-am-i-billed-for-devtest-labs"></a>DevTest Labs için nasıl faturalandırılırım?
DevTest Labs ücretsiz bir hizmettir. DevTest Labs'da laboratuvarlar oluşturmak ve ilkeleri, şablonları ve yapıları yapılandırmak ücretsizdir. Yalnızca Laboratuvarlarınızda kullanılan Sanal Kaynaklar, depolama hesapları ve sanal ağlar gibi Azure kaynakları için ödeme yaparsınız. Laboratuvar kaynaklarının maliyeti hakkında daha fazla bilgi için [Azure DevTest Labs fiyatlandırması](https://azure.microsoft.com/pricing/details/devtest-lab/)bölümüne bakın.

## <a name="security"></a>Güvenlik

### <a name="what-are-the-different-security-levels-in-devtest-labs"></a>DevTest Labs'daki farklı güvenlik düzeyleri nelerdir?
Güvenlik erişimi, Role Tabanlı Erişim Denetimi (RBAC) tarafından belirlenir. Erişimin nasıl çalıştığını öğrenmek için, RBAC tarafından tanımlandığı şekilde izin, rol ve kapsam arasındaki farkları öğrenmeye yardımcı olur.

- **İzin**: İzin, belirli bir eyleme tanımlanmış bir erişimdir. Örneğin, tüm VM'lere erişim izni okunabilir.
- **Rol**: Rol, bir kullanıcıya gruplandırılabilen ve atanabilen izinler kümesidir. Örneğin, abonelik sahibi rolü olan bir kullanıcı, abonelik içindeki tüm kaynaklara erişebilir.
- Kapsam : **Kapsam,** Azure kaynağının hiyerarşisi içindeki bir düzeydir. Örneğin, kapsam bir kaynak grubu, tek bir laboratuvar veya tüm abonelik olabilir.

DevTest Labs kapsamında, kullanıcı izinlerini tanımlayan iki tür rol vardır:

- **Laboratuvar sahibi**: Laboratuvar sahibi, laboratuardaki tüm kaynaklara erişebilir. Laboratuvar sahibi ilkeleri değiştirebilir, herhangi bir SANAL M'lere okuma ve yazma, sanal ağı değiştirebilir ve böylece devam edebilir.
- **Laboratuvar kullanıcısı**: Laboratuvar kullanıcısı, VM'ler, ilkeler ve sanal ağlar gibi tüm laboratuvar kaynaklarını görüntüleyebilir. Ancak, bir laboratuvar kullanıcısı diğer kullanıcılar tarafından oluşturulan ilkeleri veya VM'leri değiştiremez.

DevTest Labs'da özel roller de oluşturabilirsiniz. DevTest Labs'da özel roller oluşturmayı öğrenmek [için, belirli laboratuvar ilkelerine kullanıcı izinleri verme](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)konusuna bakın.

Kapsamlar hiyerarşik olduğundan, bir kullanıcının belirli bir kapsamda izinleri olduğunda, kullanıcıya bu izinler kapsamdaki her alt düzey kapsamda otomatik olarak verilir. Örneğin, bir kullanıcıya abonelik sahibi rolü atanmışsa, kullanıcı bir abonelikteki tüm kaynaklara erişebilir. Bu kaynaklar VM'leri, sanal ağları ve laboratuarları içerir. Abonelik sahibi, laboratuvar sahibi rolünü otomatik olarak devralır. Ancak, tam tersi doğru değildir. Laboratuvar sahibi, abonelik düzeyinden daha düşük bir kapsam olan bir laboratuvara erişebilir. Yani, bir laboratuvar sahibi VM'leri, sanal ağları veya laboratuvar dışındaki diğer kaynakları göremez.

### <a name="how-do-i-define-role-based-access-control-for-my-devtest-labs-environments-to-ensure-that-it-can-govern-while-developerstest-can-do-their-work"></a>Geliştiriciler/test işlerini yaparken BT'nin yönetebilmesini sağlamak için DevTest Labs ortamlarım için rol tabanlı erişim denetimini nasıl tanımlayabilirim?
Geniş bir desen vardır, ancak ayrıntı kuruluşunuza bağlıdır.

Merkezi BT yalnızca gerekli olana sahip olmalı ve proje ve uygulama ekiplerinin gerekli denetim düzeyine sahip olmasını sağlamalıdır. Genellikle, merkezi BT aboneliğin sahibi ve ağ yapılandırmaları gibi temel BT işlevlerini işlediği anlamına gelir. Abonelik **sahipleri** kümesi küçük olmalıdır. Bu sahipler, ihtiyaç olduğunda ek sahipler atayabilir veya abonelik düzeyi ilkeleri uygulayabilir, örneğin "Genel IP Yok".

Bir abonelik genelinde erişim gerektiren tier1 veya Tier 2 desteği gibi bir kullanıcı alt kümesi olabilir. Bu durumda, kaynakları yönetebilmeleri için bu kullanıcılara **katkıda bulunanlara** erişim sağlamanızı, ancak kullanıcı erişimi sağlamamanızı veya ilkeleri ayarlamamanızı öneririz.

DevTest Labs kaynağı, proje/uygulama ekibine yakın olan sahiplere ait olmalıdır. Bunun nedeni, makineler için kendi gereksinimlerini anlamak ve yazılım gerekli olmasıdır. Çoğu kuruluşta, bu DevTest Labs kaynağının sahibi genellikle proje/geliştirme lideridir. Bu sahip, laboratuvar ortamındaki kullanıcıları ve ilkeleri yönetebilir ve DevTest Labs ortamındaki tüm VM'leri yönetebilir.

Proje/uygulama ekibi üyeleri **DevTest Labs Kullanıcıları** rolüne eklenmelidir. Bu kullanıcılar sanal makineler oluşturabilir (laboratuvar ve abonelik düzeyindeki ilkelerle uyumlu). Ayrıca kendi sanal makinelerini de yönetebilirler. Diğer kullanıcılara ait sanal makineleri yönetemezler.

Daha fazla bilgi için Azure [kurumsal iskele – önceden yazılmış abonelik yönetim belgelerine](/azure/architecture/cloud-adoption/appendix/azure-scaffold)bakın.


### <a name="how-do-i-create-a-role-to-allow-users-to-do-a-specific-task"></a>Kullanıcıların belirli bir görevi yapmasına izin verecek bir rolü nasıl oluştururum?
Özel rollerin nasıl oluşturulup bir role izin atayılabilenlerle ilgili kapsamlı bir makale [için](devtest-lab-grant-user-permissions-to-specific-lab-policies.md)bkz. Burada, laboratuvardaki tüm VM'leri başlatma ve durdurma iznine sahip **DevTest Labs Advanced User**rolünü oluşturan bir komut dosyası örneği verilmiştir:


```powershell
$policyRoleDef = Get-AzRoleDefinition "DevTest Labs User"
$policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
$policyRoleDef.Id = $null
$policyRoleDef.Name = "DevTest Labs Advanced User"
$policyRoleDef.IsCustom = $true
$policyRoleDef.AssignableScopes.Clear()
$policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
$policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
$policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
$policyRoleDef = New-AzRoleDefinition -Role $policyRoleDef  
```

### <a name="how-can-an-organization-ensure-corporate-security-policies-are-in-place"></a>Bir kuruluş kurumsal güvenlik ilkelerinin yerinde olmasını nasıl sağlayabilir?
Bir kuruluş bunu aşağıdaki eylemleri yaparak elde edebilir:

- Kapsamlı bir güvenlik politikası geliştirmek ve yayınlamak. İlke, yazılım, bulut varlıkları nın kullanımıyla ilişkili kabul edilebilir kullanım kurallarını ifade eder. Ayrıca, politikayı açıkça ihlal eden şeyi tanımlar.
- Etkin dizinle tanımlanan güvenlik alanı içinde düzenlemeye olanak tanıyan özel bir görüntü, özel yapılar ve dağıtım işlemi geliştirin. Bu yaklaşım, kurumsal sınırı zorlar ve ortak bir çevresel denetim kümesi belirler. Bir geliştirici, genel süreçlerinin bir parçası olarak güvenli bir geliştirme yaşam döngüsü geliştirir ken ve takip ederken, bir geliştiricinin düşünebileceği ortama karşı bu denetimler. Amaç aynı zamanda, gelişimi engelleyebilecek aşırı kısıtlayıcı olmayan, ancak makul bir denetim kümesi sağlayan bir ortam sağlamaktır. Laboratuvar sanal makinelerini içeren kuruluş birimindeki (OU) grup ilkeleri, üretimde bulunan toplam grup ilkelerinin bir alt kümesi olabilir. Alternatif olarak, tanımlanan riskleri düzgün bir şekilde azaltmak için ek bir küme olabilir.

### <a name="how-can-an-organization-ensure-data-integrity-to-ensure-that-remoting-developers-cant-remove-code-or-introduce-malware-or-unapproved-software"></a>Bir kuruluş, remoting geliştiricilerinin kodu kaldıramamasını veya kötü amaçlı yazılım veya onaylanmamış yazılım ları tanıtamamasını sağlamak için veri bütünlüğünü nasıl sağlayabilir?
DevTest Labs'da işbirliği yapmak için yeniden motive olan harici danışmanlar, yükleniciler veya çalışanların tehdidini azaltmak için çeşitli denetim katmanları vardır.

Daha önce de belirtildiği gibi, ilk adım, birisi politikayı ihlal ettiğinde sonuçlarını açıkça özetleyen kabul edilebilir bir kullanım politikası hazırlamış ve tanımlanmalıdır.

Uzaktan erişim için ilk denetim katmanı, doğrudan laboratuvara bağlı olmayan bir VPN bağlantısı üzerinden uzaktan erişim ilkesi uygulamaktır.

Denetimlerin ikinci katmanı, uzak masaüstü üzerinden kopyalanıp yapıştırmayı engelleyen bir grup ilkesi nesnesi kümesi uygulamaktır. FTP ve RDP hizmetleri gibi çevreden giden hizmetlerin çevredışına çıkmasına izin vermemek için bir ağ ilkesi uygulanabilir. Kullanıcı tanımlı yönlendirme tüm Azure ağ trafiğini şirket başına geri dönmeye zorlayabilir, ancak yönlendirme, içeriği ve oturumları taramaya bir proxy aracılığıyla denetlenmedikçe verilerin yüklenmesine izin verebilecek tüm URL'leri hesaba katamaz. Genel IP'ler, DevTest Labs'ı destekleyen sanal ağ içinde harici bir ağ kaynağının köprülemesine izin vermeyecek şekilde kısıtlanabilir.

Sonuç olarak, aynı tür kısıtlamaların kuruluş genelinde uygulanması gerekir, bu da bir içerik gönderisini kabul edebilecek tüm çıkarılabilir ortam veya harici URL yöntemlerini hesaba katar. Bir güvenlik ilkesini gözden geçirmek ve uygulamak için güvenlik uzmanınızla danışın. Daha fazla öneri için [Microsoft Siber Güvenlik'e](https://www.microsoft.com/security/default.aspx?&WT.srch=1&wt.mc_id=AID623240_SEM_sNYnsZDs)bakın.

## <a name="lab-configuration"></a>Laboratuvar yapılandırması

### <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>Kaynak Yöneticisi şablonundan nasıl bir laboratuvar oluştururum?
Laboratuvarlarınız için özel şablonlar oluşturmak için dağıtabileceğiniz veya değiştirebileceğiniz [bir GitHub laboratuvarı Azure Kaynak Yöneticisi şablonları deposu](https://azure.microsoft.com/resources/templates/101-dtl-create-lab) sunuyoruz. Her şablonun, kendi Azure aboneliğinizde olduğu gibi laboratuarı dağıtmak için bir bağlantısı vardır. Veya [PowerShell veya Azure CLI'yi kullanarak](../azure-resource-manager/templates/deploy-powershell.md)şablonu özelleştirebilir ve dağıtabilirsiniz.


### <a name="can-i-have-all-virtual-machines-to-be-created-in-a-common-resource-group-instead-having-each-machine-in-its-own-resource-group"></a>Tüm sanal makinelerin ortak bir kaynak grubunda oluşturulmasını sağlayabilir miyim, bunun yerine her makine kendi kaynak grubunda olmalıdır?
Evet, bir laboratuvar sahibi olarak, laboratuvarın kaynak grup tahsisini sizin için işlemesine izin verebilir veya belirttiğiniz ortak bir kaynak grubunda tüm sanal makinelerin oluşturulmasını sağlayabilirsiniz.

Ayrı kaynak grubu senaryosu:
-   DevTest Labs, döndürtdöndüğünüz her genel/özel IP sanal makine için yeni bir kaynak grubu oluşturur
-   DevTest Labs, aynı boyuttaki paylaşılan IP makineleri için bir kaynak grubu oluşturur.

Ortak kaynak grubu senaryosu:
-   Tüm sanal makineler, belirttiğiniz ortak kaynak grubunda döndürülmüştür. Laboratuvar [için](https://aka.ms/RGControl)daha fazla kaynak grubu ayırma öğrenin.

### <a name="how-do-i-maintain-a-naming-convention-across-my-devtest-labs-environment"></a>DevTest Labs ortamımda bir adlandırma kuralını nasıl korurum?
Geçerli kurumsal adlandırma sözleşmelerini Azure işlemlerine genişletmek ve Bunları DevTest Labs ortamında tutarlı hale getirmek isteyebilirsiniz. DevTest Labs'ı dağıtırken, belirli başlangıç ilkelerine sahip olduğunuzu öneririz. Tutarlılığı zorlamak için bu ilkeleri merkezi bir komut dosyasına ve JSON şablonlarına göre dağıtın. Adlandırma ilkeleri, abonelik düzeyinde uygulanan Azure ilkeleri aracılığıyla uygulanabilir. Azure İlkesi için JSON örnekleri için [Azure İlkesi örneklerine](../governance/policy/samples/index.md)bakın.

### <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Aynı abonelik altında kaç laboratuvar oluşturabilirim?
Abonelik başına oluşturulabilecek laboratuvar sayısı için belirli bir sınır yoktur. Ancak, abonelik başına kullanılan kaynak miktarı sınırlıdır. [Azure aboneliklerinin sınırları ve kotaları](../azure-resource-manager/management/azure-subscription-service-limits.md) ve [bu limitlerin nasıl artırılabildiğiniz](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests)hakkında bilgi edinebilirsiniz.


### <a name="how-many-vms-can-i-create-per-lab"></a>Laboratuvar başına kaç VM oluşturabilirim?
Laboratuvar başına oluşturulabilecek VM sayısında belirli bir sınır yoktur. Ancak, kullanılan kaynaklar (VM çekirdekleri, genel IP adresleri vb.) abonelik başına sınırlıdır. [Azure aboneliklerinin sınırları ve kotaları](../azure-resource-manager/management/azure-subscription-service-limits.md) ve [bu limitlerin nasıl artırılabildiğiniz](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests)hakkında bilgi edinebilirsiniz.

### <a name="how-do-i-determine-the-ratio-of-users-per-lab-and-the-overall-number-of-labs-that-are-needed-across-an-organization"></a>Laboratuvar başına kullanıcı oranını ve bir kuruluş genelinde ihtiyaç duyulan toplam laboratuvar sayısını nasıl belirleyebilirim?
Aynı geliştirme projesiyle ilişkili iş birimlerinin ve geliştirme gruplarının aynı laboratuvarla ilişkilendirilmesini öneririz. Her iki gruba da aynı tür ilkeler, görüntüler ve kapatma ilkeleri uygulanmasına olanak tanır.

Coğrafi sınırları da göz önünde bulundurmanız gerekebilir. Örneğin, Kuzey Doğu Amerika Birleşik Devletleri 'ndeki (ABD) geliştiriciler Doğu US2'de bulunan bir laboratuvar kullanabilirler. Dallas, Teksas ve Denver, Colorado'daki geliştiriciler, ABD Güney Merkez'de bir kaynak kullanmaya yönlendirilebilir. Harici bir üçüncü tarafla ortak bir çaba varsa, bunlar dahili geliştiriciler tarafından kullanılmayan bir laboratuvara atanabilir.

Azure DevOps Projeleri içinde belirli bir proje için de bir laboratuvar kullanabilirsiniz. Ardından, güvenliği, her iki kaynağa da erişim sağlayan belirli bir Azure Etkin Dizin grubu aracılığıyla uygularsınız. Laboratuvara atanan sanal ağ, kullanıcıları birleştirmek için başka bir sınır olabilir.

### <a name="how-can-we-prevent-the-deletion-of-resources-within-a-lab"></a>Laboratuvardaki kaynakların silinmesini nasıl önleyebiliriz?
Yalnızca yetkili kullanıcıların kaynakları silebilmeleri veya laboratuvar ilkelerini değiştirebilmeleri için uygun izinleri laboratuvar düzeyinde ayarlamanızı öneririz. Geliştiriciler **DevTest Labs Kullanıcıları** grubuna yerleştirilmelidir. Müşteri adayı geliştirici veya altyapı lideri **DevTest Labs Sahibi**olmalıdır. Sadece iki laboratuar sahibiniz olduğunu öneririz. Bu ilke, bozulmayı önlemek için kod deposuna doğru uzanır. Laboratuvar kullanıcılarıkaynakları kullanma hakkına sahiptir, ancak laboratuvar ilkelerini güncelleştiremez. Her yerleşik grubun bir laboratuvarda sahip olduğu rolleri ve hakları listeleyen aşağıdaki makaleye bakın: [Azure DevTest Labs'daki sahiplerini ve kullanıcılarını ekleyin.](devtest-lab-add-devtest-user.md)

### <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Laboratuvarıma doğrudan bağlantı mı paylaşırım?

1. Azure [portalında,](https://portal.azure.com)laboratuvara gidin.
2. Tarayıcınızdan **laboratuvar URL'sini** kopyalayın ve ardından laboratuvar kullanıcılarınızla paylaşın.

> [!NOTE]
> Bir laboratuvar kullanıcısı Microsoft hesabı olan ancak kuruluşunuzun Etkin Dizin örneğinin üyesi olmayan harici bir kullanıcıysa, paylaşılan bağlantıya erişmeye çalıştıklarında kullanıcı bir hata iletisi görebilir. Harici bir kullanıcı bir hata iletisi görürse, öncelikle kullanıcıdan adını Azure portalının sağ üst köşesinden seçmesini isteyin. Ardından, menünün Dizin bölümünde, kullanıcı laboratuvarın bulunduğu dizini seçebilir.

## <a name="virtual-machines"></a>Sanal makineler

### <a name="why-cant-i-see-vms-on-the-virtual-machines-page-that-i-see-in-devtest-labs"></a>DevTest Labs'da gördüğüm Sanal Makineler sayfasında vm'leri neden göremiyorum?
DevTest Labs'da bir VM oluşturduğunuzda, bu VM'ye erişme izni verilir. VM'yi hem laboratuvarlar sayfasında hem de **Sanal Makineler** sayfasında görüntüleyebilirsiniz. **DevTest Labs Owner** rolüne atanan kullanıcılar, laboratuvarda oluşturulan tüm Sanal **Makineler** sayfasını görebilir. Ancak, **DevTest Labs Kullanıcı** rolüne sahip kullanıcılara, diğer kullanıcıların oluşturduğu VM kaynaklarına otomatik olarak okuma erişimi verilmez. Yani, bu Sanal **Makineler** sayfasında görüntülenmez.


### <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Aynı şablondan aynı anda birden çok VM'yi nasıl oluştururum?
Aynı şablondan aynı anda birden çok VM oluşturmak için iki seçeneğiniz vardır:

- [Azure DevOps Görevleri uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks)kullanabilirsiniz.
- VM oluştururken [bir Kaynak Yöneticisi şablonu oluşturabilir](devtest-lab-add-vm.md#save-azure-resource-manager-template) ve Windows [PowerShell'den Kaynak Yöneticisi şablonuna dağıtabilirsiniz.](../azure-resource-manager/templates/deploy-powershell.md)
- Ayrıca, sanal makine oluşturma sırasında oluşturulacak makinenin birden fazla örneğini belirtebilirsiniz. Birden çok sanal makine örneği oluşturma hakkında daha fazla bilgi edinmek [için, bir laboratuvar sanal makine oluşturma](devtest-lab-add-vm.md)konusunda doc bakın.

### <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>Varolan Azure VM'lerimi DevTest Labs laboratuvarıma nasıl taşıyabilirim?
Mevcut VM'lerinizi DevTest Labs'a kopyalamak için:

1.  [Windows PowerShell komut dosyası](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/Scripts/CopyVirtualMachines/CopyAzVHDFromVMToLab.ps1)kullanarak varolan VM'nizin VHD dosyasını kopyalayın.
2.  DevTest Labs laboratuvarınızda [özel görüntü](devtest-lab-create-template.md) oluşturun.
3.  Özel resminizden laboratuvarda bir VM oluşturun.

### <a name="can-i-attach-multiple-disks-to-my-vms"></a>VM'lerime birden çok disk ekleyebilir miyim?

Evet, VM'lerinize birden çok disk ekleyebilirsiniz.

### <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Sınama için Windows OS görüntüsü kullanmak istersem, MSDN aboneliği satın almam gerekiyor mu?
Azure'da geliştirme veya sınama için Windows istemci işletim sistemi görüntülerini (Windows 7 veya daha sonraki sürüm) kullanmak için aşağıdaki adımlardan birini kullanın:

- [BIR MSDN aboneliği satın alın.](https://www.visualstudio.com/products/how-to-buy-vs)
- Kurumsal Sözleşmeniz varsa, [Kurumsal Geliştirme/Test teklifiyle](https://azure.microsoft.com/offers/ms-azr-0148p)bir Azure aboneliği oluşturun.

Her MSDN teklifi için Azure kredileri hakkında daha fazla bilgi için [Visual Studio aboneleri için Aylık Azure kredisine](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)bakın.


### <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Laboratuvarımdaki tüm VM'leri silme işlemini nasıl otomatikleştirebilirim?
Laboratuvar sahibi olarak Azure portalındaki laboratuarınızdaki VM'leri silebilirsiniz. PowerShell komut dosyası kullanarak laboratuvarınızdaki tüm VM'leri de silebilirsiniz. Aşağıdaki örnekte, yorumu **değiştirmek için değerler** altında, parametre değerlerini değiştirin. Azure portalındaki laboratuvar bölmesinden subscriptionId, labResourceGroup ve labName değerlerini alabilirsiniz.

```powershell
# Delete all the VMs in a lab.

# Values to change:
$subscriptionId = "<Enter Azure subscription ID here>"
$labResourceGroup = "<Enter lab's resource group here>"
$labName = "<Enter lab name here>"

# Sign in to your Azure account.
Connect-AzAccount

# Select the Azure subscription that has the lab. This step is optional
# if you have only one subscription.
Select-AzSubscription -SubscriptionId $subscriptionId

# Get the lab that has the VMs that you want to delete.
$lab = Get-AzResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

# Get the VMs from that lab.
$labVMs = Get-AzResource | Where-Object {
          $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
          $_.Name -like "$($lab.Name)/*"}

# Delete the VMs.
foreach($labVM in $labVMs)
{
    Remove-AzResource -ResourceId $labVM.ResourceId -Force
}
```

## <a name="environments"></a>Ortamlar

### <a name="how-can-i-use-resource-manager-templates-in-my-devtest-labs-environment"></a>DevTest Labs Ortamımda Kaynak Yöneticisi şablonlarını nasıl kullanabilirim?
Kaynak Yöneticisi [şablonlarınızı, DevTest Labs makalesinde Ortamlar özelliğinde](devtest-lab-test-env.md) belirtilen adımları kullanarak DevTest Labs ortamına dağıtırsınız. Temel olarak, Kaynak Yöneticisi şablonlarınızı git deposunda (Azure Repos veya GitHub) denetler ve şablonlarınız için laboratuvara özel bir [depo eklersiniz.](devtest-lab-test-env.md) Geliştirme makinelerini barındırmak için DevTest Labs kullanıyorsanız bu senaryo yararlı olmayabilir, ancak üretimi temsil eden bir evreleme ortamı oluşturuyorsanız yararlı olabilir.

Ayrıca, laboratuvar başına veya kullanıcı başına sanal makine sayısının, herhangi bir ortam (Kaynak Yöneticisi şablonları) tarafından değil, yalnızca laboratuvarda yerel olarak oluşturulan makine sayısını sınırladığını da belirtmekte yarar vardır.

## <a name="custom-images"></a>Özel Görüntüler

### <a name="how-can-i-set-up-an-easily-repeatable-process-to-bring-my-custom-organizational-images-into-a-devtest-labs-environment"></a>Özel kuruluş görüntülerimi DevTest Labs ortamına getirmek için kolayca tekrarlanabilir bir işlemi nasıl ayarlayabilirim?
Image [Factory deseni bu videoyu](https://sec.ch9.ms/ch9/8e8a/9ea0b8d4-b803-4f23-bca4-4808d9368e8a/dtlimagefactory_mid.mp4)izleyin. Bu senaryo gelişmiş bir senaryodur ve sağlanan komut dosyaları yalnızca örnek komut dosyalarıdır. Herhangi bir değişiklik gerekiyorsa, ortamınızda kullanılan komut dosyalarını yönetmeniz ve korumanız gerekir.

Görüntü fabrikası oluşturma hakkında ayrıntılı bilgi için Azure [DevTest Labs'da özel bir resim fabrikası oluşturun'a](image-factory-create.md)bakın.

### <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>Özel bir görüntü ile formül arasındaki fark nedir?
Özel görüntü yönetilen bir görüntüdür. Formül, ek ayarlarla yapılandırabileceğiniz ve sonra kaydedip çoğaltabileceğiniz bir görüntüdür. Aynı temel, değişmez görüntüyü kullanarak hızlı bir şekilde birkaç ortam oluşturmak istiyorsanız özel bir görüntü tercih edilebilir. Sanal bir ağın veya alt ağın parçası olarak veya belirli bir boyuttaki Bir VM olarak, VM'nizin yapılandırmasını en son bitlerle yeniden oluşturmak isterseniz formül daha iyi olabilir. Daha ayrıntılı bir açıklama için, [DevTest Labs'daki özel görüntüleri ve formülleri karşılaştırmaya](devtest-lab-comparing-vm-base-image-types.md)bakın.

### <a name="when-should-i-use-a-formula-vs-custom-image"></a>Özel görüntüye karşı bir formül ne zaman kullanmalıyım?
Genellikle, bu senaryoda belirleyici faktör maliyet ve yeniden. Birçok kullanıcı/laboratuvarda temel görüntünün üstünde çok sayıda yazılım içeren bir görüntü gerektiren bir senaryonuz varsa, özel bir görüntü oluşturarak maliyetleri azaltabilirsiniz. Bu, görüntünün bir kez oluşturulduğu anlamına gelir. Sanal makinenin kurulum süresini ve kurulum gerçekleştiğinde çalışan sanal makine nedeniyle oluşan maliyeti azaltır.

Ancak, dikkat edilmesi gereken ek bir faktör yazılım paketinizdeki değişikliklerin sıklığıdır. Günlük yapılar çalıştırıyorsanız ve bu yazılımın kullanıcılarınızın sanal makinelerinde olmasını istiyorsanız, özel bir resim yerine bir formül kullanmayı düşünün.

Daha ayrıntılı bir açıklama için, DevTest Labs'daki [özel görüntüleri ve formülleri karşılaştırma](devtest-lab-comparing-vm-base-image-types.md) ya da karşılaştırma ya da bkz.

### <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Özel görüntüler oluşturmak için VHD dosyalarını yükleme işlemini nasıl otomatikleştirebilirim?

Özel görüntüler oluşturmak için VHD dosyalarını yüklemeyi otomatikleştirmek için iki seçeneğiniz vardır:

- VHD dosyalarını kopyalamak veya laboratuvarla ilişkili depolama hesabına yüklemek için [AzCopy'yi](../storage/common/storage-use-azcopy-v10.md) kullanın.
- [Azure Depolama Gezgini'ni](../vs-azure-tools-storage-manage-with-storage-explorer.md)kullanın. Storage Explorer, Windows, OS X ve Linux üzerinde çalışan bağımsız bir uygulamadır.

Laboratuvarınızla ilişkili hedef depolama hesabını bulmak için:

1.  [Azure portalında](https://portal.azure.com)oturum açın.
2.  Sol menüde **Kaynak Grupları'nı**seçin.
3.  Laboratuvarınızla ilişkili kaynak grubunu bulun ve seçin.
4.  **Genel Bakış**altında, depolama hesaplarından birini seçin.
5.  **Bloblar**'ı seçin.
6.  Listedeki yüklemeleri arayın. Yoksa, adım 4'e dönün ve başka bir depolama hesabı deneyin.
7.  **URL'yi** AzCopy komutunuzda hedef olarak kullanın.

Azure Marketi görüntüsünü kendi özel kuruluş resmime karşı ne zaman kullanmalıyım?

### <a name="when-should-i-use-an-azure-marketplace-image-vs-my-own-custom-organizational-image"></a>Azure Marketi görüntüsünü kendi özel kuruluş resmime karşı ne zaman kullanmalıyım?
Azure Marketi, belirli endişeleriniz veya kuruluş gereksinimleriniz yoksa varsayılan olarak kullanılmalıdır. Bazı yaygın örnekler şunlardır;

- Temel görüntünün bir parçası olarak bir uygulamanın eklenmesini gerektiren karmaşık yazılım kurulumu.
- Bir uygulamanın yüklenmesi ve kurulumu, Azure Marketi görüntüsüne eklenecek bilgi işlem süresinin verimli bir şekilde kullanılması ndan değil, saatlerce sürebilir.
- Geliştiriciler ve sınayıcılar sanal makineye hızlı bir şekilde erişmeye ihtiyaç duyar ve yeni bir sanal makinenin kurulum süresini en aza indirmek ister.
- Tüm makineler için olması gereken uyumluluk veya düzenleyici koşullar (örneğin, güvenlik ilkeleri).
- Özel görüntülerin kullanılması hafife alınmamalıdır. Şimdi bu temel görüntüler için VHD dosyaları yönetmek gerekir gibi, ekstra karmaşıklık tanıtmak. Ayrıca rutin yazılım güncellemeleri ile bu temel görüntüleri yama gerekir. Bu güncelleştirmeler, yeni işletim sistemi (OS) güncelleştirmelerini ve yazılım paketinin kendisi için gereken güncelleştirmeleri veya yapılandırma değişikliklerini içerir.

## <a name="artifacts"></a>Yapıtlar

### <a name="what-are-artifacts"></a>Eserler nedir?
Yapılar, en son bitlerinizi dağıtmak veya geliştirme araçlarınızı bir VM'ye dağıtmak için kullanabileceğiniz özelleştirilebilir öğelerdir. VM oluştururken yapıları VM'nize takın. VM sağlandıktan sonra, yapılar VM'nizi dağıtır ve yapılandırır. Çeşitli önceden varolan eserler [bizim kamu GitHub deposunda](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts)mevcuttur. Ayrıca [kendi eserleri yazabilirsiniz.](devtest-lab-artifact-author.md)

### <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Benim artifakı VM oluşturma sırasında başarısız oldu. Nasıl sorun giderebilirim?
Başarısız yapınızın günlüklerini nasıl alacağınızı öğrenmek için [DevTest Labs'da yapı hatalarını nasıl tanılayacağınızı](devtest-lab-troubleshoot-artifact-failure.md)görün.

### <a name="when-should-an-organization-use-a-public-artifact-repository-vs-private-artifact-repository-in-devtest-labs"></a>Bir kuruluş DevTest Labs'da ortak yapı deposu ve özel eser deposu ne zaman kullanmalıdır?
[Ortak yapı deposu,](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) en sık kullanılan bir ilk yazılım paketi kümesi sağlar. Ortak geliştirici araçlarını ve eklentilerini yeniden oluşturmak için zaman yatırım yapmak zorunda kalmadan hızlı dağıtıma yardımcı olur. Kendi özel depolarını dağıtmayı seçebilirsiniz. Ortak ve özel bir depoyı paralel olarak kullanabilirsiniz. Ayrıca, genel depoyu devre dışı bırakmayı da seçebilirsiniz. Özel bir depo dağıtmak için ölçütler aşağıdaki sorular ve hususlar tarafından yönlendirilmelidir:

- Kuruluşun DevTest Labs teklifinin bir parçası olarak kurumsal lisanslı yazılıma sahip olması gerekliliği var mı? Yanıt evet ise, özel bir depo oluşturulmalıdır.
- Kuruluş, genel sağlama sürecinin bir parçası olarak gerekli olan belirli bir işlem sağlayan özel yazılım lar geliştiriyor mu? Yanıt evet ise, özel bir depo dağıtılmalıdır.
- Kuruluşun yönetim ilkesi yalıtım gerektiriyorsa ve dış depolar kuruluş tarafından doğrudan yapılandırma yönetimi altında değilse, özel bir yapı deposu dağıtılmalıdır. Bu işlemin bir parçası olarak, kamu deposunun ilk kopyası kopyalanabilir ve özel depoile tümleştirilebilir. Daha sonra, kuruluş içindeki hiç kimsenin artık bu depoya erişemesi için ortak depo devre dışı tutulabilir. Bu yaklaşım, kuruluş içindeki tüm kullanıcıları kuruluş tarafından onaylanan tek bir depoya sahip olmaya ve yapılandırma sürüklenmesini en aza indirmeye zorlar.


### <a name="should-an-organization-plan-for-a-single-repository-or-allow-multiple-repositories"></a>Bir kuruluş tek bir depo için plan yapmalı veya birden çok depoya izin vermeli mi?
Kuruluşunuzun genel yönetim ve yapılandırma yönetimi stratejisinin bir parçası olarak, merkezi bir depo kullanmanızı öneririz. Birden çok depo kullandığınızda, bunlar zaman içinde yönetilmeyen yazılım siloları haline gelebilir. Merkezi bir depo ile, birden çok takım projeleri için bu depodaki yapıları tüketebilir. Standardizasyon, güvenlik, yönetim kolaylığı ve çabaların çoğaltılmasını ortadan kaldırır. Merkezileştirmenin bir parçası olarak, aşağıdaki eylemler uzun vadeli yönetim ve sürdürülebilirlik için tavsiye edilen uygulamalardır:

- Azure Repos'u, Azure aboneliğinin kimlik doğrulama ve yetkilendirme için kullandığı Aynı Azure Etkin Dizin kiracısıyla ilişkilendirin.
- Azure Etkin `All DevTest Labs Developers` Dizini'nde merkezi olarak yönetilen bir grup oluşturun. Yapı geliştirmeye katkıda bulunan herhangi bir geliştirici bu gruba yerleştirilmelidir.
- Aynı Azure Etkin Dizin grubu, Azure Deposu deposuna ve laboratuvara erişim sağlamak için kullanılabilir.
- Azure Deposu'nda, dallanma veya forama, birincil üretim deposundan ayrı bir geliştirme deposu için kullanılmalıdır. İçerik yalnızca uygun bir kod incelemesinden sonra çekme isteğiyle ana dala eklenir. Kod gözden geçiren değişikliği onayladıktan sonra, ana dalı bakımından sorumlu bir müşteri adayı geliştirici, güncelleştirilmiş kodu birleştirir.

## <a name="cicd-integration"></a>CI/CD entegrasyonu

### <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>DevTest Labs ci/CD araç zincirimle entegre mi?
Azure DevOps kullanıyorsanız, DevTest Labs'daki sürüm ardınızı otomatikleştirmek için Bir [DevTest Labs Görevleri uzantısını](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) kullanabilirsiniz. Bu uzantıyla yapabileceğiniz görevlerden bazıları şunlardır:

- Otomatik olarak bir VM oluşturun ve dağıtın. Ayrıca, Azure Dosya Kopyalama veya PowerShell Azure DevOps Hizmetleri görevlerini kullanarak VM'yi en son yapıyla yapılandırabilirsiniz.
- Daha fazla araştırma için aynı VM'de bir hatayı yeniden oluşturmak için test ettikten sonra VM durumunu otomatik olarak yakalayın.
- Artık ihtiyaç duyulmadığında, serbest bırakma ardışık alanının sonundaki VM'yi silin.

Aşağıdaki blog gönderileri, Azure DevOps Hizmetleri uzantısını kullanma hakkında kılavuz ve bilgi sunar:

- [DevTest Labs ve Azure DevOps uzantısı](integrate-environments-devops-pipeline.md)
- [Azure DevOps Hizmetleri'nden varolan bir DevTest Labs laboratuvarında yeni bir VM dağıtma](https://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
- [DevTest Labs'a sürekli dağıtımlar için Azure DevOps Hizmetleri sürüm yönetimini kullanma](https://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Diğer sürekli tümleştirme (CI)/sürekli teslim (CD) araç zincirleri için Azure [PowerShell cmdlets](../azure-resource-manager/templates/deploy-powershell.md) ve [.NET SDK'ları](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/)kullanarak [Azure Kaynak Yöneticisi şablonlarını](https://azure.microsoft.com/resources/templates/) dağıtarak aynı senaryoları elde edebilirsiniz. Ayrıca, araç zincirinizle tümleştirmek [için DevTest Labs için REST API'lerini](https://aka.ms/dtlrestapis) de kullanabilirsiniz.

## <a name="networking"></a>Ağ Oluşturma

### <a name="when-should-i-create-a-new-virtual-network-for-my-devtest-labs-environment-vs-using-an-existing-virtual-network"></a>DevTest Labs ortamım için varolan bir sanal ağı kullanma m evrilen yeni bir sanal ağ ne zaman oluşturmalıyım?
Sanal Bilgilerinizin varolan altyapıyla etkileşimde olması gerekiyorsa, DevTest Labs ortamınızda varolan bir sanal ağ kullanmayı düşünün. ExpressRoute kullanıyorsanız, aboneliklerde kullanılmak üzere atanan IP adres alanınızı parçalamaymak için VNet/Subnets miktarını en aza indirmek isteyebilirsiniz.

Burada VNet eşleme deseni kullanmayı düşünün[(Hub-Spoke modeli)](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke)de. Bu yaklaşım, abonelikler arasında vnet/subnet iletişimini sağlar. Aksi takdirde, her DevTest Labs ortamı kendi sanal ağı olabilir.

Abonelik başına sanal ağ sayısında [sınırlar](../azure-resource-manager/management/azure-subscription-service-limits.md) vardır. Varsayılan tutar 50'dir, ancak bu sınır 100'e yükseltilebilir.

### <a name="when-should-i-use-a-shared-ip-vs-public-ip-vs-private-ip"></a>Paylaşılan IP ile ortak IP'yi ne zaman kullanmalıyım?

Siteden siteye VPN veya Express Route kullanıyorsanız, makinelerinize dahili ağınızdan erişilebilmek ve herkese açık internet üzerinden erişilememek için özel IP'ler kullanmayı düşünün.

> [!NOTE]
> Laboratuvar sahipleri, kimsenin yanlışlıkla VM'leri için ortak IP adresleri oluşturmadığından emin olmak için bu alt net ilkesini değiştirebilir. Abonelik sahibi, genel IP'lerin oluşturulmasını engelleyen bir abonelik ilkesi oluşturmalıdır.

Paylaşılan ortak IP'leri kullanırken, laboratuardaki sanal makineler ortak bir IP adresini paylaşır. Bu yaklaşım, belirli bir abonelik için ortak IP adreslerinin sınırlarını ihlal etmekten kaçınmanız gerektiğinde yararlı olabilir.

### <a name="how-do-i-ensure-that-development-and-test-virtual-machines-are-unable-to-reach-the-public-internet-are-there-any-recommended-patterns-to-set-up-network-configuration"></a>Sanal makinelerin genel internete erişememesini nasıl sağlarım? Ağ yapılandırmasını ayarlamak için önerilen desenler var mı?

Evet. Göz önünde bulundurulması gereken iki yönü vardır - gelen ve giden trafik.

- **Gelen trafik** – Sanal makinenin genel bir IP adresi yoksa, internet üzerinden ulaşılamıyor. Ortak bir yaklaşım, hiçbir kullanıcının ortak bir IP adresi oluşturamayacağı bir abonelik düzeyi ilkesinin ayarlıolduğundan emin olmaktır.
- **Giden trafik** – Sanal makinelerin genel internete doğrudan erişmesini engellemek ve trafiği kurumsal bir güvenlik duvarı üzerinden zorlamak istiyorsanız, zorunlu yönlendirme yi kullanarak trafiği ekspres rota veya VPN üzerinden şirket içinde yönlendirebilirsiniz.

> [!NOTE]
> Proxy ayarları olmadan trafiği engelleyen bir proxy sunucunuz varsa, laboratuvarın yapı depolama hesabına özel durumlar eklemeyi unutmayın.

Sanal makineler veya alt ağlar için ağ güvenlik gruplarını da kullanabilirsiniz. Bu adım, / blok trafik izin vermek için ek bir koruma katmanı ekler.

## <a name="troubleshooting"></a>Sorun giderme

### <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Mevcut sanal ağım neden düzgün kaydolmuyor?
Bir olasılık, sanal ağ adınızı dönemleri içermenizdir. Bu öyleyse, dönemleri kaldırmayı veya tirelerle değiştirmeyi deneyin. Ardından, sanal ağı kaydetmek için yeniden deneyin.

### <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>PowerShell'den vm'si temin ettiğimde neden "Üst kaynak bulunamadı" hatası alıyorum?
Bir kaynak başka bir kaynağın üst öğesi olduğunda, alt kaynak oluşturmadan önce üst kaynağın bulunması gerekir. Ana kaynak yoksa, bir **ParentResourceNotFound** iletisi görürsünüz. Üst kaynağa bağımlılık belirtmezseniz, alt kaynak üst kaynakönce dağıtılabilir.

VM'ler, kaynak grubundaki bir laboratuvar altında alt kaynaklardır. PowerShell'i kullanarak VM'leri dağıtmak için Kaynak Yöneticisi şablonlarını kullandığınızda, PowerShell komut dosyasında sağlanan kaynak grubu adı laboratuvarın kaynak grubu adı olmalıdır. Daha fazla bilgi için [bkz.](../azure-resource-manager/templates/common-deployment-errors.md)

### <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>VM dağıtımı başarısız olursa daha fazla hata bilginini nerede bulabilirim?
VM dağıtım hataları etkinlik günlüklerinde yakalanır. Denetim günlükleri veya **Sanal makine tanılama** altında laboratuvar VM etkinlik **günlüklerini** laboratuvarın VM sayfasındaki kaynak menüsünde bulabilirsiniz (sayfa Sanal makineler listemden VM'yi seçtikten sonra görüntülenir).

Bazen, VM dağıtımı başlamadan önce dağıtım hatası oluşur. VM ile oluşturulan bir kaynağın abonelik sınırının aşılması buna bir örnektir. Bu durumda, hata ayrıntıları laboratuar düzeyinde etkinlik günlüklerinde yakalanır. Etkinlik günlükleri **Yapılandırma ve ilkeler** ayarlarının alt kısmında yer alır. Azure'da etkinlik günlüklerini kullanma hakkında daha fazla bilgi [için, kaynaklardaki eylemleri denetlemek için etkinlik günlüklerini görüntüle'ye](../azure-resource-manager/management/view-activity-logs.md)bakın.

### <a name="why-do-i-get-location-is-not-available-for-resource-type-error-when-trying-to-create-a-lab"></a>Bir laboratuvar oluşturmaya çalışırken neden "konum kaynak türü için kullanılamıyor" hatası alıyorum?
Bir laboratuvar oluşturmaya çalıştığınızda aşağıdakine benzer bir hata iletisi görebilirsiniz:

```
The provided location 'australiacentral' is not available for resource type 'Microsoft.KeyVault/vaults'. List of available regions for the resource type is 'northcentralus,eastus,northeurope,westeurope,eastasia,southeastasia,eastus2,centralus,southcentralus,westus,japaneast,japanwest,australiaeast,australiasoutheast,brazilsouth,centralindia,southindia,westindia,canadacentral,canadaeast,uksouth,ukwest,westcentralus,westus2,koreacentral,koreasouth,francecentral,southafricanorth
```

Aşağıdaki adımlardan birini alarak bu hatayı çözebilirsiniz:

#### <a name="option-1"></a>Seçenek 1
Bölgelere göre [kullanılabilen Ürünler](https://azure.microsoft.com/global-infrastructure/services/) sayfasında Azure bölgelerindeki kaynak türünün kullanılabilirliğini denetleyin. Kaynak türü belirli bir bölgede kullanılamıyorsa, DevTest Labs o bölgede bir laboratuvar oluşturulmasını desteklemez. Laboratuvarınızı oluştururken başka bir bölge seçin.

#### <a name="option-2"></a>2. Seçenek
Kaynak türü bölgenizde varsa, aboneliğinizde kayıtlı olup olmadığını kontrol edin. [Bu makalede](../azure-resource-manager/management/resource-providers-and-types.md)gösterildiği gibi abonelik sahibi düzeyinde yapılabilir.
