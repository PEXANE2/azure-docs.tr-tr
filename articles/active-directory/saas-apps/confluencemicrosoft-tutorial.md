---
title: 'Öğretici: Microsoft tarafından Confluence SAML SSO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve Confluencsam SAML SSO arasında Microsoft tarafından tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 1ad1cf90-52bc-4b71-ab2b-9a5a1280fb2d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/05/2019
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: e6ea52474f3b352fabf19050f20012438e9d0c19
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76120652"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-confluence-saml-sso-by-microsoft"></a>Öğretici: Microsoft tarafından Confluence SAML SSO ile Azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, Confluence SAML SSO'yu Microsoft'un Azure Active Directory (Azure AD) ile nasıl entegre ettiğinizi öğreneceksiniz. Microsoft'un Confluence SAML SSO'yu Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Microsoft tarafından Confluence SAML SSO erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın, Microsoft tarafından Saml SSO'yu Azure REKLAM hesaplarıyla birleştirme sayarı için otomatik olarak oturum açılmasını etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)bakın.

## <a name="description"></a>Açıklama:

Tek oturum açmayı etkinleştirmek için Atlassian Confluence sunucusundaki Microsoft Azure Active Directory hesabınızı kullanın. Bu şekilde tüm kuruluş kullanıcılarınız, Confluence uygulamasında oturum açtırmak için Azure AD kimlik bilgilerini kullanabilir. Bu eklenti federasyon için SAML 2.0 kullanır.

## <a name="prerequisites"></a>Ön koşullar

Azure AD tümleştirmesini Microsoft tarafından Birleştirme SAML SSO ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

- Azure AD aboneliği
- Windows 64 bit sunucusuna yüklenen Confluence sunucu uygulaması (şirket içinde veya bulut IaaS altyapısında)
- Confluence sunucusu HTTPS etkin
- Confluence Eklentisi için desteklenen sürümlerin aşağıdaki bölümde belirtildiğini unutmayın.
- Confluence sunucusuna internetüzerinden özellikle kimlik doğrulama için Azure AD Giriş sayfasına ulaşılabilir ve belirteci Azure AD'den alabilmeli
- Yönetici kimlik bilgileri Confluence'da ayarlanır
- WebSudo Confluence devre dışı bırakılır
- Confluence sunucu uygulamasında oluşturulan test kullanıcısı

> [!NOTE]
> Bu öğreticideki adımları test etmek için, Birleşme' nin bir üretim ortamını kullanmanızı önermiyoruz. Önce uygulamanın geliştirme veya evreleme ortamında tümleştirmeyi test edin ve ardından üretim ortamını kullanın.

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Gerekli olmadıkça üretim ortamınızı kullanmayın.
* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* Confluence SAML SSO Microsoft tek oturum açma (SSO) tarafından abonelik etkin.

## <a name="supported-versions-of-confluence"></a>Confluence'ın desteklenen sürümleri

Şu andan itibaren, Confluence'ın aşağıdaki sürümleri desteklenir:

- Biraraya: 5.0 - 5.10
- Biraraya gelmesi: 6.0.1 ile 6.15.9
- Biraraya gelmek: 7.0.1 ile 7.1.0

> [!NOTE]
> Confluence Eklentimizin Ubuntu Sürüm 16.04'te de çalıştığını lütfen unutmayın.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* Confluence SAML SSO Microsoft tarafından **SP** başlatılan SSO destekler

## <a name="adding-confluence-saml-sso-by-microsoft-from-the-gallery"></a>Galeriden Microsoft tarafından Confluence SAML SSO ekleme

Birleştirme SAML SSO'nun Microsoft tarafından Azure AD'ye entegrasyonunu yapılandırmak için, Galeriden Microsoft tarafından yönetilen SaaS uygulamaları listenize Confluence SAML SSO eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.
1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. Galeri **bölümünden Ekle** bölümünde, arama kutusuna **Microsoft tarafından Confluence SAML SSO** yazın.
1. Sonuç panelinden **Microsoft tarafından Confluence SAML SSO'yu** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-confluence-saml-sso-by-microsoft"></a>Microsoft tarafından SamL SSO'yu Birleştirme için Azure AD tek oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'yu Microsoft tarafından **B.Simon**adlı bir test kullanıcısı kullanarak Birleştirme SAML SSO ile yapılandırın ve test edin. SSO'nun çalışması için, Microsoft tarafından SAML SSO'yu Biraraya Getirmek'te bir Azure REKLAM kullanıcısı ile ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu Microsoft tarafından Saml SSO ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için **[Azure AD SSO'su yapılandırın.](#configure-azure-ad-sso)**
    1. Azure AD'yi B.Simon ile tek oturum açma test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
    1. B.Simon'ın Azure AD tek oturum açma kullanmasını sağlamak için **[Azure AD test kullanıcısını atayın.](#assign-the-azure-ad-test-user)**
1. Uygulama tarafındaki tek oturum açma ayarlarını yapılandırmak için **[SamL SSO'yu Microsoft SSO tarafından yapılandırın.](#configure-confluence-saml-sso-by-microsoft-sso)**
    1. **[Microsoft test kullanıcısı tarafından Confluence SAML SSO oluşturun](#create-confluence-saml-sso-by-microsoft-test-user)** - B.Simon'ın birleşimi olan bir benzerine sahip olmak için Microsoft tarafından kullanıcının Azure AD gösterimine bağlı olan Confluence SAML SSO'ya sahip olun.
1. **[SSO'yu test](#test-sso)** edin - yapılandırmanın çalışıp çalışmadığını doğrulamak için.

## <a name="configure-azure-ad-sso"></a>Azure AD SSO’yu yapılandırma

Azure portalında Azure AD SSO'yu etkinleştirmek için aşağıdaki adımları izleyin.

1. Microsoft uygulama tümleştirme **sayfasının Birleşimi SAML SSO'daki** [Azure portalında](https://portal.azure.com/) **Yönet** bölümünü bulun ve **tek oturum açma'yı**seçin.
1. Tek **bir oturum açma yöntemi** seç sayfasında **SAML'yi**seçin.
1. **SAML sayfasıyla tek oturum** açma'da, ayarları ayarlamak için **Temel SAML Yapılandırması** için düzenleme/kalem simgesini tıklatın.

   ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

1. Temel **SAML Yapılandırması** bölümünde, aşağıdaki alanların değerlerini girin:

    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<domain:port>/plugins/servlet/saml/auth`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<domain:port>/`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<domain:port>/plugins/servlet/saml/auth`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Tanımlayıcı, YanıtURL'i ve Oturum Açma URL'si ile güncelleştirin. Bağlantı noktası, adlandırılmış bir URL olması durumunda isteğe bağlıdır. Bu değerler, daha sonra öğreticide açıklanan Confluence eklentisinin yapılandırması sırasında alınır.

1. **SAML ile Tek Oturum** Açma sayfasında, **SAML İmza Sertifikası** bölümünde, App **Federation Metadata Url'sini** kopyalamak ve bilgisayarınıza kaydetmek için kopyala düğmesini tıklatın.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

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

Bu bölümde, Microsoft tarafından Confluence SAML SSO'ya erişim sağlayarak B.Simon'Un Azure tek oturum açma'yı kullanmasını sağlayacaksınız.

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin ve ardından **Tüm Uygulamaları**seçin.
1. Uygulamalar listesinde, Microsoft **tarafından Confluence SAML SSO'yu**seçin.
1. Uygulamanın genel bakış sayfasında, **Yönet** bölümünü bulun ve **Kullanıcıları ve grupları**seçin.

   !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

1. **Kullanıcı Ekle'yi**seçin, ardından **Atama Ekle** iletişim kutusunda Kullanıcılar ve **gruplar** seçin.

    ![Kullanıcı Ekle bağlantısı](common/add-assign-user.png)

1. Kullanıcılar **ve gruplar** iletişim kutusunda, Kullanıcılar listesinden **B.Simon'ı** seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. SAML iddiasında herhangi bir rol değeri bekliyorsanız, **Rolü Seç** iletişim kutusunda, listeden kullanıcı için uygun rolü seçin ve ardından ekranın altındaki **Seç** düğmesini tıklatın.
1. Atama **Ekle** iletişim kutusunda, **Ata ekle** düğmesini tıklatın.

## <a name="configure-confluence-saml-sso-by-microsoft-sso"></a>Microsoft SSO tarafından Birleşimi SAML SSO yapılandırma

1. Farklı bir web tarayıcısı penceresinde, Yönetici olarak Confluence örneğiniz için oturum açın.

1. Dişli üzerinde hover ve **Eklentiler**tıklayın.

    ![Tek İşaret-On'u Yapılandır](./media/confluencemicrosoft-tutorial/addon1.png)

1. Eklentiyi Microsoft [Download Center'dan](https://www.microsoft.com/download/details.aspx?id=56503)indirin. Microsoft tarafından sağlanan eklentiyi **Upload eklentisi** menüsünü kullanarak el ile yükleyin. Eklentinin karşıdan yüklenir, [Microsoft Hizmet Sözleşmesi](https://www.microsoft.com/servicesagreement/)kapsamındadır.

    ![Tek İşaret-On'u Yapılandır](./media/confluencemicrosoft-tutorial/addon12.png)

1. Confluence ters proxy senaryosunu çalıştırmak için veya yük dengeleyici senaryosu aşağıdaki adımları gerçekleştirin:

    > [!NOTE]
    > Sunucuyu önce aşağıdaki yönergelerle yapılandırmalı ve sonra eklentiyi yüklemelisiniz.

    a. JIRA sunucu uygulamasının **server.xml** dosyasındaki **bağlayıcı** bağlantı noktasına aşağıdaki özniteliği ekleyin.

    `scheme="https" proxyName="<subdomain.domain.com>" proxyPort="<proxy_port>" secure="true"`

    ![Tek İşaret-On'u Yapılandır](./media/confluencemicrosoft-tutorial/reverseproxy1.png)

    b. **Sistem Ayarları'ndaki** **Temel URL'yi** proxy/yük bakiyesine göre değiştirin.

    ![Tek İşaret-On'u Yapılandır](./media/confluencemicrosoft-tutorial/reverseproxy2.png)

1. Eklenti yüklendikten sonra Eklentiyi **Yönet** bölümünün **Kullanıcı Yüklü** eklentileri bölümünde görünür. Yeni **Configure** eklentiyi yapılandırmak için Yapıya'yı tıklatın.

    ![Tek İşaret-On'u Yapılandır](./media/confluencemicrosoft-tutorial/addon15.png)

1. Yapılandırma sayfasında aşağıdaki adımları gerçekleştirin:

    ![Tek İşaret-On'u Yapılandır](./media/confluencemicrosoft-tutorial/addon54.png)

    > [!TIP]
    > Meta verilerin çözümünde hata olmaması için uygulamaya karşı eşlenen tek bir sertifika olduğundan emin olun. Birden çok sertifika varsa, yönetici meta verileri çözerek bir hata alır.

    1. **Metadata URL** metin kutusunda, Azure portalından kopyaladığınız **Uygulama Federasyonu Metadata Url** değerini yapıştırın ve **Çözümle** düğmesini tıklatın. IdP meta veri URL'sini okur ve tüm alan bilgilerini doldurur.

    1. **Identifier, Yanıt URL'sini ve URL değerlerine** Imzala'yı kopyalayın ve Azure portalındaki Temel **SAML Yapılandırması** bölümünde **sırasıyla Identifier, YanıtURL ve URL** metin kutularında oturum aç'a yapıştırın.

    1. **Giriş Düğmesi Adı'nda** kuruluşunuzun kullanıcıların giriş ekranında görmesini istediği düğmenin adını yazın.
    
    1. **Giriş Düğmesi Açıklaması'nda** kuruluşunuzun kullanıcıların giriş ekranında görmesini istediği düğmenin açıklamasını yazın.

    1. **SAML Kullanıcı Kimliği Konumları'nda,** **Kullanıcı Kimliği'nin Konu deyiminin NameIdentifier öğesinde olduğunu** veya Kullanıcı **Kimliğinin bir Öznitelik öğesinde olduğunu**seçin.  Bu kimlik Confluence kullanıcı kimliği olmalıdır. Kullanıcı kimliği eşleşmiyorsa, sistem kullanıcıların oturum açmasına izin vermez. 

       > [!Note]
       > Varsayılan SAML Kullanıcı Kimliği konumu Ad Tanımlayıcı'dır. Bunu bir öznitelik seçeneğiolarak değiştirebilir ve uygun öznitelik adını girebilirsiniz.

    1. Kullanıcı Kimliği'ni seçerseniz **Bir Öznitelik öğesi seçeneğindeyse,** **Atnitelik adı** textbox'ta Kullanıcı Kimliğinin beklendiği özniteliğin adını yazın. 

    1. Azure AD ile federe etki alanını (ADFS vb.) kullanıyorsanız, **Ana Sayfa Alanı Bulma** seçeneğini tıklatın ve Etki Alanı **Adını**yapılandırın.

    1. **Alan** Adı'nda, ADFS tabanlı oturum açma durumunda alan adını buraya yazın.

    1. Bir kullanıcı Confluence'dan oturum açtığında Azure AD'den oturum u imzalamak istiyorsanız Tek Oturum Aç'ı **Etkinleştir'i** işaretleyin. 

    1. Yalnızca Azure AD kimlik bilgileri arasında oturum açmak **istiyorsanız, Azure Giriş** Kutusunu Zorla'yı etkinleştirin.

       > [!Note]
       > Force azure girişi etkinleştirildiğinde giriş sayfasında yönetici girişi için varsayılan giriş formunu etkinleştirmek için tarayıcı URL'sine sorgu parametresini ekleyin.
       > `https://<domain:port>/login.action?force_azure_login=false`

    1. Ayarları kaydetmek için **Kaydet** düğmesini tıklatın.

       > [!NOTE]
       > Yükleme ve sorun giderme hakkında daha fazla bilgi için [MS Confluence SSO Bağlayıcısı Yönetici Kılavuzu'nu ziyaret edin.](../ms-confluence-jira-plugin-adminguide.md) Ayrıca yardım için bir [SSS](../ms-confluence-jira-plugin-faq.md) vardır.

### <a name="create-confluence-saml-sso-by-microsoft-test-user"></a>Microsoft test kullanıcısı tarafından Confluence SAML SSO oluşturma

Azure AD kullanıcılarının Confluence şirket içi sunucuda oturum açmalarını sağlamak için, bunların Microsoft tarafından Birleştirme SAML SSO'su olarak sağlanması gerekir. Microsoft tarafından Confluence SAML SSO için, sağlama el ile bir görevdir.

**Bir kullanıcı hesabı sağlamak için aşağıdaki adımları gerçekleştirin:**

1. Confluence şirket içi sunucunuzda yönetici olarak oturum açın.

1. Dişli üzerinde hover ve **Kullanıcı yönetimi**tıklayın.

    ![Çalışan Ekle](./media/confluencemicrosoft-tutorial/user1.png)

1. Kullanıcılar bölümünün **altında, kullanıcı ekle** sekmesini tıklatın. Kullanıcı **Ekle** iletişim sayfasında aşağıdaki adımları gerçekleştirin:

    ![Çalışan Ekle](./media/confluencemicrosoft-tutorial/user2.png)

    a. Kullanıcı **adı** metin kutusuna, B.Simon gibi kullanıcının e-postasını yazın.

    b. Tam **Ad** metin kutusuna, B.Simon gibi kullanıcının tam adını yazın.

    c. **E-posta** metin kutusuna, kullanıcının B.Simon@contoso.come-posta adresini yazın.

    d. **Parola** metin kutusuna, B.Simon'ın parolasını yazın.

    e. **Parolayı Onayla'yı** tıklatın.

    f. **Ekle** düğmesini tıklatın.

## <a name="test-sso"></a>Test SSO

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'nde Microsoft döşemesi tarafından confluence SAML SSO'yu tıklattığınızda, SSO'yu kurduğunuz Microsoft tarafından otomatik olarak Confluence SAML SSO'da oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve tek oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

- [Azure Active Directory'de koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure AD ile Microsoft tarafından SAML SSO'yu Birleştirme'yi deneyin](https://aad.portal.azure.com/)
