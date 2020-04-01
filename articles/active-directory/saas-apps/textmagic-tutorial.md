---
title: 'Öğretici: TextMagic ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve TextMagic arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3e5b49d2-7096-46bc-a9ce-90e09177ba28
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca7b900a0e2203fffb5240acd4ab5201fc9d52d7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72533022"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-textmagic"></a>Öğretici: TextMagic ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, TextMagic'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. TextMagic'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* TextMagic erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla TextMagic'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* TextMagic tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **TextMagic, IDP'nin** başlattığı SSO'ya destek verdi

* TextMagic **Just In Time** kullanıcı sağlama destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-textmagic-from-the-gallery"></a>Galeriden TextMagic ekleme

TextMagic'in Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Yönetilen SaaS uygulamaları listenize TextMagic eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **TextMagic** yazın.
1. Sonuç panelinden **TextMagic'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-textmagic"></a>TextMagic için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak TextMagic ile yapılandırın ve test edin. SSO'nun çalışması için TextMagic'te bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu TextMagic ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[TextMagic SSO'yu yapılandırın](#configure-textmagic-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[TextMagic test kullanıcısını oluşturun](#create-textmagic-test-user)** - TextMagic'te Kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **TextMagic** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    **Tanımlayıcı** metin kutusuna bir URL yazın:`https://my.textmagic.com/saml/metadata`

5. TextMagic uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir, **nameidentifier** **user.userprincipalname**ile eşlenir gibi. TextMagic uygulaması **nameidentifier** **user.mail**ile eşlenen bekliyor , bu yüzden **düzenleme** simgesine tıklayarak öznitelik eşleme düzenlemeniz ve öznitelik eşleme değiştirmek gerekir.

    ![image](common/edit-attribute.png)

1. Yukarıdakilere ek olarak, TextMagic uygulaması aşağıda gösterilen SAML yanıtı geri geçirilecek birkaç daha fazla öznitelikleri bekliyor. Bu öznitelikler de önceden doldurulur, ancak gereksiniminize göre bunları gözden geçirebilirsiniz.

    | Adı |   Kaynak Özniteliği| Ad Alanı  |
    | --------------- | --------------- | --------------- |
    | şirket | user.companyname | http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | firstName               | user.givenname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | lastName            | user.surname |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |
    | telefon               | kullanıcı.telefon numarası |  http://schemas.xmlsoap.org/ws/2005/05/identity/claims |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **TextMagic'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, TextMagic'e erişim sağlayarak B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **TextMagic'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="configure-textmagic-sso"></a>Yapılandırma TextMagic SSO

1. TextMagic içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Setup TextMagic'e** tıklayın ve sizi TextMagic uygulamasına yönlendirecektir. Buradan TextMagic'e oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-5 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. TextMagic'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve TextMagic şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Kullanıcı adı altında **Hesap ayarlarını** seçin.

    ![TextMagic Yapılandırma](./media/textmagic-tutorial/config1.png)

5. **Tek Oturum Açma (SSO)** sekmesine tıklayın ve aşağıdaki alanları doldurun:  

    ![TextMagic Yapılandırma](./media/textmagic-tutorial/config2.png)

    a. **Kimlik sağlayıcısı Entity ID:** textbox'ta, Azure portalından kopyaladığınız Azure AD **Tanımlayıcısı'nın**değerini yapıştırın.

    b. **Kimlik sağlayıcısı SSO URL'de:** textbox, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.

    c. **Kimlik sağlayıcısı SLO URL'de:** textbox, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    d. Azure portalından indirilen not defterinde **base-64 kodlu sertifikanızı** açın, içeriğini panonuza kopyalayın ve ardından **Public x509 sertifikasına yapıştırın:** textbox.

    e. **Kaydet**'e tıklayın.

### <a name="create-textmagic-test-user"></a>TextMagic test kullanıcıoluşturma

Uygulama tam **zamanında kullanıcı sağlama** ve kimlik doğrulama sonrası kullanıcıları destekler otomatik olarak uygulamada oluşturulacaktır. Alt hesabı sisteme etkinleştirmek için ilk girişte bilgileri bir kez doldurmanız gerekir.
Bu bölümde sizin için bir eylem öğesi yoktur.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki TextMagic döşemesini tıklattığınızda, SSO'yu kurduğunuz TextMagic'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile TextMagic'i deneyin](https://aad.portal.azure.com/)

