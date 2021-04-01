---
title: 'Öğretici: Pulse Secure Virtual Traffic Manager ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Azure Active Directory ve Pulse Secure sanal Traffic Manager arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: CelesteDG
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 09/11/2020
ms.author: jeedes
ms.openlocfilehash: 1e295075a5c1ae8daa6673757770bbef01208c1d
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "92505755"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-pulse-secure-virtual-traffic-manager"></a>Öğretici: Pulse Secure sanal Traffic Manager ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, Pulse Secure sanal Traffic Manager Azure Active Directory (Azure AD) ile nasıl tümleştirileceğini öğreneceksiniz. Darbeli güvenli sanal Traffic Manager Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de darbeli güvenli sanal Traffic Manager erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Pulse güvenli sanal Traffic Manager için otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Önkoşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Pulse Secure Virtual Traffic Manager çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Pulse Secure sanal Traffic Manager **SP** tarafından başlatılan SSO 'yu destekler

## <a name="adding-pulse-secure-virtual-traffic-manager-from-the-gallery"></a>Galeriden Pulse güvenli sanal Traffic Manager ekleme

Pulse Secure sanal Traffic Manager 'ın Azure AD 'ye tümleştirilmesini yapılandırmak için, galerinizden yönetilen SaaS uygulamaları listenize Pulse Secure sanal Traffic Manager eklemeniz gerekir.

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Pulse Secure Virtual Traffic Manager** yazın.
1. Sonuçlar panelinden **Pulse Secure sanal Traffic Manager** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.


## <a name="configure-and-test-azure-ad-sso-for-pulse-secure-virtual-traffic-manager"></a>Pulse Secure sanal Traffic Manager için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak, Pulse Secure sanal TRAFFIC Manager Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için bir Azure AD kullanıcısı ve Pulse Secure sanal Traffic Manager ile ilgili Kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO 'yu darbeli güvenli sanal Traffic Manager yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Pulse Secure sanal TRAFFIC Manager SSO 'Yu yapılandırma](#configure-pulse-secure-virtual-traffic-manager-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Kullanıcının Azure AD gösterimine bağlı olan darbeli güvenli sanal Traffic Manager için, Pulse Secure **[sanal Traffic Manager test kullanıcısı oluşturun](#create-pulse-secure-virtual-traffic-manager-test-user)** .
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. Azure portal, **darbeli güvenli sanal Traffic Manager** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, aşağıdaki alanlar için değerleri girin:

    a. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<published virtual server FQDN>/saml/consume`

    b. **Tanımlayıcı (VARLıK kimliği)** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<published virtual server FQDN>/saml/metadata`

    c. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<published virtual server FQDN>/saml/consume`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri, gerçek oturum açma URL 'SI ve tanımlayıcısı ile güncelleştirin. Bu değerleri almak için [darbeli güvenli sanal Traffic Manager istemci desteği ekibine](mailto:support@pulsesecure.net) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML Imzalama sertifikası** bölümünde **sertifika bulun (base64)** ve sertifikayı indirip bilgisayarınıza kaydetmek için **İndir** ' i seçin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Darbeli güvenli sanal Traffic Manager ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Ekranın üst kısmındaki **Yeni Kullanıcı** ' yı seçin.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına, girin username@companydomain.extension . Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**’a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Pulse Secure sanal Traffic Manager erişim vererek Azure çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde, **Pulse Secure sanal Traffic Manager** öğesini seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-pulse-secure-virtual-traffic-manager-sso"></a>Pulse Secure sanal Traffic Manager SSO yapılandırma

Bu bölüm, Pulse sanal Traffic Manager Azure AD SAML kimlik doğrulamasını etkinleştirmek için gereken yapılandırmayı ele almaktadır. Tüm yapılandırma değişiklikleri, Yönetim Web Kullanıcı arabirimi kullanılarak Pulse sanal Traffic Manager yapılır. 

#### <a name="create-a-saml-trusted-identity-provider"></a>SAML güvenilir kimlik sağlayıcısı oluşturma

a. **Traffic Manager aracı Yöneticisi Kullanıcı arabirimi > katalog > SAML > güvenilen kimlik sağlayıcıları kataloğu** sayfasına gidin ve **Düzenle**' ye tıklayın.

![SAML katalogları sayfası](./media/pulse-secure-virtual-traffic-manager-tutorial/saml-catalogs.png)

b. Yeni SAML güvenilir kimlik sağlayıcısının ayrıntılarını ekleyin, bilgileri Azure AD kurumsal uygulamasından çoklu oturum açma ayarları sayfasında kopyalayarak ve ardından **yeni güvenilir kimlik sağlayıcısı oluştur**' a tıklayın.

![Yeni güvenilir kimlik sağlayıcısı oluştur](./media/pulse-secure-virtual-traffic-manager-tutorial/create-trusted-identity-provider.png)

* **Ad** metin kutusuna, güvenilen kimlik sağlayıcısı için bir ad girin. 

* **Entity_id** metin kutusunda, Azure Portal KOPYALADıĞıNıZ **Azure AD tanımlayıcı** değerini girin.  

* **URL** metin kutusuna, Azure Portal kopyaladığınız **oturum açma URL 'si** değerini girin. 

* İndirilen **sertifikayı** Azure Portal Not defteri ' nden açın ve içeriği **sertifika** metin kutusuna yapıştırın.

c. Yeni SAML kimlik sağlayıcısının başarıyla oluşturulduğunu doğrulayın. 

![Güvenilen kimlik sağlayıcısını doğrula](./media/pulse-secure-virtual-traffic-manager-tutorial/verify-saml-identity-provider.png)

#### <a name="configure-the-virtual-server-to-use-azure-ad-authentication"></a>Sanal sunucuyu Azure AD kimlik doğrulaması kullanacak şekilde yapılandırma

a. **Pulse virtual Traffic Manager gereç Yöneticisi Kullanıcı arabirimi > hizmetleri > sanal sunucular** sayfasına gidin ve daha önce oluşturulmuş sanal sunucunun yanındaki **Düzenle** ' ye tıklayın.

![Sanal sunucuları düzenleme](./media/pulse-secure-virtual-traffic-manager-tutorial/virtual-servers.png)

b. **Kimlik doğrulama** bölümünde **Düzenle**' ye tıklayın. 

![Kimlik doğrulama bölümü](./media/pulse-secure-virtual-traffic-manager-tutorial/authentication.png)

c. Sanal sunucu için aşağıdaki kimlik doğrulama ayarlarını yapılandırın: 

1. Yetkilendirmesi

    ![sanal sunucu için kimlik doğrulama ayarları](./media/pulse-secure-virtual-traffic-manager-tutorial/authentication-1.png)

    a. **Kimlik doğrulaması! türü**' nde **SAML hizmeti sağlayıcısı** ' nı seçin. 

    b. **AUTH! verbose** içinde herhangi bir kimlik doğrulaması sorununu gidermek Için "Evet" olarak ayarlayın, aksi takdirde, varsayılan olarak "Hayır" olarak bırakın 

2. Kimlik doğrulama oturumu yönetimi-

    ![Kimlik doğrulama oturumu yönetimi](./media/pulse-secure-virtual-traffic-manager-tutorial/authentication-session.png)

    a. **AUTH! Session! cookie_name** için, varsayılan "VS_SamlSP_Auth" olarak bırakın 

    b. **AUTH! Session! Timeout** için, varsayılanı "7200" olarak bırakın 

    c. Kimlik doğrulaması **! oturumu! log_external_state**, tüm kimlik doğrulama sorunlarını gidermek Için "Evet" olarak ayarlayın; Aksi takdirde, varsayılan olarak "Hayır" olarak bırakın 

    d. **Kimlik doğrulaması! oturum! cookie_attributes**, "HttpOnly" olarak değiştirin 

3. SAML hizmet sağlayıcısı-

    ![SAML hizmeti sağlayıcısı](./media/pulse-secure-virtual-traffic-manager-tutorial/saml-service-provider.png)

    a. **AUTH! SAML! sp_entity_id** metin kutusunda, Azure AD çoklu oturum açma yapılandırma tanımlayıcısı (varlık kimliği) olarak kullanılan aynı URL 'ye ayarlayın. Benzer `https://pulseweb.labb.info/saml/metadata` . 

    b. **AUTH! SAML! sp_acs_url**, Azure AD çoklu oturum açma yapılandırması YENIDEN yürütme URL 'Si (onaylama tüketici hizmeti URL 'si) olarak kullanılan aynı URL 'ye ayarlanır. Benzer `https://pulseweb.labb.info/saml/consume` . 

    c. **AUTH! SAML! IDP**'de, önceki adımda oluşturduğunuz **güvenilen kimlik sağlayıcısını** seçin. 

    d. Auth! SAML! time_tolerance, varsayılan "5" saniye olarak bırakın. 

    e. Auth! SAML! nameid_format **belirtilmemiş**' i seçin.

    f. Sayfanın alt kısmındaki **Güncelleştir** ' i tıklatarak değişiklikleri uygulayın.
    
### <a name="create-pulse-secure-virtual-traffic-manager-test-user"></a>Pulse Secure sanal Traffic Manager test kullanıcısı oluşturma

Bu bölümde, darbeli güvenli sanal Traffic Manager Britta Simon adlı bir Kullanıcı oluşturacaksınız. Pulse Secure sanal Traffic Manager platformunda kullanıcıları eklemek için [darbeli güvenli sanal Traffic Manager Destek ekibi](mailto:support@pulsesecure.net) ile çalışın. Çoklu oturum açma kullanılmadan önce kullanıcıların oluşturulması ve etkinleştirilmesi gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

1. Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz darbeli güvenli sanal Traffic Manager oturum açma URL 'sine yönlendirecektir. 

2. Darbeli güvenli sanal Traffic Manager oturum açma URL 'sine doğrudan gidin ve oturum akışını buradan başlatın.

3. Microsoft Access panel ' i kullanabilirsiniz. Erişim panelinde Pulse Secure sanal Traffic Manager kutucuğuna tıkladığınızda bu, Pulse Secure sanal Traffic Manager oturum açma URL 'sine yeniden yönlendirilir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](../user-help/my-apps-portal-end-user-access.md).

## <a name="next-steps"></a>Sonraki adımlar

Pulse Secure sanal Traffic Manager yapılandırıldıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).