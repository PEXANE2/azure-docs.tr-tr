---
title: Atanan bir uygulama erişim panelinde görünmüyor | Microsoft Dokümanlar
description: Bir uygulamanın Access Paneli'nde neden görünmediğini giderme
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
ms.topic: article
ms.date: 09/09/2018
ms.author: mimart
ms.reviwer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 10dfcf337dc75a202e781e931f38783291a72fe7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67272750"
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a>Atanan bir uygulama erişim panelinde görünmüyor

Erişim Paneli, Azure Etkin Dizini'nde (Azure AD) çalışan bir kullanıcının Azure AD yöneticisinin kendilerine erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan web tabanlı bir portaldır. Bu uygulamalar Azure AD portalında kullanıcı adına yapılandırılır. Uygulama düzgün bir şekilde yapılandırılmalı ve kullanıcının üyesi olduğu bir gruba Access Panel'de uygulamayı görmek için atanmalıdır.

Bir kullanıcının gördüğü uygulama türü aşağıdaki kategorilerde düşebilir:

-   Office 365 Uygulamaları

-   Microsoft ve federasyon tabanlı SSO ile yapılandırılan üçüncü taraf uygulamaları

-   Parola tabanlı SSO uygulamaları

-   Mevcut SSO çözümlerine sahip uygulamalar

## <a name="general-issues-to-check-first"></a>Önce kontrol etmek için genel konular

-   Bir uygulama kullanıcıya yeni eklendiyse, uygulamanın ekleyip eklenmeyeceğini görmek için birkaç dakika sonra kullanıcının Access Paneli'nde oturum açma ve çıkma yapmayı deneyin.

-   Bir lisans kullanıcıdan veya gruptan yeni kaldırıldıysa, kullanıcı bu kullanıcının bir üyesiyse, yapılacak değişiklikler için grubun boyutuna ve karmaşıklığına bağlı olarak uzun zaman alabilir. Erişim Paneli'nde oturum açmadan önce ek süre bekleyin.

## <a name="problems-related-to-application-configuration"></a>Uygulama yapılandırması ile ilgili sorunlar

Doğru şekilde yapılandırılmadığından, uygulama kullanıcının Access Paneli'nde görünmeyebilir. Uygulama yapılandırması ile ilgili sorunları gidermenin bazı yolları aşağıda verilmiştir:

-   [Azure AD galerisi uygulaması için federal tek oturum açma nasıl yapılandırılır?](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Galeri dışı bir uygulama için federal tek oturum açma nasıl yapılandırılır?](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Azure AD galerisi uygulaması için parola tek oturum açma uygulaması nasıl yapılandırılabilen](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Galeri dışı bir uygulama için parola tek oturum açma uygulaması nasıl yapılandırılabilen](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD galerisi uygulaması için federal tek oturum açma nasıl yapılandırılır?

Enterprise Single Sign-On özelliği ile etkinleştirilen Azure REKLAM galerisindeki tüm uygulamalar da adım adım öğreticiye sahiptir. [SaaS uygulamalarını Azure Active Directory ile nasıl entegre ettiğinize ilişkin öğreticiler listesine](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) erişerek adım adım ayrıntılı bir kılavuz alabilirsiniz.

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Azure REKLAM galerisinden uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Uygulamanın meta veri değerlerini Azure AD'de yapılandırın (URL'de oturum aç, Tanımlayıcı, Yanıt URL'si)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alın](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulamada Azure AD meta veri değerlerini yapılandırma (URL'de oturum aç, Veren, Giriş URL'si ve sertifikası)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure REKLAM galerisinden uygulama ekleme

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

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD galerisinden bir uygulama için tek oturum açma yapılandırma

Bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8. **Mod** açılır düşüşünden **SAML tabanlı Oturum** Açma'yı seçin.

9. **Etki Alanı ve URL'lere** gerekli değerleri girin. Bu değerleri uygulama satıcısından almalısınız.

   1. Uygulamayı SP tarafından başlatılan SSO olarak yapılandırmak için, URL'deki Sign bu gerekli bir değerdir. Bazı uygulamalar için Tanımlayıcı da gerekli bir değerdir.

   2. Uygulamayı IdP tarafından başlatılan SSO olarak yapılandırmak için, Yanıt URL'si gerekli bir değerdir. Bazı uygulamalar için Tanımlayıcı da gerekli bir değerdir.

10. **İsteğe bağlı:** Gerekli olmayan değerleri görmek istiyorsanız **gelişmiş URL ayarlarını göster'i** tıklatın.

11. Kullanıcı **özniteliklerinde,** **Kullanıcı Tanımlayıcı** açılır açılır açılır ayında kullanıcılarınız için benzersiz tanımlayıcıyı seçin.

12. **İsteğe bağlı:** Kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni tıklatın **ve diğer tüm kullanıcı özniteliklerini tıklatın.**

    Bir öznitelik eklemek için:

    1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

    2. **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

13. uygulamada tek oturum açma nın nasıl yapılandırılabildiğini belgelemek için ** &lt;uygulama adını&gt; yapılandır'ı** tıklatın. Ayrıca, uygulama ile SSO kurmak için gerekli meta veri URL'leri ve sertifikası var.

14. yapılandırmayı kaydetmek için **Kaydet'i** tıklatın.

15. Kullanıcıları uygulamaya atayın.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin

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
   >Azure AD, SAML AuthRequest'te seçilen değere veya uygulama tarafından istenen biçime göre NameID özniteliğinin (Kullanıcı Tanımlayıcısı) biçimini seçer. Daha fazla bilgi için NameIDPolicy bölümü altında makale [Tek Oturum-On SAML protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) ziyaret edin.
   >
   >

9. Kullanıcı öznitelikleri eklemek için, kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni yeniden güncellemek için **Görüntüle'yi tıklatın ve diğer tüm kullanıcı özniteliklerini tıklatın.**

   Bir öznitelik eklemek için:

   1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

   2. **Kaydet'i tıklatın.** Tabloda yeni öznitelik göreceksiniz.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

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

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için federal tek oturum açma nasıl yapılandırılır?

Galeri dışı bir uygulamayı yapılandırmak için Azure AD premium'una sahip olmanız gerekir ve uygulama SAML 2.0'ı destekler. Azure AD sürümleri hakkında daha fazla bilgi için [Azure AD fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)sayfasını ziyaret edin.

-   [Uygulamanın meta veri değerlerini Azure AD'de yapılandırın (URL'de oturum aç, Tanımlayıcı, Yanıt URL'si)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alın](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulamada Azure AD meta veri değerlerini yapılandırma (URL'de oturum aç, Veren, Giriş URL'si ve sertifikası)](#configure-the-application-for-password-single-sign-on-1)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Uygulamanın meta veri değerlerini Azure AD'de yapılandırın (URL'de oturum aç, Tanımlayıcı, Yanıt URL'si)

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

10. **Mod** açılır düşüşünde **SAML tabanlı Oturum** Açma'yı seçin.

11. **Etki Alanı ve URL'lere** gerekli değerleri girin. Bu değerleri uygulama satıcısından almalısınız.

    1. Uygulamayı IdP tarafından başlatılan SSO olarak yapılandırmak için Yanıt URL'sini ve Tanımlayıcıyı girin.

    2.  **İsteğe bağlı:** Uygulamayı SP tarafından başlatılan SSO olarak yapılandırmak için, URL'deki Sign bu gerekli bir değerdir.

12. Kullanıcı **özniteliklerinde,** **Kullanıcı Tanımlayıcı** açılır açılır açılır ayında kullanıcılarınız için benzersiz tanımlayıcıyı seçin.

13. **İsteğe bağlı:** Kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni tıklatın **ve diğer tüm kullanıcı özniteliklerini tıklatın.**

    Bir öznitelik eklemek için:

    1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

    2. **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

14. uygulamada tek oturum açma nın nasıl yapılandırılabildiğini belgelemek için ** &lt;uygulama adını&gt; yapılandır'ı** tıklatın. Ayrıca, uygulama için gerekli Azure REKLAM URL'leri ve sertifikalarınız da vardır.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin

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
   >Azure AD, SAML AuthRequest'te seçilen değere veya uygulama tarafından istenen biçime göre NameID özniteliğinin (Kullanıcı Tanımlayıcısı) biçimini seçer. Daha fazla bilgi için NameIDPolicy bölümü altında makale [Tek Oturum-On SAML protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) ziyaret edin.
   >
   >

9. Kullanıcı öznitelikleri eklemek için, kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni yeniden güncellemek için **Görüntüle'yi tıklatın ve diğer tüm kullanıcı özniteliklerini tıklatın.**

   Bir öznitelik eklemek için:

   1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

   2. **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

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

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD galerisi uygulaması için parola tek oturum açma nasıl yapılandırılır?

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Azure REKLAM galerisinden uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Parola tek oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure REKLAM galerisinden uygulama ekleme

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

#### <a name="configure-the-application-for-password-single-sign-on"></a>Parola tek oturum açma için uygulamayı yapılandırma

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

9. [Kullanıcıları uygulamaya atayın.](#how-to-assign-a-user-to-an-application-directly)

10. Ayrıca, kullanıcıların satırlarını seçerek ve **Kimlik Bilgilerini Güncelleştir'e** tıklayarak ve kullanıcı adına kullanıcı adı ve parola girerek kullanıcı adına kimlik bilgileri de sağlayabilirsiniz. Aksi takdirde, kullanıcılardan başlatıldıktan sonra kimlik bilgilerini kendileri girmeleri istenir.

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için parola tek oturum açma nasıl yapılandırılır?

Azure AD galerisinden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Galeri dışı uygulama ekleme](#add-a-non-gallery-application)

-   [Parola tek oturum açma için uygulamayı yapılandırma](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a>Galeri dışı uygulama ekleme

Azure AD Galerisi'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  Azure [portalını](https://portal.azure.com) açın ve **Global Administrator** veya **Co-admin**olarak oturum açın.

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6.  **Galeri Dışı uygulamasını tıklatın.**

7.  **Uygulamanızın** adını Ad metin kutusuna girin. **Ekle'yi seçin.**

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

#### <a name="configure-the-application-for-password-single-sign-on"></a><a name="configure-the-application-for-password-single-sign-on-1"></a>Parola tek oturum açma için uygulamayı yapılandırma

Bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5.  **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

    1.  Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6.  Tek oturum açma yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

8.  Parola tabanlı Oturum Açma modunu **seçin.**

9.  Oturum **Açma URL'sini**girin. Bu, kullanıcıların oturum açabilmek için kullanıcı adlarını ve parolalarını girdikleri URL'dir. Oturum açma alanlarının URL'de görünür olduğundan emin olun.

10. [Kullanıcıları uygulamaya atayın.](#how-to-assign-a-user-to-an-application-directly)

11. Ayrıca, kullanıcıların satırlarını seçerek ve **Kimlik Bilgilerini Güncelleştir'e** tıklayarak ve kullanıcı adına kullanıcı adı ve parola girerek kullanıcı adına kimlik bilgileri de sağlayabilirsiniz. Aksi takdirde, kullanıcılardan başlatıldıktan sonra kimlik bilgilerini kendileri girmeleri istenir.

## <a name="problems-related-to-assigning-applications-to-users"></a>Kullanıcılara uygulama atama ile ilgili sorunlar

Kullanıcı, uygulamaya atanmamış olduğundan Access Paneli'nde bir uygulama görmüyor olabilir. Aşağıda kontrol etmek için bazı yollar verilmiştir:

-   [Uygulamaya bir kullanıcıatanıp atanmadı denetleme](#check-if-a-user-is-assigned-to-the-application)

-   [Bir kullanıcıyı doğrudan bir uygulamaya atama](#how-to-assign-a-user-to-an-application-directly)

-   [Kullanıcının uygulamayla ilgili bir lisansa atanıp atanmamalarını denetleme](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Bir kullanıcıya lisans atama](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a>Uygulamaya bir kullanıcıatanıp atanmadı denetleme

Uygulamaya bir kullanıcı atanıp atanmadı kontrol etmek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

6. Söz konusu uygulamanın adını **arayın.**

7. **Kullanıcılar ve gruplar'ı**tıklatın.

8. Kullanıcınızın uygulamaya atanıp atanmadı olup olmadığını kontrol edin.

   * Bunu yapmak için "Bir uygulamayı doğrudan bir uygulamaya nasıl atayabilirsiniz" adımlarını izlemezseniz.

### <a name="how-to-assign-a-user-to-an-application-directly"></a>Bir kullanıcıyı doğrudan bir uygulamaya atama

Bir veya daha fazla kullanıcıyı doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator**olarak oturum açın.

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

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Kullanıcının uygulamayla ilgili bir lisans altında olup olmadığını denetleme

Bir kullanıcının atanmış lisanslarını denetlemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm kullanıcıları**tıklatın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7. kullanıcının şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

   * Kullanıcı bir Office lisansına atanmışsa, bu, Birinci Taraf Office uygulamalarının kullanıcının Erişim Paneli'nde görünmesini sağlar.

### <a name="how-to-assign-a-user-a-license"></a>Kullanıcıya lisans atama 

Bir kullanıcıya lisans atamak için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm kullanıcıları**tıklatın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7.  kullanıcının şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

8.  **Atla** düğmesini tıklatın.

9.  Kullanılabilir ürünler listesinden **bir veya daha fazla ürün** seçin.

10. Ürünleri parçalı olarak atamak için **atama seçenekleri** öğesini **isteğe bağlı** olarak tıklatın. Bu tamamlandığında **Tamam'ı** tıklatın.

11. Bu lisansları bu kullanıcıya atamak için **Atla** düğmesini tıklatın.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Gruplara uygulama atanması ile ilgili sorunlar

Kullanıcı, uygulamaya atanan bir grubun parçası olduğundan, Access Paneli'nde bir uygulama görüyor olabilir. Aşağıda kontrol etmek için bazı yollar verilmiştir:

-   [Kullanıcının grup üyeliklerini kontrol edin](#check-a-users-group-memberships)

-   [Bir uygulamayı doğrudan bir gruba atama](#how-to-assign-an-application-to-a-group-directly)

-   [Kullanıcının lisansa atanan grubun bir parçası olup olmadığını denetleme](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Bir gruba lisans atama](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a>Kullanıcının grup üyeliklerini kontrol edin

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm kullanıcıları**tıklatın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7. **Gruplar'ı**tıklatın.

8. Kullanıcınızın uygulamaya atanan bir Grubun parçası olup olmadığını kontrol edin.

   * Kullanıcıyı gruptan kaldırmak istiyorsanız, grubun **satırını tıklatın** ve sil'i seçin.

### <a name="how-to-assign-an-application-to-a-group-directly"></a>Bir uygulamayı doğrudan bir gruba atama

Bir uygulamaya doğrudan bir veya daha fazla grup atamak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator**olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **tüm uygulamalarınızın** listesini görüntülemek için Tüm Uygulamalar'ı tıklatın.

   * Burada gösterilmesini istediğiniz uygulamayı göremiyorsanız, **Tüm Uygulamalar Listesi'nin** en üstündeki **Filtre** denetimini kullanın ve Tüm Uygulamalar için **Göster** seçeneğini **ayarlayın.**

6. Listeden bir kullanıcı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol daki gezinme menüsünden **Kullanıcılar ve Gruplar'ı** tıklatın.

8. **Atama** **Ekle** bölmesini açmak için Kullanıcılar **ve Gruplar** listesinin üst kısmındaki Ekle düğmesini tıklatın.

9. **Atama Ekle** bölmesinden Kullanıcılar **ve gruplar** seçicisini tıklatın.

10. **Ada veya e-posta adresi** arama kutusuna atamak istediğiniz grubun **tam grup adını** yazın.

11. Bir onay kutusunu ortaya çıkarmak için listedeki **grubun** üzerine titreyin. **checkbox** Kullanıcınızı **Seçili** listeye eklemek için grubun profil fotoğrafının veya logosunun yanındaki onay kutusunu tıklatın.

12. **İsteğe bağlı:** **Birden fazla grup eklemek**istiyorsanız, **ada veya e-posta adresi** arama kutusuna başka bir tam grup **adı** yazın ve bu grubu **Seçili** listeye eklemek için onay kutusunu tıklatın.

13. Grupları seçmeyi bitirdiğinizde, bunları uygulamaya atanacak kullanıcı ve gruplar listesine eklemek için **Seç** düğmesini tıklatın.

14. **İsteğe bağlı:** Seçtiğiniz gruplara atamak için bir rol seçmek için **Atama Ekle** bölmesinde **Rol** seçiyi seçin' i tıklatın.

15. Uygulamayı seçili gruplara atamak için **Atla** düğmesini tıklatın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar bu uygulamaları Access Paneli'nde başlatabilir.

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a>Kullanıcının lisansa atanan grubun bir parçası olup olmadığını denetleme

1. Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5. **tüm kullanıcıları**tıklatın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçmek için **satırı tıklatın.**

7. **Gruplar'ı**tıklatın.

8. belirli bir grubun satırını tıklatın.

9. grubun hangi lisansları ona atadığını görmek için **Lisanslar'ı** tıklatın.

   * Grup bir Office lisansına atanmışsa, bu, belirli Birinci Taraf Office uygulamalarının kullanıcının Erişim Paneli'nde görünmesini sağlayabilir.

### <a name="how-to-assign-a-license-to-a-group"></a>Bir gruba lisans atama

Bir gruba lisans atamak için aşağıdaki adımları izleyin:

1.  Azure [**portalını**](https://portal.azure.com/) açın ve Global Administrator olarak oturum **açın.**

2.  Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3.  Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4.  gezinti menüsünde **Kullanıcılar ve gruplar'ı** tıklatın.

5.  **tüm grupları**tıklatın.

6.  İlgilendiğiniz grubu **arayın** ve seçmek için **satırı tıklatın.**

7.  grubun şu anda hangi lisansları atadığını görmek için **Lisanslar'ı** tıklatın.

8.  **Atla** düğmesini tıklatın.

9.  Kullanılabilir ürünler listesinden **bir veya daha fazla ürün** seçin.

10. Ürünleri parçalı olarak atamak için **atama seçenekleri** öğesini **isteğe bağlı** olarak tıklatın. Bu tamamlandığında **Tamam'ı** tıklatın.

11. Bu lisansları bu gruba atamak için **Atla** düğmesini tıklatın. Bu, grubun boyutuna ve karmaşıklığına bağlı olarak uzun zaman alabilir.

>[!NOTE]
>Bunu daha hızlı yapmak için, kullanıcıya doğrudan geçici olarak lisans atamayı düşünün. 
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory'ye yeni kullanıcı ekleme](./../fundamentals/add-users-azure-active-directory.md)

