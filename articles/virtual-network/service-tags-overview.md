---
title: Azure hizmet etiketleriyle genel bakış
titlesuffix: Azure Virtual Network
description: Hizmet etiketleri hakkında bilgi edinin. Hizmet etiketleri, güvenlik kuralı oluşturma nın karmaşıklığını en aza indirmeye yardımcı olur.
services: virtual-network
documentationcenter: na
author: jispar
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/12/2020
ms.author: jispar
ms.reviewer: kumud
ms.openlocfilehash: 568fc880711d42941fd9aef2ea19b8ac3123793a
ms.sourcegitcommit: e040ab443f10e975954d41def759b1e9d96cdade
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2020
ms.locfileid: "80384254"
---
# <a name="virtual-network-service-tags"></a>Sanal ağ hizmeti etiketleri
<a name="network-service-tags"></a>

Hizmet etiketi, belirli bir Azure hizmetinin IP adresi önekleri grubunu temsil eder. Microsoft, hizmet etiketinin kapsadığı adres önekleri yönetir ve adresler değiştikçe servis etiketini otomatik olarak günceller ve sık sık ağ güvenliği kurallarına yönelik güncelleştirmelerin karmaşıklığını en aza indirir.

[Ağ güvenlik gruplarında](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) veya [Azure Güvenlik Duvarı'nda](https://docs.microsoft.com/azure/firewall/service-tags)ağ erişim denetimlerini tanımlamak için hizmet etiketlerini kullanabilirsiniz. Güvenlik kuralları oluştururken belirli IP adresleri yerine hizmet etiketlerini kullanın. Bir kuralın uygun *kaynak* veya *hedef* alanında servis etiketi adını (örneğin **ApiManagement)** belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz.

Ortak uç noktaları olan Azure hizmetlerine erişirken ağ yalıtımı elde etmek ve Azure kaynaklarınızı genel Internet'ten korumak için hizmet etiketlerini kullanabilirsiniz. Internet'e/Internet'ten gelen trafiği reddetmek ve **Internet** **AzureCloud'a** veya belirli Azure hizmetlerinin diğer [kullanılabilir hizmet etiketlerine/trafiğine](#available-service-tags) izin vermek için gelen/giden ağ güvenliği grubu kuralları oluşturun.

## <a name="available-service-tags"></a>Kullanılabilir hizmet etiketleri
Aşağıdaki tablo, [ağ güvenliği grubu](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules) kurallarında kullanılmak üzere kullanılabilen tüm hizmet etiketlerini içerir.

Sütunlar etiketin şu şekilde olduğunu gösterir:

- Gelen veya giden trafiği kapsayan kurallar için uygundur.
- [Bölgesel](https://azure.microsoft.com/regions) kapsamı destekler.
- Azure Güvenlik [Duvarı](https://docs.microsoft.com/azure/firewall/service-tags) kurallarında kullanılabilir.

Varsayılan olarak, hizmet etiketleri tüm bulutun aralıklarını yansıtır. Bazı hizmet etiketleri, ilgili IP aralıklarını belirli bir bölgeyle sınırlandırarak daha ayrıntılı denetime de izin verir. Örneğin, hizmet etiketi **Depolama** tüm bulut için Azure Depolama'yı temsil eder, ancak **Storage.WestUS** aralığı yalnızca WestUS bölgesinden gelen depolama IP adresi aralıklarına daraltır. Aşağıdaki tablo, her hizmet etiketinin bu tür bölgesel kapsamı destekleyip desteklemediğini gösterir.  

| Etiket | Amaç | Gelen veya giden kullanabilir miyim? | Bölgesel olabilir mi? | Azure Güvenlik Duvarı ile kullanılabilir misiniz? |
| --- | -------- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| **Eylem Grubu** | Eylem Grubu. | Gelen | Hayır | Hayır |
| **ApiManagement** | Azure API Yönetimi'ne özel dağıtımlar için yönetim trafiği. <br/><br/>*Not:* Bu etiket, bölge başına denetim düzlemi için Azure API Yönetimi hizmet bitiş noktasını temsil eder. Bu, müşterilerin API'lerde, Operasyonlarda, İlkelerde, ADlandırılmış Değerler'de YÖNETIM işlemleri gerçekleştirmelerine olanak tanır.  | Gelen | Evet | Evet |
| **UygulamaInsightsKullanılabilirlik** | Uygulama Öngörüleri Kullanılabilirliği. | Gelen | Hayır | Hayır |
| **Uygulama Yapılandırması** | Uygulama Yapılandırması. | Giden | Hayır | Hayır |
| **AppService**    | Azure Uygulama Hizmeti. Bu etiket, web uygulaması ön uçlarına giden güvenlik kuralları için önerilir. | Giden | Evet | Evet |
| **AppServiceManagement** | Uygulama Hizmet Ortamına adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Evet |
| **AzureActiveDirectory** | Azure Active Directory. | Giden | Hayır | Evet |
| **AzureActiveDirectoryDomainServices** | Azure Active Directory Etki Alanı Hizmetleri'ne adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Evet |
| **AzureAdvancedThreatProtection** | Azure Gelişmiş Tehdit Koruması. | Giden | Hayır | Hayır |
| **AzureYedekleme** |Azure Yedekleme.<br/><br/>*Not:* Bu etiket, **Depolama** ve **AzureActiveDirectory** etiketlerini bağımlı hale sahiptir. | Giden | Hayır | Evet |
| **AzureBotService** | Azure Bot Hizmeti. | Giden | Hayır | Hayır |
| **AzureCloud** | Tüm [veri merkezi genel IP adresleri.](https://www.microsoft.com/download/details.aspx?id=56519) | Giden | Evet | Evet |
| **AzureCognitiveSearch** | Azure Bilişsel Arama. <br/><br/>Bu etiket veya bu etiketin kapsadığı IP adresleri, dizin leyicilere veri kaynaklarına güvenli erişim sağlamak için kullanılabilir. Daha fazla ayrıntı için [dizinleyici bağlantı belgelerine](https://docs.microsoft.com/azure/search/search-indexer-troubleshooting#connection-errors) bakın. <br/><br/> *Not*: Arama hizmetinin IP'si bu hizmet etiketi için IP aralıkları listesine dahil edilmez ve veri kaynaklarının IP güvenlik duvarına **eklenmesi gerekir.** | Gelen | Hayır | Hayır |
| **AzureBağlayıcıları** | Sonda/arka uç bağlantıları için Azure Logic Apps bağlayıcıları. | Gelen | Evet | Evet |
| **AzureContainerRegistry** | Azure Kapsayıcı Kayıt Defteri. | Giden | Evet | Evet |
| **AzureCosmosDB** | Azure Cosmos DB. | Giden | Evet | Evet |
| **Azure Databricks** | Azure Veri Tuğlaları. | Her ikisi de | Hayır | Hayır |
| **AzureDataExplorerYönetimi** | Azure Veri Gezgini Yönetimi. | Gelen | Hayır | Hayır |
| **AzureDataGölü** | Azure Veri Gölü Depolama Gen1. | Giden | Hayır | Evet |
| **AzureDevSpaces** | Azure Dev Alanları. | Giden | Hayır | Hayır |
| **AzureEventGrid** | Azure Olay Ağıt. <br/><br/>*Not:* Bu etiket, yalnızca ABD Güney Orta, ABD Doğu, ABD Doğu 2, ABD Batı 2 ve US Central'daki Azure Olay Izgara uç noktalarını kapsar. | Her ikisi de | Hayır | Hayır |
| **AzureFrontDoor.Frontend** <br/> **AzureFrontDoor.Backend** <br/> **AzureFrontDoor.FirstParty**  | Masmavi Ön Kapı. | Her ikisi de | Hayır | Hayır |
| **AzureInformationProtection** | Azure Bilgi Koruması.<br/><br/>*Not:* Bu etiket, **AzureActiveDirectory,** **AzureFrontDoor.Frontend** ve **AzureFrontDoor.FirstParty** etiketlerini bağımlı hale sahiptir. | Giden | Hayır | Hayır |
| **AzureIoTHub** | Azure IoT Hub'ı. | Giden | Hayır | Hayır |
| **AzureKeyVault** | Azure Anahtar Kasası.<br/><br/>*Not:* Bu etiketin **AzureActiveDirectory** etiketine bir bağımlılığı vardır. | Giden | Evet | Evet |
| **AzureLoadBalancer** | Azure altyapı yük dengeleyicisi. Etiket, Azure sistem sondalarının kaynağı olan [ana bilgisayar merkezinin sanal IP adresine](security-overview.md#azure-platform-considerations) (168.63.129.16) çevirir. Bu, Azure Yük Dengeleyici kaynağınıza olan trafiği içermez. Azure Yük Bakiyesi kullanmıyorsanız, bu kuralı geçersiz kılabilirsiniz. | Her ikisi de | Hayır | Hayır |
| **AzureMachineLearning** | Azure Makine Öğrenimi. | Her ikisi de | Hayır | Evet |
| **AzureMonitör** | Günlük Analitiği, Uygulama Öngörüleri, AzMon ve özel ölçümler (GiG uç noktaları).<br/><br/>*Not:* Log Analytics için bu etiketin **Depolama** etiketine bir bağımlılığı vardır. | Giden | Hayır | Evet |
| **AzureOpenVeri setleri** | Azure Açık Veri Kümeleri.<br/><br/>*Not:* Bu etiket, **AzureFrontDoor.Frontend** ve **Depolama** etiketine bağımlıdır. | Giden | Hayır | Hayır |
| **AzurePlatformdNS** | Temel altyapı (varsayılan) DNS hizmeti.<br/><br>Varsayılan DNS'yi devre dışı kalmak için bu etiketi kullanabilirsiniz. Bu etiketi kullanırken dikkatli olun. [Azure platformu yla ilgili konuları](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations)okumanızı öneririz. Ayrıca, bu etiketi kullanmadan önce sınama gerçekleştirmenizi öneririz. | Giden | Hayır | Hayır |
| **AzurePlatformIMDS** | Temel bir altyapı hizmeti olan Azure Örneği Meta Veri Hizmeti (IMDS).<br/><br/>Varsayılan IMDS'yi devre dışı kalmak için bu etiketi kullanabilirsiniz. Bu etiketi kullanırken dikkatli olun. [Azure platformu yla ilgili konuları](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations)okumanızı öneririz. Ayrıca, bu etiketi kullanmadan önce sınama gerçekleştirmenizi öneririz. | Giden | Hayır | Hayır |
| **AzurePlatformLKM** | Windows lisanslama veya anahtar yönetimi hizmeti.<br/><br/>Bu etiketi, lisanslama varsayılanlarını devre dışı kullanabilirsiniz. Bu etiketi kullanırken dikkatli olun. [Azure platformu yla ilgili konuları](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations)okumanızı öneririz.  Ayrıca, bu etiketi kullanmadan önce sınama gerçekleştirmenizi öneririz. | Giden | Hayır | Hayır |
| **AzureResourceManager** | Azure Kaynak Yöneticisi. | Giden | Hayır | Hayır |
| **AzureSignalR** | Azure Sinyalleyici. | Giden | Hayır | Hayır |
| **AzureSiteKurtarma** | Azure Site Kurtarma.<br/><br/>*Not:* Bu etiket, **AzureActiveDirectory,** **AzureKeyVault,** **EventHub,****GuestAndHybridManagement** ve **Storage** etiketlerini bağımlı hale getirir. | Giden | Hayır | Hayır |
| **AzureTrafficManager** | Azure Trafik Yöneticisi IP adreslerini yoklar.<br/><br/>Trafik Yöneticisi sondası IP adresleri hakkında daha fazla bilgi için [Azure Trafik Yöneticisi SSS](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs)bölümüne bakın. | Gelen | Hayır | Evet |  
| **BatchNodeManagement** | Azure Toplu İş'e adanmış dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Evet |
| **BilişselHizmetlerYönetimi** | Azure Bilişsel Hizmetleri için trafik aralığı. | Giden | Hayır | Hayır |
| **DataFactory**  | Azure Data Factory | Her ikisi de | Hayır | Hayır |
| **DataFactoryManagement** | Azure Veri Fabrikası için yönetim trafiği. | Giden | Hayır | Hayır |
| **Dynamics365ForMarketingEmail** | Dynamics 365'in pazarlama e-posta hizmetinin adres aralıkları. | Giden | Evet | Hayır |
| **ElasticAFD** | Elastik Azure Ön Kapı. | Her ikisi de | Hayır | Hayır |
| **EventHub** | Azure Etkinlik Hub'ları. | Giden | Evet | Evet |
| **Ağ Geçidi Yöneticisi** | Azure VPN Ağ Geçidi ve Uygulama Ağ Geçidi'ne adanmış dağıtımlar için yönetim trafiği. | Gelen | Hayır | Hayır |
| **GuestAndHybridManagement** | Azure Otomasyonu ve Konuk Yapılandırması. | Giden | Hayır | Evet |
| **HDInsight** | Azure HDInsight. | Gelen | Evet | Hayır |
| **ınternet** | Sanal ağın dışında olan ve genel internet tarafından erişilebilen IP adres alanı.<br/><br/>Adres aralığı, [Azure'a ait genel IP adres alanını](https://www.microsoft.com/download/details.aspx?id=41653)içerir. | Her ikisi de | Hayır | Hayır |
| **LogicApps** | Mantık Uygulamaları. | Her ikisi de | Hayır | Hayır |
| **LogicAppsManagement** | Logic Apps için yönetim trafiği. | Gelen | Hayır | Hayır |
| **MicrosoftCloudAppSecurity** | Microsoft Bulut Uygulaması Güvenliği. | Giden | Hayır | Hayır |
| **MicrosoftContainerRegistry** | Microsoft kapsayıcı resimleri için kapsayıcı kayıt defteri. <br/><br/>*Not:* Bu etiket, **AzureFrontDoor.FirstParty** etiketine bağımlıdır. | Giden | Evet | Evet |
| **PowerQueryOnline** | Güç Sorgusu Online. | Her ikisi de | Hayır | Hayır |
| **ServiceBus** | Premium hizmet katmanını kullanan Azure Servis Veri Servisi trafiği. | Giden | Evet | Evet |
| **ServiceFabric** | Azure Hizmet Kumaşı.<br/><br/>*Not:* Bu etiket, bölge başına denetim düzlemi için Hizmet Kumaşı hizmet bitiş noktasını temsil eder. Bu, müşterilerin Hizmet Kumaşı kümeleri için VNET'lerinden (uç nokta örneğin) yönetim işlemleri gerçekleştirmelerine olanak tanır. https:// westus.servicefabric.azure.com) | Her ikisi de | Hayır | Hayır |
| **Sql** | Azure SQL Veritabanı, MySQL için Azure Veritabanı, PostgreSQL için Azure Veritabanı ve Azure SQL Veri Ambarı.<br/><br/>*Not:* Bu etiket hizmeti temsil eder, ancak hizmetin belirli örneklerini temsil etmez. Örneğin etiket belirli bir SQL veritabanını veya sunucusunu değil Azure SQL Veritabanı hizmetini temsil eder. Bu etiket SQL yönetilen örnek için geçerli değildir. | Giden | Evet | Evet |
| **SqlManagement** | SQL'e özel dağıtımlar için yönetim trafiği. | Her ikisi de | Hayır | Evet |
| **Depolama** | Azure Depolama. <br/><br/>*Not:* Bu etiket hizmeti temsil eder, ancak hizmetin belirli örneklerini temsil etmez. Örneğin etiket belirli bir Azure Depolama hesabını değil Azure Depolama hizmetini temsil eder. | Giden | Evet | Evet |
| **StorageSyncService** | Depolama Eşitleme Hizmeti. | Her ikisi de | Hayır | Hayır |
| **WindowsVirtualDesktop** | Windows Sanal Masaüstü. | Her ikisi de | Hayır | Hayır |
| **VirtualNetwork** | Sanal ağ adres alanı (sanal ağ için tanımlanan tüm IP adres aralıkları), tüm bağlı şirket içi adres alanları, [bakan](virtual-network-peering-overview.md) sanal ağlar, [sanal ağ ağ geçidine](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json)bağlı sanal ağlar, ana bilgisayar sanal [IP adresi](security-overview.md#azure-platform-considerations)ve [kullanıcı tanımlı rotalarda](virtual-networks-udr-overview.md)kullanılan adres önekleri . Bu etiket varsayılan yolları da içerebilir. | Her ikisi de | Hayır | Hayır |

>[!NOTE]
>Klasik dağıtım modelinde (Azure Kaynak Yöneticisi'nden önce), önceki tabloda listelenen etiketlerin bir alt kümesi desteklenir. Bu etiketler farklı yazılır:
>
>| Klasik yazım | Eşdeğer Kaynak Yöneticisi etiketi |
>|---|---|
>| AZURE_LOADBALANCER | AzureLoadBalancer |
>| INTERNET | Internet |
>| VIRTUAL_NETWORK | VirtualNetwork |

> [!NOTE]
> Azure hizmetlerinin hizmet etiketleri, kullanılan belirli buluttan adres öneklerini gösterir. Örneğin, Azure Genel bulutundaki **Sql** etiket değerine karşılık gelen temel IP aralıkları, Azure Çin bulutundaki temel aralıklardan farklı olacaktır.

> [!NOTE]
> Azure Depolama veya Azure SQL Veritabanı gibi bir hizmet için bir [sanal ağ hizmet uç noktası](virtual-network-service-endpoints-overview.md) uygularsanız Azure, sanal ağ alt ağına hizmet için bir [rota](virtual-networks-udr-overview.md#optional-default-routes) ekler. Rotadaki adres önekleri, ilgili hizmet etiketiyle aynı adres önekleri veya CIDR aralıklarıdır.

## <a name="service-tags-on-premises"></a>Şirket içi hizmet etiketleri  
Şirket içi güvenlik duvarı yapılandırmalarınızın bir parçası olarak dahil etmek üzere geçerli hizmet etiketi ve aralık bilgilerini edinebilirsiniz. Bu bilgiler, her hizmet etiketine karşılık gelen IP aralıklarının geçerli zaman noktası listesidir. Bilgileri programlı olarak veya aşağıdaki bölümlerde açıklandığı gibi JSON dosya indirme yoluyla edinebilirsiniz.

### <a name="use-the-service-tag-discovery-api-public-preview"></a>Hizmet Etiketi Bulma API'sini (genel önizleme) kullanma
Ip adresi aralığı ayrıntılarıyla birlikte geçerli hizmet etiketleri listesini programlı olarak alabilirsiniz:

- [Geri kalanı](https://docs.microsoft.com/rest/api/virtualnetwork/servicetags/list)
- [Azure PowerShell](https://docs.microsoft.com/powershell/module/az.network/Get-AzNetworkServiceTag?view=azps-2.8.0&viewFallbackFrom=azps-2.3.2)
- [Azure CLI](https://docs.microsoft.com/cli/azure/network?view=azure-cli-latest#az-network-list-service-tags)

> [!NOTE]
> Genel önizlemede yken, Discovery API, JSON karşıdan yüklemeleri tarafından döndürülen bilgilerden daha az güncel bilgileri döndürebilir. (Sonraki bölüme bakın.)


### <a name="discover-service-tags-by-using-downloadable-json-files"></a>İndirilebilir JSON dosyalarını kullanarak hizmet etiketlerini keşfedin 
Ip adresi aralığı ayrıntılarıyla birlikte geçerli hizmet etiketleri listesini içeren JSON dosyalarını indirebilirsiniz. Bu listeler haftalık olarak güncellenir ve yayınlanır. Her bulutun konumları şunlardır:

- [Azure Genel](https://www.microsoft.com/download/details.aspx?id=56519)
- [Azure US Government](https://www.microsoft.com/download/details.aspx?id=57063)  
- [Azure Çin](https://www.microsoft.com/download/details.aspx?id=57062) 
- [Azure Almanya](https://www.microsoft.com/download/details.aspx?id=57064)   

> [!NOTE]
>Bu bilgilerin bir alt kümesi [Azure Geneli, Azure](https://www.microsoft.com/download/details.aspx?id=41653)Çin ivediliği ve Azure [Almanya'sı](https://www.microsoft.com/download/details.aspx?id=54770)için XML dosyalarında yayınlanmıştır. [Azure China](https://www.microsoft.com/download/details.aspx?id=42064) Bu XML indirmeleri 30 Haziran 2020'ye kadar amortismana uyacaktır ve bu tarihten sonra kullanılamaz. Önceki bölümlerde açıklandığı gibi Discovery API veya JSON dosya indirmelerini kullanarak geçiş yapmalısın.

### <a name="tips"></a>İpuçları 
- JSON dosyasındaki *changeNumber* değerlerini belirterek güncelleştirmeleri bir yayından diğerine algılayabilirsiniz. Her alt bölüm (örneğin, **Storage.WestUS)** değişiklikler meydana geldikçe artımlı kendi *changeNumber* vardır. Alt bölümlerden herhangi biri değiştirildiğinde dosyanın *changeNumber* üst düzeyi artımlanır.
- Hizmet etiketi bilgilerini nasıl ayrıştırırınız (örneğin, WestUS'taki Depolama için tüm adres aralıklarını alın) örnekler için [Service Tag Discovery API PowerShell](https://aka.ms/discoveryapi_powershell) belgelerine bakın.

## <a name="next-steps"></a>Sonraki adımlar
- [Ağ güvenlik grubu oluşturmayı](tutorial-filter-network-traffic.md)öğrenin.
