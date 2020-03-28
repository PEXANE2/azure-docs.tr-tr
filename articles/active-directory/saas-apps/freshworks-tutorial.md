---
title: 'Öğretici: Freshworks ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Freshworks arasında tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 697c3509-0e3a-4b05-9d5b-9ded4269eb60
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/11/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: b73b8769b5f7360ba4ad778600b1000b8b3f7bc7
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "72376416"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-freshworks"></a>Öğretici: Freshworks ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Freshworks'ü Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Freshworks'ün Azure AD ile tümleştirmesi, şunları yapabilirsiniz:

* Azure AD'de Freshworks'e erişimi olan denetimi denetle.
* Kullanıcılarınızın Azure REKLAM hesaplarıyla Freshworks'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Freshworks tek oturum açma (SSO) aboneliği sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Freshworks **SP** başlatılan SSO destekler

## <a name="adding-freshworks-from-the-gallery"></a>Galeriden Freshworks ekleme

Freshworks'ün Azure AD'ye entegrasyonunu yapılandırmak için galeriden yönetilen SaaS uygulamaları listenize Freshworks eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeribölümünden Ekle** bölümünde, arama kutusuna **Freshworks** yazın.
1. Sonuçlar panelinden **Freshworks'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-freshworks"></a>Freshworks için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak Freshworks ile yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure REKLAM kullanıcısı ile Freshworks'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Freshworks ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[Freshworks SSO'yu yapılandırır](#configure-freshworks-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[Freshworks test kullanıcısını oluşturun](#create-freshworks-test-user)** - Kullanıcının Azure AD gösterimine bağlı Freshworks'teki B.Simon'ın bir örneğine sahip olmak için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Azure [portalında,](https://portal.azure.com/) **Freshworks** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. URL metin kutusunda **Oturum Aç** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.freshworks.com/login`

    b. Tanımlayıcı **(Entity ID)** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<SUBDOMAIN>.freshworks.com/sp/SAML/<MODULE_ID>/metadata`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri URL ve Tanımlayıcı'daki gerçek Oturum'la güncelleştirin. Bu değerleri almak için [Freshworks Müşteri destek ekibine](mailto:support@freshworks.com) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

1. **SAML** Ile Tek Oturum Açma sayfasında, **SAML İmza Sertifikası** bölümünde **Sertifika 'yı (Base64)** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1.  **İmzalama** seçeneklerini gereksiniminize göre değiştirmek için **SAML İmzalama Sertifikası** iletişim kutusunu açmak için **Edit** düğmesini tıklatın.

     ![image](common/edit-certificate.png)

     ![Freshworks yapılandırması](./media/freshworks-tutorial/response.png)

    a. **İmzalama Seçeneği**olarak **IMZA SAML yanıtını** seçin.

    b. **Kaydet**'e tıklayın.

1. **Freshworks'ü Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın Freshworks'e erişim izni vererek Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulama listesinde **Freshworks'u**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-freshworks-sso"></a>Freshworks SSO'ya yapılandırın

1. Yeni bir web tarayıcısı penceresi açın ve Freshworks şirket sitenizde yönetici olarak oturum açın ve aşağıdaki adımları gerçekleştirin:

2. Menünün sol tarafından **Güvenlik** simgesine tıklayın ve **tek oturum açma** seçeneğini işaretleyin ve Kimlik Doğrulama **Yöntemleri**altında **SAML SSO'yu** seçin.

    ![Freshworks yapılandırması](./media/freshworks-tutorial/configure01.png)

3. Tek **oturum açma** bölümünde aşağıdaki adımları gerçekleştirin:

    ![Freshworks yapılandırması](./media/freshworks-tutorial/configure02.png)

    a. **Servis Sağlayıcı(SP) Entity Kimliğini** sizin durumunuza göre kopyalamak ve Azure portalındaki Temel **SAML Yapılandırma** sı bölümündeki Tanımlayıcı **(Entity ID)** metin kutusuna yapıştırmak için **Kopyala'yı** tıklatın.

    b. **IdP** metin kutusu tarafından sağlanan Entity Id'de, Azure portalından kopyaladığınız **Azure AD Tanımlayıcı** değerini yapıştırın.

    c. **SAML SSO URL** metin kutusuna, Azure portalından kopyalamış olduğunuz **Giriş URL** değerini yapıştırın.

    d. Base64 kodlanmış sertifikayı not defterinde açın, içeriğini kopyalayın ve **Güvenlik sertifikası** metin kutusuna yapıştırın.

    e. **Kaydet**'e tıklayın.

### <a name="create-freshworks-test-user"></a>Freshworks test kullanıcısı oluşturun

Bu bölümde, Freshworks'te B.Simon adında bir kullanıcı oluşturursunuz. Freshworks [müşteri destek ekibiyle](mailto:support@freshworks.com) birlikte çalışarak kullanıcıları Freshworks platformuna ekleyin. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir. 

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Erişim Paneli'ndeki Freshworks döşemesini tıklattığınızda, SSO'yu kurduğunuz Freshworks'te otomatik olarak oturum açmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Freshworks'u deneyin](https://aad.portal.azure.com/)

