---
title: 'Öğretici: Akamai ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Akamai arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1b7e0d7a-e78f-43a5-af93-b626186e2376
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 11/28/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 042dd242285081001ca48c9f17e4d42c2294c0ff
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74979598"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-akamai"></a>Öğretici: Akamai ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Akamai 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Akamai 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Akamai 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Akamai otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Akamai çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

- Bolluk, ıDP tarafından başlatılan SSO 'yu destekler

## <a name="adding-akamai-from-the-gallery"></a>Galeriden Akamai ekleme

Akamai tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden Akamai yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Akamai** yazın.
1. Sonuçlar panelinden **Akamai** ' i seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-akamai"></a>Akamai için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO 'yu Akamai ile yapılandırın ve test edin. SSO 'nun çalışması için, Akamai içinde bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Akamai ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    * Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    * Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. Uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için **[AKAMAI SSO 'Yu yapılandırın](#configure-akamai-sso)** .
    * Kullanıcının Azure AD gösterimine bağlı olan Akamai 'de B. Simon 'ya karşılık gelen bir **[Akamai test kullanıcısı oluşturun](#create-akamai-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Akamai** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<Yourapp>.login.go.akamai-access.com/sp/response`

    b. **Yanıt URL 'si** metin kutusuna şu kalıbı kullanarak bir URL yazın: `https:// <Yourapp>.login.go.akamai-access.com/sp/response`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Akamai istemci destek ekibine](https://www.akamai.com/us/en/contact-us/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Akamai ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Akamai 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Akamai**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-akamai-sso"></a>Akamai SSO 'yu yapılandırma

### <a name="setting-up-idp"></a>IDP 'yi ayarlama

1. **Akamai kurumsal uygulama erişimi** konsoluna oturum açma.
1. **Akamai EAA konsolunda** **kimlik** > kimlik **sağlayıcıları**' nı seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure01.png)

1. **Kimlik sağlayıcısı ekle**' ye tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure02.png)

    a. **Benzersiz adı**belirtin.
    
    b. **Üçüncü taraf SAML** ' yi seçin ve **kimlik sağlayıcısı oluştur ve Yapılandır**' a tıklayın.

### <a name="general-settings"></a>Genel Ayarlar

1. **Kimlik kesme noktası** -(SP temel URL 'Si – Azure AD yapılandırması için kullanılacak) adını belirtin

    > [!NOTE]
    > Kendi özel etki alanınızı (bir DNS girişi ve bir sertifika gerektirir) seçebilirsiniz. Bu örnekte Akamai etki alanını kullanacağız.

1. **Akamai bulut bölgesi** -uygun bulut bölgesini seçin.
1. **Sertifika doğrulama** -Akamai belgelerini denetle (isteğe bağlı)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure03.png)

### <a name="authentication-configuration"></a>Kimlik doğrulama yapılandırması

1. URL – kimlik kesme noktası ile aynı URL 'YI belirtin (Bu, kullanıcıların kimlik doğrulamasından sonra yeniden yönlendirileceği yerdir).
2. Oturum kapatma URL 'SI: oturum kapatma URL 'sini güncelleştirin.
3. SAML Isteği imzala: varsayılan seçilmemiş.
4. IDP meta veri dosyası için, uygulamayı Azure AD konsoluna ekleyin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure04.png)

### <a name="header-based-authentication"></a>Üst bilgi tabanlı kimlik doğrulaması

Akamai üst bilgi tabanlı kimlik doğrulaması

1. **Özel http** formunu uygulama ekleme Sihirbazı ' nı seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure05.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure06.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure07.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure08.png)

#### <a name="authentication"></a>Kimlik Doğrulaması

![Akamai yapılandırma](./media/header-akamai-tutorial/configure09.png)

![Akamai yapılandırma](./media/header-akamai-tutorial/configure10.png)

#### <a name="services"></a>Hizmetler

1. Kaydet ' e tıklayın ve kimlik doğrulaması ' na gidin.

![Akamai yapılandırma](./media/header-akamai-tutorial/configure11.png)

#### <a name="advanced-settings"></a>Gelişmiş Ayarlar

1. **MÜŞTERI http üstbilgileri**altında **Customerheader** ve **SAML özniteliğini**belirtin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure12.png)

1. **Kaydet ve dağıtıma git** düğmesine tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure13.png)

#### <a name="deploy-the-application"></a>Uygulamayı Dağıtma

1. **Uygulamayı dağıt** düğmesine tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure14.png)

1. Uygulamanın başarıyla dağıtıldığını doğrulayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure15.png)

### <a name="kerberos-authentication"></a>Kerberos Kimlik Doğrulaması

#### <a name="remote-desktop"></a>Uzak Masaüstü

1. Uygulama ekleme sihirbazından **RDP** ' yi seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure16.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure17.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure18.png)

1. Buna hizmet edecek bağlayıcıyı belirtin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure19.png)

#### <a name="authentication"></a>Kimlik Doğrulaması

**Kaydet ' e tıklayın ve hizmetler 'e gidin**.

![Akamai yapılandırma](./media/header-akamai-tutorial/configure20.png)

#### <a name="services"></a>Hizmetler

**Kaydet ' e tıklayın ve Gelişmiş ayarlar**' a gidin.

![Akamai yapılandırma](./media/header-akamai-tutorial/configure21.png)

#### <a name="advanced-settings"></a>Gelişmiş Ayarlar

**Kaydet ' e tıklayın ve dağıtıma gidin**.

![Akamai yapılandırma](./media/header-akamai-tutorial/configure22.png)

![Akamai yapılandırma](./media/header-akamai-tutorial/configure23.png)

![Akamai yapılandırma](./media/header-akamai-tutorial/configure24.png)

### <a name="deployment"></a>Kurulum

#### <a name="ssh"></a>SSH

1. Uygulama Ekle ' ye gidin, **SSH**' ı seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure25.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure26.png)

1. Uygulama kimliğini yapılandırın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure27.png)

    a. Ad/açıklama belirtin.

    b. SSH için uygulama sunucusu IP/FQDN ve bağlantı noktası belirtin.

    c. SSH kullanıcı adını/parolasını belirtin * Check Akamai EAA.

    d. Dış ana bilgisayar adını belirtin.

    e. Bağlayıcının konumunu belirtin ve bağlayıcıyı seçin.

#### <a name="authentication"></a>Kimlik Doğrulaması

Kaydet ' **e tıklayın ve hizmetlere gidin**.

![Akamai yapılandırma](./media/header-akamai-tutorial/configure28.png)

#### <a name="services"></a>Hizmetler

**Kaydet ' e tıklayın ve Gelişmiş ayarlar**' a gidin.

![Akamai yapılandırma](./media/header-akamai-tutorial/configure29.png)

#### <a name="advanced-settings"></a>Gelişmiş Ayarlar

Kaydet ve dağıtıma git 'e tıklayın

![Akamai yapılandırma](./media/header-akamai-tutorial/configure30.png)

![Akamai yapılandırma](./media/header-akamai-tutorial/configure31.png)

#### <a name="deployment"></a>Kurulum

**Uygulamayı dağıt**' a tıklayın.

![Akamai yapılandırma](./media/header-akamai-tutorial/configure32.png)

### <a name="kerberos-applications"></a>Kerberos uygulamaları

#### <a name="adding-directory"></a>Dizin ekleniyor

![Akamai yapılandırma](./media/header-akamai-tutorial/configure33.png)

![Akamai yapılandırma](./media/header-akamai-tutorial/configure34.png)

![Akamai yapılandırma](./media/header-akamai-tutorial/configure35.png)

![Akamai yapılandırma](./media/header-akamai-tutorial/configure36.png)

### <a name="create-akamai-test-user"></a>Akamai test kullanıcısı oluştur

Bu bölümde, Akamai içinde B. Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcıları Akamai platformunda eklemek için [Akamai istemci desteği ekibi](https://www.akamai.com/us/en/contact-us/) ile çalışın. Kullanıcı oluşturulmalı ve çoklu oturum açma kullanmadan önce etkinleştirildi. 

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Akamai kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Akamai için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Akamai deneyin](https://aad.portal.azure.com/)
