---
title: 'Öğretici: Peakon ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Peakon arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a944c397-ed3f-4d45-b9b2-6d4bcb6b0a09
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/19/2019
ms.author: jeedes
ms.openlocfilehash: b093a26848701254ad674081037c266f1fb012b2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67094715"
---
# <a name="tutorial-azure-active-directory-integration-with-peakon"></a>Öğretici: Peakon ile Azure Active Directory entegrasyonu

Bu eğitimde, Peakon'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Peakon'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Peakon erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Peakon'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Peakon ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Peakon tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Peakon **SP** ve **IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-peakon-from-the-gallery"></a>Galeriden Peakon ekleme

Peakon'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Peakon'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Peakon eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Peakon**yazın, sonuç panelinden **Peakon'u** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde peakon](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test edeyimsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Peakon'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Peakon ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Peakon Tek Oturum Açma'yı uygulama](#configure-peakon-single-sign-on)** tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Peakon test kullanıcısını oluşturun](#create-peakon-test-user)** - Peakon'daki Britta Simon'ın, kullanıcının Azure AD gösterimine bağlı bir örneğine sahip olmak için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Peakon ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Peakon** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Peakon Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.peakon.com/saml/<companyid>/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.peakon.com/saml/<companyid>/assert`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Peakon Domain ve URL'ler tek oturum açma bilgileri](common/metadata-upload-additional-signon.png)

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.peakon.com/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, öğreticide daha sonra açıklanan gerçek Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Ham)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificateraw.png)

7. **Peakon'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-peakon-single-sign-on"></a>Peakon Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Peakon'da oturum açın.

2. Sayfanın sol tarafındaki menü çubuğunda **Yapılandırma'yı**tıklatın ve ardından **Tümleştirmeler'e**gidin.

    ![The Config](./media/peakon-tutorial/tutorial_peakon_config.png)

3. **Tümleştirmeler** **sayfasında, Tek Oturum Açma'ya**tıklayın.

    ![Tek](./media/peakon-tutorial/tutorial_peakon_single.png)

4. **Tek Oturum Açma** bölümünün altında **Etkinleştir'e**tıklayın.

    ![Etkinleştirme](./media/peakon-tutorial/tutorial_peakon_enable.png)

5. **SAML** bölümünü kullanan çalışanlar için Tek oturum açma da aşağıdaki adımları gerçekleştirin:

    ![Saml](./media/peakon-tutorial/tutorial_peakon_saml.png)

    a. **SSO Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    b. **SSO Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    c. Azure portalından indirdiğiniz sertifikayı Sertifika kutusuna yüklemek için **dosyayı seç'i** tıklatın.

    d. Azure portalındaki **Temel SAML Yapılandırma** sı bölümündeKi **Identifier** textbox'ta **Entity Id** ve yapıştırmayı kopyalamak için **simgeyi** tıklatın.

    e. Azure portalındaki **Temel SAML Yapılandırması** bölümünde **Yanıtla URL'sini (ACS)** kopyalamak ve **Yanıtla URL** metin kutusuna yapıştırmak için **simgeyi** tıklatın.

    f. **Kaydet'i** tıklatın

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

Bu bölümde, Britta Simon'ın Peakon'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Peakon'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Peakon'u**seçin.

    ![Uygulamalar listesindeki Peakon bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-peakon-test-user"></a>Peakon test kullanıcısı oluşturma

Azure AD kullanıcılarının Peakon'da oturum açmalarını etkinleştirmek için Peakon'da oturum açmaları gerekir.  
Peakon söz konusu olduğunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Peakon şirket sitenizde yönetici olarak oturum açın.

2. Sayfanın sol tarafındaki menü çubuğunda **Yapılandırma'yı**tıklatın ve ardından **Çalışanlar'a**gidin.

    ![Çalışan](./media/peakon-tutorial/tutorial_peakon_employee.png)

3. Sayfanın sağ üst tarafında **çalışan ekle'yi**tıklatın.

      ![Ek çalışan](./media/peakon-tutorial/tutorial_peakon_addemployee.png)

3. Yeni **çalışan** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

     ![Yeni çalışan](./media/peakon-tutorial/tutorial_peakon_create.png)

    a. **Ad** metin kutusuna, ilk adı **Britta** ve **soyadını simon**olarak yazın.

    b. **E-posta** metin kutusuna **\@Brittasimon contoso.com**gibi e-posta adresini yazın.

    c. **Çalışan Oluştur'u**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Peakon döşemesini tıklattığınızda, SSO'yu kurduğunuz Peakon'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

