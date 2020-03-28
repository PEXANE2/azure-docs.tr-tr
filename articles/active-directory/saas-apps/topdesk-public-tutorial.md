---
title: 'Öğretici: TOPdesk ile Azure Active Directory entegrasyonu - Genel | Microsoft Dokümanlar'
description: Azure Active Directory ve TOPdesk - Public arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0873299f-ce70-457b-addc-e57c5801275f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/02/2019
ms.author: jeedes
ms.openlocfilehash: e5575a2e8f776e87fcd4e6f4a7a9244752ebfd9a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "71950416"
---
# <a name="tutorial-azure-active-directory-integration-with-topdesk---public"></a>Öğretici: TOPdesk ile Azure Active Directory entegrasyonu - Genel

Bu eğitimde, TOPdesk - Public with Azure Active Directory (Azure AD) ile nasıl entegre edilesiniz öğrenebilirsiniz.
TOPdesk'i tümleştirme - Azure AD ile herkese açık olmak size aşağıdaki avantajları sağlar:

* TOPdesk - Public erişimi olan Azure AD'da kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla TOPdesk - Public (Tek Oturum Açma) ile otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini TOPdesk - Public ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* TOPdesk - Herkese açık tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* TOPdesk - Kamu **SP** başlatılan SSO destekler

## <a name="adding-topdesk---public-from-the-gallery"></a>TOPdesk ekleme - Galeriden Genel

TOPdesk-Public'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize TOPdesk - Public eklemeniz gerekir.

**TOPdesk - Galeriden genel eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **TOPdesk**yazın - Genel , **TOPdesk** seçin - Sonuç panelinden ortak sonra uygulama eklemek için **Ekle** düğmesini tıklatın.

     ![TOPdesk - Sonuç listesinde genel](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini TOPdesk ile yapılandırıp test emzebilirsiniz - Genel olarak **Britta Simon**adlı bir test kullanıcısına göre.
Tek oturum açmanın çalışması için, bir Azure AD kullanıcısı ile TOPdesk'teki ilgili kullanıcı - Ortak kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

Azure AD oturum açma işlemlerini TOPdesk - Public ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[TOPdesk -Public Single Sign-On](#configure-topdesk---public-single-sign-on)** - yapıla.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[TOPdesk oluşturun - Genel test kullanıcısı](#create-topdesk---public-test-user)** - TOPdesk'te Britta Simon'ın bir muadili olması için - Kullanıcının Azure AD temsiline bağlı genel.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

TOPdesk - Public ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **TOPdesk - Genel** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4.  Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız**varsa aşağıdaki adımları gerçekleştirin:

    >[!NOTE]
    >Hizmet Sağlayıcı **meta veri dosyasını,** daha sonra öğreticide açıklanan **Yapılandırılan TOPdesk - Public Single Sign-On** bölümünden alırsınız.

    a. **Meta veri dosyalarını yükle'yi**tıklatın.
    
    ![Meta veri dosyalarını yükleme](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![meta veri dosyasini seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri Temel SAML Yapılandırması bölümünde otomatik olarak doldurulur.

    ![TOPdesk - Kamu Malı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    d. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.topdesk.net`

    e. Tanımlayıcı **URL** textbox'ında, TOPdesk yapılandırmasından alabileceğiniz TOPdesk meta veri URL'sini doldurun. Aşağıdaki deseni kullanmalıdır:`https://<companyname>.topdesk.net/saml-metadata/<identifier>`
    
    f. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.topdesk.net/tas/public/login/verify`
    
    > [!NOTE] 
    > **Tanımlayıcı** ve **YanıtURL** değerleri otomatik olarak doldurulmuyorsa, bunları el ile girmeniz gerekir. Tanımlayıcı için, yukarıda belirtildiği gibi deseni izleyin ve daha sonra öğreticide açıklanan **Yapılandırılan TOPdesk - Public Single Sign-On** bölümünden Yanıt URL değeri alın. **Oturum Açma URL** değeri gerçek değildir, bu nedenle değeri gerçek Oturum Açma URL'si ile güncelleştirmeniz gerekir. Değeri almak için [TOPdesk - Public Client destek ekibine](https://help.topdesk.com/saas/enterprise/user/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **ToPdesk-Ortak bölümüne ayarla'** da, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-topdesk---public-single-sign-on"></a>TOPdesk'i Yapılandır - Genel Tek Oturum Açma

1. YÖNETICI olarak **TOPdesk - Public** company sitenizde oturum açın.

2. **TOPdesk** menüsünde **Ayarlar'ı**tıklatın.
   
    ![Ayarlar](./media/topdesk-public-tutorial/ic790598.png "Ayarlar")

3. **Giriş Ayarları'nı**tıklatın.
   
    ![Giriş Ayarları](./media/topdesk-public-tutorial/ic790599.png "Giriş Ayarları")

4. Giriş **Ayarları** menüsünü genişletin ve ardından **Genel'i**tıklatın.
   
    ![Genel](./media/topdesk-public-tutorial/ic790600.png "Genel")

5. **SAML giriş** yapılandırması bölümünün **Genel** bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Teknik Ayarlar](./media/topdesk-public-tutorial/ic790601.png "Teknik Ayarlar")
   
    a. Ortak meta veri dosyasını indirmek ve ardından bilgisayarınıza yerel olarak kaydetmek için **İndir'i** tıklatın.
   
    b. İndirilen meta veri dosyasını açın ve ardından **AssertionConsumerService** düğümini bulun.

    ![İddiaTüketiciServisi](./media/topdesk-public-tutorial/ic790619.png "İddiaTüketiciServisi")
   
    c. **AssertionConsumerService** değerini kopyalayın, bu değeri **Temel SAML Yapılandırması** bölümündeki **YanıtLA URL** textbox'ına yapıştırın.      
   
6. Sertifika dosyası oluşturmak için aşağıdaki adımları gerçekleştirin:
    
    ![Sertifika](./media/topdesk-public-tutorial/ic790606.png "Sertifika")
    
    a. Azure portalından indirilen meta veri dosyasını açın.
    
    b. **Xsi:fed türü** olan **RoleScriptor** düğümünün **genişletin:ApplicationServiceType**.
    
    c. **X509Sertifika** düğümünün değerini kopyalayın.
    
    d. Kopyalanan **X509Sertifika** değerini bilgisayarınıza yerel olarak bir dosyaya kaydedin.

7. **Genel** bölümünde **Ekle'yi**tıklatın.
    
    ![SAML Giriş](./media/topdesk-public-tutorial/ic790625.png "SAML Giriş")

8. **SAML yapılandırma yardımcısı** iletişim sayfasında aşağıdaki adımları gerçekleştirin:
    
    ![SAML Yapılandırma Asistanı](./media/topdesk-public-tutorial/ic790608.png "SAML Yapılandırma Asistanı")
    
    a. İndirdiğiniz meta veri dosyanızı Azure portalından, **Federasyon Meta verileri**altında yüklemek için **Gözat'ı**tıklatın.

    b. Sertifika dosyanızı yüklemek için **Sertifika (RSA)** altında **Gözat'ı**tıklatın.

    c. TOPdesk destek ekibinden aldığınız logo dosyasını **Logo simgesinin**altına yüklemek için **Gözat'ı**tıklatın.

    d. Kullanıcı **adı özniteliği** textbox,. `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    e. Görüntü **adı** metin kutusuna yapılandırmanız için bir ad yazın.

    f. **Kaydet**'e tıklayın.

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

Bu bölümde, Britta Simon'ın TOPdesk - Public'e erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **TOPdesk - Public**'i seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar **listesinde, TOPdesk - Public'i**seçin.

    ![TOPdesk - Uygulamalar listesindeki genel bağlantı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-topdesk---public-test-user"></a>TOPdesk oluşturma - Genel test kullanıcısı

Azure AD kullanıcılarının TOPdesk -Public'te oturum açabilmeleri için, bunların TOPdesk - Public'te sağlanması gerekir. TOPdesk durumunda - Kamu, sağlama manuel bir görevdir.

### <a name="to-configure-user-provisioning-perform-the-following-steps"></a>Kullanıcı sağlama yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. YÖNETICI olarak **TOPdesk - Public** company sitenizde oturum açın.

2. Üstteki menüde **TOPdesk \> Yeni \> Destek \> Dosyaları Kişi'yi**tıklatın.
   
    ![Kişi](./media/topdesk-public-tutorial/ic790628.png "Kişi")

3. Yeni Kişi iletişim kutusunda aşağıdaki adımları gerçekleştirin:
   
    ![Yeni Kişi](./media/topdesk-public-tutorial/ic790629.png "Yeni Kişi")
   
    a. Genel sekmesini tıklatın.

    b. **Soyadı** metin kutusunda, Simon gibi kullanıcının Soyadıyazın
 
    c. Hesap için bir **Site** seçin.
 
    d. **Kaydet**'e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için diğer TOPdesk -ORTAK kullanıcı hesabı oluşturma araçlarını veya TOPdesk tarafından sağlanan API'leri - kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Genel döşeme olan TOPdesk'i tıklattığınızda, SSO'yu kurduğunuz TOPdesk - Public'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
