---
title: 'Öğretici: müzik kapatılırken Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve müzik kapatılırken çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 6f86a8a2-4bd0-40cc-b1b4-752fce123328
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 08f084bc0231bceeaf2c7d87dc0a160cdf188978
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67090974"
---
# <a name="tutorial-azure-active-directory-integration-with-settling-music"></a>Öğretici: müzik kapatılırken Azure Active Directory tümleştirme

Bu öğreticide, Azure Active Directory (Azure AD) ile müziğin nasıl tümleştirileceğini öğreneceksiniz.
Azure AD ile müziğin kullanımının tümleştirilmesi aşağıdaki avantajları sağlar:

* Azure AD 'de müzik kapatma erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla müziğin (çoklu oturum açma) bir şekilde kapatma için otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini müziğin kapatılırken yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Müziğin çoklu oturum açma etkin aboneliğini kapatma

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Müziğin kapatılırken **SP** tarafından başlatılan SSO desteklenir

## <a name="adding-settling-music-from-the-gallery"></a>Galeriden müziğin kapatma ekleme

Azure AD 'ye müzik kapatma tümleştirmesini yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize kadar olan müzikleri eklemeniz gerekir.

**Galeriden müzik kapatma eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **müziği kapatma**yazın, sonuç panelinden **müziği kapatma** ' yı seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![Sonuçlar listesinde müzik kapatma](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre müzik KAPATıLıRKEN Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ile müziğin kapatılırken ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD 'ye yönelik çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[müziğin çoklu oturum açmayı kapatma](#configure-settling-music-single-sign-on)** ' yı yapılandırma.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan müziği kapatırken Britta Simon 'a sahip olmak için bir for **[Music test kullanıcısı oluşturun](#create-settling-music-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı, müziğin kapatılırken yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **müzik** Uygulama tümleştirmesini kapatma sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Müzik etki alanı ve URL 'Leri kapatma çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<SUBDOMAIN>.rakurakuseisan.jp/<USERACCOUNT>/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [müziğin istemci destek ekibinin kapatılırken](https://rakurakuseisan.jp/) iletişim kurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Müziği kapatma bölümünü ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-settling-music-single-sign-on"></a>Müziğin çoklu oturum açma için kapatma yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, bir güvenlik yöneticisi olarak müziğin kapatılırken oturum açın.

1. Sayfanın üstünde **Yönetim** sekmesi ' ne tıklayın.

    ![Müzik adım kapatma](./media/settlingmusic-tutorial/tutorial_settlingmusic_step1.png)

1. **Sistem ayarı** sekmesine tıklayın.

    ![Müzik Step2 kapatma](./media/settlingmusic-tutorial/tutorial_settlingmusic_step2.png)

1. **Güvenlik** sekmesine geçin.

    ![Müzik step3 kapatma](./media/settlingmusic-tutorial/tutorial_settlingmusic_step3.png)

1. **Çoklu oturum açma ayarı** bölümünde aşağıdaki adımları uygulayın:

    ![Müzik Step5 kapatma](./media/settlingmusic-tutorial/tutorial_settlingmusic_step4.png)

    a. **Etkinleştirmek için**tıklayın.

    b. **Kimlik sağlayıcı metin kutusunun oturum açma URL 'si** içinde, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    c. **Kimlik sağlayıcısı oturum kapatma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **Logout URL 'si** değerini yapıştırın.

    d. Azure portal formunu indirdiğiniz **sertifikayı (base64)** karşıya yüklemek Için **Dosya Seç** ' e tıklayın.

    e. **Kaydet** düğmesine tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, müziğin kapatma erişimine izin vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **müziği kapatma**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **müziği kapatma**' yı seçin.

    ![Uygulamalar listesinde müzik kapatma bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-settling-music-test-user"></a>Kapatma müziği test kullanıcısı oluştur

Bu bölümde, müziğin kapatılırken Britta Simon adlı bir Kullanıcı oluşturacaksınız. Çalışan müzikler platformunda kullanıcıları eklemek için, [müziğin kapatılırken müzik istemcisi destek ekibi](https://rakurakuseisan.jp/) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde müziği kapatma kutucuğunu tıklattığınızda, SSO 'yu ayarladığınız müziğin kapatılırken otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)