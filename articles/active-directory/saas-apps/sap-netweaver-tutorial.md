---
title: 'Öğretici: Öğretici: SAP NetWeaver ile Azure Active Directory Tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAP NetWeaver arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: celested
ms.assetid: 1b9e59e3-e7ae-4e74-b16c-8c1a7ccfdef3
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 08/14/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 700f2ca4d46b3483531fa0784cb78699befb20ca
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78897744"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-netweaver"></a>Öğretici: SAP NetWeaver ile Azure Active Directory Tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SAP NetWeaver'ı Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. SAP NetWeaver'ı Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SAP NetWeaver erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP NetWeaver'da otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SAP NetWeaver tek oturum açma (SSO) aboneliğini etkinleştirdi.
* SAP NetWeaver V7.20 gerekli atleast

## <a name="scenario-description"></a>Senaryo açıklaması

SAP NetWeaver hem **SAML** destekler (**SP başlatılan SSO**) ve **OAuth**. Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz. 

> [!NOTE]
> Uygulamayı, kuruluş gereksiniminize göre SAML'de veya OAuth'ta yapılandırın. 

## <a name="adding-sap-netweaver-from-the-gallery"></a>Galeriden SAP NetWeaver ekleme

SAP NetWeaver'ın Azure AD'ye entegrasyonunu yapılandırmak için, galeriden SAP NetWeaver'ı yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SAP NetWeaver** yazın.
1. Sonuç panelinden **SAP NetWeaver'ı** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sap-netweaver"></a>SAP NetWeaver için Azure AD oturum açma işlemlerini yapılandırma ve test edin

**B.Simon**adlı bir test kullanıcısı kullanarak Azure AD SSO'nu SAP NetWeaver ile yapılandırın ve test edin. SSO'nun çalışması için, BIR Azure AD kullanıcısı ile SAP NetWeaver'daki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu SAP NetWeaver ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD oturum açma'yı B.Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. UYGULAMA tarafındaki SSO ayarlarını yapılandırmak için **[SAML kullanarak SAP NetWeaver'ı yapılandırın.](#configure-sap-netweaver-using-saml)**
    1. **[SAP NetWeaver test kullanıcısını,](#create-sap-netweaver-test-user)** kullanıcının Azure AD gösterimine bağlı SAP NetWeaver'da B.Simon'ın bir muadili olması için oluşturun.
1. Yapılandırmanın çalışıp çalışmadığını doğrulamak için **[SSO'yu test](#test-sso)** edin.
1. **[OAuth için SAP NetWeaver'ı](#configure-sap-netweaver-for-oauth)** uygulama tarafında OAuth ayarlarını yapılandırmak için yapılandırın.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Azure AD oturum açma işlemlerini SAP NetWeaver ile yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Yeni bir web tarayıcısı penceresi açın ve SAP NetWeaver şirket sitenizde yönetici olarak oturum açın

1. **http** ve **https** hizmetlerinin etkin olduğundan ve **SMICM** T-Code'a uygun bağlantı noktalarının atandığından emin olun.

1. SSO'nun gerekli olduğu SAP System'in (T01) iş istemcisine oturum açın ve HTTP Security oturum yönetimini etkinleştirin.

    a. **SICF_SESSIONS**İşlem koduna gidin. Geçerli değerlerle ilgili tüm profil parametrelerini görüntüler. Onlar aşağıda gibi görünüyorsun:-
    ```
    login/create_sso2_ticket = 2
    login/accept_sso2_ticket = 1
    login/ticketcache_entries_max = 1000
    login/ticketcache_off = 0  login/ticket_only_by_https = 0 
    icf/set_HTTPonly_flag_on_cookies = 3
    icf/user_recheck = 0  http/security_session_timeout = 1800
    http/security_context_cache_size = 2500
    rdisp/plugin_auto_logout = 1800
    rdisp/autothtime = 60
    ```
    >[!NOTE]
    > Yukarıdaki parametreleri kuruluş gereksinimlerinize göre ayarlayın, Yukarıdaki parametreler burada yalnızca endikasyon olarak verilmiştir.

    b. Gerekirse, SAP sisteminin örnek/varsayılan profilinde parametreleri ayarlayın ve SAP sistemini yeniden başlatın.

    c. HTTP güvenlik oturumunu etkinleştirmek için ilgili istemciye çift tıklayın.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_profileparameter.png)

    d. SICF hizmetlerinin altında etkinleştirin:
    ```
    /sap/public/bc/sec/saml2
    /sap/public/bc/sec/cdc_ext_service
    /sap/bc/webdynpro/sap/saml2
    /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
    ```
1. SAP sisteminin iş istemcisinde IŞLEM kodu **SAML2'ye** gidin [T01/122]. Bir tarayıcıda bir kullanıcı arabirimi açılır. Bu örnekte, SAP iş istemcisi olarak 122'yi varsaydık.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_sapbusinessclient.png)

1. Kullanıcı arabirimine girmek için kullanıcı adınızı ve şifrenizi girin ve **Edit'i**tıklatın.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_userpwd.png)

1. **Sağlayıcı Adını** T01122'den Kaydet'e ve `http://T01122` **kaydet'e**tıklayın.

    > [!NOTE]
    > Varsayılan olarak sağlayıcı `<sid><client>` adı biçim olarak gelir, ancak `<protocol>://<name>`Azure AD biçiminde ad `https://<sid><client>` bekler, birden çok SAP NetWeaver ABAP motorunun Azure AD'de yapılandırmasına izin verecek şekilde sağlayıcı adını korumanızı önerir.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_providername.png)

1. **Hizmet Sağlayıcı Meta verileri oluşturma**:- SAML 2.0 Kullanıcı Arabiriminde Yerel **Sağlayıcı** ve **Güvenilir Sağlayıcılar** ayarlarını yapılandırmayı bitirdikten sonra, bir sonraki adım servis sağlayıcısının meta veri dosyasını oluşturmak olacaktır (SAP'deki tüm ayarları, kimlik doğrulama bağlamlarını ve diğer yapılandırmaları içerir). Bu dosya oluşturulduktan sonra bunu Azure AD'ye yüklememiz gerekir.

    ![Sertifika indirme bağlantısı](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_generatesp.png)

    a. Yerel **Sağlayıcı sekmesine**gidin.

    b. Meta **data'yı**tıklatın.

    c. Oluşturulan **Metadata XML dosyasını** bilgisayarınıza kaydedin ve Azure portalındaki **Tanımlayıcı** ve **YanıtURL** değerlerini otomatik olarak doldurmak için **Temel SAML Yapılandırması** bölümüne yükleyin.

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. AZURE [portalında,](https://portal.azure.com/) **SAP NetWeaver** uygulama tümleştirme sayfasında, **Yönet** bölümünü bulun ve Tek **oturum açma'yı**seçin.
1. Tek **oturum açma yöntemi** sayfasında **SAML'yi**seçin.
1. **SAML** ile Tek Oturum Açma'da, ayarları düzenlemek için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırma** bölümünde, uygulamayı **IDP** tarafından başlatılan modda yapılandırmak istiyorsanız, aşağıdaki adımı gerçekleştirin:

    a. Daha önce elde ettiğiniz **Servis Sağlayıcı meta veri dosyasını**yüklemek için **meta veri dosyasını yükle'yi** tıklatın.

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    c. Meta veri dosyası başarıyla yüklendikten sonra, **Tanımlayıcı** ve **YanıtURL** değerleri aşağıda gösterildiği gibi **Temel SAML Yapılandırması** bölümünde otomatik olarak doldurulur:

    d. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<your company instance of SAP NetWeaver>`

    > [!NOTE]
    > Örnekleriyle karşılarına bir açıklama yapılandırılan yanlış Yanıt URL'si hatası bildiren çok az müşteri gördük. Böyle bir hata alırsanız, örneğiniz için doğru Yanıt URL'sini ayarlamak için powershell komut dosyasını geçici olarak kullanabilirsiniz.:
    > ```
    > Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls "<Your Correct Reply URL(s)>"
    > ``` 
    > ServicePrincipal Object ID önce kendiniz ayarlanacak veya bunu buradan da geçebilirsiniz.

1. SAP NetWeaver uygulaması, SAML belirteç öznitelikleri yapılandırmanıza özel öznitelik eşlemeleri eklemenizi gerektiren belirli bir biçimde SAML iddialarını bekler. Aşağıdaki ekran görüntüsü varsayılan özniteliklerin listesini gösterir. Kullanıcı Öznitelikleri iletişim kutusunu açmak için **Edit** simgesini tıklatın.

    ![image](common/edit-attribute.png)

1. **Kullanıcı Öznitelikleri** iletişim kutusundaki **Kullanıcı Talepleri** bölümünde, saml belirteç özniteliğini yukarıdaki resimde gösterildiği gibi yapılandırın ve aşağıdaki adımları gerçekleştirin:

    a. **Kullanıcı taleplerini yönet** iletişim kutusunu açmak için **Edit simgesini** tıklatın.

    ![image](./media/sapnetweaver-tutorial/nameidattribute.png)

    ![image](./media/sapnetweaver-tutorial/nameidattribute1.png)

    b. **Dönüşüm** listesinden **ExtractMailPrefix()** seçeneğini belirleyin.

    c. **Parametre 1** listesinden **user.userprincipalname'yi**seçin.

    d. **Kaydet**'e tıklayın.

1. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, Federation **Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

   ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. SAP **NetWeaver'ı Ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, Sap NetWeaver'a erişim sağlayarak B.Simon'ın Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **SAP NetWeaver'ı**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-sap-netweaver-using-saml"></a>SAML kullanarak SAP NetWeaver'ı yapılandır

1. SAP sisteminde oturum açın ve işlem kodu SAML2'ye gidin. Saml yapılandırma ekranı ile yeni tarayıcı penceresi açılır.

2. Güvenilen Kimlik sağlayıcısı (Azure AD) için Bitiş noktalarını yapılandırmak için **Güvenilen Sağlayıcılar** sekmesine gidin.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_samlconfig.png)

3. Bağlam menüsünden **Ekle** ve **Meta Veri Dosyasını Yükle'yi** seçin' e basın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_uploadmetadata.png)

4. Azure portalından indirdiğiniz meta veri dosyasını yükleyin.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_metadatafile.png)

5. Sonraki ekranda Diğer Ad adını yazın. Örneğin, devam etmek için aadsts ve **Devamı** tuşuna basın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_aliasname.png)

6. **Özet Algoritmanızın** **SHA-256** olduğundan ve herhangi bir değişiklik gerektirmediğinden emin olun ve **İleri**tuşuna basın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_identityprovider.png)

7. **Tek Oturum Açma Bitiş Noktalarında**HTTP **POST'u** kullanın ve devam etmek için **İleri'yi** tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect.png)

8. **Tek Giriş Son Noktalarında** **HTTPRedirect'i** seçin ve devam etmek için **İleri'yi** tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_httpredirect1.png)

9. **Artifakı Uç Noktaları'nda**devam etmek için **İleri** tuşuna basın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_artifactendpoint.png)

10. **Kimlik Doğrulama Gereksinimleri'nde** **Finish'i**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_authentication.png)

11. **Sekmeye gidin Güvenilir Sağlayıcı** > **Kimliği Federasyonu** (ekranın altından). **Düzenle**’ye tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_trustedprovider.png)

12. **Kimlik Federasyonu** sekmesinin altında **Ekle'yi** tıklatın (alt pencere).

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_addidentityprovider.png)

13. Açılan pencereden **Desteklenen NameID biçimlerinden** **Belirtilmemiş'i** seçin ve Tamam'ı tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameid.png)

14. Kullanıcı **Kimliği Kaynağı** ve **kullanıcı kimliği eşleme modu** değerlerinin SAP kullanıcısı ile Azure AD talebi arasındaki bağlantıyı belirlediğini unutmayın.  

    #### <a name="scenario-sap-user-to-azure-ad-user-mapping"></a>Senaryo: SAP Kullanıcıdan Azure AD kullanıcı eşlenemesine.

    a. SAP'den nameID ayrıntıları ekran görüntüsü.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/nameiddetails.png)

    b. Azure AD'den gerekli taleplerden bahsetme ekran görüntüsü.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/claimsaad1.png)

    #### <a name="scenario-select-sap-user-id-based-on-configured-email-address-in-su01-in-this-case-email-id-should-be-configured-in-su01-for-each-user-who-requires-sso"></a>Senaryo: SU01'de yapılandırılmış e-posta adresine göre SAP kullanıcı kimliğini seçin. Bu durumda e-posta kimliği SSO gerektiren her kullanıcı için su01 olarak yapılandırılmalıdır.

    a.  SAP'den nameID ayrıntıları ekran görüntüsü.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/tutorial_sapnetweaver_nameiddetails1.png)

    b. Azure AD'den gerekli taleplerden bahseden ekran görüntüsü.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/claimsaad2.png)

15. **Kaydet'i** tıklatın ve ardından kimlik sağlayıcısını etkinleştirmek için **Etkinleştir'i** tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/configuration1.png)

16. İstendikten sonra **Tamam'ı** tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/configuration2.png)

    ### <a name="create-sap-netweaver-test-user"></a>SAP NetWeaver test kullanıcıoluşturma

    Bu bölümde SAP NetWeaver'da B.simon adında bir kullanıcı oluşturursunuz. Lütfen sap uzman ekibinizi veya SAP NetWeaver platformundaki kullanıcıları eklemek için kuruluş SAP iş ortağınızla çalışın.

## <a name="test-sso"></a>Test SSO

1. Kimlik sağlayıcısı Azure AD etkinleştirildikten sonra, SSO'yu kontrol etmek için URL'nin altındaki url'ye erişmeye çalışın (kullanıcı adı & parola için bir istek olmaz)

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    (veya) aşağıdaki URL'yi kullanın

    `https://<sapurl>/sap/bc/bsp/sap/it00/default.htm`

    > [!NOTE]
    > Sapurl'yi gerçek SAP ana bilgisayar adı ile değiştirin.

2. Yukarıdaki URL aşağıda belirtilen ekrana götürmelidir. Aşağıdaki sayfaya ulaşabiliyorsanız, Azure AD SSO kurulumu başarıyla tamamlanır.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/testingsso.png)

3. Kullanıcı adı & parola istemi oluşursa, lütfen aşağıdaki URL'yi kullanarak izlemeyi etkinleştirerek sorunu tanılayın

    `https://<sapurl>/sap/bc/webdynpro/sap/sec_diag_tool?sap-client=122&sap-language=EN#`

## <a name="configure-sap-netweaver-for-oauth"></a>OAuth için SAP NetWeaver'ı yapılandır

1. SAP Belgelenmiş işlem şu anda kullanılabilir: [NetWeaver Ağ Geçidi Hizmeti Etkinleştirme ve OAuth 2.0 Kapsam Oluşturma](https://wiki.scn.sap.com/wiki/display/Security/NetWeaver+Gateway+Service+Enabling+and+OAuth+2.0+Scope+Creation)

2. SPRO'ya gidin ve **Etkinleştir ve Bakım hizmetlerini**bulun.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth01.png)

3. Bu örnekte OData hizmetini Bağlamak `DAAG_MNGGRP` istiyoruz: OAuth ile Azure AD SSO'ya. Hizmet `DAAG_MNGGRP` için teknik servis adı aramasını kullanın ve henüz etkin `green` değilse etkinleştirin (ICF düğümleri sekmesialtındaki duruma bakın). Sistem takma adı (bağlı arka uç sistemi, hizmetin gerçekten çalıştığı) doğru ysa emin olun.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth02.png)

    * Ardından üst düğme çubuğundaki **OAuth** düğmesine `scope` tıklayın ve atayın (varsayılan adı sunulduğu gibi tutun).

4. Örneğin kapsamı `DAAG_MNGGRP_001`, otomatik olarak bir numara ekleyerek hizmet adından oluşturulur. Rapor, `/IWFND/R_OAUTH_SCOPES` kapsam adını değiştirmek veya el ile oluşturmak için kullanılabilir.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth03.png)

    > [!NOTE]
    > İleti `soft state status is not supported` – hiçbir sorun olarak, göz ardı edilebilir. Daha fazla bilgi için [buraya](https://help.sap.com/doc/saphelp_nw74/7.4.16/1e/c60c33be784846aad62716b4a1df39/content.htm?no_cache=true) bakın

### <a name="create-a-service-user-for-the-oauth-20-client"></a>OAuth 2.0 İstemci için bir hizmet kullanıcısı oluşturma

1. OAuth2, `service ID` son kullanıcı adına erişim jetonunu almak için a kullanır. OAuth tasarımı nın önemli `OAuth 2.0 Client ID` kısıtlaması: `username` Erişim Jetonu talep ederken oturum açmak için Kullandığı OAuth 2.0 istemcisi ile aynı olmalıdır. Bu nedenle, örneğiniz için, istemci1 adında bir OAuth 2.0 istemcisi kaydedeceğiz ve ön koşul olarak aynı ada sahip bir kullanıcının (CLIENT1) SAP sisteminde bulunması ve bu kullanıcının belirtilen uygulama tarafından kullanılmak üzere yapılandırılacaktır. 

2. Bir OAuth İstemci'yi `SAML Bearer Grant type`kaydederken .

    >[!NOTE]
    >Daha fazla bilgi için, [saml](https://wiki.scn.sap.com/wiki/display/Security/OAuth+2.0+Client+Registration+for+the+SAML+Bearer+Grant+Type) taşıyıcı hibe türü için OAuth 2.0 Müşteri Kaydı bakın

3. tcod: SU01 / kullanıcı `System type` CLIENT1 olarak oluşturmak ve şifre atamak, arama koduna kullanıcı adı ile yakmak gerekir API programcı, kimlik bilgilerini sağlamak için gereken olarak kaydedin. Hiçbir profil veya rol atanmalıdır.

### <a name="register-the-new-oauth-20-client-id-with-the-creation-wizard"></a>Yeni OAuth 2.0 İstemci Kimliğini oluşturma sihirbazıyla kaydedin

1. Yeni bir **OAuth 2.0 istemci** başlangıç işlemi **SOAUTH2**kayıt için. İşlem, zaten kaydedilmiş olan OAuth 2.0 istemcileri hakkında genel bir genel bakış görüntüler. Bu örnekte CLIENT1 adlı yeni OAuth istemcisi için sihirbazı başlatmak için **Oluştur'u** seçin.

2. T-Code gidin: **SOAUTH2** ve açıklama sağlayın sonra **sonraki**tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth04.png)

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth05.png)

3. Açılan listeden zaten eklenen **SAML2 IdP - Azure AD'yi** seçin ve kaydedin.

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth06.png)

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth07.png)

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth08.png)

4. Daha önce oluşturulan kapsamı eklemek için kapsam atamaaltında **ekle'yi** tıklatın:`DAAG_MNGGRP_001`

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth09.png)

    ![Tek İşaret-On'u Yapılandır](./media/sapnetweaver-tutorial/oauth10.png)

5. **Bitiş'e**tıklayın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SAP NetWeaver'ı deneyin](https://aad.portal.azure.com/)