---
title: 'Öğretici: Openatina ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Openatina arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: dd4adfc7-e238-41d5-8b25-1811f08078b6
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 1/4/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 81e37e78246a8c021cfdea015a9c4fe7695f5e44
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72933977"
---
# <a name="tutorial-azure-active-directory-integration-with-openathens"></a>Öğretici: Openatina ile tümleştirme Azure Active Directory

Bu öğreticide, Openatina 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Openatina ile Azure AD arasında tümleştirme aşağıdaki avantajları sağlar:

* Openatina erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Openatina (çoklu oturum açma) ile otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="prerequisites"></a>Önkoşullar

Openatina ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* Openatina çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Openatina, **IDP** tarafından başlatılan SSO 'yu destekler

* Openatina **, tam zamanında** Kullanıcı sağlamayı destekler

## <a name="adding-openathens-from-the-gallery"></a>Galeriden Openatina ekleme

Openatina 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden Openatina 'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Openatina eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **openatina**yazın, sonuç panelinden **openatina** ' yı seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![Sonuç listesinde Openatina](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre openatina ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Openatina 'daki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

Openatina ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[Openatina çoklu oturum açmayı yapılandırma](#configure-openathens-single-sign-on)** uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Openatina, kullanıcının Azure AD gösterimine bağlı olan Openatina 'da Britta Simon 'a sahip olmak için **[openatina test kullanıcısı oluşturun](#create-openathens-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Openatina ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **openatina** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

5. **Temel SAML yapılandırması** bölümünde, bu öğreticide daha sonra bahsedilen adımlar Için **hizmet sağlayıcı meta veri dosyasını**karşıya yükleyin.

    a. **Meta veri dosyasını karşıya yükle**' ye tıklayın.

    ![openatina karşıya yükleme meta verileri](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

    ![Openatina tarayıcı karşıya yükleme meta verileri](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra, **temel SAML yapılandırması** bölümünde otomatik olarak doldurulmuş **tanımlayıcı** değeri Al metin kutusu:

    ![Openatina etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-identifier.png)

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

### <a name="configure-openathens-single-sign-on"></a>Openatina çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Openatina şirket sitenizde yönetici olarak oturum açın.

2. **Yönetim** sekmesi altındaki listeden **Bağlantılar** ' ı seçin. 

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application1.png)

3. **SAML 1.1/2.0**' ı seçin ve ardından **Yapılandır** düğmesini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application2.png)
    
4. Yapılandırmayı eklemek için, Azure portal indirdiğiniz Metadata. xml dosyasını karşıya yüklemek için, **Gözden** geçirme düğmesini seçin ve ardından **Ekle**' yi seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application3.png)

5. **Ayrıntılar** sekmesi altında aşağıdaki adımları gerçekleştirin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application4.png)

    a. **Görünen ad eşlemesi**' nde **özniteliği kullan**' ı seçin.

    b. **Görünen ad özniteliği** metin kutusunda `http://schemas.microsoft.com/identity/claims/displayname`değeri girin.
    
    c. **Benzersiz kullanıcı eşlemesi**' nde **özniteliği kullan**' ı seçin.

    d. **Benzersiz kullanıcı özniteliği** metin kutusuna `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name`değeri girin.

    e. **Durum**' da, tüm üç onay kutusunu seçin.

    f. **Yerel hesap oluştur**' da **otomatik olarak**' ı seçin.

    g. **Değişiklikleri Kaydet**' i seçin.

    h. **</> bağlı olan taraf** sekmesinden, **SP meta veri XML** dosyasını indirmek için **meta veri URL 'sini** kopyalayın ve bunu tarayıcıda açın. Bu SP meta veri dosyasını Azure AD 'deki **temel SAML yapılandırması** bölümüne yükleyin.

    ![Çoklu oturum açmayı yapılandırma](./media/openathens-tutorial/tutorial_openathens_application5.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanında **brittasıon\@yourşirketnotlarıetki alanı. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Openatina 'a erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve **openatina**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, yazın ve **Openatina**' yı seçin.

    ![Uygulamalar listesindeki Openatina bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-openathens-test-user"></a>Openatina test kullanıcısı oluşturma

Bu bölümde, Openatina 'da Britta Simon adlı bir Kullanıcı oluşturulur. Openatina, varsayılan olarak etkinleştirilen **tam zamanında Kullanıcı sağlamayı**destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Openatina 'da bir kullanıcı zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Openatina kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Openatina 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

