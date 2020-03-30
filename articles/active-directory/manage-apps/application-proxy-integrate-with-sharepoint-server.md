---
title: SharePoint'e uzaktan erişimi etkinleştirin - Azure AD Application Proxy
description: Şirket içi SharePoint sunucusunun Azure AD Application Proxy ile nasıl tümleştirilenekadar tümleştirilene ilişkin temel bilgileri kapsar.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 983470994c103cb25d0d2aff96ae8544080e6288
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79481305"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu ile SharePoint’e uzaktan erişimi etkinleştirme

Bu adım adım kılavuz, şirket içi bir SharePoint çiftliğinin Azure Active Directory (Azure AD) Uygulama Proxy ile nasıl entegre edilebildiğini açıklar.

## <a name="prerequisites"></a>Ön koşullar

Yapılandırmayı gerçekleştirmek için aşağıdaki kaynaklara ihtiyacınız var:
- Bir SharePoint 2013 çiftlik veya yeni.
- Uygulama Proxy'si içeren bir planı olan bir Azure AD kiracısı. [Azure REKLAM planları ve fiyatlandırması](https://azure.microsoft.com/pricing/details/active-directory/)hakkında daha fazla bilgi edinin.
- Azure AD kiracısında [özel ve doğrulanmış bir etki alanı.](../fundamentals/add-custom-domain.md)
- Kullanıcıların [Azure'da oturum açabildiği](../hybrid/plan-connect-user-signin.md)Azure AD Connect ile senkronize edilen şirket içi Active Directory.
- Kurumsal etki alanı içinde bir makineye yüklenen ve çalışan bir Uygulama Proxy bağlayıcısı.

SharePoint'i Uygulama Proxy ile yapılandırmak için iki URL gerektirir:
- Son kullanıcılar tarafından görülebilen ve Azure AD'de belirlenen harici bir URL. Bu URL özel bir etki alanı kullanabilir. [Azure AD Application Proxy'de özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)hakkında daha fazla bilgi edinin.
- Yalnızca şirket etki alanı içinde bilinen ve doğrudan kullanılmayan dahili bir URL.

> [!IMPORTANT]
> Bağlantıların doğru şekilde eşlendirildidiğinden emin olmak için, dahili URL için aşağıdaki önerileri uygulayın:
> - HTTPS'yi kullanın.
> - Özel bağlantı noktaları kullanmayın.
> - Kurumsal Alan Adı Sistemi'nde (DNS), diğer adı (CName) değil, SharePoint WFE'yi (veya yük bakiyesini) işaret edecek bir ana bilgisayar (A) oluşturun.

Bu makalede aşağıdaki değerler kullanır:
- Dahili URL:`https://sharepoint`
- Dış URL:`https://spsites-demo1984.msappproxy.net/`
- SharePoint web uygulaması için uygulama havuzu hesabı:`Contoso\spapppool`

## <a name="step-1-configure-an-application-in-azure-ad-that-uses-application-proxy"></a>Adım 1: Azure AD'de Uygulama Proxy'si kullanan bir uygulamayı yapılandırma

Bu adımda, Azure Etkin Dizin kiracınızda Uygulama Proxy'si kullanan bir uygulama oluşturursunuz. Harici URL'yi ayarlar ve her ikisi de daha sonra SharePoint'te kullanılan dahili URL'yi belirtirsiniz.

1. Uygulamayı aşağıdaki ayarlarla açıklandığı şekilde oluşturun. Adım adım yönergeler için Azure [AD Application Proxy kullanarak uygulama yayımlama'ya](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad)bakın.
   * **Dahili URL**: SharePoint dahili URL'si `https://sharepoint`daha sonra SharePoint'te ayarlanır, örneğin.
   * **Kimlik Doğrulama Öncesi**: Azure Active Directory
   * **URL'leri Üstbilgide Çevir**: Hayır
   * **URL'leri Uygulama Gövdesinde Çevir**: Hayır

   ![SharePoint'i uygulama olarak yayımla](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

1. Uygulamanız yayınlandıktan sonra, tek oturum açma ayarlarını yapılandırmak için aşağıdaki adımları izleyin:

   1. Portaldaki uygulama sayfasında Tek **oturum açma'yı**seçin.
   1. **Tek Oturum Açma Modu** **için, Tümleşik Windows Kimlik Doğrulaması'nı**seçin.
   1. **İç Uygulama SPN'sini** daha önce ayarladığınız değere ayarlayın. Bu örnekte, değer `HTTP/sharepoint`.
   1. **Temsilci Giriş Kimliği**altında, Active Directory orman yapılandırmanız için en uygun seçeneği seçin. Örneğin, ormanınızda tek bir Active Directory etki alanınız varsa, **şirket içi SAM hesap adını** seçin (aşağıdaki ekran görüntüsünde gösterildiği gibi). Ancak kullanıcılarınız SharePoint ve Application Proxy Bağlayıcı sunucuları ile aynı etki alanında değilse, **şirket içi kullanıcı ana adını** seçin (ekran görüntüsünde gösterilmez).

   ![SSO için Entegre Windows Kimlik Doğrulamasını Yapılandırma](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

1. Uygulamanızı ayarlamayı bitirmek için **Kullanıcılar ve gruplar** bölümüne gidin ve kullanıcıları bu uygulamaya erişmeleri için atayın. 

## <a name="step-2-configure-the-sharepoint-web-application"></a>Adım 2: SharePoint web uygulamasını yapılandırın

SharePoint web uygulaması, Azure AD Application Proxy ile doğru çalışması için Kerberos ve uygun alternatif erişim eşlemeleri ile yapılandırılmalıdır. İki olası seçenek vardır:

- Yeni bir web uygulaması oluşturun ve yalnızca Varsayılan bölge kullanın. SharePoint ile en iyi deneyimi sunduğundan tercih edilen seçenek budur (örneğin, SharePoint tarafından oluşturulan e-posta uyarılarındaki bağlantılar her zaman Varsayılan bölgeye işaret eder).
- Kerberos'u varsayılan olmayan bir bölgede yapılandırmak için varolan bir web uygulamasını genişletin.

> [!IMPORTANT]
> Kullanılan bölge ne olursa olsun, SharePoint web uygulamasının uygulama havuzu hesabı, Kerberos'un doğru çalışması için bir etki alanı hesabı olmalıdır.

### <a name="provision-the-sharepoint-web-application"></a>SharePoint web uygulamasını sağlama

- Yeni bir web uygulaması oluşturur ve yalnızca Varsayılan bölge (tercih edilen seçenek) kullanırsanız:

    1. **SharePoint Management Shell'i** başlatın ve aşağıdaki komut dosyasını çalıştırın:

       ```powershell
       # This script creates a web application and configures the Default zone with the internal/external URL needed to work with Azure AD Application Proxy
       # Edit variables below to fit your environment. Note that the managed account must exist and it must be a domain account
       $internalUrl = "https://sharepoint"
       $externalUrl = "https://spsites-demo1984.msappproxy.net/"
       $applicationPoolManagedAccount = "Contoso\spapppool"
            
       $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$false
       $wa = New-SPWebApplication -Name "SharePoint - AAD Proxy" -Port 443 -SecureSocketsLayer -URL $externalUrl -ApplicationPool "SharePoint - AAD Proxy" -ApplicationPoolAccount (Get-SPManagedAccount $applicationPoolManagedAccount) -AuthenticationProvider $winAp
       New-SPAlternateURL -Url $internalUrl -WebApplication $wa -Zone Default -Internal
       ```

    2. **SharePoint Merkezi Yönetim** sitesini açın.
    1. **Sistem Ayarları**altında, Alternatif **Erişim Eşlemelerini Yapılandır'ı**seçin. **Alternatif Erişim Eşleme Koleksiyonu** kutusu açılır.
    1. Ekranı yeni web uygulamasıyla filtreleyin ve böyle bir şey gördüğünüzden onaylayın:

       ![Web uygulamasının Alternatif Erişim Eşlemeleri](./media/application-proxy-integrate-with-sharepoint-server/new-webapp-aam.png)

- Varolan bir web uygulamasını yeni bir bölgeye genişletirseniz (Varsayılan bölgeyi kullanamıyorsanız):

    1. SharePoint Management Shell'i başlatın ve aşağıdaki komut dosyasını çalıştırın:

       ```powershell
       # This script extends an existing web application to Internet zone with the internal/external URL needed to work with Azure AD Application Proxy
       # Edit variables below to fit your environment
       $webAppUrl = "http://spsites/"
       $internalUrl = "https://sharepoint"
       $externalUrl = "https://spsites-demo1984.msappproxy.net/"
       
       $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$false
       $wa = Get-SPWebApplication $webAppUrl
       New-SPWebApplicationExtension -Name "SharePoint - AAD Proxy" -Identity $wa -SecureSocketsLayer -Zone Extranet -Url $externalUrl -AuthenticationProvider $winAp
       New-SPAlternateURL -Url $internalUrl -WebApplication $wa -Zone Extranet -Internal
       ```

    2. **SharePoint Merkezi Yönetim** sitesini açın.
    1. **Sistem Ayarları**altında, Alternatif **Erişim Eşlemelerini Yapılandır'ı**seçin. **Alternatif Erişim Eşleme Koleksiyonu** kutusu açılır.
    1. Genişletilmiş web uygulamasıyla ekranı filtreleyin ve böyle bir şey gördüğünüzden onaylayın:

        ![Genişletilmiş uygulamanın Alternatif Erişim Eşlemeleri](./media/application-proxy-integrate-with-sharepoint-server/extend-webapp-aam.png)

### <a name="make-sure-the-sharepoint-web-application-is-running-under-a-domain-account"></a>SharePoint web uygulamasının bir etki alanı hesabı altında olduğundan emin olun

SharePoint web uygulamasının uygulama havuzunu çalıştıran hesabı tanımlamak ve etki alanı hesabı olduğundan emin olmak için aşağıdaki adımları izleyin:

1. **SharePoint Merkezi Yönetim** sitesini açın.
1. **Güvenlik'e** gidin ve **hizmet hesaplarını yapıya belirleyin'i**seçin.
1. **Web Uygulama Havuzu seçin - YourWebApplicationName**.

   ![Hizmet hesabı yapılandırma seçenekleri](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

1. Bu **bileşen için bir hesap seç'in** bir etki alanı hesabı döndüreceğini onaylayın ve bir sonraki adımda gerekli olacağından bunu unutmayın.

### <a name="make-sure-that-an-https-certificate-is-configured-for-the-iis-site-of-the-extranet-zone"></a>Extranet bölgesinin IIS sitesi için bir HTTPS sertifikası yapılandırıldığından emin olun

İç URL HTTPS protokolü`https://SharePoint/`kullandığından , Internet Information Services (IIS) sitesinde bir sertifika ayarlanmalıdır.

1. Windows PowerShell konsolunu açın.
1. Kendi imzalı bir sertifika oluşturmak ve bilgisayarın MY deposuna eklemek için aşağıdaki komut dosyasını çalıştırın:

   ```powershell
   # Replace "SharePoint" with the actual hostname of the Internal URL of your Azure AD proxy application
   New-SelfSignedCertificate -DnsName "SharePoint" -CertStoreLocation "cert:\LocalMachine\My"
   ```

   > [!IMPORTANT]
   > Kendi imzalı sertifikalar yalnızca test amaçlı uygundur. Üretim ortamlarında, bunun yerine bir sertifika yetkilisi tarafından verilen sertifikaları kullanmanızı şiddetle öneririz.

1. Internet Information Services Manager konsolunu açın.
1. Ağaç görünümünde sunucuyu genişletin, **Siteleri**genişletin, **SharePoint - AAD Proxy** sitesini seçin ve **Ciltler'i**seçin.
1. **https binding'i** seçin ve ardından **Edit'i**seçin.
1. TLS/SSL sertifika alanında **SharePoint** sertifikasını seçin ve **ardından Tamam'ı**seçin.

Artık Azure AD Application Proxy aracılığıyla SharePoint sitesine dışarıdan erişebilirsiniz.

## <a name="step-3-configure-kerberos-constrained-delegation"></a>Adım 3: Kerberos Kısıtlı Temsilciliği Yapılandır

Kullanıcılar başlangıçta Azure AD'de kimlik doğrulaması yapacak ve ardından Azure AD Proxy bağlayıcısı aracılığıyla Kerberos'u kullanarak SharePoint'e yönlendirecektir. Bağlayıcının Azure REKLAM kullanıcısı adına bir Kerberos belirteci edinmesine izin vermek için, Protokol geçişiyle Kerberos Kısıtlı Temsilciliği'ni (KCD) yapılandırmanız gerekir. KCD hakkında daha fazla bilgi edinmek için [Kerberos Kısıtlı Delegasyon genel bakış](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj553400(v=ws.11))bakın.

### <a name="set-the-spn-for-the-sharepoint-service-account"></a>SharePoint hizmet hesabı için SPN'yi ayarlama

Bu makalede, dahili `https://sharepoint`URL ve böylece hizmet ana adı (SPN) olduğunu. `HTTP/sharepoint` Bu değerleri ortamınıza karşılık gelen değerlerle değiştirmeniz gerekir.
SharePoint uygulama `HTTP/sharepoint` havuzu hesabına `Contoso\spapppool`SPN kaydetmek için, etki alanının yöneticisi olarak aşağıdaki komutu bir komut isteminden çalıştırın:

`setspn -S HTTP/sharepoint Contoso\spapppool`

Komut, `Setspn` spn'yi ekleyenden önce arar. SPN zaten varsa, yinelenen bir **SPN Değer** hatası görürsünüz. Bu durumda, doğru uygulama havuzu hesabı altında ayarlanmazsa varolan SPN'yi kaldırmayı düşünün. -L seçeneği ile komutu `Setspn` çalıştırarak SPN'nin başarıyla eklandığını doğrulayabilirsiniz. Bu komut hakkında daha fazla bilgi edinmek için [Setspn'e](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11))bakın.

### <a name="make-sure-the-connector-is-trusted-for-delegation-to-the-spn-that-was-added-to-the-sharepoint-application-pool-account"></a>SharePoint uygulama havuzu hesabına eklenen SPN'ye temsilci lik için bağlayıcının güvenilir olduğundan emin olun

Azure AD Application Proxy hizmetinin kullanıcı kimliklerini SharePoint uygulama havuzu hesabına devredilebilmeleri için KCD'yi yapılandırın. Azure AD'de kimlik doğrulaması yapılan kullanıcılarınız için Kerberos biletlerini almak için Uygulama Proxy bağlayıcısının etkinleştirerek KCD'yi yapılandırın. Daha sonra, bu sunucu bağlamı hedef uygulamaya geçirir (Bu durumda SharePoint).

KCD'yi yapılandırmak için her bağlayıcı makinesi için aşağıdaki adımları izleyin:

1. Etki alanı yöneticisi olarak etki alanı denetleyicisinde oturum açın ve ardından Etkin Dizin Kullanıcıları ve Bilgisayarlarını açın.
1. Azure AD Proxy bağlayıcısını çalıştıran bilgisayarı bulun. Bu örnekte, SharePoint sunucusunun kendisidir.
1. Bilgisayarı çift tıklatın ve ardından **Temsilci** sekmesini seçin.
1. Delegasyon seçeneklerinin **yalnızca belirtilen hizmetlere delegasyon için bu bilgisayara güven olarak ayarlandıklarına**emin olun. Ardından, **kimlik doğrulama iletişim kuralını kullan'ı**seçin.
1. **Ekle** düğmesini seçin, **Kullanıcılar veya Bilgisayarlar'ı**seçin ve SharePoint uygulama havuzu hesabını bulun. Örneğin: `Contoso\spapppool`.
1. SPN'ler listesinde, hizmet hesabı için daha önce oluşturduğunuz dosyayı seçin.
1. **Değişikliklerinizi** kaydetmek için Tamam'ı ve ardından tekrar **Tamam'ı** seçin.
  
   ![Temsilci ayarları](./media/application-proxy-integrate-with-sharepoint-server/delegation-box2.png)

Artık harici URL'yi kullanarak SharePoint'te oturum açmaya ve Azure ile kimlik doğrulaması yapmaya hazırsınız.

## <a name="troubleshoot-sign-in-errors"></a>Oturum açma hataları

Sitede oturum açma çalışmıyorsa, Bağlayıcı günlüklerinde sorun hakkında daha fazla bilgi alabilirsiniz: Bağlayıcıyı çalıştıran makineden olay görüntüleyicisi açın, **Uygulamalar ve Hizmetler Günlükleri** > **Microsoft** > **AadApplicationProxy** > **Bağlayıcısı'na**gidin ve **Yönetici** günlüğünü inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Application Proxy'de özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)
* [Azure AD Uygulama Proxy bağlayıcılarını anlama](application-proxy-connectors.md)
