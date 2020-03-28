---
title: 'Öğretici: Ziflow ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Ziflow arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 84e60fa4-36fb-49c4-a642-95538c78f926
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: d9745bdb1cb6de86a96946564865958433d49732
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67086203"
---
# <a name="tutorial-azure-active-directory-integration-with-ziflow"></a>Öğretici: Ziflow ile Azure Active Directory entegrasyonu

Bu eğitimde, Ziflow'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Ziflow'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Ziflow erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak Ziflow'da (Tek Oturum Açma) oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Ziflow ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Ziflow tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Ziflow **SP** başlatılan SSO destekler

## <a name="adding-ziflow-from-the-gallery"></a>Galeriden Ziflow ekleme

Ziflow'un Azure AD'ye entegrasyonunu yapılandırmak için galeriden Ziflow'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Ziflow eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Ziflow**yazın, sonuç panelinden **Ziflow'u** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde ziflow](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Ziflow ile yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, bir Azure AD kullanıcısı ile Ziflow'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Ziflow ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Ziflow Tek Oturum Açma'yı yapılandırın.](#configure-ziflow-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Ziflow test kullanıcıoluşturun](#create-ziflow-test-user)** - Kullanıcının Azure AD gösterimine bağlı Ziflow'daki Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturumaçmayı Ziflow ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Ziflow** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Ziflow Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://ziflow-production.auth0.com/login/callback?connection=<UniqueID>`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`urn:auth0:ziflow-production:<UniqueID>`

    > [!NOTE]
    > Önceki değerler gerçek değildir. Tanımlayıcı'daki benzersiz kimlik değerini güncelleştirin ve URL'de Imzala gerçek değerle güncellenirsiniz ve bu değeri daha sonra öğreticide açıklarsınız.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Ziflow'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-ziflow-single-sign-on"></a>Ziflow Tek İşaret-On yapıla

1. Farklı bir web tarayıcısı penceresinde, Güvenlik Yöneticisi olarak Ziflow'da oturum açın.

2. Sağ üst köşedeki Avatar'a tıklayın ve ardından **hesabı yönet'e**tıklayın.

    ![Ziflow Yapılandırma Yönet](./media/ziflow-tutorial/tutorial_ziflow_manage.png)

3. Sol üstte, **Tek Oturum Açma'yı**tıklatın.

    ![Ziflow Yapılandırma İşareti](./media/ziflow-tutorial/tutorial_ziflow_signon.png)

4. Tek **Oturum Açma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Ziflow Yapılandırma Tek](./media/ziflow-tutorial/tutorial_ziflow_page.png)

    a. **SAML2.0**olarak **Yazın'ı** seçin.

    b. Oturum **Aç URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. Azure portalından indirdiğiniz base-64 kodlu sertifikayı **X509 İmza Sertifikası'na**yükleyin.

    d. Oturumu **Çıkış URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    e. Tanımlayıcı **Sağlayıcınız bölümünüz için Yapılandırma Ayarlarından,** vurgulanan benzersiz kimlik değerini kopyalayın ve Azure portalındaki **Temel SAML** Yapılandırması'nda URL'de Tanımlayıcı ve Url'de Oturum Aç ile tamamlayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı brittasimon@yourcompanydomain.extensiontüründe. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Ziflow'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Ziflow'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Ziflow'u**seçin.

    ![Uygulamalar listesindeki Ziflow bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-ziflow-test-user"></a>Ziflow test kullanıcısı oluşturma

Azure AD kullanıcılarının Ziflow'da oturum açabilmeleri için Ziflow'da oturum açmaları gerekir. Ziflow'da, sağlama el ile bir görevdir.

Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:

1. Güvenlik Yöneticisi olarak Ziflow'da oturum açın.

2. Üstteki **Kişiler'e** gidin.

    ![Ziflow Yapılandırma kişileri](./media/ziflow-tutorial/tutorial_ziflow_people.png)

3. **Ekle'yi** tıklatın ve ardından **Kullanıcı Ekle'yi**tıklatın.

    ![Ziflow Yapılandırma kullanıcı ekleme](./media/ziflow-tutorial/tutorial_ziflow_add.png)

4. **Kullanıcı** açılır pencereekle'de aşağıdaki adımları gerçekleştirin:

    ![Ziflow Yapılandırma kullanıcı ekleme](./media/ziflow-tutorial/tutorial_ziflow_adduser.png)

    a. **E-posta** metin kutusuna, gibi brittasimon@contoso.comkullanıcının e-posta girin.

    b. **Ad metin** kutusuna, Britta gibi kullanıcının ilk adını girin.

    c. **Soyadı** metin kutusuna, Simon gibi kullanıcının soyadını girin.

    d. Ziflow rolünüzü seçin.

    e. **1 kullanıcı ekle'yi**tıklatın.

    > [!NOTE]
    > Azure Etkin Dizin hesabı sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantı izler.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Ziflow döşemesini tıklattığınızda, SSO'yu kurduğunuz Ziflow'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

