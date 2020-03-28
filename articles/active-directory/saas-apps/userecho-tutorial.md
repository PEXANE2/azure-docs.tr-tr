---
title: 'Öğretici: UserEcho ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve UserEcho arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/29/2019
ms.author: jeedes
ms.openlocfilehash: 59d61eda7002fe46cf99fac63822b2333b2d64b5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67087760"
---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Öğretici: UserEcho ile Azure Active Directory entegrasyonu

Bu eğitimde, UserEcho'yu Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
UserEcho'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* UserEcho erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla UserEcho'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini UserEcho ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* UserEcho tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* UserEcho **SP** başlatılan SSO destekler

## <a name="adding-userecho-from-the-gallery"></a>Galeriden UserEcho Ekleme

UserEcho'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize UserEcho eklemeniz gerekir.

**Galeriden UserEcho eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **UserEcho**yazın, sonuç panelinden **UserEcho'yu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![KullanıcıYankı sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre UserEcho ile yapılandırıp test esiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure REKLAM kullanıcısı ile UserEcho'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı UserEcho ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. UserEcho Tek Oturum Açma 'yı **[yapılandırın](#configure-userecho-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[UserEcho test kullanıcısını oluşturun](#create-userecho-test-user)** - UserEcho'da kullanıcının Azure AD gösterimine bağlı Britta Simon'ın bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini UserEcho ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **UserEcho** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![UserEcho Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.userecho.com/`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.userecho.com/saml/metadata/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [UserEcho Client destek ekibine](https://feedback.userecho.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **UserEcho'yu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-userecho-single-sign-on"></a>UserEcho Tek Oturum Açma'yı Yapılandır

1. Başka bir tarayıcı penceresinde, UserEcho şirket sitenizde yönetici olarak oturum açın.

2. Üstteki araç çubuğunda, menüyü genişletmek için kullanıcı adınızı tıklatın ve ardından **Kurulum'u**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_06.png) 

3. **Tümleştirmeler'i**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_07.png) 

4. **Web Sitesi'ni**tıklatın ve ardından Tek oturum **açma (SAML2)** seçeneğini tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_08.png) 

5. Tek **oturum açma (SAML)** sayfasında aşağıdaki adımları gerçekleştirin:
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_09.png)
    
    a. **SAML özellikli**olarak **Evet'i**seçin.
    
    b. Azure portalından KOPYALADIĞINIZ **Giriş URL'sini** **SAML SSO URL** textbox'a yapıştırın.
    
    c. Azure portalından kopyaladığınız **Logout URL'yi** **Uzak Oturum Açma URL** metin kutusuna yapıştırın.
    
    d. İndirilen sertifikanızı Not Defteri'nde açın, içeriği kopyalayın ve **ardından X.509 Sertifika** metin kutusuna yapıştırın.
    
    e. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın UserEcho'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **UserEcho'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **UserEcho'yu**seçin.

    ![Uygulamalar listesindeki UserEcho bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-userecho-test-user"></a>UserEcho test kullanıcısı oluşturma

Bu bölümün amacı UserEcho'da Britta Simon adında bir kullanıcı oluşturmaktır.

**UserEcho'da Britta Simon adında bir kullanıcı oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. UserEcho şirket sitenizde yönetici olarak oturum açın.

2. Üstteki araç çubuğunda, menüyü genişletmek için kullanıcı adınızı tıklatın ve ardından **Kurulum'u**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_06.png)

3. **Kullanıcılar** bölümünü genişletmek için **Kullanıcılar'ı**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_10.png)

4. **Kullanıcılar**’a tıklayın.
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_11.png)

5. **Yeni bir kullanıcı davet et'i**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_12.png)

6. Yeni bir kullanıcı iletişim kutusunu **davet et,** aşağıdaki adımları gerçekleştirin:
   
    ![Tek İşaret-On'u Yapılandır](./media/userecho-tutorial/tutorial_userecho_13.png)

    a. **Ad** metin kutusunda, Britta Simon gibi kullanıcının adını yazın.
    
    b.  **E-posta** metin kutusuna, kullanıcının Brittasimon@contoso.come-posta adresini yazın.
    
    c. **Davet Et'i**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki UserEcho döşemesini tıklattığınızda, SSO'yu kurduğunuz UserEcho'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

