---
title: Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katma
description: Azure-SSIS tümleştirme çalışma zamanını bir Azure sanal ağına nasıl katılacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 12/23/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: mflasko
ms.openlocfilehash: a4b0debc712504e8cb3c6d61372bd3a82c7932bb
ms.sourcegitcommit: f0dfcdd6e9de64d5513adf3dd4fe62b26db15e8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/26/2019
ms.locfileid: "75497027"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katma

Azure Data Factory SQL Server Integration Services (SSIS) kullanırken, Azure-SSIS tümleştirme çalışma zamanı 'nı (IR) aşağıdaki senaryolarda bir Azure sanal ağına katılmanız gerekir:

- Proxy olarak şirket içinde barındırılan bir IR yapılandırmadan veya yönetmeden Azure-SSIS IR çalışan SSIS paketlerindeki şirket içi veri depolarına bağlanmak istiyorsunuz. 

- SSIS katalog veritabanını (SSSıSDB) IP güvenlik duvarı kuralları/sanal ağ hizmeti uç noktaları veya özel uç nokta olan yönetilen bir örnek ile bir Azure SQL veritabanında barındırmak istiyorsunuz. 

- Azure-SSIS IR çalışan SSIS paketlerindeki sanal ağ hizmet uç noktaları ile yapılandırılmış Azure kaynaklarına bağlanmak istiyorsunuz.

- Azure-SSIS IR çalışan SSIS paketlerinden IP güvenlik duvarı kurallarıyla yapılandırılmış veri depolarına/kaynaklara bağlanmak istiyorsunuz.

Data Factory, Azure-SSIS IR klasik dağıtım modeli veya Azure Resource Manager dağıtım modeli aracılığıyla oluşturulan bir sanal ağa eklemenize olanak tanır. 

> [!IMPORTANT]
> Klasik sanal ağ kullanım dışı olduğundan bunun yerine Azure Resource Manager sanal ağı kullanın.  Klasik sanal ağı zaten kullanıyorsanız, Azure Resource Manager sanal ağa mümkün olan en kısa sürede geçiş yapın.

## <a name="access-to-on-premises-data-stores"></a>Şirket içi veri depolarına erişim

SSIS 'niz kuruluşunuzda şirket içi veri depolarına erişebiliyse, Azure-SSIS IR şirket içi ağa bağlı bir sanal ağa ekleyebilirsiniz. İsterseniz, Azure-SSIS IR için otomatik olarak barındırılan bir IR yapılandırabilir ve yönetebilirsiniz. Daha fazla bilgi için bkz. [Azure-SSIS IR için otomatik olarak barındırılan BIR IR ara sunucu olarak yapılandırma](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis). 

Azure-SSIS IR bir sanal ağa katılırken şu önemli noktaları hatırlayın: 

- Şirket içi ağınıza bağlı bir sanal ağ yoksa, önce Azure-SSIS IR katılmaları için bir [Azure Resource Manager sanal ağı](../virtual-network/quick-create-portal.md#create-a-virtual-network) oluşturun. Ardından, bu sanal ağdan şirket içi ağınıza bir siteden siteye [VPN Ağ Geçidi bağlantısı](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) veya [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) bağlantısı yapılandırın. 

- Azure Resource Manager bir sanal ağ, Azure-SSIS IR aynı konumda bulunan şirket içi ağınıza zaten bağlıysa, IR 'ye bu sanal ağa katabilirsiniz. 

- Klasik bir sanal ağ, Azure-SSIS IR farklı bir konumda şirket içi ağınıza zaten bağlıysa, Azure-SSIS IR katılması için [Azure Resource Manager bir sanal ağ](../virtual-network/quick-create-portal.md#create-a-virtual-network) oluşturabilirsiniz. Ardından, [Klasik-Azure Resource Manager sanal ağ](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantısı yapılandırın. 
 
- Azure Resource Manager bir sanal ağ, Azure-SSIS IR farklı bir konumda şirket içi ağınıza zaten bağlıysa, öncelikle Azure-SSIS IR katılması için bir [Azure Resource Manager sanal ağı](../virtual-network/quick-create-portal.md##create-a-virtual-network) oluşturabilirsiniz. Ardından Azure Resource Manager Azure Resource Manager bir sanal ağ bağlantısı yapılandırın. 

## <a name="hosting-the-ssis-catalog-in-sql-database"></a>SQL veritabanında SSIS kataloğunu barındırma

SSIS kataloğunuzu sanal ağ hizmeti uç noktaları içeren bir Azure SQL veritabanında barındırdıysanız, Azure-SSIS IR aynı sanal ağa ve alt ağa eklediğinizden emin olun.

SSIS kataloğunuzu özel uç nokta ile yönetilen bir örnekte barındırdıysanız, Azure-SSIS IR aynı sanal ağa, ancak yönetilen örnekten farklı bir alt ağda eklediğinizden emin olun. Azure-SSIS IR yönetilen örneğinden farklı bir sanal ağa katmak için sanal ağ eşlemesinin (aynı bölgeyle sınırlı) veya sanal ağ ile sanal ağ arasında bağlantı öneririz. Daha fazla bilgi için bkz. [uygulamanızı Azure SQL veritabanı yönetilen örneğine bağlama](../sql-database/sql-database-managed-instance-connect-app.md).

## <a name="access-to-azure-services"></a>Azure hizmetlerine erişim

SSIS paketleriniz [sanal ağ hizmet uç noktalarını](../virtual-network/virtual-network-service-endpoints-overview.md) destekleyen Azure kaynaklarına erişebiliyorlarsa ve bu kaynaklara Azure-SSIS IR erişimi güvenli hale getirmek istiyorsanız, Azure-SSIS IR sanal ağ hizmeti uç noktaları için yapılandırılmış bir sanal ağ alt ağına katılabilir ve ardından aynı alt ağdan erişime izin vermek Için ilgili Azure kaynaklarına bir sanal ağ kuralı ekleyebilirsiniz.

## <a name="access-to-data-sources-protected-by-ip-firewall-rule"></a>IP güvenlik duvarı kuralıyla korunan veri kaynaklarına erişim

SSIS paketleriniz yalnızca belirli statik ortak IP adreslerine izin veren veri depolarına/kaynaklara erişir ve bu kaynaklara Azure-SSIS IR erişimi güvenli hale getirmek istiyorsanız, bir sanal ağa katılırken Azure-SSIS IR için kendi [genel IP adreslerinizi](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address) alabilir ve ardından bu IP adreslerinden erişime izin vermek için ilgili kaynaklara bir IP güvenlik kuralı ekleyebilirsiniz.

Her durumda, sanal ağ yalnızca Azure Resource Manager dağıtım modeliyle dağıtılabilir.

Aşağıdaki bölümlerde daha fazla ayrıntı sağlanmaktadır. 

## <a name="virtual-network-configuration"></a>Sanal ağ yapılandırması

Sanal ağınızı bu gereksinimleri karşılayacak şekilde ayarlayın: 

- `Microsoft.Batch`, Azure-SSIS IR barındıran sanal ağ alt ağınızın aboneliği kapsamında kayıtlı bir sağlayıcı olduğundan emin olun. Klasik bir sanal ağ kullanıyorsanız, bu sanal ağ için klasik sanal makine katılımcısı rolüne de `MicrosoftAzureBatch` katın. 

- Gerekli izinlere sahip olduğunuzdan emin olun. Daha fazla bilgi için bkz. [Izinleri ayarlama](#perms).

- Azure-SSIS IR barındırmak için uygun alt ağı seçin. Daha fazla bilgi için bkz. [alt ağı seçme](#subnet). 

- Azure-SSIS IR için kendi genel IP adreslerinizi alırsanız bkz [. statik genel IP adreslerini seçme](#publicIP)

- Sanal ağda kendi etki alanı adı sistemi (DNS) sunucunuzu kullanıyorsanız, bkz. [DNS sunucusunu ayarlama](#dns_server). 

- Alt ağda bir ağ güvenlik grubu (NSG) kullanıyorsanız bkz. [NSG ayarlama](#nsg). 

- Azure ExpressRoute veya Kullanıcı tanımlı bir yol (UDR) kullanıyorsanız bkz. [Azure ExpressRoute veya UDR kullanma](#route). 

- Sanal ağın kaynak grubunun (veya kendi genel IP adreslerini getirdiğinizden genel IP adreslerinin ' kaynak grubunun) belirli Azure ağ kaynaklarını oluşturup silebileceklerini doğrulayın. Daha fazla bilgi için bkz. [kaynak grubunu ayarlama](#resource-group). 

- [Azure-SSIS IR Için özel kurulumda](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup)açıklandığı gibi Azure-SSIS IR özelleştirirseniz Azure-SSIS IR düğümleriniz, önceden tanımlanmış bir 172.16.0.0 aralığından 172.31.255.255 'e özel IP adresleri alır. Bu nedenle, sanal veya şirket içi ağlarınızın özel IP adresi aralıklarının bu aralıkla çakışmıyor olduğundan emin olun.

Bu diyagramda Azure-SSIS IR için gereken bağlantılar gösterilmektedir:

![Azure-SSIS IR](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="perms"></a>İzinleri ayarla

Azure-SSIS IR oluşturan kullanıcının aşağıdaki izinlere sahip olması gerekir:

- SSIS IR 'nizi Azure Resource Manager sanal bir ağa katılıyorsanız iki seçeneğiniz vardır:

  - Yerleşik ağ katılımcısı rolünü kullanın. Bu rol, gerekenden çok daha büyük bir kapsama sahip olan _Microsoft. Network/\*_ iznine sahiptir.

  - Yalnızca gerekli _Microsoft. Network/virtualNetworks/\*/Join/Action_ iznini içeren özel bir rol oluşturun. Ayrıca, Azure Resource Manager bir sanal ağa katılırken Azure-SSIS IR için kendi genel IP adreslerini getirmek istiyorsanız, lütfen role _Microsoft. Network/publicIPAddresses/*/Join/Action_ iznini de ekleyin.

- SSIS IR 'nizi klasik bir sanal ağa katılıyorsanız yerleşik, klasik sanal makine katılımcısı rolünü kullanmanızı öneririz. Aksi takdirde, sanal ağa ekleme iznini içeren özel bir rol tanımlamanız gerekir.

### <a name="subnet"></a>Alt ağı seçin

Bir alt ağ seçtiğinizde: 

- Azure-SSIS IR dağıtmak için GatewaySubnet 'i seçmeyin. Sanal ağ geçitleri için ayrılmıştır. 

- Seçtiğiniz alt ağın Azure-SSIS IR için kullanılabilir adres alanının yeterli olduğundan emin olun. IR düğüm numarasının en az iki katı için kullanılabilir IP adreslerini bırakın. Azure, bazı IP adreslerini her alt ağ içinde ayırır. Bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumu için ayrılmıştır ve Azure hizmetleri için üç adres daha kullanılır. Daha fazla bilgi için bkz. [Bu alt AĞLARDAKI IP adreslerini kullanma konusunda herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets) 

- Diğer Azure hizmetleri tarafından (örneğin, SQL veritabanı yönetilen örneği, App Service vb.) özel olarak bulunan bir alt ağ kullanmayın. 

### <a name="publicIP"></a>Statik genel IP adreslerini seçin

Bir sanal ağa katılırken Azure-SSIS IR için kendi statik genel IP adreslerini getirmek istiyorsanız, aşağıdaki gereksinimleri karşıladığından emin olun:

- Diğer Azure kaynaklarıyla zaten ilişkilendirilmemiş iki adet kullanılmamış tane sağlanmalıdır. Azure-SSIS IR, her zaman düzenli olarak yükseltireceğiz kullanılacaktır.

- Her ikisi de standart türden statik olmalıdır. Daha fazla ayrıntı için [genel IP adresi SKU 'larına](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm#sku) bakın.

- Her ikisi de bir DNS adına sahip olmalıdır. Bunları oluştururken bir DNS adı sağlamadıysanız, bunu Azure portal yapabilirsiniz.

![Azure-SSIS IR](media/ssis-integration-runtime-management-troubleshoot/setup-publicipdns-name.png)

- Bunlar ve sanal ağ aynı abonelikte ve aynı bölgede olmalıdır.

### <a name="dns_server"></a>DNS sunucusunu ayarlama 

Azure-SSIS IR tarafından birleştirilmiş bir sanal ağda kendi DNS sunucunuzu kullanmanız gerekiyorsa, genel Azure ana bilgisayar adlarını (örneğin, `<your storage account>.blob.core.windows.net`adlı bir Azure Depolama Blobu) çözümleyediğinden emin olun. 

Aşağıdaki adımlar önerilir: 

- İstekleri Azure DNS iletmek için özel DNS 'i yapılandırın. Çözümlenmemiş DNS kayıtlarını kendi DNS sunucunuzdaki Azure özyinelemeli çözümleyiciler 'nin (168.63.129.16) IP adresine iletebilirsiniz. 

- Özel DNS 'i sanal ağın birincil DNS sunucusu olarak ayarlayın. Azure DNS ikincil DNS sunucusu olarak ayarlayın. Azure özyinelemeli çözümleyicilerine (168.63.129.16) ait IP adresini, kendi DNS sunucunuzun kullanılamaması durumunda ikincil bir DNS sunucusu olarak kaydedin. 

Daha fazla bilgi için, bkz. [kendı DNS sunucunuzu kullanan ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a>NSG ayarlama

Azure-SSIS IR tarafından kullanılan alt ağ için bir NSG uygulamanız gerekiyorsa, gelen ve giden trafiğe aşağıdaki bağlantı noktalarıyla izin verin: 

| Yön | Aktarım Protokolü | Kaynak | Kaynak bağlantı noktası aralığı | Hedef | Hedef bağlantı noktası aralığı | Yorumlar |
|---|---|---|---|---|---|---|
| Gelen | TCP | BatchNodeManagement | * | VirtualNetwork | 29876, 29877 (IR 'ye Kaynak Yöneticisi sanal bir ağa katılırsanız) <br/><br/>10100, 20100, 30100 (IR 'yi klasik bir sanal ağa katılırsanız)| Data Factory hizmeti, sanal ağdaki Azure-SSIS IR düğümlerle iletişim kurmak için bu bağlantı noktalarını kullanır. <br/><br/> Bir alt ağ düzeyinde NSG oluşturup oluşturamadıkça, Data Factory Azure-SSIS IR barındıran sanal makinelere bağlı ağ arabirimi kartları (NIC 'ler) düzeyinde her zaman bir NSG yapılandırır. Yalnızca belirtilen bağlantı noktalarında Data Factory IP adreslerinden gelen trafiğe NIC düzeyinde NSG tarafından izin verilir. Bu bağlantı noktalarını alt ağ düzeyinde internet trafiğine açmış olsanız bile, IP adreslerinden Data Factory IP adreslerinden gelen trafik NIC düzeyinde engellenir. |
| Giden | TCP | VirtualNetwork | * | AzureCloud | 443 | Sanal ağdaki Azure-SSIS IR düğümleri Azure depolama ve Azure Event Hubs gibi Azure hizmetlerine erişmek için bu bağlantı noktasını kullanır. |
| Giden | TCP | VirtualNetwork | * | Internet | 80 | Sanal ağdaki Azure-SSIS IR düğümleri Internet 'ten bir sertifika iptal listesi indirmek için bu bağlantı noktasını kullanır. |
| Giden | TCP | VirtualNetwork | * | Sql | 1433, 11000-11999 | Sanal ağdaki Azure-SSIS IR düğümleri, SQL veritabanı sunucunuz tarafından barındırılan bir SSıSDB 'ye erişmek için bu bağlantı noktalarını kullanır. SQL veritabanı sunucusu bağlantı ilkeniz **yeniden yönlendirme**yerine **proxy** olarak ayarlandıysa yalnızca bağlantı noktası 1433 gerekir. Bu giden güvenlik kuralı, sanal ağda yönetilen örneğiniz tarafından barındırılan bir SSSıSDB için geçerli değildir. |
||||||||

### <a name="route"></a>Azure ExpressRoute veya UDR kullanma

Şirket içi ağınızı Azure 'a genişletmek için sanal ağ altyapınıza bir [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantı hattı bağladığınızda, ortak bir yapılandırma Zorlamalı tünel kullanır (bir BGP yolunu, 0.0.0.0/0, sanal ağa duyuruyor). Bu tünel, denetim ve günlüğe kaydetme amacıyla sanal ağ akışından giden internet trafiğini şirket içi ağ gereçine zorlar. 
 
Ya da bir ağ [](../virtual-network/virtual-networks-udr-overview.md) Sanal Gereci (NVA) barındıran Azure-SSIS IR alt ağdan, İnceleme ve günlüğe kaydetme için güvenlik duvarı ya da Azure Güvenlik duvarı olarak bir ağ sanal gereci (NVA) barındıran başka bir alt ağda giden internet trafiğini zorlamak 

Her iki durumda da trafik yolu, bağımlı Azure Data Factory hizmetlerinden (özellikle de Azure Batch Yönetim Hizmetleri), sanal ağdaki Azure-SSIS IR için gerekli gelen bağlantıyı keser. Bunu önlemek için, Azure-SSIS IR içeren alt ağda bir veya daha fazla UDRs tanımlayın. 

Bir Azure ExpressRoute senaryosunda Azure-SSIS IR barındıran alt ağda **Internet** olarak bir sonraki atlama türü ile bir 0.0.0.0/0 yolu uygulayabilirsiniz. Ya da bir sonraki atlama türünden var olan 0.0.0.0/0 yolunu, bir NVA senaryosunda **Internet** 'e **Sanal Gereç** olarak değiştirebilirsiniz.

![Yol ekleme](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)

Giden internet trafiğini bu alt ağdan İnceleme özelliğini kaybetme konusunda endişeleriniz varsa, trafiği yalnızca Azure Batch Yönetim Hizmetleri ve Azure-SSIS IR bir sonraki atlama türü ile **İnternet**arasında yönlendirmek üzere belirli udrs tanımlayabilirsiniz.

Örneğin, Azure-SSIS IR `UK South`konumdaysa, hizmet [ETIKETLERI IP aralığı indirme bağlantısı](https://www.microsoft.com/en-us/download/details.aspx?id=56519) veya [hizmet etiketi bulma API 'si](https://aka.ms/discoveryapi)aracılığıyla HIZMET etiketi `BatchNodeManagement.UKSouth` bir IP aralığı listesi alırsınız. Ardından, sonraki atlama türü ile birlikte, ilgili IP aralığı yollarının aşağıdaki UDRs 'lerini **Internet**olarak uygulayın.

![Azure Batch UDR ayarları](media/join-azure-ssis-integration-runtime-virtual-network/azurebatch-udr-settings.png)

> [!NOTE]
> Bu yaklaşım, ek bir bakım maliyeti doğurur. Azure-SSIS IR bozmamak için IP aralığını düzenli olarak kontrol edin ve UDR 'nize yeni IP aralıkları ekleyin. IP aralığını aylık olarak kontrol etmenizi öneririz, çünkü yeni IP hizmet etiketinde göründüğünde IP 'nin başka bir ay süreceği geçerli olur. 

### <a name="resource-group"></a>Kaynak grubunu ayarlama

Azure-SSIS IR, sanal ağ ile aynı kaynak grubunda belirli ağ kaynaklarını oluşturması gerekir. Bu kaynaklar şunları içerir:
- *\<guıd >-azurebatch-cloudserviceloaddengeleyici*olan bir Azure yük dengeleyici.
- *\<guıd >-azurebatch-cloudservicepublicıp*adlı bir Azure genel IP adresi.
- *\<guıd >-azurebatch-cloudservicenetworksecuritygroup*adlı bir ağ iş güvenlik grubu. 

> [!NOTE]
> Artık Azure-SSIS IR için kendi statik genel IP adreslerinizi getirebilirsiniz. Bu senaryoda, sanal ağ yerine yalnızca statik genel IP adreslerinizle aynı kaynak grubu altında Azure yük dengeleyici ve ağ güvenlik grubunu oluşturacağız.

Azure-SSIS IR, bu kaynaklar oluşturulur. Azure-SSIS IR durdurulduğunda bunlar silinir. Azure-SSIS IR için kendi statik genel IP adreslerinizi getiriyorsa, Azure-SSIS IR durdurulduğunda bunlar silinmez. Azure-SSIS IR durdurmadan engellemeden kaçınmak için, diğer kaynaklarınızda bu ağ kaynaklarını yeniden kullanmayın. 

Sanal ağ/statik genel IP adreslerinizin ait olduğu kaynak grubunda/abonelikte kaynak kilidi olmadığından emin olun. Salt bir okuma/silme kilidi yapılandırırsanız, Azure-SSIS IR başlatıp durdurma başarısız olur veya yanıt vermeyi durdurur.

Sanal ağın/statik genel IP adreslerinin ait olduğu kaynak grubu/abonelik altında aşağıdaki kaynakların oluşturulmasını engelleyen bir Azure ilkenizin olmadığından emin olun: 
- Microsoft. Network/LoadBalancers 
- Microsoft.Network/NetworkSecurityGroups 
- Microsoft. Network/Publicıpaddresses 

### <a name="faq"></a>SSS

- Gelen bağlantı için Azure-SSIS IR kullanıma sunulan genel IP adresini nasıl koruyabilirim? Genel IP adresini kaldırmak mümkün mü?
 
  Şu anda, Azure-SSIS IR bir sanal ağa katıldığında bir genel IP adresi otomatik olarak oluşturulur. Yalnızca Azure Batch yönetim hizmetlerinin Azure-SSIS IR gelen bağlantı kurulmasına izin vermek için NIC düzeyinde bir NSG sunuyoruz. Ayrıca, gelen koruma için bir alt ağ düzeyi NSG belirtebilirsiniz.

  Herhangi bir genel IP adresinin gösterilmesini istemiyorsanız, senaryonuza geçerliyse, Azure-SSIS IR bir sanal ağa katılmasını sağlamak yerine, [kendinden konak IR 'yi Azure-SSIS IR için ara sunucu olarak yapılandırmayı](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) düşünün.
 
- Azure-SSIS IR genel IP adresini, Veri Kaynaklarım için güvenlik duvarının izin verilenler listesine ekleyebilir miyim?

  Artık Azure-SSIS IR için kendi statik genel IP adreslerinizi getirebilirsiniz. Bu durumda, IP adreslerinizi, veri kaynaklarınızın güvenlik duvarı izin verilenler listesine ekleyebilirsiniz. Senaryonuza bağlı olarak Azure-SSIS IR veri erişiminin güvenliğini sağlamak için aşağıdaki diğer seçenekleri de göz önünde bulundurmanız gerekir:

  - Veri kaynağınız şirket içinde ise, bir sanal ağı şirket içi ağınıza bağladıktan ve Azure-SSIS IR sanal ağ alt ağına katıldıktan sonra bu alt ağın özel IP adresi aralığını, veri kaynağınız için güvenlik duvarının izin verilenler listesine ekleyebilirsiniz .
  - Veri kaynağınız sanal ağ hizmet uç noktalarını destekleyen bir Azure hizmeti ise, sanal ağ alt ağınızda bir sanal ağ hizmeti uç noktası yapılandırabilir ve Azure-SSIS IR bu alt ağa katabilirsiniz. Daha sonra veri kaynağınıza yönelik güvenlik duvarına bu alt ağa sahip bir sanal ağ kuralı ekleyebilirsiniz.
  - Veri kaynağınız Azure dışı bir bulut hizmeti ise, Azure-SSIS IR giden trafiği statik bir genel IP adresi aracılığıyla NVA/Azure Güvenlik Duvarı 'na yönlendirmek için UDR kullanabilirsiniz. Daha sonra, NVA/Azure Güvenlik duvarınızın statik genel IP adresini, veri kaynağınız için güvenlik duvarının izin verilenler listesine ekleyebilirsiniz.
  - Yukarıdaki seçeneklerden hiçbiri gereksinimlerinizi karşılamıyorsa, [Azure-SSIS IR için otomatik olarak barındırılan BIR IR 'yi proxy olarak yapılandırmayı](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis)düşünün. Daha sonra, şirket içinde barındırılan IR 'yi barındıran makinenin statik genel IP adresini, veri kaynağınız için güvenlik duvarının izin verilenler listesine ekleyebilirsiniz.

- Neden Azure-SSIS IR için kendi kendinize getirmek istersem iki statik genel adres sağlamam gerekir mi?

  Azure-SSIS IR düzenli aralıklarla otomatik olarak güncelleştirilir. Yeni düğümler yükseltme sırasında oluşturulur ve eskiler silinir. Ancak, kapalı kalma süresini önlemek için eski düğümler, yeni olanlar hazırlanana kadar silinmeyecektir. Bu nedenle, eski düğümler tarafından kullanılan ilk statik genel IP adresiniz hemen yayımlanamazlar ve yeni düğümleri oluşturmak için ikinci statik genel IP adresiniz olması gerekir.

- Azure-SSIS IR için kendi statik genel IP adreslerimi aldım, ancak neden hala veri kaynaklarıma erişemiyorum?

  - İki statik genel IP adresinin, veri kaynaklarınızın güvenlik duvarı izin verilenler listesine eklendiğinden emin olun. Azure-SSIS IR her yükseltildikten sonra, statik genel IP adresi sizin tarafınızdan getirilen iki arasında geçiş yapılır. Bunlardan yalnızca birini izin verilenler listesine eklerseniz, Azure-SSIS IR için veri erişimi yükseltmeden sonra bozulur.
  - Veri kaynağınız bir Azure hizmeti ise, lütfen sanal ağ hizmeti uç noktalarıyla yapılandırılıp yapılandırılmadığını denetleyin. Bu durumda, Azure-SSIS IR veri kaynağınıza giden trafik, Azure hizmetleri tarafından yönetilen özel IP adreslerini kullanacak şekilde geçiş yapar ve kendi statik genel IP adreslerinizi, veri kaynağınız için güvenlik duvarının izin verilenler listesine ekler.

## <a name="azure-portal-data-factory-ui"></a>Azure portal (Data Factory UI)

Bu bölümde, Azure portal ve Data Factory Kullanıcı arabirimini kullanarak var olan bir Azure-SSIS IR sanal ağa (klasik veya Azure Resource Manager) nasıl katılabilmeniz gösterilmektedir. 

Azure-SSIS IR sanal ağa katılmadan önce sanal ağı düzgün şekilde yapılandırmanız gerekir. Sanal ağ türü (klasik veya Azure Resource Manager) için geçerli olan bölümdeki adımları izleyin. Ardından, Azure-SSIS IR sanal ağa katmak için üçüncü bölümdeki adımları izleyin. 

### <a name="configure-an-azure-resource-manager-virtual-network"></a>Azure Resource Manager sanal ağını yapılandırma

Bir Azure-SSIS IR katılmayı denemeden önce Azure Resource Manager sanal ağını yapılandırmak için portalını kullanın.

1. Microsoft Edge veya Google Chrome 'ı başlatın. Şu anda yalnızca bu Web tarayıcıları Data Factory Kullanıcı arabirimini destekler. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın. 

1. **Diğer hizmetler**' i seçin. **Sanal ağları**filtreleyin ve seçin. 

1. İçin filtre uygulayın ve listedeki Sanal ağınızı seçin. 

1. **Sanal ağ** sayfasında **Özellikler**' i seçin. 

1. Sanal ağ için kaynak KIMLIĞINI panoya kopyalamak üzere **kaynak kimliği** için Kopyala düğmesini seçin. KODU OneNote 'taki panodan veya bir dosyada kaydedin. 

1. Sol taraftaki menüde **alt ağlar**' ı seçin. Kullanılabilir adreslerin sayısının Azure-SSIS IR düğümlerden büyük olduğundan emin olun. 

1. Azure Batch sağlayıcısının sanal ağa sahip Azure aboneliğine kayıtlı olduğunu doğrulayın. Veya Azure Batch sağlayıcıyı kaydedin. Aboneliğinizde zaten bir Azure Batch hesabınız varsa, aboneliğiniz Azure Batch için kaydedilir. (Data Factory portalında Azure-SSIS IR oluşturursanız, Azure Batch sağlayıcı sizin için otomatik olarak kaydedilir.) 

   1. Azure portal, sol taraftaki menüden **abonelikler**' i seçin. 

   1. Aboneliğinizi seçin. 

   1. Sol tarafta **kaynak sağlayıcıları**' nı seçin ve **Microsoft. Batch** ' in kayıtlı bir sağlayıcı olduğunu onaylayın. 

   !["Kayıtlı" durumunun onayı](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Listede **Microsoft. Batch** ' i görmüyorsanız, kaydetmek için aboneliğinizde [boş bir Azure Batch hesabı oluşturun](../batch/batch-account-create-portal.md) . Daha sonra silebilirsiniz. 

### <a name="configure-a-classic-virtual-network"></a>Klasik sanal ağ yapılandırma

Bir Azure-SSIS IR katılmayı denemeden önce, klasik sanal ağı yapılandırmak için portalını kullanın. 

1. Microsoft Edge veya Google Chrome 'ı başlatın. Şu anda yalnızca bu Web tarayıcıları Data Factory Kullanıcı arabirimini destekler. 

1. [Azure Portal](https://portal.azure.com)’ında oturum açın. 

1. **Diğer hizmetler**' i seçin. **Sanal ağlar (klasik)** için filtre uygulayın ve seçin. 

1. İçin filtre uygulayın ve listedeki Sanal ağınızı seçin. 

1. **Sanal ağ (klasik)** sayfasında **Özellikler**' i seçin. 

   ![Klasik sanal ağ kaynağı KIMLIĞI](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Klasik ağın kaynak KIMLIĞINI panoya kopyalamak için **kaynak kimliği** için Kopyala düğmesini seçin. KODU OneNote 'taki panodan veya bir dosyada kaydedin. 

1. Sol taraftaki menüde **alt ağlar**' ı seçin. Kullanılabilir adreslerin sayısının Azure-SSIS IR düğümlerden büyük olduğundan emin olun. 

   ![Sanal ağdaki kullanılabilir adreslerin sayısı](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Sanal ağ için **MicrosoftAzureBatch** Ile **Klasik sanal makine katılımcısı** rolüne katın. 

   1. Sol taraftaki menüden **erişim denetimi (IAM)** öğesini seçin ve **rol atamaları** sekmesini seçin. 

   !["Erişim denetimi" ve "Ekle" düğmeleri](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

   1. Seçin **rol ataması Ekle**.

   1. Rol **ataması Ekle** sayfasında, **rol**Için, **Klasik sanal makine katılımcısı**' nı seçin. **Seç** kutusunda, **ddbf3205-c6bd-46AE-8127-60eb93363864**yapıştırın ve sonra arama sonuçları listesinden **Microsoft Azure Batch** ' yi seçin. 

   !["Rol ataması ekleme" sayfasında sonuçları ara](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

   1. Ayarları kaydetmek ve sayfayı kapatmak için **Kaydet** ' i seçin. 

   ![Erişim ayarlarını kaydet](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

   1. Katkıda bulunanlar listesinde **Microsoft Azure Batch** görtığınızdan emin olun. 

   ![Azure Batch erişimi onayla](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Azure Batch sağlayıcısının sanal ağa sahip Azure aboneliğine kayıtlı olduğunu doğrulayın. Veya Azure Batch sağlayıcıyı kaydedin. Aboneliğinizde zaten bir Azure Batch hesabınız varsa, aboneliğiniz Azure Batch için kaydedilir. (Data Factory portalında Azure-SSIS IR oluşturursanız, Azure Batch sağlayıcı sizin için otomatik olarak kaydedilir.) 

   1. Azure portal, sol taraftaki menüden **abonelikler**' i seçin. 

   1. Aboneliğinizi seçin. 

   1. Sol tarafta **kaynak sağlayıcıları**' nı seçin ve **Microsoft. Batch** ' in kayıtlı bir sağlayıcı olduğunu onaylayın. 

   !["Kayıtlı" durumunun onayı](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Listede **Microsoft. Batch** ' i görmüyorsanız, kaydetmek için aboneliğinizde [boş bir Azure Batch hesabı oluşturun](../batch/batch-account-create-portal.md) . Daha sonra silebilirsiniz. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Azure-SSIS IR bir sanal ağa ekleyin

Azure Resource Manager Sanal ağınızı veya klasik Sanal ağınızı yapılandırdıktan sonra, Azure-SSIS IR sanal ağa ekleyebilirsiniz:

1. Microsoft Edge veya Google Chrome 'ı başlatın. Şu anda yalnızca bu Web tarayıcıları Data Factory Kullanıcı arabirimini destekler. 

1. [Azure Portal](https://portal.azure.com), sol taraftaki menüden **veri fabrikaları**' nı seçin. Menüde **veri fabrikaları** görmüyorsanız, **diğer hizmetler**' i seçin ve ardından **zekası + analiz** bölümünde **veri fabrikaları**' nı seçin. 

   ![Veri fabrikaları listesi](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Listeden Azure-SSIS IR veri fabrikanızı seçin. Veri fabrikanızın giriş sayfasını görürsünüz. **& dağıtım** kutucuğunu seçin. Data Factory Kullanıcı arabirimini ayrı bir sekmede görürsünüz. 

   ![Data factory giriş sayfası](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Data Factory Kullanıcı arabiriminde, **Düzenle** sekmesine geçin, **Bağlantılar**' ı seçin ve **tümleştirme çalışma zamanları** sekmesine geçin. 

   !["Tümleştirme çalışma zamanları" sekmesi](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Azure-SSIS IR çalışıyorsa, **tümleştirme çalışma zamanları** listesinde, **eylemler** sütununda, Azure-SSIS IR için **Durdur** düğmesini seçin. Azure-SSIS IR durduruncaya kadar düzenleyemezsiniz. 

   ![IR 'yi durdur](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. Tümleştirme çalışma **zamanları** listesinde, **eylemler** sütununda, Azure-SSIS IR için **Düzenle** düğmesini seçin. 

   ![Tümleştirme çalışma zamanını düzenleme](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. Tümleştirme çalışma zamanı kurulum panelinde, **İleri** düğmesini seçerek **Genel ayarlar** ve **SQL ayarları** bölümlerinde ilerleyin. 

1. **Gelişmiş ayarlar** bölümünde: 

   1. **Azure-SSIS Integration Runtime katılması için bir VNET seçin, ADF 'nin belirli ağ kaynaklarını oluşturmasına izin verin ve isteğe bağlı olarak kendi statik ortak IP adreslerinizi getirin** onay kutusunu seçin. 

   1. **Abonelik**için sanal ağınıza sahip Azure aboneliğini seçin.

   1. **Konum**için, tümleştirme çalışma zamanından aynı konum seçilidir.

   1. **Tür**için, sanal ağınızın türünü seçin: klasik veya Azure Resource Manager. Klasik sanal ağların yakında kullanım dışı olacağı için bir Azure Resource Manager sanal ağı seçmenizi öneririz.

   1. **VNET adı**için sanal ağınızın adını seçin. Sanal ağ hizmet uç noktaları veya yönetilen örnek ile birlikte, SSıSDB 'yi barındırmak için özel uç nokta ile Azure SQL veritabanı sunucunuz için kullanılan bir tane olmalıdır. Ya da şirket içi ağınıza bağlı bir aynı olmalıdır. Aksi takdirde, Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek için herhangi bir sanal ağ olabilir.

   1. **Alt ağ adı**için, sanal ağınız için alt ağ adını seçin. SSıSDB barındırmak için sanal ağ hizmet uç noktaları ile Azure SQL veritabanı sunucunuz için kullanılan bir tane olmalıdır. Veya SSıSDB 'yi barındırmak için özel uç nokta ile yönetilen örneğiniz için kullanılan farklı bir alt ağ olmalıdır. Aksi takdirde, Azure-SSIS IR için kendi statik genel IP adreslerinizi getirmek için herhangi bir alt ağ olabilir.

   1. Azure-SSIS IR için kendi statik genel IP adreslerini getirmek isteyip istemediğinizi seçmek üzere **Azure-SSIS Integration Runtime için statik genel IP adreslerini getir** onay kutusunu seçin. bu sayede, veri kaynaklarınıza yönelik güvenlik duvarında bunlara izin verebilirsiniz.

      Onay kutusunu seçerseniz, aşağıdaki adımları uygulayın.

      1. **İlk statik genel IP adresi**için Azure-SSIS IR [gereksinimlerini karşılayan](#publicIP) ilk statik genel IP adresini seçin. Herhangi bir hesabınız yoksa, yeni bağlantı **Oluştur** ' a tıklayarak Azure Portal STATIK genel IP adresleri oluşturun ve ardından buradan Yenile düğmesine tıklayın, böylece bunları seçebilirsiniz.
      
      1. **İkinci statik genel IP adresi**için Azure-SSIS IR [gereksinimlerini karşılayan](#publicIP) ikinci statik genel IP adresini seçin. Herhangi bir hesabınız yoksa, yeni bağlantı **Oluştur** ' a tıklayarak Azure Portal STATIK genel IP adresleri oluşturun ve ardından buradan Yenile düğmesine tıklayın, böylece bunları seçebilirsiniz.

   1. **VNET doğrulaması**' nı seçin. Doğrulama başarılı olursa **devam**' ı seçin. 

   ![Bir sanal ağda gelişmiş ayarlar](./media/tutorial-create-azure-ssis-runtime-portal/advanced-settings-vnet.png)

1. **Özet** bölümünde, Azure-SSIS IR için tüm ayarları gözden geçirin. Ardından **Güncelleştir**' i seçin.

1. Azure-SSIS IR **Eylemler** sütunundaki **başlat** düğmesini seçerek Azure-SSIS IR başlatın. Bir sanal ağa katılan Azure-SSIS IR başlatmak için 20 ila 30 dakika sürer. 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configure-a-virtual-network"></a>Sanal ağı yapılandırma

Azure-SSIS IR bir sanal ağa katılabilmeniz için önce sanal ağı yapılandırmanız gerekir. Sanal ağa katılması Azure-SSIS IR için sanal ağ izinlerini ve ayarlarını otomatik olarak yapılandırmak için aşağıdaki betiği ekleyin:

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

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Azure-SSIS IR oluşturma ve sanal ağa ekleme

Bir Azure-SSIS IR oluşturabilir ve aynı anda bir sanal ağa katabilirsiniz. Tüm betik ve yönergeler için bkz. [Azure-SSIS IR oluşturma](create-azure-ssis-integration-runtime.md#use-azure-powershell-to-create-an-integration-runtime).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Mevcut bir Azure-SSIS IR sanal ağa ekleme

[Azure-SSIS IR oluştur](create-azure-ssis-integration-runtime.md) makalesinde, bir Azure-SSIS IR oluşturma ve aynı betikteki bir sanal ağa ekleme işlemlerinin nasıl yapılacağı gösterilir. Zaten bir Azure-SSIS IR varsa, sanal ağa katmak için aşağıdaki adımları izleyin: 
1. Azure-SSIS IR durdurun. 
1. Azure-SSIS IR sanal ağa katılacak şekilde yapılandırın. 
1. Azure-SSIS IR başlatın. 

### <a name="define-the-variables"></a>Değişkenleri tanımlama

```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Azure-SSIS IR durdur

Bir sanal ağa katılabilmeniz için önce Azure-SSIS IR durdurmanız gerekir. Bu komut tüm düğümlerini serbest bırakır ve faturalandırmayı durduruyor:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Azure-SSIS IR katılacak sanal ağ ayarlarını yapılandırın

Azure-SSIS 'nin birleştirileceği sanal ağın ayarlarını yapılandırmak için şu betiği kullanın: 

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

### <a name="configure-the-azure-ssis-ir"></a>Azure-SSIS IR yapılandırma

Azure-SSIS IR bir sanal ağa katmak için `Set-AzDataFactoryV2IntegrationRuntime` komutunu çalıştırın: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Azure-SSIS IR başlatın

Azure-SSIS IR başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Bu komutun tamamlanabilmesi için 20 ila 30 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar

Azure-SSIS IR hakkında daha fazla bilgi için aşağıdaki makalelere bakın: 
- [Azure-SSIS IR](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR dahil olmak üzere IRS hakkında genel kavramsal bilgiler sağlanmaktadır. 
- [Öğretici: SSIS paketlerini Azure 'A dağıtın](tutorial-create-azure-ssis-runtime-portal.md). Bu öğretici, Azure-SSIS IR oluşturmak için adım adım yönergeler sağlar. SSIS kataloğunu barındırmak için Azure SQL veritabanını kullanır. 
- [Azure-SSIS IR oluşturun](create-azure-ssis-integration-runtime.md). Bu makale öğreticide genişletilir. SSIS kataloğunu barındırmak için sanal ağ hizmet uç noktaları veya yönetilen örnek ile Azure SQL veritabanı kullanımıyla ilgili yönergeler sağlar. Azure-SSIS IR bir sanal ağa nasıl katılagösterdiğini gösterir. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR hakkında nasıl bilgi alabileceğiniz gösterilmektedir. Döndürülen bilgiler için durum açıklamalarını sağlar. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makalede Azure-SSIS IR nasıl durdurulacağı, başlatılacağı veya silineceği gösterilmektedir. Ayrıca, düğüm ekleyerek Azure-SSIS IR nasıl ölçeklendirilebilen de gösterilmektedir.