---
title: Azure güvenlik gruplarına genel bakış
titlesuffix: Azure Virtual Network
description: Ağ ve uygulama güvenlik grupları hakkında bilgi edinin. Güvenlik grupları Azure kaynakları arasındaki ağ trafiğini filtrelemenize yardımcı olur.
services: virtual-network
documentationcenter: na
author: malopMSFT
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2018
ms.author: malop
ms.reviewer: kumud
ms.openlocfilehash: 1d9fc022a0b0d5ba96517b4ed06b4a2576245a26
ms.sourcegitcommit: 7c5a2a3068e5330b77f3c6738d6de1e03d3c3b7d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2019
ms.locfileid: "70886020"
---
# <a name="security-groups"></a>Güvenlik grupları
<a name="network-security-groups"></a>

Ağ güvenlik grupları ile bir Azure [sanal ağı](virtual-networks-overview.md) içindeki Azure kaynaklarına gelen ve bu kaynaklardan giden ağ trafiğini filtreleyebilirsiniz. Ağ güvenlik grupları, farklı Azure kaynaklarına gelen ya da bu kaynaklardan dışarı giden ağ trafiğine izin veren veya bu trafiği reddeden [güvenlik kuralları](#security-rules) içerir. Sanal ağ dağıtımı ve ağ güvenlik grubu ataması için uygun olan Azure kaynakları hakkında bilgi için bkz. [Azure hizmetleri için sanal ağ tümleştirmesi](virtual-network-for-azure-services.md). Her kural için kaynak, hedef, bağlantı noktası ve protokol belirtebilirsiniz.

Bu makalede, ağ güvenlik gruplarını daha etkili bir şekilde kullanmanıza yardımcı olmak için gereken kavramlar açıklanır. Daha önce bir ağ güvenlik grubu oluşturmadıysanız deneyim edinmek için hızlı [öğreticiyi](tutorial-filter-network-traffic.md) tamamlayabilirsiniz. Ağ güvenlik grupları ve yönetimi hakkında bilginiz varsa bkz. [Ağ güvenlik gruplarını yönetme](manage-network-security-group.md). İletişim sorunları yaşıyorsanız ve ağ güvenlik gruplarıyla ilgili sorunları gidermeniz gerekiyorsa bkz. [Sanal makine ağ trafiği filtresi sorunlarını tanılama](diagnose-network-traffic-filter-problem.md). [Ağ güvenlik grubu akış günlüklerini](../network-watcher/network-watcher-nsg-flow-logging-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) etkinleştirerek bir ağ güvenlik grubu ile ilişkilendirilmiş kaynaklara gelen ve bu kaynaklardan giden [ağ trafiğini analiz edebilirsiniz](../network-watcher/traffic-analytics.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="security-rules"></a>Güvenlik kuralları

Bir ağ güvenlik grubunda Azure abonelik [limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) dahilinde sıfır veya istenen sayıda kural bulunabilir. Her bir kural aşağıdaki özellikleri belirtir:

|Özellik  |Açıklama  |
|---------|---------|
|Name|Ağ güvenlik grubu içinde benzersiz bir ad.|
|Priority | 100 ile 4096 arasında bir rakam. Kurallar öncelik sırasına göre işleme alınır ve düşük rakamlı kurallar daha yüksek önceliğe sahip olduğundan yüksek rakamlı kurallardan önce uygulanır. Trafik bir kuralla eşleştiğinde işlem durur. Bunun sonucunda yüksek önceliğe sahip olan kurallarla aynı özniteliklere sahip olan önceliği daha düşük olan (yüksek rakamlı) kurallar işleme alınmaz.|
|Kaynak veya hedef| Herhangi bir IP adresi, sınıfsız etki alanları arası yönlendirme (CIDR) bloğu (10.0.0.0/24 gibi), [hizmet etiketi](#service-tags) veya [uygulama güvenlik grubu](#application-security-groups). Bir Azure kaynağı için adres belirtirken kaynağa atanmış olan özel IP adresini belirtmeniz gerekir. Ağ güvenlik grupları, Azure gelen trafik için genel IP adresini özel IP adresine çevirdikten sonra ve giden trafik için özel IP adresini genel IP adreslerine çevirmeden önce işleme alınır. Azure [IP adresleri](virtual-network-ip-addresses-overview-arm.md) hakkında daha fazla bilgi edinin. Aralık, hizmet etiketi veya uygulama güvenlik grubu belirterek daha az sayıda güvenlik kuralı oluşturabilirsiniz. Bir kuralda birden fazla IP adresi veya aralığı belirtme özelliği (birden fazla hizmet etiketi veya uygulama grubu belirtemezsiniz) [genişletilmiş güvenlik kuralı](#augmented-security-rules) olarak adlandırılır. Genişletilmiş güvenlik kuralları yalnızca Resource Manager dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında oluşturulabilir. Klasik dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında birden fazla IP adresi ve IP adresi aralığı belirtemezsiniz. [Azure dağıtım modelleri](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hakkında daha fazla bilgi edinin.|
|Protocol     | TCP, UDP, ıCMP veya any.|
|Direction| Kuralın gelen veya giden trafiğe uygulanma seçeneği.|
|Bağlantı noktası aralığı     |Tek bir bağlantı noktası veya aralık belirtebilirsiniz. Örneğin 80 veya 10000-10005 değerini kullanabilirsiniz. Aralık belirterek oluşturmanız gereken güvenlik kuralı sayısını azaltabilirsiniz. Genişletilmiş güvenlik kuralları yalnızca Resource Manager dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında oluşturulabilir. Klasik dağıtım modeliyle oluşturulmuş olan ağ güvenlik gruplarında aynı güvenlik kuralı içinde birden fazla bağlantı noktası ve bağlantı noktası aralığı belirtemezsiniz.   |
|Action     | İzin ver veya reddet        |

Ağ güvenlik grubu güvenlik kuralları, trafiğe izin verilmesi veya trafiğin reddedilmesi için 5 tanımlama grubu bilgisi (kaynak, kaynak bağlantı noktası, hedef, hedef bağlantı noktası ve protokol) ile önceliğe göre değerlendirilir. Var olan bağlantılar için bir akış kaydı oluşturulur. Akış kaydının bağlantı durumuna göre iletişime izin verilir veya iletişim reddedilir. Akış kaydı bir ağ güvenlik grubunun durum bilgisine sahip olmasını sağlar. Örneğin 80 numaralı bağlantı noktasından tüm adreslere doğru giden bir güvenlik kuralı belirtirseniz giden trafiğe yanıt olarak bir gelen güvenlik kuralı belirtmeniz gerekli değildir. Yalnızca iletişimin dışarıdan başlatılması halinde bir gelen güvenlik kuralı belirtmeniz gerekir. Bunun tersi de geçerlidir. Gelen trafiğe bir bağlantı noktası üzerinden izin verilmesi halinde bağlantı noktasından geçen trafiğe yanıt olarak bir giden güvenlik belirtmeniz gerekli değildir.
Akışı etkinleştiren bir güvenlik kuralını kaldırdığınızda mevcut bağlantılar kesintiye uğramayabilir. Bağlantılar durdurulduğunda trafik akışları kesintiye uğrar ve en azından birkaç dakika boyunca hiçbir yönde trafik akışı gerçekleşmez.

Bir ağ güvenlik grubu içinde sınırlı sayıda güvenlik kuralı oluşturabilirsiniz. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

## <a name="augmented-security-rules"></a>Genişletilmiş güvenlik kuralları

Genişletilmiş güvenlik kuralları, sanal ağlar için güvenlik tanımını daha basit hale getirerek daha az sayıda kuralla daha geniş çaplı ve karmaşık ağ güvenlik ilkesi tanımlamanızı sağlar. Birden fazla bağlantı noktası ile birden fazla açık IP adresini ve aralığını bir araya getirerek tek ve anlaşılması kolay bir güvenlik kuralı oluşturabilirsiniz. Genişletilmiş kuralları bir kuralın kaynak, hedef ve bağlantı noktası alanlarında kullanabilirsiniz. Güvenlik kuralı tanımınızın bakımını kolaylaştırmak için genişletilmiş güvenlik kurallarını [hizmet etiketleri](#service-tags) veya [uygulama güvenlik gruplarıyla](#application-security-groups) bir arada kullanabilirsiniz. Bir kuralda belirtebileceğiniz adres, Aralık ve bağlantı noktası sayısı için sınırlar vardır. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.

## <a name="service-tags"></a>Hizmet etiketleri

Hizmet etiketi, güvenlik kuralı oluşturma sırasındaki karmaşıklığı en aza indirmeye yardımcı olmak için bir IP adresi ön eki grubunu temsil eder. Kendi hizmet etiketinizi oluşturamaz veya bir etiket içinde yer alacak IP adreslerini belirleyemezsiniz. Hizmet etiketine dahil olan adres ön ekleri Microsoft tarafından yönetilir ve hizmet etiketi adresler değiştikçe otomatik olarak güncelleştirilir. Hizmet etiketlerini güvenlik kuralı oluştururken belirli IP adreslerinin yerine kullanabilirsiniz. 

Aşağıdaki hizmet etiketleri [ağ güvenlik grupları kurallarında](https://docs.microsoft.com/azure/virtual-network/security-overview#security-rules)kullanılabilir. Sonunda yıldız işareti olan hizmet etiketleri (örn. Azurecsesli *), [Azure Güvenlik Duvarı ağ kurallarında](https://docs.microsoft.com/azure/firewall/service-tags)da kullanılabilir. 

* **Apimanayönetimi*** (yalnızca Kaynak Yöneticisi): Bu etiket, APıM adanmış dağıtımlar için yönetim trafiğinin adres öneklerini gösterir. *ApiManagement* değerini belirtirseniz ApiManagement'a gelen trafiğe izin verilir veya trafik reddedilir. Gelen/giden güvenlik kuralı için bu etiket önerilir. 
* **Appservice*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure AppService hizmetinin adres öneklerini gösterir. *AppService* değerini belirtirseniz AppService'e gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AppService’e erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AppService.[bölge adı]. Bu etiket, WebApps ön uçları için giden güvenlik kuralı için önerilir.  
* **Appservicemanagement*** (yalnızca Kaynak Yöneticisi): Bu etiket, App Service Ortamı adanmış dağıtımlar için yönetim trafiğinin adres öneklerini gösterir. *AppServiceManagement* değerini belirtirseniz AppServiceManagement'a gelen trafiğe izin verilir veya trafik reddedilir. Gelen/giden güvenlik kuralı için bu etiket önerilir. 
* **AzureActiveDirectory*** (yalnızca Kaynak Yöneticisi): Bu etiket, AzureActiveDirectory hizmetinin adres öneklerini gösterir. *AzureActiveDirectory* değerini belirtirseniz AzureActiveDirectory’ye gelen trafiğe izin verilir veya trafik reddedilir. Giden güvenlik kuralı için bu etiket önerilir.
* **AzureActiveDirectoryDomainServices*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure Active Directory Domain Services adanmış dağıtımlar için yönetim trafiğinin adres öneklerini gösterir. Değer için *AzureActiveDirectoryDomainServices* belirtirseniz, trafiğe izin verilir veya AzureActiveDirectoryDomainServices erişimi reddedilir. Gelen/giden güvenlik kuralı için bu etiket önerilir.  
* **AzureBackup*** (yalnızca Kaynak Yöneticisi): Bu etiket, AzureBackup hizmetinin adres öneklerini gösterir. Değer için *AzureBackup* belirtirseniz, trafiğe izin verilir veya AzureBackup erişimi reddedilir. Bu etiketin **depolama** ve **AzureActiveDirectory** etiketlerine bir bağımlılığı vardır. Giden güvenlik kuralı için bu etiket önerilir. 
* **Azurecyüksek*** (yalnızca Kaynak Yöneticisi): Bu etiket, tüm [veri merkezi genel IP adresleri](https://www.microsoft.com/download/details.aspx?id=41653)dahil olmak üzere Azure için IP adresi alanını gösterir. *AzureCloud* değerini belirtirseniz Azure genel IP adreslerine giden trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions)azurecı erişimine izin vermek istiyorsanız, bölgeyi azurecyüksek biçiminde belirtebilirsiniz. [bölge adı]. Giden güvenlik kuralı için bu etiket önerilir. 
* **AzureConnectors*** (yalnızca Kaynak Yöneticisi): Bu etiket, araştırma/arka uç bağlantıları için Logic Apps bağlayıcılarının adres öneklerini gösterir. *AzureConnectors* değerini belirtirseniz AzureConnectors’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureConnectors’a erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AzureConnectors.[bölge adı]. Bu etiket, gelen güvenlik kuralı için önerilir. 
* **AzureContainerRegistry*** (yalnızca Kaynak Yöneticisi): Bu etiket Azure Container Registry hizmetinin adres öneklerini gösterir. *AzureContainerRegistry* değerini belirtirseniz AzureContainerRegistry'ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureContainerRegistry’ye erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AzureContainerRegistry.[bölge adı]. Giden güvenlik kuralı için bu etiket önerilir. 
* **Azu, Smosdb*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure Cosmos veritabanı hizmetinin adres öneklerini gösterir. *AzureCosmosDB* değerini belirtirseniz AzureCosmosDB’ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureCosmosDB’ye erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AzureCosmosDB.[bölge adı]. Giden güvenlik kuralı için bu etiket önerilir. 
* **AzureDataLake*** (yalnızca Kaynak Yöneticisi): Bu etiket Azure Data Lake hizmetinin adres öneklerini gösterir. *AzureDataLake* değerini belirtirseniz AzureDataLake’e gelen trafiğe izin verilir veya trafik reddedilir. Giden güvenlik kuralı için bu etiket önerilir. 
* **AzureKeyVault*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure Keykasası hizmetinin adres öneklerini gösterir. *AzureKeyVault* değerini belirtirseniz AzureKeyVault’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) AzureKeyVault’a erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: AzureKeyVault.[bölge adı]. Bu etiketin **AzureActiveDirectory** etiketine bağımlılığı vardır. Giden güvenlik kuralı için bu etiket önerilir.  
* **AzureLoadBalancer** (Kaynak Yöneticisi) (Klasik için**AZURE_LOADBALANCER** ): Bu etiket, Azure 'un altyapı yük dengeleyiciyi gösterir. Bu etiket, Azure’ın sistem durumu araştırmalarının kaynağı olan [Ana bilgisayar sanal IP adresine](security-overview.md#azure-platform-considerations) (168.63.129.16) çevrilir. Azure yük dengeleyiciyi kullanmıyorsanız bu kuralı geçersiz kılabilirsiniz.
* **AzureMachineLearning*** (yalnızca Kaynak Yöneticisi): Bu etiket, AzureMachineLearning hizmetinin adres öneklerini gösterir. Değer için *AzureMachineLearning* belirtirseniz, trafiğe izin verilir veya AzureMachineLearning erişimi reddedilir. Giden güvenlik kuralı için bu etiket önerilir. 
* **AzureMonitor*** (yalnızca Kaynak Yöneticisi): Bu etiket Log Analytics, Application Insights, AzMon ve özel ölçümlerin (GB uç noktaları) adres öneklerini gösterir. Değer için *AzureMonitor* belirtirseniz, trafiğe izin verilir veya AzureMonitor erişimi reddedilir. Log Analytics için, bu etiketin **depolama** etiketine bağımlılığı vardır. Giden güvenlik kuralı için bu etiket önerilir.
* **AzurePlatformDNS** (Yalnızca Kaynak Yöneticisi): Bu etiket, temel bir altyapı hizmeti olan DNS 'yi gösterir. Değer için *AzurePlatformDNS* BELIRTIRSENIZ, DNS Için varsayılan [Azure platformu değerlendirmesi](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) 'ni devre dışı bırakabilirsiniz. Lütfen bu etiketi kullanırken dikkatli olun. Bu etiket kullanılmadan önce test edilmesi önerilir. 
* **AzurePlatformIMDS** (Yalnızca Kaynak Yöneticisi): Bu etiket, temel bir altyapı hizmeti olan ıMDS 'yi gösterir. Değer için *AzurePlatformIMDS* belirtirseniz, IDS Için varsayılan [Azure platformu değerlendirmesi](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) 'ni devre dışı bırakabilirsiniz. Lütfen bu etiketi kullanırken dikkatli olun. Bu etiket kullanılmadan önce test edilmesi önerilir. 
* **AzurePlatformLKM** (Yalnızca Kaynak Yöneticisi): Bu etiket Windows lisanslama veya anahtar yönetimi hizmetini gösterir. Değer için *AzurePlatformLKM* belirtirseniz, lisans Için varsayılan [Azure platformu değerlendirmesi](https://docs.microsoft.com/azure/virtual-network/security-overview#azure-platform-considerations) 'ni devre dışı bırakabilirsiniz. Lütfen bu etiketi kullanırken dikkatli olun. Bu etiket kullanılmadan önce test edilmesi önerilir. 
* **AzureTrafficManager*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure Traffic Manager araştırması IP adresleri için IP adresi alanını gösterir. Traffic Manager yoklama IP adresleri hakkında daha fazla bilgi için [Azure Traffic Manager hakkında SSS](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-faqs) sayfasına göz atın. Bu etiket, gelen güvenlik kuralı için önerilir.  
* **Batchnodemanagement*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure Batch adanmış dağıtımlar için yönetim trafiğinin adres öneklerini gösterir. Değer için *Batchnodemanagement* belirtirseniz, Batch hizmetinden işlem düğümlerine trafiğe izin verilir veya erişim engellenir. Gelen/giden güvenlik kuralı için bu etiket önerilir. 
* **Biliveservicesmanagement** (Yalnızca Kaynak Yöneticisi): Bu etiket, bilişsel hizmetler için trafiğin adres öneklerini gösterir. Değer için *Biliveservicesmanagement* belirtirseniz, trafiğe izin verilir veya biliveservicesmanagement tarafından reddedilir. Giden güvenlik kuralı için bu etiket önerilir.  
* **Dynamics365ForMarketingEmail** (Yalnızca Kaynak Yöneticisi): Bu etiket, Dynamics 365 pazarlama e-posta hizmetinin adres öneklerini gösterir. Değer için *Dynamics365ForMarketingEmail* belirtirseniz, trafiğe izin verilir veya Dynamics365ForMarketingEmail erişimi reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions)Dynamics365ForMarketingEmail erişimine izin vermek istiyorsanız, bölgeyi aşağıdaki biçimde belirtebilirsiniz Dynamics365ForMarketingEmail. [bölge adı].
* **EventHub*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure EventHub hizmetinin adres öneklerini gösterir. *EventHub* değerini belirtirseniz EventHub’a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) EventHub’a erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: EventHub.[bölge adı]. Giden güvenlik kuralı için bu etiket önerilir. 
* **Gatewaymanager** (Yalnızca Kaynak Yöneticisi): Bu etiket, VPN/uygulama ağ geçitleri adanmış dağıtımları için yönetim trafiğinin adres öneklerini gösterir. *GatewayManager* değerini belirtirseniz GatewayManager’a gelen trafiğe izin verilir veya trafik reddedilir. Bu etiket, gelen güvenlik kuralı için önerilir. 
* **Internet** (Kaynak Yöneticisi) (Klasik için**Internet** ): Bu etiket, sanal ağın dışında olan ve genel Internet tarafından erişilebilen IP adresi alanını gösterir. Adres aralığı [Azure'a ait genel IP adresi alanını](https://www.microsoft.com/download/details.aspx?id=41653) içerir.
* **Microsoftcontainerregistry*** (yalnızca Kaynak Yöneticisi): Bu etiket, Microsoft Container Registry hizmetinin adres öneklerini gösterir. *MicrosoftContainerRegistry* değerini belirtirseniz MicrosoftContainerRegistry'ye gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) MicrosoftContainerRegistry’ye erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: MicrosoftContainerRegistry.[bölge adı]. Giden güvenlik kuralı için bu etiket önerilir. 
* **ServiceBus*** (yalnızca Kaynak Yöneticisi): Bu etiket, Premium hizmet katmanını kullanarak Azure ServiceBus hizmetinin adres öneklerini gösterir. *ServiceBus* değerini belirtirseniz ServiceBus'a gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgede](https://azure.microsoft.com/regions) ServiceBus’a erişime izin vermek istiyorsanız bölgeyi şu biçimde belirtebilirsiniz: ServiceBus.[bölge adı]. Giden güvenlik kuralı için bu etiket önerilir. 
* **Servicefabric*** (yalnızca Kaynak Yöneticisi): Bu etiket, ServiceFabric hizmetinin adres öneklerini gösterir. Değer için *Servicefabric* belirtirseniz, servicefabric 'e trafiğe izin verilir veya erişim engellenir. Giden güvenlik kuralı için bu etiket önerilir. 
* **SQL*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure SQL veritabanı 'nın adres öneklerini, MySQL için Azure veritabanı, PostgreSQL için Azure veritabanı ve Azure SQL veri ambarı Hizmetleri 'ni gösterir. *Sql* değerini belirtirseniz Sql’e gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions)SQL erişimine izin vermek istiyorsanız, aşağıdaki SQL. [bölge adı] biçiminde bölgeyi belirtebilirsiniz. Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin etiket belirli bir SQL veritabanını veya sunucusunu değil Azure SQL Veritabanı hizmetini temsil eder. Giden güvenlik kuralı için bu etiket önerilir. 
* **SqlManagement*** (yalnızca Kaynak Yöneticisi): Bu etiket, SQL adanmış dağıtımlar için yönetim trafiğinin adres öneklerini gösterir. Değer için *SqlManagement* belirtirseniz, trafiğin SqlManagement 'a izni verilir veya reddedilir. Gelen/giden güvenlik kuralı için bu etiket önerilir. 
* **Depolama*** (yalnızca Kaynak Yöneticisi): Bu etiket, Azure depolama hizmeti için IP adresi alanını gösterir. *Depolama* değerini belirtirseniz depolama alanına gelen trafiğe izin verilir veya trafik reddedilir. Yalnızca belirli bir [bölgedeki](https://azure.microsoft.com/regions)depolamaya erişime izin vermek istiyorsanız, bölgeyi aşağıdaki biçim deposunda belirtebilirsiniz. [bölge adı]. Bu etiket hizmetin belirli örneklerini değil yalnızca hizmetin kendisini temsil eder. Örneğin etiket belirli bir Azure Depolama hesabını değil Azure Depolama hizmetini temsil eder. Giden güvenlik kuralı için bu etiket önerilir. 
* **VirtualNetwork** (Kaynak Yöneticisi) (Klasik için**VIRTUAL_NETWORK** ): Bu etiket sanal ağ adres alanını (sanal ağ için tanımlanan tüm CıDR aralıkları), bağlı olan tüm şirket içi adres [alanlarını, eşlenmiş](virtual-network-peering-overview.md) sanal ağları veya sanal ağ [geçidine](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%3ftoc.json)bağlı sanal ağı içerir. [ ](security-overview.md#azure-platform-considerations) [Kullanıcı tanımlı yollarla](virtual-networks-udr-overview.md)kullanılan konak ve adres ön eklerinin sanal IP adresi. Bu etiketin varsayılan yol içerebileceğini unutmayın. 

> [!NOTE]
> Azure hizmetlerinin hizmet etiketleri, kullanılan belirli buluttan gelen adres öneklerini gösterir. 

> [!NOTE]
> Azure Depolama veya Azure SQL Veritabanı gibi bir hizmet için bir [sanal ağ hizmet uç noktası](virtual-network-service-endpoints-overview.md) uygularsanız Azure, sanal ağ alt ağına hizmet için bir [rota](virtual-networks-udr-overview.md#optional-default-routes) ekler. Rotada adres ön ekleri ilgili hizmet etiketiyle aynı adres ön ekleri veya CIDR aralıklarıdır.

### <a name="service-tags-in-on-premises"></a>Şirket içinde hizmet etiketleri  
Azure [kamu](https://www.microsoft.com/download/details.aspx?id=56519), [ABD kamu](https://www.microsoft.com/download/details.aspx?id=57063), [Çin](https://www.microsoft.com/download/details.aspx?id=57062)ve [Almanya](https://www.microsoft.com/download/details.aspx?id=57064) bulutları için aşağıdaki haftalık yayınlarda ön ek ayrıntılarını içeren hizmet etiketlerinin listesini bir şirket içi güvenlik duvarıyla indirebilir ve tümleştirebilirsiniz.

**Hizmet etiketi bulma API** (Genel Önizleme)- [rest](https://aka.ms/discoveryapi_rest), [Azure PowerShell](https://aka.ms/discoveryapi_powershell)ve [Azure CLI](https://aka.ms/discoveryapi_cli)kullanarak bu bilgileri program aracılığıyla da alabilirsiniz. 

> [!NOTE]
> Azure [kamu](https://www.microsoft.com/en-us/download/details.aspx?id=41653), [Çin](https://www.microsoft.com/en-us/download/details.aspx?id=42064)ve [Almanya](https://www.microsoft.com/en-us/download/details.aspx?id=54770) bulutları için aşağıdaki haftalık yayınlar (eski sürüm) 30 Haziran 2020 ' den kullanım dışı olacaktır. Lütfen yukarıda açıklandığı gibi güncelleştirilmiş yayınları kullanmaya başlayın. 

## <a name="default-security-rules"></a>Varsayılan güvenlik kuralları

Azure, oluşturduğunuz tüm ağ güvenlik gruplarına aşağıdaki varsayılan kuralları ekler:

### <a name="inbound"></a>Gelen

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Priority|Source|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protocol|Access|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Any|Allow|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Priority|Source|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protocol|Access|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Any|Allow|

#### <a name="denyallinbound"></a>DenyAllInbound

|Priority|Source|Kaynak bağlantı noktaları|Hedef|Hedef bağlantı noktaları|Protocol|Access|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Any|Reddet|

### <a name="outbound"></a>Giden

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Priority|Source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Any | Allow |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Priority|Source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | İnternet | 0-65535 | Any | Allow |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Priority|Source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Any | Reddet |

**Kaynak** ve **Hedef** sütunlarında *VirtualNetwork*, *AzureLoadBalancer* ve *Internet*, için IP adresi yerine [hizmet etiketi](#service-tags) belirtilir. Protokol sütununda, TCP, UDP ve ıCMP **'yi kapsar.** Bir kural oluştururken TCP, UDP, ıCMP veya any belirtebilirsiniz. **Kaynak** ve **Hedef** sütunlarında yer alan *0.0.0.0/0* ifadesi tüm adresleri temsil eder. Azure portal, Azure CLı veya PowerShell gibi istemciler bu ifade için * veya herhangi birini kullanabilir.
 
Varsayılan kuralları kaldıramazsınız ancak daha yüksek önceliğe sahip kurallar oluşturarak onları geçersiz kılabilirsiniz.

## <a name="application-security-groups"></a>Uygulama güvenlik grupları

Uygulama güvenlik grupları ağ güvenliğini uygulamanın yapısının doğal bir uzantısı olarak yapılandırmanıza imkan vererek sanal makineleri gruplamanızı ve ağ güvenlik ilkelerini bu gruplara göre tanımlamanızı sağlar. Açık IP adreslerinin bakımını el ile yapmanıza gerek kalmadan güvenlik ilkesini farklı ölçeklerde yeniden kullanabilirsiniz. Platform açık IP adreslerinin ve birden fazla kural kümesinin karmaşık süreçlerini üstlenerek iş mantığınıza odaklanmanızı sağlar. Uygulama güvenlik gruplarını daha iyi anlamak için aşağıdaki örneği inceleyin:

![Uygulama güvenlik grupları](./media/security-groups/application-security-groups.png)

Yukarıdaki resimde *NIC1* ve *NIC2*, *AsgWeb* uygulama güvenlik grubunun üyeleridir. *NIC3*, *AsgLogic* uygulama güvenlik grubunun üyesidir. *NIC4*, *AsgDb* uygulama güvenlik grubunun üyesidir. Bu örnekteki tüm ağ arabirimleri tek bir uygulama güvenlik grubuna üye olsa da bir ağ arabirimi [Azure sınırları](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) dahilinde birden fazla uygulama güvenlik grubuna üye olabilir. Ağ arabirimlerinin hiçbiri bir ağ güvenlik grubuyla ilişkilendirilmemiştir. *NSG1*, iki alt ağ ile de ilişkilendirilmiştir ve aşağıdaki kuralları içerir:

### <a name="allow-http-inbound-internet"></a>Allow-HTTP-Inbound-Internet

Bu kural, internetten Web sunucularına gelen trafiğe izin vermek için kullanılır. İnternetten gelen trafik, [DenyAllInbound](#denyallinbound) varsayılan güvenlik grubu tarafından reddedildiğinden *AsgLogic* veya *AsgDb* uygulama güvenlik grupları için ek kurala ihtiyaç duyulmaz.

|Priority|Source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 100 | İnternet | * | AsgWeb | 80 | TCP | Allow |

### <a name="deny-database-all"></a>Deny-Database-All

[AllowVNetInBound](#allowvnetinbound) varsayılan güvenlik kuralı aynı sanal ağ içinde bulunan kaynaklar arasındaki tüm iletişime izin verdiğinden, tüm kaynaklardan gelen trafiği reddetmek için bu kurala ihtiyaç duyulur.

|Priority|Source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 120 | * | * | AsgDb | 1433 | Any | Reddet |

### <a name="allow-database-businesslogic"></a>Allow-Database-BusinessLogic

Bu kural *AsgLogic* uygulama güvenlik grubundan *AsgDb* uygulama güvenlik grubuna gelen trafiğe izin verir. Bu kuralın önceliği, *Deny-Database-All* kuralının önceliğinden daha yüksektir. Sonuç olarak bu kural, *Deny-Database-All* kuralından önce işlenir ve böylece *AsgLogic* uygulama güvenlik grubundan gelen trafiğe izin veriler ve diğer tüm trafik engellenir.

|Priority|Source|Kaynak bağlantı noktaları| Hedef | Hedef bağlantı noktaları | Protocol | Access |
|---|---|---|---|---|---|---|
| 110 | AsgLogic | * | AsgDb | 1433 | TCP | Allow |

Bir uygulama güvenlik grubunu kaynak veya hedef olarak belirten kurallar yalnızca uygulama güvenlik grubuna üye olan ağ arabirimlerine uygulanır. Ağ arabirimi bir uygulama güvenlik grubuna üye değilse, ağ güvenlik grubu alt ağ ile ilişkilendirilmiş olsa dahi kural ağ arabirimine uygulanmaz.

Uygulama güvenlik grupları aşağıdaki sınırlamalara sahiptir:

-   Bir abonelik içinde bulunabilecek uygulama güvenlik grubu sayısı sınırlıdır ve uygulama güvenlik gruplarıyla ilgili başka sınırlar da vardır. Ayrıntılar için [Azure limitleri](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) makalesini inceleyin.
- Bir uygulama güvenlik grubunu bir güvenlik kuralında kaynak ve hedef olarak belirtebilirsiniz. Kaynak veya hedefte birden çok uygulama güvenlik grubu belirtemezsiniz.
- Bir uygulama güvenlik grubuna atanan tüm ağ arabirimleri, uygulama güvenlik grubuna atanmış ilk ağ arabirimiyle aynı sanal ağda olmalıdır. Örneğin, ilk ağ arabirimi *VNet1* adlı sanal ağdaki *AsgWeb* adlı bir uygulama güvenlik grubuna atanmışsa *ASGWeb*’e atanan sonraki tüm ağ arabirimleri *VNet1*’de olmalıdır. Bir uygulama güvenlik grubuna farklı ağlarda bulunan ağ arabirimlerini ekleyemezsiniz.
- Uygulama güvenlik grubunu bir güvenlik kuralında kaynak ve hedef olarak belirtirseniz iki uygulama güvenlik grubundaki ağ arabirimlerinin de aynı sanal ağda bulunması gerekir. Örneğin *AsgLogic* üzerinde *VNet1* içinde bulunan ağ arabirimleri, *AsgDb* üzerinde de *VNet2* içinde bulunan ağ arabirimleri varsa, bir kural içinde *AsgLogic* grubunu kaynak olarak ve *AsgDb* grubunu da hedef olarak belirleyemezsiniz. Hem kaynak hem de hedef uygulama güvenlik gruplarının tüm ağ arabirimlerinin aynı sanal ağ içinde bulunması gerekir.

> [!TIP]
> İhtiyacınız olan güvenlik kuralı sayısını ve kural değiştirme gereksinimini en aza indirmek için, gereken uygulama güvenlik gruplarını planlarken ve kuralları oluştururken tek IP adreslerini veya IP adresi aralıklarını kullanmak yerine hizmet etiketlerini ya da uygulama güvenlik gruplarını kullanın.

## <a name="how-traffic-is-evaluated"></a>Trafik nasıl değerlendirilir?

Birden fazla Azure hizmetinde bulunan kaynakları bir Azure sanal ağına dağıtabilirsiniz. Hizmetlerin tam listesi için bkz. [Sanal ağa dağıtılabilecek hizmetler](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Bir sanal makine içindeki her bir sanal ağ [alt ağına](virtual-network-manage-subnet.md#change-subnet-settings) ve [ağ arabirimine](virtual-network-network-interface.md#associate-or-dissociate-a-network-security-group) sıfır veya bir ağ güvenlik grubu atayabilirsiniz. Bir ağ güvenlik grubunu istediğiniz sayıda alt ağ ve ağ arabirimi ile ilişkilendirebilirsiniz.

Aşağıdaki resimde, ağ güvenlik gruplarının 80 numaralı TCP bağlantı noktası üzerinden gelen ve giden internet trafiğine izin vermek için dağıtılabilecek ağ güvenlik grupları için farklı senaryolar gösterilmektedir:

![NSG-processing](./media/security-groups/nsg-interaction.png)

Azure'ın ağ güvenlik grupları için gelen ve giden kuralları nasıl işlediğini anlamak için yukarıdaki resmi ve aşağıdaki metni inceleyin:

### <a name="inbound-traffic"></a>Gelen trafik

Azure, gelen trafik için ilk olarak varsa bir alt ağ ile ilişkilendirilmiş ağ güvenlik grubu içindeki kuralları ve ardından varsa ağ arabirimi ile ilişkilendirilmiş ağ güvenlik grubundaki kuralları işler.

- **VM1**: *NSG1* ' deki güvenlik kuralları, *Subnet1* Ile Ilişkilendirildiğinden ve *VM1* *Subnet1*içinde olduğundan işlenir. Gelen trafik için 80 numaralı bağlantı noktasına izin veren bir kural oluşturmadığınız sürece trafik [DenyAllInbound](#denyallinbound) varsayılan güvenlik kuralı tarafından reddedilir ve *NSG2*, ağ arabirimi ile ilişkilendirilmiş olduğundan *NSG2* tarafından değerlendirilmez. *NSG1*, 80 numaralı bağlantı noktasına izin veren bir güvenlik kuralına sahipse trafik *NSG2* tarafından işlenir. 80 numaralı bağlantı noktasından gelen trafiğin sanal makineye iletilmesine izin vermek için hem *NSG1* hem de *NSG2* içinde 80 numaralı bağlantı noktası üzerinden gelen internet bağlantılarına izin veren bir kural olması gerekir.
- **VM2**: *VM2* de *Subnet1*içinde olduğu için *NSG1* içindeki kurallar işlenir. *VM2* ağ arabirimi ile ilişkilendirilmiş bir ağ güvenlik grubu olmadığından *NSG1* üzerinden izin verilen tüm trafiği alır veya *NSG1* tarafından reddedilen tüm trafiği reddeder. Ağ güvenlik grubu bir alt ağ ile ilişkilendirilmiş olduğunda bu alt ağ içindeki tüm kaynaklar için trafiğe izin verilir veya trafik reddedilir.
- **VM3**: *Subnet2*ile ilişkili bir ağ güvenlik grubu olmadığından, *NSG2* , *VM3*'e iliştirilmiş ağ arabirimiyle ilişkili olduğundan, trafiğe alt ağa izin verilir ve *NSG2*tarafından işlenir.
- **VM4**: Bir ağ güvenlik grubu *Subnet3*ile ilişkili olmadığından ve sanal makinedeki ağ arabirimiyle trafiğe *VM4* izin verilir. Ağ güvenlik grubu ile ilişkilendirilmiş olmayan alt ağ ve ağ arabirimleri üzerinden gelen tüm ağ trafiğine izin verilir.

### <a name="outbound-traffic"></a>Giden trafik

Azure, giden trafik için ilk olarak varsa bir ağ arabirimi ile ilişkilendirilmiş ağ güvenlik grubu içindeki kuralları ve ardından varsa alt ağ ile ilişkilendirilmiş ağ güvenlik grubundaki kuralları işler.

- **VM1**: *NSG2* içindeki güvenlik kuralları işlenir. 80 numaralı bağlantı noktası üzerinden internete giden trafiği reddeden bir güvenlik kuralı oluşturmadığınız sürece *NSG1* ve *NSG2* içindeki [AllowInternetOutbound](#allowinternetoutbound) varsayılan güvenlik kuralı trafiğe izin verir. *NSG2* içinde 80 numaralı bağlantı noktasından giden trafiği reddeden bir güvenlik kuralı varsa trafik reddedilir ve *NSG1* tarafından değerlendirilmez. Sanal makinenin 80 numaralı bağlantı noktasından giden trafiği reddetmek için ağ güvenlik gruplarından birinde veya her ikisinde 80 numaralı bağlantı noktasından internete giden trafiği reddeden bir kural olması gerekir.
- **VM2**: *VM2* 'e iliştirilmiş ağ arabirimine ilişkili bir ağ güvenlik grubu olmadığından, tüm trafik alt ağa ağ arabirimi üzerinden gönderilir. *NSG1* içindeki kurallar işlenir.
- **VM3**: *NSG2* bağlantı noktası 80 ' i engelleyen bir güvenlik kuralına sahipse trafik reddedilir. *NSG2* içinde 80 numaralı bağlantı noktasından giden trafiğe izin veren bir güvenlik kuralı varsa *Subnet2* ile ilişkilendirilmiş bir ağ güvenlik grubu bulunmadığından 80 numaralı bağlantı noktasından internete giden trafiğe izin verilir.
- **VM4**: Ağ güvenlik grubu, sanal makineye bağlı ağ arabirimiyle ilişkili olmadığından veya *Subnet3*için *VM4 '* dan tüm ağ trafiğine izin verilir.

Bir ağ arabirimi için [geçerli güvenlik kurallarını](virtual-network-network-interface.md#view-effective-security-rules) görüntüleyerek bir ağ arabirimine uygulanmış olan toplu kuralları kolayca görüntüleyebilirsiniz. Azure Ağ İzleyicisi'ndeki [IP akışı doğrulama](../network-watcher/diagnose-vm-network-traffic-filtering-problem.md?toc=%2fazure%2fvirtual-network%2ftoc.json) özelliğini kullanarak da bir ağ arabirimine gelen veya dışarı giden iletişime izin verilip verilmediğini belirleyebilirsiniz. IP akışı doğrulama, iletişime izin verme veya reddetme durumunu ve trafiğe izin veren veya onu reddeden ağ güvenlik kuralının hangisi olduğunu belirler.

> [!NOTE]
> Ağ güvenlik grupları, klasik dağıtım modelinde dağıtılan alt ağlar veya sanal makineler ve bulut Hizmetleri ile ve Kaynak Yöneticisi dağıtım modelindeki alt ağlar veya ağ arabirimleri ile ilişkilendirilir. Azure dağıtım modelleri hakkında daha fazla bilgi edinmek için bkz. [Azure dağıtım modellerini kavrama](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

> [!TIP]
> Belirli bir nedeniniz yoksa bir ağ güvenlik grubunu bir alt ağ veya ağ arabirimi ile ilişkilendirmenizi ancak ikisiyle birden ilişkilendirmemenizi öneririz. Bir alt ağ ile ilişkilendirilmiş olan ağ güvenlik grubundaki kurallar bir ağ arabirimi ile ilişkilendirilmiş olan ağ güvenlik grubundaki kurallarla çakışabileceğinden çözmeniz gereken beklenmeyen iletişim sorunlarıyla karşılaşabilirsiniz.

## <a name="azure-platform-considerations"></a>Azure platformunda dikkat edilmesi gerekenler

- **Konak düğümünün sanal IP 'si**: DHCP, DNS, IMDS ve sistem durumu izleme gibi temel altyapı hizmetleri, 168.63.129.16 ve 169.254.169.254 sanallaştırılmış ana bilgisayar IP adresleri aracılığıyla sağlanır. Bu IP adresleri Microsoft 'a aittir ve tüm bölgelerde bu amaç için kullanılan tek sanallaştırılmış IP adresleridir.
- **Lisanslama (anahtar yönetimi hizmeti)** : Sanal makinelerde çalışan Windows görüntülerinin lisanslanması gerekir. Lisanslama için, lisans isteği sorgularını işleyen Anahtar Yönetimi Hizmeti ana bilgisayar sunucularına bir lisans isteği gönderilir. İstek, bağlantı noktası 1688 üzerinden gönderilir. [default route 0.0.0.0/0](virtual-networks-udr-overview.md#default-route) yapılandırması kullanan dağıtmalar için bu platform kuralı devre dışı bırakılır.
- **Yük dengeli havuzlardaki sanal makineler**: Uygulanan kaynak bağlantı noktası ve adres aralığı, yük dengeleyici değil, kaynak bilgisayardan alınır. Hedef bağlantı noktası ve adres aralığı, yük dengeleyici değil, hedef bilgisayar içindir.
- **Azure hizmet örnekleri**: HDInsight, uygulama hizmeti ortamları ve sanal makine ölçek kümeleri gibi çeşitli Azure hizmetlerinin örnekleri, sanal ağ alt ağlarında dağıtılır. Sanal ağlara dağıtabileceğiniz hizmetlerin tam listesi için bkz. [Azure hizmetleri için sanal ağ](virtual-network-for-azure-services.md#services-that-can-be-deployed-into-a-virtual-network). Bir ağ güvenlik grubunu kaynağın dağıtılmış olduğu alt ağa uygulamadan önce hizmetlerle ilgili olan bağlantı noktası gereksinimlerini öğrendiğinizden emin olun. Gerekli bağlantı noktalarını reddetmeniz halinde hizmet düzgün çalışmaz.
- **Giden e-posta gönderiliyor**: Microsoft, Azure sanal makineler 'den e-posta göndermek için kimliği doğrulanmış SMTP geçiş Hizmetleri 'ni (genellikle TCP bağlantı noktası 587 üzerinden bağlanır, ancak diğerleri de diğerleri) kullanmanızı önerir. SMTP geçiş hizmetleri, üçüncü taraf e-posta sağlayıcılarının iletileri reddetme olasılığını en aza indirmek için gönderen saygınlığı konusunda uzmanlaşmıştır. Bu tür SMTP geçiş hizmetleri Exchange Online Protection ve SendGrid'i içerir ancak bunlarla sınırlı değildir. Abonelik türünüz ne olursa olsun, Azure'da SMTP geçiş hizmetlerinin kullanımına hiçbir kısıtlama getirilmez. 

  Azure aboneliğinizi 15 Kasım 2017'den önce oluşturduysanız, SMTP geçiş hizmetlerini kullanabileceğiniz gibi, doğrudan TCP bağlantı noktası 25 üzerinden de e-posta gönderebilirsiniz. Aboneliğinizi 15 Kasım 2017'den sonra oluşturduysanız, doğrudan bağlantı noktası 25 üzerinden e-posta gönderemeyebilirsiniz. Bağlantı noktası 25 üzerinden giden iletişimin davranışı, sahip olduğunuz aboneliğe bağlıdır:

     - **Kurumsal Anlaşma**: Giden bağlantı noktası 25 iletişimine izin verilir. Azure platformundan hiçbir kısıtlama uygulanmadan, giden e-postaları doğrudan sanal makinelerden dış e-posta sağlayıcılarına gönderebilirsiniz. 
     - **Kullandıkça öde:** Tüm kaynaklardan giden bağlantı noktası 25 iletişimi engellenir. Sanal makineden doğrudan dış e-posta sağlayıcılarına (kimliği doğrulanmış SMTP geçişi kullanmadan) e-posta göndermeniz gerekirse, kısıtlamanın kaldırılması için istekte bulunabilirsiniz. İstekler gözden geçirilip Microsoft'un takdirine bağlı olarak onaylanır ve yalnızca dolandırıcılık önleme denetimleri yapıldıktan sonra kabul edilir. İstekte bulunmak için, sorun türü *Teknik*, *Sanal Ağ Bağlantısı*, *E-posta gönderilemiyor (SMTP/Bağlantı Noktası 25)* olan bir destek olayı açın. Destek olayınıza, aboneliğinizin neden kimliği doğrulanmış SMTP geçişi üzerinden değil de doğrudan posta sağlayıcılarına e-posta göndermesi gerektiği konusundaki ayrıntıları da ekleyin. Aboneliğiniz muaf tutulursa, yalnızca muafiyet tarihinden sonra oluşturulmuş sanal makineler bağlantı noktası 25 üzerinden giden iletişimi kurabilir.
     - **MSDN, Azure Pass, Open ile Azure, eğitim, BizSpark ve ücretsiz deneme**: Tüm kaynaklardan giden bağlantı noktası 25 iletişimi engellenir. Kısıtlamayı kaldırmaya yönelik istekte bulunulamaz çünkü istekler kabul edilmez. Sanal makinenizden e-posta göndermeniz gerekirse, SMTP geçiş hizmetini kullanmanız gerekir.
     - **Bulut hizmeti sağlayıcısı**: Bir bulut hizmeti sağlayıcısı aracılığıyla Azure kaynakları kullanan müşteriler, bulut hizmeti sağlayıcısıyla bir destek talebi oluşturabilir ve güvenli bir SMTP geçişi kullanılmıyorsa sağlayıcının kendi adına bir engellemeyi kaldırma çalışması oluşturmasını ister.

  Azure bağlantı noktası 25 üzerinde e-posta göndermenize izin verirse, Microsoft e-posta sağlayıcılarının sanal makinenizden gelen e-postayı kabul edeceğini garanti edemez. Belirli bir sağlayıcı sanal makinenizden gelen postayı reddederse, tüm ileti teslimi veya istenmeyen posta filtreleme sorunlarını çözmek için doğrudan sağlayıcıyla çalışmanız veya kimliği doğrulanmış SMTP geçiş hizmeti kullanın.

## <a name="next-steps"></a>Sonraki adımlar

* [Ağ güvenlik grubu oluşturmayı](tutorial-filter-network-traffic.md) öğrenin.
