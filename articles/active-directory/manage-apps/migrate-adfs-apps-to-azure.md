---
title: AD FS uygulama kimlik doğrulamasını Azure Active Directory olarak taşıma
description: Bu makale, kuruluşların Federasyon SaaS uygulamalarına odaklanarak uygulamaların Azure AD 'ye nasıl taşınacağını anlamalarına yardımcı olmak için tasarlanmıştır.
services: active-directory
author: kenwith
manager: celestedg
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: how-to
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/01/2020
ms.author: kenwith
ms.reviewer: baselden
ms.collection: M365-identity-device-management
ms.openlocfilehash: 03fe49456ac49e0e81c108198584a2c4d8eab884
ms.sourcegitcommit: bc943dc048d9ab98caf4706b022eb5c6421ec459
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2020
ms.locfileid: "84763236"
---
# <a name="moving-application-authentication-from-active-directory-federation-services-to-azure-active-directory"></a>Active Directory Federasyon Hizmetleri (AD FS) uygulama kimlik doğrulamasını Azure Active Directory olarak taşıma

[Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) , kişilere, iş ortaklarınıza ve müşterilerinize uygulamalara erişmek ve herhangi bir platformdan ve cihazdan işbirliği yapmak için tek bir kimlik sağlayan bir evrensel kimlik platformu sunar. Azure AD ['nin eksiksiz bir kimlik yönetimi özellikleri paketi](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)vardır. Uygulama (uygulama) kimlik doğrulama ve Azure AD yetkilendirmesi için standartlaştırarak bu yeteneklerin sağladığı avantajlar sağlanır. 

> [!NOTE]
> Bu makale, uygulama kimlik doğrulamasını şirket içi Active Directory ve Active Directory Federasyon Hizmetleri (AD FS) Azure AD 'ye taşımaya odaklanır. Bu taşımanın planlanmasına ilişkin bir genel bakış için lütfen [uygulama kimlik doğrulamasını Azure AD 'ye geçirme](https://aka.ms/migrateapps/whitepaper) teknik incelemesine bakın. Teknik İnceleme, geçiş, test ve öngörüleri nasıl planlayabileceğinizi anlatmaktadır.

## <a name="introduction"></a>Giriş

Kullanıcı hesapları içeren bir şirket içi dizininiz varsa, büyük olasılıkla kullanıcıların kimlik doğrulaması yapmış birçok uygulamanız vardır. Bu uygulamaların her biri kullanıcıların kimliklerini kullanarak erişmesi için yapılandırılır. 


Kullanıcılar ayrıca, şirket içi Active Directory doğrudan kimlik doğrulaması yapabilir. Active Directory Federasyon Hizmetleri (AD FS) (AD FS), şirket içi kimlik hizmeti tabanlı bir standarttır. AD FS, güvenilen iş ortakları arasında çoklu oturum açma (SSO) işlevselliğini kullanarak kullanıcıların her bir uygulamada ayrı oturum açmasını zorunlu kılmadan genişletir. Bu, Federasyon olarak bilinir.

Birçok kuruluşta, Office 365 ve Azure AD tabanlı uygulamalar da içinde olmak üzere doğrudan AD FS federe bir hizmet olarak yazılım (SaaS) veya özel Iş kolu (LOB) uygulamaları vardır. 

![Doğrudan şirket içine bağlı uygulamalar](media/migrate-adfs-apps-to-azure/app-integration-before-migration1.png)

**Uygulama güvenliğini artırmak için amacınız, şirket içi ve bulut ortamlarınızda tek bir erişim denetimi ve ilke kümesine sahip**olmaktır. 

![Azure AD ile bağlantılı uygulamalar](media/migrate-adfs-apps-to-azure/app-integration-after-migration1.png)



## <a name="types-of-apps-to-migrate"></a>Geçirilecek uygulama türleri

Kimlik ve erişim yönetimi için tek bir denetim düzlemi sunabildiğinden, tüm uygulama kimlik doğrulamalarınızın Azure AD 'ye geçirilmesi idealdir.

Uygulamalarınız, kimlik doğrulaması için modern veya eski protokolleri kullanabilir. Önce modern kimlik doğrulama protokolleri (SAML ve açık KIMLIK Connect gibi) kullanan uygulamaları geçirmeyi düşünün. Bu uygulamalar, uygulama galerimizin yerleşik bir Bağlayıcısı aracılığıyla ya da uygulamayı Azure AD 'ye kaydederek Azure AD ile kimlik doğrulaması yapmak için yeniden yapılandırılabilir. Eski protokolleri kullanan uygulamalar, [uygulama proxy 'si](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-application-proxy)kullanılarak tümleştirilebilir. 

Daha fazla bilgi için bkz. [Azure AD Ile hangi tür uygulamaları tümleştiririm](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-application-management)?

[Azure AD Connect Health etkinleştirdiyseniz](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs), [uygulamaları Azure AD 'ye geçirmek için AD FS uygulama etkinliği raporunu](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity) kullanabilirsiniz. 

### <a name="the-migration-process"></a>Geçiş işlemi

Uygulama kimlik doğrulamasını Azure AD 'ye taşıma işlemi sırasında uygulamalarınızı ve yapılandırmanızı yeterince test edin. Üretim ortamına geçiş testi için mevcut test ortamlarını kullanmaya devam etmenizi öneririz. Şu anda bir test ortamı yoksa, uygulama mimarisine bağlı olarak [Azure App Service](https://azure.microsoft.com/services/app-service/) veya [Azure sanal makinelerini](https://azure.microsoft.com/free/virtual-machines/search/?OCID=AID2000128_SEM_lHAVAxZC&MarinID=lHAVAxZC_79233574796345_azure%20virtual%20machines_be_c__1267736956991399_kwd-79233582895903%3Aloc-190&lnkd=Bing_Azure_Brand&msclkid=df6ac75ba7b612854c4299397f6ab5b0&ef_id=XmAptQAAAJXRb3S4%3A20200306231230%3As&dclid=CjkKEQiAhojzBRDg5ZfomsvdiaABEiQABCU7XjfdCUtsl-Abe1RAtAT35kOyI5YKzpxRD6eJS2NM97zw_wcB)kullanarak bir tane ayarlayabilirsiniz.

Uygulama yapılandırmalarından geliştirme sırasında kullanmak üzere ayrı bir test Azure AD kiracısı ayarlamayı tercih edebilirsiniz. 

Geçiş işleminiz şöyle görünebilir:

**1. aşama – geçerli durum: AD FS ile üretim uygulaması kimlik doğrulaması**

![Geçiş aşaması 1 ](media/migrate-adfs-apps-to-azure/stage1.jpg)

 
**2. aşama – Isteğe bağlı: Azure kiracısına işaret eden uygulamanın test örneği**

Uygulamanın test örneğinizi bir Azure AD kiracısına işaret etmek için yapılandırmayı güncelleştirin ve gerekli değişiklikleri yapın. Uygulama, test Azure AD kiracısındaki kullanıcılarla test edilebilir. Geliştirme süreci sırasında, istekleri ve yanıtları karşılaştırmak ve doğrulamak için [Fiddler](https://www.telerik.com/fiddler) gibi araçları kullanabilirsiniz.

Ayrı bir test kiracısı ayarlamak uygun değilse, bu aşamayı atlayın ve bir uygulamanın test örneğini hazırlayın ve aşağıdaki aşama 3 ' te açıklandığı şekilde üretim Azure AD kiracınıza işaret edin.

![Geçiş aşaması 2 ](media/migrate-adfs-apps-to-azure/stage2.jpg)

**3. aşama – üretim Azure kiracısına işaret eden test uygulaması**

Uygulamanın test örneğinizi Azure üretim örneğinizle göstermek için yapılandırmayı güncelleştirin. Artık üretim örneğindeki kullanıcılarla test edebilirsiniz. Gerekirse, kullanıcıların geçişini yaparken bu makalenin bölümünü gözden geçirin.

![Geçiş aşaması 3 ](media/migrate-adfs-apps-to-azure/stage3.jpg)

**4. aşama – üretim AD kiracısına işaret eden üretim uygulaması**

Üretim uygulamanızın yapılandırmasını, üretim Azure kiracınızı işaret etmek üzere güncelleştirin.

![Geçiş aşaması 1 ](media/migrate-adfs-apps-to-azure/stage4.jpg)

 AD FS ile kimlik doğrulayan uygulamalar, izinler için Active Directory grupları kullanabilir. Geçişe başlamadan önce şirket içi ortamınız ile Azure AD arasında kimlik verilerini eşitlemek için [Azure AD Connect eşitleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) kullanın. Uygulama geçirildiğinde aynı kullanıcılara erişim sağlayabilmeniz için geçişten önce bu grupları ve üyeliği doğrulayın.

### <a name="line-of-business-lob-apps"></a>İş kolu (LOB) uygulamaları

LOB uygulamaları, kuruluşunuz tarafından dahili olarak geliştirilir veya veri merkezinizde yüklü standart bir paketlenmiş ürün olarak sunulmaktadır. Örnek olarak, Windows Identity Foundation ve SharePoint uygulamaları (SharePoint Online değil) üzerinde oluşturulmuş uygulamalar sayılabilir. 

OAuth 2,0, OpenID Connect veya WS-Federation kullanan LOB uygulamaları Azure AD ile [uygulama kayıtları](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide-for-app-registrations-legacy-users)olarak tümleştirilebilir. SAML 2,0 veya WS-Federation kullanan özel uygulamaları [Azure Portal](https://portal.azure.com/)kurumsal uygulamalar sayfasında [Galeri dışı uygulamalar](https://docs.microsoft.com/azure/active-directory/manage-apps/add-non-gallery-app) olarak tümleştirin.

## <a name="saml-based-single-sign-on"></a>SAML tabanlı çoklu oturum açma

Kimlik doğrulaması için SAML 2,0 kullanan uygulamalar, [SAML tabanlı çoklu oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) (SAML tabanlı SSO) için yapılandırılabilir. [SAML tabanlı SSO](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)Ile, SAML talepleriniz içinde tanımladığınız kurallara göre kullanıcıları belirli uygulama rolleriyle eşleyebilirsiniz. 

SAML tabanlı çoklu oturum açma için bir SaaS uygulaması yapılandırmak üzere bkz. [SAML tabanlı çoklu oturum açmayı yapılandırma](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications). 

![SSO SAML Kullanıcı ekran görüntüleri ](media/migrate-adfs-apps-to-azure/sso-saml-user-attributes-claims.png)

 
Birçok SaaS uygulamasına, SAML tabanlı çoklu oturum açma yapılandırmasında adım adım kılavuzluk eden [uygulamaya özgü bir öğretici](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) vardır.

![Uygulama öğreticisi](media/migrate-adfs-apps-to-azure/app-tutorial.png)

Bazı uygulamalar kolayca geçirilebilir. Özel talepler gibi daha karmaşık gereksinimleri olan uygulamalar Azure AD ve/veya Azure AD Connect'te ek yapılandırma gerektirebilir. Desteklenen talep eşlemeleri hakkında bilgi için bkz. [Azure Active Directory talepler eşleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping).

Öznitelikleri eşlerken aşağıdaki sınırlamaları aklınızda bulundurun:

* AD FS ' de yayımlanmayan özniteliklerin hepsi, Azure AD 'de, bu öznitelikler eşitlense bile SAML belirteçlerine yaymayacak öznitelikler olarak görünür. Özniteliği düzenlediğinizde, değer açılan listesi, Azure AD 'de kullanılabilen farklı öznitelikleri gösterir. Gerekli bir özniteliğin (örneğin, samAccountName) Azure AD ile eşitlendiğinden emin olmak için [Azure AD Connect eşitleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) yapılandırmasını denetleyin. Azure AD 'de standart Kullanıcı şemasının bir parçası olmayan herhangi bir talebi göstermek için uzantı özniteliklerini kullanabilirsiniz.

* En yaygın senaryolarda, uygulama için yalnızca NameID talebi ve diğer yaygın kullanıcı tanımlayıcısı talepleri gerekir. Herhangi bir ek talep gerekip gerekmediğini belirlemek için AD FS hangi talepleri yayınlıyoruz ' i inceleyin.

* Bazı talepler Azure AD 'de korunduğu sürece tüm talepler sorun olabilir. 

* Şifrelenmiş SAML belirteçleri kullanma özelliği artık önizlemededir. Bkz. [nasıl yapılır: kurumsal uygulamalar IÇIN SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

 

### <a name="software-as-a-service-saas-apps"></a>Hizmet olarak yazılım (SaaS) uygulamaları

Kullanıcılarınızın Salesforce, ServiceNow veya Workday gibi SaaS uygulamalarında oturum açması ve AD FS ile tümleşikse, SaaS uygulamaları için Federasyon oturum açma 'yı kullanıyorsunuz. 

Birçok SaaS uygulaması, Azure AD 'de zaten yapılandırılabilir. Microsoft, [Azure AD uygulama galerisinde](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps)SaaS uygulamalarına önceden yapılandırılmış birçok bağlantıya sahiptir ve bu da geçişinizi kolaylaştırır. SAML 2,0 uygulamaları Azure AD Uygulama Galerisi aracılığıyla veya [Galeri dışı uygulamalar](https://docs.microsoft.com/azure/active-directory/manage-apps/add-non-gallery-app)olarak Azure AD ile tümleştirilebilir. 

OAuth 2,0 veya OpenID Connect kullanan uygulamalar, Azure AD ile aynı şekilde [uygulama kayıtları](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide-for-app-registrations-legacy-users)olarak tümleştirilebilir. Eski protokolleri kullanan uygulamalar Azure AD kimlik doğrulaması için [azure ad uygulama ara sunucusu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) kullanabilir.

SaaS uygulamalarınızı ekleme ile ilgili herhangi bir sorun için, [SaaS uygulaması tümleştirme destek diğer adına](mailto:SaaSApplicationIntegrations@service.microsoft.com)başvurabilirsiniz.

**SSO Için SAML imzalama sertifikaları**: imzalama sertifikaları, HERHANGI bir SSO dağıtımının önemli bir parçasıdır. Azure AD, SaaS uygulamalarınıza SAML tabanlı Federasyon SSO 'SU kurmak için imzalama sertifikaları oluşturur. Galeri veya Galeri dışı uygulamalar eklediğinizde, eklenen uygulamayı Federasyon SSO seçeneğini kullanarak yapılandırırsınız. [Azure Active Directory için bkz. Federasyon çoklu oturum açma için sertifikaları yönetme](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-certificates-for-federated-single-sign-on).

### <a name="apps-and-configurations-that-can-be-moved-today"></a>Bugün taşınabilecek uygulamalar ve Konfigürasyonlar

Bugün kolayca taşıyabileceğiniz uygulamalar, standart yapılandırma öğeleri ve talepler kümesini kullanan SAML 2,0 uygulamalarını içerir:

* Kullanıcı Asıl Adı

* E-posta adresi

* Ad

* Soyadı

* Azure AD posta özniteliği, posta öneki, çalışan kimliği, 1-15 uzantı öznitelikleri veya şirket içi **SamAccountName** özniteliği dahil olmak üzere SAML **NameID** gibi alternatif öznitelik. Daha fazla bilgi için bkz. [NameIdentifier talebini düzenleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

* Özel talepler.

Aşağıdakiler, Azure AD 'ye geçiş için ek yapılandırma adımları gerektirir:

* AD FS içindeki özel yetkilendirme veya Multi-Factor Authentication (MFA) kuralları. [Azure AD koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) özelliğini kullanarak bunları yapılandırırsınız.

* Birden çok yanıt URL uç noktası olan uygulamalar. Bunları Azure AD 'de PowerShell kullanarak veya Azure portal arabiriminde yapılandırırsınız.

* SAML sürüm 1.1 belirteçlerini gerektiren SharePoint uygulamaları gibi WS-Federasyon uygulamaları. PowerShell kullanarak el ile yapılandırabilirsiniz. Galeriden SharePoint ve SAML 1,1 uygulamaları için önceden tümleştirilmiş bir genel şablon da ekleyebilirsiniz. SAML 2,0 protokolünü destekliyoruz.

* Karmaşık talepler verme, kuralları dönüştürür. Desteklenen talep eşlemeleri hakkında bilgi için bkz.
   *  [Azure Active Directory’de talep eşlemesi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping) 
   * [Azure Active Directory 'de kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

 

### <a name="apps-and-configurations-not-supported-in-azure-ad-today"></a>Bugün Azure AD'de desteklenmeyen uygulamalar ve yapılandırmalar

Aşağıdaki özellikleri gerektiren uygulamalar bugün geçirilemez.

**Protokol özellikleri**

* WS-Trust ActAs düzenine yönelik destek

* SAML yapı çözümlemesi

* İmzalı SAML isteklerinin imza doğrulaması  
İmzalı isteklerin kabul edildiğini, ancak imzanın doğrulanmadığını unutmayın.  
Azure AD 'nin belirteci yalnızca uygulamada önceden yapılandırılmış uç noktalara döndürecek şekilde, çoğu durumda imza doğrulaması gerekli değildir.

**Belirteç özellikleri içindeki talepler**

* Azure ad dizini dışındaki öznitelik depolarından gelen talepler, verilerin Azure AD ile eşitlenmediği durumlar değildir. Daha fazla bilgi için bkz. [Azure AD eşitleme API 'sine genel bakış](https://docs.microsoft.com/graph/api/resources/synchronization-overview?view=graph-rest-beta).

* Dizin çoklu değer özniteliklerinin verilmesi. Örneğin, şu anda proxy adresleri için çok değerli bir talep yayınlıyoruz.

## <a name="map-app-settings-from-ad-fs-to-azure-ad"></a>AD FS uygulama ayarlarını Azure AD 'ye eşleyin

Geçiş işlemi, uygulamanın şirket içinde nasıl yapılandırıldığını değerlendirerek ve bu yapılandırmayı Azure AD'ye eşleyerek başlar. AD FS ve Azure AD aynı şekilde çalışır, bu nedenle güven, oturum açma ve oturum kapatma URL 'Leri ve tanımlayıcılar yapılandırma kavramları her iki durumda da geçerlidir. Azure AD 'de kolayca yapılandırabilmeniz için uygulamalarınızın AD FS yapılandırma ayarlarını belgeleyin.

### <a name="map-app-configuration-settings"></a>Uygulama yapılandırma ayarlarını eşleme

Aşağıdaki tabloda, Azure AD kurumsal uygulamasına AD FS bağlı olan taraf güveni arasındaki ayarların en yaygın eşleştirmesinin bazıları açıklanmaktadır:

* AD FS – uygulama için AD FS bağlı olan taraf güveninde ayarı bulun. Bağlı olan tarafa sağ tıklayın ve Özellikler ' i seçin. 

* Azure AD – ayar, her uygulamanın çoklu oturum açma özelliklerinde [Azure Portal](https://portal.azure.com/) yapılandırılır.

| Yapılandırma ayarı| AD FS| Azure AD 'de yapılandırma| SAML belirteci |
| - | - | - | - |
| **Uygulama oturum açma URL'si** <p>Bir hizmet sağlayıcısı (SP) tarafından başlatılan bir SAML akışında uygulamada oturum açmak için kullanıcının URL 'SI.| Yok| SAML tabanlı oturum açma işleminden temel SAML yapılandırması 'nı açın| Yok |
| **Uygulama yanıtı URL 'SI** <p>Kimlik sağlayıcısının (IDP 'nin) perspektifinden uygulamanın URL 'SI. IDP, Kullanıcı IDP 'de oturum açtıktan sonra kullanıcıyı ve belirteci buraya gönderir.  Bu, **SAML onaylama tüketici uç noktası**olarak da bilinir.| **Uç noktalar** sekmesini seçin| SAML tabanlı oturum açma işleminden temel SAML yapılandırması 'nı açın| SAML belirtecindeki hedef öğe. Örnek değer: `https://contoso.my.salesforce.com` |
| **Uygulama oturumu kapatma URL'si** <p>Bu, Kullanıcı bir uygulamadan oturumu kapattığında "oturum kapatma temizleme" isteklerinin gönderildiği URL 'dir. IDP, kullanıcıyı diğer tüm uygulamalardan da oturumu kapatmak için isteği gönderir.| **Uç noktalar** sekmesini seçin| SAML tabanlı oturum açma işleminden temel SAML yapılandırması 'nı açın| Yok |
| **Uygulama tanımlayıcısı** <p>Bu, IDP 'nin perspektifinden uygulama tanımlayıcısıdır. Oturum açma URL 'SI değeri genellikle tanımlayıcı için kullanılır (ancak her zaman kullanılmaz).  Bazen uygulama bunu "varlık KIMLIĞI" olarak çağırır.| **Tanımlayıcılar** sekmesini seçin|SAML tabanlı oturum açma işleminden temel SAML yapılandırması 'nı açın| SAML belirtecindeki **hedef kitle** öğesiyle eşlenir. |
| **Uygulama Federasyon meta verileri** <p>Bu, uygulamanın Federasyon meta verilerinin konumudur. IdP bunu, uç noktalar veya şifreleme sertifikaları gibi belirli yapılandırma ayarlarını otomatik olarak güncelleştirmek için kullanır.| **İzleme** sekmesini seçin| Yok. Azure AD uygulama Federasyon meta verilerini doğrudan kullanmayı desteklemez. Federasyon meta verilerini el ile içeri aktarabilirsiniz.| Yok |
| **Kullanıcı tanımlayıcısı/ad KIMLIĞI** <p>Azure AD'den veya AD FS'den kullanıcı tanımlayıcınızı uygulamanıza benzersiz olarak göstermek için kullanılan öznitelik.  Bu öznitelik genellikle kullanıcının UPN 'si veya e-posta adresidir.| Talep kuralları. Çoğu durumda, talep kuralı NameIdentifier ile biten bir türle bir talep yayınlar.| Tanımlayıcıyı, **Kullanıcı öznitelikleri ve talepler**başlığı altında bulabilirsiniz. Varsayılan olarak UPN kullanılır| SAML belirtecindeki **NameID** öğesiyle eşlenir. |
| **Diğer talepler** <p>IDP 'den uygulamaya genellikle gönderilen diğer talep bilgilerine örnek olarak ad, soyadı, e-posta adresi ve grup üyeliği dahildir.| AD FS'de, bunu bağlı olan tarafta diğer talep kuralları olarak bulabilirsiniz.| Tanımlayıcıyı, **Kullanıcı öznitelikleri & talepler**altında bulabilirsiniz. **Görünüm**'ü seçin ve diğer tüm kullanıcı özniteliklerini düzenleyin.| Yok |


### <a name="map-identity-provider-idp-settings"></a>Eşleme kimlik sağlayıcısı (IDP) ayarları

Uygulamalarınızı, SSO için AD FS karşı Azure AD 'ye işaret etmek üzere yapılandırın. Burada SAML protokolünü kullanan SaaS uygulamalarına odaklanıyoruz. Ancak, bu kavram aynı zamanda özel LOB uygulamalarına da genişletilir.

> [!NOTE]
> Azure AD 'nin yapılandırma değerleri, Azure kiracı KIMLIĞINIZIN {Tenant-ID} ' ın yerini aldığı ve uygulama KIMLIĞI {Application-id} ' nin yerini aldığı düzene uyar. Bu bilgileri [Azure portal](https://portal.azure.com/) Azure Active Directory > Özellikler altında bulabilirsiniz: 

* Kiracı KIMLIĞINIZI görmek için dizin KIMLIĞI ' ni seçin. 

* Uygulama kimliğinizi görmek için uygulama KIMLIĞI ' ni seçin.

 Yüksek düzeyde, aşağıdaki anahtar SaaS uygulamaları yapılandırma öğelerini Azure AD 'ye eşleyin. 

 

| Öğe| Yapılandırma Değeri |
| - | - |
| Kimlik sağlayıcısı veren| https: \/ /STS.Windows.net/{Tenant-id}/ |
| Kimlik sağlayıcısı oturum açma URL 'SI| [https://login.microsoftonline.com/{tenant-id}/saml2](https://login.microsoftonline.com/{tenant-id}/saml2) |
| Kimlik sağlayıcısı oturum kapatma URL 'SI| [https://login.microsoftonline.com/{tenant-id}/saml2](https://login.microsoftonline.com/{tenant-id}/saml2) |
| Federasyon meta veri konumu| [https://login.windows.net/{tenant-id}/federationmetadata/2007-06/federationmetadata.xml?appid={application-id}](https://login.windows.net/{tenant-id}/federationmetadata/2007-06/federationmetadata.xml?appid={application-id}) |


### <a name="map-sso-settings-for-saas-apps"></a>SaaS uygulamaları için SSO ayarlarını eşleme

SaaS uygulamalarının kimlik doğrulama isteklerinin nereden gönderileceğini ve alınan belirteçleri nasıl doğrulayacağını bilmeleri gerekir. Aşağıdaki tabloda, uygulamada SSO ayarlarını yapılandırma ve AD FS ve Azure AD içindeki değerleri ya da konumları açıklanmaktadır.

| Yapılandırma ayarı| AD FS| Azure AD 'de yapılandırma |
| - | - | - |
| **IDP oturum açma URL 'SI** <p>Uygulamanın perspektifinden IDP 'nin oturum açma URL 'SI (kullanıcının oturum açmak için yeniden yönlendirildiği konum).| AD FS oturum açma URL 'SI, AD FS Federasyon Hizmeti adı ve ardından "/adfs/ls/." <p>Örneğin, `https://fs.contoso.com/adfs/ls/`| {Kiracı-kimliği} öğesini kiracı KIMLIĞINIZLE değiştirin. <p> SAML-P protokolünü kullanan uygulamalar için:[https://login.microsoftonline.com/{tenant-id}/saml2](https://login.microsoftonline.com/{tenant-id}/saml2) <p>WS-Federation protokolünü kullanan uygulamalar için:[https://login.microsoftonline.com/{tenant-id}/wsfed](https://login.microsoftonline.com/{tenant-id}/wsfed) |
| **IDP oturum kapatma URL 'SI**<p>Uygulamanın perspektifinden IDP 'nin oturum kapatma URL 'SI (uygulamanın oturumu kapatmak için seçerken kullanıcının yeniden yönlendirildiği).| Oturum kapatma URL 'SI, oturum açma URL 'si ile aynı URL ya da "WA = wsignout 1.0" eklenmiş URL. Örneğin, `https://fs.contoso.com/adfs/ls/?wa=wsignout1.0`| {Kiracı-kimliği} öğesini kiracı KIMLIĞINIZLE değiştirin.<p>SAML-P protokolünü kullanan uygulamalar için:<p>[https://login.microsoftonline.com/{tenant-id}/saml2](https://login.microsoftonline.com/{tenant-id}/saml2) <p> WS-Federation protokolünü kullanan uygulamalar için:[https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0](https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0) |
| **Belirteç imzalama sertifikası**<p>IDP, verilen belirteçleri imzalamak için sertifikanın özel anahtarını kullanır. Belirtecin, uygulamanın güvenmek üzere yapılandırıldığı IdP'den geldiğini doğrular.| AD FS belirteç imzalama sertifikası AD FS Yönetimi'nde **Sertifikalar**'ın altında bulabilirsiniz.| Uygulamanın **Çoklu oturum açma özelliklerinde** , **SAML imzalama sertifikası**başlığı altında Azure Portal bulun. Sertifikayı buradan indirip uygulamaya yükleyebilirsiniz.  <p>Uygulamanın birden fazla sertifikası varsa, Federasyon meta verileri XML dosyasındaki tüm sertifikaları bulabilirsiniz. |
| **Tanımlayıcı/"veren"**<p>Uygulamanın perspektifinden IDP 'nin tanımlayıcısı (bazen "verenin KIMLIĞI" olarak da adlandırılır).<p>SAML belirtecinde, değer veren öğesi olarak görünür.| AD FS için tanımlayıcı, genellikle **> Federasyon Hizmeti özelliklerini Düzenle**' nin altındaki AD FS yönetiminde Federasyon Hizmeti tanımlayıcısıdır. Örneğin, `http://fs.contoso.com/adfs/services/trust`| {Kiracı-kimliği} öğesini kiracı KIMLIĞINIZLE değiştirin.<p>https: \/ /STS.Windows.net/{Tenant-id}/ |
| **IDP Federasyon meta verileri**<p>IDP 'nin genel kullanıma açık Federasyon meta verilerinin konumu. (Bazı uygulamalar federasyon meta verilerini yönetici yapılandırma URL'lerine, tanımlayıcıya ve bağımsız olarak belirteç imzalama sertifikasına alternatif olarak kullanılır)| **Hizmet > uç noktaları > meta veri > türü: Federasyon meta verileri**altında AD FS yönetiminde Federasyon meta verileri URL 'sini AD FS bulun. Örneğin, `https://fs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml`| Azure AD için buna karşılık gelen değer, düzen izler [https://login.microsoftonline.com/{TenantDomainName}/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/{TenantDomainName}/FederationMetadata/2007-06/FederationMetadata.xml) . {TenantDomainName} öğesini kiracınızın adı "contoso.onmicrosoft.com" biçiminde değiştirin.   <p>Daha fazla bilgi için bkz. [Federasyon meta verileri](https://docs.microsoft.com/azure/active-directory/azuread-dev/azure-ad-federation-metadata). |


## <a name="represent-ad-fs-security-policies-in-azure-ad"></a>Azure AD 'de AD FS güvenlik ilkelerini temsil etme

Uygulama kimlik doğrulamasını Azure AD 'ye taşırken, mevcut güvenlik ilkelerinden Azure AD 'de bulunan eşdeğer veya alternatif çeşitlere eşlemeler oluşturun. Uygulama sahipleriniz için gereken güvenlik standartları toplantılarından dolayı bu eşlemelerin yapılabilmesini sağlamak, uygulama geçişinin geri kalanını önemli ölçüde daha kolay hale getirir.

Her kural türü ve örnekleri için, kuralın AD FS nasıl göründüğünü, AD FS kural diliyle eşdeğer kodu ve Azure AD 'de bu haritanın nasıl göründüğünü öneririz.

### <a name="map-authorization-rules"></a>Eşleme yetkilendirme kuralları

Aşağıda AD FS ' deki yetkilendirme kuralı türlerinin örnekleri verilmiştir ve bunları Azure AD ile nasıl eşleyebileceğiniz gösterilmektedir:

#### <a name="example-1-permit-access-to-all-users"></a>Örnek 1: tüm kullanıcılara erişime Izin ver

Tüm kullanıcıların erişimine izin ver AD FS şöyle görünür: 

![Geçiş aşaması 1 ](media/migrate-adfs-apps-to-azure/sso-saml-user-attributes-claims.png)


Bu, Azure AD ile aşağıdaki yollarla eşlenir:

[Azure Portal](https://portal.azure.com/):
* Seçenek 1: Kullanıcı atamasını gerekli ayarla ![SaaS uygulamaları için erişim denetimi ilkesini düzenleme ](media/migrate-adfs-apps-to-azure/permit-access-to-all-users-2.png)

    Kullanıcı Ataması gerekli anahtarı Evet olarak ayarlamanın, kullanıcıların erişim kazanmak için uygulamaya atanmasını gerektirdiğini unutmayın. Hayır olarak ayarlandığında, tüm kullanıcıların erişimi vardır. Bu anahtar, Uygulamalarım deneyimindeki kullanıcılar için neler olduğunu denetlemez. 

 
* Seçenek 2: kullanıcılar ve Gruplar sekmesinde, uygulamanızı "tüm kullanıcılar" otomatik grubuna atayın. <p>
Varsayılan ' tüm kullanıcılar ' grubunun kullanılabilmesi için Azure AD kiracınızda [dinamik grupları etkinleştirmeniz](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule) gerekir.

   ![Azure AD 'de SaaS uygulamalarım ](media/migrate-adfs-apps-to-azure/permit-access-to-all-users-3.png)


#### <a name="example-2-allow-a-group-explicitly"></a>Örnek 2: bir gruba açıkça Izin verme

AD FS açık grup yetkilendirmesi:


![verme yetkilendirme kuralları ](media/migrate-adfs-apps-to-azure/allow-a-group-explicitly-1.png)


Kuralın Azure AD 'ye nasıl eşleştiği aşağıda gösterilmiştir:

[Azure Portal](https://portal.azure.com/), önce AD FS ' dan kullanıcı grubuna karşılık gelen [bir Kullanıcı grubu oluşturacak](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal) ve ardından bu gruba uygulama izinleri atayacaksınız:

![Atama Ekle ](media/migrate-adfs-apps-to-azure/allow-a-group-explicitly-2.png)


#### <a name="example-3-authorize-a-specific-user"></a>Örnek 3: belirli bir kullanıcıyı yetkilendirme

AD FS açık kullanıcı yetkilendirmesi:

![verme yetkilendirme kuralları ](media/migrate-adfs-apps-to-azure/authorize-a-specific-user-1.png)

Kuralın Azure AD 'ye nasıl eşleştiği aşağıda gösterilmiştir:

[Azure Portal](https://portal.azure.com/), uygulamanın atama Ekle sekmesinden aşağıda gösterildiği gibi bir kullanıcı ekleyin:

![Azure 'da SaaS uygulamalarım ](media/migrate-adfs-apps-to-azure/authorize-a-specific-user-2.png)

 
### <a name="map-multi-factor-authentication-rules"></a>Harita Multi-Factor Authentication kuralları 

AD FS ile federe olduğunuz için [Multi-Factor Authentication (MFA)](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication) ve AD FS şirket içi dağıtımı geçiş sonrasında çalışmaya devam edecektir. Ancak Azure 'un Azure AD 'nin koşullu erişim iş akışlarına bağlı olan yerleşik MFA özelliklerine geçiş yapmayı düşünün. 

Aşağıda AD FS MFA kuralları türlerinin örnekleri verilmiştir ve bunları farklı koşullara göre Azure AD ile nasıl eşleyebileceğiniz gösterilmektedir:

AD FS MFA kuralı ayarları:

![Azure AD MFA ayarları](media/migrate-adfs-apps-to-azure/mfa-location-1.png)


#### <a name="example-1-enforce-mfa-based-on-usersgroups"></a>Örnek 1: kullanıcılara/gruplara göre MFA 'yı zorlama

Kullanıcı/gruplar Seçicisi, MFA 'yı grup başına (Grup SID) veya Kullanıcı başına (birincil SID) temelinde zorlamanıza olanak tanıyan bir kuraldır. Kullanıcı/Grup atamalarından ayrı olarak, AD FS MFA yapılandırma kullanıcı arabirimi işlevindeki tüm ek onay kutuları, Kullanıcı/Grup kuralı zorlandıktan sonra değerlendirilen ek kurallar olarak değerlendirilir. 


Azure AD 'de bir kullanıcı veya grup için MFA kurallarını belirtin:

1. Yeni bir [koşullu erişim ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json)oluşturun.

2. **Atamalar**' ı seçin. MFA 'yı zorlamak istediğiniz kullanıcıları veya grupları ekleyin.

3. **Erişim denetimleri** seçeneklerini aşağıda gösterildiği gibi yapılandırın:  
‎

![AAD MFA ayarları](media/migrate-adfs-apps-to-azure/mfa-usersorgroups.png)

 
 #### <a name="example-2-enforce-mfa-for-unregistered-devices"></a>Örnek 2: kayıtlı olmayan cihazlar için MFA zorlama

Azure AD 'de kayıtlı olmayan cihazlar için MFA kurallarını belirtin:

1. Yeni bir [koşullu erişim ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json)oluşturun.

2. **Atamaları** **tüm kullanıcılara**ayarlayın.

3. **Erişim denetimleri** seçeneklerini aşağıda gösterildiği gibi yapılandırın:  
‎

![AAD MFA ayarları](media/migrate-adfs-apps-to-azure/mfa-unregistered-devices.png)

 
Seçilen denetimlerden birini gerektirmek için birden çok denetim Için seçeneğini belirlediğinizde, onay kutusu tarafından belirtilen koşullardan herhangi biri Kullanıcı tarafından bulunursa, uygulamanıza erişim izni verilir demektir.

#### <a name="example-3-enforce-mfa-based-on-location"></a>Örnek 3: MFA 'yı konuma göre zorla

Azure AD 'de bir kullanıcının konumuna göre MFA kurallarını belirtin:

1. Yeni bir [koşullu erişim ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json)oluşturun.

1. **Atamaları** **tüm kullanıcılara**ayarlayın.

1. [Azure AD 'de adlandırılmış konumları yapılandırın](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) , aksi takdirde kurumsal ağınızın içinden Federasyon güvenilir. 

1. MFA 'yı uygulamak istediğiniz konumları belirtmek için **koşullar kurallarını** yapılandırın.

![Azure AD MFA ayarları](media/migrate-adfs-apps-to-azure/mfa-location-1.png)

5. **Erişim denetimleri** seçeneklerini aşağıda gösterildiği gibi yapılandırın:


![Harita erişim denetim ilkeleri](media/migrate-adfs-apps-to-azure/mfa-location-2.png)

 
### <a name="map-emit-attributes-as-claims-rule"></a>Yayma özniteliklerini talep kuralı olarak eşle

Özniteliklerin AD FS nasıl eşlendiğine ilişkin bir örnek aşağıda verilmiştir:

![Azure AD MFA ayarları](media/migrate-adfs-apps-to-azure/map-emit-attributes-as-claimsrule-1.png)


Kuralın Azure AD 'ye nasıl eşleştiği aşağıda gösterilmiştir:

[Azure Portal](https://portal.azure.com/) **Kurumsal uygulamalar**, **Çoklu oturum açma**' yı seçin ve **SAML belirteci özniteliklerini** aşağıda gösterildiği gibi ekleyin:

![Azure AD MFA ayarları](media/migrate-adfs-apps-to-azure/map-emit-attributes-as-claimsrule-2.png)



### <a name="map-built-in-access-control-policies"></a>Yerleşik erişim denetimi ilkelerini eşleme

AD FS 2016, aralarından seçim yapabileceğiniz çeşitli yerleşik erişim denetimi ilkelerine sahiptir:

![Azure AD yerleşik Access Control](media/migrate-adfs-apps-to-azure/map-builtin-access-control-policies-1.png)

 
Azure AD 'de yerleşik ilkeleri uygulamak için, [Yeni bir koşullu erişim ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json) kullanabilir ve erişim denetimlerini yapılandırabilir ya da erişim denetim ilkelerini yapılandırmak için AD FS 2016 ' de özel ilke tasarımcısını kullanabilirsiniz. Kural Düzenleyicisi, her türlü permütasyon oluşturmanıza yardımcı olabilecek, Izin verme ve dışındaki seçeneklerin kapsamlı bir listesini içerir.

![Azure AD erişim denetimi ilkeleri](media/migrate-adfs-apps-to-azure/map-builtin-access-control-policies-2.png)



Bu tabloda bazı yararlı Izin ve seçenekler dışında, Azure AD ile nasıl eşleştireceğiz listelenmiştir. 


| | Azure AD 'de Izin verme seçeneği nasıl yapılandırılır?| Azure AD 'de except seçeneği nasıl yapılandırılır? |
| - | - | - |
| Belirli bir ağdan| Azure AD 'de [adlandırılmış konuma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations) eşlenir| [Güvenilen konumlar](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition) için **Dışla** seçeneğini kullanın |
| Belirli gruplardan| [Kullanıcı/Grup ataması ayarlama](https://docs.microsoft.com/azure/active-directory/manage-apps/assign-user-or-group-access-portal)| Kullanıcılar ve gruplar 'daki **hariç tut** seçeneğini kullanın |
| Belirli güven düzeyine sahip cihazlardan| Bunu ' cihaz durumu ' denetiminden atama-> koşulları altında ayarla| Cihaz durumu koşulunun altındaki **Dışla** seçeneğini kullanın ve **tüm cihazları** ekleyin |
| Istekteki belirli taleplerle| Bu ayar geçirilemez| Bu ayar geçirilemez |


Azure portalında Güvenilen konumların dışlama seçeneğinin nasıl yapılandırılacağı hakkında bir örnek:

![Erişim denetimi ilkelerinin eşleme ekran görüntüsü](media/migrate-adfs-apps-to-azure/map-builtin-access-control-policies-3.png)



## <a name="transition-users-from-ad-fs-to-azure-ad"></a>Kullanıcıların AD FS 'den Azure AD 'ye geçiş

### <a name="sync-ad-fs-groups-in-azure-ad"></a>Azure AD 'de AD FS gruplarını eşitleme

Yetkilendirme kurallarını eşlediğinizde AD FS ile kimlik doğrulayan uygulamalar izinler için Active Directory grupları kullanabilir. Böyle bir durumda, uygulamaları geçirmeden önce bu grupları Azure AD ile eşitlemek için [Azure AD Connect](https://go.microsoft.com/fwlink/?LinkId=615771) kullanın. Uygulama geçirildiğinde aynı kullanıcılara erişim sağlayabilmeniz için geçişten önce bu grupları ve üyeliği doğruladığınızdan emin olun.

Daha fazla bilgi için, [Active Directory 'ten eşitlenen grup özniteliklerini kullanma önkoşulları](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-group-claims)konusuna bakın.

### <a name="setup-user-self-provisioning"></a>Kullanıcı kendi kendini sağlamayı ayarla 

Bazı SaaS uygulamaları, kullanıcıların uygulamada ilk kez oturum açtıklarında kendi kendine sağlama yeteneğini destekler. Azure Active Directory (Azure AD) ' de, uygulama sağlama terimi, kullanıcıların erişmesi gereken bulut ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) uygulamalarında kullanıcı kimliklerinin ve rollerinin otomatik olarak oluşturulmasını ifade eder. Geçirilen kullanıcıların SaaS uygulamasında bir hesabı zaten olacaktır. Geçiş işleminden sonra eklenen tüm yeni kullanıcılar sağlanmalıdır. Uygulama geçirildikten sonra [SaaS uygulama sağlamasını](https://docs.microsoft.com/azure/active-directory/app-provisioning/user-provisioning) test edin.

### <a name="sync-external-users-in-azure-ad"></a>Azure AD 'de dış kullanıcıları eşitleme

Mevcut dış kullanıcılarınız, AD FS içinde iki ana şekilde ayarlanabilir:

#### <a name="external-users-with-a-local-account-within-your-organization"></a>Kuruluşunuz içinde yerel bir hesabı olan dış kullanıcılar

Bu hesapları, iç Kullanıcı hesaplarınızın çalıştığı şekilde kullanmaya devam edersiniz. Bu dış Kullanıcı hesaplarının, kuruluşunuzda bir ilke adı vardır, ancak hesabın e-postası dışarıdan işaret edebilir. Geçişiniz ile ilerleyerek, bu kullanıcıları bu tür bir kimlik kullanılabilir olduğunda kendi şirket kimliklerini kullanacak şekilde geçirerek [Azure AD B2B](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b) 'nin sunduğu avantajlardan yararlanabilirsiniz. Bu, genellikle kendi kurumsal oturum açma bilgileriyle oturum açtığı için bu kullanıcılar için oturum açma sürecini kolaylaştırır. Kuruluşunuzun yönetimi, artık dış kullanıcılar için hesapları yönetmeye gerek duyulmayacak şekilde oluşturulur.

#### <a name="federated-external-identities"></a>Federasyon dış kimlikleri

Şu anda bir dış kuruluşla Federasyonunuzda yapmanız gereken birkaç yaklaşım vardır:

* [Azure portal Azure ACTIVE DIRECTORY B2B işbirliği kullanıcıları ekleyin](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator). Bireysel üyelerin, kullanıldıkları uygulamaları ve varlıkları kullanmaya devam edebilmesi için, Azure AD Yönetim portalından, iş ortağı kuruluşuna şirket içinde B2B işbirliği davetlerinin proaktif olarak gönderilmesini sağlayabilirsiniz. 

* B2B davetiyesi API 'sini kullanarak iş ortağı kuruluşunuzda bireysel kullanıcılar için bir istek üreten [self SERVIS B2B kaydolma iş akışı oluşturun](https://docs.microsoft.com/azure/active-directory/b2b/self-service-portal) .

Mevcut dış kullanıcılarınızın nasıl yapılandırıldığına bakılmaksızın, grup üyeliğinde veya belirli izinlerde kendi hesabıyla ilişkili izinleri olabilir. Bu izinlerin geçirilmesi veya temizlenmesi gerekip gerekmediğini değerlendirin. Bir dış kullanıcıyı temsil eden kuruluşunuzdaki hesapların, Kullanıcı bir dış kimliğe geçirildikten sonra devre dışı bırakılması gerekir. Kaynaklarınızın bağlanmasına yönelik bir kesinti olabileceğinden, geçiş işlemi iş ortaklarınızla birlikte ele alınmalıdır.

## <a name="migrate-and-test-your-apps"></a>Uygulamalarınızı geçirin ve test edin

Bu makalede ayrıntılı olarak açıklanan geçiş sürecini izleyin.

Ardından, geçişin başarılı olup olmadığını test etmek için [Azure Portal](https://aad.portal.azure.com/) gidin. Aşağıdaki yönergeleri izleyin:
1. **Kurumsal uygulamalar**  >  **tüm uygulamalar** ' ı seçin ve listeden uygulamanızı bulun.

1. **Manage**  >  Uygulamaya en az bir kullanıcı veya grup atamak için**kullanıcıları ve grupları yönet '** i seçin.

1. **Manage**  >  **Koşullu erişimi**Yönet ' i seçin. İlke listenizi gözden geçirin ve [koşullu erişim ilkesiyle](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)uygulamaya erişimi engellemediğinizden emin olun.

Uygulamanızı nasıl yapılandırdığınıza bağlı olarak, SSO 'nun düzgün çalıştığını doğrulayın. 

| Kimlik doğrulaması türü| Test Etme |
| - | - |
| OAuth/OpenID Connect| **Kurumsal uygulamalar > izinler** ' i seçin ve uygulamanızın kullanıcı ayarlarında kuruluşunuzda kullanılacak uygulamaya onay aldığınızdan emin olun.  
‎ |
| SAML tabanlı SSO| **Çoklu oturum açma**altında bulunan [Test SAML ayarları](https://docs.microsoft.com/azure/active-directory/develop/howto-v1-debug-saml-sso-issues) düğmesini kullanın.  
‎ |
| Parola tabanlı SSO| [Uygulamaps güvenli oturum açma](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction) [-](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction) [uzantısını](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)indirip yükleyin. Bu uzantı, bir SSO işlemi kullanmanızı gerektiren kuruluşunuzun bulut uygulamalarından herhangi birini başlatmanıza yardımcı olur.  
‎ |
| Uygulama Ara Sunucusu| Bağlayıcının çalıştığından ve uygulamanıza atandığından emin olun. Daha fazla yardım için [uygulama proxy sorun giderme kılavuzunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot) [ ](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot)ziyaret edin.  
‎ |

> [!NOTE]
> Eski AD FS ortamından gelen tanımlama bilgileri kullanıcının makinelerinde kalıcı olmaya devam edecektir. Bu tanımlama bilgileri, kullanıcıların eski AD FS oturum açma ortamına yönlendirilebilmesi için yeni Azure AD oturumuna karşı geçiş ile ilgili sorunlara neden olabilir. Kullanıcı tarayıcısı tanımlama bilgilerini el ile veya bir komut dosyası kullanarak temizlemeniz gerekebilir. System Center Configuration Manager veya benzer bir platform da kullanabilirsiniz.

### <a name="troubleshoot"></a>Sorun giderme

Geçirilen uygulamaların testinizden herhangi bir hata varsa, sorun giderme, mevcut AD FS bağlı olan taraflara geri dönmeden önce ilk adım olabilir. [Azure Active Directory içindeki uygulamalarda SAML tabanlı çoklu oturum açma hatalarını ayıklama](https://docs.microsoft.com/azure/active-directory/azuread-dev/howto-v1-debug-saml-sso-issues)bölümüne bakın.

### <a name="rollback-migration"></a>Geçişi geri alma

Geçiş başarısız olursa, mevcut bağlı olan taraflardan AD FS sunucularında ayrılmanız ve bağlı olan taraflara erişimi kaldırmanız önerilir. Bu, dağıtım sırasında gerekirse hızlı geri dönüşe izin verir.

### <a name="end-user-communication"></a>Son Kullanıcı iletişimi

Planlı kesinti penceresinin kendisi en düşük olabilmesine karşın, kesilen AD FS Azure AD 'ye alırken bu zaman çerçevelerini çalışanlara karşı nasıl iletişim kurmaya devam etmelisiniz. Uygulama deneyiminizin bir geri bildirim düğmesine veya sorunlar için yardım masasına yönelik işaretçilere sahip olduğundan emin olun.

Dağıtım tamamlandıktan sonra, başarılı dağıtım kullanıcılarına bir iletişim gönderebilir ve bunları yapması gereken yeni adımlara hatırlatabilir.

* Kullanıcılara, geçirilen tüm uygulamalara erişmek için [erişim panelini](https://myapps.microsoft.com) kullanmalarını söyleyin. 

* Kullanıcılara MFA ayarlarını güncelleştirmesi gerekebilecek kullanıcıları hatırlatın. 

* Self servis parola sıfırlama dağıtılırsa, kullanıcıların kimlik doğrulama yöntemlerini güncelleştirmesi veya doğrulaması gerekebilir. Bkz. [MFA](https://aka.ms/mfatemplates) ve [SSPR](https://aka.ms/ssprtemplates) Son Kullanıcı iletişim şablonları.

Dış kullanıcılarla iletişim: Bu Kullanıcı grubu genellikle sorunlar durumunda en kritik düzeyde etkilendi. Bu özellikle, güvenlik sonrası, dış iş ortakları için farklı koşullu erişim kuralları veya risk profilleri kümesini belirlemesi durumunda geçerlidir. Dış iş ortaklarının, bulut geçiş zamanlamasının farkında olduğundan ve dış işbirliğine özgü tüm akışları test eden bir pilot dağıtıma katılması gereken bir zaman dilimi bulunduğundan emin olun. Son olarak, sorun olması durumunda yardım masasına erişmek için bir yol olduğundan emin olun.

## <a name="next-steps"></a>Sonraki Adımlar
[Uygulama kimlik doğrulamasını Azure AD 'ye geçirme](https://aka.ms/migrateapps/whitepaper)<p>
[Koşullu erişimi](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) ve [MFA](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) 'yı ayarlama
