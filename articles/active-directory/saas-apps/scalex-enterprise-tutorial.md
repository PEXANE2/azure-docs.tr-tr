---
title: 'Öğretici: ScaleX Enterprise ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve ScaleX Enterprise arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c2379a8d-a659-45f1-87db-9ba156d83183
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5e54994d02dd1abbca1602952fbad058b3ad993d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72594270"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-scalex-enterprise"></a>Öğretici: ScaleX Enterprise ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, ScaleX Enterprise'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. ScaleX Enterprise'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* ScaleX Enterprise'a erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla ScaleX Enterprise'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* ScaleX Enterprise tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* ScaleX **Enterprise, SP ve IDP** tarafından başlatılan SSO'ya destek veriyor

## <a name="adding-scalex-enterprise-from-the-gallery"></a>Galeriden ScaleX Enterprise ekleme

ScaleX Enterprise'ın Azure AD'ye entegrasyonunu yapılandırmak için, scalex enterprise'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **ScaleX Enterprise** yazın.
1. Sonuçlar panelinden **ScaleX Enterprise'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-scalex-enterprise"></a>ScaleX Enterprise için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak ScaleX Enterprise ile yapılandırın ve test edin. SSO'nun çalışması için, ScaleX Enterprise'daki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu ScaleX Enterprise ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[ScaleX Enterprise SSO'yu yapılandırır](#configure-scalex-enterprise-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[ScaleX Enterprise test kullanıcısını oluşturun](#create-scalex-enterprise-test-user)** - ScaleX Enterprise'da kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **ScaleX Enterprise** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://platform.rescale.com/saml2/<company id>/`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://platform.rescale.com/saml2/<company id>/acs/`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://platform.rescale.com/saml2/<company id>/sso/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [ScaleX Kurumsal İstemci destek ekibine](https://info.rescale.com/contact_sales) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. ScaleX Enterprise uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **e-posta adresi** **user.mail**ile eşlenir. ScaleX Enterprise uygulaması **e-posta adresinin** **user.userprincipalname**ile eşleşeceğini bekler, bu nedenle **Düzenleme** simgesine tıklayarak öznitelik eşlemini düzenlemeniz ve öznitelik eşlemini değiştirmeniz gerekir.

    ![image](common/edit-attribute.png)

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **ScaleX Enterprise'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın ScaleX Enterprise'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **ScaleX Enterprise'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-scalex-enterprise-sso"></a>ScaleX Enterprise SSO'da yapılandırın

1. ScaleX Enterprise içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **ScaleX Enterprise'ı** ayarla'yı tıklatın ve sizi ScaleX Enterprise uygulamasına yönlendirecektir. Buradan, ScaleX Enterprise'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. ScaleX Enterprise'ı el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve ScaleX Enterprise şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Sağ üstteki menüye tıklayın ve **Contoso Administration'ı**seçin.

    > [!NOTE]
    > Contoso sadece bir örnektir. Bu gerçek Şirket Adı olmalıdır.

    ![Çoklu oturum açmayı yapılandırma](./media/scalex-enterprise-tutorial/Test_Admin.png)

1. Üst menüden **Tümleştirmeler'i** seçin ve **tek oturum açma'yı**seçin.

    ![Çoklu oturum açmayı yapılandırma](./media/scalex-enterprise-tutorial/admin_sso.png) 

1. Formu aşağıdaki gibi doldurun:

    ![Çoklu oturum açmayı yapılandırma](./media/scalex-enterprise-tutorial/scalex_admin_save.png)

    a. **SSO ile kimlik doğrulaması yapabilecek herhangi bir kullanıcıyı oluştur'u** seçin

    b. **Servis Sağlayıcı saml**: Değer ***vazoyapıştırın:vaha:isimleri:tc:SAML:2.0:nameid-format:kalıcı***

    c. **ACS yanıtında Kimlik Sağlayıcı e-posta alanının adı**: Değeri yapıştırın`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Kimlik Sağlayıcı Varlık Tanımlayıcı Entity ID:** Azure portalından kopyalanan **Azure AD Tanımlayıcı** değerini yapıştırın.

    e. **Kimlik Sağlayıcısı SingleSignonService URL:** **Giriş URL'sini** Azure portalından yapıştırın.

    f. **Kimlik Sağlayıcı kamu X509 sertifikası:** Azure'dan indirilen X509 sertifikasını not defterine açın ve içindekileri bu kutuya yapıştırın. Sertifika içeriğinin ortasında satır sonu olmadığından emin olun.

    g. Aşağıdaki onay kutularını kontrol edin: **Etkin, Şifrele NameID ve AuthnRequests imzalayın.**

    h. Ayarları kaydetmek için **SSO Ayarlarını Güncelleştir'i** tıklatın.

### <a name="create-scalex-enterprise-test-user"></a>ScaleX Enterprise test kullanıcısı oluşturma

Azure AD kullanıcılarının ScaleX Enterprise'da oturum açmalarını sağlamak için ScaleX Enterprise'da bunların sağlanması gerekir. ScaleX Enterprise durumunda, sağlama otomatik bir görevdir ve el ile adım gerekmez. SSO kimlik bilgileriyle başarılı bir şekilde kimlik doğrulaması yapabilecek tüm kullanıcı, ScaleX tarafında otomatik olarak kullanılabilir.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki ScaleX Kurumsal döşemesini tıklattığınızda, SSO'yu kurduğunuz ScaleX Enterprise'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile ScaleX Enterprise'ı deneyin](https://aad.portal.azure.com/)