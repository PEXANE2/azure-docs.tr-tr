---
title: Azure AD Uygulama Ara Sunucusu SharePoint 'e uzaktan erişimi etkinleştirme | Microsoft Docs
description: Şirket içi SharePoint sunucusunu Azure AD Uygulama Ara Sunucusu ile tümleştirme hakkında temel bilgileri içerir.
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
ms.openlocfilehash: a8f9cba1072866a3efdeb7b99981d0bfda22ab00
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72286029"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Azure AD Uygulama Ara Sunucusu ile SharePoint’e uzaktan erişimi etkinleştirme

Bu adım adım kılavuzda, şirket içi SharePoint grubunun Azure Active Directory (Azure AD) uygulama proxy 'Si ile nasıl tümleştirileceği açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Yapılandırmayı gerçekleştirmek için aşağıdaki kaynaklara ihtiyacınız vardır:
- SharePoint 2013 grubu veya daha yeni bir sürüm.
- Uygulama proxy 'Sini içeren bir plana sahip Azure Active Directory kiracısı. [Azure Active Directory planları ve fiyatlandırma](https://azure.microsoft.com/pricing/details/active-directory/)hakkında daha fazla bilgi edinin.
- Azure AD kiracısında [özel doğrulanmış bir etki alanı](../fundamentals/add-custom-domain.md) .
- Şirket içi Active Directory, kullanıcıların [Azure 'da oturum](../hybrid/plan-connect-user-signin.md)açabiliyor Azure AD Connect ile eşitlenir.
- Şirket etki alanındaki bir makineye yüklenen ve çalıştırılan bir uygulama proxy Bağlayıcısı.

SharePoint 'i uygulama proxy 'Si ile yapılandırmak için iki URL gerekir:
- Son kullanıcılar tarafından görülebilen ve Azure Active Directory belirlendiği bir dış URL. Bu URL, özel bir etki alanı kullanabilir. [Azure AD uygulama ara sunucusu özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)hakkında daha fazla bilgi edinin.
- Yalnızca şirket etki alanı içinde bilinen ve doğrudan kullanılmayan bir iç URL.

> [!IMPORTANT]
> Bağlantıların doğru şekilde eşlendiğinden emin olmak için iç URL için aşağıdaki önerilere uyun:
> - HTTPS Kullan
> - Özel bağlantı noktalarını kullanma
> - Şirket DNS 'de, bir diğer ad (CName) değil, SharePoint WFE 'yi (veya yük dengeleyiciyi) göstermek için bir konak (A) oluşturun

Bu makalede, aşağıdaki değerler kullanılacaktır:
- İç URL: `https://sharepoint`
- Dış URL: `https://spsites-demo1984.msappproxy.net/`
- SharePoint Web uygulaması için uygulama havuzu hesabı: `Contoso\spapppool`

## <a name="step-1-configure-an-application-in-azure-ad-that-uses-application-proxy"></a>1\. Adım: Azure AD 'de uygulama proxy 'Si kullanan bir uygulama yapılandırma

Bu adımda, Azure Active Directory kiracınızda uygulama proxy 'Si kullanacak bir uygulama oluşturacaksınız. Dış URL 'yi ayarlayacaksınız ve her ikisi de SharePoint 'te daha sonra kullanılan iç URL 'YI belirlersiniz.

1. Aşağıdaki ayarlarla açıklandığı gibi uygulamayı oluşturun. Adım adım yönergeler için bkz. [Azure AD uygulama ara sunucusu kullanarak uygulama yayımlama](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).
   * **Iç URL**: `https://sharepoint` gibi SharePoint 'te daha sonra ayarlanacak SharePoint iç URL 'si.
   * **Ön kimlik doğrulama yöntemi**: Azure Active Directory
   * **Üst bilgilerdeki URL 'Yi çevir**: Hayır
   * **Uygulama gövdesinde URL 'Yi çevir**: Hayır

   ![SharePoint 'i uygulama olarak Yayımla](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

1. Uygulamanız yayımlandıktan sonra, çoklu oturum açma ayarlarını aşağıdaki adımlarla yapılandırın:

   1. Portaldaki uygulama sayfasında, **Çoklu oturum açma**' yı seçin.
   1. **Çoklu oturum açma modu**Için **Tümleşik Windows kimlik doğrulaması**' nı seçin.
   1. **Iç uygulama SPN 'sini** daha önce ayarladığınız değere ayarlayın. Bu örnek için değer `HTTP/sharepoint` olur.
   1. **Temsilci olarak oturum açma kimliği**' nde, Active Directory orman yapılandırmanız için en uygun seçeneği belirleyin. Örneğin, ormanınızda tek bir AD etki alanınız varsa Şirket **ıçı Sam hesap adı** ' nı (aşağıda gösterildiği gibi) seçin, ancak kullanıcılarınız SharePoint ve uygulama proxy Bağlayıcısı sunucuları ile aynı etki alanında değilse Şirket **içi Kullanıcı sorumlusu ' nı seçin. ad** (gösterilmez).

   ![SSO için tümleşik Windows kimlik doğrulamasını yapılandırma](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

1. Uygulamanızı ayarlamayı bitirmeden, **Kullanıcılar ve gruplar** bölümüne gidin ve bu uygulamaya erişmek için kullanıcıları atayın. 

## <a name="step-2-configure-the-sharepoint-web-application"></a>2\. Adım: SharePoint Web uygulamasını yapılandırma

SharePoint Web uygulaması, Kerberos ile yapılandırılmalı ve Azure AD Uygulama Ara Sunucusu ile doğru şekilde çalışmak için uygun alternatif erişim eşlemeleriyle yapılandırılmalıdır. Olası iki seçenek vardır:

1. Yeni bir Web uygulaması oluşturun ve yalnızca varsayılan bölgeyi kullanın. Bu, SharePoint ile en iyi deneyimin olması için tercih edilen seçenektir (örneğin, SharePoint tarafından oluşturulan e-posta uyarılarında bulunan bağlantılar her zaman varsayılan bölgeye işaret ediyor).
1. Varsayılan olmayan bir bölgede Kerberos 'u yapılandırmak için mevcut bir Web uygulamasını genişletin.

> [!IMPORTANT]
> Kullanılacak bölge ne olursa olsun, SharePoint Web uygulamasının uygulama havuzu hesabı, Kerberos 'un düzgün çalışması için bir etki alanı hesabı olmalıdır.

### <a name="provision-the-sharepoint-web-application"></a>SharePoint Web uygulamasını sağlama

- Yeni bir Web uygulaması oluşturur ve yalnızca varsayılan bölgeyi (tercih edilen seçenek) kullanıyorsanız:

    1. **SharePoint Yönetim Kabuğu** 'nu başlatın ve aşağıdaki betiği çalıştırın:

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

    1. **SharePoint Yönetim Merkezi** sitesini açın.
    1. **Sistem ayarları**altında, **Alternatif erişim eşlemelerini Yapılandır**' ı seçin. Alternatif erişim eşlemeleri kutusu açılır.
    1. Ekranı yeni Web uygulamasıyla filtreleyin ve şuna benzer bir şey görmediğinizi onaylayın:

       ![Web uygulaması için alternatif erişim eşlemeleri](./media/application-proxy-integrate-with-sharepoint-server/new-webapp-aam.png)

- Var olan bir Web uygulamasını yeni bir bölgeye genişletirseniz (varsayılan bölgeyi kullanamazsınız):

    1. **SharePoint Yönetim Kabuğu** 'nu başlatın ve aşağıdaki betiği çalıştırın:

       ```powershell
       # This scripts extends an existing web application to Internet zone with the internal/external URL needed to work with Azure AD Application Proxy
       # Edit variables below to fit your environment
       $webAppUrl = "http://spsites/"
       $internalUrl = "https://sharepoint"
       $externalUrl = "https://spsites-demo1984.msappproxy.net/"
       
       $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$false
       $wa = Get-SPWebApplication $webAppUrl
       New-SPWebApplicationExtension -Name "SharePoint - AAD Proxy" -Identity $wa -SecureSocketsLayer -Zone Extranet -Url $externalUrl -AuthenticationProvider $winAp
       New-SPAlternateURL -Url $internalUrl -WebApplication $wa -Zone Extranet -Internal
       ```

    1. **SharePoint Yönetim Merkezi** sitesini açın.
    1. **Sistem ayarları**altında, **Alternatif erişim eşlemelerini Yapılandır**' ı seçin. Alternatif erişim eşlemeleri kutusu açılır.
    1. Genişletilmiş Web uygulamasıyla ekranı filtreleyin ve aşağıdakine benzer bir şey görürsünüz:

        ![Genişletilmiş uygulamanın alternatif erişim eşlemeleri](./media/application-proxy-integrate-with-sharepoint-server/extend-webapp-aam.png)

### <a name="ensure-that-the-sharepoint-web-application-is-running-under-a-domain-account"></a>SharePoint Web uygulamasının bir etki alanı hesabı altında çalıştığından emin olun

SharePoint Web uygulamasının uygulama havuzunu çalıştıran hesabı belirlemek ve bir etki alanı hesabı olduğundan emin olmak için aşağıdaki adımları kullanın:

1. **SharePoint Yönetim Merkezi** sitesini açın.
1. **Güvenlik** ' e gidin ve **hizmet hesaplarını yapılandır**' ı seçin.
1. **Web uygulaması havuzunu seçin-YourWebApplicationName**.

   ![Hizmet hesabı yapılandırma seçenekleri](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

1. **Bu bileşen için bir hesap seçin** ' in bir etki alanı hesabı döndürdüğünden emin olun ve bir sonraki adımda gerekli olacağı şekilde hatırlayın.

### <a name="ensure-that-an-https-certificate-is-configured-for-the-iis-site-of-the-extranet-zone"></a>Extranet bölgesinin IIS sitesi için bir HTTPS sertifikası yapılandırıldığından emin olun

Iç URL HTTPS protokolü kullandığından (`https://SharePoint/`), IIS sitesinde bir sertifika ayarlanmalıdır.

1. Windows PowerShell konsolunu açın.
1. Otomatik olarak imzalanan bir sertifika oluşturmak için aşağıdaki betiği çalıştırın ve depolama ALANıM ' a ekleyin:

   ```powershell
   # Replace "SharePoint" with the actual hostname of the Internal URL of your Azure AD proxy application
   New-SelfSignedCertificate -DnsName "SharePoint" -CertStoreLocation "cert:\LocalMachine\My"
   ```

   > [!IMPORTANT]
   > Otomatik olarak imzalanan sertifikalar yalnızca test amacıyla uygundur. Üretim ortamlarında, bunun yerine bir sertifika yetkilisi tarafından verilen sertifikaların kullanılması önemle önerilir.

1. "Internet Information Services Manager" konsolunu açın.
1. Ağaç görünümünde sunucuyu genişletin, "siteler" i genişletin, "SharePoint-AAD proxy" sitesini seçin ve **bağlamalar**' a tıklayın.
1. Https bağlamasını seçin ve **Düzenle...** seçeneğine tıklayın.
1. SSL sertifikası alanında **SharePoint** sertifikası ' nı seçin ve Tamam ' ı tıklatın.

SharePoint sitesine artık Azure AD Uygulama Ara Sunucusu aracılığıyla erişebilirsiniz.

## <a name="step-3-configure-kerberos-constrained-delegation-kcd"></a>3\. Adım: Kerberos kısıtlı temsilcisini yapılandırma (KCD)

Kullanıcılar başlangıçta Azure Active Directory, ardından Azure AD proxy Bağlayıcısı aracılığıyla Kerberos kullanarak SharePoint 'e kimlik doğrulaması yapacak. Bağlayıcının Azure Active Directory Kullanıcı adına Kerberos belirteci almasına izin vermek için, protokol geçişi ile Kerberos kısıtlanmış temsilciyi yapılandırmak gerekir. KCD hakkında daha fazla bilgi edinmek için bkz. [Kerberos kısıtlı temsilciye genel bakış](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj553400(v=ws.11)).

### <a name="set-the-service-principal-name-for-the-sharepoint-service-account"></a>SharePoint hizmet hesabı için hizmet sorumlusu adını ayarlayın

Bu makalede, iç URL `https://sharepoint` olduğundan SPN, `HTTP/sharepoint` ' dir. Bu değerleri ortamınıza karşılık gelen olanlarla değiştirmeniz gerekir.
SPN 'yi kaydetmek için-1 @no__t SharePoint uygulama havuzu hesabı için-0 @no__t, etki alanının yöneticisi olarak bir komut isteminden aşağıdaki komutu çalıştırın:

`setspn -S HTTP/sharepoint Contoso\spapppool`

Setspn Komutu, SPN 'YI eklemeden önce arar. Zaten varsa **Yinelenen BIR SPN değeri** hatası görürsünüz. Bu durumda, doğru uygulama havuzu hesabı altında ayarlanmamışsa mevcut SPN 'YI kaldırmayı göz önünde bulundurun.  
Setspn komutunu-L seçeneğiyle çalıştırarak SPN 'nin başarıyla eklendiğini doğrulayabilirsiniz. Bu komut hakkında daha fazla bilgi edinmek için bkz. [Setspn](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/cc731241(v=ws.11)).

### <a name="ensure-that-the-connector-is-trusted-for-delegation-to-the-spn-added-to-the-sharepoint-application-pool-account"></a>Bağlayıcının SharePoint uygulama havuzu hesabına eklenen SPN 'ye devredilmek için güvenilir olduğundan emin olun

KCD 'YI, Azure AD Uygulama Ara Sunucusu hizmetinin Kullanıcı kimliklerini SharePoint uygulama havuzu hesabına atayabilmesi için yapılandırın. Azure AD 'de kimlik doğrulamasından geçen kullanıcılarınız için Kerberos biletleri almak üzere uygulama proxy bağlayıcısını etkinleştirerek KCD 'yi yapılandırın. Daha sonra bu sunucu bağlamı hedef uygulamaya veya SharePoint 'i bu durumda geçirir.

KCD 'yi yapılandırmak için, her bağlayıcı makinesi için aşağıdaki adımları yineleyin:

1. Etki alanı denetleyicisinde etki alanı yöneticisi olarak oturum açın ve ardından **Active Directory Kullanıcıları ve bilgisayarları**' nı açın.
1. Azure AD proxy bağlayıcısını çalıştıran bilgisayarı bulun. Bu örnekte, SharePoint sunucusudur.
1. Bilgisayara çift tıklayın ve ardından **temsili** sekmesine tıklayın.
1. Atama ayarlarının, **Bu bilgisayara yalnızca belirtilen hizmetlere yetki vermek üzere güvenecek**şekilde ayarlandığından emin olun. Ardından **herhangi bir kimlik doğrulama protokolünü kullan**' ı seçin.
1. **Ekle** düğmesine tıklayın, **Kullanıcılar veya bilgisayarlar**' a tıklayın ve SharePoint uygulama havuzu hesabını bulun, örneğin `Contoso\spapppool`.
1. SPN 'Ler listesinde, daha önce hizmet hesabı için oluşturduğunuz birini seçin.
1. **Tamam**’a tıklayın. Değişiklikleri kaydetmek için yeniden **Tamam** ' a tıklayın.
  
   ![Temsili ayarları](./media/application-proxy-integrate-with-sharepoint-server/delegation-box2.png)

Artık dış URL 'YI kullanarak SharePoint 'te oturum açmaya ve Azure ile kimlik doğrulamaya hazırsınız.

## <a name="troubleshoot-sign-in-errors"></a>Oturum açma hatalarında sorun giderme

Sitede oturum açma çalışmıyorsa, bağlayıcı günlüklerinde sorun hakkında daha fazla bilgi edinebilirsiniz: bağlayıcıyı çalıştıran makineden, Olay görüntüleyiciyi açın, **uygulama ve hizmet günlükleri** > **Microsoft** >  **' e gidin AadApplicationProxy** > **Bağlayıcısı**ve **Yönetim** günlüğünü inceleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD Uygulama Ara Sunucusu özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)
* [Azure AD Uygulama Ara Sunucusu bağlayıcılarını anlama](application-proxy-connectors.md)
