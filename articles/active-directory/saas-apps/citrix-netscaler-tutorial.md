---
title: 'Öğretici: Citrix NetScaler ile çoklu oturum açma tümleştirmesi Azure Active Directory (Kerberos tabanlı kimlik doğrulaması) | Microsoft Docs'
description: Kerberos tabanlı kimlik doğrulaması kullanarak Azure Active Directory ve Citrix NetScaler arasında çoklu oturum açma (SSO) yapılandırma hakkında bilgi edinin.
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
ms.topic: tutorial
ms.date: 03/27/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6771060f05a03c82879738dc5e8caccb67e55abc
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80477995"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-citrix-netscaler-kerberos-based-authentication"></a>Öğretici: Citrix NetScaler ile çoklu oturum açma tümleştirmesi Azure Active Directory (Kerberos tabanlı kimlik doğrulaması)

Bu öğreticide, Citrix NetScaler 'ı Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Citrix NetScaler 'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Citrix NetScaler erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Citrix NetScaler 'da otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Citrix NetScaler çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. Öğreticide şu senaryolar yer almaktadır:

* **SP tarafından başlatılan** Citrix NetScaler için SSO

* Citrix NetScaler için **tam zamanında** Kullanıcı sağlama

* [Citrix NetScaler için Kerberos tabanlı kimlik doğrulaması](#publish-the-web-server)

* [Citrix NetScaler için üst bilgi tabanlı kimlik doğrulama](header-citrix-netscaler-tutorial.md#publish-the-web-server)

* Citrix NetScaler 'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve korunmasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletilir. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app).

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

    1. **Yanıt URL 'si** metin kutusuna aşağıdaki düzene sahıp bir URL girin:`http(s)://<Your FQDN>.of.vserver/cgi/samlauth`

1. Uygulamayı **SP tarafından başlatılan** modda yapılandırmak Için **ek URL 'ler ayarla** ' yı seçin ve aşağıdaki adımı doldurun:

    * **Oturum açma URL 'si** metin kutusunda, aşağıdaki düzene sahıp bir URL girin:`https://<Your FQDN>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > * Bu bölümde kullanılan URL 'Ler gerçek değer değildir. Bu değerleri tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI için gerçek değerlerle güncelleştirin. Bu değerleri almak için [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.
    > * SSO 'yu ayarlamak için, URL 'Lerin genel Web sitelerinden erişilebilir olması gerekir. Yapılandırılmış URL 'de belirteç göndermek için Citrix NetScaler tarafında güvenlik duvarını veya diğer güvenlik ayarlarını Azure AD 'ye uygun hale getirmek zorundasınız.

1. **SAML Ile çoklu oturum açmayı ayarla** bölmesinde, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'si**Için, URL 'yi kopyalayın ve Not defteri 'ne kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

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

- [Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler SSO 'yu yapılandırma](#publish-the-web-server)

- [Üst bilgi tabanlı kimlik doğrulaması için Citrix NetScaler SSO yapılandırma](header-citrix-netscaler-tutorial.md#publish-the-web-server)

### <a name="publish-the-web-server"></a>Web sunucusunu yayımlama 

Bir sanal sunucu oluşturmak için:

1. **Trafik yönetimi** > **Yük Dengeleme** > **Hizmetleri**' ni seçin.
    
1. **Add (Ekle)** seçeneğini belirleyin.

    ![Citrix NetScaler yapılandırma-hizmetler bölmesi](./media/citrix-netscaler-tutorial/web01.png)

1. Uygulamaları çalıştıran Web sunucusu için aşağıdaki değerleri ayarlayın:

   * **Hizmet adı**
   * **Sunucu IP/varolan sunucu**
   * **Protocol**
   * **Bağ**

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

    ![Citrix NetScaler yapılandırması-temel ayarlar bölmesi](./media/citrix-netscaler-tutorial/load01.png)

### <a name="bind-the-virtual-server"></a>Sanal sunucuyu bağlama

Yük dengeleyiciyi sanal sunucuyla bağlamak için:

1. **Hizmetler ve hizmet grupları** bölmesinde, **Yük Dengeleme sanal sunucu hizmeti bağlamayı yok**' u seçin.

   ![Citrix NetScaler yapılandırması-sanal sunucu hizmeti bağlama bölmesi yük dengelemesi](./media/citrix-netscaler-tutorial/bind01.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve ardından **Kapat**' ı seçin.

   ![Citrix NetScaler yapılandırması-Virtual Server Hizmetleri bağlamayı doğrulama](./media/citrix-netscaler-tutorial/bind02.png)

### <a name="bind-the-certificate"></a>Sertifikayı bağlama

Bu hizmeti TLS olarak yayımlamak için sunucu sertifikasını bağlayın ve ardından uygulamanızı test edin:

1. **Sertifika**altında **sunucu sertifikası yok**' u seçin.

   ![Citrix NetScaler yapılandırması-sunucu sertifikası bölmesi](./media/citrix-netscaler-tutorial/bind03.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve ardından **Kapat**' ı seçin.

   ![Citrix NetScaler yapılandırması-sertifikayı doğrulama](./media/citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Citrix ADC SAML profili

Citrix ADC SAML profilini yapılandırmak için aşağıdaki bölümleri doldurun.

### <a name="create-an-authentication-policy"></a>Kimlik doğrulama ilkesi oluşturma

Kimlik doğrulama ilkesi oluşturmak için:

1. **Güvenlik** > **aaa – uygulama trafik** > **ilkeleri** > **kimlik** > doğrulama**ilkeleri**' ne gidin.

1. **Add (Ekle)** seçeneğini belirleyin.

1. **Kimlik doğrulama Ilkesi oluştur** bölmesinde, aşağıdaki değerleri girin veya seçin:

    * **Ad**: kimlik doğrulama ilkeniz için bir ad girin.
    * **Eylem**: **SAML**yazın ve ardından **Ekle**' yi seçin.
    * **İfade**: **true**girin.     
    
    ![Citrix NetScaler yapılandırması-kimlik doğrulama Ilkesi bölmesi oluştur](./media/citrix-netscaler-tutorial/policy01.png)

1. **Oluştur**’u seçin.

### <a name="create-an-authentication-saml-server"></a>Kimlik doğrulama SAML sunucusu oluşturma

Bir kimlik doğrulama SAML sunucusu oluşturmak için, **kimlik doğrulaması SAML sunucusu oluştur** bölmesine gidin ve aşağıdaki adımları tamamlayın:

1. **Ad**için, KIMLIK doğrulaması SAML sunucusu için bir ad girin.

1. **SAML meta verilerini dışarı aktar**altında:

   1. **Meta verileri Içeri aktar** onay kutusunu seçin.

   1. Daha önce kopyaladığınız Azure SAML kullanıcı arabiriminden Federasyon meta verileri URL 'sini girin.
    
1. **Verenin adı**için ilgili URL 'yi girin.

1. **Oluştur**’u seçin.

![Citrix NetScaler yapılandırması-kimlik doğrulama SAML sunucu bölmesi oluştur](./media/citrix-netscaler-tutorial/server01.png)

### <a name="create-an-authentication-virtual-server"></a>Kimlik doğrulama sanal sunucusu oluşturma

Kimlik doğrulama sanal sunucusu oluşturmak için:

1.  **Güvenlik** > **aaa-uygulama trafik** > **ilkeleri** > **kimlik** > doğrulama**sanal sunucuları**' na gidin.

1.  **Ekle**' yi seçin ve ardından aşağıdaki adımları uygulayın:

    1. **Ad**için, kimlik doğrulama sanal sunucusu için bir ad girin.

    1. **Adreslenebilir olmayan** onay kutusunu seçin.

    1. **Protokol**için **SSL**' yi seçin.

    1. **Tamam**’ı seçin.
    
1. **Devam**'ı seçin.

### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Kimlik doğrulama sanal sunucusunu Azure AD kullanacak şekilde yapılandırma

Kimlik doğrulama sanal sunucusu için iki bölümü değiştirin:

1.  **Gelişmiş kimlik doğrulama ilkeleri** bölmesinde **kimlik doğrulama ilkesi yok**' u seçin.

    ![Citrix NetScaler yapılandırması-Gelişmiş kimlik doğrulama Ilkeleri bölmesi](./media/citrix-netscaler-tutorial/virtual01.png)

1. **Ilke bağlama** bölmesinde, kimlik doğrulama ilkesini seçin ve ardından **bağla**' yı seçin.

    ![Citrix NetScaler yapılandırma-Ilke bağlama bölmesi](./media/citrix-netscaler-tutorial/virtual02.png)

1. **Form tabanlı sanal sunucular** bölmesinde, **Yük Dengeleme sanal sunucusu yok**' u seçin.

    ![Citrix NetScaler yapılandırma-form tabanlı sanal sunucular bölmesi](./media/citrix-netscaler-tutorial/virtual03.png)

1. **Kimlik doğrulama FQDN 'si**için tam etki alanı adı (FQDN) (gerekli) girin.

1. Azure AD kimlik doğrulamasıyla korumak istediğiniz yük dengeleme sanal sunucusunu seçin.

1. **Bağla**' yı seçin.

    ![Citrix NetScaler yapılandırması-sanal sunucu bağlama bölmesi yük dengelemesi](./media/citrix-netscaler-tutorial/virtual04.png)

    > [!NOTE]
    > **Kimlik doğrulama sanal sunucu yapılandırma** bölmesinde **bitti** ' yi seçtiğinizden emin olun.

1. Değişikliklerinizi doğrulamak için, bir tarayıcıda uygulama URL 'sine gidin. Daha önce gördüğünüz kimliği doğrulanmamış erişim yerine kiracı oturum açma sayfanızı görmeniz gerekir.

    ![Citrix NetScaler yapılandırması-bir Web tarayıcısında oturum açma sayfası](./media/citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-sso-for-kerberos-based-authentication"></a>Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler SSO 'yu yapılandırma

### <a name="create-a-kerberos-delegation-account-for-citrix-adc"></a>Citrix ADC için Kerberos temsili hesabı oluşturma

1. Bir kullanıcı hesabı oluşturun (Bu örnekte, _Apptemsilciliğini_kullanıyoruz).

    ![Citrix NetScaler yapılandırması-Özellikler bölmesi](./media/citrix-netscaler-tutorial/kerberos01.png)

1. Bu hesap için bir konak SPN ayarlayın. 

    Örnek: `setspn -S HOST/AppDelegation.IDENTT.WORK identt\appdelegation`
    
    Bu örnekte:

    * `IDENTT.WORK`, etki alanı FQDN 'sidir.
    * `identt`, etki alanı NetBIOS adıdır.
    * `appdelegation`, temsili Kullanıcı hesabı adıdır.

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi Web sunucusu için temsilciyi yapılandırın:
 
    ![Citrix NetScaler yapılandırması-Özellikler bölmesi altında temsili](./media/citrix-netscaler-tutorial/kerberos02.png)

    > [!NOTE]
    > Ekran görüntüsü örneğinde, Windows tümleşik kimlik doğrulaması (WIA) sitesini çalıştıran iç Web sunucusu adı _CWEB2_' dir.

### <a name="citrix-netscaler-aaa-kcd-kerberos-delegation-accounts"></a>Citrix NetScaler AAA KCD (Kerberos temsili hesapları)

Citrix NetScaler AAA-CD hesabını yapılandırmak için:

1.  **Citrix Gateway** > **aaa KCD (Kerberos kısıtlı temsilcisi) hesaplarına**gidin.

1.  **Ekle**' yi seçin ve ardından aşağıdaki değerleri girin veya seçin:

    * **Ad**: KCD hesabı için bir ad girin.

    * **Bölge**: etki alanını ve uzantıyı büyük harfle girin.

    * **HIZMET SPN**'si `http/<host/fqdn>@<DOMAIN.COM>`:.
    
        > [!NOTE]
        > `@DOMAIN.COM`gereklidir ve büyük harf olmalıdır. Örnek: `http/cweb2@IDENTT.WORK`.

    * **Temsilci Kullanıcı**: temsilci Kullanıcı adını girin.

    * **Temsilci Kullanıcı Için parola** onay kutusunu seçin ve bir parola girin ve onaylayın.

1. **Tamam**’ı seçin.
 
    ![Citrix NetScaler yapılandırması-KCD hesap bölmesini yapılandırma](./media/citrix-netscaler-tutorial/kerberos03.png)

### <a name="citrix-traffic-policy-and-traffic-profile"></a>Citrix trafik ilkesi ve trafik profili

Citrix trafik ilkesini ve trafik profilini yapılandırmak için:

1.  **Güvenlik** > **aaa-uygulama trafik** > **ilkeleri** > **trafik ilkeleri, profiller ve form SSO profilestraffic ilkeleri '** ne gidin.

1.  **Trafik profillerini**seçin.

1.  **Add (Ekle)** seçeneğini belirleyin.

1.  Bir trafik profilini yapılandırmak için aşağıdaki değerleri girin veya seçin.

    * **Ad**: trafik profili için bir ad girin.

    * **Çoklu oturum açma**: **Açık '** ı seçin.

    * **KCD hesabı**: önceki bölümde oluşturduğunuz KCD hesabını seçin.

1. **Tamam**’ı seçin.

    ![Citrix NetScaler yapılandırması-trafik profili bölmesini yapılandırma](./media/citrix-netscaler-tutorial/kerberos04.png)
 
1.  **Trafik ilkesi**seçin.

1.  **Add (Ekle)** seçeneğini belirleyin.

1.  Bir trafik ilkesi yapılandırmak için aşağıdaki değerleri girin veya seçin:

    * **Ad**: trafik ilkesi için bir ad girin.

    * **Profil**: önceki bölümde oluşturduğunuz trafik profilini seçin.

    * **İfade**: **true**girin.

1. **Tamam**’ı seçin.

    ![Citrix NetScaler yapılandırması-trafik Ilkesi bölmesini yapılandırma](./media/citrix-netscaler-tutorial/kerberos05.png)

### <a name="bind-a-traffic-policy-to-a-virtual-server-in-citrix"></a>Citrix 'teki bir sanal sunucuya trafik ilkesi bağlama

GUI kullanarak bir trafik ilkesini bir sanal sunucuya bağlamak için:

1. **Trafik yönetimi** > **Yük Dengeleme** > **sanal sunucularına**gidin.

1. Sanal sunucular listesinde, yeniden yazma ilkesini bağlamak istediğiniz sanal sunucuyu seçin ve **Aç**' ı seçin.

1. **Yük Dengeleme sanal sunucusu** bölmesinde, **Gelişmiş ayarlar**altında **ilkeler**' i seçin. NetScaler örneğiniz için yapılandırılmış tüm ilkeler listede görüntülenir.
 
    ![Citrix NetScaler yapılandırması-sanal sunucu bölmesini yükle](./media/citrix-netscaler-tutorial/kerberos06.png)

    ![Citrix NetScaler yapılandırma-Ilkeler iletişim kutusu](./media/citrix-netscaler-tutorial/kerberos07.png)

1.  Bu sanal sunucuya bağlamak istediğiniz ilkenin adının yanındaki onay kutusunu işaretleyin.
 
    ![Citrix NetScaler yapılandırması-sanal sunucu trafik Ilkesi bağlama bölmesi yük dengelemesi](./media/citrix-netscaler-tutorial/kerberos09.png)

1. **Tür Seç** iletişim kutusunda:

    1. **Ilke Seç**için **trafik**' i seçin.

    1. **Tür Seç**için **istek**' ı seçin.

    ![Citrix NetScaler yapılandırması-tür bölmesini seçin](./media/citrix-netscaler-tutorial/kerberos08.png)

1. İlke bağlandığında **bitti**' yi seçin.
 
    ![Citrix NetScaler yapılandırma-Ilkeler bölmesi](./media/citrix-netscaler-tutorial/kerberos10.png)

1. Bu bağlamayı WIA Web sitesini kullanarak test edin.

    ![Citrix NetScaler yapılandırması-bir Web tarayıcısında test sayfası](./media/citrix-netscaler-tutorial/kerberos11.png)    

### <a name="create-a-citrix-netscaler-test-user"></a>Citrix NetScaler test kullanıcısı oluşturma

Bu bölümde, Citrix NetScaler 'da B. Simon adlı bir Kullanıcı oluşturulur. Citrix NetScaler, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde gerçekleştirmeniz gereken bir işlem yok. Bir Kullanıcı Citrix NetScaler 'da zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html)başvurun.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, erişim panelini kullanarak Azure AD SSO yapılandırmanızı test edersiniz.

Erişim panelinde Citrix NetScaler kutucuğunu seçtiğinizde, SSO 'yu ayarladığınız Citrix NetScaler 'da otomatik olarak oturum açmış olmanız gerekir. Erişim paneli hakkında daha fazla bilgi için bkz. [erişim paneline giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Citrix NetScaler 'ı deneyin](https://aad.portal.azure.com/)

- [Üst bilgi tabanlı kimlik doğrulaması için Citrix NetScaler çoklu oturum açmayı yapılandırma](header-citrix-netscaler-tutorial.md)

- [Microsoft Cloud App Security oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Gelişmiş görünürlük ve denetimlerle Citrix NetScaler 'ı koruma](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)