---
title: Erişim panelinden bir uygulamada oturum açma sorunları | Microsoft Dokümanlar
description: myapps.microsoft.com'daki Microsoft Azure AD Access Paneli'nden bir uygulamaya erişen sorunları giderme sorunları nasıl giderilir?
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3b93ee38666b93253c7cda6c756d4f58daaea236
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74082154"
---
# <a name="problems-signing-in-to-an-application-from-the-access-panel"></a>Erişim panelinden bir uygulamada oturum açma sorunları

Erişim Paneli, Azure Etkin Dizini'nde (Azure AD) çalışan bir kullanıcının Azure AD yöneticisinin kendilerine erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan web tabanlı bir portaldır. 

Bu uygulamalar Azure AD portalında kullanıcı adına yapılandırılır. Uygulama düzgün bir şekilde yapılandırılmalı ve kullanıcının üyesi olduğu bir gruba Access Panel'de uygulamayı görmek için atanmalıdır.

Bir kullanıcının gördüğü uygulama türü aşağıdaki kategorilerde düşebilir:

-   Office 365 Uygulamaları

-   Microsoft ve federasyon tabanlı SSO ile yapılandırılan üçüncü taraf uygulamaları

-   Parola tabanlı SSO uygulamaları

-   Mevcut SSO çözümlerine sahip uygulamalar

## <a name="general-issues-to-check-first"></a>Önce kontrol etmek için genel konular

-   Erişim Paneli için minimum gereksinimleri karşılayan bir **tarayıcı** kullandığınızdan emin olun.

-   Kullanıcının tarayıcısının uygulamanın URL'sini **güvenilir sitelerine**eklemiştir.

-   Uygulamanın doğru şekilde **yapılandırıldığından** emin olun.

-   Oturum açma için kullanıcının hesabının **etkin** olduğundan emin olun.

-   Kullanıcının hesabının **kilitlendirolmadığından** emin olun.

-   Kullanıcının **parolasının süresinin dolmadığından veya unutulmadığından** emin olun.

-   Çok **Faktörlü Kimlik Doğrulama'nın** kullanıcı erişimini engellemediğinden emin olun.

-   **Koşullu Erişim ilkesinin** veya **Kimlik Koruma** ilkesinin kullanıcı erişimini engellemediğinden emin olun.

-   Çok Faktörlü Kimlik Doğrulama veya Koşullu Erişim ilkelerinin uygulanmasına izin vermek için kullanıcının **kimlik doğrulama iletişim bilgisinin** güncel olduğundan emin olun.

-   Tarayıcınızın çerezlerini temizlemeyi ve yeniden oturum açmaya çalışmayı da denediğinden emin olun.

## <a name="meeting-browser-requirements-for-the-access-panel"></a>Erişim Paneli için tarayıcı gereksinimlerini karşılama

Erişim Paneli, JavaScript'i destekleyen ve CSS etkinleştirilmiş bir tarayıcı gerektirir. Access Paneli'nde parola tabanlı tek oturum açma (SSO) kullanmak için Erişim Masası uzantısı kullanıcının tarayıcısına yüklenmiş olmalıdır. Kullanıcı parola tabanlı SSO için yapılandırılan bir uygulamayı seçtiğinde bu uzantı otomatik olarak indirilir.

Parola tabanlı SSO için, son kullanıcının tarayıcıları şu olabilir:

-   Internet Explorer 8, 9, 10, 11 -- Windows 7 veya sonraki

-   Windows 10 Anniversary Edition veya sonraki sürümde Microsoft Edge

-   Chrome -- Windows 7 veya sonraki ve MacOS X veya daha sonra

-   Firefox 26.0 veya sonraki -- Windows XP SP2 veya sonraki ve Mac OS X 10.6 veya sonraki

## <a name="how-to-install-the-access-panel-browser-extension"></a>Access Panel Tarayıcı uzantısı nasıl yüklenir?

Access Panel Tarayıcı uzantısını yüklemek için aşağıdaki adımları izleyin:

1.  Desteklenen tarayıcılardan birinde [Erişim Paneli'ni](https://myapps.microsoft.com) açın ve Azure AD'nizde **kullanıcı** olarak oturum açın.

2.  Access Paneli'ndeki **bir parola-SSO uygulamasını** tıklatın.

3.  Yazılımı yüklemek isteyen istemi, **Şimdi Yükle'yi**seçin.

4.  Tarayıcınıza bağlı olarak indirme linkine yönlendirilirsiniz. Uzantıyı tarayıcınıza **ekleyin.**

5.  Tarayıcınız sorarsa, uzantıyı **etkinleştir** veya **ona izin ver'i** seçin.

6.  Yüklendikten sonra tarayıcı oturumunuzu **yeniden başlatın.**

7.  Access Paneli'nde oturum açın ve parola-SSO uygulamalarınızı **başlatıp başlatamayacağınızı** görün

Chrome ve Microsoft Edge uzantını aşağıdaki doğrudan bağlantılardan da indirebilirsiniz:

-   [Chrome Erişim Paneli Uzantısı](https://chrome.google.com/webstore/detail/access-panel-extension/ggjhpefgjjfobnfoldnjipclpcfbgbhl)

-   [Microsoft Edge Access Panel Uzantısı](https://www.microsoft.com/p/my-apps-secure-sign-in-extension/9pc9sckkzk84)

## <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD galerisi uygulaması için federal tek oturum açma nasıl yapılandırılır?

Enterprise Single Sign-On özelliği ile etkinleştirilen Azure REKLAM galerisindeki tüm uygulamalar da adım adım öğreticiye sahiptir. [SaaS uygulamalarını Azure Active Directory ile nasıl entegre ettiğinize ilişkin öğreticiler listesine](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) erişerek adım adım ayrıntılı bir kılavuz alabilirsiniz.

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   Azure REKLAM galerisinden uygulama ekleme

-   [Uygulamanın meta veri değerlerini Azure AD'de yapılandırın (URL'de oturum aç, Tanımlayıcı, Yanıt URL'si)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alın](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulamada Azure AD meta veri değerlerini yapılandırma (URL'de oturum aç, Veren, Giriş URL'si ve sertifikası)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   Uygulamaya kullanıcı atama

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure REKLAM galerisinden uygulama ekleme

Azure AD Galerisi'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Azure [portalını](https://portal.azure.com) açın ve **Global Yönetici** veya **Ortak Yönetici** olarak oturum açın

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6.  **Galeri bölümünden ekle** bölümünden bir ad textbox **girin,** uygulamanın adını yazın.

7.  Tek oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulamayı eklemeden önce, adını **Ad** metin kutusundan değiştirebilirsiniz.

9.  Uygulamayı eklemek için **Ekle** düğmesini tıklatın.

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD galerisinden bir uygulama için tek oturum açma yapılandırma

Bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1. <span id="_Hlk477187909" class="anchor"><span id="_Hlk477001983" class="anchor"></span></span>Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. **Mod** açılır düşüşünden **SAML tabanlı Oturum** Açma'yı seçin.

9. **Etki Alanı ve URL'lere** gerekli değerleri girin. Bu değerleri uygulama satıcısından almalısınız.

   1. Uygulamayı SP tarafından başlatılan SSO olarak yapılandırmak için Oturum Açma URL'si gerekli bir değerdir. Bazı uygulamalar için Tanımlayıcı da gerekli bir değerdir.

   2. Uygulamayı IdP tarafından başlatılan SSO olarak yapılandırmak için Yanıt URL'si gerekli bir değerdir. Bazı uygulamalar için Tanımlayıcı da gerekli bir değerdir.

10. **İsteğe bağlı:** Gerekli olmayan değerleri görmek istiyorsanız **gelişmiş URL ayarlarını göster'i** tıklatın.

11. Kullanıcı **özniteliklerinde,** **Kullanıcı Tanımlayıcı** açılır açılır açılır ayında kullanıcılarınız için benzersiz tanımlayıcıyı seçin.

12. **İsteğe bağlı:** Kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni tıklatın **ve diğer tüm kullanıcı özniteliklerini tıklatın.**

    Bir öznitelik eklemek için:

    1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

    2. **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

13. uygulamada tek oturum açma nın nasıl yapılandırılabildiğini belgelemek için ** &lt;uygulama adını&gt; yapılandır'ı** tıklatın. Ayrıca, uygulama ile SSO kurmak için gerekli meta veri URL'leri ve sertifikası var.

14. Yapılandırmayı kaydetmek için **Kaydet'i** tıklatın.

15. Kullanıcıları uygulamaya atayın.

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin

Kullanıcı Tanımlayıcısını seçmek veya kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada göstermek istediğiniz uygulamayı görmüyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yı yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. Kullanıcı **öznitelikleri** bölümünde, **Kullanıcı Tanımlayıcı** açılır bölümünde kullanıcılarınız için benzersiz tanımlayıcıyı seçin. Seçilen seçeneğin kullanıcının kimliğini doğrulamak için uygulamada beklenen değeri eşleştirmesi gerekir.

   >[!NOTE]
   >Azure AD, SAML AuthRequest'te seçilen değere veya uygulama tarafından istenen biçime göre NameID özniteliğinin (Kullanıcı Tanımlayıcısı) biçimini seçin. Daha fazla bilgi için NameIDPolicy bölümü altında makale [Tek Oturum-On SAML protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) ziyaret edin.
   >
   >

9. Kullanıcı öznitelikleri eklemek için, kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni yeniden güncellemek için **Görüntüle'yi tıklatın ve diğer tüm kullanıcı özniteliklerini tıklatın.**

   Bir öznitelik eklemek için:

   1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

   2. **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

Uygulama meta verilerini veya sertifikasını Azure AD'den indirmek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yı yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. **SAML İmza Sertifikası** bölümüne gidin ve ardından sütun değerini **karşıdan yükle'yi** tıklatın. Uygulamanın tek oturum açma yı yapılandırmayı gerektirdiğine bağlı olarak, Metadata XML'i veya Sertifikayı indirme seçeneğini görürsünüz.

   Azure AD, meta verileri almak için bir URL sağlamaz. Meta veriler yalnızca XML dosyası olarak alınabilir.

## <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için federal tek oturum açma nasıl yapılandırılır?

Galeri dışı bir uygulamayı yapılandırmak için Azure AD premium'una sahip olmanız gerekir ve uygulama SAML 2.0'ı destekler. Azure AD sürümleri hakkında daha fazla bilgi için [Azure AD fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)sayfasını ziyaret edin.

-   Uygulamanın meta veri değerlerini Azure AD'de yapılandırın (URL'de oturum aç, Tanımlayıcı, Yanıt URL'si)

-   [Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alın](#download-the-azure-ad-metadata-or-certificate)

-   Uygulamada Azure AD meta veri değerlerini yapılandırma (URL'de oturum aç, Veren, Giriş URL'si ve sertifikası)

### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Uygulamanın meta veri değerlerini Azure AD'de yapılandırın (URL'de oturum aç, Tanımlayıcı, Yanıt URL'si)

Azure AD galerisinde olmayan bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6. **Kendi uygulamanızı ekle** bölümünde **galeri dışı uygulamasını** tıklatın.

7. **Ad** metin kutusuna uygulamanın adını girin.

8. Uygulamayı eklemek için **Ekle** düğmesini tıklatın.

9. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

10. **Mod** açılır düşüşünde **SAML tabanlı Oturum** Açma'yı seçin

11. **Etki Alanı ve URL'lere** gerekli değerleri girin. Bu değerleri uygulama satıcısından almalısınız.

    1. Uygulamayı IdP tarafından başlatılan SSO olarak yapılandırmak için Yanıt URL'sini ve Tanımlayıcıyı girin.

    2. **İsteğe bağlı:** Uygulamayı SP tarafından başlatılan SSO olarak yapılandırmak için Oturum Açma URL'si gerekli bir değerdir.

12. Kullanıcı **özniteliklerinde,** **Kullanıcı Tanımlayıcı** açılır açılır açılır ayında kullanıcılarınız için benzersiz tanımlayıcıyı seçin.

13. **İsteğe bağlı:** Kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni tıklatın **ve diğer tüm kullanıcı özniteliklerini tıklatın.**

    Bir öznitelik eklemek için:

    1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

    2. **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

14. uygulamada tek oturum açma nın nasıl yapılandırılabildiğini belgelemek için ** &lt;uygulama adını&gt; yapılandır'ı** tıklatın. Ayrıca, uygulama için gerekli Azure REKLAM URL'leri ve sertifikası nız da vardır.

### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin

Kullanıcı Tanımlayıcısını seçmek veya kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yı yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. Kullanıcı **öznitelikleri** bölümünde, **Kullanıcı Tanımlayıcı** açılır bölümünde kullanıcılarınız için benzersiz tanımlayıcıyı seçin. Seçilen seçeneğin kullanıcının kimliğini doğrulamak için uygulamada beklenen değeri eşleştirmesi gerekir.

   >[!NOTE]
   >Azure AD, SAML AuthRequest'te seçilen değere veya uygulama tarafından istenen biçime göre NameID özniteliğinin (Kullanıcı Tanımlayıcısı) biçimini seçin. Daha fazla bilgi için NameIDPolicy bölümü altında makale [Tek Oturum-On SAML protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) ziyaret edin.
   >
   >

9. Kullanıcı öznitelikleri eklemek için, kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni yeniden güncellemek için **Görüntüle'yi tıklatın ve diğer tüm kullanıcı özniteliklerini tıklatın.**

   Bir öznitelik eklemek için:

   1.click **Öznitelik ekle**. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

   2 **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

Uygulama meta verilerini veya sertifikasını Azure AD'den indirmek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yı yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. **SAML İmza Sertifikası** bölümüne gidin ve ardından sütun değerini **karşıdan yükle'yi** tıklatın. Uygulamanın tek oturum açma yı yapılandırmayı gerektirdiğine bağlı olarak, Metadata XML'i veya Sertifikayı indirme seçeneğini görürsünüz.

   Azure AD, meta verileri almak için bir URL sağlamaz. Meta veriler yalnızca XML dosyası olarak alınabilir.

## <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD galerisi uygulaması için parola tek oturum açma nasıl yapılandırılır?

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   Azure REKLAM galerisinden uygulama ekleme

-   Parola tek oturum açma için uygulamayı yapılandırma

### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure REKLAM galerisinden uygulama ekleme

Azure AD Galerisi'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Azure [portalını](https://portal.azure.com) açın ve **Global Yönetici** veya **Ortak Yönetici** olarak oturum açın

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6.  **Galeri bölümünden Ekle'den** bir ad textbox **girin,** uygulamanın adını yazın

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin

8.  Uygulamayı eklemeden önce, adını **Ad** metin kutusundan değiştirebilirsiniz.

9.  Uygulamayı eklemek için **Ekle** düğmesini tıklatın.

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola tek oturum açma için uygulamayı yapılandırma

Bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. Parola tabanlı Oturum Açma modunu **seçin.**

9. Kullanıcıları uygulamaya atayın.

10. Ayrıca, kullanıcıların satırlarını seçerek ve **Kimlik Bilgilerini Güncelleştir'e** tıklayarak ve kullanıcı adına kullanıcı adı ve parola girerek kullanıcı adına kimlik bilgileri de sağlayabilirsiniz. Aksi takdirde, kullanıcılardan başlatıldıktan sonra kimlik bilgilerini kendileri girmeleri istenir.

## <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için parola tek oturum açma nasıl yapılandırılır?

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Galeri dışı uygulama ekleme](#add-a-non-gallery-application)

-   [Parola tek oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

### <a name="add-a-non-gallery-application"></a>Galeri dışı uygulama ekleme

Azure AD Galerisi'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Azure [portalını](https://portal.azure.com) açın ve **Global Yönetici** veya **Ortak Yönetici** olarak oturum açın

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6.  **Galeri Dışı uygulamasını tıklatın.**

7.  **Uygulamanızın** adını Ad metin kutusuna girin. **Ekle'yi seçin.**

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

### <a name="configure-the-application-for-password-single-sign-on"></a>Parola tek oturum açma için uygulamayı yapılandırma

Bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. Parola tabanlı Oturum Açma modunu **seçin.**

9. Oturum **Açma URL'sini**girin. Bu, kullanıcıların oturum açabilmek için kullanıcı adlarını ve parolalarını girdikleri URL'dir. Oturum açma alanlarının URL'de görünür olduğundan emin olun.

10. Kullanıcıları uygulamaya atayın.

11. Ayrıca, kullanıcıların satırlarını seçerek ve **Kimlik Bilgilerini Güncelleştir'e** tıklayarak ve kullanıcı adına kullanıcı adı ve parola girerek kullanıcı adına kimlik bilgileri de sağlayabilirsiniz. Aksi takdirde, kullanıcılardan başlatıldıktan sonra kimlik bilgilerini kendileri girmeleri istenir.

## <a name="how-to-assign-a-user-to-an-application-directly"></a>Bir kullanıcıyı doğrudan bir uygulamaya atama

Bir veya daha fazla kullanıcıyı doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Listeden bir kullanıcı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol daki gezinme menüsünden **Kullanıcılar ve Gruplar'ı** tıklatın.

8. **Atama** **Ekle** bölmesini açmak için Kullanıcılar **ve Gruplar** listesinin üst kısmındaki Ekle düğmesini tıklatın.

9. **Atama Ekle** bölmesinden Kullanıcılar **ve gruplar** seçicisini tıklatın.

10. Ada veya **e-posta adresi** arama kutusuna atamak istediğiniz kullanıcının **tam adını** veya **e-posta adresini** yazın.

11. Bir **onay kutusunu**ortaya çıkarmak için listedeki **kullanıcının** üzerine taşırın. Kullanıcınızı **Seçili** listeye eklemek için kullanıcının profil fotoğrafının veya logosunun yanındaki onay kutusunu tıklatın.

12. **İsteğe bağlı:** **Birden fazla kullanıcı eklemek**istiyorsanız, **ada veya e-posta adresi** arama kutusuna başka bir tam **ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusunu tıklatın.

13. Kullanıcıları seçmeyi bitirdiğinizde, bunları uygulamaya atanacak kullanıcı ve gruplar listesine eklemek için **Seç** düğmesini tıklatın.

14. **İsteğe bağlı:** Seçtiğiniz kullanıcılara atamak için bir rol seçmek için **Atama Ekle** bölmesinde **Rol** seçiyi seçin.

15. Uygulamayı seçili kullanıcılara atamak için **Atla** düğmesini tıklatın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar bu uygulamaları Access Paneli'nde başlatabilir.

## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözmüyorsa

varsa aşağıdaki bilgileri içeren bir destek bileti açın:

-   Korelasyon hatası kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Hata oluşur sırasında saat dilimi ve saat/zaman dilimi oluşur

-   Kemancı izleri

## <a name="next-steps"></a>Sonraki adımlar
[Application Proxy ile uygulamalarınızda tek oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)

