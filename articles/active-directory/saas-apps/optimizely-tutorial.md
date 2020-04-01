---
title: 'Öğretici: Optimize ly ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizini ve Optimize Etme arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28ef03e1-9aad-4301-af97-d94e853edc74
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/14/2019
ms.author: jeedes
ms.openlocfilehash: 2e25c615e040dd4359e278b95045fbc71ca60ef1
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68943951"
---
# <a name="tutorial-azure-active-directory-integration-with-optimizely"></a>Öğretici: Optimize ly ile Azure Active Directory entegrasyonu

Bu öğreticide, Azure Etkin Dizini (Azure AD) ile optimize edilmiş şekilde nasıl entegre acağınızı öğreneceksiniz.
Azure AD ile Optimize edilmiş şekilde tümleştirme, size aşağıdaki avantajları sağlar:

* Optimize olarak erişebilen Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla otomatik olarak Optimize (Tek Oturum Açma) oturum açmalarını etkinleştirebilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Optimize edeiyi yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* En iyi şekilde tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* **SP** tarafından başlatılan SSO'nun en iyi şekilde desteklenmelerini

## <a name="adding-optimizely-from-the-gallery"></a>Galeriden Optimize Ekleme

Optimize tümleştirmeyi Azure AD'ye yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Optimize olarak eklemeniz gerekir.

**Galeriden Optimize eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **En İyi Duruma Getir**, sonuç panelinden En İyi Duruma **Getir'i** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde en iyi şekilde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Optimize edilmiş şekilde yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Optimizely'deki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Optimize edeleştirmek ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için Optimize Tek Oturum Açma'yı **[yapılandırın.](#configure-optimizely-single-sign-on)**
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Optimize test kullanıcısı oluşturun](#create-optimizely-test-user)** - Optimize olarak kullanıcının Azure AD gösterimine bağlı olan Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Optimize ede iyi şekilde yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Optimize uygulama** tümleştirme sayfasında Tek oturum **açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![En İyi Şekilde Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.optimizely.net/<instance name>`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`urn:auth0:optimizely:contoso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Değeri, öğreticide daha sonra açıklanan gerçek Oturum Açma URL'si ve Tanımlayıcıile güncelleştireceksiniz. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. En İyi Duruma Getirici uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

6. Yukarıdakilere ek olarak, Optimize uygulama SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Adı | Kaynak Özniteliği |
    | ---------------| --------------- |
    | e-posta | kullanıcı.posta |
    
    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. En **İyi Duruma Getir** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-optimizely-single-sign-on"></a>Yapılandırma Optimize Tek Oturum Açma

1. **Optimize etme** tarafında tek oturum açma yapılandırmak için Optimize Hesap Yöneticinize başvurun ve indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'leri sağlayın.

2. E-postanıza yanıt olarak, Optimize bir şekilde size Sign On URL (SP tarafından başlatılan SSO) ve Tanımlayıcı (Hizmet Sağlayıcı Varlık Kimliği) değerlerini sağlar.

    a. Optimize ly tarafından sağlanan **SP tarafından başlatılan SSO URL'sini** kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümünde Ki **Oturum Aç URL** metin kutusuna yapıştırın.

    b. Optimize olarak sağlanan **Servis Sağlayıcı Varlık Kimliğini** kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı** metin kutusuna yapıştırın.

3. Farklı bir tarayıcı penceresinde, Optimize edici uygulamanızda oturum açın.

4. Sağ üst köşede hesap adını tıklatın ve ardından **Hesap Ayarları.**

    ![Azure AD Tek Oturum Açma](./media/optimizely-tutorial/tutorial_optimizely_09.png)

5. Hesap sekmesinde, **Genel Bakış** bölümünde Tek Oturum Açık altında **SSO etkinleştir** kutusunu işaretleyin.
  
    ![Azure AD Tek Oturum Açma](./media/optimizely-tutorial/tutorial_optimizely_10.png)

6. **Kaydet'i** tıklatın

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

Bu bölümde, Britta Simon'ın Optimize ly erişimi sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Optimize Edin'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **En İyi Duruma Getir'i**seçin.

    ![Uygulamalar listesindeki En İyi Duruma Getirilmiş bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-optimizely-test-user"></a>Optimize test kullanıcısı oluşturma

Bu bölümde, Optimize ly'de Britta Simon adında bir kullanıcı oluşturursunuz.

1. Ana **sayfada, Ortak Çalışanlar** sekmesini seçin.

2. Projeye yeni ortak çalışan eklemek için **Yeni İşbirlikçi'yi**tıklatın.
   
    ![Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/create_aaduser_10.png)

3. E-posta adresini doldurun ve onlara bir rol atayın. **Davet Et'i**tıklatın.

    ![Azure AD test kullanıcısı oluşturma](./media/optimizely-tutorial/create_aaduser_11.png)

4. Bir e-posta daveti alırlar. E-posta adresini kullanarak, Optimizely'de oturum açmaları gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki En İyi Duruma Getiriyi tıklattığınızda, SSO'yu kurduğunuz Optimize'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

