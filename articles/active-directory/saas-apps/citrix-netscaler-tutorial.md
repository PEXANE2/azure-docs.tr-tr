---
title: 'Öğretici: Azure Active Directory Citrix NetScaler (Kerberos tabanlı kimlik doğrulama) ile tek oturum açma entegrasyonu | Microsoft Dokümanlar'
description: Kerberos tabanlı kimlik doğrulamasını kullanarak Azure Active Directory ve Citrix NetScaler arasında tek oturum açma (SSO) nasıl yapılandırılabildiğini öğrenin.
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
ms.openlocfilehash: 7400c8288d8901460e462ce43b69815e178a718c
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384016"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-citrix-netscaler-kerberos-based-authentication"></a>Öğretici: Citrix NetScaler ile Azure Active Directory tek oturum açma tümlemi (Kerberos tabanlı kimlik doğrulama)

Bu eğitimde, Citrix NetScaler'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Citrix NetScaler'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Citrix NetScaler'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Citrix NetScaler'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Citrix NetScaler tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Öğretici şu senaryoları içerir:

* **SP tarafından başlatılan** SITRIX NetScaler için SSO

* Citrix NetScaler için **tam zamanında** kullanıcı sağlama

* [Citrix NetScaler için Kerberos tabanlı kimlik doğrulama](#publish-the-web-server)

* [Citrix NetScaler için üstbilgi tabanlı kimlik doğrulama](header-citrix-netscaler-tutorial.md#publish-the-web-server)

* Citrix NetScaler'ı yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)

## <a name="add-citrix-netscaler-from-the-gallery"></a>Galeriden Citrix NetScaler ekleyin

Citrix NetScaler'ı Azure AD ile entegre etmek için, citrix NetScaler'ı önce galeriden yönetilen SaaS uygulamaları listenize ekleyin:

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.

1. Sol menüde **Azure Etkin Dizin'i**seçin.

1. Kurumsal **Uygulamalar'a**gidin ve ardından **Tüm Uygulamaları**seçin.

1. Yeni bir uygulama eklemek için **Yeni uygulama'yı**seçin.

1. **Galeriden Ekle bölümüne,** arama kutusuna **Citrix NetScaler'ı** girin.

1. Sonuçlarda, **Citrix NetScaler'ı**seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-citrix-netscaler"></a>Citrix NetScaler için Azure AD oturum açma işlemlerini yapılandırın ve test edin

Azure AD SSO'nu Citrix NetScaler ile **B.Simon**adlı bir test kullanıcısı kullanarak yapılandırın ve test edin. SSO'nun çalışması için, Bir Azure AD kullanıcısı ile Citrix NetScaler'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Citrix NetScaler ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için [Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)

    1. Azure AD SSO'nu B.Simon ile sınamak için [bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)

    1. B.Simon'ın Azure AD SSO'yu kullanmasını sağlamak için [Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)

1. Uygulama tarafındaki SSO ayarlarını yapılandırmak için [Citrix NetScaler SSO'yu yapılandırın.](#configure-citrix-netscaler-sso)

    * Citrix NetScaler'da, kullanıcının Azure AD gösterimine bağlı olan B.Simon'ın bir muadili için [bir Citrix NetScaler test kullanıcısı oluşturun.](#create-a-citrix-netscaler-test-user)

1. [SSO'yu test](#test-sso) edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalını kullanarak Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları tamamlayın:

1. Azure [portalında,](https://portal.azure.com/) **Citrix NetScaler** uygulama tümleştirme bölmesinde, **Yönet**altında, **Tek oturum açma'yı**seçin.

1. Tek **oturum açma yöntemi** bölmesini seç'te **SAML'yi**seçin.

1. **SAML bölmesiyle Tek Oturum** Açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için kalem **Düzenleme** simgesini seçin.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölümünde, **Uygulamayı IDP tarafından başlatılan** modda yapılandırmak için:

    1. **Tanımlayıcı** metin kutusuna, aşağıdaki deseni içeren bir URL girin:`https://<Your FQDN>`

    1. **Yanıtla URL** metin kutusuna, aşağıdaki deseni içeren bir URL girin:`http(s)://<Your FQDN>.of.vserver/cgi/samlauth`

1. Uygulamayı **SP tarafından başlatılan** modda yapılandırmak için **ek URL'ler** ayarla'yı seçin ve aşağıdaki adımı tamamlayın:

    * Oturum **Açma URL** metin kutusuna, aşağıdaki deseni içeren bir URL girin:`https://<Your FQDN>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > * Bu bölümde kullanılan URL'ler gerçek değerler değildir. Bu değerleri Tanımlayıcı, YanıtURL'si ve Oturum Açma URL'si için gerçek değerlerle güncelleştirin. Bu değerleri almak için [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.
    > * SSO'yu kurmak için URL'lere genel web sitelerinden erişilebilmelidir. Citrix NetScaler tarafındaki güvenlik duvarını veya diğer güvenlik ayarlarını, yapılandırılan URL'de belirteç göndermek için Azure AD'yi kaydetmek için etkinleştirmelisiniz.

1. **SAML bölmesiyle Tek Oturum** Açma'da, **SAML İmza Sertifikası** bölümünde, App Federation **Metadata Url**için URL'yi kopyalayın ve Not Defteri'ne kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. **Citrix NetScaler'ı Ayarla** bölümünde, gereksinimlerinize göre ilgili URL'leri kopyalayın.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümde, Azure portalında B.Simon adında bir test kullanıcısı oluşturursunuz.

1. Azure portalının sol menüsünde **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

1. Bölmenin üst kısmında **Yeni kullanıcı** yı seçin.

1. **Kullanıcı** özelliklerinde şu adımları tamamlayın:

   1. **Ad**için `B.Simon`, girin .  

   1. **Kullanıcı adı**için _username@companydomain.extension_, girin . Örneğin, `B.Simon@contoso.com`.

   1. **Parolayı Göster** onay kutusunu seçin ve ardından **Parola'da**görüntülenen değeri yazın veya kopyalayın.

   1. **Oluştur'u**seçin.

### <a name="assign-the-azure-ad-test-user"></a>Azure AD test kullanıcısını atama

Bu bölümde, kullanıcıya Citrix NetScaler'a erişim izni vererek B.Simon kullanıcısının Azure SSO'yu kullanmasını sağlarsınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.

1. Uygulamalar listesinde **Citrix NetScaler'ı**seçin.

1. Uygulama genel bakışında, **Yönet**altında, **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin. Ardından, **Atama Ekle** iletişim **kutusunda, Kullanıcılar ve gruplar**seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, **Kullanıcılar** listesinden **B.Simon'ı** seçin. **Seç**’i seçin.

1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için ilgili rolü seçin ve sonra **Seç'i**seçin.

1. Atama **Ekle** iletişim kutusunda **Atama'yı**seçin.

## <a name="configure-citrix-netscaler-sso"></a>Citrix NetScaler SSO'nun yapılandırılsın

Yapılandırmak istediğiniz kimlik doğrulama türü için adımlar için bir bağlantı seçin:

- [Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler SSO'ya yapılandırın](#publish-the-web-server)

- [Üstbilgi tabanlı kimlik doğrulama için Citrix NetScaler SSO'ya yapılandırın](header-citrix-netscaler-tutorial.md#publish-the-web-server)

### <a name="publish-the-web-server"></a>Web sunucusunu yayımlama 

Sanal sunucu oluşturmak için:

1. **Trafik Yönetimi** > **Yük Dengeleme** > **Hizmetleri'ni**seçin.
    
1. **Ekle'yi**seçin.

    ![Citrix NetScaler yapılandırması - Hizmetler bölmesi](./media/citrix-netscaler-tutorial/web01.png)

1. Uygulamaları çalıştıran web sunucusu için aşağıdaki değerleri ayarlayın:

   * **Hizmet Adı**
   * **Sunucu IP/ Varolan Sunucu**
   * **Protokolü**
   * **Bağlantı noktası**

### <a name="configure-the-load-balancer"></a>Yük dengeleyicisini yapılandırma

Yük dengeleyicisini yapılandırmak için:

1. Trafik **Yönetimi** > **Yük Dengeleme** > **Sanal Sunucular**gidin.

1. **Ekle'yi**seçin.

1. Aşağıdaki ekran görüntüsünde açıklandığı gibi aşağıdaki değerleri ayarlayın:

    * **Adı**
    * **Protokolü**
    * **IP Adresi**
    * **Bağlantı noktası**

1. **Tamam'ı**seçin.

    ![Citrix NetScaler yapılandırması - Temel Ayarlar bölmesi](./media/citrix-netscaler-tutorial/load01.png)

### <a name="bind-the-virtual-server"></a>Sanal sunucuyu bağlama

Yük bakiyesini sanal sunucuya bağlamak için:

1. Hizmet **ve Hizmet Grupları** bölmesinde, **Yük Dengeleme Yok Sanal Sunucu Hizmeti Bağlama'yı**seçin.

   ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu Hizmeti Bağlama bölmesi](./media/citrix-netscaler-tutorial/bind01.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve sonra **Kapat'ı**seçin.

   ![Citrix NetScaler yapılandırması - Sanal sunucu hizmetlerinin bağlanmasını doğrulayın](./media/citrix-netscaler-tutorial/bind02.png)

### <a name="bind-the-certificate"></a>Sertifikayı bağlama

Bu hizmeti SSL olarak yayımlamak için sunucu sertifikasını bağlayın ve ardından uygulamanızı sınlayın:

1. **Sertifika**altında, **Sunucu Sertifikası yok'u**seçin.

   ![Citrix NetScaler yapılandırması - Sunucu Sertifikası bölmesi](./media/citrix-netscaler-tutorial/bind03.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve sonra **Kapat'ı**seçin.

   ![Citrix NetScaler yapılandırması - Sertifikayı doğrulayın](./media/citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Citrix ADC SAML profili

Citrix ADC SAML profilini yapılandırmak için aşağıdaki bölümleri tamamlayın.

### <a name="create-an-authentication-policy"></a>Kimlik doğrulama ilkesi oluşturma

Kimlik doğrulama ilkesi oluşturmak için:

1. **Güvenlik** > **AAA'ya** > git – Uygulama Trafik**İlkeleri** > **Kimlik Doğrulama** > **Kimlik Doğrulama İlkeleri.**

1. **Ekle'yi**seçin.

1. Kimlik **Doğrulama İlkesi Oluştur** bölmesine aşağıdaki değerleri girin veya seçin:

    * **Adı**: Kimlik doğrulama ilkeniz için bir ad girin.
    * **Eylem**: **SAML**girin ve sonra **Ekle'yi**seçin.
    * **İfade**: **Doğru**girin .     
    
    ![Citrix NetScaler yapılandırması - Kimlik Doğrulama İlkesi bölmesi oluşturma](./media/citrix-netscaler-tutorial/policy01.png)

1. **Oluştur'u**seçin.

### <a name="create-an-authentication-saml-server"></a>Kimlik doğrulama SAML sunucusu oluşturma

Kimlik doğrulama SAML sunucusu oluşturmak için Kimlik **Doğrulama SAML Server** bölmesine gidin ve ardından aşağıdaki adımları tamamlayın:

1. **Ad**için, kimlik doğrulama SAML sunucusu için bir ad girin.

1. **İhracat SAML Meta verileri**altında:

   1. Meta **veri** onay kutusunu al'ı seçin.

   1. Daha önce kopyaladığınız Azure SAML Kullanıcı Birliğini federasyon meta veri URL'sinden girin.
    
1. **İhraççı Adı**için ilgili URL'yi girin.

1. **Oluştur'u**seçin.

![Citrix NetScaler yapılandırması - Kimlik Doğrulama SAML Server bölmesi oluşturma](./media/citrix-netscaler-tutorial/server01.png)

### <a name="create-an-authentication-virtual-server"></a>Kimlik doğrulama sanal sunucusu oluşturma

Kimlik doğrulama sanal sunucusu oluşturmak için:

1.  **Security** > **AAA '** > ya gidin - Uygulama Trafik**İlkeleri** > **Kimlik Doğrulama** > **Sanal Sunucular**.

1.  **Ekle'yi**seçin ve ardından aşağıdaki adımları tamamlayın:

    1. **Ad**için, kimlik doğrulama sanal sunucu için bir ad girin.

    1. **Adreslemeyen** onay kutusunu seçin.

    1. **Protokol**için **SSL'yi**seçin.

    1. **Tamam'ı**seçin.
    
1. **Devam**'ı seçin.

### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Azure AD'yi kullanacak şekilde kimlik doğrulama sanal sunucusunu yapılandırın

Kimlik doğrulama sanal sunucusu için iki bölümü değiştirin:

1.  Gelişmiş **Kimlik Doğrulama İlkeleri** bölmesine, **Kimlik Doğrulama İlkesi Yok'u**seçin.

    ![Citrix NetScaler yapılandırması - Gelişmiş Kimlik Doğrulama İlkeleri bölmesi](./media/citrix-netscaler-tutorial/virtual01.png)

1. **İlke Bağlama** bölmesine, kimlik doğrulama ilkesini seçin ve sonra **Bind'i**seçin.

    ![Citrix NetScaler yapılandırması - İlke Bağlama bölmesi](./media/citrix-netscaler-tutorial/virtual02.png)

1. Form **Tabanlı Sanal Sunucular** bölmesine, **Yük Dengeleme Yok Sanal Sunucu'yu**seçin.

    ![Citrix NetScaler yapılandırma - Form Tabanlı Sanal Sunucular bölme](./media/citrix-netscaler-tutorial/virtual03.png)

1. **Kimlik Doğrulama FQDN**için, tam nitelikli bir etki alanı adı (FQDN) (gerekli) girin.

1. Azure AD kimlik doğrulaması ile korumak istediğiniz yük dengeleme sanal sunucusunu seçin.

1. **Bind'i**seçin.

    ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu Bağlama bölmesi](./media/citrix-netscaler-tutorial/virtual04.png)

    > [!NOTE]
    > **Kimlik Doğrulama Sanal Sunucu Yapılandırma** bölmesindeki **Bitti'yi** seçtiğinizden emin olun.

1. Değişikliklerinizi doğrulamak için bir tarayıcıda uygulama URL'sine gidin. Daha önce görebileceğiniz kimlik doğrulaması olmayan erişim yerine kiracı oturum açma sayfanızı görmeniz gerekir.

    ![Citrix NetScaler yapılandırması - Bir web tarayıcısında oturum açma sayfası](./media/citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-sso-for-kerberos-based-authentication"></a>Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler SSO'ya yapılandırın

### <a name="create-a-kerberos-delegation-account-for-citrix-adc"></a>Citrix ADC için bir Kerberos delegasyonu hesabı oluşturun

1. Bir kullanıcı hesabı oluşturun (bu örnekte _AppDelegation_kullanıyoruz).

    ![Citrix NetScaler yapılandırması - Özellikler bölmesi](./media/citrix-netscaler-tutorial/kerberos01.png)

1. Bu hesap için bir HOST SPN ayarlayın. 

    Örnek: `setspn -S HOST/AppDelegation.IDENTT.WORK identt\appdelegation`
    
    Bu örnekte:

    * `IDENTT.WORK`etki alanı FQDN olduğunu.
    * `identt`etki alanı NetBIOS adıdır.
    * `appdelegation`temsilci kullanıcı hesabı adıdır.

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi web sunucusu için delegasyon yapılandırın:
 
    ![Citrix NetScaler yapılandırması - Özellikler bölmesi altında heyet](./media/citrix-netscaler-tutorial/kerberos02.png)

    > [!NOTE]
    > Ekran görüntüsü örneğinde, Windows Tümleşik Kimlik Doğrulama (WIA) sitesini çalıştıran dahili web sunucusu adı _CWEB2'dir._

### <a name="citrix-netscaler-aaa-kcd-kerberos-delegation-accounts"></a>Citrix NetScaler AAA KCD (Kerberos delegasyon hesapları)

Citrix NetScaler AAA KCD hesabını yapılandırmak için:

1.  **Citrix Gateway** > **AAA KCD (Kerberos Kısıtlı Temsilcilik) Hesapları**gidin.

1.  **Ekle'yi**seçin ve ardından aşağıdaki değerleri girin veya seçin:

    * **Ad**: KCD hesabıiçin bir ad girin.

    * **Alem**: Büyük harfe etki alanını ve uzantısıgirin.

    * **Servis SPN**: `http/<host/fqdn>@<DOMAIN.COM>`.
    
        > [!NOTE]
        > `@DOMAIN.COM`gereklidir ve büyük harfle olmalıdır. Örnek: `http/cweb2@IDENTT.WORK`.

    * **Temsilci Kullanıcı**: Devredilen kullanıcı adını girin.

    * Temsilci **Kullanıcı** onay kutusunu seçin ve bir parola girin ve onaylayın.

1. **Tamam'ı**seçin.
 
    ![Citrix NetScaler yapılandırma - KCD Hesap bölmesini yapılandır](./media/citrix-netscaler-tutorial/kerberos03.png)

### <a name="citrix-traffic-policy-and-traffic-profile"></a>Citrix trafik politikası ve trafik profili

Citrix trafik politikasını ve trafik profilini yapılandırmak için:

1.  **Security** > **AAA'ya** > git - Uygulama Trafik**Politikaları** > **Trafik Politikaları, Profiller ve Form SSO ProfilleriTrafik İlkeleri**.

1.  **Trafik Profillerini**seçin.

1.  **Ekle'yi**seçin.

1.  Trafik profilini yapılandırmak için aşağıdaki değerleri girin veya seçin.

    * **Adı**: Trafik profili için bir ad girin.

    * **Tek Oturum Açma**: **On'u**seçin.

    * **KCD Hesabı**: Önceki bölümde oluşturduğunuz KCD hesabını seçin.

1. **Tamam'ı**seçin.

    ![Citrix NetScaler yapılandırması - Trafik Profili bölmelerini yapılandır](./media/citrix-netscaler-tutorial/kerberos04.png)
 
1.  **Trafik Politikası'nı**seçin.

1.  **Ekle'yi**seçin.

1.  Trafik ilkesini yapılandırmak için aşağıdaki değerleri girin veya seçin:

    * **Adı**: Trafik ilkesi için bir ad girin.

    * **Profil**: Önceki bölümde oluşturduğunuz trafik profilini seçin.

    * **İfade**: **Doğru**girin .

1. **Tamam'ı**seçin.

    ![Citrix NetScaler yapılandırma - Trafik Politikası bölmesi yapılandırır](./media/citrix-netscaler-tutorial/kerberos05.png)

### <a name="bind-a-traffic-policy-to-a-virtual-server-in-citrix"></a>Citrix'teki sanal sunucuya trafik ilkesini bağlama

GUI'yi kullanarak bir trafik ilkesini sanal sunucuya bağlamak için:

1. Trafik **Yönetimi** > **Yük Dengeleme** > **Sanal Sunucular**gidin.

1. Sanal sunucular listesinde, yeniden yazma ilkesini bağlamak istediğiniz sanal sunucuyu seçin ve sonra **Aç'ı**seçin.

1. Yük **Dengeleme Sanal Sunucu** bölmesinde, **Gelişmiş Ayarlar**altında, **İlkeler'i**seçin. NetScaler örneğiniz için yapılandırılan tüm ilkeler listede görünür.
 
    ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu bölmesi](./media/citrix-netscaler-tutorial/kerberos06.png)

    ![Citrix NetScaler yapılandırması - İlkeler iletişim kutusu](./media/citrix-netscaler-tutorial/kerberos07.png)

1.  Bu sanal sunucuya bağlamak istediğiniz ilkenin adının yanındaki onay kutusunu seçin.
 
    ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu Trafik Politikası Bağlama bölmesi](./media/citrix-netscaler-tutorial/kerberos09.png)

1. Türü **Seç** iletişim kutusunda:

    1. **İlke Seç**için **Trafik'i**seçin.

    1. **Türü Seç**, **İstek'i**seçin.

    ![Citrix NetScaler yapılandırması - Tür bölmesini seçin](./media/citrix-netscaler-tutorial/kerberos08.png)

1. İlke bağlandığında, **Bitti'yi**seçin.
 
    ![Citrix NetScaler yapılandırması - İlkeler bölmesi](./media/citrix-netscaler-tutorial/kerberos10.png)

1. WIA web sitesini kullanarak bağlamayı test edin.

    ![Citrix NetScaler yapılandırması - Bir web tarayıcısında bir test sayfası](./media/citrix-netscaler-tutorial/kerberos11.png)    

### <a name="create-a-citrix-netscaler-test-user"></a>Citrix NetScaler test kullanıcısı oluşturma

Bu bölümde, B.Simon adlı bir kullanıcı Citrix NetScaler oluşturulur. Citrix NetScaler, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde işlem yapmak için herhangi bir işlem yoktur. Citrix NetScaler'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD SSO yapılandırmanızı sınayabilirsiniz.

Erişim Paneli'ndeki Citrix NetScaler döşemesini seçtiğinizde, SSO'yu kurduğunuz Citrix NetScaler'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure Active Directory ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Citrix NetScaler'ı deneyin](https://aad.portal.azure.com/)

- [Üstbilgi tabanlı kimlik doğrulama için Citrix NetScaler tek oturumunu yapılandırın](header-citrix-netscaler-tutorial.md)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)

- [Citrix NetScaler'ı gelişmiş görünürlük ve kontrollerle nasıl koruyabilen](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)