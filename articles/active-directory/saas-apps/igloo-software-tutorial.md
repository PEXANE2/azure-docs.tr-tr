---
title: 'Öğretici: Igloo Software ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Igloo Software arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 2eb625c1-d3fc-4ae1-a304-6a6733a10e6e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/06/2019
ms.author: jeedes
ms.openlocfilehash: df1d70f895e2e0a81344cf2a4e8e2d9963c951fa
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67100590"
---
# <a name="tutorial-azure-active-directory-integration-with-igloo-software"></a>Öğretici: Igloo Software ile Azure Active Directory entegrasyonu

Bu eğitimde, Igloo Yazılımlarını Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Igloo Yazılımını Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Igloo Yazılımı'na erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Igloo Yazılımında (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Igloo Software ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Igloo Yazılım tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Igloo **Software, SP** tarafından başlatılan SSO'ya destek veriyor
* Igloo Software **Just In Time** kullanıcı sağlama destekler

## <a name="adding-igloo-software-from-the-gallery"></a>Galeriden Igloo Yazılımı Ekleme

Igloo Yazılımının Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Igloo Yazılımını eklemeniz gerekir.

**Galeriden Igloo Yazılımı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Igloo Yazılımı**yazın, sonuç panelinden **Igloo Yazılımı'nı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Igloo Yazılım](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı olarak Igloo Software ile Azure AD tek oturum açma işlemini yapılandırın ve test edersiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Igloo Software'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Igloo Software ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Igloo Software Single Sign-On](#configure-igloo-software-single-sign-on)** 'u yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Igloo Yazılım test kullanıcıoluşturun](#create-igloo-software-test-user)** - kullanıcının Azure AD gösterimi ile bağlantılı Igloo Software Britta Simon bir meslektaşı olması.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Igloo Software ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Igloo Software** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Igloo Yazılım Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.igloocommmunities.com`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.igloocommmunities.com/saml.digest`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<company name>.igloocommmunities.com/saml.digest`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [Igloo Software Client destek ekibine](https://www.igloosoftware.com/services/support) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Igloo Yazılımını Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-igloo-software-single-sign-on"></a>Igloo Yazılım Tek Oturum-On yapılandırma

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak Igloo Software şirket sitenizde oturum açın.

2. **Kontrol Paneline**git.

     ![Denetim Masası](./media/igloo-software-tutorial/ic799949.png "Denetim Masası")

3. **Üyelik** sekmesinde, **Oturum Açma Ayarları'nı**tıklatın.

    ![Ayarlar'da Oturum Aç](./media/igloo-software-tutorial/ic783968.png "Ayarlar'da Oturum Aç")

4. SAML Yapılandırma bölümünde, **SAML Kimlik Doğrulamasını Yapılandır'ı**tıklatın.

    ![SAML Yapılandırması](./media/igloo-software-tutorial/ic783969.png "SAML Yapılandırması")

5. Genel **Yapılandırma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Genel Yapılandırma](./media/igloo-software-tutorial/ic783970.png "Genel Yapılandırma")

    a. Bağlantı **Adı** metin kutusunda, yapılandırmanız için özel bir ad yazın.

    b. **IdP Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.

    c. **IdP Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    d. **Gİrİş Yanıtı ve Post** olarak HTTP Tipi **Niyaz'ı**seçin.

    e. Azure portalından indirilen not defterinde **base-64** kodlu sertifikanızı açın, içeriğini panonuza kopyalayın ve ardından **Herkese Açık Sertifika** metin kutusuna yapıştırın.

6. Yanıt **ve Kimlik Doğrulama Yapılandırmasında**aşağıdaki adımları gerçekleştirin:

    ![Yanıt ve Kimlik Doğrulama Yapılandırması](./media/igloo-software-tutorial/IC783971.png "Yanıt ve Kimlik Doğrulama Yapılandırması")
  
    a. **Kimlik Sağlayıcı**olarak Microsoft **ADFS'yi**seçin.

    b. **Tanımlayıcı Türü**olarak, **E-posta Adresi**seçin. 

    c. **E-posta Özniteliği** metin **kutusuna, e-posta adresini**yazın.

    d. Ad **Özniteliği** textbox'ına, **verilen adı**yazın.

    e. Soyadı **Öznitelik** **textbox,.**

7. Yapılandırmayı tamamlamak için aşağıdaki adımları gerçekleştirin:

    ![Oturum Aç'ta kullanıcı oluşturma](./media/igloo-software-tutorial/IC783972.png "Oturum Aç'ta kullanıcı oluşturma") 

    a. **Oturum Aç'ta Kullanıcı oluşturma**olarak, oturum **açken sitenizde yeni bir kullanıcı oluştur'u**seçin.

    b. **Ayarlar'da Oturum Aç'ta** **"Oturum Aç" ekranında SAML'i kullan düğmesini**seçin.

    c. **Kaydet**'e tıklayın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde**brittasimon@yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Igloo Yazılımına erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Igloo Yazılımı'nı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, Igloo Software'i**seçin.

    ![Uygulamalar listesindeki Igloo Yazılım bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-igloo-software-test-user"></a>Igloo Yazılım test kullanıcı oluşturma

Kullanıcı sağlamayı Igloo Software'e yapılandırmanız için bir eylem öğesi yoktur.  

Atanmış bir kullanıcı erişim panelini kullanarak Igloo Software'de oturum açmaya çalıştığında, Igloo Software kullanıcının var olup olmadığını denetler.  Henüz kullanılabilir bir kullanıcı hesabı yoksa, otomatik olarak Igloo Software tarafından oluşturulur.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Igloo Yazılım döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Igloo Yazılımı'nda oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)