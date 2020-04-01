---
title: 'Öğretici: MyWorkDrive ile Azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve MyWorkDrive arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 4d049778-3c7b-46c0-92a4-f2633a32334b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 06/27/2019
ms.author: jeedes
ms.openlocfilehash: 60fdd9b0a8fb272da885df97e39804a98e48de67
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80478873"
---
# <a name="tutorial-integrate-myworkdrive-with-azure-active-directory"></a>Öğretici: MyWorkDrive'ı Azure Active Directory ile tümleştirin

Bu eğitimde, MyWorkDrive'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. MyWorkDrive'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* MyWorkDrive erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla MyWorkDrive'da otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz yoksa, [burada](https://azure.microsoft.com/pricing/free-trial/)bir aylık ücretsiz deneme sürümü alabilirsiniz.
* MyWorkDrive tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. MyWorkDrive **SP** ve **IDP** başlatılan SSO destekler

## <a name="adding-myworkdrive-from-the-gallery"></a>Galeriden MyWorkDrive ekleme

MyWorkDrive'ın Azure AD'ye entegrasyonunu yapılandırmak için, MyWorkDrive'ı galeriden yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **MyWorkDrive** yazın.
1. Sonuç panelinden **MyWorkDrive'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Azure AD tek oturum açma yapılandırma ve test

Azure AD SSO'nu MyWorkDrive ile **Britta Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, MyWorkDrive'daki bir Azure AD kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu MyWorkDrive ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
2. **[MyWorkDrive SSO'yu yapılandırır](#configure-myworkdrive-sso)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. Britta Simon'ın Azure AD tek oturum açma işlemini kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
5. **[MyWorkDrive test kullanıcısını oluşturun](#create-myworkdrive-test-user)** - MyWorkDrive'da britta Simon'ın kullanıcının Azure AD gösterimine bağlı bir muadili olması için.
6. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

### <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. **MyWorkDrive** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **Tek oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** sayfasında, **Uygulamayı IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki alanın değerlerini girin:

    **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SERVER.DOMAIN.COM>/SAML/AssertionConsumerService.aspx`

1. Uygulamayı **SP** başlatılan modda yapılandırmak istiyorsanız **ek URL'ler ayarla'yı** tıklatın ve aşağıdaki adımı gerçekleştirin:

    Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<SERVER.DOMAIN.COM>/Account/Login-saml`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Yanıtla URL'si ve Oturum Açma URL'si ile güncelleştirin. Kendi şirketinizin MyWorkDrive Server ana bilgisayar adını gir:örn.
    > 
    > Yanıt URL'si: `https://yourserver.yourdomain.com/SAML/AssertionConsumerService.aspx`
    > 
    > Oturum açma URL'si:`https://yourserver.yourdomain.com/Account/Login-saml`
    > 
    > Kendi ana bilgisayar adınızı ve bu değerler için TLS/SSL sertifikanızı nasıl kurabileceğinizden emin değilseniz [MyWorkDrive destek ekibine](mailto:support@myworkdrive.com) başvurun.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** panonuza kopyalamak için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

### <a name="configure-myworkdrive-sso"></a>MyWorkDrive SSO'ya yapılandırın

1. MyWorkDrive içindeki yapılandırmayı otomatikleştirmek için, **uzantıyı yükle'yi**tıklatarak **Uygulamalarım Güvenli Oturum Açma tarayıcı uzantısını** yüklemeniz gerekir.

    ![Uygulamalar uzantım](common/install-myappssecure-extension.png)

1. Tarayıcıya uzantı ekledikten sonra, **Setup MyWorkDrive'a** tıklayın ve sizi MyWorkDrive uygulamasına yönlendirecektir. Buradan MyWorkDrive'da oturum açabilmek için yönetici kimlik bilgilerini sağlayın. Tarayıcı uzantısı uygulamayı sizin için otomatik olarak yapılandıracak ve 3-4 adımlarını otomatikleştirecektir.

    ![Kurulum yapılandırması](common/setup-sso.png)

1. MyWorkDrive'ı el ile kurmak istiyorsanız, farklı bir web tarayıcıpenceresinde Güvenlik Yöneticisi olarak MyWorkDrive'da oturum açın.

1. Yönetici panelindeki MyWorkDrive Server'da **ENTERPRISE'a** tıklayın ve aşağıdaki adımları gerçekleştirin:

    ![Yönetici](./media/myworkdrive-tutorial/tutorial_myworkdrive_admin.png)

    a. **SAML/ADFS SSO'ya**etkinle.

    b. **SAML'yi** seçin - Azure AD

    c. Azure **Uygulama Federasyonu Metadata Url** textbox'ına, Azure portalından kopyalamış olduğunuz **Uygulama Federasyonu Metaveri Url'sinin** değerini yapıştırın.

    d. **Kaydet'i** tıklatın

    > [!NOTE]
    > Ek bilgi için [MyWorkDrive Azure AD destek makalesini](https://www.myworkdrive.com/support/saml-single-sign-on-azure-ad/)inceleyin.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında Britta Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalındaki sol bölmeden **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.
1. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.
1. **Kullanıcı** özelliklerinde aşağıdaki adımları izleyin:
   1. **Ad** alanına `Britta Simon` girin.  
   1. Kullanıcı **adı** alanına. username@companydomain.extension Örneğin, `BrittaSimon@contoso.com`.
   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur'u**tıklatın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, MyWorkDrive'a erişim sağlayarak Britta Simon'ın Azure tek oturum açma işlemini kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **MyWorkDrive'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **Britta Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

### <a name="create-myworkdrive-test-user"></a>MyWorkDrive test kullanıcını oluşturma

Bu bölümde, MyWorkDrive'da Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları MyWorkDrive platformuna eklemek için [MyWorkDrive destek ekibiyle](mailto:support@myworkdrive.com) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-sso"></a>Test SSO

Access Paneli'ndeki MyWorkDrive döşemesini seçtiğinizde, SSO'yu kurduğunuz MyWorkDrive'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek Kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)