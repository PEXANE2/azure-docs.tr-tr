---
title: 'Öğretici: Floxx Labs ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Floxx Labs arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d8fac770-bb57-4e1f-b50b-9ffeae239d07
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/01/2019
ms.author: jeedes
ms.openlocfilehash: e624520a9d1f39bc8115ac72e9df0398065928f1
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67102385"
---
# <a name="tutorial-azure-active-directory-integration-with-fluxx-labs"></a>Öğretici: Floxx Labs ile tümleştirme Azure Active Directory

Bu öğreticide, Floxx laboratuvarlarını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Floxx laboratuvarlarını Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Floxx Labs 'e erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Floxx Labs (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini akışkan xx Labs ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Floxx Labs çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Floxx Labs **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-fluxx-labs-from-the-gallery"></a>Galeriden Floxx Labs ekleme

Floxx Labs 'in Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeri 'den yönetilen SaaS uygulamaları listenize Floxx Labs eklemeniz gerekir.

**Galeriden Floxx Labs eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Floxx Labs**yazın, sonuç panelinden **floxx Labs** ' i seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

     ![Sonuçlar listesinde floxx Labs](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Floxx Labs ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Floxx laboratuvardaki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açmayı akıcı xx Labs ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[Akışkan xx Labs çoklu oturum açma yapılandırma](#configure-fluxx-labs-single-sign-on)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Floxx Labs test kullanıcısı oluşturun](#create-fluxx-labs-test-user)** . Bu, kullanıcının Azure AD gösterimine bağlı olan akıcı xx laboratuvarlarında Britta Simon 'a sahip olmak için.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Floxx Labs ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **floxx Labs** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki adımları uygulayın:

    ![Floxx Labs etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

    | Ortam | URL kalıbı|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com`|

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    | Ortam | URL kalıbı|
    |-------------|------------|
    | Üretim | `https://<subdomain>.fluxx.io/auth/saml/callback` |
    | Üretim öncesi | `https://<subdomain>.preprod.fluxxlabs.com/auth/saml/callback`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Floxx Labs istemci destek ekibine](mailto:travis@fluxxlabs.com) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Akışkan xx Labs 'ı ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-fluxx-labs-single-sign-on"></a>Floxx Labs çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Floxx Labs şirket sitenizde yönetici olarak oturum açın.

2. **Ayarlar** bölümünün altındaki **yönetici** ' yi seçin.

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config1.png)

3. Yönetim panelinde, **Eklentiler** > **tümleştirme** ' i seçin ve ardından **SAML SSO-(devre dışı)** seçeneğini belirleyin.

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config2.png)

4. Öznitelik bölümünde aşağıdaki adımları uygulayın:

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config3.png)

    a. **SAML SSO** onay kutusunu seçin.

    b. **Istek yolu** metin kutusuna **/AUTH/SAML**yazın.

    c. **Geri çağırma yolu** metin kutusuna **/AUTH/SAML/callback**yazın.

    d. **Onaylama tüketici hizmeti URL 'si (çoklu oturum açma URL 'si)** metin kutusunda, Azure Portal girdiğiniz **yanıt URL 'si** değerini girin.

    e. **Hedef kitle (SP VARLıK kimliği)** metin kutusuna, Azure Portal girdiğiniz **tanımlayıcı** değerini girin.

    f. **Kimlik sağlayıcısı SSO hedef URL** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    g. Base-64 kodlu sertifikanızı Not defteri 'nde açın, içeriğini panonuza kopyalayın ve ardından **kimlik sağlayıcısı sertifikası** metin kutusuna yapıştırın.

    h. **Ad tanımlayıcı biçimi** metin kutusuna değeri `urn:oasis:names:tc:SAML:1.1:nameid-format:emailAddress`girin.

    i. **Kaydet**’e tıklayın.

    > [!NOTE]
    > İçerik kaydedildikten sonra, bu alan güvenlik için boş görünür, ancak değer yapılandırmada kaydedilir.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın brittasimon@yourcompanydomain.extension. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, akıcı xx Labs 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **floxx Labs**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Floxx Labs**' i seçin.

    ![Uygulamalar listesindeki Floxx Labs bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-fluxx-labs-test-user"></a>Floxx Labs test kullanıcısı oluşturma

Azure AD kullanıcılarının akıcı xx Labs 'de oturum açmasını sağlamak için, Floxx Labs 'e sağlanması gerekir. Akıcı xx Labs durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Floxx Labs şirket sitenizde yönetici olarak oturum açın.

2. Aşağıdaki görüntülenmiş **simgeye**tıklayın.

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config6.png)

3. Panoda, **yenı kişiler** kartını açmak için aşağıdaki görüntülenmiş simgeye tıklayın.

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config4.png)

4. **Yenı kişiler** bölümünde aşağıdaki adımları uygulayın:

    ![Floxx Labs yapılandırması](./media/fluxxlabs-tutorial/config5.png)

    a. Floxx Labs, SSO oturum açmaları için benzersiz tanımlayıcı olarak e-postayı kullanır. **SSO UID** ALANıNı, SSO ile oturum açma olarak kullandıkları e-posta adresiyle eşleşen kullanıcının e-posta adresiyle doldurun.

    b. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Floxx Labs kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Floxx laboratuvarlarında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

