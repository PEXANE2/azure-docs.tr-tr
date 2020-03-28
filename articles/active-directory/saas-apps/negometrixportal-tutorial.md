---
title: 'Öğretici: NegometrixPortal Tek Oturum Açma (SSO) ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve NegometrixPortal Tek İşaret Açma (SSO) arasında tek oturum açma yı nasıl yapılandırıştırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 99429415-778f-49c0-a267-575fb61e59b2
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/06/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 97cc8e4a69155dcce402d34d3d1afba18f74d60e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74085460"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-negometrixportal-single-sign-on-sso"></a>Öğretici: NegometrixPortal Tek Oturum Açma (SSO) ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, NegometrixPortal Tek İşaret Açma 'yı (SSO) Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. NegometrixPortal Tek Oturum Açma (SSO) ile Azure AD'yi entegre ettiğinizde şunları yapabilirsiniz:

* NegometrixPortal Tek Oturum Açma (SSO) erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla NegometrixPortal Tek Oturum Açma (SSO) ile otomatik olarak oturum açmalarını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* NegometrixPortal Tek Oturum Açma (SSO) tek oturum açma (SSO) aboneliği ni etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* NegometrixPortal Tek İşaret On (SSO) **SP** başlatılan SSO destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-negometrixportal-single-sign-on-sso-from-the-gallery"></a>Galeriden NegometrixPortal Tek İşaret Açma (SSO) ekleme

NegometrixPortal Tek Oturum Açma'nın (SSO) Azure AD'ye entegrasyonunu yapılandırmak için galeriden Yönetilen SaaS uygulamaları listenize NegometrixPortal Tek İşaret Açma 'yı (SSO) eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **NegometrixPortal Tek İşaret Açma (SSO)** yazın.
1. Sonuç panelinden **NegometrixPortal Tek Oturum Açma'yı (SSO)** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-negometrixportal-single-sign-on-sso"></a>NegometrixPortal Tek Oturum Açma (SSO) için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak NegometrixPortal Tek Oturum Açma (SSO) ile yapılandırın ve test edin. SSO'nun çalışması için, NegometrixPortal Tek İşaret Açık'ta (SSO) bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu NegometrixPortal Tek Oturum Açma (SSO) ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[NegometrixPortal Tek İşaret Açma (SSO) SSO'yu uygulama](#configure-negometrixportal-single-sign-on-sso-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * **[NegometrixPortal Tek İşaret Açma (SSO) test kullanıcısını oluşturun](#create-negometrixportal-single-sign-on-sso-test-user)** - Kullanıcının Azure AD gösterimine bağlı NegometrixPortal Tek İşaret Açma 'da (SSO) B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **NegometrixPortal Tek Oturum Açma (SSO)** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma**seçeneğini seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://portal.negometrix.com/sso/<CUSTOMURL>`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [NegometrixPortal Tek İşaret Açma (SSO) İstemci destek ekibiyle](mailto:sander.hoek@negometrix.com) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. NegometrixPortal Tek İşaret Açma (SSO) uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıda ek olarak, NegometrixPortal Tek İşaret On (SSO) uygulaması aşağıda gösterilen SAML yanıtı geri geçirilmesi için birkaç öznitelikleri bekliyor. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | ---------------|  --------- |
    | Upn | user.userprincipalname |

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

Bu bölümde, B.Simon'ın NegometrixPortal Tek Oturum Açma 'ya (SSO) erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama **listesinde, NegometrixPortal Tek İşaret Açma (SSO)** seçeneğini belirleyin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-negometrixportal-single-sign-on-sso-sso"></a>Yapılandırılan NegometrixPortal Tek İşaret On (SSO) SSO

**NegometrixPortal Tek İşaret Açma (SSO)** tarafında tek oturum açma yı yapılandırmak için, **App Federation Metadata Url'sini** [NegometrixPortal Tek Sign On (SSO) destek ekibine](mailto:sander.hoek@negometrix.com)göndermeniz gerekmektedir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-negometrixportal-single-sign-on-sso-test-user"></a>NegometrixPortal Tek İşaret Açma (SSO) test kullanıcısı oluşturma

Bu bölümde, NegometrixPortal Tek İşaret Açma (SSO) adlı b.simon adında bir kullanıcı oluşturursunuz.  [NegometrixPortal Tek İşaret Açma (SSO) destek ekibiyle](mailto:sander.hoek@negometrix.com) çalışarak Kullanıcıları NegometrixPortal Tek Oturum Açma (SSO) platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki NegometrixPortal Tek İşaret Açma (SSO) döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz NegometrixPortal Tek Oturum Açma 'da (SSO) oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile NegometrixPortal Tek İşaret Açma (SSO) deneyin](https://aad.portal.azure.com/)