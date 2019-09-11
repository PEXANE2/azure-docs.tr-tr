---
title: 'Öğretici: DocuSign ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve DocuSign arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a691288b-84c1-40fb-84bd-5b06878865f0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe1f018612e889f49993895d88cdaf9ad732b393
ms.sourcegitcommit: f176e5bb926476ec8f9e2a2829bda48d510fbed7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70306308"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-docusign"></a>Öğretici: DocuSign ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, DocuSign 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. DocuSign 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de DocuSign 'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesapları ile DocuSign 'a otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* DocuSign çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* DocuSign, **SP** tarafından başlatılan SSO 'yu destekler

* DocuSign **tam zamanında** Kullanıcı sağlamayı destekliyor

* DocuSign [Otomatik Kullanıcı sağlamayı](https://docs.microsoft.com/azure/active-directory/saas-apps/docusign-provisioning-tutorial) destekliyor

## <a name="adding-docusign-from-the-gallery"></a>Galeriden DocuSign ekleme

DocuSign 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için Galeri 'den yönetilen SaaS uygulamaları listenize DocuSign ' ı eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Docusign** yazın.
1. Sonuçlar panelinden **Docusign** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-docusign"></a>DocuSign için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak DocuSign Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, bir Azure AD kullanıcısı ve DocuSign içindeki ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu DocuSign ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[DocuSign SSO 'Yu yapılandırın](#configure-docusign-sso)** .
    1. Azure AD gösterimi ile bağlantılı olan DocuSign 'da B. Simon 'a sahip olmak için **[Docusign test kullanıcısı oluşturun](#create-docusign-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Docusign** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, öğreticideki daha sonra **SAML 2,0 uç noktalarını görüntüle** bölümünde açıklanan gerçek oturum açma URL 'Si ve tanımlayıcı ile güncelleştirin.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **DocuSign 'ı ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, username@companydomain.extensiongirin. Örneğin: `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak DocuSign 'a erişim vererek B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Docusign**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-docusign-sso"></a>DocuSign SSO 'yu yapılandırma

1. Bu yapılandırmayı, DocuSign içinde otomatik hale getirmek için, **uzantıyı yüklemek**üzere **uygulamalar güvenli oturum açma tarayıcı uzantısı** ' nı yüklemeniz gerekir.

    ![Uygulamalarım uzantısı](common/install-myappssecure-extension.png)

2. Tarayıcıya Uzantı eklendikten sonra, **Kurulum Docusign** ' a tıklayarak sizi Docusign uygulamasına yönlendirirsiniz. Buradan, DocuSign 'da oturum açmak için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı, uygulamayı sizin için otomatik olarak yapılandırır ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. DocuSign 'ı el ile ayarlamak istiyorsanız yeni bir Web tarayıcı penceresi açın ve bir yönetici olarak DocuSign şirket sitenizde oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın sağ üst kısmında profil **logosu** ' na tıklayın ve ardından **yönetici 'ye git ' e**tıklayın.
  
    ![Çoklu oturum açmayı yapılandırma][51]

5. Etki alanı çözümleri sayfasında **etki alanları** ' na tıklayın.

    ![Çoklu oturum açmayı yapılandırma][50]

6. **Etki alanları** bölümünde, **talep etki alanı**' na tıklayın.

    ![Çoklu oturum açmayı yapılandırma][52]

7. **Etki alanı talep** et iletişim kutusunda, etki alanı **adı** metin kutusuna şirket etki alanınızı yazın ve **talep**' e tıklayın. Etki alanını ve durumunun etkin olduğundan emin olun.

    ![Çoklu oturum açmayı yapılandırma][53]

8. Etki alanı çözümleri sayfasında **kimlik sağlayıcıları**' na tıklayın.
  
    ![Çoklu oturum açmayı yapılandırma][54]

9. **Kimlik sağlayıcıları** bölümünde, **kimlik sağlayıcısı ekle**' ye tıklayın. 

    ![Çoklu oturum açmayı yapılandırma][55]

10. **Kimlik sağlayıcısı ayarları** sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma][56]

    a. **Ad** metin kutusuna yapılandırmanız için benzersiz bir ad yazın. Boşluk kullanmayın.

    b. **Kimlik sağlayıcısı veren metin kutusunda**, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının**değerini yapıştırın.

    c. **Kimlik sağlayıcısı oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si**değerini yapıştırın.

    d. **Kimlik sağlayıcısı oturum kapatma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum kapatma URL 'si**değerini yapıştırın.

    e. **Imza AuthN isteğini**seçin.

    f. **Tarafından AuthN Isteği gönder**olarak **Postala**' yı seçin.

    g. **Tarafından oturum kapatma Isteği gönder**olarak **Al**' ı seçin.

    h. **Özel öznitelik eşleme** bölümünde **Yeni eşleme Ekle**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma][62]

    i. Azure AD talebi ile eşlemek istediğiniz alanı seçin. Bu örnekte, **emadresi** talebi değeri **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** ile eşleştirilir. Bu, Azure AD 'den e-posta talebi için varsayılan talep adıdır ve **Kaydet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma][57]

    > [!NOTE]
    > Kullanıcıyı Azure AD 'den DocuSign Kullanıcı eşlemesine eşlemek için uygun **Kullanıcı tanımlayıcısını** kullanın. Uygun alanı seçin ve kuruluşunuzun ayarlarına bağlı olarak uygun değeri girin.

    j. **Kimlik sağlayıcısı sertifikaları** bölümünde **sertifika ekle**' ye tıklayın ve ardından Azure AD portalından indirdiğiniz sertifikayı karşıya yükleyin ve **Kaydet**' e tıklayın.

    ![Çoklu oturum açmayı yapılandırma][58]

    k. **Kimlik sağlayıcıları** bölümünde **Eylemler**' i ve ardından **uç noktalar**' ı tıklatın.

    ![Çoklu oturum açmayı yapılandırma][59]

    girişindeki. **Docusign yönetim PORTALıNıN** **SAML 2,0 uç noktalarını görüntüle** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma][60]

    * **Hizmet sağlayıcı verenin URL 'sini**kopyalayın ve ardından Azure Portal **temel SAML yapılandırması** bölümündeki **tanımlayıcı** metin kutusuna yapıştırın.

    * **Hizmet sağlayıcısı oturum açma URL**'sini kopyalayın ve ardından Azure Portal **temel SAML yapılandırması** bölümündeki **URL 'yi oturum aç** metin kutusuna yapıştırın.

    * **Kapat** 'a tıklayın

### <a name="create-docusign-test-user"></a>DocuSign test kullanıcısı oluştur

Bu bölümde, DocuSign içinde B. Simon adlı bir Kullanıcı oluşturulur. DocuSign, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir kullanıcı zaten DocuSign 'da yoksa, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [Docusign destek ekibine](https://support.docusign.com/)başvurun.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde DocuSign kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız DocuSign 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile DocuSign 'ı deneyin](https://aad.portal.azure.com/)

<!--Image references-->

[50]: ./media/docusign-tutorial/tutorial_docusign_18.png
[51]: ./media/docusign-tutorial/tutorial_docusign_21.png
[52]: ./media/docusign-tutorial/tutorial_docusign_22.png
[53]: ./media/docusign-tutorial/tutorial_docusign_23.png
[54]: ./media/docusign-tutorial/tutorial_docusign_19.png
[55]: ./media/docusign-tutorial/tutorial_docusign_20.png
[56]: ./media/docusign-tutorial/tutorial_docusign_24.png
[57]: ./media/docusign-tutorial/tutorial_docusign_25.png
[58]: ./media/docusign-tutorial/tutorial_docusign_26.png
[59]: ./media/docusign-tutorial/tutorial_docusign_27.png
[60]: ./media/docusign-tutorial/tutorial_docusign_28.png
[61]: ./media/docusign-tutorial/tutorial_docusign_29.png
[62]: ./media/docusign-tutorial/tutorial_docusign_30.png