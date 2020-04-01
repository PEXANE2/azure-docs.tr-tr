---
title: 'Öğretici: Nuclino ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Nuclino arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 74bbab82-5581-4dcf-8806-78f77c746968
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0a75869c257e6b875a00036218b05db5521e8d0b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72532959"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-nuclino"></a>Öğretici: Nuclino ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Nuclino'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edileceğinizi öğreneceksiniz. Nuclino'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Nuclino erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Nuclino'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Nuclino tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Nuclino **SP ve IDP** sso başlatılan destekler
* Nuclino **Just In Time** kullanıcı sağlama destekler

## <a name="adding-nuclino-from-the-gallery"></a>Galeriden Nuclino ekleme

Nuclino'nun Azure AD'ye entegrasyonunu yapılandırmak için galeriden Nuclino'yu yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Nuclino** yazın.
1. Sonuç panelinden **Nuclino'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-nuclino"></a>Nuclino için Azure AD oturumunu yapılandırma ve test

Azure AD SSO'nu Nuclino ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Nuclino'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Nuclino ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Nuclino SSO'yu yapılandırın](#configure-nuclino-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Nuclino test kullanıcısını oluşturun](#create-nuclino-test-user)** - Kullanıcının Azure AD gösterimine bağlı Nuclino'daki B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Nuclino** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://api.nuclino.com/api/sso/<UNIQUE-ID>/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://api.nuclino.com/api/sso/<UNIQUE-ID>/acs`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, bu öğreticide daha sonra açıklanan **Kimlik Doğrulama** bölümünden gerçek Tanımlayıcı ve Yanıt URL'si ile güncelleştirin.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://app.nuclino.com/<UNIQUE-ID>/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Nuclino Client destek ekibine](mailto:contact@nuclino.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

6. Nuclino uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/edit-attribute.png)

7. Yukarıdakilere ek olarak, Nuclino uygulaması aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | ---------------| --------- |
    | Ilk_ad | user.givenname |
    | last_name | user.surname |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Nuclino'yu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Nuclino'ya erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Nuclino'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-nuclino-sso"></a>Nuclino SSO'nun yapılandırılması

1. Nuclino içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı**yükleyin'e tıklayarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten sonra, **Nuclino'yu** Ayarla'yı tıklatın ve sizi Nuclino uygulamasına yönlendirecektir. Buradan, Nuclino'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-7 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Nuclino'yu el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Nuclino şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. **ICON'a**tıklayın.

    ![Nuclino Yapılandırma](./media/nuclino-tutorial/configure1.png)

5. **Azure AD SSO'ya** tıklayın ve açılır yerden **Takım ayarlarını** seçin.

    ![Nuclino Yapılandırma](./media/nuclino-tutorial/configure2.png)

6. Sol gezinti bölmesinden **Kimlik Doğrulama'yı** seçin.

    ![Nuclino Yapılandırma](./media/nuclino-tutorial/configure3.png)

7. Kimlik **Doğrulama** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Nuclino Yapılandırma](./media/nuclino-tutorial/configure4.png)

    a. **SAML tabanlı tek oturum açma (SSO) seçeneğini**belirleyin.

    b. **ACS URL'sini kopyalayın (Bunu SSO sağlayıcınıza kopyalayıp yapıştırmalısınız)** değerine ekleyip Azure portalındaki **Temel SAML Yapılandırması** bölümünün **YanıtURL** metin kutusuna yapıştırın.

    c. Varlık Kimliğini Kopyala **(Bunu SSO sağlayıcınıza kopyalayıp yapıştırmanız gerekir)** değerine ve Azure portalındaki **Temel SAML Yapılandırmabölümünün** **Tanımlayıcı** metin kutusuna yapıştırmanız gerekir.

    d. **SSO URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    e. Entity **ID** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    f. İndirilen **Sertifika(Base64)** dosyanızı Notepad'de açın. İçeriğini panonuza kopyalayın ve **ardından Ortak sertifika** metin kutusuna yapıştırın.

    g. **DEĞIŞIKLIKLERI KAYDET'i**tıklatın.

### <a name="create-nuclino-test-user"></a>Nuclino test kullanıcısı oluşturma

Bu bölümde, B.Simon adlı bir kullanıcı Nuclino oluşturulur. Nuclino, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Nuclino'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Nuclino destek ekibine](mailto:contact@nuclino.com)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Nuclino döşemesini tıklattığınızda, SSO'yu kurduğunuz Nuclino'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Nuclino'u deneyin](https://aad.portal.azure.com/)

