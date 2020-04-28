---
title: Atanan uygulama erişim panelinde görünmüyor | Microsoft Docs
description: Uygulamanın erişim panelinde neden görünmediğini sorun giderme
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67272750"
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a>Atanan uygulama erişim panelinde görünmüyor

Erişim paneli, Azure Active Directory (Azure AD) ' de iş veya okul hesabı olan bir kullanıcının Azure AD yöneticisinin erişim izni verdiği bulut tabanlı uygulamaları görüntülemesini ve başlatmasını sağlayan Web tabanlı bir portaldır. Bu uygulamalar, Azure AD portalındaki Kullanıcı adına yapılandırılır. Uygulama, uygulamayı erişim paneli 'nde görmek için bir Kullanıcı ya da Kullanıcı üyesi olan bir gruba atanmalıdır.

Bir kullanıcının görebilecekleri uygulamaların türü aşağıdaki kategorilere ayrılır:

-   Office 365 uygulamaları

-   Federasyon tabanlı SSO ile yapılandırılan Microsoft ve üçüncü taraf uygulamalar

-   Parola tabanlı SSO uygulamaları

-   Var olan SSO çözümlerini içeren uygulamalar

## <a name="general-issues-to-check-first"></a>Önce denetlenecek genel sorunlar

-   Bir uygulama bir kullanıcıya yeni eklendiyse, uygulamanın eklenip eklendiğine bakmak için birkaç dakika sonra kullanıcının erişim paneline yeniden oturum açmayı deneyin.

-   Bir lisans bir kullanıcı veya gruptan kaldırıldıktan sonra, değişikliklerin yapılması için grubun boyutuna ve karmaşıklığına bağlı olarak, Kullanıcı bu işlemi uzun zaman alabilir. Erişim panelinde oturum açmadan önce ek zamana izin verin.

## <a name="problems-related-to-application-configuration"></a>Uygulama yapılandırmasıyla ilgili sorunlar

Bir uygulama, düzgün yapılandırılmadığı için kullanıcının erişim panelinde görünmeyebilir. Aşağıda, uygulama yapılandırmasıyla ilgili sorunları giderebileceğiniz bazı yollar verilmiştir:

-   [Azure AD Galeri uygulaması için Federasyon çoklu oturum açmayı yapılandırma](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Galeri dışı bir uygulama için Federasyon çoklu oturum açmayı yapılandırma](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Azure AD Galeri uygulaması için parola çoklu oturum açma uygulaması yapılandırma](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Galeri dışı bir uygulama için parola çoklu oturum açma uygulaması yapılandırma](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulaması için Federasyon çoklu oturum açmayı yapılandırma

Azure AD galerisinde Kurumsal Çoklu oturum açma özelliğiyle etkinleştirilen tüm uygulamaların sunulan adım adım öğretici vardır. Ayrıntılı adım adım yönergeler için, [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğretici listesine](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) erişebilirsiniz.

Azure AD Galerisi 'nden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Azure AD Galerisi 'nden uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Azure AD 'de uygulamanın meta veri değerlerini yapılandırma (oturum açma URL 'SI, tanımlayıcı, yanıt URL 'SI)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı ' nı seçin ve uygulamaya gönderilecek kullanıcı özniteliklerini ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulamada Azure AD meta veri değerlerini yapılandırma (oturum açma URL 'SI, verenin, oturum kapatma URL 'SI ve sertifika)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galerisi 'nden uygulama ekleme

Azure AD Galerisi 'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  [Azure Portal](https://portal.azure.com) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6.  **Galeriden Ekle** bölümünden **bir ad girin** metin kutusuna uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulamayı eklemeden önce **ad metin kutusundan adını değiştirebilirsiniz** .

9.  Uygulamayı eklemek için **Ekle** düğmesine tıklayın.

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD galerisinden bir uygulama için çoklu oturum açmayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. **Mod** açılır listesinden **SAML tabanlı oturum açma '** yı seçin.

9. **Etki alanı ve URL 'ler** için gerekli değerleri girin. Bu değerleri uygulama satıcısından almalısınız.

   1. Uygulamayı SP tarafından başlatılan SSO olarak yapılandırmak için, oturum açma URL 'SI gerekli bir değerdir. Bazı uygulamalarda, tanımlayıcı de gerekli bir değerdir.

   2. Uygulamayı IDP tarafından başlatılan SSO olarak yapılandırmak için, yanıt URL 'SI gerekli bir değerdir. Bazı uygulamalarda, tanımlayıcı de gerekli bir değerdir.

10. **Isteğe bağlı:** gerekli olmayan değerleri görmek ISTIYORSANıZ **Gelişmiş URL ayarlarını göster** ' e tıklayın.

11. **Kullanıcı öznitelikleri**' nde, **Kullanıcı tanımlayıcısı** açılan menüsünde kullanıcılarınız için benzersiz tanımlayıcıyı seçin.

12. **Isteğe bağlı:** kullanıcılar oturum açtığında SAML belirtecinde uygulamaya gönderilecek öznitelikleri düzenlemek için **diğer tüm Kullanıcı özniteliklerini görüntüle ve Düzenle** ' ye tıklayın.

    Bir öznitelik eklemek için:

    1. **öznitelik Ekle**' ye tıklayın. **Adı** girin ve açılan listeden **değeri** seçin.

    2. Kaydet ' e tıklayın **.** Yeni özniteliği tabloda görürsünüz.

13. uygulamada çoklu oturum açmayı yapılandırma hakkında belgeye erişmek için **uygulama adını &lt;&gt; Yapılandır** ' a tıklayın. Ayrıca, uygulama ile SSO 'yu ayarlamak için gereken meta veri URL 'Leri ve sertifikaya sahip olursunuz.

14. yapılandırmayı kaydetmek için **Kaydet** ' e tıklayın.

15. Kullanıcılara uygulama atama.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı ' nı seçin ve uygulamaya gönderilecek kullanıcı özniteliklerini ekleyin

Kullanıcı tanımlayıcısını seçmek veya Kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Çoklu oturum açma yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. **Kullanıcı öznitelikleri** bölümünde, **Kullanıcı tanımlayıcısı** açılan menüsünde kullanıcılarınız için benzersiz tanımlayıcıyı seçin. Seçili seçeneğin, kullanıcının kimliğini doğrulamak için uygulamadaki beklenen değerle eşleşmesi gerekir.

   >[!NOTE] 
   >Azure AD, NameID özniteliği (Kullanıcı tanımlayıcısı) için seçilen değere veya SAML AuthRequest içinde uygulama tarafından istenen biçime göre biçim seçer. Daha fazla bilgi için, Nameıdpolicy bölümünde [Çoklu oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) makalesini ziyaret edin.
   >
   >

9. Kullanıcı öznitelikleri eklemek için, kullanıcılar oturum açtığında SAML belirtecinde uygulamaya gönderilecek öznitelikleri düzenlemek için **diğer tüm Kullanıcı özniteliklerini görüntüle ve Düzenle** ' ye tıklayın.

   Bir öznitelik eklemek için:

   1. **öznitelik Ekle**' ye tıklayın. **Adı** girin ve açılan listeden **değeri** seçin.

   2. Kaydet ' e tıklayın **.** Yeni özniteliğini tablosunda görürsünüz.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

Azure AD 'den uygulama meta verilerini veya sertifikasını indirmek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Çoklu oturum açma yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. **SAML Imzalama sertifikası** bölümüne gidin ve ardından sütun değerini **İndir** ' e tıklayın. Uygulamanın çoklu oturum açmayı yapılandırma ihtiyacı olduğuna bağlı olarak, meta veri XML 'sini veya sertifikayı indirme seçeneğini görürsünüz.

   Azure AD meta verileri almak için bir URL sağlamıyor. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için Federasyon çoklu oturum açmayı yapılandırma

Galeri olmayan bir uygulamayı yapılandırmak için Azure AD Premium 'a sahip olmanız ve uygulamanın SAML 2,0 ' yi desteklemesi gerekir. Azure AD sürümleri hakkında daha fazla bilgi için [Azure AD fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)ziyaret edin.

-   [Azure AD 'de uygulamanın meta veri değerlerini yapılandırma (oturum açma URL 'SI, tanımlayıcı, yanıt URL 'SI)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı ' nı seçin ve uygulamaya gönderilecek kullanıcı özniteliklerini ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulamada Azure AD meta veri değerlerini yapılandırma (oturum açma URL 'SI, verenin, oturum kapatma URL 'SI ve sertifika)](#configure-the-application-for-password-single-sign-on-1)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Azure AD 'de uygulamanın meta veri değerlerini yapılandırma (oturum açma URL 'SI, tanımlayıcı, yanıt URL 'SI)

Azure AD galerisinde olmayan bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6. **kendi uygulamanızı ekleme** bölümünde **Galeri dışı uygulama** ' ya tıklayın.

7. **Ad** metin kutusuna uygulamanın adını girin.

8. Uygulamayı eklemek için **Ekle** düğmesine tıklayın.

9. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

10. **Mod** açılan menüsünde **SAML tabanlı oturum açma '** yı seçin.

11. **Etki alanı ve URL 'ler** için gerekli değerleri girin. Bu değerleri uygulama satıcısından almalısınız.

    1. Uygulamayı IDP tarafından başlatılan SSO olarak yapılandırmak için, yanıt URL 'sini ve tanımlayıcıyı girin.

    2.  **Isteğe bağlı:** Uygulamayı SP tarafından başlatılan SSO olarak yapılandırmak için, oturum açma URL 'SI gerekli bir değerdir.

12. **Kullanıcı öznitelikleri**' nde, **Kullanıcı tanımlayıcısı** açılan menüsünde kullanıcılarınız için benzersiz tanımlayıcıyı seçin.

13. **Isteğe bağlı:** kullanıcılar oturum açtığında SAML belirtecinde uygulamaya gönderilecek öznitelikleri düzenlemek için **diğer tüm Kullanıcı özniteliklerini görüntüle ve Düzenle** ' ye tıklayın.

    Bir öznitelik eklemek için:

    1. **öznitelik Ekle**' ye tıklayın. **Adı** girin ve açılan listeden **değeri** seçin.

    2. Kaydet ' e tıklayın **.** Yeni özniteliği tabloda görürsünüz.

14. uygulamada çoklu oturum açmayı yapılandırma hakkında belgeye erişmek için **uygulama adını &lt;&gt; Yapılandır** ' a tıklayın. Ayrıca, Azure AD URL 'Lerine ve uygulama için gereken sertifikalara sahip olursunuz.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı ' nı seçin ve uygulamaya gönderilecek kullanıcı özniteliklerini ekleyin

Kullanıcı tanımlayıcısını seçmek veya Kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Çoklu oturum açma yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. **Kullanıcı öznitelikleri** bölümünde, **Kullanıcı tanımlayıcısı** açılan menüsünde kullanıcılarınız için benzersiz tanımlayıcıyı seçin. Seçili seçeneğin, kullanıcının kimliğini doğrulamak için uygulamadaki beklenen değerle eşleşmesi gerekir.

   >[!NOTE] 
   >Azure AD, NameID özniteliği (Kullanıcı tanımlayıcısı) için seçilen değere veya SAML AuthRequest içinde uygulama tarafından istenen biçime göre biçim seçer. Daha fazla bilgi için, Nameıdpolicy bölümünde [Çoklu oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) makalesini ziyaret edin.
   >
   >

9. Kullanıcı öznitelikleri eklemek için, kullanıcılar oturum açtığında SAML belirtecinde uygulamaya gönderilecek öznitelikleri düzenlemek için **diğer tüm Kullanıcı özniteliklerini görüntüle ve Düzenle** ' ye tıklayın.

   Bir öznitelik eklemek için:

   1. **öznitelik Ekle**' ye tıklayın. **Adı** girin ve açılan listeden **değeri** seçin.

   2. Kaydet ' e tıklayın **.** Yeni özniteliği tabloda görürsünüz.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

Azure AD 'den uygulama meta verilerini veya sertifikasını indirmek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Çoklu oturum açma yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. **SAML Imzalama sertifikası** bölümüne gidin ve ardından sütun değerini **İndir** ' e tıklayın. Uygulamanın çoklu oturum açmayı yapılandırma ihtiyacı olduğuna bağlı olarak, meta veri XML 'sini veya sertifikayı indirme seçeneğini görürsünüz.

Azure AD meta verileri almak için bir URL sağlamıyor. Meta veriler yalnızca bir XML dosyası olarak alınabilir.

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulaması için parola çoklu oturum açmayı yapılandırma

Azure AD Galerisi 'nden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Azure AD Galerisi 'nden uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Uygulamayı parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galerisi 'nden uygulama ekleme

Azure AD Galerisi 'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  [Azure Portal](https://portal.azure.com) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6.  **Galeriden Ekle** bölümünden **bir ad girin** metin kutusuna uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulamayı eklemeden önce **ad metin kutusundan adını değiştirebilirsiniz** .

9.  Uygulamayı eklemek için **Ekle** düğmesine tıklayın.

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

#### <a name="configure-the-application-for-password-single-sign-on"></a>Uygulamayı parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. Mod **parola tabanlı oturum açmayı seçin.**

9. [Kullanıcılara uygulama atama](#how-to-assign-a-user-to-an-application-directly).

10. Ayrıca, kullanıcıların adına Kullanıcı adına kimlik bilgileri de sağlayabilirsiniz ve **kimlik bilgilerini güncelleştir** ' i tıklatarak ve Kullanıcı adına Kullanıcı adını ve parolayı girebilirsiniz. Aksi takdirde, kullanıcılardan başlatma sırasında kimlik bilgilerini girmesi istenir.

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için parola çoklu oturum açmayı yapılandırma

Azure AD Galerisi 'nden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Galeri dışı bir uygulama ekleme](#add-a-non-gallery-application)

-   [Uygulamayı parola çoklu oturum açma için yapılandırma](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a>Galeri dışı bir uygulama ekleme

Azure AD Galerisi 'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  [Azure Portal](https://portal.azure.com) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6.  **Galeri dışı uygulama** ' ya tıklayın.

7.  **Ad** metin kutusuna uygulamanızın adını girin. Ekle ' yi seçin **.**

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

#### <a name="configure-the-application-for-password-single-sign-on"></a><a name="configure-the-application-for-password-single-sign-on-1"></a>Uygulamayı parola çoklu oturum açma için yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

    1.  Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6.  Çoklu oturum açmayı yapılandırmak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8.  Mod **parola tabanlı oturum açmayı seçin.**

9.  **Oturum açma URL 'sini**girin. Bu, kullanıcıların oturum açmak için Kullanıcı adını ve parolasını girmesi gereken URL 'dir. Oturum açma alanlarının URL 'de görünür olduğundan emin olun.

10. [Kullanıcılara uygulama atama](#how-to-assign-a-user-to-an-application-directly).

11. Ayrıca, kullanıcıların adına Kullanıcı adına kimlik bilgileri de sağlayabilirsiniz ve **kimlik bilgilerini güncelleştir** ' i tıklatarak ve Kullanıcı adına Kullanıcı adını ve parolayı girebilirsiniz. Aksi takdirde, kullanıcılardan başlatma sırasında kimlik bilgilerini girmesi istenir.

## <a name="problems-related-to-assigning-applications-to-users"></a>Kullanıcılara uygulama atama ile ilgili sorunlar

Bir Kullanıcı, uygulamaya atanmadığından, erişim panelinde bir uygulama görmeyebilir. Aşağıda, denetlenecek bazı yollar verilmiştir:

-   [Bir kullanıcının uygulamaya atanıp atanmadığını denetleyin](#check-if-a-user-is-assigned-to-the-application)

-   [Bir kullanıcıyı doğrudan uygulamaya atama](#how-to-assign-a-user-to-an-application-directly)

-   [Kullanıcının uygulamayla ilgili bir lisansa atanıp atanmadığını denetleyin](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Bir kullanıcıya lisans atama](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a>Bir kullanıcının uygulamaya atanıp atanmadığını denetleyin

Bir kullanıcının uygulamaya atanıp atanmadığını denetlemek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

6. Söz konusu uygulamanın adını **arayın** .

7. **Kullanıcılar ve gruplar**' a tıklayın.

8. Kullanıcının uygulamaya atanıp atanmadığından emin olun.

   * Değilse, "bir uygulamayı doğrudan uygulamaya atama" bölümündeki adımları uygulayın.

### <a name="how-to-assign-a-user-to-an-application-directly"></a>Bir kullanıcıyı doğrudan uygulamaya atama

Bir uygulamaya doğrudan bir veya daha fazla kullanıcı atamak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici**olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Listeden Kullanıcı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8. **Atama Ekle** bölmesini açmak için **Kullanıcılar ve gruplar** listesinin üstündeki **Ekle** düğmesine tıklayın.

9. **atama Ekle** bölmesinden **Kullanıcılar ve gruplar** seçicisini tıklatın.

10. **Ada veya e-posta adresine göre ara** arama kutusuna, ilgilendiğiniz kullanıcının **tam adını** veya **e-posta adresini** yazın.

11. Bir **onay kutusunu**açığa çıkarmak için listedeki **kullanıcının** üzerine gelin. Kullanıcıyı **Seçili** listeye eklemek için kullanıcının profil fotoğrafı veya logosu yanındaki onay kutusuna tıklayın.

12. **Isteğe bağlı:** Birden **fazla kullanıcı eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir **tam ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusuna tıklayın.

13. Kullanıcıları seçmeyi tamamladığınızda, uygulamayı atanacak kullanıcılar ve gruplar listesine eklemek için **Seç** düğmesine tıklayın.

14. **Isteğe bağlı:** seçtiğiniz kullanıcılara atanacak bir rol seçmek Için **atama Ekle** bölmesinde **rol seçicisini Seç** ' e tıklayın.

15. Uygulamayı seçili kullanıcılara atamak için **ata** düğmesine tıklayın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar erişim panelinde bu uygulamaları başlatabilir.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Kullanıcının uygulamayla ilgili bir lisansın altında olup olmadığını denetleyin

Bir kullanıcının atanmış lisanslarını denetlemek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5. **tüm kullanıcılar**' a tıklayın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7. kullanıcının şu anda hangi lisansları atandığını görmek için **lisanslar** ' a tıklayın.

   * Kullanıcı bir Office lisansına atanırsa bu, birinci taraf Office uygulamalarının kullanıcının erişim panelinde görünmesini sağlar.

### <a name="how-to-assign-a-user-a-license"></a>Kullanıcıya lisans atama 

Bir kullanıcıya lisans atamak için aşağıdaki adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm kullanıcılar**' a tıklayın.

6.  İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7.  kullanıcının şu anda hangi lisansları atandığını görmek için **lisanslar** ' a tıklayın.

8.  **ata** düğmesine tıklayın.

9.  Kullanılabilir ürünler listesinden **bir veya daha fazla ürün** seçin.

10. **Isteğe bağlı** olarak, ürünleri daha fazla atamak için **atama seçenekleri** öğesine tıklayın. Bu tamamlandığında **Tamam** ' a tıklayın.

11. Bu lisansları bu kullanıcıya atamak için **ata** düğmesine tıklayın.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Gruplara uygulama atama ile ilgili sorunlar

Bir Kullanıcı, uygulamanın atandığı bir grubun parçası olduklarından, erişim panelinde bir uygulama görüyor olabilir. Aşağıda, denetlenecek bazı yollar verilmiştir:

-   [Kullanıcının grup üyeliklerini denetleme](#check-a-users-group-memberships)

-   [Bir uygulamayı doğrudan bir gruba atama](#how-to-assign-an-application-to-a-group-directly)

-   [Bir kullanıcının bir lisansa atanan grubun parçası olup olmadığını denetleyin](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Bir gruba lisans atama](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a>Kullanıcının grup üyeliklerini denetleme

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5. **tüm kullanıcılar**' a tıklayın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7. **gruplar**' a tıklayın.

8. Kullanıcının uygulamaya atanan bir grubun parçası olup olmadığını denetleyin.

   * Kullanıcıyı gruptan kaldırmak istiyorsanız, grubun **satırına tıklayın ve Sil ' i** seçin.

### <a name="how-to-assign-an-application-to-a-group-directly"></a>Bir uygulamayı doğrudan bir gruba atama

Bir uygulamaya doğrudan bir veya daha fazla grup atamak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici**olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tüm uygulamalarınızın listesini görüntülemek için **tüm uygulamalar** ' a tıklayın.

   * Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar** olarak ayarlayın.

6. Listeden Kullanıcı atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8. **Atama Ekle** bölmesini açmak için **Kullanıcılar ve gruplar** listesinin üstündeki **Ekle** düğmesine tıklayın.

9. **atama Ekle** bölmesinden **Kullanıcılar ve gruplar** seçicisini tıklatın.

10. **Ada veya e-posta adresine göre ara** arama kutusuna, ilgilendiğiniz grubun **tam grup adını** yazın.

11. Bir **onay kutusunu**açığa çıkarmak için listedeki **grubun** üzerine gelin. Kullanıcıyı **Seçili** listeye eklemek için grubun profil fotoğrafı veya logosu yanındaki onay kutusuna tıklayın.

12. **Isteğe bağlı:** Birden **fazla grup eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir **tam grup adı** yazın ve bu grubu **Seçili** listeye eklemek için onay kutusuna tıklayın.

13. Grupları seçmeyi tamamladığınızda, uygulamayı atanacak kullanıcılar ve gruplar listesine eklemek için **Seç** düğmesine tıklayın.

14. **Isteğe bağlı:** seçtiğiniz gruplara atanacak bir rol seçmek Için **atama Ekle** bölmesinde **rol seçicisini Seç** ' e tıklayın.

15. Uygulamayı seçili gruplara atamak için **ata** düğmesine tıklayın.

Kısa bir süre sonra, seçtiğiniz kullanıcılar erişim panelinde bu uygulamaları başlatabilir.

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a>Bir kullanıcının bir lisansa atanan grubun parçası olup olmadığını denetleyin

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5. **tüm kullanıcılar**' a tıklayın.

6. İlgilendiğiniz kullanıcıyı **arayın** ve seçilecek **satıra tıklayın** .

7. **gruplar**' a tıklayın.

8. belirli bir grubun satırına tıklayın.

9. grubun hangi lisansa atandığını görmek için **lisanslar** ' a tıklayın.

   * Grup bir Office lisansına atanırsa bu durum, bazı birinci taraf Office uygulamalarının kullanıcının erişim panelinde görünmesini sağlayabilir.

### <a name="how-to-assign-a-license-to-a-group"></a>Bir gruba lisans atama

Bir gruba lisans atamak için aşağıdaki adımları izleyin:

1.  [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4.  Gezinti menüsünde **Kullanıcılar ve gruplar** ' a tıklayın.

5.  **tüm gruplar**' a tıklayın.

6.  İlgilendiğiniz grubu **arayın** ve seçilecek **satıra tıklayın** .

7.  grubun şu anda hangi lisanslardan atandığını görmek için **lisanslar** ' a tıklayın.

8.  **ata** düğmesine tıklayın.

9.  Kullanılabilir ürünler listesinden **bir veya daha fazla ürün** seçin.

10. **Isteğe bağlı** olarak, ürünleri daha fazla atamak için **atama seçenekleri** öğesine tıklayın. Bu tamamlandığında **Tamam** ' a tıklayın.

11. Bu lisansları bu gruba atamak için **ata** düğmesine tıklayın. Bu işlem, grubun boyutuna ve karmaşıklığına bağlı olarak uzun zaman alabilir.

>[!NOTE]
>Bunu daha hızlı yapmak için, kullanıcıya doğrudan bir lisans atamayı göz önünde bulundurun. 
>
>

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory'ye yeni kullanıcı ekleme](./../fundamentals/add-users-azure-active-directory.md)

