---
title: 'Öğretici: Grovo ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Grovo arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 399cecc3-aa62-4914-8b6c-5a35289820c1
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fb462905d1f8700214a0c4fc0c2d0bcb3e70a0f
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "70163503"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-grovo"></a>Öğretici: Grovo ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Grovo'nun Azure Etkin Dizini (Azure AD) ile nasıl entegre edilebildiğini öğreneceksiniz. Grovo'yı Azure AD ile entegre ettiğinizde, şunları yapabilirsiniz:

* Grovo erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Grovo'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Grovo tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Grovo **SP ve IDP'nin** Başlattığı SSO'ya destek verdi
* Grovo **Just In Time** kullanıcı sağlama destekler

## <a name="adding-grovo-from-the-gallery"></a>Galeriden Grovo ekleme

Grovo'nun Azure AD'ye entegrasyonunu yapılandırmak için, Grovo'yu galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Grovo** yazın.
1. Sonuçlar panelinden **Grovo'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-grovo"></a>Grovo için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Grovo ile yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Grovo'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Grovo ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Grovo SSO'yu yapılandırır](#configure-grovo-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. Grovo'da B.Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için **[Grovo test kullanıcısı oluşturun.](#create-grovo-test-user)**
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Grovo** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.grovo.com/sso/saml2/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

    c. **Ek URL'ler ayarla'yı**tıklatın.

    d. **Röle Durumu** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.grovo.com`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımları gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<subdomain>.grovo.com/sso/saml2/saml-assertion`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, Yanıt URL'si, Oturum Açma URL'si ve Röle Durumu ile güncelleştirin. Bu değerleri almak için [Grovo İstemci destek ekibine](https://www.grovo.com/contact-us) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Grovo'yu Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Grovo'ya erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Grovo'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-grovo-sso"></a>Grovo SSO'nun yapılandırılsın

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Grovo'da oturum açın.

2. **Yönetici** > **Tümleştirmeleri'ne**gidin.
 
    ![Grovo Yapılandırması](./media/grovo-tutorial/tutorial_grovo_admin.png) 

3. **SP Başlatılan SAML 2.0** bölümü altında **KURULUM'u** tıklatın.

    ![Grovo Yapılandırması](./media/grovo-tutorial/tutorial_grovo_setup.png)

4. **SP Başlatılan SAML 2.0** açılır penceresi aşağıdaki adımları gerçekleştirin:

    ![Grovo Yapılandırması](./media/grovo-tutorial/tutorial_grovo_saml.png)

    a. Entity **ID** textbox'a, Azure portalından kopyaladığınız **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    b. Tek **oturum açma hizmeti bitiş noktası textbox'ına,** Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. **Tek oturum açma hizmeti bitiş noktası bağlamayı** ' olarak `urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect`seçin.
    
    d. Azure portalından indirilen **Base64 kodlu sertifikayı** not defterine açın ve **Ortak anahtar** metin kutusuna yapıştırın.

    e. **İleri**'ye tıklayın.

### <a name="create-grovo-test-user"></a>Grovo test kullanıcısı oluşturma

Bu bölümde Grovo'da B.Simon adında bir kullanıcı oluşturuldu. Grovo, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Grovo'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!Note]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Grovo destek ekibine](https://www.grovo.com/contact-us)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Grovo döşemesini tıklattığınızda, SSO'yu kurduğunuz Grovo'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Grovo'u deneyin](https://aad.portal.azure.com/)