---
title: 'Öğretici: Citrix NetScaler ile çoklu oturum açma tümleştirmesi Azure Active Directory (üst bilgi tabanlı kimlik doğrulaması) | Microsoft Docs'
description: Üst bilgi tabanlı kimlik doğrulaması kullanarak Azure Active Directory ve Citrix NetScaler arasında çoklu oturum açma (SSO) yapılandırma hakkında bilgi edinin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: b0adc7bf-696d-44c9-a57a-f9e9471b8710
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 07ea6824975d0cb3f4b909db41188c490bbba6d2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80477948"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-citrix-netscaler-header-based-authentication"></a>Öğretici: Citrix NetScaler ile çoklu oturum açma tümleştirmesi Azure Active Directory (üst bilgi tabanlı kimlik doğrulaması)

Bu öğreticide, Citrix NetScaler 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Citrix NetScaler 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Citrix NetScaler erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Citrix NetScaler 'da otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Citrix NetScaler çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. Öğreticide şu senaryolar yer almaktadır:

* **SP tarafından başlatılan** Citrix NetScaler için SSO

* Citrix NetScaler için **tam zamanında** Kullanıcı sağlama

* [Citrix NetScaler için üst bilgi tabanlı kimlik doğrulama](#publish-the-web-server)

* [Citrix NetScaler için Kerberos tabanlı kimlik doğrulaması](citrix-netscaler-tutorial.md#publish-the-web-server)

## <a name="add-citrix-netscaler-from-the-gallery"></a>Galeriden Citrix NetScaler ekleyin

Citrix NetScaler 'ı Azure AD ile tümleştirmek için, önce galerideki yönetilen SaaS uygulamaları listenize Citrix NetScaler ' ı ekleyin:

1. [Azure Portal](https://portal.azure.com) iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.

1. Sol menüden **Azure Active Directory**' yi seçin.

1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.

1. **Galeriden Ekle** bölümünde, arama kutusuna **Citrix NetScaler** yazın.

1. Sonuçlarda **Citrix NetScaler**' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-citrix-netscaler"></a>Citrix NetScaler için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, Citrix NetScaler Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Citrix NetScaler içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Citrix NetScaler ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso) -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.

    1. B. Simon ile Azure AD SSO 'yu test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .

    1. Azure AD [Test kullanıcısına atama](#assign-the-azure-ad-test-user) -Azure AD SSO 'yu kullanmak için B. Simon 'u etkinleştirmek için.

1. Uygulama tarafında SSO ayarlarını yapılandırmak için [Citrix NetScaler SSO 'Yu yapılandırın](#configure-citrix-netscaler-sso) .

    * Citrix [NetScaler test kullanıcısı oluşturun](#create-a-citrix-netscaler-test-user) -kullanıcının Azure AD gösterimine bağlı olan Citrix NetScaler 'da B. Simon 'a karşılık gelen bir.

1. [Test SSO](#test-sso) -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure AD SSO 'yu Azure portal kullanarak etkinleştirmek için şu adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/), **Citrix NetScaler** uygulama tümleştirme bölmesindeki **Yönet**bölümünde **Çoklu oturum açma**' yı seçin.

1. **Çoklu oturum açma yöntemi seç** bölmesinde **SAML**' yi seçin.

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem **düzenleme** simgesini seçin.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP tarafından başlatılan** modda yapılandırmak için:

    1. **Tanımlayıcı** metin kutusuna aşağıdaki düzene sahıp bir URL girin:`https://<Your FQDN>`

    1. **Yanıt URL 'si** metin kutusuna aşağıdaki düzene sahıp bir URL girin:`https://<Your FQDN>/CitrixAuthService/AuthService.asmx`

1. Uygulamayı **SP tarafından başlatılan** modda yapılandırmak Için **ek URL 'ler ayarla** ' yı seçin ve aşağıdaki adımı doldurun:

    * **Oturum açma URL 'si** metin kutusunda, aşağıdaki düzene sahıp bir URL girin:`https://<Your FQDN>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > * Bu bölümde kullanılan URL 'Ler gerçek değer değildir. Bu değerleri tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI için gerçek değerlerle güncelleştirin. Bu değerleri almak için [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.
    > * SSO 'yu ayarlamak için, URL 'Lerin genel Web sitelerinden erişilebilir olması gerekir. Yapılandırılmış URL 'de belirteç göndermek için Citrix NetScaler tarafında güvenlik duvarını veya diğer güvenlik ayarlarını Azure AD 'ye uygun hale getirmek zorundasınız.

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'si**Için, URL 'yi kopyalayın ve Not defteri 'ne kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Citrix NetScaler uygulaması SAML onayları 'nin belirli bir biçimde olmasını bekler, bu da SAML belirteci öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektirir. Aşağıdaki ekran görüntüsünde varsayılan özniteliklerin listesi gösterilmektedir. **Düzenle** simgesini seçin ve öznitelik eşlemelerini değiştirin.

    ![SAML öznitelik eşlemesini Düzenle](common/edit-attribute.png)

1. Citrix NetScaler uygulaması, SAML yanıtına daha fazla özniteliğin geri geçirilmesini de bekler. **Kullanıcı öznitelikleri** iletişim kutusunda, **Kullanıcı talepleri**' ın altında, SAML belirteci özniteliklerini tabloda gösterildiği gibi eklemek için aşağıdaki adımları izleyin:

    | Adı | Kaynak özniteliği|
    | ---------------| --------------- |
    | Mysecretıd  | User. UserPrincipalName |
    
    1. **Kullanıcı taleplerini Yönet** iletişim kutusunu açmak için **yeni talep Ekle** ' yi seçin.

    1. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını girin.

    1. **Ad alanını** boş bırakın.

    1. **Öznitelik**için **kaynak**' ı seçin.

    1. **Kaynak özniteliği** listesinde, bu satır için gösterilen öznitelik değerini girin.

    1. **Tamam**’ı seçin.

    1. **Kaydet**’i seçin.

1. **Citrix NetScaler ayarla** bölümünde, gereksinimlerinize göre Ilgili URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol menüsünde **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

1. Bölmenin en üstünde bulunan **Yeni Kullanıcı** ' yı seçin.

1. **Kullanıcı** özellikleri ' nde şu adımları uygulayın:

   1. **Ad**için girin `B.Simon`.  

   1. **Kullanıcı adı**için girin _username@companydomain.extension_. Örneğin, `B.Simon@contoso.com`.

   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola**' ya, sonra da görüntülenen değeri yazın veya kopyalayın.

   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Kullanıcı, Citrix NetScaler 'a erişim izni vererek, B. Simon kullanıcısını Azure SSO 'yu kullanacak şekilde etkinleştirecektir.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.

1. Uygulamalar listesinde **Citrix NetScaler**' ı seçin.

1. Uygulamaya genel bakış bölümünde, **Yönet**altında **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin. Sonra **atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinden **B. Simon** öğesini seçin. **Seç**’i seçin.

1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, listeden Kullanıcı için ilgili rolü seçin ve ardından **Seç**' i seçin.

1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-citrix-netscaler-sso"></a>Citrix NetScaler SSO 'yu yapılandırma

Yapılandırmak istediğiniz kimlik doğrulaması türü için adımlar için bir bağlantı seçin:

- [Üst bilgi tabanlı kimlik doğrulaması için Citrix NetScaler SSO yapılandırma](#publish-the-web-server)

- [Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler SSO 'yu yapılandırma](citrix-netscaler-tutorial.md#publish-the-web-server)

### <a name="publish-the-web-server"></a>Web sunucusunu yayımlama 

Bir sanal sunucu oluşturmak için:

1. **Trafik yönetimi** > **Yük Dengeleme** > **Hizmetleri**' ni seçin.
    
1. **Add (Ekle)** seçeneğini belirleyin.

    ![Citrix NetScaler yapılandırma-hizmetler bölmesi](./media/header-citrix-netscaler-tutorial/web01.png)

1. Uygulamaları çalıştıran Web sunucusu için aşağıdaki değerleri ayarlayın:

   * **Hizmet adı**
   * **Sunucu IP/varolan sunucu**
   * **Protocol**
   * **Bağ**

     ![Citrix NetScaler yapılandırma bölmesi](./media/header-citrix-netscaler-tutorial/web01.png)

### <a name="configure-the-load-balancer"></a>Yük dengeleyiciyi yapılandırma

Yük dengeleyiciyi yapılandırmak için:

1. **Trafik yönetimi** > **Yük Dengeleme** > **sanal sunucularına**gidin.

1. **Add (Ekle)** seçeneğini belirleyin.

1. Aşağıdaki değerleri aşağıdaki ekran görüntüsünde açıklandığı gibi ayarlayın:

    * **Adı**
    * **Protocol**
    * **IP Adresi**
    * **Bağ**

1. **Tamam**’ı seçin.

    ![Citrix NetScaler yapılandırması-temel ayarlar bölmesi](./media/header-citrix-netscaler-tutorial/load01.png)

### <a name="bind-the-virtual-server"></a>Sanal sunucuyu bağlama

Yük dengeleyiciyi sanal sunucuyla bağlamak için:

1. **Hizmetler ve hizmet grupları** bölmesinde, **Yük Dengeleme sanal sunucu hizmeti bağlamayı yok**' u seçin.

   ![Citrix NetScaler yapılandırması-sanal sunucu hizmeti bağlama bölmesi yük dengelemesi](./media/header-citrix-netscaler-tutorial/bind01.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve ardından **Kapat**' ı seçin.

   ![Citrix NetScaler yapılandırması-Virtual Server Hizmetleri bağlamayı doğrulama](./media/header-citrix-netscaler-tutorial/bind02.png)

### <a name="bind-the-certificate"></a>Sertifikayı bağlama

Bu hizmeti TLS olarak yayımlamak için sunucu sertifikasını bağlayın ve ardından uygulamanızı test edin:

1. **Sertifika**altında **sunucu sertifikası yok**' u seçin.

   ![Citrix NetScaler yapılandırması-sunucu sertifikası bölmesi](./media/header-citrix-netscaler-tutorial/bind03.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve ardından **Kapat**' ı seçin.

   ![Citrix NetScaler yapılandırması-sertifikayı doğrulama](./media/header-citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Citrix ADC SAML profili

Citrix ADC SAML profilini yapılandırmak için aşağıdaki bölümleri doldurun:

### <a name="create-an-authentication-policy"></a>Kimlik doğrulama ilkesi oluşturma

Kimlik doğrulama ilkesi oluşturmak için:

1. **Güvenlik** > **aaa – uygulama trafik** > **ilkeleri** > **kimlik** > doğrulama**ilkeleri**' ne gidin.

1. **Add (Ekle)** seçeneğini belirleyin.

1. **Kimlik doğrulama Ilkesi oluştur** bölmesinde, aşağıdaki değerleri girin veya seçin:

    * **Ad**: kimlik doğrulama ilkeniz için bir ad girin.
    * **Eylem**: **SAML**yazın ve ardından **Ekle**' yi seçin.
    * **İfade**: **true**girin.     
    
    ![Citrix NetScaler yapılandırması-kimlik doğrulama Ilkesi bölmesi oluştur](./media/header-citrix-netscaler-tutorial/policy01.png)

1. **Oluştur**’u seçin.

### <a name="create-an-authentication-saml-server"></a>Kimlik doğrulama SAML sunucusu oluşturma

Bir kimlik doğrulama SAML sunucusu oluşturmak için, **kimlik doğrulaması SAML sunucusu oluştur** bölmesine gidin ve aşağıdaki adımları tamamlayın:

1. **Ad**için, KIMLIK doğrulaması SAML sunucusu için bir ad girin.

1. **SAML meta verilerini dışarı aktar**altında:

   1. **Meta verileri Içeri aktar** onay kutusunu seçin.

   1. Daha önce kopyaladığınız Azure SAML kullanıcı arabiriminden Federasyon meta verileri URL 'sini girin.
    
1. **Verenin adı**için ilgili URL 'yi girin.

1. **Oluştur**’u seçin.

![Citrix NetScaler yapılandırması-kimlik doğrulama SAML sunucu bölmesi oluştur](./media/header-citrix-netscaler-tutorial/server01.png)

### <a name="create-an-authentication-virtual-server"></a>Kimlik doğrulama sanal sunucusu oluşturma

Kimlik doğrulama sanal sunucusu oluşturmak için:

1.  **Güvenlik** > **aaa-uygulama trafik** > **ilkeleri** > **kimlik** > doğrulama**sanal sunucuları**' na gidin.

1.  **Ekle**' yi seçin ve ardından aşağıdaki adımları uygulayın:

    1. **Ad**için, kimlik doğrulama sanal sunucusu için bir ad girin.

    1. **Adreslenebilir olmayan** onay kutusunu seçin.

    1. **Protokol**için **SSL**' yi seçin.

    1. **Tamam**’ı seçin.

    ![Citrix NetScaler yapılandırması-kimlik doğrulama sanal sunucu bölmesi](./media/header-citrix-netscaler-tutorial/server02.png)
    
### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Kimlik doğrulama sanal sunucusunu Azure AD kullanacak şekilde yapılandırma

Kimlik doğrulama sanal sunucusu için iki bölümü değiştirin:

1.  **Gelişmiş kimlik doğrulama ilkeleri** bölmesinde **kimlik doğrulama ilkesi yok**' u seçin.

    ![Citrix NetScaler yapılandırması-Gelişmiş kimlik doğrulama Ilkeleri bölmesi](./media/header-citrix-netscaler-tutorial/virtual01.png)

1. **Ilke bağlama** bölmesinde, kimlik doğrulama ilkesini seçin ve ardından **bağla**' yı seçin.

    ![Citrix NetScaler yapılandırma-Ilke bağlama bölmesi](./media/header-citrix-netscaler-tutorial/virtual02.png)

1. **Form tabanlı sanal sunucular** bölmesinde, **Yük Dengeleme sanal sunucusu yok**' u seçin.

    ![Citrix NetScaler yapılandırma-form tabanlı sanal sunucular bölmesi](./media/header-citrix-netscaler-tutorial/virtual03.png)

1. **Kimlik doğrulama FQDN 'si**için tam etki alanı adı (FQDN) (gerekli) girin.

1. Azure AD kimlik doğrulamasıyla korumak istediğiniz yük dengeleme sanal sunucusunu seçin.

1. **Bağla**' yı seçin.

    ![Citrix NetScaler yapılandırması-sanal sunucu bağlama bölmesi yük dengelemesi](./media/header-citrix-netscaler-tutorial/virtual04.png)

    > [!NOTE]
    > **Kimlik doğrulama sanal sunucu yapılandırma** bölmesinde **bitti** ' yi seçtiğinizden emin olun.

1. Değişikliklerinizi doğrulamak için, bir tarayıcıda uygulama URL 'sine gidin. Daha önce gördüğünüz kimliği doğrulanmamış erişim yerine kiracı oturum açma sayfanızı görmeniz gerekir.

    ![Citrix NetScaler yapılandırması-bir Web tarayıcısında oturum açma sayfası](./media/header-citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-sso-for-header-based-authentication"></a>Üst bilgi tabanlı kimlik doğrulaması için Citrix NetScaler SSO yapılandırma

### <a name="configure-citrix-adc"></a>Citrix ADC 'yi yapılandırma

Üst bilgi tabanlı kimlik doğrulaması için Citrix ADC yapılandırmak için aşağıdaki bölümleri doldurun.

#### <a name="create-a-rewrite-action"></a>Yeniden yazma eylemi oluştur

1. **Appexpert** > **yeniden** > yazma**eylemleri**' ne gidin.
 
    ![Citrix NetScaler yapılandırması-yeniden yazma eylemleri bölmesi](./media/header-citrix-netscaler-tutorial/header01.png)

1.  **Ekle**' yi seçin ve ardından aşağıdaki adımları uygulayın:

    1. **Ad**için, yeniden yazma eylemi için bir ad girin.

    1. **Tür**için **INSERT_HTTP_HEADER**girin.

    1. **Üst bilgi adı**için bir başlık adı girin (Bu örnekte, _secretıd_kullanırız).

    1. **İfade**için AAA girin **. Kullanıcısını. ÖZNITELIĞI ("Mysecretıd")**, burada **Mysecretıd** , Citrix ADC 'ye GÖNDERILEN Azure AD SAML talebi.

    1. **Oluştur**’u seçin.

    ![Citrix NetScaler yapılandırması-yeniden yazma eylem bölmesi oluştur](./media/header-citrix-netscaler-tutorial/header02.png)
 
#### <a name="create-a-rewrite-policy"></a>Yeniden yazma ilkesi oluşturma

1.  **Appexpert** > **yeniden** > yazma**ilkelerine**gidin.
 
    ![Citrix NetScaler yapılandırması-Ilkeleri yeniden yaz bölmesi](./media/header-citrix-netscaler-tutorial/header03.png)

1.  **Ekle**' yi seçin ve ardından aşağıdaki adımları uygulayın:

    1. **Ad**için, yeniden yazma ilkesi için bir ad girin.

    1. **Eylem**için, önceki bölümde oluşturduğunuz yeniden yazma eylemini seçin.

    1. **İfade**için **true**girin.

    1. **Oluştur**’u seçin.

    ![Citrix NetScaler yapılandırması-yeniden yazma Ilkesi bölmesi oluştur](./media/header-citrix-netscaler-tutorial/header04.png)

### <a name="bind-a-rewrite-policy-to-a-virtual-server"></a>Yeniden yazma ilkesini bir sanal sunucuya bağlama

GUI kullanarak bir yeniden yazma ilkesini bir sanal sunucuya bağlamak için:

1. **Trafik yönetimi** > **Yük Dengeleme** > **sanal sunucularına**gidin.

1. Sanal sunucular listesinde, yeniden yazma ilkesini bağlamak istediğiniz sanal sunucuyu seçin ve **Aç**' ı seçin.

1. **Yük Dengeleme sanal sunucusu** bölmesinde, **Gelişmiş ayarlar**altında **ilkeler**' i seçin. NetScaler örneğiniz için yapılandırılmış tüm ilkeler listede görüntülenir.
 
    ![Citrix NetScaler yapılandırması-sanal sunucu bölmesini yükle](./media/header-citrix-netscaler-tutorial/header05.png)

    ![Citrix NetScaler yapılandırması-sanal sunucu bölmesini yükle](./media/header-citrix-netscaler-tutorial/header06.png)

1.  Bu sanal sunucuya bağlamak istediğiniz ilkenin adının yanındaki onay kutusunu işaretleyin.
 
    ![Citrix NetScaler yapılandırması-sanal sunucu trafik Ilkesi bağlama bölmesi yük dengelemesi](./media/header-citrix-netscaler-tutorial/header08.png)

1. **Tür Seç** iletişim kutusunda:

    1. **Ilke Seç**için **trafik**' i seçin.

    1. **Tür Seç**için **istek**' ı seçin.

    ![Citrix NetScaler yapılandırma-Ilkeler iletişim kutusu](./media/header-citrix-netscaler-tutorial/header07.png)

1.  **Tamam**’ı seçin. Durum çubuğundaki bir ileti, ilkenin başarıyla yapılandırıldığını gösterir.

### <a name="modify-the-saml-server-to-extract-attributes-from-a-claim"></a>Bir talepten öznitelikleri ayıklamak için SAML sunucusunu değiştirin

1.  **Güvenlik** > **aaa-uygulama trafik** > **ilkeleri** > **kimlik doğrulama** > **Gelişmiş İlkeler** > **Eylemler** > **sunucular**' a gidin.

1.  Uygulama için uygun kimlik doğrulama SAML sunucusunu seçin.
 
    ![Citrix NetScaler yapılandırması-kimlik doğrulama SAML sunucu bölmesini yapılandırma](./media/header-citrix-netscaler-tutorial/header09.png)

1. **Özniteliklerde** sorun, AYıKLAMAK istediğiniz SAML özniteliklerini virgülle ayırarak girin. Örneğimizde özniteliği `mySecretID`giriyoruz.
 
    ![Citrix NetScaler yapılandırması-Öznitelikler bölmesi](./media/header-citrix-netscaler-tutorial/header10.png)

1. Erişimi doğrulamak için, bir tarayıcıdaki URL 'de, **üstbilgiler koleksiyonu**altında saml özniteliği ' ni arayın.

    ![Citrix NetScaler yapılandırması-URL 'de üst bilgi koleksiyonu](./media/header-citrix-netscaler-tutorial/header11.png)

### <a name="create-a-citrix-netscaler-test-user"></a>Citrix NetScaler test kullanıcısı oluşturma

Bu bölümde, Citrix NetScaler 'da B. Simon adlı bir Kullanıcı oluşturulur. Citrix NetScaler, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde gerçekleştirmeniz gereken bir işlem yok. Bir Kullanıcı Citrix NetScaler 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html)başvurun.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD SSO yapılandırmanızı test edersiniz.

Erişim panelinde Citrix NetScaler kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Citrix NetScaler 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Citrix NetScaler 'ı deneyin](https://aad.portal.azure.com/)

- [Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler çoklu oturum açmayı yapılandırma](citrix-netscaler-tutorial.md)
