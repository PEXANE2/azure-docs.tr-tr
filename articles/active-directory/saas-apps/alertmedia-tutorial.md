---
title: 'Öğretici: AlertMedia ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve AlertMedia arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5c77f6fc-807c-47e8-8e49-eece5ebd7f09
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/17/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b6aaaa24535c95e0a0d3f2b67709374c083ab738
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81726252"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-alertmedia"></a>Öğretici: AlertMedia ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, AlertMedia'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. AlertMedia'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de AlertMedia'ya erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla AlertMedia'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* AlertMedia tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **AlertMedia, IDP** tarafından başlatılan SSO'ya destek verdi
* AlertMedia **Just In Time** kullanıcı sağlama destekler
* AlertMedia'yı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-alertmedia-from-the-gallery"></a>Galeriden AlertMedia ekleme

AlertMedia'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize AlertMedia eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **AlertMedia** yazın.
1. Sonuçlar panelinden **AlertMedia'yı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-alertmedia"></a>AlertMedia için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak AlertMedia ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile AlertMedia'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu AlertMedia ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[AlertMedia SSO'yu yapılandırın.](#configure-alertmedia-sso)**
    1. **[AlertMedia test kullanıcısını oluşturun](#create-alertmedia-test-user)** - AlertMedia'da Kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **AlertMedia** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da** aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.alertmedia.com`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.alertmedia.com/api/sso/saml/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [AlertMedia İstemci destek ekibine](mailto:support@alertmedia.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. AlertMedia uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, AlertMedia uygulaması, aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Adı |   |  Kaynak Özniteliği|
    | ---------------| --------------- | --------- |
    | e-posta |  | user.userprincipalname |
    | Ad |  | user.givenname |
    | Soyadı |  | user.surname |

1. **SAML ile Tek Oturum** Açma sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, B.Simon'ın AlertMedia'ya erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **AlertMedia'yı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-alertmedia-sso"></a>AlertMedia SSO'ya yapılandır

1. Yeni bir web tarayıcısı penceresinde, AlertMedia şirket sitenizde yönetici olarak oturum açın.
1. **Şirket'e** gidin ve **Tek Oturum Açma'yı**seçin.

    ![Hesap düğmesi](./media/alertmedia-tutorial/Configure1.png)
1. Kimlik **Doğrulama**Yöntemi'nde, **Uzak SAML Meta verilerini** seçin
1. **İşaret İsteğini** Geçiş
1. İzin Ver **Pasif İstekleri** Üzerine Geçiş
1. **MetaData URL** textbox'ına, Azure portalını kopyaladığınız **App Federation Metadata Url** değerini yapıştırın.
1. Tam **olarak** **İstenen Kimlik Doğrulama Bağlamı** Karşılaştırması'nı seçin
1. **IDP Giriş URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.
1. **Kaydet'i** tıklatın

### <a name="create-alertmedia-test-user"></a>AlertMedia test kullanıcısı oluşturma

Bu bölümde, AlertMedia'da Britta Simon adında bir kullanıcı oluşturulur. AlertMedia, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. AlertMedia'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki AlertMedia döşemesini tıklattığınızda, SSO'yu kurduğunuz AlertMedia'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile AlertMedia'yı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [AlertMedia'yı gelişmiş görünürlük ve kontrollerle koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

