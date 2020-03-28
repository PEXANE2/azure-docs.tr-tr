---
title: 'Öğretici: Citrix ShareFile ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Citrix ShareFile arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: e14fc310-bac4-4f09-99ef-87e5c77288b6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/24/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 71d2fe97a4af07f48038ffd88de648247b56bb80
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73160021"
---
# <a name="tutorial-azure-active-directory-integration-with-citrix-sharefile"></a>Öğretici: Citrix ShareFile ile Azure Active Directory entegrasyonu

Bu eğitimde, Citrix ShareFile'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
Citrix ShareFile'ı Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Azure AD'de Citrix ShareFile'a erişimi olan kişiler kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla Citrix ShareFile 'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Citrix ShareFile ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Citrix ShareFile tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* Citrix **ShareFile, SP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-citrix-sharefile-from-the-gallery"></a>Galeriden Citrix ShareFile ekleme

Citrix ShareFile'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Citrix ShareFile eklemeniz gerekir.

**Galeriden Citrix ShareFile eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **Citrix ShareFile**yazın, sonuç panelinden **Citrix ShareFile'ı** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Ki Citrix ShareFile](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre Citrix ShareFile ile yapılandırıp test esinizsiniz.
Tek oturum açmanın işe yaraması için, Bir Azure AD kullanıcısı ile Citrix ShareFile'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı Citrix ShareFile ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Citrix ShareFile Tek Oturum Açma](#configure-citrix-sharefile-single-sign-on)** 'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Citrix ShareFile test kullanıcısını oluşturun](#create-citrix-sharefile-test-user)** - Kullanıcının Azure AD gösterimine bağlı Citrix ShareFile'da Britta Simon'ın bir muadili olsun.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini Citrix ShareFile ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **Citrix ShareFile** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Citrix ShareFile Domain ve URL'ler tek oturum açma bilgileri](common/sp-identifier-reply.png)

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<tenant-name>.sharefile.com/saml/login`

    b. Tanımlayıcı **(Entity ID)** textbox'ında, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |---|
    | `https://<tenant-name>.sharefile.com`|
    | `https://<tenant-name>.sharefile.com/saml/info`|
    | `https://<tenant-name>.sharefile1.com/saml/info`|
    | `https://<tenant-name>.sharefile1.eu/saml/info`|
    | `https://<tenant-name>.sharefile.eu/saml/info`|
    | |
    
    c. **Yanıtla URL** metin kutusunda, aşağıdaki deseni kullanarak bir URL yazın:
    
    | |
    |---|
    | `https://<tenant-name>.sharefile.com/saml/acs`|
    | `https://<tenant-name>.sharefile.eu/saml/<URL path>`|
    | `https://<tenant-name>.sharefile.com/saml/<URL path>`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [Citrix ShareFile Client destek ekibine](https://www.citrix.co.in/products/citrix-content-collaboration/support.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Citrix ShareFile'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-citrix-sharefile-single-sign-on"></a>Citrix ShareFile Tek Oturum Açma'yı Yapılandır

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak **Citrix ShareFile** şirket sitenize giriş yapın.

2. Üstteki araç çubuğunda **Yönetici'yi**tıklatın.

3. Sol gezinti bölmesinde, **Tek İşaretle Yapıla'yı**seçin.
   
    ![Hesap Yönetimi](./media/sharefile-tutorial/ic773627.png "Hesap Yönetimi")

4. **Temel Ayarlar**altındaki **Tek Oturum Açma/ SAML 2.0 Yapılandırma** iletişim sayfasında aşağıdaki adımları gerçekleştirin:
   
    ![Çoklu oturum açma](./media/sharefile-tutorial/ic773628.png "Çoklu oturum açma")
   
    a. **SAML'yi etkinleştir'i**tıklatın.
    
    b. **IDP Veren/ Entity ID textbox'ınıza,** Azure portalından kopyalamış olduğunuz **Azure Reklam Tanımlayıcısı** değerini yapıştırın.

    c. **X.509 Sertifikası** alanının yanındaki **Değiştir'i** tıklatın ve ardından Azure portalından indirdiğiniz sertifikayı yükleyin.
    
    d. **Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin** değerini yapıştırın.
    
    e. **Logout URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

5. Citrix ShareFile yönetim portalına **kaydet'i** tıklatın.

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

Bu bölümde, Britta Simon'ın Citrix ShareFile'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve **ardından Citrix ShareFile'ı**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Citrix ShareFile**yazın ve seçin.

    ![Uygulamalar listesindeki Citrix ShareFile bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-citrix-sharefile-test-user"></a>Citrix ShareFile test kullanıcıoluşturma

Azure AD kullanıcılarının Citrix ShareFile'a giriş yapabilmeleri için Citrix ShareFile'da bunların sağlanması gerekir. Citrix ShareFile durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Citrix ShareFile** kiracınıza giriş yapın.

2. **Kullanıcıları Yönet \> Kullanıcı \> Larını Yönet Ana Sayfa + Çalışan Oluştur'u**tıklatın.
   
    ![Çalışan Oluşturma](./media/sharefile-tutorial/IC781050.png "Çalışan Oluşturma")

3. Temel **Bilgiler** bölümünde aşağıdaki adımları gerçekleştirin:
   
    ![Temel Bilgiler](./media/sharefile-tutorial/IC799951.png "Temel Bilgiler")
   
    a. **E-posta Adresi** metin kutusuna Brittasimon'ın e-posta adresini **\@brittasimon contoso.com**olarak yazın.
   
    b. Ad **metin** kutusunda, **britta**olarak kullanıcının **ilk adını** yazın.
   
    c. **Soyadı** metin kutusunda, kullanıcının **soyadını** **Simon**olarak yazın.

4. **Kullanıcı Ekle**'ye tıklayın.
  
    >[!NOTE]
    >Azure AD hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantıyı izler. Azure AD kullanıcı hesaplarını sağlamak için Citrix ShareFile tarafından sağlanan diğer Citrix ShareFile kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Citrix ShareFile döşemesini tıklattığınızda, SSO'yu kurduğunuz Citrix ShareFile'da otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

