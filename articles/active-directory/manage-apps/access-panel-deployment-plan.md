---
title: Azure Active Directory Access panel dağıtımı planlayın
description: Azure AD erişim paneli işlevlerini dağıtma Kılavuzu
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2019
ms.author: baselden
ms.reviewer: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 448af7075297c2b18df1eceaeaac34d5da762489
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69576530"
---
# <a name="plan-an-azure-active-directory-access-panel-deployment"></a>Azure Active Directory Access panel dağıtımı planlayın

Azure Active Directory Access panel, destek maliyetlerini düşürmenizi, üretkenliği ve güvenliği artırmanıza ve Kullanıcı güvenliğini azaltmanıza olanak sağlayan Web tabanlı bir portaldır. Sistem, kullanıcılar sisteme erişirken izleyen ayrıntılı raporlama içerir ve yöneticilere kötüye kullanma veya kötüye kullanma bildirir.

Azure Active Directory erişim paneli kullanıcıların şunları yapmasına olanak sağlar:

* Tüm şirketlerinin, uygulamalar gibi bağlı Azure Active Directory kaynaklarını bulur ve bunlara erişin.
* Yeni uygulamalara ve gruplara erişim isteyin veya bu kaynaklara erişimi başkaları için yönetin.
* Self servis parola sıfırlama ve Multi-Factor Authentication ayarlarını yönetin.
* Cihazlarını yönetin.

Ayrıca yöneticilerin yönetmesine izin verir:

* Hizmet şartları
* Kuruluşlar
* Erişim gözden geçirmeleri


## <a name="benefits-of-azure-active-directory-access-panel-integration"></a>Azure Active Directory Access panel tümleştirmesinin avantajları

Microsoft Azure Active Directory (Azure AD) erişim paneli, işletmelerden aşağıdaki yollarla faydalanır:

**Sezgisel kullanıcı deneyimi sağlar**: Erişim paneli 'nin etkinleştirilmesi, kullanıcılara tüm Azure çoklu oturum açma uygulamaları için tek bir başlatma Pad özelliği sağlar. Grup Yönetimi ve self servis parola sıfırlama gibi yetenekler eklendikçe, kullanıcılar bu ayarları bulmak için birleştirilmiş bir portala sahip olmaya devam eder. Sezgisel deneyim, kullanıcıların daha hızlı çalışmaya ve daha üretken olmasına olanak tanılarken, bu da daha üretken hale getirilir.

**Üretkenliği artırır**: Erişim panelindeki tüm kullanıcı uygulamalarının çoklu oturum açma (SSO) özelliği etkinleştirilmiştir. Kurumsal uygulamalar ve Office 365 genelinde çoklu oturum açmayı etkinleştirmek, mevcut kullanıcılara yönelik bir üstün oturum açma deneyimi sağlar, ek oturum açma istemlerini azaltır veya ortadan kaldırır. Erişim paneli self servis ve dinamik üyelik sağlar ve kişilerin uygulamalara erişimi yönetmesine olanak sağlayarak kimlik sisteminizin genel güvenliğini geliştirir. Erişim paneli, bir kullanıcının kaynakları hızlı bir şekilde bulması ve iş görevlerine devam etmesi için tutarlı bir giriş sayfası görevi görür.

**Maliyeti yönetir**: Azure Active Directory ile erişim panelini etkinleştirmek, şirket içi altyapılara yönelik bir engel verebilir. Kullanıcıların tüm uygulamalarını bulmak, kaynaklara erişim istemesi ve hesaplarını yönetmek için tutarlı bir portala sahip olmasını sağlayarak destek maliyetlerini azaltır.

**Esneklik ve güvenliği artırır**: Erişim paneli, bir bulut platformunun sağladığı güvenliğe ve esnekliğe erişmenizi sağlar. Yöneticiler ayarları kolayca uygulamalar ve kaynaklarla değiştirebilir ve kullanıcıları etkilemeden yeni güvenlik gereksinimlerine uyum sağlayabilir.

**Sağlam denetim ve kullanım Izlemeyi mümkün**bir şekilde sunar: Tüm Son Kullanıcı özellikleri için denetim ve kullanım izleme, kullanıcıların kaynaklarını ne zaman kullandığını ve güvenliği değerlendirmenizi sağlar.

### <a name="licensing-considerations"></a>Lisanslama konuları

Erişim paneli ücretsizdir ve temel tüm kullanıcılara kullanılmak üzere lisans gerektirmez. Ancak, dizininizdeki nesne sayısı ve dağıtmak istediğiniz özellikler ek lisanslar gerektirebilir. Ortak Azure AD senaryoları, lisans gereksinimlerine sahip olan aşağıdaki güvenlik özelliklerini içerir.

* [Azure çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works)

* [Grup tabanlı üyelik](https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups)

* [Self servis parola gereksinimi](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)

* [Kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

[Azure Active Directory için tam lisanslama kılavuzuna](https://azure.microsoft.com/pricing/details/active-directory/)bakın.

### <a name="prerequisites-for-deploying-the-azure-ad-access-panel"></a>Azure AD erişim paneli 'ni dağıtmaya yönelik önkoşullar

Bu projenin başlangıcından önce aşağıdaki önkoşulların tamamlanması gerekir.

* [Uygulama SSO tümleştirmesi](https://docs.microsoft.com/azure/active-directory/manage-apps/plan-sso-deployment)

* [Azure AD Kullanıcı ve grup altyapısı](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups)

## <a name="plan-the-azure-ad-access-panel-deployment"></a>Azure AD erişim paneli dağıtımını planlayın

Aşağıdaki tabloda, bir erişim paneli dağıtımı için anahtar kullanım durumları özetlenmektedir:

| Alan| Açıklama |
| - | - |
| Access| Şirket ağı içindeki şirket ve kişisel cihazlardan erişim paneli portalına erişilebilir. |
|Access | Şirket ağı dışındaki şirket cihazlarından erişim paneli portalına erişilebilir. |
| Denetim| Kullanım verileri en az 29 günde bir şirket sistemine indirilir. |
| İdare| Azure AD bağlantılı uygulamalar ve gruplar için Kullanıcı atamalarının yaşam döngüsü tanımlanır ve izlenir. |
| Güvenlik| Kaynaklara erişim, Kullanıcı ve Grup atamaları aracılığıyla denetlenir. Yalnızca yetkili kullanıcılar, kaynak erişimini yönetebilir. |
| Performans| Erişim atama yayma zaman çizelgeleri belgelenmiştir ve izlenir. |
| Kullanıcı deneyimi| Kullanıcılar, erişim paneli özelliklerini ve bunların nasıl kullanılacağını algılar.|
| Kullanıcı deneyimi| Kullanıcılar uygulamalara ve gruplara erişimini kendi kendilerine yönetebilir.|
| Kullanıcı deneyimi| Kullanıcılar hesaplarını yönetebilir. |
| Kullanıcı deneyimi| Kullanıcılar tarayıcı uyumluluğuna duyarlı. |
| Destek| Kullanıcılar, erişim paneli sorunları için destek bulabilir. |

### <a name="best-practices-for-deploying-the-azure-ad-access-panel"></a>Azure AD erişim paneli 'ni dağıtmaya yönelik en iyi uygulamalar

Erişim panelinin işlevselliği, yavaş şekilde etkinleştirilebilir. Aşağıdaki dağıtım sırasını öneriyoruz:

1. Uygulamalarım
   * Uygulama başlatıcısı
   * Self Servis uygulama yönetimi
   * Microsoft Office 365 tümleştirmesi

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

### <a name="plan-configurations-for-the-azure-ad-access-panel"></a>Azure AD erişim paneli için yapılandırma planlaması

Aşağıdaki tabloda birçok önemli erişim paneli yapılandırması ve kullanabileceğiniz tipik değerler listelenmiştir:

| Yapılandırma| Tipik değerler |
| - | - |
| Pilot grupları belirle| Kullanılacak Azure AD güvenlik grubunu ve tüm pilot üyelerinin grubun bir parçası olduğundan emin olun. |
| Üretime etkinleştirilecek grup veya grupları belirleme.| Kullanılacak Azure AD güvenlik gruplarını veya Azure AD ile eşitlenen AD gruplarını belirler. Tüm pilot üyelerinin grubun bir parçası olduğundan emin olun. |
| Kullanıcıların, uygulama türleri için çoklu oturum açma kullanmasına izin ver| Federasyon SSO, OAuth, parola SSO, uygulama proxy 'Si |
| Kullanıcıların Self servis parola sıfırlama kullanmasına izin ver| Evet |
| Kullanıcıların Multi-Factor Authentication kullanmasına izin ver| Evet |
| Kullanıcıların, Grup türleri için Self Servis Grup yönetimini kullanmasına izin ver| Güvenlik grupları, O365 grupları |
| Kullanıcıların Self Servis uygulama yönetimini kullanmasına izin ver| Evet |
| Kullanıcıların erişim gözden geçirmeleri kullanmasına izin ver| Evet |

### <a name="plan-consent-strategy"></a>Onay stratejisini planlayın

Kullanıcıların veya yöneticilerin herhangi bir uygulamanın kullanım koşullarını ve gizlilik ilkelerini kabul etmelidir. İş kurallarınız verildiğinde, kullanıcılara daha iyi bir deneyim sunan yönetici onayı ' nı kullanmayı tavsiye ederiz.

Yönetici onayını kullanmak için kiracının genel yöneticisi olmanız ve uygulamalar şu şekilde olmalıdır:

* Kiracınızda kayıtlı veya

* Başka bir Azure AD kiracısına kaydolmuş ve daha önce en az bir son kullanıcı tarafından onaylanan.

Daha fazla bilgi için bkz. [Azure Active Directory bir uygulamaya son kullanıcıların izin vermesini yapılandırma](configure-user-consent.md).

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlara katılın

Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuç ve sorumlulukların eşleşmeyen beklentileri nedeniyle bunu yapılır. Bu sınırları önlemek için, doğru proje katılımcıları ile ilgileniyorsanız ve projedeki paydaş rollerinin iyi anlaşıldığından [emin olun](../fundamentals/active-directory-deployment-plans.md) .

### <a name="plan-communications"></a>İletişimleri planlayın

İletişim, her yeni hizmetin başarısı için önemlidir. Kullanıcılarınıza deneyimin nasıl ve ne zaman değiştirileceğini ve gerektiğinde nasıl destek kazanabileceğini önceden bilgilendirirsiniz.

Erişim paneli genellikle kullanıcı sorunları oluşturmazsa, hazırlık yapmak önemlidir. Başlamadan önce destek personelinize yönelik tüm kaynakların kılavuzlarını ve bir listesini oluşturun.

#### <a name="communications-templates"></a>İletişim şablonları

Microsoft, [e-postalar için özelleştirilebilen şablonlar ve](https://aka.ms/APTemplates) erişim paneline yönelik diğer iletişimler sağlar. Bu varlıkları, kurumsal külgereksinimlerinize uygun şekilde diğer iletişim kanallarında kullanmak üzere uyarlayabilirsiniz.

## <a name="plan-your-sso-configuration"></a>SSO yapılandırmanızı planlayın

Bir Kullanıcı bir uygulamada oturum açtığında, bir kimlik doğrulama işleminden geçer ve kim olduklarını kanıtlamaları gerekir. Çoklu oturum açma olmadan, uygulamada depolanan bir parola ve kullanıcının bu parolayı bilmeleri gerekir. Çoklu oturum açma (SSO) kullanıcılarının kimlik bilgileri uygulamaya geçirilir, böylece her bir uygulama için parolaları yeniden girmeye gerek kalmaz.

Uygulamalarım üzerinde uygulamaları başlatmak için, uygulamalar için çoklu oturum açma (SSO) etkinleştirilmelidir.

Azure AD, [uygulamalarda çoklu oturum açmayı](what-is-single-sign-on.md)etkinleştirmek için üç farklı yolu destekler:

* **Federasyon çoklu oturum açma** 
    * Bir uygulamanın parola istemek yerine Kullanıcı kimlik doğrulaması için Azure AD 'ye yönlendirilmesini sağlar. 
    * SAML 2,0, WS-Federation veya OpenID Connect gibi protokolleri kullanan uygulamalar için desteklenir ve çoklu oturum açma 'nın zenginest modudur.

* **Parola tabanlı çoklu oturum açma** 
    * Güvenli uygulama parolası depolamayı ve bir Web tarayıcısı uzantısı ya da mobil uygulama kullanarak yeniden yürütmeyi mümkün hale getirin. 
    * Uygulama tarafından sunulan mevcut oturum açma işleminden yararlanır, ancak yöneticinin parolaları yönetmesine olanak sağlar. Kullanıcının parolayı bilmeleri için gerekli değildir.

* **Mevcut çoklu oturum açma** 
    * Azure AD 'nin, uygulama için yapılandırılmış mevcut çoklu oturum açma özelliğinden yararlanmasını sağlar.
    * Bu uygulamaların Office 365 veya Azure AD erişim paneli portallarına bağlanmasını sağlar. 
    * Uygulamalar orada başlatıldığında Azure AD 'de ek raporlamaya izin vermez. 
    * Azure uygulama proxy 'Si ve bağlantılı çoklu oturum açma modunu kullanmayı içerir.

Uygulamanın SSO modunu buradan yapılandırmayı öğrenin: [Azure Active Directory uygulamalarda çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Uygulamalarım sayfasında en iyi deneyim için, Federasyon SSO 'SU için kullanılabilen bulut uygulamalarının tümleştirilmesine başlamadan önce önerilir. Federasyon SSO, kullanıcıların, uygulama yüzeyleri üzerinde tutarlı bir tıklama deneyimine sahip olmasını sağlar ve yapılandırma denetiminde daha sağlam olma eğilimindedir.

Bir uygulama tarafından desteklenen parola tabanlı SSO ve ADFS yerine, Azure AD ile Federasyon SSO (OpenID Connect/SAML) kullanın.

SaaS uygulamalarınızı dağıtma ve yapılandırma hakkında daha fazla bilgi için bkz. [SaaS SSO dağıtım planı](https://aka.ms/deploymentplans/sso).

#### <a name="plan-to-deploy-the-my-apps-browser-extension"></a>Uygulamalarım tarayıcı uzantısını dağıtmayı planlayın

Parola tabanlı SSO uygulamaları son kullanıcılar için kullanılabilir hale getirildiklerinde, oturum açmak için uygulamalarımın güvenli oturum açma uzantısını yüklemeleri gerekir. Uzantı, parolayı uygulamanın oturum açma formuna ileten bir betiği yürütür. Kullanıcılardan, parola tabanlı SSO uygulamasını ilk kez başlattığında uzantıyı yüklemesi istenir. Uzantı hakkında daha fazla bilgi, [erişim paneli tarayıcı uzantısını yükleme](https://docs.microsoft.com/azure/active-directory/application-access-panel-extension-problem-installing?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav)konusundaki belgelerimizde bulunabilir.

Parola tabanlı SSO uygulamalarını tümleştirmeniz gerekiyorsa, uzantıyı [desteklenen tarayıcılarla](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bir ölçekte dağıtmaya yönelik bir mekanizma tanımlamanız gerekir. Şu seçenekler mevcuttur:

* [Internet Explorer için grup ilkesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)

* [Internet Explorer için System Center Configuration Manager (SCCM)](https://docs.microsoft.com/sccm/core/clients/deploy/deploy-clients-to-windows-computers)

* [Chrome, Firefox, Microsoft Edge veya IE için Kullanıcı odaklı indirme ve yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Daha fazla bilgi edinin: [Parola çoklu oturum açma yapılandırması](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery).

Parola tabanlı SSO uygulamaları kullanmayan kullanıcılar da uzantısından faydalanır. Bu avantajlar, arama çubuğundan herhangi bir uygulamayı başlatma, son kullanılan uygulamalara erişimi bulma ve uygulamalarım sayfasına bir bağlantı olma imkanını içerir.

Kullanıcının bir parola tabanlı SSO uygulamasını ilk kez başlatırken göreceği Kullanıcı şöyle olacaktır:

![Uygulamalarım tarayıcı uzantısı yüklemesi ekranının ekran görüntüsü ](media/access-panel-deployment-plan/ap-dp-sign-in.png)

#### <a name="plan-for-mobile-access"></a>Mobil erişim planlaması

Intune ilkesi (Microsoft Edge veya Intune Managed Browser) ile korunan bir tarayıcı, parola tabanlı SSO uygulamalarını başlatan mobil kullanıcılar için gereklidir. İlke korumalı bir tarayıcı, uygulama için kaydedilen parolanın aktarılmasına izin vermez. Microsoft Edge veya Managed Browser, bir dizi Web veri koruma özelliği sağlar. İOS ve Android cihazlarda kurumsal senaryolar için Microsoft Edge 'i de kullanabilirsiniz. Microsoft Edge, Intune Managed Browser ile aynı yönetim senaryolarını destekler ve son kullanıcı deneyimini geliştirir. Daha fazla bilgi edinin: [Microsoft Intune İlkeyle korunan bir tarayıcı kullanarak Web erişimini yönetme](https://docs.microsoft.com/intune/app-configuration-managed-browser)

## <a name="plan-your-my-apps-deployment"></a>Uygulamalarım dağıtımınızı planlayın

Erişim panelinin temeli, uygulama başlatıcısı Uygulamalarım, kullanıcıların erişim [https://myapps.microsoft.com](https://myapps.microsoft.com/)' i. Uygulamalarım sayfaları, kullanıcılara işlerini başlatmak ve gerekli uygulamalarına ulaşmak için tek bir yer sunar. Burada, kullanıcılar çoklu oturum açma erişimine sahip oldukları tüm uygulamaların bir listesini bulur. 

![Uygulamalar panelinin ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-app-launcher.png)

Kullanıcılar Office 365 portalını kullanırken, Office 365 uygulama başlatıcısı 'nda aynı uygulamalar gösterilir.

Uygulamalarım başlatıcısında uygulama ekleme sırasını ve bunları her zaman aşamalı veya hepsi için bir kez mi aktaradığınızı planlayın. Bunu yapmak için, her bir uygulama için kimlik doğrulaması türünü ve mevcut çoklu oturum açma (SSO) tümleştirmelerini listelemek üzere bir uygulama envanteri oluşturun.

#### <a name="add-applications-to-the-my-apps-panel"></a>Uygulamalarım paneline uygulama ekleme

Tüm Azure AD SSO özellikli uygulamaları My Apps Launcher öğesine eklenebilir. Diğer uygulamalar, bağlantılı SSO seçeneği kullanılarak eklenir. Mevcut Web uygulamanızın URL 'sine bağlanan bir uygulama kutucuğu yapılandırabilirsiniz. Bağlantılı SSO, tüm uygulamaları Azure Active Directory SSO 'ya geçirmek zorunda kalmadan, kullanıcıları uygulamalar portalına yönlendirmeye başlayabilmeniz için izin verir. Kullanıcıların deneyimini kesintiye uğratmadan, Azure AD SSO ile yapılandırılmış uygulamalara giderek geçebilirsiniz.

#### <a name="plan-whether-to-use-my-apps-or-an-existing-portal"></a>Uygulamalarımı mı yoksa mevcut bir portalı mı kullanacağınızı planlayın

Kullanıcılarınız uygulamalarım dışında bir uygulama veya portala zaten sahip olabilir. Bu durumda, her iki portalı da destekleyip desteklemediğini veya yalnızca bir tane kullanacağınızı belirleyin.

Mevcut bir portal zaten kullanıcılar için bir başlangıç noktası olarak kullanılıyorsa, "Kullanıcı erişimi URL 'Leri" kullanarak uygulamalar işlevselliğini tümleştirebilirsiniz. Kullanıcı erişimi URL 'Leri, uygulamalar portalında bulunan uygulamalara doğrudan bağlantılar olarak çalışır. Bu URL 'Ler var olan herhangi bir Web sitesi içine gömülebilir. Kullanıcı bağlantıya tıkladığında uygulama Uygulamam portalından uygulamayı başlatır.

Kullanıcı erişimi URL 'SI özelliğini, Azure portal uygulamanın Özellikler alanında bulabilirsiniz.

![Uygulamalar panelinin ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-user-access-url.png)


## <a name="plan-self-service-application-discovery-and-access"></a>Self Servis uygulama bulmayı ve erişimini planlayın

Bir temel uygulama kümesi bir kullanıcının uygulamalarım sayfasına dağıtıldıktan sonra self servis uygulama yönetimi özelliklerini etkinleştirmenizi öneririz. Self Servis uygulama bulma şunları sunar:
* Kullanıcıların uygulamalarıma ekleyebilecekleri yeni uygulamaları bulmasına olanak sağlar. 
* Kullanıcılar, kurulum sırasında ihtiyacı olmayan isteğe bağlı uygulamalar ekleyebilirler.

Onay iş akışları, uygulamalara erişim için açık onay için kullanılabilir. Onaylayanlar olan kullanıcılar, uygulamaya erişim için bekleyen istek olduğunda uygulamalar portalı içinde bildirim alır.

## <a name="plan-self-service-group-membership"></a>Self servis grup üyeliklerini planlayın 

Kullanıcıların Azure Active Directory (Azure AD) içinde kendi güvenlik gruplarını veya Office 365 gruplarını oluşturmasını ve yönetmesini sağlayabilirsiniz. Grubun sahibi, üyelik isteklerini onaylayabilir veya reddedebilir ve grup üyeliği denetimini devredebilir. Self Servis Grup yönetimi özellikleri posta etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

Self Servis Grup üyeliğini planlamak için, kuruluşunuzdaki tüm kullanıcıların grup oluşturma ve yönetme izni mi yoksa yalnızca bir kullanıcı alt kümesi mi olacağını saptayın. Kullanıcıların bir alt kümesi varsa, bu kişilerin eklendiği bir grup ayarlamanız gerekir. Bu senaryoların etkinleştirilmesi hakkındaki ayrıntılar için bkz. [Azure Active Directory 'de Self Servis Grup yönetimini ayarlama](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management) .

## <a name="plan-reporting-and-auditing"></a>Raporlama ve denetim planlaması

Azure AD, [Teknik ve iş öngörüleri sunan raporlar](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/)sağlar. Sahipliğini ve bu raporların düzenli olarak tüketilmesi için iş ve teknik uygulama sahipleriyle birlikte çalışın. Aşağıdaki tabloda tipik raporlama senaryolarının bazı örnekleri verilmiştir.

|   | Riski yönetin| Üretkenliği artırın| İdare ve uyum |
|  - |- | - | - |
| Rapor türleri|  Uygulama izinleri ve kullanımı.| Hesap sağlama etkinliği| Uygulamalara kimlerin eriştiğini gözden geçirme |
| Olası eylemler| Denetim erişimi; izinleri iptal et| Sağlama hatalarını düzelt| Erişimi iptal et |

Azure AD, en çok denetim verilerini 30 gün boyunca tutar. Veriler, çözümleme sistemlerinize indirmeniz için Azure yönetim portalı veya API aracılığıyla kullanılabilir.

#### <a name="auditing"></a>Denetim

Uygulama erişimi için denetim günlükleri 30 gün boyunca kullanılabilir. Kurumunuz içindeki güvenlik denetimi daha uzun bekletme gerektiriyorsa, günlüklerin splunk veya Arctıma gibi bir güvenlik bilgileri olay ve yönetim (SıEM) aracına aktarılması gerekir.

Denetim, raporlama ve olağanüstü durum kurtarma yedeklemeleri için gereken indirme sıklığını, hedef sistemi ve her bir yedeklemeyi yönetmekten sorumlu olanları belgeleyin. Ayrı denetim ve raporlama yedeklemelerine ihtiyacınız olmayabilir. Olağanüstü durum kurtarma yedeklemeniz ayrı bir varlık olmalıdır.

## <a name="deploy-applications-to-users-my-apps-panel"></a>Uygulamaları kullanıcıların uygulamalarım paneline dağıtma

Bir uygulama SSO için yapılandırıldıktan sonra gruplara erişim atanır. Atanan gruplardaki kullanıcıların erişimi olur ve uygulamayı Uygulamalarım ve Office 365 uygulama başlatıcısı 'nda görür

Bkz. [Active Directory bir uygulamaya Kullanıcı ve Grup atama](methods-for-assigning-users-and-groups.md).

Test veya dağıtım sırasında, grupları eklemek istediğiniz ancak uygulamaların uygulamamda görüntülemesine izin vermedikçe, bkz. [Azure Active Directory kullanıcının deneyiminden bir uygulamayı gizleme](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

### <a name="deploy-microsoft-office-365-applications-to-my-apps"></a>Microsoft Office 365 uygulamalarını uygulamalarıma dağıtma

Office 365 uygulamaları için, kullanıcılar, bu kullanıcılara atanmış lisansları temel alarak Office 'in bir kopyasını alırlar. Office uygulamalarına erişim için bir önkoşul, kullanıcıları Office uygulamalarına bağlı doğru lisanslara atamaya yöneliktir. Lisansı bir Kullanıcı atadığınızda, kendi uygulamalarım sayfasında ve O365 uygulama başlatıcısı 'nda lisansla ilişkili uygulamaları otomatik olarak görürler.

Kullanıcılardan bir dizi Office uygulamasını gizlemek istiyorsanız, uygulamaları uygulama portalından gizleme seçeneği vardır ve bu da O365 portalından erişime izin vermeye devam eder. Uygulamanın kullanıcı ayarları bölümünde bu ayarları bulun. Daha fazla bilgi edinin: [Bir uygulamayı kullanıcının Azure Active Directory deneyiminden gizleyin](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

![Uygulamaların nasıl gizlenmeyeceğini yapılandırma ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-o365-portal.png)

### <a name="deploy-application-self-service-capabilities"></a>Uygulama self servis yeteneklerini dağıtma

Self Servis uygulama erişimi, kullanıcıların tüm uygulamalara erişimi kendi kendine bulmasına ve istemesine izin verir. Kullanıcılara, her erişim isteğinde bulunan her seferinde bir BT grubuna geçmeden ihtiyaç duydukları uygulamalara erişim yetkisi verilir. Bir Kullanıcı erişim istediğinde ve bir uygulama sahibi tarafından otomatik olarak ya da el ile onaylandığında, arka uçtaki bir gruba eklenir. Raporlama, erişim isteğinde bulunan, onaylanan veya kaldırılan kişiler üzerinde etkinleştirilir ve atanan rollerin yönetilmesi üzerinde denetim sağlar.

Uygulama erişim isteklerinin onayını iş onaylayanlara atayabilirsiniz. İş onaylayan, uygulama erişimi parolalarını iş onaylayanın uygulamalarım sayfasından doğrudan ayarlayabilir.

Daha fazla bilgi edinin: [Self Servis uygulama erişimini kullanma](https://docs.microsoft.com/azure/active-directory/application-access-panel-self-service-applications-how-to)

![Self Servis uygulama yönetimini yapılandırma ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-salesforce.png)

## <a name="validate-your-deployment"></a>Dağıtımınızı doğrulama

Erişim paneli dağıtımınızın eksiksiz bir şekilde sınanmış ve geri alma planının yapıldığından emin olun.

Aşağıdaki testler, şirkete ait cihazlar ve kişisel cihazlarla birlikte gerçekleştirilmelidir. Bu test çalışmaları, iş kullanım örneklerini de yansıtmalıdır. Aşağıda, bu belgedeki örnek iş gereksinimlerine ve tipik teknik senaryolara göre bazı durumlar yer alır. Gereksinimlerinize özgü diğerlerini ekleyin.

#### <a name="application-sso-access-test-case-examples"></a>Uygulama SSO 'SU erişim test çalışması örnekleri:


| Kurum başarı hikayesi| Beklenen sonuç |
| - | -|
| Kullanıcı My Apps portalında oturum açar| Kullanıcı oturum açabilir ve uygulamalarını görebilir |
| Kullanıcı bir Federasyon SSO uygulaması başlatıyor| Kullanıcı otomatik olarak uygulamada oturum açtı |
| Kullanıcı ilk kez parola SSO uygulaması başlatıyor| Kullanıcının uygulama Uzantımı yüklemesi gerekiyor |
| Kullanıcı bir parola SSO uygulamasını sonraki bir zamanda başlatır| Kullanıcı otomatik olarak uygulamada oturum açtı |
| Kullanıcı O365 portalından bir uygulama başlatıyor| Kullanıcı otomatik olarak uygulamada oturum açtı |
| Kullanıcı Managed Browser bir uygulamayı başlatır| Kullanıcı otomatik olarak uygulamada oturum açtı |


#### <a name="application-self-service-capabilities-test-case-examples"></a>Uygulama self servis yetenekleri test çalışması örnekleri


| Kurum başarı hikayesi| Beklenen sonuç |
| - | - |
| Kullanıcı, uygulama üyeliğini yönetebilir| Kullanıcı, uygulamaya erişimi olan üyeleri ekleyebilir/kaldırabilir |
| Kullanıcı uygulamayı düzenleyebilir| Kullanıcı, parola SSO uygulamaları için uygulamanın açıklamasını ve kimlik bilgilerini düzenleyebilir |

### <a name="rollback-steps"></a>Geri alma adımları

Dağıtımınızın planlanmaması durumunda ne yapılacağını planlamanız önemlidir. Dağıtım sırasında SSO yapılandırması başarısız olursa, [SSO sorunlarını](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sso) nasıl giderebileceğinizi ve kullanıcılarınıza etkisini nasıl azaltacağınızı anlamanız gerekir. Olağanüstü durumlarda, [SSO 'yu geri](../manage-apps/plan-sso-deployment.md)almanız gerekebilir.


## <a name="manage-your-implementation"></a>Uygulamanızı yönetme

Microsoft, Azure Active Directory içinde gerekli bir görevi gerçekleştirmek için en az ayrıcalıklı rolün kullanılmasını önerir. Microsoft, [kullanılabilir farklı rollerin gözden geçirilmesini](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) ve bu uygulamaya yönelik her kişi için gereksinimlerinizi çözmenin doğru bir şekilde seçilmesini öneriyor. Dağıtım tamamlandıktan sonra bazı rollerin geçici olarak uygulanması ve kaldırılması gerekebilir.

| Kişilikler| Roller| Azure AD rolü  |
| - | -| -|
| Yardım Masası Yöneticisi| Katman 1 desteği| Yok. |
| Kimlik Yöneticisi| Sorunları Azure AD ' i etkileyen yapılandırma ve hata ayıklama| Genel Yönetici |
| Uygulama Yöneticisi| Uygulamada Kullanıcı kanıtlaması, izinleri olan kullanıcılar üzerinde yapılandırma| Yok. |
| Altyapı Yöneticileri| Sertifika aktarma sahibi| Genel Yönetici |
| İşletme sahibi/paydaş| Uygulamada Kullanıcı kanıtlaması, izinleri olan kullanıcılar üzerinde yapılandırma| Yok. |

Dizin izinleri olan kullanıcılar için ek denetim, denetim ve erişim incelemesi sağlamak üzere rollerinizi yönetmek için [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) kullanmanızı öneririz.

### <a name="troubleshoot-access-panel-issues"></a>Erişim paneli sorunlarını giderme

Ortak senaryolarla destek kuruluşunuz için sorun giderme kılavuzlarını oluşturun ve çözümlerinden Microsoft belgelerine gelin. Kuruluşunuzun kullandığı katmanlara yönelik desteği kesen kılavuzlar oluşturmak isteyebilirsiniz.

Başvuru için aşağıdaki sorun giderme kılavuzlarını inceleyin:

[Görünmeyen uygulamalar](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-application-not-appearing)

[Beklenmeyen uygulamalar görünüyor](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-unexpected-application)

[Kullanıcı erişim panelinde oturum açılamıyor](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-web-sign-in-problem)

[Self Servis uygulama erişimini kullanan sorunlar](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-self-service-access)

[Tarayıcı uzantısıyla ilgili sorunlar](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-access-panel-browser-extension)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Active Directory Multi-Factor Authentication 'ın dağıtımını planlayın](https://aka.ms/deploymentplans/mfa)
