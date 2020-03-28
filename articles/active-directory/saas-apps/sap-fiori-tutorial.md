---
title: 'Öğretici: SAP Fiori ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SAP Fiori arasında tek oturum açma yı nasıl yapılandırıştırmayı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 77ad13bf-e56b-4063-97d0-c82a19da9d56
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/05/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 917ba9274276fec5d01a40bdf7219e8d4fee1395
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78897754"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sap-fiori"></a>Öğretici: SAP Fiori ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SAP Fiori'yi Azure Etkin Dizini (Azure AD) ile nasıl entegre acağınızı öğreneceksiniz. SAP Fiori'yi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* SAP Fiori erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla SAP Fiori'de otomatik olarak oturum açmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SAP Fiori tek oturum açma (SSO) aboneliğini etkinleştirdi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SAP Fiori **SP** başlatılan SSO destekler

> [!NOTE]
> SAP Fiori tarafından başlatılan iFrame Kimlik Doğrulaması için, sessiz kimlik doğrulaması için SAML AuthnRequest'teki **IsPassive** parametresini kullanmanızı öneririz. **IsPassive** parametresi hakkında daha fazla bilgi için [Azure AD SAML tek oturum açma](https://docs.microsoft.com/azure/active-directory/develop/single-sign-on-saml-protocol) bilgilerine bakın

## <a name="adding-sap-fiori-from-the-gallery"></a>Galeriden SAP Fiori ekleme

SAP Fiori'nin Azure AD'ye entegrasyonunu yapılandırmak için, galeriden SAP Fiori'yi yönetilen SaaS uygulamaları listenize eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **SAP Fiori** yazın.
1. Sonuç panelinden **SAP Fiori'yi** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sap-fiori"></a>SAP Fiori için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak SAP Fiori ile yapılandırma ve test edin. SSO'nun çalışması için, BIR Azure AD kullanıcısı ile SAP Fiori'deki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu SAP Fiori ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. **[SAP Fiori SSO'yu yapılandırır](#configure-sap-fiori-sso)** - uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için.
    1. **[SAP Fiori test kullanıcısını oluşturun](#create-sap-fiori-test-user)** - SAP Fiori'de kullanıcının Azure AD gösterimine bağlı b.simon'ın bir muadili olması için.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Yeni bir web tarayıcısı penceresi açın ve SAP Fiori şirket sitenizde yönetici olarak oturum açın.

1. **http** ve **https** hizmetlerinin etkin olduğundan ve ilgili bağlantı noktalarının Işlem kodu **SMICM'e**atandığından emin olun.

1. Tek oturum açmanın gerekli olduğu SAP sistemi **T01**için SAP Business Client'da oturum açın. Ardından, HTTP Güvenlik Oturum Yönetimi'ni etkinleştirin.

    1. Hareket koduna git **SICF_SESSIONS.** Geçerli değerlere sahip tüm ilgili profil parametreleri gösterilir. Bunlar aşağıdaki örnek gibi görünür:

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
        > Parametreleri kuruluş gereksinimlerinize göre ayarlayın. Önceki parametreler yalnızca bir örnek olarak verilmiştir.

    1. Gerekirse, SAP sisteminin örnek (varsayılan) profilinde parametreleri ayarlayın ve SAP sistemini yeniden başlatın.

    1. BIR HTTP güvenlik oturumunu etkinleştirmek için ilgili istemciyi çift tıklatın.

        ![SAP'de İlgili Profil Parametrelerinin Geçerli Değerleri sayfası](./media/sapfiori-tutorial/tutorial-sapnetweaver-profileparameter.png)

    1. Aşağıdaki SICF hizmetlerini etkinleştirin:

        ```
        /sap/public/bc/sec/saml2
        /sap/public/bc/sec/cdc_ext_service
        /sap/bc/webdynpro/sap/saml2
        /sap/bc/webdynpro/sap/sec_diag_tool (This is only to enable / disable trace)
        ```

1. SAP sistemi için İş İstemci'sinde IŞLEM kodu **SAML2'ye** gidin [**T01/122**]. Yapılandırma UI yeni bir tarayıcı penceresinde açılır. Bu örnekte, SAP sistemi 122 için Business Client'ı kullanıyoruz.

    ![SAP Fiori Business Client oturum açma sayfası](./media/sapfiori-tutorial/tutorial-sapnetweaver-sapbusinessclient.png)

1. Kullanıcı adınızı ve şifrenizi girin ve ardından **Oturum Aç'ı**seçin.

    ![SAP'de ABAP Sistemi T01/122 sayfasının SAML 2.0 Yapılandırması](./media/sapfiori-tutorial/tutorial-sapnetweaver-userpwd.png)

1. Sağlayıcı **Adı** kutusunda, **T01122'yi** http ile değiştirin: **\//T01122**, ve sonra **Kaydet'i**seçin.

    > [!NOTE]
    > Varsayılan olarak, sağlayıcı adı>\< \<istemci> biçimindedir. Azure AD, biçim \<protokolündeki adı\<>>. Azure AD'de birden çok\://\<SAP \<Fiori ABAP motorlarını yapılandırabilmeniz için sağlayıcı adını https sid>istemci> olarak tutmanızı öneririz.

    ![SAP'deki ABAP System T01/122 sayfasının SAML 2.0 Yapılandırmasında güncelleştirilmiş sağlayıcı adı](./media/sapfiori-tutorial/tutorial-sapnetweaver-providername.png)

1. **Yerel Sağlayıcı sekmesi** > **Metadata'yı**seçin.

1. **SAML 2.0 Metadata** iletişim kutusunda, oluşturulan meta data XML dosyasını indirin ve bilgisayarınıza kaydedin.

    ![SAP SAML 2.0 Metadata iletişim kutusundaki Meta veri indirme bağlantısı](./media/sapfiori-tutorial/tutorial-sapnetweaver-generatesp.png)

1. **SAP Fiori** uygulama tümleştirme sayfasındaki [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve tek **oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, **Servis Sağlayıcı meta veri dosyanız**varsa aşağıdaki adımları gerçekleştirin:

    a. **Meta veri dosyalarını yükle'yi**tıklatın.

    ![Meta veri dosyalarını yükleme](common/upload-metadata.png)

    b. Meta veri dosyasını seçmek için **klasör logosuna** tıklayın ve **Yükle'yi**tıklatın.

    ![meta veri dosyasini seçin](common/browse-upload-metadata.png)

    c. Meta veri dosyası başarıyla yüklendiğinde, **Tanımlayıcı** ve **YanıtLA URL** değerleri **Temel SAML Yapılandırma** bölmesinde otomatik olarak doldurulur. **URL'de Oturum Aç** kutusuna, aşağıdaki desene `https:\//\<your company instance of SAP Fiori\>`sahip bir URL girin: .

    > [!NOTE]
    > Birkaç müşteri, yanlış yapılandırılan **YanıtURL** değerleriyle ilgili hataları bildirir. Bu hatayı görürseniz, örneğiniz için doğru Yanıt URL'sini ayarlamak için aşağıdaki PowerShell komut dosyasını kullanabilirsiniz:
    >
    > ```
    > Set-AzureADServicePrincipal -ObjectId $ServicePrincipalObjectId -ReplyUrls "<Your Correct Reply URL(s)>"
    > ``` 
    > 
    > Komut dosyasını `ServicePrincipal` çalıştırmadan önce nesne kimliğini kendiniz ayarlayabilir veya buradan geçirebilirsiniz.

1. SAP Fiori uygulaması, SAML iddialarının belirli bir biçimde olmasını bekler. Bu uygulama için aşağıdaki talepleri yapılandırın. Bu öznitelik değerlerini yönetmek için **SAML bölmesiyle Tek Oturum Açma'da** **Edit'i**seçin.

    ![Kullanıcı özdeyiş bölmesi](common/edit-attribute.png)

1. Kullanıcı **Öznitelikleri & Talepler** bölmesinde, önceki resimde gösterildiği gibi SAML belirteç özniteliklerini yapılandırın. Ardından, aşağıdaki adımları tamamlayın:

    1. **Kullanıcı taleplerini yönet** bölmesini açmak için **Edit'i** seçin.

    1. **Dönüşüm** listesinde **ExtractMailPrefix()** seçeneğini belirleyin.

    1. **Parametre 1** listesinde **user.userprincipalname'yi**seçin.

    1. **Kaydet'i**seçin.

       ![Kullanıcı taleplerini yönet bölmesi](./media/sapfiori-tutorial/nameidattribute.png)

       ![Kullanıcı talepleri bölmesinde Dönüşüm bölümü](./media/sapfiori-tutorial/nameidattribute1.png)
    
1. **SAML İmza Sertifikası** bölümünde **SAML ile tek oturum açma'da** **Federation Metadata XML'i** bulun ve sertifikayı indirmek ve bilgisayarınıza kaydetmek için **İndir'i** seçin.

    ![Sertifika indirme bağlantısı](common/metadataxml.png)

1. SAP **Fiori'yi ayarla** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.

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

Bu bölümde, B.Simon'ın SAP Fiori'ye erişim sağlayarak Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde **SAP Fiori'yi**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-sap-fiori-sso"></a>SAP Fiori SSO'nun yapılandırılsın

1. SAP sisteminde oturum açın ve işlem kodu **SAML2'ye**gidin. SAML yapılandırma sayfasıyla birlikte yeni bir tarayıcı penceresi açılır.

1. Güvenilir bir kimlik sağlayıcısı (Azure AD) için uç noktalarını yapılandırmak için **Güvenilen Sağlayıcılar** sekmesini seçin.

    ![SAP'de Güvenilir Sağlayıcılar sekmesi](./media/sapfiori-tutorial/tutorial-sapnetweaver-samlconfig.png)

1. **Ekle'yi**ve ardından bağlam menüsünden **Meta Veri Dosyasını Yükle'yi** seçin.

    ![SAP'de Meta Veri Dosyası Ekle ve Yükle seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-uploadmetadata.png)

1. Azure portalına indirdiğiniz meta veri dosyasını yükleyin. **Sonraki'ni**seçin.

    ![SAP'de yüklemek için meta veri dosyasını seçin](./media/sapfiori-tutorial/tutorial-sapnetweaver-metadatafile.png)

1. Sonraki sayfada, Diğer **Ad** kutusuna diğer ad girin. Örneğin, **aadsts**. **Sonraki'ni**seçin.

    ![SAP'deki Diğer Ad kutusu](./media/sapfiori-tutorial/tutorial-sapnetweaver-aliasname.png)

1. **Özet Algoritma** kutusundaki değerin **SHA-256**olduğundan emin olun. **Sonraki'ni**seçin.

    ![SAP'de Özet Algoritma değerini doğrulama](./media/sapfiori-tutorial/tutorial-sapnetweaver-identityprovider.png)

1. **Tek Oturum Açma Bitiş Noktaları**altında HTTP **POST'u**seçin ve sonra **İleri'yi**seçin.

    ![SAP'de Tek Oturum Açma Bitiş Noktaları seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-httpredirect.png)

1. **Tek Oturum Sonu Bitiş Noktaları**altında HTTP Yeniden **Yönlendirme'yi**seçin ve sonra **İleri'yi**seçin.

    ![SAP'de Tek Oturum Sonu Son Noktaları seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-httpredirect1.png)

1. **Artefakt Uç Noktaları**altında devam etmek için **İleri'yi** seçin.

    ![SAP'de Artefakt Uç Noktaları seçenekleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-artifactendpoint.png)

1. **Kimlik Doğrulama Gereksinimleri**altında **Finish'i**seçin.

    ![SAP'de kimlik doğrulama gereksinimleri seçenekleri ve Bitiş seçeneği](./media/sapfiori-tutorial/tutorial-sapnetweaver-authentication.png)

1. **Güvenilir Sağlayıcı** > **Kimliği Federasyonu'nu** seçin (sayfanın alt kısmında). **Edit'i**seçin.

    ![SAP'de Güvenilir Sağlayıcı ve Kimlik Federasyonu sekmeleri](./media/sapfiori-tutorial/tutorial-sapnetweaver-trustedprovider.png)

1. **Ekle'yi**seçin.

    ![Kimlik Federasyonu sekmesinde Ekle seçeneği](./media/sapfiori-tutorial/tutorial-sapnetweaver-addidentityprovider.png)

1. Desteklenen **NameID Biçimleri** iletişim kutusunda **Belirtilmemiş'i**seçin. **Tamam'ı**seçin.

    ![DESTEKLENEN NameID Biçimleri iletişim kutusu ve SAP'deki seçenekler](./media/sapfiori-tutorial/tutorial-sapnetweaver-nameid.png)

    **Kullanıcı Kimliği Kaynağı** ve Kullanıcı Kimliği **Eşleme Modu** değerleri SAP kullanıcısı ile Azure AD talebi arasındaki bağlantıyı belirler.  

    **Senaryo 1**: SAP kullanıcısından Azure AD kullanıcı eşlenesine

    1. SAP'de, **NameID Formatı "Belirtilmemiş" ayrıntıları**altında, ayrıntılara dikkat edin:

        ![SAP'de NameID Formatı "Belirtilmemiş" iletişim kutusunun ayrıntıları](./media/sapfiori-tutorial/nameiddetails.png)

    1. Azure portalında, **Kullanıcı Öznitelikleri & Talepler**altında, Azure AD'den gerekli talepleri not edin.

        ![Azure portalındaki Kullanıcı Öznitelikleri & Talepler iletişim kutusu](./media/sapfiori-tutorial/claimsaad1.png)

    **Senaryo 2**: SU01'de yapılandırılan e-posta adresine göre SAP kullanıcı kimliğini seçin. Bu durumda, SSO gerektiren her kullanıcı için e-posta kimliği SU01 olarak yapılandırılmalıdır.

    1.  SAP'de, **NameID Formatı "Belirtilmemiş" ayrıntıları**altında, ayrıntılara dikkat edin:

        ![SAP'de NameID Formatı "Belirtilmemiş" iletişim kutusunun ayrıntıları](./media/sapfiori-tutorial/tutorial-sapnetweaver-nameiddetails1.png)

    1. Azure portalında, **Kullanıcı Öznitelikleri & Talepler**altında, Azure AD'den gerekli talepleri not edin.

       ![Azure portalındaki Kullanıcı Öznitelikleri & Talepler iletişim kutusu](./media/sapfiori-tutorial/claimsaad2.png)

1. **Kaydet'i**seçin ve ardından kimlik sağlayıcısını etkinleştirmek için **Etkinleştir'i** seçin.

    ![SAP'de Kaydet ve Etkinleştir seçenekleri](./media/sapfiori-tutorial/configuration1.png)

1. İstendiğinde **Tamam'ı** seçin.

    ![SAP'de SAML 2.0 Yapılandırma iletişim kutusunda Tamam seçeneği](./media/sapfiori-tutorial/configuration2.png)

### <a name="create-sap-fiori-test-user"></a>SAP Fiori test kullanıcısı oluşturma

Bu bölümde, SAP Fiori'de Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıyı SAP Fiori platformuna eklemek için şirket içi SAP uzman ekibinizle veya kuruluş SAP iş ortağınızla çalışın.

## <a name="test-sso"></a>Test SSO

1. Kimlik sağlayıcısı Azure AD SAP Fiori'de etkinleştirildikten sonra, tek oturum açma yı test etmek için aşağıdaki URL'lerden birine erişmeye çalışın (kullanıcı adı ve parola istenmemelidir):

    * https:\//\<sapurl\>/sap/bc/bsp/sap/it00/default.htm
    * https:\//\<sapurl\>/sap/bc/bsp/sap/it00/default.htm

    > [!NOTE]
    > *Sapurl'yi* gerçek SAP ana bilgisayar adı ile değiştirin.

1. Test URL'si sizi SAP'deki aşağıdaki test uygulama sayfasına götürmelidir. Sayfa açılırsa, Azure AD tek oturum açma başarıyla ayarlanır.

    ![SAP'deki standart test uygulama sayfası](./media/sapfiori-tutorial/testingsso.png)

1. Bir kullanıcı adı ve parola istenirse, sorunu tanılamaya yardımcı olmak için izlemeyi etkinleştirin. İzleme için aşağıdaki URL'yi\//\<kullanın:\>https: sapurl /sap/bc/webdynpro/sap/sec_diag_tool?sap-client=122&sap-language=EN#.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile SAP Fiori'yi deneyin](https://aad.portal.azure.com/)
