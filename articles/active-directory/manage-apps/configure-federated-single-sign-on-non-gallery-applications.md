---
title: Galeri dışı bir uygulama için federal tek oturum açma nasıl yapılandırılır?
description: Azure AD ile tümleştirmek istediğiniz özel galeri dışı bir uygulama için federal tek oturum açma nasıl yapılandırılır?
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
ms.collection: M365-identity-device-management
ms.openlocfilehash: c1d49ec5ef80b284aa6b1a305b037d19dae34870
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74274586"
---
# <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için federal tek oturum açma nasıl yapılandırılır?

*Kod yazmadan*galeri olmayan bir uygulama için tek oturum açma yapılandırmak için bir aboneliğiniz veya Azure AD Premium'unuzun olması gerekir ve uygulama SAML 2.0'ı desteklemesi gerekir. Azure AD sürümleri hakkında daha fazla bilgi için [Azure AD fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)sayfasını ziyaret edin.

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış
Aşağıda, galeri dışı (örn. özel) bir uygulama için SAML 2.0 ile federal tek oturum açma yapılandırmak için gereken adımların üst düzey bir özeti verilmiştir.

-   Uygulamanın meta veri değerlerini Azure AD'de yapılandırın (URL'de oturum aç, Tanımlayıcı, Yanıt URL'si)

-   [Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alın](#download-the-azure-ad-metadata-or-certificate)

-   Uygulamada Azure AD meta veri değerlerini yapılandırma (URL'de oturum aç, Veren, Giriş URL'si ve sertifikası)

-   Uygulamaya kullanıcı atama

## <a name="configuring-single-sign-on-to-non-gallery-applications"></a>Galeri dışı uygulamalara tek oturum açma yapılandırma

Azure AD galerisinde olmayan bir uygulama için tek oturum açma yapılandırmak için aşağıdaki adımları izleyin:

1. Azure [**portalını**](https://portal.azure.com/) açın ve **Global Administrator** veya **Co-admin** olarak oturum açın.

2. Ana sol gezinme menüsünün üst kısmındaki **Tüm hizmetleri** tıklatarak Azure **Etkin Dizin Uzantısı'nı** açın.

3. Filtre arama kutusuna **"Azure Etkin Dizini"** yazın ve **Azure Etkin Dizin** öğesini seçin.

4. Azure Active Directory sol navigasyon menüsünden **Kurumsal Uygulamalar'ı** tıklatın.

5. **Kurumsal Uygulamalar** bölmesinin sağ üst köşesindeki **Ekle** düğmesini tıklatın.

6. **kendi uygulamanızı ekle** bölümünde **ki galeri dışı uygulamayı** tıklatın

7. **Ad** metin kutusuna uygulamanın adını girin.

8. Uygulamayı eklemek için **Ekle** düğmesini tıklatın.

9. Uygulama yüklendikten sonra, uygulamanın sol navigasyon menüsünden **Tek oturum** açma'yı tıklatın.

10. **Mod** açılır düşüşünde **SAML tabanlı Oturum** Açma'yı seçin.

11. **Etki Alanı ve URL'lere** gerekli değerleri girin. Bu değerleri uygulama satıcısından almalısınız.

    1. Uygulamayı IdP tarafından başlatılan SSO olarak yapılandırmak için Yanıt URL'sini ve Tanımlayıcıyı girin.

    2. **İsteğe bağlı:** Uygulamayı SP tarafından başlatılan SSO olarak yapılandırmak için, Oturum Açma URL'si gerekli bir değerdir.

12. Kullanıcı **özniteliklerinde,** **Kullanıcı Tanımlayıcı** açılır açılır açılır ayında kullanıcılarınız için benzersiz tanımlayıcıyı seçin.

13. **İsteğe bağlı:** Kullanıcılar oturum açarken SAML belirtecinde uygulamaya gönderilecek öznitelikleri ni tıklatın **ve diğer tüm kullanıcı özniteliklerini tıklatın.**

    Bir öznitelik eklemek için:

    1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

    2. **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

14. uygulamada tek oturum açma nın nasıl yapılandırılabildiğini belgelemek için ** &lt;uygulama adını&gt; yapılandır'ı** tıklatın. Ayrıca, uygulama için gerekli Azure REKLAM URL'leri ve sertifikası vardır.

15. [Kullanıcıları uygulamaya atayın.](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı Tanımlayıcı'yı seçin ve uygulamaya gönderilecek kullanıcı öznitelikleriekleyin

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

   1. **öznitelik ekle'yi**tıklatın. **Adı** girin ve açılır açılır yerden **Değeri** seçin.

   2. **Kaydet'i tıklatın.** Yeni özniteliği tabloda görürsünüz.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

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

Azure AD, meta verileri almak için bir URL de sağlar. Uygulamaya özgü meta veri URL'sini almak `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`için bu deseni izleyin: .

## <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcı atama

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

Kısa bir süre sonra, seçtiğiniz kullanıcılar çözüm açıklaması bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatabilir.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulamaya gönderilen SAML taleplerini özelleştirme

Uygulamanıza gönderilen SAML öznitelik taleplerini nasıl özelleştirebilirsiniz öğrenmek için daha fazla bilgi için [Azure Etkin Dizini'nde Talepler eşleme](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) sine bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Application Proxy ile uygulamalarınızda tek oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
