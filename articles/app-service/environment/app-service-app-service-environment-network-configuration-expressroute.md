---
title: Azure ExpressRoute v1'i yapılandırma
description: Azure ExpressRoute ile PowerApps için Uygulama Hizmet Ortamı için ağ yapılandırması. Bu doküman yalnızca eski v1 ASE'yi kullanan müşteriler için sağlanır.
author: stefsch
ms.assetid: 34b49178-2595-4d32-9b41-110c96dde6bf
ms.topic: article
ms.date: 10/14/2016
ms.author: stefsch
ms.custom: seodec18
ms.openlocfilehash: fc11c6932d625b119ad933f5d4d128b4355530c5
ms.sourcegitcommit: 6397c1774a1358c79138976071989287f4a81a83
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80804444"
---
# <a name="network-configuration-details-for-app-service-environment-for-powerapps-with-azure-expressroute"></a>Azure ExpressRoute ile PowerApps için Uygulama Hizmet Ortamı için ağ yapılandırma ayrıntıları

Müşteriler, şirket içi ağlarını Azure'a genişletmek için bir [Azure ExpressRoute][ExpressRoute] devresini sanal ağ altyapılarına bağlayabilir. App Service [Environment, sanal ağ][virtualnetwork] altyapısının bir alt ağında oluşturulur. App Service Environment'da çalışan uygulamalar, yalnızca ExpressRoute bağlantısı üzerinden erişilebilen arka uç kaynaklara güvenli bağlantılar kurar.  

Uygulama Hizmeti Ortamı şu senaryolarda oluşturulabilir:
- Azure Kaynak Yöneticisi sanal ağlar.
- Klasik dağıtım modeli sanal ağlar.
- Ortak adres aralıkları veya RFC1918 adres alanları (diğer bir şekilde özel adresler) kullanan sanal ağlar. 

[!INCLUDE [app-service-web-to-api-and-mobile](../../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="required-network-connectivity"></a>Gerekli ağ bağlantısı

Uygulama Hizmet Ortamı, ExpressRoute'a bağlı bir sanal ağda başlangıçta karşılanamayan ağ bağlantısı gereksinimlerine sahiptir.

Uygulama Hizmet Ortamı nın düzgün çalışması için aşağıdaki ağ bağlantısı ayarlarının gerekli olması gerektir:

* Dünya genelinde hem bağlantı noktası 80 hem de port 443'te Azure Depolama uç noktalarına giden ağ bağlantısı. Bu uç noktalar, App Service Environment ve diğer Azure bölgeleriyle aynı bölgede bulunur. Azure Depolama uç noktaları aşağıdaki DNS etki alanları altında çözümlenin: table.core.windows.net, blob.core.windows.net, queue.core.windows.net ve file.core.windows.net.  

* Bağlantı noktası 445'teki Azure Dosyaları hizmetine giden ağ bağlantısı.

* Uygulama Hizmeti Ortamı ile aynı bölgede bulunan Azure SQL Veritabanı uç noktalarına giden ağ bağlantısı. SQL Veritabanı uç noktaları, 1433, 11000-11999 ve 14000-14999 bağlantı noktalarına açık erişim gerektiren database.windows.net etki alanı altında çözülür. SQL Database V12 bağlantı noktası kullanımı hakkında ayrıntılı bilgi [için, 4,5 ADO.NET için 1433'ün ötesindeki Bağlantı Noktaları'na](../../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md)bakın.

* Azure yönetim düzlemi uç noktalarına giden ağ bağlantısı (hem Azure klasik dağıtım modeli hem de Azure Kaynak Yöneticisi uç noktaları). Bu uç noktalara bağlantı, management.core.windows.net ve management.azure.com etki alanlarını içerir. 

* ocsp.msocsp.com, mscrl.microsoft.com ve crl.microsoft.com etki alanına giden ağ bağlantısı. TLS işlevselliğini desteklemek için bu etki adlarına bağlantı gereklidir.

* Sanal ağ için DNS yapılandırması, bu makalede belirtilen tüm uç noktaları ve etki alanlarını çözebilmeli. Uç noktalar çözülemezse, App Service Environment oluşturma başarısız olur. Varolan tüm Uygulama Hizmet Ortamı sağlıksız olarak işaretlenir.

* DNS sunucuları ile iletişim için bağlantı noktası 53'te giden erişim gereklidir.

* VPN ağ geçidinin diğer ucunda özel bir DNS sunucusu varsa, DNS sunucusuna App Service Environment içeren alt ağdan erişilebilmelidir. 

* Giden ağ yolu şirket içi yakınlıklar aracılığıyla seyahat edemez ve şirket içinde tünel eitemez. Bu eylemler, Uygulama Hizmeti Ortamından giden ağ trafiğinin etkili NAT adresini değiştirir. App Service Environment giden ağ trafiğinin NAT adresinde yapılan değişiklikler, uç noktaların çoğunda bağlantı hatalarına neden olur. Uygulama Hizmeti Ortamı oluşturma başarısız olur. Varolan tüm Uygulama Hizmet Ortamı sağlıksız olarak işaretlenir.

* App Service Environment için gerekli bağlantı noktalarına gelen ağ erişimine izin verilmelidir. Ayrıntılar için, [Uygulama Hizmeti Ortamı'na gelen trafiği nasıl kontrol edin.][requiredports]

DNS gereksinimlerini karşılamak için, sanal ağ için geçerli bir DNS altyapısının yapılandırıldığından ve korundığından emin olun. Uygulama Hizmet Ortamı oluşturulduktan sonra DNS yapılandırması değiştirilirse, geliştiriciler App Service Environment'ı yeni DNS yapılandırmasını almaya zorlayabilir. [Azure portalında][NewPortal]Uygulama Hizmeti Ortamı yönetimi altında **Yeniden Başlat** simgesini kullanarak yuvarlanma ortamını yeniden başlatmayı tetikleyebilirsiniz. Yeniden başlatma, ortamın yeni DNS yapılandırmasını almasına neden olur.

Gelen ağ erişim gereksinimlerini karşılamak için, App Service Environment alt ağında bir [ağ güvenlik grubu (NSG)][NetworkSecurityGroups] yapılandırın. NSG, [App Service Environment'a gelen trafiği kontrol etmek için][requiredports]gerekli erişime izin verir.

## <a name="outbound-network-connectivity"></a>Giden ağ bağlantısı

Varsayılan olarak, yeni oluşturulan ExpressRoute devresi, giden internet bağlantısına izin veren varsayılan bir rotanın reklamını yapmaktadır. Uygulama Hizmet Ortamı, diğer Azure uç noktalarına bağlanmak için bu yapılandırmayı kullanabilir.

Ortak bir müşteri yapılandırması, giden internet trafiğini şirket içinde akmaya zorlayan kendi varsayılan rotalarını (0.0.0.0/0) tanımlamaktır. Bu trafik akışı her zaman App Servis Ortamı tatili. Giden trafik, artık çeşitli Azure uç noktalarıyla çalışmayan tanınmaz bir adres kümesine şirket içinde veya NAT'd tarafından engellenir.

Çözüm, Uygulama Hizmet Ortamı nı içeren alt ağda kullanıcı tanımlı (veya daha fazla) bir (veya daha fazla) rota (ÜDR) tanımlamaktır. UDR, varsayılan rota yerine onurlandırıldı alt ağa özgü yolları tanımlar.

Mümkünse aşağıdaki yapılandırmayı kullanın:

* ExpressRoute yapılandırması 0.0.0.0/0 reklamını sunar. Varsayılan olarak, yapılandırma kuvveti tüm giden trafiği şirket içinde tüneller.
* App Service Environment içeren alt nete uygulanan UDR, bir sonraki internet atlama türüyle 0.0.0.0/0 tanımlar. Bu yapılandırmanın bir örneği daha sonra bu makalede açıklanmıştır.

Bu yapılandırmanın birleşik etkisi, alt net düzeyindeki UDR'nin ExpressRoute kuvvet tünellemeden önce olmasıdır. App Service Environment'dan giden internet erişimi garanti altındadır.

> [!IMPORTANT]
> UDR'de tanımlanan rotalar, ExpressRoute yapılandırması tarafından reklamı yapılan rotalardan önce gelmek için yeterince özel olmalıdır. Bir sonraki bölümde açıklanan örnekte geniş 0.0.0.0/0 adres aralığı nı kullanır. Bu aralık, daha belirli adres aralıkları kullanan rota reklamları tarafından yanlışlıkla geçersiz kılınabilir.
> 
> Uygulama Hizmeti Ortamı, özel bakış yoluna genel bakış yolundan yolları çapraz reklam veren ExpressRoute yapılandırmalarıyla desteklenmez. Genel olarak yapılandırılan ExpressRoute yapılandırmaları, büyük bir Microsoft Azure IP adresi aralığı kümesi için Microsoft'tan rota reklamları alır. Bu adres aralıkları özel bakış yolunda çapraz reklamı yapılırsa, App Service Environment alt ağındaki tüm giden ağ paketleri müşterinin şirket içi ağ altyapısına zorla tünellenir. Bu ağ akışı şu anda App Service Environment ile desteklenmemektedir. Bir çözüm, özel bakan yolu için kamu bakan yolu çapraz reklam yolları durdurmaktır.
> 
> 

Kullanıcı tanımlı rotalar hakkında arka plan bilgileri için Sanal ağ trafiği yönlendirmesi'ne bakın. [Virtual network traffic routing][UDROverview]  

Kullanıcı tanımlı yolların nasıl oluşturulup yapılandırılabildiğini öğrenmek için [PowerShell'i kullanarak bir rota tablosuyla Route ağ trafiğine][UDRHowTo]bakın.

## <a name="udr-configuration"></a>UDR yapılandırması

Bu bölümde, Uygulama Hizmet Ortamı için örnek bir UDR yapılandırması gösterilmektedir.

### <a name="prerequisites"></a>Ön koşullar

* [Azure İndirmeler sayfasından][AzureDownloads]Azure PowerShell'i yükleyin. Haziran 2015 veya daha sonraki bir tarihi olan bir indirme seçin. **Komut satırı araçları** > **altında Windows PowerShell**, en son PowerShell cmdlets yüklemek için **Yükle'yi** seçin.

* App Service Environment tarafından özel kullanım için benzersiz bir alt ağ oluşturun. Benzersiz alt ağ, ÜTK'lerin yalnızca App Service Environment için alt net açık giden trafiğe uygulanmasını sağlar.

> [!IMPORTANT]
> Yalnızca yapılandırma adımlarını tamamladıktan sonra Uygulama Hizmet Ortamı'nı dağıtın. Adımlar, Uygulama Hizmeti Ortamı'nı dağıtmaya çalışmadan önce giden ağ bağlantısının kullanılabilir olmasını sağlar.

### <a name="step-1-create-a-route-table"></a>Adım 1: Rota tablosu oluşturma

Bu parçacıkta gösterildiği gibi, Batı ABD Azure bölgesinde **DirectInternetRouteTable** adlı bir rota tablosu oluşturun:

`New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest`

### <a name="step-2-create-routes-in-the-table"></a>Adım 2: Tabloda rota oluşturma

Giden internet erişimini etkinleştirmek için rota tablosuna rotalar ekleyin.  

İnternete giden erişimi yapılandırın. Bu snippet'te gösterildiği gibi 0.0.0.0/0 için bir rota tanımlayın:

`Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet`

0.0.0.0/0 geniş bir adres aralığıdır. Aralık, ExpressRoute tarafından reklamı yapılan ve daha spesifik olan adres aralıklarına göre geçersiz kılınan üründür. 0.0.0.0/0 rotası olan bir UDR, yalnızca 0.0.0.0/0'ın reklamını yapan bir ExpressRoute yapılandırmasıyla birlikte kullanılmalıdır. 

Alternatif olarak, Azure tarafından kullanılan CIDR aralıklarının güncel ve kapsamlı bir listesini indirin. Tüm Azure IP adres aralıkları için XML dosyası [Microsoft İndirme Merkezi'nden][DownloadCenterAddressRanges]edinilebilir.  

> [!NOTE]
>
> Azure IP adresi aralıkları zaman içinde değişir. Kullanıcı tanımlı rotalar senkronize tutmak için periyodik manuel güncelleştirmelere ihtiyaç duyar.
>
> Tek bir UDR varsayılan üst sınırı 100 rotavardır. 100 rota sınırına sığacak şekilde Azure IP adres aralıklarını "özetlemeniz" gerekir. UDR tanımlı rotaların ExpressRoute bağlantınız tarafından reklamı yapılan rotalardan daha spesifik olması gerekir.
> 

### <a name="step-3-associate-the-table-to-the-subnet"></a>Adım 3: Tabloyu alt ağla ilişkilendir

Rota tablosunu, Uygulama Hizmet Ortamı'nı dağıtmak istediğiniz alt ağla ilişkilendirin. Bu komut, **Doğrudan İnternetRouteTable** tablosunu Uygulama Hizmet Ortamı nı içeren **ASESubnet** alt ağına ilişkilendirir.

`Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'`

### <a name="step-4-test-and-confirm-the-route"></a>Adım 4: Rotayı test edin ve onaylayın

Rota tablosu alt ağa bağlandıktan sonra rotayı test edin ve onaylayın.

Sanal bir makineyi alt ağa dağıtın ve şu koşulları onaylayın:

* Bu makalede açıklanan Azure ve Azure dışı uç noktalarına giden trafik ExpressRoute devresi aşağı **akmaz.** Alt ağdan giden trafik şirket içinde tünel oluşturmaya zorlanırsa, App Service Environment oluşturma her zaman başarısız olur.
* DNS, bu makalede açıklanan uç noktaları arar, tüm bunları düzgün bir şekilde çözer. 

Yapılandırma adımlarını tamamladıktan ve rotayı onayladıktan sonra sanal makineyi silin. Uygulama Hizmet Ortamı oluşturulduğunda alt ağın "boş" olması gerekir.

Artık Uygulama Hizmet Ortamı'nı dağıtmaya hazırsınız!

## <a name="next-steps"></a>Sonraki adımlar

PowerApps için Uygulama Hizmet Ortamı'na başlamak için uygulama [hizmet ortamına giriş 'e][IntroToAppServiceEnvironment]bakın.

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/services/virtual-network/ 
[ExpressRoute]: https://azure.microsoft.com/services/expressroute/ 
[requiredports]: app-service-app-service-environment-control-inbound-traffic.md 
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/ 
[UDROverview]: https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/ 
<!-- Old link -- [UDRHowTo]: https://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/ -->

[UDRHowTo]: https://docs.microsoft.com/azure/virtual-network/tutorial-create-route-table-powershell 
[HowToCreateAnAppServiceEnvironment]: app-service-web-how-to-create-an-app-service-environment.md 
[AzureDownloads]: https://azure.microsoft.com/downloads/ 
[DownloadCenterAddressRanges]: https://www.microsoft.com/download/details.aspx?id=41653 
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/ 
[IntroToAppServiceEnvironment]:  app-service-app-service-environment-intro.md 
[NewPortal]:  https://portal.azure.com 


<!-- IMAGES -->
