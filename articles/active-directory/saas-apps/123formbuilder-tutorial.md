---
title: 'Öğretici: 123FormBuilder SSO ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve 123FormBuilder SSO arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/14/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 947a9d632089b18f6b950c5eecbcb74d061f32eb
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81274217"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-123formbuilder-sso"></a>Öğretici: 123FormBuilder SSO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, 123FormBuilder SSO'nun Azure Active Directory (Azure AD) ile nasıl entegre edileceğinizi öğreneceksiniz. 123FormBuilder SSO'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* 123FormBuilder SSO erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla 123FormBuilder SSO'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* 123FormBuilder SSO tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* 123FormBuilder SSO **SP ve IDP** SSO başlatılan destekler.
* 123FormBuilder SSO **Just In Time** kullanıcı sağlama destekler.
* 123FormBuilder SSO'u yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-123formbuilder-sso-from-the-gallery"></a>Galeriden 123FormBuilder SSO ekleme

123FormBuilder SSO'nun Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize 123FormBuilder SSO eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **123FormBuilder SSO** yazın.
1. Sonuç panelinden **123FormBuilder SSO'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-123formbuilder-sso"></a>123FormBuilder SSO için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak 123FormBuilder SSO ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure AD kullanıcısı ile 123FormBuilder SSO'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu 123FormBuilder SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[123FormBuilder SSO'yu yapılandırın](#configure-123formbuilder-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[123FormBuilder SSO test kullanıcıoluşturun](#create-123formbuilder-sso-test-user)** - kullanıcının Azure AD gösterimi ile bağlantılı 123FormBuilder SSO B.Simon bir meslektaşı olması.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **123FormBuilder SSO** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırma** sı bölümünde, Uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımları gerçekleştirin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/metadata`


    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/acs`

5. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://www.123formbuilder.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değeri, öğreticide daha sonra açıklanan gerçek URL'lerden ve Tanımlayıcı'dan güncelleştirmeniz gerekir.

1. **SAML** Ile Kurulum tek oturum açma bölümünde, **SAML İmza Sertifikası** bölümünde, **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Set **up 123FormBuilder SSO** bölümünde, gereksiniminize göre uygun URL(ler) kopyalayın.

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

Bu bölümde, B.Simon'ın 123FormBuilder SSO'ya erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar **listesinde, 123FormBuilder SSO'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-123formbuilder-sso"></a>123FormBuilder SSO'ya yapılandırın

1. **123FormBuilder SSO** tarafında tek oturum açma yapılandırmak [https://www.123formbuilder.com/form-2709121/](https://www.123formbuilder.com/form-2709121/) için aşağıdaki adımları gidin ve gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/123formbuilder-tutorial/submit.png) 

    a. **E-posta** metin kutusuna, kullanıcının `B.Simon@Contoso.com`e-postasını yazın.

    b. Azure portalından indirdiğiniz Metadata XML dosyasına **Yükle'yi** tıklatın ve göz atın.

    c. **FORMU GÖNDER'e**tıklayın.

2. Microsoft **Azure AD'da - Tek oturum açma - Uygulama Ayarlarını Yapılandır:** Aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/123formbuilder-tutorial/url3.png)

    a. Uygulamayı **IDP tarafından başlatılan modda**yapılandırmak istiyorsanız, örneğiniz için **IDENTIFIER** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırma** bölümündeki **Tanımlayıcı** textbox'a yapıştırın.

    b. Uygulamayı **IDP tarafından başlatılan modda**yapılandırmak istiyorsanız, örneğiniz için **YANıT URL** değerini kopyalayın ve Azure portalındaki **Temel SAML Yapılandırması** **bölümündeki YanıtURL** textbox'ına yapıştırın.

    c. Uygulamayı **SP tarafından başlatılan modda**yapılandırmak istiyorsanız, örneğiniz için **SIGN ON URL** değerini kopyalayın ve Azure portalındaki Temel **SAML Yapılandırma** bölümünde Oturum Aç **URL** textbox'a yapıştırın.

### <a name="create-123formbuilder-sso-test-user"></a>123FormBuilder SSO test kullanıcıoluşturma

Bu bölümde, Britta Simon adlı bir kullanıcı 123FormBuilder SSO oluşturulur. 123FormBuilder SSO varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlama destekler. Bu bölümde sizin için bir eylem öğesi yoktur. Bir kullanıcı zaten 123FormBuilder SSO yoksa, yeni bir kimlik doğrulama sonra oluşturulur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Panelindeki 123FormBuilder SSO döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz 123FormBuilder SSO'ya oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile 123FormBuilder SSO'u deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
