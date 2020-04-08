---
title: Güvenli bir Azure AD Web uygulaması geliştirin | Microsoft Dokümanlar
description: Bu basit örnek uygulama, Azure'da geliştirdiğinizde uygulamanızı ve kuruluşunuzun güvenlik duruşunu iyileştiren en iyi güvenlik uygulamalarını uygular.
keywords: yok
services: security
documentationcenter: na
author: TerryLanfear
manager: alclabo
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/12/2019
ms.author: terrylan
ms.openlocfilehash: 599c4a31840b47294b43c4c4d1f0200b17f04540
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80810539"
---
# <a name="develop-secure-app-for-an-azure-ad-app"></a>Azure AD uygulaması için güvenli uygulama geliştirme
## <a name="overview"></a>Genel Bakış

Bu örnek, Azure'da uygulama geliştirmek için güvenlik kaynaklarına bağlantı veren web uygulamasına sahip basit bir Azure Etkin Dizinidir. Uygulama, Azure'da uygulamalar geliştirdiğinizde uygulamanızın ve kuruluşunuzun güvenlik duruşunu iyileştirmeye yardımcı olabilecek en iyi güvenlik uygulamalarını uygular.

Dağıtım komut dosyaları altyapıyı ayarlar. Dağıtım komut dosyalarını çalıştırdıktan sonra, bileşenleri ve hizmetleri birbirine bağlamak için Azure portalında bazı el ile yapılandırma yapmanız gerekir. Bu örnek, Azure'da perakende sektöründe çalışan ve güvenli Azure altyapısıyla güvenli bir Azure Etkin Dizinoluşturmak isteyen deneyimli geliştiricilere yöneliktir. 


Bu uygulamayı geliştirirken ve dağıtAbiliyorsanız, nasıl 
- Azure Anahtar Kasası örneği oluşturun, ondan sırlar saklayın ve alın.
- Ön uç güvenlik duvarı erişimiyle yalıtılmış olarak ayrılmış olan Azure Web Uygulamasını dağıtın. 
- OWASP Top 10 Ruleset kullanan bir güvenlik duvarıyla bir Azure Uygulama Ağ Geçidi örneği oluşturun ve yapılandırın. 
- Azure hizmetlerini kullanarak aktarım sırasında ve istirahatte veri şifrelemesini etkinleştirin. 
- Uyumlulukları değerlendirmek için Azure ilkesini ve güvenlik merkezini ayarlayın. 

Bu uygulamayı geliştirip dağıttıktan sonra, açıklanan yapılandırma ve güvenlik önlemleriyle birlikte aşağıdaki örnek web uygulamasını kurmuş olabilirsiniz.

## <a name="architecture"></a>Mimari
Uygulama, üç katmanlı tipik bir n katmanı uygulamasıdır. İzleme ve gizli yönetim bileşenleritümleşik ön uç, arka uç ve veritabanı katmanı burada gösterilmiştir:

![Uygulama mimarisi](./media/secure-aad-app/architecture.png)

Bu çözüm, aşağıdaki Azure hizmetlerini kullanır. Dağıtım mimarisinin ayrıntıları Dağıtım Mimarisi bölümünde bulunmaktadır. 

Mimari bu bileşenlerden oluşur

- [Azure Uygulama Ağ Geçidi](../../application-gateway/index.yml). Uygulama mimarimiz için ağ geçidi ve güvenlik duvarı sağlar.
- [Uygulama Öngörüleri](../../azure-monitor/app/app-insights-overview.md). Birden çok platformda genişletilebilir Bir Uygulama Performans Yönetimi (APM) hizmeti sağlar.
- [Azure Anahtar Kasası](../../key-vault/index.yml). Uygulamamızın sırlarını saklar ve şifreler ve bunların etrafında erişim politikaları oluşturulmasını yönetir.
- [Azure Etkin Dizin.](../../active-directory/fundamentals/active-directory-whatis.md) Bulut tabanlı kimlik ve erişim yönetimi hizmeti, oturum açma ve erişim kaynakları sağlar.
- [Azure Alan Adı Sistemi](../../dns/dns-overview.md). Etki alanını barındırmak için hizmeti sağlayın.
- [Azure Yük Dengeleyicisi](../../load-balancer/load-balancer-overview.md). Uygulamalarınızı ölçeklendirmek ve hizmetleriniz için yüksek kullanılabilirlik oluşturmak için sağlar.
- [Azure Web Uygulaması](../../app-service/overview.md).  Web uygulamalarını barındırmak için HTTP tabanlı bir hizmet sağlar.
- [Azure Güvenlik Merkezi.](../../security-center/index.yml) buluttaki karma iş yükleri nizde gelişmiş tehdit koruması sağlar.
- [Azure İlkesi](../../governance/policy/overview.md). Atanan ilkelere uyulmaması için kaynaklarınızın değerlendirilmesinde izin sağlar.

## <a name="threat-model"></a>Tehdit modeli
Tehdit modelleme, işletmenize ve uygulamanıza yönelik olası güvenlik tehditlerini belirleme ve ardından uygun bir azaltma planının uygulanmasını sağlama işlemidir.

Bu örnek, güvenli örnek uygulama için tehdit modellemesi uygulamak için [Microsoft Tehdit Modelleme Aracı'nı](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool) kullanmıştır. Bileşenleri ve veri akışlarını diyagramlayarak, geliştirme sürecinin başlarında sorunları ve tehditleri tanımlayabilirsiniz. Zaman ve para daha sonra bu kullanılarak kaydedilir.

İşte örnek uygulama için tehdit modeli

![Tehdit modeli](./media/secure-aad-app/threat-model.png)

Tehdit modelleme aracının oluşturduğu bazı örnek tehditler ve olası güvenlik açıkları aşağıdaki ekran görüntüsünde gösterilir. Tehdit modeli, maruz kalan saldırı yüzeyine genel bir bakış sağlar ve geliştiricileri sorunları nasıl azaltacakları konusunda düşünmeye teşvik eder.

![Tehdit modeli çıktısı](./media/secure-aad-app/threat-model-output.png)

### <a name="prerequisites"></a>Ön koşullar
Uygulamayı çalışır hale getirmek için aşağıdaki araçları yüklemeniz gerekir:

- Uygulama kodunu değiştirmek ve görüntülemek için bir kod düzenleyicisi. [Visual Studio Code](https://code.visualstudio.com/) açık kaynak seçeneğidir.
- Geliştirme bilgisayarınızda [Azure CLI.](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest&viewFallbackFrom=azure-cli-latest,)
- Sisteminizde [git.](https://git-scm.com/) Git, kaynak kodunu yerel olarak klonlamak için kullanılır.
- [jq](https://stedolan.github.io/jq/), kullanıcı dostu bir şekilde JSON sorgulama kiçin bir UNIX aracı.

Örnek uygulamanın kaynaklarını dağıtmak için bir Azure aboneliğine ihtiyacınız vardır. Azure aboneliğiniz yoksa, örnek uygulamayı test etmek için [ücretsiz bir hesap oluşturabilirsiniz.](https://azure.microsoft.com/free/)

Bu araçları yükledikten sonra uygulamayı Azure'da dağıtmaya hazırsınız.

### <a name="implementation-guidance"></a>Uygulama kılavuzu
Dağıtım komut dosyası, dört aşamaya ayrılabilen bir komut dosyasıdır. Her [aşama, mimari diyagramında](#architecture)yer alan bir Azure kaynağını dağıtır ve yapılandırır.

Dört aşama

- Azure Anahtar Kasası'nı dağıtın.
- Azure Web Uygulamalarını dağıtın.
- Uygulama Ağ Geçidi'ni web uygulaması güvenlik duvarıyla dağıtın.
- Dağıtılmış Uygulama ile bir Azure REKLAM'ı yapılandırın.

Her aşama, önceden dağıtılan kaynaklardan gelen yapılandırmayı kullanarak bir önceki aşamaya göre inşa edilir.

Uygulama adımlarını tamamlamak [için, Önkoşullar](#prerequisites)altında listelenen araçları yüklediğinizden emin olun.

#### <a name="deploy-azure-key-vault"></a>Azure Anahtar Kasası Dağıtma
Bu bölümde, sırları ve sertifikaları depolamak için kullanılan bir Azure Anahtar Kasası örneği oluşturabilir ve dağıtabilirsiniz.

Dağıtımı tamamladıktan sonra, Azure'da dağıtılan bir Azure Anahtar Kasası örneğine sahip siniz.

Powershell'i kullanarak Azure Anahtar Kasası'nı dağıtmak için
 
1. Azure Anahtar Kasası için değişkenleri bildirin.
2. Azure Anahtar Kasası sağlayıcısını kaydedin.
3. Örnek için kaynak grubu oluşturun.
4. Adım 3'te oluşturulan kaynak grubunda Azure Anahtar Kasası örneğini oluşturun.

#### <a name="the-below-azure-ad-user-will-have-admin-permissions-to-the-key-vault"></a>Aşağıdaki Azure AD kullanıcısı, Key Vault için yönetici izinlerine sahip olacak
    $keyVaultAdminUsers = @($user1,user2)

#### <a name="register-the-az-providers"></a>Az Sağlayıcılarını Kaydedin
    Register-AzResourceProvider -ProviderNamespace Microsoft.KeyVault

#### <a name="create-the-azure-key-vault-instance"></a>Azure Anahtar Kasası örneğini oluşturun
    New-AzKeyVault -Name $kvName 
                -ResourceGroupName $ResourceGroup 
                -Location 'East US'
                -EnabledForDiskEncryption

#### <a name="add-the-administrator-policies-to-the-key-vault"></a>Yönetici ilkelerini Anahtar Kasasına ekleme
    foreach ($keyVaultAdminUser in $keyVaultAdminUsers) {
    $UserObjectId = (Get-AzADUser -SearchString $keyVaultAdminUser).Id
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName -ResourceGroupName $resourceGroupName -ObjectId $UserObjectId 
    -PermissionsToKeys all -PermissionsToSecrets all -PermissionsToCertificates all
    }

#### <a name="to-create-an-access-policy-to-allow-a-user-to-get-and-list-cryptographic-keys-certificates-and-secrets-if-you-know-the-user-principal-name"></a>Kullanıcı Ana Adı'nı biliyorsanız, bir kullanıcının şifreleme anahtarlarını, sertifikalarını ve sırlarını alıp listelemesine izin verecek bir erişim ilkesi oluşturmak için:
    Set-AzKeyVaultAccessPolicy -VaultName $keyVaultName 
                           -ResourceGroupName $resourceGroupName 
                           -UserPrincipalName 'user1@contoso.com 
                           -PermissionsToCertificates list, get 
                           -PermissionsToKeys list, get 
                           -PermissionsToSecrets list, get 

Kaynaklara erişmek için Key Vault kullanan uygulamalarda Azure kaynakları için yönetilen kimlikleri kullanmak en iyi yöntemdir. Key Vault'un erişim anahtarları kodda veya yapılandırmada depolandığında güvenlik duruşunuz artar.

Bir kök sertifikası kapsayıcıya dahildir. Sertifikayı almak için atılan adımlar

1. [Sertifika](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt)Yetkilisi'nden sertifika dosyasını indirin.
2. Sertifika dosyanızın kodunu çöz:

   ```powershell
   openssl x509 -inform DER -in BaltimoreCyberTrustRoot.crt -text -out root.crt
   ```
Bu komut dosyası, kod veya yapılandırmada sert kodlama sırları olmadan Azure Key Vault ile etkileşim kurmak için MSI ile kullanılabilecek Uygulama Hizmeti örneği için atanmış bir kimlik oluşturur.

Erişim ilkesi sekmesinde atanan kimliği yetkilendirmek için portaldaki Azure Anahtar Kasası örneğine gidin. **Yeni erişim ilkesi ekle'yi**seçin. **Select principal**altında, oluşturulan Uygulama Hizmeti örneğinin adına benzer uygulama adını arayın.
Uygulamaya bağlı bir hizmet ilkesi görünür olmalıdır. Aşağıdaki ekran görüntüsünde gösterildiği gibi, seçin ve erişim ilkesi sayfasını kaydedin.

Uygulamanın yalnızca anahtarları alması gerektiğinden, verilen ayrıcalıkları azaltırken erişime izin vererek sırlar seçeneklerinden izin **al'ı** seçin.

![Anahtar Vault Erişim Politikası](./media/secure-aad-app/kv-access-policy.png)

*Key Vault erişim ilkesi oluşturma*

Erişim ilkesini kaydedin ve ilkeleri güncelleştirmek için **Access İlkeleri** sekmesindeki yeni değişikliği kaydedin.

#### <a name="deploy-application-gateway-with-web-application-firewall-enabled"></a>Web uygulaması güvenlik duvarı etkinleştirilmiş Uygulama Ağ Geçidi'ni dağıtma
Web uygulamalarında, hizmetleri doğrudan internet üzerinden dış dünyaya maruz bırakmanız önerilmez.
Yük dengeleme ve güvenlik duvarı kuralları, gelen trafik üzerinde daha fazla güvenlik ve denetim sağlar ve bunu yönetmenize yardımcı olur.

Uygulama Ağ Geçidi örneğini dağıtmak için

1. Uygulama ağ geçidini barındırmak için kaynak grubu oluşturun.
2. Ağ geçidine bağlanmak için sanal bir ağ sağlama.
3. Sanal ağdaki ağ geçidi için bir alt ağ oluşturun.
4. Genel bir IP adresi sağlama.
5. Uygulama ağ geçidini sağlama.
6. Ağ geçidinde web uygulaması güvenlik duvarını etkinleştirin.

```
Connect-AzAccount
Select-AzSubscription -SubscriptionId '$SubscriptionId'
New-AzResourceGroup -Name appgw-rg -Location "East US"

#Create a virtual network and a subnet for the application gateway

#Assign an address range for the subnet to be used for the application gateway.

$gwSubnet = New-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

#Assign an address range to be used for the back-end address pool.

$nicSubnet = New-AzVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

#Create a virtual network with the subnets defined in the preceding steps.

$vnet = New-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "East US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

#Retrieve the virtual network resource and subnet resources to be used in the steps that follow.

$vnet = Get-AzvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
$gwSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
$nicSubnet = Get-AzVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet


#Create a public IP address for the front-end configuration

$publicip = New-AzPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "East US" -AllocationMethod Dynamic

#Create an application gateway configuration object

$gipconfig = New-AzApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

#Create a front-end IP configuration

$fipconfig = New-AzApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

#Configure the back-end IP address pool with the IP addresses of the back-end web servers

$pool = New-AzApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 10.0.3.11

#Configure the front-end IP port for the public IP endpoint

$fp = New-AzApplicationGatewayFrontendPort -Name 'port01'  -Port 443

#Configure the certificate for the application gateway. This certificate is used to decrypt and reencrypt the traffic on the application gateway

$passwd = ConvertTo-SecureString  "P@ssword!1" -AsPlainText -Force 
$cert = New-AzApplicationGatewaySSLCertificate -Name cert01 -CertificateFile "C:\AAD\Securities\Certificates\sslcert.com.cer" -Password $passwd 


#Create the HTTP listener for the application gateway

$listener = New-AzApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SSLCertificate $cert

#Upload the certificate to be used on the TLS/SSL-enabled back-end pool resources

#$authcert = New-AzApplicationGatewayAuthenticationCertificate -Name 'allowlistcert1' -CertificateFile C:\cert.cer

$trustedRootCert01 = New-AzApplicationGatewayTrustedRootCertificate -Name "test1" -CertificateFile "C:\AAD\Securities\Certificates\sslcert.com.cer"

#Configure the HTTP settings for the application gateway back end

$poolSetting01 = New-AzApplicationGatewayBackendHttpSettings -Name "setting01" -Port 443 -Protocol Https -CookieBasedAffinity Disabled -TrustedRootCertificate $trustedRootCert01 -HostName "test1"

#Create a load-balancer routing rule that configures the load balancer

$rule = New-AzApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

#Configure the instance size of the application gateway

$sku = New-AzApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

#Configure the TLS/SSL policy to be used on the application gateway

$SSLPolicy = New-AzApplicationGatewaySSLPolicy -MinProtocolVersion TLSv1_2 -CipherSuite "TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256", "TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384", "TLS_RSA_WITH_AES_128_GCM_SHA256" -PolicyType Custom

$appgw = New-AzApplicationGateway -Name appgateway -SSLCertificates $cert -ResourceGroupName "appgw-rg" -Location "East US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting01 -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SSLPolicy $SSLPolicy -TrustedRootCertificate $trustedRootCert01 -Verbose

```

#### <a name="deploy-azure-web-apps"></a>Azure Web Uygulamalarını Dağıtma
Azure Uygulama Hizmeti, Python, Ruby, C#ve Java gibi dilleri kullanarak web uygulamaları oluşturmanıza ve barındırmanıza olanak tanır. Azure, hemen hemen tüm programlama dillerinin Azure Uygulama Hizmeti platformunda çalışmasına izin veren özel kapsayıcıları da destekler.

#### <a name="create-an-app-service-plan-in-free-tier"></a>Ücretsiz katmanda Bir Uygulama Hizmeti planı oluşturma
    New-AzAppServicePlan -Name $webappname -Location $location -ResourceGroupName $webappname -Tier Free

#### <a name="create-a-web-app"></a>Web uygulaması oluşturma
    New-AzWebApp -Name $webappname -Location $location -AppServicePlan $webappname -ResourceGroupName $webappname

    Write-Host "Configure a CNAME record that maps $fqdn to $webappname.azurewebsites.net"
    Read-Host "Press [Enter] key when ready ..."

#### <a name="before-continuing-go-to-your-azure-domain-name-system-configuration-ui-for-your-custom-domain-and-follow-the-instructions-at-httpsakamsappservicecustomdns-to-configure-a-cname-record-for-the-hostname-www-and-point-it-your-web-apps-default-domain-name"></a>Devam etmeden önce, özel etki alanınız için Azure Alan Adı Sistemi https://aka.ms/appservicecustomdns yapılandırma Kullanıcı Arabirimi'ne gidin ve ana bilgisayar adı "www" için bir CNAME kaydı yapılandırmak ve web uygulamanızın varsayılan etki alanı adını işaret etmek için yönergeleri izleyin

#### <a name="upgrade-app-service-plan-to-shared-tier-minimum-required-by-custom-domains"></a>Uygulama Hizmeti planını Paylaşılan katmana yükseltme (özel etki alanları tarafından en az gerekli)
    Set-AzAppServicePlan -Name $webappname -ResourceGroupName $webappname -Tier Shared

#### <a name="add-a-custom-domain-name-to-the-web-app"></a>Web uygulamasına özel bir etki alanı adı ekleme
    Set-AzWebApp -Name $webappname -ResourceGroupName $webappname `-HostNames @($fqdn,"$webappname.azurewebsites.net")

## <a name="guidance-and-recommendations"></a>Rehberlik ve öneriler

### <a name="network"></a>Ağ
Dağıtımı tamamladıktan sonra, web uygulaması güvenlik duvarı etkin leştirilmiş bir uygulama ağ geçidiniz vardır.

Ağ geçidi örneği HTTPS için 443 bağlantı noktasını ortaya çıkarır. Bu yapılandırma, uygulamamıza yalnızca HTTPS üzerinden 443 bağlantı noktasından erişilmesini sağlar.

Kullanılmayan bağlantı noktalarını engellemek ve saldırı yüzeyine maruz kalmanın sınırlandırılması en iyi güvenlik uygulamasıdır.

#### <a name="add-network-security-groups-to-the-app-service-instance"></a>Uygulama Hizmeti örneğine ağ güvenlik grupları ekleme

Uygulama Hizmeti örnekleri sanal ağlarla tümleştirilebilir. Bu tümleştirme, uygulamanın gelen ve giden trafiğini yöneten ağ güvenliği grubu ilkeleriyle yapılandırılmasını sağlar.

1. Bu özelliği etkinleştirmek için, **Ayarlar**altında azure uygulaması örnek bıçak, **Ağ**seçer. Sağ bölmede, **VNet Tümleştirme**altında yapılandırın.

   ![Yeni sanal ağ entegrasyonu](./media/secure-web-app/app-vnet-menu.png)

    *App Service için yeni sanal ağ entegrasyonu*
1. Bir sonraki sayfada **VNET Ekle 'yi (önizleme)** seçin.

1. Sonraki menüde, dağıtımda oluşturulan ve .' `aad-vnet`ile başlayan sanal ağı seçin. Yeni bir alt ağ oluşturabilir veya varolan bir alt ağ seçebilirsiniz.
   Bu durumda, yeni bir alt ağ oluşturun. Adres **aralığını** **10.0.3.0/24** olarak ayarlayın ve alt **net app-subnet'inadını**belirleyin.

   ![App Service sanal ağ yapılandırması](./media/secure-web-app/app-vnet-config.png)

    *App Service için sanal ağ yapılandırması*

Artık sanal ağ tümleştirmesini etkinleştirdiğinize göre, uygulamamıza ağ güvenlik grupları ekleyebilirsiniz.

1. Arama kutusunu kullanın, **ağ güvenlik gruplarını**arayın. Sonuçlarda **Ağ güvenlik gruplarını** seçin.

    ![Ağ güvenlik gruplarını arama](./media/secure-web-app/nsg-search-menu.png)

    *Ağ güvenlik gruplarını arama*

2. Sonraki menüde **Ekle'yi**seçin. NSG'nin **adını** ve bulunması gereken **Kaynak grubunu** girin. Bu NSG, uygulama ağ geçidinin alt ağına uygulanacaktır.

    ![NSG oluşturma](./media/secure-web-app/nsg-create-new.png)

    *NSG oluşturma*

3. NSG oluşturulduktan sonra seçin. Bıçaklarında, **Ayarlar**altında **Gelen Güvenlik kurallarını**seçin. Bu ayarları, bağlantı noktası 443 üzerinden uygulama ağ geçidine gelen bağlantılara izin verecek şekilde yapılandırın.

   ![NSG'yi yapılandırın](./media/secure-web-app/nsg-gateway-config.png)

   *NSG'yi yapılandırın*

4. Ağ geçidi NSG için giden kurallarda, hizmet etiketini hedefleyen bir kural oluşturarak Uygulama Hizmeti örneğine giden bağlantılara izin veren bir kural ekleyin`AppService`

   ![NSG için giden kuralları ekleme](./media/secure-web-app/nsg-outbound-allowappserviceout.png)

   *NSG için giden kuralları ekleme*

    Ağ geçidinin giden kuralları sanal ağa göndermesine izin vermek için başka bir giden kural ekleyin.

   ![Başka bir giden kuralı ekleme](./media/secure-web-app/nsg-outbound-vnet.png)

    *Başka bir giden kuralı ekleme*

5. NSG'nin alt ağlarında, **Ilişkilendir'i**seçin , dağıtımda oluşturulan sanal ağı seçin ve **gw-subnet**adlı ağ geçidi alt ünü seçin. NSG alt ağa uygulanır.

6. Önceki adımda olduğu gibi, bu kez App Service örneği için başka bir NSG oluşturun. Ona bir isim ver. Uygulama ağ geçidi NSG için yaptığınız gibi bağlantı noktası 443 için gelen kuralı ekleyin.

   Bir Uygulama Hizmeti Ortamı örneğinde dağıtılan bir Uygulama Hizmeti örneğiniz varsa, ki bu uygulama için geçerli değildir, Uygulama Hizmeti NSG'nizin gelen güvenlik gruplarında 454-455 bağlantı noktalarını açarak Azure Hizmet Durumu sondalarına izin vermek için gelen kurallar ekleyebilirsiniz. Yapılandırma şu şekildedir:

   ![Azure Hizmet Durumu sondaları için kurallar ekleme](./media/secure-web-app/nsg-create-healthprobes.png)

    *Azure Hizmet Durumu sondaları için kurallar ekleme (yalnızca Uygulama Hizmet Ortamı)*

Saldırı yüzeyini sınırlamak için, Uygulama Hizmeti ağ ayarlarını yalnızca uygulama ağ geçidinin uygulamaya erişmesine izin verecek şekilde değiştirin.
Ayarları uygulamak için, **IP Kısıtlamaları** sekmesini seçerek ve yalnızca uygulama ağ geçidinin IP'sinin hizmete doğrudan erişmesine izin veren bir izin kuralı oluşturarak App Service ağ sekmesine gidin. Genel bakış sayfasından ağ geçidinin IP adresini alabilirsiniz. IP **Adresi CIDR** sekmesine, ip adresini bu `<GATEWAY_IP_ADDRESS>/32`biçimde girin: .

![Yalnızca ağ geçidine izin ver](./media/secure-web-app/app-allow-gw-only.png)

*Uygulama Hizmetine erişmek için yalnızca ağ geçidi IP'sine izin ver*

### <a name="azure-domain-name-system"></a>Azure Alan Adı Sistemi 
Azure Etki Alanı Adı Sistemi veya Azure Etki Alanı Adı Sistemi, bir web sitesinin veya hizmet adının IP adresine çevrilme (veya çözümlenmesinden) sorumludur. Azure Alan Adıhttps://docs.microsoft.com/azure/dns/dns-overview) Sistemi, Azure altyapılarını kullanarak ad çözümlemesi sağlayan Alan Adı Sistemi etki alanları için bir barındırma hizmetidir. Kullanıcılar, Azure'da etki alanlarını barındırarak, alan adı sistemi kayıtlarını diğer Azure hizmetleriyle aynı kimlik bilgilerini, API'leri, araçları ve faturalandırmayı kullanarak yönetebilir. Azure Alan Adı Sistemi, özel Alan Adı Sistemi etki alanlarını da destekler.

### <a name="azure-disk-encryption"></a>Azure Disk Şifrelemesi
Azure Disk Şifreleme, veri diskleri için birim şifreleme sağlamak için Windows'un BitLocker özelliğinden yararlanır. Çözüm, disk şifreleme anahtarlarını kontrol etmeye ve yönetmeye yardımcı olmak için Azure Key Vault ile tümleşir.

### <a name="identity-management"></a>Kimlik yönetimi
Aşağıdaki teknolojiler, Azure ortamında kart sahibi verilerine erişimi yönetmek için özellikler sağlar
- Azure Active Directory, Microsoft'un çok kiracılı bulut tabanlı dizin ve kimlik yönetimi hizmetidir. Bu çözümün tüm kullanıcıları, Azure WebApp'a erişen kullanıcılar da dahil olmak üzere Azure Active Directory'de oluşturulur.
- Azure rol tabanlı erişim denetimi, yöneticilerin yalnızca kullanıcıların işlerini gerçekleştirmek için gereken erişim miktarını vermek için ince taneli erişim izinlerini tanımlamasına olanak tanır. Yöneticiler, her kullanıcıya Azure kaynakları için sınırsız izin vermek yerine, kart sahibi verilerine erişmek için yalnızca belirli eylemlere izin verebilir. Abonelik erişimi abonelik yöneticisiyle sınırlıdır.
- Azure Active Directory Ayrıcalıklı Kimlik Yönetimi, müşterilerin kart sahibi verileri gibi belirli bilgilere erişimi olan kullanıcı sayısını en aza indirmesini sağlar. Yöneticiler, ayrıcalıklı kimlikleri ve kaynaklara erişimlerini keşfetmek, kısıtlamak ve izlemek için Azure Etkin Dizin Ayrıcalıklı Kimlik Yönetimi'ni kullanabilir. Bu işlevsellik, gerektiğinde isteğe bağlı, tam zamanında yönetim erişimi zorlamak için de kullanılabilir.
- Azure Etkin Dizin Kimlik Koruması, kuruluşun kimliklerini etkileyen olası güvenlik açıklarını algılar, kuruluşun kimlikleri ile ilgili algılanan şüpheli eylemlere otomatik yanıtlar yapılandırır ve bunları çözmek için uygun eylemi yapmak üzere şüpheli olayları araştırır.
### <a name="secrets-management"></a>Gizli dizi yönetimi
Çözüm, anahtarların ve sırların yönetimi için Azure Key Vault'u kullanır. Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Aşağıdaki Azure Anahtar Kasası özellikleri, müşterilerin bu tür verileri korumasına ve bunlara erişmemede yardımcı olur
   - Gelişmiş erişim ilkeleri gereksinim bazında yapılandırılır.
   - Anahtar Vault erişim ilkeleri, anahtarlar ve sırlar için gereken en az izinle tanımlanır.
   - Key Vault'taki tüm anahtarlar ve sırlar son kullanma tarihlerine sahiptir.
   - Key Vault'taki tüm anahtarlar özel donanım güvenlik modülleri ile korunmaktadır. Anahtar türü bir donanım güvenlik modülü (HSM) Korumalı 2048 bit RSA Anahtarıdır.
   - Key Vault ile anahtarları ve sırları (kimlik doğrulama anahtarları, depolama hesap anahtarları, veri şifreleme anahtarları gibi) şifreleyebilirsiniz. PFX dosyaları ve parolaları) donanım güvenlik modülleri (HSM) tarafından korunan anahtarları kullanarak. 
   - Belirli bir kapsamdaki kullanıcılara, gruplara ve uygulamalara izin atamak için Rol Tabanlı Erişim Denetimi'ni (RBAC) kullanın.     
   - TLS sertifikalarınızı otomatik yenileme ile yönetmek için Key Vault'u kullanın. 
   - Key Vault için tanılama günlükleri en az 365 günlük bir bekletme süresi ile etkinleştirilir.
   - Anahtarlar için izin verilen şifreleme işlemleri gerekli olanla sınırlıdır.
### <a name="azure-security-center"></a>Azure Güvenlik Merkezi
Azure Güvenlik Merkezi ile müşteriler iş yükleri arasında güvenlik ilkelerini merkezi olarak uygulayabilir ve yönetebilir, tehditlere maruz kalma sınırını sınırlayabilir ve saldırıları algılayabilir ve yanıtlayabilir. Ayrıca 
   - Azure Güvenlik Merkezi, güvenlik duruşunu iyileştirmeye ve verileri korumaya yardımcı olmak için yapılandırma ve hizmet önerileri sağlamak için Azure hizmetlerinin varolan yapılandırmalarına erişir.
   - Azure Güvenlik Merkezi, müşterileri ortamlarını hedef alan olası saldırılarkonusunda uyarmak için çeşitli algılama özellikleri kullanır. Bu uyarılar uyarıyı neyin tetiklediği, hedeflenen kaynaklar ve saldırının kaynağı hakkındaki değerli bilgileri içerir. Azure Güvenlik Merkezi'nde, bir tehdit veya şüpheli etkinlik gerçekleştiğinde tetiklenen önceden tanımlanmış güvenlik uyarıları kümesi vardır. Azure Güvenlik Merkezi'ndeki özel uyarı kuralları, müşterilerin çevrelerinden zaten toplanan verilere dayalı olarak yeni güvenlik uyarıları tanımlamasına olanak tanır.
   - Azure Güvenlik Merkezi, müşterilerin olası güvenlik sorunlarını keşfetmesini ve busorunları çözmesini kolaylaştırarak öncelikli güvenlik uyarıları ve olayları sağlar. Tespit edilen her tehdit için, tehditleri araştırmave tedavi etmede olay müdahale ekiplerine yardımcı olmak için bir tehdit istihbarat raporu oluşturulur.
### <a name="azure-application-gateway"></a>Azure Application Gateway 
   Mimari, web uygulama güvenlik duvarı yapılandırılan ve OWASP kural kümesi etkinleştirilmiş bir Azure Uygulama Ağ Geçidi kullanarak güvenlik açıkları riskini azaltır. Ek özellikler şunlardır:
   - Uçuça TLS.
   - TLS v1.0 ve v1.1'i devre dışı.
   - TLSv1.2'yi etkinleştirin.
   - Web uygulaması güvenlik duvarı (önleme modu).
   - OWASP 3.0 ruleset ile önleme modu.
   - Tanılama günlüğe kaydetmeyi etkinleştirin.
   - Özel sağlık sondaları.
   - Azure Güvenlik Merkezi ve Azure Danışmanı ek koruma ve bildirimler sağlar. Azure Güvenlik Merkezi ayrıca bir itibar sistemi de sağlar.
### <a name="logging-and-auditing"></a>Günlük kaydı ve denetim
Azure hizmetleri, sistem ve kullanıcı etkinliğinin yanı sıra sistem durumunu kapsamlı bir şekilde kaydeder:
   - Etkinlik [günlükleri: Etkinlik günlükleri,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) abonelikteki kaynaklarda gerçekleştirilen işlemlerhakkında bilgi sağlar. Etkinlik günlükleri, bir işlemin başlatıcısının, oluşum zamanının ve durumunun belirlenmesine yardımcı olabilir.
   - Tanılama günlükleri: [Tanılama günlükleri,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) her kaynak tarafından yayılan tüm günlükleri içerir. Bu günlükler Arasında Windows olay sistem günlükleri, Azure Depolama günlükleri, Key Vault denetim günlükleri ve Uygulama Ağ Geçidi erişimi ve Güvenlik Duvarı günlükleri yer alır. Tüm tanılama günlükleri arşivleme için merkezi ve şifreli bir Azure depolama hesabına yazar. Bekletme, kuruluşa özgü bekletme gereksinimlerini karşılamak için 730 güne kadar kullanıcı tarafından yapılandırılabilir.
### <a name="azure-monitor-logs"></a>Azure İzleyici günlükleri
   Bu günlükler, işleme, depolama ve pano raporlaması için [Azure Monitor günlüklerinde](https://azure.microsoft.com/services/log-analytics/) birleştirilir. Veriler toplandıktan sonra, Log Analytics çalışma alanlarındaki her veri türü için ayrı tablolar halinde düzenlenir ve bu da tüm verilerin orijinal kaynağından bağımsız olarak birlikte analiz edilmesine olanak tanır. Ayrıca Azure Güvenlik Merkezi, müşterilerin güvenlik olay verilerine erişmek ve diğer hizmetlerden gelen verilerle birleştirmek için Kusto sorgularını kullanmasına olanak tanıyan Azure Monitor günlükleriyle tümleşir.

   Aşağıdaki Azure [izleme çözümleri](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) bu mimarinin bir parçası olarak dahildir

   - [Etkin Dizin Değerlendirmesi](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): Active Directory Health Check çözümü, sunucu ortamlarının risk ve sistem durumunu düzenli aralıklarla değerlendirir ve dağıtılan sunucu altyapısına özgü öncelikli bir öneri listesi sağlar.
   - [Aracı Sağlık](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): Aracı Sağlık çözümü kaç aracının dağıtılan ve coğrafi dağılımlarının yanı sıra yanıt vermeyen kaç aracının ve operasyonel veri gönderen aracı ların sayısını bildirir.
   - [Etkinlik Günlüğü Analizi](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): Activity Log Analytics çözümü, bir müşteri için tüm Azure aboneliklerinde Azure etkinlik günlüklerinin analizine yardımcı olur.
### <a name="azure-monitor"></a>Azure İzleyici
   [Azure Monitor,](https://docs.microsoft.com/azure/monitoring-and-diagnostics/)kuruluşların Azure kaynaklarında API çağrılarını izleme de dahil olmak üzere, kuruluşların denetim yapmasını, uyarıları oluşturmasını ve verileri arşivlemesini sağlayarak kullanıcıların performansı izlemesine, güvenliği korumasına ve eğilimleri belirlemesine yardımcı olur.
### <a name="application-insights"></a>Application Insights 
   [Application Insights,](https://docs.microsoft.com/azure/application-insights/app-insights-overview) web geliştiricileri için birden çok platformda genişletilebilir bir Uygulama Performans Yönetimi hizmetidir. Uygulama Öngörüleri performans anormalliklerini algılar ve müşteriler bunu canlı web uygulamasını izlemek için kullanabilir. Müşterilerin sorunları tanılayabamalarına ve kullanıcıların uygulamalarıyla gerçekte ne yaptıklarını anlamalarına yardımcı olan güçlü analiz araçları içerir. Müşterilerin performansı ve kullanılabilirliği sürekli olarak geliştirmelerine yardımcı olmak için tasarlanmıştır.

### <a name="azure-key-vault"></a>Azure Key Vault
   Anahtarları depolayabilmek için kuruluş için bir kasa oluşturun ve aşağıdaki gibi operasyonel görevler için hesap verebilirliği koruyun

   - Key Vault'ta depolanan veriler,   
   - Uygulama içgörü anahtarı
   - Veri Depolama Erişim anahtarı
   - Bağlantı dizesi
   - Veri tablosu adı
   - Kullanıcı Kimlik Bilgileri
   - Gelişmiş erişim ilkeleri gereksinim bazında yapılandırılır
   - Key Vault erişim ilkeleri anahtarlar ve sırlar için en az gerekli izinler ile tanımlanır
   - Key Vault'taki tüm anahtarlar ve sırlar son kullanma tarihlerine sahiptir
   - Key Vault'taki tüm anahtarlar donanım güvenlik modülü (HSM) [Key Type = donanım güvenlik modülü (HSM) Korumalı       
     2048-bit RSA Anahtar]
   - Tüm kullanıcılara/kimliklere Rol Tabanlı Erişim Denetimi (RBAC) kullanılarak minimum gerekli izinverilir
   - Başvurular, birbirlerine güvenmedikleri sürece Bir Anahtar Kasası'nı paylaşmaz ve çalışma zamanında aynı sırlara erişmelerine ihtiyaç duyarlar.
   - Key Vault için tanılama günlükleri en az 365 günlük bir bekletme süresi ile etkinleştirilir.
   - Anahtarlar için izin verilen şifreleme işlemleri gerekli olanla sınırlıdır

### <a name="vpn-and-expressroute"></a>VPN ve ExpressRoute
   Güvenli bir VPN tüneli veya [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) güvenli bu PaaS web uygulama başvuru mimarisinin bir parçası olarak dağıtılan kaynaklara bir bağlantı kurarak yapılandırılması gerekiyordu. Uygun şekilde vpn veya ExpressRoute ayarlayarak, müşteriler aktarım sırasındaki veriler için bir koruma katmanı ekleyebilir.

   Azure ile güvenli bir VPN tüneli uygulayarak, şirket içi ağ ile Azure Sanal Ağı arasında sanal özel bir bağlantı oluşturulabilir. Bu bağlantı Internet üzerinden gerçekleşir ve müşterilerin müşterinin ağı ile Azure arasındaki şifreli bir bağlantı içinde güvenli bir şekilde "tünel" bilgilerini "tünel" sağlar. Siteden Siteye VPN, on yıllardır her boyuttaki işletmeler tarafından dağıtılan güvenli ve olgun bir teknolojidir. Bu seçenekte IPsec tünel modu şifreleme mekanizması olarak kullanılır.

   VPN tüneli içindeki trafik, siteden siteye VPN ile Internet'te geçiş yaptığından, Microsoft başka, daha güvenli bir bağlantı seçeneği sunar. Azure ExpressRoute, Azure ile şirket içi konum veya Exchange barındırma sağlayıcısı arasında özel bir WAN bağlantısıdır. ExpressRoute bağlantıları Internet üzerinden gitmediğinden, bu bağlantılar Internet üzerinden normal bağlantılara göre daha fazla güvenilirlik, daha yüksek hızlar, daha düşük gecikme süreleri ve daha yüksek güvenlik sunar. Ayrıca, bu müşterinin telekomünikasyon sağlayıcısının doğrudan bir bağlantısı olduğundan, veriler Internet üzerinden seyahat etmez ve bu nedenle buna maruz kalmaz.

   Şirket içi ağı Azure'a genişleten güvenli bir karma ağ uygulamak için en iyi uygulamalar [kullanılabilir.](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid)

#### <a name="implement-azure-active-directory-oidc"></a>Azure Active Directory OIDC'yi uygulayın

1. Kaynak kod deposunu klonlamak için bu Git komutunu kullanın

 ``` git
 git clone https://github.com/Azure-Samples/AAD-Security
   ```
## <a name="update-the-redirect-urls"></a>Yönlendirme URL'lerini güncelleştirme
1.  Azure portalına geri dön. Sol daki gezinti bölmesinde Azure Etkin Dizin hizmetini seçin ve ardından Uygulama kayıtlarını seçin.
2.  Ortaya çıkan ekranda WebApp-OpenIDConnect-DotNet-code-v2 uygulamasını seçin.
3.  Kimlik Doğrulama sekmesinde o Yönlendirme URL'leri bölümünde, açılan kutudaki Web'i seçin ve aşağıdaki yönlendirme URL'lerini ekleyin.
    https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.nethttps://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net/signin-oidc o Gelişmiş ayarlar bölümünde Logout URL'sinihttps://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net/signout-oidc
4.  Marka sekmesinde o Giriş sayfası NıN URL'sini uygulama servisinizin adresine güncelleyin, örneğin. https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.net
        o Yapılandırmayı kaydedin.
5.  Uygulamanız bir web api'si arıyorsa, proje appsettings.json üzerinde gerekli değişiklikleri uyguladığınızdan emin olun, böylece localhost yerine yayınlanmış API URL'sini çağırır.
Örneği yayımlama
    1.  Uygulama Hizmeti'nin Genel Bakış sekmesinden, yayın profilini al bağlantısını tıklayarak yayımlama profilini indirin ve kaydedin. Kaynak denetimi gibi diğer dağıtım mekanizmaları da kullanılabilir.
    2.  Visual Studio'ya geçin ve WebApp-OpenIDConnect-DotNet-code-v2 projesine gidin. Çözüm Gezgini'ndeki projeye sağ tıklayın ve Yayımla'yı seçin. Alt çubuktaki Profili İçe Aktar'ı tıklatın ve daha önce indirdiğiniz yayımlama profilini aktarın.
    3.  Yapılaşı'ya tıklayın ve Bağlantı sekmesinde Hedef URL'yi ana sayfa url'sinde https://WebApp-OpenIDConnect-DotNet-code-v2-contoso.azurewebsites.nethttps olacak şekilde güncelleştirin. İleri'ye tıklayın.
    4.  Ayarlar sekmesinde, Kuruluş Kimlik Doğrulamasını Etkinleştir'in seçili olmadığından emin olun. Kaydet’e tıklayın. Ana ekranda Yayınla'ya tıklayın.
    5.  Visual Studio projeyi yayımlar ve otomatik olarak projenin URL'sine bir tarayıcı açar. Projenin varsayılan web sayfasını görürseniz, yayın başarılı oldu.
#### <a name="implement-multi-factor-authentication-for-azure-active-directory"></a>Azure Etkin Dizini için Çok Faktörlü Kimlik Doğrulama'yı uygulama
   Yöneticilerin portaldaki abonelik hesaplarının korunduğundan emin olması gerekir. Abonelik, oluşturduğunuz kaynakları yönettiği için saldırılara karşı savunmasızdır. Aboneliği korumak için, aboneliğin **Azure Etkin Dizin** sekmesinde Çok Faktörlü Kimlik Doğrulaması'nı etkinleştirin.

   Azure AD, belirli bir ölçüte uyan bir kullanıcıya veya kullanıcı gruplarına uygulanan ilkelere göre çalışır.
Azure, yöneticilerin portalda oturum açmak için iki faktörlü kimlik doğrulamasına ihtiyaç duyduğunu belirten bir varsayılan ilke oluşturur.
Bu ilkeyi etkinleştirdikten sonra, oturum açmanız ve Azure portalında tekrar oturum açmanız istenebilir.

Yönetici oturum açmaiçin MFA'yı etkinleştirmek için

   1. Azure portalındaki **Azure Etkin Dizin** sekmesine gidin
   2. Güvenlik kategorisi altında koşullu erişimi seçin. Bu ekranı görüyorsunuz

       ![Koşullu Erişim - İlkeler](./media/secure-aad-app/ad-mfa-conditional-add.png)

Yeni bir ilke oluşturamıyorsanız

   1. **MFA** sekmesine gidin.
   2. Ücretsiz deneme sürümüne abone olmak için Azure AD Premium **Free deneme sürümü** bağlantısını seçin.

   ![Azure AD Premium ücretsiz deneme sürümü](./media/secure-aad-app/ad-trial-premium.png)

Koşullu erişim ekranına geri dönün.

   1. Yeni ilke sekmesini seçin.
   2. İlke adını girin.
   3. MFA'yı etkinleştirmek istediğiniz kullanıcıları veya grupları seçin.
   4. **Access denetimleri** **altında, Hibe** sekmesini seçin ve ardından çok **faktörlü kimlik doğrulamasını (ve** isterseniz diğer ayarları) talep et'i seçin.

   ![MFA gerektirme](./media/secure-aad-app/ad-mfa-conditional-add.png)

   Ekranın üst kısmındaki onay kutusunu seçerek ilkeyi etkinleştirebilir veya **Koşullu Erişim** sekmesinde bunu yapabilirsiniz. İlke etkinleştirildiğinde, kullanıcıların portalda oturum açabilmesi için MFA'ya ihtiyaç duyar.

   Tüm Azure yöneticileri için MFA gerektiren bir temel ilke vardır. Hemen portalda etkinleştirebilirsiniz. Bu ilkeyi etkinleştirmek geçerli oturumu geçersiz kakabilir ve sizi yeniden oturum açmaya zorlayabilir.

   Temel ilke etkin değilse
   1.   **Yöneticiler için MFA'yı**gerektir'i seçin.
   2.   **Hemen Kullan ilkesini**seçin.

   ![Hemen Kullan ilkesini seçin](./media/secure-aad-app/ad-mfa-conditional-enable.png)

#### <a name="use-azure-sentinel-to-monitor-apps-and-resources"></a>Uygulamaları ve kaynakları izlemek için Azure Sentinel'i kullanın

   Bir uygulama büyüdükçe, kaynaklardan alınan tüm güvenlik sinyallerini ve ölçümlerini toplamak ve bunları eylem odaklı bir şekilde kullanışlı hale getirmek zorlaşır.

   Azure Sentinel, veri toplamak, olası tehdit türlerini algılamak ve güvenlik olaylarında görünürlük sağlamak üzere tasarlanmıştır.
Azure Sentinel, el ile müdahale beklerken, uyarıları ve olay yönetimi işlemlerini başlatmak için önceden yazılmış oyun kitaplarına güvenebilir.

   Örnek uygulama, Azure Sentinel'in izleyebileceği çeşitli kaynaklardan oluşur.
Azure Sentinel'i ayarlamak için öncelikle çeşitli kaynaklardan toplanan tüm verileri depolayan bir Log Analytics çalışma alanı oluşturmanız gerekir.

Bu çalışma alanını oluşturmak için

   1. Azure portalındaki arama **kutusunda, Log Analytics'i**arayın. **Günlük Analizi çalışma alanlarını**seçin.

   ![Günlük Analizi çalışma alanlarını ara](./media/secure-aad-app/sentinel-log-analytics.png)

   *Günlük Analizi çalışma alanlarını ara*

   2. Sonraki sayfada **Ekle'yi** seçin ve ardından çalışma alanı için bir ad, kaynak grubu ve konum sağlayın.
   ![Log Analytics çalışma alanı oluşturma](./media/secure-aad-app/sentinel-log-analytics-create.png)

   *Log Analytics çalışma alanı oluşturma*

   3. **Azure Sentinel'i**aramak için arama kutusunu kullanın.

   ![Azure Sentinel'i arama](./media/secure-aad-app/sentinel-add.png)

   *Azure Sentinel'i arama*

   4. **Ekle'yi** seçin ve ardından daha önce oluşturduğunuz Log Analytics çalışma alanını seçin.

   ![Günlük Analizi çalışma alanı ekleme](./media/secure-aad-app/sentinel-workspace-add.png)

   *Günlük Analizi çalışma alanı ekleme*

   5. Azure **Sentinel - Veri bağlayıcıları** sayfasında, **Yapılandırma** **altında, Veri bağlayıcılarını**seçin. Azure Sentinel'de analiz için Log Analytics depolama örneğine bağlayabileceğiniz bir dizi Azure hizmeti görürsünüz.

   ![Günlük Analytics veri bağlayıcıları](./media/secure-aad-app/sentinel-connectors.png)

      *Azure Sentinel'e veri bağlayıcısı ekleme*

   Örneğin, uygulama ağ geçidini bağlamak için aşağıdaki adımları izleyin:

   1. Azure Uygulama Ağ Geçidi örnek bıçaklarını açın.
   2. **İzleme** bölümünde **Tanılama ayarları**'nı seçin.
   3. **Tanıayar ayarı ekle'yi**seçin.

   ![Uygulama Ağ Geçidi tanılama ekleme](./media/secure-aad-app/sentinel-gateway-connector.png)
         
   *Uygulama Ağ Geçidi tanılama ekleme*

   4. **Tanılama ayarları** sayfasında, oluşturduğunuz Günlük Analizi çalışma alanını seçin ve ardından toplayıp Azure Sentinel'e göndermek istediğiniz tüm ölçümleri seçin. **Kaydet'i**seçin.

   ![Azure Sentinel konektör ayarları](./media/secure-aad-app/sentinel-connector-settings.png)



## <a name="cost-considerations"></a>Maliyetle ilgili konular
   Zaten bir Azure hesabınız yoksa, ücretsiz bir hesap oluşturabilirsiniz. Başlamak için [ücretsiz hesap sayfasına](https://azure.microsoft.com/free/) gidin, ücretsiz bir Azure hesabıyla neler yapabileceğinizi görün ve hangi ürünlerin 12 ay boyunca ücretsiz olduğunu öğrenin.

   Güvenlik özellikleriyle örnek uygulamadaki kaynakları dağıtmak için bazı premium özellikler için ödeme yapmanız gerekir. Uygulama ölçeklendirildikçe ve Azure tarafından sunulan ücretsiz katmanlar ve denemeler uygulama gereksinimlerini karşılamak için yükseltilmesi gerektiğinden, maliyetleriniz artabilir. Maliyetlerinizi tahmin etmek için Azure [fiyatlandırma hesaplayıcısını](https://azure.microsoft.com/pricing/calculator/) kullanın.

## <a name="next-steps"></a>Sonraki adımlar
   Aşağıdaki makaleler, güvenli uygulamalar tasarlamanıza, geliştirmenize ve dağıtmanıza yardımcı olabilir.

- [Tasarım](secure-design.md)
- [Geliştirme](secure-develop.md)
- [Dağıt](secure-deploy.md)
