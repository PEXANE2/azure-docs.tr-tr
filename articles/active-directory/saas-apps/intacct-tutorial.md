---
title: 'Öğretici: Sage Intacct ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Sage Intacct arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 92518e02-a62c-4b1b-a8e9-2803eb2b49ac
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b28390dba009226d493f5bfc6a5270b067f5bba0
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "73570533"
---
# <a name="tutorial-integrate-sage-intacct-with-azure-active-directory"></a>Öğretici: Sage Intacct'i Azure Etkin Dizini ile tümleştirin

Bu eğitimde, Sage Intacct'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Sage Intacct'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Sage Intacct erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Sage Intacct'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Sage Intacct tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Sage Intacct **IDP** başlatılan SSO destekler

## <a name="adding-sage-intacct-from-the-gallery"></a>Galeriden Sage Intacct ekleme

Sage Intacct'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize Sage Intacct eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Sage Intacct** yazın.
1. Sonuç panelinden **Sage Intacct'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sage-intacct"></a>Sage Intacct için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Sage Intacct ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Sage Intacct'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Sage Intacct ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
2. Uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için **[Sage Intacct SSO'yu yapılandırın.](#configure-sage-intacct-sso)**
    1. **[Sage Intacct test kullanıcısını oluşturun](#create-sage-intacct-test-user)** - Kullanıcının Azure AD gösterimine bağlı Sage Intacct'te B.Simon'ın bir muadili olması için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Sage Intacct** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    **Yanıtla URL** metin kutusuna bir URL yazın:`https://www.intacct.com/ia/acct/sso_response.phtml`

1. Sage Intacct uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın...

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, Sage Intacct uygulaması SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, aşağıdaki tabloda gösterildiği gibi SAML belirteç özniteliği eklemek için aşağıdaki adımları gerçekleştirin:

    | Adı  |  Kaynak Özniteliği|
    | ---------------| --------------- |
    | Şirket Adı | **Adaçayı Intacct Şirket Kimliği** |
    | ad | Değer, daha sonra öğreticide açıklanan **Create Sage Intacct test kullanıcı bölümüne girdiğiniz Sage Intacct Kullanıcı** **Kimliği**ile aynı olmalıdır |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier | Değer, daha sonra öğreticide açıklanan **Create Sage Intacct test kullanıcı bölümüne girdiğiniz Sage Intacct** **Federe SSO Kullanıcı Kimliği**ile aynı olmalıdır |

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni **talep ekle'yi** tıklatın.

    b. **Ad** metin kutusunda, bu satır için gösterilen öznitelik adını yazın.

    c. Ad **alanını** boş bırakın.

    d. **Kaynak'ı Öznitelik**olarak seçin.

    e. Kaynak **öznitelik** listesinden, bu satır için gösterilen öznitelik değerini yazın.

    f. **Tamam'ı** tıklatın

    g. **Kaydet**'e tıklayın.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Sage Intacct'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Sage Intacct'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, **Sage Intacct'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-sage-intacct-sso"></a>Sage Intacct SSO'nun yapılandırılması

1. Farklı bir web tarayıcısı penceresinde, Sage Intacct şirket sitenizde yönetici olarak oturum açın.

1. **Şirket** sekmesini tıklatın ve ardından **Şirket Bilgileri'ni**tıklatın.

    ![Şirket](./media/intacct-tutorial/ic790037.png "Şirket")

1. **Güvenlik** sekmesini tıklatın ve sonra **Edit'i**tıklatın.

    ![Güvenlik](./media/intacct-tutorial/ic790038.png "Güvenlik")

1. Tek **işaret (SSO)** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Tek işaret](./media/intacct-tutorial/ic790039.png "tek işaret")

    a. **Tek oturum aç'ı etkinleştir'i**seçin.

    b. **Kimlik sağlayıcı türü olarak** **SAML 2.0'ı**seçin.

    c. **Veren URL** textbox'ına, Azure portalından kopyaladığınız Azure **AD Tanımlayıcısı'nın**değerini yapıştırın.

    d. **Giriş URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    e. **Taban-64** kodlu sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve **ardından Sertifika** kutusuna yapıştırın.

    f. **Kaydet**'e tıklayın.

### <a name="create-sage-intacct-test-user"></a>Sage Intacct test kullanıcıoluşturma

Azure AD kullanıcılarını Sage Intacct'te oturum açabilmeleri için sage Intacct'e dahil olmaları gerekir. Sage Intacct için, sağlama manuel bir görevdir.

**Kullanıcı hesaplarını sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Sage Intacct** kiracınızla oturum açın.

1. **Şirket** sekmesini tıklatın ve ardından **Kullanıcılar'ı**tıklatın.

    ![Kullanıcılar](./media/intacct-tutorial/ic790041.png "Kullanıcılar")

1. **Ekle** sekmesini tıklatın.

    ![Ekle](./media/intacct-tutorial/ic790042.png "Ekle")

1. Kullanıcı **Bilgileri** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Kullanıcı Bilgileri](./media/intacct-tutorial/ic790043.png "Kullanıcı Bilgileri")

    a. Kullanıcı **Kimliği,** **Soyadı,** **Adı,** **E-posta adresi,** **Başlık**ve **Kullanıcı Bilgileri** bölümüne sağlamak istediğiniz bir Azure REKLAM hesabının **Telefonunu** girin.

    > [!NOTE]
    > Yukarıdaki ekran görüntüsündeki **Kullanıcı Kimliği** ile Azure portalındaki **Kullanıcı Öznitelikleri** bölümünde **ad** özniteliği ile eşlenen **Kaynak Öznitelik** değerinin aynı olduğundan emin olun.

    b. Sağlamak istediğiniz bir Azure REKLAM hesabının **Yönetici ayrıcalıklarını** seçin.

    c. **Kaydet**'e tıklayın. 
    
    d. Azure AD hesap sahibi bir e-posta alır ve etkin hale gelmeden önce hesaplarını onaylamak için bir bağlantı izler.

1. **Tek oturum açma** sekmesini tıklatın ve aşağıda ekran görüntüsündeki **Federe SSO kullanıcı kimliğinin** ve Azure `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier` portalındaki Kullanıcı **Öznitelikleri** bölümünde eşlenen **Kaynak Öznitelik** değerinin aynı olduğundan emin olun.

    ![Kullanıcı Bilgileri](./media/intacct-tutorial/ic790044.png "Kullanıcı Bilgileri")

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için, Sage Intacct tarafından sağlanan diğer Sage Intacct kullanıcı hesabı oluşturma araçlarını veya API'leri kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Sage Intacct döşemesini tıklattığınızda, SSO'yu kurduğunuz Sage Intacct'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

