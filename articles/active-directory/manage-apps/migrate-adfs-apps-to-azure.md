---
title: Uygulama kimlik doğrulamasını AD FS'den Azure Etkin Dizini'ne taşıma
description: Bu makale, kuruluşların federe SaaS uygulamalarına odaklanarak uygulamaları Azure AD'ye nasıl taşıyacaklarını anlamalarına yardımcı olmak amacıyla tasarlanmıştır.
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 04/01/2020
ms.author: baselden
ms.collection: M365-identity-device-management
ms.openlocfilehash: bcb39606cdbb6488ccdee2828029d3617d689508
ms.sourcegitcommit: df8b2c04ae4fc466b9875c7a2520da14beace222
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80892125"
---
# <a name="moving-application-authentication-from-active-directory-federation-services-to-azure-active-directory"></a>Uygulama kimlik doğrulamasını Active Directory Federation Services'den Azure Etkin Dizini'ne taşıma

[Azure Active Directory (Azure AD),](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) çalışanlarınıza, iş ortaklarınıza ve müşterilerinize uygulamalara erişmelerini ve herhangi bir platform ve cihazdan işbirliği yapmalarını sağlayan evrensel bir kimlik platformu sunar. Azure AD, [kimlik yönetimi özelliklerinden oluşan tam](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis)bir pakete sahiptir. Uygulamanızın (uygulama) kimlik doğrulamasını ve yetkilendirmenizi Azure AD'ye standartlaştırmak, bu özelliklerin sağladığı avantajları sağlar. 

> [!NOTE]
> Bu makalede, uygulama kimlik doğrulaması şirket içi Active Directory ve Active Directory Federation Services'den Azure AD'ye taşınması üzerinde duruluyor. Bu hareketi planlamaya genel bir bakış için lütfen azure [AD'ye uygulama kimlik doğrulamasını geçiren](https://aka.ms/migrateapps/whitepaper) beyaz makaleye bakın. Teknik incelemede geçişin, testin ve öngörülerin nasıl planlanı anlatılmaktadır.

## <a name="introduction"></a>Giriş

Kullanıcı hesapları içeren bir şirket dizininvarsa, büyük olasılıkla kullanıcıların kimlik doğrulaması yaptığı birçok uygulamanız vardır. Bu uygulamaların her biri, kullanıcıların kimliklerini kullanarak erişebilmek için yapılandırılmıştır. 


Kullanıcılar ayrıca şirket içi Active Directory ile doğrudan kimlik doğrulaması yapabilir. Active Directory Federation Services (AD FS), şirket içi kimlik hizmetine dayalı bir standarttır. AD FS, kullanıcıların her uygulamada ayrı oturum açmalarına gerek kalmadan, güvenilen iş ortakları arasında tek oturum açma (SSO) işlevini kullanma olanağını genişletir. Bu Federasyon olarak bilinir.

Birçok kuruluş, Office 365 ve Azure AD tabanlı uygulamaların yanı sıra doğrudan AD FS'ye aktarılan Hizmet Olarak Yazılım (SaaS) veya özel İşletmeler Hattı (LOB) uygulamalarına sahiptir. 

![Doğrudan şirket içinde bağlantılı uygulamalar](media/migrate-adfs-apps-to-azure/app-integration-before-migration1.png)

**Uygulama güvenliğini artırmak için amacınız, şirket içi ve bulut ortamlarınızda tek bir erişim denetimi ve ilkeleri kümesine sahip olmaktır.** 

![Azure AD üzerinden bağlanan uygulamalar](media/migrate-adfs-apps-to-azure/app-integration-after-migration1.png)



## <a name="types-of-apps-to-migrate"></a>Geçirilen uygulama türleri

Kimlik ve erişim yönetimi için tek bir denetim düzlemi sağladığından, tüm uygulama kimlik doğrulamanızı Azure AD'ye geçirmek en iyi sidir.

Uygulamalarınız kimlik doğrulaması için modern veya eski protokoller kullanabilir. Modern kimlik doğrulama protokolleri (SAML ve Open ID Connect gibi) kullanan ilk geçiş uygulamalarını düşünün. Bu uygulamalar, Uygulama Galerimizdeki yerleşik bir bağlayıcı aracılığıyla veya uygulamayı Azure AD'ye kaydederek Azure AD ile kimlik doğrulaması yapacak şekilde yeniden yapılandırılabilir. Eski protokolleri kullanan uygulamalar [Application Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-application-proxy)kullanılarak entegre edilebilir. 

Daha fazla bilgi için, [Azure AD ile ne tür uygulamalar entegre edebileceğime](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-application-management)bakın?

[Azure AD Connect Health etkinse](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs)uygulamaları Azure [AD'ye geçirmek için AD FS uygulama etkinlik raporunu](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity) kullanabilirsiniz. 

### <a name="the-migration-process"></a>Geçiş süreci

Uygulama kimlik doğrulamanızı Azure AD'ye taşıma işlemi sırasında uygulamalarınızı ve yapılandırmanızı yeterince test edin. Üretim ortamına geçiş testi için varolan test ortamlarını kullanmaya devam etmenizi öneririz. Şu anda bir test ortamı yoksa, uygulamanın mimarisine bağlı olarak [Azure Uygulama Hizmeti](https://azure.microsoft.com/services/app-service/) veya Azure Sanal [Makineleri](https://azure.microsoft.com/free/virtual-machines/search/?OCID=AID2000128_SEM_lHAVAxZC&MarinID=lHAVAxZC_79233574796345_azure%20virtual%20machines_be_c__1267736956991399_kwd-79233582895903%3Aloc-190&lnkd=Bing_Azure_Brand&msclkid=df6ac75ba7b612854c4299397f6ab5b0&ef_id=XmAptQAAAJXRb3S4%3A20200306231230%3As&dclid=CjkKEQiAhojzBRDg5ZfomsvdiaABEiQABCU7XjfdCUtsl-Abe1RAtAT35kOyI5YKzpxRD6eJS2NM97zw_wcB)kullanarak bir test ortamı ayarlayabilirsiniz.

Uygulama yapılandırmalarınızı geliştirirken kullanmak üzere ayrı bir test Azure AD kiracısı ayarlamayı seçebilirsiniz. 

Geçiş işleminiz şu şekilde görünebilir:

**Aşama 1 – Geçerli durum: AD FS ile doğrulayan üretim uygulaması**

![Geçiş aşaması 1 ](media/migrate-adfs-apps-to-azure/stage1.jpg)

 
**Aşama 2 – İstEĞE BAĞLI: Azure kiracısını test etmeyi gösteren uygulamanın test örneği**

Uygulamanın test örneğini bir test Azure AD kiracısına işaret etmek ve gerekli değişiklikleri yapmak için yapılandırmayı güncelleştirin. Uygulama, Azure AD kiracısına test eden kullanıcılarla test edilebilir. Geliştirme işlemi sırasında, istekleri ve yanıtları karşılaştırmak ve doğrulamak için [Fiddler](https://www.telerik.com/fiddler) gibi araçları kullanabilirsiniz.

Ayrı bir test kiracısı ayarlamak mümkün değilse, bu aşamayı atlayın ve bir uygulamanın test örneğini ayağa çıkarın ve aşağıdaki Aşama 3'te açıklandığı gibi bunu üretim Azure AD kiracınıza yönlendirin.

![Geçiş aşaması 2 ](media/migrate-adfs-apps-to-azure/stage2.jpg)

**Aşama 3 – Üretime işaret eden test uygulaması Azure kiracı**

Uygulamanın test örneğini Azure'un üretim örneğine işaret etmek için yapılandırmayı güncelleştirin. Artık üretim örneğinizdeki kullanıcılarla test edebilirsiniz. Gerekirse, kullanıcıların geçiş ile ilgili bu makalenin bölümünü gözden geçirin.

![Geçiş aşaması 3 ](media/migrate-adfs-apps-to-azure/stage3.jpg)

**Aşama 4 - Üretim AD kiracı işaret üretim uygulaması**

Üretim uygulamanızın yapılandırmasını, üretim Azure kiracınızı işaret etmek için güncelleştirin.

![Geçiş aşaması 1 ](media/migrate-adfs-apps-to-azure/stage4.jpg)

 AD FS ile kimlik doğrulayan uygulamalar, izinler için Active Directory gruplarını kullanabilir. Geçişe başlamadan önce şirket ortamınız ile Azure AD arasında kimlik verilerini eşitlemek için [Azure AD Connect eşitlemeyi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) kullanın. Uygulama geçirildiğinde aynı kullanıcılara erişim izni verebilmek için geçişten önce bu grupları ve üyeliği doğrulayın.

### <a name="line-of-business-lob-apps"></a>İş hattı (LOB) uygulamaları

LOB uygulamaları kuruluşunuz tarafından dahili olarak geliştirilir veya veri merkezinizde yüklenmiş standart bir paket ürün olarak kullanılabilir. Örnekler arasında Windows Identity Foundation ve SharePoint uygulamalarında yerleşik uygulamalar (SharePoint Online değil) verilebilir. 

OAuth 2.0, OpenID Connect veya WS-Federation kullanan LOB uygulamaları [uygulama kaydı](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide-for-app-registrations-legacy-users)olarak Azure AD ile tümleştirilebilir. [Azure portalındaki](https://portal.azure.com/)kurumsal uygulamalar sayfasında SAML 2.0 veya WS-Federation kullanan özel [uygulamaları galeri dışı uygulamalar](https://docs.microsoft.com/azure/active-directory/manage-apps/add-non-gallery-app) olarak tümleştirin.

## <a name="saml-based-single-sign-on"></a>SAML tabanlı tek işaret-On

Kimlik doğrulaması için SAML 2.0 kullanan [uygulamalar, SAML tabanlı tek oturum açma](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on) (SAML tabanlı SSO) için yapılandırılabilir. [SAML tabanlı SSO](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)ile, kullanıcıları SAML taleplerinizde tanımladığınız kurallara göre belirli uygulama rolleriile eşleyebilirsiniz. 

SAML tabanlı tek oturum açma için bir SaaS uygulamasını yapılandırmak için [bkz.](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-non-gallery-applications) 

![SSO SAML Kullanıcı Ekran Görüntüleri ](media/migrate-adfs-apps-to-azure/sso-saml-user-attributes-claims.png)

 
Birçok SaaS uygulaması, SAML tabanlı tek oturum açma yapılandırması boyunca sizi adımlayan uygulamaya özel bir [öğreticiye](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list) sahiptir.

![uygulama eğitimi](media/migrate-adfs-apps-to-azure/app-tutorial.png)

Bazı uygulamalar kolayca geçirilebilir. Özel talepler gibi daha karmaşık gereksinimleri olan uygulamalar Azure AD ve/veya Azure AD Connect'te ek yapılandırma gerektirebilir. Desteklenen talep eşlemeleri hakkında bilgi için Azure [Etkin Dizini'nde Talepler eşleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping)sın'a bakın.

Öznitelikleri eşleme yaparken aşağıdaki sınırlamaları aklınızda bulundurun:

* AD FS'de verilebilen tüm öznitelikler, bu öznitelikler eşitlenmiş olsa bile SAML belirteçlerine yayacak öznitelikler olarak Azure AD'de gösterilmez. Özniteliği siz de yaptığınızda, Değer açılır listesi Azure AD'de kullanılabilen farklı öznitelikleri gösterir. Gerekli bir özniteliğin (örneğin samAccountName) Azure AD ile eşitlendiğini sağlamak için [Azure AD Connect eşitleme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis) yapılandırmasını denetleyin. Uzantı özniteliklerini, Azure AD'deki standart kullanıcı şemasının bir parçası olmayan herhangi bir hak talebinde yatabilirsiniz.

* En yaygın senaryolarda, uygulama için yalnızca NameID talebi ve diğer yaygın kullanıcı tanımlayıcısı talepleri gerekir. Ek talep gerekip gerekip gerekip gerekmeden olup olmadığını belirlemek için, AD FS'den hangi talepleri yayınladığınızı inceleyin.

* Azure AD'de bazı talepler korunduğundan, tüm talepler sorun olamaz. 

* Şifreli SAML belirteçlerini kullanma özelliği artık önizlemede. [Bkz. Nasıl? kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirin.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

 

### <a name="software-as-a-service-saas-apps"></a>Hizmet olarak yazılım (SaaS) uygulamaları

Kullanıcınızın Salesforce, ServiceNow veya Workday gibi SaaS uygulamalarında oturum açması ve AD FS ile entegre olması durumunda, SaaS uygulamaları için federe oturum açma kullanıyorsunuz. 

Çoğu SaaS uygulaması Azure AD'de zaten yapılandırılabilir. Microsoft'un [Azure AD uygulama galerisindeki](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps)SaaS uygulamalarına önceden yapılandırılmış birçok bağlantısı vardır ve bu da geçişinizi kolaylaştırır. SAML 2.0 uygulamaları Azure AD uygulama galerisi aracılığıyla veya [galeri dışı uygulamalar](https://docs.microsoft.com/azure/active-directory/manage-apps/add-non-gallery-app)aracılığıyla Azure AD ile entegre edilebilir. 

OAuth 2.0 veya OpenID Connect kullanan [uygulamalar, uygulama kayıtları](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide-for-app-registrations-legacy-users)gibi Azure AD ile entegre edilebilir. Eski protokolleri kullanan uygulamalar, Azure AD ile kimlik doğrulaması yapmak için [Azure AD Uygulama Proxy'sini](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy) kullanabilir.

SaaS uygulamalarınızda herhangi bir sorun için [SaaS Uygulama Tümleştirme destek takma adı ile](mailto:SaaSApplicationIntegrations@service.microsoft.com)iletişime geçebilirsiniz.

**SSO için SAML imzalama sertifikaları**: Sertifikaları imzalama, herhangi bir SSO dağıtımının önemli bir parçasıdır. Azure AD, SaaS uygulamalarınızda SAML tabanlı federe SSO oluşturmak için imzalama sertifikaları oluşturur. Galeri veya galeri dışı uygulamalar ekledikten sonra, eklenen uygulamayı federal SSO seçeneğini kullanarak yapılandırmış olursunuz. Azure [Active Directory'de federe tek oturum açma sertifikalarını yönet'e](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-certificates-for-federated-single-sign-on)bakın.

### <a name="apps-and-configurations-that-can-be-moved-today"></a>Bugün taşınabilen uygulamalar ve yapılandırmalar

Bugün kolayca taşıyabileceğiniz uygulamalar, standart yapılandırma öğeleri ni ve taleplerini kullanan SAML 2.0 uygulamalarını içerir:

* Kullanıcı Asıl Adı

* E-posta adresi

* Ad

* Soyadı

* Azure AD posta özniteliği, posta öneki, çalışan kimliği, 1-15 uzantı öznitelikleri veya şirket içi **SamAccountName** özniteliği dahil olmak üzere SAML **NameID** gibi alternatif öznitelik. Daha fazla bilgi için bkz. [NameIdentifier talebini düzenleme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

* Özel talepler.

Aşağıdakiler, Azure AD'ye geçiş yapmak için ek yapılandırma adımları gerektirir:

* AD FS'de özel yetkilendirme veya Çok Faktörlü Kimlik Doğrulama (MFA) kuralları. [Azure AD Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) özelliğini kullanarak bunları yapılandırabilirsiniz.

* Birden çok Yanıt URL uç noktası olan uygulamalar. Bunları Azure AD'de PowerShell'i kullanarak veya Azure portal arabiriminde yapılandırabilirsiniz.

* SAML sürüm 1.1 belirteçlerini gerektiren SharePoint uygulamaları gibi WS-Federasyon uygulamaları. PowerShell'i kullanarak bunları el ile yapılandırabilirsiniz. Galeriden SharePoint ve SAML 1.1 uygulamaları için önceden entegre edilmiş genel şablon da ekleyebilirsiniz. SAML 2.0 protokolünü destekliyoruz.

* Karmaşık talep verme kuralları dönüştürür. Desteklenen talep eşlemeleri hakkında bilgi için bkz:
   *  [Azure Active Directory’de talep eşlemesi](https://docs.microsoft.com/azure/active-directory/develop/active-directory-claims-mapping) 
   * [Azure Active Directory'deki kurumsal uygulamalar için SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

 

### <a name="apps-and-configurations-not-supported-in-azure-ad-today"></a>Bugün Azure AD'de desteklenmeyen uygulamalar ve yapılandırmalar

Aşağıdaki özellikleri gerektiren uygulamalar bugün geçirilemez.

**Protokol özellikleri**

* WS-Trust ActAs deseni için destek

* SAML yapı çözümlemesi

* İmzalanan SAML isteklerinin imza doğrulaması  
İmzalanan isteklerin kabul edildiğini, ancak imzanın doğrulanmadığını unutmayın.  
Azure AD'nin belirteci yalnızca uygulamada önceden yapılandırılmış uç noktalara döndüreceği göz önüne alındığında, çoğu durumda imza doğrulaması gerekmez.

**Belirteç yeteneklerindeki talepler**

* Bu veriler Azure AD ile eşitlenmedikçe, Azure AD dizini dışındaki öznitelik mağazalarından gelen talepler. Daha fazla bilgi için [Azure AD eşitleme API'ye genel bakış](https://docs.microsoft.com/graph/api/resources/synchronization-overview?view=graph-rest-beta)bakın.

* Dizin çok değer öznitelikleri verilmesi. Örneğin, şu anda proxy adresleri için çok değerli bir talepte bulunamayız.

## <a name="map-app-settings-from-ad-fs-to-azure-ad"></a>AD FS'den Azure AD'ye uygulama ayarları

Geçiş işlemi, uygulamanın şirket içinde nasıl yapılandırıldığını değerlendirerek ve bu yapılandırmayı Azure AD'ye eşleyerek başlar. AD FS ve Azure AD benzer şekilde çalışır, bu nedenle güven, oturum açma ve oturum açma URL'lerini ve tanımlayıcıları yapılandırma kavramları her iki durumda da geçerlidir. Uygulamalarınızın AD FS yapılandırma ayarlarını Azure AD'de kolayca yapılandırabilmeniz için belgeleyin.

### <a name="map-app-configuration-settings"></a>Harita uygulaması yapılandırma ayarları

Aşağıdaki tablo, Bir AD FS Relying Party Trust to Azure AD Enterprise Uygulaması arasındaki ayarların en yaygın eşlemelerinden bazılarını açıklar:

* AD FS – Uygulama için AD FS Relying Party Trust'taki ayarı bulun. Güvenen tarafa sağ tıklayın ve Özellikler'i seçin. 

* Azure AD- Ayar, her uygulamanın Tek oturum açma özelliklerinde [Azure portalı](https://portal.azure.com/) içinde yapılandırılır.

| Yapılandırma ayarı| AD FS| Azure AD'de yapılandırma| SAML Belirteci |
| - | - | - | - |
| **Uygulama oturum açma URL'si** <p>Kullanıcının bir Servis Sağlayıcı (SP) tarafından başlatılan SAML akışında uygulamada oturum açma url'si.| Yok| TEMEL SAML Yapılandırmasını SAML tabanlı oturum açmadan aç| Yok |
| **Uygulama yanıt URL'si** <p>Kimliği sağlayıcısının (IdP) bakış açısından uygulamanın URL'si. IdP, kullanıcı IdP'ye oturum ettikten sonra kullanıcıyı ve jetonu buraya gönderir.  Bu, **SAML iddiası tüketici bitiş noktası**olarak da bilinir.| Uç **Noktalar** sekmesini seçin| TEMEL SAML Yapılandırmasını SAML tabanlı oturum açmadan aç| SAML belirtecindeki hedef öğesi. Örnek değer:[https://contoso.my.salesforce.com](https://contoso.my.salesforce.com/) |
| **Uygulama oturumu kapatma URL'si** <p>Bu URL, bir kullanıcı bir uygulamadan çıkış yaptığında "oturum açma temizleme" isteklerinin gönderildiği URL'dir. IdP, kullanıcıyı diğer tüm uygulamalardan da oturum alabilmek için istek gönderir.| Uç **Noktalar** sekmesini seçin| TEMEL SAML Yapılandırmasını SAML tabanlı oturum açmadan aç| Yok |
| **Uygulama tanımlayıcısı** <p>Bu, IdP'nin bakış açısından uygulama tanımlayıcısI. Oturum açma URL değeri genellikle tanımlayıcı için kullanılır (ancak her zaman değil).  Bazen uygulama buna "entity ID" diyor.| **Tanımlayıcılar** sekmesini seçin|TEMEL SAML Yapılandırmasını SAML tabanlı oturum açmadan aç| SAML belirtecindeki **Hedef Kitle** öğesine haritalar. |
| **Uygulama federasyonu meta verileri** <p>Bu, uygulamanın federasyon meta verilerinin konumudur. IdP bunu, uç noktalar veya şifreleme sertifikaları gibi belirli yapılandırma ayarlarını otomatik olarak güncelleştirmek için kullanır.| **İzleme** sekmesini seçin| Yok. Azure AD, uygulama federasyonu meta verilerini doğrudan tüketen leri desteklemez. Federasyon meta verilerini el ile içe aktarabilirsiniz.| Yok |
| **Kullanıcı Tanımlayıcı/ Ad Kimliği** <p>Azure AD'den veya AD FS'den kullanıcı tanımlayıcınızı uygulamanıza benzersiz olarak göstermek için kullanılan öznitelik.  Bu öznitelik genellikle UPN veya kullanıcının e-posta adresidir.| Talep kuralları. Çoğu durumda, talep kuralı NameIdentifier ile biten bir tür ile bir talep sorunları.| Kullanıcı **Öznitelikleri ve Talepleri**üstbilginin altında tanımlayıcıyı bulabilirsiniz. Varsayılan olarak, UPN kullanılır| SAML belirtecindeki **NameID** öğesine eşler. |
| **Diğer talepler** <p>IdP'den uygulamaya yaygın olarak gönderilen diğer talep bilgilerine örnek olarak Ad, Soyad, E-posta adresi ve grup üyeliği verilebilir.| AD FS'de, bunu bağlı olan tarafta diğer talep kuralları olarak bulabilirsiniz.| Kullanıcı **Öznitelikleri & Talepleri**üstbilginin altında tanımlayıcıyı bulabilirsiniz. **Görünüm**'ü seçin ve diğer tüm kullanıcı özniteliklerini düzenleyin.| Yok |


### <a name="map-identity-provider-idp-settings"></a>Harita Kimlik Sağlayıcısı (IdP) ayarları

Uygulamalarınızı SSO için Azure AD ve AD FS'ye işaret etmek üzere yapılandırın. Burada, SAML protokolünü kullanan SaaS uygulamalarına odaklanıyoruz. Ancak, bu kavram özel LOB uygulamaları da uzanır.

> [!NOTE]
> Azure AD için yapılandırma değerleri, Azure Kiracı Kimliğinizin {kiracı kimliği} ve Uygulama Kimliği'nin {application-id} yerine geldiği deseni izler. Bu bilgileri Azure [portalında](https://portal.azure.com/) Azure Active Directory > Properties altında bulabilirsiniz: 

* Kiracı Kimliğinizi görmek için Dizin Kimliği'ni seçin. 

* Uygulama Kimliğinizi görmek için Uygulama Kimliği'ni seçin.

 Üst düzey bir anda, aşağıdaki önemli SaaS uygulamaları yapılandırma öğelerini Azure AD ile eşleyin. 

 

| Öğe| Yapılandırma Değeri |
| - | - |
| Kimlik sağlayıcı veren kuruluş| [https://sts.windows.net/{tenant-id}/](https://sts.windows.net/{tenant-id}/) |
| Kimlik sağlayıcı giriş URL'si| [https://login.microsoftonline.com/{tenant-id}/saml2](https://login.microsoftonline.com/{tenant-id}/saml2) |
| Kimlik sağlayıcı giriş URL'si| [https://login.microsoftonline.com/{tenant-id}/saml2](https://login.microsoftonline.com/{tenant-id}/saml2) |
| Federasyon meta veri konumu| [https://login.windows.net/{tenant-id}/federationmetadata/2007-06/federationmetadata.xml?appid={application-id}](https://login.windows.net/{tenant-id}/federationmetadata/2007-06/federationmetadata.xml?appid={application-id}) |


### <a name="map-sso-settings-for-saas-apps"></a>SaaS uygulamaları için Harita SSO ayarları

SaaS uygulamalarının kimlik doğrulama isteklerini nereye göndereceğini ve alınan belirteçleri nasıl doğrulayabildiklerini bilmesi gerekir. Aşağıdaki tabloda, uygulamadaki SSO ayarlarını yapılandıracak öğeleri ve bunların AD FS ve Azure AD içindeki değerleri veya konumları açıklanmaktadır.

| Yapılandırma ayarı| AD FS| Azure AD'de yapılandırma |
| - | - | - |
| **IdP Oturum Açma URL'si** <p>Uygulamanın bakış açısından IdP'nin oturum açma URL'si (kullanıcının oturum açmak için yönlendirildiği yer).| AD FS oturum açma URL'si AD FS federasyon hizmet adı ve ardından "/adfs/ls/." <p>Örneğin:[https://fs.contoso.com/adfs/ls/](https://fs.contoso.com/adfs/ls/)| {kiracı kimliği} ile kiracı kimliğinizi değiştirin. <p> SAML-P protokolünü kullanan uygulamalar için:[https://login.microsoftonline.com/{tenant-id}/saml2](https://login.microsoftonline.com/{tenant-id}/saml2) <p>WS-Federation protokolünü kullanan uygulamalar için:[https://login.microsoftonline.com/{tenant-id}/wsfed](https://login.microsoftonline.com/{tenant-id}/wsfed) |
| **IdP oturum açma URL'si**<p>Uygulamanın bakış açısından IdP'nin oturum açma URL'si (kullanıcı nın uygulamadan çıkış yapmayı seçtiğinde yeniden yönlendirildiği yer).| Oturum açma URL'si oturum açma URL'si ile aynıdır veya "wa=wsignout1.0" eklenen aynı URL'dir. Örneğin:[https://fs.contoso.com/adfs/ls/?wa=wsignout1.0](https://fs.contoso.com/adfs/ls/?wa=wsignout1.0)| {kiracı kimliği} ile kiracı kimliğinizi değiştirin.<p>SAML-P protokolünü kullanan uygulamalar için:<p>[https://login.microsoftonline.com/{tenant-id}/saml2](https://login.microsoftonline.com/{tenant-id}/saml2) <p> WS-Federation protokolünü kullanan uygulamalar için:[https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0](https://login.microsoftonline.com/common/wsfederation?wa=wsignout1.0) |
| **Belirteç imzalama sertifikası**<p>IdP, verilen belirteçleri imzalamak için sertifikanın özel anahtarını kullanır. Belirtecin, uygulamanın güvenmek üzere yapılandırıldığı IdP'den geldiğini doğrular.| AD FS belirteç imzalama sertifikası AD FS Yönetimi'nde **Sertifikalar**'ın altında bulabilirsiniz.| Üstbilgi **SAML İmzaSertifikası**altında uygulamanın **Tek oturum açma özelliklerinde** Azure portalında bulabilirsiniz. Sertifikayı buradan indirip uygulamaya yükleyebilirsiniz.  <p>Uygulamanın birden fazla sertifikası varsa, federasyon meta data XML dosyasındaki tüm sertifikaları bulabilirsiniz. |
| **Tanımlayıcı/ "veren"**<p>Uygulamanın bakış açısından IdP tanımlayıcısı (bazen "veren kimliği" olarak adlandırılır).<p>SAML belirtecinde, değer Veren öğesi olarak görünür.| AD FS için tanımlayıcı genellikle **Hizmet > Federasyon Hizmet Özellikleri düzenlendi**altında AD FS Yönetimi federasyon hizmet tanımlayıcısI. Örneğin:[http://fs.contoso.com/adfs/services/trust](http://fs.contoso.com/adfs/services/trust)| {kiracı kimliği} ile kiracı kimliğinizi değiştirin.<p>[https://sts.windows.net/{tenant-id}/](https://sts.windows.net/{tenant-id}/) |
| **IdP federasyon meta verileri**<p>IdP'nin genel kullanıma açık federasyon meta verilerinin konumu. (Bazı uygulamalar federasyon meta verilerini yönetici yapılandırma URL'lerine, tanımlayıcıya ve bağımsız olarak belirteç imzalama sertifikasına alternatif olarak kullanılır)| Ad FS Yönetimi'nde AD FS meta veri URL'sini, **Meta veri > Türü: Federasyon Metaverileri > > Son Noktaları > altında Ad**FS Yönetimi'nde bulun. Örneğin:[https://fs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml](https://fs.contoso.com/FederationMetadata/2007-06/FederationMetadata.xml)| Azure AD için karşılık gelen [https://login.microsoftonline.com/{TenantDomainName}/FederationMetadata/2007-06/FederationMetadata.xml](https://login.microsoftonline.com/{TenantDomainName}/FederationMetadata/2007-06/FederationMetadata.xml)değer deseni izler. {TenantDomainName} (KiracıAlan Adı} biçimini "contoso.onmicrosoft.com" biçiminde kiracınızın adı ile değiştirin.   <p>Daha fazla bilgi için bkz. [Federasyon meta verileri](https://docs.microsoft.com/azure/active-directory/azuread-dev/azure-ad-federation-metadata). |


## <a name="represent-ad-fs-security-policies-in-azure-ad"></a>Azure AD'de AD FS güvenlik ilkelerini temsil edin

Uygulama kimlik doğrulamasını Azure AD'ye taşıyarak, varolan güvenlik ilkelerinden Azure AD'de bulunan eşdeğer veya alternatif türevlerine eşlemeler oluşturun. Uygulama sahiplerinizin gerektirdiği güvenlik standartlarını karşılarken bu eşlemelerin yapılabilmesini sağlamak, uygulamanın geri kalanının geçişini önemli ölçüde kolaylaştırır.

Her kural türü ve örnekleri için, ad FS'de kuralın nasıl göründüğünü, AD FS kural dili eşdeğer kodunda nasıl göründüğünü ve bu haritanın Azure AD'deki nasıl olduğunu burada öneriyoruz.

### <a name="map-authorization-rules"></a>Harita yetkilendirme kuralları

AD FS'deki yetkilendirme kuralları türlerine ve bunları Azure AD ile nasıl eşlenebildiğinize örnekler aşağıda verilmiştir:

#### <a name="example-1-permit-access-to-all-users"></a>Örnek 1: Tüm kullanıcılara erişime izin verme

AD FS'de Tüm Kullanıcılara Erişim İzni: 

![Geçiş aşaması 1 ](media/migrate-adfs-apps-to-azure/sso-saml-user-attributes-claims.png)


Bu, Azure AD ile aşağıdaki yollardan biriyle eşler:

Azure [portalında:](https://portal.azure.com/)
* Seçenek 1: Kullanıcı atamasını Hayır olarak ayarlama ![SaaS uygulamaları için erişim denetimi ilkesini edin ](media/migrate-adfs-apps-to-azure/permit-access-to-all-users-2.png)

    Kullanıcı atamasının Evet olarak ayarlanması için kullanıcıların erişim kazanmak için uygulamaya atanmasını gerektirdiğini unutmayın. Hayır olarak ayarlandığında, tüm kullanıcılar erişebilir. Bu anahtar, Uygulamalarım deneyimindeki kullanıcılar için nelerin gösteriş yaptığını denetlemez. 

 
* Seçenek 2: Kullanıcılar ve gruplar sekmesinde, uygulamanızı "Tüm Kullanıcılar" otomatik grubuna atayın. <p>
Varsayılan 'Tüm Kullanıcılar' grubunun kullanılabilmesi için Azure AD kiracınızda Dinamik Grupların bulunmasını [sağlamanız](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule) gerekir.

   ![Azure AD'deki SaaS Uygulamalarım ](media/migrate-adfs-apps-to-azure/permit-access-to-all-users-3.png)


#### <a name="example-2-allow-a-group-explicitly"></a>Örnek 2: Bir gruba açıkça izin ver

AD FS'de açık grup yetkilendirmesi:


![verme Yetkilendirme Kuralları ](media/migrate-adfs-apps-to-azure/allow-a-group-explicitly-1.png)


Kural Azure AD ile nasıl eşlenir:

Azure [portalında,](https://portal.azure.com/)önce AD FS'deki kullanıcı grubuna karşılık gelen [bir kullanıcı grubu oluşturacak](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal) ve ardından bu gruba uygulama izinleri atayacaksınız:

![Atama Ekle ](media/migrate-adfs-apps-to-azure/allow-a-group-explicitly-2.png)


#### <a name="example-3-authorize-a-specific-user"></a>Örnek 3: Belirli bir kullanıcıya yetki

AD FS'de açık kullanıcı yetkilendirmesi:

![verme Yetkilendirme Kuralları ](media/migrate-adfs-apps-to-azure/authorize-a-specific-user-1.png)

Kural Azure AD ile nasıl eşlenir:

Azure [portalında,](https://portal.azure.com/)uygulamanın Atama Ekle sekmesi aracılığıyla uygulamaya aşağıda gösterildiği gibi bir kullanıcı ekleyin:

![Azure'daki SaaS uygulamalarım ](media/migrate-adfs-apps-to-azure/authorize-a-specific-user-2.png)

 
### <a name="map-multi-factor-authentication-rules"></a>Harita Çok Faktörlü Kimlik Doğrulama kuralları 

[ÇOK Faktörlü Kimlik Doğrulama (MFA)](https://docs.microsoft.com/azure/active-directory/authentication/multi-factor-authentication) ve AD FS'nin şirket içi dağıtımı, AD FS ile federated çünkü geçişten sonra çalışmaya devam edecektir. Ancak, Azure AD'nin Koşullu Erişim iş akışlarına bağlı olan Yerleşik MFA özelliklerine geçiş yapmayı düşünün. 

AD FS'deki MFA kuralları türlerine ve bunları farklı koşullara göre Azure AD ile nasıl eşlenebildiğinize örnekler aşağıda verilmiştir:

AD FS'de MFA kural ayarları:

![Azure AD MFA ayarları](media/migrate-adfs-apps-to-azure/mfa-location-1.png)


#### <a name="example-1-enforce-mfa-based-on-usersgroups"></a>Örnek 1: MFA'yı kullanıcılara/gruplara göre uygulayın

Kullanıcı/Gruplar seçici, MFA'yı Gruplar başına (Grup SID) veya kullanıcı başına (Birincil SID) temelinde zorlamanızı sağlayan bir kuraldır. Kullanıcı/Gruplar atamaları dışında, AD FS MFA yapılandırma kullanıcı arabirimi işlevindeki tüm ek onay kutuları, Kullanıcı/Gruplar kuralı uygulandıktan sonra değerlendirilen ek kurallar olarak işlev görür. 


Azure AD'deki bir kullanıcı veya grup için MFA kurallarını belirtin:

1. Yeni bir [koşullu erişim ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json)oluşturun.

2. **Atamalar'ı**seçin. MFA'yı uygulamak istediğiniz kullanıcı(lar) veya grup(lar)ı ekleyin.

3. Aşağıda gösterildiği gibi **Access denetimleri** seçeneklerini yapılandırın:  
‎

![AAD MFA ayarları](media/migrate-adfs-apps-to-azure/mfa-usersorgroups.png)

 
 #### <a name="example-2-enforce-mfa-for-unregistered-devices"></a>Örnek 2: Kayıt dışı aygıtlar için MFA'yı uygulayın

Azure AD'de kayıtlı olmayan aygıtlar için MFA kurallarını belirtin:

1. Yeni bir [koşullu erişim ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json)oluşturun.

2. **Atamaları** **Tüm kullanıcılara**ayarlayın.

3. Aşağıda gösterildiği gibi **Access denetimleri** seçeneklerini yapılandırın:  
‎

![AAD MFA ayarları](media/migrate-adfs-apps-to-azure/mfa-unregistered-devices.png)

 
Seçili denetimlerden birini gerektirecek şekilde Birden çok denetim için'yi ayarladığınızda, onay kutusu tarafından belirtilen koşullardan herhangi biri kullanıcı tarafından yerine getirildiğinde, uygulamanıza erişim hakkı tanıyacaktır.

#### <a name="example-3-enforce-mfa-based-on-location"></a>Örnek 3: Konuma göre MFA'yı uygulayın

Azure AD'de bir kullanıcının konumuna göre MFA kurallarını belirtin:

1. Yeni bir [koşullu erişim ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json)oluşturun.

1. **Atamaları** **Tüm kullanıcılara**ayarlayın.

1. [Azure AD'de adlandırılmış konumları yapılandırın](https://docs.microsoft.com/azure/active-directory/active-directory-named-locations) aksi takdirde kurumsal ağınızın içinden federasyon güvenilirdir. 

1. MFA'yı uygulamak istediğiniz konumları belirtmek için **Koşullar kurallarını** yapılandırın.

![Azure AD MFA ayarları](media/migrate-adfs-apps-to-azure/mfa-location-1.png)

5. Aşağıda gösterildiği gibi **Access denetimleri** seçeneklerini yapılandırın:


![Harita erişim denetimi ilkeleri](media/migrate-adfs-apps-to-azure/mfa-location-2.png)

 
### <a name="map-emit-attributes-as-claims-rule"></a>Talepler kuralı olarak Harita Yayı öznitelikleri

Ad FS'de özniteliklerin nasıl eşlendirilene bir örnek aşağıda verilmiştir:

![Azure AD MFA ayarları](media/migrate-adfs-apps-to-azure/map-emit-attributes-as-claimsrule-1.png)


Kural Azure AD ile nasıl eşlenir:

Azure [portalında](https://portal.azure.com/) **Kurumsal Uygulamalar**, Tek **oturum açma'yı**seçin ve aşağıda gösterildiği gibi **SAML Belirteç Öznitelikleri** ekleyin:

![Azure AD MFA ayarları](media/migrate-adfs-apps-to-azure/map-emit-attributes-as-claimsrule-2.png)



### <a name="map-built-in-access-control-policies"></a>Yerleşik erişim denetim ilkelerini haritala

AD FS 2016'da şu aralarından seçim yapabileceğiniz birkaç yerleşik erişim denetimi politikası vardır:

![Azure AD erişim denetiminde yerleşik](media/migrate-adfs-apps-to-azure/map-builtin-access-control-policies-1.png)

 
Azure AD'de yerleşik ilkeleri uygulamak için, yeni bir [koşullu erişim ilkesi](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-enable-azure-mfa?toc=/azure/active-directory/conditional-access/toc.json&bc=/azure/active-directory/conditional-access/breadcrumb/toc.json) kullanabilir ve erişim denetimlerini yapılandırabilir veya erişim denetimi ilkelerini yapılandırmak için AD FS 2016'daki özel ilke tasarımcısını kullanabilirsiniz. Kural Düzenleyicisi, her türlü permütasyon yapabilmenize yardımcı olabilecek kapsamlı bir İzin ve Hariç seçenekleri listesine sahiptir.

![Azure AD erişim denetim ilkeleri](media/migrate-adfs-apps-to-azure/map-builtin-access-control-policies-2.png)



Bu tabloda, bazı kullanışlı İzin ve Hariç seçenekleri ve Azure AD ile nasıl eşlendiklerini listeledik. 


| | Azure AD'de İzin seçeneği nasıl yapılandırılır?| Azure AD'de Hariç seçeneği nasıl yapılandırılatır? |
| - | - | - |
| Belirli bir ağdan| Azure AD'de [Adlandırılmış Konuma](https://docs.microsoft.com/azure/active-directory/reports-monitoring/quickstart-configure-named-locations) Haritalar| [Güvenilen konumlar](https://docs.microsoft.com/azure/active-directory/conditional-access/location-condition) için **Dışla** seçeneğini kullanma |
| Belirli gruplardan| [Kullanıcı/Grup Ataması Ayarlama](https://docs.microsoft.com/azure/active-directory/manage-apps/assign-user-or-group-access-portal)| Kullanıcılar ve Gruplarda **Dışlama** seçeneğini kullanma |
| Belirli Güven Düzeyine Sahip Cihazlardan| Bunu Atamalar altında 'Aygıt Durumu' denetiminden > Koşullar'dan ayarlayın| Aygıt Durumu Durumu Altında **Dışla** seçeneğini kullanın ve **Tüm aygıtları** dahil edin |
| İstekte Belirli Talepler| Bu ayar geçirilenemiyor| Bu ayar geçirilenemiyor |


Azure Portalı'nda güvenilen konumlar için Dışla seçeneğinin nasıl yapılandırılabildiğinibir örnek:

![Eşleme erişim denetim ilkeleri ekran görüntüsü](media/migrate-adfs-apps-to-azure/map-builtin-access-control-policies-3.png)



## <a name="transition-users-from-ad-fs-to-azure-ad"></a>Kullanıcıları AD FS'den Azure AD'ye geçiş

### <a name="sync-ad-fs-groups-in-azure-ad"></a>Azure AD'de AD FS gruplarını eşitleme

Yetkilendirme kurallarını eşlediğinizde, AD FS ile kimlik doğrulayan uygulamalar izinler için Active Directory gruplarını kullanabilir. Böyle bir durumda, uygulamaları geçirmeden önce bu grupları Azure AD ile eşitlemek için [Azure AD Connect'i](https://go.microsoft.com/fwlink/?LinkId=615771) kullanın. Uygulama geçirildiğinde aynı kullanıcılara erişim izni verebilmek için geçişten önce bu grupları ve üyeliği doğruladığından emin olun.

Daha fazla bilgi için Active [Directory'den eşitlenmiş Grup özniteliklerini kullanmak için Ön koşullara](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-group-claims)bakın.

### <a name="setup-user-self-provisioning"></a>Kurulum kullanıcı self-provisioning 

Bazı SaaS uygulamaları, uygulamada ilk oturum açabildiklerinde kullanıcıları kendi kendine sağlama yeteneğini destekler. Azure Etkin Dizini'nde (Azure AD) uygulama sağlama terimi, kullanıcıların erişilmesi gereken bulut[(SaaS)](https://azure.microsoft.com/overview/what-is-saas/)uygulamalarında otomatik olarak kullanıcı kimlikleri ve rolleri oluşturmayı ifade eder. Geçirilen kullanıcıların SaaS uygulamasında zaten bir hesabı olacaktır. Geçişten sonra eklenen yeni kullanıcıların sağlanması gerekir. Uygulama geçirildikten sonra [SaaS uygulamasını test](https://docs.microsoft.com/azure/active-directory/app-provisioning/user-provisioning) edin.

### <a name="sync-external-users-in-azure-ad"></a>Azure AD'deki harici kullanıcıları eşitleme

Mevcut harici kullanıcılarınız AD FS içinde iki ana şekilde ayarlanabilir:

#### <a name="external-users-with-a-local-account-within-your-organization"></a>Kuruluşunuz içinde yerel bir hesabı olan harici kullanıcılar

Bu hesapları dahili kullanıcı hesaplarınız gibi kullanmaya devam eceksiniz. Bu dış kullanıcı hesapları, hesabınızdaki e-posta dışarıdan işaret etse de, kuruluşunuz içinde bir ilke adı vardır. Geçişinizi ilerlerken, [azure AD B2B'nin](https://docs.microsoft.com/azure/active-directory/b2b/what-is-b2b) sunduğu avantajlardan, bu kullanıcıları böyle bir kimlik kullanılabilir olduğunda kendi kurumsal kimliklerini kullanmaya geçirerek yararlanabilirsiniz. Bu, genellikle kendi kurumsal oturum açma ile oturum açtıkları için bu kullanıcılar için oturum açma işlemini kolaylaştırır. Kuruluşunuzun yönetimi, artık dış kullanıcıların hesaplarını yönetmek zorunda kalmadan da kolaylaşacaktır.

#### <a name="federated-external-identities"></a>Federe dış kimlikler

Şu anda harici bir kuruluşla federating iseniz, almak için birkaç yaklaşımlar var:

* [Azure portalına Azure Etkin Dizin B2B işbirliği kullanıcıları ekleyin.](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator) B2B işbirliği davetlerini Azure AD yönetim portalından, bireysel üyelerin alışık oldukları uygulamaları ve varlıkları kullanmaya devam etmeleri için ortak kuruluşa proaktif olarak gönderebilirsiniz. 

* B2B davet API'sini kullanarak iş ortağı kuruluşunuzdaki bireysel kullanıcılar için bir istek oluşturan [self servis B2B kayıt iş akışı oluşturun.](https://docs.microsoft.com/azure/active-directory/b2b/self-service-portal)

Mevcut dış kullanıcılarınız nasıl yapılandırılırsa yapılandırılırlarsa yapılandırırlar, büyük olasılıkla grup üyeliği veya belirli izinlerde kendi hesaplarıyla ilişkili izinlere sahiptirler. Bu izinlerin geçirilip geçirilmediğini veya temizlenmesi gerekip gerekmediğini değerlendirin. Kuruluşunuz içinde harici bir kullanıcıyı temsil eden hesapların, kullanıcı dış bir kimliğe geçtikten sonra devre dışı bırakılmaları gerekir. Kaynaklarınıza bağlanma yeteneklerinde bir kesinti olabileceğinden, geçiş işlemi iş ortaklarınızla tartışılmalıdır.

## <a name="migrate-and-test-your-apps"></a>Uygulamalarınızı geçirin ve test edin

Bu makalede ayrıntılı geçiş işlemini izleyin.

Ardından, geçişin başarılı olup olmadığını test etmek için [Azure portalına](https://aad.portal.azure.com/) gidin. Aşağıdaki talimatları izleyin:
1. **Kurumsal Uygulamalar** > **Tüm uygulamaları** seçin ve uygulamanızı listeden bulun.

1. Uygulamaya en az bir kullanıcı veya grup atamak için**Kullanıcıları ve grupları** **yönet'i** > seçin.

1. **Koşullu Erişimi** **Yönet'i** > seçin. İlke listenizi gözden geçirin ve [koşullu erişim ilkesiyle](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)uygulamaya erişimi engellemediğinizden emin olun.

Uygulamanızı nasıl yapılandırdığınıza bağlı olarak, SSO'nun düzgün çalıştığından doğrulayın. 

| Kimlik doğrulaması türü| Sınama |
| - | - |
| OAuth / OpenID Bağlantı| **İzinler > Kurumsal uygulamaları** seçin ve uygulamanızın kullanıcı ayarlarında kuruluşunuzda kullanılacak uygulamayı kabul ettiğinizden emin olun.  
‎ |
| SAML tabanlı SSO| **Tek Oturum Açma**altında bulunan TEST [SAML Ayarlarını](https://docs.microsoft.com/azure/active-directory/develop/howto-v1-debug-saml-sso-issues) düğmesini kullanın.  
‎ |
| Parola Tabanlı SSO| [MyApps Güvenli Oturum](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)[-](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)[Uzantısı'nı](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)indirin ve kurun. Bu uzantı, kuruluşunuzun bir SSO işlemini kullanmanızı gerektiren bulut uygulamalarından herhangi birini başlatmanıza yardımcı olur.  
‎ |
| Uygulama Ara Sunucusu| Konektörünüzün çalıştığını ve uygulamanız için atandığından emin olun. Daha fazla yardım için [Uygulama Proxy sorun giderme kılavuzunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot) [ ](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot)ziyaret edin.  
‎ |

> [!NOTE]
> Eski AD FS ortamından gelen çerezler kullanıcının makinelerinde kalıcı olmaya devam eder. Kullanıcılar eski AD FS giriş ortamına ve yeni Azure AD girişine yönlendirilebildiği için bu tanımlama bilgileri geçişte sorunlara neden olabilir. Kullanıcı tarayıcısı çerezlerini el ile veya komut dosyası kullanarak temizlemeniz gerekebilir. System Center Configuration Manager'ı veya benzer bir platformu da kullanabilirsiniz.

### <a name="troubleshoot"></a>Sorun giderme

Geçirilen uygulamaların testinde herhangi bir hata varsa, sorun giderme, varolan AD FS Relying Parties'e geri dönmeden önce ilk adım olabilir. [Azure Etkin Dizini'ndeki uygulamalarda SAML tabanlı tek oturum](https://docs.microsoft.com/azure/active-directory/azuread-dev/howto-v1-debug-saml-sso-issues)açma hatasını nasıl alıto göreceğiz.

### <a name="rollback-migration"></a>Geri alma geçişi

Geçiş başarısız olursa, mevcut Güvenen Tarafları AD FS sunucularında bırakmanızı ve Güvenilen Taraflara erişimi kaldırmanızı öneririz. Bu, dağıtım sırasında gerekirse hızlı bir geri dönüş sağlar.

### <a name="end-user-communication"></a>Son kullanıcı iletişimi

Planlanan kesinti penceresi çok az olsa da, AD FS'den Azure AD'ye kesinti yaparken bu zaman dilimlerini çalışanlara proaktif bir şekilde iletmeyi planlamanız gerekir. Uygulama deneyiminizde bir Geri Bildirim düğmesi veya sorunlar için yardım masanıza işaretçiler olduğundan emin olun.

Dağıtım tamamlandıktan sonra, kullanıcıları başarılı dağıtım hakkında bilgilendiren iletişim gönderebilir ve onlara atmaları gereken yeni adımları hatırlatabilirsiniz.

* Kullanıcılara, geçirilen tüm uygulamalara erişmek için [Access Panelini](https://myapps.microsoft.com.com/) kullanmalarını emredin. 

* Kullanıcılara MFA ayarlarını güncellemeleri gerektirebileceklerini hatırlatın. 

* Self Servis Parola Sıfırlama dağıtılırsa, kullanıcıların kimlik doğrulama yöntemlerini güncelleştirmeleri veya doğrulamaları gerekebilir. Bkz. [MFA](https://aka.ms/mfatemplates) ve [SSPR](https://aka.ms/ssprtemplates) son kullanıcı iletişim şablonları.

Dış kullanıcılara iletişim: Bu kullanıcı grubu genellikle sorunlar durumunda en kritik etkilenen gruptur. Güvenlik duruşunuz dış ortaklar için farklı bir Koşullu Erişim kuralları kümesi veya risk profilleri gerektiriyorsa, bu özellikle doğrudur. Dış iş ortaklarının bulut geçiş zamanlamasından haberdar olduğundan ve dış işbirliğine özgü tüm akışları test eden bir pilot dağıtıma katılmaları için teşvik edildikleri bir zaman dilimine sahip olduğundan emin olun. Son olarak, sorunları niçin olursa koluna erişmelerine izin verdiklerinden emin olun.

## <a name="next-steps"></a>Sonraki Adımlar
[Azure AD'ye Geçiş uygulama kimlik doğrulaması](https://aka.ms/migrateapps/whitepaper) okuma<p>
[Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/conditional-access/overview) ve [MFA'yı](https://docs.microsoft.com/azure/active-directory/authentication/concept-mfa-howitworks) ayarlama
