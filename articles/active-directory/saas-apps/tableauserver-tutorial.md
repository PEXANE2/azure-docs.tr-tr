---
title: 'Öğretici: Tableau Server ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Tableau Server arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c1917375-08aa-445c-a444-e22e23fa19e0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: d19a7cc8d81f9e6e913f147b24c5cce03ff82027
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76986742"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-tableau-server"></a>Öğretici: Tableau Server ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Tableau Server'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Tableau Server'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Tableau Server erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Tableau Server'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Tableau Server tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Tableau Server **SP** başlatılan SSO destekler
* Tableau Server'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

## <a name="adding-tableau-server-from-the-gallery"></a>Galeriden Tableau Server ekleme

Tableau Server'ın Azure AD'ye entegrasyonunu yapılandırmak için, tableau Server'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Tableau Server** yazın.
1. Sonuç panelinden **Tableau Server'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-tableau-server"></a>Tableau Server için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'yu Tablo Server ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Tableau Server'daki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Tableau Server ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Tableau Server SSO'yu yapılandırın.](#configure-tableau-server-sso)**
    1. **[Tableau Server test kullanıcısını oluşturun](#create-tableau-server-test-user)** - Tableau Server'da kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Tableau Server** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://azure.<domain name>.link`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://azure.<domain name>.link`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://azure.<domain name>.link/wg/saml/SSO/index.html`

    > [!NOTE]
    > Önceki değerler gerçek değerler değildir. Değerleri, öğreticide daha sonra açıklanan Tableau Server yapılandırma sayfasından gerçek URL ve tanımlayıcıyla güncelleştirin.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Tableau Server'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

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

Bu bölümde, B.Simon'ın Tableau Server'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Tableau Server'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-tableau-server-sso"></a>Tableau Server SSO'yu yapılandır

1. SSO'nun uygulamanız için yapılandırılmak için Tableau Server kiracınızda yönetici olarak oturum açmanız gerekir.

2. **CONFIGURATION** sekmesinde, **Kullanıcı Kimliği & Access'i**seçin ve ardından **Kimlik Doğrulama** Yöntemi sekmesini seçin.

    ![Tek İşaret-On'u Yapılandır](./media/tableauserver-tutorial/tutorial-tableauserver-auth.png)

3. **CONFIGURATION** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/tableauserver-tutorial/tutorial-tableauserver-config.png)

    a. **Kimlik Doğrulama Yöntemi**için SAML'yi seçin.

    b. **Sunucu için SAML Kimlik Doğrulamasını Etkinleştir**onay kutusunu seçin.

    c. Tableau Server return URL—Tableau Server kullanıcılarının erişeceği <http://tableau_server>URL, örneğin. Kullanılması `http://localhost` önerilmez. Sonda eğik çizgi içeren bir `http://tableau_server/`URL kullanmak (örneğin, ) desteklenmez. **Tableau Server return URL'sini** kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümünde **URL** textbox'a yapıştırın

    d. SAML entity ID—Entity ID, Tableau Server yüklemenizi Benzersiz olarak IdP'ye tanımlar. İsterseniz Tableau Server URL'nizi buraya tekrar girebilirsiniz, ancak tableau Server URL'niz olması gerekmez. **SAML entity id'i** kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı** textbox'a yapıştırın

    e. **XML Meta veri dosyasını indir'i** tıklatın ve metin düzenleyicisi uygulamasında açın. İddia Tüketici Hizmeti URL'sini Http Post ve Index 0 ile bulun ve URL'yi kopyalayın. Azure portalındaki **Temel SAML Yapılandırması** bölümündeki Url textbox'a yapıştırın **Reply URL**

    f. Azure portalından indirilen Federasyon Meta veri dosyanızı bulun ve ardından **SAML Idp meta veri dosyasına**yükleyin.

    g. IdP'nin kullanıcı adlarını, görüntüleme adlarını ve e-posta adreslerini tutmak için kullandığı özniteliklerin adlarını girin.

    h. **Kaydet'i** tıklatın

    > [!NOTE]
    > Müşteri Tableau Server SAML SSO yapılandırmasında herhangi bir sertifika yüklemek zorunda ve SSO akışında göz ardı alırsınız. Tableau Server'da SAML yapılandırmak için yardıma ihtiyacınız varsa, lütfen bu makaleye bakın [SAML'yi yapılandırın.](https://help.tableau.com/current/online/en-us/saml_config_azure_ad.htm)

### <a name="create-tableau-server-test-user"></a>Tableau Server test kullanıcıoluşturma

Bu bölümün amacı Tableau Server'da B.Simon adında bir kullanıcı oluşturmaktır. Tableau sunucusundaki tüm kullanıcıları sağlamanız gerekir.

Kullanıcı nın bu kullanıcı adı, **kullanıcı adının**Azure AD özel özelliğinde yapılandırdığınız değerle eşleşmelidir. Doğru eşleme ile tümleştirme Azure AD Tek Oturum-On Yapılandırma çalışması gerekir.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, kuruluşunuzdaki Tableau Server yöneticisine başvurmanız gerekir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Tableau Server döşemesini tıklattığınızda, SSO'yu kurduğunuz Tableau Server'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Tableau Server'ı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)