---
title: 'Öğretici: Netskope Kullanıcı Kimlik Doğrulaması ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Netskope Kullanıcı Kimlik Doğrulaması arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e9bb71cd-aaf9-4403-aca5-c5a349ec562a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e33af932e405552cf9d8f5aaf6d42cbd095607b0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74085376"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-netskope-user-authentication"></a>Öğretici: Netskope Kullanıcı Kimlik Doğrulaması ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Netskope Kullanıcı Kimlik Doğrulaması'nı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Netskope Kullanıcı Kimlik Doğrulaması'nı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Netskope Kullanıcı Kimlik Doğrulaması'na erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Netskope Kullanıcı Kimlik Doğrulaması'nda otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Netskope Kullanıcı Kimlik Doğrulaması tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Netskope Kullanıcı Kimlik Doğrulaması **SP ve IDP** SSO başlatılan destekler

## <a name="adding-netskope-user-authentication-from-the-gallery"></a>Galeriden Netskope Kullanıcı Kimlik Doğrulaması Ekleme

Netskope Kullanıcı Kimlik Doğrulama'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize Netskope Kullanıcı Kimlik Doğrulaması eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Netskope Kullanıcı Kimlik Doğrulaması** yazın.
1. Sonuç panelinden **Netskope Kullanıcı Kimlik Doğrulaması'nı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-netskope-user-authentication"></a>Netskope Kullanıcı Kimlik Doğrulaması için Azure AD oturumunu yapılandırma ve test

Azure AD SSO'nu Netskope Kullanıcı Kimlik Doğrulaması ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Netskope Kullanıcı Kimlik Doğrulaması'nda bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Netskope Kullanıcı Kimlik Doğrulaması ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Netskope Kullanıcı Kimlik Doğrulaması SSO'yu yapılandırın.](#configure-netskope-user-authentication-sso)**
    * **[Netskope Kullanıcı Kimlik Doğrulama testi kullanıcısını oluşturun](#create-netskope-user-authentication-test-user)** - Kullanıcının Azure AD gösterimine bağlı Netskope Kullanıcı Kimlik Doğrulaması'nda B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Netskope Kullanıcı Kimlik Doğrulama** uygulaması tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenantname>.goskope.com/<customer entered string>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenantname>.goskope.com/nsauth/saml2/http-post/<customer entered string>`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri daha sonra öğretici de açıklanan alırsınız.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<tenantname>.goskope.com`

    > [!NOTE]
    > Oturum Açma URL değerleri gerçek değildir. Oturum Açma URL değerini gerçek Oturum Açma URL'si ile güncelleştirin. Oturum açma URL değerini almak için [Netskope Kullanıcı Kimlik Doğrulama İstemci destek ekibiyle](mailto:support@netskope.com) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federasyon **Metadata XML'ini** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Netskope Kullanıcı Kimlik Doğrulaması Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Netskope Kullanıcı Kimlik Doğrulaması'na erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Netskope Kullanıcı Kimlik Doğrulaması'nı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-netskope-user-authentication-sso"></a>Netskope Kullanıcı Kimlik Doğrulaması SSO'su yapılandırın

1. Tarayıcınızda yeni bir sekme açın ve Netskope Kullanıcı Kimlik Doğrulama şirket sitenizde yönetici olarak oturum açın.

1. **Etkin Platform** sekmesini tıklatın.

    ![Netskope Kullanıcı Kimlik Doğrulama Yapılandırması](./media/netskope-user-authentication-tutorial/user1.png)

1. **FORWARD PROXY'ye** gidin ve **SAML'yi**seçin.

    ![Netskope Kullanıcı Kimlik Doğrulama Yapılandırması](./media/netskope-user-authentication-tutorial/config-saml.png)

1. **SAML Ayarları** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Netskope Kullanıcı Kimlik Doğrulama Yapılandırması](./media/netskope-user-authentication-tutorial/configure-copyurls.png)

    a. **SAML Entity ID** değerini kopyalayın ve Azure portalındaki Temel **SAML Yapılandırma** sı bölümündeki **Tanımlayıcı** metin kutusuna yapıştırın.

    b. **SAML ACS URL** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Yanıtla URL** textbox'ına yapıştırın.

1. **HESAP EKLE'yi**tıklatın.

    ![Netskope Kullanıcı Kimlik Doğrulama Yapılandırması](./media/netskope-user-authentication-tutorial/config-addaccount.png)

1. **SAML Hesabı Ekle** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Netskope Kullanıcı Kimlik Doğrulama Yapılandırması](./media/netskope-user-authentication-tutorial/config-settings1.png)

    a. **AD** metin kutusunda, adı Azure AD gibi sağlayın.

    b. **IDP URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    c. **IDP ENTITY ID** textbox'a, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    d. İndirilen meta veri dosyanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **IDP CERTIFICATE** textbox'a yapıştırın.

    e. **KAYDET'i**tıklatın.

### <a name="create-netskope-user-authentication-test-user"></a>Netskope Kullanıcı Kimlik Doğrulama testi kullanıcısı oluşturma

1. Tarayıcınızda yeni bir sekme açın ve Netskope Kullanıcı Kimlik Doğrulama şirket sitenizde yönetici olarak oturum açın.

1. Sol daki gezinti bölmesinden **Ayarlar** sekmesine tıklayın.

    ![Netskope Kullanıcı Kimlik Doğrulama Kullanıcı Oluşturma](./media/netskope-user-authentication-tutorial/config-settings.png)

1. **Etkin Platform** sekmesini tıklatın.

    ![Netskope Kullanıcı Kimlik Doğrulama Kullanıcı Oluşturma](./media/netskope-user-authentication-tutorial/user1.png)

1. **Kullanıcılar** sekmesini tıklatın.

    ![Netskope Kullanıcı Kimlik Doğrulama Kullanıcı Oluşturma](./media/netskope-user-authentication-tutorial/add-user.png)

1. **KULLANICIEKLE'yi**tıklatın.

    ![Netskope Kullanıcı Kimlik Doğrulama Kullanıcı Oluşturma](./media/netskope-user-authentication-tutorial/user-add.png)

1. Eklemek istediğiniz kullanıcının e-posta adresini girin ve **ADD'yi**tıklatın.

    ![Netskope Kullanıcı Kimlik Doğrulama Kullanıcı Oluşturma](./media/netskope-user-authentication-tutorial/add-user-popup.png)

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Netskope Kullanıcı Kimlik Doğrulama döşemesini tıklattığınızda, SSO'yu kurduğunuz Netskope Kullanıcı Kimlik Doğrulaması'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Netskope Kullanıcı Kimlik Doğrulamasını Deneyin](https://aad.portal.azure.com/)