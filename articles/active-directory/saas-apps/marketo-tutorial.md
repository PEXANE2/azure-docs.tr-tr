---
title: 'Öğretici: Marketo ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Marketo arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b88c45f5-d288-4717-835c-ca965add8735
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 02/19/2019
ms.author: jeedes
ms.openlocfilehash: 0488fd1e9bc10d61d6660745acfc8c39becf3a89
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73159467"
---
# <a name="tutorial-azure-active-directory-integration-with-marketo"></a>Öğretici: Marketo ile Azure Active Directory entegrasyonu

Bu eğitimde, Marketo'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğrenirsiniz.
Marketo'yu Azure AD ile tümleştirmek size aşağıdaki avantajları sağlar:

* Marketo erişimi olan Azure AD'de denetim yapabilirsiniz.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Marketo'da (Tek Oturum Açma) otomatik olarak oturum açmalarını sağlayabilirsiniz.
* Hesaplarınızı tek bir merkezi konumda yönetebilirsiniz - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi almak istiyorsanız, [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Marketo ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Azure REKLAM ortamınız yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/) bir aylık deneme sürümü alabilirsiniz
* Marketo tek oturum açma özellikli abonelik

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD tek oturum açma işlemlerini bir test ortamında yapılandırıp sınayabilirsiniz.

* **Marketo, IDP'nin** Başlattığı SSO'ya destek verdi

## <a name="adding-marketo-from-the-gallery"></a>Galeriden Marketo ekleme

Marketo'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Marketo'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

**Galeriden Marketo eklemek için aşağıdaki adımları gerçekleştirin:**

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

    ![Azure Etkin Dizin düğmesi](common/select-azuread.png)

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

    ![Enterprise uygulamaları bıçak](common/enterprise-applications.png)

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

    ![Yeni uygulama düğmesi](common/add-new-app.png)

4. Arama kutusunda **Marketo**yazın, sonuç panelinden **Marketo'yu** seçin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

     ![Sonuç listesinde Marketo](common/search-new-app.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Bu bölümde, Azure AD tek oturum açma işlemini **Britta Simon**adlı bir test kullanıcısına göre yapılandırıp pazarlayın test emzebilirsiniz.
Tek oturum açmanın işe yaraması için, bir Azure AD kullanıcısı ile Marketo'daki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure AD oturumaçma işlemlerini Marketo ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[Marketo Tek Oturum](#configure-marketo-single-sign-on)** Açma'yı yapılandırır - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Marketo test kullanıcısını oluşturun](#create-marketo-test-user)** - Marketo'da Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Marketo ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. **Marketo** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. **SAML sayfasıyla Tek Oturum Açma'da** aşağıdaki adımları gerçekleştirin:

    ![Marketo Domain ve URL'ler tek oturum açma bilgileri](common/idp-intiated.png)

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://saml.marketo.com/sp`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://login.marketo.com/saml/assertion/\<munchkinid\>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [Marketo İstemci destek ekibine](https://investors.marketo.com/contactus.cfm) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

6. **Marketo'yu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

### <a name="configure-marketo-single-sign-on"></a>Marketo Tek Oturum Aç'ı Yapılandır

1. Uygulamanızın Munchkin Kimliğini almak için, yönetici kimlik bilgilerini kullanarak Marketo'ya giriş yapın ve aşağıdaki işlemleri gerçekleştirin:
   
    a. Yönetici kimlik bilgilerini kullanarak Marketo uygulamasına giriş yapın.
   
    b. Üst gezinti bölmesindeki **Yönetici** düğmesini tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve **Munchkin bağlantısını**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_11.png)
   
    d. Ekranda gösterilen Munchkin Id'yi kopyalayın ve Yanıt URL'nizi Azure AD yapılandırma sihirbazında tamamlayın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_12.png) 

2. Uygulamadaki SSO'yu yapılandırmak için aşağıdaki adımları izleyin:
   
    a. Yönetici kimlik bilgilerini kullanarak Marketo uygulamasına giriş yapın.
   
    b. Üst gezinti bölmesindeki **Yönetici** düğmesini tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. Tümleştirme menüsüne gidin ve **Tek İşaret Açık'ı**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_07.png) 
   
    d. SAML Ayarlarını etkinleştirmek **için, Düzelt** düğmesini tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_08.png) 
   
    e. **Etkin** Tek Oturum Açma ayarları.
   
    f. **Azure AD Tanımlayıcısını,** **Veren Kimliği** metin kutusuna yapıştırın.
   
    g. Varlık **Kimliği** metin kutusuna URL'yi ' olarak `http://saml.marketo.com/sp`girin.
   
    h. **Ad Tanımlayıcı öğesi**olarak Kullanıcı Kimliği Konumunu seçin.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_09.png)
   
    > [!NOTE]
    > Kullanıcı Tanımlayıcınız UPN değeri değilse, Öznitelik sekmesindeki değeri değiştirin.
   
    i. Azure AD yapılandırma sihirbazından indirdiğiniz sertifikayı yükleyin. Ayarları **kaydedin.**
   
    j. Sayfaları Yeniden Yönlendir ayarlarını değiştirin.
   
    k. **Giriş** URL'si metin kutusuna **Giriş URL'sini** yapıştırın.
   
    l. **Logout URL'sini Logout** **URL** metin kutusuna yapıştırın.
   
    m. Hata **URL'sinde** **Marketo örnek URL'nizi** kopyalayın ve ayarları kaydetmek için **Kaydet** düğmesini tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_10.png)

3. Kullanıcılar için SSO'yu etkinleştirmek için aşağıdaki eylemleri tamamlayın:
   
    a. Yönetici kimlik bilgilerini kullanarak Marketo uygulamasına giriş yapın.
   
    b. Üst gezinti bölmesindeki **Yönetici** düğmesini tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_06.png) 
   
    c. **Güvenlik** menüsüne gidin ve **Giriş Ayarları'nı**tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_13.png)
   
    d. **SSO'yu gerek** tir seçeneğini kontrol edin ve ayarları **kaydedin.**
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_14.png)

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

Bu bölümde, Britta Simon'ın Marketo'ya erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından **Marketo'yu**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde **Marketo'yu**seçin.

    ![Uygulamalar listesindeki Marketo bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle** düğmesini tıklatın ve ardından **Atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar'ı** seçin.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinde **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

6. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

7. Atama **Ekle** iletişim kutusunda **Atla** düğmesini tıklatın.

### <a name="create-marketo-test-user"></a>Marketo test kullanıcısı oluşturma

Bu bölümde Marketo'da Britta Simon adında bir kullanıcı oluşturursunuz. Marketo platformunda bir kullanıcı oluşturmak için aşağıdaki adımları izleyin.

1. Yönetici kimlik bilgilerini kullanarak Marketo uygulamasına giriş yapın.

2. Üst gezinti bölmesindeki **Yönetici** düğmesini tıklatın.
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_06.png) 

3. **Güvenlik** menüsüne gidin ve **Kullanıcılar & Rolleri** tıklatın
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_19.png)  

4. Kullanıcılar sekmesinde **Yeni Kullanıcı Davet et** bağlantısını tıklayın
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_15.png) 

5. Yeni Kullanıcı Davet sihirbazında aşağıdaki bilgileri doldurun
   
    a. Kullanıcı **E-posta** adresini textbox'a girin
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_16.png)
   
    b. Metin kutusuna **Ad Girin**
   
    c. Metin kutusuna **Soyadı** nı girin
   
    d. **İleri'yi** tıklatın

6. **İzinler** **sekmesinde, userRoles'u** seçin ve **İleri'yi** tıklatın
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_17.png)
7. Kullanıcı davetini göndermek için **Gönder** düğmesini tıklatın
   
    ![Tek İşaret-On'u Yapılandır](./media/marketo-tutorial/tutorial_marketo_18.png)

8. Kullanıcı e-posta bildirimini alır ve bağlantıyı tıklatmak ve hesabı etkinleştirmek için parolayı değiştirmek zorundadır. 

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Marketo döşemesini tıklattığınızda, SSO'yu kurduğunuz Marketo'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

