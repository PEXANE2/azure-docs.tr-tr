---
title: 'Öğretici: Pipedrive ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Pipedrive arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3b97628d-9d0c-45ac-b8ef-7480cf7ec602
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 03/06/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 235169efdcd415df055885d325394c942a2a8d88
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79485653"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-pipedrive"></a>Öğretici: Pipedrive ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Pipedrive'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Pipedrive'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Pipedrive erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Pipedrive'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Pipedrive tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Pipedrive **SP ve IDP** başlatılan SSO destekler
* Pipedrive SSO'yu yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)


## <a name="adding-pipedrive-from-the-gallery"></a>Galeriden Pipedrive Ekleme

Pipedrive'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden Pipedrive'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Pipedrive** yazın.
1. Sonuç panelinden **Pipedrive'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-pipedrive"></a>Pipedrive için Azure AD oturum açma işlemlerini yapılat ve test edin

Azure AD SSO'yu Pipedrive ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Pipedrive'daki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Pipedrive ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Pipedrive SSO'yu yapılandırır](#configure-pipedrive-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[Pipedrive test kullanıcısını oluşturun](#create-pipedrive-test-user)** - Kullanıcının Azure AD gösterimine bağlı Pipedrive'da B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Pipedrive** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<COMPANY-NAME>.pipedrive.com/sso/auth/samlp/metadata.xml`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<COMPANY-NAME>.pipedrive.com/sso/auth/samlp`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<COMPANY-NAME>.pipedrive.com/`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [Pipedrive Client destek ekibine](mailto:support@pipedrive.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. Pipedrive uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir.

    ![image](common/default-attributes.png)

1. Yukarıdakilere ek olarak, Pipedrive uygulaması, aşağıda gösterilen SAML yanıtında birkaç özniteliğin daha geçirilmesini bekler. Bu öznitelikler de önceden doldurulur, ancak gereksinimlerinize göre bunları gözden geçirebilirsiniz.

    | Adı | Kaynak Özniteliği|
    | ------------ | --------- |
    | e-posta | kullanıcı.posta |

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma** işlemi nde Sertifika **'yı bulun** ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin ve ayrıca App **Federation Metadata Url'sini** kopyalayın ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](./media/pipedrive-tutorial/certificate-data.png)

1. **Pipedrive'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Pipedrive'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Pipedrive'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-pipedrive-sso"></a>Pipedrive SSO'yu yapılandır

1. Farklı bir tarayıcı penceresinde, Administrator olarak Pipedrive web sitesinde oturum açın.

1. Kullanıcı **Profili'ne** tıklayın ve **Ayarlar'ı**seçin.

    ![Pipedrive Yapılandırması](./media/pipedrive-tutorial/configure1.png)

1. Güvenlik merkezine aşağı kaydırın ve **Tek oturum açma'yı**seçin.

    ![Pipedrive Yapılandırması](./media/pipedrive-tutorial/configure2.png)

1. Pipedrive bölümü **için SAML yapılandırmada** aşağıdaki adımları gerçekleştirin:

    ![Pipedrive Yapılandırması](./media/pipedrive-tutorial/configure3.png)

    a. **Veren** metin kutusuna, Azure portalından kopyaladığınız **Uygulama Federasyonu Metaveri Url** değerini yapıştırın.

    b. Tek **Oturum Açma(SSO) url** textbox'ına, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    c. Tek **Oturum Açma(SLO) url** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    d. **x.509 sertifika** metin kutusunda, indirilen **Sertifika (Base64)** dosyasını Azure portalından Notepad'e açın ve içeriği kopyalayın ve **x.509 sertifika** textbox'a yapıştırın ve değişiklikleri kaydedin.

### <a name="create-pipedrive-test-user"></a>Pipedrive test kullanıcısı oluşturma

1. Farklı bir tarayıcı penceresinde, Administrator olarak Pipedrive web sitesinde oturum açın.

1. Şirkete gidin ve **kullanıcıları yönet'i**seçin.

    ![Pipedrive Yapılandırması](./media/pipedrive-tutorial/user1.png)

1. Kullanıcı **Ekle'ye**tıklayın.
    
    ![Pipedrive Yapılandırması](./media/pipedrive-tutorial/user2.png)

1. Kullanıcıları **Yönet** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Pipedrive Yapılandırması](./media/pipedrive-tutorial/user3.png)

    a. **E-posta** metin kutusuna, kullanıcının e-posta adresini girin. `B.Simon@contoso.com`

    b. Ad **metin** kutusuna, kullanıcının adını girin.

    c. **Soyadı** metin kutusuna, kullanıcının soyadını girin.

    d. **Onu onayla'yı tıklatın ve kullanıcıları davet edin.**

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Pipedrive döşemesini tıklattığınızda, SSO'yu kurduğunuz Pipedrive'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Pipedrive'ı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)