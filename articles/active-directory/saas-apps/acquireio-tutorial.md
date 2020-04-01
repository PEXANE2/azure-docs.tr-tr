---
title: 'Öğretici: Azure Active Directory, AcquireIO ile tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve AcquireIO arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 97815e13-32ec-4470-b075-1253f5814f4f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 01bcc3678485119afae1d567d97eff9dcebe6b95
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76714626"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-acquireio"></a>Öğretici: Edinme IO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, AcquireIO'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğreneceksiniz. Edinme'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Edinme IO erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Otomatik olarak AcquireIO'da oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* AcquireIO tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* AcquireIO **IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-acquireio-from-the-gallery"></a>Galeriden AcquireIO ekleme

Edinme IO'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize AcquireIO eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **AcquireIO** yazın.
1. Sonuç panelinden **AcquireIO'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-acquireio"></a>EdinmeIO için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak AcquireIO ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile AcquireIO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu AcquireIO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Yapıla, Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[AcquireIO SSO'yu](#configure-acquireio-sso)** yapılandırın.
    * **[Edinme IO test kullanıcısını oluşturun](#create-acquireio-test-user)** - Kullanıcının Azure AD gösterimine bağlı AcquireIO'da B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **AcquireIO** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.acquire.io/ad/<acquire_account_uid>`

    > [!NOTE]
    > Değer gerçek değil. Öğreticinin Daha sonra **Yapılandırılan AcquireIO** bölümünde açıklanan gerçek Yanıt URL'sini alırsınız. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **EdinmeIO'yu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın AcquireIO'ya erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama listesinde **AcquireIO'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-acquireio-sso"></a>Yapılandırma AcquireIO SSO

1. EdinmeIO içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Uzantıyı tarayıcıya ekledikten sonra, sizi AcquireIO uygulamasına yönlendiren **AcquireIO'yu**ayarla'yı tıklatın. Buradan, AcquireIO'da oturum açmanın yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. AcquireIO'yu el ile, farklı bir web tarayıcıpenceresinde kurmak istiyorsanız, Yönetici olarak AcquireIO'da oturum açın.

1. Menünün sol tarafından App **Store'a**tıklayın.

     ![AcquireIO yapılandırması](./media/acquireio-tutorial/config01.png)

1. **Active Directory'ye** gidin ve **Yükle'ye**tıklayın.

    ![AcquireIO yapılandırması](./media/acquireio-tutorial/config02.png)

1. Active Directory açılır pencerede aşağıdaki adımları gerçekleştirin:

    ![AcquireIO yapılandırması](./media/acquireio-tutorial/config03.png)

    a. Örneğiniz için Yanıtla URL'sini kopyalamak ve Azure portalındaki **Temel SAML Yapılandırması** **bölümündeki YanıtLA URL** metin kutusuna yapıştırmak için **Kopyala'yı** tıklatın.

    b. Giriş **URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. Base64 kodlanmış sertifikayı Notepad'de açın, içeriğini kopyalayın ve **X.509 Sertifika** metin kutusuna yapıştırın.

    d. **Şimdi Bağlan'ı**tıklatın.

### <a name="create-acquireio-test-user"></a>AcquireIO test kullanıcısı oluşturma

Azure AD kullanıcılarının AcquireIO'da oturum açabilmeleri için, bu kullanıcıların AcquireIO'da sağlanması gerekir. AcquireIO'da, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak AcquireIO'da oturum açın.

1. Menünün sol tarafından **Profiller'i** tıklatın ve **Profil Ekle'ye**gidin.

     ![AcquireIO yapılandırması](./media/acquireio-tutorial/config04.png)

1. **Müşteri** açılır ekle'de aşağıdaki adımları gerçekleştirin:

    ![AcquireIO yapılandırması](./media/acquireio-tutorial/config05.png)

    a. **Ad** metin kutusuna, **B.simon**gibi kullanıcının adını girin.

    b. **E-posta** metin kutusuna, gibi **B.simon@contoso.com**kullanıcının e-posta girin.

    c. **Gönder'i**tıklatın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki AcquireIO döşemesini tıklattığınızda, SSO'yu kurduğunuz AcquireIO'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile EdinmeIO'u deneyin](https://aad.portal.azure.com/)
