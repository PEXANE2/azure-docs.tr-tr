---
title: 'Öğretici: SharePoint şirket içi azure Active Directory entegrasyonu | Microsoft Dokümanlar'
description: Azure Active Directory ve SharePoint şirket içinde tek oturum açma yı nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 85b8d4d0-3f6a-4913-b9d3-8cc327d8280d
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 01/15/2020
ms.author: miguego
ms.openlocfilehash: 7e47891a74feb60b0f3e4594bd1621e8b62d64d1
ms.sourcegitcommit: af1cbaaa4f0faa53f91fbde4d6009ffb7662f7eb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81867000"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharepoint-on-premises"></a>Öğretici: SharePoint şirket içi azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SharePoint'i azure etkin diziniyle (Azure AD) şirket içi nasıl entegre acağınızı öğreneceksiniz. SharePoint'i şirket içi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Şirket içinde SharePoint erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla şirket içinde SharePoint'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Önkoşullar

Azure Active Directory tümleştirmesini Şirket içinde SharePoint ile yapılandırmak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure Active Directory aboneliği. Azure REKLAM ortamınız yoksa, ücretsiz bir [hesap](https://azure.microsoft.com/free/) alabilirsiniz
* Bir SharePoint 2013 çiftlik veya yeni.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure Active Directory'yi bir test ortamında tek oturum açma işlemlerini yapılandırır ve sınarsınız. Azure Active Directory'si kullanıcıları Sharepoint Şirket Başına erişebilir.

## <a name="create-the-enterprise-applications-in-azure-portal"></a>Azure portalında Kurumsal Uygulamalar oluşturma

SharePoint'in şirket içi tümleştirmesini Azure AD'ye yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize SharePoint'i şirket içinde eklemeniz gerekir.

Galeriden SharePoint'i şirket içinde eklemek için aşağıdaki adımları gerçekleştirin:

1. Sol daki gezinti panelindeki **[Azure portalında](https://portal.azure.com)** **Azure Active Directory simgesini** tıklatın.

 > [!NOTE]
 > Öğe kullanılamıyorsa, sol daki gezinti panelinin üst kısmındaki sabit **Tüm hizmetler** bağlantısı üzerinden de açılabilir. Aşağıdaki genel bakışta, **Azure Etkin Dizin** bağlantısı **Kimlik** bölümünde bulunur veya filtre metin kutusunu kullanarak aranabilir.

2. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamalar** seçeneğini belirleyin.

3. Yeni uygulama eklemek için iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesini tıklatın.

4. Arama kutusunda, **sharepoint şirket içi**yazın, sonuç panelinden **SharePoint'i şirket içinde** seçin.

    <kbd>![Sonuç listesinde ki SharePoint şirket içi](./media/sharepoint-on-premises-tutorial/search-new-app.png)</kbd>

1. SharePoint OnPrem için bir ad belirtin ve uygulamayı eklemek için **Ekle** düğmesini tıklatın.

1. Yeni Kurumsal Uygulama'da Özellikler'e tıklayın ve **Gerekli Kullanıcı atamasının** değerini kontrol edin

   <kbd>![Sonuç listesinde ki SharePoint şirket içi](./media/sharepoint-on-premises-tutorial/user-assignment-required.png)</kbd>

bizim senaryomuzda bu değer **No**olarak ayarlanır.

## <a name="configure-and-test-azure-ad"></a>Azure AD'yi yapılandırma ve test

Bu bölümde, SharePoint şirket içi Azure AD oturum açma işlemlerini yapılandırırsınız.
Tek oturum açma nın işe yaraması için, bir Azure AD kullanıcısı ile SharePoint'teki ilgili kullanıcı arasında bir bağlantı ilişkisinin kurulması gerekir.

Azure Active Directory'yi şirket içinde SharePoint ile tek oturum açma işlemlerini yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlamanız gerekir:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
1. **[SharePoint'i şirket içinde yapılandırın](#configure-sharepoint-on-premises)** - uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için.
1. **[Azure portalında bir Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user-in-the-azure-portal)** - tek oturum açma için Azure AD'de yeni bir kullanıcı oluşturun.
1. **[Azure portalında bir Azure AD Güvenlik Grubu oluşturun](#create-an-azure-ad-security-group-in-the-azure-portal)** ve tek oturum açma için Azure AD'de yeni bir güvenlik grubu oluşturun.
1. **[SharePoint'teki Azure Active Directory Account'a şirket içi izin verin](#grant-permissions-to-azure-active-directory-account-in-sharepoint-on-premises)** - Azure AD kullanıcısına izin verin.
1. **[SharePoint'teki Azure REKLAM grubuna izin verin](#grant-permissions-to-azure-ad-group-in-sharepoint-on-premises)** ve Azure REKLAM grubuna izin verin.
1. **[Azure portalında şirket içi SharePoint için Konuk Hesabı'na erişim izni verin](#grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal)** - SharePoint şirket içi için Azure AD'deki Konuk hesabına izin verin.
1. **[Birden çok web uygulaması için güvenilir kimlik sağlayıcısını yapılandırma](#configuring-the-trusted-identity-provider-for-multiple-web-applications)** - birden çok web uygulaması için aynı güvenilir kimlik sağlayıcısının nasıl kullanılacağı

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Şirket içinde SharePoint ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında](https://portal.azure.com/)Azure AD dizinini açın, **Kurumsal uygulamalara**tıklayın, **önceden oluşturulmuş Kurumsal Uygulama adını** tıklatın ve Tek oturum **açma'ya**tıklayın.

2. Tek **oturum açma yöntemi** iletişim kutusunu seç'te, tek oturum açmayı etkinleştirmek için **SAML** moduna tıklayın.
 
3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SharePoint şirket içi Etki Alanı ve URL'ler tek oturum açma bilgileri](./media/sharepoint-on-premises-tutorial/sp-identifier-reply.png)

    1. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`urn:<sharepointFarmName>:<federationName>`

    1. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<YourSharePointSiteURL>/_trust/`

    1. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<YourSharePointSiteURL>/`
    1. kaydet'e tıklayın.

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](./media/sharepoint-on-premises-tutorial/certificatebase64.png)

6. **SharePoint'i şirket içi ayarlama** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın.
    
    1. **Giriş URL'si**
    
        Copy giriş URL replace **/saml2** sonunda **/wsfed**ile **https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed** , bu gibi görünür (bu url doğru değil)

    1. **Azure AD Tanımlayıcısı**
    1. **Giriş URL'si**
    > [!NOTE]
    > Bu URL SharePoint'te olduğu gibi kullanılamaz: **/saml2'yi** **/wsfed**ile değiştirmeniz gerekir. Sharepoint Şirket İçi uygulaması SAML 1.1 belirteci kullanır, bu nedenle Azure AD SharePoint sunucusundan WS Fed isteği bekler ve kimlik doğrulamadan sonra SAML 1.1'i yayınlar. Belirte -ci.

### <a name="configure-sharepoint-on-premises"></a>SharePoint'i şirket içinde yapılandır

1. **SharePoint Server 2016'da yeni bir güvenilir kimlik sağlayıcısı oluşturun**

    SharePoint sunucusunda oturum açın ve SharePoint yönetim Kabuğu'nu açın. Değerleri doldurun:
    1. **$realm** (Azure portalındaki SharePoint şirket içi Etki Alanı ve URL'ler bölümünden tanımlayıcı değer).
    1. **$wsfedurl** (Tek Oturum Açma Hizmeti URL'si).
   1. Azure portalından **$filepath** (sertifika dosyasını indirdiğiniz dosya yolu).

    Yeni bir güvenilen kimlik sağlayıcısını yapılandırmak için aşağıdaki komutları çalıştırın.

    > [!TIP]
    > PowerShell'i kullanmaya yeni başladıysanız veya PowerShell'in nasıl çalıştığı hakkında daha fazla bilgi edinmek istiyorsanız, [Bkz. SharePoint PowerShell.](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps)


    ```
    $realm = "urn:sharepoint:sps201x"
    $wsfedurl="https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed"
    $filepath="C:\temp\SharePoint 2019 OnPrem.cer"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map1 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "Azure AD SharePoint server 201x" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map1,$map2 -SignInUrl $wsfedurl -IdentifierClaim $map1.InputClaimType
    ```
1. **Uygulamanız için güvenilir kimlik sağlayıcısını etkinleştirme**

    a. Merkezi Yönetim'de Web **Uygulamasını Yönet'e** gidin ve Azure AD ile güvenli hale getirmek istediğiniz web uygulamasını seçin.

    b. Şeritte, **Kimlik Doğrulama Sağlayıcıları'nı** tıklatın ve kullanmak istediğiniz bölgeyi seçin.

    c. **Güvenilir Kimlik sağlayıcısını** seçin ve *AzureAD*adında yeni kaydolduğunuz kimlik sağlayıcınızı seçin.

    d. **Tamam**'a tıklayın.

    ![Kimlik doğrulama sağlayıcınızı yapılandırma](./media/sharepoint-on-premises-tutorial/config-auth-provider.png)

### <a name="create-an-azure-ad-test-user-in-the-azure-portal"></a>Azure portalında bir Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede, **Yönet** pan'da **Azure Etkin Dizini'ni** **seçin.**

2. Ardından ekranın üst kısmında **Yeni kullanıcı** tarafından takip edilen **tüm kullanıcıları** seçin.

3. **Kullanıcı Oluştur** seçeneğini seçin ve Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.  
   Azure REKLAM'ınızda kiracı sonekinizi veya doğrulanmış herhangi bir etki alanını kullanarak kullanıcılar oluşturabilirsiniz. 

    a. **Ad** alanında kullanıcı adını girin , biz **TestUser**kullanılır.
  
    b. Kullanıcı **adı** alanı türünde`TestUser@yourcompanydomain.extension`  
    Örneğin, TestUser@contoso.com

    ![Kullanıcı iletişim kutusu](./media/sharepoint-on-premises-tutorial/user-properties.png)

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

    e. Artık siteyi paylaşabilir TestUser@contoso.com ve bu kullanıcının siteye erişmesine izin verebilirsiniz.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Azure portalında Bir Azure REKLAM Güvenlik Grubu oluşturma

1. Azure **Active Directory > Grupları'na**tıklayın.

2. **Yeni grubu**tıklatın:

3. Grup **türünü,** **Grup adını,** **Grup açıklamasını,** **Üyelik türünü**doldurun. Üyeleri seçmek için oka tıklayın, sonra arayabilirsiniz veya gruba eklemek istediğiniz üyeyi tıklatın. Seçili üyeleri eklemek için **Seç'e** tıklayın ve ardından **Oluştur'a**tıklayın.

![Azure AD Güvenlik Grubu Oluşturma](./media/sharepoint-on-premises-tutorial/new-group.png)

### <a name="grant-permissions-to-azure-active-directory-account-in-sharepoint-on-premises"></a>SharePoint'teki Azure Active Directory hesabına şirket içi izin verme

Şirket içi SharePoint'teki Azure Etkin Dizin Kullanıcısına erişim izni vermek için site koleksiyonunu paylaşmanız veya Azure Etkin Dizin Kullanıcısını site koleksiyonunun grubuna eklemeniz gerekir. Kullanıcılar artık Azure AD'nin kimliklerini kullanarak SharePoint 201x'e giriş yapabilir, ancak kullanıcı deneyimini iyileştirme fırsatları hala vardır. Örneğin, bir kullanıcıyı aramak, kişi seçicide birden çok arama sonucu sunar. Talep eşlemesinde oluşturulan talep türlerinin her biri için bir arama sonucu vardır. Kişi seçiciyi kullanarak bir kullanıcı seçmek için, kullanıcı adını tam olarak yazmanız ve **ad** talebi sonucunu seçmeniz gerekir.

![Talep arama sonuçları](./media/sharepoint-on-premises-tutorial/claims-search-results.png)

Aradığınız değerlerde, yazım hatalarına veya kullanıcıların yanlışlıkla yanlış talep türünü seçmesine yol açabilecek bir doğrulama yoktur. Bu, kullanıcıların kaynaklara başarıyla erişmesini engelleyebilir.

Bu senaryoyla **kişi seçiciyi düzeltmek** için, SharePoint 2013, 2016 ve 2019 için özel bir talep sağlayıcısı sağlayan [AzureCP](https://yvand.github.io/AzureCP/) adlı açık kaynak çözümü vardır. Kullanıcıların neleri gireceğini ve doğrulamayı gerçekleştirdiğini çözmek için Microsoft Graph API'sını kullanır. [AzureCP'de](https://yvand.github.io/AzureCP/)daha fazla bilgi edinin.

  > [!NOTE]
  > AzureCP olmadan Azure REKLAM grubunun kimliğini ekleyerek Gruplar ekleyebilirsiniz, ancak bu kullanıcının dostu ve güvenilir değildir. İşte nasıl görünüyor:  
  >   
  >![Azure REKLAM grubunu Sharepoint Group'a ekleme](./media/sharepoint-on-premises-tutorial/adding-group-by-id.png)
  
### <a name="grant-permissions-to-azure-ad-group-in-sharepoint-on-premises"></a>SharePoint'teki Azure REKLAM grubuna şirket içinde izin verme

Azure Active Directory Security Groups'u SharePoint'e şirket içinde atamak için SharePoint Server için özel bir talep sağlayıcısı kullanmanız gerekir. Örneğimizde AzureCP'yi kullandık.

 > [!NOTE]
 > AzureCP'nin bir Microsoft ürünü olmadığını veya Microsoft Teknik Destek tarafından desteklenmediğini lütfen unutmayın. AzureCP'yi şirket içi SharePoint çiftliğinde indirin, yükleyin ve https://yvand.github.io/AzureCP/yapılandırın. 

1. AzureCP'yi SharePoint şirket içi çiftlikte veya alternatif bir özel talep sağlayıcısı çözümünde yapılandırın. AzureCP yapılandırmasının adımlarıhttps://yvand.github.io/AzureCP/Register-App-In-AAD.html

1. Azure portalında Azure REKLAM dizinini açın. Kurumsal **uygulamalara**tıklayın , **daha önce oluşturulmuş Kurumsal Uygulama adını** tıklayın ve Tek oturum **açma'ya**tıklayın.

1. Sayfada **SAM ile Tek Oturum Açma,** Kullanıcı **Öznitelikleri & Talepler** bölümünü güncelleyin.

1. Grup **talebi ekle'ye**tıklayın.

1. Talepte kullanıcıyla ilişkili grupların döndürülmesi gerektiğini seçin, bizim durumumuzda **tüm grupları** seçin ve Kaynak öznitelik bölümünde **Grup Kimliği'ni** seçin ve **Kaydet'e**tıklayın.

Şirket içi SharePoint'te Azure Active Directory Security Group'a erişim sağlamak için site koleksiyonunu paylaşmanız veya Azure Etkin Dizin Güvenlik Grubu'nu site koleksiyonunun grubuna eklemeniz gerekir.

1. Site Koleksiyonu için Site Ayarları altında, SharePoint Site Koleksiyonu'na göz atın, "Kişiler ve gruplar"ı tıklayın. SharePoint grubunu seçin ve Yeni,"Bu Gruba Kullanıcı Ekle"yi tıklayın ve grubunuzun adını Yazmaya başlayın Kişi Seçici Azure Etkin Dizin Güvenlik Grubu'nu görüntüleyecek.

    ![Azure REKLAM grubunu Sharepoint Group'a ekleme](./media/sharepoint-on-premises-tutorial/permission-azure-ad-group.png)

### <a name="grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal"></a>Azure portalında şirket içinde SharePoint'e Konuk hesabına erişim izni verme

sharepoint sitenize sürekli olarak Konuk Hesabına erişim izni vermek artık mümkün. BU UPN değiştirilir olur. Kullanıcı jdoe@outlook.com gibi `jdoe_outlook.com#ext#@TENANT.onmicrosoft.com`temsil edilecektir. Sitenizi harici kullanıcılarla paylaşırken sorunsuz bir deneyim elde etmek mümkündür, Azure portalındaki **Kullanıcı Özellikleri & İddialar** bölümünde bazı değişiklikler eklemek gerekir.

1. Azure portalında Azure REKLAM dizinini açın. Kurumsal **uygulamalara**tıklayın , **daha önce oluşturulmuş Kurumsal Uygulama adını** tıklayın ve Tek oturum **açma'ya**tıklayın.

1. Sayfada **SAM ile Tek Oturum Açma,** Kullanıcı **Öznitelikleri & Talepler** bölümünü güncelleyin.

1. Gerekli **talep** bölgesinde **Benzersiz Kullanıcı Tanımlayıcısı'nı (Ad Kimliği)** tıklayın.

1. Kaynak **Öznitelik** Özelliğini value **user.localuserprincipalname** ile değiştirin ve **kaydedin.**

    ![Kullanıcı Öznitelikleri & Talepleri ilk](./media/sharepoint-on-premises-tutorial/manage-claim.png)

1. Şerit kullanarak **SAML tabanlı Oturum açma'ya** geri dön, şimdi **Kullanıcı Öznitelikleri & Talepler** bölümü şuna benzer: 

    ![Kullanıcı Öznitelikleri & Talepler son](./media/sharepoint-on-premises-tutorial/user-attributes-claims-final.png)  

    > [!NOTE]
    > Bu kurulumda soyadı ve verilen ad gerekli değildir.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin.

1. yeni **konuk kullanıcı** tıklayın

1. **Kullanıcıdavet** seçeneğini seçin ve Kullanıcının özelliklerini doldurun ve **Davet'i**tıklatın.

1. Artık siteyi paylaşabilir MyGuestAccount@outlook.com ve bu kullanıcının siteye erişmesine izin verebilirsiniz.

    ![Konuk Hesabı ile Site Paylaşımı](./media/sharepoint-on-premises-tutorial/sharing-guest-account.png)

### <a name="configuring-the-trusted-identity-provider-for-multiple-web-applications"></a>Birden çok web uygulaması için güvenilir kimlik sağlayıcısını yapılandırma

Yapılandırma tek bir web uygulaması için çalışır, ancak birden çok web uygulaması için aynı güvenilir kimlik sağlayıcısını kullanmak istiyorsanız ek yapılandırma gerekir. Örneğin, URL'yi `https://sales.contoso.com` kullanmak için bir web uygulamasını genişletdiğimizi ve şimdi `https://marketing.contoso.com` de kullanıcıların kimliğini doğrulamak istediğimizi varsayalım. Bunu yapmak için, WReply parametresini onurlandırmak için kimlik sağlayıcısını güncelleştirmemiz ve yanıt URL'si eklemek için Azure AD'deki uygulama kaydını güncelleştirmemiz gerekir.

1. Azure portalında Azure REKLAM dizinini açın. Kurumsal **uygulamalara**tıklayın , **daha önce oluşturulmuş Kurumsal Uygulama adını** tıklayın ve Tek oturum **açma'ya**tıklayın.

2. Sayfada SAM ile Tek Oturum Açma'yı **ayarlayın,** **Temel SAML Yapılandırmasını**düzenlemeyi ayarlayın.

    ![temel SAML Yapılandırması'nı düzenleme](./media/sharepoint-on-premises-tutorial/add-reply-url.png)

3. **Yanıt URL'sinde (Tüketici Hizmeti URL'sini İddia Et)** ek web uygulamaları için URL ekleyin ve **Kaydet'i**tıklatın.

    ![temel SAML Yapılandırması'nı düzenleme](./media/sharepoint-on-premises-tutorial/reply-url-for-web-application.png)

4. SharePoint sunucusunda, **SharePoint 201x Management Shell'i** açın ve daha önce kullandığınız güvenilir kimlik belirteci verenin adını kullanarak aşağıdaki komutları çalıştırın.
    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
5. Merkezi Yönetim'de, web uygulamasına gidin ve varolan güvenilir kimlik sağlayıcısını etkinleştirin.

Dahili kullanıcılarınız için Şirket İçi Paylaşım Noktanıza erişim vermek istediğiniz başka senaryonuz da olabilir, bu senaryo için Şirket içi kullanıcılarınızı Azure Etkin Diziniyle senkronize etmeye izin verecek Microsoft Azure Active Directory Connect'i dağıtmanız gerekir, bu kurulum başka bir makalenin parçası olacaktır. 

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure Active Directory ile karma kimlik nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
