---
title: 'Öğretici: Jbıt yardım masası ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Jbıt yardım masası arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 5d4900713cd8d96180bf74b300a738a8b676421e
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88547163"
---
# <a name="tutorial-azure-active-directory-integration-with-jitbit-helpdesk"></a>Öğretici: Jbıt yardım masanızla Azure Active Directory tümleştirme

Bu öğreticide, Jbıt yardım masanızla Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz.
Jbıt yardım masasına Azure AD ile tümleştirme aşağıdaki avantajları sağlar:

* Azure AD 'de, Jbıt yardım masasına erişimi olan denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Jbıt yardım masasına (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Jbıt yardım masası ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Jbıt yardım masası çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Jbıt Yardım Masası **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-jitbit-helpdesk-from-the-gallery"></a>Galeriden Jbıt yardım masası ekleme

Jbıt yardım masasının Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden, yönetilen SaaS uygulamaları listenize Jbıt yardım masası eklemeniz gerekir.

**Galeriden Jbıt yardım masası eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Ara kutusuna **Jbıt yardım masası**yazın, sonuç panelinden **Jbıt yardım masası** ' nı seçin ve sonra uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![Sonuçlar listesinde jbıt yardım masası](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Jbıt yardım masanızla yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Jbıt yardım masadaki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı, Jbıt yardım masası ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[Jbıt yardım masası çoklu oturum açma yapılandırma](#configure-jitbit-helpdesk-single-sign-on)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Jbıt yardım masası test kullanıcısı oluşturun](#create-jitbit-helpdesk-test-user)** . Bu, kullanıcının Azure AD gösterimine bağlı olan Jbıt yardım masasına Britta Simon 'un bir karşılığı.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Jbıt yardım masası ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Jbıt yardım masası** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Jbıt yardım masası etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:
    | |
    | ----------------------------------------|
    | `https://<hostname>/helpdesk/User/Login`|
    | `https://<tenant-name>.Jitbit.com`|
    | |
    
    > [!NOTE] 
    > Bu değer gerçek değil. Bu değeri, gerçek oturum açma URL 'siyle güncelleştirin. Bu değeri almak için, [Jbıt yardım masası istemci destek ekibine](https://www.jitbit.com/support/) başvurun.

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusuna aşağıdaki şekılde bir URL yazın:`https://www.jitbit.com/web-helpdesk/`

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Jbit yardım masası 'Nı ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-jitbit-helpdesk-single-sign-on"></a>Jbıt yardım masası çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Jbıt yardım masası şirket sitenizde yönetici olarak oturum açın.

1. Üstteki araç çubuğunda **Yönetim**' e tıklayın.

    ![Yönetim](./media/jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

1. **Genel ayarlar**' a tıklayın.

    ![Kullanıcılar, şirketler ve izinler](./media/jitbit-helpdesk-tutorial/ic777680.png "Kullanıcılar, şirketler ve izinler")

1. **Kimlik doğrulama ayarları** yapılandırması bölümünde aşağıdaki adımları uygulayın:

    ![Kimlik doğrulaması ayarları](./media/jitbit-helpdesk-tutorial/ic777683.png "Kimlik doğrulaması ayarları")

    a. Çoklu oturum açma (SSO) kullanarak **Onelogin**ile oturum açmak için **SAML 2,0 çoklu oturum açmayı etkinleştir**' i seçin.

    b. **Uç nokta URL** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    c. **Base-64** kodlu sertifikanızı Not defteri 'nde açın, bu içeriği panonuza kopyalayın ve **X. 509.440 sertifikası** metin kutusuna yapıştırın

    d. **Değişiklikleri Kaydet**' e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alan türü**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Azure çoklu oturum açma özelliğini kullanarak, Jbıt yardım masasına erişim izni vererek Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **jbıt yardım masası**' nı seçin

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Jbıt yardım masası**' nı seçin.

    ![Uygulamalar listesinde Jbıt yardım masası bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-jitbit-helpdesk-test-user"></a>Jbıt yardım masası test kullanıcısı oluşturma

Azure AD kullanıcılarının, Jbıt yardım masasına oturum açmasını sağlamak için, bu kullanıcıların Jbıt yardım masasına sağlanması gerekir. Jbıt yardım masasının sağlanması, el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Jbıt yardım masası** kiracınızda oturum açın.

1. Üstteki menüde **Yönetim**' e tıklayın.

    ![Yönetim](./media/jitbit-helpdesk-tutorial/ic777681.png "Yönetim")

1. **Kullanıcılar, şirketler ve izinler**' e tıklayın.

    ![Kullanıcılar, şirketler ve izinler](./media/jitbit-helpdesk-tutorial/ic777682.png "Kullanıcılar, şirketler ve izinler")

1. **Kullanıcı Ekle**' ye tıklayın.

    ![Kullanıcı ekle](./media/jitbit-helpdesk-tutorial/ic777685.png "Kullanıcı ekleme")

1. Oluştur bölümünde, sağlamak istediğiniz Azure AD hesabının verilerini aşağıdaki gibi yazın:

    ![Oluştur](./media/jitbit-helpdesk-tutorial/ic777686.png "Oluştur")

   a. Kullanıcı **adı** metin kutusuna, **Brittasıon**gibi kullanıcının Kullanıcı adını yazın.

   b. **E-posta** metin kutusuna, gibi kullanıcının e-postasını yazın **BrittaSimon@contoso.com** .

   c. **Ilk ad** metin kutusuna, **Britta**gibi kullanıcının adını yazın.

   d. **Soyadı** metin kutusunda, **Simon**adlı kullanıcının soyadını yazın.

   e. **Oluştur**’a tıklayın.

> [!NOTE]
> Azure AD Kullanıcı hesaplarını sağlamak için, jbıx yardım masası tarafından sunulan diğer bir Jbım Yardım Masası Kullanıcı hesabı oluşturma araçlarını veya API 'Leri kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Jbıt yardım masası kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Jbıt yardım masasına otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
