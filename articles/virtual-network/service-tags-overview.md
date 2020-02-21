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
ms.openlocfilehash: c93460797fc1cf953d467e2739d71c5a2a9b74ed
ms.sourcegitcommit: 98a5a6765da081e7f294d3cb19c1357d10ca333f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2020
ms.locfileid: "77486177"
---
# <a name="virtual-network-service-tags"></a>Sanal ağ hizmeti etiketleri 
<a name="network-service-tags"></a>

Hizmet etiketi, belirli bir Azure hizmetinden bir IP adresi önekleri grubunu temsil eder. Microsoft, hizmet etiketi ile çevrelenmiş adres öneklerini yönetir ve adres değişikliği olarak hizmet etiketini otomatik olarak güncelleştirir ve ağ güvenlik kuralları için sık sık güncelleştirmelerin karmaşıklığını en aza indirir. 

Hizmet etiketlerini, [ağ güvenlik grupları](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) veya [Azure Güvenlik Duvarı](https://docs.microsoft.com/azure/firewall/service-tags)'nda ağ erişim denetimleri tanımlamak için kullanabilirsiniz. Güvenlik kuralları oluştururken belirli IP adreslerinin yerine hizmet etiketleri kullanın. Bir kuralın uygun *kaynak* veya *hedef* alanındaki hizmet etiketi adını (örneğin, **apimanaya**) belirterek, ilgili hizmet için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. 

Hizmet etiketlerini kullanarak, genel uç noktalarından oluşan Azure hizmetlerine erişirken ağ yalıtımı elde edebilir ve Azure kaynaklarınızı genel Internet üzerinden koruyabilirsiniz. **Internet** **'ten gelen** /giden trafiği reddetmek ve belirli Azure hizmetlerinin diğer [kullanılabilir hizmet etiketlerine](#available-service-tags) /sayısına giden trafiğe izin vermek için gelen/giden ağ güvenlik grubu kuralları oluşturun. 

## <a name="available-service-tags"></a>Kullanılabilir hizmet etiketleri
Aşağıdaki tablo, [ağ güvenlik grubu](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) kurallarında kullanılabilecek tüm hizmet etiketlerini içerir.

Sütunlar, etiketin şu şekilde olduğunu belirtir:

- Gelen veya giden trafiği kapsayan kurallar için uygundur.
- [Bölgesel](https://azure.microsoft.com/regions) kapsamı destekler.
- [Azure Güvenlik Duvarı](https://docs.microsoft.com/azure/firewall/service-tags) kurallarında kullanılabilir.

Varsayılan olarak, hizmet etiketleri tüm bulutun aralıklarını yansıtır. Bazı hizmet etiketleri, ilgili IP aralıklarını belirtilen bir bölgeye kısıtlayarak daha ayrıntılı denetime de olanak tanır. Örneğin, hizmet etiketi **depolaması** tüm bulut Için Azure Storage 'ı temsil eder, ancak **Storage. WestUS** aralığı yalnızca WestUS bölgesindeki depolama IP adresi aralıklarına göre daraltır. Aşağıdaki tablo, her bir hizmet etiketinin bu tür bölgesel kapsamları destekleyip desteklemediğini gösterir.  

| Etiket | Amaç | Gelen veya giden trafiği kullanabilir miyim? | Bölgesel olabilir mi? | Azure Güvenlik Duvarı ile kullanılabilir mi? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **ApiManagement** | Azure API Management adanmış dağıtımlar için yönetim trafiği. <br/><br/>*Note:* Bu etiket, bölge başına denetim düzlemi için Azure API Management hizmet uç noktasını temsil eder. Bu, müşterilerin API Management hizmetinde yapılandırılmış API 'Ler, Işlemler, Ilkeler, NamedValues üzerinde yönetim işlemleri gerçekleştirmesini sağlar.  | Gelen | Yes | Yes |
| **Applicationınsi, Savailability** | Kullanılabilirliği Application Insights. | Gelen | Hayır | Hayır |
| **AppService**    | Azure uygulama hizmeti. Bu etiket, Web uygulaması ön uçları için giden güvenlik kuralları için önerilir. | Giden | Yes | Yes |
| **AppServiceManagement** | App Service Ortamı adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **AzureActiveDirectory** | Azure Active Directory. | Giden | Hayır | Yes |
| **AzureActiveDirectoryDomainServices** | Azure Active Directory Domain Services adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **AzureAdvancedThreatProtection** | Azure Gelişmiş tehdit koruması. | Giden | Hayır | Hayır |
| **AzureBackup** |Azure Backup.<br/><br/>*Note:* Bu etiketin **depolama** ve **AzureActiveDirectory** etiketlerine bir bağımlılığı vardır. | Giden | Hayır | Yes |
| **AzureBotService** | Azure bot hizmeti. | Giden | Hayır | Hayır |
| **AzureCloud** | Tüm [veri merkezi genel IP adresleri](https://www.microsoft.com/download/details.aspx?id=56519). | Giden | Yes | Yes |
| **Azurecognivesearch** | Azure Bilişsel Arama. <br/><br/>Bu etiket veya bu etiketin kapsadığı IP adresleri, dizin oluşturucularının veri kaynaklarına güvenli erişimini sağlamak için kullanılabilir. Daha fazla ayrıntı için [Dizin Oluşturucu bağlantı belgelerine](https://docs.microsoft.com/azure/search/search-indexer-troubleshooting#connection-errors) bakın. | Gelen | Hayır | Hayır |
| **AzureConnectors** | Araştırma/arka uç bağlantıları için bağlayıcılar Azure Logic Apps. | Gelen | Yes | Yes |
| **AzureContainerRegistry** | Azure Container Registry. | Giden | Yes | Yes |
| **Azu, Smosdb** | Azure Cosmos DB. | Giden | Yes | Yes |
| **AzureDatabricks** | Azure Databricks. | Her ikisi de | Hayır | Hayır |
| **AzureDataExplorerManagement** | Azure Veri Gezgini yönetimi. | Gelen | Hayır | Hayır |
| **AzureDataLake** | Azure Data Lake. | Giden | Hayır | Yes |
| **AzureEventGrid** | Azure Event Grid. <br/><br/>*Note:* Bu etiket yalnızca ABD Orta Güney, ABD Doğu, ABD Doğu 2, ABD Batı 2 ve ABD Orta Azure Event Grid uç noktaları içerir. | Her ikisi de | Hayır | Hayır |
| **Azurefrontkapısı** | Azure ön kapısı. | Her ikisi de | Hayır | Hayır |
| **Azureınformationprotection** | Azure Information Protection.<br/><br/>*Note:* Bu etiketin **AzureActiveDirectory** ve **Azurefrontkapısı. ön uç** etiketlerine bağımlılığı vardır. Ayrıca, aşağıdaki IP 'Leri de beyaz listeye girin (Bu bağımlılık yakında kaldırılacak): 13.107.6.181 & 13.107.9.181. | Giden | Hayır | Hayır |
| **AzureIoTHub** | Azure IoT Hub. | Giden | Hayır | Hayır |
| **AzureKeyVault** | Azure Key Vault.<br/><br/>*Note:* Bu etiketin **AzureActiveDirectory** etiketine bağımlılığı vardır. | Giden | Yes | Yes |
| **AzureLoadBalancer** | Azure altyapı yük dengeleyici. Etiketi, Azure sistem durumu araştırmalarının gerçekleştiği [konağın sanal IP adresine](security-overview.md#azure-platform-considerations) (168.63.129.16) çevirir. Bu, Azure Load Balancer kaynağına giden trafiği içermez. Azure Load Balancer kullanmıyorsanız, bu kuralı geçersiz kılabilirsiniz. | Her ikisi de | Hayır | Hayır |
| **AzureMachineLearning** | Azure Machine Learning. | Her ikisi de | Hayır | Yes |
| **AzureMonitor** | Log Analytics, Application Insights, AzMon ve özel ölçümler (GB uç noktaları).<br/><br/>*Note:* Log Analytics için, bu etiketin **depolama** etiketine bağımlılığı vardır. | Giden | Hayır | Yes |
| **AzurePlatformDNS** | Temel altyapı (varsayılan) DNS hizmeti.<br/><br>Varsayılan DNS 'yi devre dışı bırakmak için bu etiketi kullanabilirsiniz. Bu etiketi kullanırken dikkatli olun. [Azure platformu konularını](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations)okumanızı öneririz. Ayrıca, bu etiketi kullanmadan önce test gerçekleştirmenizi öneririz. | Giden | Hayır | Hayır |
| **AzurePlatformIMDS** | Temel bir altyapı hizmeti olan Azure Instance Metadata Service (ıMDS).<br/><br/>Bu etiketi, varsayılan ıDS 'yi devre dışı bırakmak için kullanabilirsiniz. Bu etiketi kullanırken dikkatli olun. [Azure platformu konularını](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations)okumanızı öneririz. Ayrıca, bu etiketi kullanmadan önce test gerçekleştirmenizi öneririz. | Giden | Hayır | Hayır |
| **AzurePlatformLKM** | Windows lisanslama veya anahtar yönetimi hizmeti.<br/><br/>Lisanslama için Varsayılanları devre dışı bırakmak üzere bu etiketi kullanabilirsiniz. Bu etiketi kullanırken dikkatli olun. [Azure platformu konularını](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations)okumanızı öneririz.  Ayrıca, bu etiketi kullanmadan önce test gerçekleştirmenizi öneririz. | Giden | Hayır | Hayır |
| **AzureResourceManager** | Azure Resource Manager. | Giden | Hayır | Hayır |
| **Azuresterecovery** | Azure Site Recovery.<br/><br/>*Note:* Bu etiketin **depolama**, **AzureActiveDirectory**ve **EventHub** etiketlerine bağımlılığı vardır. | Giden | Hayır | Hayır |
| **AzureTrafficManager** | Azure Traffic Manager araştırması IP adresleri.<br/><br/>Traffic Manager araştırma IP adresleri hakkında daha fazla bilgi için bkz. [Azure TRAFFIC Manager SSS](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs). | Gelen | Hayır | Yes |  
| **BatchNodeManagement** | Azure Batch adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **Biliveservicesmanagement** | Azure bilişsel hizmetler için trafik için adres aralıkları. | Giden | Hayır | Hayır |
| **Dynamics365ForMarketingEmail** | Dynamics 365 pazarlama e-posta hizmeti için adres aralıkları. | Giden | Yes | Hayır |
| **ElasticAFD** | Elastik Azure ön kapısı. | Her ikisi de | Hayır | Hayır |
| **EventHub** | Azure Event Hubs. | Giden | Yes | Yes |
| **GatewayManager** | Azure VPN Gateway ve Application Gateway adanmış dağıtımlar için yönetim trafiği. | Gelen | Hayır | Hayır |
| **GuestAndHybridManagement** | Azure Otomasyonu ve konuk yapılandırması. | Giden | Hayır | Yes |
| **HDInsight** | Azure HDInsight. | Gelen | Yes | Hayır |
| **Internet** | Sanal ağın dışında olan ve genel İnternet tarafından erişilebilen IP adresi alanı.<br/><br/>Adres aralığı, [Azure 'a ait genel IP adresi alanını](https://www.microsoft.com/download/details.aspx?id=41653)içerir. | Her ikisi de | Hayır | Hayır |
| **MicrosoftCloudAppSecurity** | Microsoft Cloud App Security. | Giden | Hayır | Hayır |
| **MicrosoftContainerRegistry** | Microsoft Container görüntüleri için kapsayıcı kayıt defteri. <br/><br/>*Note:* Lütfen aşağıdaki IP 'yi de beyaz listeye ekleyin (Bu bağımlılık yakında kaldırılacak): 204.79.197.219. | Giden | Yes | Yes |
| **ServiceBus** | Premium hizmet katmanını kullanan trafiği Azure Service Bus. | Giden | Yes | Yes |
| **ServiceFabric** | Azure Service Fabric.<br/><br/>*Note:* Bu etiket, bölge başına denetim düzlemi için Service Fabric hizmet uç noktasını temsil eder. Bu, müşterilerin VNET 'lerden Service Fabric kümeleri için yönetim işlemleri gerçekleştirmesini sağlar (uç nokta örn. https://westus.servicefabric.azure.com) | Her ikisi de | Hayır | Hayır |
| **Sql** | Azure SQL veritabanı, MySQL için Azure veritabanı, PostgreSQL için Azure veritabanı ve Azure SQL veri ambarı.<br/><br/>*Note:* Bu etiket hizmetin belirli örneklerini değil hizmeti temsil eder. Örneğin etiket belirli bir SQL veritabanını veya sunucusunu değil Azure SQL Veritabanı hizmetini temsil eder. | Giden | Yes | Yes |
| **SqlManagement** | SQL adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Yes |
| **Depolama** | Azure depolama. <br/><br/>*Note:* Bu etiket hizmetin belirli örneklerini değil hizmeti temsil eder. Örneğin etiket belirli bir Azure Depolama hesabını değil Azure Depolama hizmetini temsil eder. | Giden | Yes | Yes |
| **VirtualNetwork** | Sanal ağ adres alanı (sanal ağ için tanımlanan tüm IP adresi aralıkları), bağlı olan tüm şirket içi adres [alanları, eşlenen sanal ağlar](virtual-network-peering-overview.md) , [sanal ağ geçidine](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json)bağlı sanal ağlar, [ana bilgisayarın sanal IP adresi](security-overview.md#azure-platform-considerations)ve [Kullanıcı tanımlı yollar](virtual-networks-udr-overview.md)üzerinde kullanılan adres önekleri. Bu etiket varsayılan yollar da içerebilir. | Her ikisi de | Hayır | Hayır |

>[!NOTE]
>Klasik dağıtım modelinde (Azure Resource Manager önce), önceki tabloda listelenen etiketlerin bir alt kümesi desteklenir. Bu Etiketler farklı şekilde yazılmış:
>
>| Klasik yazım | Eşdeğer Kaynak Yöneticisi etiketi |
>|---|---|
>| AZURE_LOADBALANCER | AzureLoadBalancer |
>| INTERNET | Internet |
>| VIRTUAL_NETWORK | VirtualNetwork |

> [!NOTE]
> Azure hizmetlerinin hizmet etiketleri, kullanılan belirli buluttan gelen adres öneklerini gösterir. Örneğin, Azure genel bulutundaki **SQL** etiketi değerine karşılık gelen temel IP aralıkları, Azure Çin bulutundaki temel aralıklardan farklı olacaktır.

> [!NOTE]
> Azure Depolama veya Azure SQL Veritabanı gibi bir hizmet için bir [sanal ağ hizmet uç noktası](virtual-network-service-endpoints-overview.md) uygularsanız Azure, sanal ağ alt ağına hizmet için bir [rota](virtual-networks-udr-overview.md#optional-default-routes) ekler. Rotadaki adres önekleri, karşılık gelen hizmet etiketiyle aynı adres ön ekleri veya CıDR aralıklarıdır.

## <a name="service-tags-on-premises"></a>Şirket içi hizmet etiketleri  
Şirket içi güvenlik duvarı yapılandırmalarının bir parçası olarak dahil edilecek geçerli hizmet etiketi ve Aralık bilgilerini elde edebilirsiniz. Bu bilgiler, her bir hizmet etiketine karşılık gelen IP aralıklarının geçerli zaman noktası listesidir. Aşağıdaki bölümlerde açıklandığı gibi, bilgileri programlı bir şekilde veya bir JSON dosya indirme aracılığıyla elde edebilirsiniz.

### <a name="use-the-service-tag-discovery-api-public-preview"></a>Hizmet etiketi bulma API 'sini (Genel Önizleme) kullanma
Hizmet etiketlerinin geçerli listesini, IP adres aralığı ayrıntıları ile birlikte program aracılığıyla alabilirsiniz:

- [REST](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [Azure CLI](https://docs.microsoft.com/cli/azure/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> Ortak önizlemedeyken, bulma API 'si JSON İndirmeleri tarafından döndürülen bilgilerden daha az güncel bilgiler döndürebilir. (Sonraki bölüme bakın.)


### <a name="discover-service-tags-by-using-downloadable-json-files"></a>İndirilebilir JSON dosyalarını kullanarak hizmet etiketlerini bulma 
Hizmet etiketlerinin geçerli listesini içeren JSON dosyalarını, IP adresi aralığı ayrıntıları ile birlikte indirebilirsiniz. Bu listeler haftalık olarak güncelleştirilir ve yayımlanır. Her bulutun konumları şunlardır:

- [Azure genel](https://www.microsoft.com/download/details.aspx?id=56519)
- [Azure US Government](https://www.microsoft.com/download/details.aspx?id=57063)  
- [Azure Çin](https://www.microsoft.com/download/details.aspx?id=57062) 
- [Azure Almanya](https://www.microsoft.com/download/details.aspx?id=57064)   

> [!NOTE]
>Bu bilgilerin bir alt kümesi, [Azure genel](https://www.microsoft.com/download/details.aspx?id=41653), [Azure Çin](https://www.microsoft.com/download/details.aspx?id=42064)ve [Azure Almanya](https://www.microsoft.com/download/details.aspx?id=54770)için XML dosyalarında yayımlanmıştır. Bu XML İndirmeleri 30 Haziran 2020 ' den kullanım dışı kalacak ve bu tarihten sonra kullanılamayacak. Önceki bölümlerde açıklandığı şekilde bulma API 'sini veya JSON dosya indirmelerini kullanarak geçiş yapmalısınız.

### <a name="tips"></a>İpuçları 
- JSON dosyasındaki daha fazla *ChangeNumber* değerini belirterek bir yayından sonraki güncelleştirmeleri tespit edebilirsiniz. Her alt bölüm (örneğin, **Storage. WestUS**), değişiklik gerçekleştiğinde arttırılan kendi *ChangeNumber* değerini içerir. Alt bölümleri değiştirildiğinde dosyanın *ChangeNumber* en üst düzeyi artırılır.
- Hizmet etiketi bilgilerini ayrıştırmayla ilgili örnekler için (örneğin, WestUS ' de depolama için tüm adres aralıklarını alma), [hizmet etiketi bulma API PowerShell](https://aka.ms/discoveryapi_powershell) belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Ağ güvenlik grubu oluşturmayı](tutorial-filter-network-traffic.md)öğrenin.
