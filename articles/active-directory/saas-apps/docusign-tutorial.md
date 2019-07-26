---
title: 'Öğretici: DocuSign ile tümleştirme Azure Active Directory | Microsoft Docs'
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
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: 5c7d6116ed2925e57f094a67f27a11f9e2d61831
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/25/2019
ms.locfileid: "68499255"
---
# <a name="tutorial-azure-active-directory-integration-with-docusign"></a>Öğretici: DocuSign ile tümleştirme Azure Active Directory

Bu öğreticide, DocuSign 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
DocuSign 'ı Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de DocuSign 'a erişimi olan denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak DocuSign (çoklu oturum açma) ile oturum açmasını sağlayabilirsiniz.
* Hesaplarınız bir merkezi konumda - Azure portalında yönetebilir.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Önkoşullar

Azure AD tümleştirmesini DocuSign ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* DocuSign çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* DocuSign, **SP** tarafından başlatılan SSO 'yu destekler

* DocuSign [Otomatik Kullanıcı sağlamayı](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial) destekliyor

## <a name="adding-docusign-from-the-gallery"></a>Galeriden DocuSign ekleme

DocuSign 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için Galeri 'den yönetilen SaaS uygulamaları listenize DocuSign ' ı eklemeniz gerekir.

**Galeriden DocuSign eklemek için aşağıdaki adımları uygulayın:**

1. İçinde **[Azure portalında](https://portal.azure.com)** , sol gezinti panelinde tıklayın **Azure Active Directory** simgesi.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için tıklatın **yeni uygulama** iletişim üst kısmındaki düğmesi.

    ![Yeni Uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Docusign**yazın, sonuç panelinden **Docusign** ' ı seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuç listesinde DocuSign](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Yapılandırma ve Azure AD çoklu oturum açmayı test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre DocuSign ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve DocuSign içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Azure AD çoklu oturum açma 'yı DocuSign ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açmayı yapılandırmayı](#configure-azure-ad-single-sign-on)**  - bu özelliği kullanmak, kullanıcılarınızın etkinleştirmek için.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[DocuSign çoklu oturum açmayı yapılandırın](#configure-docusign-single-sign-on)** .
3. **[Bir Azure AD test kullanıcısı oluşturma](#create-an-azure-ad-test-user)**  - Azure AD çoklu oturum açma Britta Simon ile test etmek için.
4. **[Azure AD test kullanıcı atama](#assign-the-azure-ad-test-user)**  - Azure AD çoklu oturum açmayı kullanmak Britta Simon etkinleştirmek için.
5. Kullanıcının Azure AD gösterimine bağlı olan DocuSign 'da Britta Simon 'ın bir karşılığı olan **[Docusign test kullanıcısı oluşturun](#create-docusign-test-user)** .
6. **[Çoklu oturum açmayı test](#test-single-sign-on)**  - yapılandırma çalışıp çalışmadığını doğrulayın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırın

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı DocuSign ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Docusign** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısı yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. Üzerinde **yukarı çoklu oturum açma SAML ile ayarlanmış** sayfasında **Düzenle** açmak için simgeyi **temel SAML yapılandırma** iletişim.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![DocuSign etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2/login/sp/<IDPID>`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.docusign.com/organizations/<OrganizationID>/saml2`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, öğreticideki daha sonra **SAML 2,0 uç noktalarını görüntüle** bölümünde açıklanan gerçek oturum açma URL 'Si ve tanımlayıcı ile güncelleştirin.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **DocuSign 'ı ayarla** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL'si:

    b. Azure AD Tanımlayıcısı

    c. Oturum Kapatma URL'si

### <a name="configure-docusign-single-sign-on"></a>DocuSign çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, bir yönetici olarak **Docusign yönetici portalınıza** oturum açın.

2. Sayfanın sağ üst kısmında profil **logosu** ' na tıklayın ve ardından **yönetici 'ye git ' e**tıklayın.
  
    ![Çoklu oturum açma yapılandırılıyor][51]

3. Etki alanı çözümleri sayfasında **etki alanları** ' na tıklayın.

    ![Çoklu oturum açma yapılandırılıyor][50]

4. **Etki alanları** bölümünde, **talep etki alanı**' na tıklayın.

    ![Çoklu oturum açma yapılandırılıyor][52]

5. **Etki alanı talep** et iletişim kutusunda, etki alanı **adı** metin kutusuna şirket etki alanınızı yazın ve **talep**' e tıklayın. Etki alanını ve durumunun etkin olduğundan emin olun.

    ![Çoklu oturum açma yapılandırılıyor][53]

6. Etki alanı çözümleri sayfasında **kimlik sağlayıcıları**' na tıklayın.
  
    ![Çoklu oturum açma yapılandırılıyor][54]

7. **Kimlik sağlayıcıları** bölümünde, **kimlik sağlayıcısı ekle**' ye tıklayın. 

    ![Çoklu oturum açma yapılandırılıyor][55]

8. **Kimlik sağlayıcısı ayarları** sayfasında, aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma yapılandırılıyor][56]

    a. **Ad** metin kutusuna yapılandırmanız için benzersiz bir ad yazın. Boşluk kullanmayın.

    b. **Kimlik sağlayıcısı veren metin kutusunda**, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının**değerini yapıştırın.

    c. **Kimlik sağlayıcısı oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si**değerini yapıştırın.

    d. **Kimlik sağlayıcısı oturum kapatma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum kapatma URL 'si**değerini yapıştırın.

    e. **Imza AuthN isteğini**seçin.

    f. **Tarafından AuthN Isteği gönder**olarak **Postala**' yı seçin.

    g. **Tarafından oturum kapatma Isteği gönder**olarak **Al**' ı seçin.

    h. **Özel öznitelik eşleme** bölümünde **Yeni eşleme Ekle**' ye tıklayın.

    ![Çoklu oturum açma yapılandırılıyor][62]

    i. Azure AD talebi ile eşlemek istediğiniz alanı seçin. Bu örnekte, **emadresi** talebi değeri **http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress** ile eşleştirilir. Bu, Azure AD 'den e-posta talebi için varsayılan talep adıdır ve **Kaydet**' e tıklayın.

    ![Çoklu oturum açma yapılandırılıyor][57]

    > [!NOTE]
    > Kullanıcıyı Azure AD 'den DocuSign Kullanıcı eşlemesine eşlemek için uygun **Kullanıcı tanımlayıcısını** kullanın. Uygun alanı seçin ve kuruluşunuzun ayarlarına bağlı olarak uygun değeri girin.

    j. **Kimlik sağlayıcısı sertifikaları** bölümünde **sertifika ekle**' ye tıklayın ve ardından Azure AD portalından indirdiğiniz sertifikayı karşıya yükleyin ve **Kaydet**' e tıklayın.

    ![Çoklu oturum açma yapılandırılıyor][58]

    k. **Kimlik sağlayıcıları** bölümünde **Eylemler**' i ve ardından **uç noktalar**' ı tıklatın.

    ![Çoklu oturum açma yapılandırılıyor][59]

    girişindeki. **Docusign yönetim PORTALıNıN** **SAML 2,0 uç noktalarını görüntüle** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açma yapılandırılıyor][60]

    * **Hizmet sağlayıcı verenin URL 'sini**kopyalayın ve ardından Azure Portal **temel SAML yapılandırması** bölümündeki **tanımlayıcı** metin kutusuna yapıştırın.

    * **Hizmet sağlayıcısı oturum açma URL**'sini kopyalayın ve ardından Azure Portal **temel SAML yapılandırması** bölümündeki **URL 'yi oturum aç** metin kutusuna yapıştırın.

    * **Kapat** 'a tıklayın

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Britta Simon adlı Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede seçin **Azure Active Directory**seçin **kullanıcılar**ve ardından **tüm kullanıcılar**.

    !["Kullanıcılar ve Gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Seçin **yeni kullanıcı** ekranın üstünde.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı özellikleri, aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d.           **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, DocuSign 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Docusign**' ı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **Docusign**' ı seçin.

    ![Uygulamalar listesindeki DocuSign bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. İçinde **kullanıcılar ve gruplar** iletişim kutusunda **Britta Simon** 'a tıklayın kullanıcı listesinde **seçin** ekranın alt kısmındaki düğmesi.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-docusign-test-user"></a>DocuSign test kullanıcısı oluştur

Bu bölümde, DocuSign 'da Britta Simon adlı bir Kullanıcı oluşturulur. DocuSign, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir kullanıcı zaten DocuSign 'da yoksa, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [Docusign destek ekibine](https://support.docusign.com/)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde DocuSign kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız DocuSign 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamaları Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

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
