---
title: 'Öğretici: Genesys tarafından PureCloud ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Genesys tarafından Azure Active Directory ve PureCloud arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e16a46db-5de2-4681-b7e0-94c670e3e54e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/03/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: dac8e0f2e10906f2cc56ecf86e0cc70947cb7e85
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78897776"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-purecloud-by-genesys"></a>Öğretici: Genesys tarafından PureCloud ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Genesys tarafından PureCloud'u Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Bunu yaptıktan sonra, şunları yapabilirsiniz:

* Genesys tarafından purecloud'a hangi kullanıcıların erişebileceğini denetlemek için Azure AD'yi kullanın.
* Kullanıcılarınızın Azure AD hesaplarıyla Genesys tarafından PureCloud'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin: Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Eğer yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Genesys tarafından PureCloud tek oturum açma (SSO)-etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Genesys tarafından PureCloud **SP ve IDP**destekler -SSO başlatılan.

> [!NOTE]
> Bu uygulamanın kimliği sabit dize değeri olduğundan, yalnızca bir örnek tek bir kiracıda yapılandırılabilir.

## <a name="adding-purecloud-by-genesys-from-the-gallery"></a>Galeriden Genesys tarafından PureCloud ekleme

PureCloud'un Genesys tarafından Azure AD'ye entegrasyonunu yapılandırmak için, galeriden PureCloud by Genesys'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir. Bunu yapmak için şu adımları uygulayın:

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabı kullanarak veya kişisel bir Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Genesys tarafından PureCloud** yazın.
1. Sonuç panelinden **Genesys'e göre PureCloud'u** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-purecloud-by-genesys"></a>PureCloud için PureCloud için Genesys tarafından Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu Genesys tarafından **B.Simon**adında bir test kullanıcısı kullanarak PureCloud ile yapılandırın ve test edin. SSO'nun çalışması için Genesys tarafından PureCloud'daki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Genesys tarafından PureCloud ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[PureCloud'u Genesys SSO tarafından uygulama](#configure-purecloud-by-genesys-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    1. **[Genesys test kullanıcısı tarafından](#create-purecloud-by-genesys-test-user)** PureCloud'da B.Simon'ın purecloud'daki bir örneğine sahip olmak için bir PureCloud oluşturun ve bu kullanıcının Azure AD gösterimine bağlı.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin:

1. **Genesys uygulama** tümleştirme sayfasındaki PureCloud [portalında,](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **Oturum Açma yöntemi** ni seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla Tek Oturum** Açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem simgesini seçin.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulamayı **IDP**tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** kutusuna, bölgenize karşılık gelen bir URL girin:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.au/saml` |

    b. **Yanıtla URL** kutusuna, bölgenize karşılık gelen bir URL girin:

    | |
    |--|
    | `https://login.mypurecloud.com/saml` |
    | `https://login.mypurecloud.de/saml` |
    | `https://login.mypurecloud.jp/saml` |
    | `https://login.mypurecloud.ie/saml` |
    | `https://login.mypurecloud.com.au/saml`|

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler** ayarla'yı seçin ve aşağıdaki adımı atın:

    Oturum **Açma URL** kutusuna, bölgenize karşılık gelen bir URL girin:
    
    | |
    |--|
    | `https://login.mypurecloud.com` |
    | `https://login.mypurecloud.de` |
    | `https://login.mypurecloud.jp` |
    | `https://login.mypurecloud.ie` |
    | `https://login.mypurecloud.com.au` |

1. Genesys uygulaması tarafından PureCloud, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir:

    ![image](common/default-attributes.png)

1. Ayrıca, Genesys uygulaması tarafından PureCloud, aşağıdaki tabloda gösterildiği gibi SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulmuş, ancak gerektiğinde bunları gözden geçirebilirsiniz.

    | Adı | Kaynak özniteliği|
    | ---------------| --------------- |
    | Email | user.userprincipalname |
    | OrganizationName | `Your organization name` |

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **PureCloud by Genesys** bölümünde, gereksinimlerinize göre uygun URL'yi (veya URL'leri) kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz:

1. Azure portalının sol bölmesinde **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. Kullanıcı **adı** alanına, kullanıcı adını aşağıdaki biçimde username@companydomain.extensiongirin: . Örneğin: `B.Simon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değere dikkat edin.
   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Genesys tarafından PureCloud'a erişim sağlayarak B.Simon'ı Azure tek oturum açma'yı kullanacak şekilde ayarlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Genesys tarafından PureCloud'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin ve ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini seçin.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini seçin.
1. Atama **Ekle** iletişim kutusunda Atama **düğmesini** seçin.

## <a name="configure-purecloud-by-genesys-sso"></a>PureCloud'u Genesys SSO'ya göre yapılandırın

1. Farklı bir web tarayıcısı penceresinde, Genesys tarafından purecloud'da yönetici olarak oturum açın.

1. Üstte **Yönetici'yi** seçin ve **ardından Tümleştirmeler**altında **Tek Oturum Açma'ya** gidin.

    ![Tek İşaret-On'u Yapılandır](./media/purecloud-by-genesys-tutorial/configure01.png)

1. **ADFS/Azure AD(Premium)** sekmesine geçin ve aşağıdaki adımları izleyin:

    ![Tek İşaret-On'u Yapılandır](./media/purecloud-by-genesys-tutorial/configure02.png)

    a. Azure portalından indirdiğiniz base-64 kodlanmış sertifikayı **ADFS Sertifikası'na**yüklemek için **Gözat'ı** seçin.

    b. **ADFS Veren URI** kutusuna, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın** değerini yapıştırın.

    c. Hedef **URI** kutusuna, Azure portalından kopyaladığınız **Giriş URL** değerini yapıştırın.

    d. **Güvenen Taraf Tanımlayıcı** değeri için Azure portalına gidin ve ardından **Genesys** uygulama tümleştirme sayfasında PureCloud'da **Özellikler** sekmesini seçin ve **Uygulama Kimliği** değerini kopyalayın. **Güvenen Parti Tanımlayıcı** kutusuna yapıştırın.

    ![Tek İşaret-On'u Yapılandır](./media/purecloud-by-genesys-tutorial/configure06.png)

    e. **Kaydet'i**seçin.

### <a name="create-purecloud-by-genesys-test-user"></a>Genesys test kullanıcısı tarafından PureCloud oluşturma

Azure AD kullanıcılarının Genesys tarafından PureCloud'da oturum açabilmeleri için Genesys tarafından PureCloud'a dahil edilmeleri gerekir. Genesys tarafından PureCloud'da, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları izleyin:**

1. Genesys tarafından PureCloud'da yönetici olarak oturum açın.

1. En üstte **Yönetici'yi** seçin ve **Kişiler & İzinleri**Altındaki **Kişiler'e** gidin.

    ![Tek İşaret-On'u Yapılandır](./media/purecloud-by-genesys-tutorial/configure03.png)

1. **Kişiler** sayfasında Kişi **Ekle'yi**seçin.

    ![Tek İşaret-On'u Yapılandır](./media/purecloud-by-genesys-tutorial/configure04.png)

1. **Kuruluşa Kişi Ekle** iletişim kutusunda aşağıdaki adımları izleyin:

    ![Tek İşaret-On'u Yapılandır](./media/purecloud-by-genesys-tutorial/configure05.png)

    a. Tam **Ad** kutusuna bir kullanıcının adını girin. Örneğin: **B.simon**.

    b. **E-posta** kutusuna, kullanıcının e-postasını girin. Örneğin: **b.simon\@contoso.com**.

    c. **Oluştur'u**seçin.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki **PureCloud by Genesys** döşemesini seçtiğinizde, SSO'yu ayarladığınız Genesys hesabı tarafından PureCloud'da otomatik olarak oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure AD ile nasıl tümleştirilenlerle ilgili öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure AD ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure AD'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Genesys tarafından PureCloud'u deneyin](https://aad.portal.azure.com/)
