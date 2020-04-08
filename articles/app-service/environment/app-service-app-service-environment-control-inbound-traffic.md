---
title: Gelen trafiği kontrol etmek v1
description: Bir Uygulama Hizmet Ortamı'na gelen trafiği nasıl kontrol edin. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: ccompy
ms.assetid: 4cc82439-8791-48a4-9485-de6d8e1d1a08
ms.topic: article
ms.date: 01/11/2017
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: 857b2b00aadced567bc8ac191cdd9908f7bea7a3
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804410"
---
# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>Uygulama Servis Ortamına Gelen Trafiği Denetleme
## <a name="overview"></a>Genel Bakış
Bir Azure Kaynak Yöneticisi **either** sanal ağında **veya** klasik dağıtım modeli [sanal ağında][virtualnetwork]Bir Uygulama Hizmet Ortamı oluşturulabilir.  Uygulama Hizmet Ortamı oluşturulduğunda yeni bir sanal ağ ve yeni alt ağ tanımlanabilir.  Alternatif olarak, önceden varolan bir sanal ağda ve önceden varolan bir alt ağda bir Uygulama Hizmet Ortamı oluşturulabilir.  Haziran 2016'da yapılan bir değişiklikle, AS'ler genel adres aralıklarını veya RFC1918 adres alanlarını (örn. özel adresler) kullanan sanal ağlara da dağıtılabilir.  Bir Uygulama Hizmeti Ortamı oluşturma hakkında daha fazla bilgi için, [Uygulama Hizmeti Ortamı Nasıl Oluşturulur'][HowToCreateAnAppServiceEnvironment]a bakın.

Bir alt ağ, gelen trafiği, http ve HTTPS trafiğinin yalnızca belirli upstream IP adreslerinden kabul edilmesi gibi yukarı akışlı aygıtların ve hizmetlerin arkasına kilitlemek için kullanılabilecek bir ağ sınırı sağladığından, her zaman bir alt ağ içinde oluşturulmalıdır.

Bir alt ağdaki gelen ve giden ağ trafiği bir [ağ güvenlik grubu][NetworkSecurityGroups]kullanılarak denetlenir. Gelen trafiği denetlemek, ağ güvenlik grubunda ağ güvenliği kuralları oluşturmayı ve ardından ağ güvenlik grubuna App Service Environment'ı içeren alt ağı atamayı gerektirir.

Bir ağ güvenlik grubu bir alt ağa atandıktan sonra, Uygulama Hizmeti Ortamındaki uygulamalara gelen trafiğe, ağ güvenlik grubunda tanımlanan izin verme ve reddetme kurallarına göre izin verilir/engellenir.

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="inbound-network-ports-used-in-an-app-service-environment"></a>Uygulama Hizmeti Ortamında Kullanılan Gelen Ağ Bağlantı Noktaları
Bir ağ güvenlik grubuyla gelen ağ trafiğini kilitlemeden önce, Bir Uygulama Hizmeti Ortamı tarafından kullanılan gerekli ve isteğe bağlı ağ bağlantı noktaları kümesini bilmek önemlidir.  Bazı bağlantı noktalarına trafiğin yanlışlıkla kapatılması, Bir Uygulama Hizmeti Ortamında işlevsellik kaybına neden olabilir.

Aşağıda, Bir Uygulama Hizmet Ortamı tarafından kullanılan bağlantı noktalarının listesi verilmiştir. Aksi açıkça belirtilmedikçe tüm bağlantı noktaları **TCP'dir:**

* 454: TLS üzerinden Uygulama Hizmet Ortamlarını yönetmek ve sürdürmek için Azure altyapısı tarafından kullanılan **gerekli bağlantı noktası.**  Bu bağlantı noktasının trafiğini engellemeyin.  Bu bağlantı noktası her zaman bir ASE'nin genel VIP'sine bağlıdır.
* 455: TLS üzerinden Uygulama Hizmet Ortamlarını yönetmek ve sürdürmek için Azure altyapısı tarafından kullanılan **gerekli bağlantı noktası.**  Bu bağlantı noktasının trafiğini engellemeyin.  Bu bağlantı noktası her zaman bir ASE'nin genel VIP'sine bağlıdır.
* 80: Uygulama Hizmeti Ortamında Uygulama Hizmeti Planlarında çalışan uygulamalara gelen HTTP trafiği için varsayılan bağlantı noktası.  ILB özellikli bir ASE'de bu bağlantı noktası ASE'nin ILB adresine bağlıdır.
* 443: Uygulama Hizmet Ortamında Uygulama Hizmet Planları'nda çalışan uygulamalara gelen TLS trafiği için varsayılan bağlantı noktası.  ILB özellikli bir ASE'de bu bağlantı noktası ASE'nin ILB adresine bağlıdır.
* 21: FTP için kontrol kanalı.  FTP kullanılıyorsa bu bağlantı noktası güvenle engellenebilir.  ILB özellikli bir ASE'de, bu bağlantı noktası bir ASE için ILB adresine bağlanabilir.
* 990: FTPS için kontrol kanalı.  FTPS kullanılmadığı takdirde bu bağlantı noktası güvenli bir şekilde engellenebilir.  ILB özellikli bir ASE'de, bu bağlantı noktası bir ASE için ILB adresine bağlanabilir.
* 10001-10020: FTP için veri kanalları.  Kontrol kanalında olduğu gibi, FTP kullanılmadığı takdirde bu bağlantı noktaları güvenli bir şekilde engellenebilir.  ILB özellikli bir ASE'de bu bağlantı noktası ASE'nin ILB adresine bağlanabilir.
* 4016: Visual Studio 2012 ile uzaktan hata ayıklama için kullanılır.  Özellik kullanılıyorsa bu bağlantı portu güvenle engellenebilir.  ILB özellikli bir ASE'de bu bağlantı noktası ASE'nin ILB adresine bağlıdır.
* 4018: Visual Studio 2013 ile uzaktan hata ayıklama için kullanılır.  Özellik kullanılıyorsa bu bağlantı portu güvenle engellenebilir.  ILB özellikli bir ASE'de bu bağlantı noktası ASE'nin ILB adresine bağlıdır.
* 4020: Visual Studio 2015 ile uzaktan hata ayıklama için kullanılır.  Özellik kullanılıyorsa bu bağlantı portu güvenle engellenebilir.  ILB özellikli bir ASE'de bu bağlantı noktası ASE'nin ILB adresine bağlıdır.

## <a name="outbound-connectivity-and-dns-requirements"></a>Giden Bağlantı ve DNS Gereksinimleri
Bir Uygulama Hizmet Ortamının düzgün çalışması için, çeşitli uç noktalara giden erişim de gerektirir. Bir ASE tarafından kullanılan harici uç noktaların tam [listesi, ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity) makalesi için Ağ Yapılandırması'nın "Gerekli Ağ Bağlantısı" bölümünde yer almaktadır.

Uygulama Hizmet Ortamları, sanal ağ için yapılandırılmış geçerli bir DNS altyapısı gerektirir.  Herhangi bir nedenle, Bir Uygulama Hizmet Ortamı oluşturulduktan sonra DNS yapılandırması değiştirilirse, geliştiriciler yeni DNS yapılandırmasını almaya bir Uygulama Hizmet Ortamı zorlayabilir.  [Azure portalında][NewPortal] Uygulama Hizmeti Ortamı yönetim bıçağının üst kısmında bulunan "Yeniden Başlat" simgesini kullanarak yuvarlanma ortamını yeniden başlatmayı tetiklemek, ortamın yeni DNS yapılandırmasını almasını sağlar.

Ayrıca, vnet'teki özel DNS sunucularının Bir Uygulama Hizmet Ortamı oluşturmadan önce kuruluma önceden kurulması önerilir.  Bir Uygulama Hizmet Ortamı oluşturulurken sanal ağın DNS yapılandırması değiştirilirse, bu Uygulama Hizmeti Ortamı oluşturma işleminin başarısız olmasına neden olur.  Benzer bir şekilde, VPN ağ geçidinin diğer ucunda özel bir DNS sunucusu varsa ve DNS sunucusuna erişilemez veya kullanılamıyorsa, Uygulama Hizmeti Ortamı oluşturma işlemi de başarısız olur.

## <a name="creating-a-network-security-group"></a>Ağ Güvenlik Grubu oluşturma
Ağ güvenlik gruplarının nasıl çalıştığına ilişkin tüm ayrıntılar için aşağıdaki [bilgilere][NetworkSecurityGroups]bakın.  Aşağıdaki Azure Hizmet Yönetimi örneği, bir ağ güvenlik grubunu uygulama hizmeti ortamı içeren bir alt ağa yapılandırmaya ve uygulamaya odaklanarak ağ güvenlik gruplarının önemli noktalarına değinmektedir.

**Not:** Ağ güvenlik [grupları, Azure Portalı](https://portal.azure.com) kullanılarak veya Azure PowerShell aracılığıyla grafik olarak yapılandırılabilir.

Ağ güvenlik grupları ilk olarak bir abonelikle ilişkili bağımsız bir varlık olarak oluşturulur. Ağ güvenlik grupları bir Azure bölgesinde oluşturulduğundan, ağ güvenlik grubunun App Service Environment ile aynı bölgede oluşturulduğundan emin olun.

Aşağıdakiler bir ağ güvenlik grubu oluşturmayı gösterir:

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

Bir ağ güvenlik grubu oluşturulduktan sonra, bir veya daha fazla ağ güvenlik kuralı eklenir.  Kurallar kümesi zaman içinde değişebileceğinden, zaman içinde ek kurallar eklemeyi kolaylaştırmak için kural öncelikleri için kullanılan numaralandırma düzenini ayarlamaları önerilir.

Aşağıdaki örnekte, Bir Uygulama Hizmeti Ortamını yönetmek ve sürdürmek için Azure altyapısının ihtiyaç duyduğu yönetim bağlantı noktalarına erişim sağlayan bir kural gösterilmektedir.  Tüm yönetim trafiğinin TLS üzerinden aktığını ve istemci sertifikaları yla güvence altına alınmıştır, bu nedenle bağlantı noktaları açılsa bile Azure yönetim altyapısı dışındaki herhangi bir kuruluş tarafından erişilemez.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP


Bir Uygulama Hizmet Ortamını yukarı akım aygıtların veya hizmetlerin arkasına "gizlemek" için 80 ve 443 bağlantı noktasına erişimi kilitlerken, yukarı akım IP adresini bilmeniz gerekir.  Örneğin, bir web uygulaması güvenlik duvarı (WAF) kullanıyorsanız, WAF'ın trafiği bir aşağı Uygulama Hizmet Ortamına proxy verirken kullandığı kendi IP adresi (veya adresleri) olacaktır.  Bu IP adresini bir ağ güvenlik kuralının *SourceAddressPrefix* parametresinde kullanmanız gerekir.

Aşağıdaki örnekte, belirli bir yukarı IP adresinden gelen trafiğe açıkça izin verilir.  *1.2.3.4* adresi, yukarı akım bir WAF'ın IP adresi için yer tutucu olarak kullanılır.  Yukarı akım aygıtınız veya hizmetiniz tarafından kullanılan adresle eşleşecek şekilde değeri değiştirin.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

FTP desteği isteniyorsa, FTP denetim bağlantı noktasına ve veri kanalı bağlantı noktalarına erişim sağlamak için aşağıdaki kurallar şablon olarak kullanılabilir.  FTP özel bir protokol olduğundan, FTP trafiğini geleneksel bir HTTP/HTTPS güvenlik duvarı veya proxy aygıtı üzerinden yönlendiremeyebilirsiniz.  Bu durumda *SourceAddressPrefix'i* farklı bir değere ayarlamanız gerekir ( örneğin FTP istemcilerinin çalıştırdığı geliştirici veya dağıtım makinelerinin IP adresi aralığı. 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**Not:** veri kanalı bağlantı noktası aralığı önizleme döneminde değişebilir.)

Visual Studio ile uzaktan hata ayıklama kullanılıyorsa, aşağıdaki kurallar erişimin nasıl verilebildiğini gösterir.  Her sürüm uzaktan hata ayıklama için farklı bir bağlantı noktası kullandığından, Visual Studio'nun desteklenen her sürümü için ayrı bir kural vardır.  FTP erişiminde olduğu gibi, uzaktan hata ayıklama trafiği geleneksel WAF veya proxy aygıtı üzerinden düzgün akmayabilir.  *SourceAddressPrefix* bunun yerine Visual Studio çalıştıran geliştirici makinelerinIP adresi aralığına ayarlanabilir.

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>Bir Alt Ağa Ağ Güvenlik Grubu Atama
Ağ güvenlik grubunun, tüm dış trafiğe erişimi reddeden varsayılan bir güvenlik kuralı vardır.  Yukarıda açıklanan ağ güvenliği kurallarının ve gelen trafiği engelleyen varsayılan güvenlik kuralının birleştirilmesinin sonucu, yalnızca *Bir İzin Verme* eylemiyle ilişkili kaynak adres aralıklarından gelen trafiğin Bir Uygulama Hizmet Ortamında çalışan uygulamalara trafik gönderebilmesidir.

Bir ağ güvenlik grubu güvenlik kurallarıyla doldurulduktan sonra, Uygulama Hizmet Ortamı'nı içeren alt ağa atanması gerekir.  Atama komutu, hem App Service Environment'ın bulunduğu sanal ağın adını hem de Uygulama Hizmet Ortamının oluşturulduğu alt ağın adını başvurur.  

Aşağıdaki örnekte, bir alt ağa ve sanal ağa atanan bir ağ güvenlik grubu gösterilmektedir:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

Ağ güvenliği grubu ataması başarılı olduktan sonra (atama uzun süren bir işlemdir ve tamamlanması birkaç dakika sürebilir), yalnızca gelen trafik eşleştirme kurallarına *izin verme* kuralları Uygulama Hizmet Ortamındaki uygulamalara başarıyla ulaşır.

Bütünlük için aşağıdaki örnek, ağ güvenlik grubunun alt ağdan nasıl kaldırılıp ilişkilendirilen bir şekilde kaldırılabildiğini ve böylece ilişkilendirilen ibareleri gösterir:

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>Açık IP-SSL için Özel Hususlar
Bir uygulama, Uygulama Hizmet Ortamının varsayılan IP adresini kullanmak yerine açık bir IP-SSL adresiyle *(yalnızca* genel VIP'si olan AS'ler için geçerli) yapılandırılırsa, hem HTTP hem de HTTPS trafiği 80 ve 443 bağlantı noktaları dışında farklı bir bağlantı noktası kümesi üzerinden alt ağa akar.

Her IP-SSL adresi tarafından kullanılan tek tek bağlantı noktaları çifti, Uygulama Servis Ortamı'nın ayrıntıları UX blade'den portal kullanıcı arabiriminde bulunabilir.  "Tüm ayarlar" --> "IP adresleri"ni seçin.  "IP adresleri" bıçak App Servis Ortamı için açıkça yapılandırılmış tüm IP-SSL adreslerinin bir tablo gösterir, her IP-SSL adresi ile ilişkili HTTP ve HTTPS trafiği yönlendirmek için kullanılan özel bağlantı noktası çifti ile birlikte.  Bir ağ güvenlik grubunda kuralları yapılandırırken DestinationPortRange parametreleri için kullanılması gereken bu bağlantı noktası çiftidir.

ASE'deki bir uygulama IP-SSL kullanacak şekilde yapılandırıldığında, harici müşteriler özel bağlantı noktası çifti eşlemesini görmez ve endişelenmelerine gerek kalmaz.  Uygulamalara giden trafik normal olarak yapılandırılan IP-SSL adresine akar.  Özel bağlantı noktası çiftine çeviri, trafiğin ASE'yi içeren alt ağına yönlendirmesinin son etabı sırasında otomatik olarak dahili olarak gerçekleşir. 

## <a name="getting-started"></a>Başlarken
Uygulama Hizmet Ortamlarına başlamak için Uygulama [Hizmet Ortamına Giriş][IntroToAppServiceEnvironment]

Bir Uygulama Hizmet Ortamından arka uç kaynağına güvenli bir şekilde bağlanan uygulamalarla ilgili ayrıntılar için, [Bir Uygulama Hizmet Ortamından Arka Uç kaynaklarına güvenli bir şekilde bağlanma][SecurelyConnecttoBackend]

[!INCLUDE [app-service-web-try-app-service](../../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md
[SecurelyConnecttoBackend]:  app-service-app-service-environment-securely-connecting-to-backend-resources.md
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->

