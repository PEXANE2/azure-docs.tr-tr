---
title: 'Öğretici: Akamai ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Akamai arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1b7e0d7a-e78f-43a5-af93-b626186e2376
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 042dd242285081001ca48c9f17e4d42c2294c0ff
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74979598"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-akamai"></a>Öğretici: Akamai ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Akamai'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Akamai'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Akamai erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Akamai'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Akamai tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

- Bolluk IDP'nin Başlattığı SSO'ya destek verdi

## <a name="adding-akamai-from-the-gallery"></a>Galeriden Akamai ekleme

Akamai'nin Azure AD'ye entegrasyonunu yapılandırmak için, Akamai'yi galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama **kutusuna Akamai** yazın.
1. Sonuç panelinden **Akamai'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-akamai"></a>Akamai için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Akamai ile yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Akamai'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Akamai ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Akamai SSO'yu yapılandırın](#configure-akamai-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Akamai test kullanıcıoluşturun](#create-akamai-test-user)** - Kullanıcının Azure AD gösterimi ile bağlantılı Akamai B.Simon bir meslektaşı olması.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Akamai** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<Yourapp>.login.go.akamai-access.com/sp/response`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https:// <Yourapp>.login.go.akamai-access.com/sp/response`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [Akamai Müşteri destek ekibine](https://www.akamai.com/us/en/contact-us/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Akamai'yi ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Akamai'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Akamai'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-akamai-sso"></a>Akamai SSO'nun yapılandırılsın

### <a name="setting-up-idp"></a>IDP'nin kurulması

1. **Akamai Enterprise Application Access** konsolunda oturum açın.
1. **Akamai EAA konsolunda,** **Kimlik Kimliği** > **Sağlayıcıları**seçin.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure01.png)

1. **Kimlik Sağlayıcı Ekle'yi**tıklatın.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure02.png)

    a. Benzersiz **Adı**belirtin.
    
    b. **Üçüncü Taraf SAML'yi** seçin ve **Kimlik Sağlayıcısı Oluştur'u ve Yapılandır'ı**tıklatın.

### <a name="general-settings"></a>Genel Ayarlar

1. **Kimlik Engelleme** - (SP temel URL'si-Azure AD Yapılandırması için kullanılacaktır) adını belirtin

    > [!NOTE]
    > Kendi özel etki alanınız olmasını seçebilirsiniz (DNS girişi ve Sertifika gerektirir). Bu örnekte Akamai Domain'i kullanacağız.

1. **Akamai Bulut Bölgesi** - Uygun bulut bölgesini seçin.
1. **Sertifika Doğrulama** - Akamai Belgeleri Kontrol Et (isteğe bağlı)

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure03.png)

### <a name="authentication-configuration"></a>Kimlik Doğrulama Yapılandırması

1. URL – URL'yi kimlik engellemenizle aynı şekilde belirtin (bu, kullanıcıların kimlik doğrulamadan sonra yeniden yönlendirildiği yerdir).
2. Oturum Açma URL'si : Oturum açma URL'sini güncelleyin.
3. SAML İsteği'ni imzalayın: varsayılan olarak işaretlenmemiş.
4. IDP Meta veri dosyası için Uygulamayı Azure AD Konsoluna ekleyin.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure04.png)

### <a name="header-based-authentication"></a>Üstbilgi Tabanlı Kimlik Doğrulama

Akamai Başlık Tabanlı Kimlik Doğrulama

1. Uygulamalar Ekle **Sihirbazı'nı Özel HTTP** formunu seçin.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure05.png)

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure06.png)

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure07.png)

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure08.png)

#### <a name="authentication"></a>Kimlik doğrulaması

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure09.png)

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure10.png)

#### <a name="services"></a>Hizmetler

1. Kaydet ve Kimlik Doğrulamaya Git'i tıklatın.

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure11.png)

#### <a name="advanced-settings"></a>Gelişmiş Ayarlar

1. Müşteri **HTTP Üstbilgi altında,** **CustomerHeader** ve **SAML Özniteliği**ni belirtin.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure12.png)

1. **Kaydet'i tıklatın ve Dağıtım düğmesine gidin.**

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure13.png)

#### <a name="deploy-the-application"></a>Uygulamayı Dağıt

1. **Uygulamayı Dağıt** düğmesini tıklatın.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure14.png)

1. Uygulamanın başarıyla dağıtıldığını doğrulayın.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure15.png)

### <a name="kerberos-authentication"></a>Kerberos Kimlik Doğrulaması

#### <a name="remote-desktop"></a>Uzak Masaüstü

1. ADD Uygulamaları Sihirbazı'ndan **RDP'yi** seçin.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure16.png)

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure17.png)

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure18.png)

1. Buna hizmet verecek Bağlayıcıyı belirtin.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure19.png)

#### <a name="authentication"></a>Kimlik doğrulaması

**Kaydet'i tıklatın ve Hizmetlere gidin.**

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure20.png)

#### <a name="services"></a>Hizmetler

**Kaydet'i tıklatın ve Gelişmiş Ayarlar'a gidin.**

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure21.png)

#### <a name="advanced-settings"></a>Gelişmiş Ayarlar

**Kaydet'i tıklatın ve Dağıtım'a gidin.**

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure22.png)

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure23.png)

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure24.png)

### <a name="deployment"></a>Dağıtım

#### <a name="ssh"></a>SSH

1. Uygulama Ekle'ye git , **SSH'yi**seçin.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure25.png)

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure26.png)

1. Uygulama Kimliğini Yapılandırın.

    ![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure27.png)

    a. Ad / Açıklama belirtin.

    b. SSH için Uygulama Sunucusu IP/FQDN ve bağlantı noktasını belirtin.

    c. SSH kullanıcı adını / parolayı *Akamai EAA'yı kontrol edin.

    d. Dış ana bilgisayar adını belirtin.

    e. Bağlayıcının Konumunu belirtin ve bağlayıcıyı seçin.

#### <a name="authentication"></a>Kimlik doğrulaması

**Kaydet'e tıklayın ve Hizmetlere gidin.**

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure28.png)

#### <a name="services"></a>Hizmetler

**Kaydet'i tıklatın ve Gelişmiş Ayarlar'a gidin.**

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure29.png)

#### <a name="advanced-settings"></a>Gelişmiş Ayarlar

Kaydet'i tıklatın ve Dağıtıma gitmek için

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure30.png)

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure31.png)

#### <a name="deployment"></a>Dağıtım

**Uygulamayı Dağıt'ı**tıklatın.

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure32.png)

### <a name="kerberos-applications"></a>Kerberos Uygulamaları

#### <a name="adding-directory"></a>Dizin Ekleme

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure33.png)

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure34.png)

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure35.png)

![Akamai'nin Yapılandırılması](./media/header-akamai-tutorial/configure36.png)

### <a name="create-akamai-test-user"></a>Akamai test kullanıcısı oluşturma

Bu bölümde, Akamai'de B.Simon adında bir kullanıcı oluşturursunuz. Akamai platformunda kullanıcıları eklemek için [Akamai Müşteri destek ekibi](https://www.akamai.com/us/en/contact-us/) ile çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir. 

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Akamai karosu tıklattığınızda, SSO'yu kurduğunuz Akamai'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Akamai'yi deneyin](https://aad.portal.azure.com/)
