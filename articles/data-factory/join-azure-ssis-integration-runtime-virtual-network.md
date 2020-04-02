---
title: Azure-SSIS Integration Runtime'ı sanal ağa bağlama
description: Azure sanal ağında bir Azure-SSIS tümleştirme çalışma süresine nasıl katılacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 02/01/2020
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.openlocfilehash: 4819eaf2a65cf542029cf36f262d0cea5be75f2e
ms.sourcegitcommit: b0ff9c9d760a0426fd1226b909ab943e13ade330
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80521956"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Azure-SSIS Integration Runtime'ı sanal ağa bağlama

Azure Veri Fabrikası'nda SQL Server Integration Services'ı (SSIS) kullanırken, aşağıdaki senaryolarda Azure-SSIS tümleştirme çalışma zamanınızı (IR) bir Azure sanal ağına katılmanız gerekir:

- Azure-SSIS IR'nizde çalışan SSIS paketlerinden şirket içi veri depolarına, proxy olarak kendi barındırılan bir IR'yi yapılandırmadan veya yönetmeden bağlanmak istiyorsunuz. 

- SSIS katalog veritabanını (SSISDB) IP güvenlik duvarı kuralları/sanal ağ hizmeti bitiş noktaları veya özel bitiş noktası olan yönetilen bir örnekiçeren bir Azure SQL Veritabanında barındırmak istiyorsunuz. 

- Azure-SSIS IR'nizde çalışan SSIS paketlerinden sanal ağ hizmeti uç noktalarıyla yapılandırılan Azure kaynaklarına bağlanmak istiyorsunuz.

- Azure-SSIS IR'nizde çalışan SSIS paketlerinden IP güvenlik duvarı kurallarıyla yapılandırılan veri depolarına/kaynaklarına bağlanmak istiyorsunuz.

Veri Fabrikası, Azure-SSIS IR'nize klasik dağıtım modeli veya Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulan sanal bir ağa katılmanızı sağlar.

> [!IMPORTANT]
> Klasik sanal ağ amortismana sokuluyor, bu nedenle bunun yerine Azure Kaynak Yöneticisi sanal ağını kullanın.  Klasik sanal ağı zaten kullanıyorsanız, mümkün olan en kısa sürede Azure Kaynak Yöneticisi sanal ağına geçin.

Bir Sanal ağ öğreticisine [katılmak için azure-SQL Server Integration Services (SSIS) tümleştirme çalışma zamanının (IR) yapılandırılması,](tutorial-deploy-ssis-virtual-network.md) Azure portalı üzerinden en az adımları gösterir. Bu makalede öğretici genişletir ve tüm isteğe bağlı görevleri açıklar:

- Sanal ağ (klasik) kullanıyorsanız.
- Azure-SSIS IR için kendi genel IP adreslerinizi getirirseniz.
- Kendi Etki Alanı Adı Sistemi (DNS) sunucunuzu kullanıyorsanız.
- Alt ağda bir ağ güvenlik grubu (NSG) kullanıyorsanız.
- Azure ExpressRoute veya kullanıcı tanımlı bir rota (UDR) kullanıyorsanız.
- Özelleştirilmiş Azure-SSIS IR kullanıyorsanız.
- Azure Powershell sağlama yı kullanıyorsanız.

## <a name="access-to-on-premises-data-stores"></a>Şirket içi veri depolarına erişim

SSIS paketleriniz şirket içi veri depolarına erişirse, Azure-SSIS IR'nize şirket içi ağa bağlı sanal bir ağa katılabilirsiniz. Veya Azure-SSIS IR'niz için kendi kendine barındırılan bir IR'yi proxy olarak yapılandırabilir ve yönetebilirsiniz. Daha fazla bilgi için, [azure-SSIS IR için proxy olarak kendi barındırılan bir IR'yi yapılandırın.](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) 

Azure-SSIS IR'nizi sanal bir ağa katılırken şu önemli noktaları unutmayın: 

- Şirket içi ağınıza hiçbir sanal ağ bağlı değilse, önce Azure-SSIS IR'nizin katılması için bir [Azure Kaynak Yöneticisi sanal ağı](../virtual-network/quick-create-portal.md#create-a-virtual-network) oluşturun. Ardından, bu sanal ağdan şirket içi ağınıza bir site-to-site [VPN ağ geçidi bağlantısı](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) veya [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) bağlantısı yapılandırın. 

- Azure Kaynak Yöneticisi sanal ağınız, Azure-SSIS IR'inizle aynı konumda şirket içi ağınıza zaten bağlıysa, IR'ye bu sanal ağa katılabilirsiniz. 

- Azure-SSIS IR'nizden farklı bir konumda şirket içi ağınıza zaten klasik bir sanal ağ bağlıysa, Azure-SSIS IR'nizin katılması için bir [Azure Kaynak Yöneticisi sanal ağı](../virtual-network/quick-create-portal.md#create-a-virtual-network) oluşturabilirsiniz. Ardından, [klasik-to-Azure Kaynak Yöneticisi sanal ağ](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantısını yapılandırın. 
 
- Azure Kaynak Yöneticisi sanal ağınız, Azure-SSIS IR'nizden farklı bir konumda şirket içi ağınıza zaten bağlıysa, önce Azure-SSIS IR'nizin katılması için bir [Azure Kaynak Yöneticisi sanal ağı](../virtual-network/quick-create-portal.md#create-a-virtual-network) oluşturabilirsiniz. Ardından, Azure Kaynak Yöneticisi'nin Azure Kaynak Yöneticisi sanal ağ bağlantısını yapılandırın. 

## <a name="hosting-the-ssis-catalog-in-sql-database"></a>SSIS kataloğunu SQL Veritabanında Barındırma

SSIS kataloğunuzu sanal ağ hizmeti bitiş noktalarına sahip bir Azure SQL Veritabanında barındırıyorsanız, Azure-SSIS IR'nize aynı sanal ağ ve alt ağda katıldığınızdan emin olun.

SSIS kataloğunuzu özel bitiş noktası olan yönetilen bir örnekte barındırıyorsanız, Azure-SSIS IR'nize aynı sanal ağa, yönetilen örnekten farklı bir alt ağda katıldığınızdan emin olun. Azure-SSIS IR'nize yönetilen örnekten farklı bir sanal ağa katılmak için, sanal ağ da (aynı bölgeyle sınırlı) sanal ağ ağını veya sanal ağdan sanal ağa bağlantı göndermenizi öneririz. Daha fazla bilgi için bkz: [Uygulamanızı Azure SQL Veritabanı yönetilen örneğine bağlayın.](../sql-database/sql-database-managed-instance-connect-app.md)

## <a name="access-to-azure-services"></a>Azure hizmetlerine erişim

SSIS paketleriniz sanal ağ [hizmeti uç noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md) destekleyen Azure kaynaklarına erişiyorsa ve bu kaynaklara Azure-SSIS IR'den erişimi güvence altına almak istiyorsanız, Azure-SSIS IR'nize sanal ağ hizmeti uç noktaları için yapılandırılan sanal ağ alt ağına katılabilir ve ardından aynı alt ağdan erişime izin vermek için ilgili Azure kaynaklarına sanal ağ kuralı ekleyebilirsiniz.

## <a name="access-to-data-sources-protected-by-ip-firewall-rule"></a>IP güvenlik duvarı kuralı ile korunan veri kaynaklarına erişim

SSIS paketleriniz yalnızca belirli statik genel IP adreslerine izin veren veri depolarına/kaynaklarına erişiyorsa ve bu kaynaklara Azure-SSIS IR'den erişimi güvence altına almak istiyorsanız, azure-SSIS IR için kendi [genel IP adreslerinizi](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address) sanal ağa katılırken getirebilir ve ardından bu IP adreslerinden erişime izin vermek için ilgili kaynaklara bir IP güvenlik duvarı kuralı ekleyebilirsiniz.

Her durumda, sanal ağ yalnızca Azure Kaynak Yöneticisi dağıtım modeli aracılığıyla dağıtılabilir.

Aşağıdaki bölümlerdaha fazla ayrıntı sağlar. 

## <a name="virtual-network-configuration"></a>Sanal ağ yapılandırması

Sanal ağınızı bu gereksinimleri karşılayacak şekilde ayarlayın: 

- Azure-SSIS IR'yi barındıran sanal ağ alt ağınızın aboneliği altında kayıtlı bir sağlayıcı olduğundan `Microsoft.Batch` emin olun. Klasik bir sanal ağ kullanıyorsanız, bu sanal ağ için Klasik Sanal Makine Katılımcısı rolüne de katılın. `MicrosoftAzureBatch` 

- Gerekli izinlere sahip olduğundan emin olun. Daha fazla bilgi için [bkz.](#perms)

- Azure-SSIS IR'yi barındırmak için uygun alt ağı seçin. Daha fazla bilgi için [bkz.](#subnet) 

- Azure-SSIS IR için kendi genel IP adreslerinizi getiriyorsanız, [bkz.](#publicIP)

- Sanal ağda kendi Etki Alanı Adı Sistemi (DNS) sunucunuzu kullanıyorsanız, [bkz.](#dns_server) 

- Alt ağda bir ağ güvenlik grubu (NSG) kullanıyorsanız, [bkz.](#nsg) 

- Azure ExpressRoute veya kullanıcı tanımlı bir rota (UDR) kullanıyorsanız, [bkz.](#route) 

- Sanal ağın kaynak grubunun (veya kendi genel IP adreslerinizi getirirseniz genel IP adreslerinin kaynak grubunun) belirli Azure ağ kaynaklarını oluşturup silediğinden emin olun. Daha fazla bilgi için kaynak [grubunu ayarlama'ya](#resource-group)bakın. 

- [Azure-SSIS IR'nizi Azure-SSIS IR için Özel kurulumda](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)açıklandığı şekilde özelleştirirseniz, Azure-SSIS IR düğümleriniz önceden tanımlanmış 172.16.0.0 ile 172.31.255.255 aralığından özel IP adresleri alır. Bu nedenle, sanal veya şirket içi ağlarınızın özel IP adresi aralıklarının bu aralıkla çarpışmadığından emin olun.

Bu diyagram, Azure-SSIS IR'niz için gerekli bağlantıları gösterir:

![Azure-SSIS IR](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="set-up-permissions"></a><a name="perms"></a>İzinleri ayarlama

Azure-SSIS IR'yi oluşturan kullanıcının aşağıdaki izinlere sahip olması gerekir:

- SSIS IR'nizi bir Azure Kaynak Yöneticisi sanal ağına katılıyorsanız, iki seçeneğiniz vardır:

  - Yerleşik Ağ Katılımcısı rolünü kullanın. Bu rol, gerekenden çok daha büyük bir kapsama sahip olan _Microsoft.Network/\* _ izniyle birlikte gelir.

  - Yalnızca gerekli _Microsoft.Network/virtualNetworks/\*/join/action_ iznini içeren özel bir rol oluşturun. Azure Kaynak Yöneticisi sanal ağına katılırken Azure-SSIS IR için kendi genel IP adreslerinizi de getirmek istiyorsanız, lütfen role _Microsoft.Network/publicIPAddresses/*/join/action_ izni ekleyin.

- SSIS IR'nizi klasik bir sanal ağa katılıyorsanız, yerleşik Klasik Sanal Makine Katılımcısı rolünü kullanmanızı öneririz. Aksi takdirde, sanal ağa katılma iznini içeren özel bir rol tanımlamanız gerekir.

### <a name="select-the-subnet"></a><a name="subnet"></a>Alt ağı seçin

Bir alt ağ seçtiğinizde: 

- Azure-SSIS IR dağıtmak için GatewaySubnet'i seçmeyin. Sanal ağ ağ geçitleri için ayrılmıştır. 

- Seçtiğiniz alt ağın Azure-SSIS IR'nin kullanabileceği yeterli kullanılabilir adres alanına sahip olduğundan emin olun. Kullanılabilir IP adreslerini IR düğümü numarasının en az iki katı olarak bırakın. Azure, her alt ağda bazı IP adresleri ayırır. Bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumluluğu için ayrılmıştır ve Azure hizmetleri için üç adres daha kullanılır. Daha fazla bilgi için, bu [alt ağlar içinde IP adresleri nin kullanılmasıyla ilgili herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets) 

- Yalnızca diğer Azure hizmetleri tarafından işgal edilen bir alt ağ kullanmayın (örneğin, SQL Veritabanı yönetilen örneği, Uygulama Hizmeti vb.). 

### <a name="select-the-static-public-ip-addresses"></a><a name="publicIP"></a>Statik genel IP adreslerini seçin

Azure-SSIS IR için sanal bir ağa katılırken kendi statik genel IP adreslerinizi getirmek istiyorsanız, aşağıdaki gereksinimleri karşıladıklarından emin olun:

- Diğer Azure kaynaklarıyla zaten ilişkili olmayan tam olarak iki kullanılmamış kaynak sağlanmalıdır. Azure-SSIS IR'nizi düzenli olarak yükselttiğinde ekstra bir tane kullanılacaktır. Etkin Azure-SSIS IRs'leriniz arasında herkese açık bir IP adresipaylaşılamayacağını unutmayın.

- Her ikisi de standart tip statik olanlar olmalıdır. Daha fazla bilgi için [Genel IP Adresi SKS'lerine](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#sku) bakın.

- Her ikisinin de bir DNS adı olmalı. Bunları oluştururken bir DNS adı vermediyseniz, bunu Azure portalında yapabilirsiniz.

![Azure-SSIS IR](media/ssis-integration-runtime-management-troubleshoot/setup-publicipdns-name.png)

- Onlar ve sanal ağ aynı abonelik altında ve aynı bölgede olmalıdır.

### <a name="set-up-the-dns-server"></a><a name="dns_server"></a>DNS sunucusunu ayarlama 
Özel ana bilgisayar adınızı çözmek için Azure-SSIS IR'nizin katıldığı sanal bir ağda kendi DNS sunucunuzu kullanmanız gerekiyorsa, genel Azure ana `<your storage account>.blob.core.windows.net`bilgisayar adlarını da çözebildiğinizden emin olun (örneğin, bir Azure Depolama blob adlı). 

Önerilen bir yaklaşım aşağıdadır: 

-   İstekleri Azure DNS'ye iletmek için özel DNS'yi yapılandırın. Çözülmemiş DNS kayıtlarını azure özyinelemeli çözümleyicilerinin (168.63.129.16) IP adresine kendi DNS sunucunuzda iletebilirsiniz. 

Daha fazla bilgi için, [kendi DNS sunucunuzu kullanan Ad çözünürlüğüne](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server)bakın. 

> [!NOTE]
> Azure-SSIS IR otomatik olarak kendi DNS sonekinizi eklemeyeceğinden, `<your_private_server>`lütfen özel ana bilgisayar adınız için tam nitelikli bir etki alanı adı (FQDN) kullanın. `<your_private_server>.contoso.com`

### <a name="set-up-an-nsg"></a><a name="nsg"></a>Bir NSG ayarlama
Azure-SSIS IR'niz tarafından kullanılan alt ağ için bir NSG uygulamanız gerekiyorsa, gelen ve giden trafiğin aşağıdaki bağlantı noktalarından geçmesine izin verin: 

-   **Azure-SSIS IR'nin gelen gereksinimi**

| Yön | Ulaşım protokolü | Kaynak | Kaynak bağlantı noktası aralığı | Hedef | Hedef bağlantı noktası aralığı | Yorumlar |
|---|---|---|---|---|---|---|
| Gelen | TCP | BatchNodeManagement | * | VirtualNetwork | 29876, 29877 (Bir Kaynak Yöneticisi sanal ağ ir katılırsanız) <br/><br/>10100, 20100, 30100 (klasik bir sanal ağa IR katılırsanız)| Veri Fabrikası hizmeti, sanal ağdaki Azure-SSIS IR'nizin düğümleriyle iletişim kurmak için bu bağlantı noktalarını kullanır. <br/><br/> Alt net düzeyinde bir NSG oluştursanız da oluşturmasanız da, Veri Fabrikası her zaman Azure-SSIS IR'yi barındıran sanal makinelere bağlı ağ arabirimi kartları (NIC' ler) düzeyinde bir NSG yapılandırır. Belirtilen bağlantı noktalarındaki Veri Fabrikası IP adreslerinden yalnızca gelen trafiğe nic düzeyinden NSG tarafından izin verilir. Bu bağlantı noktalarını alt net düzeyinde internet trafiğine açsanız bile, Veri Fabrikası IP adresi olmayan IP adreslerinden gelen trafik NIC düzeyinde engellenir. |
| Gelen | TCP | Corpnetsaw | * | VirtualNetwork | 3389 | (İsteğe bağlı) Bu kural yalnızca Microsoft destekçisi müşteriden gelişmiş sorun giderme için açılmasını istediğinde gereklidir ve sorun giderme den hemen sonra kapatılabilir. **CorpNetSaw** hizmet etiketi, uzak masaüstünü kullanmak için yalnızca Microsoft kurumsal ağındaki güvenli erişim iş istasyonlarına izin verir. Bu hizmet etiketi portaldan seçilemez ve yalnızca Azure PowerShell veya Azure CLI üzerinden kullanılabilir. <br/><br/> NIC düzeyinde NSG'de, bağlantı noktası 3389 varsayılan olarak açıktır ve alt net düzeyinde NSG'de 3389 numaralı bağlantı noktasını kontrol etmenize izin veririz, bu arada Azure-SSIS IR koruma için her IR düğümünde varsayılan olarak 3389 numaralı bağlantı noktasını devre dışı kılmıştır. |
||||||||

-   **Azure-SSIS IR'nin giden gereksinimi**

| Yön | Ulaşım protokolü | Kaynak | Kaynak bağlantı noktası aralığı | Hedef | Hedef bağlantı noktası aralığı | Yorumlar |
|---|---|---|---|---|---|---|
| Giden | TCP | VirtualNetwork | * | AzureCloud | 443 | Sanal ağdaki Azure-SSIS IR'nizin düğümleri, Azure Depolama ve Azure Etkinlik Hub'ları gibi Azure hizmetlerine erişmek için bu bağlantı noktasını kullanır. |
| Giden | TCP | VirtualNetwork | * | Internet | 80 | (İsteğe bağlı) Sanal ağdaki Azure-SSIS IR'nizin düğümleri, internetten bir sertifika iptal listesi indirmek için bu bağlantı noktasını kullanır. Bu trafiği engellerseniz, IR başlatıldığında performans düşürme ile karşılaşabilirsiniz ve sertifika kullanımı için sertifika iptal listesini denetleme özelliğini kaybedebilirsiniz. Hedefi belirli FQDN'lere daha da daraltmak istiyorsanız, lütfen **Azure ExpressRoute veya UDR kullan** bölümüne bakın|
| Giden | TCP | VirtualNetwork | * | Sql | 1433, 11000-11999 | (İsteğe bağlı) Bu kural, yalnızca sanal ağdaki Azure-SSIS IR'nizin düğümleri SQL Veritabanı sunucunuz tarafından barındırılan bir SSISDB'ye erişiğinde gereklidir. SQL Database sunucu bağlantı ilkeniz **Yönlendirme**yerine **Proxy** olarak ayarlanmışsa, yalnızca bağlantı noktası 1433 gereklidir. <br/><br/> Bu giden güvenlik kuralı, sanal ağda veya özel bitiş noktasıyla yapılandırılan Azure Veritabanı sunucusunda yönetilen örneğiniz tarafından barındırılan bir SSISDB için geçerli değildir. |
| Giden | TCP | VirtualNetwork | * | VirtualNetwork | 1433, 11000-11999 | (İsteğe bağlı) Bu kural, yalnızca sanal ağdaki Azure-SSIS IR'nizin düğümleri, sanal ağda yönetilen örneğiniz tarafından barındırılan bir SSISDB'ye veya özel bitiş noktasıyla yapılandırılan Azure Veritabanı sunucusuna erişiğinde gereklidir. SQL Database sunucu bağlantı ilkeniz **Yönlendirme**yerine **Proxy** olarak ayarlanmışsa, yalnızca bağlantı noktası 1433 gereklidir. |
| Giden | TCP | VirtualNetwork | * | Depolama | 445 | (İsteğe bağlı) Bu kural yalnızca Azure Dosyaları'nda depolanan SSIS paketini yürütmek istediğinizde gereklidir. |
||||||||

### <a name="use-azure-expressroute-or-udr"></a><a name="route"></a>Azure ExpressRoute veya UDR'yi kullanma
Azure-SSIS IR'den giden trafiği incelemek istiyorsanız, Azure-SSIS IR'den başlatılan trafiği [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) kuvvet tünelleme (Bir BGP rotası, 0,0,0,0/0'ın reklamını yapmak) aracılığıyla şirket içi güvenlik duvarı cihazına veya [TCOM'lar](../virtual-network/virtual-networks-udr-overview.md)aracılığıyla Ağ Sanal Cihaz'a (NVA) güvenlik duvarı veya [Azure Güvenlik Duvarı](https://docs.microsoft.com/azure/firewall/) olarak yönlendirebilirsiniz. 

![Azure-SSIS IR için NVA senaryosu](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir-nva.png)

Tüm senaryonun işe yaraması için aşağıdaki şeyleri yapmanız gerekir
   -   Azure Toplu İş yönetimi hizmetleri ile Azure-SSIS IR arasındaki gelen trafik güvenlik duvarı cihazı yla yönlendirilemez.
   -   Güvenlik duvarı cihazı, Azure-SSIS IR tarafından gerekli olan giden trafiğe izin verir.

Azure Toplu İşlem yönetimi hizmetleri ile Azure-SSIS IR arasındaki gelen trafik güvenlik duvarı cihazına yönlendirilemez aksi takdirde, asimetrik yönlendirme sorunu nedeniyle trafik bozulur. Gelen trafik için rotalar tanımlanmalıdır, böylece trafik geldiği gibi yanıtlayabilir. Azure Toplu İş yönetimi hizmetleri ile Azure-SSIS IR arasındaki trafiği **Internet**olarak bir sonraki atlama türüyle yönlendirmek için belirli ÜTR'ler tanımlayabilirsiniz.

Örneğin, Azure-SSIS IR'niz şu `UK South` adreste bulunuyorsa ve giden trafiği Azure Güvenlik Duvarı üzerinden incelemek istiyorsanız, `BatchNodeManagement.UKSouth` öncelikle [hizmet etiketleri IP aralığı indirme bağlantısından](https://www.microsoft.com/download/details.aspx?id=56519) veya Service Tag [Discovery API'den](https://aka.ms/discoveryapi)bir IP aralığı hizmet etiketi listesi alırsınız. Daha sonra sanal **cihaz**olarak bir sonraki hop türü ile birlikte 0.0.0.0/0 rota ile birlikte **Internet** olarak bir sonraki hop türü ile ilgili IP aralığı yollarının aşağıdaki ÜTR uygulayın.

![Azure Toplu UDR ayarları](media/join-azure-ssis-integration-runtime-virtual-network/azurebatch-udr-settings.png)

> [!NOTE]
> Bu yaklaşım ek bir bakım maliyetine neden olabilir. Azure-SSIS IR'yi kırmamak için IP aralığını düzenli olarak kontrol edin ve UDR'nize yeni IP aralıkları ekleyin. Yeni IP hizmet etiketinde göründüğünde IP'nin yürürlüğe girmesi bir ay daha süreceği için IP aralığını aylık olarak denetlemenizi öneririz. 

UDR kurallarının kurulumunun kolaylaşmasını kolaylaştırmak için, Azure Toplu İş yönetimi hizmetleri için UDR kuralları eklemek için aşağıdaki Powershell komut dosyasını çalıştırabilirsiniz:
```powershell
$Location = "[location of your Azure-SSIS IR]"
$RouteTableResourceGroupName = "[name of Azure resource group that contains your Route Table]"
$RouteTableResourceName = "[resource name of your Azure Route Table ]"
$RouteTable = Get-AzRouteTable -ResourceGroupName $RouteTableResourceGroupName -Name $RouteTableResourceName
$ServiceTags = Get-AzNetworkServiceTag -Location $Location
$BatchServiceTagName = "BatchNodeManagement." + $Location
$UdrRulePrefixForBatch = $BatchServiceTagName
if ($ServiceTags -ne $null)
{
    $BatchIPRanges = $ServiceTags.Values | Where-Object { $_.Name -ieq $BatchServiceTagName }
    if ($BatchIPRanges -ne $null)
    {
        Write-Host "Start to add rule for your route table..."
        for ($i = 0; $i -lt $BatchIPRanges.Properties.AddressPrefixes.Count; $i++)
        {
            $UdrRuleName = "$($UdrRulePrefixForBatch)_$($i)"
            Add-AzRouteConfig -Name $UdrRuleName `
                -AddressPrefix $BatchIPRanges.Properties.AddressPrefixes[$i] `
                -NextHopType "Internet" `
                -RouteTable $RouteTable `
                | Out-Null
            Write-Host "Add rule $UdrRuleName to your route table..."
        }
        Set-AzRouteTable -RouteTable $RouteTable
    }
}
else
{
    Write-Host "Failed to fetch service tags, please confirm that your Location is valid."
}
```

Güvenlik duvarı cihazının giden trafiğe izin verebilmesi için, giden bağlantı noktalarının NSG giden kurallarındaki gereksinimle aynı şekilde aşağıya inmesine izin vermeniz gerekir.
-   Azure Bulut hizmetleri olarak hedefe sahip Port 443.

    Azure Güvenlik Duvarı kullanıyorsanız, AzureCloud Hizmet Etiketi ile ağ kuralını belirtebilirsiniz. Diğer tür güvenlik duvarı için, yalnızca 443 nolu bağlantı noktası için hedef olarak izin verebilir veya Azure ortamınızın türüne bağlı olarak FQDN'lerin altında izin verebilirsiniz:
    | Azure Ortamı | Uç Noktalar                                                                                                                                                                                                                                                                                                                                                              |
    |-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Azure Genel      | <ul><li><b>Azure Veri Fabrikası (Yönetim)</b></li><li style="list-style-type:none"><ul><li>\*.frontend.clouddatahub.net</li></ul></li><li><b>Azure Depolama (Yönetim)</b></li><li style="list-style-type:none"><ul><li>\*.blob.core.windows.net</li><li>\*.table.core.windows.net</li></ul></li><li><b>Azure Kapsayıcı Kayıt Defteri (Özel Kurulum)</b></li><li style="list-style-type:none"><ul><li>\*.azurecr.io</li></ul></li><li><b>Olay Merkezi (Günlük)</b></li><li style="list-style-type:none"><ul><li>\*.servicebus.windows.net</li></ul></li><li><b>Microsoft Günlük hizmeti (Dahili Kullanım)</b></li><li style="list-style-type:none"><ul><li>gcs.prod.monitoring.core.windows.net</li><li>prod.warmpath.msftcloudes.com</li><li>azurewatsonanalysis-prod.core.windows.net</li></ul></li></ul> |
    | Azure Kamu  | <ul><li><b>Azure Veri Fabrikası (Yönetim)</b></li><li style="list-style-type:none"><ul><li>\*.frontend.datamovement.azure.us</li></ul></li><li><b>Azure Depolama (Yönetim)</b></li><li style="list-style-type:none"><ul><li>\*.blob.core.usgovcloudapi.net</li><li>\*.table.core.usgovcloudapi.net</li></ul></li><li><b>Azure Kapsayıcı Kayıt Defteri (Özel Kurulum)</b></li><li style="list-style-type:none"><ul><li>\*.azurecr.us</li></ul></li><li><b>Olay Merkezi (Günlük)</b></li><li style="list-style-type:none"><ul><li>\*.servicebus.usgovcloudapi.net</li></ul></li><li><b>Microsoft Günlük hizmeti (Dahili Kullanım)</b></li><li style="list-style-type:none"><ul><li>fairfax.warmpath.usgovcloudapi.net</li><li>azurewatsonanalysis.usgovcloudapp.net</li></ul></li></ul> |
    | Azure Çin 21Vianet     | <ul><li><b>Azure Veri Fabrikası (Yönetim)</b></li><li style="list-style-type:none"><ul><li>\*.frontend.datamovement.azure.cn</li></ul></li><li><b>Azure Depolama (Yönetim)</b></li><li style="list-style-type:none"><ul><li>\*.blob.core.chinacloudapi.cn</li><li>\*.table.core.chinacloudapi.cn</li></ul></li><li><b>Azure Kapsayıcı Kayıt Defteri (Özel Kurulum)</b></li><li style="list-style-type:none"><ul><li>\*.azurecr.cn</li></ul></li><li><b>Olay Merkezi (Günlük)</b></li><li style="list-style-type:none"><ul><li>\*.servicebus.chinacloudapi.cn</li></ul></li><li><b>Microsoft Günlük hizmeti (Dahili Kullanım)</b></li><li style="list-style-type:none"><ul><li>mooncake.warmpath.chinacloudapi.cn</li><li>azurewatsonanalysis.chinacloudapp.cn</li></ul></li></ul>

    Azure Depolama, Azure Kapsayıcı Kayıt Defteri ve Etkinlik Hub'ının FQDN'lerine gelince, sanal ağınız için aşağıdaki hizmet bitiş noktalarını etkinleştirmeyi de seçebilirsiniz, böylece bu uç noktalara giden ağ trafiği güvenlik duvarı aygıtınıza yönlendirilmek yerine Azure omurga ağından geçer:
    -  Microsoft.Storage
    -  Microsoft.ContainerRegistry
    -  Microsoft.EventHub


-   CRL indirme siteleri olarak hedef ile Port 80.

    Azure-SSIS IR yönetimi amacıyla CRL (Sertifika İptal Listesi) indirme siteleri olarak kullanılan FQDN'lere aşağıda izin vereceksiniz:
    -  crl.microsoft.com:80
    -  mscrl.microsoft.com:80
    -  crl3.digicert.com:80
    -  crl4.digicert.com:80
    -  ocsp.digicert.com:80
    -  cacerts.digicert.com:80
    
    Farklı CRL'ye sahip sertifikalar kullanıyorsanız, bunları da eklemeniz önerilir. [Sertifika İptal Listesi'nde](https://social.technet.microsoft.com/wiki/contents/articles/2303.understanding-access-to-microsoft-certificate-revocation-list.aspx)daha fazla bilgi için bu okuyun.

    Bu trafiğe izin vermezseniz, Azure-SSIS IR'yi başlatırken performans düşüşü yaşayabilir ve güvenlik açısından önerilmeyen sertifika kullanımı için sertifika iptal listesini denetleme olanağınızı kaybedebilirsiniz.

-   Azure SQL olarak hedef olan 1433, 11000-11999 numaralı bağlantı noktası (yalnızca sanal ağdaki Azure-SSIS IR'nizin düğümleri SQL Veritabanı sunucunuz tarafından barındırılan bir SSISDB'ye erişiğinde gereklidir).

    Azure Güvenlik Duvarı kullanıyorsanız, Azure SQL Hizmet Etiketi ile ağ kuralını belirtebilirsiniz, aksi takdirde güvenlik duvarı cihazında hedefe belirli azure sql url'si olarak izin verebilirsiniz.

-   Hedef olarak Azure Depolama olarak bağlantı noktası olan 445 bağlantı noktası (yalnızca Azure Dosyaları'nda depolanan SSIS paketini çalıştırdığınızda gereklidir).

    Azure Güvenlik Duvarı kullanıyorsanız, Depolama Hizmeti Etiketi yle ağ kuralını belirtebilirsiniz, aksi takdirde güvenlik duvarı aygıtında hedefe belirli azure dosya depolama url'si olarak izin verebilirsiniz.

> [!NOTE]
> Azure SQL ve Depolama için, alt netinizde Sanal Ağ hizmet uç noktalarını yapılandırırsanız, aynı bölgede azure-SSIS IR ve Azure SQL arasındaki trafik \ Aynı bölgedeki veya eşleştirilmiş bölgedeki Azure Depolama alanı arasındaki trafik, güvenlik duvarı aygıtınız yerine doğrudan Microsoft Azure omurga ağına yönlendirilir.

Azure-SSIS IR'nin giden trafiğini denetleme yeteneğine ihtiyacınız yoksa, tüm trafiği bir sonraki hop türü **Internet'e**zorlamak için rota uygulayabilirsiniz:

-   Azure ExpressRoute senaryosunda, Azure-SSIS IR'yi barındıran alt ağda **Bir** sonraki atlama türüyle 0.0.0.0/0 rota uygulayabilirsiniz. 
-   Bir NVA senaryosunda, Azure-SSIS IR'yi **sanal cihaz** olarak bir sonraki atlama türünden **Internet'e**barındıran alt ağüzerinde uygulanan varolan 0.0.0.0/0 rotasını değiştirebilirsiniz.

![Rota ekleme](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

> [!NOTE]
> Sonraki atlama türü **Internet** ile rota belirtin tüm trafiğin Internet üzerinden gideceği anlamına gelmez. Azure, hedef adres Azure'un hizmetlerinden biri olduğu sürece, trafiği Internet'e yönlendirmek yerine trafiği doğrudan Azure'un omurga ağı üzerinden hizmete yönlendirir.

### <a name="set-up-the-resource-group"></a><a name="resource-group"></a>Kaynak grubunu ayarlama

Azure-SSIS IR'nin sanal ağla aynı kaynak grubu altında belirli ağ kaynakları oluşturması gerekir. Bu kaynaklar şunlardır:
- * \<Guid>-azurebatch-cloudserviceloadbalancer*adını taşıyan bir Azure yük dengeleyicisi.
- * \<Guid>-azurebatch-cloudservicepublicip*adını taşıyan bir Azure genel IP adresi.
- * \<Guid>-azurebatch-cloudservicenetworksecuritygroup*adını taşıyan bir ağ iş güvenliği grubu. 

> [!NOTE]
> Artık Azure-SSIS IR için kendi statik genel IP adreslerinizi getirebilirsiniz. Bu senaryoda, sanal ağ yerine statik genel IP adreslerinizle aynı kaynak grubu altında yalnızca Azure yük bakiyesi ve ağ güvenlik grubu oluştururuz.

Bu kaynaklar Azure-SSIS IR'niz başladığında oluşturulur. Azure-SSIS IR'niz durduğunda silinirler. Azure-SSIS IR için kendi statik genel IP adreslerinizi getirirseniz, Azure-SSIS IR'niz durduğunda kendi statik genel IP adresleriniz silinmez. Azure-SSIS IR'nizin durmasını engellemek için bu ağ kaynaklarını diğer kaynaklarınızda yeniden kullanmayın.

Sanal ağın/statik genel IP adreslerinin ait olduğu kaynak grubunda/aboneliğinde kaynak kilidi olmadığından emin olun. Salt okunur/sil kilidi yapılandırırsanız, Azure-SSIS IR'nizi başlatma ve durdurma başarısız olur veya yanıt vermeyi durdurur.

Sanal ağın/statik genel IP adreslerinin ait olduğu kaynak grubu/aboneliği altında aşağıdaki kaynakların oluşturulmasını engelleyen bir Azure ilkeniz olmadığından emin olun: 
- Microsoft.Network/LoadBalancers 
- Microsoft.Network/AğGüvenliği Grupları 
- Microsoft.Network/PublicIPAdresleri 

Aboneliğinizin kaynak kotasının yukarıdaki üç ağ kaynağı için yeterli olduğundan emin olun. Özellikle, sanal ağda oluşturulan her Azure-SSIS IR için, yukarıdaki üç ağ kaynağının her biri için iki boş kota ayırmanız gerekir. Azure-SSIS IR'nizi düzenli olarak yükselttiğinde fazladan bir kota kullanılacaktır.

### <a name="faq"></a><a name="faq"></a>SSS

- Gelen bağlantı için Azure-SSIS IR'mde açıklanan genel IP adresini nasıl koruyabilirim? Genel IP adresini kaldırmak mümkün mü?
 
  Şu anda, Azure-SSIS IR'niz sanal bir ağa katıldığında herkese açık bir IP adresi otomatik olarak oluşturulur. Yalnızca Azure Toplu İş yönetim hizmetlerinin Azure-SSIS IR'nize gelen bağlantıya bağlanmasına izin veren NIC düzeyinde bir NSG'miz vardır. Gelen koruma için alt net düzeyinde bir NSG de belirtebilirsiniz.

  Herkese açık bir IP adresinin açığa çıkarOlmasını istemiyorsanız, senaryonuz için geçerliyse Azure-SSIS IR'nizi sanal bir ağa katılmak yerine [Azure-SSIS IR'niz için proxy olarak barındırılan bir IR'yi yapılandırmayı](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) düşünün.
 
- Azure-SSIS IR'min genel IP adresini güvenlik duvarının veri kaynaklarım için izin listesine ekleyebilir miyim?

  Artık Azure-SSIS IR için kendi statik genel IP adreslerinizi getirebilirsiniz. Bu durumda, IP adreslerinizi veri kaynaklarınız için güvenlik duvarının izin listesine ekleyebilirsiniz. Senaryonuza bağlı olarak Azure-SSIS IR'nizden veri erişimi sağlamak için aşağıdaki diğer seçenekleri de göz önünde bulundurabilirsiniz:

  - Veri kaynağınız şirket içindeyse, bir sanal ağı şirket içi ağınıza bağladıktan ve Azure-SSIS IR'nizi sanal ağ alt ağına katıldıktan sonra, bu alt ağın özel IP adresi aralığını güvenlik duvarının veri kaynağınız için izin veren listesine ekleyebilirsiniz.
  - Veri kaynağınız sanal ağ hizmeti uç noktalarını destekleyen bir Azure hizmetiyse, sanal ağ alt netinizde bir sanal ağ hizmeti bitiş noktası yapılandırabilir ve Azure-SSIS IR'nize bu alt ağa katılabilirsiniz. Daha sonra, veri kaynağınız için güvenlik duvarına bu alt ağla birlikte bir sanal ağ kuralı ekleyebilirsiniz.
  - Veri kaynağınız Azure olmayan bir bulut hizmetiyse, giden trafiği Azure-SSIS IR'nizden statik bir genel IP adresi aracılığıyla NVA/Azure Güvenlik Duvarı'na yönlendirmek için udr kullanabilirsiniz. Ardından, NVA/Azure Güvenlik Duvarınızın statik genel IP adresini güvenlik duvarının veri kaynağınız için izin veren listesine ekleyebilirsiniz.
  - Yukarıdaki seçeneklerden hiçbiri gereksinimlerinizi karşılamazsa, [Azure-SSIS IR'niz için proxy olarak kendi kendine barındırılan bir IR yapılandırmayı](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis)düşünün. Daha sonra, kendi barındırdığı IR'nizi barındıran makinenin statik genel IP adresini güvenlik duvarının veri kaynağınız için izin veren listesine ekleyebilirsiniz.

- Azure-SSIS IR için kendi adresimi getirmek istiyorsam neden iki statik ortak adres sağlamam gerekiyor?

  Azure-SSIS IR düzenli olarak otomatik olarak güncelleştirilir. Yükseltme sırasında yeni düğümler oluşturulur ve eskileri silinir. Ancak, kapalı kalma süresini önlemek için, yenidüğümler hazır olana kadar eski düğümler silinmez. Bu nedenle, eski düğümler tarafından kullanılan ilk statik genel IP adresiniz hemen serbest bırakılamaz ve yeni düğümleri oluşturmak için ikinci statik genel IP adresinize ihtiyacımız vardır.

- Azure-SSIS IR için kendi statik genel IP adreslerimi getirdim, ancak neden hala veri kaynaklarıma erişemiyor?

  - İki statik genel IP adresinin de güvenlik duvarının veri kaynaklarınız için izin veren listesine eklenmiştir. Azure-SSIS IR'niz her yükseltilse, statik genel IP adresi sizin getirdiğiniz ikisi arasında geçiş yapılır. İzin listesine bunlardan yalnızca birini eklerseniz, Azure-SSIS IR'nizin veri erişimi yükseltildikten sonra bozulur.
  - Veri kaynağınız bir Azure hizmetiyse, lütfen sanal ağ hizmeti bitiş noktalarıyla yapılandırıp yapılandırmadığınızı kontrol edin. Bu durumda, Azure-SSIS IR'den veri kaynağınıza gelen trafik, Azure hizmetleri tarafından yönetilen özel IP adreslerini kullanmak için geçiş yapacak ve güvenlik duvarının veri kaynağınız için izin veren listesine kendi statik genel IP adreslerinizi eklemek etkili olmayacaktır.

## <a name="azure-portal-data-factory-ui"></a>Azure portalı (Veri Fabrikası UI)

Bu bölümde, Azure portalı ve Veri Fabrikası UI'sini kullanarak sanal ağa (klasik veya Azure Kaynak Yöneticisi) varolan bir Azure-SSIS IR'ye nasıl katılacağınız gösterilmektedir. 

Azure-SSIS IR'nizi sanal ağa katılmadan önce sanal ağı düzgün bir şekilde yapılandırmanız gerekir. Sanal ağ türünüz (klasik veya Azure Kaynak Yöneticisi) için geçerli olan bölümdeki adımları izleyin. Ardından, Azure-SSIS IR'nizi sanal ağa katılmak için üçüncü bölümdeki adımları izleyin. 

### <a name="configure-an-azure-resource-manager-virtual-network"></a>Azure Kaynak Yöneticisi sanal ağ yapılandırma

Bir Azure-SSIS IR'sine katılmaya çalışmadan önce bir Azure Kaynak Yöneticisi sanal ağı yapılandırmak için portalı kullanın.

1. Microsoft Edge veya Google Chrome'u başlatın. Şu anda, yalnızca bu web tarayıcıları Veri Fabrikası UI'yi destekler. 

1. [Azure portalında](https://portal.azure.com)oturum açın. 

1. **Diğer hizmetleri**seçin. **Sanal ağlar**için filtre uygulayın ve seçin. 

1. Listedeki sanal ağınıza filtre uygulayın ve seçin. 

1. Sanal **ağ** sayfasında **Özellikler'i**seçin. 

1. Sanal ağ için kaynak kimliğini panoya kopyalamak için **KAYNAK Kimliği** için kopyalama düğmesini seçin. Kimliği OneNote'taki panodan veya bir dosyadan kaydedin. 

1. Sol menüde **Alt Ağlar'ı**seçin. Kullanılabilir adres sayısının Azure-SSIS IR'nizdeki düğümlerden daha büyük olduğundan emin olun. 

1. Azure Toplu İş sağlayıcısının sanal ağa sahip Azure aboneliğinde kayıtlı olduğunu doğrulayın. Veya Azure Toplu İş sağlayıcısını kaydedin. Aboneliğinizde zaten bir Azure Toplu İş hesabınız varsa, aboneliğiniz Azure Toplu İş için kaydedilir. (Veri Fabrikası portalında Azure-SSIS IR oluşturursanız, Azure Toplu İşlem sağlayıcısı sizin için otomatik olarak kaydedilir.) 

   1. Azure portalında, sol menüde **Abonelikler'i**seçin. 

   1. Aboneliğinizi seçin. 

   1. Solda, Kaynak **sağlayıcılarını**seçin ve **Microsoft.Batch'In** kayıtlı bir sağlayıcı olduğunu onaylayın. 

   !["Kayıtlı" durumunun teyidi](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   **Microsoft.Batch'i** listede göremiyorsanız, kaydolmak için aboneliğinizde [boş bir Azure Toplu İş hesabı oluşturun.](../batch/batch-account-create-portal.md) Daha sonra silebilirsiniz. 

### <a name="configure-a-classic-virtual-network"></a>Klasik bir sanal ağı yapılandırma

Bir Azure-SSIS IR'ye katılmaya çalışmadan önce portalı klasik bir sanal ağı yapılandırmak için kullanın. 

1. Microsoft Edge veya Google Chrome'u başlatın. Şu anda, yalnızca bu web tarayıcıları Veri Fabrikası UI'yi destekler. 

1. [Azure portalında](https://portal.azure.com)oturum açın. 

1. **Diğer hizmetleri**seçin. Sanal ağlar için filtre uygulayın ve seçin **(klasik)**. 

1. Listedeki sanal ağınıza filtre uygulayın ve seçin. 

1. Sanal **ağ (klasik)** sayfasında **Özellikler'i**seçin. 

   ![Klasik sanal ağ kaynağı kimliği](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Klasik ağın kaynak kimliğini panoya kopyalamak için **KAYNAK Kimliği** için kopyalama düğmesini seçin. Kimliği OneNote'taki panodan veya bir dosyadan kaydedin. 

1. Sol menüde **Alt Ağlar'ı**seçin. Kullanılabilir adres sayısının Azure-SSIS IR'nizdeki düğümlerden daha büyük olduğundan emin olun. 

   ![Sanal ağdaki kullanılabilir adres sayısı](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. **MicrosoftAzureBatch'e** sanal ağ için **Klasik Sanal Makine Katılımcısı** rolüne katılın. 

   1. Sol menüde **Access denetimi (IAM)** seçeneğini belirleyin ve **Rol atamaları** sekmesini seçin. 

       !["Erişim denetimi" ve "Ekle" düğmeleri](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

   1. **Rol ataması ekle**’yi seçin.

   1. **Rol** **Için Rol Atama Ekle** sayfasında Klasik Sanal **Makine Katılımcısı'nı**seçin. **Select** kutusunda **ddbf3205-c6bd-46ae-8127-60eb93363864'ü**yapıştırın ve ardından arama sonuçları listesinden **Microsoft Azure Toplu İş'i** seçin. 

       !["Rol ataması ekle" sayfasında arama sonuçları](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

   1. Ayarları kaydetmek ve sayfayı kapatmak için **Kaydet'i** seçin. 

       ![Erişim ayarlarını kaydetme](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

   1. Microsoft Azure **Toplu İş'i** katkıda bulunanlar listesinde gördüğünüzü doğrulayın. 

       ![Azure Toplu İş erişimini onaylama](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Azure Toplu İş sağlayıcısının sanal ağa sahip Azure aboneliğinde kayıtlı olduğunu doğrulayın. Veya Azure Toplu İş sağlayıcısını kaydedin. Aboneliğinizde zaten bir Azure Toplu İş hesabınız varsa, aboneliğiniz Azure Toplu İş için kaydedilir. (Veri Fabrikası portalında Azure-SSIS IR oluşturursanız, Azure Toplu İşlem sağlayıcısı sizin için otomatik olarak kaydedilir.) 

   1. Azure portalında, sol menüde **Abonelikler'i**seçin. 

   1. Aboneliğinizi seçin. 

   1. Solda, Kaynak **sağlayıcılarını**seçin ve **Microsoft.Batch'In** kayıtlı bir sağlayıcı olduğunu onaylayın. 

   !["Kayıtlı" durumunun teyidi](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   **Microsoft.Batch'i** listede göremiyorsanız, kaydolmak için aboneliğinizde [boş bir Azure Toplu İş hesabı oluşturun.](../batch/batch-account-create-portal.md) Daha sonra silebilirsiniz. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Azure-SSIS IR'ye sanal ağda katılma

Azure Kaynak Yöneticisi sanal ağınızı veya klasik sanal ağınızı yapılandırdıktan sonra, Azure-SSIS IR'ye sanal ağa katılabilirsiniz:

1. Microsoft Edge veya Google Chrome'u başlatın. Şu anda, yalnızca bu web tarayıcıları Veri Fabrikası UI'yi destekler. 

1. Sol menüdeki [Azure portalında](https://portal.azure.com) **Veri fabrikalarını**seçin. Menüde **Veri fabrikalarını** görmüyorsanız, Daha **fazla hizmet**seçin ve ardından INTELLIGENCE **+ ANALYTICS** bölümünde **Veri fabrikalarını**seçin. 

   ![Veri fabrikaları listesi](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Listede Azure-SSIS IR ile veri fabrikanızı seçin. Veri fabrikanızın ana sayfasını görürsünüz. Yazar **& Monitör** döşemesini seçin. Ayrı bir sekmede Veri Fabrikası UI'sini görürsünüz. 

   ![Data factory giriş sayfası](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Veri Fabrikası UI'sinde, **Edit** sekmesine geçin, **Bağlantılar'ı**seçin ve **Tümleştirme Runtimes** sekmesine geçin. 

   !["Tümleştirme çalışma saatleri" sekmesi](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Azure-SSIS IR'niz çalışıyorsa, **Tümleştirme Çalışma Saatleri** listesinde, **Eylemler** sütununda, Azure-SSIS IR'niz için **Durdur** düğmesini seçin. Durdurana kadar Azure-SSIS IR'nizi kaldıramazsınız. 

   ![IR'yi durdurun](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. **Tümleştirme Çalışma Saatleri** listesinde, **Eylemler** sütununda Azure-SSIS IR'niz için **Edit** düğmesini seçin. 

   ![Tümleştirme çalışma süresini yönetme](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. Tümleştirme çalışma zamanı kurulum panelinde, **Sonraki** düğmesini seçerek **Genel Ayarlar** ve **SQL Ayarları** bölümlerinde ilerleyin. 

1. Gelişmiş **Ayarlar** bölümünde: 

   1. Katılmak **için Azure-SSIS Tümleştirme Çalışma süreniz için Bir VNet Seçin' i seçin, ADF' nin belirli ağ kaynakları oluşturmasına izin verin ve isteğe bağlı olarak kendi statik genel IP adreslerinizi** onay kutlayın. 

   1. **Abonelik**için, sanal ağınıza sahip Azure aboneliğini seçin.

   1. **Konum**için, tümleştirme çalışma sürenizin aynı konumu seçilir.

   1. **Türü**için sanal ağınızın türünü seçin: klasik veya Azure Kaynak Yöneticisi. Klasik sanal ağlar yakında küçümseneceği için bir Azure Kaynak Yöneticisi sanal ağı seçmenizi öneririz.

   1. **VNet Adı**için sanal ağınızın adını seçin. Sanal ağ hizmeti uç noktaları veya yönetilen örneği ile Azure SQL Veritabanı sunucunuz için kullanılan ın aynısı olmalıdır. Ya da şirket içi ağınıza bağlı olanın aynısı olmalıdır. Aksi takdirde, Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek herhangi bir sanal ağ olabilir.

   1. **Subnet Adı**için, sanal ağınız için alt ağ adını seçin. SSISDB'yi barındırmak için sanal ağ hizmeti uç noktalarına sahip Azure SQL Veritabanı sunucunuz için kullanılanın aynısı olmalıdır. Ya da SSISDB'yi barındırmak için özel bitiş noktası olan yönetilen örneğiniz için kullanılandan farklı bir alt ağ olmalıdır. Aksi takdirde, Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek herhangi bir alt ağ olabilir.

   1. Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek isteyip istemediğinizi seçmek için **Azure-SSIS Tümleştirme Çalışma zamanı onay kutunuz için statik genel IP adreslerini getir'i** seçin, böylece veri kaynaklarınız için güvenlik duvarında bunlara izin verebilirsiniz.

      Onay kutusunu seçerseniz, aşağıdaki adımları tamamlayın.

      1. **İlk statik genel IP adresi**için, Azure-SSIS IR'nizin gereksinimlerini [karşılayan](#publicIP) ilk statik genel IP adresini seçin. Yoksa, Azure portalında statik genel IP adresleri oluşturmak için yeni bağlantı **oluştur'u** tıklatın ve ardından bunları seçebilmeniz için burada yenile düğmesini tıklatın.
      
      1. **İkinci statik genel IP adresi**için, Azure-SSIS IR'nizin gereksinimlerini [karşılayan](#publicIP) ikinci statik genel IP adresini seçin. Yoksa, Azure portalında statik genel IP adresleri oluşturmak için yeni bağlantı **oluştur'u** tıklatın ve ardından bunları seçebilmeniz için burada yenile düğmesini tıklatın.

   1. **VNet Doğrulama'yı**seçin. Doğrulama başarılı olursa, **Devam et'i**seçin. 

   ![Bir sanal ağda gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

1. **Özet** bölümünde, Azure-SSIS IR'nizin tüm ayarlarını gözden geçirin. Sonra **Güncelleştir'i**seçin.

1. Azure-SSIS IR'niz için **Eylemler** sütunundaki **Başlat** düğmesini seçerek Azure-SSIS IR'nizi başlatın. Sanal bir ağa katılan Azure-SSIS IR'yi başlatmak yaklaşık 20 ila 30 dakika sürer. 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="define-the-variables"></a>Değişkenleri tanımlama

```powershell
$ResourceGroupName = "[your Azure resource group name]"
$DataFactoryName = "[your data factory name]"
$AzureSSISName = "[your Azure-SSIS IR name]"
# Virtual network info: Classic or Azure Resource Manager
$VnetId = "[your virtual network resource ID or leave it empty]" # REQUIRED if you use an Azure SQL Database server with IP firewall rules/virtual network service endpoints or a managed instance with private endpoint to host SSISDB, or if you require access to on-premises data without configuring a self-hosted IR. We recommend an Azure Resource Manager virtual network, because classic virtual networks will be deprecated soon.
$SubnetName = "[your subnet name or leave it empty]" # WARNING: Use the same subnet as the one used for your Azure SQL Database server with virtual network service endpoints, or a different subnet from the one used for your managed instance with a private endpoint
# Public IP address info: OPTIONAL to provide two standard static public IP addresses with DNS name under the same subscription and in the same region as your virtual network
$FirstPublicIP = "[your first public IP address resource ID or leave it empty]"
$SecondPublicIP = "[your second public IP address resource ID or leave it empty]"
```

### <a name="configure-a-virtual-network"></a>Sanal ağ yapılandırma

Azure-SSIS IR'nizi sanal ağa katmadan önce sanal ağı yapılandırmanız gerekir. Azure-SSIS IR'nizin sanal ağ izinlerini ve ayarlarını sanal ağa katılmak üzere otomatik olarak yapılandırmak için aşağıdaki komut dosyasını ekleyin:

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
    Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign the VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Bir Azure-SSIS IR oluşturun ve sanal ağa katılın

Bir Azure-SSIS IR oluşturabilir ve aynı anda sanal bir ağa katılabilirsiniz. Tam komut dosyası ve yönergeler için [bkz.](create-azure-ssis-integration-runtime.md#use-azure-powershell-to-create-an-integration-runtime)

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Sanal ağa mevcut bir Azure-SSIS IR'ye katılma

[Azure-SSIS IR oluştur](create-azure-ssis-integration-runtime.md) makalesi, bir Azure-SSIS IR'si oluşturup aynı komut dosyasındaki sanal ağa nasıl katılacağınızı gösterir. Zaten bir Azure-SSIS IR'niz varsa, sanal ağa katılmak için aşağıdaki adımları izleyin: 
1. Azure-SSIS IR'yi durdurun. 
1. Azure-SSIS IR'yi sanal ağa katılmak için yapılandırın. 
1. Azure-SSIS IR'yi başlatın. 

### <a name="stop-the-azure-ssis-ir"></a>Azure-SSIS IR'yi durdurun

Sanal ağa katılmadan önce Azure-SSIS IR'yi durdurmanız gerekir. Bu komut tüm düğümlerini serbest bırakır ve faturalandırmayı durdurur:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Azure-SSIS IR'nin katılması için sanal ağ ayarlarını yapılandırın

Azure-SSIS'in katılacağı sanal ağın ayarlarını yapılandırmak için aşağıdaki komut dosyasını kullanın: 

```powershell
# Make sure to run this script against the subscription to which the virtual network belongs.
if(![string]::IsNullOrEmpty($VnetId) -and ![string]::IsNullOrEmpty($SubnetName))
{
    # Register to the Azure Batch resource provider
    $BatchApplicationId = "ddbf3205-c6bd-46ae-8127-60eb93363864"
    $BatchObjectId = (Get-AzADServicePrincipal -ServicePrincipalName $BatchApplicationId).Id
    Register-AzResourceProvider -ProviderNamespace Microsoft.Batch
    while(!(Get-AzResourceProvider -ProviderNamespace "Microsoft.Batch").RegistrationState.Contains("Registered"))
    {
        Start-Sleep -s 10
    }
    if($VnetId -match "/providers/Microsoft.ClassicNetwork/")
    {
        # Assign VM contributor role to Microsoft.Batch
        New-AzRoleAssignment -ObjectId $BatchObjectId -RoleDefinitionName "Classic Virtual Machine Contributor" -Scope $VnetId
    }
}
```

### <a name="configure-the-azure-ssis-ir"></a>Azure-SSIS IR'yi yapılandırma

Azure-SSIS IR'nize sanal bir ağa `Set-AzDataFactoryV2IntegrationRuntime` katılmak için aşağıdaki komutu çalıştırın: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -VnetId $VnetId `
    -Subnet $SubnetName

# Add public IP address parameters if you bring your own static public IP addresses
if(![string]::IsNullOrEmpty($FirstPublicIP) -and ![string]::IsNullOrEmpty($SecondPublicIP))
{
    $publicIPs = @($FirstPublicIP, $SecondPublicIP)
    Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
        -DataFactoryName $DataFactoryName `
        -Name $AzureSSISName `
        -PublicIPs $publicIPs
}
```

### <a name="start-the-azure-ssis-ir"></a>Azure-SSIS IR'yi başlatın

Azure-SSIS IR'yi başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
    -DataFactoryName $DataFactoryName `
    -Name $AzureSSISName `
    -Force
```

Bu komutun tamamlanması 20 ila 30 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS IR hakkında daha fazla bilgi için aşağıdaki makalelere bakın: 
- [Azure-SSIS IR](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR de dahil olmak üzere IR'ler hakkında genel kavramsal bilgiler verilmektedir. 
- [Öğretici: SSIS paketlerini Azure'a dağıtın.](tutorial-create-azure-ssis-runtime-portal.md) Bu öğretici, Azure-SSIS IR'nizi oluşturmak için adım adım yönergeler sağlar. SSIS kataloğunu barındırmak için Azure SQL Veritabanı'nı kullanır. 
- [Bir Azure-SSIS IR oluşturun.](create-azure-ssis-integration-runtime.md) Bu makale, öğretici genişletir. SSIS kataloğunu barındırmak için Sanal ağ hizmet uç noktaları yla veya yönetilen bir örnekle Azure SQL Veritabanı'nı kullanma hakkında yönergeler sağlar. Azure-SSIS IR'nize sanal bir ağa nasıl katılacağınızı gösterir. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR'niz hakkında nasıl bilgi alacağınızı gösterilmektedir. Döndürülen bilgiler için durum açıklamaları sağlar. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makalede, Azure-SSIS IR'nizi nasıl durdurabileceğiniz, başlatabileceğiniz veya silebilirsiniz. Ayrıca, düğüm ekleyerek Azure-SSIS IR'nizi nasıl ölçeklendirdiğinizi de gösterir.