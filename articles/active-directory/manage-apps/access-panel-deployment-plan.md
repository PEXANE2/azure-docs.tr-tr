---
title: Azure Active Directory uygulamalarımın dağıtımını planlayın
description: Uygulamalarımı Azure Active Directory dağıtma Kılavuzu
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.topic: conceptual
ms.date: 09/27/2019
ms.author: kenwith
ms.openlocfilehash: b7ddbff2643086f1875ca190b67f521edb115c3e
ms.sourcegitcommit: 62e1884457b64fd798da8ada59dbf623ef27fe97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88930545"
---
# <a name="plan-an-azure-active-directory-my-apps-deployment"></a>Azure Active Directory uygulamalarımın dağıtımını planlayın

Azure Active Directory (Azure AD) Uygulamalarım, destek maliyetlerini azaltmaya, üretkenliği ve güvenliği artırmaya ve Kullanıcı azalmasını azaltmaya yardımcı olan Web tabanlı bir portaldır. Sistem sisteme eriştiğinizde izleyen ayrıntılı raporlama içerir ve yöneticilere kötüye kullanımı veya kötüye kullanımı bildirir.

Azure AD My Apps 'i kullanarak şunları yapabilirsiniz:

* Tüm şirketlerinden uygulamalar gibi Azure AD bağlantılı kaynaklarına keşif ve erişme
* Yeni uygulamalara ve gruplara erişim isteme
* Başkaları için bu kaynaklara erişimi yönetin
* Self servis parola sıfırlama ve Azure Multi-Factor Authentication ayarlarını yönetme
* Cihazlarını yönetme

Ayrıca yöneticilerin yönetmesine izin verir:

* Hizmet koşulları
* Kuruluşlar
* Erişim gözden geçirmeleri


## <a name="benefits-of-azure-ad-my-apps-integration"></a>Azure AD My Apps tümleştirmesinin avantajları

Azure AD My Apps, işletmelerden aşağıdaki yollarla faydalanır:

**Sezgisel kullanıcı deneyimi sağlar**: uygulamalarım size Azure çoklu oturum açma (SSO) ile bağlantılı tüm uygulamalarınız için tek bir platform sağlar. Var olan ayarları ve Grup Yönetimi ve self servis parola sıfırlama gibi yeni özellikleri, bunlar eklendikçe bulmak için birleştirilmiş bir portala sahipsiniz. Sezgisel deneyim, kullanıcıların daha hızlı çalışmasına ve daha üretken olmasına karşın daha üretken olmasına olanak sağlar.

**Üretkenliği artırır**: uygulamamdaki tüm kullanıcı uygulamalarının SSO 'su etkinleştirilmiştir. Kurumsal uygulamalarda SSO 'yu etkinleştirme ve Microsoft 365, ek oturum açma istemlerini azaltarak veya ortadan kaldırarak üstün bir oturum açma deneyimi oluşturur. Uygulamalarım self servis ve dinamik üyelik kullanır ve kimlik sisteminizin genel güvenliğini geliştirir. Bu, doğru kişilerin uygulamalara erişimi yönetmesini sağlayarak bunu yapar. Uygulamalarım hızlı bir şekilde kaynakları bulmanıza ve iş görevlerine devam etmeniz için tutarlı bir giriş sayfası görevi görür.

**Maliyeti yönetir**: Azure AD Ile uygulamalarımı etkinleştirmek, şirket içi altyapılarla ilgili yardımcı olabilir. Uygulamalarınızın tümünü bulmak, kaynaklara erişim istemek ve hesapları yönetmek için tutarlı bir portal sunarak destek maliyetlerini azaltır.

**Esneklik ve güvenliği artırır**: Uygulamalarım, bir bulut platformunun sağladığı güvenlik ve esnekliğe erişmenizi sağlar. Yöneticiler ayarları kolayca uygulamalar ve kaynaklarla değiştirebilir ve kullanıcıları etkilemeden yeni güvenlik gereksinimlerine uyum sağlayabilir.

**Sağlam denetim ve kullanım Izlemeyi sağlar**: tüm Kullanıcı özellikleri için denetim ve kullanım izleme, kullanıcıların kaynaklarını ne zaman kullandığını bilmenize ve güvenliği değerlendirebilmenizi sağlamanıza olanak tanır.

### <a name="licensing-considerations"></a>Lisanslama konuları

Uygulamalarım ücretsizdir ve temel düzeyde kullanılacak bir lisans gerektirmez. Ancak, dizininizdeki nesne sayısı ve dağıtmak istediğiniz ek özellikler ek lisanslar gerektirebilir. Lisanslama gereksinimlerine sahip bazı yaygın Azure AD senaryoları aşağıdaki güvenlik özelliklerini içerir:

* [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works)

* [Grup tabanlı üyelik](https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups)

* [Self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)

* [Azure Active Directory Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

[Azure AD için tam lisanslama kılavuzuna](https://azure.microsoft.com/pricing/details/active-directory/)bakın.

### <a name="prerequisites-for-deploying-azure-ad-my-apps"></a>Azure AD uygulamalarımı dağıtmaya yönelik önkoşullar

Bu projeye başlamadan önce aşağıdaki önkoşulları doldurun:

* [Uygulama SSO 'SU tümleştirme](https://docs.microsoft.com/azure/active-directory/manage-apps/plan-sso-deployment)

* [Azure AD Kullanıcı ve grup altyapısını yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups)

## <a name="plan-azure-ad-my-apps-deployment"></a>Azure AD My Apps dağıtımını planlayın

Aşağıdaki tabloda, uygulamalar dağıtımı için anahtar kullanım durumları özetlenmektedir:

| Alan| Açıklama |
| - | - |
| Access| Şirket ağı içindeki şirket ve kişisel cihazlardan uygulama portalından erişilebilir. |
|Access | Uygulama portalından şirket ağı dışındaki şirket aygıtlarından erişilebilir. |
| Denetim| Kullanım verileri en az 29 günde bir şirket sistemine indirilir. |
| İdare| Azure AD bağlantılı uygulamalar ve gruplar için Kullanıcı atamalarının yaşam döngüsü tanımlanmıştır ve izlenir. |
| Güvenlik| Kaynaklara erişim, Kullanıcı ve Grup atamaları aracılığıyla denetlenir. Yalnızca yetkili kullanıcılar, kaynak erişimini yönetebilir. |
| Performans| Erişim atama yayma zaman çizelgeleri belgelenmiştir ve izlenir. |
| Kullanıcı Deneyimi| Kullanıcılar, uygulama olanaklarımı ve bunların nasıl kullanılacağını algılar.|
| Kullanıcı Deneyimi| Kullanıcılar, uygulamalarına ve gruplarına erişimleri yönetebilir.|
| Kullanıcı Deneyimi| Kullanıcılar hesaplarını yönetebilir. |
| Kullanıcı Deneyimi| Kullanıcılar tarayıcı uyumluluğuna duyarlı. |
| Destek| Kullanıcılar uygulamalarım sorunlarıyla ilgili destek bulabilir. |

### <a name="best-practices-for-deploying-azure-ad-my-apps"></a>Azure AD uygulamalarımı dağıtmaya yönelik en iyi uygulamalar

Uygulamalarımın işlevselliği, yavaş şekilde etkinleştirilebilir. Aşağıdaki dağıtım sırasını öneriyoruz:

1. Uygulamalarım
   * Uygulama başlatıcısı
   * Self Servis uygulama yönetimi
   * Microsoft 365 tümleştirme

1. Self Servis uygulama bulma
   * Self servis parola sıfırlama
   * Multi-Factor Authentication ayarları
   * Cihaz yönetimi
   * Kullanım koşulları
   * Kuruluşları yönetme

1. Gruplarım
   * Self servis grup yönetimi
1. Erişim gözden geçirmeleri
   * Erişim gözden geçirmesi yönetimi

Uygulamalarım ' dan itibaren, kullanıcılara kaynaklara erişmek için ortak bir yer olarak kullanıcıları portala tanıtır. Self Servis uygulama bulmanın eklenmesi uygulamalarım deneyimiyle oluşturulur. Gruplarım ve erişim İncelemeleri self servis yeteneklerini oluşturur.

### <a name="plan-configurations-for-azure-my-apps"></a>Azure Uygulamalarım için yapılandırma planlaması

Aşağıdaki tabloda birçok önemli uygulama yapılandırması ve kullanabileceğiniz tipik değerler listelenmiştir:

| Yapılandırma| Tipik değerler |
| - | - |
| Pilot gruplarını belirleme| Kullanılacak Azure AD güvenlik grubunu ve tüm pilot üyelerinin grubun bir parçası olduğundan emin olun. |
| Üretime etkinleştirilecek grup veya grupları belirleme.| Kullanılacak Azure AD güvenlik gruplarını veya Azure AD ile eşitlenen Active Directory gruplarını belirler. Tüm pilot üyelerinin grubun bir parçası olduğundan emin olun. |
| Kullanıcıların SSO 'yu belirli türlerde uygulamalara kullanmasına izin ver| Federasyon SSO, OAuth, parola SSO, uygulama proxy 'Si |
| Kullanıcıların Self servis parola sıfırlama kullanmasına izin ver | Evet |
| Kullanıcıların Multi-Factor Authentication kullanmasına izin ver| Evet |
| Kullanıcıların belirli Grup türleri için Self Servis Grup yönetimini kullanmasına izin ver| Güvenlik grupları, Microsoft 365 grupları |
| Kullanıcıların Self Servis uygulama yönetimini kullanmasına izin ver| Evet |
| Kullanıcıların erişim gözden geçirmeleri kullanmasına izin ver| Evet |

### <a name="plan-consent-strategy"></a>Onay stratejisini planlayın

Kullanıcıların veya yöneticilerin herhangi bir uygulamanın kullanım koşullarını ve gizlilik ilkelerini kabul etmelidir. Mümkünse, iş kurallarınız verildiğinde kullanıcılara daha iyi bir deneyim sunan yönetici onayı ' nı kullanın.

Yönetici onayını kullanmak için kuruluşun genel yöneticisi olmanız ve uygulamalar şunlardan biri olmalıdır:

* Kuruluşunuzda kayıtlı

* Başka bir Azure AD kuruluşunda kayıtlı ve daha önce en az bir kullanıcı tarafından onaylanan

Daha fazla bilgi için bkz. [Azure Active Directory bir uygulamaya son kullanıcıların izin vermesini yapılandırma](configure-user-consent.md).

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlara katılın

Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuç ve sorumlulukların eşleşmeyen beklentileri nedeniyle bunu yapılır. Bu sınırları önlemek için, doğru proje katılımcıları ile ilgileniyorsanız ve projedeki paydaş rollerinin iyi anlaşıldığından [emin olun](../fundamentals/active-directory-deployment-plans.md) .

### <a name="plan-communications"></a>İletişimi planlama

İletişim, her yeni hizmetin başarısı için önemlidir. Kullanıcılarınıza deneyiminizin nasıl ve ne zaman değiştirileceğini ve gerektiğinde nasıl destek kazanabileceğini önceden bilgilendirirsiniz.

Uygulamalarım genellikle kullanıcı sorunları oluşturmasa da, hazırlanması önemlidir. Başlamadan önce destek personelinize yönelik tüm kaynakların kılavuzlarını ve bir listesini oluşturun.

#### <a name="communications-templates"></a>İletişim şablonları

Microsoft, [e-postalar için özelleştirilebilen şablonlar ve](https://aka.ms/APTemplates) Uygulamalarım için diğer iletişimler sağlar. Bu varlıkları, kurumsal külgereksinimlerinize uygun şekilde diğer iletişim kanallarında kullanmak üzere uyarlayabilirsiniz.

## <a name="plan-your-sso-configuration"></a>SSO yapılandırmanızı planlayın

Bir Kullanıcı bir uygulamada oturum açtığında, bir kimlik doğrulama işleminden geçer ve kim olduklarını kanıtlamaları gerekir. SSO olmadan uygulamada bir parola depolanır ve kullanıcının bu parolayı bilmeleri gerekir. SSO ile, kullanıcıların kimlik bilgileri uygulamaya geçirilir, dolayısıyla her bir uygulama için parolaları yeniden girmeye gerek kalmaz.

Uygulamalarım üzerinde uygulamaları başlatmak için SSO etkinleştirilmelidir. Azure AD birden çok SSO seçeneğini destekler. Daha fazla bilgi edinmek için bkz. [Azure AD 'de çoklu oturum açma seçenekleri](sso-options.md).

> [!NOTE]
> Azure AD 'yi bir uygulama için kimlik sağlayıcısı olarak kullanma hakkında daha fazla bilgi edinmek için bkz. [uygulama yönetiminde hızlı başlangıç serisi](view-applications-portal.md).

Uygulamalarım sayfasında en iyi deneyim için, Federasyon SSO için kullanılabilen bulut uygulamalarının tümleştirmesiyle başlayın. Federasyon SSO, kullanıcıların, uygulama yüzeyleri üzerinde tutarlı bir tıklama deneyimine sahip olmasını sağlar ve yapılandırma denetiminde daha sağlam olma eğilimindedir.

Bir uygulama tarafından desteklenen parola tabanlı SSO ve ADFS yerine, Azure AD ile Federasyon SSO (OpenID Connect/SAML) kullanın.

SaaS uygulamalarınızı dağıtma ve yapılandırma hakkında daha fazla bilgi için bkz. [SaaS SSO dağıtım planı](https://aka.ms/deploymentplans/sso).

#### <a name="plan-to-deploy-the-my-apps-browser-extension"></a>Uygulamalarım tarayıcı uzantısını dağıtmayı planlayın

Kullanıcılar parola tabanlı SSO uygulamalarında oturum açtıklarında, uygulamaların güvenli oturum açma uzantısını yükleyip kullanmaları gerekir. Uzantı, parolayı uygulamanın oturum açma formuna ileten bir betiği yürütür. Kullanıcılardan, parola tabanlı SSO uygulamasını ilk kez başlattığında uzantıyı yüklemeleri istenir. Bu belgede uzantı hakkında daha fazla bilgi, [uygulamalarımı tarayıcı uzantısından yükleme](access-panel-extension-problem-installing.md)konusunda bulunabilir.

Parola tabanlı SSO uygulamalarını tümleştirmeniz gerekiyorsa, uzantıyı [desteklenen tarayıcılarla](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bir ölçekte dağıtmak için bir mekanizma tanımlamanız gerekir. Seçeneklere şunlar dahildir:

* [Internet Explorer için grup ilkesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)

* [Internet Explorer için Configuration Manager](https://docs.microsoft.com/configmgr/core/clients/deploy/deploy-clients-to-windows-computers)

* [Chrome, Firefox, Microsoft Edge veya IE için Kullanıcı odaklı indirme ve yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Parola tabanlı SSO uygulamaları kullanmayan kullanıcılar da uzantısından faydalanır. Bu avantajlar, arama çubuğundan herhangi bir uygulamayı başlatma, son kullanılan uygulamalara erişimi bulma ve uygulamalarım sayfasına bir bağlantı olma imkanını içerir.

#### <a name="plan-for-mobile-access"></a>Mobil erişim planlaması

Intune ilkesi (Microsoft Edge veya Intune Managed Browser) ile korunan bir tarayıcı, parola tabanlı SSO uygulamaları başlatan mobil kullanıcılar için gereklidir. İlke korumalı bir tarayıcı, uygulama için kaydedilen parolanın aktarılmasına izin vermez. Microsoft Edge veya Managed Browser, bir dizi Web veri koruma özelliği sağlar. İOS ve Android cihazlarda kurumsal senaryolar için Microsoft Edge 'i de kullanabilirsiniz. Microsoft Edge, Intune Managed Browser ile aynı yönetim senaryolarını destekler ve Kullanıcı deneyimini geliştirir. Daha fazla bilgi: [Microsoft Intune İlkeyle korunan bir tarayıcı kullanarak Web erişimini yönetme](https://docs.microsoft.com/intune/app-configuration-managed-browser).

## <a name="plan-your-my-apps-deployment"></a>Uygulamalarım dağıtımınızı planlayın

Uygulamalarımın temeli, ' de kullanıcıların erişebileceği uygulama başlatıcısı portalıdır [https://myapps.microsoft.com](https://myapps.microsoft.com/) . Uygulamalarım sayfası, kullanıcılara işlerini başlatmak ve gerekli uygulamalarına ulaşmak için tek bir yer sunar. Burada, kullanıcılar çoklu oturum açma erişimine sahip oldukları tüm uygulamaların bir listesini bulur. 

> [!NOTE]
> Aynı uygulamalar Microsoft 365 App Launcher içinde gösterilir.

Uygulamalarım başlatıcısı için uygulama ekleyeceğinize ilişkin sırayı planlayın ve bunları her zaman aşamalı veya hepsi için bir kez mi yapacağınıza karar verin. Bunu yapmak için, her bir uygulama için kimlik doğrulama türünü ve var olan SSO tümleştirmelerini listelemek üzere bir uygulama envanteri oluşturun.

#### <a name="add-applications-to-the-my-apps-panel"></a>Uygulamalarım paneline uygulama ekleme

Tüm Azure AD SSO özellikli uygulamaları My Apps Launcher öğesine eklenebilir. Diğer uygulamalar, bağlantılı SSO seçeneği kullanılarak eklenir. Mevcut Web uygulamanızın URL 'sine bağlanan bir uygulama kutucuğu yapılandırabilirsiniz. Bağlantılı SSO, tüm uygulamaları Azure AD SSO 'ya geçirmeden kullanıcıları uygulama portalına yönlendirmeye başlayabilmeniz için izin verir. Kullanıcıların deneyimini kesintiye uğratmadan, Azure AD SSO ile yapılandırılmış uygulamalara giderek geçebilirsiniz.

#### <a name="use-my-apps-collections"></a>Uygulama koleksiyonlarımı kullan

Varsayılan olarak, tüm uygulamalar tek bir sayfada birlikte listelenir. Ancak, koleksiyonları kullanarak ilgili uygulamaları gruplandırabilir ve bunları ayrı bir sekmede sunabilir, daha kolay bulunmalarını sağlayabilirsiniz. Örneğin, koleksiyonları, belirli iş rolleri, görevler, projeler vb. için mantıksal uygulama gruplandırmaları oluşturmak üzere kullanabilirsiniz. Bilgi için bkz. [uygulama koleksiyonlarımı kullanma](access-panel-collections.md). 

#### <a name="plan-whether-to-use-my-apps-or-an-existing-portal"></a>Uygulamalarımı mı yoksa mevcut bir portalı mı kullanacağınızı planlayın

Kullanıcılarınız uygulamalarım dışında bir uygulama veya portala zaten sahip olabilir. Bu durumda, her iki portalı da destekleme veya yalnızca bir tane kullanma seçeneklerinden birini belirleyin.

Mevcut bir portal, kullanıcılar için bir başlangıç noktası olarak zaten kullanılıyorsa, Kullanıcı erişim URL 'Lerini kullanarak Apps işlevsellikten tümleştirilebilir. Kullanıcı erişimi URL 'Leri, uygulamalar portalında bulunan uygulamalara doğrudan bağlantılar olarak çalışır. Bu URL 'Ler var olan herhangi bir Web sitesi içine gömülebilir. Bir Kullanıcı bağlantıyı seçtiğinde uygulama Uygulamam portalından uygulamayı açar.

Kullanıcı erişimi URL 'SI özelliğini, Azure portal uygulamanın **Özellikler** alanında bulabilirsiniz.

![Uygulamalar panelinin ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-user-access-url.png)


## <a name="plan-self-service-application-discovery-and-access"></a>Self Servis uygulama bulmayı ve erişimini planlayın

Bir temel uygulama kümesi bir kullanıcının uygulamalarım sayfasına dağıtıldıktan sonra self servis uygulama yönetimi özelliklerini etkinleştirmelisiniz. Self Servis uygulama bulma, kullanıcıların şunları yapmasına olanak sağlar:

* Uygulamalarıma eklenecek yeni uygulamalar bulun. 
* Kurulum sırasında ihtiyacı olmayan isteğe bağlı uygulamalar ekleyin.

Onay iş akışları, uygulamalara erişim için açık onay için kullanılabilir. Onaylayanlar olan kullanıcılar, uygulamaya erişim için bekleyen istek olduğunda uygulamalar portalı içinde bildirim alır.

## <a name="plan-self-service-group-membership"></a>Self servis grup üyeliklerini planlayın 

Kullanıcıların Azure AD 'de kendi güvenlik gruplarını veya Microsoft 365 gruplarını oluşturmasını ve yönetmesini sağlayabilirsiniz. Grubun sahibi, üyelik isteklerini onaylayabilir veya reddedebilir ve grup üyeliği denetimini devredebilir. Self Servis Grup yönetimi özellikleri posta etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

Self Servis Grup üyeliğini planlamak için, kuruluşunuzdaki tüm kullanıcıların grup oluşturma ve yönetme izni mi yoksa yalnızca bir kullanıcı alt kümesi mi olacağını saptayın. Kullanıcıların bir alt kümesine izin verirseniz, bu kişilerin eklendiği bir grup ayarlamanız gerekir. Bu senaryoları etkinleştirme hakkında daha fazla bilgi için, bkz. [Azure Active Directory self servis grup yönetimini ayarlama](../users-groups-roles/groups-self-service-management.md) .

## <a name="plan-reporting-and-auditing"></a>Raporlama ve denetim planlaması

Azure AD, [Teknik ve iş öngörüleri sunan raporlar](../reports-monitoring/overview-reports.md)sağlar. Bu raporların sahipliğini varsaymak ve bunları düzenli olarak tüketmek için iş ve teknik uygulama sahipleriyle birlikte çalışın. Aşağıdaki tabloda tipik raporlama senaryolarının bazı örnekleri verilmiştir.

| Örnek | Riski yönetin| Üretkenliği artırın| İdare ve uyum |
|  - |- | - | - |
| Rapor türleri|  Uygulama izinleri ve kullanımı| Hesap sağlama etkinliği| Uygulamalara kimlerin eriştiğini gözden geçirme |
| Olası eylemler| Denetim erişimi; izinleri iptal et| Sağlama hatalarını düzelt| Erişimi iptal et |

Azure AD, en çok denetim verilerini 30 gün boyunca tutar. Veriler, çözümleme sistemlerinize indirmeniz için Azure yönetim portalı veya API aracılığıyla kullanılabilir.

#### <a name="auditing"></a>Denetim

Uygulama erişimi için denetim günlükleri 30 gün boyunca kullanılabilir. Kurumunuz içindeki güvenlik denetimi daha uzun bekletme gerektiriyorsa, günlüklerin splunk veya Arctıma gibi bir güvenlik bilgileri olay ve yönetim (SıEM) aracına aktarılması gerekir.

Denetim, raporlama ve olağanüstü durum kurtarma yedeklemeleri için gereken indirme sıklığını, hedef sistemin ne olduğunu ve her yedeklemeyi yönetmekten sorumlu olanları belgeleyin. Ayrı denetim ve raporlama yedeklemeleri gerekmez. Olağanüstü durum kurtarma yedeklemeniz ayrı bir varlık olmalıdır.

## <a name="deploy-applications-to-users-my-apps-panel"></a>Uygulamaları kullanıcıların uygulamalarım paneline dağıtma

Bir uygulama SSO için yapılandırıldıktan sonra gruplara erişim atanır. Atanan gruplardaki kullanıcıların erişimi olur ve uygulamayı Uygulamalarım ve Microsoft 365 uygulama başlatıcısı 'nda görür.

Bkz. [Active Directory bir uygulamaya Kullanıcı ve Grup atama](methods-for-assigning-users-and-groups.md).

Testi veya dağıtımı sırasında, grupları eklemek istiyorsanız ancak uygulamaların uygulamamda görüntülemesine izin vermediyse, bkz. [Azure Active Directory kullanıcının deneyiminden bir uygulamayı gizleme](hide-application-from-user-portal.md).

### <a name="deploy-microsoft-microsoft-365-applications-to-my-apps"></a>Microsoft Microsoft 365 uygulamalarını uygulamalarıma dağıtma

Microsoft 365 uygulamalar için, kullanıcılar, bunlara atanan lisanslardan birine göre Office 'in bir kopyasını alırlar. Office uygulamalarına erişim için bir önkoşul, kullanıcılara Office uygulamalarına bağlı doğru lisansları atamalıdır. Bir kullanıcıya lisans atadığınızda, bu kullanıcılar kendi uygulamalarım sayfasında ve Microsoft 365 App Launcher ' de lisansla ilişkili uygulamaları otomatik olarak görürler.

Kullanıcılardan bir dizi Office uygulamasını gizlemek istiyorsanız uygulamalar portalından uygulama gizleme seçeneği vardır, ancak Microsoft 365 portalından erişime izin vermeye devam edebilirsiniz. Daha fazla bilgi: [bir uygulamayı kullanıcının Azure Active Directory deneyiminden gizleyin](hide-application-from-user-portal.md).

### <a name="deploy-application-self-service-capabilities"></a>Uygulama self servis yeteneklerini dağıtma

Self Servis uygulama erişimi, kullanıcıların uygulamalara erişimi kendi kendine bulmasına ve istemesine izin verir. Kullanıcılar, her erişim isteğinde bulundukları her seferinde bir BT grubuna geçmeden ihtiyaç duydukları uygulamalara erişme özgürlüğü sunar. Bir Kullanıcı erişim istediğinde ve bir uygulama sahibi tarafından otomatik olarak veya el ile onaylandığında, arka uçtaki bir gruba eklenir. Raporlama, erişim isteğinde bulunan, onaylanan veya kaldırılan kişiler üzerinde etkinleştirilir ve atanan rollerin yönetilmesi üzerinde denetim sağlar.

Uygulama erişim isteklerinin onayını iş onaylayanlara atayabilirsiniz. İş onaylayan, uygulama erişim parolalarını iş onaylayanın uygulamalar sayfasından ayarlayabilir.

Daha fazla bilgi: [self servis uygulama erişimini kullanma](access-panel-manage-self-service-access.md).

## <a name="validate-your-deployment"></a>Dağıtımınızı doğrulama

Uygulamalarımın dağıtımının tamamen sınanmış olduğundan ve geri alma planının yapıldığından emin olun.

Aşağıdaki testler, şirkete ait cihazlar ve kişisel cihazlarla birlikte gerçekleştirilmelidir. Bu test çalışmaları, iş kullanım örneklerini de yansıtmalıdır. Aşağıda, bu belgedeki örnek iş gereksinimlerine ve tipik teknik senaryolara göre bazı durumlar yer alır. Gereksinimlerinize özgü diğerlerini ekleyin.

#### <a name="application-sso-access-test-case-examples"></a>Uygulama SSO 'SU erişim test çalışması örnekleri:


| İş örneği| Beklenen sonuç |
| - | -|
| Kullanıcı My Apps portalında oturum açar| Kullanıcı oturum açabilir ve uygulamalarını görebilir |
| Kullanıcı bir Federasyon SSO uygulaması başlatıyor| Kullanıcı otomatik olarak uygulamada oturum açtı |
| Kullanıcı ilk kez parola SSO uygulaması başlatıyor| Kullanıcının uygulama Uzantımı yüklemesi gerekiyor |
| Kullanıcı bir parola SSO uygulamasını sonraki bir zamanda başlatır| Kullanıcı otomatik olarak uygulamada oturum açtı |
| Kullanıcı Microsoft 365 portalından bir uygulama başlatıyor| Kullanıcı otomatik olarak uygulamada oturum açtı |
| Kullanıcı Managed Browser bir uygulamayı başlatır| Kullanıcı otomatik olarak uygulamada oturum açtı |


#### <a name="application-self-service-capabilities-test-case-examples"></a>Uygulama self servis yetenekleri test çalışması örnekleri


| İş örneği| Beklenen sonuç |
| - | - |
| Kullanıcı, uygulama üyeliğini yönetebilir| Kullanıcı, uygulamaya erişimi olan üyeleri ekleyebilir/kaldırabilir |
| Kullanıcı uygulamayı düzenleyebilir| Kullanıcı, parola SSO uygulamaları için uygulamanın açıklamasını ve kimlik bilgilerini düzenleyebilir |

### <a name="rollback-steps"></a>Geri alma adımları

Dağıtımınız planlanmazsa ne yapılacağını planlamanız önemlidir. Dağıtım sırasında SSO yapılandırması başarısız olursa, [SSO sorunlarını nasıl giderebileceğinizi](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sso) ve kullanıcılarınıza etkisini nasıl azaltacağınızı anlamanız gerekir. Olağanüstü durumlarda, [SSO 'yu geri almanız](../manage-apps/plan-sso-deployment.md#rollback-process)gerekebilir.


## <a name="manage-your-implementation"></a>Uygulamanızı yönetme

Azure Active Directory içinde gerekli bir görevi gerçekleştirmek için en az ayrıcalıklı rolü kullanmanız gerekir. [Kullanılabilir farklı rolleri gözden geçirin](../users-groups-roles/directory-assign-admin-roles.md) ve bu uygulamaya yönelik her kişi için ihtiyaçlarınızı çözümlemek üzere doğru olanı seçin. Dağıtım tamamlandıktan sonra bazı rollerin geçici olarak uygulanması ve kaldırılması gerekebilir.

| Kişilikler| Roller| Azure AD rolü  |
| - | -| -|
| Yardım Masası Yöneticisi| Katman 1 desteği| Hiçbiri |
| Kimlik Yöneticisi| Sorunları Azure AD ' i etkileyen yapılandırma ve hata ayıklama| Genel yönetici |
| Uygulama Yöneticisi| Uygulamada Kullanıcı kanıtlaması, izinleri olan kullanıcılar üzerinde yapılandırma| Hiçbiri |
| Altyapı Yöneticileri| Sertifika aktarma sahibi| Genel yönetici |
| İşletme sahibi/paydaş| Uygulamada Kullanıcı kanıtlaması, izinleri olan kullanıcılar üzerinde yapılandırma| Hiçbiri |

Dizin izinleri olan kullanıcılar için ek denetim, denetim ve erişim incelemesi sağlamak üzere rollerinizi yönetmek için [Privileged Identity Management](../privileged-identity-management/pim-configure.md) kullanabilirsiniz.

### <a name="troubleshoot-my-apps-issues"></a>Uygulamalarımın sorunlarını giderme

Ortak senaryolarla destek kuruluşunuz için sorun giderme kılavuzlarını oluşturun ve bu çözüm, çözümlerinde Microsoft belgelerine işaret eder. Kuruluşunuzun kullandığı katmanlara yönelik desteği kesen kılavuzlar oluşturmak isteyebilirsiniz.

Başvuru için şu sorun giderme kılavuzlarını inceleyin:

[Görünmeyen uygulamalar](access-panel-troubleshoot-application-not-appearing.md)

[Beklenmeyen uygulamalar görünüyor](access-panel-troubleshoot-unexpected-application.md)

[Kullanıcı uygulamamda oturum açılamıyor](access-panel-troubleshoot-web-sign-in-problem.md)

[Self Servis uygulama erişimini kullanan sorunlar](access-panel-troubleshoot-self-service-access.md)

[Tarayıcı uzantısıyla ilgili sorunlar](manage-access-panel-browser-extension.md)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Multi-Factor Authentication dağıtımını planlayın](https://aka.ms/deploymentplans/mfa)
