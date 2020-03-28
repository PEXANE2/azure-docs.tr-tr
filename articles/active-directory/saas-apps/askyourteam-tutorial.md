---
title: 'Öğretici: AskYourTeam ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve AskYourTeam arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1cef6764-de54-4920-b0ad-e560c214c72e
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 02/28/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 403f39de1b7d226d9feeb33388c32f63271fdcd6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78968630"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-askyourteam"></a>Öğretici: AskYourTeam ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, AskYourTeam'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. AskYourTeam'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* AskYourTeam erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla AskYourTeam'e otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* AskYourTeam tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* AskYourTeam **SP ve IDP** sso başlattı destekler.
* AskYourTeam'i yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızmasını ve sızmasını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-askyourteam-from-the-gallery"></a>Galeriden AskYourTeam ekleme

AskYourTeam'in Azure AD'ye entegrasyonunu yapılandırmak için galeriden AskYourTeam'i yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **AskYourTeam** yazın.
1. Sonuç panelinden **AskYourTeam'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-askyourteam"></a>AskYourTeam için Azure AD oturumunu yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak AskYourTeam ile yapılandırın ve test edin. SSO'nun çalışması için, AskYourTeam'deki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu AskYourTeam ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[AskYourTeam SSO'yu yapılandırın](#configure-askyourteam-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[AskYourTeam test kullanıcısını oluşturun](#create-askyourteam-test-user)** - AskYourTeam'de kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **AskYourTeam** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<COMPANY>.app.askyourteam.com/users/auth/saml/callback`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<COMPANY>.app.askyourteam.com/login`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, öğreticide daha sonra açıklanan gerçek YanıtLA URL'si ve Oturum Açma URL değerleriyle güncelleştirin.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **AskYourTeam'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın AskYourTeam'e erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **AskYourTeam'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-askyourteam-sso"></a>AskYourTeam SSO'ya yapılandır

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak AskYourTeam web sitesinde oturum açın.

1. **Kuruluşum'a**tıklayın.

    ![AskYourTeam Yapılandırma](./media/askyourteam-tutorial/user1.png)

1. **Entegrasyonlar'a**tıklayın.

    ![AskYourTeam Yapılandırma](./media/askyourteam-tutorial/configure1.png)

1. Ayarları **Edit'e**tıklayın.

    ![AskYourTeam Yapılandırma](./media/askyourteam-tutorial/configure2.png)

1. Tek **Oturum Açma Tümleştirmesini Edit** sayfasında aşağıdaki adımları gerçekleştirin: 

    ![AskYourTeam Yapılandırma](./media/askyourteam-tutorial/configure3.png)

    a. **SAML Tek Oturum Açma Hizmeti URL** textbox'ına, Azure portalından kopyalamış olduğunuz Giriş **URL** değerini yapıştırın.

    b. **SAML Entity ID** textbox'a, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcı** değerini yapıştırın.

    c. Oturumu **Çıkış URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL** değerini yapıştırın.

    d. Azure portalından indirilen **Sertifikayı (Base64)** Notepad'e açın ve içeriği **SAML İmza Sertifikası - Base64** textbox'ına yapıştırın.

    > [!NOTE]
    > Alternatif olarak, **Dosya Seç** seçeneğini tıklayarak **Federasyon Metadata XML** dosyasını da yükleyebilirsiniz.

    e. **Yanıtla URL'sini kopyalayın (Tüketici Hizmeti URL'sini İddia Et)** değeri, bu değeri Azure portalındaki **Temel SAML Yapılandırması** bölümündeki **YanıtLA URL** metin kutusuna yapıştırın.

    f. URL değerinde **İmza'yı** kopyalayın, bu değeri Azure portalındaki **Temel SAML Yapılandırması** **bölümündeki URL** metin kutusuna yapıştırın.

    g. **Kaydet**'e tıklayın.

### <a name="create-askyourteam-test-user"></a>AskYourTeam test kullanıcınızı oluşturun

1. Farklı bir web tarayıcısı penceresinde, yönetici olarak AskYourTeam web sitesinde oturum açın.

1. **Kuruluşum'a**tıklayın.

    ![AskYourTeam Yapılandırma](./media/askyourteam-tutorial/user1.png)

1. **Kullanıcılar'ı** tıklatın ve **Yeni Kullanıcı'yı**seçin.

    ![AskYourTeam Yapılandırma](./media/askyourteam-tutorial/user2.png)

1. Yeni **kullanıcı** bölümünde aşağıdaki adımları gerçekleştirin:

    ![AskYourTeam Yapılandırma](./media/askyourteam-tutorial/user3.png)

    1. Ad **metin** kutusuna, kullanıcının adını girin.

    1. **Soyadı** metin kutusuna, kullanıcının soyadını girin.

    1. **E-posta** metin kutusuna, kullanıcının e-posta adresini girin. B.Simon@contoso.com

    1. Kuruluş gereksiniminize göre kullanıcıiçin **Rolü** seçin.

    1. **Kaydet**'e tıklayın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki AskYourTeam döşemesini tıklattığınızda, SSO'yu kurduğunuz AskYourTeam'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure REKLAM ile SaskYourTeam'i deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)
