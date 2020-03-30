---
title: Azure Etkin Dizini için otomatik kullanıcı sağlama dağıtımı planlama
description: Otomatik kullanıcı sağlamanın planlanması ve yürütülmesi için yönerge
services: active-directory
author: martincoetzer
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-provisioning
ms.topic: conceptual
ms.workload: identity
ms.date: 10/17/2019
ms.author: martinco
ms.reviewer: arvindha
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d2f284fddfc49632e467adbf5877856b40a81dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522419"
---
# <a name="plan-an-automatic-user-provisioning-deployment"></a>Otomatik kullanıcı sağlama dağıtımı planlama

Birçok kuruluş, son kullanıcı üretkenliği için ServiceNow, Zscaler ve Slack gibi hizmet (SaaS) uygulamaları olarak yazılıma güvenir. Tarihsel olarak BT personeli, her SaaS uygulamasında kullanıcı kimliklerini güvenli bir şekilde yönetmek için CSV dosyalarını yükleme veya özel komut dosyaları kullanma gibi manuel sağlama yöntemlerine güvendi. Bu işlemler hataya yatkın, güvensiz ve yönetilmesi zor.

Azure Active Directory (Azure AD) otomatik kullanıcı sağlama, iş kurallarına dayalı olarak SaaS uygulamalarında kullanıcı kimliklerinin oluşturulmasını, bakımını ve kaldırılmasını güvenli bir şekilde otomatikleştirerek bu işlemi basitleştirir. Bu otomasyon, kimlik yönetim sistemlerinizi bulut tabanlı çözümlere olan bağımlılığını genişletirken hem yalnızca bulut hem de karma ortamlarda etkin bir şekilde ölçeklendirmenize olanak tanır.

İşlevselliği daha iyi anlamak için [Azure Active Directory ile SaaS uygulamalarına kullanıcı sağlama ve bu](../app-provisioning/user-provisioning.md) uygulamaları sağlama yı otomatikleştir' e bakın.

## <a name="learn"></a>Öğrenin

Kullanıcı sağlama, sürekli kimlik yönetimi için bir temel oluşturur ve yetkili kimlik verilerine dayanan iş süreçlerinin kalitesini artırır.

### <a name="key-benefits"></a>Önemli avantajlar

Otomatik kullanıcı sağlamayı etkinleştirmenin temel avantajları şunlardır:

* **Artan verimlilik.** Tek bir kullanıcı sağlama yönetim arabirimi yle SaaS uygulamalarında kullanıcı kimliklerini yönetebilirsiniz. Bu arabirimde tek bir sağlama ilkeleri kümesi vardır.

* **Riski yönetin.** Rolleri ve/veya erişimi tanımlayan çalışan durumuna veya grup üyeliklerine göre değişiklikleri otomatikleştirerek güvenliği artırabilirsiniz.

* **Adres uyum ve yönetişim**. Azure AD, her kullanıcı sağlama isteği için yerel denetim günlüklerini destekler. İstekler hem kaynak hem de hedef sistemlerde yürütülür. Bu, uygulamalara erişimi olan kişilerle tek bir ekrandan izlemenize olanak tanır.

* **Maliyeti azaltın.** Otomatik kullanıcı sağlama, manuel sağlamayla ilişkili verimsizlikleri ve insan hatalarını önleyerek maliyetleri azaltır. Özel olarak geliştirilmiş kullanıcı sağlama çözümleri, komut dosyaları ve denetim günlükleri gereksinimini azaltır.

### <a name="licensing"></a>Lisanslama

Azure AD, uygulama galerisi menüsünde sağlanan şablonları kullanarak tüm uygulamaların self servis tümleştirmesini sağlar. Lisans gereksinimlerinin tam listesi için [Azure AD lisanslama sayfasına](https://azure.microsoft.com/pricing/details/active-directory/)bakın.

#### <a name="application-licensing"></a>Başvuru lisansı

Otomatik olarak sağlamak istediğiniz uygulama(lar) için uygun lisanslara ihtiyacınız olacaktır. Uygulama için atanan kullanıcıların uygulama rolleri için uygun lisanslara sahip olup olmadığını uygulama sahipleriyle görüşün. Azure AD rolleri temel alarak otomatik sağlamayı yönetiyorsa, Azure AD'de atanan rolleruygulama lisanslarına hizalanmalıdır. Uygulamada sahip olunan yanlış lisanslar, kullanıcının sağlanması/güncellenmesi sırasında hatalara neden olabilir.

### <a name="terms"></a>Koşullar

Bu makalede aşağıdaki terimler kullan›r:

* CRUD işlemleri - Kullanıcı hesaplarında gerçekleştirilen işlemler: Oluşturma, Okuma, Güncelleme, Silme.

* Tek oturum açma (SSO) - Bir kullanıcının bir kez oturum açma ve Tüm SSO özellikli uygulamalara erişme olanağı. Kullanıcı sağlama bağlamında SSO, kullanıcıların otomatik kullanıcı sağlama kullanan tüm sistemlere erişmek için tek bir hesaba sahip olmasının bir sonucudur.

* Kaynak sistem - Azure REKLAM'ın karşılık verdiği kullanıcı deposu. Azure AD, çoğu önceden tümleşik sağlama bağlayıcısı için kaynak sistemdir. Ancak, SAP, İş Günü ve AWS gibi bulut uygulamaları için bazı özel durumlar vardır. Örneğin, [İş Günü'nden AD'ye Kullanıcı sağlama](../saas-apps/workday-inbound-tutorial.md)konusuna bakın.

* Hedef sistem - Azure REKLAM'ın karşılık verdiği kullanıcı deposu. Hedef sistem genellikle ServiceNow, Zscaler ve Slack gibi bir SaaS uygulamasıdır. Hedef sistem, AD gibi şirket içi bir sistem de olabilir.

* [Etki alanları arası Kimlik Yönetimi Sistemi (SCIM)](https://aka.ms/scimoverview) - Kullanıcı sağlamanın otomasyonuna olanak tanıyan açık bir standarttır. SCIM, Microsoft gibi kimlik sağlayıcıları ile Salesforce veya kullanıcı kimlik bilgileri gerektiren diğer SaaS uygulamaları gibi hizmet sağlayıcıları arasında kullanıcı kimlik verilerini iletir.

### <a name="training-resources"></a>Eğitim kaynakları

| Kaynaklar| Bağlantı ve Açıklama |
| - | - |
| İsteğe bağlı web seminerleri| [Azure AD ile Kurumsal Uygulamalarınızı Yönetme](https://info.microsoft.com/CO-AZUREPLAT-WBNR-FY18-03Mar-06-ManageYourEnterpriseApplicationsOption1-MCW0004438_02OnDemandRegistration-ForminBody.html)<br>Azure AD'nin kurumsal SaaS uygulamalarınızda sso'yu elde etmenize ve erişimi denetlemek için en iyi uygulamalara nasıl yardımcı olabileceğini öğrenin. |
| Videolar| [Active Azure Directory'de kullanıcı sağlama nedir?](https://youtu.be/_ZjARPpI6NI) <br> [Active Azure Directory'de kullanıcı sağlama nasıl dağıtılır?](https://youtu.be/pKzyts6kfrw) <br> [Salesforce'u Azure AD ile tümleştirme: Kullanıcı Sağlama otomatikleştirme](https://azure.microsoft.com/resources/videos/integrating-salesforce-with-azure-ad-how-to-automate-user-provisioning/) |
| Online kurslar| SkillUp Online: [Kimlikleri Yönetme](https://skillup.online/courses/course-v1:Microsoft+AZ-100.5+2018_T3/about) <br> Azure AD'yi birçok SaaS uygulamasıyla nasıl entegre edacağınızı ve bu uygulamalara kullanıcı erişimini nasıl sağlayacağınızı öğrenin. |
| Kitaplar| [Web Uygulamaları için Azure Active Directory ile Modern Kimlik Doğrulama (Geliştirici Başvurusu) 1.](https://www.amazon.com/Authentication-Directory-Applications-Developer-Reference/dp/0735696942/ref=sr_1_fkmr0_1?keywords=Azure+multifactor+authentication&qid=1550168894&s=gateway&sr=8-1-fkmr0)  <br> Bu, bu yeni ortamlar için Active Directory kimlik doğrulama çözümleri oluşturmak için yetkili, derin dalış kılavuzudur. |
| Öğreticiler| [SaaS uygulamalarını Azure AD ile nasıl tümleştirebilirsiniz hakkındaki öğreticiler listesine](../saas-apps/tutorial-list.md)bakın. |
| SSS| Otomatik kullanıcı sağlama ile ilgili [sık sorulan sorular](../app-provisioning/user-provisioning.md) |

### <a name="solution-architectures"></a>Çözüm mimarileri

Azure AD sağlama hizmeti, her uygulama satıcısı tarafından sağlanan kullanıcı yönetimi API uç noktalarına bağlanarak kullanıcıları SaaS uygulamalarına ve diğer sistemlere karşı sağlar. Bu kullanıcı yönetimi API uç noktaları, Azure AD'nin kullanıcıları programlı bir şekilde oluşturmasına, güncelleştirmesine ve kaldırmasına olanak tanır.

#### <a name="automatic-user-provisioning-for-hybrid-enterprises"></a>Hibrit işletmeler için otomatik kullanıcı sağlama

Bu örnekte, kullanıcılar ve gruplar şirket içi dizine bağlı bir İk veritabanında oluşturulur. Azure AD sağlama hizmeti, hedef SaaS uygulamalarına otomatik kullanıcı sağlamayı yönetir.

 ![kullanıcı sağlama](./media/plan-auto-user-provisioning/hybridprovisioning.png)

**İş akışının açıklaması:**

1. Kullanıcılar/gruplar SAP gibi şirket içi bir İk uygulamasında/sisteminde oluşturulur. 

1. **Azure AD Connect aracısı,** yerel AD'den Azure AD'ye kimliklerin (kullanıcılar ve gruplar) zamanlanmış eşitlemelerini çalıştırın.

1. **Azure AD sağlama hizmeti,** kaynak sisteme ve hedef sisteme karşı bir [başlangıç döngüsü](../app-provisioning/user-provisioning.md) başlatır. 

1. **Azure AD sağlama hizmeti,** ilk döngüden itibaren değiştirilen tüm kullanıcılar ve gruplar için kaynak sistemi sorgular ve [artımlı döngülerde](../app-provisioning/user-provisioning.md)değişiklikleri zorlar.

#### <a name="automatic-user-provisioning-for-cloud-only-enterprises"></a>Yalnızca bulut işletmeleri için otomatik kullanıcı sağlama

Bu örnekte, Kullanıcı oluşturma Azure AD oluşur ve Azure AD sağlama hizmeti hedef (SaaS) uygulamalarına otomatik kullanıcı sağlama yönetir.

![Resim 2](./media/plan-auto-user-provisioning/cloudprovisioning.png)

**İş akışının açıklaması:**

1. Kullanıcılar/gruplar Azure AD'de oluşturulur.

1. **Azure AD sağlama hizmeti,** kaynak sisteme ve hedef sisteme karşı bir [başlangıç döngüsü](../app-provisioning/user-provisioning.md) başlatır. 

1. **Azure AD sağlama hizmeti,** ilk döngüden bu yana güncelleştirilen tüm kullanıcılar ve gruplar için kaynak sistemi sorgular ve [artımlı döngüler](../app-provisioning/user-provisioning.md)gerçekleştirir.

#### <a name="automatic-user-provisioning-for-cloud-hr-applications"></a>Bulut İk uygulamaları için otomatik kullanıcı sağlama 

Bu örnekte, kullanıcılar ve veya gruplar, İş Günü ve Başarı Faktörleri gibi bir bulut İk uygulamasında oluşturulur. Azure AD sağlama hizmeti ve Azure AD Connect sağlama aracısı, bulut İk uygulaması kiracısından kullanıcı verilerini AD'ye sağlar. Hesaplar AD'de güncelleştirildikten sonra Azure AD Connect aracılığıyla Azure AD ile senkronize edilir ve e-posta adresleri ve kullanıcı adı öznitelikleri bulut İk uygulaması kiracısına geri yazılabilir.

![Resim 2](./media/plan-auto-user-provisioning/workdayprovisioning.png)

1.  **İk ekibi** bulut İk uygulaması kiracı işlemleri gerçekleştirir.
2.  **Azure AD sağlama hizmeti,** bulut İk uygulaması kiracısından zamanlanan döngüleri çalıştırır ve AD ile eşitlenmesi için işlenmesi gereken değişiklikleri tanımlar.
3.  **Azure AD sağlama hizmeti,** AD hesabı oluşturma/güncelleştirme/etkinleştirme/devre dışı etme işlemlerini içeren bir istek yükü yle Azure AD Connect sağlama aracısını çağırır.
4.  **Azure AD Connect sağlama aracısı,** AD hesabı verilerini yönetmek için bir hizmet hesabı kullanır.
5.  **Azure AD Connect,** AD'deki güncelleştirmeleri çekmek için delta eşitleme çalıştırın.
6.  **AD** güncelleştirmeleri Azure AD ile senkronize edilir. 
7.  **Azure AD sağlama hizmeti,** Azure AD'den bulut İk uygulaması kiracısına e-posta özniteliği ve kullanıcı adı yazar.

## <a name="plan-the-deployment-project"></a>Dağıtım projesini planlama

Ortamınızda kullanıcı sağlama dağıtma stratejisini belirlemek için kuruluş gereksinimlerinizi göz önünde bulundurun.

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlarla etkileşimde bulunun

Teknoloji projeleri başarısız olduğunda, bunun nedeni genellikle etki, sonuçlar ve sorumluluklar üzerindeki uyumsuz beklentilerdir. Bu tuzaklardan kaçınmak için, [doğru paydaşlarla etkileşimde bulunduğunuzdan](https://aka.ms/deploymentplans) ve projedeki paydaş rollerinin paydaşları ve bunların proje girdilerini ve hesap yeteneklerini belgeleyerek iyi anlaşıldığından emin olun.

### <a name="plan-communications"></a>İletişimi planlama

İletişim, herhangi bir yeni hizmetin başarısı için çok önemlidir. Kullanıcılarınızla deneyimlerinin nasıl değişeceğini, ne zaman değişeceğini ve sorunlarla karşılaşırsa nasıl destek kazanabileceklerini proaktif bir şekilde iletin.

### <a name="plan-a-pilot"></a>Bir pilot planlayın

Otomatik kullanıcı sağlamanın ilk yapılandırmasının, üretimdeki tüm kullanıcılara ölçeklemeden önce küçük bir kullanıcı alt kümesiyle test ortamında olmasını öneririz. Bir pilot çalıştırmak için [en iyi uygulamalara](../fundamentals/active-directory-deployment-plans.md#best-practices-for-a-pilot) bakın.

#### <a name="best-practices-for-a-pilot"></a>Bir pilot için en iyi uygulamalar  

Bir pilot herkes için bir yetenek dağıtmadan önce küçük bir grup ile test etmenizi sağlar. Testinizin bir parçası olarak, kuruluşunuzdaki her kullanım örneğinin kapsamlı bir şekilde test edildiğinden emin olun.

İlk dalganızda, BT'yi, kullanılabilirliği ve geri bildirimi test edip sağlayabilen diğer uygun kullanıcıları hedef alın. Kullanıcılarınıza gönderdiğiniz iletişimleri ve talimatları daha da geliştirmek ve destek personelinizin görebileceği sorun türleri hakkında bilgi vermek için bu geri bildirimi kullanın.

Hedeflenen grubun (lar) kapsamını artırarak daha büyük kullanıcı gruplarına rollout genişletin. Bu, dinamik [grup üyeliği](../users-groups-roles/groups-dynamic-membership.md)veya hedeflenen gruba kullanıcıları el ile ekleyerek yapılabilir.

## <a name="plan-application-connections-and-administration"></a>Uygulama bağlantılarını ve yönetimini planla

Sağlamayı destekleyen tüm uygulamaları görüntülemek ve yönetmek için Azure AD portalını kullanın. Bkz. [Uygulamalarınızı portalda bulma](../app-provisioning/configure-automatic-user-provisioning-portal.md).

### <a name="determine-the-type-of-connector-to-use"></a>Kullanılacak bağlayıcının türünü belirleme

Otomatik sağlamayı etkinleştirmek ve yapılandırmak için gereken gerçek adımlar uygulamaya bağlı olarak değişir. Otomatik olarak sağlamak istediğiniz uygulama [Azure AD SaaS uygulama galerisinde listeleniyorsa,](../saas-apps/tutorial-list.md)önceden tümleşik kullanıcı sağlama bağlayıcısını yapılandırmak için uygulamaya özel [tümleştirme öğreticisini](../saas-apps/tutorial-list.md) seçmeniz gerekir.

Değilse, aşağıdaki adımları izleyin:

1. Önceden tümleşik bir kullanıcı sağlama bağlayıcısı için [istek oluşturun.](../develop/howto-app-gallery-listing.md) Ekibimiz, SCIM'i destekliyorsa, uygulamanızı platformumuza dahil etmek için sizinle ve uygulama geliştiricisiyle birlikte çalışacaktır.

1. Uygulama için [BYOA SCIM](../app-provisioning/use-scim-to-provision-users-and-groups.md) genel kullanıcı sağlama desteğini kullanın. Bu, Azure AD'nin kullanıcıları uygulamaya önceden entegre bir sağlama bağlayıcısı olmadan sağlaması için bir gerekliliktir.

1. Uygulama BYOA SCIM konektörünü kullanabiliyorsa, uygulama için BYOA SCIM bağlayıcısını yapılandırmak için [BYOA SCIM tümleştirme öğreticisine](../app-provisioning/use-scim-to-provision-users-and-groups.md) bakın.

Daha fazla bilgi için Azure [AD otomatik kullanıcı sağlama da hangi uygulamaları ve sistemleri kullanabilirim?](../app-provisioning/user-provisioning.md)

### <a name="collect-information-to-authorize-application-access"></a>Uygulama erişimini yetkilendirmek için bilgi toplama

Otomatik kullanıcı sağlamanın ayarlanması, uygulama başına bir işlemdir. Her uygulama için, hedef sistemin kullanıcı yönetimi bitiş noktasına bağlanmak için [yönetici kimlik bilgilerini](../app-provisioning/configure-automatic-user-provisioning-portal.md) sağlamanız gerekir.

Aşağıdaki resimde gerekli yönetici kimlik bilgilerinin bir sürümü gösterilmektedir:

![Kullanıcı hesabı sağlama ayarlarını yönetmek için sağlama ekranı](./media/plan-auto-user-provisioning/userprovisioning-admincredentials.png)

Bazı uygulamalar yönetici kullanıcı adı ve parola gerektirirken, diğerleri taşıyıcı belirteci gerektirebilir.

## <a name="plan-user-and-group-provisioning"></a>Kullanıcı ve grup sağlama yı planlama

Kurumsal uygulamalar için kullanıcı sağlama özelliğini etkinleştiriyorsanız, [Azure portalı](https://portal.azure.com/) öznitelik eşleme yoluyla öznitelik değerlerini denetler.

### <a name="determine-operations-for-each-saas-app"></a>Her SaaS uygulaması için işlemleri belirleme

Her uygulama, Azure AD'nizdeki özniteliklerle eşlenmiş olması gereken benzersiz kullanıcı veya grup özniteliklerine sahip olabilir. Uygulamada yalnızca CRUD işlemlerinin bir alt kümesi olabilir.

Her uygulama için aşağıdaki bilgileri belgele:

* Hedef sistemler için kullanıcı ve veya Grup nesneleri üzerinde gerçekleştirilecek CRUD sağlama işlemleri. Örneğin, her SaaS uygulaması işletme sahibi tüm olası işlemleri istemeyebilir.

* Kaynak sistemde bulunan öznitelikler

* Hedef sistemde bulunan öznitelikler

* Sistemler arasındaki özniteliklerin eşlenemi.

### <a name="choose-which-users-and-groups-to-provision"></a>Hangi kullanıcı ve grupların sağlanmasını seçin

Otomatik kullanıcı sağlama uygulamadan önce, uygulamanıza sağlanacak kullanıcıları ve grupları belirlemeniz gerekir.

* Bir uygulamaya hangi kullanıcıların sağlanmış olduğunu belirleyen öznitelik tabanlı kuralları tanımlamak için [kapsam filtreleri](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md) kullanın.

* Ardından, ek filtreleme için gerektiğinde [kullanıcı ve grup atamalarını](../manage-apps/assign-user-or-group-access-portal.md) kullanın.

### <a name="define-user-and-group-attribute-mapping"></a>Kullanıcı ve grup özniteliği eşleme özelliğini tanımlama

Otomatik kullanıcı sağlama uygulamak için, uygulama için gerekli olan kullanıcı ve grup özniteliklerini tanımlamanız gerekir. Azure AD kullanıcı nesneleri ve her SaaS uygulamasının kullanıcı nesneleri arasında önceden yapılandırılmış öznitelikler ve [öznitelik eşlemeleri](../app-provisioning/configure-automatic-user-provisioning-portal.md) kümesi vardır. Tüm SaaS uygulamaları grup özniteliklerini etkinleştirmez.

Azure AD, sabit değerler sağlayarak veya [öznitelik eşlemeleri için ifadeler yazarak](../app-provisioning/functions-for-customizing-application-data.md)doğrudan öznitelik-öznitelik eşleme ile destekler. Bu esneklik, hedeflenen sistemin özniteliğinde nelerin doldurulacağı üzerinde iyi bir denetim sağlar. [Microsoft Graph API](../app-provisioning/export-import-provisioning-configuration.md) ve Graph Explorer'ı kullanarak kullanıcı sağlama öznitelik eşlemelerinizi ve şemasını bir JSON dosyasına dışa aktarabilir ve Azure AD'ye geri aktarabilirsiniz.

Daha fazla bilgi için bkz: [Azure Etkin Dizini'ndeki SaaS Uygulamaları için Kullanıcı Sağlama Özellik Eşlemeleri'ni Özelleştirme.](../app-provisioning/customize-application-attributes.md)

### <a name="special-considerations-for-user-provisioning"></a>Kullanıcı sağlama için özel hususlar

Dağıtım sonrası sorunları azaltmak için aşağıdakileri göz önünde bulundurun:

* Kaynak ve hedef uygulamalar arasında kullanıcı/grup nesneleri eşlemek için kullanılan özniteliklerin esnek olduğundan emin olun. Öznitelikler değişirse (örneğin, kullanıcı şirketin farklı bir bölümüne taşınırsa) kullanıcıların/grupların yanlış sağlanmasına neden olmamalıdır.

* Uygulamaların, kullanıcı sağlamanın doğru çalışması için karşılanması gereken belirli kısıtlamaları ve/veya gereksinimleri olabilir. Örneğin, Slack belirli öznitelikler için değerleri truncates. Her uygulamaya özgü [otomatik kullanıcı sağlama öğreticilerine](../saas-apps/tutorial-list.md) bakın.

* Kaynak ve hedef sistemler arasındaki şema tutarlılığını doğrulayın. Sık karşılaşılan sorunlar arasında UPN veya posta eşleşmeyen gibi öznitelikler yer alır. Örneğin, Azure AD'deki UPN, uygulama olarak *john_smith@contoso.com* ve *jsmith@contoso.com*uygulamada ayarlanır. Daha fazla bilgi için [Bkz. Kullanıcı ve grup şema başvurusu.](../app-provisioning/use-scim-to-provision-users-and-groups.md)

## <a name="plan-testing-and-security"></a>Test ve güvenlik planı

Dağıtımınızın her aşamasında, sonuçların beklendiği gibi test ettiğinizden ve sağlama döngülerini denetlediğinizden emin olun.

### <a name="plan-testing"></a>Planı testi

Uygulama için otomatik kullanıcı sağlama yı yapılandırıldıktan sonra, bu çözümün kuruluşunuzun gereksinimlerini karşıladığını doğrulamak için test servis taleplerini çalıştırırsınız.

| Senaryolar| Beklenen sonuçlar |
| - | - |
| Kullanıcı hedef sisteme atanan bir gruba eklenir | Kullanıcı nesnesi hedef sistemde sağlanır. <br>Kullanıcı hedef sistemde oturum açabilir ve istenen eylemleri gerçekleştirebilir. |
| Kullanıcı hedef sisteme atanan bir gruptan kaldırılır | Kullanıcı nesnesi hedef sistemde deprovisioned.<br>Kullanıcı hedef sistemde oturum açamaz. |
| Kullanıcı bilgileri Azure AD'de herhangi bir yöntemle güncelleştirilir | Güncelleştirilmiş kullanıcı öznitelikleri, artımlı bir döngüden sonra hedef sisteme yansıtılır |
| Kullanıcı kapsam dışında | Kullanıcı nesnesi devre dışı bırakılır veya silinir. <br>Not: Bu [davranış, İş Günü sağlama](skip-out-of-scope-deletions.md)için geçersiz kılındı. |

### <a name="plan-security"></a>Güvenlik planı

Bir görevin bir parçası olarak güvenlik incelemesi nin gerekli olması yaygındır. Bir güvenlik incelemesi ne gerekiyorsa, hizmet olarak kimlik için genel bir bakış sağlayan birçok Azure AD [teknik incelemesine](https://www.microsoft.com/download/details.aspx?id=36391) bakın.

### <a name="plan-rollback"></a>Geri alma planı

Otomatik kullanıcı sağlama uygulaması üretim ortamında istenildiği gibi çalışamazsa, aşağıdaki geri alma adımları bilinen iyi duruma geri dönmenize yardımcı olabilir:

1. Etkilenen kullanıcılar ve/veya gruplar üzerinde hangi yanlış işlemlerin gerçekleştiğini belirlemek için [sağlama özet raporunu](../app-provisioning/check-status-user-account-provisioning.md) ve sağlama [günlüklerini](../app-provisioning/check-status-user-account-provisioning.md#provisioning-logs-preview) gözden geçirin.

1. Etkilenen kullanıcıların ve/veya grupların bilinen son iyi durumunu belirlemek için sağlama denetim günlüklerini kullanın. Ayrıca kaynak sistemleri (Azure AD veya AD) gözden geçirin.

1. Bilinen son iyi durum değerlerini kullanarak doğrudan uygulamada etkilenen kullanıcıları ve/veya grupları güncelleştirmek için uygulama sahibiyle birlikte çalışın.

## <a name="deploy-automatic-user-provisioning-service"></a>Otomatik kullanıcı sağlama hizmetini dağıtma

Çözüm gereksinimlerinize uygun adımları seçin.

### <a name="prepare-for-the-initial-cycle"></a>İlk döngüye hazırlanın

Azure AD sağlama hizmeti ilk kez çalıştığında, kaynak sisteme ve hedef sistemlere karşı ilk döngü, her hedef sistem için tüm kullanıcı nesnelerinin anlık görüntüsünü oluşturur.

Bir uygulama için otomatik sağlama etkinleştirirken, ilk döngü 20 dakika ile birkaç saat arasında sürebilir. Süre, Azure REKLAM dizininin boyutuna ve sağlama kapsamındaki kullanıcı sayısına bağlıdır. Sağlama [performansını nasıl artırın](../app-provisioning/application-provisioning-when-will-provisioning-finish.md)bkz.

Sağlama hizmeti, sonraki artımlı döngülerin performansını artırarak, ilk döngüden sonra her iki sistem durumunu depolar.

### <a name="configure-automatic-user-provisioning"></a>Otomatik kullanıcı sağlamayı yapılandırma

Azure [portalını,](https://portal.azure.com/) otomatik kullanıcı hesabı sağlama ve destekleyen uygulamalar için sağlamanın kaldırılmasını yönetmek için kullanın. Bir uygulamaya [otomatik sağlamayı nasıl ayarlıyorum adımlarını izleyin?](../app-provisioning/user-provisioning.md)

Azure AD kullanıcı sağlama hizmeti, [Microsoft Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)kullanılarak da yapılandırılabilir ve yönetilebilir.

## <a name="manage-automatic-user-provisioning"></a>Otomatik kullanıcı sağlamayı yönetme

Artık dağıttığınız için çözümü yönetmeniz gerekir.

### <a name="monitor-user-provisioning-operation-health"></a>Kullanıcı sağlama işleminin sağlığını izleme

Başarılı bir [ilk döngüden](../app-provisioning/user-provisioning.md)sonra, Azure AD sağlama hizmeti, aşağıdaki olaylardan biri meydana gelene kadar her uygulamaya özgü aralıklarla, süresiz olarak artımlı güncelleştirmeler çalıştırır:

* Hizmet el ile durdurulur ve [Azure portalı](https://portal.azure.com/)veya uygun Microsoft [Graph API](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview) komutu kullanılarak yeni bir ilk döngü tetiklenir.

* Yeni bir ilk döngü öznitelik eşlemeleri veya kapsam filtreleri bir değişiklik tarafından tetiklenir.

* Sağlama işlemi yüksek hata oranı nedeniyle karantinaya girer ve otomatik olarak devre dışı bırakıldığında dört haftadan uzun süre karantinada kalır.

Bu olayları ve sağlama hizmeti tarafından gerçekleştirilen diğer tüm etkinlikleri gözden geçirmek için Azure AD [sağlama günlüklerine](../reports-monitoring/concept-provisioning-logs.md?context=azure/active-directory/manage-apps/context/manage-apps-context)bakın.

Sağlama döngülerinin ne kadar sürdüğünü anlamak ve sağlama işinin ilerlemesini izlemek [için, kullanıcı sağlama durumunu denetleyebilirsiniz.](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md)

### <a name="gain-insights-from-reports"></a>Raporlardan öngörüler elde edin

Azure AD, denetim [günlükleri](../app-provisioning/application-provisioning-when-will-provisioning-finish-specific-user.md) ve raporlar aracılığıyla kuruluşunuzun kullanıcı sağlama kullanımı ve operasyonel durumu hakkında ek bilgiler sağlayabilir.

Yöneticiler, sağlama işinin operasyonel durumunu izlemek için sağlama özet raporunu kontrol etmelidir. Sağlama hizmeti tarafından gerçekleştirilen tüm etkinlikler Azure AD denetim günlüklerine kaydedilir. Bkz. [Öğretici: Otomatik kullanıcı hesabı sağlama hakkında raporlama.](../app-provisioning/check-status-user-account-provisioning.md)

Bu raporların sahipliğini üstlenmenizi ve kuruluşunuzun gereksinimlerini karşılayan bir cadence üzerinde kullanmanızı öneririz. Azure AD, denetim verilerinin çoğunu 30 gün boyunca saklar.

### <a name="troubleshoot"></a>Sorun giderme

Sağlama sırasında ortaya çıkabilecek sorunları gidermek için aşağıdaki bağlantılara bakın:

* [Kullanıcı sağlamayı azure AD Galerisi uygulamasına yapılandırma sorunu](../app-provisioning/application-provisioning-config-problem.md)

* [Bir uygulamayı sağlamak için şirket içi Active Directory'nizden Azure AD'ye bir özniteliği senkronize etme](../app-provisioning/user-provisioning-sync-attributes-for-mapping.md)

* [Azure AD Galerisi uygulamasına kullanıcı sağlama süresi birkaç saat veya daha uzun sürüyor](../app-provisioning/application-provisioning-when-will-provisioning-finish.md)

* [Kullanıcı sağlamayı Azure Etkin Dizin Galerisi uygulamasına yapılandırırken yönetici kimlik bilgilerini kaydetme sorunu](../app-provisioning/application-provisioning-config-problem-storage-limit.md)

* [Azure AD Galerisi uygulamasında hiçbir kullanıcı sağlanmadı](../app-provisioning/application-provisioning-config-problem-no-users-provisioned.md)

* [Azure AD Galerisi uygulamasına yanlış kullanıcı kümesi sağlanıyor](../app-provisioning/application-provisioning-config-problem-wrong-users-provisioned.md)

### <a name="helpful-documentation"></a>Yararlı belgeler

* [Öznitelik eşlemeleri için ifadeler yazma](../app-provisioning/functions-for-customizing-application-data.md)

* [Azure AD senkronizasyon API'ye genel bakış](https://developer.microsoft.com/graph/docs/api-reference/beta/resources/synchronization-overview)

* [Kapsam dışına çıkan kullanıcı hesaplarının silinmesini atlama](skip-out-of-scope-deletions.md)

* [Azure AD Connect Provisioning Aracısı: Sürüm sürüm geçmişi](provisioning-agent-release-version-history.md)

#### <a name="resources"></a>Kaynaklar

* [Ürün geri bildirimi sağlama](https://feedback.azure.com/forums/169401-azure-active-directory)

* [Azure AD ile yeniliklerden haberdar olun](https://azure.microsoft.com/updates/?product=active-directory)

* [Yığın taşması Azure AD forumu](https://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="next-steps"></a>Sonraki adımlar
* [Otomatik Kullanıcı Sağlama'yı Yapılandır](../app-provisioning/configure-automatic-user-provisioning-portal.md)

* [Microsoft Graph API'yi kullanarak sağlama yapılandırmanızı dışa aktarma veya alma](../app-provisioning/export-import-provisioning-configuration.md)

* [Azure Etkin dizininde öznitelik eşlemeleri için ifadeler yazma](../app-provisioning/functions-for-customizing-application-data.md)
