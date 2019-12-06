---
title: Azure hizmet etiketlerine genel bakış
titlesuffix: Azure Virtual Network
description: Hizmet etiketleri hakkında bilgi edinin. Hizmet etiketleri, güvenlik kuralı oluşturma karmaşıklığını en aza indirmenize yardımcı olur.
services: virtual-network
documentationcenter: na
author: jispar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/22/2019
ms.author: jispar
ms.reviewer: kumud
ms.openlocfilehash: 152b9f3974f24644e55bed68f5ed65faa90d7fe7
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2019
ms.locfileid: "74851664"
---
# <a name="virtual-network-service-tags"></a>Sanal ağ hizmeti etiketleri 
<a name="network-service-tags"></a>

Hizmet etiketi, belirli bir Azure hizmetinden bir IP adresi önekleri grubunu temsil eder. Ağ güvenlik kurallarında sık sık güncelleştirmelerin karmaşıklığını en aza indirmenize yardımcı olur. [Ağ güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) veya [Azure Güvenlik duvarında](https://docs.microsoft.com/azure/firewall/service-tags)ağ erişim denetimleri tanımlamak için hizmet etiketlerini kullanabilirsiniz. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. Bir kuralın uygun *kaynak* veya *hedef* alanında hizmet etiketi adı (örn., **apimanaya**) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir. 

## <a name="available-service-tags"></a>Kullanılabilir hizmet etiketleri
Aşağıdaki tablo, [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) kurallarında kullanılabilecek tüm hizmet etiketlerini içerir.

Sütunlar etiketin şu şekilde olduğunu belirtir:

- Gelen veya giden trafiği kapsayan kurallar için uygundur
- Destek [bölge](https://azure.microsoft.com/regions) kapsamı 
- [Azure Güvenlik Duvarı](https://docs.microsoft.com/azure/firewall/service-tags) kurallarında kullanılabilir

Varsayılan olarak, hizmet etiketleri tüm bulutun aralıklarını yansıtır.  Bazı hizmet etiketleri, ilgili IP aralıklarını belirtilen bir bölgeye kısıtlayarak daha hassas denetime de olanak tanır.  Örneğin, hizmet etiketi **depolaması** tüm bulut Için Azure Storage 'ı temsil ettiğinde, **Storage. WestUS** , yalnızca WestUS bölgesindeki depolama IP adresi aralıklarına göre daraltır.  Aşağıdaki her bir hizmet etiketinin açıklamaları, bu tür bölgesel kapsamları destekleyip desteklemediğini belirtir.  



| Etiket | Amaç | Gelen veya giden trafiği kullanabilir miyim? | Bölgesel olabilir mi? | Azure Güvenlik Duvarı ile kullanılabilir mi? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ApiManagement** | APıM adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **AppService**    | App Service hizmet. Bu etiket, WebApp ön uçları için giden güvenlik kuralları için önerilir. | Giden | Yes | Yes |
| **AppServiceManagement** | App Service Ortamı adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **AzureActiveDirectory** | Azure Active Directory hizmet. | Giden | Hayır | Yes |
| **AzureActiveDirectoryDomainServices** | Azure Active Directory Domain Services adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **AzureBackup** |Azure Backup hizmet.<br/><br/>*Note:* Bu etiketin **depolama** ve **AzureActiveDirectory** etiketlerine bir bağımlılığı vardır. | Giden | Hayır | Yes |
| **AzureCloud** | Tüm [veri merkezi genel IP adresleri](https://www.microsoft.com/download/details.aspx?id=41653). | Giden | Yes | Yes |
| **AzureConnectors** | Araştırma/arka uç bağlantıları için bağlayıcılar Logic Apps. | Gelen | Yes | Yes |
| **AzureContainerRegistry** | Azure Container Registry hizmet. | Giden | Yes | Yes |
| **Azu, Smosdb** | Azure Cosmos veritabanı hizmeti. | Giden | Yes | Yes |
| **AzureDataLake** | Azure Data Lake hizmet. | Giden | Hayır | Yes |
| **AzureHDInsight** | Azure HDInsight hizmeti. | Gelen | Yes | Hayır |
| **AzureIoTHub** | Azure IoT Hub hizmeti. | Giden | Hayır | Hayır |
| **AzureKeyVault** | Azure Keykasa hizmeti.<br/><br/>*Note:* Bu etiketin **AzureActiveDirectory** etiketine bağımlılığı vardır. | Giden | Yes | Yes |
| **AzureLoadBalancer** | Azure 'un altyapı yük dengeleyici. Bu etiket, Azure’ın sistem durumu araştırmalarının kaynağı olan [Ana bilgisayar sanal IP adresine](security-overview.md#azure-platform-considerations) (168.63.129.16) çevrilir. Azure yük dengeleyiciyi kullanmıyorsanız bu kuralı geçersiz kılabilirsiniz. | Her ikisi de | Hayır | Hayır |
| **AzureMachineLearning** | Azure Machine Learning hizmet. | Giden | Hayır | Yes |
| **AzureMonitor** | Log Analytics, App Insights, AzMon ve özel ölçümler (GB uç noktaları).<br/><br/>*Note:* Log Analytics için, bu etiketin **depolama** etiketine bağımlılığı vardır. | Giden | Hayır | Yes |
| **AzurePlatformDNS** | Temel altyapı (varsayılan) DNS hizmeti.<br/><br>Varsayılan DNS 'yi devre dışı bırakmak için bu etiketi kullanabilirsiniz. Lütfen bu etiketi kullanırken dikkatli olun. [Azure platformu ile ilgili önemli noktaları](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) okumanız önerilir. Bu etiket kullanılmadan önce test edilmesi önerilir. | Giden | Hayır | Hayır |
| **AzurePlatformIMDS** | Temel bir altyapı hizmeti olan ıMDS 'ler.<br/><br/>Bu etiketi, varsayılan ıDS 'yi devre dışı bırakmak için kullanabilirsiniz.  Lütfen bu etiketi kullanırken dikkatli olun. [Azure platformu ile ilgili önemli noktaları](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) okumanız önerilir. Bu etiket kullanılmadan önce test edilmesi önerilir. | Giden | Hayır | Hayır |
| **AzurePlatformLKM** | Windows lisanslama veya anahtar yönetimi hizmeti.<br/><br/>Lisanslama için Varsayılanları devre dışı bırakmak üzere bu etiketi kullanabilirsiniz. Lütfen bu etiketi kullanırken dikkatli olun.  [Azure platformu ile ilgili önemli noktaları](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) okumanız önerilir. Bu etiket kullanılmadan önce test edilmesi önerilir. | Giden | Hayır | Hayır |
| **AzureTrafficManaged** | Azure Traffic Manager araştırması IP adresleri.<br/><br/>Traffic Manager yoklama IP adresleri hakkında daha fazla bilgi için [Azure Traffic Manager hakkında SSS](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs) sayfasına göz atın. | Gelen | Hayır | Yes |  
| **BatchNodeManagement** | Azure Batch adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **Biliveservicesmanagement** | Bilişsel hizmetler için trafiğin adres aralıkları | Giden | Hayır | Hayır |
| **Dynamics365ForMarketingEmail** | Dynamics 365 pazarlama e-posta hizmeti için adres aralıkları. | Giden | Yes | Hayır |
| **EventHub** | Azure EventHub hizmeti. | Giden | Yes | Yes |
| **GatewayManager** | VPN/uygulama ağ geçitleri adanmış dağıtımlar için yönetim trafiği. | Gelen | Hayır | Hayır |
| **Internet** | Sanal ağın dışında olan ve genel Internet tarafından erişilebilen IP adresi alanı.<br/><br/>Adres aralığı [Azure'a ait genel IP adresi alanını](https://www.microsoft.com/download/details.aspx?id=41653) içerir. | Her ikisi de | Hayır | Hayır |
| **MicrosoftContainerRegistry** | Microsoft Container Registry hizmeti. | Giden | Yes | Yes |
| **ServiceBus** | Premium hizmet katmanını kullanarak hizmeti Azure Service Bus. | Giden | Yes | Yes |
| **ServiceFabric** | Service Fabric hizmet. | Giden | Hayır | Hayır |
| **SQL** | Azure SQL veritabanı, MySQL için Azure veritabanı, PostgreSQL için Azure veritabanı ve Azure SQL veri ambarı Hizmetleri.<br/><br/>*Note:* Bu etiket hizmetin belirli örneklerini değil hizmeti temsil eder. Örneğin etiket belirli bir SQL veritabanını veya sunucusunu değil Azure SQL Veritabanı hizmetini temsil eder. | Giden | Yes | Yes |
| **SqlManagement** | SQL adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **Depolama** | Azure depolama hizmeti. <br/><br/>*Note:* Etiket hizmeti temsil eder, ancak hizmetin belirli örneklerini kullanmaz. Örneğin etiket belirli bir Azure Depolama hesabını değil Azure Depolama hizmetini temsil eder. | Giden | Yes | Yes |
| **VirtualNetwork** | Sanal ağ adres alanı (sanal ağ için tanımlanan tüm IP adresi aralıkları), bağlı olan tüm şirket içi adres alanları, [eşlenen sanal ağlar](virtual-network-peering-overview.md) veya bir sanal ağ [geçidine](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json)bağlı sanal ağ, [Kullanıcı tanımlı yollarla](virtual-networks-udr-overview.md)kullanılan konak ve adres öneklerinin [sanal IP adresidir](security-overview.md#azure-platform-considerations) . Bu etiketin de varsayılan yollar içerebileceğini unutmayın. | Her ikisi de | Hayır | Hayır |

>[!NOTE]
>*Klasik* (Azure Resource Manager öncesi) ortamda çalışırken Yukarıdaki etiketlerin seçim kümesi desteklenir.  Bu, alternatif bir yazım kullanır:

| Klasik yazım | Eşdeğer Kaynak Yöneticisi etiketi |
|---|---|
| AZURE_LOADBALANCER | AzureLoadBalancer |
| INTERNET | Internet |
| VIRTUAL_NETWORK | VirtualNetwork |

> [!NOTE]
> Azure hizmetlerinin hizmet etiketleri, kullanılan belirli buluttan gelen adres öneklerini gösterir. Örneğin, **SQL** etiketi değerine karşılık gelen temel IP aralıkları, Azure genel bulutu ve Azure Çin bulutu arasında farklılık gösterir.

> [!NOTE]
> Azure Depolama veya Azure SQL Veritabanı gibi bir hizmet için bir [sanal ağ hizmet uç noktası](virtual-network-service-endpoints-overview.md) uygularsanız Azure, sanal ağ alt ağına hizmet için bir [rota](virtual-networks-udr-overview.md#optional-default-routes) ekler. Rotada adres ön ekleri ilgili hizmet etiketiyle aynı adres ön ekleri veya CIDR aralıklarıdır.



## <a name="service-tags-in-on-premises"></a>Şirket içinde hizmet etiketleri  
Şirket içi güvenlik duvarı yapılandırmalarının bir parçası olarak dahil edilecek geçerli hizmet etiketi ve Aralık bilgilerini elde edebilirsiniz.  Bu bilgiler, her bir hizmet etiketine karşılık gelen IP aralıklarının geçerli zaman noktası listesidir.  Bilgiler program aracılığıyla veya JSON dosya indirme aracılığıyla aşağıdaki şekilde alınabilir.

### <a name="service-tag-discovery-api-public-preview"></a>Hizmet etiketi bulma API 'SI (Genel Önizleme)
IP adresi aralığı ayrıntıları ile geçerli hizmet etiketleri listesini Program aracılığıyla alabilirsiniz:

- [REST](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [Azure CLI](https://docs.microsoft.com/cli/azure/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> Genel önizlemede, bulma API 'si JSON İndirmeleri (aşağıda) olarak geçerli olmayan bilgileri döndürebilir.


### <a name="discover-service-tags-using-downloadable-json-files"></a>İndirilebilir JSON dosyalarını kullanarak hizmet etiketlerini bulma 
Geçerli hizmet etiketi listesini içeren JSON dosyalarını, IP adresi aralığı ayrıntıları ile indirebilirsiniz. Bunlar haftalık olarak güncelleştirilir ve yayımlanır.  Her bulutun konumları şunlardır:

- [Azure genel](https://www.microsoft.com/download/details.aspx?id=56519)
- [Azure Devlet Kurumları](https://www.microsoft.com/download/details.aspx?id=57063)  
- [Azure Çin](https://www.microsoft.com/download/details.aspx?id=57062) 
- [Azure Almanya](https://www.microsoft.com/download/details.aspx?id=57064)   

> [!NOTE]
>Bu bilgilerin bir alt kümesi daha önce [Azure genel](https://www.microsoft.com/download/details.aspx?id=41653), [Azure Çin](https://www.microsoft.com/download/details.aspx?id=42064) ve [Azure Almanya](https://www.microsoft.com/download/details.aspx?id=54770)için XML dosyalarında yayımlanmıştır. Bu XML İndirmeleri 30 Haziran 2020 ' den kullanım dışı kalacak ve bu tarihten sonra kullanılamayacak. Lütfen yukarıdaki bulma API 'sini veya JSON dosya indirmelerini kullanarak yukarıda açıklanan şekilde geçiş yapın.

### <a name="tips"></a>İpuçları 
- Bir yayındaki güncelleştirmeleri, JSON dosyasındaki artan *ChangeNumber* değerlerini kullanarak tespit edebilirsiniz. Her alt bölüm (örn. **Storage. WestUS**), değişiklik gerçekleştiğinde arttırılan kendi *ChangeNumber* değerini içerir.  Alt bölümleri değiştiğinde dosyanın *ChangeNumber* en üst düzeyi artırılır.
- Hizmet etiketi bilgilerini ayrıştırmayla ilgili örnekler için (örn. WestUS ' de depolama için tüm adres aralıklarını alma), lütfen [hizmet etiketi bulma API PowerShell](https://aka.ms/discoveryapi_powershell) belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Ağ güvenlik grubu oluşturmayı](tutorial-filter-network-traffic.md) öğrenin.

