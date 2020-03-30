---
title: 'Öğretici: Logz.io ile Azure Active Directory tek oturum açma (SSO) entegrasyonu - Mühendisler için Bulut Gözlemlenebilirliği | Microsoft Dokümanlar'
description: Azure Active Directory ile Logz.io - Mühendisler için Bulut Gözlemlenebilirliği arasında tek oturum açma yı nasıl yapılandırılabildiğini öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b0984471-d12f-4f58-896e-194ea45b01fd
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/26/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: de1c929ffa790d2abe3a1922cecc2175cd7a8e12
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80385483"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-logzio---cloud-observability-for-engineers"></a>Öğretici: Logz.io ile Azure Active Directory tek oturum açma (SSO) entegrasyonu - Mühendisler için Bulut Gözlemlenebilirliği

Bu eğitimde, Logz.io - Mühendisler için Bulut Gözlemlenebilirliği ve Azure Etkin Dizin (Azure AD) ile nasıl entegre edileceksiniz. Logz.io - Mühendisler için Bulut Gözlemlenebilirliği'ni Azure AD ile entegre ettiğinizde şunları yapabilirsiniz:

* Mühendisler için Logz.io - Bulut Gözlemlenebilirliği'ne erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla mühendisler için Bulut Gözlemlenebilirliği Logz.io otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Logz.io - Mühendisler için Bulut Gözlemlenebilirliği tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Logz.io - Mühendisler için Bulut Gözlemlenebilirliği **IDP'nin** başlattığı SSO'ya destek veriyor
* Logz.io yapılandırdıktan sonra - Mühendisler için Bulut Gözlemlenebilirliği, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-logzio---cloud-observability-for-engineers-from-the-gallery"></a>Logz.io Ekleme - Galeriden Mühendisler için Bulut Gözlemlenebilirliği

mühendisler için bulut gözlemlenebilirliği Logz.io azure AD'ye entegrasyonunu yapılandırmak için, galeriden mühendisler için Logz.io - Bulut Gözlemlenebilirliğini yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Mühendisler için Logz.io - Bulut Gözlemlenebilirliği** yazın.
1. Sonuçlar panelinden **Mühendisler için Logz.io - Bulut Gözlemlenebilirliğini** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-logzio---cloud-observability-for-engineers"></a>Logz.io için Azure AD oturum açma işlemlerini yapılandırma ve test etme - Mühendisler için Bulut Gözlemlenebilirliği

Azure AD SSO'Logz.io - **B.Simon**adlı bir test kullanıcısı kullanarak Mühendisler için Bulut Gözlemlenebilirliği ile yapılandırın ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında Logz.io - Mühendisler için Bulut Gözlemlenebilirliği arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu mühendisler için Bulut Gözlemlenebilirliği Logz.io ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Mühendisler SSO için bulut gözlemlenebilirlik Logz.io yapılandırın](#configure-logzio-cloud-observability-for-engineers-sso)** - uygulama tarafında tek oturum açma ayarlarını yapılandırmak için.
    1. **[Mühendisler test kullanıcısı için bulut gözlemlenebilirliği Logz.io oluşturun](#create-logzio-cloud-observability-for-engineers-test-user)** - Logz.io'da B.Simon'Un bir muadili olması - Mühendisler için kullanıcının Azure AD gösterimine bağlı Bulut Gözlemlenebilirliği.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında](https://portal.azure.com/), **Logz.io - Mühendisler için Bulut Gözlemlenebilirliği** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. **SAML sayfasıyla tek oturum açma'da** aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`urn:auth0:logzio:CONNECTION-NAME`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://logzio.auth0.com/login/callback?connection=CONNECTION-NAME`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı ve YanıtURL'i ile güncelleştirin. İletişim Logz.io - Engineers Client destek ekibinin bu değerleri alması [için Bulut Gözlemlenebilirliği.](mailto:help@logz.io) Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Logz.io - Mühendisler için Bulut Gözlemlenebilirliği uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddiaları bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Logz.io - Mühendisler için Bulut Gözlemlenebilirliği uygulaması, saml yanıtında aşağıda gösterilen birkaç özniteliğin daha geri geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.
    
    | Adı |  Kaynak Özniteliği|
    | ---------------| --------- |
    | oturum-sona erme | user.session-sona erme |
    | e-posta | kullanıcı.posta |
    | Grup | user.groups |

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Mühendisler için Bulut Gözlemi Logz.io - **Bulut Gözlemlenebilirliği** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'Un mühendisler için bulut gözlemlenebilirliği Logz.io erişim sağlayarak Azure oturum açma özelliğini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, mühendisler **için Logz.io - Bulut Gözlemlenebilirliğini**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-logzio-cloud-observability-for-engineers-sso"></a>Mühendisler SSO için bulut gözlemlenebilirlik Logz.io yapılandırın

Logz.io tek oturum açma **- Mühendisler için Bulut Gözlemlenebilirliği** tarafını yapılandırmak için, indirilen **Sertifikayı (Base64)** ve uygun kopyalanmış URL'leri Azure portalından Logz.io - [Mühendisler için Bulut Gözlemlenebilirliği destek ekibine](mailto:help@logz.io)göndermeniz gerekir. Bu ayarı, SAML SSO bağlantısının her iki tarafta da düzgün bir şekilde ayarlanması için ayarlarlar.

### <a name="create-logzio-cloud-observability-for-engineers-test-user"></a>Mühendisler test kullanıcısı için bulut gözlemlenebilirliği Logz.io oluşturun

Bu bölümde, Logz.io Britta Simon adında bir kullanıcı oluşturursunuz - Mühendisler için Bulut Gözlemlenebilirliği. Logz.io ile çalışın [- Mühendisler için Bulut Gözlemlenebilirliği destek ekibi,](mailto:help@logz.io) kullanıcıları Logz.io - Mühendisler için Bulut Gözlemlenebilirliği platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki mühendisler için Bulut Gözlemlenebilirliği Logz.io tıkladığınızda, SSO'yu kurduğunuz Mühendisler için bulut gözlemlenebilirliği Logz.io otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Logz.io deneyin - Azure AD ile Mühendisler için Bulut Gözlemlenebilirliği](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Logz.io nasıl korunur - Gelişmiş görünürlüğe ve kontrollere sahip Mühendisler için Bulut Gözlemlenebilirliği](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

