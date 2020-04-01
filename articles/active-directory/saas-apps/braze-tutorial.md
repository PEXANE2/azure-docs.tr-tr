---
title: 'Öğretici: Braze ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Braze arasında tek oturum açma işlemlerini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: e8748f49-9a4a-4a9e-a196-dc8311f15f13
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/10/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 52a53735ebf8e0e4affa7daf6c345de6bf467228
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72373448"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-braze"></a>Öğretici: Braze ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Braze'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Braze'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Braze'ye erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Braze'de otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Braze tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Braze **SP ve IDP'nin** SSO'ya başlattığı destekler

## <a name="adding-braze-from-the-gallery"></a>Galeriden Braze ekleme

Braze'nin Azure AD'ye entegrasyonunu yapılandırmak için galeriden Braze'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Braze** yazın.
1. Sonuçlar panelinden **Braze'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-braze"></a>Braze için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu Braze ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Braze'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Braze ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Braze SSO'yu yapılandırır](#configure-braze-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Braze test kullanıcısını oluşturun](#create-braze-test-user)** - Braze'deki B.Simon'ın, kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Braze** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.braze.com/auth/saml/callback `

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.braze.com/sign_in`

    > [!NOTE]
    > Alt etki alanı için Braze örnek URL'nizde listelenen koordinasyon alt etki alanını kullanın. Örneğin, örneğinIZ US-01 ise, URL'niz https://dashboard-01.braze.com. Bu, alt etki alanınızın pano-01 olacağı anlamına gelir.

1. Braze uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Braze uygulaması aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | -----| --------------- |
    | e-posta | user.userprincipalname |
    | Ilk_ad| user.givenname|
    | last_name| user.surname|

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Braze'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Braze'ye erişim izni vererek B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Braze'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-braze-sso"></a>Braze SSO'nun yapılandırılması

**Braze** tarafında tek oturum açma yapılandırması için, braze hesap yöneticisinin hesabınız için SAML SSO'ya olanak sağladığından emin olmanız gerekir. Etkinleştirildikten sonra Şirket Ayarları > Güvenlik Ayarları'na gidebilir ve SAML SSO bölümünü AÜZERINE değiştirebilirsiniz. Bu bölümde, indirilen **Sertifikayı (Base64)** kopyalamanız ve yapıştırmanız ve bir SAML Adı eklemeniz gerekir.

### <a name="create-braze-test-user"></a>Braze test kullanıcısı oluşturma

Bu bölümde, Braze'de B.Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları Braze platformuna eklemek için [Braze destek ekibiyle](mailto:support@braze.com) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Braze döşemesini tıklattığınızda, SSO'yu kurduğunuz Braze'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Braze'yi deneyin](https://aad.portal.azure.com/)