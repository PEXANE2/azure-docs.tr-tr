---
title: 'Öğretici: ON24 sanal ortamıyla Azure Active Directory tümleştirme SAML bağlantısı | Microsoft Docs'
description: Azure Active Directory ve ON24 sanal ortam SAML bağlantısı arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d4028fb5-b2ad-4c5d-b123-7b675c509d64
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/13/2019
ms.author: jeedes
ms.openlocfilehash: 801a631b56a11e68c444ede846ff82195cd7627f
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67095720"
---
# <a name="tutorial-azure-active-directory-integration-with-on24-virtual-environment-saml-connection"></a>Öğretici: ON24 sanal ortamı SAML bağlantısı ile Azure Active Directory tümleştirme

Bu öğreticide, ON24 sanal ortam SAML bağlantısını Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
ON24 sanal ortamı SAML bağlantısını Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* ON24 sanal ortam SAML bağlantısına erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla ON24 sanal ortam SAML bağlantısına (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini ON24 sanal ortam SAML bağlantısıyla yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü edinebilirsiniz
* ON24 sanal ortam SAML bağlantısı çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* ON24 sanal ortamı SAML bağlantısı **SP** ve **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-on24-virtual-environment-saml-connection-from-the-gallery"></a>Galeriden ON24 sanal ortam SAML bağlantısı ekleniyor

ON24 sanal ortam SAML bağlantısı 'nın Azure AD 'ye tümleştirilmesini yapılandırmak için, Galeriden ON24 sanal ortam SAML bağlantısı ' nı galerisinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden ON24 sanal ortam SAML bağlantısı eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **ON24 sanal ortam SAML bağlantısı**yazın, sonuç panelinden **ON24 sanal ortam SAML bağlantısı** ' nı seçin ve sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![ON24 sanal ortamı SAML Connection for Results List](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test KULLANıCıSıNA göre ON24 sanal ortamı SAML bağlantısı Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve ON24 sanal ortamındaki ilgili Kullanıcı ile ilgili bir bağlantı ilişkisi SAML bağlantısı kurulması gerekir.

Azure AD çoklu oturum açma 'yı ON24 sanal ortam SAML bağlantısıyla yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[ON24 sanal ortamını yapılandırın SAML bağlantısı çoklu oturum açma](#configure-on24-virtual-environment-saml-connection-single-sign-on)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[ON24 sanal ORTAMı SAML bağlantısı test kullanıcısı oluşturun](#create-on24-virtual-environment-saml-connection-test-user)** ; bu, kullanıcının Azure AD gösterimine bağlı olan ON24 sanal ortam SAML bağlantısı 'Nda Britta Simon 'a sahip olmalıdır.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı ON24 sanal ortam SAML bağlantısıyla yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **ON24 sanal ortamı SAML bağlantısı** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![ON24 sanal ortamı SAML bağlantısı etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-relay.png)

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:

     **Üretim ortamı URL 'SI**
    
    `SAML-VSHOW.on24.com`

    `SAML-Gateway.on24.com` 

    `SAP PROD SAML-EliteAudience.on24.com` 
                
     **QA ortam URL 'SI**
    
    `SAMLQA-VSHOW.on24.com` 

    `SAMLQA-Gateway.on24.com` 

    `SAMLQA-EliteAudience.on24.com`

    b. **Yanıt URL** 'si metin kutusuna bir URL yazın:

     **Üretim ortamı URL 'SI**
    
    `https://federation.on24.com/sp/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1WU2hvdy5vbjI0LmNvbSJ9/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1HYXRld2F5Lm9uMjQuY29tIn0/ACS.saml2`

    `https://federation.on24.com/sp/eyJ2c2lkIjoiU0FNTC1FbGl0ZUF1ZGllbmNlLm9uMjQuY29tIn0/ACS.saml2`

     **QA ortam URL 'SI**
    
    `https://qafederation.on24.com/sp/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLVZzaG93Lm9uMjQuY29tIn0/ACS.saml2`

    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUdhdGV3YXkub24yNC5jb20ifQ/ACS.saml2`
     
    `https://qafederation.on24.com/sp/eyJ2c2lkIjoiU0FNTFFBLUVsaXRlQXVkaWVuY2Uub24yNC5jb20ifQ/ACS.saml2` 

    c. **Ek URL 'Ler ayarla**' ya tıklayın. 

    d. **Geçiş durumu** metin kutusuna bir URL yazın:`https://vshow.on24.com/vshow/ms_azure_saml_test?r=<ID>`

5.  Uygulamayı **SP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımı uygulayın:

    ![ON24 sanal ortamı SAML bağlantısı etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/both-signonurl.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://vshow.on24.com/vshow/<INSTANCENAME>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek geçiş durumu ve oturum açma URL 'SI ile güncelleştirin. Bu değerleri almak için [ON24 sanal ortam SAML bağlantısı istemci destek ekibine](https://www.on24.com/contact-us/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **ON24 sanal ortamını Ayarla SAML bağlantısı** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-on24-virtual-environment-saml-connection-single-sign-on"></a>ON24 sanal ortamını yapılandırma SAML bağlantısı çoklu oturum açma

**ON24 sanal ORTAMıNDAKI SAML bağlantı** tarafında çoklu oturum açmayı yapılandırmak için, Indirilen **Federasyon meta verileri XML** 'Sini ve Azure Portal [sanal ortam SAML bağlantısı destek ekibine](https://www.on24.com/about-us/support/)uygun şekilde kopyalanmış URL 'leri göndermeniz gerekir. Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına **bricompansıon\@yourcompanydomain. Extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, ON24 sanal ortam SAML bağlantısına erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin, sonra **ON24 sanal ortam SAML bağlantısı**' nı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **ON24 sanal ortam SAML bağlantısı**' nı seçin.

    ![Uygulamalar listesindeki ON24 sanal ortam SAML bağlantı bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-on24-virtual-environment-saml-connection-test-user"></a>ON24 sanal ortamı oluşturma SAML bağlantısı test kullanıcısı

Bu bölümde, ON24 sanal ortamı SAML bağlantısı 'nda Britta Simon adlı bir Kullanıcı oluşturacaksınız. ON24 sanal ortam SAML bağlantı platformuna kullanıcıları eklemek için [ON24 sanal ORTAMı SAML bağlantısı destek ekibi](https://www.on24.com/about-us/support/) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde ON24 sanal ortam SAML bağlantısı kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız ON24 sanal ortam SAML bağlantısında otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

