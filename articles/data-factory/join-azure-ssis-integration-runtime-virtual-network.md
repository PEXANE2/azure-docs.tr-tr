---
title: Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa ekleyin | Microsoft Docs
description: Azure-SSIS tümleştirme çalışma zamanını bir Azure sanal ağına nasıl katılacağınızı öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/15/2019
author: swinarko
ms.author: sawinark
ms.reviewer: douglasl
manager: craigg
ms.openlocfilehash: 3a1e272fa332c0bf0ee4e5ececa3edd83aec1d46
ms.sourcegitcommit: 0c906f8624ff1434eb3d3a8c5e9e358fcbc1d13b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69543151"
---
# <a name="join-an-azure-ssis-integration-runtime-to-a-virtual-network"></a>Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katma
Azure Data Factory (ADF) içinde SQL Server Integration Services (SSIS) kullandığınızda, aşağıdaki senaryolarda Azure-SSIS Integration Runtime (IR) bir Azure sanal ağına katılmanız gerekir: 

- Azure-SSIS IR 'niz üzerinde çalışan SSIS paketlerinden şirket içi veri depolarına, ara sunucu olarak otomatik olarak barındırılan bir IR yapılandırmadan/yönetmeden bağlanmak istiyorsunuz. 

- Azure-SSIS IR 'niz üzerinde çalışan SSIS paketlerindeki sanal ağ hizmet uç noktaları ile desteklenen Azure hizmet kaynaklarına bağlanmak istiyorsunuz.

- Sanal ağ hizmet uç noktaları/yönetilen örneği olan Azure SQL veritabanı 'nda SSIS katalog veritabanını (SSSıSDB) barındırıyorsanız. 

ADF, Azure-SSIS IR 'nizi klasik dağıtım modeliyle veya Azure Resource Manager dağıtım modeliyle oluşturulan bir sanal ağa eklemenize olanak tanır. 

> [!IMPORTANT]
> Klasik sanal ağ Şu anda kullanım dışıdır, bu nedenle lütfen bunun yerine Azure Resource Manager sanal ağını kullanın.  Klasik sanal ağı zaten kullanıyorsanız, lütfen Azure Resource Manager sanal ağını mümkün olan en kısa sürede kullanmaya geçin.

## <a name="access-to-on-premises-data-stores"></a>Şirket içi veri depolarına erişim
SSIS 'niz şirket içi veri depolarına erişebiliyse, Azure-SSIS IR 'nizi şirket içi ağa bağlı bir sanal ağa ekleyebilir veya Azure-SSIS IR 'niz için otomatik olarak barındırılan bir IR 'yi yapılandırma/yönetme konusuna bakın. [ Azure-SSIS IR için proxy](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) makalesi. Azure-SSIS IR 'nizi bir sanal ağa katılırken, dikkat edilmesi gereken birkaç önemli nokta vardır: 

- Şirket içi ağınıza bağlı mevcut bir sanal ağ yoksa, ilk olarak Azure-SSIS tümleştirme çalışma zamanının katılması için bir [Azure Resource Manager sanal ağı](../virtual-network/quick-create-portal.md#create-a-virtual-network) veya [Klasik bir sanal ağ](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) oluşturun. Ardından, bu sanal ağdan şirket içi ağınıza bir siteden siteye [VPN Ağ Geçidi bağlantısı](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) veya [ExpressRoute](../expressroute/expressroute-howto-linkvnet-classic.md) bağlantısı yapılandırın. 

- Azure-SSIS IR ile aynı konumda bulunan şirket içi ağınıza bağlı bir Azure Resource Manager veya klasik sanal ağ varsa, IR 'ye bu sanal ağa katabilirsiniz. 

- Azure-SSIS IR 'nizden farklı bir konumda şirket içi ağınıza bağlı mevcut bir klasik sanal ağ varsa, ilk olarak Azure-SSIS IR 'nizin katılması için bir [Klasik sanal ağ](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) oluşturabilirsiniz. Ardından, [Klasik-Klasik bir sanal ağ](../vpn-gateway/vpn-gateway-howto-vnet-vnet-portal-classic.md) bağlantısı yapılandırın. Veya Azure-SSIS tümleştirme çalışma zamanının katılması için bir [Azure Resource Manager sanal ağı](../virtual-network/quick-create-portal.md#create-a-virtual-network) oluşturabilirsiniz. Ardından, [Klasik-Azure Resource Manager sanal ağ](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantısı yapılandırın. 
 
- Azure-SSIS IR 'nizden farklı bir konumda şirket içi ağınıza bağlı bir Azure Resource Manager sanal ağ varsa, ilk olarak Azure-SSIS IR 'nizin katılması için bir [Azure Resource Manager sanal ağı](../virtual-network/quick-create-portal.md##create-a-virtual-network) oluşturabilirsiniz. Ardından, Azure Resource Manager Azure Resource Manager bir sanal ağ bağlantısı yapılandırın. İsterseniz, Azure-SSIS IR 'nizin katılması için [Klasik bir sanal ağ](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) oluşturabilirsiniz. Ardından, [Klasik-Azure Resource Manager sanal ağ](../vpn-gateway/vpn-gateway-connect-different-deployment-models-portal.md) bağlantısı yapılandırın. 

## <a name="access-to-azure-services-with-virtual-network-service-endpoints"></a>Sanal ağ hizmeti uç noktaları ile Azure hizmetlerine erişim
SSIS paketleriniz, [sanal ağ hizmeti uç noktaları](../virtual-network/virtual-network-service-endpoints-overview.md) ile desteklenen Azure hizmet kaynaklarına erişebiliyorlarsa ve bu kaynakların Azure-SSIS IR ile güvenliğini sağlamak Istiyorsanız, Azure-SSIS IR 'nizi sanal ağ ile yapılandırılmış sanal ağ alt ağına ekleyebilirsiniz aynı alt ağdan erişime izin vermek için Azure hizmet kaynağına sanal ağ kuralı eklerken hizmet uç noktası ve bu sırada.

## <a name="host-the-ssis-catalog-database-in-azure-sql-database-with-virtual-network-service-endpointsmanaged-instance"></a>Sanal ağ hizmeti uç noktaları/yönetilen örnek ile Azure SQL veritabanı 'nda SSIS katalog veritabanını barındırma
SSIS kataloğunuzu Azure SQL veritabanı 'nda sanal ağ hizmet uç noktaları ile barındırdıysanız, Azure-SSIS IR 'nizi aynı sanal ağa ve alt ağa katdığınızdan emin olun.

Azure-SSIS IR 'nizi yönetilen örnekle aynı sanal ağa katılırsanız, Azure-SSIS IR 'nin yönetilen örnekten farklı bir alt ağda olduğundan emin olun. Azure-SSIS IR 'nizi yönetilen örneğinden farklı bir sanal ağa katılırsanız, sanal ağ eşlemesinin (aynı bölgeyle sınırlı) veya sanal ağ bağlantısıyla sanal ağı kullanmanızı öneririz. Bkz. [uygulamanızı Azure SQL veritabanı yönetilen örneğine bağlama](../sql-database/sql-database-managed-instance-connect-app.md).

Her durumda, sanal ağ yalnızca Azure Resource Manager dağıtım modeliyle dağıtılabilir.

Aşağıdaki bölümlerde daha fazla ayrıntı sağlanmaktadır. 

## <a name="requirements-for-virtual-network-configuration"></a>Sanal ağ yapılandırması için gereksinimler
-   Azure-SSIS IR 'yi barındıran sanal ağ alt ağınızın aboneliği kapsamında kayıtlı bir sağlayıcı olduğundan emin `Microsoft.Batch` olun. Klasik sanal ağ kullanıyorsanız, bu sanal ağ için klasik `MicrosoftAzureBatch` sanal makine katılımcısı rolüne de katın. 

-   Gerekli izinlere sahip olduğunuzdan emin olun. Bkz. [gerekli izinler](#perms).

-   Azure-SSIS IR 'yi barındıracak doğru alt ağı seçin. Bkz. [alt ağı seçin](#subnet). 

-   Sanal ağ üzerinde kendi etki alanı adı hizmetleri (DNS) sunucunuzu kullanıyorsanız, bkz. [etki alanı adı Hizmetleri sunucusu](#dns_server). 

-   Alt ağda bir ağ güvenlik grubu (NSG) kullanıyorsanız bkz. [ağ güvenlik grubu](#nsg) 

-   Azure Express Route kullanıyorsanız veya Kullanıcı tanımlı yol (UDR) yapılandırıyorsanız, bkz. [Azure ExpressRoute veya Kullanıcı tanımlı yol kullanma](#route). 

-   Sanal ağın kaynak grubunun belirli Azure ağ kaynaklarını oluşturup silmesine da emin olun. [Kaynak grubu Için gereksinimlere](#resource-group)bakın. 

-   Azure-SSIS IR 'nizi [Azure-SSIS IR Için özel kurulum](https://docs.microsoft.com/azure/data-factory/how-to-configure-azure-ssis-ir-custom-setup) makalesinde açıklandığı şekilde özelleştirirseniz, Azure-SSIS IR düğümleriniz önceden tanımlanmış bir 172.16.0.0-172.31.255.255 ARALıĞıNDAN özel IP adresleri tahsis eder, bu nedenle lütfen özel IP 'nin sanal/şirket içi ağlarınızın adres aralıkları bu aralıkla çakışmıyor.

Azure-SSIS IR 'niz için gereken bağlantıları gösteren bir diyagram aşağıda verilmiştir:

![Azure-SSIS IR](media/join-azure-ssis-integration-runtime-virtual-network/azure-ssis-ir.png)

### <a name="perms"></a>Gerekli izinler

Azure-SSIS Integration Runtime oluşturan kullanıcının aşağıdaki izinlere sahip olması gerekir:

- SSIS IR 'nizi Azure Resource Manager sanal bir ağa katılıyorsanız iki seçeneğiniz vardır:

  - Yerleşik *ağ katılımcısı* rolünü kullanın. Bu rol, gerekenden çok daha büyük bir kapsama sahip olan _Microsoft. Network/\*_  Permission ile birlikte gelir.

  - Yalnızca gerekli _Microsoft. Network/virtualnetworks/\*/Join/Action_ iznini içeren özel bir rol oluşturun. 

- SSIS IR 'nizi klasik bir sanal ağa katılıyorsanız yerleşik, *Klasik sanal makine katılımcısı* rolünü kullanmanızı öneririz. Aksi takdirde, sanal ağa ekleme iznini içeren özel bir rol tanımlamanız gerekir.

### <a name="subnet"></a>Alt ağı seçin
-   Sanal ağ geçitleri için ayrıldığından, Azure-SSIS Integration Runtime dağıtmak için GatewaySubnet 'i seçmeyin. 

-   Seçtiğiniz alt ağın, Azure-SSIS IR 'nin kullanması için yeterli kullanılabilir adres alanına sahip olduğundan emin olun. Kullanılabilir IP adreslerinde en az 2 * IR düğüm numarası bırakın. Azure, bazı IP adreslerini her alt ağ içinde ayırır ve bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumu için ayrılmıştır ve Azure hizmetleri için kullanılan üç adres daha vardır. Daha fazla bilgi için, [Bu alt ağlar IÇINDEKI IP adreslerini kullanma konusunda herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets). 

-   Diğer Azure hizmetleri tarafından (örneğin, SQL veritabanı yönetilen örneği, App Service vb.) özel olarak bulunan bir alt ağ kullanmayın. 

### <a name="dns_server"></a>Etki alanı adı Hizmetleri sunucusu 
Azure-SSIS tümleştirme çalışma zamanı tarafından birleştirilmiş bir sanal ağda kendi etki alanı adı hizmetleri (DNS) sunucusunu kullanmanız gerekiyorsa, genel Azure ana bilgisayar adlarını (örneğin, bir Azure Depolama Blobu adı `<your storage account>.blob.core.windows.net`) çözümleyediğinden emin olun. 

Aşağıdaki adımlar önerilir: 

-   İstekleri Azure DNS iletmek için özel DNS yapılandırma. Çözümlenmemiş DNS kayıtlarını, kendi DNS sunucunuzdaki Azure özyinelemeli çözümleyiciler 'nin (168.63.129.16) IP adresine iletebilirsiniz. 

-   Özel DNS 'i birincil ve Azure DNS sanal ağ için ikincil olarak ayarlayın. Kendi DNS sunucunuzun kullanılamaması durumunda Azure 'un özyinelemeli çözümleyicilerine (168.63.129.16) ait IP adresini ikincil bir DNS sunucusu olarak kaydedin. 

Daha fazla bilgi için, bkz. [kendı DNS sunucunuzu kullanan ad çözümlemesi](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server). 

### <a name="nsg"></a>Ağ güvenlik grubu
Azure-SSIS tümleştirme çalışma zamanı tarafından kullanılan alt ağ için bir ağ güvenlik grubu (NSG) uygulamanız gerekiyorsa aşağıdaki bağlantı noktalarıyla gelen/giden trafiğe izin verin: 

| Direction | Aktarım Protokolü | Source | Kaynak bağlantı noktası aralığı | Hedef | Hedef bağlantı noktası aralığı | Açıklamalar |
|---|---|---|---|---|---|---|
| Gelen | TCP | BatchNodeManagement | * | VirtualNetwork | 29876, 29877 (IR 'ye Azure Resource Manager sanal bir ağa katılırsanız) <br/><br/>10100, 20100, 30100 (IR 'yi klasik bir sanal ağa katılırsanız)| Data Factory hizmeti, sanal ağdaki Azure-SSIS tümleştirme çalışma zamanının düğümleri ile iletişim kurmak için bu bağlantı noktalarını kullanır. <br/><br/> Bir alt ağ düzeyinde NSG oluşturup oluşturamadıkça Data Factory her zaman Azure-SSIS IR 'yi barındıran sanal makinelere bağlı ağ arabirimi kartları (NIC) düzeyinde bir NSG 'yi yapılandırır. Yalnızca belirtilen bağlantı noktalarında Data Factory IP adreslerinden gelen trafiğe NIC düzeyinde NSG tarafından izin verilir. Bu bağlantı noktalarını alt ağ düzeyinde Internet trafiğine açmış olsanız bile, IP adreslerinden gelen trafik Data Factory IP adreslerinden gelen trafiği NIC düzeyinde engellenir. |
| Giden | TCP | VirtualNetwork | * | AzureCloud | 443 | Sanal ağdaki Azure-SSIS tümleştirme çalışma zamanının düğümleri, Azure depolama ve Azure Event Hubs gibi Azure hizmetlerine erişmek için bu bağlantı noktasını kullanır. |
| Giden | TCP | VirtualNetwork | * | İnternet | 80 | Sanal ağdaki Azure-SSIS tümleştirme çalışma zamanının düğümleri, sertifika iptal listesini Internet 'ten indirmek için bu bağlantı noktasını kullanır. |
| Giden | TCP | VirtualNetwork | * | Sql | 1433, 11000-11999 | Sanal ağdaki Azure-SSIS tümleştirme çalışma zamanının düğümleri, Azure SQL veritabanı sunucunuz tarafından barındırılan SSıSDB 'ye erişmek için bu bağlantı noktalarını kullanır. Azure SQL veritabanı sunucusu bağlantı ilkeniz **yeniden yönlendirme**yerine **proxy** olarak ayarlandıysa yalnızca bağlantı noktası 1433 gerekir. Bu giden güvenlik kuralı, sanal ağda yönetilen örneğiniz tarafından barındırılan SSSıSDB 'ye uygulanamaz. |
||||||||

### <a name="route"></a>Azure ExpressRoute veya Kullanıcı tanımlı yol kullanma
Şirket içi ağınızı Azure 'a genişletmek için sanal ağ altyapınıza bir [Azure ExpressRoute](https://azure.microsoft.com/services/expressroute/) bağlantı hattı bağladığınızda, ortak bir yapılandırma Zorlamalı tünel oluşturma (sanal ağa 0.0.0.0/0) ve bu da zorlanır İnceleme ve günlüğe kaydetme için sanal ağ akışından şirket içi ağ gereçine giden Internet trafiği. 
 
Veya Azure-SSIS IR 'yi barındıran alt ağdan giden Internet trafiğini, bir güvenlik duvarı veya İnceleme ve günlüğe kaydetme için Azure Güvenlik duvarı olarak bir sanal ağ gereci (NVA) barındıran başka bir alt ağa zorlamak için [Kullanıcı tanımlı yollar (UDRs)](../virtual-network/virtual-networks-udr-overview.md) tanımlayabilirsiniz.
 
Her iki durumda da trafik yolu, bağımlı Azure Data Factory hizmetlerinden (özel olarak Azure Batch Yönetim Hizmetleri), sanal ağda Azure-SSIS IR 'ye gerekli gelen bağlantı bağlantısını keser. 
 
Çözüm, Azure-SSIS IR 'yi içeren alt ağda bir (veya daha fazla) Kullanıcı tanımlı yol (UDRs) tanımlamak için kullanılır. 

- Azure ExpressRoute senaryosunda Azure-SSIS IR 'yi barındıran alt ağ üzerinde **Internet** olarak bir 0.0.0.0/0 yolu uygulayabilir veya sonraki atlama türünden var olan 0.0.0.0/0 yolunu, NVA 'daki **Internet** 'e **Sanal Gereç** olarak değiştirebilirsiniz Senaryon.

![Yol ekleme](media/join-azure-ssis-integration-runtime-virtual-network/add-route-for-vnet.png)
- Giden Internet trafiğini bu alt ağdan incelemeye yönelik bir sorun yaşıyorsanız. Yalnızca Azure Batch Yönetim Hizmetleri ve Azure-SSIS IR arasındaki trafiği, **Internet**olarak sonraki atlama türü ile yönlendirmek Için belirli udrs tanımlayabilirsiniz.
Örneğin Azure `UK South`-SSIS IR 'niz ' de bulunuyorsa, hizmet [etiketleri IP aralığı indirme bağlantısı](https://www.microsoft.com/en-us/download/details.aspx?id=56519) veya [hizmet etiketi bulma API 'si](https://aka.ms/discoveryapi)aracılığıyla hizmet etiketinin `BatchNodeManagement.UKSouth` IP aralığı listesini almanız gerekir. Ardından, sonraki atlama türü ile ilgili IP aralığı yolunun aşağıdaki UDRs 'sini **Internet**olarak uygulayın.

![AzureBatch UDR ayarları](media/join-azure-ssis-integration-runtime-virtual-network/azurebatch-udr-settings.png)
> [!NOTE]
> Bu yaklaşımın, Azure-SSIS IR 'nin kesilmesini önlemek için IP aralığını düzenli olarak kontrol etmeniz ve UDR 'nize yeni IP aralığı eklemeniz gereken ek bakım maliyeti vardır. Yeni IP, hizmet etiketinde göründüğünde yeni IP 'nin etkili olması için bir ay daha sürer. Bu nedenle, IP aralığını aylık olarak denetlemeniz önerilir. 

### <a name="resource-group"></a>Kaynak grubu gereksinimleri
-   Azure-SSIS IR 'nin, sanal ağla aynı kaynak grubunda belirli ağ kaynaklarını oluşturması gerekir. Bu kaynaklar şunları içerir:
    -   *GUID >-azurebatch-cloudserviceloaddengeleyici olan bir Azure yük dengeleyici. \<*
    -   *GUID >-azurebatch-cloudservicepublicıp adlı bir Azure genel IP adresi. \<*
    -   *GUID >-azurebatch-cloudservicenetworksecuritygroup adlı bir ağ iş güvenlik grubu. \<* 

    IR başlatıldığında, bu kaynaklar IR başlatıldığında oluşturulur ve silinir. Lütfen bunları diğer kaynaklarınızda yeniden kullanmayın, aksi takdirde IR durmasını engeller. 

-   Kaynak grubu veya sanal ağın ait olduğu abonelikte kaynak kilidi olmadığından emin olun. Salt bir okuma kilidi veya silme kilidi yapılandırırsanız, IR 'nin başlatılması ve durdurulması başarısız olabilir veya yanıt vermeyi durdurabilir. 

-   Aşağıdaki kaynakların, sanal ağın ait olduğu kaynak grubu veya abonelik altında oluşturulmasını engelleyen bir Azure ilkenizin olmadığından emin olun: 
    -   Microsoft. Network/LoadBalancers 
    -   Microsoft.Network/NetworkSecurityGroups 
    -   Microsoft. Network/Publicıpaddresses 

### <a name="faq"></a>SSS

- Azure-SSIS IR 'de kullanıma sunulan genel IP adreslerini gelen bağlantı için nasıl koruyacağız? Genel IP adresini kaldırmak mümkün mü?
 
    Şu anda, Azure-SSIS IR JOIN VNet olduğunda genel IP adresi otomatik olarak oluşturulacaktır. Yalnızca Azure Batch yönetim hizmetlerinin Azure-SSIS IR 'ye bağlanmasına izin vermek için NIC düzeyinde NSG, gelen koruma için de alt ağ düzeyi NSG 'yi belirtebilirsiniz.

    Genel IP adresinin gösterilmesini istemiyorsanız, senaryonuza uygulanmışsa VNet yerine [Azure-SSIS IR için bir proxy olarak şirket Içinde BARıNDıRıLAN IR yapılandırma](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) yaklaşımını düşünebilirsiniz.
 
- Azure-SSIS IR 'nin, veri kaynağına erişmek için güvenlik duvarı listesinin izin verilenler listesine koyabilen statik IP adresi var mı?
 
    - Veri kaynağınız şirket içinde ise, sanal ağı şirket içi ağınıza bağlayıp bu sanal ağ alt ağına Azure-SSIS IR 'nizi ekleyin ve bu alt ağın IP aralığını izin verilenler listesine koyabilirsiniz.
    - Veri kaynağınız, sanal ağ hizmeti uç noktası ile desteklenen bir Azure hizmeti ise, sanal ağınızda sanal ağ hizmet noktasını yapılandırma ve Azure-SSIS IR 'nizi bu sanal ağ alt ağına katma, ardından sanal ağ kuralını kullanabiliyor olabilirsiniz erişimine izin vermek için IP aralığı yerine Azure hizmetleri.
    - Veri kaynağınız diğer bulut veri kaynağı ise, Azure-SSIS IR 'den NVA 'ya veya statik genel IP adresiyle Azure Güvenlik Duvarı 'na giden trafiği yönlendirmek için UDR 'yi kullanabilirsiniz. bu sayede, NVA veya Azure Güvenlik duvarının genel IP adresini izin verilenler listesine koyabilirsiniz.
    - Yukarıdakilerin hiçbiri gereksiniminizi karşılamıyorsa, [Azure-SSIS IR için ara sunucu olarak şirket Içinde BARıNDıRıLAN IR 'Yi yapılandırma](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis)yoluyla veri kaynağı erişiminizin gerçekleştirilip yapılameyeceğini değerlendirebilir, daha sonra şirket IÇINDE barındırılan IR 'yi barındıran makinenin IP adresini Şunun yerine izin verilenler listesine koyabilirsiniz Azure-SSIS IR 'yi VNet 'e katma.

## <a name="azure-portal-data-factory-ui"></a>Azure portal (Data Factory UI)
Bu bölümde, Azure portal ve Data Factory Kullanıcı arabirimini kullanarak var olan bir Azure-SSIS çalışma zamanının bir sanal ağa (klasik veya Azure Resource Manager) nasıl ekleneceği gösterilmektedir. İlk olarak, Azure-SSIS IR 'nizi buna katılmadan önce sanal ağı uygun şekilde yapılandırmanız gerekir. Sanal ağınızın türüne (klasik veya Azure Resource Manager) göre sonraki iki bölümden birine gidin. Ardından, Azure-SSIS IR 'nizi sanal ağa katmak için üçüncü bölüme devam edin. 

### <a name="use-the-portal-to-configure-an-azure-resource-manager-virtual-network"></a>Azure Resource Manager sanal ağını yapılandırmak için portalı kullanma
Bir Azure-SSIS IR 'ye katılabilmeniz için önce bir sanal ağ yapılandırmanız gerekir. 

1. Microsoft Edge veya Google Chrome 'ı başlatın. Şu anda Data Factory Kullanıcı arabirimi yalnızca bu Web tarayıcılarında desteklenir. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

1. **Diğer hizmetler**' i seçin. **Sanal ağları**filtreleyin ve seçin. 

1. İçin filtre uygulayın ve listedeki Sanal ağınızı seçin. 

1. **Sanal ağ** sayfasında **Özellikler**' i seçin. 

1. Sanal ağ için kaynak KIMLIĞINI panoya kopyalamak üzere **kaynak kimliği** için Kopyala düğmesini seçin. KODU OneNote 'taki panodan veya bir dosyada kaydedin. 

1. Sol menüdeki **alt ağları** seçin. **Kullanılabilir adreslerin** sayısının Azure-SSIS Integration Runtime 'inizdeki düğümlerden büyük olduğundan emin olun. 

1. Azure Batch sağlayıcısının sanal ağa sahip Azure aboneliğine kayıtlı olduğunu doğrulayın. Veya Azure Batch sağlayıcıyı kaydedin. Aboneliğinizde zaten bir Azure Batch hesabınız varsa, aboneliğiniz Azure Batch için kaydedilir. (Data Factory portalında Azure-SSIS IR 'yi oluşturursanız, Azure Batch sağlayıcı sizin için otomatik olarak kaydedilir.) 

   a. Azure portal sol taraftaki menüden **abonelikler** ' i seçin. 

   b. Aboneliğinizi seçin. 

   c. Sol taraftaki **kaynak sağlayıcıları** ' nı seçin ve **Microsoft. Batch** ' in kayıtlı bir sağlayıcı olduğunu doğrulayın. 

   !["Kayıtlı" durumunun onayı](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Listede **Microsoft. Batch** ' i görmüyorsanız, kaydetmek için aboneliğinizde [boş bir Azure Batch hesabı oluşturun](../batch/batch-account-create-portal.md) . Daha sonra silebilirsiniz. 

### <a name="use-the-portal-to-configure-a-classic-virtual-network"></a>Klasik sanal ağı yapılandırmak için portalı kullanma
Bir Azure-SSIS IR 'ye katılabilmeniz için önce bir sanal ağ yapılandırmanız gerekir. 

1. Microsoft Edge veya Google Chrome 'ı başlatın. Şu anda Data Factory Kullanıcı arabirimi yalnızca bu Web tarayıcılarında desteklenir. 

1. [Azure Portal](https://portal.azure.com) oturum açın. 

1. **Diğer hizmetler**' i seçin. **Sanal ağlar (klasik)** için filtre uygulayın ve seçin. 

1. İçin filtre uygulayın ve listedeki Sanal ağınızı seçin. 

1. **Sanal ağ (klasik)** sayfasında **Özellikler**' i seçin. 

   ![Klasik sanal ağ kaynağı KIMLIĞI](media/join-azure-ssis-integration-runtime-virtual-network/classic-vnet-resource-id.png)

1. Klasik ağın kaynak KIMLIĞINI panoya kopyalamak için **kaynak kimliği** için Kopyala düğmesini seçin. KODU OneNote 'taki panodan veya bir dosyada kaydedin. 

1. Sol menüdeki **alt ağları** seçin. **Kullanılabilir adreslerin** sayısının Azure-SSIS Integration Runtime 'inizdeki düğümlerden büyük olduğundan emin olun. 

   ![Sanal ağdaki kullanılabilir adreslerin sayısı](media/join-azure-ssis-integration-runtime-virtual-network/number-of-available-addresses.png)

1. Sanal ağ için **MicrosoftAzureBatch** Ile **Klasik sanal makine katılımcısı** rolüne katın. 

    a. Sol menüde **erişim denetimi (IAM)** seçeneğini belirleyin ve **rol atamaları** sekmesini seçin. 

    !["Erişim denetimi" ve "Ekle" düğmeleri](media/join-azure-ssis-integration-runtime-virtual-network/access-control-add.png)

    b. Seçin **rol ataması Ekle**.

    c. **Rol ataması Ekle** sayfasında, **rol**Için **Klasik sanal makine katılımcısı** ' nı seçin. **Seçim** kutusuna **ddbf3205-c6bd-46AE-8127-60eb93363864** yapıştırın ve sonra arama sonuçları listesinden **Microsoft Azure Batch** ' yi seçin. 

    !["Rol ataması ekleme" sayfasında sonuçları ara](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-to-vm-contributor.png)

    d. Ayarları kaydetmek ve sayfayı kapatmak için **Kaydet** ' i seçin. 

    ![Erişim ayarlarını kaydet](media/join-azure-ssis-integration-runtime-virtual-network/save-access-settings.png)

    e. Katkıda bulunanlar listesinde **Microsoft Azure Batch** görtığınızdan emin olun. 

    ![Azure Batch erişimi onayla](media/join-azure-ssis-integration-runtime-virtual-network/azure-batch-in-list.png)

1. Azure Batch sağlayıcısının sanal ağa sahip Azure aboneliğine kayıtlı olduğunu doğrulayın. Veya Azure Batch sağlayıcıyı kaydedin. Aboneliğinizde zaten bir Azure Batch hesabınız varsa, aboneliğiniz Azure Batch için kaydedilir. (Data Factory portalında Azure-SSIS IR 'yi oluşturursanız, Azure Batch sağlayıcı sizin için otomatik olarak kaydedilir.) 

   a. Azure portal sol taraftaki menüden **abonelikler** ' i seçin. 

   b. Aboneliğinizi seçin. 

   c. Sol taraftaki **kaynak sağlayıcıları** ' nı seçin ve **Microsoft. Batch** ' in kayıtlı bir sağlayıcı olduğunu doğrulayın. 

   !["Kayıtlı" durumunun onayı](media/join-azure-ssis-integration-runtime-virtual-network/batch-registered-confirmation.png)

   Listede **Microsoft. Batch** ' i görmüyorsanız, kaydetmek için aboneliğinizde [boş bir Azure Batch hesabı oluşturun](../batch/batch-account-create-portal.md) . Daha sonra silebilirsiniz. 

### <a name="join-the-azure-ssis-ir-to-a-virtual-network"></a>Azure-SSIS IR 'yi bir sanal ağa ekleme
1. Microsoft Edge veya Google Chrome 'ı başlatın. Şu anda Data Factory Kullanıcı arabirimi yalnızca bu Web tarayıcılarında desteklenir. 

1. [Azure Portal](https://portal.azure.com)Sol menüdeki **veri fabrikaları** ' nı seçin. Menüde **veri fabrikaları** görmüyorsanız, **diğer hizmetler**' i ve **Intelligence + ANALYTICS** bölümünde **veri fabrikaları** Seç ' i seçin. 

   ![Veri fabrikaları listesi](media/join-azure-ssis-integration-runtime-virtual-network/data-factories-list.png)

1. Listede Azure-SSIS tümleştirme çalışma zamanı ile veri fabrikanızı seçin. Veri fabrikanızın giriş sayfasını görürsünüz. **& dağıtım** kutucuğunu seçin. Data Factory Kullanıcı arabirimini ayrı bir sekmede görürsünüz. 

   ![Data factory giriş sayfası](media/join-azure-ssis-integration-runtime-virtual-network/data-factory-home-page.png)

1. Data Factory Kullanıcı arabiriminde, **Düzenle** sekmesine geçin, **Bağlantılar**' ı seçin ve **tümleştirme çalışma zamanları** sekmesine geçin. 

   !["Tümleştirme çalışma zamanları" sekmesi](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtimes-tab.png)

1. Azure-SSIS IR 'niz çalışıyorsa, tümleştirme çalışma zamanı listesinde, Azure-SSIS IR 'nizin **Eylemler** sütunundaki **Durdur** düğmesini seçin. Bir IR 'yi durduruncaya kadar düzenleyemezsiniz. 

   ![IR 'yi durdur](media/join-azure-ssis-integration-runtime-virtual-network/stop-ir-button.png)

1. Tümleştirme çalışma zamanı listesinde, Azure-SSIS IR 'nizin **Eylemler** sütunundaki **Düzenle** düğmesini seçin. 

   ![Tümleştirme çalışma zamanını düzenleme](media/join-azure-ssis-integration-runtime-virtual-network/integration-runtime-edit.png)

1. **Integration Runtime kurulum** panelinde, **Ileri** düğmesine tıklayarak **Genel ayarlar** ve **SQL ayarları** sayfalarından ilerleyin. 

1. **Gelişmiş ayarlar** sayfasında, aşağıdaki işlemleri yapın: 

   a. **VNET seç...** onay kutusunu işaretleyin. 

   b. **Abonelik**için, altında var olan bir sanal ağı seçin ' ın altında Azure aboneliğinizi seçin. 
  
   c. **VNET adı**için Sanal ağınızı seçin. 

   d. **Alt ağ adı**için sanal ağda alt ağınızı seçin. 

   e. Azure-SSIS IR 'niz için şirket içinde barındırılan bir IR yapılandırmak/yönetmek istiyorsanız, şirket içinde barındırılan **...** onay kutusunu Işaretleyin ve [Azure-SSIS IR Için proxy olarak ŞIRKET içinde barındırılan IR 'yi yapılandırma](https://docs.microsoft.com/azure/data-factory/self-hosted-integration-runtime-proxy-ssis) makalesine bakın.

   f. **VNET doğrulama** düğmesine tıklayın ve başarılı olursa **İleri** düğmesine tıklayın. 

   ![IR kurulumu için Gelişmiş ayarlar](media/join-azure-ssis-integration-runtime-virtual-network/ir-setup-advanced-settings.png)

1. **Özet** sayfasında, Azure-SSIS IR 'nizin tüm ayarlarını gözden geçirin ve **Güncelleştir** düğmesine tıklayın.

1. Şimdi Azure-SSIS IR 'nizin **Eylemler** sütunundaki **Başlat** DÜĞMESINE tıklayarak Azure-SSIS IR 'nizi başlatabilirsiniz. Bir sanal ağa katılan Azure-SSIS IR 'nizi başlatmak yaklaşık 20 ila 30 dakika sürer. 

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="configure-a-virtual-network"></a>Sanal ağ yapılandırma
Azure-SSIS IR 'nizi buna katmadan önce bir sanal ağ yapılandırmanız gerekir. Azure-SSIS tümleştirme çalışma zamanı için sanal ağ izinlerini/ayarlarını sanal ağa katmak üzere otomatik olarak yapılandırmak için aşağıdaki betiği ekleyin:

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

### <a name="create-an-azure-ssis-ir-and-join-it-to-a-virtual-network"></a>Azure-SSIS IR oluşturma ve sanal ağa katma
Azure-SSIS IR oluşturabilir ve aynı anda bir sanal ağa katabilirsiniz. Tam betik ve yönergeler için bkz. [Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md#azure-powershell).

### <a name="join-an-existing-azure-ssis-ir-to-a-virtual-network"></a>Mevcut bir Azure-SSIS IR 'yi bir sanal ağa ekleme
[Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md) makalesindeki betik, Azure-SSIS IR oluşturma ve bu dosyayı aynı betikteki bir sanal ağa ekleme işlemini gösterir. Mevcut bir Azure-SSIS IR varsa, sanal ağa katmak için aşağıdaki adımları gerçekleştirin: 
1. Azure-SSIS IR 'yi durdurun. 
1. Azure-SSIS IR 'yi sanal ağa katmak üzere yapılandırın. 
1. Azure-SSIS IR 'yi başlatın. 

### <a name="define-the-variables"></a>Değişkenleri tanımlama
```powershell
$ResourceGroupName = "<your Azure resource group name>"
$DataFactoryName = "<your Data Factory name>" 
$AzureSSISName = "<your Azure-SSIS IR name>"
# Specify the information about your classic or Azure Resource Manager virtual network.
$VnetId = "<your Azure virtual network resource ID>"
$SubnetName = "<the name of subnet in your virtual network>"
```

### <a name="stop-the-azure-ssis-ir"></a>Azure-SSIS IR 'yi durdur
Azure-SSIS tümleştirme çalışma zamanını bir sanal ağa katılmadan önce durdurun. Bu komut tüm düğümlerini serbest bırakır ve faturalandırmayı durduruyor:

```powershell
Stop-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                            -DataFactoryName $DataFactoryName `
                                            -Name $AzureSSISName `
                                            -Force 
```

### <a name="configure-virtual-network-settings-for-the-azure-ssis-ir-to-join"></a>Azure-SSIS IR 'nin katılması için sanal ağ ayarlarını yapılandırma
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

### <a name="configure-the-azure-ssis-ir"></a>Azure-SSIS IR 'yi yapılandırma
Azure-SSIS tümleştirme çalışma zamanını sanal ağa katmak üzere yapılandırmak için `Set-AzDataFactoryV2IntegrationRuntime` komutunu çalıştırın: 

```powershell
Set-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                           -DataFactoryName $DataFactoryName `
                                           -Name $AzureSSISName `
                                           -Type Managed `
                                           -VnetId $VnetId `
                                           -Subnet $SubnetName
```

### <a name="start-the-azure-ssis-ir"></a>Azure-SSIS IR 'yi başlatma
Azure-SSIS tümleştirme çalışma zamanını başlatmak için aşağıdaki komutu çalıştırın: 

```powershell
Start-AzDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                             -DataFactoryName $DataFactoryName `
                                             -Name $AzureSSISName `
                                             -Force

```

Bu komutun tamamlanabilmesi için 20 ila 30 dakika sürer.

## <a name="next-steps"></a>Sonraki adımlar
Azure-SSIS Integration Runtime hakkında daha fazla bilgi için aşağıdaki konulara bakın: 
- [Azure-SSIS tümleştirme çalışma zamanı](concepts-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR dahil olmak üzere genel olarak tümleştirme çalışma zamanları hakkında kavramsal bilgiler sağlanmaktadır. 
- [Öğretici: SSIS paketlerini Azure’a dağıtma](tutorial-create-azure-ssis-runtime-portal.md). Bu öğretici, Azure-SSIS IR 'nizi oluşturmak için adım adım yönergeler sağlar. SSIS kataloğunu barındırmak için Azure SQL veritabanını kullanır. 
- [Azure-SSIS tümleştirme çalışma zamanı oluşturma](create-azure-ssis-integration-runtime.md). Bu makalede öğreticide, SSIS kataloğunu barındırmak ve Azure-SSIS IR 'nizi bir sanal ağa birleştirmek için sanal ağ hizmet uç noktaları/yönetilen örneği ile Azure SQL veritabanı kullanma hakkında yönergeler sağlanmaktadır. 
- [Azure-SSIS IR’yi izleme](monitor-integration-runtime.md#azure-ssis-integration-runtime). Bu makalede, Azure-SSIS IR 'niz hakkında bilgilerin nasıl alınacağını ve döndürülen bilgilerin durum açıklamalarını nasıl sağladığı gösterilmektedir. 
- [Azure-SSIS IR’yi yönetme](manage-azure-ssis-integration-runtime.md). Bu makalede, Azure-SSIS IR 'nizi durdurma, başlatma veya silme işlemlerinin nasıl yapılacağı gösterilir. Ayrıca, düğüm ekleyerek Azure-SSIS IR 'nizi nasıl ölçeklendirebilmeniz gerektiğini gösterir.
