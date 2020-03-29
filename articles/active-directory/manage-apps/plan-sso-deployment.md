---
title: Azure Etkin Dizin tek oturum açma dağıtımı planlama
description: Kuruluşunuzdaki SSO'yı planlamanıza, dağıtmanıza ve yönetmenize yardımcı olacak kılavuz.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 92496fa572c5c1cae4588f82ac61c18de3024045
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76512836"
---
# <a name="plan-a-single-sign-on-deployment"></a>Çoklu oturum açma dağıtımını planlama

Tek oturum açma (SSO), tek bir kullanıcı hesabı kullanarak oturum açarak kullanıcının ihtiyaç duyduğu tüm uygulamalara ve kaynaklara erişmek anlamına gelir. SSO ile kullanıcılar ikinci kez kimlik doğrulamasına gerek kalmadan gerekli tüm uygulamalara erişebilirler.

## <a name="benefits-of-sso"></a>SSO'nun Faydaları

Tek oturum açma (SSO), kullanıcılar Azure Active Directory (Azure AD)'deki uygulamalarda oturum açınca güvenlik ve kolaylık sağlar. 

Birçok kuruluş, son kullanıcı üretkenliği için Office 365, Box ve Salesforce gibi hizmet (SaaS) uygulamalarına güvenir. Tarihsel olarak, BT personelinin her SaaS uygulamasında kullanıcı hesaplarını tek tek oluşturması ve güncelleştirmesi ve kullanıcıların her biri için bir parola hatırlaması gerekiyordu.

Azure Marketi'nde önceden entegre edilmiş SSO bağlantılarına sahip 3000'den fazla uygulama vardır ve bu uygulamalar kiracınıza entegre etmeyi kolaylaştırır.

## <a name="licensing"></a>Lisanslama

- **Azure AD lisanslama** - Önceden tümleştirilen SaaS uygulamaları için SSO ücretsizdir. Ancak, dizininizdeki nesnelerin sayısı ve dağıtmak istediğiniz özellikler ek lisanslar gerektirebilir. Lisans gereksinimlerinin tam listesi için [Azure Etkin Dizin](https://azure.microsoft.com/pricing/details/active-directory/)Fiyatlandırması'na bakın.
- **Başvuru lisanslama** - İş ihtiyaçlarınızı karşılamak için SaaS uygulamalarınız için uygun lisanslara ihtiyacınız olacaktır. Uygulamaya atanan kullanıcıların uygulamadaki rolleri için uygun lisanslara sahip olup olmadığını belirlemek için uygulama sahibiyle birlikte çalışın. Azure AD rolleri temel alarak otomatik sağlamayı yönetiyorsa, Azure AD'de atanan roller, uygulamada sahip olunan lisans sayısıyla uyumlu olmalıdır. Uygulamada sahip olunan yanlış sayıda lisans, kullanıcının sağlanması/güncellenmesi sırasında hatalara yol açabilir.

## <a name="plan-your-sso-team"></a>SSO ekibinizi planlayın

- **Doğru paydaşları devreye** sokmak - Teknoloji projeleri başarısız olduğunda, bunun nedeni genellikle etki, sonuçlar ve sorumluluklar üzerindeki uyumsuz beklentilerdir. Bu tuzaklardan kaçınmak için, [doğru paydaşlarla etkileşimde bulunduğunuzdan](https://aka.ms/deploymentplans) ve paydaşların rollerini anladığından emin olun.
- **Plan iletişimi** - İletişim, herhangi bir yeni hizmetin başarısı için çok önemlidir. Kullanıcılarınızla deneyimlerinin nasıl değişeceği, ne zaman değişeceği ve sorunlarla karşılaşırsa nasıl destek kazanabilecekleri hakkında proaktif olarak iletişim kurun. Son kullanıcıların [SSO özellikli uygulamalarına nasıl erişeceklerine](end-user-experiences.md)yönelik seçenekleri gözden geçirin ve iletişimlerinizi seçiminizle eşleşecek şekilde hazırlayın. 

## <a name="plan-your-sso-protocol"></a>SSO protokolünüzün planını

Federasyon protokollerine dayalı bir SSO uygulaması güvenliği, güvenilirliği ve son kullanıcı deneyimlerini geliştirir ve uygulanması daha kolaydır. Birçok uygulama Azure AD'ye adım adım kılavuzlar ile önceden entegre [edilmiştir.](../saas-apps/tutorial-list.md) Bunları [Azure Marketimizde](https://azuremarketplace.microsoft.com/marketplace/)bulabilirsiniz. Her SSO yöntemi hakkında ayrıntılı [bilgi, Azure Active Directory'deki uygulamalariçin tek oturum açma makalesinde](what-is-single-sign-on.md)bulunabilir.

Kullanıcılarınızın uygulamalarınızda tek oturum açmalarını etkinleştirmenizin iki temel yolu vardır:

- **Federe tek oturum açma ile** Azure AD, Azure AD hesabını kullanarak kullanıcının uygulama kimliğini doğrular. Bu yöntem, SAML 2.0, WS-Federation veya OpenID Connect gibi protokolleri destekleyen uygulamalar için desteklenir ve en zengin tek oturum açma modudur. Parola tabanlı SSO ve ADFS yerine bir uygulama desteklediğinde Federe SSO'yu Azure AD ile kullanmanızı öneririz.

- **Parola tabanlı tek oturum açma** kullanıcıları ilk kez bir kullanıcı adı ve parola ile uygulamaya oturum açarak uygulamaya erişirler. Azure AD, ilk oturum açmadan sonra uygulamaya kullanıcı adı ve parola sağlar. Parola tabanlı tek oturum açma, güvenli uygulama parolası depolamave bir web tarayıcısı uzantısı veya mobil uygulama kullanarak yeniden oynatma sağlar. Bu seçenek, uygulama tarafından sağlanan varolan oturum açma işleminden yararlanır, yöneticinin parolaları yönetmesini sağlar ve kullanıcının parolayı bilmesini gerektirmez.

### <a name="considerations-for-federation-based-sso"></a>Federasyon bazlı SSO için dikkat edilecek hususlar

- **OpenID Connect ve OAuth'u kullanma** - Bağlandığınız uygulama destekliyorsa, SSO'nuzu bu uygulamaya dahil etmek için OIDC/OAuth 2.0 yöntemini kullanın. Bu yöntem daha az yapılandırma gerektirir ve daha zengin bir kullanıcı deneyimi sağlar. Daha fazla bilgi için [Bkz. OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md), [OpenID Connect 1.0](../develop/v2-protocols-oidc.md)ve [Azure Active Directory geliştirici kılavuzu.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide)
- **SAML tabanlı SSO için Uç Nokta Yapılandırmaları** - SAML kullanıyorsanız, geliştiricilerin uygulama yapılandırmadan önce belirli bilgilere ihtiyacı olacaktır. Daha fazla bilgi için [Temel SAML Yapılandırmasını Düzenleme'ye](configure-single-sign-on-non-gallery-applications.md)bakın.
- **SAML tabanlı SSO için sertifika yönetimi** - Uygulamanız için Federe SSO'yu etkinleştirdiğinizde, Azure AD varsayılan olarak üç yıl boyunca geçerli olan bir sertifika oluşturur. Gerekirse bu sertifikanın son kullanma tarihini özelleştirebilirsiniz. Sertifikaların süresi dolmadan önce yenileme işlemleriniz olduğundan emin olun. Daha fazla bilgi için Azure [AD Yönetimi Sertifikaları'na](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)bakın.

### <a name="considerations-for-password-based-sso"></a>Parola tabanlı SSO için dikkat edilecek noktalar

Parola tabanlı SSO için Azure AD'nin kullanılması, kimlik bilgilerini güvenli bir şekilde alacak ve giriş formlarını dolduracak bir tarayıcı uzantısı dağıtmayı gerektirir. Desteklenen tarayıcılarla uzantıyı ölçekte dağıtmak için bir mekanizma [tanımlayın.](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction) Seçeneklere şunlar dahildir:

- [Internet Explorer için Grup İlkesi](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)
- [Internet Explorer için Configuration Manager](https://docs.microsoft.com/configmgr/core/clients/deploy/deploy-clients-to-windows-computers)
- [Chrome, Firefox, Microsoft Edge veya IE için kullanıcı odaklı indirme ve yapılandırma](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery)

#### <a name="capturing-login-forms-metadata-for-applications-that-arent-in-the-gallery"></a>Oturum açma, galeride olmayan uygulamalar için meta veri oluşturur

Microsoft, parola atlama için bir web uygulamasında meta verilerin yakalanmasını (kullanıcı adı ve parola alanlarını yakalama) destekler. Formları meta verileri yakalamak için uygulamayı yapılandırma işlemi sırasında giriş URL'sine gidin. Tam giriş URL'si için uygulama sahibinden sorun. Bu bilgiler oturum açma işlemi sırasında, oturum açma sırasında Azure AD kimlik bilgilerini uygulamaya eşleme işlemi sırasında kullanılır.

Daha fazla bilgi için Azure [AD ile uygulama erişimi ve SSO nedir? – Parola tabanlı SSO](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/).

#### <a name="indications-that-metadata-in-forms-needs-to-be-recaptured"></a>Formlarda meta verilerin yeniden ele geçirilmesi gerektiğine dair göstergeler

Uygulamalar HTML düzenlerini değiştirdiğinde, değişiklikleri ayarlamak için meta verileri yeniden yakalamanız gerekebilir. HTML düzeninin değiştiğini gösteren yaygın belirtiler şunlardır:

- Uygulamaya tıkladığınızda giriş sayfasında "takılıp" kaldığını bildiren kullanıcılar
- Kullanıcı adının veya parolanın doldurulmadığını bildiren kullanıcılar

#### <a name="shared-accounts"></a>Paylaşılan hesaplar

Oturum açma açısından bakıldığında, paylaşılan hesaplara sahip uygulamalar, tek tek kullanıcılar için parola SSO kullanan bir galeri uygulamasından farklı değildir. Ancak, paylaşılan hesapları kullanmak üzere bir uygulama planlama ve yapılandırma yaparken gereken bazı ek adımlar vardır:

1. Aşağıdakileri belgelemek için uygulama iş kullanıcılarıyla birlikte çalışın:
   1. Uygulamayı kullanacak kuruluştaki kullanıcı kümesi
   1. Kullanıcı kümesiyle ilişkili uygulamada varolan kimlik bilgileri kümesi 
1. Kullanıcı kümesi ve kimlik bilgilerinin her birleşimi için, bulutta veya gereksinimlerinize göre şirket içinde bir güvenlik grubu oluşturun.
1. Paylaşılan kimlik bilgilerini sıfırla. Uygulama Azure AD'de dağıtıldıktan sonra, bireylerin paylaşılan hesabın parolasına ihtiyacı yoktur. Azure AD parolayı depolayacaktır, çok uzun ve karmaşık olarak ayarlamayı düşünün. 
1. Uygulama destekliyorsa parolanın otomatik olarak devrini yapılandırın. Bu şekilde, ilk kurulumu yapan yönetici bile paylaşılan hesabın parolasını bilmeyecek. 

## <a name="plan-your-authentication-method"></a>Kimlik doğrulama yönteminizi planlama

Doğru kimlik doğrulama yöntemini seçmek, Azure AD karma kimlik çözümü oluşturmada çok önemli bir ilk karardır. Buluttaki kullanıcıları da sağlayan Azure AD Connect kullanılarak yapılandırılan kimlik doğrulama yöntemini uygulayın.

Kimlik doğrulama yöntemini seçmek için, seçiminizi uygulamanın zamanını, varolan altyapıyı, karmaşıklığı ve maliyetini göz önünde bulundurmanız gerekir. Bu etkenler her kuruluş için farklıdır ve zaman içinde değişebilir. Belirli senaryonuzla en yakından eşleşen senaryoyu seçmelisiniz. Daha fazla bilgi için bkz. [Azure Active Directory karma kimlik çözümünüz için doğru kimlik doğrulama yöntemini seçin.](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn)

## <a name="plan-your-security-and-governance"></a>Güvenliğinizi ve yönetiminizi planlayın 

Ağ çevreleri BYOD cihazları nın ve bulut uygulamalarının patlamasıyla giderek daha gözenekli ve daha az etkili hale geldiği için kimlik, güvenlik dikkatleri ve yatırımları için yeni birincil pivotdur. 

### <a name="plan-access-reviews"></a>Erişim incelemelerini planla

[Erişim İncelemeleri,](https://docs.microsoft.com/azure/active-directory/governance/create-access-review) kuruluşların grup üyeliklerini, kurumsal uygulamalara erişimi ve rol atamalarını verimli bir şekilde yönetmesini sağlar. Yalnızca doğru kişilerin sürekli erişime sahip olduğundan emin olmak için kullanıcı erişimini düzenli olarak gözden geçirmeyi planlamalısınız.

Erişim gözden incelemelerini ayarlarken planlanması gereken önemli konulardan bazıları şunlardır:

1. İş gereksiniminize uygun erişim değerlendirmeleri için bir cadence tanımlama. Bu, haftada bir kez, aylık, yıllık veya isteğe bağlı egzersiz kadar sık olabilir.

1. Uygulama erişim raporlarının gözden geçirenlerini temsil eden gruplar oluşturun. Uygulamayı ve hedef kullanıcılarını en çok tanıyan paydaşların ve kullanım servis taleplerini erişim değerlendirmelerinizde katılımcı olduğundan emin olmanız gerekir

1. Bir erişim incelemesini tamamlamak, artık erişime ihtiyacı olmayan kullanıcılara uygulama erişim izinlerinin alınmasını içerir. Reddedilen kullanıcılardan gelen olası destek isteklerini işleme planı. Silinen bir kullanıcı, azure AD'de 30 gün boyunca silinir ve gerekirse bir yönetici tarafından geri yüklenebilir. 30 gün sonra bu kullanıcı kalıcı olarak silinir. Azure Etkin Dizin portalını kullanan Global Administrator, bu süre ye ulaşmadan önce yakın zamanda silinen bir kullanıcıyı açıkça silebilir.

### <a name="plan-auditing"></a>Plan denetimi

Azure AD, [teknik ve iş öngörüleri içeren raporlar](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/)sağlar. 

Hem güvenlik hem de etkinlik raporları kullanılabilir. Güvenlik raporları, kullanıcıların risk ve riskli oturum açma için işaretlendiğini gösterir. Etkinlik raporları, oturum açma etkinliğini ayrıntılı olarak inceleyerek ve tüm oturum açma işlemlerinin denetim izlerini sağlayarak kuruluşunuzdaki kullanıcıların davranışını anlamanıza yardımcı olur. Raporları riski yönetmek, üretkenliği artırmak ve uyumluluğu izlemek için kullanabilirsiniz.

| Rapor türü | Erişim incelemesi | Güvenlik raporları | Oturum açma raporu |
|-------------|---------------|------------------|----------------|
| İncelemek için kullanın | Uygulama izinleri ve kullanımı. | Potansiyel olarak tehlikeye atılmış hesaplar | Uygulamalara kimler erişiyor |
| Olası eylemler | Denetim erişimi; izinleri iptal edin | Erişimi iptal edin; kuvvet güvenlik sıfırlama | Erişimi iptal et |

Azure AD, denetim verilerinin çoğunu 30 gün boyunca saklar ve verileri Azure yönetici portalı veya analiz sistemlerinize indirmeniz için bir API aracılığıyla kullanılabilir hale getirir.

### <a name="consider-using-microsoft-cloud-application-security"></a>Microsoft Bulut Uygulama Güvenliği'ni kullanmayı düşünün

Microsoft Cloud App Security (MCAS), Bulut Erişimi Güvenlik Aracısı (CASB) çözümüdür. Bulut uygulamalarınız ve hizmetlerinizde görünürlük sağlar, siber tehditleri tanımlamak ve bunlarla mücadele etmek için gelişmiş analizler sağlar ve verilerinizin nasıl gittiğini kontrol etmenizi sağlar.

MCAS'yi dağıtmak şunları yapmanızı sağlar:

- Bulut ortamınızı ve kuruluşunuzun kullanmakta olduğu bulut uygulamalarını haritalamak ve tanımlamak için Bulut Bulma'yı kullanın.
- Bulutunuzda yaptırım uygulama ve yaptırım uygulamalarını onaylama
- Bağlandığınız uygulamaların görünürlüğü ve yönetimi için sağlayıcı API'lerinden yararlanan kolay dağıtılabilen uygulama bağlayıcılarını kullanın
- Bulut uygulamalarınızdaki erişim ve etkinlikler üzerinde gerçek zamanlı görünürlük ve denetim elde etmek için Koşullu Erişim Uygulama Denetimi korumasını kullanın
- İlkeleri ayarlayarak sürekli denetime sahip olabilirsiniz ve ardından sürekli olarak ince ayar yapmanızı sağlar.

Microsoft Bulut Uygulama Güvenliği (MCAS) Oturum denetimi, herhangi bir işletim sistemindeki herhangi bir ana platformdaki tüm tarayıcılar için kullanılabilir. Mobil uygulamalar ve masaüstü uygulamaları da engellenebilir veya izin verilir. Azure AD ile yerel olarak tümleştirilerek, SAML ile yapılandırılan tüm uygulamalar veya Azure AD'de tek oturum açma lı Open ID Connect uygulamaları, [birkaç özellikli uygulama](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)da dahil olmak üzere desteklenebilir.

MCAS hakkında daha fazla bilgi için [Microsoft Bulut Uygulaması Güvenliği'ne genel bakış](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security)abakın. MCAS kullanıcı tabanlı bir abonelik hizmetidir. [McAS lisanslama veri sayfasında](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2NXYO)lisans ayrıntılarını inceleyebilirsiniz.

### <a name="use-conditional-access"></a>Koşullu Erişimi kullanma

Koşullu Erişim ile bulut uygulamalarınız için ölçüt tabanlı erişim denetimi kararlarını otomatikleştirebilirsiniz.

Koşullu Erişim ilkeleri, ilk faktör kimlik doğrulaması tamamlandıktan sonra uygulanır. Bu nedenle, Koşullu Erişim hizmet reddi (DoS) saldırıları gibi senaryolar için ilk satır savunması olarak tasarlanmamıştır, ancak erişimi belirlemek için bu olaylardan gelen sinyalleri kullanabilirsiniz. Örneğin oturum açma risk düzeyi, isteğin konumu ve benzeri kullanılabilir. Koşullu Erişim hakkında daha fazla bilgi için [genel bakış](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access) ve [dağıtım planına](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access)bakın.

## <a name="azure-sso-technical-requirements"></a>Azure SSO teknik gereksinimleri

Aşağıdaki bölümde, gerekli ortam(lar), uç noktalar, talep eşlemesi, gerekli öznitelikler, sertifikalar ve kullanılan protokoller de dahil olmak üzere özel uygulamanızı yapılandırma gereksinimleri ayrıntılı olarak açıkolun. [Azure AD portalında](https://portal.azure.com/)SSO'yu yapılandırmak için bu bilgilere ihtiyacınız olacak.

### <a name="authentication-mechanism-details"></a>Kimlik doğrulama mekanizması ayrıntıları

Tüm önceden tümleşik SaaS uygulamaları için Microsoft bir öğretici sağlar ve bu bilgilere ihtiyacınız olmaz. Uygulama uygulama pazarımızda / galerimizde değilse, aşağıdaki veri parçalarını toplamanız gerekebilir:

- **Geçerli kimlik sağlayıcısı uygulama varsa SSO için kullanır** - Örneğin: AD FS, PingFederate, Okta
- **Hedef uygulama tarafından desteklenen protokoller** - Örneğin, SAML 2.0, OpenID Connect, OAuth, Forms Tabanlı Auth, WS-Fed, WS-Trust
- **Protokol Azure AD ile yapılandırılan** - Örneğin, SAML 2.0 veya 1.1, OpenID Connect, OAuth, Forms Tabanlı, WS-Fed

### <a name="attribute-requirements"></a>Öznitelik gereksinimleri

Azure AD kullanıcı nesneleri ile her SaaS uygulamasının kullanıcı nesneleri arasında önceden yapılandırılmış öznitelikler ve öznitelik eşlemeleri kümesi vardır. Bazı uygulamalar gruplar gibi diğer nesne türlerini yönetir. Azure AD'deki kullanıcı özniteliklerinin eşlemesi ile uygulamanız için planlayın ve [varsayılan öznitelik eşlemelerini](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) işletme gereksinimlerinize göre özelleştirin.

### <a name="certificate-requirements"></a>Sertifika gereksinimleri

Uygulama sertifikası güncel olmalıdır veya kullanıcıların uygulamaya erişememe riski vardır. Çoğu SaaS başvuru sertifikası 36 ay boyunca iyidir. Bu sertifika süresini başvuru bıçağında değiştirirsiniz. Son kullanma tarihini belgelediğinizden ve sertifika yenilemenizi nasıl yöneteceğinize emin olun. 

Sertifikalarınızı yönetmenin iki yolu vardır. 

- **Otomatik sertifika devri** - Microsoft, [Azure AD'de anahtar devrilme imzalamayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover)destekler. Sertifikaları yönetmek için tercih ettiğimiz yöntem bu olsa da, tüm ISV'ler bu senaryoyu desteklemez.

- **El ile güncelleştir -** Her uygulamanın nasıl tanımlandığına bağlı olarak süresi dolan kendi sertifikası vardır. Uygulamanın sertifikasının süresi dolmadan önce yeni bir sertifika oluşturun ve ISV'ye gönderin. Bu bilgiler federasyon meta verilerinden alınabilir. [Burada federasyon meta verileri hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata)

## <a name="implement-sso"></a>SSO'ya uygulayın

Çözümünüzü planlamak ve kuruluşunuzda dağıtmak için aşağıdaki aşamaları kullanın:

### <a name="user-configuration-for-sso"></a>SSO için kullanıcı yapılandırması

- **Test kullanıcılarınızı belirleme**

   Uygulama sahibiyle iletişime geçin ve uygulama içinde en az üç test kullanıcısı oluşturmalarını isteyin. Birincil tanımlayıcı olarak kullanacağınız bilgilerin doğru şekilde doldurulup Azure AD'de kullanılabilen bir öznitelikle eşleştiğinden emin olun. Çoğu durumda bu, SAML tabanlı uygulamalar için "NameID" ile eşlenecektir. JWT belirteçleri için, bu "preferred_username."
   
   Kullanıcıyı Azure AD'de bulut tabanlı bir kullanıcı olarak el ile oluşturun veya Azure AD Connect eşitleme altyapısını kullanarak kullanıcıyı şirket içinde senkronize edin. Bilgilerin uygulamaya gönderilen taleple eşleştiğinden emin olun.

- **SSO'nun yapılandırılsın**

   Uygulamalar [listesinden,](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)uygulamanız için SSO öğreticisini bulun ve açın, ardından SaaS uygulamanızı başarıyla yapılandırmak için öğreticinin adımlarını izleyin.

   Uygulamanızı bulamıyorsanız, [Özel Uygulama belgelerine](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery)bakın. Bu, Azure REKLAM galerisinde bulunmayan bir uygulamanın nasıl ekleneceği konusunda size yol verecektir.

   İsteğe bağlı olarak, [Microsoft'un kılavuz belgelerini](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping)kullanarak kurumsal uygulama için SAML belirtecinde verilen talepleri kullanabilirsiniz. Bu haritaları, uygulamanız için SAML yanıtında almayı beklediğiniz haritalarla elde edin. Yapılandırma sırasında sorunlarla karşılaşırsanız, [Hata Ayıklama SSO tümleştirmesi hakkındaki](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)kılavuzumuzu kullanın.

Biniş tebinen özel uygulama, Azure AD Premium P1 veya P2 lisansları özelliğidir.

### <a name="provide-sso-change-communications-to-end-users"></a>Son kullanıcılara SSO değişiklik iletişimi sağlama

İletişim planınızı uygulayın. Son kullanıcılarınıza bir değişikliğin yaklaştığını, ne zaman geldiğini, şimdi ne yapacağını ve nasıl yardım isteyeceğinizi bildiğinizden emin olun.

### <a name="verify-end-user-scenarios-for-sso"></a>SSO için son kullanıcı senaryolarını doğrulama

SSO yapılandırmalarınızın beklendiği gibi çalıştığından emin olmak için kurumsal ve kişisel cihazlarüzerinde testler yapmak için aşağıdaki test örneklerini kullanabilirsiniz. Aşağıdaki senaryolar, bir kullanıcının bir uygulama URL'sine doğru gittiğini ve servis sağlayıcısı tarafından başlatılan bir kimlik doğrulama akışından geçtiğini varsayar (SP tarafından başlatılan auth akışı).

| Senaryo | Kullanıcı tarafından SP tarafından başlatılan auth akışında beklenen sonuç |
|----------|---------------------------------------------------|
| Corpnet üzerinde iken IE ile uygulamaya giriş yapın. | Tümleşik Windows Kimlik Doğrulama (IWA) ek istemleri olmadan oluşur. |
| Yeni giriş denemesi ile corpnet kapalı iken IE ile uygulamaya giriş yapın. | AD FS Server'da form tabanlı istem. Kullanıcı başarıyla oturum açar ve MFA için tarayıcı istemleri. |
| Geçerli bir oturum ile corpnet kapalı iken IE ile uygulamaya giriş ve MFA hiç. | Kullanıcı ilk faktör için istem almaz. Kullanıcı MFA için istem alır. |
| Geçerli bir oturumla corpnet kapalı iken IE ile uygulamaya giriş ve zaten bu oturumda MFA gerçekleştirmiştir. | Kullanıcı ilk faktör için istem almaz. Kullanıcı MFA almaz. Kullanıcı SSO'ları uygulamaya girer. |
| Geçerli bir oturumla corpnet kapalı iken Chrome / Firefox / Safari ile uygulamaya giriş ve zaten bu oturumda MFA gerçekleştirmiştir. | Kullanıcı ilk faktör için istem almaz. Kullanıcı MFA almaz. Kullanıcı SSO's uygulama içine. |
| Chrome/Firefox/Safari ile uygulamaya giriş yaparken yeni giriş denemesi ile corpnet'ten silinin. | AD FS Server'da form tabanlı istem. Kullanıcı başarıyla oturum açar ve MFA için tarayıcı istemleri. |
| Geçerli bir oturumla kurumsal ağdayken Chrome/Firefox ile uygulamaya giriş yapın. | Kullanıcı ilk faktör için istem almaz. Kullanıcı MFA almaz. Kullanıcı SSO's uygulama içine. |
| Yeni bir giriş denemesi ile uygulama mobil uygulaması ile uygulamaya giriş yapın. | AD FS Server'da form tabanlı istem. Kullanıcı başarıyla oturum açar ve ADAL istemcisi MFA için istemleri. |
| Yetkisiz kullanıcı, oturum açma URL'si ile uygulamaya giriş yapmaya çalışır. | AD FS Server'da form tabanlı istem. Kullanıcı ilk faktörle oturum açamıyor. |
| Yetkili kullanıcı oturum açmaya çalışır, ancak yanlış bir parola girer. | Kullanıcı uygulama URL'sine gider ve kötü kullanıcı adı/parola hatası alır. |
| Yetkili kullanıcı bir e-postadaki bağlantıyı tıklatır ve zaten doğrulanmış durumdadır. | Kullanıcı URL'yi tıklatıyor ve ek istemleri olmadan uygulamaya oturum açar. |
| Yetkili kullanıcı bir e-postadaki bağlantıyı tıklatır ve henüz doğrulanmaz. | Kullanıcı URL'yi tıklatıyor ve ilk faktörle kimlik doğrulaması istenir. |
| Yetkili Kullanıcı, yeni bir oturum açma denemesi ile uygulama mobil uygulaması (SP tarafından başlatılan) ile uygulamaya giriş yaptı. | AD FS Server'da form tabanlı istem. Kullanıcı başarıyla oturum açar ve ADAL istemcisi MFA için istemleri. |
| Yetkisiz Kullanıcı, oturum açma URL'si (SP tarafından başlatılan) ile uygulamaya giriş yapmaya çalışır. | AD FS Server'da form tabanlı istem. Kullanıcı ilk faktörle oturum açamıyor. |
| Yetkili kullanıcı oturum açmaya çalışır, ancak yanlış bir parola girer.| Kullanıcı uygulama URL'sine gider ve kötü kullanıcı adı/parola hatası alır. |
| Yetkili kullanıcı oturumu açar ve sonra tekrar oturum açar. | Oturum Açma URL'si yapılandırılırsa, kullanıcı tüm hizmetlerden çıkış alar ve kimlik doğrulaması istenir. |
| Yetkili kullanıcı oturumu açar ve sonra tekrar oturum açar. | Oturum Açma URL'si yapılandırılmamışsa, kullanıcı mevcut Azure AD tarayıcı oturumundan varolan belirteçkullanılarak otomatik olarak yeniden oturum alacaktır. |
| Yetkili kullanıcı bir e-postadaki bağlantıyı tıklatır ve zaten doğrulanmış durumdadır. | Kullanıcı URL'yi tıklatıyor ve ek istemleri olmadan uygulamaya oturum açar. |
| Yetkili kullanıcı bir e-postadaki bağlantıyı tıklatır ve henüz doğrulanmaz. | Kullanıcı URL'yi tıklatıyor ve ilk faktörle kimlik doğrulaması istenir. |

## <a name="manage-sso"></a>SSO'yi yönet

Bu bölümde, SSO'yu başarıyla yönetmek için gereken ler ve öneriler sıralanıyor.

### <a name="required-administrative-roles"></a>Gerekli idari roller

Azure Etkin Dizini içinde gerekli görevi gerçekleştirmek için her zaman rolü en az izinle kullanın. Microsoft, [kullanılabilen farklı rolleri gözden geçirmenizi](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) ve bu uygulama için her kişi için gereksinimlerinizi çözmek için doğru rolleri seçmenizi önerir. Bazı rollerin geçici olarak uygulanması ve dağıtım tamamlandıktan sonra kaldırılması gerekebilir.

| Persona| Roller | Azure AD rolü (gerekirse) |
|--------|-------|-----------------------------|
| Yardım masası yöneticisi | Tier 1 desteği | None |
| Kimlik yöneticisi | Sorunlar Azure AD'yi etkilediğinde yapılandırma ve hata ayıklama | Genel yönetici |
| Uygulama yöneticisi | Uygulamada kullanıcı attestation, izinleri olan kullanıcılar üzerinde yapılandırma | None |
| Altyapı yöneticileri | Cert rollover sahibi | Genel yönetici |
| İşletme sahibi/paydaş | Uygulamada kullanıcı attestation, izinleri olan kullanıcılar üzerinde yapılandırma | None |

Dizin izinlerine sahip kullanıcılar için ek denetim, denetim ve erişim incelemesi sağlamak için rollerinizi yönetmek için [Ayrıcalıklı Kimlik Yönetimi](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) 'ni (PIM) kullanmanızı öneririz.

### <a name="sso-certificate-lifecycle-management"></a>SSO sertifika yaşam döngüsü yönetimi

Azure AD ile tek oturum açma ile yapılandırılan uygulama arasında imzalama sertifikasının yaşam döngüsünü yönetmekle görevli doğru rolleri ve e-posta dağıtım listelerini belirlemek önemlidir. Burada sahip öneririz önemli rollerden bazıları şunlardır:

- Uygulamadaki kullanıcı özelliklerini güncelleştirmek için sahip
- Uygulama sonu/düzeltme desteği için Sahip On-Call
- Sertifikayla ilgili değişiklik bildirimleri için yakından izlenen e-posta dağıtım listesi

Bir sertifikanın maksimum ömrü üç yıldır. Azure AD ile uygulamanız arasında bir sertifika değişikliğini nasıl işleyeceğinize ilişkin bir işlem oluşturmanızı öneririz. Bu, sertifikanın süresinin dolması veya sertifika devrinin zorlanması nedeniyle bir kesintinin önlenmesine veya en aza indirilmesine yardımcı olabilir.

### <a name="rollback-process"></a>Geri alma işlemi

Test çalışmalarınızı temel alan testleri tamamladıktan sonra, uygulamanızla üretime geçme nin zamanı dolsun. Üretime geçmek, planlı ve test edilmiş yapılandırmalarınızı üretim kiracınızda uygulayacağınız ve kullanıcılarınıza sunacağınız anlamına gelir. Ancak, dağıtımınızın planlandığı gibi gitmemesi durumunda ne yapacağınızı planlamanız önemlidir. Dağıtım sırasında SSO yapılandırması başarısız olursa, herhangi bir kesintinin nasıl azaltılabildiğini ve kullanıcılarınız üzerindeki etkisini nasıl azaltabileceğinizi anlamanız gerekir.

Uygulama içinde kimlik doğrulama yöntemlerinin kullanılabilirliği en iyi stratejinizi belirleyecektir. Dağıtımınızın sorunlarla karşılanması durumunda, uygulama sahipleri için orijinal oturum açma yapılandırma durumuna tam olarak nasıl geri döneceğinize ilişkin ayrıntılı belgelere sahip olduğunuzdan her zaman emin olun.

- **Uygulamanız birden çok kimlik sağlayıcısını (örneğin**LDAP ve AD FS ve Ping) destekliyorsa, kullanıma sunulması sırasında varolan SSO yapılandırmasını silmeyin. Bunun yerine, daha sonra değiştirmeniz gerektiğinde geçiş sırasında devre dışı kaldığınız da ortaya atılabilir. 

- **Uygulamanız birden çok IDP'yi desteklemiyorsa** ancak kullanıcıların form tabanlı kimlik doğrulaması (kullanıcı adı/parola) kullanarak oturum açmasına izin veriyorsa, yeni SSO yapılandırma kullanıma sunulması başarısız olursa kullanıcıların bu yaklaşıma geri dönebilmesini sağlayın.

### <a name="access-management"></a>Erişim yönetimi

Kaynaklara erişimi yönetirken ölçeklenmiş bir yaklaşım seçmenizi öneririz. Yaygın yaklaşımlar arasında Azure AD Connect üzerinden eşitleyerek şirket içi grupların kullanılması, [kullanıcı özelliklerine göre Azure AD'de Dinamik Gruplar oluşturulması](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal)veya bir kaynak sahibi tarafından yönetilen Azure AD'de self servis [grupları](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) oluşturulması yer almaktadır.

### <a name="monitor-security"></a>Güvenliği izleme

SaaS uygulama güvenliğinin farklı yönlerini gözden geçirdiğiniz ve gerekli olan düzeltici eylemleri gerçekleştirdiğiniz düzenli bir ortam ayarlamanızı öneririz.

### <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki bağlantılar sorun giderme senaryoları sunar. Destek personeliniz için bu senaryoları ve bunları düzeltme adımlarını içeren belirli bir kılavuz oluşturmak isteyebilirsiniz.

#### <a name="consent-issues"></a>İzin sorunları

- [Beklenmeyen onay hatası](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-prompt)

- [Kullanıcı onayı hatası](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-error)

#### <a name="sign-in-issues"></a>Oturum açma sorunları

- [Özel bir portaldan oturum açma sorunları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-deeplink)

- [Özel bölmeden oturum açma sorunları](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-access-panel)

- [Uygulama oturum açma sayfasında hata](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-application-error)

- [Microsoft uygulamasında oturum açma sorunu](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-first-party-microsoft)

#### <a name="sso-issues-for-applications-listed-in-the-azure-application-gallery"></a>Azure Uygulama Galerisi'nde listelenen uygulamalar için SSO sorunları

- [Azure Uygulama Galerisi'nde listelenen uygulamalar için parola SSO ile ilgili sorun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery) 

- [Azure Uygulama Galerisi'nde listelenen uygulamalar için federe SSO ile ilgili sorun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-gallery)   

#### <a name="sso-issues-for-applications-not-listed-in-the-azure-application-gallery"></a>Azure Uygulama Galerisi'nde listelenmemiş uygulamalar için SSO sorunları

- [Azure Uygulama Galerisi'nde listelenmemiş uygulamalar için parola SSO ile ilgili sorun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-non-gallery) 

- [Azure Uygulama Galerisi'nde listelenmemiş uygulamalar için federe SSO ile ilgili sorun](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-non-gallery)

## <a name="next-steps"></a>Sonraki adımlar

[SAML tabanlı SSO hata ayıklama](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

[PowerShell üzerinden Uygulamalar için talep haritalama](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping)

[SAML belirtecinde verilen talepleri özelleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

[Tek Oturum Açma SAML protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

[Tek Oturum Açma SAML protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-out-protocol-reference)

[Azure AD B2B](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) (iş ortakları ve satıcılar gibi harici kullanıcılar için)

[Azure AD Koşullu Erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)

[Azure Kimlik Koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

[SSO erişimi](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

[Uygulama SSO Öğretici](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
