---
title: Azure Avustralya 'da Azure VPN Gateway
description: Azure Avustralya 'da VPN Gateway uygulama, HARONLE uyumlu olmak ve Avustralya kamu kuruluşlarını etkin bir şekilde korumak için kullanılır
author: galey801
ms.service: azure-australia
ms.topic: article
ms.date: 07/22/2019
ms.author: grgale
ms.openlocfilehash: 00588042fa11ace51eef40cdedbae14c1bd99801
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575426"
---
# <a name="azure-vpn-gateway-in-azure-australia"></a>Azure Avustralya 'da Azure VPN Gateway

Herhangi bir genel buluta sahip kritik bir hizmet, bulut kaynakları ve Hizmetleri 'nin mevcut şirket içi sistemlere güvenli bağlantısıdır. Azure 'da bu özelliği sağlayan hizmet Azure VPN Gateway. Bu makalede, bir VPN ağ geçidini, Avustralya sinyallerinin (ASD) [Information Security Manual (ISM) denetimleriyle](https://acsc.gov.au/infosec/ism/)uyumlu olacak şekilde yapılandırdığınızda göz önünde bulundurmanız gereken önemli noktaları özetlenmektedir.

Bir VPN ağ geçidi, Azure 'daki bir sanal ağ ve başka bir ağ arasında şifrelenmiş trafik göndermek için kullanılır. VPN ağ geçitleri tarafından üç senaryo karşılanır:

- Siteden siteye (S2S)
- Noktadan siteye (P2S)
- Ağdan ağa

Bu makale S2S VPN ağ geçitlerine odaklanır. Diyagram 1, örnek bir S2S VPN ağ geçidi yapılandırmasını gösterir.

![Çok siteli bağlantılar içeren VPN Gateway](media/vpngateway-multisite-connection-diagram.png)

*Diyagram 1 – Azure S2S VPN Gateway*

## <a name="key-design-considerations"></a>Önemli tasarım konuları

Azure 'ı Avustralya kamu müşterilerine bağlamak için üç ağ seçeneği vardır:

- SIMG
- Azure ExpressRoute
- Genel internet

Azure için Avustralya Cyber Güvenlik Merkezi 'nin [Tüketici Kılavuzu](https://servicetrust.microsoft.com/viewpage/Australia) , VPN Gateway (veya eşit korumalı sertifikalı bir üçüncü taraf hizmeti) üç ağ seçeneği ile birlikte kullanılır. Bu öneri, bağlantıların şifreleme ve bütünlük için ıSM denetimleriyle uyumlu olmasını sağlamaktır.

### <a name="encryption-and-integrity"></a>Şifreleme ve bütünlük

Varsayılan olarak VPN, ıKE el sıkışmalarının bir parçası olarak bağlantı kurulurken şifreleme ve bütünlük algoritmalarından ve parametrelerine göre anlaşır. IKE anlaşması sırasında, tercihe ait yapılandırma ve sıra, VPN ağ geçidinin Başlatıcı mi yoksa Yanıtlayıcı mı olduğuna bağlıdır. Bu atama, VPN cihazı aracılığıyla denetlenir. Bağlantının son yapılandırması VPN cihazının yapılandırması tarafından denetlenir. Doğrulanan VPN cihazları ve yapılandırmaları hakkında daha fazla bilgi için bkz. [VPN hizmetleri hakkında](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).

VPN ağ geçitleri, bağlantıda özel bir IPSec/ıKE ilkesi yapılandırarak şifrelemeyi ve bütünlüğü denetleyebilir.

### <a name="resource-operations"></a>Kaynak işlemleri

VPN ağ geçitleri, Azure ve Azure olmayan ortamlar arasında genel İnternet üzerinden bir bağlantı oluşturur. ISM, bağlantıların açık yetkilendirmesiyle ilgili denetimleri içerir. Varsayılan olarak, güvenli ortamlarda yetkisiz tüneller oluşturmak için VPN ağ geçitleri kullanmak mümkündür. Kuruluşların, VPN ağ geçitlerini ve bağlantılarını kimlerin oluşturup değiştirebilmelerini denetlemek için Azure rol tabanlı erişim denetimi 'ni (RBAC) kullanması önemlidir. Azure 'da VPN ağ geçitlerini yönetmek için yerleşik bir rol yoktur, bu nedenle özel bir rol gerekir.

Sahibe, katkıda buluna ve ağ katılımcısı rollerine erişim sıkı bir şekilde denetlenir. Ayrıca daha ayrıntılı erişim denetimi için Azure Active Directory Privileged Identity Management kullanmanızı öneririz.

### <a name="high-availability"></a>Yüksek kullanılabilirlik

Azure VPN ağ geçitleri birden çok bağlantıya sahip olabilir ve aynı şirket içi ortama birden çok şirket içi VPN cihazını destekleyebilir. Bkz. diyagram 1.

Azure 'daki sanal ağlarda bağımsız, etkin-Pasif veya etkin-etkin yapılandırmalarda dağıtılabilecek birden fazla VPN ağ geçidi olabilir.

Tüm VPN ağ geçitlerini [yüksek kullanılabilirliğe sahip bir yapılandırmada](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-highlyavailable)dağıtmanızı öneririz. Bir örnek, etkin-Pasif veya etkin-etkin modda iki VPN ağ geçidine bağlı iki şirket içi VPN cihazlarıdır. Bkz. diyagram 2.

![VPN Gateway yedekli bağlantıları](media/dual-redundancy.png)

*Diyagram 2 – etkin-etkin VPN ağ geçitleri ve iki VPN cihazı*

### <a name="forced-tunneling"></a>Zorlamalı tünel oluşturma

Zorlamalı tünel, Internet 'e bağlı tüm trafiği İnceleme ve denetim için VPN Gateway aracılığıyla şirket içi ortama geri yönlendirir veya zorlar. Zorlamalı tünel olmadan, Azure 'daki VM 'lerden Internet 'e yönelik trafik, trafiği İnceleme veya denetleme seçeneği olmadan Azure ağ altyapısını doğrudan genel Internet 'e erişir. Bir kuruluşun bir ortam için güvenli bir Internet ağ geçidi (SıG) kullanması gerektiğinde Zorlamalı tünel kritik öneme sahiptir.

## <a name="detailed-configuration"></a>Ayrıntılı yapılandırma

### <a name="service-attributes"></a>Hizmet öznitelikleri

Avustralya kamu sektörü için yapılandırılan S2S bağlantıları için VPN ağ geçitleri aşağıdaki özniteliklere sahip olmalıdır:

|Öznitelik | Seçilmelidir|
|--- | --- |
|gatewayType | SANAL|
|

KORUNAN için ıSM denetimleriyle uyum sağlamak için gereken öznitelik ayarları şunlardır:

|Öznitelik | Seçilmelidir|
|--- |---|
|vpnType |RouteBased|
|vpnClientConfiguration/vpnClientProtocols | Ike|
|

Azure VPN ağ geçitleri, IPSec ve ıKE protokol standartlarından bir dizi şifreleme algoritmasını destekler. Varsayılan ilke, çok sayıda üçüncü taraf VPN cihazıyla birlikte çalışabilirliği ayarlar. Sonuç olarak, ıKE el sıkışması sırasında uyumsuz bir yapılandırma üzerinde anlaşma yapılabilir. Bağlantıların Azure 'a yönelik şirket içi ortam bağlantıları için ıSM denetimlerini karşıladığından emin olmak için VPN ağ geçitlerinde vpnClientConfiguration 'a [Özel IPSec/IKE ilkesi](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-ipsecikepolicy-rm-powershell) parametreleri uygulamanızı öneririz. Anahtar öznitelikleri aşağıdaki tabloda gösterilmiştir.

|Öznitelik|Gerekir|Seçilmelidir|
|---|---|---|
|saLifeTimeSeconds|< anlamına gelen 14400 saniye|> 300 saniye|
|saDataSizeKilobytes| |> 1024 KB|
|ıpsecencryption şifreleme| |AES256-GCMAES256|
|ıpsecıntegrity| |SHA256-GCMAES256|
|ıkeencryptıon| |AES256-GCMAES256|
|ıkeıntegrity| |SHA256-GCMAES256|
|dhGroup|DHGroup14, DHGroup24, ECP256, ECP384|DHGroup2|
|pfsGroup|PFS2048, PFS24, ECP256, ECP384||
|

Önceki tabloda bulunan dhGroup ve pfsGroup için, diğer ayarların kullanılmasına rağmen ECP256 ve ECP384 tercih edilir.

### <a name="related-services"></a>İlgili hizmetler

Bir Azure VPN ağ geçidi tasarladığınızda ve yapılandırdığınızda, bir dizi ilgili hizmetin da mevcut ve yapılandırılmış olması gerekir.

|Hizmet | Eylem gerekiyor|
|--- | ---|
|Sanal ağ | VPN ağ geçitleri bir sanal ağa eklenir. Yeni bir VPN ağ geçidi oluşturmadan önce bir sanal ağ oluşturun.|
|Genel IP adresi | S2S VPN ağ geçitlerinin, şirket içi VPN cihazı ile VPN ağ geçidi arasında bağlantı kurması için genel bir IP adresi olması gerekir. S2S VPN ağ geçidi oluşturmadan önce genel IP adresi oluşturun.|
|Subnet | VPN ağ geçidi için sanal ağın bir alt ağını oluşturun.|
|

## <a name="implementation-steps-using-powershell"></a>PowerShell kullanarak uygulama adımları

### <a name="role-based-access-control"></a>Rol tabanlı erişim denetimi

1. Özel bir rol oluşturun. VirtualNetworkGateway katılımcısı bir örnektir. VPN ağ geçitleri oluşturmalarına ve değiştirmesine izin verilecek kullanıcılara atanacak bir rol oluşturun. Özel rol aşağıdaki işlemlere izin verir:

   Microsoft. Network/Virtualnetworkgateway/*  
   Microsoft. Network/Connections/*  
   Microsoft. Network/localnetworkgateway/*  
   Microsoft. Network/virtualNetworks/alt ağları/*  
   Microsoft. Network/publicIPAddresses/*  
   Microsoft. Network/Publicipöneklerini/*  
   Microsoft. Network/routeTables/*  

2. VPN Gateway 'leri ve şirket içi ortamlara bağlantıları oluşturmalarına ve yönetmesine izin verilen kullanıcılara özel rolü ekleyin.

### <a name="create-a-vpn-gateway"></a>VPN ağ geçidi oluşturma

Bu adımlarda, zaten bir sanal ağ oluşturmuş olduğunuz varsayılır.

1. Yeni bir genel IP adresi oluşturun.
2. Bir VPN ağ geçidi alt ağı oluşturun.
3. Bir VPN ağ geçidi IP yapılandırma dosyası oluşturun.
4. Bir VPN Ağ Geçidi oluşturun.
5. Şirket içi VPN cihazı için yerel ağ geçidi oluşturun.
6. Bir IPSec ilkesi oluşturun. Bu adım, Özel IPSec/ıKE ilkeleri kullandığınızı varsayar.
7. VPN ağ geçidi ve yerel ağ geçidi arasında IPSec ilkesini kullanarak bir bağlantı oluşturun.

### <a name="enforce-tunneling"></a>Tüneli zorla

Zorlamalı tünel gerekliyse, VPN ağ geçidini oluşturmadan önce:

1. Yol tablosu ve yol kuralları oluşturun.
2. Bir yol tablosunu uygun alt ağlarla ilişkilendirin.

VPN ağ geçidini oluşturduktan sonra:

- GatewayDefaultSite 'yi VPN ağ geçidinde şirket içi ortama ayarlayın.

### <a name="example-powershell-script"></a>Örnek PowerShell betiği

Özel bir IPSec/ıKE ilkesi oluşturmak için kullanılan PowerShell betiğinin bir örneği, Avustralya KORUMALı güvenlik sınıflandırması için ıSM denetimleriyle uyumludur.

Sanal ağın, VPN ağ geçidinin ve yerel ağ geçitlerinin var olduğunu varsayar.

#### <a name="create-an-ipsecike-policy"></a>IPSec/ıKE ilkesi oluşturma

Aşağıdaki örnek betik, aşağıdaki algoritmalara ve parametrelere sahip bir IPSec/ıKE ilkesi oluşturur:

- Ike AES256, SHA256, DHGroup ECP256
- Bkz AES256, SHA256, PFS ECP256, SA yaşam 14.400 saniye ve 102.400.000 KB

```powershell
$custompolicy = New-AzIpsecPolicy `
                    -IkeEncryption AES256 `
                    -IkeIntegrity SHA256 `
                    -DhGroup ECP256 `
                    -IpsecEncryption AES256 `
                    -IpsecIntegrity SHA256 `
                    -PfsGroup ECP256 `
                    -SALifeTimeSeconds 14400 `
                    -SADataSizeKilobytes 102400000
```

#### <a name="create-a-s2s-vpn-connection-with-the-custom-ipsecike-policy"></a>Özel IPSec/ıKE ilkesiyle bir S2S VPN bağlantısı oluşturma

```powershell
$vpngw = Get-AzVirtualNetworkGateway `
                    -Name "<yourVPNGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"
$localgw = Get-AzLocalNetworkGateway  `
                    -Name "<yourLocalGatewayName>" `
                    -ResourceGroupName "<yourResourceGroupName>"

New-AzVirtualNetworkGatewayConnection `
                    -Name "ConnectionName" `
                    -ResourceGroupName "<yourResourceGroupName>" `
                    -VirtualNetworkGateway1 $vpngw `
                    -LocalNetworkGateway2 $localgw `
                    -Location "Australia Central" `
                    -ConnectionType IPsec `
                    -IpsecPolicies $custompolicy `
                    -SharedKey "AzureA1b2C3"
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, aktarım sırasında Avustralya kamu KORUMALı verilerinin güvenliğini sağlamak için bilgi güvenliği el kitabınızda belirtilen gereksinimleri karşılamak üzere VPN Gateway belirli bir yapılandırması ele alınmıştır. VPN ağ geçidinizi yapılandırma adımları için, bkz.:

- [Azure sanal ağ geçidine genel bakış](https://docs.microsoft.com/azure/vpn-gateway/)  
- [VPN Gateway nedir?](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways)  
- [PowerShell kullanarak siteden siteye VPN bağlantısı olan bir sanal ağ oluşturma](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell)  
- [VPN ağ geçidi oluşturma ve yönetme](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-tutorial-create-gateway-powershell)