---
title: 'Öğretici: Way We Do ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Yaptığımız Yol arasında tek oturum açma yı nasıl yapılandıracağız öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 84fc4f36-ecd1-42c6-8a70-cb0f3dc15655
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: faa23f61e5a213c492a7fb51bfc5b108e5c77946
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67310399"
---
# <a name="tutorial-integrate-way-we-do-with-azure-active-directory"></a>Öğretici: Azure Active Directory ile Yaptığımız Tümleştirme

Bu eğitimde, Azure Etkin Dizini (Azure AD) ile Yaptığımız Yol'u nasıl entegre edeceğinizi öğreneceksiniz. Azure REKLAM ile Yaptığımız Yolu entegre ettiğinizde şunları yapabilirsiniz:

* Azure AD'de, Yaptığımız Yol'a erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla yaptığımız şekilde otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* Way We Do tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Way We Do **SP** başlatılan SSO destekler
* Yaptığımız Yol **Just In Time** kullanıcı sağlama destekler

## <a name="adding-way-we-do-from-the-gallery"></a>Galeriden Yaptığımız Yol Ekleme

Yaptığımız Yol'un Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yaptığımız Yolu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Yaptığımız Şekilde** yazın.
1. Sonuç panelinden **Yaptığımız Yol'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Yaptığımız Şekilde yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında Yaptığımız Şekilde bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Yaptığımız Yol ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Yapılandırma ŞekliBiz SSO](#configure-way-we-do-sso)** - uygulama tarafında Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Create Way We Do test kullanıcısı](#create-way-we-do-test-user)** - Britta Simon'ın bir örneğine sahip olmak için, kullanıcının Azure AD gösterimine bağlı olan Biz Yaptığımız Şekilde.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Uygulama **tümleştirmesi** yapma yolundaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.waywedo.com/Authentication/ExternalSignIn`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.waywedo.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Müşteri destek ekibine](mailto:support@waywedo.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **(Ham)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificateraw.png)

1. Yaptığımız **Şekilde Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-way-we-do-sso"></a>SSO'ya Yapıla

1. Yapılandırmayı Yaptığımız Şekilde otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **Kurulum Şekli'ni** tıklayın ve sizi Yaptığımız Yol uygulamasına yönlendirecektir. Buradan, Way We Do'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. El ile Yaptığımız Şekilde kurulum yapmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve bir yönetici olarak İş yapıyoruz şirket sitenizde oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Yol Yaptığımız şekilde herhangi bir sayfanın sağ üst köşesindeki **kişi simgesini** tıklatın ve ardından açılan menüde **Hesap'ı** tıklatın.

    ![Hesap Yapma Şeklimiz](./media/waywedo-tutorial/tutorial_waywedo_account.png)

1. Push gezinti menüsünü açmak için **menü simgesine** tıklayın ve **Tek İşaret Aç'a**tıklayın.

    ![Way We Do single](./media/waywedo-tutorial/tutorial_waywedo_single.png)

1. Tek **oturum açma kurulum** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Kaydettiğimiz Yol](./media/waywedo-tutorial/tutorial_waywedo_save.png)

    a. Tek **Oturum Açma'yı** etkinleştirmek için Tek Oturum Açma'yı **Evet'e** çevir'i tıklatın.

    b. Tek **oturum açma adı** textbox'ına adınızı girin.

    c. Entity **ID** textbox'a, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    d. **SAML SSO URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    e. **Sertifika'nın**yanındaki **seç** düğmesini tıklatarak sertifikayı yükleyin.

    f. **İsteğe Bağlı Ayarlar** -
    
    * Parolaları Etkinleştir - Bu seçenek devre dışı bırakıldığında, kullanıcıların yalnızca tek oturum açma kullanabilmeleri için Yaptığımız Yol için normal parola işlevleri çalışır.

    * Otomatik sağlamayı etkinleştirin - Bu etkinleştirildiğinde, oturum açma için kullanılan e-posta adresi otomatik olarak Yaptığımız Şekildeki kullanıcı listesiyle karşılaştırılır. E-posta adresi Bizim Yaptığımız Şekilde etkin bir kullanıcıyla eşleşmiyorsa, oturum imzalayan kişi için otomatik olarak yeni bir kullanıcı hesabı ekler ve eksik bilgiler ister.

      > [!NOTE]
      > Tek oturum açma yoluyla eklenen kullanıcılar genel kullanıcı olarak eklenir ve sistemde bir rol atanır. Yönetici, bir düzenleyici veya yönetici olarak güvenlik rollerini değiştirebilir ve bir veya birkaç Org Grafiği rolü atayabilir.

    g. Ayarlarınızı devam ettirebilmek için **Kaydet'i** tıklatın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, B.Simon'ın Way We Do'ya erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, Yaptığımız **Yol'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-way-we-do-test-user"></a>Test kullanıcımızı oluşturma şekli

Bu bölümde, Britta Simon adlı bir kullanıcı Way We Do oluşturulur. Yaptığımız Yol, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı Bizim Yaptığımız Şekilde zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!Note]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Müşteri Miz destek ekibine](mailto:support@waywedo.com)başvurun.

### <a name="test-sso"></a>Test SSO

Erişim Paneli'nde Yaptığımız Yol'u seçtiğinizde, SSO'yu kurduğunuz Şekilde otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)