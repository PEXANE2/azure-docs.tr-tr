---
title: 'Öğretici: Netskope Administrator Console ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Netskope Administrator Console arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e286adec-8d19-4d41-9afa-a2e39d7a5983
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/31/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3c38900d4ded3d2ee08245674bda90d96226c1eb
ms.sourcegitcommit: 632e7ed5449f85ca502ad216be8ec5dd7cd093cb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2020
ms.locfileid: "80396569"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-netskope-administrator-console"></a>Öğretici: Netskope Administrator Console ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Netskope Yönetici Konsolu'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edileceksiniz öğreneceksiniz. Netskope Yönetici Konsolu'nun Azure AD ile tümleştirildiğinde şunları yapabilirsiniz:

* Netskope Yönetici Konsolu'na erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Netskope Yönetici Konsolu'nda otomatik olarak oturum açabilmesini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Netskope Administrator Console tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Netskope Yönetici **KonsolU SP ve IDP** SSO başlatılan destekler

## <a name="adding-netskope-administrator-console-from-the-gallery"></a>Galeriden Netskope Yönetici Konsolu Ekleme

Netskope Yönetici Konsolu'nun Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Netskope Yönetici Konsolu'nu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Netskope Yönetici Konsolu** yazın.
1. Sonuç panelinden **Netskope Yönetici** Konsolu'nu seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-netskope-administrator-console"></a>Netskope Yönetici Konsolu için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'sunu Netskope Administrator Console ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Netskope Yönetici Konsolu'ndaki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Netskope Administrator Console ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Netskope Administrator Console SSO'yu yapılandırın.](#configure-netskope-administrator-console-sso)**
    * **[Netskope Yönetici Konsolu test kullanıcısını oluşturun](#create-netskope-administrator-console-test-user)** - Kullanıcının Azure AD gösterimine bağlı Netskope Yönetici Konsolu'nda B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Netskope Yönetici Konsolu** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`<OrgKey>`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenant_host_name>/saml/acs`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri daha sonra öğretici de açıklanan alırsınız.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<tenantname>.goskope.com`

    > [!NOTE]
    > Oturum Açma URL değerleri gerçek değildir. Oturum Açma URL değerini gerçek Oturum Açma URL'si ile güncelleştirin. Oturum açma URL değerini almak için [Netskope Administrator Console Client destek ekibine](mailto:support@netskope.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Netskope Administrator Console uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıda ek olarak, Netskope Administrator Console uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | ---------| --------- |
    | yönetici rolü | user.assignedroles |

    > [!NOTE]
    > Azure AD'de nasıl roller oluşturacağınıöğrenmek için [burayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-enterprise-app-role-management) tıklatın.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Netskope Yönetici Konsolu'nu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Netskope Yönetici Konsolu'na erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Netskope Yönetici Konsolu'nu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-netskope-administrator-console-sso"></a>Netskope Yönetici Konsolu SSO'sunu yapılandır

1. Tarayıcınızda yeni bir sekme açın ve Netskope Administrator Console şirket sitenizde yönetici olarak oturum açın.

1. Sol daki gezinti bölmesinden **Ayarlar** sekmesine tıklayın.

    ![Netskope Yönetici Konsol Yapılandırması](./media/netskope-cloud-security-tutorial/config-settings.png)

1. **Yönetim** sekmesini tıklatın.

    ![Netskope Yönetici Konsol Yapılandırması](./media/netskope-cloud-security-tutorial/config-administration.png)

1. **SSO sekmesini** tıklatın.

    ![Netskope Yönetici Konsol Yapılandırması](./media/netskope-cloud-security-tutorial/config-sso.png)

1. Ağ **Ayarları** bölümünde aşağıdaki adımları gerçekleştirin:
    
    ![Netskope Yönetici Konsol Yapılandırması](./media/netskope-cloud-security-tutorial/config-pasteurls.png)

    a. **İddia Tüketici Hizmeti URL** değerini kopyalayın ve Azure portalındaki Temel **SAML Yapılandırması** bölümündeki **Yanıtla URL** textbox'ına yapıştırın.

    b. **Hizmet Sağlayıcısı Varlık Kimliği** değerini kopyalayın ve Azure portalındaki Temel **SAML Yapılandırma** sı bölümündeki **Tanımlayıcı** metin kutusuna yapıştırın.

1. **SSO/SLO Ayarları** bölümündeki **EDIT** AYARLARI'na tıklayın.

    ![Netskope Yönetici Konsol Yapılandırması](./media/netskope-cloud-security-tutorial/config-editsettings.png)

1. **Ayarlar** açılır penceresinde aşağıdaki adımları gerçekleştirin;

    ![Netskope Yönetici Konsol Yapılandırması](./media/netskope-cloud-security-tutorial/configuration.png)

    a. **SSO'ya Etkin'i**seçin.

    b. **IDP URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    c. **IDP ENTITY ID** textbox'a, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    d. İndirilen Base64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **IDP CERTIFICATE** textbox'a yapıştırın.

    e. **SSO'ya Etkin'i**seçin.

    f. **IDP SLO URL** textbox'ına Azure portalından kopyaladığınız **Logout URL** değerini yapıştırın.

    g. **GÖNDER'i**tıklatın.

### <a name="create-netskope-administrator-console-test-user"></a>Netskope Yönetici Konsolu test kullanıcıoluşturma

1. Tarayıcınızda yeni bir sekme açın ve Netskope Administrator Console şirket sitenizde yönetici olarak oturum açın.

1. Sol daki gezinti bölmesinden **Ayarlar** sekmesine tıklayın.

    ![Netskope Yönetici Konsolkullanıcı Oluşturma](./media/netskope-cloud-security-tutorial/config-settings.png)

1. **Etkin Platform** sekmesini tıklatın.

    ![Netskope Yönetici Konsolkullanıcı Oluşturma](./media/netskope-cloud-security-tutorial/user1.png)

1. **Kullanıcılar** sekmesini tıklatın.

    ![Netskope Yönetici Konsolkullanıcı Oluşturma](./media/netskope-cloud-security-tutorial/add-user.png)

1. **KULLANICIEKLE'yi**tıklatın.

    ![Netskope Yönetici Konsolkullanıcı Oluşturma](./media/netskope-cloud-security-tutorial/user-add.png)

1. Eklemek istediğiniz kullanıcının e-posta adresini girin ve **ADD'yi**tıklatın.

    ![Netskope Yönetici Konsolkullanıcı Oluşturma](./media/netskope-cloud-security-tutorial/add-user-popup.png)

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Netskope Yönetici Konsolu döşemesini tıklattığınızda, SSO'yu kurduğunuz Netskope Yönetici Konsolu'nda otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Netskope Yönetici Konsolu'nun deneyin](https://aad.portal.azure.com/)
