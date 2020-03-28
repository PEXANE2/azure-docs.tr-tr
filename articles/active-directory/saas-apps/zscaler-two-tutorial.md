---
title: 'Öğretici: Zscaler Two ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler Two arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1fd8a940-7320-47e0-a176-2dd4eeca6db2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/24/2019
ms.author: jeedes
ms.openlocfilehash: 7ea4eecd5c27b6a9f14bc358c7db61da544e7530
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68825103"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-two"></a>Öğretici: Zscaler Two ile Azure Active Directory entegrasyonu

Bu eğitimde, Zscaler Two'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
Zscaler Two'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Zscaler Two'ya erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Zscaler Two 'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zscaler Two ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Zscaler İki tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Zscaler İki **SP** başlatılan SSO destekler

* Zscaler İki **Just In Time** kullanıcı sağlama destekler

## <a name="adding-zscaler-two-from-the-gallery"></a>Galeriden Zscaler İki ekleme

Zscaler Two'nun Azure AD'ye entegrasyonunu yapılandırmak için galeriden Zscaler Two'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zscaler Two eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Zscaler Two**yazın, sonuç panelinden **Zscaler Two'yu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Zscaler Sonuç listesinde iki](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD oturum açma işlemini Zscaler Two ile yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Zscaler Two'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturum açma işlemlerini Zscaler Two ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Zscaler İki Tek Oturum Açma](#configure-zscaler-two-single-sign-on)** -uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırır.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Zscaler İki test kullanıcısı oluşturun](#create-zscaler-two-test-user)** - Zscaler Two'daki Britta Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Zscaler Two ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **Zscaler İki** uygulama tümleştirme sayfasında, **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Zscaler İki Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum Açma URL metin kutusuna, zscaler Two uygulamanızda oturum açmanız için kullanıcılarınız tarafından kullanılan URL'yi yazın.

    > [!NOTE]
    > Değeri gerçek Oturum Açma URL'si ile güncellersiniz. Değeri almak için [Zscaler İki İstemci destek ekibine](https://www.zscaler.com/company/contact) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. Zscaler İki uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

6. Yukarıdakilere ek olarak, Zscaler İki uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:
    
    | Adı | Kaynak Özniteliği |
    | ---------| ------------ |
    | Memberof     | user.assignedroles |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    ![image](common/new-save-attribute.png)

    ![image](common/new-attribute-details.png)

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.
    
    f. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Azure AD'de Rolü nasıl yapılandıracağınıöğrenmek için [lütfen buraya](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) tıklayın

7. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

8. **Zscaler İki'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-zscaler-two-single-sign-on"></a>Zscaler İki Tek İşaret-On yapıla

1. Zscaler Two içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'i tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Kurulum Zscaler İki'ye** tıklayın ve sizi Zscaler Two uygulamasına yönlendirecektir. Buradan, Zscaler Two'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum sso](common/setup-sso.png)

3. Zscaler Two'yu el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Zscaler Two şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Yönetim **> Kimlik Doğrulama ayarları>** gidin ve aşağıdaki adımları gerçekleştirin:
   
    ![Yönetim](./media/zscaler-two-tutorial/ic800206.png "Yönetim")

    a. Kimlik Doğrulama Türü altında **SAML'yi**seçin.

    b. **SAML'yi Yapılandır'ı**tıklatın.

5. **SAML'yi Edit** penceresinde aşağıdaki adımları gerçekleştirin ve Kaydet'i tıklatın.  
            
    ![Kullanıcıları & Kimlik Doğrulamayı Yönetin](./media/zscaler-two-tutorial/ic800208.png "Kullanıcıları & Kimlik Doğrulamayı Yönetin")
    
    a. **SAML Portal URL** textbox'ına Azure portalından kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    b. Giriş **Adı Özniteliği** metin kutusuna **NameID'i**girin.

    c. Azure portalından indirdiğiniz Azure SAML imza sertifikasını **Genel SSL Sertifikası'na**yüklemek için **Yükle'yi**tıklatın.

    d. **SAML Otomatik Sağlama'yı Etkinleştir'i**geçişin.

    e. Kullanıcı **Görüntülü Reklam Adı Özniteliği** metin kutusuna, displayName öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **displayName'i** girin.

    f. Grup **Adı Özniteliği** textbox'ına, üye Of öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **üyeOf** girin.

    g. Bölüm **Adı Özniteliği** Bölüm Girin **bölümünde,** bölüm öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız.

    h. **Kaydet**'e tıklayın.

6. Kullanıcı **Kimlik Doğrulaması'nı Yapılandırma** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-two-tutorial/ic800207.png)

    a. Sol alttaki **Etkinleştirme** menüsünün üzerine titreyin.

    b. **Etkinleştir'i**tıklatın.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'da proxy ayarlarını yapılandırmak için

1. **Internet Explorer'ı başlatın.**

2. **Internet Seçenekleri** iletişim kutusunu açmak için **Araçlar** menüsünden **Internet seçeneklerini** seçin.   
    
     ![İnternet Seçenekleri](./media/zscaler-two-tutorial/ic769492.png "İnternet Seçenekleri")

3. **Bağlantılar** sekmesini tıklatın.   
  
     ![Bağlantılar](./media/zscaler-two-tutorial/ic769493.png "Bağlantılar")

4. **LAN Ayarları** iletişim kutusunu açmak için **LAN ayarlarını** tıklatın.

5. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-two-tutorial/ic769494.png "Proxy sunucu")

    a. **LAN'ınız için proxy sunucusu kullan'ı**seçin.

    b. Adres metin kutusuna **ağ geçidi yazın. Zscaler Two.net**.

    c. Bağlantı Noktası metin kutusunda **80**yazın.

    d. **Yerel adresler için Bypass proxy sunucusu'ni**seçin.

    e. **Yerel Alan Ağı (LAN) Ayarları** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

6. **Internet Seçenekleri** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma 

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı `brittasimon@yourcompanydomain.extension`türünde. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Zscaler Two'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Zscaler Two'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler Two'yu**seçin.

    ![Uygulamalar listesindeki Zscaler İki bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, listeden **Britta Simon** gibi kullanıcıyı seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

    ![image](./media/zscaler-two-tutorial/tutorial_zscalertwo_users.png)

6. Rolü **Seç** iletişim kutusundan listedeki uygun kullanıcı rolünü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

    ![image](./media/zscaler-two-tutorial/tutorial_zscalertwo_roles.png)

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini seçin.

    ![image](./media/zscaler-two-tutorial/tutorial_zscalertwo_assign.png)

### <a name="create-zscaler-two-test-user"></a>Zscaler Oluşturma İki test kullanıcısı

Bu bölümde, Britta Simon adlı bir kullanıcı Zscaler İki oluşturulur. Zscaler İki, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Zscaler Two'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [Zscaler Two destek ekibine](https://www.zscaler.com/company/contact)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Zscaler İki döşemesini tıklattığınızda, SSO'yu kurduğunuz Zscaler Two'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

