---
title: 'Öğretici: InVision ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve InVision arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 02487206-30b0-4b1d-ae99-573c3d2ef9b0
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 04/09/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1c2427fff37158745e416f4b2f0641697ad62ea9
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81682686"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-invision"></a>Öğretici: InVision ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, InVision'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. InVision'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* InVision erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla InVision'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* InVision tek oturum açma (SSO) aboneliği ni etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **InVision, SP ve IDP** tarafından başlatılan SSO'ya destek veriyor
* InVision'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="adding-invision-from-the-gallery"></a>Galeriden InVision ekleme

InVision'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize InVision eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **InVision** yazın.
1. Sonuç panelinden **InVision'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-invision"></a>InVision için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak InVision ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile InVision'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu InVision ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[InVision SSO'yu yapılandırın](#configure-invision-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    * **[InVision test kullanıcısını oluşturun](#create-invision-test-user)** - InVision'da Kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **InVision** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    a. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.invisionapp.com`

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.invisionapp.com//sso/auth`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.invisionapp.com`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtLA URL'si ve Oturum Açma URL'si ile güncelleştirin. Bu değerleri almak için [InVision İstemci destek ekibine](mailto:support@invisionapp.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **InVision'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın InVision'a erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **InVision'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-invision-sso"></a>InVision SSO'da yapılandır

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak InVision sitesinde oturum açın.

1. **Takım'a** tıklayın ve **Ayarlar'ı**seçin.

    ![InVision Yapılandırma](./media/invision-tutorial/config1.png)

1. **Tek oturum açma'ya** gidin ve sonra **Değiştir'i**tıklatın.

    ![InVision Yapılandırma](./media/invision-tutorial/config3.png)

1. Tek **oturum açma** sayfasında aşağıdaki adımları gerçekleştirin:

    ![InVision Yapılandırma](./media/invision-tutorial/config4.png)

    a. Değişiklik < **hesap adının her üyesi için SSO gerektirir >.** **On**

    b. **Ad** metin kutusuna, örneğin `azureadsso`.

    c. **Oturum** Açma URL'si metin kutusuna Oturum Aç URL değerini girin.

    d. Oturumu **Çıkış URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş çıkış** URL değerini yapıştırın.

    e. **SAML Sertifikası** metin kutusunda, indirilen **Sertifikayı (Base64)** Notepad'e açın, içeriği kopyalayın ve SAML Certificate textbox'a yapıştırın.

    f. Ad **Kimliği Biçimi** metin kutusunda, **Ad Kimliği Biçimi**için kullanın. `Unspecified`

    g. **HASH Algoritması**için açılan dosyadan **SHA-256'yı** seçin.

    h. **SSO Düğme Etiketi**için uygun adı girin.

    i. **Just-in-Time provisioning On'a izin** verin.

    j. **Güncelleştir**’e tıklayın.

### <a name="create-invision-test-user"></a>InVision test kullanıcısı oluşturma

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak InVision sitesinde oturum açın.

1. **Takım'a** tıklayın ve **Kişiler'i**seçin.

    ![InVision Yapılandırma](./media/invision-tutorial/config2.png)

1. Yeni kullanıcı eklemek için **+ ICON'a** tıklayın.

    ![InVision Yapılandırma](./media/invision-tutorial/user1.png)

1. Kullanıcının e-posta adresini girin ve **İleri'yi**tıklatın.

    ![InVision Yapılandırma](./media/invision-tutorial/user2.png)

1. E-posta adresini doğruladıktan sonra **Davet Et'i**tıklatın.

    ![InVision Yapılandırma](./media/invision-tutorial/user3.png)

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki InVision döşemesini tıklattığınızda, SSO'yu kurduğunuz InVision'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile InVision'ı deneyin](https://aad.portal.azure.com/)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve kontrollerle InVision nasıl korunur?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)