---
title: 'Öğretici: VERITAS Enterprise kasası ile tümleştirme Azure Active Directory. Cloud SSO | Microsoft Docs'
description: Azure Active Directory ve VERITAS Enterprise Kasası arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin. Cloud SSO.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/28/2019
ms.author: jeedes
ms.openlocfilehash: cdb3f30813d2650737cfa43507cef2b1d456573d
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88532549"
---
# <a name="tutorial-azure-active-directory-integration-with-veritas-enterprise-vaultcloud-sso"></a>Öğretici: VERITAS Enterprise kasası ile tümleştirme Azure Active Directory. Cloud SSO

Bu öğreticide, VERITAS Enterprise kasasını tümleştirme hakkında bilgi edinebilirsiniz. Cloud SSO 'yu Azure Active Directory (Azure AD) ile.
VERITAS Enterprise kasasını tümleştirme. Azure AD ile bulut SSO 'SU aşağıdaki avantajları sağlar:

* Azure AD 'de, VERITAS Enterprise Kasası 'na erişimi olan bir denetim yapabilirsiniz. Cloud SSO.
* Kullanıcılarınızın Azure AD hesaplarıyla, Veritas Enterprise kasasında otomatik olarak oturum açmasını sağlayabilirsiniz. Cloud SSO (çoklu oturum açma).
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini VERITAS Enterprise Kasası. Cloud SSO ile yapılandırmak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* VERITAS Enterprise Kasası. Cloud SSO çoklu oturum açma etkin abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* VERITAS Enterprise Kasası. Cloud SSO, **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-veritas-enterprise-vaultcloud-sso-from-the-gallery"></a>Galerideki VERITAS Enterprise Kasası. Cloud SSO 'SU ekleme

VERITAS Enterprise Kasası. Cloud SSO 'yu Azure AD 'ye tümleştirmeyi yapılandırmak için, galerideki VERITAS Enterprise Kasası. Cloud SSO ' yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galerideki VERITAS Enterprise Kasası. Cloud SSO 'SU eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **VERITAS Enterprise Kasası. Cloud SSO**yazın, **VERITAS Enterprise Kasası** ' nı seçin. sonuç panelinden Cloud SSO 'yu, sonra da uygulamayı eklemek için **Ekle** düğmesine tıklayın.

     ![VERITAS Enterprise Kasası. Cloud SSO, sonuçlar listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, Azure AD çoklu oturum açmayı, Veritas, **Simon**adlı bir test kullanıcısına bağlı olarak VERITAS Enterprise Kasası. Cloud SSO ile yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve VERITAS Enterprise kasasındaki ilgili Kullanıcı arasındaki bağlantı ilişkisi. Cloud SSO 'nun kurulması gerekir.

Azure AD çoklu oturum açma 'yı VERITAS Enterprise kasası ile yapılandırmak ve test etmek için, aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[VERITAS Enterprise kasasını yapılandırma. Cloud SSO çoklu oturum açma](#configure-veritas-enterprise-vaultcloud-sso-single-sign-on)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[VERITAS Enterprise Kasası oluşturun. Cloud SSO test kullanıcısı](#create-veritas-enterprise-vaultcloud-sso-test-user)** -VERITAS Enterprise kasasında bir Britta Simon, kullanıcının Azure AD gösterimine bağlı olan bulut SSO 'su.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Azure AD çoklu oturum açmayı VERITAS Enterprise Kasası. Cloud SSO ile yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **VERITAS Enterprise KASASı. Cloud SSO** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![VERITAS Enterprise Kasası. Cloud SSO etki alanı ve URL 'Ler çoklu oturum açma bilgileri](common/sp-identifier-reply.png)

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://personal.ap.archive.veritas.com/CID=<CUSTOMERID>`

    b. **Tanımlayıcı** kutusunda, veri merkezine göre URL 'yi kullanın:

    | Veri merkezi| URL |
    |----------|----|
    | Kuzey Amerika| `https://auth.lax.archivecloud.net` |
    | Avrupa | `https://auth.ams.archivecloud.net` |
    | Asya Pasifik| `https://auth.syd.archivecloud.net`|

    c. **Yanıt URL** 'si metin kutusunda, veri merkezine göre URL 'yi kullanın:

    | Veri merkezi| URL |
    |----------|----|
    | Kuzey Amerika| `https://auth.lax.archivecloud.net` |
    | Avrupa | `https://auth.ams.archivecloud.net` |
    | Asya Pasifik| `https://auth.syd.archivecloud.net`|

    > [!NOTE]
    > Bu değer gerçek değil. Bu değeri, gerçek oturum açma URL 'siyle güncelleştirin. Bu değeri almak için [VERITAS Enterprise Kasası. Cloud SSO istemci desteği ekibine](https://www.veritas.com/support/.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **VERITAS Enterprise Kasası ayarlama. Cloud SSO** bölümünde, uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-veritas-enterprise-vaultcloud-sso-single-sign-on"></a>VERITAS Enterprise kasasını yapılandırma. Cloud SSO çoklu oturum açma

**VERITAS Enterprise kasasında** çoklu oturum açma 'yı yapılandırmak için. Cloud SSO tarafında, indirilen **sertifikayı (Base64)** ve Azure Portal ' den Veritas Enterprise Kasası 'Na uygun şekilde kopyalanmış URL 'leri GÖNDERMENIZ gerekir. [bulut SSO destek ekibi](https://www.veritas.com/support/.html). Bu ayar, SAML SSO bağlantısının her iki tarafında da düzgün bir şekilde ayarlanmasını sağlamak üzere ayarlanmıştır.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portal Britta Simon adlı bir test kullanıcısı oluşturmaktır.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

    !["Kullanıcılar ve gruplar" ve "tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.

    ![Yeni Kullanıcı düğmesi](common/new-user.png)

3. Kullanıcı Özellikleri ' nde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **Brittasıon**girin.
  
    b. **Kullanıcı adı** alanına yazın brittasimon@yourcompanydomain.extension . Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Veritas Enterprise Kasası 'na erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **VERıTAS Enterprise Kasası. Cloud SSO**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **VERITAS Enterprise Kasası. Cloud SSO**' yı seçin.

    ![Uygulamalar listesindeki VERITAS Enterprise Kasası. Cloud SSO bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-veritas-enterprise-vaultcloud-sso-test-user"></a>VERITAS Enterprise Kasası oluşturma. Cloud SSO test kullanıcısı

Bu bölümde, VERITAS Enterprise kasasında Britta Simon adlı bir Kullanıcı oluşturacaksınız. Cloud SSO. VERITAS Enterprise kasası ile çalışma [. Cloud SSO destek ekibi](https://www.veritas.com/support/.html) , kullanıcıları VERITAS Enterprise kasasında ekleme. Cloud SSO platformu. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde VERITAS Enterprise Kasası. Cloud SSO kutucuğuna tıkladığınızda otomatik olarak VERITAS Enterprise kasasında oturum açmanız gerekir. SSO 'yu ayarladığınız bulut SSO 'SU. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

