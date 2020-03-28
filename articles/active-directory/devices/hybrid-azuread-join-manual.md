---
title: Hibrit Azure Active Directory'ye katılmış cihazları elle yapılandır | Microsoft Docs
description: Karma Azure Active Directory birleştirilmiş aygıtları el ile nasıl yapılandırabilirsiniz öğrenin.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: tutorial
ms.date: 05/14/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6754393bdeabcd67dcf6514102e3c825a26fc3e9
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79239087"
---
# <a name="tutorial-configure-hybrid-azure-active-directory-joined-devices-manually"></a>Öğretici: Hibrit Azure Active Directory'ye katılmış cihazları elle yapılandırma

Azure Active Directory 'deki (Azure AD) aygıt yönetimiyle, kullanıcıların kaynaklarınıza güvenlik ve uyumluluk standartlarınızı karşılayan aygıtlardan erişmelerini sağlayabilirsiniz. Daha fazla bilgi için Azure [Etkin Dizini'nde aygıt yönetimine giriş](overview.md)'e bakın.

> [!TIP]
> Azure AD Connect'i kullanmak sizin için bir seçenekse, [yönetilen](hybrid-azuread-join-managed-domains.md) veya [federe](hybrid-azuread-join-federated-domains.md) etki alanları için ilgili öğreticilere bakın. Azure AD Connect'i kullanarak, karma Azure AD birleştirme yapılandırmasını önemli ölçüde basitleştirebilirsiniz.

Şirket içi Active Directory ortamınız varsa ve etki alanınıza katılmış cihazları Azure AD'ye katmak istiyorsanız hibrit Azure AD'ye katılmış cihazları yapılandırarak bunu gerçekleştirebilirsiniz. Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Karma Azure AD birleştirme'yi el ile yapılandırma
> * Hizmet bağlantı noktası yapılandırma
> * Taleplerin verilmesini ayarlama
> * Windows alt düzey cihazlarını etkinleştirme
> * Katılmış cihazları doğrulama
> * Uygulamanızda sorun giderme

## <a name="prerequisites"></a>Ön koşullar

Bu öğretici, aşina olduğunuzu varsayar:

* [Azure Active Directory'de cihaz yönetimine giriş](../device-management-introduction.md)
* [Karma Azure Etkin Dizininizi uygulamanızı planlayın](hybrid-azuread-join-plan.md)
* [Cihazlarınızın hibrit Azure AD katılımını denetleme](hybrid-azuread-join-control.md)

Kuruluşunuzdaki karma Azure AD'nin birleştiği aygıtları etkinleştirmeye başlamadan önce şunları yaptığınızdan emin olun:

* Azure AD Connect'in güncel bir sürümünü çalıştırıyorsunuz.
* Azure AD Connect, karma Azure AD olmak istediğiniz aygıtların bilgisayar nesnelerini senkronize etti ve Azure AD'ye katıldı. Bilgisayar nesneleri belirli kuruluş birimlerine (OS) aitse, bu OS'lerin Azure AD Connect'te de eşitleme için yapılandırılması gerekir.

Azure AD Connect:

* Şirket içi Active Directory örneğinde bilgisayar hesabı ile Azure AD'deki aygıt nesnesi arasındaki ilişkilendirme tutar.
* Windows Hello for Business gibi aygıtla ilgili diğer özellikleri etkinleştirir.

Aşağıdaki URL'lere kuruluşunuzun ağındaki bilgisayarlardan erişilebildiğinden emin olun ve bilgisayarların Azure AD'ye kaydedilmesi için şunlara erişebilirsiniz:

* `https://enterpriseregistration.windows.net`
* `https://login.microsoftonline.com`
* `https://device.login.microsoftonline.com`
* Kuruluşunuzun kullanıcının yerel intranet ayarlarına dahil edilmesi gereken STS'si (federe etki alanları için)

Kuruluşunuz Sorunsuz SSO kullanmayı planlıyorsa, aşağıdaki URL'ye kuruluşunuzdaki bilgisayarlardan erişilebilmesi gerekir. Ayrıca kullanıcının yerel intranet bölgesine eklenmelidir.

* `https://autologon.microsoftazuread-sso.com`

Ayrıca kullanıcının intranet bölgesinde aşağıdaki ayarların etkinleştirilebilmesi gereklidir: "Betikle durum çubuğu güncelleştirmelerine izin ver".

Kuruluşunuz şirket içi Active Directory ile yönetilen (federe olmayan) kurulumu kullanıyorsa ve Azure AD ile federe kullanmak için Active Directory Federation Services (AD FS) kullanmıyorsa, Windows 10'daki karma Azure AD join'i Active'deki bilgisayar nesnelerine dayanır Azure AD ile eşitlenecek dizin. Azure AD Connect eşitleme yapılandırmasında eşitlemek için karma Azure AD birleştirilmiş bilgisayar nesneleri içeren tüm OS'lerin etkin olduğundan emin olun.

Sürüm 1703 veya daha önceki Sürüm 10 aygıtları için, kuruluşunuz giden bir proxy üzerinden Internet erişimi gerektiriyorsa, Windows 10 bilgisayarlarının Azure AD'ye kaydolmasını sağlamak için Web Proxy Otomatik Bulma (WPAD) uygulamanız gerekir.

Windows 10 1803 ile başlayarak, AD FS aracılığıyla federal etki alanında bulunan bir aygıtın karma Azure AD katılma girişimi başarısız olsa ve Azure AD Connect bilgisayar/aygıt nesnelerini Azure AD ile eşitleyecek şekilde yapılandırılırsa, aygıt senkronize edilmiş bilgisayarı/aygıtı kullanarak karma Azure AD birleştirmesini tamamlamaya çalışır.

Aygıtın sistem hesabı altında yukarıdaki Microsoft kaynaklarına erişip erişemeyebileceğini doğrulamak için [Test Cihazı Kaydı Bağlantı](https://gallery.technet.microsoft.com/Test-Device-Registration-3dc944c0) komut dosyasını kullanabilirsiniz.

## <a name="verify-configuration-steps"></a>Yapılandırma adımlarını doğrulama

Hibrit Azure AD'ye katılmış cihazları çeşitli türlerdeki Windows cihazı platformları için yapılandırabilirsiniz. Bu konu, tüm tipik yapılandırma senaryoları için gereken adımları içerir.  

Senaryonuz için gereken adımlara ilişkin genel bir bakış elde etmek için aşağıdaki tabloyu kullanın:  

| Adımlar | Windows geçerli ve parola karması eşitleme | Windows geçerli ve federasyon | Windows alt düzey |
| :--- | :---: | :---: | :---: |
| Hizmet bağlantı noktasını yapılandırma | ![İşaretli][1] | ![İşaretli][1] | ![İşaretli][1] |
| Taleplerin verilmesini ayarlama |     | ![İşaretli][1] | ![İşaretli][1] |
| Windows 10 olmayan cihazları etkinleştirme |       |        | ![İşaretli][1] |
| Katılmış cihazları doğrulama | ![İşaretli][1] | ![İşaretli][1] | [İşaretli][1] |

## <a name="configure-a-service-connection-point"></a>Hizmet bağlantı noktası yapılandırma

Aygıtlarınız, Kayıt sırasında Azure AD kiracı bilgilerini bulmak için bir hizmet bağlantı noktası (SCP) nesnesi kullanır. Şirket içi Active Directory örneğinde, karma Azure AD birleştirilmiş aygıtların SCP nesnesinin bilgisayarın ormanının yapılandırma adlandırma bağlamı bölmesinde bulunması gerekir. Orman başına yalnızca bir yapılandırma adlandırma bağlamı bulunur. Çok ormanlı Etkin Dizin yapılandırmasında, hizmet bağlantı noktası etki alanı birleştirilmiş bilgisayarlar içeren tüm ormanlarda bulunmalıdır.

Ormanınızın yapılandırma adlandırma bağlamını almak için [**Get-ADRootDSE**](https://technet.microsoft.com/library/ee617246.aspx) cmdlet öğesini kullanabilirsiniz.  

Active Directory etki alanı adı *fabrikam.com* olan bir orman için yapılandırma adlandırma bağlamı:

`CN=Configuration,DC=fabrikam,DC=com`

Ormanınızda, etki alanına katılmış cihazların otomatik kaydına ilişkin SCP nesnesi şu konumdadır:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Azure AD Connect'i nasıl dağıttığınız bağlı olarak, SCP nesnesi zaten yapılandırılmış olabilir.
Aşağıdaki Windows PowerShell komut dosyasını kullanarak nesnenin varlığını doğrulayabilir ve bulma değerlerini alabilirsiniz:

   ```PowerShell
   $scp = New-Object System.DirectoryServices.DirectoryEntry;

   $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

   $scp.Keywords;
   ```

**$scp. Anahtar sözcükler çıktısı** Azure AD kiracı bilgilerini gösterir. Bir örneği aşağıda verilmiştir:

   ```
   azureADName:microsoft.com
   azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47
   ```

Hizmet bağlantı noktası mevcut değilse Azure AD Connect sunucunuzda `Initialize-ADSyncDomainJoinedComputerSync` cmdlet öğesini çalıştırarak oluşturabilirsiniz. Kurumsal yönetici kimlik bilgileri bu cmdlet çalıştırmak için gereklidir.  

cmdlet:

* Azure AD Connect'in bağlı olduğu Active Directory ormanındaki servis bağlantı noktasını oluşturur.
* `AdConnectorAccount` parametresini belirtmenizi gerektirir. Bu hesap, Azure AD Connect'te Active Directory bağlayıcısı olarak yapılandırılır.


Aşağıdaki betikte, cmdlet kullanımına ilişkin bir örnek gösterilmektedir. Bu betikte, `$aadAdminCred = Get-Credential` bir kullanıcı adı yazmanızı gerektirir. Kullanıcı asıl adı (UPN) biçiminde kullanıcı adı sağlamanız gerekir (`user@example.com`).

   ```PowerShell
   Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

   $aadAdminCred = Get-Credential;

   Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;
   ```

`Initialize-ADSyncDomainJoinedComputerSync` cmdlet:

* Active Directory PowerShell modüllerini ve Azure Active Directory Etki Alanı Hizmetleri (Azure AD DS) araçlarını kullanır. Bu araçlar, etki alanı denetleyicisi üzerinde çalışan Active Directory Web Services'a dayanır. Active Directory Web Hizmetleri Windows Server 2008 R2 ve sonraki sürümleri çalıştıran etki alanı denetleyicilerinde desteklenir.
* Yalnızca MSOnline PowerShell modülü sürüm 1.1.166.0 ile desteklenir. Bu modülü indirmek için [bu bağlantıyı](https://msconfiggallery.cloudapp.net/packages/MSOnline/1.1.166.0/)kullanın.
* AD DS araçları yüklenmezse, `Initialize-ADSyncDomainJoinedComputerSync` başarısız olur. AD DS araçlarını **Özellikler** > **Remote Server Yönetim Araçları** > **Rol Yönetim Araçları**altında Server Manager aracılığıyla yükleyebilirsiniz.

Windows Server 2008 veya önceki sürümleri çalıştıran etki alanı denetleyicileri için, hizmet bağlantı noktasını oluşturmak için aşağıdaki komut dosyasını kullanın. Çok ormanlı yapılandırmada, bilgisayarların bulunduğu her ormanda servis bağlantı noktasını oluşturmak için aşağıdaki komut dosyasını kullanın.

   ```PowerShell
   $verifiedDomain = "contoso.com" # Replace this with any of your verified domain names in Azure AD
   $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47" # Replace this with you tenant ID
   $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com" # Replace this with your Active Directory configuration naming context

   $de = New-Object System.DirectoryServices.DirectoryEntry
   $de.Path = "LDAP://CN=Services," + $configNC
   $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
   $deDRC.CommitChanges()

   $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
   $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
   $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

   $deSCP.CommitChanges()
   ```

Önceki komut dosyasında, `$verifiedDomain = "contoso.com"` bir yer tutucu. Azure AD'de doğrulanmış alan adlarından biriyle değiştirin. Kullanmadan önce etki alanına sahip olmak zorundasınız.

Doğrulanmış alan adları hakkında daha fazla bilgi için [bkz.](../active-directory-domains-add-azure-portal.md)

Doğrulanmış şirket etki alanlarınızın bir listesini edinmek için [Get-AzureADDomain](/powershell/module/Azuread/Get-AzureADDomain?view=azureadps-2.0) cmdlet öğesini kullanabilirsiniz.

![Şirket etki alanları listesi](./media/hybrid-azuread-join-manual/01.png)

## <a name="set-up-issuance-of-claims"></a>Taleplerin verilmesini ayarlama

Federe bir Azure AD yapılandırmasında, aygıtlar bir Microsoft iş ortağından Azure AD'ye kimlik doğrulamasına kadar AD FS'ye veya şirket içi federasyon hizmetine güvenir. Cihazlar, Azure Active Directory Device Registration Service (Azure DRS) kullanarak kayıt için erişim belirteci almak üzere kimlik doğrulaması yapar.

Windows geçerli aygıtları, şirket içi federasyon hizmeti tarafından barındırılan etkin bir WS-Trust bitiş noktasına (1,3 veya 2005 sürümleri) Tümleşik Windows Kimlik Doğrulaması kullanılarak kimlik doğrulaması yapar.

AD FS kullanırken, aşağıdaki WS-Trust uç noktalarını etkinleştirmeniz gerekir
- `/adfs/services/trust/2005/windowstransport`
- `/adfs/services/trust/13/windowstransport`
- `/adfs/services/trust/2005/usernamemixed`
- `/adfs/services/trust/13/usernamemixed`
- `/adfs/services/trust/2005/certificatemixed`
- `/adfs/services/trust/13/certificatemixed`

> [!WARNING]
> Hem **adfs/services/trust/2005/windowstransport** veya **adfs/services/trust/13/windowstransport,** intranet uç noktalarına bakan intranet olarak etkinleştirilmeli ve Web Application Proxy aracılığıyla uç noktalara bakan extranet olarak açıklanmamalıdır. WS-Trust Windows uç noktalarını nasıl devre dışı kalacaksınız hakkında daha fazla bilgi edinmek için [proxy'deki WS-Trust Windows uç noktalarını devre dışı etme bilgisini](/windows-server/identity/ad-fs/deployment/best-practices-securing-ad-fs#disable-ws-trust-windows-endpoints-on-the-proxy-ie-from-extranet)edinin. **Service** > **Endpoints**altında AD FS yönetim konsolu aracılığıyla hangi uç noktaların etkinleştirildigini görebilirsiniz.

> [!NOTE]
>Şirket içi federasyon hizmetiniz olarak AD FS'niz yoksa, WS-Trust 1.3 veya 2005 uç noktalarını desteklediklerinden ve bunların Meta Veri Değişimi dosyası (MEX) aracılığıyla yayınlandığından emin olmak için satıcınızdan gelen talimatları izleyin.

Cihaz kaydının bitmesi için Azure DRS'nin aldığı belirteçte aşağıdaki taleplerin bulunması gerekir. Azure DRS, Azure AD'de bu bilgilerin bazılarıyla birlikte bir aygıt nesnesi oluşturur. Azure AD Connect daha sonra bu bilgileri, yeni oluşturulan aygıt nesnesini şirket içi bilgisayar hesabıyla ilişkilendirmek için kullanır.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Birden fazla doğrulanmış etki alanı adınız varsa bilgisayarlar için aşağıdaki talebi sağlamanız gerekir:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Zaten Bir ImmutableID talebi (örneğin, alternatif oturum açma kimliği) yayınediyorsanız, bilgisayarlar için karşılık gelen bir talep sağlamanız gerekir:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

Aşağıdaki bölümlerde, şu konular hakkında bilgiler edinebilirsiniz:

* Her iddianın sahip olması gereken değerler.
* AD FS'de bir tanım nasıl görünür.

Tanım, değerlerin mevcut olup olmadığını veya bunları oluşturmanızın gerekip gerekmediğini doğrulamanıza yardımcı olur.

> [!NOTE]
> Şirket içi federasyon sunucunuz için AD FS kullanmıyorsanız bu talepleri vermek üzere uygun yapılandırmayı oluşturmak için satıcınızın talimatlarını uygulayın.

### <a name="issue-account-type-claim"></a>Hesap türü talep verme

Talep, `http://schemas.microsoft.com/ws/2012/01/accounttype` aygıtı etki alanı birleştirilmiş bilgisayar olarak tanımlayan **DJ**değerini içermelidir. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

   ```
   @RuleName = "Issue account type for domain-joined computers"
   c:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(
      Type = "http://schemas.microsoft.com/ws/2012/01/accounttype",
      Value = "DJ"
   );
   ```

### <a name="issue-objectguid-of-the-computer-account-on-premises"></a>Şirket içi bilgisayar hesabının objectGUID değerini verme

Talep, `http://schemas.microsoft.com/identity/claims/onpremobjectguid` şirket içi bilgisayar hesabının **objectGUID** değerini içermelidir. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

   ```
   @RuleName = "Issue object GUID for domain-joined computers"
   c1:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$", 
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   &&
   c2:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(
      store = "Active Directory",
      types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"),
      query = ";objectguid;{0}",
      param = c2.Value
   );
   ```

### <a name="issue-objectsid-of-the-computer-account-on-premises"></a>Şirket içi bilgisayar hesabının objectSID değerini verme

Talep, `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid` şirket içi bilgisayar hesabının **objectSid** değerini içermelidir. AD FS'de, aşağıdaki gibi görünen bir verme aktarım kuralı ekleyebilirsiniz:

   ```
   @RuleName = "Issue objectSID for domain-joined computers"
   c1:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   &&
   c2:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(claim = c2);
   ```

### <a name="issue-issuerid-for-the-computer-when-multiple-verified-domain-names-are-in-azure-ad"></a>Birden çok doğrulanmış etki alanı adı Azure AD'de olduğunda bilgisayar için veren kuruluş

Talep, `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid` belirteci veren şirket içi federasyon hizmetiyle (AD FS veya ortak) bağlanan doğrulanmış alan adlarından herhangi birinin Tekdüzen Kaynak Tanımlayıcısını (URI) içermelidir. AD FS'de, önceki sıralardan sonra, belirli sırada aşağıdakilere benzeyen verme dönüştürme kuralları ekleyebilirsiniz. Kullanıcılar için kuralı açıkça vermek için bir kuralın gerekli olduğunu unutmayın. Aşağıdaki kurallarda, kullanıcı ile bilgisayar kimlik doğrulaması tanımlayan ilk kural eklenir.

   ```
   @RuleName = "Issue account type with the value User when its not a computer"
   NOT EXISTS(
   [
      Type == "http://schemas.microsoft.com/ws/2012/01/accounttype",
      Value == "DJ"
   ]
   )
   => add(
      Type = "http://schemas.microsoft.com/ws/2012/01/accounttype",
      Value = "User"
   );

   @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
   c1:[
      Type == "http://schemas.xmlsoap.org/claims/UPN"
   ]
   &&
   c2:[
      Type == "http://schemas.microsoft.com/ws/2012/01/accounttype",
      Value == "User"
   ]
   => issue(
      Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid",
      Value = regexreplace(
      c1.Value,
      ".+@(?<domain>.+)",
      "http://${domain}/adfs/services/trust/"
      )
   );

   @RuleName = "Issue issuerID for domain-joined computers"
   c:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(
      Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid",
      Value = "http://<verified-domain-name>/adfs/services/trust/"
   );
   ```

Önceki iddiada, `<verified-domain-name>` bir yer tutucu olduğunu. Azure AD'de doğrulanmış alan adlarından biriyle değiştirin. Örneğin, kullanın. `Value = "http://contoso.com/adfs/services/trust/"`

Doğrulanmış alan adları hakkında daha fazla bilgi için [bkz.](../active-directory-domains-add-azure-portal.md)  

Doğrulanmış şirket etki alanlarınızın bir listesini edinmek için [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet öğesini kullanabilirsiniz.

![Şirket etki alanları listesi](./media/hybrid-azuread-join-manual/01.png)

### <a name="issue-immutableid-for-the-computer-when-one-for-users-exists-for-example-an-alternate-login-id-is-set"></a>Kullanıcılar için bir tane olduğunda bilgisayar için Değişmez Kimlik Sorunu (örneğin, alternatif bir giriş kimliği ayarlanır)

Talep `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID` bilgisayarlar için geçerli bir değer içermelidir. AD FS'de aşağıda şekilde verme aktarım kuralı oluşturabilirsiniz:

   ```
   @RuleName = "Issue ImmutableID for computers"
   c1:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   &&
   c2:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(
      store = "Active Directory",
      types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"),
      query = ";objectguid;{0}",
      param = c2.Value
   );
   ```

### <a name="helper-script-to-create-the-ad-fs-issuance-transform-rules"></a>AD FS verme aktarım kuralları oluşturmak üzere yardımcı betik

Aşağıdaki komut dosyası, daha önce açıklanan verme dönüştürme kurallarının oluşturulmasında size yardımcı olur.

   ```
   $multipleVerifiedDomainNames = $false
   $immutableIDAlreadyIssuedforUsers = $false
   $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains

   $rule1 = '@RuleName = "Issue account type for domain-joined computers"
   c:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(
      Type = "http://schemas.microsoft.com/ws/2012/01/accounttype",
      Value = "DJ"
   );'

   $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
   c1:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   &&
   c2:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(
      store = "Active Directory",
      types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"),
      query = ";objectguid;{0}",
      param = c2.Value
   );'

   $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
   c1:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   &&
   c2:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(claim = c2);'

   $rule4 = ''
   if ($multipleVerifiedDomainNames -eq $true) {
   $rule4 = '@RuleName = "Issue account type with the value User when it is not a computer"
   NOT EXISTS(
   [
      Type == "http://schemas.microsoft.com/ws/2012/01/accounttype",
      Value == "DJ"
   ]
   )
   => add(
      Type = "http://schemas.microsoft.com/ws/2012/01/accounttype",
      Value = "User"
   );

   @RuleName = "Capture UPN when AccountType is User and issue the IssuerID"
   c1:[
      Type == "http://schemas.xmlsoap.org/claims/UPN"
   ]
   &&
   c2:[
      Type == "http://schemas.microsoft.com/ws/2012/01/accounttype",
      Value == "User"
   ]
   => issue(
      Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid",
      Value = regexreplace(
      c1.Value,
      ".+@(?<domain>.+)",
      "http://${domain}/adfs/services/trust/"
      )
   );

   @RuleName = "Issue issuerID for domain-joined computers"
   c:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(
      Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid",
      Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
   );'
   }

   $rule5 = ''
   if ($immutableIDAlreadyIssuedforUsers -eq $true) {
   $rule5 = '@RuleName = "Issue ImmutableID for computers"
   c1:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid",
      Value =~ "-515$",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   &&
   c2:[
      Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname",
      Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
   ]
   => issue(
      store = "Active Directory",
      types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"),
      query = ";objectguid;{0}",
      param = c2.Value
   );'
   }

   $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

   $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

   $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

   Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString
   ```

#### <a name="remarks"></a>Açıklamalar

* Bu betik, kuralları mevcut kurallara ekler. Komut dosyasını iki kez çalıştırmayın, çünkü kurallar kümesi iki kez eklenir. Betiği yeniden çalıştırmadan önce bu talepler için hiçbir karşılık gelen kural olmadığından emin olun (karşılık gelen koşullar bölümünde).
* Birden çok doğrulanmış alan adınız varsa (Azure AD portalında veya **Get-MsolDomain** cmdlet aracılığıyla gösterildiği gibi), komut dosyasındaki **$multipleVerifiedDomainNames** değerini **$true**ayarlayın. Ayrıca, Azure AD Connect tarafından veya başka yollarla oluşturulmuş olabilecek varolan **sorunlu** iddiayı kaldırdığınızdan emin olun. Bu kurala bir örnek aşağıda verilmiştir:

   ```
   c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
   => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 
   ```

Kullanıcı hesapları için zaten bir **ImmutableID** talebi verdiyseniz betikteki **$immutableIDAlreadyIssuedforUsers** değerini **$true** olarak ayarlayın.

## <a name="enable-windows-down-level-devices"></a>Windows alt düzey cihazlarını etkinleştirme

Bazı etki alanına katılmış cihazlar Windows alt düzey cihazlarıysa şunları gerçekleştirmeniz gerekir:

* Kullanıcıların cihazları kaydedebilmesini sağlamak için Azure AD'de bir ilke ayarlayın.
* Cihaz kaydı için Tümleşik Windows Kimlik Doğrulaması (IWA) özelliğini desteklemek üzere talepleri vermek için şirket içi federasyon hizmetinizi yapılandırın.
* Aygıtın kimlik doğruluğunu yaparken sertifika istemlerinden kaçınmak için Azure AD aygıtı kimlik doğrulama bitiş noktasını yerel intranet bölgelerine ekleyin.
* Windows alt düzey aygıtları denetleyin.

### <a name="set-a-policy-in-azure-ad-to-enable-users-to-register-devices"></a>Kullanıcıların aygıtları kaydetmesini sağlamak için Azure AD'de bir ilke ayarlama

Windows'un alt düzey aygıtlarını kaydetmek için, kullanıcıların Azure AD'de aygıtları kaydetmesine izin veren ayarın etkin olduğundan emin olun. Azure portalında, bu ayarı **Azure Etkin Dizin** > Kullanıcıları altında bulabilirsiniz ve Aygıt > **ayarlarını****grupla.**

Aşağıdaki ilke **Tümü**olarak ayarlanmalıdır : **Kullanıcılar cihazlarını Azure AD'ye kaydedebilirler.**

![Kullanıcıların cihazları kaydetmesini sağlayan Tüm düğmesi](./media/hybrid-azuread-join-manual/23.png)

### <a name="configure-the-on-premises-federation-service"></a>Şirket içi federasyon hizmetini yapılandırma

Şirket içi federasyon hizmetiniz, azure AD'ye aşağıdaki kodlanmış değere sahip bir resource_params parametresi olan bir kimlik doğrulama isteği aldığında **kimlik doğrulama yönteminin** ve **wiaormultiauthn** taleplerinin verilmesini desteklemelidir:

   ```
   eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

   which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}
   ```

Böyle bir istek geldiğinde, şirket içi federasyon hizmeti, Tümleşik Windows Kimlik Doğrulaması'nı kullanarak kullanıcının kimliğini doğrulamalıdır. Kimlik doğrulaması başarılı olduğunda, federasyon hizmeti aşağıdaki iki iddiayı yayımlamalıdır:

   `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows` `http://schemas.microsoft.com/claims/wiaormultiauthn`

AD FS'de, kimlik doğrulama yönteminden geçen bir verme dönüştürme kuralı eklemeniz gerekir. Bu kuralı eklemek için:

1. AD FS yönetim konsolunda, **AD FS** > **Trust İlişkilerI** > **Güvenerek Parti Güvenlerine**gidin.
1. Microsoft Office 365 Identity Platform bağlı olan taraf güven nesnesine sağ tıklayın ve ardından **Talep Kurallarını Düzenle** seçeneğini belirleyin.
1. **Verme Aktarım Kuralları** sekmesinde, **Kural Ekle** seçeneğini belirleyin.
1. **Talep kuralı** şablon listesinde, **Talepleri Özel Kural Kullanarak Gönder** seçeneğini belirleyin.
1. **Sonraki'ni**seçin.
1. Talep **kuralı adı** kutusuna **Auth Yöntem Talep Kuralı'nı**girin.
1. Talep **kuralı** kutusuna aşağıdaki kuralı girin:

   `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

1. Federasyon sunucunuzda aşağıdaki PowerShell komutunu girin. ** \<\> RPObjectName'i** Azure AD relying party trust object'iniz için güvenilen parti nesnesi adı ile değiştirin. Bu nesne genellikle **Microsoft Office 365 Identity Platform** olarak adlandırılır.

   `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-the-azure-ad-device-authentication-endpoint-to-the-local-intranet-zones"></a>Azure AD aygıtı kimlik doğrulama bitiş noktasını yerel intranet bölgelerine ekleme

Kayıtlı aygıtların kullanıcıları Azure AD kimliğine geldiğinde sertifika istemlerini önlemek için, Internet Explorer'daki yerel intranet bölgesine aşağıdaki URL'yi eklemek için etki alanı yla birleştirilmiş aygıtlarınıza bir ilke taşıyabilirsiniz:

`https://device.login.microsoftonline.com`

### <a name="control-windows-down-level-devices"></a>Windows alt düzey cihazlarını denetleme

Windows alt düzey cihazlarını kaydetmek için İndirme Merkezi’nden bir Windows Installer paketi (.msi) indirip yüklemeniz gerekir. Daha fazla bilgi için, [Windows alt düzey aygıtlarda karma Azure AD birleştirme bölümünün Denetimli doğrulama bölümüne](hybrid-azuread-join-control.md#controlled-validation-of-hybrid-azure-ad-join-on-windows-down-level-devices)bakın.

## <a name="verify-joined-devices"></a>Katılmış cihazları doğrulama

You can check for successfully joined devices in your organization by using the [Get-MsolDevice](/powershell/msonline/v1/get-msoldevice) cmdlet in the [Azure Active Directory PowerShell module](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

Bu cmdlet öğesinin çıktısı, Azure AD ile kaydedilmiş ve katılmış cihazları gösterir. Tüm aygıtları almak için **-Tümü** parametresini kullanın ve sonra **aygıtTrustType** özelliğini kullanarak filtreleyin. Etki alanına katılan **aygıtlar, Etki Alanı Birleştirilmiş**bir değeri vardır.

## <a name="troubleshoot-your-implementation"></a>Uygulamanızda sorun giderme

Etki alanı yla birleştirilmiş Windows aygıtları için karma Azure AD birleştirmeyi tamamlamayla ilgili sorunlar yaşıyorsanız, bkz:

* [Windows geçerli cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-current.md)
* [Windows alt düzey cihazları için Hibrit Azure AD'ye katılım sorunlarını giderme](troubleshoot-hybrid-join-windows-legacy.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Active Directory'de cihaz yönetimine giriş](overview.md)

<!--Image references-->
[1]: ./media/hybrid-azuread-join-manual/12.png
