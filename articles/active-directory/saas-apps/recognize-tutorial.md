---
title: 'Öğretici: Azure Active Directory tümleştirmesi ile Recognize | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve Tanı arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: cfad939e-c8f4-45a0-bd25-c4eb9701acaa
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: jeedes
ms.openlocfilehash: c0a513ad8fe772839813615d065616f2c911a031
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68943337"
---
# <a name="tutorial-azure-active-directory-integration-with-recognize"></a>Öğretici: Recognize ile Azure Active Directory entegrasyonu

Bu eğitimde, Azure Etkin Dizini (Azure AD) ile Tanıma'yı nasıl entegre acağınızı öğrenirsiniz.
Azure AD ile Tanıma'yı tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Tanıma'ya erişimi olan denetimi yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak Tanıma (Tek Oturum Açma) oturum açmalarını etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Tanın ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Tek oturum açma özelliğine dayalı aboneliği tanıma

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* **SP** tarafından başlatılan SSO'nun desteklerini tanıyın

## <a name="adding-recognize-from-the-gallery"></a>Galeriden Tanı ekleme

Tanıma'nın Azure AD'ye tümleştirmesini yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Tanı'yı eklemeniz gerekir.

**Galeriden Tanı'yı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Tanıyın**, sonuç panelinden **Tanı'yı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde tanıma](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Tanıyın ile yapılandırıp test esinizsiniz.
Tek oturum açmanın çalışabilmesi için, bir Azure AD kullanıcısı ile Tanı'daki ilgili kullanıcı arasında bir bağlantı ilişkisikurulması gerekir.

Azure AD oturum açma işlemlerini Recognize ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Tek Oturum Açma'yı Tanıyın](#configure-recognize-single-sign-on)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılayın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Kullanıcının](#create-recognize-test-user)** Azure AD gösterimine bağlı Recognize'daki Britta Simon'ın bir örneğine sahip olmak için test kullanıcısını tanıyın.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Tanıt ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), Uygulama tümleştirmesini **tanı** yat sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız**varsa aşağıdaki adımları gerçekleştirin:

    >[!NOTE]
    >Hizmet Sağlayıcısı **meta veri dosyasını,** öğreticinin **Tek Oturum Açma'yı Tanıyın** bölümünden alırsınız.

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![Meta veri dosyalarını yükleme](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![meta veri dosyasini seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** değeri Temel SAML Yapılandırması bölümünde otomatik olarak doldurulur.

    ![Etki Alanı ve URL'leri tek oturum açma bilgilerini tanıyın](common/sp-identifier.png)

     URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://recognizeapp.com/<your-domain>/saml/sso`

    > [!Note]
    > **Tanımlayıcı** değeri otomatik olarak doldurulmazsa, öğreticinin Daha sonra **Yapılandırılan Tek Oturum Açma** bölümünde açıklanan SSO Ayarları bölümünden Servis Sağlayıcı Metaveri URL'sini açarak Tanımlayıcı değerini alırsınız. Oturum açma URL değeri gerçek değildir. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [İstemciyi Tanıyın destek ekibine](mailto:support@recognizeapp.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Algıla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-recognize-single-sign-on"></a>Tek İşaret-On'u Tanıyın

1. Farklı bir web tarayıcısı penceresinde, kiracınızı yönetici olarak tanıyın'da oturum açın.

2. Sağ üst köşede **Menü'ye**tıklayın. Şirket **Yöneticisi'ne**git.
   
    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/recognize-tutorial/tutorial_recognize_000.png)

3. Soldaki gezinti bölmesine **Ayarlar'ı**tıklatın.
   
    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/recognize-tutorial/tutorial_recognize_001.png)

4. **SSO Ayarları** bölümünde aşağıdaki adımları gerçekleştirin.
   
    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/recognize-tutorial/tutorial_recognize_002.png)
    
    a. **SSO'yı etkinleştirin,** **AÇI'yı**seçin.

    b. **IDP Entity ID** textbox'a, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı** değerini yapıştırın.
    
    c. **Sso hedef url** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.
    
    d. **Slo hedef url** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın. 
    
    e. İndirilen **Sertifika (Base64)** dosyanızı not defterinde açın, içeriğini panonuza kopyalayın ve **ardından Sertifika** metin kutusuna yapıştırın.
    
    f. Ayarları **Kaydet** düğmesini tıklatın. 

5. **SSO Ayarları** bölümünün yanı sıra, **URL'yi Servis Sağlayıcı Metadata url'sinin**altında kopyalayın.
   
    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/recognize-tutorial/tutorial_recognize_003.png)

6. Meta **veri** belgesini indirmek için boş bir tarayıcının altındaki Meta veri URL bağlantısını açın. Ardından EntityDescriptor değerini (entityID) dosyadan kopyalayın ve Azure portalındaki **Temel SAML** Yapılandırması'ndaki **Tanımlayıcı** textbox'a yapıştırın.
    
    ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/recognize-tutorial/tutorial_recognize_004.png)

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

Bu bölümde, Britta Simon'ın Tanıma'ya erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve sonra **Tanı'yı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Tanıyın'ı**seçin.

    ![Uygulamalar listesindeki Tanıma bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-recognize-test-user"></a>Test kullanıcısını Tanıyın

Azure AD kullanıcılarının Tanı'ya giriş yapabilmeleri için Tanı'ya dahil olmaları gerekir. Recognize durumunda, sağlama el ile bir görevdir.

Bu uygulama SCIM sağlama desteklemez, ancak kullanıcıları sağlayan alternatif bir kullanıcı eşitlemesi vardır. 

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Şirket siteyi tanı yörenizde yönetici olarak oturum açın.

2. Sağ üst köşede **Menü'ye**tıklayın. Şirket **Yöneticisi'ne**git.

3. Soldaki gezinti bölmesine **Ayarlar'ı**tıklatın.

4. **Kullanıcı Eşitleme** bölümünde aşağıdaki adımları gerçekleştirin.
   
    ![Yeni Kullanıcı](./media/recognize-tutorial/tutorial_recognize_005.png "Yeni Kullanıcı")
   
    a. **Eşitleme Etkin**olarak, **ON**seçin.
   
    b. **Eşitleme sağlayıcısını seçin,** **Microsoft / Office 365'i**seçin.
   
    c. **Kullanıcı Eşitle'yi Çalıştır'ı**tıklatın.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Tanıma döşemesini tıklattığınızda, SSO'yu kurduğunuz Tanıma'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

