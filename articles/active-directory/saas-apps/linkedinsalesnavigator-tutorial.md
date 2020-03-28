---
title: 'Öğretici: LinkedIn Sales Navigator ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve LinkedIn Sales Navigator arasında tek oturum açma işlemlerini nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0d8293b23559860e70191576db13c3cd14f520e6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75430894"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-linkedin-sales-navigator"></a>Öğretici: LinkedIn Sales Navigator ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, LinkedIn Sales Navigator'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. LinkedIn Sales Navigator'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* LinkedIn Satış Gezgini'ne erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla LinkedIn Sales Navigator'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* LinkedIn Sales Navigator tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* LinkedIn Sales Navigator **SP ve IDP** başlatılan SSO destekler
* LinkedIn Sales Navigator **Just In Time** kullanıcı sağlama destekler
* LinkedIn Sales Navigator [ **Otomatik** kullanıcı sağlama destekler](linkedinsalesnavigator-provisioning-tutorial.md)

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>Galeriden LinkedIn Satış Gezgini Ekleme

LinkedIn Sales Navigator'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize LinkedIn Sales Navigator eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **LinkedIn Sales Navigator** yazın.
1. Sonuç panelinden **LinkedIn Sales Navigator'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-linkedin-sales-navigator"></a>LinkedIn Satış Gezgini için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu LinkedIn Sales Navigator ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, LinkedIn Sales Navigator'daki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu LinkedIn Sales Navigator ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[LinkedIn Sales Navigator SSO'yu uygulama](#configure-linkedin-sales-navigator-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * **[LinkedIn Sales Navigator test kullanıcısını oluşturun](#create-linkedin-sales-navigator-test-user)** - LinkedIn Sales Navigator'da kullanıcının Azure AD gösterimine bağlı B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **LinkedIn Sales Navigator** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, **Entity ID** değerini girin, bu öğreticide daha sonra açıklanan Linkedin Portalı'ndan Entity ID değerini kopyalayaceksiniz.

    b. **Yanıtla URL** metin kutusuna, **Assertion Consumer Access (ACS) Url** değerini girin, bu öğreticide daha sonra açıklanan Linkedin Portalı'ndan Assertion Consumer Access (ACS) Url değerini kopyalayaceksiniz.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator`

1. LinkedIn Sales Navigator uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, LinkedIn Sales Navigator uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | --- | --- |
    | e-posta| kullanıcı.posta |
    | bölüm| user.department |
    | Ad| user.givenname |
    | Soyadı| user.surname |
    | Benzersiz Kullanıcı Tanımlayıcısı | kullanıcı.posta |

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **LinkedIn Satış Gezinme Aracı'nı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın LinkedIn Sales Navigator'a erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **LinkedIn Sales Navigator'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-linkedin-sales-navigator-sso"></a>LinkedIn Satış Gezgini SSO'da yapılandırın

1. Farklı bir web tarayıcısı penceresinde, **LinkedIn Sales Navigator** web sitenize yönetici olarak oturum açın.

1. **Hesap Merkezi'nde** **Ayarlar**altında **Genel Ayarlar'ı** tıklatın. Ayrıca, açılan listeden **Sales Navigator'ı** seçin.

    ![Tek İşaret-On'u Yapılandır](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

1. Formdan tek tek alanları yüklemek ve kopyalamak ve aşağıdaki adımları gerçekleştirmek **için OR'a tıklayın:**

    ![Tek İşaret-On'u Yapılandır](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

    a. **Varlık Kimliği'ni** kopyalayın ve Azure portalındaki Temel **SAML Yapılandırması'ndaki** **Tanımlayıcı** metin kutusuna yapıştırın.

    b. **İddia Tüketici Erişimi (ACS) Url'sini** kopyalayın ve Azure portalındaki Temel **SAML** Yapılandırması'ndaki **YanıtLA URL** metin kutusuna yapıştırın.

1. **LinkedIn Yönetici Ayarları** bölümüne gidin. Azure portalından indirdiğiniz **XML dosyasını Upload XML dosya** seçeneğine tıklayarak yükleyin.

    ![Tek İşaret-On'u Yapılandır](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

1. SSO'ya etkinleştirmek için **Üzerine** tıklayın. Bağlı Olmayan'dan **Bağlı** **Olmayan'a** SSO durumu değişir

    ![Tek İşaret-On'u Yapılandır](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)

### <a name="create-linkedin-sales-navigator-test-user"></a>LinkedIn Sales Navigator test kullanıcısı oluşturma

Bağlantılı Satış Gezgini Uygulaması, Uygulamada otomatik olarak kimlik doğrulama kullanıcıları oluşturulduktan sonra Tam Zamanında (JIT) kullanıcı sağlamayı destekler. Kullanıcıya lisans atamak için otomatik olarak lisans **atamayı** etkinleştirin.

   ![Azure AD test kullanıcısı oluşturma](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki LinkedIn Sales Navigator döşemesini tıklattığınızda, SSO'yu kurduğunuz LinkedIn Sales Navigator'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile LinkedIn Satış Gezgini'ni deneyin](https://aad.portal.azure.com/)