---
title: 'Öğretici: RunMyProcess ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve RunMyProcess arasında tek oturum açma işlemini nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: d31f7395-048b-4a61-9505-5acf9fc68d9b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/07/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 46c31a209e8521b24e7f604dbe630f689fca484e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "68880392"
---
# <a name="tutorial-integrate-runmyprocess-with-azure-active-directory"></a>Öğretici: RunMyProcess'i Azure Active Directory ile tümleştir

Bu eğitimde, RunMyProcess'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. RunMyProcess'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* RunMyProcess erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla RunMyProcess'de otomatik olarak oturum açarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* RunMyProcess tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* RunMyProcess **SP** başlatılan SSO destekler

## <a name="adding-runmyprocess-from-the-gallery"></a>Galeriden RunMyProcess ekleme

RunMyProcess'in Azure AD'ye entegrasyonunu yapılandırmak için, RunMyProcess'i galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **RunMyProcess** yazın.
1. Sonuçlar panelinden **RunMyProcess'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak RunMyProcess ile yapılandırın ve test edin. SSO'nun çalışması için, RunMyProcess'deki bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu RunMyProcess ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[RunMyProcess SSO'yu yapılandırın](#configure-runmyprocess-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[RunMyProcess test kullanıcısını oluşturun](#create-runmyprocess-test-user)** - RunMyProcess'de kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **RunMyProcess** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://live.runmyprocess.com/live/<tenant id>`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [RunMyProcess Client destek ekibine](mailto:support@runmyprocess.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde Sertifika **'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **RunMyProcess'i ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="configure-runmyprocess-sso"></a>RunMyProcess SSO'ya yapılandır

1. Farklı bir web tarayıcısı penceresinde, RunMyProcess kiracınızı yönetici olarak oturum açın.

1. Sol daki gezinti panelinde **Hesap'ı** tıklatın ve **Yapılandırma'yı**seçin.

    ![Uygulama Tarafında Tek Oturum Açma'yı Yapılandır](./media/runmyprocess-tutorial/tutorial_runmyprocess_001.png)

1. Kimlik **Doğrulama yöntemi** bölümüne gidin ve aşağıdaki adımları gerçekleştirin:

    ![Uygulama Tarafında Tek Oturum Açma'yı Yapılandır](./media/runmyprocess-tutorial/tutorial_runmyprocess_002.png)

    a. **Yöntem**olarak, **Samlv2 ile SSO**seçin.

    b. **SSO yeniden yönlendirme** textbox'ında, Azure portalından kopyaladığınız **Giriş URL'sinin**değerini yapıştırın.

    c. Oturum **Açma yeniden yönlendirme** textbox'ına, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.

    d. Ad **Kimliği Biçimli** textbox'a, **Ad Tanımlayıcı Biçimi'nin** değerini **vazo:oasis:names:tc:SAML:1.1:nameid-format:emailAddress**olarak yazın.

    e. İndirilen sertifika dosyasını not defterinde Azure portalından açın, sertifika dosyasının içeriğini kopyalayın ve **ardından Sertifika** metin kutusuna yapıştırın.

    f. **Kaydet** simgesini tıklatın.

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

Bu bölümde, B.Simon'ın RunMyProcess'e erişim izni vererek Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **RunMyProcess'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-runmyprocess-test-user"></a>RunMyProcess test kullanıcıoluşturma

Azure AD kullanıcılarının RunMyProcess'de oturum açabilmeleri için RunMyProcess'de oturum açmaları gerekir. RunMyProcess durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. RunMyProcess şirket sitenizde yönetici olarak oturum açın.

1. **Hesap'ı** tıklatın ve sol navigasyon panelinde **Kullanıcıları** seçin, ardından **Yeni Kullanıcı'yı**tıklatın.

    ![Yeni Kullanıcı](./media/runmyprocess-tutorial/tutorial_runmyprocess_003.png "Yeni Kullanıcı")

1. Kullanıcı **Ayarları** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Profil](./media/runmyprocess-tutorial/tutorial_runmyprocess_004.png "Profil")
  
    a. Sağlamak istediğiniz geçerli bir Azure REKLAM hesabının **Adını** ve **E-postasını** ilgili metin kutularına yazın.

    b. Bir **IDE dili,** **Dili**ve **Profili**seçin.

    c. **Bana hesap oluşturma e-postagönder'i**seçin.

    d. **Kaydet**'e tıklayın.

    > [!NOTE]
    > Azure Active Directory kullanıcı hesaplarını sağlamak için RunMyProcess tarafından sağlanan diğer RunMyProcess kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

### <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki RunMyProcess döşemesini tıklattığınızda, SSO'yu kurduğunuz RunMyProcess'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)