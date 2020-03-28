---
title: 'Öğretici: Dmarcian ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve dmarcian arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: a04b9383-3a60-4d54-9412-123daaddff3b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/01/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 602c885deca429b56417181971ced495831ba5d3
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68823704"
---
# <a name="tutorial-integrate-dmarcian-with-azure-active-directory"></a>Öğretici: Dmarcian'ı Azure Active Directory ile tümleştirin

Bu eğitimde, dmarcian'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Dmarcian'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Dmarcian erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla otomatik olarak oturum açarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* dmarcian tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* dmarcian **SP ve IDP** SSO başlatılan destekler

## <a name="adding-dmarcian-from-the-gallery"></a>Galeriden dmarcian ekleme

Dmarcian'ın Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize dmarcian eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **dmarcian** yazın.
1. Sonuç panelinden **dmarcian'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'yu **B.Simon**adlı bir test kullanıcısı kullanarak dmarcian ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile dmarcian'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu dmarcian ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[Dmarcian SSO'yu yapılandırın](#configure-dmarcian-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[Dmarcian test kullanıcıoluşturun](#create-dmarcian-test-user)** - kullanıcının Azure AD gösterimi ile bağlantılı dmarcian B.Simon bir meslektaşı olması.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **dmarcian** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    | -- |
    | `https://us.dmarcian.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-eu.com/sso/saml/<ACCOUNT_ID>/sp.xml` |
    | `https://dmarcian-ap.com/sso/saml/<ACCOUNT_ID>/sp.xml` |

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>/handle/` |
    | `https://dmarcian-ap.com/login/<ACCOUNT_ID>/handle/` |

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:
    
    | |
    |--|
    | `https://us.dmarcian.com/login/<ACCOUNT_ID>` |
    | `https://dmarcian-eu.com/login/<ACCOUNT_ID>` |
    | `https://dmarciam-ap.com/login/<ACCOUNT_ID>` |
     
    > [!NOTE] 
    > Bu değerler gerçek değildir. Bu değerleri, öğreticide daha sonra açıklanan gerçek Tanımlayıcı, Yanıt URL'si ve Oturum Açma URL'si ile güncelleştireceksiniz.

4. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-dmarcian-sso"></a>Dmarcian SSO'yu yapılandırın

1. dmarcian içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'e tıklayarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantısı ekledikten sonra, **Kurulum dmarcian** tıklayın dmarcian uygulamasına yönlendirecektir. Oradan, dmarcian oturum için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Dmarcian'ı el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve dmarcian şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sağ üst köşedeki **Profil'e** tıklayın ve **Tercihler'e**gidin.

    ![Tercihler](./media/dmarcian-tutorial/tutorial_dmarcian_pref.png)

5. Aşağı kaydırın ve **Tek Oturum Açma** bölümüne tıklayın, ardından **Yapıla'ya**tıklayın.

    ![Tek](./media/dmarcian-tutorial/tutorial_dmarcian_sso.png)

6. **SAML Tek Oturum Açma** sayfasında **Etkin Olarak Durum'u** ayarlayın ve aşağıdaki adımları gerçekleştirin: **Enabled**

    ![Kimlik doğrulama](./media/dmarcian-tutorial/tutorial_dmarcian_auth.png)

    * **Kimlik Sağlayıcınıza dmarcian ekle** bölümünün altında, örneğiniz için Tüketici Hizmeti **URL'sini** kopyalamak için **COPY'yi** tıklatın ve Azure portalındaki **Temel SAML Yapılandırması bölümünde** **yanıtURL** metin kutusuna yapıştırın.

    * **Kimlik Sağlayıcınıza dmarcian ekle'nin** altında, örneğiniz için **Varlık Kimliğini** kopyalamak için **KOPYALA'yı** tıklatın ve Azure portalındaki Temel **SAML Yapılandırması bölümündeki** **Tanımlayıcı** metin kutusuna yapıştırın.

    * **Kimlik Doğrulamayı Ayarlama** bölümünde, Kimlik Sağlayıcısı **Metadata** textbox'ta Azure portalından kopyaladığınız **Uygulama Federasyonu Metaveri Url'sini**yapıştırın.

    * **Kimlik Doğrulamayı Ayarlama** bölümünde, **Öznitelik İfadeleri** textbox yapıştırın url`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    * **Giriş URL'sini Ayarla** bölümünde, örneğiniz için **Giriş URL'sini** kopyalayın ve Azure portalındaki **Temel SAML Yapılandırma bölümünde** Oturum Aç **URL** textbox'a yapıştırın.

        > [!Note]
        > **Giriş URL'sini** kuruluşunuza göre değiştirebilirsiniz.

    * **Kaydet**'e tıklayın.

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

Bu bölümde, B.Simon'ın dmarcian erişimi vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, **dmarcian**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-dmarcian-test-user"></a>Dmarcian test kullanıcısı oluşturma

Azure AD kullanıcılarının dmarcian'da oturum açabilmeleri için, dmarcian'a dahil edilmeleri gerekir. Dmarcian olarak, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Dmarcian'da Güvenlik Yöneticisi olarak oturum açın.

2. Sağ üst köşedeki **Profil'e** tıklayın ve **Kullanıcıları Yönet'e**gidin.

    ![Kullanıcı](./media/dmarcian-tutorial/tutorial_dmarcian_user.png)

3. **SSO Kullanıcıları** bölümünün sağ tarafında Yeni **Kullanıcı Ekle**bölümüne tıklayın.

    ![Ekle kullanıcısı](./media/dmarcian-tutorial/tutorial_dmarcian_addnewuser.png)

4. Yeni **Kullanıcı Ekle** açılır pencerede aşağıdaki adımları gerçekleştirin:

    ![Yeni kullanıcı](./media/dmarcian-tutorial/tutorial_dmarcian_save.png)

    a. Yeni **Kullanıcı E-posta** textbox, **\@brittasimon contoso.com**gibi kullanıcının e-posta girin.

    b. Kullanıcıya yönetici hakları vermek istiyorsanız, **Kullanıcı Yı Yönetici Yap'ı**seçin.

    c. **Kullanıcı Ekle**'ye tıklayın.

### <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki dmarcian karo'yu tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz dmarcian'da oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

