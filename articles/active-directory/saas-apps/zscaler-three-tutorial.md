---
title: 'Öğretici: Zscaler Three ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Zscaler Three arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f352e00d-68d3-4a77-bb92-717d055da56f
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b23abcf9a39ce7f6d77bc40e7143505bc68e8b72
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72554995"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-zscaler-three"></a>Öğretici: Zscaler Three ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Zscaler Three'ü Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Zscaler Three'ü Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Zscaler Three'e erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Zscaler Three'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Zscaler Üç tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Zscaler Üç **SP** başlatılan SSO destekler

* Zscaler Üç **Just In Time** kullanıcı sağlama destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-zscaler-three-from-the-gallery"></a>Galeriden Zscaler Üç ekleme

Zscaler Three'ün Azure AD'ye entegrasyonunu yapılandırmak için galeriden Zscaler Three'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Zscaler Three** yazın.
1. Sonuç panelinden **Zscaler Three'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-zscaler-three"></a>Zscaler Three için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu Zscaler Three ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Zscaler Three'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Zscaler Three ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Zscaler Three SSO'yu yapılandırır](#configure-zscaler-three-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Zscaler Üç test kullanıcısı oluşturun](#create-zscaler-three-test-user)** - Zscaler Three'deki B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Zscaler Üç** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://login.zscalerthree.net/sfc_sso`

1. Zscaler Üç uygulamanız, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/edit-attribute.png)

6. Yukarıdakilere ek olarak, Zscaler Üç uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.
    
    | Adı | Kaynak Özniteliği |
    | ---------| ------------ |
    | Memberof     | user.assignedroles |

    > [!NOTE]
    > Azure AD'de Rolü nasıl yapılandıracağınıöğrenmek için [lütfen buraya](https://docs.microsoft.com/azure/active-directory/active-directory-enterprise-app-role-management) tıklayın

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Zscaler Üç'ü Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Zscaler Three'e erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Zscaler Three'u**seçin.
1. Kullanıcılar **ve gruplar** iletişim kutusunda, listeden **Britta Simon** gibi kullanıcıyı seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_users.png)

1. Rolü **Seç** iletişim kutusundan listedeki uygun kullanıcı rolünü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_roles.png)

1. Atama **Ekle** iletişim kutusunda **Atla** düğmesini seçin.

    ![image](./media/zscaler-three-tutorial/tutorial_zscalerthree_assign.png)

## <a name="configure-zscaler-three-sso"></a>Zscaler Üç SSO'nun yapılandırılması

1. Zscaler Three içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'e tıklayarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **Kurulum Zscaler Three'e** tıklayın ve sizi Zscaler Three uygulamasına yönlendirecektir. Buradan, Zscaler Three'de oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum](common/setup-sso.png)

3. Zscaler Three'ü el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Zscaler Three şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Yönetim **> Kimlik Doğrulama ayarları>** gidin ve aşağıdaki adımları gerçekleştirin:
   
    ![Yönetim](./media/zscaler-three-tutorial/ic800206.png "Yönetim")

    a. Kimlik Doğrulama Türü altında **SAML'yi**seçin.

    b. **SAML'yi Yapılandır'ı**tıklatın.

5. **SAML'yi Edit** penceresinde aşağıdaki adımları gerçekleştirin ve Kaydet'i tıklatın.  
            
    ![Kullanıcıları & Kimlik Doğrulamayı Yönetin](./media/zscaler-three-tutorial/ic800208.png "Kullanıcıları & Kimlik Doğrulamayı Yönetin")
    
    a. **SAML Portal URL** textbox'ına Azure portalından kopyalamış olduğunuz **Giriş URL'sini** yapıştırın.

    b. Giriş **Adı Özniteliği** metin kutusuna **NameID'i**girin.

    c. Azure portalından indirdiğiniz Azure SAML imza sertifikasını **Genel SSL Sertifikası'na**yüklemek için **Yükle'yi**tıklatın.

    d. **SAML Otomatik Sağlama'yı Etkinleştir'i**geçişin.

    e. Kullanıcı **Görüntülü Reklam Adı Özniteliği** metin kutusuna, displayName öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **displayName'i** girin.

    f. Grup **Adı Özniteliği** textbox'ına, üye Of öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız **üyeOf** girin.

    g. Bölüm **Adı Özniteliği** Bölüm Girin **bölümünde,** bölüm öznitelikleri için SAML otomatik sağlamayı etkinleştirmek istiyorsanız.

    h. **Kaydet**'e tıklayın.

6. Kullanıcı **Kimlik Doğrulaması'nı Yapılandırma** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Yönetim](./media/zscaler-three-tutorial/ic800207.png)

    a. Sol alttaki **Etkinleştirme** menüsünün üzerine titreyin.

    b. **Etkinleştir'i**tıklatın.

## <a name="configuring-proxy-settings"></a>Ara sunucu ayarlarını yapılandırma
### <a name="to-configure-the-proxy-settings-in-internet-explorer"></a>Internet Explorer'da proxy ayarlarını yapılandırmak için

1. **Internet Explorer'ı başlatın.**

2. **Internet Seçenekleri** iletişim kutusunu açmak için **Araçlar** menüsünden **Internet seçeneklerini** seçin.   
    
     ![İnternet Seçenekleri](./media/zscaler-three-tutorial/ic769492.png "İnternet Seçenekleri")

3. **Bağlantılar** sekmesini tıklatın.   
  
     ![Bağlantılar](./media/zscaler-three-tutorial/ic769493.png "Bağlantılar")

4. **LAN Ayarları** iletişim kutusunu açmak için **LAN ayarlarını** tıklatın.

5. Proxy sunucusu bölümünde aşağıdaki adımları gerçekleştirin:   
   
    ![Proxy sunucusu](./media/zscaler-three-tutorial/ic769494.png "Proxy sunucu")

    a. **LAN'ınız için proxy sunucusu kullan'ı**seçin.

    b. Adres metin kutusuna **ağ geçidi yazın. Zscaler Three.net**.

    c. Bağlantı Noktası metin kutusunda **80**yazın.

    d. **Yerel adresler için Bypass proxy sunucusu'ni**seçin.

    e. **Yerel Alan Ağı (LAN) Ayarları** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

6. **Internet Seçenekleri** iletişim kutusunu kapatmak için **Tamam'ı** tıklatın.

### <a name="create-zscaler-three-test-user"></a>Zscaler Üç test kullanıcısı oluşturma

Bu bölümde, B.Simon adlı bir kullanıcı Zscaler Üç oluşturulur. Zscaler Three, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Zscaler Üç'te bir kullanıcı zaten yoksa, Zscaler Three'e erişmeye çalıştığınızda yeni bir kullanıcı oluşturulur.

>[!Note]
>El ile bir kullanıcı oluşturmanız gerekiyorsa, [Zscaler Üç destek ekibine](https://www.zscaler.com/company/contact)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Zscaler Üç döşemesini tıklattığınızda, SSO'yu kurduğunuz Zscaler Three'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Zscaler Üç'ü deneyin](https://aad.portal.azure.com/)

