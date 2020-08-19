---
title: 'Öğretici Azure Active Directory: Bamboo için Kantega SSO ile tümleştirme | Microsoft Docs'
description: Bamboo için Azure Active Directory ve Kantega SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: jeedes
ms.openlocfilehash: b8bf8ef1affc42027ef0bec149f8e7c9eb9b2b90
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88546964"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-bamboo"></a>Öğretici: Bamboo için Kantega SSO ile tümleştirme Azure Active Directory

Bu öğreticide, Bamboo için Kantega SSO 'yu Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz.
Bamboo için Kantega SSO 'yu Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* Bamboo için Kantega SSO 'ya erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Bamboo (çoklu oturum açma) için Kantega SSO 'ya otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).

## <a name="prerequisites"></a>Ön koşullar

Bamboo için Kantega SSO 'SU ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Bamboo çoklu oturum açma özellikli abonelik için Kantega SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* Bamboo için Kantega SSO **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-kantega-sso-for-bamboo-from-the-gallery"></a>Galeriden Bamboo için Kantega SSO 'SU ekleme

Bamboo için Kantega SSO 'nun Azure AD 'ye tümleştirilmesini yapılandırmak için, galerinin Bamboo için Kantega SSO 'SU ile yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden Bamboo için Kantega SSO 'SU eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Bamboo Için KANTEGA SSO**yazın, sonuç panelinden **Bamboo Için KANTEGA SSO** ' yı seçin, sonra da uygulamayı eklemek için düğme **Ekle** ' ye tıklayın.

    ![Sonuçlar listesinde Bamboo için Kantega SSO 'SU](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına bağlı olarak Bamboo Için Kantega SSO Ile Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve Bamboo için Kantega SSO 'SU içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Bamboo için Kantega SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[Bamboo Single Sign-on Için Kantega SSO 'Yu yapılandırın](#configure-kantega-sso-for-bamboo-single-sign-on)** .
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Bamboo test kullanıcısı Için Kantega SSO oluşturma](#create-kantega-sso-for-bamboo-test-user)** -kullanıcının Azure AD gösterimine bağlı olan Bamboo Için Kantega SSO 'Da Britta Simon 'un bir karşılığı olmalıdır.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Bamboo için Kantega SSO ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Bamboo Için KANTEGA SSO** uygulama tümleştirmesi sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![Bamboo etki alanı ve URL 'Ler için Kantega SSO, çoklu oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![Bamboo etki alanı ve URL 'Ler için Kantega SSO, çoklu oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerler, öğreticide daha sonra açıklanan Bamboo eklentisi yapılandırması sırasında alınır.

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **Bamboo için up Tega SSO 'Yu ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-kantega-sso-for-bamboo-single-sign-on"></a>Bamboo çoklu oturum açma için Kantega SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, Bamboo şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli üzerine gelin ve **eklentilere**tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon1.png)

1. Eklentiler sekmesi bölümünde **yeni eklentiler bul**' a tıklayın. **Bamboo Için Kantega SSO 'yu arayın (saml & Kerberos)** ve yeni SAML **eklentisini yüklemek için Kaldır düğmesine tıklayın** .

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon2.png)

1. Eklenti yüklemesi başlar.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon21.png)

1. Yükleme tamamlandıktan sonra. **Kapat**’a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon33.png)

1. **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon34.png)

1. Yeni eklentiyi yapılandırmak için **Yapılandır** ' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon3.png)

1. **SAML** bölümünde. **Kimlik sağlayıcısı ekle** açılır listesinden **Azure ACTIVE DIRECTORY (Azure AD)** öğesini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon4.png)

1. Abonelik düzeyini **temel**olarak seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon5.png)

1. **Uygulama özellikleri** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon6.png)

    a. **Uygulama KIMLIĞI URI** değerini kopyalayın ve Azure Portal IÇINDEKI **temel SAML yapılandırması** bölümünde **tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'si** olarak kullanın.

    b. **İleri**’ye tıklayın.

1. **Meta veri içeri aktarma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon7.png)

    a. Bilgisayarımdaki **meta veri dosyasını**seçin ve Azure Portal 'ten indirdiğiniz meta veri dosyasını karşıya yükleyin.

    b. **İleri**’ye tıklayın.

1. **Ad ve SSO konumu** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon8.png)

    a. Kimlik sağlayıcısı **adı** metin kutusuna kimlik sağlayıcısının adını ekleyin (ör. Azure AD).

    b. **İleri**’ye tıklayın.

1. Imzalama sertifikasını doğrulayın ve **İleri**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon9.png)

1. **Bamboo Kullanıcı hesapları** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon10.png)

    a. **Gerekirse, Bamboo 'in Iç dizininde kullanıcı oluştur** ' u seçin ve Kullanıcı için grubun uygun adını girin (birden çok No olabilir. virgülle ayrılmış gruplar).

    b. **İleri**’ye tıklayın.

1. **Son**'a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon11.png)

1. **Azure AD Için bilinen etki alanları** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforbamboo-tutorial/addon12.png)

    a. Sayfanın sol panelinden **bilinen etki alanları ' nı** seçin.

    b. **Bilinen etki alanları** metin kutusuna etki alanı adını girin.

    c. **Kaydet**’e tıklayın.

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

Bu bölümde, Bamboo için Kantega SSO 'ya erişim vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı seçin ve ardından **Bamboo için Kantega SSO**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde, **Bamboo Için Kantega SSO**' yı seçin.

    ![Uygulamalar listesindeki Bamboo için Kantega SSO bağlantısı](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-kantega-sso-for-bamboo-test-user"></a>Bamboo test kullanıcısı için Kantega SSO oluştur

Azure AD kullanıcılarının Bamboo 'de oturum açmasını sağlamak için, Bamboo ' a sağlanması gerekir. Bamboo için Kantega SSO olması durumunda, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Bamboo şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli 'ye gelin ve **Kullanıcı yönetimine**tıklayın.

    ![Çalışan Ekle](./media/kantegassoforbamboo-tutorial/user1.png)

1. **Kullanıcılar**’a tıklayın. **Kullanıcı Ekle** bölümünde aşağıdaki adımları uygulayın:

    ![Çalışan Ekle](./media/kantegassoforbamboo-tutorial/user2.png)

    a. Kullanıcı **adı** metin kutusuna, gibi kullanıcının e-postasını yazın Brittasimon@contoso.com .

    b. **Parola** metin kutusuna kullanıcının parolasını yazın.

    c. **Parolayı Onayla** metin kutusuna kullanıcı parolasını yeniden girin.

    d. **Tam ad** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    e. **E-posta** metin kutusuna, gibi kullanıcının e-posta adresini yazın Brittasimon@contoso.com .

    f. **Kaydet**’e tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Bamboo for the Kantega SSO kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Bamboo için Kantega SSO 'SU içinde otomatik olarak oturum açmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
