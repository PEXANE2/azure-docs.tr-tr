---
title: Azure AD App Proxy ile şirket içi uygulamalar için SAML tek oturum açma
description: SAML kimlik doğrulaması ile güvenli olan şirket içi uygulamalar için tek oturum açma sağlamayı öğrenin. Application Proxy ile şirket içi uygulamalara uzaktan erişim sağlayın.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/24/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ccf34b52e06e369fe4dd459ff9dfa2880596fb35
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481356"
---
# <a name="saml-single-sign-on-for-on-premises-applications-with-application-proxy"></a>Uygulama Proxy ile şirket içi uygulamalar için SAML tek oturum açma

SAML kimlik doğrulaması ile güvenli olan şirket içi uygulamalara tek oturum açma (SSO) sağlayabilir ve Uygulama Proxy aracılığıyla bu uygulamalara uzaktan erişim sağlayabilirsiniz. SAML tek oturum açma ile Azure Active Directory (Azure AD), kullanıcının Azure AD hesabını kullanarak uygulamaya kimlik doğrulaması sağlar. Azure AD, oturum açma bilgilerini bir bağlantı protokolü aracılığıyla uygulamaya iletir. Ayrıca, kullanıcıları SAML taleplerinizde tanımladığınız kurallara göre belirli uygulama rolleriile eşleyebilirsiniz. Saml SSO'ya ek olarak Application Proxy'yi etkinleştirerek, kullanıcılarınız uygulamaya harici erişime ve sorunsuz bir SSO deneyimine sahip olacaktır.

Uygulamalar **Azure Active Directory**tarafından verilen SAML belirteçlerini tüketebilmeli. Bu yapılandırma, şirket içi kimlik sağlayıcısı kullanan uygulamalar için geçerli değildir. Bu senaryolar [için, uygulamaları Azure AD'ye geçirmek için Kaynakları](migration-resources.md)gözden geçirmenizi öneririz.

Application Proxy ile SAML SSO da SAML belirteç şifreleme özelliği ile çalışır. Daha fazla bilgi için Azure [AD SAML belirteç şifrelemesini Yapılandır'](howto-saml-token-encryption.md)a bakın.

Aşağıdaki protokol diyagramları, hem hizmet sağlayıcısı tarafından başlatılan (SP tarafından başlatılan) akış hem de kimlik sağlayıcısı tarafından başlatılan (IdP tarafından başlatılan) akış için tek oturum açma sırasını açıklar. Application Proxy, SAML isteğini ve şirket içi uygulamaya yanıt vererek SAML SSO ile birlikte çalışır.

  ![SAML SP Akışı](./media/application-proxy-configure-single-sign-on-on-premises-apps/saml-sp-initiated-flow.png)

  ![SAML SP Akışı](./media/application-proxy-configure-single-sign-on-on-premises-apps/saml-idp-initiated-flow.png)

## <a name="create-an-application-and-set-up-saml-sso"></a>Bir uygulama oluşturun ve SAML SSO'yu ayarlayın

1. Azure portalında, **Azure Active Directory > Enterprise uygulamalarını** seçin ve **Yeni uygulamayı**seçin.

2. Yeni uygulamanızın ekran adını girin, **galeride bulamayacağınız diğer uygulamaları tümleştir'i**seçin, ardından **Oluştur'u**seçin.

3. Uygulamanın Genel **Bakış** sayfasında **Tek oturum açma'yı**seçin.

4. Tek oturum açma yöntemi olarak **SAML'yi** seçin.

5. İlk olarak SAML SSO'yu kurumsal ağ üzerinde çalışırken çalışacak şekilde ayarlayın. **SAML sayfasıyla Tek Oturum Açma'da** Temel **SAML Yapılandırması** başlığına gidin ve **Edit** simgesini (kalem) seçin. Uygulama için SAML tabanlı kimlik doğrulamasını yapılandırmak için [temel SAML yapılandırmasını girin](configure-single-sign-on-non-gallery-applications.md#step-1-edit-the-basic-saml-configuration) adımlarını izleyin.

6. Uygulamaya en az bir kullanıcı ekleyin ve test hesabının uygulamaya erişimi olduğundan emin olun. Şirket ağına bağlıyken, uygulamada tek oturum açma olup olmadığını görmek için test hesabını kullanın. 

   > [!NOTE]
   > Application Proxy'yi ayarladıktan sonra geri gelir ve SAML **Yanıt URL'sini**güncellersiniz.

## <a name="publish-the-on-premises-application-with-application-proxy"></a>Uygulama Proxy ile şirket içi başvuruyu yayımlayın

Şirket içi uygulamalar için SSO'yu etkinleştirmeden önce Application Proxy'yi etkinleştirmeniz ve bir bağlayıcı yüklemeniz gerekir. Uygulama, şirket içi ortamınızı nasıl hazırlayacağınızı, bir bağlayıcıyı nasıl yükleyip kaydlayacağınızı ve konektörü nasıl test edileceksiniz öğrenmek [için Azure AD'deki Application Proxy aracılığıyla uzaktan erişim için](application-proxy-add-on-premises-application.md) şirket içi uygulama ekleyin. Ardından, Uygulama Proxy ile yeni uygulamanızı yayınlamak için aşağıdaki adımları izleyin. Aşağıda belirtilmeyen diğer ayarlar için, öğreticideki [Azure AD bölümüne şirket içi uygulama ekle uygulamasına](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad) bakın.

1. Uygulama Azure portalında hala açıkken, **Uygulama Proxy'sini**seçin. Uygulama için **Dahili URL'yi** sağlayın. Özel bir etki alanı kullanıyorsanız, uygulamanız için TLS/SSL sertifikasını da yüklemeniz gerekir. 
   > [!NOTE]
   > En iyi uygulama olarak, optimize edilmiş bir kullanıcı deneyimi için mümkün olduğunca özel etki alanlarını kullanın. [Azure AD Application Proxy'de özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)hakkında daha fazla bilgi edinin.

2. Uygulamanız için Ön **Kimlik Doğrulama** yöntemi olarak **Azure Etkin Dizini'ni** seçin.

3. Uygulama için **Harici URL'yi** kopyalayın. SAML yapılandırmasını tamamlamak için bu URL'ye ihtiyacınız olacak.

4. Test hesabını kullanarak, Uygulama Proxy'sinin doğru ayarlandığını doğrulamak için uygulamayı **Dış URL** ile açmayı deneyin. Sorunlar varsa, [Sorun Giderme Uygulaması Proxy sorunları ve hata iletileri](application-proxy-troubleshoot.md)bakın.

## <a name="update-the-saml-configuration"></a>SAML yapılandırmasını güncelleştirme

1. Uygulama Azure portalında hala açıkken, **Tek oturum açma'yı**seçin. 

2. **SAML sayfasıyla Tek Oturum Açma'da** Temel **SAML Yapılandırması** başlığına gidin ve **Edit** simgesini (kalem) seçin. Uygulama Proxy'sinde yapılandırdığınız **Harici URL'nin** **Tanımlayıcı,** **YanıtURL'si**ve **Giriş URL** alanlarında olduğundan emin olun. Bu URL'lerin Uygulama Proxy'si için doğru çalışması gerekir. 

3. Etki alanına Application Proxy tarafından erişilebilmek için daha önce yapılandırılan **Yanıt URL'sini** düzenleme. Örneğin, Harici **URL'niz** `https://contosotravel-f128.msappproxy.net` ve özgün **Yanıtla URL'niz** ise, `https://contosotravel.com/acs`özgün **Yanıtla URL'sini** güncelleştirmeniz `https://contosotravel-f128.msappproxy.net/acs`gerekir. 

    ![Temel SAML yapılandırma verilerini girin](./media/application-proxy-configure-single-sign-on-on-premises-apps/basic-saml-configuration.png)


4. Varsayılan olarak işaretlemek için güncelleştirilmiş **Yanıt URL'sinin** yanındaki onay kutusunu seçin.

   * Gerekli **Yanıt URL'si** zaten listelenmişse, bu Yanıt **URL'sini** varsayılan olarak işaretleyin ve önceden yapılandırılmış Yanıt **URL'sini**silin.

   * SP tarafından başlatılan akış için, arka uç uygulamasının kimlik doğrulama belirteci almak için doğru **YanıtURL'sini** veya İddia Tüketici Hizmeti URL'sini belirttiğinden emin olun.

    > [!NOTE]
    > Arka uç uygulaması **YanıtURL'sinin** Dahili URL olmasını bekliyorsa, iç ve dış URL'lerle eşleşen [özel etki alanlarını](application-proxy-configure-custom-domain.md) kullanmanız veya Kullanıcıların cihazlarında Uygulamalarım güvenli oturum açma uzantısını yüklemeniz gerekir. Bu uzantı otomatik olarak ilgili Uygulama Proxy Hizmetine yönlendirecektir. Uzantıyı yüklemek için, [Uygulamalarım güvenli oturum açma uzantısına](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension)bakın.
    
## <a name="test-your-app"></a>Uygulamanızı test etme

Tüm bu adımları tamamladığınızda, uygulamanız çalışır durumda olmalıdır. Uygulamayı test etmek için:

1. Bir tarayıcı açın ve uygulamayı yayınladığınızda oluşturduğunuz **Harici URL'ye** gidin. 
1. Uygulamaya atadığınız test hesabıyla oturum açın. Uygulamayı yükleyebilmeli ve Uygulamaya SSO'yu yükleyebilmelisin.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD Application Proxy tek oturum açma sağlar?](application-proxy-single-sign-on.md)
- [Uygulama Ara Sunucusu ile ilgili sorunları giderme](application-proxy-troubleshoot.md)
