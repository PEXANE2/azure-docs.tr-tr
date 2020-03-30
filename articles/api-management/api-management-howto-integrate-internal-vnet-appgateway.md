---
title: Uygulama Ağ Geçidi ile Sanal Ağ'da API Yönetimi nasıl kullanılır?
titleSuffix: Azure API Management
description: Azure API Yönetimini Internal Virtual Network'te Application Gateway (WAF) ile FrontEnd olarak nasıl ayarlayıp yapılandıracağız öğrenin
services: api-management
documentationcenter: ''
author: solankisamir
manager: kjoshi
editor: vlvinogr
ms.assetid: a8c982b2-bca5-4312-9367-4a0bbc1082b1
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/04/2019
ms.author: sasolank
ms.openlocfilehash: 2b8cf66afa1d8aa592d5755ebab70cd6ad2e75fd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79298071"
---
# <a name="integrate-api-management-in-an-internal-vnet-with-application-gateway"></a>API Yönetimini Uygulama Ağ Geçidi ile dahili bir VNET'e entegre edin

## <a name="overview"></a><a name="overview"> </a> Genel Bakış

API Yönetimi hizmeti, bir Sanal Ağ'da dahili modda yapılandırılabilir ve bu da yalnızca Sanal Ağ içinden erişilebilir olmasını sağlar. Azure Application Gateway, Katman-7 yük dengeleyicisi sağlayan bir PAAS Hizmetidir. Bir ters proxy hizmeti olarak görür ve bir Web Uygulama Güvenlik Duvarı (WAF) sunan arasında sağlar.

Dahili bir VNET'te sağlanan API Yönetimi ile Application Gateway ön uçunun birleştirilmesi aşağıdaki senaryoları sağlar:

* Hem dahili tüketiciler hem de dış tüketiciler tarafından tüketim için aynı API Yönetimi kaynağını kullanın.
* Tek bir API Yönetimi kaynağı kullanın ve harici tüketiciler için KULLANıLABILIR API Yönetimi'nde tanımlanmış bir API alt kümesine sahip.
* API Yönetimi'ne erişimi herkese açık ve kapalı olarak genel Internet'ten değiştirmek için anahtar teslimi bir yol sağlayın.

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu makalede açıklanan adımları izlemek için aşağıdakilere sahip olmalısınız:

* Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

* Sertifikalar - api hostname için pfx ve cer ve geliştirici portalının ana adı için pfx.

## <a name="scenario"></a><a name="scenario"> </a> Senaryo

Bu makalede, hem dahili hem de harici tüketiciler için tek bir API Yönetimi hizmetinin nasıl kullanılacağı ve hem şirket içi hem de bulut API'leri için tek bir ön taraf olarak nasıl hareket edilebiz. Uygulama Ağ Geçidi'nde kullanılabilir yönlendirme işlevini kullanarak Harici Tüketim için API'lerinizin yalnızca bir alt kümesini (yeşil olarak vurgulanan örnekte) nasıl ortaya çıkaracağınızı da göreceksiniz.

İlk kurulum örneğinde tüm API'leriniz yalnızca Sanal Ağınızdan yönetilir. Dahili tüketiciler (turuncu renkle vurgulanır) tüm dahili ve harici API'lerinize erişebilir. Trafik asla internete gitmez. Yüksek performanslı bağlantı Express Route devreleri üzerinden gerçekleştirilir.

![url rotası](./media/api-management-howto-integrate-internal-vnet-appgateway/api-management-howto-integrate-internal-vnet-appgateway.png)

## <a name="before-you-begin"></a><a name="before-you-begin"> </a> Başlamadan önce

* Azure PowerShell’in en yeni sürümünü kullandığınızdan emin olun. [Azure PowerShell'i Yükle'deki](/powershell/azure/install-az-ps)yükleme yönergelerine bakın. 

## <a name="what-is-required-to-create-an-integration-between-api-management-and-application-gateway"></a>API Yönetimi ve Uygulama Ağ Geçidi arasında tümleştirme oluşturmak için ne gerekir?

* **Arka uç sunucu havuzu:** Bu, API Yönetimi hizmetinin dahili sanal IP adresidir.
* **Arka uç sunucu havuzu ayarları**: Her havuzun bağlantı noktası, protokol ve tanımlama bilgisi temelli benzeşim gibi ayarları vardır. Bu ayarlar havuz içindeki tüm sunuculara uygulanır.
* **Ön uç bağlantı noktası:** Bu, uygulama ağ geçidinde açılan ortak bağlantı noktasıdır. Trafik isabet arka uç sunucularından birine yönlendirilir alır.
* **Dinleyici:** Dinleyicide bir ön uç bağlantı noktası, bir protokol (Http veya Https, bu değerler büyük/küçük harfe duyarlıdır) ve SSL sertifika adı (SSL yük boşaltımı yapılandırılıyorsa) vardır.
* **Kural:** Kural, dinleyiciyi arka uç sunucu havuzuna bağlar.
* **Özel Sağlık Sondası:** Uygulama Ağ Geçidi, varsayılan olarak, BackendAddressPool'daki hangi sunucuların etkin olduğunu anlamak için IP adresi tabanlı probları kullanır. API Yönetimi hizmeti yalnızca doğru ana bilgisayar üstbilgisiyle istekleri yanıtlar, bu nedenle varsayılan sondalar başarısız olur. Uygulama ağ geçidinin hizmetin canlı olduğunu belirlemesine ve istekleri iletmesi gerektiğine yardımcı olmak için özel bir sistem durumu sondasının tanımlanması gerekir.
* **Özel etki alanı sertifikaları:** API Yönetimi'ne internetten erişmek için, ana bilgisayar adının Uygulama Ağ Geçidi ön uç DNS adına cname eşlemesi oluşturmanız gerekir. Bu, API Yönetimi'ne iletilen Uygulama Ağ Geçidi'ne gönderilen ana bilgisayar adı üstbilgisinin ve sertifikasının APIM'nin geçerli olarak tanıyabileceği bir üstbilgi olmasını sağlar. Bu örnekte, arka uç ve geliştirici portalı için iki sertifika kullanacağız.  

## <a name="steps-required-for-integrating-api-management-and-application-gateway"></a><a name="overview-steps"> </a> API Yönetimi ve Uygulama Ağ Geçidi'ni tümleştirmek için gereken adımlar

1. Resource Manager için kaynak grubu oluşturun.
2. Uygulama Ağ Geçidi için sanal ağ, alt ağ ve genel IP oluşturun. API Yönetimi için başka bir alt ağ oluşturun.
3. Yukarıda oluşturulan VNET alt ağı içinde bir API Yönetimi hizmeti oluşturun ve Dahili modunu kullandığınızdan emin olun.
4. API Yönetimi hizmetinde özel bir etki alanı adı ayarlayın.
5. Bir Uygulama Ağ Geçidi yapılandırma nesnesi oluşturun.
6. Uygulama Ağ Geçidi kaynağı oluşturun.
7. UYGULAMA Ağ Geçidi'nin ortak DNS adından API Yönetimi proxy ana adı için bir CNAME oluşturun.

## <a name="exposing-the-developer-portal-externally-through-application-gateway"></a>Uygulama Ağ Geçidi aracılığıyla geliştirici portalını harici olarak teşhir etme

Bu kılavuzda, geliştirici **portalını** Uygulama Ağ Geçidi aracılığıyla dış kitlelere de sunacağız. Geliştirici portalının dinleyicisi, sondası, ayarları ve kuralları oluşturmak için ek adımlar gerektirir. Tüm ayrıntılar ilgili adımlarda sağlanır.

> [!WARNING]
> Azure AD veya üçüncü taraf kimlik doğrulaması kullanıyorsanız, lütfen Uygulama Ağ Geçidi'nde [çerez tabanlı oturum yakınlığı](../application-gateway/features.md#session-affinity) özelliğini etkinleştirin.

> [!WARNING]
> Uygulama Ağ Geçidi WAF'ın geliştirici portalında OpenAPI belirtimini karşıdan yüklemesini engellemesini önlemek için güvenlik duvarı kuralını `942200 - "Detects MySQL comment-/space-obfuscated injections and backtick termination"`devre dışı kesmeniz gerekir.

## <a name="create-a-resource-group-for-resource-manager"></a>Resource Manager için kaynak grubu oluşturun

### <a name="step-1"></a>1. Adım

Azure'da oturum açma

```powershell
Connect-AzAccount
```

Kimlik bilgilerinizle kimlik doğrulaması.

### <a name="step-2"></a>2. Adım

İstediğin aboneliği seçin.

```powershell
$subscriptionId = "00000000-0000-0000-0000-000000000000" # GUID of your Azure subscription
Get-AzSubscription -Subscriptionid $subscriptionId | Select-AzSubscription
```

### <a name="step-3"></a>3. Adım

Bir kaynak grubu oluşturun (mevcut bir kaynak grubu kullanıyorsanız bu adımı atlayın).

```powershell
$resGroupName = "apim-appGw-RG" # resource group name
$location = "West US"           # Azure region
New-AzResourceGroup -Name $resGroupName -Location $location
```

Azure Resource Manager, tüm kaynak gruplarının bir konum belirtmesini gerektirir. Bu, kaynak grubunda kaynaklar için varsayılan konum olarak kullanılır. Bir uygulama ağ geçidi oluşturmak için tüm komutların aynı kaynak grubunu kullandığından emin olun.

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Uygulama ağ geçidi için sanal ağ ve bir alt ağ oluşturma

Aşağıdaki örnek, Kaynak Yöneticisi'ni kullanarak sanal ağ oluşturmanın nasıl yapılacağını gösterir.

### <a name="step-1"></a>1. Adım

Sanal Ağ oluştururken Uygulama Ağ Geçidi için kullanılacak alt net değişkenine 10.0.0.0/24 adres aralığını atayın.

```powershell
$appgatewaysubnet = New-AzVirtualNetworkSubnetConfig -Name "apim01" -AddressPrefix "10.0.0.0/24"
```

### <a name="step-2"></a>2. Adım

Sanal Ağ oluştururken API Yönetimi için kullanılacak alt net değişkenine 10.0.1.0/24 adres aralığını atayın.

```powershell
$apimsubnet = New-AzVirtualNetworkSubnetConfig -Name "apim02" -AddressPrefix "10.0.1.0/24"
```

### <a name="step-3"></a>3. Adım

Batı ABD bölgesi için kaynak grubu **apim-appGw-RG'de** **appgwvnet** adında bir Sanal Ağ oluşturun. 10.0.0.0/16 önekini 10.0.0.0/24 ve 10.0.1.0/24 alt ağları ile kullanın.

```powershell
$vnet = New-AzVirtualNetwork -Name "appgwvnet" -ResourceGroupName $resGroupName -Location $location -AddressPrefix "10.0.0.0/16" -Subnet $appgatewaysubnet,$apimsubnet
```

### <a name="step-4"></a>4. Adım

Sonraki adımlar için bir alt ağ değişkeni atama

```powershell
$appgatewaysubnetdata = $vnet.Subnets[0]
$apimsubnetdata = $vnet.Subnets[1]
```

## <a name="create-an-api-management-service-inside-a-vnet-configured-in-internal-mode"></a>Dahili modda yapılandırılan bir VNET içinde BIR API Yönetimi hizmeti oluşturma

Aşağıdaki örnek, yalnızca dahili erişim için yapılandırılmış bir VNET'te bir API Yönetimi hizmetinin nasıl oluşturuluredildiğini gösterir.

### <a name="step-1"></a>1. Adım

Yukarıda oluşturulan alt $apimsubnetdata kullanarak bir API Yönetimi Sanal Ağ nesnesi oluşturun.

```powershell
$apimVirtualNetwork = New-AzApiManagementVirtualNetwork -SubnetResourceId $apimsubnetdata.Id
```

### <a name="step-2"></a>2. Adım

Sanal Ağ içinde bir API Yönetimi hizmeti oluşturun.

```powershell
$apimServiceName = "ContosoApi"       # API Management service instance name
$apimOrganization = "Contoso"         # organization name
$apimAdminEmail = "admin@contoso.com" # administrator's email address
$apimService = New-AzApiManagement -ResourceGroupName $resGroupName -Location $location -Name $apimServiceName -Organization $apimOrganization -AdminEmail $apimAdminEmail -VirtualNetwork $apimVirtualNetwork -VpnType "Internal" -Sku "Developer"
```

Yukarıdaki komut başarılı olduktan sonra, iç [VNET API Yönetimi hizmetine erişmek için gereken DNS Yapılandırmasına](api-management-using-with-internal-vnet.md#apim-dns-configuration) bakın. Bu adım yarım saatten fazla sürebilir.

## <a name="set-up-a-custom-domain-name-in-api-management"></a>API Yönetimi'nde özel bir etki alanı adı ayarlama

> [!IMPORTANT]
> [Yeni geliştirici portalı,](api-management-howto-developer-portal.md) aşağıdaki adımlara ek olarak API Yönetimi'nin yönetim bitiş noktasına bağlantı sağlamayı da gerektirir.

### <a name="step-1"></a>1. Adım

Aşağıdaki değişkenleri, etki alanları için özel anahtarlarla sertifikaların ayrıntılarıyla birlikte başlangıç olarak adlandırır. Bu örnekte, biz `api.contoso.net` `portal.contoso.net`kullanacağız ve .  

```powershell
$gatewayHostname = "api.contoso.net"                 # API gateway host
$portalHostname = "portal.contoso.net"               # API developer portal host
$gatewayCertCerPath = "C:\Users\Contoso\gateway.cer" # full path to api.contoso.net .cer file
$gatewayCertPfxPath = "C:\Users\Contoso\gateway.pfx" # full path to api.contoso.net .pfx file
$portalCertPfxPath = "C:\Users\Contoso\portal.pfx"   # full path to portal.contoso.net .pfx file
$gatewayCertPfxPassword = "certificatePassword123"   # password for api.contoso.net pfx certificate
$portalCertPfxPassword = "certificatePassword123"    # password for portal.contoso.net pfx certificate

$certPwd = ConvertTo-SecureString -String $gatewayCertPfxPassword -AsPlainText -Force
$certPortalPwd = ConvertTo-SecureString -String $portalCertPfxPassword -AsPlainText -Force
```

### <a name="step-2"></a>2. Adım

Proxy ve portal için ana bilgisayar yapılandırma nesnelerini oluşturun ve ayarlayın.  

```powershell
$proxyHostnameConfig = New-AzApiManagementCustomHostnameConfiguration -Hostname $gatewayHostname -HostnameType Proxy -PfxPath $gatewayCertPfxPath -PfxPassword $certPwd
$portalHostnameConfig = New-AzApiManagementCustomHostnameConfiguration -Hostname $portalHostname -HostnameType DeveloperPortal -PfxPath $portalCertPfxPath -PfxPassword $certPortalPwd

$apimService.ProxyCustomHostnameConfiguration = $proxyHostnameConfig
$apimService.PortalCustomHostnameConfiguration = $portalHostnameConfig
Set-AzApiManagement -InputObject $apimService
```

> [!NOTE]
> Eski geliştirici portal bağlantısını yapılandırmak için `-HostnameType DeveloperPortal` `-HostnameType Portal`' ile değiştirmeniz gerekir.

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Kaynak grubunda genel BIR IP kaynağı **publicIP01** oluşturun.

```powershell
$publicip = New-AzPublicIpAddress -ResourceGroupName $resGroupName -name "publicIP01" -location $location -AllocationMethod Dynamic
```

Hizmet başlatıldığında uygulama ağ geçidine bir IP adresi atanır.

## <a name="create-application-gateway-configuration"></a>Uygulama ağ geçidi yapılandırması oluşturma

Tüm yapılandırma öğeleri, uygulama ağ geçidi oluşturulmadan önce ayarlanmalıdır. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

### <a name="step-1"></a>1. Adım

**GatewayIP01**adlı bir uygulama ağ geçidi IP yapılandırması oluşturun. Application Gateway başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

```powershell
$gipconfig = New-AzApplicationGatewayIPConfiguration -Name "gatewayIP01" -Subnet $appgatewaysubnetdata
```

### <a name="step-2"></a>2. Adım

Ortak IP bitiş noktası için ön uç IP bağlantı noktasını yapılandırın. Bu bağlantı noktası, son kullanıcıların bağlandığı bağlantı noktasıdır.

```powershell
$fp01 = New-AzApplicationGatewayFrontendPort -Name "port01"  -Port 443
```

### <a name="step-3"></a>3. Adım

Ön uç IP’sini genel IP uç noktası ile yapılandırın.

```powershell
$fipconfig01 = New-AzApplicationGatewayFrontendIPConfig -Name "frontend1" -PublicIPAddress $publicip
```

### <a name="step-4"></a>4. Adım

Geçen trafiğin şifresini çözmek ve yeniden şifrelemek için kullanılacak Olan Uygulama Ağ Geçidi'nin sertifikalarını yapılandırın.

```powershell
$cert = New-AzApplicationGatewaySslCertificate -Name "cert01" -CertificateFile $gatewayCertPfxPath -Password $certPwd
$certPortal = New-AzApplicationGatewaySslCertificate -Name "cert02" -CertificateFile $portalCertPfxPath -Password $certPortalPwd
```

### <a name="step-5"></a>5. Adım

Uygulama Ağ Geçidi için HTTP dinleyicilerini oluşturun. Ön uç IP yapılandırmasını, bağlantı noktasını ve ssl sertifikalarını onlara atayın.

```powershell
$listener = New-AzApplicationGatewayHttpListener -Name "listener01" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $cert -HostName $gatewayHostname -RequireServerNameIndication true
$portalListener = New-AzApplicationGatewayHttpListener -Name "listener02" -Protocol "Https" -FrontendIPConfiguration $fipconfig01 -FrontendPort $fp01 -SslCertificate $certPortal -HostName $portalHostname -RequireServerNameIndication true
```

### <a name="step-6"></a>6. Adım

API Management hizmeti `ContosoApi` proxy etki alanı bitiş noktasına özel problar oluşturun. Yol, `/status-0123456789abcdef` tüm API Yönetimi hizmetlerinde barındırılan varsayılan bir sistem durumu bitiş noktasıdır. SSL sertifikası ile güvenli hale getirmek için özel bir sonda ana bilgisayar adı olarak ayarlayın. `api.contoso.net`

> [!NOTE]
> Ana bilgisayar `contosoapi.azure-api.net` adı, ortak Azure'da adı `contosoapi` geçen bir hizmet oluşturulduğunda yapılandırılan varsayılan proxy ana bilgisayar adıdır.
>

```powershell
$apimprobe = New-AzApplicationGatewayProbeConfig -Name "apimproxyprobe" -Protocol "Https" -HostName $gatewayHostname -Path "/status-0123456789abcdef" -Interval 30 -Timeout 120 -UnhealthyThreshold 8
$apimPortalProbe = New-AzApplicationGatewayProbeConfig -Name "apimportalprobe" -Protocol "Https" -HostName $portalHostname -Path "/signin" -Interval 60 -Timeout 300 -UnhealthyThreshold 8
```

### <a name="step-7"></a>7. Adım

SSL özellikli arka uç havuzu kaynaklarında kullanılacak sertifikayı yükleyin. Bu, yukarıda Adım 4'te sağladığınız sertifikanın aynısI.

```powershell
$authcert = New-AzApplicationGatewayAuthenticationCertificate -Name "whitelistcert1" -CertificateFile $gatewayCertCerPath
```

### <a name="step-8"></a>8. Adım

Uygulama Ağ Geçidi için HTTP arka uç ayarlarını yapılandırın. Bu, arka uç isteği için bir zaman sonu sınırı ayarlamayı ve ardından iptal edilir. Bu değer sonda zaman diliminden farklıdır.

```powershell
$apimPoolSetting = New-AzApplicationGatewayBackendHttpSettings -Name "apimPoolSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimprobe -AuthenticationCertificates $authcert -RequestTimeout 180
$apimPoolPortalSetting = New-AzApplicationGatewayBackendHttpSettings -Name "apimPoolPortalSetting" -Port 443 -Protocol "Https" -CookieBasedAffinity "Disabled" -Probe $apimPortalProbe -AuthenticationCertificates $authcert -RequestTimeout 180
```

### <a name="step-9"></a>9. Adım

Yukarıda oluşturulan API Yönetimi hizmetinin dahili sanal IP adresi ile **apimbackend** adlı bir arka uç IP adresi havuzunu yapılandırın.

```powershell
$apimProxyBackendPool = New-AzApplicationGatewayBackendAddressPool -Name "apimbackend" -BackendIPAddresses $apimService.PrivateIPAddresses[0]
```

### <a name="step-10"></a>Adım 10

Temel yönlendirmeyi kullanmak için Uygulama Ağ Geçidi için kurallar oluşturun.

```powershell
$rule01 = New-AzApplicationGatewayRequestRoutingRule -Name "rule1" -RuleType Basic -HttpListener $listener -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolSetting
$rule02 = New-AzApplicationGatewayRequestRoutingRule -Name "rule2" -RuleType Basic -HttpListener $portalListener -BackendAddressPool $apimProxyBackendPool -BackendHttpSettings $apimPoolPortalSetting
```

> [!TIP]
> Geliştirici portalının belirli sayfalarına erişimi kısıtlamak için -RuleType ve yönlendirmeyi değiştirin.

### <a name="step-11"></a>Adım 11

Uygulama Ağ Geçidi için örnek sayısını ve boyutunu yapılandırın. Bu örnekte, API Yönetimi kaynağının artan güvenliği için [WAF SKU'yu](../application-gateway/application-gateway-webapplicationfirewall-overview.md) kullanıyoruz.

```powershell
$sku = New-AzApplicationGatewaySku -Name "WAF_Medium" -Tier "WAF" -Capacity 2
```

### <a name="step-12"></a>Adım 12

WAF'ı "Önleme" modunda olacak şekilde yapılandırın.

```powershell
$config = New-AzApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode "Prevention"
```

## <a name="create-application-gateway"></a>Uygulama Ağ Geçidi Oluşturma

Önceki adımlardaki tüm yapılandırma nesneleriyle bir Uygulama Ağ Geçidi oluşturun.

```powershell
$appgwName = "apim-app-gw"
$appgw = New-AzApplicationGateway -Name $appgwName -ResourceGroupName $resGroupName -Location $location -BackendAddressPools $apimProxyBackendPool -BackendHttpSettingsCollection $apimPoolSetting, $apimPoolPortalSetting  -FrontendIpConfigurations $fipconfig01 -GatewayIpConfigurations $gipconfig -FrontendPorts $fp01 -HttpListeners $listener, $portalListener -RequestRoutingRules $rule01, $rule02 -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert, $certPortal -AuthenticationCertificates $authcert -Probes $apimprobe, $apimPortalProbe
```

## <a name="cname-the-api-management-proxy-hostname-to-the-public-dns-name-of-the-application-gateway-resource"></a>CNAME UYGULAMA Ağ Geçidi kaynağının genel DNS adına API Yönetimi proxy ana bilgisayar adı

Ağ geçidi oluşturulduktan sonraki adım, iletişim için ön uç yapılandırması yapmaktır. Ortak bir IP kullanırken, Uygulama Ağ Geçidi dinamik olarak atanmış bir DNS adı gerektirir ve kullanımı kolay olmayabilir.

Uygulama Ağ Geçidi'nin DNS adı, APIM proxy ana bilgisayar adını (örn. `api.contoso.net` yukarıdaki örneklerde) bu DNS adına işaret eden bir CNAME kaydı oluşturmak için kullanılmalıdır. Ön uç IP CNAME kaydını yapılandırmak için PublicIPAddress öğesini kullanarak Uygulama Ağ Geçidi'nin ayrıntılarını ve ilişkili IP/DNS adını alın. VIP ağ geçidinin yeniden başlatılmasında değişebileceğinden A kayıtlarının kullanılması önerilmez.

```powershell
Get-AzPublicIpAddress -ResourceGroupName $resGroupName -Name "publicIP01"
```

## <a name="summary"></a><a name="summary"> </a> Özet
VNET'te yapılandırılan Azure API Yönetimi, ister şirket içinde ister bulutta barındırılan tüm yapılandırılmış API'ler için tek bir ağ geçidi arabirimi sağlar. Uygulama Ağ Geçidi'ni API Yönetimi ile tümleştirmek, belirli API'lerin Internet'te erişilebilir olmasını seçerek etkinleştirme esnekliği sağlamanın yanı sıra API Yönetimi örneğinizin ön koşulu olarak bir Web Uygulaması Güvenlik Duvarı sağlama esnekliği sağlar.

## <a name="next-steps"></a><a name="next-steps"> </a> Sonraki adımlar
* Azure Uygulama Ağ Geçidi hakkında daha fazla bilgi edinin
  * [Uygulama Ağ Geçidine Genel Bakış](../application-gateway/application-gateway-introduction.md)
  * [Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı](../application-gateway/application-gateway-webapplicationfirewall-overview.md)
  * [Yol Tabanlı Yönlendirme'yi kullanarak Uygulama Ağ Geçidi](../application-gateway/application-gateway-create-url-route-arm-ps.md)
* API Yönetimi ve VNET'ler hakkında daha fazla bilgi edinin
  * [Yalnızca VNET içinde kullanılabilen API Yönetimini kullanma](api-management-using-with-internal-vnet.md)
  * [VNET'te API Yönetimini Kullanma](api-management-using-with-vnet.md)
