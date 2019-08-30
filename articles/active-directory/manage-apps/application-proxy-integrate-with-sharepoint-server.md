---
title: Azure AD uygulama ara sunucusu ile SharePoint için uzaktan erişimi etkinleştir | Microsoft Docs
description: Şirket içi SharePoint server, Azure AD uygulama proxy'si ile tümleştirme hakkında temel kavramları kapsar.
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
ms.date: 08/28/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1265341ecfdb7f418ea89bb0ec848a20c6b430cd
ms.sourcegitcommit: 07700392dd52071f31f0571ec847925e467d6795
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70127678"
---
# <a name="enable-remote-access-to-sharepoint-with-azure-ad-application-proxy"></a>Azure AD uygulama ara sunucusu ile SharePoint uzaktan erişimi etkinleştirme

Bu makalede, şirket içi SharePoint server, Azure Active Directory (Azure AD) uygulama proxy'si ile tümleştirme nasıl ele alınmaktadır.

Azure AD uygulama ara sunucusu ile SharePoint için uzaktan erişimi etkinleştirmek için adım adım bu makaledeki bölümler izleyin.

## <a name="prerequisites"></a>Önkoşullar

Bu makalede, ortamınızda SharePoint 2013 veya daha yeni zaten sahibi olduğunuzu varsayar. Ayrıca, aşağıdaki önkoşulları göz önünde bulundurun:

* SharePoint yerel Kerberos desteği içerir. Bu nedenle, iç sitelere Azure AD uygulama proxy'si aracılığıyla uzaktan erişen kullanıcılar, bir çoklu oturum açma (SSO) deneyimi sağlamak için kabul edilebilir.
* Bu senaryo, SharePoint Server'ınıza yapılandırma değişiklikleri içerir. Bir hazırlık ortamı kullanmanızı öneririz. Bu şekilde güncelleştirmeleri hazırlama sunucunuza ilk olun ve sonra üretime geçmeden önce bir test döngüsünü kolaylaştırmak.
* Yayımlanmış URL'sini SSL zorunlu kılarız. Bağlantıların doğru gönderildiğinden/eşlendiğinden emin olmak için iç URL 'de SSL de gereklidir.

> [!NOTE]
> En iyi uygulama olarak, mümkün olan her durumda özel etki alanları kullanın. Özel bir etki alanı ile, hem Iç URL hem de dış URL için aynı URL 'YI yapılandırabilirsiniz. Daha sonra aynı bağlantı, uygulamaya ağınızın içinden veya dışından erişmek için de kullanılabilir. Bu yapılandırma, uygulamanıza erişmesi gereken kullanıcılar ve diğer uygulamalar için deneyimi en iyi duruma getirir. [Azure AD uygulama ara sunucusu özel etki alanlarıyla çalışma](application-proxy-configure-custom-domain.md)hakkında daha fazla bilgi edinin.

## <a name="step-1-configure-kerberos-constrained-delegation-kcd"></a>1\. adım: Kerberos kısıtlanmış temsilciyi yapılandırma (KCD)

Windows kimlik doğrulaması kullanan şirket içi uygulamalar için çoklu oturum açma (SSO) ile Kerberos kimlik doğrulama protokolünü ve Kerberos Kısıtlı temsilci (KCD) adlı bir özellik elde edebilirsiniz. Kullanıcı için Windows doğrudan oturum henüz bile yapılandırıldığında, KCD, bir kullanıcı için bir Windows belirteç almak uygulama Proxy Bağlayıcısı sağlar. KCD hakkında daha fazla bilgi için bkz: [Kerberos Kısıtlı temsilci seçmeye genel bakış](https://technet.microsoft.com/library/jj553400.aspx).

KCD ' için bir SharePoint sunucusu ayarlamak için aşağıdaki sıralı bölümlerdeki yordamları kullanın:

### <a name="ensure-that-sharepoint-web-application-is-running-under-a-domain-account"></a>SharePoint Web uygulamasının bir etki alanı hesabı altında çalıştığından emin olun

İlk olarak, SharePoint Web uygulamasının, yerel sistem, yerel hizmet veya ağ hizmeti olmayan bir etki alanı hesabı altında çalıştığından emin olun. Bu hesaba hizmet asıl adları (SPN 'Ler) iliştirebilmeniz için bunu yapın. SPN'ler Kerberos protokolü farklı hizmetleri nasıl tanımlar var. Ve daha sonra KCD yapılandırmak için hesabı gerekir.

> [!NOTE]
> Önceden oluşturulmuş olması gerekir hizmeti için Azure AD hesabı. Bir otomatik parola değişikliği için izin öneririz. Tüm adım ve sorun giderme sorunları hakkında daha fazla bilgi için bkz. [SharePoint 'te otomatik parola değişikliğini yapılandırma](https://technet.microsoft.com/library/ff724280.aspx).

Sitelerinizi bir tanımlı bir hizmet hesabı altında çalıştığından emin olun için aşağıdaki adımları gerçekleştirin:

1. **SharePoint Yönetim Merkezi** sitesini açın.
1. Git **güvenlik** seçip **hizmet hesaplarını yapılandırın**.
1. Seçin **Web uygulama havuzu - SharePoint - 80**. Seçenekler, web havuzu adına göre biraz farklı olabilir veya web havuzu varsayılan olarak SSL kullanır.

   ![Bir hizmet hesabı yapılandırma seçenekleri](./media/application-proxy-integrate-with-sharepoint-server/service-web-application.png)

1. Varsa **bu bileşen için bir hesap seçersiniz** ayarlanmış **yerel hizmet** veya **ağ hizmeti**, bir hesap oluşturmanız gerekir. Değilse, tamamlanmış ve sonraki bölüme geçebilirsiniz.
1. Seçin **kaydı yeni yönetilen hesabı**. Hesabınızı oluşturduktan sonra ayarlamalısınız **Web uygulama havuzu** hesabı kullanabilmeniz için önce.

### <a name="set-a-service-principal-name-for-the-sharepoint-service-account"></a>SharePoint hizmet hesabı için bir hizmet asıl adı ayarlayın

KCD 'yi yapılandırmadan önce şunları yapmanız gerekir:

* Azure AD proxy 'nin kullanıma sunabilmesi için SharePoint Web uygulamasını çalıştıran etki alanı hesabını belirler.
* Azure AD proxy ve SharePoint 'te yapılandırılacak Iç URL 'yi seçin. Bu Iç URL Web uygulamasında henüz kullanılmamalıdır ve Web tarayıcısında hiçbir şekilde görünmez.

Seçilen <https://sharepoint>iç URL 'nin olduğu varsayıldığında SPN şu şekilde olur:

```
HTTP/SharePoint
```

> [!NOTE]
> İç URL için aşağıdaki önerileri izleyin:
> * HTTPS Kullan
> * Özel bağlantı noktalarını kullanma
> * DNS 'de, bir diğer ad (CName) değil, SharePoint WFE 'yi (veya yük dengeleyiciyi) göstermek için bir konak (A) oluşturun

Bu SPN 'yi kaydetmek için, komut isteminden etki alanının yöneticisi olarak aşağıdaki komutu çalıştırın:

```
setspn -S HTTP/SharePoint demo\spAppPoolAccount
```

Bu komut, _Demo\spAppPoolAccount_SharePoint uygulama havuzu HESABı Için SPN _http/SharePoint_ 'i ayarlar.

_Http/SharePoint_ 'ı iç URL 'NIZ için SPN ile, ortamınızdaki uygulama havuzu hesabıyla _demo\spAppPoolAccount_ değiştirin. Bunu eklemeden önce SPN'yi Setspn komut arar. Zaten mevcut olduğundan **Yinelenen BIR SPN değeri** hatası görürsünüz. Bu durumda, doğru uygulama havuzu hesabı altında ayarlanmamışsa mevcut SPN 'yi kaldırmayı göz önünde bulundurun.

Setspn komutunu-L seçeneğiyle çalıştırarak SPN 'nin eklendiğini doğrulayabilirsiniz. Bu komut hakkında daha fazla bilgi için bkz. [Setspn](https://technet.microsoft.com/library/cc731241.aspx).

### <a name="ensure-that-the-connector-is-trusted-for-delegation-to-the-spn-added-to-the-sharepoint-application-pool-account"></a>Bağlayıcının SharePoint uygulama havuzu hesabına eklenen SPN 'ye devredilmek için güvenilir olduğundan emin olun

KCD 'YI, Azure AD Uygulama Ara Sunucusu hizmetinin Kullanıcı kimliklerini SharePoint uygulama havuzu hesabına atayabilmesi için yapılandırın. KCD, kullanıcılarınız Azure AD'de kimlik doğrulaması için Kerberos anahtarlarını almak uygulama Proxy Bağlayıcısı'nı etkinleştirerek yapılandırın. Daha sonra o sunucu bağlamı hedef uygulama ya da SharePoint için bu durumda geçirir.

KCD yapılandırmak için her bir bağlayıcı makine için aşağıdaki adımları yineleyin:

1. Bir DC için bir etki alanı yöneticisi olarak oturum açın ve ardından açın **Active Directory Kullanıcıları ve Bilgisayarları**.
1. Bağlayıcıyı çalıştıran bilgisayarı bulmak. Bu örnekte, aynı SharePoint sunucusudur.
1. Bilgisayarı çift tıklatın ve ardından **temsilci** sekmesi.
1. Temsilci seçme ayarları ayarlandığından emin olun **bu bilgisayara yalnızca belirtilen hizmetlere temsilci seçmek için güven**. Ardından, **herhangi bir kimlik doğrulama protokolünü kullan**.
1. **Ekle** düğmesine tıklayın, **Kullanıcılar veya bilgisayarlar**' a tıklayın ve SharePoint uygulama havuzu hesabını bulun, örneğin _demo\spAppPoolAccount_.
1. SPN'ler listesinde, daha önce oluşturduğunuz hizmet hesabı için bir tane seçin.
1. **Tamam** düğmesine tıklayın. Tıklayın **Tamam** yeniden değişiklikleri kaydedin.
  
   ![Temsilci seçme ayarları](./media/application-proxy-integrate-with-sharepoint-server/delegation-box2.png)

## <a name="step-2-configure-azure-ad-proxy"></a>2\. adım: Azure AD proxy 'yi yapılandırma

KCD 'yi yapılandırdığınıza göre, Azure AD Uygulama Ara Sunucusu 'yi yapılandırmaya hazırsınız demektir.

1. Aşağıdaki ayarlar ile SharePoint sitenizi yayımlayın. Adım adım yönergeler için bkz: [Azure AD uygulama proxy'si kullanarak uygulamaları yayımlama](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad).
   * **Iç URL**: Daha önce **<https://SharePoint/>** seçilmiş olan SharePoint iç URL 'si.
   * **Ön kimlik doğrulama yöntemi**: Azure Active Directory
   * **Üst bilgilerdeki URL 'Yi çevir**: NO

   > [!TIP]
   > SharePoint kullanan _ana bilgisayar üst bilgisini_ siteyi aramak için değer. Ayrıca bu değere göre bağlantılar oluşturur. Net etkisiyle SharePoint oluşturan herhangi bir bağlantıyı dış URL'sini kullanmak üzere doğru şekilde yayımlanmış bir URL olmasıdır. Değerini **Evet** Ayrıca arka uç uygulaması isteği iletmek bağlayıcıyı etkinleştirir. Ancak, ayar değeri **Hayır** bağlayıcı iç ana bilgisayar adını göndermez anlamına gelir. Bunun yerine, bağlayıcıyı arka uç uygulaması için yayımlanan URL olarak ana bilgisayar üst bilgisi gönderir.

   ![SharePoint uygulaması olarak Yayımla](./media/application-proxy-integrate-with-sharepoint-server/publish-app.png)

1. Uygulamanızı yayımladıktan sonra aşağıdaki adımlarla çoklu oturum açma ayarları yapılandırın:

   1. Uygulama sayfasında portalında seçin **çoklu oturum açma**.
   1. Çoklu oturum açma modu için seçin **tümleşik Windows kimlik doğrulaması**.
   1. İç uygulama SPN'si daha önce ayarlanan değere ayarlayın. Bu örnekte, **http/SharePoint**olacaktır.
   1. "Temsilci oturum kimliği" içinde, Active Directory orman yapılandırmanız için en uygun seçeneği belirleyin. Örneğin, ormanınızda tek bir AD etki alanınız varsa Şirket **ıçı Sam hesap adı** ' nı (aşağıda gösterildiği gibi) seçin, ancak kullanıcılarınız SharePoint ile aynı etki alanında değilse ve uygulama proxy Bağlayıcısı sunucuları için şirket **içi Kullanıcı asıl adı ' nı seçin.** (gösterilmez).

   ![SSO için tümleşik Windows kimlik doğrulamasını yapılandırma](./media/application-proxy-integrate-with-sharepoint-server/configure-iwa.png)

1. Uygulamanızı ayarlama işlemini sonlandırmak için şuraya gidin: **kullanıcılar ve gruplar** bölümünde ve bu uygulamaya erişmek için kullanıcı atama. 

## <a name="step-3-configure-sharepoint-to-use-kerberos-and-azure-ad-proxy-urls"></a>3\. adım: SharePoint 'i Kerberos ve Azure AD proxy URL 'Leri kullanacak şekilde yapılandırma

Sonraki adım, SharePoint Web uygulamasını yeni bir bölgeye genişletmenin yanı sıra, SharePoint 'in Iç URL 'ye gönderilen gelen istekleri işlemesini ve dış URL için oluşturulan bağlantılarla yanıt vermesini sağlamak için uygun alternatif erişim eşlemesi.

1. **SharePoint Yönetim Kabuğu**'nu başlatın.
1. Web uygulamasını extranet bölgesine genişletmek ve Kerberos kimlik doğrulamasını etkinleştirmek için aşağıdaki betiği çalıştırın:

   ```powershell
   # Replace "http://spsites/" with the URL of your web application
   # Replace "https://sharepoint-f128.msappproxy.net/" with the External URL in your Azure AD proxy application
   $winAp = New-SPAuthenticationProvider -UseWindowsIntegratedAuthentication -DisableKerberos:$false
   Get-SPWebApplication "http://spsites/" | New-SPWebApplicationExtension -Name "SharePoint - AAD Proxy" -SecureSocketsLayer -Zone "Extranet" -Url "https://sharepoint-f128.msappproxy.net/" -AuthenticationProvider $winAp
   ```

1. **SharePoint Yönetim Merkezi** sitesini açın.
1. Altında **sistem ayarlarını**seçin **alternatif erişim eşlemelerini yapılandırma**. Alternatif erişim eşlemeleri kutusu açılır.
1. Sitenizi (örneğin, **SharePoint-80**) seçin. Bu durumda, extranet bölgesinde Iç URL henüz doğru şekilde ayarlanmadı:

   ![Alternatif erişim eşlemeleri kutusunu gösterir](./media/application-proxy-integrate-with-sharepoint-server/alternate-access1.png)

1. **Iç URL Ekle**' ye tıklayın.
1. **URL protokolü, ana bilgisayar ve bağlantı noktası** metin kutusunda, Azure AD proxy 'de YAPıLANDıRıLMıŞ **İç URL** 'yi yazın, <https://SharePoint/>örneğin.
1. Açılan listede bölge **extranet** ' i seçin.
1. **Kaydet**’e tıklayın.
1. Alternatif erişim eşlemeleri şu şekilde görünmelidir:

    ![Alternatif erişim eşlemelerini düzeltin](./media/application-proxy-integrate-with-sharepoint-server/alternate-access3.png)

## <a name="step-4-ensure-that-an-https-certificate-is-configured-for-the-iis-site-of-the-extranet-zone"></a>4\. Adım: Extranet bölgesinin IIS sitesi için bir HTTPS sertifikası yapılandırıldığından emin olun

SharePoint yapılandırması tamamlandı, ancak extranet bölgesinin iç URL 'si olduğundan <https://SharePoint/>, bu site için bir sertifika ayarlanması gerekiyor.

1. Windows PowerShell konsolunu açın.
1. Otomatik olarak imzalanan bir sertifika oluşturmak için aşağıdaki betiği çalıştırın ve depolama ALANıM ' a ekleyin:

   ```powershell
   # Replace "SharePoint" with the actual hostname of the Internal URL of your Azure AD proxy application
   New-SelfSignedCertificate -DnsName "SharePoint" -CertStoreLocation "cert:\LocalMachine\My"
   ```

   > [!NOTE]
   > Otomatik olarak imzalanan sertifikalar yalnızca test amacıyla uygundur. Üretim ortamlarında, bunun yerine bir sertifika yetkilisi tarafından verilen sertifikaların kullanılması önemle önerilir.

1. "Internet Information Services Manager" konsolunu açın.
1. Ağaç görünümünde sunucuyu genişletin, "siteler" i genişletin, "SharePoint-AAD proxy" sitesini seçin ve **bağlamalar**' a tıklayın.
1. Https bağlamasını seçin ve **Düzenle...** seçeneğine tıklayın.
1. SSL sertifikası alanında **SharePoint** sertifikası ' nı seçin ve Tamam ' ı tıklatın.

Artık Azure AD uygulama proxy'si aracılığıyla harici olarak SharePoint sitesine erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure AD uygulama proxy'sinde özel etki alanları ile çalışma](application-proxy-configure-custom-domain.md)
* [Azure AD uygulama ara sunucusu bağlayıcıları anlama](application-proxy-connectors.md)
