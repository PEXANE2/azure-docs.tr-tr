---
title: 'Öğretici: CakeHR ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve CakeHR arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1c9bbdb1-ac47-4fb8-a1cc-1e647b0323a7
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/16/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: c0860411c95e48a16d75df4aeeedf3405a5b1835
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72595031"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-cakehr"></a>Öğretici: CakeHR ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, CakeHR'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. CakeHR'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* CakeHR erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla CakeHR'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* CakeHR tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* CakeHR **SP** başlatılan SSO destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-cakehr-from-the-gallery"></a>Galeriden CakeHR ekleme

CakeHR'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden CakeHR'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **CakeHR** yazın.
1. Sonuçlar panelinden **CakeHR'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-cakehr"></a>CakeHR için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak CakeHR ile yapılandırın ve test edin. SSO'nun çalışması için, CakeHR'deki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu CakeHR ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[CakeHR SSO'yu yapılandırın](#configure-cakehr-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[CakeHR test kullanıcısını oluşturun](#create-cakehr-test-user)** - Kullanıcının Azure AD gösterimine bağlı CakeHR'de B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **CakeHR** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<yourcakedomain>.cake.hr/`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<yourcakedomain>.cake.hr/services/saml/consume`
    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [CakeHR İstemci destek ekibine](mailto:info@cake.hr) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmzalama Sertifikası** bölümünde, **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

    ![SAML İmza Sertifikasını Edin](common/edit-certificate.png)

1. **SAML İmza Sertifikası** **bölümünde, THUMBPRINT** değerini kopyalayın ve Not Defterinize kaydedin.

    ![Parmak Izi değerini kopyala](common/copy-thumbprint.png)

1. **CakeHR'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın CakeHR'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **CakeHR'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-cakehr-sso"></a>CakeHR SSO'nun yapılandırılSın

1. CakeHR içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **CakeHR'i** ayarla'ya tıklayın ve sizi CakeHR uygulamasına yönlendirecektir. Buradan CakeHR'de oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. CakeHR'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve CakeHR şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Sayfanın sağ üst köşesinde **Profil'e** tıklayın ve ardından **Ayarlar'a**gidin.

    ![CakeHR yapılandırması](./media/cakehr-tutorial/config01.png)

1. Menü çubuğunun sol **tarafından, TÜMLESSAMS** > **SAML SSO'ya** tıklayın ve aşağıdaki adımları gerçekleştirin:

    ![CakeHR yapılandırması](./media/cakehr-tutorial/config02.png)

    a. Varlık **Kimliği** metin kutusuna `cake.hr`.

    b. Kimlik **Doğrulama URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. Anahtar **parmak izi (SHA1 biçimi)** metin kutusuna, Azure portalından kopyaladığınız **THUMBPRINT** değerini yapıştırın.

    d. Kutudaki **Tek İşareti Etkinleştir'i** işaretleyin.

    e. **Kaydet**'e tıklayın.

### <a name="create-cakehr-test-user"></a>CakeHR test kullanıcıoluşturma

Azure AD kullanıcılarının CakeHR'de oturum açabilmeleri için CakeHR'de oturum açmaları gerekir. CakeHR'de, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Güvenlik Yöneticisi olarak CakeHR'de oturum açın.

2. Menü çubuğunun sol **tarafından, Company** > **ADD'ye**tıklayın.

    ![CakeHR yapılandırması](./media/cakehr-tutorial/config03.png)

3. Yeni **çalışan** açılır pencere ekle'de aşağıdaki adımları gerçekleştirin:

     ![CakeHR yapılandırması](./media/cakehr-tutorial/config04.png)

    a. **Tam ad** metin kutusuna, B.Simon gibi kullanıcının adını girin.

    b. **İş e-posta** metin kutusuna, `B.Simon@contoso.com`kullanıcının e-postasını girin.

    c. **HESAP OLUŞTUR'u**tıklatın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki CakeHR döşemesini tıklattığınızda, SSO'yu kurduğunuz CakeHR'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile CakeHR'i deneyin](https://aad.portal.azure.com/)
