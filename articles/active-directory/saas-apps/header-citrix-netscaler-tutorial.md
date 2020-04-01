---
title: 'Öğretici: Azure Active Directory, Citrix NetScaler ile tek oturum açma tümleştirmesi (üstbilgi tabanlı kimlik doğrulama) | Microsoft Dokümanlar'
description: Üstbilgi tabanlı kimlik doğrulaması kullanarak Azure Active Directory ve Citrix NetScaler arasında tek oturum açma (SSO) nasıl yapılandırılabildiğini öğrenin.
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
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80477948"
---
# <a name="tutorial-azure-active-directory-single-sign-on-integration-with-citrix-netscaler-header-based-authentication"></a>Öğretici: Azure Active Directory, Citrix NetScaler ile tek oturum açma tümlemesi (üstbilgi tabanlı kimlik doğrulama)

Bu eğitimde, Citrix NetScaler'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. Citrix NetScaler'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD'de Citrix NetScaler'a erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla Citrix NetScaler'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile hizmet olarak yazılım (SaaS) uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Citrix NetScaler tek oturum açma (SSO) aboneliği ni sağladı.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. Öğretici şu senaryoları içerir:

* **SP tarafından başlatılan** SITRIX NetScaler için SSO

* Citrix NetScaler için **tam zamanında** kullanıcı sağlama

* [Citrix NetScaler için üstbilgi tabanlı kimlik doğrulama](#publish-the-web-server)

* [Citrix NetScaler için Kerberos tabanlı kimlik doğrulama](citrix-netscaler-tutorial.md#publish-the-web-server)

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

    1. **Yanıtla URL** metin kutusuna, aşağıdaki deseni içeren bir URL girin:`https://<Your FQDN>/CitrixAuthService/AuthService.asmx`

1. Uygulamayı **SP tarafından başlatılan** modda yapılandırmak için **ek URL'ler** ayarla'yı seçin ve aşağıdaki adımı tamamlayın:

    * Oturum **Açma URL** metin kutusuna, aşağıdaki deseni içeren bir URL girin:`https://<Your FQDN>/CitrixAuthService/AuthService.asmx`

    > [!NOTE]
    > * Bu bölümde kullanılan URL'ler gerçek değerler değildir. Bu değerleri Tanımlayıcı, YanıtURL'si ve Oturum Açma URL'si için gerçek değerlerle güncelleştirin. Bu değerleri almak için [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.
    > * SSO'yu kurmak için URL'lere genel web sitelerinden erişilebilmelidir. Citrix NetScaler tarafındaki güvenlik duvarını veya diğer güvenlik ayarlarını, yapılandırılan URL'de belirteç göndermek için Azure AD'yi kaydetmek için etkinleştirmelisiniz.

1. **SAML bölmesiyle Tek Oturum** Açma'da, **SAML İmza Sertifikası** bölümünde, App Federation **Metadata Url**için URL'yi kopyalayın ve Not Defteri'ne kaydedin.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

1. Citrix NetScaler uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren, SAML iddialarının belirli bir biçimde olmasını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. **Edit** simgesini seçin ve öznitelik eşlemelerini değiştirin.

    ![SAML öznitelik eşlemi'ni edin](common/edit-attribute.png)

1. Citrix NetScaler uygulaması da saml yanıtı geri geçirilmesi için birkaç öznitelikleri bekliyor. Kullanıcı **Öznitelikleri** iletişim kutusunda, **Kullanıcı Talepleri**altında, tabloda gösterildiği gibi SAML belirteç öznitelikleri eklemek için aşağıdaki adımları tamamlayın:

    | Adı | Kaynak özniteliği|
    | ---------------| --------------- |
    | mySecretID  | user.userprincipalname |
    
    1. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için yeni talep **ekle'yi** seçin.

    1. **Ad** metin kutusuna, bu satır için gösterilen öznitelik adını girin.

    1. Ad **alanını** boş bırakın.

    1. **Öznitelik**için **Kaynak'ı**seçin.

    1. Kaynak **öznitelik** listesinde, bu satır için gösterilen öznitelik değerini girin.

    1. **Tamam'ı**seçin.

    1. **Kaydet'i**seçin.

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

- [Üstbilgi tabanlı kimlik doğrulama için Citrix NetScaler SSO'ya yapılandırın](#publish-the-web-server)

- [Kerberos tabanlı kimlik doğrulaması için Citrix NetScaler SSO'ya yapılandırın](citrix-netscaler-tutorial.md#publish-the-web-server)

### <a name="publish-the-web-server"></a>Web sunucusunu yayımlama 

Sanal sunucu oluşturmak için:

1. **Trafik Yönetimi** > **Yük Dengeleme** > **Hizmetleri'ni**seçin.
    
1. **Ekle'yi**seçin.

    ![Citrix NetScaler yapılandırması - Hizmetler bölmesi](./media/header-citrix-netscaler-tutorial/web01.png)

1. Uygulamaları çalıştıran web sunucusu için aşağıdaki değerleri ayarlayın:

   * **Hizmet Adı**
   * **Sunucu IP/ Varolan Sunucu**
   * **Protokolü**
   * **Bağlantı noktası**

     ![Citrix NetScaler yapılandırma bölmesi](./media/header-citrix-netscaler-tutorial/web01.png)

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

    ![Citrix NetScaler yapılandırması - Temel Ayarlar bölmesi](./media/header-citrix-netscaler-tutorial/load01.png)

### <a name="bind-the-virtual-server"></a>Sanal sunucuyu bağlama

Yük bakiyesini sanal sunucuya bağlamak için:

1. Hizmet **ve Hizmet Grupları** bölmesinde, **Yük Dengeleme Yok Sanal Sunucu Hizmeti Bağlama'yı**seçin.

   ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu Hizmeti Bağlama bölmesi](./media/header-citrix-netscaler-tutorial/bind01.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve sonra **Kapat'ı**seçin.

   ![Citrix NetScaler yapılandırması - Sanal sunucu hizmetlerinin bağlanmasını doğrulayın](./media/header-citrix-netscaler-tutorial/bind02.png)

### <a name="bind-the-certificate"></a>Sertifikayı bağlama

Bu hizmeti TLS olarak yayımlamak için sunucu sertifikasını bağlayın ve ardından uygulamanızı sınlayın:

1. **Sertifika**altında, **Sunucu Sertifikası yok'u**seçin.

   ![Citrix NetScaler yapılandırması - Sunucu Sertifikası bölmesi](./media/header-citrix-netscaler-tutorial/bind03.png)

1. Aşağıdaki ekran görüntüsünde gösterildiği gibi ayarları doğrulayın ve sonra **Kapat'ı**seçin.

   ![Citrix NetScaler yapılandırması - Sertifikayı doğrulayın](./media/header-citrix-netscaler-tutorial/bind04.png)

## <a name="citrix-adc-saml-profile"></a>Citrix ADC SAML profili

Citrix ADC SAML profilini yapılandırmak için aşağıdaki bölümleri tamamlayın:

### <a name="create-an-authentication-policy"></a>Kimlik doğrulama ilkesi oluşturma

Kimlik doğrulama ilkesi oluşturmak için:

1. **Güvenlik** > **AAA'ya** > git – Uygulama Trafik**İlkeleri** > **Kimlik Doğrulama** > **Kimlik Doğrulama İlkeleri.**

1. **Ekle'yi**seçin.

1. Kimlik **Doğrulama İlkesi Oluştur** bölmesine aşağıdaki değerleri girin veya seçin:

    * **Adı**: Kimlik doğrulama ilkeniz için bir ad girin.
    * **Eylem**: **SAML**girin ve sonra **Ekle'yi**seçin.
    * **İfade**: **Doğru**girin .     
    
    ![Citrix NetScaler yapılandırması - Kimlik Doğrulama İlkesi bölmesi oluşturma](./media/header-citrix-netscaler-tutorial/policy01.png)

1. **Oluştur'u**seçin.

### <a name="create-an-authentication-saml-server"></a>Kimlik doğrulama SAML sunucusu oluşturma

Kimlik doğrulama SAML sunucusu oluşturmak için Kimlik **Doğrulama SAML Server** bölmesine gidin ve ardından aşağıdaki adımları tamamlayın:

1. **Ad**için, kimlik doğrulama SAML sunucusu için bir ad girin.

1. **İhracat SAML Meta verileri**altında:

   1. Meta **veri** onay kutusunu al'ı seçin.

   1. Daha önce kopyaladığınız Azure SAML Kullanıcı Birliğini federasyon meta veri URL'sinden girin.
    
1. **İhraççı Adı**için ilgili URL'yi girin.

1. **Oluştur'u**seçin.

![Citrix NetScaler yapılandırması - Kimlik Doğrulama SAML Server bölmesi oluşturma](./media/header-citrix-netscaler-tutorial/server01.png)

### <a name="create-an-authentication-virtual-server"></a>Kimlik doğrulama sanal sunucusu oluşturma

Kimlik doğrulama sanal sunucusu oluşturmak için:

1.  **Security** > **AAA '** > ya gidin - Uygulama Trafik**İlkeleri** > **Kimlik Doğrulama** > **Sanal Sunucular**.

1.  **Ekle'yi**seçin ve ardından aşağıdaki adımları tamamlayın:

    1. **Ad**için, kimlik doğrulama sanal sunucu için bir ad girin.

    1. **Adreslemeyen** onay kutusunu seçin.

    1. **Protokol**için **SSL'yi**seçin.

    1. **Tamam'ı**seçin.

    ![Citrix NetScaler yapılandırma - Kimlik Doğrulama Sanal Sunucu bölmesi](./media/header-citrix-netscaler-tutorial/server02.png)
    
### <a name="configure-the-authentication-virtual-server-to-use-azure-ad"></a>Azure AD'yi kullanacak şekilde kimlik doğrulama sanal sunucusunu yapılandırın

Kimlik doğrulama sanal sunucusu için iki bölümü değiştirin:

1.  Gelişmiş **Kimlik Doğrulama İlkeleri** bölmesine, **Kimlik Doğrulama İlkesi Yok'u**seçin.

    ![Citrix NetScaler yapılandırması - Gelişmiş Kimlik Doğrulama İlkeleri bölmesi](./media/header-citrix-netscaler-tutorial/virtual01.png)

1. **İlke Bağlama** bölmesine, kimlik doğrulama ilkesini seçin ve sonra **Bind'i**seçin.

    ![Citrix NetScaler yapılandırması - İlke Bağlama bölmesi](./media/header-citrix-netscaler-tutorial/virtual02.png)

1. Form **Tabanlı Sanal Sunucular** bölmesine, **Yük Dengeleme Yok Sanal Sunucu'yu**seçin.

    ![Citrix NetScaler yapılandırma - Form Tabanlı Sanal Sunucular bölme](./media/header-citrix-netscaler-tutorial/virtual03.png)

1. **Kimlik Doğrulama FQDN**için, tam nitelikli bir etki alanı adı (FQDN) (gerekli) girin.

1. Azure AD kimlik doğrulaması ile korumak istediğiniz yük dengeleme sanal sunucusunu seçin.

1. **Bind'i**seçin.

    ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu Bağlama bölmesi](./media/header-citrix-netscaler-tutorial/virtual04.png)

    > [!NOTE]
    > **Kimlik Doğrulama Sanal Sunucu Yapılandırma** bölmesindeki **Bitti'yi** seçtiğinizden emin olun.

1. Değişikliklerinizi doğrulamak için bir tarayıcıda uygulama URL'sine gidin. Daha önce görebileceğiniz kimlik doğrulaması olmayan erişim yerine kiracı oturum açma sayfanızı görmeniz gerekir.

    ![Citrix NetScaler yapılandırması - Bir web tarayıcısında oturum açma sayfası](./media/header-citrix-netscaler-tutorial/virtual05.png)

## <a name="configure-citrix-netscaler-sso-for-header-based-authentication"></a>Üstbilgi tabanlı kimlik doğrulama için Citrix NetScaler SSO'ya yapılandırın

### <a name="configure-citrix-adc"></a>Citrix ADC'yi yapılandır

Üstbilgi tabanlı kimlik doğrulaması için Citrix ADC'yi yapılandırmak için aşağıdaki bölümleri tamamlayın.

#### <a name="create-a-rewrite-action"></a>Yeniden yazma eylemi oluşturma

1. **AppExpert** > **Rewrite Rewrite** > **Eylemler**gidin.
 
    ![Citrix NetScaler yapılandırması - Eylemler bölmesine yeniden yazma](./media/header-citrix-netscaler-tutorial/header01.png)

1.  **Ekle'yi**seçin ve ardından aşağıdaki adımları tamamlayın:

    1. **Ad**için, yeniden yazma eylemi için bir ad girin.

    1. **Türü**için , **INSERT_HTTP_HEADER**girin .

    1. **Üstbilgi Adı**için, bir üstbilgi adı girin (bu örnekte, _SecretID_kullanın).

    1. **İfade**için , **aaa girin. Kullanıcı. ÖZNITELIK("mySecretID"),** **mySecretID'in** Citrix ADC'ye gönderilen Azure AD SAML iddiası olduğu yerdir.

    1. **Oluştur'u**seçin.

    ![Citrix NetScaler yapılandırması - Yeniden Yazma Eylem bölmesi oluştur](./media/header-citrix-netscaler-tutorial/header02.png)
 
#### <a name="create-a-rewrite-policy"></a>Yeniden yazma ilkesi oluşturma

1.  **AppExpert** > **Yeniden Yazma** > **İlkeleri'ne**gidin.
 
    ![Citrix NetScaler yapılandırması - İlkeler bölmelerini yeniden yazma](./media/header-citrix-netscaler-tutorial/header03.png)

1.  **Ekle'yi**seçin ve ardından aşağıdaki adımları tamamlayın:

    1. **Ad**için, yeniden yazma ilkesi için bir ad girin.

    1. **Eylem**için, önceki bölümde oluşturduğunuz yeniden yazma eylemini seçin.

    1. **İfade**için , **doğru**girin .

    1. **Oluştur'u**seçin.

    ![Citrix NetScaler yapılandırması - Yeniden Yazma İlkesi bölmesi oluştur](./media/header-citrix-netscaler-tutorial/header04.png)

### <a name="bind-a-rewrite-policy-to-a-virtual-server"></a>Yeniden yazma ilkesini sanal sunucuya bağlama

GUI'yi kullanarak yeniden yazma ilkesini sanal sunucuya bağlamak için:

1. Trafik **Yönetimi** > **Yük Dengeleme** > **Sanal Sunucular**gidin.

1. Sanal sunucular listesinde, yeniden yazma ilkesini bağlamak istediğiniz sanal sunucuyu seçin ve sonra **Aç'ı**seçin.

1. Yük **Dengeleme Sanal Sunucu** bölmesinde, **Gelişmiş Ayarlar**altında, **İlkeler'i**seçin. NetScaler örneğiniz için yapılandırılan tüm ilkeler listede görünür.
 
    ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu bölmesi](./media/header-citrix-netscaler-tutorial/header05.png)

    ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu bölmesi](./media/header-citrix-netscaler-tutorial/header06.png)

1.  Bu sanal sunucuya bağlamak istediğiniz ilkenin adının yanındaki onay kutusunu seçin.
 
    ![Citrix NetScaler yapılandırması - Yük Dengeleme Sanal Sunucu Trafik Politikası Bağlama bölmesi](./media/header-citrix-netscaler-tutorial/header08.png)

1. Türü **Seç** iletişim kutusunda:

    1. **İlke Seç**için **Trafik'i**seçin.

    1. **Türü Seç**, **İstek'i**seçin.

    ![Citrix NetScaler yapılandırması - İlkeler iletişim kutusu](./media/header-citrix-netscaler-tutorial/header07.png)

1.  **Tamam'ı**seçin. Durum çubuğundaki bir ileti, ilkenin başarıyla yapılandırıldığını gösterir.

### <a name="modify-the-saml-server-to-extract-attributes-from-a-claim"></a>Bir hak talebinden öznitelikleri ayıklamak için SAML sunucusunu değiştirme

1.  **Security** > **AAA'** > ya git - Uygulama Trafik**İlkeleri** > **Kimlik Doğrulama** > **Gelişmiş İlkeler** > **Eylemleri** > **Sunucuları**.

1.  Uygulama için uygun kimlik doğrulama SAML sunucusunu seçin.
 
    ![Citrix NetScaler yapılandırma - Yapılandırma Kimlik Doğrulama SAML Server bölmesi](./media/header-citrix-netscaler-tutorial/header09.png)

1. **Öznitelikler** ağrı, ayıklamak istediğiniz SAML öznitelikleri girin, virgül ile ayrılmış. Örneğimizde, özniteliği `mySecretID`giriyoruz.
 
    ![Citrix NetScaler yapılandırması - Öznitelikler bölmesi](./media/header-citrix-netscaler-tutorial/header10.png)

1. Erişimi doğrulamak için, tarayıcıdaki URL'de, **Üstbilgi Koleksiyonu**altında SAML özniteliğini arayın.

    ![Citrix NetScaler yapılandırması - URL'de Başlıklar Koleksiyonu](./media/header-citrix-netscaler-tutorial/header11.png)

### <a name="create-a-citrix-netscaler-test-user"></a>Citrix NetScaler test kullanıcısı oluşturma

Bu bölümde, B.Simon adlı bir kullanıcı Citrix NetScaler oluşturulur. Citrix NetScaler, varsayılan olarak etkinleştirilen tam zamanında kullanıcı sağlamayı destekler. Bu bölümde işlem yapmak için herhangi bir işlem yoktur. Citrix NetScaler'da bir kullanıcı zaten yoksa, kimlik doğrulamadan sonra yeni bir kullanıcı oluşturulur.

> [!NOTE]
> El ile bir kullanıcı oluşturmanız gerekiyorsa, [Citrix NetScaler istemci destek ekibine](https://www.citrix.com/contact/technical-support.html)başvurun.

## <a name="test-sso"></a>Test SSO 

Bu bölümde, Access Panelini kullanarak Azure AD SSO yapılandırmanızı sınayabilirsiniz.

Erişim Paneli'ndeki Citrix NetScaler döşemesini seçtiğinizde, SSO'yu kurduğunuz Citrix NetScaler'da otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarının Azure Active Directory ile nasıl entegre edilebildiğini anlatan öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Citrix NetScaler'ı deneyin](https://aad.portal.azure.com/)

- [Kerberos tabanlı kimlik doğrulama için Citrix NetScaler tek oturumunu yapılandırın](citrix-netscaler-tutorial.md)
