---
title: 'Öğretici: Workteam ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Workteam arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 41df17a1-ba69-414f-8ec3-11079b030df6
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/19/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0b14c08604fcc0f6d2550127ca1f1aa053172b75
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72026742"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-workteam"></a>Öğretici: Workteam ile Azure Active Directory tek oturum açma (SSO) tümleştirmesi

Bu eğitimde, Workteam'i Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Workteam'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Workteam erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Workteam'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Workteam tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Workteam **SP ve IDP** başlatılan SSO destekler

## <a name="adding-workteam-from-the-gallery"></a>Galeriden Workteam ekleme

Workteam'in Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Yönetilen SaaS uygulamaları listenize Workteam eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Workteam** yazın.
1. Sonuç panelinden **Workteam'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-workteam"></a>Workteam için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Workteam ile yapılandırma ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Workteam'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Workteam ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Workteam SSO'yu yapılandırır](#configure-workteam-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Workteam test kullanıcısını oluşturun](#create-workteam-test-user)** - B.Simon'ın Workteam'deki bir örneğine sahip olmak için kullanıcının Azure AD gösterimine bağlı.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Workteam** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde uygulama **IDP** tarafından başlatılan modda önceden yapılandırılmıştır ve gerekli URL'ler azure ile önceden doldurulmuş durumdadır. Kullanıcının **Kaydet** düğmesini tıklatarak yapılandırmayı kaydetmesi gerekir.

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna bir URL yazın:`https://app.workte.am`

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Workteam'i Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Workteam'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Workteam'i**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="configure-workteam-sso"></a>Workteam SSO'nun yapılandırılması

1. Workteam içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

2. Tarayıcıya uzantı ekledikten **sonra, Kurulum Çalışması ekibine** tıklayın ve sizi Workteam uygulamasına yönlendirecektir. Buradan Workteam'de oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-6 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

3. Workteam'i el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve Workteam şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

4. Sağ üst köşede **profil logosuna** tıklayın ve ardından **Organizasyon ayarlarına**tıklayın. 

    ![Çalışma ekibi ayarları](./media/workteam-tutorial/tutorial_workteam_settings.png)

5. **AUTHENTICATION** bölümünde Ayarlar **logosunu**tıklatın.

     ![Çalışma ekibi azure](./media/workteam-tutorial/tutorial_workteam_azure.png)

6. **SAML Ayarları** sayfasında aşağıdaki adımları gerçekleştirin:

     ![Çalışma ekibi saml](./media/workteam-tutorial/tutorial_workteam_saml.png)

    a. **AD Azure**olarak **SAML IdP'yi** seçin.

    b. **SAML Tek Oturum Açma Hizmeti URL** metin kutusuna, Azure portalından kopyalamış olduğunuz Giriş **URL'sinin**değerini yapıştırın.

    c. **SAML Entity ID** textbox'ına, Azure portalından kopyalamış olduğunuz **Azure AD Tanımlayıcısı'nın**değerini yapıştırın.

    d. Not Defteri'nde, Azure portalından indirdiğiniz **base-64 kodlu sertifikayı** açın, içeriğini kopyalayın ve ardından **SAML İmza Sertifikası (Base64)** kutusuna yapıştırın.

    e. **Tamam**'a tıklayın.

### <a name="create-workteam-test-user"></a>Workteam test kullanıcısı oluşturma

Azure AD kullanıcılarının Workteam'de oturum açabilmeleri için Workteam'de oturum açmaları gerekir. Workteam'de, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Workteam'de Güvenlik Yöneticisi olarak oturum açın.

2. **Organizasyon ayarları** sayfasının üst ortasında **KULLANICILAR'ı** tıklatın ve ardından **Yenİ KULLANICI'yı**tıklatın.

    ![Workteam kullanıcısı](./media/workteam-tutorial/tutorial_workteam_user.png)

3. Yeni **çalışan** sayfasında aşağıdaki adımları gerçekleştirin:

    ![Workteam yeni kullanıcı](./media/workteam-tutorial/tutorial_workteam_newuser.png)

    a. **Ad** metin kutusuna, **B.Simon**gibi kullanıcının ilk adını girin.

    b. **E-posta** metin kutusuna, gibi `B.Simon\@contoso.com`kullanıcının e-posta girin.

    c. **Tamam**'a tıklayın.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki Workteam döşemesini tıklattığınızda, SSO'yu kurduğunuz Workteam'de otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Workteam'i deneyin](https://aad.portal.azure.com/)

