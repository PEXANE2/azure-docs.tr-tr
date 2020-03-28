---
title: 'Öğretici: SD Elements ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Etkin Dizin ve SD Elements arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: f0386307-bb3b-4810-8d4b-d0bfebda04f4
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1a387659e2375444fd32cf731ab4bccc210b669a
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74081674"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sd-elements"></a>Öğretici: SD Elements ile Azure Active Directory tek oturum açma (SSO) tümleştirmesi

Bu eğitimde, SD Öğelerini Azure Etkin Dizini (Azure AD) ile nasıl tümleştireceğinizi öğreneceksiniz. SD Öğeleriazure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SD Elements'a erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla SD Elements'ta otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SD Elements tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SD Elements **IDP** başlatılan SSO destekler

## <a name="adding-sd-elements-from-the-gallery"></a>Galeriden SD Öğeleri Ekleme

SD Elements'In Azure AD'ye entegrasyonunu yapılandırmak için galerideki SD Öğeleri'ni yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **SD Öğeleri** yazın.
1. Sonuç panelinden **SD Elements'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-single-sign-on-for-sd-elements"></a>SD Elements için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak SD Elements ile yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile SD Elements'taki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu SD Elements ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[SD Elements SSO'yu yapılandırın.](#configure-sd-elements-sso)**
    * **[SD Elements test kullanıcısını oluşturun](#create-sd-elements-test-user)** - Kullanıcının Azure AD gösterimine bağlı SD Elements'ta B.Simon'Un bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **SD Elements** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da** aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenantname>.sdelements.com/sso/saml2/metadata`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<tenantname>.sdelements.com/sso/saml2/acs/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [SD Elements İstemci destek ekibine](mailto:support@sdelements.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. SD Elements uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, SD Elements uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı |  Kaynak Özniteliği|
    | --- | --- |
    | e-posta |kullanıcı.posta |
    | Ad |user.givenname |
    | Soyadı |user.surname |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **SD Elements'ı ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın SD Elements'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **SD Elements'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-sd-elements-sso"></a>SD Elemanlarını Yapılandırma SSO

1. Tek oturum açma özelliğini etkinleştirmek için [SD Elements destek ekibinizle](mailto:support@sdelements.com) iletişime geçin ve indirilen sertifika dosyasını sağlayın.

1. Farklı bir tarayıcı penceresinde, Yönetici olarak SD Elements kiracınızı oturum açın.

1. Üstteki menüde **Sistem'e**ve ardından **Tek Oturum Açma'ya**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/sd-elements-tutorial/tutorial_sd-elements_09.png)

1. Tek **Oturum Açma Ayarları** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/sd-elements-tutorial/tutorial_sd-elements_10.png)

    a. **SSO Türü**olarak, **SAML'yi**seçin.

    b. Kimlik **Sağlayıcı Varlık Kimliği** metin kutusuna, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    c. Kimlik **Sağlayıcı Tek Oturum Açma Hizmeti** metin kutusuna, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin**değerini yapıştırın.

    d. **Kaydet**'e tıklayın.

### <a name="create-sd-elements-test-user"></a>SD Elements test kullanıcısı oluşturma

Bu bölümün amacı SD Elements b.simon adlı bir kullanıcı oluşturmaktır. SD Elements durumunda, SD Elements kullanıcıları oluşturmak el ile bir görevdir.

**SD Elements'ta B.Simon oluşturmak için aşağıdaki adımları gerçekleştirin:**

1. Bir web tarayıcısı penceresinde, yönetici olarak SD Elements şirket sitenize oturum açın.

1. Üstteki **menüde, Kullanıcı Yönetimi'ni**ve ardından **Kullanıcılar'ı**tıklatın.

    ![SD Elements test kullanıcısı oluşturma](./media/sd-elements-tutorial/tutorial_sd-elements_11.png) 

1. **Yeni Kullanıcı Ekle'yi**tıklatın.

    ![SD Elements test kullanıcısı oluşturma](./media/sd-elements-tutorial/tutorial_sd-elements_12.png)

1. Yeni **Kullanıcı Ekle** iletişim kutusunda aşağıdaki adımları gerçekleştirin:

    ![SD Elements test kullanıcısı oluşturma](./media/sd-elements-tutorial/tutorial_sd-elements_13.png) 

    a. **E-posta** textbox, gibi **b.simon@contoso.com**kullanıcının e-posta girin.

    b. Ad **metin** kutusuna, B gibi kullanıcının ilk adını **girin.**

    c. **Soyadı** metin kutusuna, **Simon**gibi kullanıcının soyadını girin.

    d. **Rol**olarak, **Kullanıcı'yı**seçin.

    e. **Kullanıcı Oluştur'u**tıklatın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki SD Elements döşemesini tıklattığınızda, SSO'yu kurduğunuz SD Elements'ta otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SD Öğelerini Deneyin](https://aad.portal.azure.com/)