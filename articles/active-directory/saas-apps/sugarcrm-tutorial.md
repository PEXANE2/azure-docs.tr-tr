---
title: 'Öğretici: Sugar CRM ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Sugar CRM arasında tek oturum açma işlemlerini nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 3331b9fc-ebc0-4a3a-9f7b-bf20ee35d180
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/17/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: fae7b80fd4d2fcec32bbef5e4cdf18e576412a86
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74231974"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sugar-crm"></a>Öğretici: Sugar CRM ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Sugar CRM'yi Azure Active Directory (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Sugar CRM'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Sugar CRM erişimi olan denetim.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Sugar CRM'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Sugar CRM tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Şeker CRM **SP** başlatılan SSO destekler

> [!NOTE]
> Bu uygulamanın tanımlayıcısı sabit bir dize değeridir, bu nedenle yalnızca bir örnek bir kiracıda yapılandırılabilir.

## <a name="adding-sugar-crm-from-the-gallery"></a>Galeriden Şeker CRM ekleme

Sugar CRM'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize Sugar CRM eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Şeker CRM** yazın.
1. Sonuçlar panelinden **Sugar CRM'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sugar-crm"></a>Sugar CRM için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Sugar CRM ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Sugar CRM'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Sugar CRM ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[Sugar CRM SSO'yu yapılandırın.](#configure-sugar-crm-sso)**
    1. **[Sugar CRM test kullanıcısını oluşturun](#create-sugar-crm-test-user)** - Kullanıcının Azure AD gösterimine bağlı Olan Şeker CRM'de B.Simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Sugar CRM** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.sugarondemand.com`|
    | `https://<companyname>.trial.sugarcrm`|

    b. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:

    | |
    |--|
    | `https://<companyname>.sugarondemand.com/<companyname>`|
    | `https://<companyname>.trial.sugarcrm.com/<companyname>`|
    | `https://<companyname>.trial.sugarcrm.eu/<companyname>`|

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si ve YanıtURL'i ile güncelleştirin. Bu değerleri almak için [Sugar CRM İstemci destek ekibine](https://support.sugarcrm.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Şeker **CRM'yi Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Sugar CRM'ye erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **Şeker CRM'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-sugar-crm-sso"></a>Şeker CRM SSO yapılandırılması

1. Farklı bir web tarayıcısı penceresinde, Sugar CRM şirket sitenizde yönetici olarak oturum açın.

1. **Admin'e**git.

    ![Yönetici](./media/sugarcrm-tutorial/ic795888.png "Yönetici")

1. **Yönetim** bölümünde Parola **Yönetimi'ni**tıklatın.

    ![Yönetim](./media/sugarcrm-tutorial/ic795889.png "Yönetim")

1. **SAML Kimlik Doğrulamasını Etkinleştir'i**seçin.

    ![Yönetim](./media/sugarcrm-tutorial/ic795890.png "Yönetim")

1. **SAML Kimlik Doğrulama** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SAML Kimlik Doğrulama](./media/sugarcrm-tutorial/ic795891.png "SAML Kimlik Doğrulama")  

    a. Giriş **URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL'sinin**değerini yapıştırın.
  
    b. **SLO URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Logout URL**değerini yapıştırın.
  
    c. Taban-64 kodlanmış sertifikanızı not defterinde açın, içeriğini panonuza kopyalayın ve ardından tüm Sertifikayı **X.509 Sertifika** metin kutusuna yapıştırın.
  
    d. **Kaydet**'e tıklayın.

### <a name="create-sugar-crm-test-user"></a>Şeker CRM test kullanıcısı oluşturma

Azure AD kullanıcılarının Sugar CRM'de oturum açabilmeleri için, bunların Sugar CRM'ye uygun olarak sağlanması gerekir. Sugar CRM durumunda, sağlama manuel bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. **Sugar CRM** şirket sitenizde yönetici olarak oturum açın.

1. **Admin'e**git.

    ![Yönetici](./media/sugarcrm-tutorial/ic795888.png "Yönetici")

1. **Yönetim** **bölümünde, Kullanıcı Yönetimi'ni**tıklatın.

    ![Yönetim](./media/sugarcrm-tutorial/ic795893.png "Yönetim")

1. **Kullanıcılara \> **Git Yeni Kullanıcı Oluştur .

    ![Yeni Kullanıcı Oluşturma](./media/sugarcrm-tutorial/ic795894.png "Yeni Kullanıcı Oluşturma")

1. Kullanıcı **Profili** sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Yeni Kullanıcı](./media/sugarcrm-tutorial/ic795895.png "Yeni Kullanıcı")

    * İlgili metin kutularına geçerli bir Azure Active Directory kullanıcısının **kullanıcı adını,** **soyadını**ve **e-posta adresini** yazın.
  
1. **Durum**olarak, **Etkin'i**seçin.

1. Parola sekmesinde aşağıdaki adımları gerçekleştirin:

    ![Yeni Kullanıcı](./media/sugarcrm-tutorial/ic795896.png "Yeni Kullanıcı")

    a. Parolayı ilgili textbox'a yazın.

    b. **Kaydet**'e tıklayın.

> [!NOTE]
> Azure AD kullanıcı hesaplarını sağlamak için Sugar CRM tarafından sağlanan diğer Sugar CRM kullanıcı hesabı oluşturma araçlarını veya API'lerini kullanabilirsiniz.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Şeker CRM döşemesini tıklattığınızda, SSO'yu kurduğunuz Sugar CRM'de otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Sugar CRM'yi deneyin](https://aad.portal.azure.com/)

