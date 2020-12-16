---
title: 'Öğretici: Citrix ADC (Kerberos tabanlı kimlik doğrulaması) ile çoklu oturum açma tümleştirmesi Azure Active Directory | Microsoft Docs'
description: Kerberos tabanlı kimlik doğrulaması kullanarak Azure Active Directory ve Citrix ADC arasında çoklu oturum açma (SSO) yapılandırma hakkında bilgi edinin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 12/15/2020
ms.author: jeedes
ms.openlocfilehash: e5f1f16ba9a2f276d41373eaf4c819522f2d9e2b
ms.sourcegitcommit: e15c0bc8c63ab3b696e9e32999ef0abc694c7c41
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97607584"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-citrix-adc-kerberos-based-authentication"></a>Öğretici: Citrix ADC ile çoklu oturum açma tümleştirmesi Azure Active Directory (Kerberos tabanlı kimlik doğrulaması)

Bu öğreticide Citrix ADC 'yi Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. Citrix ADC 'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de Citrix ADC erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Citrix ADC 'de otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğeler gereklidir:

* Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Citrix ADC çoklu oturum açma (SSO) etkin aboneliği.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, Azure AD SSO 'yu bir test ortamında yapılandırıp test edersiniz. Öğreticide şu senaryolar yer almaktadır:

* **SP tarafından başlatılan** Citrix ADC için SSO

* Citrix ADC için **tam zamanında** Kullanıcı sağlama

* [Citrix ADC için Kerberos tabanlı kimlik doğrulaması](#publish-the-web-server)

* [Citrix ADC için üst bilgi tabanlı kimlik doğrulaması](header-citrix-netscaler-tutorial.md#publish-the-web-server)


## <a name="add-citrix-adc-from-the-gallery"></a>Galeriden Citrix ADC ekleme

Citrix ADC 'yi Azure AD ile tümleştirmek için, önce galerideki yönetilen SaaS uygulamaları listenize Citrix ADC 'yi ekleyin:

1. Azure portal iş veya okul hesabı ya da kişisel Microsoft hesabı kullanarak oturum açın.

1. Sol menüden **Azure Active Directory**' yi seçin.

1. **Kurumsal uygulamalar**' a gidin ve **tüm uygulamalar**' ı seçin.

1. Yeni bir uygulama eklemek için **Yeni uygulama**' yı seçin.

1. **Galeriden Ekle** bölümünde, arama kutusuna **Citrix ADC** girin.

1. Sonuçlarda **CITRIX ADC**' yi seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-sso-for-citrix-adc"></a>Citrix ADC için Azure AD SSO 'yu yapılandırma ve test etme

**B. Simon** adlı bir test kullanıcısı kullanarak, Citrix ADC Ile Azure AD SSO 'yu yapılandırın ve test edin. SSO 'nun çalışması için, Citrix ADC içindeki bir Azure AD kullanıcısı ve ilgili Kullanıcı arasında bir bağlantı ilişkisi oluşturmanız gerekir.

Azure AD SSO 'yu Citrix ADC ile yapılandırmak ve test etmek için aşağıdaki adımları gerçekleştirin:

1. [Azure AD SSO 'Yu yapılandırın](#configure-azure-ad-sso) -kullanıcılarınızın bu özelliği kullanmasını sağlamak için.

    1. B. Simon ile Azure AD SSO 'yu test etmek için [bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user) .

    1. Azure AD [Test kullanıcısına atama](#assign-the-azure-ad-test-user) -Azure AD SSO 'yu kullanmak için B. Simon 'u etkinleştirmek için.

1. Uygulama tarafında SSO ayarlarını yapılandırmak için [CITRIX ADC SSO 'Yu yapılandırın](#configure-citrix-adc-sso) .

    * Kullanıcının Azure AD gösterimine bağlı olan Citrix ADC 'de B. Simon 'ya karşılık gelen bir [CITRIX ADC test kullanıcısı oluşturun](#create-a-citrix-adc-test-user) .

1. [Test SSO](#test-sso) -yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure AD SSO 'yu Azure portal kullanarak etkinleştirmek için şu adımları uygulayın:

1. Azure portal, **CITRIX ADC** uygulama tümleştirme bölmesindeki **Yönet** altında, **Çoklu oturum açma**' yı seçin.

1. **Çoklu oturum açma yöntemi seç** bölmesinde **SAML**' yi seçin.

1. **SAML Ile tek Sign-On ayarla** bölmesinde, ayarları düzenlemek IÇIN **temel SAML yapılandırması** kalem **düzenleme** simgesini seçin.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

1. **Temel SAML yapılandırması** bölümünde, uygulamayı **IDP tarafından başlatılan** modda yapılandırmak için:

    1. **Tanımlayıcı** metin kutusuna aşağıdaki düzene sahıp bir URL girin:`https://<Your FQDN>`

    1. **Yanıt URL 'si** metin kutusuna aşağıdaki düzene sahıp bir URL girin:`http(s)://<Your FQDN>.of.vserver/cgi/samlauth`

1. Uygulamayı **SP tarafından başlatılan** modda yapılandırmak Için **ek URL 'ler ayarla** ' yı seçin ve aşağıdaki adımı doldurun:

    * **Oturum açma URL 'si** metin kutusunda, aşağıdaki düzene sahıp bir URL girin:`https://<Your FQDN>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > * Bu bölümde kullanılan URL 'Ler gerçek değer değildir. Bu değerleri tanımlayıcı, yanıt URL 'SI ve oturum açma URL 'SI için gerçek değerlerle güncelleştirin. Bu değerleri almak için [CITRIX ADC istemci destek ekibine](https://www.citrix.com/contact/technical-support.html) başvurun. Ayrıca, Azure portal **temel SAML yapılandırması** bölümünde gösterilen desenlere de başvurabilirsiniz.
    > * SSO 'yu ayarlamak için, URL 'Lerin genel Web sitelerinden erişilebilir olması gerekir. Bir güvenlik duvarını veya Citrix ADC tarafında bulunan güvenlik ayarlarını, belirteci yapılandırılmış URL 'de göndermek için Azure AD 'ye uygun hale getirmek üzere etkinleştirmeniz gerekir.

1. **SAML Ile tek Sign-On ayarla** bölmesinde, **SAML imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'si** Için, URL 'yi kopyalayın ve Not defteri 'ne kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **CITRIX ADC 'Yi ayarla** bölümünde, gereksinimlerinize göre Ilgili URL 'leri kopyalayın.

    ![Yapılandırma URL 'Lerini Kopyala](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, B. Simon adlı Azure portal bir test kullanıcısı oluşturacaksınız.

1. Azure portal sol menüsünde **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin ve ardından **tüm kullanıcılar**' ı seçin.

1. Bölmenin en üstünde bulunan **Yeni Kullanıcı** ' yı seçin.

1. **Kullanıcı** özellikleri ' nde şu adımları uygulayın:

   1. **Ad** için girin `B.Simon` .  

   1. **Kullanıcı adı** için girin _username@companydomain.extension_ . Örneğin, `B.Simon@contoso.com`.

   1. **Parolayı göster** onay kutusunu seçin ve ardından **parola**' ya, sonra da görüntülenen değeri yazın veya kopyalayın.

   1. **Oluştur**’u seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, Kullanıcı, Citrix ADC 'ye erişim izni vererek, B. Simon kullanıcısını Azure SSO 'yu kullanacak şekilde etkinleştirecektir.

1. Azure portal **Kurumsal uygulamalar**' ı seçin ve ardından **tüm uygulamalar**' ı seçin.

1. Uygulamalar listesinde **CITRIX ADC**' yi seçin.

1. Uygulamaya genel bakış bölümünde, **Yönet** altında **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcı ekle**'yi seçin. Sonra **atama Ekle** Iletişim kutusunda **Kullanıcılar ve gruplar**' ı seçin.
1. **Kullanıcılar ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinden **B. Simon** öğesini seçin. **Seç**’i seçin.
1. Kullanıcılara bir rolün atanmasını bekliyorsanız, **Rol Seç** açılır listesinden bunu seçebilirsiniz. Bu uygulama için ayarlanmış bir rol yoksa, "varsayılan erişim" rolü seçili olduğunu görürsünüz.
1. **Atama Ekle** Iletişim kutusunda **ata**' yı seçin.

## <a name="configure-citrix-adc-sso"></a>Citrix ADC SSO 'yu yapılandırma

Yapılandırmak istediğiniz kimlik doğrulaması türü için adımlar için bir bağlantı seçin:

- [Kerberos tabanlı kimlik doğrulaması için Citrix ADC SSO 'yu yapılandırma](#publish-the-web-server)

- [Üst bilgi tabanlı kimlik doğrulaması için Citrix ADC SSO 'yu yapılandırma](header-citrix-netscaler-tutorial.md#publish-the-web-server)

### <a name="publish-the-web-server"></a>Web sunucusunu yayımlama 

Bir sanal sunucu oluşturmak için:

1. **Trafik yönetimi**  >  **Yük Dengeleme**  >  **Hizmetleri**' ni seçin.
    
1. **Add (Ekle)** seçeneğini belirleyin.

    ![Citrix ADC yapılandırma-hizmetler bölmesi](./media/citrix-netscaler-tutorial/web01.png)

1. Uygulamaları çalıştıran Web sunucusu için aşağıdaki değerleri ayarlayın:

   * **Hizmet Adı**
   * **Sunucu IP/varolan sunucu**
   * **Protokol**
   * **Bağlantı noktası**

### <a name="configure-the-load-balancer"></a>Yük dengeleyiciyi yapılandırma

Yük dengeleyiciyi yapılandırmak için:

1. **Trafik yönetimi**  >  **Yük Dengeleme**  >  **sanal sunucularına** gidin.

1. **Add (Ekle)** seçeneğini belirleyin.

1. Aşağıdaki değerleri aşağıdaki ekran görüntüsünde açıklandığı gibi ayarlayın:

    * **Ad**
    * **Protokol**
    * **IP Adresi**
    * **Bağlantı noktası**

1. **Tamam**’ı seçin.

    ![Citrix ADC yapılandırması-temel ayarlar bölmesi](./media/citrix-netscaler-tutorial/load01.png)

### <a name="bind-the-virtual-server"></a>Sanal sunucuyu bağlama

Yük dengeleyiciyi sanal sunucuyla bağlamak için:

1. **Hizmetler ve hizmet grupları** bölmesinde, **Yük Dengeleme sanal sunucu hizmeti bağlamayı yok**' u seçin.

   ![Citrix ADC yapılandırması-sanal sunucu hizmeti bağlama bölmesi yük dengelemesi](./media/citrix-netscaler-tutorial/bind01.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve ardından **Kapat**' ı seçin.

   ![Citrix ADC yapılandırması-Virtual Server Hizmetleri bağlamayı doğrulama](./media/citrix-netscaler-tutorial/bind02.png)

### <a name="bind-the-certificate"></a>Sertifikayı bağlama

Bu hizmeti TLS olarak yayımlamak için sunucu sertifikasını bağlayın ve ardından uygulamanızı test edin:

1. **Sertifika** altında **sunucu sertifikası yok**' u seçin.

   ![Citrix ADC yapılandırması-sunucu sertifikası bölmesi](./media/citrix-netscaler-tutorial/bind03.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve ardından **Kapat**' ı seçin.

   ![Citrix ADC yapılandırması-sertifikayı doğrulama](./media/citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Citrix ADC SAML profili

Citrix ADC SAML profilini yapılandırmak için aşağıdaki bölümleri doldurun.

### <a name="create-an-authentication-policy"></a>Kimlik doğrulama ilkesi oluşturma

Kimlik doğrulama ilkesi oluşturmak için:

1. **Güvenlik**  >  **aaa – uygulama trafik**  >  **ilkeleri**  >  **kimlik** doğrulama  >  **ilkeleri**' ne gidin.

1. **Add (Ekle)** seçeneğini belirleyin.

1. **Kimlik doğrulama Ilkesi oluştur** bölmesinde, aşağıdaki değerleri girin veya seçin:

    * **Ad**: kimlik doğrulama ilkeniz için bir ad girin.
    * **Eylem**: **SAML** yazın ve ardından **Ekle**' yi seçin.
    * **İfade**: **true** girin.     
    
    ![Citrix ADC yapılandırması-kimlik doğrulama Ilkesi oluştur bölmesi](./media/citrix-netscaler-tutorial/policy01.png)

1. **Oluştur**’u seçin.

### <a name="create-an-authentication-saml-server"></a>Kimlik doğrulama SAML sunucusu oluşturma

Bir kimlik doğrulama SAML sunucusu oluşturmak için, **kimlik doğrulaması SAML sunucusu oluştur** bölmesine gidin ve aşağıdaki adımları tamamlayın:

1. **Ad** için, KIMLIK doğrulaması SAML sunucusu için bir ad girin.

1. **SAML meta verilerini dışarı aktar** altında:

   1. **Meta verileri Içeri aktar** onay kutusunu seçin.

   1. Daha önce kopyaladığınız Azure SAML kullanıcı arabiriminden Federasyon meta verileri URL 'sini girin.
    
1. **Verenin adı** için ilgili URL 'yi girin.

1. **Oluştur**’u seçin.

![Citrix ADC yapılandırması-kimlik doğrulama SAML sunucu bölmesi oluştur](./media/citrix-netscaler-tutorial/server01.png)

### <a name="create-an-authentication-virtual-server"></a>Kimlik doğrulama sanal sunucusu oluşturma

Kimlik doğrulama sanal sunucusu oluşturmak için:

1.  **Güvenlik**  >  **aaa-uygulama trafik**  >  **ilkeleri**  >  **kimlik** doğrulama  >  **sanal sunucuları**' na gidin.

1.  **Ekle**' yi seçin ve ardından aşağıdaki adımları uygulayın:

    1. **Ad** için, kimlik doğrulama sanal sunucusu için bir ad girin.

    1. **Adreslenebilir olmayan** onay kutusunu seçin.

    1. **Protokol** için **SSL**' yi seçin.

    1. **Tamam**’ı seçin.
    
1. **Devam**’ı seçin.

### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Kimlik doğrulama sanal sunucusunu Azure AD kullanacak şekilde yapılandırma

Kimlik doğrulama sanal sunucusu için iki bölümü değiştirin:

1.  **Gelişmiş kimlik doğrulama ilkeleri** bölmesinde **kimlik doğrulama ilkesi yok**' u seçin.

    ![Citrix ADC yapılandırması-Gelişmiş kimlik doğrulama Ilkeleri bölmesi](./media/citrix-netscaler-tutorial/virtual01.png)

1. **Ilke bağlama** bölmesinde, kimlik doğrulama ilkesini seçin ve ardından **bağla**' yı seçin.

    ![Citrix ADC yapılandırma-Ilke bağlama bölmesi](./media/citrix-netscaler-tutorial/virtual02.png)

1. **Form tabanlı sanal sunucular** bölmesinde, **Yük Dengeleme sanal sunucusu yok**' u seçin.

    ![Citrix ADC yapılandırma-form tabanlı sanal sunucular bölmesi](./media/citrix-netscaler-tutorial/virtual03.png)

1. **Kimlik doğrulama FQDN 'si** için tam etki alanı adı (FQDN) (gerekli) girin.

1. Azure AD kimlik doğrulamasıyla korumak istediğiniz yük dengeleme sanal sunucusunu seçin.

1. **Bağla**' yı seçin.

    ![Citrix ADC yapılandırması-sanal sunucu bağlama bölmesinin yükünü dengeleme](./media/citrix-netscaler-tutorial/virtual04.png)

    > [!NOTE]
    > **Kimlik doğrulama sanal sunucu yapılandırma** bölmesinde **bitti** ' yi seçtiğinizden emin olun.

1. Değişikliklerinizi doğrulamak için, bir tarayıcıda uygulama URL 'sine gidin. Daha önce gördüğünüz kimliği doğrulanmamış erişim yerine kiracı oturum açma sayfanızı görmeniz gerekir.

    ![Citrix ADC yapılandırması-bir Web tarayıcısında oturum açma sayfası](./media/citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-adc-sso-for-kerberos-based-authentication"></a>Kerberos tabanlı kimlik doğrulaması için Citrix ADC SSO 'yu yapılandırma

### <a name="create-a-kerberos-delegation-account-for-citrix-adc"></a>Citrix ADC için Kerberos temsili hesabı oluşturma

1. Bir kullanıcı hesabı oluşturun (Bu örnekte, _Apptemsilciliğini_ kullanıyoruz).

    ![Citrix ADC yapılandırması-Özellikler bölmesi](./media/citrix-netscaler-tutorial/kerberos01.png)

1. Bu hesap için bir konak SPN ayarlayın. 

    Örnek: `setspn -S HOST/AppDelegation.IDENTT.WORK identt\appdelegation`
    
    Bu örnekte:

    * `IDENTT.WORK` , etki alanı FQDN 'sidir.
    * `identt` , etki alanı NetBIOS adıdır.
    * `appdelegation` , temsili Kullanıcı hesabı adıdır.

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi Web sunucusu için temsilciyi yapılandırın:
 
    ![Citrix ADC yapılandırması-Özellikler bölmesi altında temsili](./media/citrix-netscaler-tutorial/kerberos02.png)

    > [!NOTE]
    > Ekran görüntüsü örneğinde, Windows tümleşik kimlik doğrulaması (WIA) sitesini çalıştıran iç Web sunucusu adı _CWEB2_' dir.

### <a name="citrix-adc-aaa-kcd-kerberos-delegation-accounts"></a>Citrix ADC AAA KCD (Kerberos temsili hesapları)

Citrix ADC AAA KCD hesabını yapılandırmak için:

1.  **Citrix Gateway**  >  **aaa KCD (Kerberos kısıtlı temsilcisi) hesaplarına** gidin.

1.  **Ekle**' yi seçin ve ardından aşağıdaki değerleri girin veya seçin:

    * **Ad**: KCD hesabı için bir ad girin.

    * **Bölge**: etki alanını ve uzantıyı büyük harfle girin.

    * **HIZMET SPN**'si: `http/<host/fqdn>@<DOMAIN.COM>` .
    
        > [!NOTE]
        > `@DOMAIN.COM` gereklidir ve büyük harf olmalıdır. Örnek: `http/cweb2@IDENTT.WORK`.

    * **Temsilci Kullanıcı**: temsilci Kullanıcı adını girin.

    * **Temsilci Kullanıcı Için parola** onay kutusunu seçin ve bir parola girin ve onaylayın.

1. **Tamam**’ı seçin.
 
    ![Citrix ADC yapılandırması-KCD hesap bölmesini yapılandırma](./media/citrix-netscaler-tutorial/kerberos03.png)

### <a name="citrix-traffic-policy-and-traffic-profile"></a>Citrix trafik ilkesi ve trafik profili

Citrix trafik ilkesini ve trafik profilini yapılandırmak için:

1.  **Güvenlik**  >  **aaa-uygulama trafik**  >  **ilkeleri**  >  **trafik ilkeleri, profiller ve form SSO profilestraffic ilkeleri '** ne gidin.

1.  **Trafik profillerini** seçin.

1.  **Add (Ekle)** seçeneğini belirleyin.

1.  Bir trafik profilini yapılandırmak için aşağıdaki değerleri girin veya seçin.

    * **Ad**: trafik profili için bir ad girin.

    * **Çoklu oturum açma**: **Açık '** ı seçin.

    * **KCD hesabı**: önceki bölümde oluşturduğunuz KCD hesabını seçin.

1. **Tamam**’ı seçin.

    ![Citrix ADC yapılandırması-trafik profili bölmesini yapılandırma](./media/citrix-netscaler-tutorial/kerberos04.png)
 
1.  **Trafik ilkesi** seçin.

1.  **Add (Ekle)** seçeneğini belirleyin.

1.  Bir trafik ilkesi yapılandırmak için aşağıdaki değerleri girin veya seçin:

    * **Ad**: trafik ilkesi için bir ad girin.

    * **Profil**: önceki bölümde oluşturduğunuz trafik profilini seçin.

    * **İfade**: **true** girin.

1. **Tamam**’ı seçin.

    ![Citrix ADC yapılandırması-trafik Ilkesi bölmesini yapılandırma](./media/citrix-netscaler-tutorial/kerberos05.png)

### <a name="bind-a-traffic-policy-to-a-virtual-server-in-citrix"></a>Citrix 'teki bir sanal sunucuya trafik ilkesi bağlama

GUI kullanarak bir trafik ilkesini bir sanal sunucuya bağlamak için:

1. **Trafik yönetimi**  >  **Yük Dengeleme**  >  **sanal sunucularına** gidin.

1. Sanal sunucular listesinde, yeniden yazma ilkesini bağlamak istediğiniz sanal sunucuyu seçin ve **Aç**' ı seçin.

1. **Yük Dengeleme sanal sunucusu** bölmesinde, **Gelişmiş ayarlar** altında **ilkeler**' i seçin. NetScaler örneğiniz için yapılandırılmış tüm ilkeler listede görüntülenir.
 
    ![Citrix ADC yapılandırması-sanal sunucu bölmesini yükle](./media/citrix-netscaler-tutorial/kerberos06.png)

    ![Citrix ADC yapılandırma Ilkeleri iletişim kutusu](./media/citrix-netscaler-tutorial/kerberos07.png)

1.  Bu sanal sunucuya bağlamak istediğiniz ilkenin adının yanındaki onay kutusunu işaretleyin.
 
    ![Citrix ADC yapılandırması-sanal sunucu trafik Ilkesi bağlama bölmesini yükle](./media/citrix-netscaler-tutorial/kerberos09.png)

1. **Tür Seç** iletişim kutusunda:

    1. **Ilke Seç** için **trafik**' i seçin.

    1. **Tür Seç** için **istek**' ı seçin.

    ![Citrix ADC yapılandırması-tür bölmesini seçin](./media/citrix-netscaler-tutorial/kerberos08.png)

1. İlke bağlandığında **bitti**' yi seçin.
 
    ![Citrix ADC yapılandırma Ilkeleri bölmesi](./media/citrix-netscaler-tutorial/kerberos10.png)

1. Bu bağlamayı WIA Web sitesini kullanarak test edin.

    ![Citrix ADC yapılandırması-Web tarayıcısında bir test sayfası](./media/citrix-netscaler-tutorial/kerberos11.png)    

### <a name="create-a-citrix-adc-test-user"></a>Citrix ADC test kullanıcısı oluşturma

Bu bölümde, Citrix ADC 'de B. Simon adlı bir Kullanıcı oluşturulur. Citrix ADC, varsayılan olarak etkinleştirilen tam zamanında Kullanıcı sağlamayı destekler. Bu bölümde gerçekleştirmeniz gereken bir işlem yok. Bir Kullanıcı Citrix ADC 'de zaten mevcut değilse, kimlik doğrulamasından sonra yeni bir tane oluşturulur.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [CITRIX ADC istemci desteği ekibine](https://www.citrix.com/contact/technical-support.html)başvurun.

## <a name="test-sso"></a>Test SSO 'SU 

Bu bölümde, Azure AD çoklu oturum açma yapılandırmanızı aşağıdaki seçeneklerle test edersiniz. 

* Azure portal içinde **Bu uygulamayı test et** ' e tıklayın. Bu, oturum açma akışını başlatabileceğiniz Citrix ADC oturum açma URL 'sine yeniden yönlendirilir. 

* Doğrudan Citrix ADC oturum açma URL 'sine gidin ve oturum açma akışını buradan başlatın.

* Microsoft My Apps ' i kullanabilirsiniz. Uygulamalarım içindeki Citrix ADC kutucuğuna tıkladığınızda bu işlem Citrix ADC oturum açma URL 'sine yönlendirilir. Uygulamalarım hakkında daha fazla bilgi için bkz. [uygulamalarıma giriş](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction).


## <a name="next-steps"></a>Sonraki adımlar

Citrix ADC 'yi yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak ayıklanmasını ve zaman korumasını koruyan oturum denetimini zorunlu kılabilirsiniz. Oturum denetimi koşullu erişimden genişletiliyor. [Microsoft Cloud App Security ile oturum denetimini nasıl zorlayacağınızı öğrenin](/cloud-app-security/proxy-deployment-any-app).