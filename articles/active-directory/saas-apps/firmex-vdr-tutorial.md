---
title: 'Öğretici: Firmex VDR ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Firmex VDR arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 670ff192-c23e-49e4-8fd1-516e02d8856c
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/21/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdfb857d3a68081fda84aef33e6b5a4b4d1bce28
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76761241"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-firmex-vdr"></a>Öğretici: Firmex VDR ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Firmex VDR'yi Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Firmex VDR'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Firmex VDR erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Firmex VDR ile otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Firmex VDR tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Firmex VDR **SP ve IDP** sso başlatılan destekler

* Firmex'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimleri Koşullu Erişim'den itibaren genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-firmex-vdr-from-the-gallery"></a>Galeriden Firmex VDR ekleme

Firmex VDR'nin Azure AD'ye entegrasyonunu yapılandırmak için galeriden firmex VDR'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Firmex VDR** yazın.
1. Sonuç panelinden **Firmex VDR'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-firmex-vdr"></a>Firmex VDR için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Firmex VDR ile yapılandırın ve test edin. SSO'nun çalışması için, Firmex VDR'deki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Firmex VDR ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Firmex VDR SSO'yu yapılandırır](#configure-firmex-vdr-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Firmex VDR test kullanıcısını oluşturun](#create-firmex-vdr-test-user)** - Firmex VDR'de kullanıcının Azure AD gösterimine bağlı B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Firmex VDR** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla ayarla tek oturum** açmada, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, uygulama Azure ile önceden entegre edilmiş olduğundan, kullanıcının herhangi bir adım gerçekleştirmesi gerekmez.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna URL'yi yazın:`https://login.firmex.com`

1. **Kaydet**'e tıklayın.

1. Firmex VDR uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıda ek olarak, Firmex VDR uygulaması aşağıda gösterilen SAML yanıtı geri geçirilmesi için birkaç öznitelikleri bekliyor. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | ------------ | --------- |
    | e-posta | kullanıcı.posta |

1. **SAML** ile Kurulum tek oturum açma bölümünde, **SAML İmza Sertifikası** bölümünde, **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. Kurulum **Firmex VDR** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Firmex VDR'ye erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama listesinde **Firmex VDR'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-firmex-vdr-sso"></a>Firmex VDR SSO'nun yapılandırılması

### <a name="before-you-get-started"></a>Başlamadan önce

#### <a name="what-youll-need"></a>İhtiyacınız olan

-   Etkin bir Firmex aboneliği
-   SSO hizmetiniz olarak Azure AD
-   SSO'nun yapılandırması için BT yöneticiniz
-   SSO etkinleştirildikten sonra, şirketinizin tüm kullanıcılarının SSO kullanarak Firmex'e giriş yapması ve oturum açma/parola kullanmaması gerekir.

#### <a name="how-long-will-this-take"></a>Bu ne kadar sürecek?

SSO'nun uygulanması birkaç dakika sürer. Firmex Support'un siteniz için SSO'ya olanak sağlaması ile şirketinizin kullanıcıları arasında SSO'nun kimlik doğrulaması neredeyse hiç kesinti yoktur. Aşağıdaki adımları takip edin.

### <a name="step-1---identify-your-companys-domains"></a>Adım 1 - Şirketinizin etki alanlarını belirleme

Şirketinizin kullanıcılarının oturum açtırdığı etki alanlarını belirleyin.

Örnek:

- @firmex.com
- @firmex.ca

### <a name="step-2---contact-firmex-support-with-your-domains"></a>Adım 2 - Etki alanlarınızla Firmex Desteği ile iletişime geçin

[Firmex Destek Ekibine](mailto:support@firmex.com) e-posta gönderveya Firmex Destek'e konuşmak için 1888 688 4042 x.11 numaralı telefonu arayın. Etki alanı bilgilerinizi iletin. Firmex Desteği, etki alanlarını vdr'nize **talep edilen etki alanları**olarak ekler. Yöneticinizin artık SSO'u yapılandırması gerekiyor.

Uyarı: Sitenizin yöneticisi talep edilen etki alanlarını yapılandırana kadar şirketinizin kullanıcıları VDR'de oturum açamayacaktır. Şirket dışı kullanıcılar (diğer bir deyişle konuk kullanıcılar) e-postalarını/parolalarını kullanarak oturum açabilirler. Yapılandırma birkaç dakika sürer.

### <a name="step-3---configure-the-claimed-domains"></a>Adım 3 - İddia edilen etki alanlarını yapılandırma

1. Site Yöneticisi olarak Firmex'e giriş yapın.
1. Sol üst köşeden şirket logonuzu tıklatın.
1. **SSO** sekmesini seçin. Ardından **SSO Yapılandırması'nı**seçin. Yapılandırmak istediğiniz etki alanını tıklatın.

    ![İddia Edilen Etki Alanları](./media/firmex-vdr-tutorial/edit-sso.png)  

1. BT Yöneticinizin aşağıdaki alanları doldurmasını iste. Alanlar kimlik sağlayıcınızdan alınmalıdır:  

    ![SSO Yapılandırması](./media/firmex-vdr-tutorial/SSO-config.png)

    a. Entity **ID** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    b. Kimlik **Sağlayıcısı URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    c. **Ortak Anahtar Sertifikası** - Kimlik doğrulama amacıyla, saml iletisi veren kuruluş tarafından dijital olarak imzalanabilir. İletideki imzayı doğrulamak için ileti alıcısı, verene ait olduğu bilinen ortak bir anahtar kullanır. Benzer şekilde, bir iletiyi şifrelemek için, nihai alıcıya ait genel bir şifreleme anahtarının veren tarafından bilinmesi gerekir. İmzalama ve şifreleme gibi her iki durumda da güvenilir ortak anahtarlar önceden paylaşılmalıdır.  Bu **Federasyon Metadata XML** **gelen X509Sertifikası**

    d. SSO yapılandırmasını tamamlamak için **Kaydet'i** tıklatın. Değişiklikler hemen etkili olur.

1. Şu anda, Siteniz için SSO etkinleştirilir.

### <a name="create-firmex-vdr-test-user"></a>Firmex VDR test kullanıcısı oluşturma

Bu bölümde Firmex'te B.Simon adında bir kullanıcı oluşturursunuz. Firmex [Destek Ekibi](mailto:support@firmex.com) ile birlikte çalışarak kullanıcıları Firmex platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Firmex VDR karosu tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz Firmex VDR ile oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Firmex VDR'yi deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Firmex'i gelişmiş görünürlük ve kontrollerle nasıl koruyabilen](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
