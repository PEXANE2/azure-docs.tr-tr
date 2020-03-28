---
title: 'Öğretici: Azure Active Directory tek oturum açma (SSO) Claromentis ile tümleştirme | Microsoft Dokümanlar'
description: Azure Active Directory ve Claromentis arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7980e0c5-e4d8-4678-afa2-7ec219432114
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/04/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4c6eaf23950f83592a33709574be464f4499f0ab
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74823224"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-claromentis"></a>Öğretici: Azure Active Directory tek oturum açma (SSO) Claromentis ile tümleştirme

Bu eğitimde, Claromentis'i Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Claromentis'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Claromentis'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Claromentis'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Claromentis tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Claromentis **SP ve IDP'nin** sso başlattığını destekliyor
* Claromentis **Just In Time** kullanıcı sağlama destekler

## <a name="adding-claromentis-from-the-gallery"></a>Galeriden Claromentis ekleme

Claromentis'in Azure AD'ye entegrasyonunu yapılandırmak için galerideki Claromentis'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Claromentis** yazın.
1. Sonuç panelinden **Claromentis'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-claromentis"></a>Claromentis için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Claromentis ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Claromentis'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Claromentis ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Claromentis SSO'yu yapılandırır](#configure-claromentis-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Claromentis test kullanıcısını oluşturun](#create-claromentis-test-user)** - Claromentis'te B.Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Claromentis** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, kuruluş gereksiniminize göre tanımlayıcı değerini girin.

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<customer_site_url>/custom/loginhandler/simplesaml/www/module.php/saml/sp/saml2-acs.php/claromentis`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:

    | | |
    |-|-|
    | `https://<customer_site_url>/login`|
    | `https://<customer_site_url>/login?no_auto=0`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, daha sonra turorial'da açıklanan gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Claromentis'i ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Claromentis'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Claromentis'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-claromentis-sso"></a>Claromentis SSO'nun yapılandırılması

1. Farklı bir tarayıcı penceresinde, claromentis web sitesinde yönetici olarak oturum açın.

1. **Uygulamalar simgesine** tıklayın ve **Admin'i**seçin.

    ![Claromentis Konfigürasyonu](./media/claromentis-tutorial/config1.png)

1. **Özel Giriş İşleyicisi sekmesini** seçin.

    ![Claromentis Konfigürasyonu](./media/claromentis-tutorial/config2.png)

1. **SAML Config'i**seçin.

    ![Claromentis Konfigürasyonu](./media/claromentis-tutorial/config3.png)

1. **SAML Config** sekmesinde **Config** bölümüne gidin ve aşağıdaki adımları gerçekleştirin:

    ![Claromentis Konfigürasyonu](./media/claromentis-tutorial/config4.png)

    a. Teknik **Kişi Adı** metin kutusuna teknik ilgili kişinin adını girin.

    b. Teknik **İletişim E-posta** metin kutusuna, teknik ilgili kişinin E-posta adresini girin.

    c. **Şifreyi Auth Admin Password** textbox'ta girin.

1. **Auth Sources'a** gidin ve aşağıdaki adımları gerçekleştirin:

    ![Claromentis Konfigürasyonu](./media/claromentis-tutorial/config5.png)

    a. **IDP** metin kutusuna, Azure portalından kopyaladığınız **Azure AD Tanımlayıcı** değerini girin.

    b. Varlık **Kimliği** metin kutusuna Varlık Kimliği değerini girin.

    c. Azure portalından indirdiğiniz **Federasyon Metadata XML** dosyasını yükleyin.

    d. **Kaydet**'e tıklayın.

1. Artık tüm URL'lerin **SAML Config** bölümündeki **Kimlik Sağlayıcı** lar bölümünde dolduruldığını fark edeceksiniz.

    ![Claromentis Konfigürasyonu](./media/claromentis-tutorial/config6.png)

    a. **Identifier (Entity ID)** değerini kopyalayın, bu değeri Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Tanımlayıcı** metin kutusuna yapıştırın.

    b. **Yanıtla URL** değerini kopyalayın, bu değeri Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Yanıtla URL** metin kutusuna yapıştırın.

    c. **URL'de Oturum Aç** değerini kopyalayın, bu değeri Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **Oturum Aç URL** metin kutusuna yapıştırın.

### <a name="create-claromentis-test-user"></a>Claromentis test kullanıcıoluşturma

Bu bölümde, Claromentis'te B.Simon adında bir kullanıcı oluşturulur. Claromentis, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Claromentis'te bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Claromentis döşemesini tıklattığınızda, SSO'yu kurduğunuz Claromentis'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Claromentis'i deneyin](https://aad.portal.azure.com/)