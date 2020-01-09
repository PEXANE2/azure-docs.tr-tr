---
title: 'Öğretici: Citrix NetScaler ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory (Kerberos tabanlı kimlik doğrulaması) | Microsoft Docs'
description: Azure Active Directory ve Citrix NetScaler arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: af501bd0-8ff5-468f-9b06-21e607ae25de
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 12/13/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 75e825f55a890be49000e209859670caa2c1c875
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75431207"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-citrix-netscaler-kerberos-based-authentication"></a>Öğretici: Citrix NetScaler ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory (Kerberos tabanlı kimlik doğrulaması)

Bu öğreticide, Citrix NetScaler 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Citrix NetScaler 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Citrix NetScaler erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Citrix NetScaler 'da otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Citrix NetScaler çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz.

* Citrix NetScaler, **SP** tarafından başlatılan SSO 'yu destekler

* Citrix NetScaler **, tam zamanında** Kullanıcı sağlamayı destekliyor

- [Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler çoklu oturum açmayı yapılandırma](#configure-citrix-netscaler-single-sign-on-for-kerberos-based-authentication)

- [Üst bilgi tabanlı kimlik doğrulaması için Citrix NetScaler çoklu oturum açmayı yapılandırma](header-citrix-netscaler-tutorial.md)

## <a name="adding-citrix-netscaler-from-the-gallery"></a>Galeriden Citrix NetScaler ekleme

Citrix NetScaler 'ın Azure AD 'ye tümleştirmesini yapılandırmak için, Galeriden Citrix NetScaler ' ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Bir iş veya okul hesabını ya da kişisel bir Microsoft hesabını kullanarak [Azure portalda](https://portal.azure.com) oturum açın.
1. Sol gezinti bölmesinde **Azure Active Directory** hizmeti ' ni seçin.
1. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar**' ı seçin.
1. Yeni uygulama eklemek için **Yeni uygulama**' yı seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **Citrix NetScaler** yazın.
1. Sonuçlar panelinden **Citrix NetScaler** ' ı seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-citrix-netscaler"></a>Citrix NetScaler için Azure AD çoklu oturum açmayı yapılandırma ve test etme

**B. Simon**adlı bir test kullanıcısı kullanarak, Citrix NetScaler Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Citrix NetScaler içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Citrix NetScaler ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını doldurun:

1. **[Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso)** -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.
    1. Azure AD **[test kullanıcısı oluşturun](#create-an-azure-ad-test-user)** -B. Simon Ile Azure AD çoklu oturum açma sınamasını test edin.
    1. Azure AD **[Test kullanıcısına atama](#assign-the-azure-ad-test-user)** -Azure AD çoklu oturum açma özelliğini kullanmak için B. Simon 'u etkinleştirmek için.
1. **[Citrix NetScaler SSO 'Yu yapılandırma](#configure-citrix-netscaler-sso)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
    1. Citrix **[NetScaler test kullanıcısı oluşturun](#create-citrix-netscaler-test-user)** ; Citrix NetScaler 'Da kullanıcının Azure AD gösterimine bağlanan B. Simon 'a karşılık gelen bir karşılığı vardır.
1. **[Test SSO](#test-sso)** -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO 'yu yapılandırma

Azure portal Azure AD SSO 'yu etkinleştirmek için bu adımları izleyin.

1. [Azure Portal](https://portal.azure.com/), **Citrix NetScaler** uygulama tümleştirmesi sayfasında, **Yönet** bölümünü bulun ve **Çoklu oturum açma**' yı seçin.
1. **Çoklu oturum açma yöntemi seçin** sayfasında **SAML**' yi seçin.
1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, ayarları düzenlemek IÇIN **temel SAML yapılandırması** için Düzenle/kalem simgesine tıklayın.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, **IDP** tarafından başlatılan modda uygulamayı yapılandırmak istiyorsanız aşağıdaki alanlar için değerleri girin:

    a. **Tanımlayıcı** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<<Your FQDN>>`

    b. **Yanıt URL 'si** metin kutusuna şu kalıbı kullanarak bir URL yazın: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`

1. Uygulamayı **SP** tarafından başlatılan modda yapılandırmak Istiyorsanız **ek URL 'ler ayarla** ' ya tıklayın ve aşağıdaki adımı gerçekleştirin:

    **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın: `https://<<Your FQDN>>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirin. Bu değerleri almak için [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.

    > [!NOTE]
    > SSO 'nun çalışması için bu URL 'Lerin ortak sitelerden erişilebilir olması gerekir. Yapılandırılmış ACS URL 'sine belirteç göndermek için, NetScaler tarafında güvenlik duvarını veya diğer güvenlik ayarlarını Azure AD 'ye uygun hale getirmeniz gerekir.

1. **SAML ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'sini**bulun, URL 'yi kopyalayın ve Not defteri 'nize kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Citrix NetScaler ayarla** bölümünde, gereksiniminize göre uygun URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Bir Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol bölmeden **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.
1. Seçin **yeni kullanıcı** ekranın üstünde.
1. **Kullanıcı** özellikleri ' nde şu adımları izleyin:
   1. **Ad** alanına `B.Simon` girin.  
   1. **Kullanıcı adı** alanına username@companydomain.extensiongirin. Örneğin, `B.Simon@contoso.com`.
   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola** kutusunda görüntülenen değeri yazın.
   1. **Oluştur**'a tıklayın.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısı atayın

Bu bölümde, Citrix NetScaler 'a erişim vererek, B. Simon 'u Azure çoklu oturum açma özelliğini kullanacak şekilde etkinleştireceksiniz.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.
1. Uygulamalar listesinde **Citrix NetScaler**' ı seçin.
1. Uygulamanın genel bakış sayfasında **Yönet** bölümünü bulun ve **Kullanıcılar ve gruplar**' ı seçin.

   !["Kullanıcılar ve Gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle**' yi seçin, sonra **atama Ekle** iletişim kutusunda **Kullanıcılar ve gruplar** ' ı seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. **Kullanıcılar ve gruplar** iletişim kutusunda, kullanıcılar listesinden **B. Simon** ' ı seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. SAML assertion 'da herhangi bir rol değeri bekliyorsanız, **Rol Seç** iletişim kutusunda, Kullanıcı için listeden uygun rolü seçin ve ardından ekranın alt kısmındaki **Seç** düğmesine tıklayın.
1. **Atama Ekle** Iletişim kutusunda **ata** düğmesine tıklayın.

## <a name="configure-citrix-netscaler-sso"></a>Citrix NetScaler SSO 'yu yapılandırma

- [Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler çoklu oturum açmayı yapılandırma](#configure-citrix-netscaler-single-sign-on-for-kerberos-based-authentication)

- [Üst bilgi tabanlı kimlik doğrulaması için Citrix NetScaler çoklu oturum açmayı yapılandırma](header-citrix-netscaler-tutorial.md)

### <a name="publishing-web-server"></a>Web sunucusu yayımlanıyor 

1. Sanal bir **sunucu**oluşturun.

    a. **Trafik yönetimi > yük dengeleme > Hizmetleri**' ne gidin.
    
    b. **Ekle**'ye tıklayın.

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/web01.png)

    c. Aşağıdaki uygulamaları çalıştıran Web sunucusunun ayrıntılarını belirtin:
    * **Hizmet adı**
    * **Sunucu IP/varolan sunucu**
    * **Protokol**
    * **Bağlantı Noktası**

     ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/web01.png)

### <a name="configuring-load-balancer"></a>Load Balancer yapılandırma

1. Load Balancer yapılandırmak için aşağıdaki adımları uygulayın:

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/load01.png)

    a. **Sanal sunucular > yük dengelemeye > trafik yönetimine**gidin.

    b. **Ekle**'ye tıklayın.

    c. Aşağıdaki ayrıntıları belirtin:

    * **Adı**
    * **Protokol**
    * **IP Adresi**
    * **Bağlantı Noktası**
    * **Tamam 'a** tıklayın

### <a name="bind-virtual-server"></a>Sanal sunucuyu bağla

Load Balancer daha önce oluşturulan sanal sunucu ile bağlayın.

![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/bind01.png)

![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/bind02.png)

### <a name="bind-certificate"></a>Sertifika bağlama

Bu hizmeti SSL olarak yayımladığımızdan, sunucu sertifikasını bağlayın ve ardından uygulamanızı test edin.

![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/bind03.png)

![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Citrix ADC SAML profili

### <a name="create-authentication-policy"></a>Kimlik doğrulama Ilkesi oluştur

1. **Kimlik doğrulama > kimlik doğrulama ilkelerine > güvenlik > aaa – uygulama trafiği > ilkelerine**gidin.

2. **Ekle** ' ye tıklayın ve ardından ayrıntıları belirtin.

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/policy01.png)

    a. **Kimlik doğrulama ilkesinin**adı.

    b. İfade: **true**.

    c. **SAML**eylem türü.

    d. Eylem = **Ekle** ' ye tıklayın (KIMLIK doğrulama SAML sunucusu oluşturma Sihirbazı ' nı izleyin).
    
    e. **Kimlik doğrulama Ilkesinde**oluştur ' a tıklayın.

### <a name="create-authentication-saml-server"></a>Kimlik doğrulama SAML sunucusu oluşturma

1. Aşağıdaki adımları uygulayın:

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/server01.png)

    a. **Adı**belirtin.

    b. Meta verileri içeri aktar (yukarıda kopyaladığınız Azure SAML kullanıcı arabiriminden Federasyon meta veri URL 'sini belirtin).
    
    c. **Verenin adını**belirtin.

    d. **Oluştur**' a tıklayın.

### <a name="create-authentication-virtual-server"></a>Kimlik doğrulama sanal sunucusu oluştur

1.  **> kimlik doğrulaması sanal sunucuları > güvenlik > aaa-uygulama trafiği**' ne gidin.

2.  **Ekle** ' ye tıklayın ve aşağıdaki adımları gerçekleştirin:

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/server02.png)

    a.  Sağlayan bir **adı**.

    b.  **Adreslenebilir değil**' i seçin.

    c.  Protokol **SSL**.

    d.  **Tamam**’a tıklayın.

    e.  **Devam**’a tıklayın.

### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Kimlik doğrulama sanal sunucusunu Azure AD kullanacak şekilde yapılandırma

Kimlik doğrulama sanal sunucusunun 2 bölümünü değiştirmeniz gerekir.

1.  **Gelişmiş kimlik doğrulama Ilkeleri**

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/virtual01.png)

    a. Daha önce oluşturduğunuz **kimlik doğrulama ilkesini** seçin.

    b. **Bağla**' ya tıklayın.

      ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/virtual02.png)

2. **Form tabanlı sanal sunucular**

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/virtual03.png)

    a.  Kullanıcı arabirimi tarafından zorlandığından bir **FQDN** sağlamanız gerekir.

    b.  Azure AD kimlik doğrulamasıyla korumak istediğiniz **sanal sunucu Load Balancer** seçin.

    c.  **Bağla**' ya tıklayın.

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/virtual04.png)

    >[!NOTE]
    >Kimlik doğrulama sanal sunucu yapılandırma sayfasında **bitti** ' ye tıklatığınızdan emin olun.

3. Değişiklikleri doğrulayın. Uygulama URL 'sine gidin. Daha önce kimliği doğrulanmamış erişim yerine kiracı oturum açma sayfanızı görmeniz gerekir.

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-single-sign-on-for-kerberos-based-authentication"></a>Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler çoklu oturum açmayı yapılandırma

### <a name="create-a-kerberos-delegation-account-for-citrix-adc"></a>Citrix ADC için Kerberos temsili hesabı oluşturma

1. Bir kullanıcı hesabı oluşturun (Bu örnekte Apptemsili).

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos01.png)

2. Bu hesaplarda bir konak SPN 'si ayarlayın.

    * Setspn-S HOST/Apptemsilcisini. ıDENTT. WORK ıdentt\apptemsili
    
        Yukarıdaki örnekte

        a. Identt. Work (etki alanı FQDN 'SI)

        b. Identt (etki alanı NetBIOS adı)

        c. Apptemsili (temsili Kullanıcı hesabı adı)

3. Web sunucusu için temsilciyi yapılandırma 
 
    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos02.png)

    >[!NOTE]
    >Yukarıdaki örnekte, WIA sitesini çalıştıran Iç Web sunucusu adı cweb2 ' dir.

### <a name="citrix-aaa-kcd--kerberos-delegation-accounts"></a>Citrix AAA KCD (Kerberos temsili hesapları)

1.  **Citrix Gateway > aaa KCD (Kerberos kısıtlanmış temsili) hesaplarına**gidin.

2.  Ekle ' ye tıklayın ve aşağıdaki ayrıntıları belirtin:

    a.  **Ad**belirtin.

    b.  **Realm**.

    c.  **HIZMET SPN** 'si`http/<host/fqdn>@DOMAIN.COM`.
    
    >[!NOTE]
    >@DOMAIN.com zorunludur ve büyük harfle yazılır.

    d.  **Temsil edilen kullanıcı hesabını**belirtin.

    e.  Temsilci kullanıcı için parolayı denetleyin ve **parolayı**belirtin.

    f.  **Tamam**’a tıklayın.
 
    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos03.png)

### <a name="citrix-traffic-policy-and-traffic-profile"></a>Citrix trafik Ilkesi ve trafik profili

1.  **Trafik ilkeleri, profiller ve form SSO ProfilesTraffic ilkeleri > güvenlik > aaa-uygulama trafiği > ilkelerine**gidin.

2.  **Trafik profillerini**seçin.

3.  **Ekle**'ye tıklayın.

4.  Trafik profilini yapılandırın.

    a.  **Ad**belirtin.

    b.  **Çoklu oturum açma**belirtin.

    c.  Açılan listeden daha önce oluşturulan **KCD hesabını** belirtin.

    d.  **Tamam**’a tıklayın.

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos04.png)
 
5.  **Trafik ilkesi**seçin.

6.  **Ekle**'ye tıklayın.

7.  Trafik Ilkesini yapılandırın.

    a.  **Ad**belirtin.

    b.  Açılan listeden önceden oluşturulmuş **trafik profilini** seçin.

    c.  İfadeyi **true**olarak ayarlayın.

    d.  **Tamam**’a tıklayın.

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos05.png)

### <a name="citrix-bind-traffic-policy-to-virtual-servers"></a>Citrix 'e trafik Ilkesini sanal sunuculara bağlama

Bir trafik ilkesini GUI kullanarak belirli bir sanal sunucuya bağlamak için.

* **Sanal sunucular > yük dengeleme > trafik yönetimi**' ne gidin.

* Sanal sunucuların Ayrıntılar bölmesinde listesinde, yeniden yazma ilkesini bağlamak istediğiniz **sanal sunucuyu** seçin ve ardından **Aç**' a tıklayın.

* Sanal sunucuyu Yapılandır (Yük Dengeleme) iletişim kutusunda **ilkeler sekmesini**seçin. NetScaler üzerinde yapılandırılan tüm ilkeler listede görüntülenir.
 
    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos06.png)

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos07.png)

1.  Bu sanal sunucuya bağlamak istediğiniz ilkenin adının yanındaki **onay kutusunu** işaretleyin.
 
    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos08.png)

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos09.png)

1. Yalnızca ilke bağlanır, **bitti**' ye tıklayın.
 
    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos10.png)

1. Windows tümleşik Web sitesini kullanarak test edin.

    ![Citrix NetScaler yapılandırması](./media/citrix-netscaler-tutorial/kerberos11.png)    

### <a name="create-citrix-netscaler-test-user"></a>Citrix NetScaler test kullanıcısı oluşturma

Bu bölümde, Citrix NetScaler 'da B. Simon adlı bir Kullanıcı oluşturulur. Citrix NetScaler, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde sizin için herhangi bir eylem öğesi yok. Bir Kullanıcı Citrix NetScaler 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!NOTE]
> Bir kullanıcıyı el ile oluşturmanız gerekiyorsa, [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html)başvurmanız gerekir.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD çoklu oturum açma yapılandırmanızı test edin.

Erişim panelinde Citrix NetScaler kutucuğuna tıkladığınızda, SSO 'yu ayarladığınız Citrix NetScaler 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Citrix NetScaler 'ı deneyin](https://aad.portal.azure.com/)

- [Üst bilgi tabanlı kimlik doğrulaması için Citrix NetScaler çoklu oturum açmayı yapılandırma](header-citrix-netscaler-tutorial.md)
