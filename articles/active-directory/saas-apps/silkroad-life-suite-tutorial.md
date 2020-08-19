---
title: 'Öğretici: SilkRoad yaşam paketiyle Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve SilkRoad yaşam paketi arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.openlocfilehash: 49dd333454f0eb92f5fb0dddc40390ec1baa91c5
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88525603"
---
# <a name="tutorial-azure-active-directory-integration-with-silkroad-life-suite"></a>Öğretici: SilkRoad yaşam paketiyle Azure Active Directory tümleştirme

Bu öğreticide, SilkRoad yaşam paketini Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
SilkRoad yaşam paketini Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Azure AD 'de SilkRoad Life Suite 'e erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla SilkRoad yaşam döngüsü paketine (çoklu oturum açma) otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini SilkRoad yaşam paketiyle birlikte yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* SilkRoad Life Suite çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* SilkRoad Life Suite **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-silkroad-life-suite-from-the-gallery"></a>Galeriden SilkRoad yaşam paketi ekleme

SilkRoad Life Suite 'in Azure AD ile tümleştirilmesini yapılandırmak için, Galeriden silodan SilkRoad yaşam paketi ' ni yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden SilkRoad yaşam paketi eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **silkroad Life Suite**yazın, sonuç panelinden **silkroad yaşam paketi** ' ni seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

    ![Sonuç listesindeki SilkRoad yaşam döngüsü](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına bağlı olarak SilkRoad yaşam PAKETIYLE Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve SilkRoad yaşam döngüsü paketindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Azure AD çoklu oturum açma 'yı SilkRoad yaşam paketiyle yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[SilkRoad Life Suite çoklu oturum açma yapılandırma](#configure-silkroad-life-suite-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Silkroad yaşam paketi test kullanıcısı oluşturma](#create-silkroad-life-suite-test-user)** -SilkRoad yaşam paketinde kullanıcının Azure AD gösterimine bağlı Britta Simon 'a sahip olmak için.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı SilkRoad yaşam paketi ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **silkroad yaşam paketi** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız**varsa, aşağıdaki adımları uygulayın:

    > [!NOTE]
    > Bu öğreticide daha sonra açıklanacak **hizmet sağlayıcısı meta veri dosyasını** alacaksınız.

    a. **Meta veri dosyasını karşıya yükle**' ye tıklayın.

    ![image](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosu** ' na tıklayın ve **karşıya yükle**' ye tıklayın.

    ![image](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla karşıya yüklendikten sonra **tanımlayıcı** ve **yanıt URL** değerleri temel SAML yapılandırması bölümünde otomatik olarak doldurulur:

    ![image](common/sp-identifier-reply.png)

    > [!Note]
    > **Tanımlayıcı** ve **yanıt URL 'si** değerleri otomatik olarak almıyorsanız, değerleri gereksinimlerinize göre el ile girin.

    d. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.silkroad-eng.com/Authentication/`

5. **Temel SAML yapılandırması** bölümünde, **hizmet sağlayıcısı meta verileri dosyanız**yoksa, aşağıdaki adımları uygulayın:

    ![SilkRoad Life Suite etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier-reply.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<subdomain>.silkroad-eng.com/Authentication/`

    b. **Tanımlayıcı** kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

    - `https://<subdomain>.silkroad-eng.com/Authentication/SP`
    - `https://<subdomain>.silkroad.com/Authentication/SP`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    - `https://<subdomain>.silkroad-eng.com/Authentication/`
    - `https://<subdomain>.silkroad.com/Authentication/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [silkroad Life Suite istemci destek ekibine](https://www.silkroad.com/locations/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **SilkRoad yaşam paketi ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-silkroad-life-suite-single-sign-on"></a>SilkRoad yaşam paketi çoklu oturum açmayı yapılandırma

1. SilkRoad şirket sitenizde yönetici olarak oturum açın.

    > [!NOTE]
    > Microsoft Azure AD ile Federasyonu yapılandırmak için SilkRoad Life Suite kimlik doğrulama uygulamasına erişim sağlamak için lütfen SilkRoad support veya SilkRoad Services temsilcinizle iletişime geçin.

1. **Hizmet sağlayıcısına**gidin ve ardından **Federasyon ayrıntıları**' na tıklayın.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_06.png)

1. **Federasyon meta verilerini indir**' e tıklayın ve ardından meta veri dosyasını bilgisayarınıza kaydedin. Azure portal **temel SAML yapılandırması** bölümünde **hizmet sağlayıcı meta** verileri olarak indirilen Federasyon meta verilerini kullanın.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_07.png)

1. **Silkroad** uygulamanızda **kimlik doğrulama kaynakları**' na tıklayın.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_08.png) 

1. **Kimlik doğrulama kaynağı Ekle**' ye tıklayın.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_09.png)

1. **Kimlik doğrulama kaynağı Ekle** bölümünde aşağıdaki adımları uygulayın:

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_10.png)
  
    a. **Seçenek 2-meta veri dosyası**altında, Azure Portal indirilen meta veri dosyasını karşıya yüklemek Için, **Araştır** ' a tıklayın.
  
    b. **Dosya verilerini kullanarak kimlik sağlayıcısı oluştur**' a tıklayın.

1. **Kimlik doğrulama kaynakları** bölümünde, **Düzenle**' ye tıklayın.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_11.png)

1. **Kimlik doğrulama kaynağını Düzenle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_12.png)

    a. **Etkin**olarak **Evet**' i seçin.

    b. **EntityId** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcısının** değerini yapıştırın.

    c. **IDP Description** metin kutusunda, yapılandırmanız için bir açıklama yazın (örneğin: *Azure AD SSO*).

    d. **Meta veri dosyası** metin kutusuna, Azure Portal indirdiğiniz **meta veri** dosyasını karşıya yükleyin.
  
    e. **IDP Name** metin kutusunda, yapılandırmanıza özgü bir ad yazın (örneğin: *Azure SP*).
  
    f. **Oturum kapatma hizmeti URL 'si** metin kutusunda, Azure Portal kopyaladığınız **Logout URL 'si** değerini yapıştırın.

    örneğin: Oturum açma **hizmeti URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    h. **Kaydet**’e tıklayın.

1. Diğer tüm kimlik doğrulama kaynaklarını devre dışı bırakın.

    ![Azure AD çoklu oturum açma](./media/silkroad-life-suite-tutorial/tutorial_silkroad_13.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alan türü`brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, SilkRoad yaşam paketine erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon özelliğini etkinleştirirsiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **silkroad yaşam paketi**' ni seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **silkroad yaşam paketi**' ni seçin.

    ![Uygulamalar listesindeki SilkRoad Life Suite bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-silkroad-life-suite-test-user"></a>SilkRoad yaşam paketi test kullanıcısı oluşturma

Bu bölümde, SilkRoad yaşam paketinde Britta Simon adlı bir Kullanıcı oluşturacaksınız. Silkroad yaşam paketi platformunda kullanıcıları eklemek için [silkroad Life Suite istemci destek ekibi](https://www.silkroad.com/locations/) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim paneli 'nde SilkRoad yaşam paketi kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız SilkRoad yaşam paketinde otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
