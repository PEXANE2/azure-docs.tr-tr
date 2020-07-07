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
ms.topic: tutorial
ms.date: 01/03/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8fef2d7e3afb0cdda3c3c3e2506b39ba542e0760
ms.sourcegitcommit: 845a55e6c391c79d2c1585ac1625ea7dc953ea89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2020
ms.locfileid: "85956894"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-akamai"></a>Öğretici: Akamai ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Akamai 'i Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Akamai 'i Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Akamai 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Akamai otomatik olarak oturum açmalarına olanak tanıyın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Azure Active Directory ve Akamai kurumsal uygulama erişimi tümleştirmesi, bulutta veya şirket içinde barındırılan eski uygulamalara sorunsuz erişim sağlar. Tümleşik çözüm, [Azure AD koşullu erişim](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), [Azure AD kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) ve [Azure AD Identity Governance](https://docs.microsoft.com/azure/active-directory/governance/identity-governance-overview) uygulama değişiklikleri veya aracı yüklemesi olmadan eski uygulama erişimi için Azure Active Directory tüm modern olanaklarından faydalanmasıdır.

Aşağıdaki görüntüde, Akamai EAA 'nın daha geniş karma güvenli erişim senaryosuna uygun olduğu açıklanmaktadır

![Akamai EAA, daha geniş karma güvenli erişim senaryosuna uyar](./media/header-akamai-tutorial/introduction01.png)

### <a name="key-authentication-scenarios"></a>Anahtar kimlik doğrulama senaryoları

Açık KIMLIK Connect, SAML ve WS-Besde gibi modern kimlik doğrulama protokolleri için Azure Active Directory yerel tümleştirme desteğini kullanarak, Azure AD ile hem iç hem de dış erişim için eski tabanlı kimlik doğrulama uygulamalarına yönelik güvenli erişimi genişleterek bu uygulamalara modern senaryolar (örneğin, parola-daha az erişim) sağlar. Buna aşağıdakiler dahildir:

* Üst bilgi tabanlı kimlik doğrulama uygulamaları
* Uzak Masaüstü
* SSH (Secure Shell)
* Kerberos kimlik doğrulama uygulamaları
* VNC (sanal ağ bilgi Işlem)
* Anonim kimlik doğrulama veya yerleşik kimlik doğrulaması uygulamaları
* NTLM kimlik doğrulama uygulamaları (Kullanıcı için çift istemlerle koruma)
* Form tabanlı uygulama (Kullanıcı için çift istemlerle koruma)

### <a name="integration-scenarios"></a>Tümleştirme senaryoları

Microsoft ve Akamai EAA ortaklığı, iş gereksinimlerinize göre birden çok tümleştirme senaryosunu destekleyerek iş gereksinimlerinizi karşılamak için esneklik sağlar. Bunlar tüm uygulamalarda sıfır günlük kapsam sağlamak için kullanılabilir ve uygun ilke sınıflandırmalarını aşamalı olarak sınıflandırabilir ve yapılandırır.

#### <a name="integration-scenario-1"></a>Tümleştirme senaryosu 1

Akamai EAA, Azure AD 'de tek bir uygulama olarak yapılandırılır. Yönetici, uygulama üzerinde CA Ilkesini yapılandırabilir ve koşullar karşılandıktan sonra kullanıcılara Akamai EAA portalına erişim elde edebilir.

**Uzmanları**:

• IDP 'yi yalnızca bir kez yapılandırmanız gerekiyor

**Dezavantajlarını**:

• Kullanıcılar iki uygulama portalı ile sona erdir

• Tüm uygulamalar için tek ortak CA Ilkesi kapsamı.

![Tümleştirme senaryosu 1](./media/header-akamai-tutorial/scenario1.png)

#### <a name="integration-scenario-2"></a>Tümleştirme senaryosu 2

Akamai EAA uygulaması, Azure AD portalında ayrı ayrı ayarlanır. Yönetici, uygulamalar üzerinde bireysel CA Ilkesini yapılandırabilir ve koşullar karşılandıktan sonra kullanıcılar doğrudan belirli bir uygulamaya yeniden yönlendirilebilir.

**Uzmanları**:

• Tek tek CA Ilkelerini tanımlayabilirsiniz

• Tüm uygulamalar 0365 waffle ve myApps.microsoft.com panelinde temsil edilir.

**Dezavantajlarını**:

• Birden çok ıDP 'yi yapılandırmanız gerekir.

![Tümleştirme senaryosu 2](./media/header-akamai-tutorial/scenario2.png)

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Akamai çoklu oturum açma (SSO) etkin abonelik.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

- Akamai, ıDP tarafından başlatılan SSO 'yu destekler

#### <a name="important"></a>Önemli

Aşağıda listelenen tüm kurulum, **tümleştirme senaryosu 1** ve **Senaryo 2**için aynıdır. **Tümleştirme senaryosu 2** Için, Akamai EAA 'da tek BIR IDP ayarlamanız gerekır ve URL ÖZELLIĞININ uygulama URL 'sine işaret etmek için değiştirilmesi gerekir.

![Önemli](./media/header-akamai-tutorial/important.png)

## <a name="adding-akamai-from-the-gallery"></a>Galeriden Akamai ekleme

Akamai tümleştirmesini Azure AD 'ye göre yapılandırmak için, Galeriden Akamai yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
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
    * **[IDP 'yi ayarlama](#setting-up-idp)**
    * **[Üst bilgi tabanlı kimlik doğrulaması](#header-based-authentication)**
    * **[Uzak Masaüstü](#remote-desktop)**
    * **[SSH](#ssh)**
    * **[Kerberos Kimlik Doğrulaması](#kerberos-authentication)**
    * Kullanıcının Azure AD gösterimine bağlı olan Akamai 'de B. Simon 'ya karşılık gelen bir **[Akamai test kullanıcısı oluşturun](#create-akamai-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Akamai** uygulama tümleştirmesi sayfasında **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<Yourapp>.login.go.akamai-access.com/saml/sp/response`

    b. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https:// <Yourapp>.login.go.akamai-access.com/saml/sp/response`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Akamai istemci destek ekibine](https://www.akamai.com/us/en/contact-us/) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **Federasyon meta verileri XML** 'i bulun ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. **Akamai ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Akamai 'e erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Akamai**' yi seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-akamai-sso"></a>Akamai SSO 'yu yapılandırma

### <a name="setting-up-idp"></a>IDP 'yi ayarlama

**AKAMAI EAA ıDP yapılandırması**

1. **Akamai kurumsal uygulama erişim** konsolunda oturum açın.
1. **Akamai EAA konsolunda** **kimlik**  >  **kimlik sağlayıcıları** ' nı seçin ve **kimlik sağlayıcısı ekle**' ye tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure01.png)

1. **Yeni kimlik sağlayıcısı oluştur** sayfasında aşağıdaki adımları uygulayın:

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure02.png)

    a. **Benzersiz adı**belirtin.

    b. **Üçüncü taraf SAML** ' yi seçin ve **kimlik sağlayıcısı oluştur ve Yapılandır**' a tıklayın.

### <a name="general-settings"></a>Genel ayarlar

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

### <a name="session-settings"></a>Oturum ayarları

Ayarları varsayılan olarak bırakın.

![Akamai yapılandırma](./media/header-akamai-tutorial/sessionsettings.png)

### <a name="directories"></a>Dizinler

Dizin yapılandırmasını atlayın.

![Akamai yapılandırma](./media/header-akamai-tutorial/directories.png)

### <a name="customization-ui"></a>Özelleştirme Kullanıcı arabirimi

IDP 'ye özelleştirme ekleyebilirsiniz.

![Akamai yapılandırma](./media/header-akamai-tutorial/customizationui.png)

### <a name="advanced-settings"></a>Gelişmiş Ayarlar

Daha fazla bilgi için Gelişmiş ayarları atlayın/Akamai belgelerine başvurun.

![Akamai yapılandırma](./media/header-akamai-tutorial/advancesettings.png)

### <a name="deployment"></a>Dağıtım

1. Kimlik sağlayıcısını Dağıt ' a tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/deployment.png)

2. Dağıtımın başarılı olduğunu doğrulayın

### <a name="header-based-authentication"></a>Üst bilgi tabanlı kimlik doğrulaması

Akamai üst bilgi tabanlı kimlik doğrulaması

1. **Özel http** formunu uygulama ekleme Sihirbazı ' nı seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure05.png)

2. **Uygulama adı** ve **açıklamasını**girin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure06.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure07.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure08.png)

#### <a name="authentication"></a>Kimlik Doğrulaması

1. **Kimlik doğrulama** sekmesini seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure09.png)

2. **Kimlik sağlayıcısını** ata

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure10.png)

#### <a name="services"></a>Hizmetler

Kaydet ' e tıklayın ve kimlik doğrulaması ' na gidin.

![Akamai yapılandırma](./media/header-akamai-tutorial/configure11.png)

#### <a name="advanced-settings"></a>Gelişmiş Ayarlar

1. **MÜŞTERI http üstbilgileri**altında **Customerheader** ve **SAML özniteliğini**belirtin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure12.png)

1. **Kaydet ve dağıtıma git** düğmesine tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure13.png)

#### <a name="deploy-the-application"></a>Uygulamayı dağıtma

1. **Uygulamayı dağıt** düğmesine tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure14.png)

1. Uygulamanın başarıyla dağıtıldığını doğrulayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure15.png)

1. Son Kullanıcı deneyimi.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/enduser01.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/enduser02.png)

1. Koşullu erişim.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess01.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess02.png)

#### <a name="remote-desktop"></a>Uzak Masaüstü

1. Uygulama ekleme sihirbazından **RDP** ' yi seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure16.png)

1. **Uygulama adı** ve **açıklamasını**girin.

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

1. **Kaydet ' e tıklayın ve dağıtıma gidin**.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure22.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure23.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure24.png)

1. Son Kullanıcı deneyimi

    ![Akamai yapılandırma](./media/header-akamai-tutorial/enduser03.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/enduser02.png)

1. Koşullu Erişim

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess04.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess05.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess06.png)

1. Alternatif olarak, RDP uygulama URL 'sini de doğrudan yazabilirsiniz.

#### <a name="ssh"></a>SSH

1. Uygulama Ekle ' ye gidin, **SSH**' ı seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure25.png)

1. **Uygulama adı** ve **açıklamasını**girin.

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

#### <a name="deployment"></a>Dağıtım

1. **Uygulamayı dağıt**' a tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure32.png)

1. Son Kullanıcı deneyimi

    ![Akamai yapılandırma](./media/header-akamai-tutorial/enduser03.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/enduser04.png)

1. Koşullu Erişim

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess04.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess07.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess08.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess09.png)

### <a name="kerberos-authentication"></a>Kerberos Kimlik Doğrulaması

Aşağıdaki örnekte, bir Iç Web sunucusu yayımlayacağız [http://frp-app1.superdemo.live](http://frp-app1.superdemo.live/) ve KCD kullanarak SSO 'yu etkinleştireceğiz

#### <a name="general-tab"></a>Genel Sekmesi

![Akamai yapılandırma](./media/header-akamai-tutorial/generaltab.png)

#### <a name="authentication-tab"></a>Kimlik doğrulama sekmesi

Kimlik sağlayıcısını ata

![Akamai yapılandırma](./media/header-akamai-tutorial/authenticationtab.png)

#### <a name="services-tab"></a>Hizmetler sekmesi

![Akamai yapılandırma](./media/header-akamai-tutorial/servicestab.png)

#### <a name="advanced-settings"></a>Gelişmiş Ayarlar

![Akamai yapılandırma](./media/header-akamai-tutorial/advancesettings02.png)

> [!NOTE]
> Web sunucusu için SPN, SPN@Domain `HTTP/frp-app1.superdemo.live@SUPERDEMO.LIVE` Bu tanıtım için: EX biçimindedir. Ayarların geri kalanını varsayılan olarak bırakın.

#### <a name="deployment-tab"></a>Dağıtım sekmesi

![Akamai yapılandırma](./media/header-akamai-tutorial/deploymenttab.png)

#### <a name="adding-directory"></a>Dizin ekleniyor

1. Açılan listeden **ad** ' ı seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure33.png)

1. Gerekli verileri sağlayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/configure34.png)

1. Dizin oluşturmayı doğrulayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/directorydomain.png)

1. Erişim gerektirecek grupları/OU 'Ları ekleyin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/addgroup.png)

1. Aşağıdaki grupta EAAGroup adı verilir ve 1 üyesi vardır.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/eaagroup.png)

1. **Kimlik**  >  **kimlik sağlayıcıları** ' na tıklayıp **dizinler** sekmesine tıklayın ve **Dizin ata**' ya tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/assigndirectory.png)

### <a name="configure-kcd-delegation-for-eaa-walkthrough"></a>HER adım için KCD temsilcisini yapılandırma

#### <a name="step-1-create-an-account"></a>1. Adım: hesap oluşturma 

1. Örnekte **Eaatemsili**adlı bir hesap kullanacağız. Bunu, **Kullanıcı ve bilgisayar** Snappin Active Directory kullanarak yapabilirsiniz.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/assigndirectory.png)

    > [!NOTE]
    > Kullanıcı adının **kimlik kesme adına**göre belirli bir biçimde olması gerekebilir. Şekil 1 ' den **corpapps.Login.go.Akamai-Access.com** olduğunu görtik

1. Kullanıcı oturum açma adı şu şekilde olacaktır:`HTTP/corpapps.login.go.akamai-access.com`

    ![Akamai yapılandırma](./media/header-akamai-tutorial/eaadelegation.png)

#### <a name="step-2-configure-the-spn-for-this-account"></a>2. Adım: SPN 'YI bu hesap için yapılandırma

1. Bu örneğe göre SPN aşağıdaki gibi olacaktır.

2. Setspn-s **http/corpapps. Login. go. Akamai-Access. com eaatemsili**

    ![Akamai yapılandırma](./media/header-akamai-tutorial/spn.png)

#### <a name="step-3-configure-delegation"></a>3. Adım: temsilciyi yapılandırma

1. Eaatemsili hesabı için, temsili sekmesine tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/spn.png)

    * Herhangi bir kimlik doğrulama protokolünü kullan belirtin
    * Ekle ' ye tıklayın ve Kerberos Web sitesi için uygulama havuzu hesabını ekleyin. Doğru yapılandırılmışsa doğru SPN 'ye otomatik olarak çözümlenmelidir.

#### <a name="step-4-create-a-keytab-file-for-akamai-eaa"></a>4. Adım: AKAMAI EAA için bir keytab dosyası oluşturma

1. Genel sözdizimi aşağıda verilmiştir.

1. Ktpass/Out ActiveDirectorydomain. keytab/Princ `HTTP/yourloginportalurl@ADDomain.com` /mapuser serviceaccount@ADdomain.com /Pass + rdnPass/şifre All/ptype KRB5_NT_PRINCIPAL

1. Örnek açıklanacak

    | Kod Parçacığı | Açıklama |
    | - | - |
    | Ktpass/Out EAADemo. keytab | Çıkış keytab dosyasının adı |
    | /PrincHTTP/corpapps.login.go.akamai-access.com@superdemo.live | // HTTP/yourIDPName@YourdomainName |
    | /mapusereaadelegation@superdemo.live | EAA temsili hesabı |
    | /Pass RANDOMPASS | EAA temsili hesap parolası |
    | /şifre tüm pType KRB5_NT_PRINCIPAL | Akamai EAA belgelerine başvurun |
    | | |

1. Ktpass/Out EAADemo. keytab/Princ HTTP/corpapps.login.go.akamai-access.com@superdemo.live /mapuser eaadelegation@superdemo.live /Pass RANDOMPASS/şifre tüm ptype KRB5_NT_PRINCIPAL

    ![Akamai yapılandırma](./media/header-akamai-tutorial/administrator.png)

#### <a name="step-5-import-keytab-in-the-akamai-eaa-console"></a>5. Adım: AKAMAI EAA konsolundaki keytab 'ı Içeri aktarma

1. **Sistem**  >  **keysekmeleri**' ne tıklayın.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/keytabs.png)

1. Keytab türünde **Kerberos temsili**' ni seçin.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/keytabdelegation.png)

1. Keytab 'ın dağıtıldığından ve doğrulandığından emin olun.

    ![Akamai yapılandırma](./media/header-akamai-tutorial/keytabs02.png)

1. Kullanıcı Deneyimi

    ![Akamai yapılandırma](./media/header-akamai-tutorial/enduser03.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/enduser04.png)

1. Koşullu Erişim

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess04.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess10.png)

    ![Akamai yapılandırma](./media/header-akamai-tutorial/conditionalaccess11.png)

### <a name="create-akamai-test-user"></a>Akamai test kullanıcısı oluştur

Bu bölümde, Akamai içinde B. Simon adlı bir Kullanıcı oluşturacaksınız. Kullanıcıları Akamai platformunda eklemek için [Akamai istemci desteği ekibi](https://www.akamai.com/us/en/contact-us/) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir. 

## <a name="test-sso"></a>Test SSO 'SU

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edersiniz.

Erişim panelinde Akamai kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Akamai için otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Akamai deneyin](https://aad.portal.azure.com/)
