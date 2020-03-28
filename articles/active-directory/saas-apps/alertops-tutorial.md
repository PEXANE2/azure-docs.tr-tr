---
title: 'Öğretici: AlertOps ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve AlertOps arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 3db13ed4-35c2-4b1e-bed8-9b5977061f93
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/20/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 21b8cb06712e370972e0b8fec518c37d078262e0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "67107067"
---
# <a name="tutorial-integrate-alertops-with-azure-active-directory"></a>Öğretici: AlertOps'ları Azure Active Directory ile tümleştirin

Bu eğitimde, AlertOps'leri Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. AlertOps'leri Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de AlertOps erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla AlertOps'ta otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* AlertOps tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. **AlertOps, SP ve IDP** tarafından başlatılan SSO'ları destekler.

## <a name="adding-alertops-from-the-gallery"></a>Galeriden AlertOps ekleme

AlertOps'lerin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize AlertOps eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **AlertOps** yazın.
1. Sonuç panelinden **AlertOps'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **Britta Simon**adlı bir test kullanıcısı kullanarak AlertOps ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile AlertOps'taki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu AlertOps ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. Uygulama tarafındaki SSO ayarlarını yapılandırmak için **[AlertOps'u yapılandırın.](#configure-alertops)**
3. Britta Simon ile Azure AD tek oturum açma işlemini test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. AlertOps'ta Britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için **[AlertOps test kullanıcısı oluşturun.](#create-alertops-test-user)**
6. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **AlertOps** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    1. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.alertops.com`

    1. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.alertops.com/login.aspx`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.alertops.com/login.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [AlertOps İstemci destek ekibine](mailto:support@alertops.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **AlertOps'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

   ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-alertops"></a>AlertOps'u Yapılandır

1. AlertOps içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyi'ni tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Setup AlertOps'a** tıklayın ve sizi AlertOps uygulamasına yönlendirecektir. Buradan AlertOps'ta oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. AlertOps'u el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve AlertOps şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sol daki navigasyon panelinden **Hesap ayarlarına** tıklayın.

    ![AlertOps yapılandırması](./media/alertops-tutorial/configure1.png)

5. Abonelik **Ayarları** sayfasında **SSO'yu** seçin ve aşağıdaki adımları gerçekleştirin:

    ![AlertOps yapılandırması](./media/alertops-tutorial/configure2.png)

    a. **Tek Oturum Açma(SSO)** onay kutusunu kullan'ı seçin.

    b. Açılan dosyadan **SSO Sağlayıcısı** olarak **Azure Active Directory'yi** seçin.

    c. Veren **URL** metin kutusunda, Azure portalındaki **Temel SAML Yapılandırması** bölümünde kullandığınız tanımlayıcı değerini kullanın.

    d. **SAML bitiş noktası URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    e. **SLO bitiş noktası URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    f. Açılan dosyadan **SAML İmza Algoritması** olarak **SHA256'yı** seçin.

    g. İndirilen Sertifika(Base64) dosyanızı Notepad'de açın. İçeriğini panonuza kopyalayın ve Ardından X.509 Sertifika metin kutusuna yapıştırın.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın AlertOps'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **AlertOps'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-alertops-test-user"></a>AlertOps test kullanıcıoluşturma

1. Farklı bir tarayıcı penceresinde, AlertOps şirket sitenizde yönetici olarak oturum açın.

2. Sol navigasyon panelinden **Kullanıcılar'a** tıklayın.

    ![AlertOps yapılandırması](./media/alertops-tutorial/user1.png)

3. **Kullanıcı Ekle'yi**seçin.

    ![AlertOps yapılandırması](./media/alertops-tutorial/user2.png)

4. Kullanıcı **Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![AlertOps yapılandırması](./media/alertops-tutorial/user3.png)

    a. Giriş **Kullanıcı Adı** metin **kutusuna Brittasimon**gibi kullanıcının kullanıcı adını girin.

    b. Resmi **E-posta** textbox, **Brittasimon\@contoso.com**gibi kullanıcının e-posta adresini girin.

    c. Ad **metin** **kutusuna, Britta**gibi kullanıcının ilk adını girin.

    d. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının ilk adını girin.

    e. Kuruluşa göre açılır bırakma türü **değerini** seçin.

    f. Kuruluşunuzun başına açılır bırakma dan kullanıcının **Rolünü** seçin.

    g. **Ekle'yi**seçin.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki AlertOps döşemesini seçtiğinizde, SSO'yu kurduğunuz AlertOps'ta otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)