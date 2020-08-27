---
title: Gelen trafik v1 denetimi
description: App Service Ortamı gelen trafiği denetleme hakkında bilgi edinin. Bu belge yalnızca eski v1 Ao kullanan müşteriler için sağlanır.
author: ccompy
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: fe9326ea9ebd5afe981b7ba6c34b1a5d51e084b0
ms.sourcegitcommit: 648c8d250106a5fca9076a46581f3105c23d7265
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88962069"
---
# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>App Service Ortamı gelen trafiği denetleme
## <a name="overview"></a>Genel Bakış
**Bir App Service Ortamı** Azure Resource Manager Sanal ağda **veya** klasik dağıtım modeli [sanal ağında][virtualnetwork]oluşturulabilir.  Yeni bir sanal ağ ve yeni alt ağ bir App Service Ortamı oluşturulduğu sırada tanımlanabilir. Bunun yerine, önceden var olan bir sanal ağda ve önceden var olan alt ağda bir App Service Ortamı oluşturulabilir.  Haziran 2016 itibariyle, ASE 'ler, ortak adres aralıkları veya RFC1918 adres alanları (özel adresler) kullanan sanal ağlara da dağıtılabilir.  Daha fazla bilgi için bkz. [App Service ortamı oluşturma][HowToCreateAnAppServiceEnvironment].

Her zaman bir alt ağ içinde App Service Ortamı oluşturun. Bir alt ağ, yukarı akış cihazları ve Hizmetleri arkasındaki gelen trafiği kilitlemek için kullanılabilen bir ağ sınırı sağlar. Bu kurulum yalnızca belirli bir yukarı akış IP adresinin HTTP ve HTTPS trafiğini kabul etmesine izin verir.

Bir alt ağdaki gelen ve giden ağ trafiği bir [ağ güvenlik grubu][NetworkSecurityGroups]kullanılarak denetlenir. Gelen trafiği denetlemek için bir ağ güvenlik grubunda ağ güvenlik kuralları oluşturun. Ardından, App Service Ortamı içeren alt ağa ağ güvenlik grubu atayın.

Bir alt ağa ağ güvenlik grubu atadıktan sonra, ağ güvenlik grubunda tanımlanan izin verme ve reddetme kurallarına göre App Service Ortamı uygulamalara gelen trafiğe izin verilir veya bu kuralları engellenir.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>App Service Ortamı kullanılan gelen ağ bağlantı noktaları
Ağ güvenlik grubuyla gelen ağ trafiğini kilitleyerek, bir App Service Ortamı tarafından kullanılan gerekli ve isteğe bağlı ağ bağlantı noktaları kümesini öğrenin.  Bazı bağlantı noktalarına gelen trafiği yanlışlıkla kapatmak bir App Service Ortamı işlev kaybına neden olabilir.

Aşağıdaki liste, bir App Service Ortamı tarafından kullanılan bağlantı noktalarını içerir. Tersi belirtilmedikçe, tüm bağlantı noktaları **TCP**ile yapılır:

* 454: Azure altyapısı tarafından TLS aracılığıyla App Service ortamları yönetmek ve sürdürmek için kullanılan  **bağlantı noktası** .  Bu bağlantı noktasına giden trafiği engellemez.  Bu bağlantı noktası her zaman bir ASE 'nin Genel VIP 'sine bağlanır.
* 455: Azure altyapısı tarafından TLS aracılığıyla App Service ortamları yönetmek ve sürdürmek için kullanılan  **bağlantı noktası** .  Bu bağlantı noktasına giden trafiği engellemez.  Bu bağlantı noktası her zaman bir ASE 'nin Genel VIP 'sine bağlanır.
* 80: bir App Service Ortamı App Service planlarında çalışan uygulamalara gelen HTTP trafiği için varsayılan bağlantı noktası.  ILB etkin bir Ao, bu bağlantı noktası Ao 'nun ıLB adresine bağlıdır.
* 443: bir App Service Ortamı App Service planlarında çalışan uygulamalara gelen TLS trafiği için varsayılan bağlantı noktası.  ILB etkin bir Ao, bu bağlantı noktası Ao 'nun ıLB adresine bağlıdır.
* 21: FTP için denetim kanalı.  Bu bağlantı noktası, FTP kullanılmıyorsa güvenli bir şekilde engellenebilir.  ILB özellikli bir AO 'da, bu bağlantı noktası bir AO için ıLB adresine bağlanabilir.
* 990: FTPS için denetim kanalı.  Bu bağlantı noktası, FTPS kullanılmıyorsa güvenle engellenebilir.  ILB özellikli bir AO 'da, bu bağlantı noktası bir AO için ıLB adresine bağlanabilir.
* 10001-10020: FTP için veri kanalları.  Denetim kanalında olduğu gibi, FTP kullanılmıyorsa bu bağlantı noktaları güvenle engellenebilir.  ILB özellikli bir AO 'da, bu bağlantı noktası ATıCı 'in ıLB adresine bağlanabilir.
* 4016: Visual Studio 2012 ile uzaktan hata ayıklama için kullanılır.  Bu bağlantı noktası, özellik kullanılmıyorsa güvenle engellenebilir.  ILB etkin bir Ao, bu bağlantı noktası Ao 'nun ıLB adresine bağlıdır.
* 4018: Visual Studio 2013 ile uzaktan hata ayıklama için kullanılır.  Bu bağlantı noktası, özellik kullanılmıyorsa güvenle engellenebilir.  ILB etkin bir Ao, bu bağlantı noktası Ao 'nun ıLB adresine bağlıdır.
* 4020: Visual Studio 2015 ile uzaktan hata ayıklama için kullanılır.  Bu bağlantı noktası, özellik kullanılmıyorsa güvenle engellenebilir.  ILB etkin bir Ao, bu bağlantı noktası Ao 'nun ıLB adresine bağlıdır.

## <a name="outbound-connectivity-and-dns-requirements"></a>Giden Bağlantı ve DNS Gereksinimleri
App Service Ortamı düzgün bir şekilde çalışması için, ayrıca çeşitli uç noktalara giden erişim gerekir. AX tarafından kullanılan dış uç noktaların tam listesi [ExpressRoute Için ağ yapılandırması](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) makalesinin "gerekli ağ bağlantısı" bölümünde bulunur.

App Service ortamları, sanal ağ için yapılandırılmış geçerli bir DNS altyapısı gerektirir.  Bir App Service Ortamı oluşturulduktan sonra DNS yapılandırması değiştirilirse, geliştiriciler bir App Service Ortamı yeni DNS yapılandırmasını çekmeye zorlayabilir.  **Yeniden başlatma** simgesini kullanarak bir sıralı ortam yeniden başlatması tetiklemeniz halinde, ortam yeni DNS yapılandırmasını alır. ( **Yeniden başlatma** simgesi, [Azure Portal][NewPortal]App Service ortamı yönetim dikey penceresinin en üstünde bulunur.)

Ayrıca, VNET üzerindeki özel DNS sunucularının bir App Service Ortamı oluşturmadan önce zaman ayarlaması önerilir.  Bir App Service Ortamı oluşturulması sırasında bir sanal ağın DNS yapılandırması değiştirilirse, App Service Ortamı oluşturma işlemi başarısız olur.  Benzer şekilde, VPN ağ geçidinin diğer ucunda erişilemeyen veya kullanılamayan özel bir DNS sunucusu varsa App Service Ortamı oluşturma işlemi de başarısız olur.

## <a name="creating-a-network-security-group"></a>Ağ Güvenlik Grubu oluşturma
Ağ güvenlik gruplarının nasıl çalıştığı hakkında tam Ayrıntılar için aşağıdaki [bilgileri][NetworkSecurityGroups]görün.  Aşağıdaki Azure hizmet yönetimi örneği, ağ güvenlik gruplarının vurgulamaları üzerine dokunmaktadır. Örnek, bir App Service Ortamı içeren bir alt ağa ağ güvenlik grubu yapılandırır ve uygular.

**Note:** Ağ güvenlik grupları, [Azure Portal](https://portal.azure.com) veya Azure PowerShell kullanılarak grafiksel olarak yapılandırılabilir.

Ağ güvenlik grupları öncelikle bir abonelikle ilişkili bir tek başına varlık olarak oluşturulur. Ağ güvenlik grupları bir Azure bölgesinde oluşturulduğundan, App Service Ortamı ağ güvenlik grubunu aynı bölgede oluşturun.

Aşağıdaki komut, bir ağ güvenlik grubu oluşturmayı gösterir:

```azurepowershell-interactive
New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"
```

Bir ağ güvenlik grubu oluşturulduktan sonra, bir veya daha fazla ağ güvenlik kuralı eklenir.  Kural kümesi zaman içinde değişebileceğinizden, kural öncelikleri için kullanılan numaralandırma düzeninin üzerinde yer kaplamalısınız. Bu uygulama zaman içinde ek kurallar eklemeyi kolaylaştırır.

Aşağıdaki örnekte, bir kural bir App Service Ortamı yönetmek ve sürdürmek için Azure altyapısının gerek duyduğu yönetim bağlantı noktalarına açıkça erişim verir.  Tüm yönetim trafiği TLS üzerinden akar ve istemci sertifikaları tarafından korunur. Bağlantı noktaları açık olsa da, Azure Yönetim Altyapısı dışında herhangi bir varlık tarafından erişilemez.

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
```

80 ve 443 numaralı bağlantı noktasına erişimi, yukarı akış cihazları veya hizmetleri 'nin arkasındaki bir App Service Ortamı gizlemek için kilitlerseniz, yukarı akış IP adresini unutmayın.  Örneğin, bir Web uygulaması güvenlik duvarı (WAF) kullanıyorsanız, WAF 'nin kendi IP adresi veya adresleri olacaktır. WAF, trafiği bir aşağı akış App Service Ortamı proxy 'ye alırken kullanır.  Bir ağ güvenlik kuralının *Sourceaddresspredüzeltmesini* PARAMETRESINDE bu IP adresini kullanmanız gerekir.

Aşağıdaki örnekte, belirli bir yukarı akış IP adresinden gelen trafiğe açıkça izin verilir.  *1.2.3.4* adresi, bir yukarı akış WAF IP adresi için yer tutucu olarak kullanılır.  Değeri, yukarı akış cihazınız veya hizmetiniz tarafından kullanılan adresle eşleşecek şekilde değiştirin.

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
```

FTP desteği istenirse, FTP denetim bağlantı noktası ve veri kanalı bağlantı noktalarına erişim sağlamak için aşağıdaki kuralları şablon olarak kullanın.  FTP bir durum bilgisi olmayan protokol olduğundan, FTP trafiğini geleneksel bir HTTP/HTTPS güvenlik duvarı veya proxy cihazından yönlendiremeyebilirsiniz.  Bu durumda, *Sourceaddresspredüzeltmesini* , FTP istemcilerinin üzerinde çalıştığı geliştirici veya DAĞıTıM makinelerinin IP adresi aralığı gibi farklı bir değere ayarlamanız gerekir. 

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP
```

(**Note:**  önizleme döneminde veri kanalı bağlantı noktası aralığı değişebilir.)

Visual Studio ile uzaktan hata ayıklama kullanılıyorsa aşağıdaki kurallar nasıl erişim izni verildiğini gösterir.  Her sürüm uzaktan hata ayıklama için farklı bir bağlantı noktası kullandığından, Visual Studio 'nun desteklenen her sürümü için ayrı bir kural vardır.  FTP erişiminde olduğu gibi, uzaktan hata ayıklama trafiği geleneksel bir WAF veya proxy cihazından düzgün şekilde akamayabilir.  *Sourceaddresspredüzeltmesini* , Visual Studio çalıştıran GELIŞTIRICI makinelerinin IP adresi aralığına ayarlanabilir.

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP
```

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Bir alt ağa ağ güvenlik grubu atama
Bir ağ güvenlik grubunun tüm dış trafiğe erişimi reddeden bir varsayılan güvenlik kuralı vardır. Bu kuralı yukarıdaki ağ güvenliği kuralları ile birleştirdiğinizde, yalnızca bir *Izin verme* eylemiyle ilişkili kaynak adres aralıklarından gelen trafik, bir App Service ortamı çalışan uygulamalara trafik gönderebilecektir.

Bir ağ güvenlik grubu güvenlik kuralları ile doldurulduktan sonra, App Service Ortamı içeren alt ağa atayın.  Atama komutu iki ada başvurur: App Service Ortamı sanal ağın adı ve App Service Ortamı oluşturulduğu alt ağın adı.  

Aşağıdaki örnekte, bir alt ağa ve sanal ağa atanmış bir ağ güvenlik grubu gösterilmektedir:

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'
```

Atama uzun süredir çalışan bir işlemdir ve tamamlanması birkaç dakika sürebilir. Ağ güvenlik grubu ataması başarılı olduktan sonra yalnızca *Izin verme* kurallarıyla eşleşen gelen trafik App Service ortamı uygulamalara başarılı bir şekilde ulaşacaktır.

Tamamlana için aşağıdaki örnek, ağ güvenlik grubunun alt ağdan nasıl kaldırılacağını ve ilişkisini kaldırmayı göstermektedir:

```azurepowershell-interactive
Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'
```

## <a name="special-considerations-for-explicit-ip-ssl"></a>Açık IP-SSL için özel konular
Bir uygulama açık bir IP-SSL adresiyle yapılandırılmışsa ( *yalnızca* genel VIP 'Si olan ASE 'ler için geçerlidir), App SERVICE ORTAMı varsayılan IP adresini kullanmak yerine, hem http hem de HTTPS trafiği, 80 ve 443 bağlantı noktaları dışındaki bağlantı noktaları üzerinden alt ağa akar.

Her IP SSL adresi tarafından kullanılan her bir bağlantı noktası çiftini bulmak için portala gidin ve App Service Ortamı Ayrıntılar UX dikey penceresini görüntüleyin.  **Tüm ayarlar**  >  **IP adresleri**' ni seçin.  **IP adresleri** dikey penceresinde, App Service ortamı için açıkça YAPıLANDıRıLMıŞ tüm IP-SSL adreslerinin bir tablosu gösterilir. Dikey pencerede Ayrıca, her IP SSL adresiyle ilişkili HTTP ve HTTPS trafiğini yönlendirmek için kullanılan özel bağlantı noktası çifti gösterilmektedir.  Bir ağ güvenlik grubundaki kuralları yapılandırırken DestinationPortRange parametreleri için bu bağlantı noktası çiftini kullanın.

Ao üzerindeki bir uygulama IP-SSL kullanmak üzere yapılandırıldığında, dış müşteriler özel bağlantı noktası çifti eşlemesi hakkında endişelenmez veya bu konuda endişelenmenize gerek kalmaz.  Uygulamalara giden trafik normal olarak yapılandırılan IP-SSL adresine akacaktır.  Özel bağlantı noktası çiftiyle çeviri, yönlendirme trafiğinin son bacağı Ao 'yu içeren alt ağa otomatik olarak yapılır. 

## <a name="getting-started"></a>Başlarken
App Service ortamları ile çalışmaya başlamak için bkz. [App Service ortamı giriş][IntroToAppServiceEnvironment].

Daha fazla bilgi için bkz. [App Service ortamı arka uç kaynaklarına güvenli bir şekilde bağlanma][SecurelyConnecttoBackend].

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: ../../virtual-network/virtual-networks-faq.md
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[NetworkSecurityGroups]: ../../virtual-network/virtual-network-vnet-plan-design-arm.md
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[SecurelyConnecttoBackend]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->