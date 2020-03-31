---
title: B2B için bir kimlik sağlayıcısıyla doğrudan federasyon - Azure AD
description: Konukların Azure REKLAM uygulamalarınızda oturum açabilmesi için bir SAML veya WS-Fed kimlik sağlayıcısıyla doğrudan federate
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: conceptual
ms.date: 02/27/2019
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2b99a80a90df8fcfc5efe6dfa0c2cd7e8e5e04e0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80050892"
---
# <a name="direct-federation-with-ad-fs-and-third-party-providers-for-guest-users-preview"></a>KONUK KULLANıCıLAR için AD FS ve üçüncü taraf sağlayıcılarla doğrudan federasyon (önizleme)
|     |
| --- |
| Doğrudan federasyon, Azure Etkin Dizini'nin genel önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.|
|     |

Bu makalede, B2B işbirliği için başka bir kuruluşla doğrudan federasyon nasıl kurulacağı açıklanmaktadır. Kimlik sağlayıcısı (IdP) SAML 2.0 veya WS-Fed protokolünü destekleyen herhangi bir kuruluşla doğrudan federasyon kurabilirsiniz.
Bir ortağın IdP'si ile doğrudan federasyon kurduğunuzda, söz alan daki yeni konuk kullanıcılar Azure REKLAM kiracınızda oturum açmak ve sizinle işbirliği yapmaya başlamak için kendi IdP yönetilen kuruluş hesaplarını kullanabilir. Konuk kullanıcının ayrı bir Azure REKLAM hesabı oluşturmasına gerek yoktur.
> [!NOTE]
> Doğrudan federasyon konuk kullanıcıları kiracı bağlamını içeren bir bağlantı `https://myapps.microsoft.com/?tenantid=<tenant id>` kullanarak `https://portal.azure.com/<tenant id>`oturum açmalı (örneğin, veya `https://myapps.microsoft.com/\<verified domain>.onmicrosoft.com`doğrulanmış bir etki alanı durumunda). Uygulamalara ve kaynaklara doğrudan bağlantılar, kiracı bağlamını içerdiği sürece de çalışır. Doğrudan federasyon kullanıcıları şu anda kiracı bağlamı olmayan ortak uç noktaları kullanarak oturum açamıyor. Örneğin, , `https://myapps.microsoft.com` `https://portal.azure.com`, `https://teams.microsoft.com` veya bir hataya neden olur.
 
## <a name="when-is-a-guest-user-authenticated-with-direct-federation"></a>Konuk kullanıcı ne zaman doğrudan federasyon la doğrulanır?
Bir kuruluşla doğrudan federasyon kurduktan sonra, davet ettiğiniz yeni konuk kullanıcılar doğrudan federasyon kullanılarak kimlik doğrulanacaktır. Doğrudan federasyon kurmanın, daveti zaten sizden kılmış olan konuk kullanıcılar için kimlik doğrulama yöntemini değiştirmediğini unutmayın. İşte bazı örnekler:
 - Konuk kullanıcılar sizden gelen davetiyeleri zaten kullandıysa ve siz daha sonra kuruluşlarıyla doğrudan federasyon kurduysanız, bu konuk kullanıcılar doğrudan federasyon kurmadan önce kullandıkları kimlik doğrulama yöntemini kullanmaya devam edecektir.
 - Bir ortak kuruluşla doğrudan federasyon kurup konuk kullanıcıları davet ederseniz ve daha sonra ortak kuruluş Azure AD'ye taşınırsa, davetleri zaten kullanan konuk kullanıcılar doğrudan federasyon kullanmaya devam edecektir. kiracınız federasyon politikası vardır.
 - Bir ortak kuruluşla doğrudan federasyonu silerseniz, şu anda doğrudan federasyon kullanan konuk kullanıcılar oturum açamaz.

Bu senaryolardan herhangi birinde, konuk kullanıcı hesabını dizininizden silerek ve yeniden davet ederek konuk kullanıcının kimlik doğrulama yöntemini güncelleştirebilirsiniz.

Doğrudan federasyon, contoso.com ve fabrikam.com gibi etki alanı ad alanlarına bağlıdır. AD FS veya üçüncü taraf IdP ile doğrudan federasyon yapılandırması oluştururken, kuruluşlar bu IDP'lerle bir veya daha fazla alan ad alanı ilişkilendirir. 

## <a name="end-user-experience"></a>Son kullanıcı deneyimi 
Doğrudan federasyon ile konuk kullanıcılar kendi kuruluş hesaplarını kullanarak Azure AD kiracınızda oturum açın. Paylaşılan kaynaklara eriştiklerinde ve oturum açmaları istendiğinde, doğrudan federasyon kullanıcıları IdP'lerine yönlendirilir. Başarılı oturum açmadan sonra, kaynaklara erişmek için Azure AD'ye döndürülürler. Doğrudan federasyon kullanıcılarının yenileme belirteçleri, Azure AD'deki [geçiş yenileme belirteci için varsayılan uzunluk](../develop/active-directory-configurable-token-lifetimes.md#exceptions) olan 12 saat boyunca geçerlidir. Federe IdP SSO etkin varsa, kullanıcı SSO deneyimi yaşayacak ve ilk kimlik doğrulama sonra herhangi bir oturum açma istemi görmez.

## <a name="limitations"></a>Sınırlamalar

### <a name="dns-verified-domains-in-azure-ad"></a>Azure AD'de DNS tarafından doğrulanmış etki alanları
Federate istediğiniz etki alanı ***not*** Azure AD'de DNS doğrulanmamalıdır. DNS onaylı olmadıkları için yönetilmeyen (e-posta yla doğrulanmış veya "viral") Azure AD kiracılarıyla doğrudan federasyon kurabilirsiniz.

### <a name="authentication-url"></a>Kimlik doğrulama URL'si
Doğrudan federasyon, yalnızca kimlik doğrulama URL'sinin etki alanının hedef etki alanıyla eşleştiği veya kimlik doğrulama URL'sinin bu izin verilen kimlik sağlayıcılarından biri olduğu (bu liste değiştirilebilir) ilkeleri için izin verilir:
-   accounts.google.com
-   pingidentity.com
-   login.pingone.com
-   okta.com
-   my.salesforce.com
-   federation.exostar.com
-   federation.exostartest.com

Örneğin, **fabrikam.com**için doğrudan federasyon ayarlarken, `https://fabrikam.com/adfs` kimlik doğrulama URL'si doğrulamayı geçer. Örneğin, `https://sts.fabrikam.com/adfs`aynı etki alanında bulunan bir ana bilgisayar da geçer. Ancak, kimlik doğrulama `https://fabrikamconglomerate.com/adfs` `https://fabrikam.com.uk/adfs` URL'si veya aynı etki alanı için geçilmez.

### <a name="signing-certificate-renewal"></a>Sertifika yenileme imzalama
Kimlik sağlayıcısı ayarlarında meta veri URL'sini belirtirseniz, Azure AD imza sertifikasını süresi dolduğunda otomatik olarak yeniler. Ancak, sertifika son kullanma tarihinden önce herhangi bir nedenle döndürülürse veya bir meta veri URL'si sağlamazsanız, Azure AD sertifikayı yenileyemez. Bu durumda, imzalama sertifikasını el ile güncelleştirmeniz gerekir.

### <a name="limit-on-federation-relationships"></a>Federasyon ilişkilerinde sınır
Şu anda en fazla 1.000 federasyon ilişkisi desteklenmiş. Bu sınır hem [iç federasyonları](https://docs.microsoft.com/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) hem de doğrudan federasyonları içerir.

### <a name="limit-on-multiple-domains"></a>Birden çok etki alanında sınırlama
Şu anda aynı kiracıdan birden fazla etki alanı yla doğrudan federasyonu destekliyoruz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular
### <a name="can-i-set-up-direct-federation-with-a-domain-for-which-an-unmanaged-email-verified-tenant-exists"></a>Yönetilmeyen (e-posta yla doğrulanmış) bir kiracının bulunduğu bir etki alanıyla doğrudan federasyon kurabilir miyim? 
Evet. Etki alanı doğrulanmadıysa ve kiracı yönetici [devralma](../users-groups-roles/domains-admin-takeover.md)işleminden geçmediyse, bu etki alanıyla doğrudan federasyon kurabilirsiniz. Bir kullanıcı B2B davetini kullandığında veya şu anda var olmayan bir etki alanı kullanarak Azure AD için self servis kaydolduğunda, yönetilmeyen veya e-posta yla doğrulanan kiracılar oluşturulur. Bu etki alanlarıyla doğrudan federasyon kurabilirsiniz. Azure portalında veya PowerShell üzerinden DNS tarafından doğrulanmış bir etki alanıyla doğrudan federasyon kurmaya çalışırsanız, bir hata görürsünüz.
### <a name="if-direct-federation-and-email-one-time-passcode-authentication-are-both-enabled-which-method-takes-precedence"></a>Doğrudan federasyon ve e-posta bir kerelik şifre kimlik doğrulaması etkinse, hangi yöntem önceliklidir?
Bir ortak kuruluşla doğrudan federasyon kurulduğunda, bu kuruluştan yeni konuk kullanıcılar için e-posta bir kerelik parola kimlik doğrulaması üzerinde önceliklidir. Bir konuk kullanıcı, doğrudan federasyon ayarlamadan önce bir kerelik parola kimlik doğrulaması kullanarak bir davet kullandıysa, tek seferlik parola kimlik doğrulaması kullanmaya devam edin. 
### <a name="does-direct-federation-address-sign-in-issues-due-to-a-partially-synced-tenancy"></a>Kısmen senkronize edilmiş bir kira nedeniyle doğrudan federasyon oturum açma sorunlarını ele alıyor mu?
Hayır, bu senaryoda [e-posta tek seferlik parola](one-time-passcode.md) özelliği kullanılmalıdır. "Kısmen senkronize edilmiş kira", şirket içi kullanıcı kimliklerinin bulutla tam olarak eşitlenmediği bir ortak Azure AD kiracısını ifade eder. Kimliği bulutta henüz bulunmayan ancak B2B davetinizi kurtarmaya çalışan bir konuk oturum açamaz. Tek seferlik parola özelliği bu konuğun oturum açmasına izin verir. Doğrudan federasyon özelliği, konuğun kendi IdP tarafından yönetilen kuruluş hesabı nın olduğu, ancak kuruluşun Azure AD varlığı olmadığı senaryoları giderer.

## <a name="step-1-configure-the-partner-organizations-identity-provider"></a>Adım 1: Ortak kuruluşun kimlik sağlayıcısını yapılandırma
İlk olarak, ortak kuruluşunuzun kimlik sağlayıcılarını gerekli taleplerle ve güvenerek parti güvenleriyle yapılandırması gerekir. 

> [!NOTE]
> Doğrudan federasyon için bir kimlik sağlayıcısının nasıl yapılandırılabildiğini göstermek için Active Directory Federation Services'ı (AD FS) örnek olarak kullanırız. Doğrudan federasyona hazırlık olarak AD FS'nin SAML 2.0 veya WS-Fed kimlik sağlayıcısı olarak nasıl yapılandırılacaklarına örnek veren [AD FS ile doğrudan federasyonu yapılandıran](direct-federation-adfs.md)makaleye bakın.

### <a name="saml-20-configuration"></a>SAML 2.0 yapılandırması

Azure AD B2B, saml protokolünü kullanan kimlik sağlayıcılarıyla, aşağıda listelenen belirli gereksinimlerle federalete olacak şekilde yapılandırılabilir. SAML kimlik sağlayıcınız ile Azure AD arasında güven ayarlama hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-fed-saml-idp)  

> [!NOTE]
> Doğrudan federasyon için hedef etki alanı Azure AD'de DNS doğrulanmamalıdır. Kimlik doğrulama URL etki alanı hedef etki alanıyla eşleşmeli veya izin verilen bir kimlik sağlayıcısının etki alanı olmalıdır. Ayrıntılar için [Sınırlamalar](#limitations) bölümüne bakın. 

#### <a name="required-saml-20-attributes-and-claims"></a>Gerekli SAML 2.0 öznitelikleri ve talepleri
Aşağıdaki tablolar, üçüncü taraf kimlik sağlayıcısında yapılandırılması gereken belirli öznitelikler ve talepler için gereksinimleri gösterir. Doğrudan federasyon ayarlamak için, kimlik sağlayıcısından SAML 2.0 yanıtında aşağıdaki özniteliklerin alınması gerekir. Bu öznitelikler, çevrimiçi güvenlik belirteci hizmeti XML dosyasına bağlanarak veya el ile girilerek yapılandırılabilir.

IdP'den SAML 2.0 yanıtı için gerekli öznitelikler:

|Öznitelik  |Değer  |
|---------|---------|
|İddiaTüketiciServisi     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |Örneğin, ortak IdP'nin ihraççıURI'si`http://www.example.com/exk10l6w90DHM0yi...`         |


IdP tarafından yayınlanan SAML 2.0 belirteci için gerekli talepler:

|Öznitelik  |Değer  |
|---------|---------|
|NameID Biçimi     |`urn:oasis:names:tc:SAML:2.0:nameid-format:persistent`         |
|Emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

### <a name="ws-fed-configuration"></a>WS-Fed yapılandırması 
Azure AD B2B, WS-Fed protokolünü kullanan kimlik sağlayıcılarıyla, aşağıda listelenen bazı özel gereksinimlerle federate yapacak şekilde yapılandırılabilir. Şu anda, iki WS-Fed sağlayıcısı Azure AD ile uyumluluk açısından test edilmiştir AD FS ve Shibboleth içerir. Azure AD ile WS-Fed uyumlu bir sağlayıcı arasında güvenesahip bir taraf güveni oluşturma hakkında daha fazla bilgi için [Azure AD Kimlik Sağlayıcısı Uyumluluk Dokümanları'nda](https://www.microsoft.com/download/details.aspx?id=56843)bulunan "WS Protokollerini Kullanarak STS Tümleştirme Belgesi"ne bakın.

> [!NOTE]
> Doğrudan federasyon için hedef etki alanı Azure AD'de DNS doğrulanmamalıdır. Kimlik doğrulama URL etki alanı, hedef etki alanı yla veya izin verilen bir kimlik sağlayıcısının etki alanıyla eşleşmelidir. Ayrıntılar için [Sınırlamalar](#limitations) bölümüne bakın. 

#### <a name="required-ws-fed-attributes-and-claims"></a>Gerekli WS-Fed özellikleri ve talepleri

Aşağıdaki tablolar, üçüncü taraf WS-Fed kimlik sağlayıcısında yapılandırılması gereken belirli öznitelikler ve talepler için gereksinimleri gösterir. Doğrudan federasyon kurmak için, kimlik sağlayıcısından gelen WS-Fed iletisinde aşağıdaki özniteliklerin alınması gerekir. Bu öznitelikler, çevrimiçi güvenlik belirteci hizmeti XML dosyasına bağlanarak veya el ile girilerek yapılandırılabilir.

IdP'den gelen WS-Fed mesajında gerekli özellikler:
 
|Öznitelik  |Değer  |
|---------|---------|
|PasifİstekorEndpoint     |`https://login.microsoftonline.com/login.srf`         |
|Hedef kitle     |`urn:federation:MicrosoftOnline`         |
|Veren     |Örneğin, ortak IdP'nin ihraççıURI'si`http://www.example.com/exk10l6w90DHM0yi...`         |

IdP tarafından yayınlanan WS-Fed jetonu için gerekli talepler:

|Öznitelik  |Değer  |
|---------|---------|
|Değişmez ID     |`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`         |
|Emailaddress     |`http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress`         |

## <a name="step-2-configure-direct-federation-in-azure-ad"></a>Adım 2: Azure AD'de doğrudan federasyonu yapılandırma 
Ardından, Azure AD'de adım 1'de yapılandırılan kimlik sağlayıcısıyla federasyonu yapılandıracaksınız. Azure AD portalını veya PowerShell'i kullanabilirsiniz. Doğrudan federasyon politikasının yürürlüğe girmesi 5-10 dakika sürebilir. Bu süre zarfında, doğrudan federasyon etki alanı için bir daveti kullanmagirişiminde bulunmayın. Aşağıdaki öznitelikler gereklidir:
- Ortak IdP'nin ihraççı URI'si
- İş ortağı IdP'nin pasif kimlik doğrulama bitiş noktası (yalnızca https desteklenir)
- Sertifika

### <a name="to-configure-direct-federation-in-the-azure-ad-portal"></a>Azure AD portalında doğrudan federasyonu yapılandırmak için

1. [Azure portalına](https://portal.azure.com/)gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. **Kuruluş İlişkileri'ni**seçin.
3. **Kimlik sağlayıcılarını**seçin ve ardından **Yeni SAML/WS-Fed IdP'yi**seçin.

    ![Yeni bir SAML veya WS-Fed IdP eklemek için ekran görüntüsü gösterme düğmesi](media/direct-federation/new-saml-wsfed-idp.png)

4. Yeni **SAML/WS-Fed IdP** sayfasında, **Kimlik sağlayıcı protokolü** **altında, SAML** veya **WS-FED'i**seçin.

    ![SAML veya WS-Fed IdP sayfasında ayrışdırıcı düğmesini gösteren ekran görüntüsü](media/direct-federation/new-saml-wsfed-idp-parse.png)

5. Doğrudan federasyonun hedef etki alanı adı olacak ortak kuruluşunuzun etki alanı adını girin
6. Meta veri ayrıntılarını doldurmak için bir meta veri dosyası yükleyebilirsiniz. Meta verileri el ile girmeyi seçerseniz, aşağıdaki bilgileri girin:
   - İş ortağı IdP'nin alan adı
   - Ortak IdP'nin kuruluş kimliği
   - Ortak IdP'nin pasif istek veya bitiş noktası
   - Sertifika
   > [!NOTE]
   > Metaveri URL'si isteğe bağlıdır, ancak şiddetle tavsiye ediyoruz. Meta veri URL'sini sağlarsanız, Azure AD imza sertifikasını süresi dolduğunda otomatik olarak yenileyebilir. Sertifika, son kullanma tarihinden önce herhangi bir nedenle döndürülürse veya bir meta veri URL'si sağlamazsanız, Azure AD sertifikayı yenileyemez. Bu durumda, imzalama sertifikasını el ile güncelleştirmeniz gerekir.

7. **Kaydet'i**seçin. 

### <a name="to-configure-direct-federation-in-azure-ad-using-powershell"></a>PowerShell'i kullanarak Azure AD'de doğrudan federasyonu yapılandırmak için

1. Azure AD PowerShell'in en son sürümünü Grafik modülü için yükleyin[(AzureADPreview).](https://www.powershellgallery.com/packages/AzureADPreview) (Ayrıntılı adımlara ihtiyacınız varsa, konuk kullanıcı eklemek için hızlı başlangıç en [son AzureADPreview modüllerini yükleye](b2b-quickstart-invite-powershell.md#install-the-latest-azureadpreview-module)bölümünü içerir.) 
2. Şu komutu çalıştırın: 
   ```powershell
   Connect-AzureAD
   ```
1. Oturum açma isteminde, yönetilen Global Administrator hesabıyla oturum açın. 
2. Federasyon meta veri dosyasındaki değerleri değiştirerek aşağıdaki komutları çalıştırın. AD FS Server ve Okta için federasyon dosyası federationmetadata.xml'dir, örneğin: `https://sts.totheclouddemo.com/federationmetadata/2007-06/federationmetadata.xml`. 

   ```powershell
   $federationSettings = New-Object Microsoft.Open.AzureAD.Model.DomainFederationSettings
   $federationSettings.PassiveLogOnUri ="https://sts.totheclouddemo.com/adfs/ls/"
   $federationSettings.LogOffUri = $federationSettings.PassiveLogOnUri
   $federationSettings.IssuerUri = "http://sts.totheclouddemo.com/adfs/services/trust"
   $federationSettings.MetadataExchangeUri="https://sts.totheclouddemo.com/adfs/services/trust/mex"
   $federationSettings.SigningCertificate= <Replace with X509 signing cert’s public key>
   $federationSettings.PreferredAuthenticationProtocol="WsFed" OR "Samlp"
   $domainName = <Replace with domain name>
   New-AzureADExternalDomainFederation -ExternalDomainName $domainName  -FederationSettings $federationSettings
   ```

## <a name="step-3-test-direct-federation-in-azure-ad"></a>Adım 3: Azure AD'de doğrudan federasyonu test edin
Şimdi yeni bir B2B konuk kullanıcı davet ederek doğrudan federasyon kurulum test edin. Ayrıntılar için Azure [portalındaki Azure AD B2B işbirliği kullanıcıları ekleyin'e](add-users-administrator.md)bakın.
 
## <a name="how-do-i-edit-a-direct-federation-relationship"></a>Doğrudan federasyon ilişkisini nasıl edinebilirim?

1. [Azure portalına](https://portal.azure.com/)gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. **Kuruluş İlişkileri'ni**seçin.
3. **Kimlik sağlayıcılarını** seçin
4. **SAML/WS-Fed kimlik sağlayıcıları**altında sağlayıcıyı seçin.
5. Kimlik sağlayıcı ayrıntıları bölmesinde, değerleri güncelleştirin.
6. **Kaydet'i**seçin.


## <a name="how-do-i-remove-direct-federation"></a>Doğrudan federasyonu nasıl kaldıracağım?
Doğrudan federasyon kurulumunuzu kaldırabilirsiniz. Bunu yaparsanız, davetlerini zaten kullanan doğrudan federasyon konuk kullanıcıları oturum açamaz. Ancak, bunları dizinden silerek ve yeniden davet ederek kaynaklarınıza yeniden erişmelerini sağlayabilirsiniz. Azure AD portalında bir kimlik sağlayıcısıyla doğrudan federasyonu kaldırmak için:

1. [Azure portalına](https://portal.azure.com/)gidin. Sol bölmede **Azure Active Directory**’yi seçin. 
2. **Kuruluş İlişkileri'ni**seçin.
3. **Kimlik sağlayıcılarını**seçin.
4. Kimlik sağlayıcısını seçin ve sonra **Sil'i**seçin. 
5. Silme işlemini onaylamak için **Evet'i** seçin. 

PowerShell'i kullanarak bir kimlik sağlayıcısıyla doğrudan federasyonu kaldırmak için:
1. Azure AD PowerShell'in en son sürümünü Grafik modülü için yükleyin[(AzureADPreview).](https://www.powershellgallery.com/packages/AzureADPreview)
2. Şu komutu çalıştırın: 
   ```powershell
   Connect-AzureAD
   ```
3. Oturum açma isteminde, yönetilen Global Administrator hesabıyla oturum açın. 
4. Aşağıdaki komutu girin:
   ```powershell
   Remove-AzureADExternalDomainFederation -ExternalDomainName  $domainName
   ```
