---
title: 'Öğretici: InstaVR Görüntüleyici ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve InstaVR Viewer arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.reviewer: barbkess
ms.assetid: 13ffa29f-d0a5-4b21-b296-cfd76f380940
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 01/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8de94f83c260a86f313a2dd04cdd5a7ae8fc1cda
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73155319"
---
# <a name="tutorial-azure-active-directory-integration-with-instavr-viewer"></a>Öğretici: InstaVR Görüntüleyici ile Azure Active Directory entegrasyonu

Bu eğitimde, InstaVR Görüntüleyici'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz.
InstaVR Görüntüleyici'yi Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* InstaVR Görüntüleyici'ye erişimi olan Azure AD'da kontrol edebilirsiniz.
* Kullanıcılarınızın Azure AD hesaplarıyla InstaVR Görüntüleyici 'de (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini InstaVR Görüntüleyici ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* InstaVR Viewer tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* InstaVR Viewer **SP** başlatılan SSO destekler
* InstaVR Viewer **Just In Time** kullanıcı sağlama destekler

## <a name="adding-instavr-viewer-from-the-gallery"></a>Galeriden InstaVR Görüntüleyici Ekleme

InstaVR Viewer'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize InstaVR Görüntüleyici eklemeniz gerekir.

**Galeriden InstaVR Görüntüleyici eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda, **InstaVR Viewer**yazın, sonuç panelinden **InstaVR Görüntüleyici'yi** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde InstaVR Görüntüleyici](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, **Britta Simon**adlı bir test kullanıcısına göre Azure AD tek oturum açma işlemini InstaVR Viewer ile yapılandırıp test esinizsiniz.
Tek oturum açmanın çalışabilmesi için, Bir Azure AD kullanıcısı ile InstaVR Viewer'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçmayı InstaVR Viewer ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[InstaVR Viewer Tek Oturum](#configure-instavr-viewer-single-sign-on)** Açma'yı yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[InstaVR Görüntüleyici test kullanıcısını oluşturun](#create-instavr-viewer-test-user)** - Kullanıcının Azure AD gösterimine bağlı InstaVR Görüntüleyici'de Britta Simon'ın bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini InstaVR Görüntüleyici ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **InstaVR Viewer** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![InstaVR Görüntüleyici Etki Alanı ve URL'ler tek oturum açma bilgileri](common/sp-identifier.png)

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://console.instavr.co/auth/saml/login/<WEBPackagedURL>`

    > [!NOTE]
    > URL'de İşaret için sabit bir desen yoktur. InstaVR Viewer müşterisi web ambalajı yaptığında oluşturulur. Her müşteri ve paket için benzersizdir. URL'deki tam İşaret'i almak için InstaVR Görüntüleyici örneğinize giriş yapmanız ve web ambalajı yapmanız gerekir.

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://console.instavr.co/auth/saml/sp/<WEBPackagedURL>`

    > [!NOTE]
    > Tanımlayıcı değeri gerçek değildir. Bu değeri, daha sonra bu öğreticide açıklanan gerçek Tanımlayıcı değeriyle güncelleştirin.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** ve **Federasyon Meta Veri Dosyasını** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadata-certificatebase64.png)

6. **InstaVR Görüntüleyici'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure Reklam Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-instavr-viewer-single-sign-on"></a>InstaVR Görüntüleyici tek sign-on yapılandırma

1. Yeni bir web tarayıcısı penceresi açın ve yönetici olarak InstaVR Viewer şirket sitenize giriş yapın.

2. Kullanıcı **Simgesi'ne** tıklayın ve **Hesap'ı**seçin.

    ![InstaVR Görüntüleyici yapılandırması](media/instavr-viewer-tutorial/tutorial-instavr-viewer-account.png)

3. **SAML Auth'a** gidin ve aşağıdaki adımları gerçekleştirin:

    ![InstaVR Görüntüleyici yapılandırması](media/instavr-viewer-tutorial/tutorial-instavr-viewer-configure.png)

    a. **SSO URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    b. Oturum **Açma URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    c. Varlık **Kimliği** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure Reklam Tanımlayıcı** değerini yapıştırın.

    d. İndirilen Sertifika dosyanızı yüklemek için **Güncelleştir'i**tıklatın.

    e. İndirilen Federasyon Meta veri dosyanızı yüklemek için **Güncelleştir'i**tıklatın.

    f. Varlık **Kimliği** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı (Entity ID)** metin kutusuna yapıştırın.

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

Bu bölümde, Britta Simon'ın InstaVR Viewer'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **InstaVR Görüntüleyici'yi**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **InstaVR Viewer**yazın ve seçin.

    ![Uygulamalar listesindeki InstaVR Görüntüleyici bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-instavr-viewer-test-user"></a>InstaVR Görüntüleyici test kullanıcısı oluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı InstaVR Görüntüleyici oluşturulur. InstaVR Viewer, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. InstaVR Görüntüleyici'de bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur. Herhangi bir sorunla karşılaşıyorsanız, lütfen [InstaVR Viewer destek ekibiyle](mailto:contact@instavr.co)irtibata geçiniz.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

1. Yeni bir web tarayıcısı penceresi açın ve yönetici olarak InstaVR Viewer şirket sitenize giriş yapın.

2. Sol daki gezinti panelinden **Paket'i** seçin ve **Web için paket yap'ı**seçin.

    ![InstaVR Görüntüleyici yapılandırması](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing1.png)

3. **Download** (İndir) seçeneğini belirleyin.

    ![InstaVR Görüntüleyici yapılandırması](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing2.png)

4. Daha sonra **Açık Barındırılan Sayfa'yı** seçin ve oturum açmak için Azure AD'ye yönlendirilir.

    ![InstaVR Görüntüleyici yapılandırması](media/instavr-viewer-tutorial/tutorial-instavr-viewer-testing3.png)

5. SSO aracılığıyla Azure AD'ye başarılı bir şekilde giriş yapmak için Azure AD kimlik bilgilerinizi girin.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)
