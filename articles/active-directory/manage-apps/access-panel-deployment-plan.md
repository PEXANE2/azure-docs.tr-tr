---
title: Azure Etkin Dizin Erişim Paneli dağıtımı planlama
description: Azure Active Directory Access Panelini dağıtma kılavuzu
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
ms.date: 09/27/2019
ms.author: baselden
ms.reviewer: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 04d8b6c6d40aa81bf56baed59f90417f2147fa56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76897066"
---
# <a name="plan-an-azure-active-directory-access-panel-deployment"></a>Azure Etkin Dizin Erişim Paneli dağıtımı planlama

Azure Etkin Dizin (Azure AD) Erişim Paneli, destek maliyetlerini düşürmeye, üretkenliği ve güvenliği artırmaya ve kullanıcı hayal kırıklığını azaltmaya yardımcı olan web tabanlı bir portaldır. Sistem, sisteme erişdiğinizde izleyen ve yöneticilere kötüye kullanım veya kötüye kullanım bildiriminde bulunduğu ayrıntılı raporlamayı içerir.

Azure AD Erişim Paneli'ni kullanarak şunları yapabilirsiniz:

* Uygulamaları gibi, şirketinin Azure AD'ye bağlı tüm kaynaklarını keşfedin ve bunlara erişin
* Yeni uygulamalara ve gruplara erişim isteğinde bulunun
* Başkaları için bu kaynaklara erişimi yönetme
* Self servis parola sıfırlama ayarlarını ve Azure Çok Faktörlü Kimlik Doğrulama ayarlarını yönetme
* Cihazlarını yönetme

Ayrıca yöneticilerin yönetmenlik yapmasına da olanak tanır:

* Hizmet şartları
* Kuruluşlar
* Erişim gözden geçirmeleri


## <a name="benefits-of-azure-ad-access-panel-integration"></a>Azure AD Erişim Paneli entegrasyonunun avantajları

Azure AD Erişim Paneli işletmelere aşağıdaki yollarla yararlanır:

**Sezgisel kullanıcı deneyimi sağlar**: Access Panel, Azure tek oturum açma (SSO) bağlı tüm uygulamalarınız için tek bir platform sağlar. Grup yönetimi ve self servis parola sıfırlama gibi varolan ayarları ve yeni yetenekleri bulmak için birleşik bir portalınız vardır. Sezgisel deneyim, kullanıcıların işlerine daha hızlı dönmelerini ve daha üretken olmalarına olanak sağlarken, hayal kırıklıklarını da azaltır.

**Üretkenliği artırır**: Access Paneli'ndeki tüm kullanıcı uygulamaları SSO özelliklidir. Kurumsal uygulamalar ve Office 365 arasında SSO'yu etkinleştirmek, ek oturum açma istemlerini azaltarak veya ortadan kaldırarak üstün bir oturum açma deneyimi oluşturur. Erişim Paneli self servis ve dinamik üyelik kullanır ve kimlik sisteminizin genel güvenliğini artırır. Bunu, doğru kişilerin uygulamalara erişimi yönetmesini sağlayarak yapar. Erişim Paneli, kaynakları hızlı bir şekilde bulmanız ve iş görevlerini sürdürmeniz için tutarlı bir açılış sayfası görevi görebilirsiniz.

**Maliyeti yönetir**: Access Panelini Azure AD ile etkinleştirmek, şirket içi altyapıların elden kaldırılmasına yardımcı olabilir. Tüm uygulamalarınızı bulmanız, kaynaklara erişim isteğinde bulunmanız ve hesapları yönetmeniz için tutarlı bir portal sağlayarak destek maliyetlerini azaltır.

**Esnekliği ve güvenliği artırır**: Erişim Paneli, bulut platformunun sağladığı güvenliğe ve esnekliğe erişmenizi sağlar. Yöneticiler ayarları uygulamalara ve kaynaklara kolayca değiştirebilir ve kullanıcıları etkilemeden yeni güvenlik gereksinimlerini karşılayabilir.

**Sağlam denetim ve kullanım izlemeyi sağlar**: Tüm kullanıcı yetenekleri için denetim ve kullanım izleme, kullanıcıların kaynaklarını ne zaman kullandıklarını size bildirebilir ve güvenliği değerlendirebilmenizi sağlar.

### <a name="licensing-considerations"></a>Lisanslama hususları

Erişim Paneli ücretsizdir ve temel düzeyde kullanmak için lisans gerektirmez. Ancak, dizininizdeki nesne sayısı ve dağıtmak istediğiniz ek özellikler ek lisanslar gerektirebilir. Lisanslama gereksinimleri olan bazı yaygın Azure REKLAM senaryoları aşağıdaki güvenlik özelliklerini içerir:

* [Azure Çok Faktörlü Kimlik Doğrulama](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works)

* [Grup tabanlı üyelik](https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups)

* [Self servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)

* [Azure Active Directory Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

Azure [AD için tam lisans kılavuzuna](https://azure.microsoft.com/pricing/details/active-directory/)bakın.

### <a name="prerequisites-for-deploying-the-azure-ad-access-panel"></a>Azure AD Erişim Paneli'ni dağıtmak için ön koşullar

Bu projeye başlamadan önce aşağıdaki ön koşulları tamamlayın:

* [Uygulama SSO'yu entegre edin](https://docs.microsoft.com/azure/active-directory/manage-apps/plan-sso-deployment)

* [Azure AD kullanıcı ve grup altyapılarını yönetme](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups)

## <a name="plan-the-azure-ad-access-panel-deployment"></a>Azure AD Erişim Paneli dağıtımını planlayın

Aşağıdaki tablo, Access Panel dağıtımı için anahtar kullanım örneklerini özetlemektedir:

| Alan| Açıklama |
| - | - |
| Erişim| Access Panel portalına kurumsal ağ daki kurumsal ve kişisel cihazlardan erişilebilir. |
|Erişim | Access Panel portalına kurumsal ağ dışındaki kurumsal cihazlardan erişilebilir. |
| Denetim| Kullanım verileri en az 29 günde bir kurumsal sistemlere indirilir. |
| İdare| Kullanıcı atamalarının Azure AD'ye bağlı uygulama ve gruplara olan yaşam döngüsü tanımlanır ve izlenir. |
| Güvenlik| Kaynaklara erişim, kullanıcı ve grup atamaları aracılığıyla denetlenir. Kaynak erişimini yalnızca yetkili kullanıcılar yönetebilir. |
| Performans| Erişim ataması yayılma zaman çizelgeleri belgelenir ve izlenir. |
| Kullanıcı Deneyimi| Kullanıcılar Erişim Masası özelliklerinin ve bunların nasıl kullanılacağının farkındadır.|
| Kullanıcı Deneyimi| Kullanıcılar uygulamalara ve gruplara erişimlerini yönetebilir.|
| Kullanıcı Deneyimi| Kullanıcılar hesaplarını yönetebilir. |
| Kullanıcı Deneyimi| Kullanıcılar tarayıcı uyumluluğunun farkındadır. |
| Destek| Kullanıcılar Access Panel sorunları için destek bulabilir. |

### <a name="best-practices-for-deploying-the-azure-ad-access-panel"></a>Azure AD Erişim Paneli'ni dağıtmak için en iyi uygulamalar

Access Panelinin işlevselliği aşamalı olarak etkinleştirilebilir. Aşağıdaki dağıtım sırasını öneririz:

1. Uygulamalarım
   * Uygulama başlatıcısı
   * Self servis uygulama yönetimi
   * Microsoft Office 365 tümleştirmesi

1. Self servis uygulama bulma
   * Self servis parola sıfırlama
   * Çok Faktörlü Kimlik Doğrulama ayarları
   * Cihaz yönetimi
   * Kullanım koşulları
   * Kuruluşları yönetme

1. Gruplarım
   * Self servis grup yönetimi
1. Erişim gözden geçirmeleri
   * Erişim gözden geçirme yönetimi

Uygulamalarım'dan başlayarak, kullanıcılar akaynaklara erişmek için ortak bir yer olarak portalı tanıtıyor. Self servis uygulama keşfinin eklenmesi, Uygulamalarım deneyimine dayanıyor. Gruplarım ve erişim incelemelerim self servis yeteneklerine göre geliştirilir.

### <a name="plan-configurations-for-the-azure-ad-access-panel"></a>Azure AD Erişim Paneli için yapılandırmaları planlama

Aşağıdaki tablo, birkaç önemli Access Panel yapılandırmasını ve kullanabileceğiniz tipik değerleri listeler:

| Yapılandırma| Tipik değerler |
| - | - |
| Pilot grupları belirleme| Kullanılacak Azure REKLAM güvenlik grubunu belirleyin ve tüm pilot üyelerin grubun bir parçası olduğundan emin olun. |
| Üretim için etkinleştirilecek grubu veya grupları belirleyin.| Kullanılmak üzere Azure AD güvenlik gruplarını veya Azure AD ile senkronize edilen Etkin Dizin gruplarını tanımlayın. Tüm pilot üyelerin grubun bir parçası olduğundan emin olun. |
| Kullanıcıların Belirli uygulama türlerinde SSO'yu kullanmasına izin verme| Federe SSO, OAuth, Şifre SSO, App Proxy |
| Kullanıcıların self servis parola sıfırlama kullanmalarına izin verme | Evet |
| Kullanıcıların Çok Faktörlü Kimlik Doğrulama'yı kullanmasına izin ver| Evet |
| Kullanıcıların belirli grup türleri için self servis grup yönetimini kullanmasına izin verme| Güvenlik grupları, Office 365 grupları |
| Kullanıcıların self servis uygulama yönetimini kullanmasına izin verme| Evet |
| Kullanıcıların erişim yorumlarını kullanmasına izin verme| Evet |

### <a name="plan-consent-strategy"></a>İzin stratejisini planlayın

Kullanıcılar veya yöneticiler, herhangi bir uygulamanın kullanım koşullarını ve gizlilik ilkelerini kabul etmelidir. Mümkünse, işletme kurallarınız göz önüne alındığında, kullanıcılara daha iyi bir deneyim sunan yönetici onayı kullanın.

Yönetici onayı kullanmak için kuruluşun genel yöneticisi olmalısınız ve uygulamalar aşağıdakilerden biri olmalıdır:

* Kuruluşunuza kayıtlı

* Başka bir Azure REKLAM kuruluşuna kayıtlı ve daha önce en az bir kullanıcı tarafından onaylanmış

Daha fazla bilgi için bkz. Son [kullanıcıların Azure Etkin Dizini'ndeki bir uygulamayı nasıl kabul etme şekilleri yapılandırıldı.](configure-user-consent.md)

### <a name="engage-the-right-stakeholders"></a>Doğru paydaşlarla etkileşimde bulunun

Teknoloji projeleri başarısız olduğunda, genellikle etki, sonuçlar ve sorumluluklar üzerindeki uyumsuz beklentiler nedeniyle bunu yaparlar. Bu tuzaklardan kaçınmak için, [doğru paydaşlarla etkileşimde bulunduğunuzdan](../fundamentals/active-directory-deployment-plans.md) ve projedeki paydaş rollerinin iyi anlaşıldığından emin olun.

### <a name="plan-communications"></a>İletişimi planlama

İletişim, herhangi bir yeni hizmetin başarısı için çok önemlidir. Kullanıcılarınızı deneyimlerinin nasıl ve ne zaman değişeceğini ve gerekirse nasıl destek kazanacağını proaktif bir şekilde bildirin.

Access Panel genellikle kullanıcı sorunları oluşturmasa da, hazırlamak önemlidir. Başlatmadan önce destek personeliniz için kılavuzları ve tüm kaynakların listesini oluşturun.

#### <a name="communications-templates"></a>İletişim şablonları

Microsoft, Erişim Paneli [için e-postalar ve diğer iletişimler için özelleştirilebilir şablonlar](https://aka.ms/APTemplates) sağlar. Bu varlıkları, şirket kültürünüze uygun olarak diğer iletişim kanallarında kullanılmak üzere uyarlayabilirsiniz.

## <a name="plan-your-sso-configuration"></a>SSO yapılandırmanızı planlayın

Bir kullanıcı bir uygulamaya girdiğinde, bir kimlik doğrulama işleminden geçer ve kim olduklarını kanıtlamaları gerekir. SSO olmadan, uygulamada bir parola depolanır ve kullanıcının bu parolayı bilmesi gerekir. SSO ile kullanıcıların kimlik bilgileri uygulamaya aktarılır, bu nedenle her uygulama için parolaları yeniden girmeleri gerekmez.

Uygulamalarım'da uygulama başlatmak için SSO'nun etkinleştirilmesi gerekir.

Azure AD, [uygulamalarda tek oturum açmayı](what-is-single-sign-on.md)etkinleştirmenin üç farklı yolunu destekler:

* **Federe tek oturum açma** 
    * Bir uygulamanın parola istenmesi yerine kullanıcı kimlik doğrulaması için Azure AD'ye yönlendirilmesini sağlar. 
    * SAML 2.0, WS-Federation veya OpenID Connect gibi protokolleri kullanan uygulamalar için desteklenir ve en zengin tek oturum açma modudur.

* **Parola tabanlı tek oturum açma** 
    * Bir web tarayıcısı uzantısı veya mobil uygulama kullanarak güvenli uygulama parolası depolama ve yeniden oynatma sağlar. 
    * Uygulama tarafından sağlanan varolan oturum açma işleminden yararlanır, ancak yöneticinin parolaları yönetmesini sağlar. Kullanıcının parolayı bilmesi gerekmez.

* **Varolan tek oturum açma** 
    * Azure AD'nin uygulama için yapılandırılan varolan tek oturum açma dan yararlanmasını sağlar.
    * Bu uygulamaların Office 365 veya Azure AD Erişim Paneli portallarına bağlanmasını sağlar. 
    * Uygulamalar burada başlatıldığında Azure AD'de ek raporlama sağlar. 
    * Azure Application Proxy ve bağlantılı tek oturum açma modunu kullanmayı içerir.

Bir uygulamanın SSO modunu nasıl yapılandırıştırmayı buradan öğrenin: [Azure Active Directory'deki uygulamalarda tek oturum](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)açma .

Uygulamalarım sayfasındaen iyi deneyimi yaşamak için, federe SSO için kullanılabilen bulut uygulamalarının entegrasyonuyla başlayın. Federe SSO, kullanıcıların uygulama başlatma yüzeylerinde tutarlı bir tek tıklamadeneyimi yaşamalarını sağlar ve yapılandırma denetiminde daha güçlü olma eğilimindedir.

Bir uygulama parola tabanlı SSO ve ADFS yerine azure AD (OpenID Connect/SAML) ile Federe SSO'yu kullanın.

SaaS uygulamalarınızı nasıl dağıtıp yapılandırılabildiğiniz hakkında daha fazla bilgi için [SaaS SSO dağıtım planına](https://aka.ms/deploymentplans/sso)bakın.

#### <a name="plan-to-deploy-the-my-apps-browser-extension"></a>Uygulamalarım tarayıcı uzantısını dağıtmayı planlayın

Kullanıcılar parola tabanlı SSO uygulamalarında oturum açınca, Uygulamalarım güvenli oturum açma uzantısını yüklemeleri ve kullanmaları gerekir. Uzantı, parolayı uygulamanın oturum açma formuna ileten bir komut dosyası yürütür. Kullanıcılardan, parola tabanlı SSO uygulamasını ilk başlattıklarında uzantıyı yüklemeleri istenir. Uzantı hakkında daha fazla bilgi [Access Panel tarayıcı uzantısı yükleme](https://docs.microsoft.com/azure/active-directory/application-access-panel-extension-problem-installing?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav)bu belgelerde bulunabilir.

Parola tabanlı SSO uygulamalarını tümleştirmeniz gerekiyorsa, uzantıyı [desteklenen tarayıcılarla](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)ölçekte dağıtmak için bir mekanizma tanımlamanız gerekir. Seçeneklere şunlar dahildir:

* [Internet Explorer için Grup İlkesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)

* [Internet Explorer için Configuration Manager](https://docs.microsoft.com/configmgr/core/clients/deploy/deploy-clients-to-windows-computers)

* [Chrome, Firefox, Microsoft Edge veya IE için kullanıcı odaklı indirme ve yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Daha fazla bilgi: Parola tek oturum açma [nasıl yapılandırılabilen.](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery)

Parola tabanlı SSO uygulamalarını kullanmayan kullanıcılar da uzantıdan yararlanır. Bu avantajlar arasında herhangi bir uygulamayı arama çubuğundan başlatabilme, son zamanlarda kullanılan uygulamalara erişim bulma ve Uygulamalarım sayfasına bağlantı verme yer alıyor.

Parola tabanlı bir SSO uygulamasını ilk kez başlatırken kullanıcının göreceği aşağıdakileri:

![My Apps tarayıcı uzantısı yükleme ekranıekran görüntüsü ](media/access-panel-deployment-plan/ap-dp-sign-in.png)

#### <a name="plan-for-mobile-access"></a>Mobil erişim için plan

Parola tabanlı SSO uygulamalarını başlatan mobil kullanıcılar için Intune ilkesi (Microsoft Edge veya Intune Yönetilen Tarayıcı) ile korunan bir tarayıcı gereklidir. İlke korumalı bir tarayıcı, uygulama için kaydedilen parolanın aktarılmasını sağlar. Microsoft Edge veya yönetilen tarayıcı bir dizi web veri koruma özelliği sağlar. Microsoft Edge'i iOS ve Android cihazlardaki kurumsal senaryolar için de kullanabilirsiniz. Microsoft Edge, Intune Yönetilen Tarayıcı ile aynı yönetim senaryolarını destekler ve kullanıcı deneyimini geliştirir. Daha fazla bilgi: [Microsoft Intune ilkesi korumalı bir tarayıcı kullanarak web erişimini yönetin.](https://docs.microsoft.com/intune/app-configuration-managed-browser)

## <a name="plan-your-my-apps-deployment"></a>Uygulamalarım Dağıtımını Planla

Access Paneli'nin temeli, kullanıcıların [https://myapps.microsoft.com](https://myapps.microsoft.com/). Uygulamalarım sayfaları, kullanıcılara çalışmalarına başlamaları ve gerekli uygulamalarına uyolmaları için tek bir yer sağlar. Burada, kullanıcılar tek oturum açma erişimi olan tüm uygulamaların bir listesini bulurlar. 

![Uygulamalar panelinin ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-app-launcher.png)

Kullanıcılar Office 365 portalını kullanırken aynı uygulamalar Office 365 uygulama başlatıcısında gösterilir.

Uygulamalarımı Başlatıcım'a uygulama ekleme sırasını planlayın ve bunları kademeli olarak mı yoksa hepsini tek seferde mi yoksa hepsini mi yayınlayacağımize karar verin. Bunu yapmak için, kimlik doğrulama türünü ve her uygulama için varolan SSO tümleştirmelerini listeleyen bir uygulama envanteri oluşturun.

#### <a name="add-applications-to-the-my-apps-panel"></a>Uygulamalarım paneline uygulama ekleme

Azure AD SSO özellikli tüm uygulamalar Uygulamalarım başlatıcısına eklenebilir. Diğer uygulamalar Linked SSO seçeneği kullanılarak eklenir. Mevcut web uygulamanızın URL'sine bağlantı veren bir uygulama döşemesini yapılandırabilirsiniz. Linked SSO, tüm uygulamaları Azure AD SSO'ya geçirmeden kullanıcıları Uygulamalarım portalına yönlendirmeye başlamanızı sağlar. Kullanıcıların deneyimini kesintiye uğratmadan yavaş yavaş Azure AD SSO tarafından yapılandırılan uygulamalara geçebilirsiniz.

#### <a name="use-my-apps-collections"></a>Uygulamalarım koleksiyonlarını kullanma

Varsayılan olarak, tüm uygulamalar tek bir sayfada birlikte listelenir. Ancak koleksiyonları, ilgili uygulamaları gruplandırmak ve ayrı bir sekmede sunmak için kullanabilirsiniz, bu da koleksiyonları bulmayı kolaylaştırır. Örneğin, belirli iş rolleri, görevler, projeler ve benzeri uygulamalar için mantıksal gruplandırmaları oluşturmak için koleksiyonları kullanabilirsiniz. Daha fazla bilgi için, [kullanıcı erişim panellerini özelleştirmek için Uygulamalarım koleksiyonlarını nasıl kullanacağınız](access-panel-collections.md)bilgisine bakın. 

#### <a name="plan-whether-to-use-my-apps-or-an-existing-portal"></a>Uygulamalarımı veya varolan bir portalı kullanıp kullanmamayı planlayın

Kullanıcılarınızın Uygulamalarım dışında zaten bir uygulaması veya portalı olabilir. Bu nedenle, her iki portalı da desteklemeye veya yalnızca bir portal kullanmaya karar verin.

Varolan bir portal zaten kullanıcılar için bir başlangıç noktası olarak kullanılıyorsa, kullanıcı erişim URL'lerini kullanarak Uygulamalarım işlevini tümleştirebilirsiniz. Kullanıcı erişim URL'leri, Uygulamalarım portalında bulunan uygulamalara doğrudan bağlantı işlevi görür. Bu URL'ler mevcut herhangi bir web sitesine gömülü olabilir. Bir kullanıcı bağlantıyı seçtiğinde, uygulamayı Uygulamalarım portalından açar.

Azure portalında uygulamanın **Özellikler** alanında kullanıcı erişim URL özelliğini bulabilirsiniz.

![Uygulamalar panelinin ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-user-access-url.png)


## <a name="plan-self-service-application-discovery-and-access"></a>Self servis uygulama bulma ve erişim planı

Bir kullanıcının Uygulamalarım sayfasına temel bir uygulama kümesi dağıtıldıktan sonra, self servis uygulama yönetimi özelliklerini etkinleştirmelisiniz. Self servis uygulama bulma, kullanıcıların şunları yapmalarını sağlar:

* Uygulamalarım'a ekleyecek yeni uygulamalar bulun. 
* Kurulum sırasında gereksinim duymaları gerektiğini bilmeyebilecekleri isteğe bağlı uygulamalar ekleyin.

Onay iş akışları, uygulamalara erişmek için açık onay için kullanılabilir. Onaylayan kullanıcılar, uygulamaya erişim için bekleyen istek olduğunda Uygulamalarım portalında bildirim alır.

## <a name="plan-self-service-group-membership"></a>Self servis grup üyeliğini planlayın 

Azure AD'de kullanıcıların kendi güvenlik gruplarını veya Office 365 gruplarını oluşturmalarını ve yönetmelerini sağlayabilirsiniz. Grubun sahibi üyelik isteklerini onaylayabilir veya reddedebilir ve grup üyeliğinin denetimini devredebilir. Self servis grup yönetimi özellikleri posta yla etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

Self servis grup üyeliği planlamak için, kuruluşunuzdaki tüm kullanıcıların gruplar veya yalnızca bir kullanıcı alt kümesi oluşturmasına ve yönetmesine izin verilip verilmeyeceksiniz belirleyin. Bir kullanıcı alt kümesine izin verecekseniz, bu kişilerin eklendiği bir grup kurmanız gerekir. Bu senaryoları etkinleştirme yle ilgili ayrıntılar için [Azure Active Directory'de self servis grup yönetimini ayarlama'ya](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management) bakın.

## <a name="plan-reporting-and-auditing"></a>Planlama raporlama ve denetim

Azure AD, [teknik ve iş öngörüleri sunan raporlar](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/)sağlar. Bu raporların sahipliğini üstlenmek ve düzenli olarak tüketmek için işletmeniz ve teknik uygulama sahiplerinizle birlikte çalışın. Aşağıdaki tablo, tipik raporlama senaryolarına bazı örnekler sağlar.

|   | Riski yönetin| Üretkenliği artırın| İdare ve uyum |
|  - |- | - | - |
| Rapor türleri|  Uygulama izinleri ve kullanımı| Hesap sağlama etkinliği| Uygulamalara kimlerin erişteceklerini gözden geçirin |
| Olası eylemler| Denetim erişimi; izinleri iptal edin| Herhangi bir sağlama hatalarını düzeltme| Erişimi iptal et |

Azure AD, denetim verilerinin çoğunu 30 gün boyunca tutar. Veriler, analiz sistemlerinize indiremeniz için Azure Yönetici Portalı veya API üzerinden kullanılabilir.

#### <a name="auditing"></a>Denetim

Uygulama erişimi için denetim günlükleri 30 gün boyunca kullanılabilir. İşletmenizdeki güvenlik denetimi daha uzun süre bekletilmesi gerektiriyorsa, günlüklerin Splunk veya ArcSight gibi bir Güvenlik Bilgi Olayı ve Yönetimi (SIEM) aracına dışa aktarılması gerekir.

Denetim, raporlama ve olağanüstü durum kurtarma yedeklemeleri için, gerekli indirme sıklığını, hedef sistemin ne olduğunu ve her yedeklemeyi yönetmekten kimin sorumlu olduğunu belgele. Ayrı denetim ve raporlama yedeklemelerine gerek olmayabilir. Olağanüstü durum kurtarma yedeklemeniz ayrı bir varlık olmalıdır.

## <a name="deploy-applications-to-users-my-apps-panel"></a>Uygulamaları kullanıcıların Uygulamalarım paneline dağıtma

SSO için bir uygulama yapılandırıldıktan sonra, gruplara erişim atanır. Atanan gruplardaki kullanıcılar erişime sahip olacak ve uygulamayı Uygulamalarım'da ve Office 365 uygulama başlatıcılarında görecekler.

Bkz. [Etkin Dizin'deki bir uygulamaya kullanıcı ve gruplar atama.](methods-for-assigning-users-and-groups.md)

Sınama veya dağıtım sırasında grupları eklemek istiyorsanız, ancak uygulamaların Uygulamalarım'da gösterilmesine henüz izin vermiyorsanız, [bkz.](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)

### <a name="deploy-microsoft-office-365-applications-to-my-apps"></a>Microsoft Office 365 uygulamalarını Uygulamalarıma dağıtma

Office 365 uygulamaları için kullanıcılar, kendilerine atanan lisansları temel alan Office'in bir kopyasını alır. Office uygulamalarına erişim için ön koşul, kullanıcıların Office uygulamalarına bağlı doğru lisansların atanmasıdır. Bir kullanıcıya lisans atadığınızda, lisansla ilişkili uygulamaları Uygulamalarım sayfasında ve Office 365 uygulama başlatıcısında otomatik olarak görürler.

Bir dizi Office uygulamasını kullanıcılardan gizlemek istiyorsanız, UygulamalarıM portalından gizleme seçeneği sunarken Office 365 portalından erişime izin vermeye devam edebilirsiniz. Bu ayarları uygulamanın Kullanıcı ayarları bölümünde bulun. Daha fazla bilgi: [Bir uygulamayı Azure Active Directory'deki kullanıcı deneyiminden gizle.](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)

![Uygulamaların nasıl gizlenirilir yapılandırılma ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-o365-portal.png)

### <a name="deploy-application-self-service-capabilities"></a>Uygulama self servis özelliklerini dağıtma

Self servis uygulama erişimi, kullanıcıların uygulamaları kendi kendine keşfetmelerine ve uygulamalara erişim istemelerine olanak tanır. Kullanıcılar, erişim istemek için her seferinde bir BT grubuna girmeden ihtiyaç duydukları uygulamalara erişme özgürlüğüne sahiptir. Bir kullanıcı erişim istediğinde ve bir uygulama sahibi tarafından otomatik olarak veya el ile onaylandığında, arka uçtaki bir gruba eklenir. Raporlama, erişimi kimin istediği, onayladığı veya kaldırdığı konusunda etkinleştirilir ve atanan rolleri yönetme konusunda size denetim sağlar.

Uygulama erişim isteklerinin onayını işletme onaylayanlara devredebilirsiniz. İş onaylayıcısı, işletme yi onaylayanın Uygulamalarım sayfasından uygulama erişim parolalarını ayarlayabilir.

Daha fazla bilgi: [Self servis uygulama erişimi nasıl kullanılır.](https://docs.microsoft.com/azure/active-directory/application-access-panel-self-service-applications-how-to)

![Self servis uygulama yönetimini yapılandırmanın ekran görüntüsü](media/access-panel-deployment-plan/ap-dp-salesforce.png)

## <a name="validate-your-deployment"></a>Dağıtımınızı doğrulayın

Access Panel dağıtımınızın kapsamlı bir şekilde test edildiğinden ve bir geri alma planının hazır olduğundan emin olun.

Aşağıdaki testler hem kurumsal cihazlar hem de kişisel cihazlarla yapılmalıdır. Bu test örnekleri, iş kullanım durumlarınızı da yansıtmalıdır. Aşağıda, bu belgedeki örnek iş gereksinimlerine ve tipik teknik senaryolara dayalı birkaç örnek durum verilmiştir. İhtiyaçlarınıza özel başkalarını ekleyin.

#### <a name="application-sso-access-test-case-examples"></a>Uygulama SSO erişim test örneği örnekleri:


| İş durumu| Beklenen sonuç |
| - | -|
| Kullanıcı Uygulamalarım portalında giriş yaptı| Kullanıcı oturum açabilir ve uygulamalarını görebilir |
| Kullanıcı federe SSO uygulaması başlattı| Kullanıcı uygulamada otomatik olarak oturum |
| Kullanıcı ilk kez bir şifre SSO uygulaması başlattı| Kullanıcının Uygulamalarım uzantısını yüklemesi gerekiyor |
| Kullanıcı bir sonraki zamanda bir şifre SSO uygulaması başlattı| Kullanıcı uygulamada otomatik olarak oturum |
| Kullanıcı Office 365 Portal'dan bir uygulama başlattı| Kullanıcı uygulamada otomatik olarak oturum |
| Kullanıcı Yönetilen Tarayıcı'dan bir uygulama başlattı| Kullanıcı uygulamada otomatik olarak oturum |


#### <a name="application-self-service-capabilities-test-case-examples"></a>Uygulama self servis yetenekleri test örneği örnekleri


| İş durumu| Beklenen sonuç |
| - | - |
| Kullanıcı uygulamaya üyeliği yönetebilir| Kullanıcı, uygulamaya erişimi olan üyeleri ekleyebilir/kaldırabilir |
| Kullanıcı uygulamayı edinebilir| Kullanıcı, parola SSO uygulamaları için uygulamanın açıklamasını ve kimlik bilgilerini edinebilir |

### <a name="rollback-steps"></a>Geri alma adımları

Dağıtımınız planlandığı gibi gitmezse ne yapacağınızı planlamak önemlidir. Dağıtım sırasında SSO yapılandırması başarısız olursa, [SSO sorunlarını](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sso) nasıl gidererve kullanıcılarınız üzerindeki etkiyi nasıl azalttığınızda anlamanız gerekir. Ekstrem durumlarda, [SSO'yu geri almanız](../manage-apps/plan-sso-deployment.md)gerekebilir.


## <a name="manage-your-implementation"></a>Uygulamanızı yönetme

Azure Etkin Dizini içinde gerekli bir görevi gerçekleştirmek için en az ayrıcalıklı rolü kullanmanız gerekir. [Kullanılabilir farklı rolleri gözden geçirin](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) ve bu uygulama için her kişi için gereksinimlerinizi çözmek için doğru rolleri seçin. Bazı rollerin geçici olarak uygulanması ve dağıtım tamamlandıktan sonra kaldırılması gerekebilir.

| Kişilikler| Roller| Azure AD rolü  |
| - | -| -|
| Yardım Masası yöneticisi| Tier 1 desteği| None |
| Kimlik yöneticisi| Sorunlar Azure AD'yi etkilediğinde yapılandırma ve hata ayıklama| Genel yönetici |
| Uygulama yöneticisi| Uygulamada kullanıcı attestation, izinleri olan kullanıcılar üzerinde yapılandırma| None |
| Altyapı yöneticileri| Cert rollover sahibi| Genel yönetici |
| İşletme sahibi/paydaş| Uygulamada kullanıcı attestation, izinleri olan kullanıcılar üzerinde yapılandırma| None |

Dizin izinlerine sahip kullanıcılar için ek denetim, denetim ve erişim incelemesi sağlamak için rollerinizi yönetmek için [Ayrıcalıklı Kimlik Yönetimi'ni](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) kullanabilirsiniz.

### <a name="troubleshoot-access-panel-issues"></a>Sorun Giderme Access Panel sorunları

Destek kuruluşunuz için ortak senaryolarla sorun giderme kılavuzları oluşturun ve bu da çözümlerinde Microsoft belgelerine işaret ediyor. Desteğini kuruluşunuz tarafından kullanılan katmanlara kıran kılavuzlar oluşturmak isteyebilirsiniz.

Başvuru için şu sorun giderme kılavuzlarına bakın:

[Uygulamalar görünmüyor](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-application-not-appearing)

[Beklenmeyen uygulamalar görünür](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-unexpected-application)

[Kullanıcı Erişim Paneli'nde oturum açamıyor](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-web-sign-in-problem)

[Self servis uygulama erişimini kullanma sorunları](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-self-service-access)

[Tarayıcı uzantısı ile ilgili sorunlar](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-access-panel-browser-extension)

## <a name="next-steps"></a>Sonraki adımlar

[Azure Çok Faktörlü Kimlik Doğrulama'yı düzenleme yi planlayın](https://aka.ms/deploymentplans/mfa)
