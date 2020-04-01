---
title: 'Öğretici: RingCentral ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve RingCentral arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 5848c875-5185-4f91-8279-1a030e67c510
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2019
ms.author: jeedes
ms.openlocfilehash: de7cf57d177902efdbb44524703481e8c65c75c5
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72991464"
---
# <a name="tutorial-integrate-ringcentral-with-azure-active-directory"></a>Öğretici: RingCentral'ı Azure Active Directory ile tümleştirin

Bu eğitimde, RingCentral'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. RingCentral'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* RingCentral erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla RingCentral'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* RingCentral tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **RingCentral, IDP'nin** başlattığı SSO'ya destek verdi

## <a name="adding-ringcentral-from-the-gallery"></a>Galeriden RingCentral ekleme

RingCentral'ın Azure AD'ye entegrasyonunu yapılandırmak için, ringcentral'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **RingCentral** yazın.
1. Sonuçlar panelinden **RingCentral'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **Britta Simon**adlı bir test kullanıcısı kullanarak RingCentral ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile RingCentral'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu RingCentral ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[RingCentral SSO'yu yapılandırın](#configure-ringcentral-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[RingCentral test kullanıcısını oluşturun](#create-ringcentral-test-user)** - Kullanıcının Azure AD gösterimine bağlı RingCentral'daki B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **RingCentral** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız**varsa aşağıdaki adımları gerçekleştirin:

    1. **Meta veri dosyalarını yükle'yi**tıklatın.
    1. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.
    1. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri Temel **SAML Yapılandırması** bölümünde otomatik olarak doldurulur.

    > [!Note]
    > Daha sonra öğreticide açıklanan RingCentral SSO Yapılandırma sayfasında **Servis Sağlayıcı meta veri dosyasını** alırsınız.

1. Servis Sağlayıcı meta **veri dosyanız**yoksa, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna bir URL yazın:

    | |
    |--|
    |  `https://sso.ringcentral.com` |
    | `https://ssoeuro.ringcentral.com` |

    b. **Yanıtla URL** metin kutusuna bir URL yazın:

    | |
    |--|
    | `https://sso.ringcentral.com/sp/ACS.saml2` |
    | `https://ssoeuro.ringcentral.com/sp/ACS.saml2` |

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Britta Simon'ın RingCentral'a erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **RingCentral'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-ringcentral-sso"></a>RingCentral SSO'da yapılandır

1. RingCentral içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyi'ni tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **RingCentral'ı kur'a** tıklayın ve sizi RingCentral uygulamasına yönlendirecektir. Buradan, RingCentral'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. RingCentral'ı el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve RingCentral şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. **Üstte, Araçlar'a**tıklayın.

    ![image](./media/ringcentral-tutorial/ringcentral1.png)

1. Tek **Oturum Açma'ya**gidin.

    ![image](./media/ringcentral-tutorial/ringcentral2.png)

1. Tek **Oturum Açma** sayfasında, **SSO Yapılandırması** bölümünün altında, **Adım 1'den** **Edit'i** tıklatın ve aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral3.png)

1. Tek **Oturum Açma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral4.png)

    a. Azure portalından indirdiğiniz meta veri dosyasını yüklemek için **Gözat'ı** tıklatın.

    b. Meta verileri yükledikten sonra değerler **SSO Genel Bilgiler** bölümünde otomatik olarak doldurulur.

    c. **Öznitelik Eşleme** bölümü altında, **Harita E-posta Özniteliği'ni**`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`

    d. **Kaydet**'e tıklayın.

    e. Hizmet Sağlayıcısı **Download** meta veri **dosyasını** indirmek ve Azure portalındaki **Tanımlayıcı** ve **YanıtURL** değerlerini otomatik olarak doldurmak için **Temel SAML Yapılandırması** bölümüne yüklemek için **2.**

    ![image](./media/ringcentral-tutorial/ringcentral6.png) 

    f. Aynı sayfada, **SSO'yu etkinleştir** bölümüne gidin ve aşağıdaki adımları gerçekleştirin:

    ![image](./media/ringcentral-tutorial/ringcentral5.png)

    * **SSO Hizmetini Etkinleştir'i**seçin.

    * **Kullanıcıların SSO veya RingCentral kimlik bilgileriyle oturum açmasına izin ver'i**seçin.

    * **Kaydet**'e tıklayın.

### <a name="create-ringcentral-test-user"></a>RingCentral test kullanıcısını oluşturma

Bu bölümde, RingCentral'da Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları RingCentral platformuna eklemek için [RingCentral Client destek ekibiyle](https://success.ringcentral.com/RCContactSupp) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki RingCentral döşemesini seçtiğinizde, Otomatik olarak SSO'yu kurduğunuz RingCentral'da oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile RingCentral'ı deneyin](https://aad.portal.azure.com/)