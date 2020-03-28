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
ms.date: 03/19/2020
ms.author: jeedes
ms.openlocfilehash: 22b673ef481593247b6ee1007c13390a498c66be
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80048626"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharepoint-on-premises"></a>Öğretici: SharePoint şirket içi azure Active Directory tek oturum açma (SSO) entegrasyonu

Bu eğitimde, SharePoint'i azure etkin diziniyle (Azure AD) şirket içi nasıl entegre acağınızı öğreneceksiniz. SharePoint'i şirket içi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Şirket içinde SharePoint erişimi olan Azure AD'de denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla şirket içinde SharePoint'te otomatik olarak oturum açabilmelerini etkinleştirin.
* Hesaplarınızı tek bir merkezi konumda yönetin - Azure portalı.

Azure AD ile SaaS uygulama tümleştirmesi hakkında daha fazla bilgi edinmek için Azure [Active Directory ile uygulama erişimi ve tek oturum açma nedir'e](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)bakın.

## <a name="prerequisites"></a>Ön koşullar

Başlamak için aşağıdaki öğelere ihtiyacınız vardır:

* Azure AD aboneliği. Aboneliğiniz [yoksa, ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
* SharePoint şirket içi tek oturum açma (SSO) aboneliğini etkinleştirildi.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu eğitimde, Azure AD SSO'su bir test ortamında yapılandırın ve test esiniz.

* SharePoint şirket içi **SP** başlatılan SSO destekler
* SharePoint'i şirket içinde yapılandırdıktan sonra, kuruluşunuzun hassas verilerinin gerçek zamanlı olarak sızma ve sızmalarını koruyan oturum denetimlerini uygulayabilirsiniz. Oturum denetimi Koşullu Erişim'den genişletir. [Microsoft Cloud App Security ile oturum denetimini nasıl uygulayacağınızı öğrenin.](https://docs.microsoft.com/cloud-app-security/proxy-deployment-any-app)
* Kullanıcı Profillerini SharePoint Şirket İçinden Azure AD'ye nasıl senkronize edilenöğrenmek için lütfen bu [Bağlantıya](https://docs.microsoft.com/archive/blogs/kaevans/sharepoint-2013-user-profile-sync-for-claims-users) bakın

## <a name="adding-sharepoint-on-premises-from-the-gallery"></a>Galeriden SharePoint şirket içi ekleme

SharePoint'in şirket içi tümleştirmesini Azure AD'ye yapılandırmak için, galeriden yönetilen SaaS uygulamaları listenize SharePoint'i şirket içinde eklemeniz gerekir.

1. Azure [portalında](https://portal.azure.com) bir iş veya okul hesabını veya kişisel bir Microsoft hesabını kullanarak oturum açın.
1. Sol gezinti bölmesinde **Azure Etkin Dizin** hizmetini seçin.

    > [!NOTE]
    > Öğe kullanılamıyorsa, sol daki gezinti panelinin üst kısmındaki sabit **Tüm hizmetler** bağlantısı üzerinden de açılabilir. Aşağıdaki genel bakışta, **Azure Etkin Dizin** bağlantısı **Kimlik** bölümünde bulunur veya filtre metin kutusunu kullanarak aranabilir.

1. Kurumsal **Uygulamalar'a** gidin ve ardından **Tüm Uygulamaları**seçin.
1. Yeni uygulama eklemek için **Yeni uygulama'yı**seçin.
1. **Galeriden Ekle** bölümünde, arama kutusuna **şirket içi SharePoint** yazın.
1. Sonuç panelinden **şirket içinde SharePoint'i** seçin ve ardından uygulamayı ekleyin. Uygulama kiracınıza eklenirken birkaç saniye bekleyin.

## <a name="configure-and-test-azure-ad-single-sign-on-for-sharepoint-on-premises"></a>Şirket içinde SharePoint için Azure AD oturum açma işlemlerini yapılandırma ve test edin

Azure AD SSO'nu **B.Simon**adlı bir test kullanıcısı kullanarak sharepoint şirket içinde yapılandırın ve test edin. SSO'nun çalışması için, bir Azure REKLAM kullanıcısı ile SharePoint'teki ilgili kullanıcı arasında bir bağlantı ilişkisi kurmanız gerekir.

Azure AD SSO'yu şirket içinde SharePoint ile yapılandırmak ve test etmek için aşağıdaki yapı taşlarını tamamlayın:

1. Kullanıcılarınızın bu özelliği kullanmasını sağlamak için Azure AD Tek Oturum Açma'yı **[yapılandırın.](#configure-azure-ad-single-sign-on)**
2. **[SharePoint'i şirket içi Tek Oturum](#configure-sharepoint-on-premises-single-sign-on)** Açma'yı uygulama tarafındaki Tek Oturum Açma ayarlarını yapılandırmak için yapılandırın.
3. Azure AD tek oturum açma işlemini Britta Simon ile test etmek için **[bir Azure AD test kullanıcısı oluşturun.](#create-an-azure-ad-test-user)**
4. **[Azure portalında tek oturum](#create-an-azure-ad-security-group-in-the-azure-portal)** açma için Azure AD'de yeni bir güvenlik grubu etkinleştirmek için bir Azure REKLAM Güvenlik Grubu oluşturun.
5. **[SharePoint şirket içi Güvenlik Grubu'na erişim izni ver](#grant-access-to-sharepoint-on-premises-security-group)** , belirli bir grup için Azure AD'ye erişim tanıyın.
6. Azure **[portalındaki Azure AD Güvenlik Grubu'nu atayın](#assign-the-azure-ad-security-group-in-the-azure-portal)** ve belirli grubu kimlik doğrulaması için Azure AD'ye atayın.
7. **[Yapılandırmanın](#test-single-sign-on)** çalışıp çalışmadığını doğrulamak için tek oturum açma testi yapın.

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD'yi tek oturum açma yapılandırma

Bu bölümde, Azure portalında Azure AD oturum açma'yı etkinleştirin.

Şirket içinde SharePoint ile Azure AD oturum açma işlemlerini yapılandırmak için aşağıdaki adımları gerçekleştirin:

1. Azure [portalında,](https://portal.azure.com/) **SharePoint şirket içi** uygulama tümleştirme sayfasında **Tek oturum açma'yı**seçin.

    ![Tek oturum açma bağlantısını yapılandırma](common/select-sso.png)

2. Tek **oturum açma yöntemi** iletişim kutusunda, tek oturum açmayı etkinleştirmek için **SAML/WS-Fed** modunu seçin.

    ![Tek oturum açma seçme modu](common/select-saml-option.png)

3. **SAML sayfasıyla Tek Oturum Açma'da** **Temel SAML Yapılandırma** iletişim kutusunu açmak için **Düzenleme** simgesini tıklatın.

    ![Temel SAML Yapılandırması'nı düzenleme](common/edit-urls.png)

4. Temel **SAML Yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:


    a. Oturum **Açma URL** metin kutusuna aşağıdaki deseni kullanarak bir URL yazın:`https://<YourSharePointServerURL>/_trust/default.aspx`

    b. **Tanımlayıcı** kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`urn:sharepoint:federation`

    c. **Yanıtla URL** metin kutusuna, aşağıdaki deseni kullanarak bir URL yazın:`https://<YourSharePointServerURL>/_trust/default.aspx`

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek Oturum Açma URL'si, Tanımlayıcı ve Yanıt URL'si ile güncelleştirin. Bu değerleri almak için [SharePoint şirket içi İstemci destek ekibine](https://support.office.com/) başvurun. Azure portalındaki **Temel SAML Yapılandırması** bölümünde gösterilen desenlere de bakabilirsiniz.

5. **SAML ile Tek Oturum Açma** sayfasında, **SAML İmza Sertifikası** bölümünde, sertifikayı **(Base64)** gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir'i** tıklatın.

    ![Sertifika indirme bağlantısı](common/certificatebase64.png)

    > [!Note]
    > Yapılandırma için PowerShell komut dosyasında daha sonra kullanmanız gerektiğinden, sertifika dosyasını indirdiğiniz dosya yolunu lütfen not edin.

6. **SharePoint'i şirket içi ayarlama** bölümünde, gereksiniminize göre uygun URL'yi kopyalayın. **Tek Oturum Açma Hizmeti URL'si için**aşağıdaki desenin değerini kullanın:`https://login.microsoftonline.com/_my_directory_id_/wsfed`

    > [!Note]
    > _my_directory_id,_ Azure Reklam aboneliğinin kiracı kimliğidir.

    ![Yapılandırma URL'lerini kopyalama](common/copy-configuration-urls.png)

    a. Oturum Açma URL’si

    b. Azure AD Tanımlayıcısı

    c. Giriş URL'si

    > [!NOTE]
    > Sharepoint Şirket İçi uygulaması SAML 1.1 belirteci kullanır, bu nedenle Azure AD SharePoint sunucusundan WS Fed isteği bekler ve kimlik doğrulamadan sonra SAML 1.1'i yayınlar. Belirte -ci.

### <a name="configure-sharepoint-on-premises-single-sign-on"></a>SharePoint'i şirket içi Tek Oturum Açma'yı yapılandır

1. Farklı bir web tarayıcısı penceresinde, SharePoint şirket içi şirket sitenizde yönetici olarak oturum açın.

2. **SharePoint Server 2016'da yeni bir güvenilir kimlik sağlayıcısı nı yapılandırma**

    SharePoint Server 2016 sunucusunda oturum açın ve SharePoint 2016 Management Shell'i açın. Azure portalındaki $realm (SharePoint şirket içi Etki Alanı ve URL'ler bölümünden tanımlayıcı değer), $wsfedurl (Tek Oturum Açma Hizmeti URL'si) ve $filepath (sertifika dosyasını indirdiğiniz dosya yolu) değerlerini Azure portalından doldurun ve çalıştırın yeni bir güvenilir kimlik sağlayıcısı yapılandırmak için aşağıdaki komutları.

    > [!TIP]
    > PowerShell'i kullanmaya yeni başladıysanız veya PowerShell'in nasıl çalıştığı hakkında daha fazla bilgi edinmek istiyorsanız, [Bkz. SharePoint PowerShell.](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps)

    ```
    $realm = "<Identifier value from the SharePoint on-premises Domain and URLs section in the Azure portal>"
    $wsfedurl="<SAML single sign-on service URL value which you have copied from the Azure portal>"
    $filepath="<Full path to SAML signing certificate file which you have downloaded from the Azure portal>"
    $cert = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2($filepath)
    New-SPTrustedRootAuthority -Name "AzureAD" -Certificate $cert
    $map = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" -IncomingClaimTypeDisplayName "name" -LocalClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
    $map2 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname" -IncomingClaimTypeDisplayName "GivenName" -SameAsIncoming
    $map3 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname" -IncomingClaimTypeDisplayName "SurName" -SameAsIncoming
    $map4 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress" -IncomingClaimTypeDisplayName "Email" -SameAsIncoming
    $map5 = New-SPClaimTypeMapping -IncomingClaimType "http://schemas.microsoft.com/ws/2008/06/identity/claims/role" -IncomingClaimTypeDisplayName "Role" -SameAsIncoming
    $ap = New-SPTrustedIdentityTokenIssuer -Name "AzureAD" -Description "SharePoint secured by Azure AD" -realm $realm -ImportTrustCertificate $cert -ClaimsMappings $map,$map2,$map3,$map4,$map5 -SignInUrl $wsfedurl -IdentifierClaim "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name"
    ```

    Ardından, uygulamanız için güvenilir kimlik sağlayıcısını etkinleştirmek için aşağıdaki adımları izleyin:

    a. Merkezi Yönetim'de Web **Uygulamasını Yönet'e** gidin ve Azure AD ile güvenli hale getirmek istediğiniz web uygulamasını seçin.

    b. Şeritte, **Kimlik Doğrulama Sağlayıcıları'nı** tıklatın ve kullanmak istediğiniz bölgeyi seçin.

    c. **Güvenilir Kimlik sağlayıcısını** seçin ve *AzureAD*adında yeni kaydolduğunuz kimlik sağlayıcınızı seçin.

    d. Oturum açma sayfası URL ayarında, **Özel oturum açma sayfasını** seçin ve "/_trust/" değerini sağlayın.

    e. **Tamam**'a tıklayın.

    ![Kimlik doğrulama sağlayıcınızı yapılandırma](./media/sharepoint-on-premises-tutorial/fig10-configauthprovider.png)

    > [!NOTE]
    > Bazı harici kullanıcılar, UPN'leri gibi `MYEMAIL_outlook.com#ext#@TENANT.onmicrosoft.com`ezilmiş değere sahip olacaklarından, bu tek oturum açma tümleştirmesini kullanamayacaklar. Yakında, müşterilerin kullanıcı türüne bağlı olarak UPN'yi nasıl işleyeceklerine ilişkin config uygulamasına izin veririz. Bundan sonra tüm konuk kullanıcılarınız SSO'yu organizasyon çalışanları olarak sorunsuz bir şekilde kullanabilmelidir.

### <a name="create-an-azure-ad-test-user"></a>Azure AD test kullanıcısı oluşturma

Bu bölümün amacı, Azure portalında Britta Simon adında bir test kullanıcısı oluşturmaktır.

1. Azure portalında, sol bölmede **Azure Etkin Dizini'ni**seçin, **Kullanıcılar'ı**seçin ve ardından **Tüm Kullanıcıları**seçin.

    !["Kullanıcılar ve gruplar" ve "Tüm kullanıcılar" bağlantıları](common/users.png)

2. Ekranın üst kısmında **Yeni kullanıcı** yı seçin.

    ![Yeni kullanıcı Düğmesi](common/new-user.png)

3. Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.

    ![Kullanıcı iletişim kutusu](common/user-properties.png)

    a. **Ad** alanında **BrittaSimon**girin.
  
    b. Kullanıcı **adı** alanı türünde`brittasimon@yourcompanydomain.extension`  
    Örneğin, BrittaSimon@contoso.com

    c. Parola onay kutusunu **göster'i** seçin ve ardından Parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur'u**tıklatın.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Azure portalında Bir Azure REKLAM Güvenlik Grubu oluşturma

1. Tüm **Gruplar > Azure Etkin Dizini'ni**tıklatın.

    ![Azure AD Güvenlik Grubu Oluşturma](./media/sharepoint-on-premises-tutorial/allgroups.png)

2. **Yeni grubu**tıklatın:

    ![Azure AD Güvenlik Grubu Oluşturma](./media/sharepoint-on-premises-tutorial/newgroup.png)

3. Grup **türünü,** **Grup adını,** **Grup açıklamasını,** **Üyelik türünü**doldurun. Üyeleri seçmek için oka tıklayın, sonra arayabilirsiniz veya gruba eklemek istediğiniz üyeyi tıklatın. Seçili üyeleri eklemek için **Seç'e** tıklayın ve ardından **Oluştur'a**tıklayın.

    ![Azure AD Güvenlik Grubu Oluşturma](./media/sharepoint-on-premises-tutorial/addingmembers.png)

    > [!NOTE]
    > Azure Active Directory Security Groups'u şirket içinde SharePoint'e atamak için, [AzureCP'yi](https://yvand.github.io/AzureCP/) şirket içi SharePoint çiftliğine yüklemek ve yapılandırmak veya SharePoint için alternatif bir özel talep sağlayıcısı geliştirmek ve yapılandırmak gerekir.  AzureCP kullanmıyorsanız, kendi özel talep sağlayıcınızı oluşturmak için belgenin sonundaki daha fazla bilgi bölümüne bakın.

### <a name="grant-access-to-sharepoint-on-premises-security-group"></a>SharePoint şirket içi Güvenlik Grubu'na erişim izni verme

**Uygulama Kaydında Güvenlik Gruplarını ve İzinleri Yapılandır**

1. Azure portalında **Azure Etkin Dizin'i**seçin ve ardından **Uygulama kayıtlarını**seçin.

    ![Kurumsal uygulamalar bıçak](./media/sharepoint-on-premises-tutorial/appregistrations.png)

2. Arama kutusuna şirket **içinde SharePoint**yazın ve seçin.

    ![Sonuç listesinde ki SharePoint şirket içi](./media/sharepoint-on-premises-tutorial/appsearch.png)

3. **Manifesto'ya**tıklayın.

    ![Bildirim seçeneği](./media/sharepoint-on-premises-tutorial/manifest.png)

4. Değiştir `groupMembershipClaims` `NULL`: `groupMembershipClaims`, `SecurityGroup`To : . Ardından Kaydet'e tıklayın

    ![Manifesto'yu Edit](./media/sharepoint-on-premises-tutorial/manifestedit.png)

5. **Ayarlar'a**tıklayın, ardından **Gerekli İzinler'e**tıklayın.

    ![Gerekli izinler](./media/sharepoint-on-premises-tutorial/settings.png)

6. **Ekle'ye** tıklayın ve ardından **bir API seçin.**

    ![API Erişimi](./media/sharepoint-on-premises-tutorial/required_permissions.png)

7. Hem **Windows Azure Etkin Dizini** hem de **Microsoft Graph API'sini**ekleyin, ancak aynı anda yalnızca bir tane seçmek mümkündür.

    ![API Seçin](./media/sharepoint-on-premises-tutorial/permissions.png)

8. Windows Azure Etkin Dizini'ni seçin, Okuma dizini verilerini denetleyin ve Seç'e tıklayın. Geri gidin ve Microsoft Graph ekleyin ve bunun için de dizin verilerini oku'nun yanı sıra' yı seçin.  Seç'e tıklayın ve Bitti'ye tıklayın.

    ![Erişimi Etkinleştirme](./media/sharepoint-on-premises-tutorial/readpermission.png)

9. Şimdi, Gerekli Ayarlar altında, **Hibe izinlerini** tıklatın ve ardından İzin vermek için Evet'i tıklatın.

    ![İzin Ver](./media/sharepoint-on-premises-tutorial/grantpermission.png)

    > [!NOTE]
    > İzinlerin başarıyla verilip verilmediğini belirlemek için bildirimlerin altında niçin onaylandığını denetleyin.  Değilse, AzureCP düzgün çalışmaz ve SharePoint'i Azure Etkin Dizin Güvenlik Grupları ile şirket içinde yapılandırmak mümkün olmayacaktır.

10. AzureCP'yi SharePoint şirket içi çiftlikte veya alternatif bir özel talep sağlayıcısı çözümünde yapılandırın.  Bu örnekte, AzureCP kullanıyoruz.

    > [!NOTE]
    > AzureCP'nin bir Microsoft ürünü olmadığını veya Microsoft Teknik Destek tarafından desteklenmediğini lütfen unutmayın. AzureCP'yi şirket içi SharePoint çiftliğinde her zaman indirin, yükleyin ve yapılandırın[https://yvand.github.io/AzureCP/](https://yvand.github.io/AzureCP/) 

11. **Şirket içi SharePoint'teki Azure Active Directory Security Group'a erişim izni verilmelidir** :- Gruplara SharePoint'teki uygulamaya şirket içi erişim izni verilmelidir.  Web uygulamasına erişmek için izinleri ayarlamak için aşağıdaki adımları kullanın.

12. Merkezi Yönetim'de, Uygulama Yönetimi'ne tıklayın, web uygulamalarını yönetin, ardından şeridi etkinleştirmek için web uygulamasını seçin ve Kullanıcı Politikası'na tıklayın.

    ![Merkezi Yönetim](./media/sharepoint-on-premises-tutorial/centraladministration.png)

13. Web Uygulaması İlkesi altında, Kullanıcı Ekle'yi tıklatın, ardından bölgeyi seçin, İleri'yi tıklatın.  Adres Defteri'ne tıklayın.

    ![Web uygulaması için ilke](./media/sharepoint-on-premises-tutorial/webapp-policy.png)

14. Ardından, Azure Etkin Dizin Güvenlik Grubu'nu arayın ve ekleyin ve Tamam'ı tıklatın.

    ![Güvenlik grubu ekleme](./media/sharepoint-on-premises-tutorial/securitygroup.png)

15. İzinleri seçin ve ardından Bitir'e tıklayın.

    ![Güvenlik grubu ekleme](./media/sharepoint-on-premises-tutorial/permissions1.png)

16. Bkz. Web Uygulaması İlkesi altında, Azure Etkin Dizin Grubu eklenir.  Grup talebi, Kullanıcı Adı için Azure Etkin Dizin Güvenlik Grubu Nesne Kimliğini gösterir.

    ![Güvenlik grubu ekleme](./media/sharepoint-on-premises-tutorial/addgroup.png)

17. SharePoint site koleksiyonuna göz atın ve Grubu da oraya ekleyin. Site Ayarları'na tıklayın, ardından Site İzinleri ve İzinler Ver'i tıklatın.  Grup Rolü iddiasını arayın, izin düzeyini atayın ve Paylaş'ı tıklatın.

    ![Güvenlik grubu ekleme](./media/sharepoint-on-premises-tutorial/grantpermission1.png)

### <a name="configuring-one-trusted-identity-provider-for-multiple-web-applications"></a>Birden çok web uygulaması için güvenilir bir kimlik sağlayıcısını yapılandırma

Yapılandırma tek bir web uygulaması için çalışır, ancak birden çok web uygulaması için aynı güvenilir kimlik sağlayıcısını kullanmak istiyorsanız ek yapılandırma gerekir. Örneğin, URL'yi `https://portal.contoso.local` kullanmak için bir web uygulamasını genişletdiğimizi ve şimdi `https://sales.contoso.local` de kullanıcıların kimliğini doğrulamak istediğimizi varsayalım. Bunu yapmak için, WReply parametresini onurlandırmak için kimlik sağlayıcısını güncelleştirmemiz ve yanıt URL'si eklemek için Azure AD'deki uygulama kaydını güncelleştirmemiz gerekir.

1. Azure portalında Azure REKLAM dizinini açın. **Uygulama kayıtlarını**tıklatın, ardından tüm **uygulamaları görüntüle'yi**tıklatın. Daha önce oluşturduğunuz uygulamayı tıklatın (SharePoint SAML Tümleştirmesi).

2. **Ayarlar**'a tıklayın.

3. Ayarlar bıçağında **URL'leri Yanıtla'yı**tıklatın.

4. URL'ye `/_trust/default.aspx` eklenen ek web uygulamasının URL'sini `https://sales.contoso.local/_trust/default.aspx`ekleyin (örneğin) ve **Kaydet'i**tıklatın.

5. SharePoint sunucusunda, **SharePoint 2016 Management Shell'i** açın ve daha önce kullandığınız güvenilir kimlik belirteci verenin adını kullanarak aşağıdaki komutları çalıştırın.

    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```

6. Merkezi Yönetim'de, web uygulamasına gidin ve varolan güvenilir kimlik sağlayıcısını etkinleştirin. Oturum açma sayfası URL'sini de özel oturum açma `/_trust/`sayfası olarak yapılandırmayı unutmayın.

7. Merkezi Yönetim'de web uygulamasını tıklatın ve **Kullanıcı Politikası'nı**seçin. Bu makalede daha önce gösterildiği gibi uygun izinleri ile bir kullanıcı ekleyin.

### <a name="fixing-people-picker"></a>Sabitleme İnsanlar Picker

Kullanıcılar artık Azure AD'nin kimliklerini kullanarak SharePoint 2016'ya giriş yapabilir, ancak kullanıcı deneyimini iyileştirme fırsatları hala vardır. Örneğin, bir kullanıcıyı aramak, kişi seçicide birden çok arama sonucu sunar. Talep eşlemesinde oluşturulan 3 talep türünün her biri için bir arama sonucu vardır. Kişi seçiciyi kullanarak bir kullanıcı seçmek için, kullanıcı adını tam olarak yazmanız ve **ad** talebi sonucunu seçmeniz gerekir.

![Talep arama sonuçları](./media/sharepoint-on-premises-tutorial/fig16-claimssearchresults.png)

Aradığınız değerlerde, yazım hatalarına veya kullanıcıların yanlışlıkla **Ekin** talebi gibi atamak üzere yanlış talep türünü seçmesine yol açabilecek bir doğrulama yoktur. Bu, kullanıcıların kaynaklara başarıyla erişmesini engelleyebilir.

Bu senaryoya yardımcı olmak için, SharePoint 2016 için özel bir talep sağlayıcısı sağlayan [AzureCP](https://yvand.github.io/AzureCP/) adlı açık kaynak çözümü vardır. Kullanıcıların neleri gireceğini ve doğrulamayı gerçekleştirdiğini çözmek için Microsoft Graph API'sını kullanır. [AzureCP'de](https://yvand.github.io/AzureCP/)daha fazla bilgi edinin.

### <a name="assign-the-azure-ad-security-group-in-the-azure-portal"></a>Azure portalında Azure AD Güvenlik Grubu'nu atama

1. Azure portalında **Kurumsal Uygulamalar'ı**seçin, **Tüm uygulamaları**seçin ve ardından şirket **içinde SharePoint'i**seçin.

    ![Kurumsal uygulamalar bıçak](common/enterprise-applications.png)

2. Uygulamalar listesinde, şirket içinde **SharePoint**yazın ve seçin.

    ![Uygulamalar listesindeki SharePoint şirket içi bağlantısı](common/all-applications.png)

3. Soldaki **menüde, Kullanıcılar ve gruplar**seçin.

    !["Kullanıcılar ve gruplar" bağlantısı](common/users-groups-blade.png)

4. Kullanıcı **Ekle'yi**tıklatın.

    ![Atama Ekle bölmesi](common/add-assign-user.png)

5. Kullanmak istediğiniz Güvenlik Grubu'nu arayın ve ardından üyeleri seç bölümüne eklemek için gruba tıklayın. **Seç'i**tıklatın, ardından **Atla'yı**tıklatın.

    ![Arama Güvenlik Grubu](./media/sharepoint-on-premises-tutorial/securitygroup1.png)

    > [!NOTE]
    > Grubun Azure portalındaki Kurumsal uygulamaya başarıyla atandığını bildiren menü çubuğundaki bildirimleri denetleyin.

### <a name="create-sharepoint-on-premises-test-user"></a>SharePoint şirket içi test kullanıcıoluşturma

Bu bölümde, SharePoint şirket içinde Britta Simon adında bir kullanıcı oluşturursunuz. Kullanıcıları SharePoint şirket içi platformuna eklemek için [SharePoint şirket içi destek ekibiyle](https://support.office.com/) birlikte çalışın. Tek oturum açmadan önce kullanıcılar oluşturulmalı ve etkinleştirilmelidir.

### <a name="test-single-sign-on"></a>Çoklu oturum açma testi

Bu bölümde, Access Panelini kullanarak Azure AD tek oturum açma yapılandırmanızı sınarsınız.

Access Paneli'ndeki SharePoint şirket içi döşemesini tıklattığınızda, SSO'yu kurduğunuz SharePoint şirket içi oturumda otomatik olarak oturum açmış olmalısınız. Erişim Paneli hakkında daha fazla bilgi için [Erişim Paneline Giriş'e](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)bakın.

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS Uygulamalarının Azure Etkin Dizini ile Nasıl Entegre Edilen Öğreticiler Listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Etkin Dizinde Koşullu Erişim Nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Microsoft Cloud App Security'de oturum denetimi nedir?](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad)