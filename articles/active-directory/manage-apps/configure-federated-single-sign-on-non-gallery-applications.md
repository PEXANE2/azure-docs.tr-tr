---
title: Galeri dışı bir uygulama için Federasyon çoklu oturum açmayı yapılandırma
description: Azure AD ile tümleştirilen özel galeri dışı bir uygulama için Federasyon çoklu oturum açmayı yapılandırma
services: active-directory
documentationcenter: ''
author: kenwith
manager: celestedg
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: kenwith
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28af2a51119cd8328f9fa29000ead1622ae5f5e4
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84763559"
---
# <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Galeri dışı bir uygulama için Federasyon çoklu oturum açmayı yapılandırma

Galeri dışı bir uygulama için *kod yazmadan*çoklu oturum açmayı yapılandırmak için bir aboneliğiniz veya Azure AD Premium olması gerekir ve uygulamanın SAML 2,0 ' i desteklemesi gerekir. Azure AD sürümleri hakkında daha fazla bilgi için [Azure AD fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)ziyaret edin.

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış
Galeri olmayan (ör. özel) bir uygulama için SAML 2,0 ile federe çoklu oturum açmayı yapılandırmak için gereken adımlara yüksek düzeyde bir genel bakış aşağıda verilmiştir.

-   Azure AD 'de uygulamanın meta veri değerlerini yapılandırma (oturum açma URL 'SI, tanımlayıcı, yanıt URL 'SI)

-   [Kullanıcı tanımlayıcısı ' nı seçin ve uygulamaya gönderilecek kullanıcı özniteliklerini ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alma](#download-the-azure-ad-metadata-or-certificate)

-   Uygulamada Azure AD meta veri değerlerini yapılandırma (oturum açma URL 'SI, verenin, oturum kapatma URL 'SI ve sertifika)

-   Uygulamaya kullanıcı atama

## <a name="configuring-single-sign-on-to-non-gallery-applications"></a>Galeri olmayan uygulamalarda çoklu oturum açmayı yapılandırma

Azure AD galerisinde olmayan bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Filtre arama kutusuna **"Azure Active Directory**" yazın ve **Azure Active Directory** öğesini seçin.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6. **kendi uygulamanızı ekleme** bölümünde **Galeri dışı uygulama** ' ya tıklayın

7. **Ad** metin kutusuna uygulamanın adını girin.

8. Uygulamayı eklemek için **Ekle** düğmesine tıklayın.

9. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

10. **Mod** açılan menüsünde **SAML tabanlı oturum açma '** yı seçin.

11. **Etki alanı ve URL 'ler** için gerekli değerleri girin. Bu değerleri uygulama satıcısından almalısınız.

    1. Uygulamayı IDP tarafından başlatılan SSO olarak yapılandırmak için, yanıt URL 'sini ve tanımlayıcıyı girin.

    2. **Isteğe bağlı:** Uygulamayı SP tarafından başlatılan SSO olarak yapılandırmak için, oturum açma URL 'SI gerekli bir değerdir.

12. **Kullanıcı öznitelikleri**' nde, **Kullanıcı tanımlayıcısı** açılan menüsünde kullanıcılarınız için benzersiz tanımlayıcıyı seçin.

13. **Isteğe bağlı:** kullanıcılar oturum açtığında SAML belirtecinde uygulamaya gönderilecek öznitelikleri düzenlemek için **diğer tüm Kullanıcı özniteliklerini görüntüle ve Düzenle** ' ye tıklayın.

    Bir öznitelik eklemek için:

    1. **öznitelik Ekle**' ye tıklayın. **Adı** girin ve açılan listeden **değeri** seçin.

    2. Kaydet ' e tıklayın **.** Yeni özniteliği tabloda görürsünüz.

14. uygulamada çoklu oturum açmayı yapılandırma hakkında belgeye erişmek için ** &lt; uygulama &gt; adını Yapılandır** ' a tıklayın. Ayrıca, Azure AD URL 'Leri ve uygulama için gereken sertifikaya sahip olursunuz.

15. [Kullanıcılara uygulama atama.](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı ' nı seçin ve uygulamaya gönderilecek kullanıcı özniteliklerini ekleyin

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
   >Azure AD, seçilen değere veya SAML AuthRequest 'te uygulama tarafından istenen biçime göre NameID özniteliği (Kullanıcı tanımlayıcısı) biçimini seçin. Daha fazla bilgi için, Nameıdpolicy bölümünde [Çoklu oturum açma SAML Protokolü](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) makalesini ziyaret edin.
   >
   >

9. Kullanıcı öznitelikleri eklemek için, kullanıcılar oturum açtığında SAML belirtecinde uygulamaya gönderilecek öznitelikleri düzenlemek için **diğer tüm Kullanıcı özniteliklerini görüntüle ve Düzenle** ' ye tıklayın.

   Bir öznitelik eklemek için:

   1. **öznitelik Ekle**' ye tıklayın. **Adı** girin ve açılan listeden **değeri** seçin.

   2. Kaydet ' e tıklayın **.** Yeni özniteliği tabloda görürsünüz.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

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

Azure AD, meta verileri almak için de bir URL sağlar. Uygulamaya özel meta veri URL 'sini almak için bu düzene uyun: `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>` .

## <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcı atama

Bir uygulamaya doğrudan bir veya daha fazla kullanıcı atamak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** olarak oturum açın.

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

Kısa bir süre sonra, seçtiğiniz kullanıcılar çözüm açıklaması bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatabilir.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulamaya gönderilen SAML taleplerini özelleştirme

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmeyi öğrenmek için, daha fazla bilgi için [Azure Active Directory 'Da talep eşleme](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama proxy 'Si ile uygulamalarınıza çoklu oturum açma sağlama](application-proxy-configure-single-sign-on-with-kcd.md)
