---
title: 'Öğretici: UniFLOW Online ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve uniFLOW Online arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 28313d27-638c-4d50-92ad-d093f2ae9ecf
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: f26af813fcd4032aabce2305ac8845307d1fca65
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76262138"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-uniflow-online"></a>Öğretici: UniFLOW Online ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, uniFLOW Online'ı Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. uniFLOW Online'ı Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* uniFLOW Online erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla uniFLOW Online'da oturum açmalarına olanak tanır.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* uniFLOW Online kiracı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* uniFLOW Online **SP** başlatılan SSO destekler

## <a name="adding-uniflow-online-from-the-gallery"></a>Galeriden uniFLOW Online ekleme

uniFLOW Online'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize uniFLOW Online eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **uniFLOW Online** yazın.
1. Sonuç panelinden **uniFLOW Online'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-uniflow-online"></a>UniFLOW Online için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak uniFLOW Online ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile uniFLOW Online'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu uniFLOW Online ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
   1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
   1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[UniFLOW Online SSO'yu yapılandırır](#configure-uniflow-online-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Oluşturulan test kullanıcısını kullanarak uniFLOW Online'da oturum açın](#sign-in-to-uniflow-online-using-the-created-test-user)** - uygulama tarafında kullanıcı oturum açma test etmek için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **uniFLOW Online** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | | |
    |-|-|
    | `https://<tenant_domain_name>.eu.uniflowonline.com`|
    | `https://<tenant_domain_name>.us.uniflowonline.com`|
    | `https://<tenant_domain_name>.sg.uniflowonline.com`|
    | `https://<tenant_domain_name>.jp.uniflowonline.com`|
    | `https://<tenant_domain_name>.au.uniflowonline.com`|

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | | |
    |-|-|
    | `https://<tenant_domain_name>.eu.uniflowonline.com`|
    | `https://<tenant_domain_name>.us.uniflowonline.com`|
    | `https://<tenant_domain_name>.sg.uniflowonline.com`|
    | `https://<tenant_domain_name>.jp.uniflowonline.com`|
    | `https://<tenant_domain_name>.au.uniflowonline.com`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [uniFLOW Online İstemci destek ekibine](mailto:support@nt-ware.com) başvurun. Ayrıca Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere veya uniFLOW Online kiracınızda görüntülenen yanıt URL'sine de bakabilirsiniz.

1. uniFLOW Online uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, uniFLOW Online uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | -----------| --------------- |
    | Displayname | user.displayname |
    | Takma | user.onpremisessamaccountname |

   > [!NOTE]
   > Bu `user.onpremisessamaccountname` özellik, yalnızca Azure AD kullanıcılarınız yerel bir Windows Etkin Dizin'den eşitlenirse bir değer içerir.

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

Bu bölümde, B.Simon'ın uniFLOW Online'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **uniFLOW Online'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümüne gidin ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

   ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

> [!NOTE]
> Tüm kullanıcıların el ile atama olmadan uygulamaya erişmesine izin vermek için **Yönet** bölümüne gidin ve **Özellikleri**seçin. Daha sonra, **Kullanıcı ataması gerekli** parametreyi **NO**olarak değiştirin.

## <a name="configure-uniflow-online-sso"></a>uniFLOW Online SSO'nun yapılandırılması

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak uniFLOW Online web sitesinde oturum açın.

1. Soldaki gezinti panelinden **Kullanıcı** sekmesini seçin.

    ![uniFLOW Online Yapılandırma](./media/uniflow-online-tutorial/configure1.png)

1. **Kimlik sağlayıcılar'ı**tıklatın.

    ![uniFLOW Online Yapılandırma](./media/uniflow-online-tutorial/configure2.png)

1. Kimlik **sağlayıcı ekle'ye**tıklayın.

    ![uniFLOW Online Yapılandırma](./media/uniflow-online-tutorial/configure3.png)

1. **KİmLİk SADIKLARI EKLE** bölümünde aşağıdaki adımları gerçekleştirin:


    ![uniFLOW Online Yapılandırma](./media/uniflow-online-tutorial/configure4.png)

    a. Görüntü adı Ex girin: *AzureAD SSO*.

    b. **Sağlayıcı türü için**açılır dosyadan **WS-Fed** seçeneğini seçin.

    c. **WS-Fed türü için**açılır dosyadan **Azure Active Directory** seçeneğini seçin.

    d. **Kaydet**'e tıklayın.

1. **Genel** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![uniFLOW Online Yapılandırma](./media/uniflow-online-tutorial/configure5.png)

    a. Görüntü adı Ex girin: *AzureAD SSO*.

    b. **ADFS Federasyonu Meta verileri**için **URL'den** seçeneğini seçin.

    c. Federasyon **Metadata URl** textbox'ına, Azure portalından kopyaladığınız **Uygulama Federasyonu Metaveri Url** değerini yapıştırın.

    d. **Etkin**olarak **Kimlik sağlayıcısını** seçin.

    e. Otomatik **kullanıcı kaydını** **Etkinleştirildi**olarak seçin.

    f. **Kaydet**'e tıklayın.

### <a name="sign-in-to-uniflow-online-using-the-created-test-user"></a>Oluşturulan test kullanıcısını kullanarak uniFLOW Online'da oturum açın

1. Farklı bir web tarayıcısı penceresinde, kiracınız için uniFLOW Online URL'ye gidin.

1. Azure AD örneğiniz aracılığıyla oturum açmanız için daha önce oluşturulmuş kimlik sağlayıcısını seçin.

1. Test kullanıcısını kullanarak oturum açın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile uniFLOW Online'ı deneyin](https://aad.portal.azure.com/)
