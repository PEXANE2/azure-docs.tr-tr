---
title: 'Öğretici: Xönemli OnDemand ile tümleştirme Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Xönemli OnDemand arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ca0633db-4f95-432e-b3db-0168193b5ce9
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: e8ae31122d59238ac104d7d873cf56f32977c9af
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67086512"
---
# <a name="tutorial-azure-active-directory-integration-with-xmatters-ondemand"></a>Öğretici: Xönemli OnDemand ile tümleştirme Azure Active Directory

Bu öğreticide, Xönemli OnDemand Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Azure AD ile Xönemli OnDemand tümleştirme aşağıdaki avantajları sağlar:

* Azure AD 'de, Xönemli OnDemand erişimi olan bir denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak bir Xönemli OnDemand (çoklu oturum açma) ile oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Xönemli OnDemand ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Xönemli OnDemand çoklu oturum açma etkin aboneliği

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Xönemli OnDemand **IDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-xmatters-ondemand-from-the-gallery"></a>Galeriden Xönemli OnDemand ekleme

Azure AD 'de Xönemli OnDemand tümleştirmesini yapılandırmak için, Galeriden yönetilen SaaS uygulamaları listenize Xönemli OnDemand eklemeniz gerekir.

**Galeriden Xönemli OnDemand eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Xönemli OnDemand**yazın, sonuç panelinden **xönemli OnDemand** ' i seçin ve ardından **Ekle** düğmesine tıklayarak uygulamayı ekleyin.

     ![sonuçlar listesinde Xönemli OnDemand](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, **Britta Simon**adlı bir test kullanıcısına göre Xönemli OnDemand ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ile ilgili Kullanıcı arasındaki bir bağlantı ilişkisinin Xönemli OnDemand ' de kurulması gerekir.

Azure AD çoklu oturum açma 'yı Xönemli OnDemand ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Xönemli OnDemand çoklu oturum açmayı yapılandırın](#configure-xmatters-ondemand-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. Kullanıcının Azure AD gösterimine bağlı olan Xönemli bir OnDemand ' de Britta Simon 'ın bir karşılığı olacak şekilde **[xönemli bir test kullanıcısı oluşturun](#create-xmatters-ondemand-test-user)** .
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı Xönemli OnDemand olarak yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **xönemli OnDemand** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, aşağıdaki adımları uygulayın:

    ![Xönemli OnDemand etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au/`|
    | `https://<companyname>.cs1.xmatters.com/`|
    | `https://<companyname>.xmatters.com/`|
    | `https://www.xmatters.com`|
    | `https://<companyname>.xmatters.com.au/`|
    | |

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.au1.xmatters.com.au`|
    | `https://<companyname>.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.cs1.xmatters.com/sp/<instancename>`|
    | `https://<companyname>.au1.xmatters.com.au/<instancename>`|
    | |

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için, [Xönemli OnDemand istemci destek ekibine](https://www.xmatters.com/company/contact-us/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

    > [!IMPORTANT]
    > Sertifikayı [Xönemli ondemandsupport ekibine](https://www.xmatters.com/company/contact-us/)iletmeniz gerekir. Çoklu oturum açma yapılandırmasını sonuçlayabilmeniz için önce sertifikanın, Xönemli destek ekibi tarafından yüklenmesi gerekir.

6. **Xönemli OnDemand ayarla** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-xmatters-ondemand-single-sign-on"></a>Xönemli OnDemand çoklu oturum açmayı yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, bir yönetici olarak Xönemli OnDemand şirket sitenizde oturum açın.

2. Üstteki araç çubuğunda, **yönetici**' ye tıklayın ve ardından sol taraftaki Gezinti çubuğunda **Şirket ayrıntıları** ' na tıklayın.

    ![Yönetici](./media/xmatters-ondemand-tutorial/IC776795.png "Yönetici")

3. **SAML yapılandırması** sayfasında, aşağıdaki adımları gerçekleştirin:

    ![SAML yapılandırması](./media/xmatters-ondemand-tutorial/IC776796.png "SAML yapılandırması")

    a. **SAML etkinleştir**' i seçin.

    b. **Kimlik sağlayıcısı kimliği** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini yapıştırın.

    c. **Çoklu oturum açma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini yapıştırın.

    d. **Çoklu oturum kapatma URL 'si** metin kutusunda, Azure Portal kopyaladığınız **Logout URL 'sini**yapıştırın.

    e. Şirket ayrıntıları sayfasında, üstteki **Değişiklikleri Kaydet**' e tıklayın.

    ![Şirket bilgileri](./media/xmatters-ondemand-tutorial/IC776797.png "Şirket bilgileri")

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

Bu bölümde, Xönemli OnDemand erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **xönemli OnDemand**' i seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Xönemli OnDemand**öğesini seçin.

    ![Uygulamalar listesindeki Xönemli OnDemand bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-xmatters-ondemand-test-user"></a>Xönemli OnDemand test kullanıcısı oluşturma

Bu bölümün amacı, Xönemli OnDemand ' de Britta Simon adlı bir Kullanıcı oluşturmaktır.

**Kullanıcı el ile oluşturmanız gerekiyorsa aşağıdaki adımları gerçekleştirin:**

1. **Xönemli OnDemand** kiracınızda oturum açın.

2. **Kullanıcılar** sekmesi ' ne ve ardından **Kullanıcı Ekle**' ye tıklayın.

    ![Kullanıcılar](./media/xmatters-ondemand-tutorial/IC781048.png "Kullanıcılar")

3. **Kullanıcı Ekle** bölümünde aşağıdaki adımları uygulayın:

    ![Kullanıcı ekleme](./media/xmatters-ondemand-tutorial/IC781049.png "Kullanıcı ekleme")

    a. **Etkin**' i seçin.

    b. **Kullanıcı kimliği** metin kutusuna, gibi Brittasimon@contoso.comkullanıcının kullanıcı kimliğini yazın.

    c. **Ilk ad** metin kutusuna, Britta gibi kullanıcının adını yazın.

    d. **Soyadı** metin kutusunda, Simon adlı kullanıcının soyadını yazın.

    e. **Site** metin kutusuna, sağlamak istediğiniz geçerli BIR Azure AD hesabının geçerli sitesini girin.

    f. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Xönemli OnDemand kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Xönemli OnDemand ' de otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

