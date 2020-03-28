---
title: 'Öğretici: ClickUp Üretkenlik Platformu ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve ClickUp Üretkenlik Platformu arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 06853edd-e8da-4ad2-a4e6-5555d592cee5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: f497955b698d7ca390b40686e94d553a7cd5c948
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048730"
---
# <a name="tutorial-azure-active-directory-integration-with-clickup-productivity-platform"></a>Öğretici: ClickUp Üretkenlik Platformu ile Azure Active Directory entegrasyonu

Bu eğitimde, ClickUp Üretkenlik Platformu'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
ClickUp Üretkenlik Platformu'nu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* ClickUp Üretkenlik Platformu'na erişimi olan Azure AD'da kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla ClickUp Üretkenlik Platformu'nda (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini ClickUp Üretkenlik Platformu ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* ClickUp Verimlilik Platformu tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* ClickUp Verimlilik Platformu **SP** tarafından başlatılan SSO'ya destek

## <a name="adding-clickup-productivity-platform-from-the-gallery"></a>Galeriden ClickUp Verimlilik Platformu Ekleme

ClickUp Üretkenlik Platformu'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize ClickUp Üretkenlik Platformu eklemeniz gerekir.

**Galeriden ClickUp Üretkenlik Platformu eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **ClickUp Verimlilik Platformu**yazın, sonuç panelinden **ClickUp Verimlilik Platformu'nu** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Verimlilik Platformu'nu tıklattı](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre ClickUp Üretkenlik Platformu ile Azure AD tek oturum açma işlemini yapılandırıp test edersiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile ClickUp Üretkenlik Platformu'ndaki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

ClickUp Üretkenlik Platformu ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[ClickUp Üretkenlik Platformu Tek Oturum Açma](#configure-clickup-productivity-platform-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[ClickUp Üretkenlik Platformu test kullanıcısını oluşturun](#create-clickup-productivity-platform-test-user)** - kullanıcının Azure REKLAM gösterimine bağlı ClickUp Üretkenlik Platformu'nda Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

ClickUp Üretkenlik Platformu ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/) **ClickUp Üretkenlik Platformu** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![ClickUp Verimlilik Platformu Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç'a** bir URL yazın:`https://app.clickup.com/login/sso`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://api.clickup.com/v1/team/<team_id>/microsoft`

    > [!NOTE]
    > Tanımlayıcı değeri gerçek değildir. Bu değeri, daha sonra bu öğreticide açıklanan gerçek Tanımlayıcı ile güncelleştirin.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-clickup-productivity-platform-single-sign-on"></a>ClickUp Verimlilik Platformu Tek Oturum Açma

1. Farklı bir web tarayıcısı penceresinde, ClickUp Üretkenlik Platformu kiracınızı yönetici olarak oturum açın.

2. **Kullanıcı profiline**tıklayın ve ardından **Ayarlar'ı**seçin.

    ![ClickUp Verimlilik Yapılandırması](./media/clickup-productivity-platform-tutorial/configure0.png)

    ![ClickUp Verimlilik Yapılandırması](./media/clickup-productivity-platform-tutorial/configure1.png)

3. Tek Oturum Açma (SSO) Sağlayıcısı altında **Microsoft'u**seçin.

    ![ClickUp Verimlilik Yapılandırması](./media/clickup-productivity-platform-tutorial/configure2.png)

4. Microsoft **Tek İşaretini Yapılandır** sayfasında aşağıdaki adımları gerçekleştirin:

    ![ClickUp Verimlilik Yapılandırması](./media/clickup-productivity-platform-tutorial/configure3.png)

    a. Varlık Kimliği değerini kopyalamak ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı (Entity ID)** textbox'a yapıştırmak için **Kopyala'yı** tıklatın.
    
    b. Azure **Federasyonu Metadata URL** textbox'ına, Azure portalından kopyaladığınız App Federation Metadata Url değerini yapıştırın ve ardından **Kaydet'i**tıklatın.

5. Kurulumu tamamlamak için, kurulumu tamamlamak ve Microsoft hesabıyla kimlik doğrulaması **yapmak için Microsoft ile Kimlik Doğrula'yı** tıklatın.

    ![ClickUp Verimlilik Yapılandırması](./media/clickup-productivity-platform-tutorial/configure4.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı **türünde\@brittasimon yourcompanydomain.extension**  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın ClickUp Üretkenlik Platformu'na erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **ClickUp Üretkenlik Platformu'nu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **ClickUp Verimlilik Platformu'nu**seçin.

    ![Uygulamalar listesindeki ClickUp Verimlilik Platformu bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-clickup-productivity-platform-test-user"></a>ClickUp Verimlilik Platformu test kullanıcı oluşturma

1. Farklı bir web tarayıcısı penceresinde, ClickUp Üretkenlik Platformu kiracınızı yönetici olarak oturum açın.

2. **Kullanıcı profiline**tıklayın ve ardından **Kişiler'i**seçin.
   
    ![ClickUp Verimlilik Yapılandırması](./media/clickup-productivity-platform-tutorial/configure0.png)

    ![ClickUp Verimlilik Yapılandırması](./media/clickup-productivity-platform-tutorial/user1.png)

3. Metin kutusuna kullanıcının e-posta adresini girin ve **Davet et'i**tıklatın.

    ![ClickUp Verimlilik Yapılandırması](./media/clickup-productivity-platform-tutorial/user2.png)

    > [!NOTE]
    > Kullanıcı bildirimi alır ve hesabı etkinleştirme davetini kabul etmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki ClickUp Verimlilik Platformu döşemesini tıklattığınızda, SSO'yu kurduğunuz ClickUp Üretkenlik Platformu'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

