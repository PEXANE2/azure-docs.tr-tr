---
title: 'Öğretici: HRworks Tek Oturum Açma ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve HRworks Single Sign-On arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: c4c5d434-3f8a-411e-83a5-c3d5276ddc0a
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/02/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4b20ce5f754333aec78513e32901b0608f8bee3b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75638782"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-hrworks-single-sign-on"></a>Öğretici: HRworks Tek Oturum Açma ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, HRworks Tek Oturum Açma'yı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. HRworks Tek Oturum Açma'yı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de HRworks Tek Oturum Açma erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla HRworks Tek Oturum Açma'da otomatik olarak oturum açmalarını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* HRworks Tek Oturum Açma tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* HRworks Tek İşaret-On **SP** başlatılan SSO destekler

## <a name="adding-hrworks-single-sign-on-from-the-gallery"></a>Galeriden HRworks Tek Oturum Açma Ekleme

HRworks Tek Oturum Açma'nın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize HRworks Tek Oturum Açma eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **HRworks Tek İşaret-On** yazın.
1. Sonuç panelinden **HRworks Tek Oturum** Açma'yı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-hrworks-single-sign-on"></a>HRworks Tek Oturum Açma için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak HRworks Tek Oturum Açma ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile HRworks Tek Oturum Açma'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu HRworks Tek Oturum Açma ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    * Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    * B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[HRworks Tek Oturum Açma SSO'yu uygulama](#configure-hrworks-single-sign-on-sso)** tarafındaki tek oturum açma ayarlarını yapılandırmak için yapılandırın.
    * **[HRworks Tek Oturum](#create-hrworks-single-sign-on-test-user)** Açma test kullanıcısını oluşturun - Kullanıcının Azure AD gösterimine bağlı HRworks Tek Oturum Açma'da B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **HRworks Tek Oturum** Açma uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://login.hrworks.de/?companyId=<companyId>&directssologin=true`

    > [!NOTE]
    > Değer gerçek değil. Değeri gerçek Oturum Açma URL'si ile güncelleştirin. Değeri almak için [HRworks Tek Oturum Açma İstemci destek ekibiyle](mailto:nadja.sommerfeld@hrworks.de) iletişime geçin. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **HRworks Tek Oturum Açma** bölümünü ayarlayın, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın HRworks Tek Oturum Açma'ya erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **HRworks Tek Oturum Açma'yı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-hrworks-single-sign-on-sso"></a>HRworks Tek İşaret-On SSO yapılandırılması

1. HRworks Tek Oturum Açma içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **HRworks Tek Oturum Açma'yı** ayarla'ya tıklayın ve sizi HRworks Tek Oturum Açma uygulamasına yönlendirecektir. Buradan, HRworks Tek Oturum Açma'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-4 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. HRworks Tek Oturum Açma'yı el ile kurmak istiyorsanız, yeni bir web tarayıcısı penceresi açın ve HRworks Tek Oturum Açma şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

1. Menü çubuğunun sol tarafından **Administrator** > **Basics** > **Security** > Tek**Oturum** Açma'ya tıklayın ve aşağıdaki adımları gerçekleştirin:

    ![Çoklu oturum açmayı yapılandırma](./media/hrworks-single-sign-on-tutorial/configure01.png)

    a. Tek **Oturum Açma** kutusunu Kullan'ı işaretleyin.

    b. Meta veri giriş **yöntemi**olarak **XML Meta verilerini** seçin.

    c. **NameID için Değer**olarak Bireysel **NameID tanımlayıcısı** seçin.

    d. Not Defteri'nde, Azure portalından indirdiğiniz Metadata XML'yi açın, içeriğini kopyalayın ve meta **veri** metin kutusuna yapıştırın.

    e. **Kaydet**'e tıklayın.

### <a name="create-hrworks-single-sign-on-test-user"></a>HRworks Tek Oturum Açma test kullanıcısı oluşturma

Azure AD kullanıcılarının HRworks Tek Oturum Açma'da oturum açmalarını etkinleştirmek için HRworks Tek Oturum Açma'ya dahil olmaları gerekir. HRworks Tek Oturum Açma'da, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Yönetici olarak HRworks Tek Oturum Açma'da oturum açın.

1. **Yönetici** > **Kişiler** > **Kişiler** > menüsü çubuğunun sol tarafında yeni**kişi** tıklayın.

     ![Tek İşaret-On'u Yapılandır](./media/hrworks-single-sign-on-tutorial/configure02.png)

1. Açılır Pencere'de **İleri'yi**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/hrworks-single-sign-on-tutorial/configure03.png)

1. Yasal **terimler için ülke ile yeni kişi oluştur** açılır, **Ad ,** **Soyad** ve **oluştur**gibi ilgili ayrıntıları doldurun .

    ![Tek İşaret-On'u Yapılandır](./media/hrworks-single-sign-on-tutorial/configure04.png)

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki HRworks Tek Oturum Açma döşemesini tıklattığınızda, Otomatik olarak SSO'yu kurduğunuz HRworks Single Sign-On'da oturum açmanız gerekir. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile HRworks Tek Oturum Açma'yı deneyin](https://aad.portal.azure.com/)