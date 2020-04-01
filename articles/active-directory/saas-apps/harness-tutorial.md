---
title: 'Öğretici: Harness ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Harness arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 82367f62-173e-4e14-bf84-d8f611706086
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/02/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 21409eb056743d92db42e0787af24f8cec07db1b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72026967"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-harness"></a>Öğretici: Harness ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Harness'in Azure Active Directory (Azure AD) ile nasıl entegre edileceğinizi öğreneceksiniz. Harness'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Harness erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Harness'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Harness tek oturum açma (SSO) özellikli abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* **Koşum, SP ve IDP'nin** başlattığı SSO'ya destek veriyor

## <a name="adding-harness-from-the-gallery"></a>Galeriden Koşum Ekleme

Harness'in Azure AD'ye entegrasyonunu yapılandırmak için, harness'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Harness** yazın.
1. Sonuçlar panelinden **Harness'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-harness"></a>Harness için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Harness ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Harness'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Harness ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Harness SSO'yu yapılandırın](#configure-harness-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Harness test kullanıcısını oluşturun](#create-harness-test-user)** - Kullanıcının Azure AD gösterimine bağlı Harness'teki B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **Azure** [portalında,](https://portal.azure.com/)Harness uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sı bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanların değerlerini girin:

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://app.harness.io/gateway/api/users/saml-login?accountId=<harness_account_id>`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.harness.io/`

    > [!NOTE]
    > YanıtURL değeri gerçek değil. Gerçek Yanıt URL'sini, daha sonra öğreticide açıklanan **Yapılandırma Harness SSO** bölümünden alırsınız. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Harness'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Harness'e erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Harness'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-harness-sso"></a>Harness SSO'nun yapılandırılması

1. Harness içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Kurulum Kayışı'na** tıklayın ve sizi Harness uygulamasına yönlendirecektir. Buradan, Harness'te oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Harness'i el ile kurmak istiyorsanız, yeni bir web tarayıcıpenceresi açın ve Harness şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sayfanın sağ üst **kısmında, Sürekli Güvenlik** > **Erişim Yönetimi** > **Kimlik Doğrulama Ayarları'na**tıklayın.

    ![Koşum yapılandırması](./media/harness-tutorial/configure01.png)

5. **SSO Sağlayıcıları** bölümünde + **Ekle SSO Sağlayıcıları** > **SAML'e**tıklayın.

    ![Koşum yapılandırması](./media/harness-tutorial/configure03.png)

6. **SAML Sağlayıcı** açılır pencerede aşağıdaki adımları gerçekleştirin:

    ![Koşum yapılandırması](./media/harness-tutorial/configure02.png)

    a. **SSO Sağlayıcınızda kopyalayın, lütfen SAML tabanlı girişi etkinleştirin, ardından aşağıdaki URL** örneğini girin ve Azure portalındaki **Temel SAML Yapılandırması** bölümünde YanıtLA URL metin kutusuna yapıştırın.

    b. Görüntü **Adı** metin kutusuna, görüntü adınızı yazın.

    c. Azure AD'den indirdiğiniz Federasyon Metadata XML dosyasını yüklemek için **dosyayı seçin'i** tıklatın.

    d. **GÖNDER'i**tıklatın.

### <a name="create-harness-test-user"></a>Harness test kullanıcıoluşturma

Azure AD kullanıcılarının Harness'te oturum açabilmeleri için, bu kullanıcıların Harness'te oturum açmaları gerekir. Harness'te, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak Harness'te oturum açın.

1. Sayfanın sağ üst kısmında, **Sürekli Güvenlik** > **Erişim Yönetimi** > **Kullanıcıları**tıklayın.

    ![Koşum yapılandırması](./media/harness-tutorial/configure04.png)

1. Sayfanın sağ tarafında + **Kullanıcı Ekle'ye**tıklayın.

    ![Koşum yapılandırması](./media/harness-tutorial/configure05.png)

1. Kullanıcı **Ekle** açılır pencerede aşağıdaki adımları gerçekleştirin:

    ![Koşum yapılandırması](./media/harness-tutorial/configure06.png)

    a. **E-posta Adresi(es)** metin kutusuna, `B.simon@contoso.com`kullanıcının e-posta adresini girin.

    b. Kullanıcı **Gruplarınızı**seçin.

    c. **Gönder'i**tıklatın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Koşum döşemesini tıklattığınızda, SSO'yu kurduğunuz Koşum'da otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Harness'i deneyin](https://aad.portal.azure.com/)

