---
title: 'Öğretici: Teamphoria ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Teamphoria arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d569c705-6f0f-4ec1-b485-ba82526b5d32
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/09/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2631b34f5658c9d4f76ca26d378bc63fe59ad156
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72373263"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-teamphoria"></a>Öğretici: Teamphoria ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Teamphoria'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Teamphoria'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Teamphoria erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Teamphoria'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Teamphoria tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Teamphoria **SP** başlatılan SSO destekler

## <a name="adding-teamphoria-from-the-gallery"></a>Galeriden Teamphoria ekleme

Teamphoria'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Teamphoria'yı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama **kutusuna Teamphoria** yazın.
1. Sonuç panelinden **Teamphoria'yı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-teamphoria"></a>Teamphoria için Azure AD oturum açma işlemlerini yapılat ve test edin

Azure AD SSO'nu Teamphoria ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Teamphoria'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Teamphoria ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Teamphoria SSO'yu yapılandırın](#configure-teamphoria-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Teamphoria test kullanıcısını oluşturun](#create-teamphoria-test-user)** - Teamphoria'da B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Teamphoria** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<sub-domain>.teamphoria.com/login`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [Teamphoria Müşteri destek ekibine](https://www.teamphoria.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Teamphoria'yı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Teamphoria'ya erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Teamphoria'yı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-teamphoria-sso"></a>Teamphoria SSO'nun yapılandırılsın

1. Teamphoria içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **Teamphoria'yı** ayarla'yı tıklatın ve sizi Teamphoria uygulamasına yönlendirecektir. Buradan Teamphoria'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Teamphoria'yı el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Teamphoria şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sol araç çubuğundaki **ADMIN AYARLARi** seçeneğine gidin ve Yapılandırma Sekmesi'nin altında SSO yapılandırma penceresini açmak için **TEK OTURUM AÇMA'ya** tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/teamphoria-tutorial/admin_sso_configure.png)

5. SSO ayarlarını eklemek için formu açmak için sağ üst köşedeki **Yenİ KİmLİk SAHİBİ** ekle seçeneğini tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/teamphoria-tutorial/add_new_identity_provider.png)

6. Aşağıda açıklandığı gibi alanlara ayrıntıları girin-

    ![Tek İşaret-On'u Yapılandır](./media/teamphoria-tutorial/Teamphoria_sso_save.png)

    a. **DISPLAY Name**: Yönetici sayfasına eklentinin ekran adını girin.

    b. **DÜĞME ADI**: SSO üzerinden giriş yapmak için giriş sayfasında görüntülenecek sekme adı.

    c. **SERTIFIKA**: Azure portalından daha önce indirilen Sertifikayı not defterinde açın, aynı içeriği kopyalayın ve kutuya yapıştırın.

    d. **Gİrİş NOKTASI**: Azure portalından daha önce kopyalanan **Giriş URL'sini** yapıştırın.

    e. Seçeneği **AÇ'a** geçirin ve **KAYDET'e**tıklayın.

### <a name="create-teamphoria-test-user"></a>Teamphoria test kullanıcısı oluşturma

Azure AD kullanıcılarının Teamphoria'da oturum açabilmeleri için Teamphoria'da oturum açmaları gerekir. Teamphoria durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Teamphoria şirket sitenizde yönetici olarak oturum açın.

1. Sol araç çubuğunda ve **YÖNET** sekmesinin altındaki **ADMIN** ayarlarına tıklayın Kullanıcılar için yönetici sayfasını açmak için **KULLANIMCIyi** tıklatın.

    ![Çalışan Ekle](./media/teamphoria-tutorial/admin_manage_users.png)

1. **MANUEL DAVET** seçeneğine tıklayın.

    ![Kişileri Davet Et](./media/teamphoria-tutorial/admin_manage_add_users.png)

1. Bu sayfada, aşağıdaki eylemi gerçekleştirin.

    ![Kişileri Davet Et](./media/teamphoria-tutorial/manual_user_invite.png)

    a. **E-POSTA ADRESİ** textbox'ına, B.Simon gibi kullanıcının **e-posta adresini** girin.

    b. First **NAME** textbox'a, **Kullanıcının**adını B gibi girin.

    c. LAST **NAME** textbox'a, **Simon**gibi kullanıcının soyadını girin.

    d. **DAVET 1 KULLANICI'yı**tıklatın. Kullanıcının sistemde oluşturulmak için daveti kabul etmesi gerekir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Teamphoria döşemesini tıklattığınızda, SSO'yu kurduğunuz Teamphoria'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Teamphoria'yı deneyin](https://aad.portal.azure.com/)

