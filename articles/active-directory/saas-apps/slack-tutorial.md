---
title: 'Öğretici: Slack ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Slack arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: ffc5e73f-6c38-4bbb-876a-a7dd269d4e1c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/31/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6c80963976783321d05fc6f32bb24daed36fa105
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76985568"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-slack"></a>Öğretici: Slack ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Slack'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Slack'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Slack erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Slack'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Slack tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Slack **SP** başlatılan SSO destekler
* Bolluk **Just In Time** kullanıcı sağlama destekler
* Bolluk [ **Otomatik** kullanıcı sağlama destekler](https://docs.microsoft.com/azure/active-directory/saas-apps/slack-provisioning-tutorial)
* Slack'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan Oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-aad)

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-slack-from-the-gallery"></a>Galeriden Bolluk Ekleme

Slack'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize Slack eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Slack** yazın.
1. Sonuç panelinden **Slack'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-slack"></a>Slack için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Slack ile yapılandırma ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile Slack'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Slack ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Slack SSO'yu yapılandırır](#configure-slack-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Slack test kullanıcısını oluşturun](#create-slack-test-user)** - Kullanıcının Azure AD gösterimine bağlı olan Slack'teki B.Simon'Un bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Slack** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<companyname>.slack.com`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna bir URL yazın:`https://slack.com`

    > [!NOTE]
    > URL değerindeki Oturum gerçek değildir. URL'deki gerçek İşaret ile değeri güncelleştirin. Değeri almak için [Slack İstemci destek ekibine](https://slack.com/help/contact) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Bolluk uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, Slack uygulaması aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz. Kullanıcıların e-posta adresi yoksa **e-posta adresini** **user.userprincipalname**ile eşlerler.

    | Adı | Kaynak Özniteliği |
    | -----|---------|
    | Emailaddress | user.userprincipalname |
    | | |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Bolluk **Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Slack'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Slack'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-slack-sso"></a>Slack SSO'ya yapılandır

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Slack şirket sitenizde oturum açın.

2. Microsoft **Azure AD'ye** gidin ve **ardından Takım Ayarları'na**gidin.

     ![Uygulama Tarafında tek oturum açma yapılandırma](./media/slack-tutorial/tutorial_slack_001.png)

3. Takım **Ayarları** bölümünde Kimlik **Doğrulama** sekmesini tıklatın ve ardından **Ayarları Değiştir'i**tıklatın.

    ![Uygulama Tarafında tek oturum açma yapılandırma](./media/slack-tutorial/tutorial_slack_002.png)

4. **SAML Kimlik Doğrulama Ayarları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Uygulama Tarafında tek oturum açma yapılandırma](./media/slack-tutorial/tutorial_slack_003.png)

    a.  **SAML 2.0 Endpoint (HTTP)** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    b.  Kimlik **Sağlayıcı Veren** metin kutusuna, Azure portalından kopyaladığınız **Azure Reklam Tanımlayıcısı'nın**değerini yapıştırın.

    c.  İndirilen sertifika dosyanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından **Herkese Açık Sertifika** metin kutusuna yapıştırın.

    d. Yukarıdaki üç ayarı Slack ekibiniz için uygun şekilde yapılandırın. Ayarlar hakkında daha fazla bilgi için lütfen **Slack'in SSO yapılandırma kılavuzunu** buraya bulabilirsiniz. `https://get.slack.help/hc/articles/220403548-Guide-to-single-sign-on-with-Slack%60`

    e.  **Yapılandırmayı Kaydet'i**tıklatın.

### <a name="create-slack-test-user"></a>Bolluk testi kullanıcısı oluşturma

Bu bölümün amacı Slack B.Simon adlı bir kullanıcı oluşturmaktır. Bolluk, varsayılan olarak etkinleştirilen tam zamanında sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Henüz yoksa Slack'e erişme girişimi sırasında yeni bir kullanıcı oluşturulur. Bolluk da otomatik kullanıcı sağlama destekler, otomatik kullanıcı sağlama yapılandırmak için nasıl daha fazla bilgi [burada](slack-provisioning-tutorial.md) bulabilirsiniz.

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Slack destek ekibine](https://slack.com/help/contact)başvurmanız gerekir.

> [!NOTE]
> Azure AD Connect, şirket içinde Etkin Dizin Kimliklerini Azure AD ile senkronize edebilen bir eşitleme aracıdır ve bu senkronize kullanıcılar uygulamaları diğer bulut kullanıcıları gibi kullanabilir.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Bolluk döşemesini tıklattığınızda, SSO'yu kurduğunuz Bolluk'ta otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Bolluğu Deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)