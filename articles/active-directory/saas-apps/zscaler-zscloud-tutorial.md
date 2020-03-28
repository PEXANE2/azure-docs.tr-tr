---
title: 'Öğretici: Zscaler ZSCloud ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler ZSCloud arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 411d5684-a780-410a-9383-59f92cf569b5
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/24/2019
ms.author: jeedes
ms.openlocfilehash: 43d7e58f0c267afe8a22c217d9800abb041df8cb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68723050"
---
# <a name="tutorial-azure-active-directory-integration-with-zscaler-zscloud"></a>Öğretici: Zscaler ZSCloud ile Azure Active Directory entegrasyonu

Bu eğitimde, Zscaler ZSCloud'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Zscaler ZSCloud'u Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Zscaler ZSCloud erişimi olan Azure AD'de kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Zscaler ZSCloud 'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Zscaler ZSCloud ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Zscaler ZSCloud tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Zscaler ZSCloud **SP** başlatılan SSO destekler

* Zscaler ZSCloud **Just In Time** kullanıcı sağlama destekler

## <a name="adding-zscaler-zscloud-from-the-gallery"></a>Galeriden Zscaler ZSCloud ekleme

Zscaler ZSCloud'un Azure AD'ye entegrasyonunu yapılandırmak için galeriden Zscaler ZSCloud'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Zscaler ZSCloud eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Zscaler ZSCloud**yazın, sonuç panelinden **Zscaler ZSCloud'u** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Zscaler ZSCloud sonuç listesinde](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Zscaler ZSCloud ile Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp test esiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Zscaler ZSCloud'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Zscaler ZSCloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Zscaler ZSCloud Tek Oturum Açma](#configure-zscaler-zscloud-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Zscaler ZSCloud test kullanıcısını oluşturun](#create-zscaler-zscloud-test-user)** - Zscaler ZSCloud'da Britta Simon'ın, kullanıcının Azure REKLAM gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Zscaler ZSCloud ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Zscaler ZSCloud** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Zscaler ZSCloud Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-signonurl.png)

    Oturum **Açma URL** metin kutusuna, ZScaler ZSCloud uygulamanızda oturum açmanız için kullanıcılarınız tarafından kullanılan URL'yi yazın.

    > [!NOTE]
    > Değeri gerçek Oturum Açma URL'si ile güncelleştirmeniz gerekir. Değeri almak için [Zscaler ZSCloud İstemci destek ekibine](https://help.zscaler.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. Zscaler ZSCloud uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Kullanıcı Öznitelikleri** iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

6. Zscaler ZSCloud uygulaması, yukarıdakilere ek olarak SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:
    
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

8. **Zscaler ZSCloud'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-zscaler-zscloud-single-sign-on"></a>Zscaler ZSCloud Tek Oturum Açma'yı Yapılandır

1. Zscaler ZSCloud içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Kurulum Zscaler ZSCloud'a** tıklayın ve sizi Zscaler ZSCloud uygulamasına yönlendirecektir. Buradan, Zscaler ZSCloud'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum sso](common/setup-sso.png)

3. Zscaler ZSCloud'u el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Zscaler ZSCloud şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Yönetim **> Kimlik Doğrulama ayarları>** gidin ve aşağıdaki adımları gerçekleştirin:
   
    ![Yönetim](./media/zscaler-zscloud-tutorial/ic800206.png "Yönetim")

    a. Kimlik Doğrulama Türü altında **SAML'yi**seçin.

    b. **SAML'yi Yapılandır'ı**tıklatın.

5. **SAML'yi Edit** penceresinde aşağıdaki adımları gerçekleştirin ve Kaydet'i tıklatın.  
            
    ![Kullanıcıları & Kimlik Doğrulamayı Yönetin](./media/zscaler-zscloud-tutorial/ic800208.png "Kullanıcıları & Kimlik Doğrulamayı Yönetin")
    
    a. **SAML Portal URL** textbox'ına Azure portalından kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    b. Giriş **Adı Özniteliği** metin kutusuna **NameID'i**girin.

    c. Azure portalından indirdiğiniz Azure SAML imza sertifikasını **Genel SSL Sertifikası'na**yüklemek için **Yükle'yi**tıklatın.

    d. **SAML Otomatik Sağlama'yı Etkinleştir'i**geçişin.

    e. Kullanıcı **Görüntülü Reklam Adı Özniteliği** metin kutusuna, displayName öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **displayName'i** girin.

    f. Grup **Adı Özniteliği** textbox'ına, üye Of öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **üyeOf** girin.

    g. Bölüm **Adı Özniteliği** Bölüm Girin **bölümünde,** bölüm öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız.

    h. **Kaydet**'e tıklayın.

6. Kullanıcı **Kimlik Doğrulaması'nı Yapılandırma** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-zscloud-tutorial/ic800207.png)

    a. Sol alttaki **Etkinleştirme** menüsünün üzerine titreyin.

    b. **Etkinleştir'i**tıklatın.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'da proxy ayarlarını yapılandırmak için

1. **Internet Explorer'ı başlatın.**

2. **Internet Seçenekleri** iletişim kutusunu açmak için **Araçlar** menüsünden **Internet seçeneklerini** seçin.   
    
     ![İnternet Seçenekleri](./media/zscaler-zscloud-tutorial/ic769492.png "İnternet Seçenekleri")

3. **Bağlantılar** sekmesini tıklatın.   
  
     ![Bağlantılar](./media/zscaler-zscloud-tutorial/ic769493.png "Bağlantılar")

4. **LAN Ayarları** iletişim kutusunu açmak için **LAN ayarlarını** tıklatın.

5. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-zscloud-tutorial/ic769494.png "Proxy sunucu")

    a. **LAN'ınız için proxy sunucusu kullan'ı**seçin.

    b. Adres metin kutusuna **ağ geçidi yazın. Zscaler ZSCloud.net**.

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
  
    b. Kullanıcı **adı** alanı brittasimon@yourcompanydomain.extensiontüründe. Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın Zscaler ZSCloud'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **Zscaler ZSCloud'u**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Zscaler ZSCloud'u**seçin.

    ![Uygulamalar listesindeki Zscaler ZSCloud bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, listeden **Britta Simon** gibi kullanıcıyı seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_users.png)

6. Rolü **Seç** iletişim kutusundan listedeki uygun kullanıcı rolünü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_roles.png)

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini seçin.

    ![image](./media/zscaler-zscloud-tutorial/tutorial_zscalerzscloud_assign.png)

    >[!NOTE]
    >Varsayılan erişim rolü, bu sağlamayı kıracağı için desteklenmez, bu nedenle kullanıcı yıatama sırasında varsayılan rol seçilemez.

### <a name="create-zscaler-zscloud-test-user"></a>Zscaler ZSCloud test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı Zscaler ZSCloud oluşturulur. Zscaler ZSCloud, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Zscaler ZSCloud'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [Zscaler ZSCloud destek ekibine](https://help.zscaler.com/)başvurun.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Zscaler ZSCloud döşemesini tıklattığınızda, SSO'yu kurduğunuz Zscaler ZSCloud'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

