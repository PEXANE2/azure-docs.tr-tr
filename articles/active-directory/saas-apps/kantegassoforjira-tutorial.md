---
title: "Öğretici: JIRA için Kantega SSO 'SU ile Azure Active Directory tümleştirme | Microsoft Docs"
description: JIRA için Azure Active Directory ve Kantega SSO arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e2af4891-e3c8-43b3-bdcb-0500c493e9b4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e97a66a1b21b46649a1206eb522c180e84a0a0d4
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67099165"
---
# <a name="tutorial-azure-active-directory-integration-with-kantega-sso-for-jira"></a>Öğretici: JIRA için Kantega SSO ile tümleştirme Azure Active Directory

Bu öğreticide, JIRA için Azure Active Directory (Azure AD) ile Kantega SSO 'yu nasıl tümleştirileceğini öğreneceksiniz.
JIRA için Kantega SSO 'SU Azure AD ile tümleştirmek aşağıdaki avantajları sağlar:

* JIRA için Kantega SSO 'ya erişimi olan Azure AD 'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla JIRA (çoklu oturum açma) için Kantega SSO 'ya otomatik olarak oturum açmasını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz-Azure portal.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek istiyorsanız, bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/) .

## <a name="prerequisites"></a>Ön koşullar

JIRA için Kantega SSO 'SU ile Azure AD tümleştirmesini yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Bir Azure AD aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* JIRA çoklu oturum açma özellikli abonelik için Kantega SSO

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD çoklu oturum açmayı bir test ortamında yapılandırıp test edersiniz.

* JIRA için Kantega SSO **, SP ve ıDP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-kantega-sso-for-jira-from-the-gallery"></a>Galeriden JIRA için Kantega SSO 'SU ekleme

JIRA için Kantega SSO 'yu Azure AD 'ye tümleştirmeyi yapılandırmak için, galerideki JIRA için Kantega SSO 'SU ile yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

**Galeriden JIRA için Kantega SSO eklemek için aşağıdaki adımları uygulayın:**

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

    ![Azure Active Directory düğmesi](common/select-azuread.png)

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusuna **Jira Için KANTEGA SSO**yazın, bir sonuç panelinden **Jira Için Kantega SSO** 'yu seçin, sonra uygulamayı eklemek için **Ekle** düğmesine tıklayın.

    ![Sonuç listesinde JIRA için Kantega SSO](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma ve test etme

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre JIRA Için Kantega IÇIN Azure AD çoklu oturum açmayı yapılandırıp test edersiniz.
Çoklu oturum açma için, bir Azure AD kullanıcısı ve JIRA için Kantega SSO 'SU içindeki ilgili Kullanıcı arasındaki bağlantı ilişkisinin oluşturulması gerekir.

JIRA için Kantega SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
2. **[JIRA çoklu oturum açma Için Kantega SSO](#configure-kantega-sso-for-jira-single-sign-on)** 'yu, uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -Britta Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
4. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanarak Britta Simon 'u etkinleştirin.
5. **[Jira test kullanıcısı Için Kantega SSO oluşturma](#create-kantega-sso-for-jira-test-user)** -kullanıcının Azure AD gösterimine bağlanmış JIRA Için KANTEGA SSO 'Su Içinde Britta Simon 'un bir karşılığı.
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[Çoklu oturum açmayı sınayın](#test-single-sign-on)** .

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

JIRA için Kantega SSO 'SU ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Jira uygulama tümleştirmesi Için KANTEGA SSO** sayfasında, **Çoklu oturum açma**' yı seçin.

    ![Çoklu oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML/WS-Besme** modunu seçin.

    ![Çoklu oturum açma seçme modu](common/select-saml-option.png)

3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

    ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

4. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız aşağıdaki adımları uygulayın:

    ![JIRA etki alanı ve URL 'Ler çoklu oturum açma bilgileri için Kantega SSO](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

5. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    ![JIRA etki alanı ve URL 'Ler çoklu oturum açma bilgileri için Kantega SSO](common/metadata-upload-additional-signon.png)

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<server-base-url>/plugins/servlet/no.kantega.saml/sp/<uniqueid>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI ile güncelleştirin. Bu değerler, Öğreticinin ilerleyen kısımlarında açıklanan Jira eklentisinin yapılandırması sırasında alınır.

6. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imza sertifikası** bölümünde, **Federasyon meta veri XML** 'sini gereksiniminize göre belirtilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

7. **JIRA için up Tega SSO 'Yu ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD tanımlayıcısı

    c. Oturum kapatma URL 'SI

### <a name="configure-kantega-sso-for-jira-single-sign-on"></a>JIRA çoklu oturum açma için Kantega SSO 'yu yapılandırma

1. Farklı bir Web tarayıcısı penceresinde, JIRA şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli üzerine gelin ve **eklentilere**tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon1.png)

1. Eklentiler sekmesi bölümünde **yeni eklentiler bul**' a tıklayın. **JIRA (saml & Kerberos) Için Kantega SSO** 'yu arayın ve yeni SAML **eklentisini yüklemek için Kaldır düğmesine tıklayın** .

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon2.png)

1. Eklenti yüklemesi başlar.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon3.png)

1. Yükleme tamamlandıktan sonra. **Kapat**' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon33.png)

1.  **Yönet**'e tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon34.png)
    
1. Yeni eklenti **tümleştirmeler**altında listelenmiştir. Yeni eklentiyi yapılandırmak için **Yapılandır** ' a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon35.png)

1. **SAML** bölümünde. **Kimlik sağlayıcısı ekle** açılır listesinden **Azure ACTIVE DIRECTORY (Azure AD)** öğesini seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon4.png)

1. Abonelik düzeyini **temel**olarak seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon5.png)       

1. **Uygulama özellikleri** bölümünde aşağıdaki adımları gerçekleştirin: 

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon6.png)

    a. **Uygulama KIMLIĞI URI** değerini kopyalayın ve Azure Portal IÇINDEKI **temel SAML yapılandırması** bölümünde **tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'si** olarak kullanın.

    b. **İleri**’ye tıklayın.

1. **Meta veri içeri aktarma** bölümünde aşağıdaki adımları gerçekleştirin: 

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon7.png)

    a. Bilgisayarımdaki **meta veri dosyasını**seçin ve Azure Portal 'ten indirdiğiniz meta veri dosyasını karşıya yükleyin.

    b. **İleri**’ye tıklayın.

1. **Ad ve SSO konumu** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon8.png)

    a. Kimlik sağlayıcısı **adı** metin kutusuna kimlik sağlayıcısının adını ekleyin (ör. Azure AD).

    b. **İleri**’ye tıklayın.

1. Imzalama sertifikasını doğrulayın ve **İleri**' ye tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon9.png)

1. **Jira Kullanıcı hesapları** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon10.png)

    a. **Gerekirse JIRA 'nın Iç dizininde kullanıcı oluştur** ' u seçin ve Kullanıcı için grubun uygun adını girin (birden çok No olabilir. virgülle ayrılmış gruplar).

    b. **İleri**’ye tıklayın.

1. **Son**'a tıklayın.

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon11.png)

1. **Azure AD Için bilinen etki alanları** bölümünde aşağıdaki adımları uygulayın:

    ![Çoklu oturum açmayı yapılandırma](./media/kantegassoforjira-tutorial/addon12.png)

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
  
    b. **Kullanıcı adı** alanına yazın `brittasimon@yourcompanydomain.extension`. Örneğin, BrittaSimon@contoso.com

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, JIRA için Kantega SSO 'SU erişimi vererek Azure çoklu oturum açma özelliğini kullanmak için Britta Simon 'u etkinleştirin.

1. Azure portal **Kurumsal uygulamalar**' ı seçin, **tüm uygulamalar**' ı SEÇIN ve ardından **Jira için Kantega SSO**' yı seçin.

    ![Kurumsal uygulamalar dikey penceresi](common/enterprise-applications.png)

2. Uygulamalar listesinde **JIRA Için Kantega SSO**' yı seçin.

    ![Uygulamalar listesindeki JIRA bağlantısı için Kantega SSO 'SU](common/all-applications.png)

3. Soldaki menüde **Kullanıcılar ve gruplar**' ı seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. **Kullanıcı Ekle** düğmesine tıklayın, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. **Kullanıcılar ve gruplar** Iletişim kutusunda kullanıcılar listesinde **Britta Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

6. SAML onaylama işlemi içinde herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.

7. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

### <a name="create-kantega-sso-for-jira-test-user"></a>JIRA test kullanıcısı için Kantega SSO oluştur

Azure AD kullanıcılarının JIRA 'da oturum açmasını sağlamak için, JIRA ' da sağlanması gerekir. JIRA için Kantega SSO 'da, sağlama el ile gerçekleştirilen bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. JIRA şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli 'ye gelin ve **Kullanıcı yönetimine**tıklayın.

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user1.png) 

1. **Kullanıcı yönetimi** sekmesi bölümünde **Kullanıcı oluştur**' a tıklayın.

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user2.png) 

1. **"Yeni Kullanıcı Oluştur"** iletişim sayfasında, aşağıdaki adımları uygulayın:

    ![Çalışan Ekle](./media/kantegassoforjira-tutorial/user3.png) 

    a. **E-posta adresi** metin kutusuna, gibi Brittasimon@contoso.comkullanıcının e-posta adresini yazın.

    b. **Tam ad** metin kutusuna, Britta Simon gibi kullanıcının tam adını yazın.

    c. Kullanıcı **adı** metin kutusuna, gibi Brittasimon@contoso.comkullanıcının e-postasını yazın.

    d. **Parola** metin kutusuna kullanıcının parolasını yazın.

    e. **Kullanıcı oluştur**' a tıklayın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde JIRA kutucuğunun Kantega SSO 'SU ' ne tıkladığınızda, SSO 'yu ayarladığınız JIRA için Kantega SSO 'ya otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
