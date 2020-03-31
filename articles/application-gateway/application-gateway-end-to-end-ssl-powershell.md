---
title: Azure Uygulama Ağ Geçidi ile uçlardan uca SSL'yi yapılandırma
description: Bu makalede, PowerShell kullanarak Azure Application Gateway ile uçlardan uca SSL'nin nasıl yapılandırılabildiğini açıklamaktadır.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/8/2019
ms.author: victorh
ms.openlocfilehash: 7ba273cddb6cf41872c4db1c34560c104b992787
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72286460"
---
# <a name="configure-end-to-end-ssl-by-using-application-gateway-with-powershell"></a>Application Gateway’i PowerShell ile kullanarak uçtan uca SSL yapılandırma

## <a name="overview"></a>Genel Bakış

Azure Application Gateway, trafiğin uçtan uca şifrelemesini destekler. Uygulama Ağ Geçidi, uygulama ağ geçidindeki SSL bağlantısını sonlandırır. Ağ geçidi daha sonra yönlendirme kurallarını trafiğe uygular, paketi yeniden şifreler ve tanımlanan yönlendirme kurallarına göre paketi uygun arka uç sunucusuna iletir. Web sunucusundan alınan herhangi bir yanıt, son kullanıcıya dönerken aynı süreci izler.

Uygulama Ağ Geçidi özel SSL seçeneklerini tanımlamayı destekler. Ayrıca aşağıdaki protokol sürümlerinin devre dışı bırakılmasını destekler: **TLSv1.0**, **TLSv1.1 , ve TLSv1.2**, ayrıca hangi şifre lerin kullanılacağını ve tercih sırasını tanımlar. **TLSv1.2** Yapılandırılabilir SSL seçenekleri hakkında daha fazla bilgi edinmek için [SSL ilkesine genel bakış alabilme](application-gateway-SSL-policy-overview.md)bilgisini edinin.

> [!NOTE]
> SSL 2.0 ve SSL 3.0 varsayılan olarak devre dışı bırakılır ve etkinleştirilemez. Güvenli olmadıkları kabul edilir ve Uygulama Ağ Geçidi ile kullanılamazlar.

![senaryo görüntüsü][scenario]

## <a name="scenario"></a>Senaryo

Bu senaryoda, PowerShell ile uçlardan uca SSL kullanarak bir uygulama ağ geçidi oluşturmayı öğrenirsiniz.

Bu senaryo şunları yapacaktır:

* **appgw-rg**adında bir kaynak grubu oluşturun.
* Adres alanı **10.0.0.0/16**olan **appgwvnet** adında bir sanal ağ oluşturun.
* **Appgwsubnet** ve **appsubnet**adlı iki alt ağ oluşturun.
* SSL protokol sürümlerini ve şifreleme paketlerini sınırlayan uçlardan uca SSL şifrelemesini destekleyen küçük bir uygulama ağ geçidi oluşturun.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bir uygulama ağ geçidi ile uçlardan uca SSL yapılandırmak için ağ geçidi için bir sertifika gereklidir ve arka uç sunucular için sertifikalar gereklidir. Ağ geçidi sertifikası, SSL protokol belirtimi uyarınca simetrik bir anahtar türetmek için kullanılır. Simetrik anahtar daha sonra ağ geçidine gönderilen trafiği şifrelemek ve şifresini çözmek için kullanılır. Ağ geçidi sertifikasının Kişisel Bilgi Alışverişi (PFX) biçiminde olması gerekir. Bu dosya biçimi, trafiğin şifreleme ve şifre çözme gerçekleştirmek için uygulama ağ geçidi tarafından gerekli olan özel anahtarı dışa aktarmanızı sağlar.

Uçlardan uca SSL şifrelemesi için, arka uç uygulama ağ geçidi tarafından açıkça izin verilmelidir. Arka uç sunucularının ortak sertifikasını uygulama ağ geçidine yükleyin. Sertifikanın eklenmesi, uygulama ağ geçidinin yalnızca bilinen arka uç örnekleriyle iletişim kurmasını sağlar. Bu, uçlardan uca iletişimi daha da güvence altına alar.

Yapılandırma işlemi aşağıdaki bölümlerde açıklanmıştır.

## <a name="create-the-resource-group"></a>Kaynak grubunu oluşturma

Bu bölüm, uygulama ağ geçidi içeren bir kaynak grubu oluşturma yoluyla size yol açar.

1. Azure hesabınızda oturum açın.

   ```powershell
   Connect-AzAccount
   ```

2. Bu senaryo için kullanılacak aboneliği seçin.

   ```powershell
   Select-Azsubscription -SubscriptionName "<Subscription name>"
   ```

3. Bir kaynak grubu oluşturun. (Mevcut bir kaynak grubunu kullanıyorsanız bu adımı atlayın.)

   ```powershell
   New-AzResourceGroup -Name appgw-rg -Location "West US"
   ```

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>Uygulama ağ geçidi için bir sanal ağ ve bir alt ağ oluştur

Aşağıdaki örnekte bir sanal ağ ve iki alt ağ oluşturulur. Uygulama ağ geçidini tutmak için bir alt ağ kullanılır. Diğer alt ağ, web uygulamasını barındıran arka uçlar için kullanılır.

1. Uygulama ağ geçidi için kullanılacak alt ağ için bir adres aralığı atayın.

   ```powershell
   $gwSubnet = New-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24
   ```

   > [!NOTE]
   > Bir uygulama ağ geçidi için yapılandırılan alt ağlar düzgün boyutlandırılmalıdır. Bir uygulama ağ geçidi en fazla 10 örnek için yapılandırılabilir. Her örnek alt ağdan bir IP adresi alır. Bir alt ağ çok küçük olumsuz bir uygulama ağ geçidi ölçekleme etkileyebilir.
   >

2. Arka uç adres havuzu için kullanılacak bir adres aralığı atayın.

   ```powershell
   $nicSubnet = New-AzVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24
   ```

3. Önceki adımlarda tanımlanan alt ağlarla sanal ağ oluşturun.

   ```powershell
   $vnet = New-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet
   ```

4. Aşağıdaki adımlarda kullanılacak sanal ağ kaynağını ve alt ağ kaynaklarını alın.

   ```powershell
   $vnet = Get-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
   $gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
   $nicSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet
   ```

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>Ön uç yapılandırma için genel bir IP adresi oluşturun

Uygulama ağ geçidi için kullanılacak genel bir IP kaynağı oluşturun. Bu genel IP adresi, izleyen adımlardan birinde kullanılır.

```powershell
$publicip = New-AzPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic
```

> [!IMPORTANT]
> Uygulama Ağ Geçidi, tanımlı bir etki alanı etiketiyle oluşturulan genel bir IP adresinin kullanımını desteklemez. Yalnızca dinamik olarak oluşturulmuş etki alanı etiketine sahip ortak bir IP adresi desteklenir. Uygulama ağ geçidi için uygun bir DNS adı gerekiyorsa, takma ad olarak cname kaydı kullanmanızı öneririz.

## <a name="create-an-application-gateway-configuration-object"></a>Uygulama ağ geçidi yapılandırma nesnesi oluşturun

Tüm yapılandırma öğeleri uygulama ağ geçidi oluşturmadan önce ayarlanır. Aşağıdaki adımlar uygulama ağ geçidi kaynağı için gerekli yapılandırma öğelerini oluşturur.

1. Bir uygulama ağ geçidi IP yapılandırması oluşturun. Bu ayar, uygulama ağ geçidinin kullandığı alt ağağlarından hangisini yapılandırır. Uygulama ağ geçidi başladığında, yapılandırılan alt ağdan bir IP adresi alır ve ağ trafiğini arka uç IP havuzundaki IP adreslerine yönlendirir. Her örneğin bir IP adresi aldığını göz önünde bulundurun.

   ```powershell
   $gipconfig = New-AzApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet
   ```

2. Bir ön uç IP yapılandırması oluşturun. Bu ayar, uygulama ağ geçidinin ön ucuna özel veya genel bir IP adresi eşler. Aşağıdaki adım, önceki adımdaki genel IP adresini ön uç IP yapılandırmasıyla ilişkilendirer.

   ```powershell
   $fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip
   ```

3. Arka uç IP adresi havuzunu arka uç web sunucularının IP adresleriyle yapılandırın. Bu IP adresleri ön uç IP uç noktasından gelen ağ trafiğinin yönlendirildiği IP adresleridir. Örnekteki IP adreslerini kendi uygulama IP adres uç noktalarınızla değiştirin.

   ```powershell
   $pool = New-AzApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3
   ```

   > [!NOTE]
   > Tam nitelikli alan adı (FQDN), arka uç sunucular için ip adresi yerine kullanılacak geçerli bir değerdir. **-BackendFqdns** anahtarını kullanarak etkinleştirin. 

4. Ortak IP bitiş noktası için ön uç IP bağlantı noktasını yapılandırın. Bu bağlantı noktası, son kullanıcıların bağlandığı bağlantı noktasıdır.

   ```powershell
   $fp = New-AzApplicationGatewayFrontendPort -Name 'port01'  -Port 443
   ```

5. Uygulama ağ geçidi için sertifikayı yapılandırın. Bu sertifika, uygulama ağ geçidindeki trafiğin şifresini çözmek ve yeniden şifrelemek için kullanılır.

   ```powershell
   $passwd = ConvertTo-SecureString  <certificate file password> -AsPlainText -Force 
   $cert = New-AzApplicationGatewaySSLCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password $passwd 
   ```

   > [!NOTE]
   > Bu örnek, SSL bağlantısı için kullanılan sertifikayı yapılandırır. Sertifikanın .pfx biçiminde olması ve parolanın 4 ila 12 karakter olması gerekir.

6. Uygulama ağ geçidi için HTTP dinleyicisini oluşturun. Kullanmak üzere ön uç IP yapılandırması, bağlantı noktası ve SSL sertifikası atayın.

   ```powershell
   $listener = New-AzApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert
   ```

7. SSL özellikli arka uç havuzu kaynaklarında kullanılacak sertifikayı yükleyin.

   > [!NOTE]
   > Varsayılan sonda, ortak anahtarı arka uçTAKI IP adresindeki *varsayılan* SSL bağlayıcısından alır ve aldığı ortak anahtar değerini burada sağladığınız ortak anahtar değeriyle karşılaştırır. 
   > 
   > Arka uçta ana bilgisayar üstbilgilerini ve Sunucu Adı Göstergesi (SNI) kullanıyorsanız, alınan ortak anahtar trafiğin aktığı amaçlanan site olmayabilir. Şüpheniz varsa, *varsayılan* https://127.0.0.1/ SSL bağlama için hangi sertifikanın kullanıldığını onaylamak için arka uç sunucularını ziyaret edin. Bu bölümdeki istekteki ortak anahtarı kullanın. HTTPS bağlamalarında ana bilgisayar başlıkları ve SNI kullanıyorsanız ve el ile tarayıcı isteğinden https://127.0.0.1/ arka uç sunuculara yanıt ve sertifika almıyorsanız, bunlar üzerinde varsayılan bir SSL bağlama ayarlamanız gerekir. Bunu yapmazsanız, sondalar başarısız olur ve arka uç beyaz listeye alınmaz.

   ```powershell
   $authcert = New-AzApplicationGatewayAuthenticationCertificate -Name 'allowlistcert1' -CertificateFile C:\cert.cer
   ```

   > [!NOTE]
   > Önceki adımda sağlanan sertifika, arka uçta bulunan .pfx sertifikasının ortak anahtarı olmalıdır. Talep, Kanıt ve Akıl Yürütme (CER) biçiminde arka uç sunucusuna yüklenen sertifikayı (kök sertifikası değil) dışa aktarın ve bu adımda kullanın. Bu adım, uygulama ağ geçidi ile arka ucunu beyaz listeler.

   Uygulama Ağ Geçidi v2 SKU kullanıyorsanız, kimlik doğrulama sertifikası yerine güvenilir bir kök sertifikası oluşturun. Daha fazla bilgi için, [Uygulama Ağ Geçidi ile sona SSL'ye Genel Bakış:](ssl-overview.md#end-to-end-ssl-with-the-v2-sku)

   ```powershell
   $trustedRootCert01 = New-AzApplicationGatewayTrustedRootCertificate -Name "test1" -CertificateFile  <path to root cert file>
   ```

8. Uygulama arka uç için HTTP ayarlarını yapılandırın. Önceki adımda yüklenen sertifikayı HTTP ayarlarına atayın.

   ```powershell
   $poolSetting = New-AzApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert
   ```

   Uygulama Ağ Geçidi v2 SKU için aşağıdaki komutu kullanın:

   ```powershell
   $poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name “setting01” -Port 443 -Protocol Https -CookieBasedAffinity Disabled -TrustedRootCertificate $trustedRootCert01 -HostName "test1"
   ```

9. Yük dengeleyici davranışını yapılandıran bir yük dengeleyici yönlendirme kuralı oluşturun. Bu örnekte, temel bir round-robin kuralı oluşturulur.

   ```powershell
   $rule = New-AzApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   ```

10. Uygulama ağ geçidinin örnek boyutunu yapılandırın. Mevcut boyutları **Standart\_Küçük,** **\_Standart Orta**ve Standart **\_Büyük.**  Kapasite için kullanılabilir değerler **1** ile **10**arasındadır.

    ```powershell
    $sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2
    ```

    > [!NOTE]
    > Test amacıyla 1 örnek sayısı seçilebilir. İki örnek altında herhangi bir örnek sayısının SLA tarafından karşılanmadığını ve bu nedenle önerilmediğini bilmek önemlidir. Küçük ağ geçitleri üretim amaçlı değil, dev testi için kullanılacaktır.

11. Uygulama ağ geçidinde kullanılacak SSL ilkesini yapılandırın. Uygulama Ağ Geçidi, SSL protokol sürümleri için minimum sürüm ayarlama yeteneğini destekler.

    Aşağıdaki değerler tanımlanabilecek protokol sürümlerinin bir listesidir:

    - **TLSV1_0**
    - **TLSV1_1**
    - **TLSV1_2**
    
    Aşağıdaki **örnekte, TLSv1_2** için minimum protokol sürümünü belirleyen ve **TLS\_ECDHE\_ECDSA\_With\_AES\_128\_GCM\_SHA256,** **TLS\_\_ECDHE ECDSA\_With\_AES 256\_\_GCM\_SHA384**ve **TLS\_RSA\_With\_AES\_128\_GCM\_SHA256'ya** olanak sağlar.

    ```powershell
    $SSLPolicy = New-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -PolicyType Custom
    ```

## <a name="create-the-application-gateway"></a>Uygulama ağ geçidi oluşturma

Önceki tüm adımları kullanarak, uygulama ağ geçidioluşturun. Ağ geçidinin oluşturulması, çalışması uzun zaman alan bir işlemdir.

V1 SKU için aşağıdaki komutu kullanın
```powershell
$appgw = New-AzApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -AuthenticationCertificates $authcert -Verbose
```

V2 SKU için aşağıdaki komutu kullanın
```powershell
$appgw = New-AzApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -TrustedRootCertificate $trustedRootCert01 -Verbose
```

## <a name="apply-a-new-certificate-if-the-back-end-certificate-is-expired"></a>Arka uç sertifikasının süresi dolmuşsa yeni bir sertifika uygulama

Arka uç sertifikasının süresi dolmuşsa, yeni bir sertifika uygulamak için bu yordamı kullanın.

1. Güncelleştirmek için uygulama ağ geçidini alın.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```
   
2. Yeni sertifika kaynağını,.sertifikanın ortak anahtarını içeren ve uygulama ağ geçidinde SSL sonlandırma için dinleyiciye eklenen sertifikanın aynısı olan .cer dosyasından ekleyin.

   ```powershell
   Add-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name 'NewCert' -CertificateFile "appgw_NewCert.cer" 
   ```
    
3. Yeni kimlik doğrulama sertifikası nesnesini bir değişkene alın (TypeName: Microsoft.Azure.Commands.Network.Models.PSApplicationGatewayAuthenticationCertificate).

   ```powershell
   $AuthCert = Get-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name NewCert
   ```
 
 4. Yeni sertifikayı **BackendHttp** Ayarına atayın ve $AuthCert değişkeniyle başvurun. (Değiştirmek istediğiniz HTTP ayar adını belirtin.)
 
   ```powershell
   $out= Set-AzApplicationGatewayBackendHttpSetting -ApplicationGateway $gw -Name "HTTP1" -Port 443 -Protocol "Https" -CookieBasedAffinity Disabled -AuthenticationCertificates $Authcert
   ```
    
 5. Değişikliği uygulama ağ geçidine dönüştürün ve $out değişkeninde bulunan yeni yapılandırmayı geçirin.
 
   ```powershell
   Set-AzApplicationGateway -ApplicationGateway $gw  
   ```

## <a name="remove-an-unused-expired-certificate-from-http-settings"></a>Kullanılmayan süresi dolmuş bir sertifikayı HTTP Ayarları'ndan kaldırma

Kullanılmayan süresi dolmuş bir sertifikayı HTTP Ayarları'ndan kaldırmak için bu yordamı kullanın.

1. Güncelleştirmek için uygulama ağ geçidini alın.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```
   
2. Kaldırmak istediğiniz kimlik doğrulama sertifikasının adını listele.

   ```powershell
   Get-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw | select name
   ```
    
3. Kimlik doğrulama sertifikasını bir uygulama ağ geçidinden kaldırın.

   ```powershell
   $gw=Remove-AzApplicationGatewayAuthenticationCertificate -ApplicationGateway $gw -Name ExpiredCert
   ```
 
 4. Değişimi gerçekleştirin.
 
   ```powershell
   Set-AzApplicationGateway -ApplicationGateway $gw
   ```

   
## <a name="limit-ssl-protocol-versions-on-an-existing-application-gateway"></a>Varolan bir uygulama ağ geçidinde SSL protokol sürümlerini sınırlandırın

Önceki adımlar, uçtan uca SSL içeren bir uygulama oluşturmanız ve belirli SSL protokol sürümlerini devre dışı bırakmanız için sizi zora sokalmıştır. Aşağıdaki örnek, varolan bir uygulama ağ geçidindeki belirli SSL ilkelerini devre dışı kalmaktadır.

1. Güncelleştirmek için uygulama ağ geçidini alın.

   ```powershell
   $gw = Get-AzApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG
   ```

2. Bir SSL ilkesi tanımlayın. Aşağıdaki örnekte, **TLSv1.0** ve **TLSv1.1** devre dışı bırakılmış olup, **aes\_\_\_\_\_128\_GCM SHA256,\_** **AES\_256\_\_\_GCM\_SHA384 İle\_TLS ECDHE ECDSA\_** ve **\_AES\_\_\_128 GCM SHA256\_ile\_ciper** süitleri sadece izin verilenler arasındadır.

   ```powershell
   Set-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -PolicyType Custom -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -ApplicationGateway $gw

   ```

3. Son olarak, ağ geçidini güncelleştirin. Bu son adım uzun süren bir görevdir. Bu yapıldığında, uçlardan uca SSL uygulama ağ geçidi üzerinde yapılandırılır.

   ```powershell
   $gw | Set-AzApplicationGateway
   ```

## <a name="get-an-application-gateway-dns-name"></a>Uygulama ağ geçidi DNS adı alma

Ağ geçidi oluşturulduktan sonra, bir sonraki adım iletişim için ön ucunu yapılandırmaktır. Uygulama Ağ Geçidi, ortak bir IP kullanırken dinamik olarak atanmış bir DNS adı gerektirir, bu da uygun değildir. Son kullanıcıların uygulama ağ geçidine ulaşabilmesini sağlamak için, uygulama ağ geçidinin ortak bitiş noktasını işaret etmek için bir CNAME kaydı kullanabilirsiniz. Daha fazla bilgi için [Azure'da özel bir etki alanı adını yapılandırma](../cloud-services/cloud-services-custom-domain-name-portal.md)ya da yapılandırma 'ya bakın. 

Bir diğer adı yapılandırmak için, uygulama ağ geçidine eklenen **PublicIPAddress** öğesini kullanarak uygulama ağ geçidinin ayrıntılarını ve ilişkili IP/DNS adını alın. İki web uygulamasını bu DNS adına işaret eden bir CNAME kaydı oluşturmak için uygulama ağ geçidinin DNS adını kullanın. VIP uygulama ağ geçidinin yeniden başlatılmasında değişiklik olabileceğinden, A kayıtlarının kullanılmasını önermiyoruz.

```powershell
Get-AzPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
```

```
Name                     : publicIP01
ResourceGroupName        : appgw-RG
Location                 : westus
Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
ResourceGuid             : 00000000-0000-0000-0000-000000000000
ProvisioningState        : Succeeded
Tags                     : 
PublicIpAllocationMethod : Dynamic
IpAddress                : xx.xx.xxx.xx
PublicIpAddressVersion   : IPv4
IdleTimeoutInMinutes     : 4
IpConfiguration          : {
                                "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                            Configurations/frontend1"
                            }
DnsSettings              : {
                                "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                            }
```

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Ağ Geçidi aracılığıyla Web Uygulama Güvenlik Duvarı ile web uygulamalarınızın güvenliğini sertleştirme hakkında daha fazla bilgi için [Web uygulaması güvenlik duvarı genel bakış](application-gateway-webapplicationfirewall-overview.md)bakın.

[scenario]: ./media/application-gateway-end-to-end-SSL-powershell/scenario.png
