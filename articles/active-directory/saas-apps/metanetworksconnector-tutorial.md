---
title: 'Öğretici: Meta Networks Bağlayıcısı ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Meta Networks Bağlayıcısı arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 4ae5f30d-113b-4261-b474-47ffbac08bf7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/21/2019
ms.author: jeedes
ms.openlocfilehash: a09eda25e8c7cc087770210cdfbe7e2bc9832acf
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160648"
---
# <a name="tutorial-azure-active-directory-integration-with-meta-networks-connector"></a>Öğretici: Meta Networks Bağlayıcısı ile Azure Active Directory entegrasyonu

Bu eğitimde, Meta Networks Connector'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Meta Networks Connector'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Meta Networks Bağlayıcısı'na erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Meta Networks Connector 'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Meta Networks Bağlayıcısı ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Meta Networks Connector tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Meta Networks Connector **SP** ve **IDP** başlatılan SSO destekler
 
* Meta Networks Connector **Just In Time** kullanıcı sağlama destekler

## <a name="adding-meta-networks-connector-from-the-gallery"></a>Galeriden Meta Ağlar Bağlayıcısı Ekleme

Meta Networks Connector'Un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Meta Networks Connector'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Meta Networks Bağlayıcısı eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda Meta **Networks Bağlayıcısı**yazın, sonuç panelinden **Meta Networks Bağlayıcısı'nı** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde meta ağlar bağlayıcısı](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Meta Networks Connector ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure REKLAM kullanıcısı ile Meta Networks Connector'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Meta Networks Connector ile Azure AD oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Meta Networks Connector'u Tek Oturum Açma](#configure-meta-networks-connector-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Meta Networks Connector test kullanıcısını oluşturun](#create-meta-networks-connector-test-user)** - Kullanıcının Azure AD gösterimine bağlı Meta Networks Connector'da Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Meta Networks Connector ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Meta Networks Connector** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    ![Meta Networks Bağlayıcı Etki Alanı ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/saml/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://login.nsof.io/v1/<ORGANIZATION-SHORT-NAME>/sso/saml`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    ![Meta Networks Bağlayıcı Etki Alanı ve URL'ler tek oturum açma bilgileri](common/both-advanced-urls.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/login`

    b. **Röle Durumu** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:`https://<ORGANIZATION-SHORT-NAME>.metanetworks.com/#/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtURL'si ve Oturum Açma URL'si ile güncelleştirin, daha sonra öğreticide açıklanır.

6. Meta Networks Connector uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)
    
7. Yukarıdakilere ek olarak, Meta Networks Connector uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:
    
    | Adı | Kaynak özniteliği | Ad Alanı|
    | ---------------| --------------- | -------- |
    | Ad | user.givenname | |
    | Soyadı | user.surname | |
    | Emailaddress| kullanıcı.posta| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | ad | user.userprincipalname| `http://schemas.xmlsoap.org/ws/2005/05/identity/claims` |
    | telefon | kullanıcı.telefon numarası | |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

8. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

9. Meta **Networks Connector'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-meta-networks-connector-single-sign-on"></a>Meta Ağları Konektörü Tek İşaretli Yapıla

1. Tarayıcınızda yeni bir sekme açın ve Meta Networks Connector yönetici hesabınızda oturum açın.
    
    > [!NOTE]
    > Meta Networks Connector güvenli bir sistemdir. Bu nedenle, portallarına erişmeden önce halka açık IP adresinizin yanlarındaki izin listesine eklenmesi ni almalısınız. Genel IP adresinizi almak [için, burada](https://whatismyipaddress.com/)belirtilen aşağıdaki bağlantıyı izleyin. IP adresinizin izin listesine eklenmesi için IP adresinizi [Meta Networks Connector Client destek ekibine](mailto:support@metanetworks.com) gönderin.
    
2. **Administrator'a** gidin ve **Ayarlar'ı**seçin.
    
    ![Tek İşaret-On'u Yapılandır](./media/metanetworksconnector-tutorial/configure3.png)
    
3. Günlük **Internet Trafiği** ve **Force VPN MFA** kapalı ayarlanmış olduğundan emin olun.
    
    ![Tek İşaret-On'u Yapılandır](./media/metanetworksconnector-tutorial/configure1.png)
    
4. **Yönetici'ye** gidin ve **SAML'yi**seçin.
    
    ![Tek İşaret-On'u Yapılandır](./media/metanetworksconnector-tutorial/configure4.png)
    
5. **AYRıNTıLAR** sayfasında aşağıdaki adımları gerçekleştirin:
    
    ![Tek İşaret-On'u Yapılandır](./media/metanetworksconnector-tutorial/configure2.png)
    
    a. **SSO URL** değerini kopyalayın ve **Meta Networks Bağlayıcı Etki Alanı ve URL'ler** bölümündeki **Oturum Açma URL** textbox'ına yapıştırın.
    
    b. **Alıcı URL** değerini kopyalayın ve Meta Networks Bağlayıcı Etki Alanı ve URL'ler bölümündeki **Yanıtla URL** textbox'ına yapıştırın. **Meta Networks Connector Domain and URLs**
    
    c. **Hedef Kitle URI (SP Entity ID)** değerini kopyalayın ve Meta Networks Bağlayıcı Etki Alanı ve **URL'ler** bölümündeki **Tanımlayıcı (Entity ID)** textbox'ına yapıştırın.
    
    d. SAML'yi etkinleştirme
    
6. **GENEL** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/metanetworksconnector-tutorial/configure5.png)

    a. Kimlik **Sağlayıcı Tek Oturum Açma**URL'sinde, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    b. Kimlik **Sağlayıcı**Veren'e, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    c. İndirilen sertifikayı not defterinde Azure portalından açın, **X.509 Sertifikası** metin kutusuna yapıştırın.

    d. **Just-in-Time Provisioning**etkinleştirin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanına **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanında **brittasimon\@yourcompanydomain.extension** yazın  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Meta Networks Bağlayıcısı'na erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından Meta **Networks Bağlayıcısı'nı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Meta Networks Bağlayıcısı'nı**seçin.

    ![Uygulamalar listesindeki Meta Networks Bağlayıcısı bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-meta-networks-connector-test-user"></a>Meta Networks Bağlayıcı sı test kullanıcısı oluşturma

Bu bölümde, Meta Networks Connector'da Britta Simon adında bir kullanıcı oluşturulur. Meta Networks Connector, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Meta Networks Connector'da bir kullanıcı zaten yoksa, Meta Networks Connector'a erişmeye çalıştığınızda yeni bir kullanıcı oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [Meta Networks Connector Client destek ekibine](mailto:support@metanetworks.com)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Meta Networks Bağlayıcı döşemesini tıklattığınızda, SSO'yu kurduğunuz Meta Networks Bağlayıcısı'nda otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

