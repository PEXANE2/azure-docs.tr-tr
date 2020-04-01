---
title: 'Öğretici: Elium ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Elium arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: fae344b3-5bd9-40e2-9a1d-448dcd58155f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0900f730c287586725722f0b8baaeb0c22f850c2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72791236"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-elium"></a>Öğretici: Elium ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Elium'u Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Elium'u Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Elium'a erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Elium'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Elium tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Elium **SP ve IDP'nin** SSO'ya başlamasını destekliyor
* Elium **Just In Time** kullanıcı sağlama destekler

## <a name="adding-elium-from-the-gallery"></a>Galeriden Elium ekleme

Elium'un Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Elium'u yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Elium** yazın.
1. Sonuç panelinden **Elium'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-elium"></a>Elium için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu Elium ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Elium'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Elium ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Elium SSO'yu yapılandırın.](#configure-elium-sso)**
    * **[Elium test kullanıcısını oluşturun](#create-elium-test-user)** - Kullanıcının Azure AD gösterimine bağlı Olan Elium'daki B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Elium** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<platform-domain>.elium.com/login/saml2/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<platform-domain>.elium.com/login/saml2/acs`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<platform-domain>.elium.com/login/saml2/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, bu öğreticide daha sonra açıklanan `https://<platform-domain>.elium.com/login/saml2/metadata` **SP meta veri dosyasından** indirilebilir olarak alırsınız.

1. Elium uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Elium uygulaması aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | ---------------| ----------------|
    | e-posta   |kullanıcı.posta |
    | Ilk_ad| user.givenname |
    | last_name| user.surname|
    | job_title| user.jobtitle|
    | şirket| user.companyname|

    > [!NOTE]
    > Bunlar varsayılan taleplerdir. **Yalnızca e-posta talebi gereklidir.** JIT sağlama için de sadece e-posta talebi zorunludur. Diğer özel talepler bir müşteri platformundan başka bir müşteri platformuna değişebilir.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Elium'u Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Elium'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Elium'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-elium-sso"></a>Elium SSO'nun yapılandırılması

1. Elium içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **Elium'u** Ayarla'yı tıklatın ve sizi Elium uygulamasına yönlendirecektir. Oradan, Elium oturum için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. Elium'u el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Yönetici olarak Elium şirket sitenizde oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Sağ üst köşeden **Kullanıcı profiline** tıklayın ve ardından **İdare'yi**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/elium-tutorial/user1.png)

1. **Güvenlik** sekmesini seçin.

    ![Tek İşaret-On'u Yapılandır](./media/elium-tutorial/user2.png)

1. Tek oturum **açma (SSO)** bölümüne gidin ve aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/elium-tutorial/user3.png)

    a. **SAML2 kimlik doğrulamasının hesabınız için çalıştığını doğrula** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Oturum Aç URL** metin kutusuna yapıştırın.

    > [!NOTE]
    > SSO'yu yapılandırdıktan sonra, varsayılan uzaktan giriş sayfasına her zaman aşağıdaki URL'den erişebilirsiniz:`https://<platform_domain>/login/regular/login` 

    b. **SAML2 federasyonu** onay kutusunu etkinleştir'i seçin.

    c. **JIT Provisioning** onay kutusunu seçin.

    d. **İndir** düğmesine tıklayarak **SP Meta verilerini** açın.

    e. **SP Metadata** dosyasında **entityID'yi** arayın, **entityID** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı** metin kutusuna yapıştırın. 

    ![Tek İşaret-On'u Yapılandır](./media/elium-tutorial/user4.png)

    f. **SP Metaveri** dosyasındaki **AssertionConsumerService'i** arayın, **Konum** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **YanıtURL** metin kutusuna yapıştırın.

    ![Tek İşaret-On'u Yapılandır](./media/elium-tutorial/user5.png)

    g. Azure portalından indirilen meta veri dosyasını not defterine açın, içeriği kopyalayın ve **IdP Metadata** metin kutusuna yapıştırın.

    h. **Kaydet**'e tıklayın.

### <a name="create-elium-test-user"></a>Elium test kullanıcıoluşturma

Bu bölümde Elium'da B.Simon adında bir kullanıcı oluşturuldu. Elium, varsayılan olarak etkinleştirilen **tam zamanında sağlamayı**destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Elium'da bir kullanıcı zaten yoksa, Elium'a erişmeye çalıştığınızda yeni bir kullanıcı oluşturulur.

> [!Note]
> El ile bir kullanım oluşturmanız gerekiyorsa, [Elium destek ekibine](mailto:support@elium.com)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Elium döşemesini tıklattığınızda, SSO'yu kurduğunuz Elium'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Elium'u deneyin](https://aad.portal.azure.com/)