---
title: 'Öğretici: MOVEit Aktarımı ile Azure Active Directory tümleştirmesi - Azure AD tümleştirmesi | Microsoft Dokümanlar'
description: Azure Active Directory ve MOVEit Aktarımı - Azure AD tümleştirmesi arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 8ff7102d-be73-4888-ae81-d8e3d01dd534
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/25/2019
ms.author: jeedes
ms.openlocfilehash: 43383e82d983c998d159728997da4757cc364999
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73161325"
---
# <a name="tutorial-azure-active-directory-integration-with-moveit-transfer---azure-ad-integration"></a>Öğretici: MOVEit Aktarımı ile Azure Active Directory tümleştirmesi - Azure AD tümleştirmesi

Bu eğitimde, MOVEit Aktarım - Azure AD tümleştirmesini Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
MOVEit Transferini tümleştirmek - Azure AD tümleştirmesi size aşağıdaki avantajları sağlar:

* MOVEit Aktarım - Azure AD tümleştirmesine erişimi olan Azure AD'da kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla MOVEit Aktarımı - Azure AD tümleştirmesinde (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini MOVEit Aktarım - Azure AD tümleştirmesiyle yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* MOVEit Transfer - Azure AD tümleştirmesi tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* MOVEit Transfer - Azure AD **tümleştirmesi SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-moveit-transfer---azure-ad-integration-from-the-gallery"></a>Galeriden MOVEit Aktarımı Ekleme - Azure AD tümleştirmesi

MOVEit Aktarımı - Azure AD tümleştirmesini Azure AD'ye yapılandırmak için galeriden MOVEit Transfer - Azure AD tümleştirmesini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden MOVEit Aktarımı - Azure AD tümleştirmesini eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **MOVEit Aktarım - Azure AD tümleştirmesini**yazın, sonuç panelinden **MOVEit Aktarımı - Azure AD tümleştirmesini** seçin ve ardından uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![MOVEit Aktarımı - Sonuç listesinde Azure AD tümleştirmesi](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına dayalı Olarak Azure AD oturumunu MOVEit Transfer - Azure AD tümleştirmesi ile yapılandırıp test edeyimsiniz.
Tek oturum açma nın işe yaraması için, Bir Azure REKLAM kullanıcısı ile MOVEit Aktarımı - Azure AD tümleştirmesinde ilgili kullanıcı arasında bir bağlantı ilişkisi kurulması gerekir.

MOVEit Transfer - Azure AD tümleştirmesi ile Azure AD oturumunu yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[MOVEit Transferini - Azure AD tümleştirmesi Tek Oturum](#configure-moveit-transfer---azure-ad-integration-single-sign-on)** Açma - yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. MOVEit Aktarımı ' nda Britta Simon' un muadili olması için **[MOVEit Aktarımı - Azure AD tümleştirmesi](#create-moveit-transfer---azure-ad-integration-test-user)** - Kullanıcının Azure AD gösterimine bağlı Azure AD tümleştirmesini oluşturun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

MOVEit Transfer - Azure AD tümleştirmesi ile Azure AD oturumunu yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/), **MOVEit Aktarım - Azure AD tümleştirme** uygulaması tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız**varsa aşağıdaki adımları gerçekleştirin:

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![Meta veri dosyalarını yükleme](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![meta veri dosyasini seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değeri Temel **SAML Yapılandırması** bölümünde otomatik olarak doldurulur:

    ![MOVEit Aktarımı - Azure AD tümleştirme Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://contoso.com`

    > [!NOTE]
    > **Oturum açma URL** değeri gerçek değildir. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [MOVEit Transfer - Azure AD tümleştirme Istemci destek](https://community.ipswitch.com/s/support) ekibine başvurun. Hizmet Sağlayıcı **Meta veri dosyasını,** öğreticinin **Yapılandırma MOVEit Aktarımı - Azure AD tümleştirmesi Tek Oturum** Açma bölümünde daha sonra açıklanan Hizmet Sağlayıcı **Metaveri URL'sinden** indirebilirsiniz. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

6. **MOVEit Aktarım** - Azure AD tümleştirmesi bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-moveit-transfer---azure-ad-integration-single-sign-on"></a>MOVEit Transferini Yapılandırın - Azure AD tümleştirmesi Tek Oturum Açma

1. Yönetici olarak MOVEit Transfer kiracınıza oturum açın.

2. Soldaki gezinti bölmesine **Ayarlar'ı**tıklatın.

    ![Uygulama tarafında Ayarlar Bölümü](./media/moveittransfer-tutorial/tutorial_moveittransfer_000.png)

3. **Güvenlik İlkeleri -> Kullanıcı Auth**altında **Tek Signon** bağlantısını tıklatın.

    ![Uygulama tarafında Güvenlik İlkeleri](./media/moveittransfer-tutorial/tutorial_moveittransfer_001.png)

4. Meta veri belgesini indirmek için Meta veri URL bağlantısını tıklatın.

    ![Servis Sağlayıcı Metaveri URL'si](./media/moveittransfer-tutorial/tutorial_moveittransfer_002.png)
    
   * **EntityID'nin** Temel **SAML Yapılandırması** **bölümündeki Tanımlayıcıyla** eşleştiğini doğrulayın.
   * **İddiayı DoğrulaConsumerService** Konum URL' **si, Temel SAML Yapılandırması** **bölümündeki YANıT** URL'si ile eşleşir.
    
     ![Uygulama tarafında Tek Oturum Açma'yı yapılandırın](./media/moveittransfer-tutorial/tutorial_moveittransfer_007.png)

5. Yeni bir Federe Kimlik Sağlayıcısı eklemek için **Kimlik Sağlayıcı Ekle** düğmesini tıklatın.

    ![Kimlik Sağlayıcı Ekle](./media/moveittransfer-tutorial/tutorial_moveittransfer_003.png)

6. Azure portalından indirdiğiniz meta veri dosyasını seçmek için **Gözat...'ı** tıklatın ve ardından indirilen dosyayı yüklemek için **Kimlik Sağlayıcı Ekle'yi** tıklatın.

    ![SAML Kimlik Sağlayıcısı](./media/moveittransfer-tutorial/tutorial_moveittransfer_004.png)

7. **Federe Kimlik Sağlayıcı Ayarlarını Edit'** te **Etkinleştirilen** "**Evet"** sayfasını seçin ve **Kaydet'i**tıklatın.

    ![Federe Kimlik Sağlayıcı Ayarları](./media/moveittransfer-tutorial/tutorial_moveittransfer_005.png)

8. **Federe Kimlik Sağlayıcısı Kullanıcı Ayarlarını Edit** sayfasında aşağıdaki işlemleri gerçekleştirin:
    
    ![Federe Kimlik Sağlayıcı Ayarlarını Değiştir](./media/moveittransfer-tutorial/tutorial_moveittransfer_006.png)
    
    a. **Giriş adı**olarak **SAML NameID'yi** seçin.
    
    b. **Diğer'i Tam ad** olarak seçin ve **Öznitelik** `http://schemas.microsoft.com/identity/claims/displayname`adı textbox'ına değer koyun: . **Other**
    
    c. **E-posta** olarak **Diğer'i** seçin ve **Öznitelik adı** textbox değeri koymak: `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`.
    
    d. **Oturum açmada Otomatik Oluştur hesabı**olarak **Evet'i** seçin.
    
    e. **Kaydet** düğmesini tıklatın.

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

Bu bölümde, Britta Simon'ın MOVEit Aktarımı - Azure AD tümleştirmesine erişim sağlayarak Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin, ardından **MOVEit Aktarımı - Azure AD tümleştirmesini**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **MOVEit Aktarımı - Azure AD tümleştirmesini**seçin.

    ![Uygulamalar listesindeki MOVEit Aktarımı - Azure AD tümleştirme bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-moveit-transfer---azure-ad-integration-test-user"></a>MOVEit Aktarım Oluşturma - Azure AD tümleştirme testi kullanıcısı

Bu bölümün amacı, MOVEit Aktarımı - Azure AD tümleştirmesinde Britta Simon adında bir kullanıcı oluşturmaktır. MOVEit Aktarımı - Azure AD tümleştirmesi, etkinleştirdiğiniz tam zamanında sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. MOVEit Aktarım - Azure AD tümleştirmesine henüz erişme girişiminde yeni bir kullanıcı oluşturulur.

>[!NOTE]
>Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [MOVEit Aktarımı - Azure AD tümleştirme istemci destek ekibine](https://community.ipswitch.com/s/support)başvurmanız gerekir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki MOVEit Aktarım - Azure AD tümleştirme döşemesini tıklattığınızda, SSO'yu ayarladığınız MOVEit Aktarımı - Azure AD tümleştirmesinde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

