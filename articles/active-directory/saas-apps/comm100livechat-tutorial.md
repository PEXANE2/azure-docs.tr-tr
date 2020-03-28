---
title: 'Öğretici: Comm100 Live Chat ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Comm100 Canlı Sohbet arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 0340d7f3-ab54-49ef-b77c-62a0efd5d49c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/22/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e41161a2e1bfd5544410bba0cb470bacbf152fd2
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75561276"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-comm100-live-chat"></a>Öğretici: Comm100 Live Chat ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Azure Etkin Dizini (Azure AD) ile Comm100 Canlı Sohbet'i nasıl entegre acağınızı öğreneceksiniz. Comm100 Live Chat'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Comm100 Canlı Sohbet'e erişimi olan denetimi.
* Kullanıcılarınızın Azure AD hesaplarıyla Comm100 Live Chat'te otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Comm100 Live Chat tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Comm100 Canlı Sohbet **SP** başlatılan SSO destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-comm100-live-chat-from-the-gallery"></a>Galeriden Comm100 Canlı Sohbet Ekleme

Comm100 Live Chat'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Comm100 Live Chat eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Comm100 Live Chat** yazın.
1. Sonuç panelinden **Comm100 Canlı Sohbet'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-comm100-live-chat"></a>Comm100 Live Chat için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Comm100 Live Chat ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Comm100 Live Chat'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Comm100 Live Chat ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Comm100 Live Chat SSO'yu yapılandırır](#configure-comm100-live-chat-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Comm100 Live Chat test kullanıcısını oluşturun](#create-comm100-live-chat-test-user)** - Kullanıcının Azure REKLAM gösterimine bağlı Comm100 Live Chat'te B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Comm100 Live Chat** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.comm100.com/AdminManage/LoginSSO.aspx?siteId=<SITEID>`

    > [!NOTE] 
    > Oturum açma URL değeri gerçek değildir. Oturum Açma URL değerini, daha sonra öğreticide açıklanan gerçek Oturum Açma URL'si ile güncellersiniz.

1. Comm100 Live Chat uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, Comm100 Live Chat uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | ---------------| --------------- |
    |   e-posta    | kullanıcı.posta |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Comm100 Canlı Sohbet'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Comm100 Live Chat'e erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Comm100 Canlı Sohbet'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-comm100-live-chat-sso"></a>Yapılandırma Comm100 Canlı Sohbet SSO

1. Farklı bir web tarayıcısı penceresinde, Güvenlik Yöneticisi olarak Comm100 Live Chat'te oturum açın.

1. Sayfanın sağ üst tarafında **Hesabım'ı**tıklatın.

   ![Comm100 Canlı Sohbet myaccount](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

1. Menünün sol tarafından **Güvenlik'i** tıklatın ve ardından **Aracı Tek Oturum Aç'ı**tıklatın.

   ![Comm100 Canlı Sohbet güvenliği](./media/comm100livechat-tutorial/tutorial_comm100livechat_security.png)

1. Aracı **Tek Oturum Açma** sayfasında aşağıdaki adımları gerçekleştirin:

   ![Comm100 Canlı Sohbet güvenliği](./media/comm100livechat-tutorial/tutorial_comm100livechat_singlesignon.png)

   a. Vurgulanan ilk bağlantıyı kopyalayın ve Azure **portalındaki** **Temel SAML Yapılandırma** bölümünde Oturum Aç URL textbox'a yapıştırın.

   b. **SAML SSO URL** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

   c. Uzaktan **Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

   d. Azure portalından indirdiğiniz base-64 kodlanmış sertifikayı **Sertifika'ya**yüklemek için **Dosya Seç'i** tıklatın.

   e. **Değişiklikleri Kaydet**’e tıklayın.

### <a name="create-comm100-live-chat-test-user"></a>Comm100 Canlı Sohbet test kullanıcıoluşturma

Azure AD kullanıcılarının Comm100 Live Chat'te oturum açabilmeleri için Comm100 Live Chat'te oturum açmaları gerekir. Comm100 Canlı Sohbet'te, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Güvenlik Yöneticisi olarak Comm100 Canlı Sohbet'te oturum açın.

2. Sayfanın sağ üst tarafında **Hesabım'ı**tıklatın.

    ![Comm100 Canlı Sohbet myaccount](./media/comm100livechat-tutorial/tutorial_comm100livechat_account.png)

3. Menünün sol tarafından **Aracılar'ı** tıklatın ve ardından **Yeni Aracı'yı**tıklatın.

    ![Comm100 Canlı Sohbet aracısı](./media/comm100livechat-tutorial/tutorial_comm100livechat_agent.png)

4. Yeni **Aracılı** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Comm100 Canlı Sohbet yeni ajan](./media/comm100livechat-tutorial/tutorial_comm100livechat_newagent.png)

    a. a. **E-posta** metin kutusuna, **\@B.simon contoso.com**gibi kullanıcının e-posta girin.

    b. **Ad metin** kutusuna, **B**gibi kullanıcının ilk adını girin.

    c. **Soyadı** metin kutusuna, **simon**gibi kullanıcının soyadını girin.

    d. Görüntü **Adı** metin kutusuna, **B.simon** gibi kullanıcının görüntü adını girin

    e. **Parola** metin kutusuna parolanızı yazın.

    f. **Kaydet**'e tıklayın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Comm100 Canlı Sohbet döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Comm100 Live Chat'te oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Comm100 Canlı Sohbet'i deneyin](https://aad.portal.azure.com/)

