---
title: 'Öğretici: SharePoint şirket içi ile Azure Active Directory tümleştirme | Microsoft Docs'
description: Şirket içi Azure Active Directory ve SharePoint arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
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
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "81867000"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-sharepoint-on-premises"></a>Öğretici: SharePoint şirket içi ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

Bu öğreticide, SharePoint şirket içi Azure Active Directory (Azure AD) ile tümleştirmeyi öğreneceksiniz. SharePoint şirket içi Azure AD ile tümleştirdiğinizde şunları yapabilirsiniz:

* Azure AD 'de, şirket içi SharePoint 'e erişimi olan denetim.
* Kullanıcılarınızın Azure AD hesaplarıyla şirket içi SharePoint 'te otomatik olarak oturum açmalarına olanak sağlayın.
* Hesaplarınızı tek bir merkezi konumda yönetin-Azure portal.

Azure AD ile SaaS uygulaması tümleştirmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma nedir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on).

## <a name="prerequisites"></a>Ön koşullar

Şirket içi SharePoint ile Azure Active Directory tümleştirmesini yapılandırmak için aşağıdaki öğeler gereklidir:

* Azure Active Directory aboneliği. Bir Azure AD ortamınız yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) alabilirsiniz
* SharePoint 2013 grubu veya daha yeni bir sürüm.

## <a name="scenario-description"></a>Senaryo açıklaması

Bu öğreticide, bir test ortamında Azure Active Directory çoklu oturum açmayı yapılandırıp test edersiniz. Azure Active Directory Kullanıcılar SharePoint şirket Içi sunucunuza erişebiliyor.

## <a name="create-the-enterprise-applications-in-azure-portal"></a>Kurumsal uygulamaları Azure portal oluşturma

SharePoint 'in şirket içi tümleştirmesini Azure AD ile yapılandırmak için, Galeriden SharePoint Şirket içinden yönetilen SaaS uygulamaları listesine eklemeniz gerekir.

Galeriden SharePoint şirket içi eklemek için aşağıdaki adımları uygulayın:

1. **[Azure Portal](https://portal.azure.com)** sol gezinti panelinde **Azure Active Directory** simgesine tıklayın.

 > [!NOTE]
 > Öğe kullanılabilir olmaması gerekiyorsa, sol gezinti bölmesinin en üstündeki sabit **tüm hizmetler** bağlantısı aracılığıyla da açılabilirler. Aşağıdaki genel bakışta, **Azure Active Directory** bağlantısı **kimlik** bölümünde bulunur veya filtre metin kutusu kullanılarak aranabilir.

2. **Kurumsal uygulamalar** ' a gidin ve **tüm uygulamalar** seçeneğini belirleyin.

3. Yeni uygulama eklemek için, iletişim kutusunun üst kısmındaki **Yeni uygulama** düğmesine tıklayın.

4. Arama kutusuna **SharePoint şirket içi**yazın, sonuç panelinden **SharePoint şirket içi** seçeneğini belirleyin.

    <kbd>![Sonuç listesinde SharePoint şirket içi](./media/sharepoint-on-premises-tutorial/search-new-app.png)</kbd>

1. SharePoint Onpred 'niz için bir ad belirtin ve uygulamayı eklemek için **Ekle** düğmesine tıklayın.

1. Yeni kurumsal uygulamada Özellikler ' e tıklayın ve **Kullanıcı atamasının gerekli** değerini denetleyin

   <kbd>![Sonuç listesinde SharePoint şirket içi](./media/sharepoint-on-premises-tutorial/user-assignment-required.png)</kbd>

senaryomızda bu değer **Hayır**olarak ayarlanır.

## <a name="configure-and-test-azure-ad"></a>Azure AD 'yi yapılandırma ve test etme

Bu bölümde Azure AD çoklu oturum açma 'yı şirket içi SharePoint ile yapılandırırsınız.
Çoklu oturum açma 'nın çalışması için, bir Azure AD kullanıcısı ve SharePoint şirket içi ile ilgili Kullanıcı arasındaki bağlantı ilişkisinin kurulması gerekir.

Şirket içi SharePoint ile Azure Active Directory çoklu oturum açmayı yapılandırmak ve test etmek için aşağıdaki yapı taşlarını gerçekleştirmeniz gerekir:

1. **[Azure AD çoklu oturum açma özelliğini yapılandırarak](#configure-azure-ad-single-sign-on)** kullanıcılarınızın bu özelliği kullanmasına olanak sağlayın.
1. **[Şirket Içi SharePoint 'ı yapılandırma](#configure-sharepoint-on-premises)** -uygulama tarafında çoklu oturum açma ayarlarını yapılandırmak için.
1. **[Azure Portal Azure AD test kullanıcısı oluşturun](#create-an-azure-ad-test-user-in-the-azure-portal)** -çoklu oturum açma IÇIN Azure AD 'de yeni kullanıcı oluşturun.
1. Çoklu oturum açma için Azure AD 'de yeni güvenlik grubu oluşturma **[Azure Portal bir Azure AD güvenlik grubu oluşturun](#create-an-azure-ad-security-group-in-the-azure-portal)** .
1. **[SharePoint şirket içi Azure Active Directory hesabına Izin verin](#grant-permissions-to-azure-active-directory-account-in-sharepoint-on-premises)** . Azure AD kullanıcısına izin verin.
1. **[SharePoint şirket Içi Azure AD grubuna Izin verme](#grant-permissions-to-azure-ad-group-in-sharepoint-on-premises)** -Azure AD grubuna izin verin.
1. Şirket içi **[SharePoint 'e bir Konuk hesabına erişim Izni verin Azure Portal](#grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal)** -şirket içi SharePoint IÇIN Azure AD 'de Konuk hesabına izin verin.
1. **[Birden çok Web uygulaması için güvenilen kimlik sağlayıcısını yapılandırma](#configuring-the-trusted-identity-provider-for-multiple-web-applications)** -birden çok Web uygulaması için aynı güvenilir kimlik sağlayıcısını kullanma

### <a name="configure-azure-ad-single-sign-on"></a>Azure AD çoklu oturum açmayı yapılandırma

Bu bölümde, Azure portal Azure AD çoklu oturum açma özelliğini etkinleştirirsiniz.

Şirket içi SharePoint ile Azure AD çoklu oturum açmayı yapılandırmak için aşağıdaki adımları uygulayın:

1. [Azure Portal](https://portal.azure.com/)Azure AD dizinini açın, **Kurumsal uygulamalar**' a tıklayın, **daha önce oluşturulan kurumsal uygulama adına** tıklayın ve **Çoklu oturum**açma ' ya tıklayın.

2. Çoklu oturum **açma yöntemi seç** iletişim kutusunda, çoklu oturum açmayı etkinleştirmek için **SAML** modu ' na tıklayın.
 
3. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **temel SAML yapılandırması** Iletişim kutusunu açmak için **Düzenle** simgesine tıklayın.

4. **Temel SAML yapılandırması** bölümünde aşağıdaki adımları gerçekleştirin:

    ![SharePoint şirket içi etki alanı ve URL 'Ler çoklu oturum açma bilgileri](./media/sharepoint-on-premises-tutorial/sp-identifier-reply.png)

    1. **Tanımlayıcı** kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`urn:<sharepointFarmName>:<federationName>`

    1. **Yanıt URL 'si** metin kutusuna aşağıdaki kalıbı kullanarak bir URL yazın:`https://<YourSharePointSiteURL>/_trust/`

    1. **Oturum açma URL 'si** metin kutusunda, aşağıdaki kalıbı kullanarak bir URL yazın:`https://<YourSharePointSiteURL>/`
    1. Kaydet ' e tıklayın.

    > [!NOTE]
    > Bu değerler gerçek değildir. Bu değerleri gerçek oturum açma URL 'SI, tanımlayıcı ve yanıt URL 'siyle güncelleştirin.

5. **SAML Ile çoklu oturum açmayı ayarlama** sayfasında, **SAML imzalama sertifikası** bölümünde, **sertifika (base64)** ' i gereksiniminize göre verilen seçeneklerden indirmek ve bilgisayarınıza kaydetmek için **İndir** ' e tıklayın.

    ![Sertifika indirme bağlantısı](./media/sharepoint-on-premises-tutorial/certificatebase64.png)

6. SharePoint Şirket **Içi ayarlama** bölümünde uygun URL 'leri gereksiniminize göre kopyalayın.
    
    1. **Oturum açma URL 'SI**
    
        Oturum açma URL 'sini Kopyala **/SAML2** ' yi sonunda **/wsbesle**değiştirin, şöyle **https://login.microsoftonline.com/2c4f1a9f-be5f-10ee-327d-a95dac567e4f/wsfed** görünür (Bu URL doğru değil)

    1. **Azure AD tanımlayıcısı**
    1. **Oturum kapatma URL 'SI**
    > [!NOTE]
    > Bu URL, SharePoint 'te olduğu gibi kullanılamaz: **/SAML2** değerini **/wsbesle**değiştirmelisiniz. SharePoint şirket Içi uygulaması SAML 1,1 belirtecini kullanır, bu nedenle Azure AD, SharePoint sunucusundan WS Bestalebi bekliyor ve kimlik doğrulamasından sonra SAML 1,1 ' i yayınlar. simgesinde.

### <a name="configure-sharepoint-on-premises"></a>SharePoint 'i şirket içinde yapılandırma

1. **SharePoint Server 2016 ' de yeni bir güvenilen kimlik sağlayıcısı oluşturma**

    SharePoint sunucusunda oturum açın ve SharePoint Yönetim Kabuğu 'nu açın. Değerleri girin:
    1. **$Realm** (Azure Portal) SharePoint şirket Içi etki alanı ve URL 'ler bölümünde tanımlayıcı değer.
    1. **$wsfedurl** (çoklu oturum açma hizmeti URL 'si).
   1. **$FilePath** (sertifika dosyasını indirdiğiniz dosya yolu) Azure Portal.

    Yeni bir güvenilen kimlik sağlayıcısı yapılandırmak için aşağıdaki komutları çalıştırın.

    > [!TIP]
    > PowerShell 'i kullanmaya yeni başladıysanız veya PowerShell 'in nasıl çalıştığı hakkında daha fazla bilgi edinmek istiyorsanız bkz. [SharePoint PowerShell](https://docs.microsoft.com/powershell/sharepoint/overview?view=sharepoint-ps).


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
1. **Uygulamanız için güvenilen kimlik sağlayıcısını etkinleştirin**

    a. Merkezi Yönetim 'de **Web uygulamasını Yönet** ' e gidin ve Azure AD ile güvenli hale getirmek istediğiniz Web uygulamasını seçin.

    b. Şeritte, **kimlik doğrulama sağlayıcıları** ' na tıklayın ve kullanmak istediğiniz bölgeyi seçin.

    c. **Güvenilen kimlik sağlayıcısı** ' nı seçin ve ardından *azuread*adlı yeni kaydettiğiniz sağlayıcıyı tanımla ' yı seçin.

    d. **Tamam**'a tıklayın.

    ![Kimlik doğrulama sağlayıcınızı yapılandırma](./media/sharepoint-on-premises-tutorial/config-auth-provider.png)

### <a name="create-an-azure-ad-test-user-in-the-azure-portal"></a>Azure portal bir Azure AD test kullanıcısı oluşturun

Bu bölümün amacı, Azure portal bir test kullanıcısı oluşturmaktır.

1. Azure portal sol bölmedeki **Azure Active Directory**' i seçin ve **Yönet** çevir ' te **Kullanıcılar**' ı seçin.

2. Ardından ekranın en üstünde **Yeni Kullanıcı** tarafından izlenen **tüm kullanıcılar** ' ı seçin.

3. **Kullanıcı oluştur** seçeneğini belirleyin ve Kullanıcı özelliklerinde aşağıdaki adımları gerçekleştirin.  
   Azure AD 'de kiracı sonekini veya doğrulanmış herhangi bir etki alanını kullanarak Kullanıcı oluşturabilirsiniz. 

    a. **Ad** alanına kullanıcı adını girin, **testuser**kullandık.
  
    b. **Kullanıcı adı** alan türü`TestUser@yourcompanydomain.extension`  
    Örneğin, TestUser@contoso.com

    ![Kullanıcı iletişim kutusu](./media/sharepoint-on-premises-tutorial/user-properties.png)

    c. **Parolayı göster** onay kutusunu seçin ve ardından parola kutusunda görüntülenen değeri yazın.

    d. **Oluştur**' a tıklayın.

    e. Artık siteyi ile TestUser@contoso.com paylaşabilir ve bu kullanıcıya erişime izin verebilirsiniz.

### <a name="create-an-azure-ad-security-group-in-the-azure-portal"></a>Azure portal bir Azure AD güvenlik grubu oluşturun

1. **Azure Active Directory > grupları**' na tıklayın.

2. **Yeni Grup**' a tıklayın:

3. **Grup türü**, **Grup adı**, **Grup açıklaması**, **üyelik türü**' nde Fill yazın. Üyeler ' i seçmek için oka tıklayın ve ardından gruba eklemek istediğiniz üyeyi arayın veya tıklayın. Seçili üyeleri eklemek için **Seç** ' e tıklayın, ardından **Oluştur**' a tıklayın.

![Azure AD güvenlik grubu oluşturma](./media/sharepoint-on-premises-tutorial/new-group.png)

### <a name="grant-permissions-to-azure-active-directory-account-in-sharepoint-on-premises"></a>SharePoint şirket içi Azure Active Directory hesabına izin verme

Şirket içi SharePoint 'te Azure Active Directory kullanıcısına erişim Izni vermek için, site koleksiyonunu paylaşmanız veya Azure Active Directory kullanıcıyı site koleksiyonunun bir grubuna eklemeniz gerekir. Kullanıcılar artık Azure AD 'deki kimlikleri kullanarak SharePoint 201x 'te oturum açabilir, ancak kullanıcı deneyimine yönelik geliştirme için hala fırsatlar vardır. Örneğin, bir kullanıcının aranması, kişiler seçicisindeki birden çok arama sonucu sunar. Talep eşlemesinde oluşturulan her talep türü için bir arama sonucu vardır. Kişiler seçiciyi kullanarak bir kullanıcı seçmek için, Kullanıcı adını tam olarak yazmanız ve **ad** talep sonucunu seçmeniz gerekir.

![Talep arama sonuçları](./media/sharepoint-on-premises-tutorial/claims-search-results.png)

Arama yaptığınız değerlerde herhangi bir doğrulama yoktur, bu, yanlış bir talep türünü yanlışlıkla seçme hatalarına veya kullanıcılara yol açabilir. Bu, kullanıcıların kaynaklara başarıyla erişmesini engelleyebilir.

Bu senaryoya sahip **kişiler seçicisini düzeltmek için** , SharePoint 2013, 2016 ve 2019 için özel bir talep sağlayıcısı sağlayan [azurecp](https://yvand.github.io/AzureCP/) adlı açık kaynaklı bir çözüm vardır. Kullanıcıların hangi kullanıcılara girip doğrulama gerçekleştireceğini çözümlemek için Microsoft Graph API 'sini kullanır. [Azurecp](https://yvand.github.io/AzureCP/)'de daha fazla bilgi edinin.

  > [!NOTE]
  > AzureCP olmadan, Azure AD grubunun KIMLIĞINI ekleyerek gruplar ekleyebilirsiniz, ancak bu, kullanıcının kolay ve güvenilir değildir. Şöyle görünür:  
  >   
  >![Azure AD grubunu SharePoint grubuna ekleme](./media/sharepoint-on-premises-tutorial/adding-group-by-id.png)
  
### <a name="grant-permissions-to-azure-ad-group-in-sharepoint-on-premises"></a>SharePoint 'te şirket içi Azure AD grubuna izin verme

Şirket içi SharePoint 'e Azure Active Directory güvenlik grupları atamak için, SharePoint Server için özel bir talep sağlayıcısı kullanılması gerekir. Örneğimizde, AzureCP ' yi kullandık.

 > [!NOTE]
 > AzureCP 'nin bir Microsoft ürünü değil veya Microsoft teknik desteği tarafından desteklendiğine lütfen emin olun. Şirket içi SharePoint grubuna göre AzureCP 'yi indirin, yükleyin ve yapılandırın https://yvand.github.io/AzureCP/. 

1. AzureCP 'yi SharePoint şirket içi grubunda veya alternatif bir özel talep sağlayıcı çözümünde yapılandırın. AzureCP yapılandırmasının adımları şurada mevcuttur:https://yvand.github.io/AzureCP/Register-App-In-AAD.html

1. Azure portal Azure AD dizinini açın. **Kuruluş uygulamaları**' na tıklayın, **daha önce oluşturulan kurumsal uygulama adına** tıklayın ve **Çoklu oturum**açma ' ya tıklayın.

1. **Sam Ile çoklu oturum açmayı ayarlama**sayfasında, **Kullanıcı öznitelikleri & talepler** bölümünü düzenleyin.

1. **Grup talebi ekle**' ye tıklayın.

1. Kullanıcı ile ilişkili olan grupları talebe göre seçin. bizim örneğimizde **tüm gruplar** ' ı seçin ve kaynak öznitelik bölümünde **Grup Kimliği** ' ni seçin ve **Kaydet**' e tıklayın.

Şirket içi SharePoint 'te Azure Active Directory güvenlik grubuna erişim izni vermek için site koleksiyonunu paylaşmanız veya site koleksiyonunun grubundan birine Azure Active Directory güvenlik grubunu eklemeniz gerekir.

1. SharePoint site koleksiyonuna gidin, site koleksiyonu için site ayarları altında "kişiler ve gruplar" a tıklayın. SharePoint grubunu seçin, ardından yeni ' ye tıklayın, "kullanıcıları bu gruba ekle" ' ye tıklayın ve grubunuzun adını yazarak, kişiler Seçicisi Azure Active Directory güvenlik grubunu görüntüler.

    ![Azure AD grubunu SharePoint grubuna ekleme](./media/sharepoint-on-premises-tutorial/permission-azure-ad-group.png)

### <a name="grant-access-to-a-guest-account-to-sharepoint-on-premises-in-the-azure-portal"></a>Azure portal şirket içi SharePoint 'e bir Konuk hesabı erişimi verme

artık SharePoint sitenize, tutarlı bir şekilde bir Konuk hesabına erişim hakkı vermek mümkündür. Bu, UPN 'nin değiştirilmesidir. İle jdoe@outlook.com Kullanıcı şöyle `jdoe_outlook.com#ext#@TENANT.onmicrosoft.com`temsil edilir. Sitenizi dış kullanıcılarla paylaşırken sorunsuz bir deneyim almak mümkündür. Azure portal **Kullanıcı özniteliklerine & talepler** bölümünde bazı değişiklikler eklenmesi gerekir.

1. Azure portal Azure AD dizinini açın. **Kuruluş uygulamaları**' na tıklayın, **daha önce oluşturulan kurumsal uygulama adına** tıklayın ve **Çoklu oturum**açma ' ya tıklayın.

1. **Sam Ile çoklu oturum açmayı ayarlama**sayfasında, **Kullanıcı öznitelikleri & talepler** bölümünü düzenleyin.

1. **gerekli talep** bölgesinde **benzersiz kullanıcı tanımlayıcısı (ad kimliği)** seçeneğine tıklayın.

1. **Kaynak özniteliği** özelliğini **User. localuserprincipalname** değerine değiştirin ve **kaydedin**.

    ![Kullanıcı öznitelikleri & talepler ilk](./media/sharepoint-on-premises-tutorial/manage-claim.png)

1. Şeriti kullanmak artık **SAML tabanlı oturum açma** 'ya geri dönerek **Kullanıcı öznitelikleri & talepler** bölümünde şu şekilde görünür: 

    ![Kullanıcı öznitelikleri & talepler son](./media/sharepoint-on-premises-tutorial/user-attributes-claims-final.png)  

    > [!NOTE]
    > Bu kurulumda soyadı ve verilen ad gerekli değildir.

1. Azure portal, sol bölmedeki **Azure Active Directory**' i seçin, **Kullanıcılar**' ı seçin.

1. **Yeni Konuk Kullanıcı** ' ya tıklayın

1. **Kullanıcıyı davet et** seçeneğini belirleyin ve kullanıcının özelliklerini doldurup **davet**' e tıklayın.

1. Artık siteyi ile MyGuestAccount@outlook.com paylaşabilir ve bu kullanıcıya erişime izin verebilirsiniz.

    ![Siteyi Konuk hesapla paylaşma](./media/sharepoint-on-premises-tutorial/sharing-guest-account.png)

### <a name="configuring-the-trusted-identity-provider-for-multiple-web-applications"></a>Birden çok Web uygulaması için güvenilen kimlik sağlayıcısını yapılandırma

Yapılandırma tek bir Web uygulaması için kullanılabilir, ancak birden fazla Web uygulaması için aynı güvenilir kimlik sağlayıcısını kullanmak istiyorsanız ek yapılandırma gerekir. Örneğin, URL 'YI `https://sales.contoso.com` kullanmak için bir Web uygulaması genişlettik ve şimdi de kullanıcıların `https://marketing.contoso.com` kimliğini doğrulamak istiyorsunuz. Bunu yapmak için, kimlik sağlayıcısını WReply parametresini kabul etmek ve Azure AD 'de uygulama kaydını güncelleştirmek ve bir yanıt URL 'SI eklemek için güncelleştirmeniz gerekir.

1. Azure portal Azure AD dizinini açın. **Kuruluş uygulamaları**' na tıklayın, **daha önce oluşturulan kurumsal uygulama adına** tıklayın ve **Çoklu oturum**açma ' ya tıklayın.

2. **Sam Ile çoklu oturum açmayı ayarlama**sayfasında, **temel SAML yapılandırmasını**düzenleyin.

    ![Temel SAML yapılandırmasını düzenle](./media/sharepoint-on-premises-tutorial/add-reply-url.png)

3. **Yanıt URL 'si (onaylama tüketici hizmeti URL 'si)** ' nde ek Web uygulamalarının URL 'sini ekleyin ve **Kaydet**' e tıklayın.

    ![Temel SAML yapılandırmasını düzenle](./media/sharepoint-on-premises-tutorial/reply-url-for-web-application.png)

4. SharePoint sunucusunda, **SharePoint 201x Yönetim Kabuğu** 'nu açın ve daha önce kullandığınız güvenilen kimlik belirteci verenin adını kullanarak aşağıdaki komutları yürütün.
    ```
    $t = Get-SPTrustedIdentityTokenIssuer "AzureAD"
    $t.UseWReplyParameter=$true
    $t.Update()
    ```
5. Merkezi Yönetim 'de Web uygulamasına gidin ve mevcut güvenilen kimlik sağlayıcısını etkinleştirin.

İç kullanıcılarınız için şirket Içi SharePoint 'e erişim vermek istediğiniz başka bir senaryonuz olabilir. Bu senaryo için, şirket Içi kullanıcılarınızın Azure Active Directory eşitlemesine izin verecek Microsoft Azure Active Directory Connect dağıtmanız gerekir, bu kurulum başka bir makalenin parçası olur. 

## <a name="additional-resources"></a>Ek kaynaklar

- [SaaS uygulamalarını Azure Active Directory ile tümleştirme hakkında öğreticiler listesi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list)

- [Azure Active Directory ile uygulama erişimi ve çoklu oturum açma özellikleri nelerdir?](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on)

- [Azure Active Directory Koşullu erişim nedir?](https://docs.microsoft.com/azure/active-directory/conditional-access/overview)

- [Azure Active Directory ile karma kimlik nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-hybrid-identity)
