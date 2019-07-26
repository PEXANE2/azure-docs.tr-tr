---
title: Azure AD Galeri uygulaması için Federasyon çoklu oturum açmayı yapılandırma | Microsoft Docs
description: Mevcut bir Azure AD Galeri uygulaması için Federasyon çoklu oturum açmayı yapılandırma ve öğreticilerle hızlı bir şekilde çalışmaya başlama
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
ROBOTS: NOINDEX
ms.openlocfilehash: bb5d2c693047dd0aa53430ba531dfd246cc77be9
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422537"
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Azure AD Galeri uygulaması için Federasyon çoklu oturum açmayı yapılandırma

Azure AD galerisinde Kurumsal Çoklu oturum açma özelliğiyle etkinleştirilen tüm uygulamaların sunulan adım adım öğretici vardır. Ayrıntılı adım adım yönergeler için, [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğretici listesine](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) erişebilirsiniz.

## <a name="overview-of-steps-required"></a>Gerekli adımlara genel bakış
Azure AD Galerisi 'nden bir uygulamayı yapılandırmak için şunları yapmanız gerekir:

-   [Azure AD Galerisi 'nden uygulama ekleme](#add-an-application-from-the-azure-ad-gallery)

-   [Azure AD 'de uygulamanın meta veri değerlerini yapılandırma (oturum açma URL 'SI, tanımlayıcı, yanıt URL 'SI)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Kullanıcı tanımlayıcısı ' nı seçin ve uygulamaya gönderilecek kullanıcı özniteliklerini ekleyin](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD meta verilerini ve sertifikasını alma](#download-the-azure-ad-metadata-or-certificate)

-   [Uygulamada Azure AD meta veri değerlerini yapılandırma (oturum açma URL 'SI, verenin, oturum kapatma URL 'SI ve sertifika)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Uygulamaya Kullanıcı atama](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Azure AD Galerisi 'nden uygulama ekleme

Azure AD Galerisi 'nden bir uygulama eklemek için aşağıdaki adımları izleyin:

1.  [Azure Portal](https://portal.azure.com) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın

2.  Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5.  **Kurumsal uygulamalar** bölmesindeki sağ üst köşedeki **Ekle** düğmesine tıklayın.

6.  **Galeriden Ekle** bölümünden **bir ad girin** metin kutusuna uygulamanın adını yazın.

7.  Çoklu oturum açma için yapılandırmak istediğiniz uygulamayı seçin.

8.  Uygulamayı eklemeden önce **ad metin kutusundan** adını değiştirebilirsiniz.

9.  Uygulamayı eklemek için **Ekle** düğmesine tıklayın.

Kısa bir süre sonra, uygulamanın yapılandırma bölmesini görebilirsiniz.

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Azure AD galerisinden bir uygulama için çoklu oturum açmayı yapılandırma

Bir uygulama için çoklu oturum açmayı yapılandırmak için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici**olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

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

    1. Kaydet ' e tıklayın **.** Yeni özniteliği tabloda görürsünüz.

13. uygulamada çoklu oturum açmayı yapılandırma hakkında belgeye erişmek için  **&lt;uygulama adını&gt; Yapılandır** ' a tıklayın. Ayrıca, uygulama ile SSO 'yu ayarlamak için gereken meta veri URL 'Leri ve sertifikaya sahip olursunuz.

14. Yapılandırmayı kaydetmek için **Kaydet** ' e tıklayın.

15. Kullanıcılara uygulama atama.

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Kullanıcı tanımlayıcısı ' nı seçin ve uygulamaya gönderilecek kullanıcı özniteliklerini ekleyin

Kullanıcı tanımlayıcısını seçmek veya Kullanıcı öznitelikleri eklemek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

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

   2. **Kaydet**’e tıklayın. Yeni özniteliği tabloda görürsünüz.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>Azure AD meta verilerini veya sertifikasını indirin

Azure AD 'den uygulama meta verilerini veya sertifikasını indirmek için aşağıdaki adımları izleyin:

1. [**Azure Portal**](https://portal.azure.com/) açın ve **genel yönetici** veya **ortak yönetici** olarak oturum açın.

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   *  Burada görünmesini istediğiniz uygulamayı görmüyorsanız, **tüm uygulamalar listesinin** en üstündeki **filtre** denetimini kullanın ve **göster** seçeneğini **tüm uygulamalar**olarak ayarlayın.

6. Çoklu oturum açma yapılandırdığınız uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Çoklu oturum açma** seçeneğine tıklayın.

8. **SAML Imzalama sertifikası** bölümüne gidin ve ardından sütun değerini **İndir** ' e tıklayın. Uygulamanın çoklu oturum açmayı yapılandırma ihtiyacı olduğuna bağlı olarak, meta veri XML 'sini veya sertifikayı indirme seçeneğini görürsünüz.

Azure AD, meta verileri almak için de bir URL sağlar. Uygulamaya özel meta veri URL 'sini almak için bu kalıbı izleyin:`https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`

## <a name="assign-users-to-the-application"></a>Uygulamaya kullanıcı atama

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Sol taraftaki Gezinti menüsünün en üstündeki **tüm hizmetler** ' i tıklatarak **Azure Active Directory uzantısını** açın.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Azure Active Directory sol taraftaki gezinti menüsünden **Kurumsal uygulamalar** ' a tıklayın.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra, uygulamanın sol taraftaki gezinti menüsünden **Kullanıcılar ve gruplar** ' a tıklayın.

8. Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama isteyen kullanıcının **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **Seçim** Birden **fazla kullanıcı eklemek**istiyorsanız **ada veya e-posta adresine göre ara** kutusuna başka bir **tam ad** veya **e-posta adresi** yazın ve bu kullanıcıyı **Seçili** listeye eklemek için onay kutusuna tıklayın.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

Bir kısa süre sonra seçtiğiniz kullanıcıların çözüm Açıklama bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatması mümkün.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>Bir uygulamaya gönderilen SAML taleplerini özelleştirme

Uygulamanıza gönderilen SAML öznitelik taleplerini özelleştirmeyi öğrenmek için, daha fazla bilgi için [Azure Active Directory 'Da talep eşleme](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) konusuna bakın.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)



