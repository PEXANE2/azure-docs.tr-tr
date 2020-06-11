---
title: Sanal ağlar ile Azure API Management’ı kullanma
description: Azure API Management 'de bir sanal ağa bağlantı ayarlamayı ve üzerinden Web hizmetlerine erişmeyi öğrenin.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/10/2020
ms.author: apimpm
ms.custom: references_regions
ms.openlocfilehash: 76107a3713a7570bc3bbca15aa1b47e76560bf66
ms.sourcegitcommit: f01c2142af7e90679f4c6b60d03ea16b4abf1b97
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84674287"
---
# <a name="how-to-use-azure-api-management-with-virtual-networks"></a>Sanal ağlar ile Azure API Management’ı kullanma
Azure Sanal Ağları (VNET’ler) Azure kaynaklarınızdan herhangi birini, erişimini denetlediğiniz İnternet tabanlı olmayan ve yönlendirilebilir bir ağa yerleştirmenizi sağlar. Bu ağlar daha sonra, çeşitli VPN teknolojileri kullanılarak şirket içi ağlarınıza bağlanabilir. Azure sanal ağları hakkında daha fazla bilgi edinmek için buradaki bilgilerle başlayın: [Azure sanal ağına genel bakış](../virtual-network/virtual-networks-overview.md).

Azure API Management, sanal ağ (VNET) içinde dağıtılabilir ve bu sayede ağ içindeki arka uç hizmetlerine erişebilir. Geliştirici portalı ve API ağ geçidi, Internet 'ten ya da yalnızca sanal ağ içinden erişilebilir olacak şekilde yapılandırılabilir.

> [!NOTE]
> API içeri aktarma belgesi URL 'SI, genel olarak erişilebilen bir internet adresinde barındırılmalıdır.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [premium-dev.md](../../includes/api-management-availability-premium-dev.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu makalede açıklanan adımları gerçekleştirmek için şunları yapmanız gerekir:

+ Etkin bir Azure aboneliği.

    [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

+ Bir APıM örneği. Daha fazla bilgi için bkz. [Azure API Management örneği oluşturma](get-started-create-service-instance.md).

## <a name="enable-vnet-connection"></a><a name="enable-vpn"> </a>VNET bağlantısını etkinleştir

### <a name="enable-vnet-connectivity-using-the-azure-portal"></a>Azure portal kullanarak VNET bağlantısını etkinleştirme

1. API Management örneğinizi bulmak için [Azure Portal](https://portal.azure.com) gidin. **API Management Hizmetleri**arayın ve seçin.

2. API Management örneğinizi seçin.

3. **Sanal ağ**' ı seçin.
4. API Management örneğini bir sanal ağın içine dağıtılacak şekilde yapılandırın.

    ![API Management sanal ağ menüsü][api-management-using-vnet-menu]
5. İstenen erişim türünü seçin:

    * **Kapalı**: bu varsayılandır. API Management bir sanal ağa dağıtılmadı.

    * **Harici**: API Management ağ geçidi ve geliştirici portalına bir dış yük dengeleyici aracılığıyla genel İnternet üzerinden erişilebilir. Ağ Geçidi, sanal ağ içindeki kaynaklara erişebilir.

        ![Ortak eşleme][api-management-vnet-public]

    * **İç**: API Management ağ geçidi ve geliştirici portalına yalnızca iç yük dengeleyici aracılığıyla sanal ağ içinden erişilebilir. Ağ Geçidi, sanal ağ içindeki kaynaklara erişebilir.

        ![Özel eşleme][api-management-vnet-private]

6. **Dış** veya **iç**' yı seçtiyseniz, API Management hizmetinizin sağlandığı tüm bölgelerin bir listesini görürsünüz. Bir **konum**seçin ve ardından **sanal ağını** ve **alt ağını**seçin. Sanal ağ listesi, yapılandırdığınız bölgede ayarlanmış olan Azure aboneliklerinizde bulunan hem klasik hem de Kaynak Yöneticisi sanal ağlarla doldurulur.

    > [!IMPORTANT]
    > Bir Azure API Management örneğini bir Kaynak Yöneticisi VNET 'e dağıttığınızda, hizmet Azure API Management örnekleri dışında başka hiçbir kaynak içermeyen bir ayrılmış alt ağda olmalıdır. Azure API Management örneğini diğer kaynakları içeren bir Kaynak Yöneticisi VNET alt ağına dağıtmaya yönelik bir girişim yapılırsa, dağıtım başarısız olur.

    Ardından **Uygula**'yı seçin. API Management örneğinizin **sanal ağ** sayfası, yeni sanal ağ ve alt ağ seçimlerinizle güncelleştirilir.

    ![VPN seçin][api-management-setup-vpn-select]

7. Üst gezinti çubuğunda **Kaydet**' i ve ardından **ağ yapılandırması Uygula**' yı seçin.

> [!NOTE]
> API Management örneğinin VIP adresi, VNET 'in her etkin veya devre dışı bırakıldığı her seferinde değişecektir.
> API Management **dış** sunucudan **iç**veya tam tersi yönde taşındığında VIP adresi de değişecektir.
>

> [!IMPORTANT]
> VNET 'ten API Management kaldırır veya içinde dağıtıldığı bir değişikliği değiştirirseniz, daha önce kullanılmış olan VNET, altı saate kadar kilitli kalabilir. Bu süre boyunca VNET 'i silmek veya yeni bir kaynak dağıtmak mümkün olmayacaktır. Bu davranış, API-sürüm 2018-01-01 ve öncesini kullanan istemciler için geçerlidir. API-sürüm 2019-01-01 ve üzeri kullanan istemciler, ilişkili API Management hizmeti silindikten hemen sonra sanal ağ serbest bırakılır.

## <a name="enable-vnet-connection-using-powershell-cmdlets"></a><a name="enable-vnet-powershell"> </a>PowerShell cmdlet 'LERINI kullanarak VNET bağlantısını etkinleştirme
Ayrıca, PowerShell cmdlet 'lerini kullanarak VNET bağlantısını etkinleştirebilirsiniz.

* **VNET içinde API Management hizmeti oluşturma**: sanal ağ Içinde bir Azure API Management hizmeti oluşturmak için [New-Azapimana,](/powershell/module/az.apimanagement/new-azapimanagement) cmdlet 'ini kullanın.

* **Mevcut bir API Management HIZMETINI VNET Içinde dağıtma**: var olan bir Azure API Management hizmetini bir sanal ağ içinde taşımak için [Update-AzApiManagementRegion](/powershell/module/az.apimanagement/update-azapimanagementregion) cmdlet 'ini kullanın.

## <a name="connect-to-a-web-service-hosted-within-a-virtual-network"></a><a name="connect-vnet"> </a>Sanal ağ içinde barındırılan bir Web hizmetine bağlanma
API Management hizmetiniz VNET 'e bağlandıktan sonra, içindeki arka uç hizmetlerine erişmek, ortak hizmetlere erişmeden farklı değildir. Yeni bir API oluştururken veya var olan bir API 'yi düzenlediğinizde, Web hizmetinizin Web hizmeti **URL 'si** ALANıNA yerel IP adresini veya ana bilgisayar adını (VNet IÇIN bir DNS sunucusu yapılandırılmışsa) yazmanız yeterlidir.

![VPN 'den API ekleme][api-management-setup-vpn-add-api]

## <a name="common-network-configuration-issues"></a><a name="network-configuration-issues"> </a>Ortak ağ yapılandırma sorunları
Aşağıda, API Management hizmeti bir sanal ağa dağıttığınızda oluşabilecek yaygın yanlış yapılandırma sorunlarının bir listesi verilmiştir.

* **Özel DNS sunucusu kurulumu**: API Management hizmeti, birkaç Azure hizmetine bağlıdır. API Management, özel bir DNS sunucusu olan bir VNET 'te barındırılıyorsa, bu Azure hizmetlerinin ana bilgisayar adlarını çözmesi gerekir. Lütfen özel DNS kurulumu için [Bu](../virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-that-uses-your-own-dns-server) yönergeleri izleyin. Başvuru için aşağıdaki bağlantı noktaları tablosuna ve diğer ağ gereksinimlerine bakın.

> [!IMPORTANT]
> VNET için özel bir DNS sunucusu kullanmayı planlıyorsanız, API Management bir hizmeti dağıtmaya **başlamadan önce** onu ayarlamanız gerekir. Aksi takdirde, [ağ yapılandırmasını Uygula işlemini](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/ApiManagementService/ApplyNetworkConfigurationUpdates) çalıştırarak DNS sunucusunu her değiştirişinizde API Management hizmetini güncelleştirmeniz gerekir

* **API Management için gereken bağlantı noktaları**: gelen ve giden trafik, API Management dağıtıldığı alt ağa [ağ güvenlik grubu][Network Security Group]kullanılarak denetlenebilir. Bu bağlantı noktalarından herhangi biri kullanılamıyorsa API Management düzgün çalışmayabilir ve erişilemez hale gelebilir. Bu bağlantı noktalarından biri veya daha fazlası engelleniyorsa, VNET ile API Management kullanırken başka bir yaygın yanlış yapılandırma sorunu vardır.

<a name="required-ports"> </a> Bir API Management hizmet ÖRNEĞI bir sanal ağda barındırıldığı zaman, aşağıdaki tablodaki bağlantı noktaları kullanılır.

| Kaynak/hedef bağlantı noktaları | Yön          | Aktarım Protokolü |   [Hizmet etiketleri](../virtual-network/security-overview.md#service-tags) <br> Kaynak/hedef   | Amaç ( \* )                                                 | Sanal ağ türü |
|------------------------------|--------------------|--------------------|---------------------------------------|-------------------------------------------------------------|----------------------|
| */[80], 443                  | Gelen            | TCP                | ıNTERNET/VIRTUAL_NETWORK            | API Management istemci iletişimi                      | Dış             |
| */3443                     | Gelen            | TCP                | Apimanaya/VIRTUAL_NETWORK       | Azure portal ve PowerShell için yönetim uç noktası         | Dış & Iç  |
| */443                  | Giden           | TCP                | VIRTUAL_NETWORK/depolama             | **Azure Storage bağımlılığı**                             | Dış & Iç  |
| */443                  | Giden           | TCP                | VIRTUAL_NETWORK/AzureActiveDirectory | [Azure Active Directory](api-management-howto-aad.md) (uygun olduğunda)                   | Dış & Iç  |
| */1433                     | Giden           | TCP                | VIRTUAL_NETWORK/SQL                 | **Azure SQL uç noktalarına erişim**                           | Dış & Iç  |
| */5671, 5672, 443          | Giden           | TCP                | VIRTUAL_NETWORK/EventHub            | [Olay Hub 'ı ilkesine](api-management-howto-log-event-hubs.md) ve Izleme aracısına günlük bağımlılığı | Dış & Iç  |
| */445                      | Giden           | TCP                | VIRTUAL_NETWORK/depolama             | [GIT](api-management-configuration-repository-git.md) Için Azure dosya paylaşımında bağımlılık                      | Dış & Iç  |
| */443                     | Giden           | TCP                | VIRTUAL_NETWORK/Azurecyüksek            | Durum ve Izleme uzantısı         | Dış & Iç  |
| */1886, 443                     | Giden           | TCP                | VIRTUAL_NETWORK/AzureMonitor         | [Tanılama günlüklerini ve ölçümleri](api-management-howto-use-azure-monitor.md) yayımlama ve [kaynak durumu](../service-health/resource-health-overview.md)                     | Dış & Iç  |
| */25, 587, 25028                       | Giden           | TCP                | VIRTUAL_NETWORK/INTERNET            | E-posta göndermek için SMTP geçişine Bağlan                    | Dış & Iç  |
| */6381-6383              | Gelen & giden | TCP                | VIRTUAL_NETWORK/VIRTUAL_NETWORK     | Makineler arasındaki [önbellek](api-management-caching-policies.md) Ilkeleri için Redsıs hizmetine erişme         | Dış & Iç  |
| */4290              | Gelen & giden | UDP                | VIRTUAL_NETWORK/VIRTUAL_NETWORK     | Makineler arasındaki [hız limiti](api-management-access-restriction-policies.md#LimitCallRateByKey) ilkeleri Için eşitleme sayaçları         | Dış & Iç  |
| * / *                        | Gelen            | TCP                | AZURE_LOAD_BALANCER/VIRTUAL_NETWORK | Azure altyapı Load Balancer                          | Dış & Iç  |

>[!IMPORTANT]
> API Management hizmetin başarıyla dağıtılması için *Purpose* , amacının **kalın** olduğu bağlantı noktaları gereklidir. Diğer bağlantı noktalarını engellemek, **çalışan hizmeti kullanma ve izleme ve taahhüt EDILEN SLA sağlama**yeteneğinin **azalmasına** neden olur.

+ **TLS işlevselliği**: TLS/SSL sertifika zinciri oluşturma ve doğrulamayı etkinleştirmek için API Management hizmeti, ocsp.msocsp.com, mscrl.microsoft.com ve CRL.Microsoft.com giden ağ bağlantısı gerektirir. API Management karşıya yüklediğiniz herhangi bir sertifika, CA köküne tam zinciri içeriyorsa, bu bağımlılık gerekli değildir.

+ **DNS erişimi**: bağlantı noktası 53 üzerinde giden ERIŞIM, DNS sunucularıyla iletişim için gereklidir. Bir VPN ağ geçidinin diğer ucunda özel bir DNS sunucusu varsa, DNS sunucusuna API Management barındıran alt ağdan erişilebilir olması gerekir.

+ **Ölçümler ve sistem durumu izleme**: Azure izleme uç noktalarına giden ağ bağlantısı, aşağıdaki etki alanları altında çözümlenir. Tabloda gösterildiği gibi, bu URL 'Ler ağ güvenlik gruplarıyla kullanılmak üzere AzureMonitor Service etiketi altında temsil edilir.

    | Azure ortamı | Uç Noktalar                                                                                                                                                                                                                                                                                                                                                              |
    |-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | Azure Genel      | <ul><li>gcs.prod.monitoring.core.windows.net (**Yeni**)</li><li>prod.warmpath.msftcloudes.com (**kullanım dışı**)</li><li>global.prod.microsoftmetrics.com (**Yeni**)</li><li>global.metrics.nsatc.net (**kullanım dışı**)</li><li>shoebox2.prod.microsoftmetrics.com (**Yeni**)</li><li>shoebox2.metrics.nsatc.net (**kullanım dışı**)</li><li>shoebox2-red.prod.microsoftmetrics.com</li><li>shoebox2-black.prod.microsoftmetrics.com</li><li>shoebox2-red.shoebox2.metrics.nsatc.net</li><li>shoebox2-black.shoebox2.metrics.nsatc.net</li><li>prod3.prod.microsoftmetrics.com (**Yeni**)</li><li>prod3.metrics.nsatc.net (**kullanım dışı**)</li><li>prod3-black.prod.microsoftmetrics.com (**Yeni**)</li><li>prod3-black.prod3.metrics.nsatc.net (**kullanım dışı**)</li><li>prod3-red.prod.microsoftmetrics.com (**Yeni**)</li><li>prod3-red.prod3.metrics.nsatc.net (**kullanım dışı**)</li><li>gcs.prod.warm.ingestion.monitoring.azure.com</li></ul> |
    | Azure Kamu  | <ul><li>fairfax.warmpath.usgovcloudapi.net</li><li>global.prod.microsoftmetrics.com (**Yeni**)</li><li>global.metrics.nsatc.net (**kullanım dışı**)</li><li>shoebox2.prod.microsoftmetrics.com (**Yeni**)</li><li>shoebox2.metrics.nsatc.net (**kullanım dışı**)</li><li>shoebox2-red.prod.microsoftmetrics.com</li><li>shoebox2-black.prod.microsoftmetrics.com</li><li>shoebox2-red.shoebox2.metrics.nsatc.net</li><li>shoebox2-black.shoebox2.metrics.nsatc.net</li><li>prod3.prod.microsoftmetrics.com (**Yeni**)</li><li>prod3.metrics.nsatc.net (**kullanım dışı**)</li><li>prod3-black.prod.microsoftmetrics.com</li><li>prod3-red.prod.microsoftmetrics.com</li><li>prod5.prod.microsoftmetrics.com</li><li>prod5-black.prod.microsoftmetrics.com</li><li>prod5-red.prod.microsoftmetrics.com</li><li>gcs.prod.warm.ingestion.monitoring.azure.us</li></ul>                                                                                                                                                                                                                                                |
    | Azure China 21Vianet     | <ul><li>mooncake.warmpath.chinacloudapi.cn</li><li>global.prod.microsoftmetrics.com (**Yeni**)</li><li>global.metrics.nsatc.net (**kullanım dışı**)</li><li>shoebox2.prod.microsoftmetrics.com (**Yeni**)</li><li>shoebox2.metrics.nsatc.net (**kullanım dışı**)</li><li>shoebox2-red.prod.microsoftmetrics.com</li><li>shoebox2-black.prod.microsoftmetrics.com</li><li>shoebox2-red.shoebox2.metrics.nsatc.net</li><li>shoebox2-black.shoebox2.metrics.nsatc.net</li><li>prod3.prod.microsoftmetrics.com (**Yeni**)</li><li>prod3.metrics.nsatc.net (**kullanım dışı**)</li><li>prod3-black.prod.microsoftmetrics.com</li><li>prod3-red.prod.microsoftmetrics.com</li><li>prod5.prod.microsoftmetrics.com</li><li>prod5-black.prod.microsoftmetrics.com</li><li>prod5-red.prod.microsoftmetrics.com</li><li>gcs.prod.warm.ingestion.monitoring.azure.cn</li></ul>                                                                                                                                                                                                                                                |

  >[!IMPORTANT]
  > DNS bölgesi **. nsatc.net** to **. microsoftmetrics.com** ile yukarıdaki kümelerin DEĞIŞIKLIĞI, genellikle bir DNS değişiklidir. Kümenin IP adresi değişmeyecektir.

+ **Bölgesel hizmet etiketleri**: bir depolama, SQL ve Event Hubs hizmet etiketlerine giden bağlantıya izin veren NSG kuralları, API Management örneğini içeren bölgeye karşılık gelen etiketlerin bölgesel sürümlerini kullanabilir (örneğin, Batı ABD bölgesindeki bir API Management örneği için Storage. WestUS). Çok bölgeli dağıtımlarda, her bölgedeki NSG bu bölge ve birincil bölge için hizmet etiketlerine giden trafiğe izin verilmelidir.

+ **SMTP geçişi**: konak `smtpi-co1.msn.com` , `smtpi-ch1.msn.com` , `smtpi-db3.msn.com` `smtpi-sin.msn.com` ve altında çözümlenen SMTP geçişi için giden ağ bağlantısı`ies.global.microsoft.com`

+ **Geliştirici PORTALı CAPTCHA**: Geliştirici portalının CAPTCHA için giden ağ bağlantısı, konaklar ve altında çözümleniyor `client.hip.live.com` `partner.hip.live.com` .

+ **Tanılama Azure Portal**: Azure Portal bir sanal ağ içinden API Management uzantısı kullanılırken tanılama günlüklerinin akışını etkinleştirmek için, `dc.services.visualstudio.com` 443 numaralı bağlantı noktasına giden erişim gerekir. Bu, uzantıyı kullanırken karşılaşabileceğiniz sorunları gidermeye yardımcı olur.

+ **Azure Load Balancer**: hizmet etiketinin gelen istek için izin verme, bu, `AZURE_LOAD_BALANCER` `Developer` arkasında yalnızca bir işlem birimi dağıttiğimiz için SKU için bir gereksinim değildir. Ancak [168.63.129.16](../virtual-network/what-is-ip-address-168-63-129-16.md) `Premium` , Load Balancer ' den gelen sistem durumu araştırmasının başarısız olması nedeniyle, 168.63.129.16 from, daha yüksek SKU 'ya ölçeklendirilirken kritik hale gelir.

+ **Express Route veya ağ sanal gereci kullanarak tünel trafiğini şirket Içi güvenlik duvarında zorla**: ortak bir müşteri yapılandırması, API Management atanmış alt ağdan gelen tüm trafiği şirket içi bir güvenlik duvarı veya ağ sanal gereci üzerinden akışa zorlayan kendi varsayılan yolunu (0.0.0.0/0) tanımlamaktır. Giden trafik, şirket içi veya NAT 'ın, artık çeşitli Azure uç noktalarıyla çalışmayan tanınmayan bir adres kümesine engellediği için bu trafik akışı, Azure API Management ile bağlantıyı keser. Çözüm için birkaç şey yapmanız gerekir:

  * API Management hizmetinin dağıtıldığı alt ağda hizmet uç noktalarını etkinleştirin. Azure SQL, Azure depolama, Azure EventHub ve Azure ServiceBus için [hizmet uç noktalarının][ServiceEndpoints] etkinleştirilmesi gerekir. Uç noktaların bu hizmetlere doğrudan API Management atanmış alt ağdan etkinleştirilmesi, hizmet trafiği için en iyi yönlendirmeyi sağlayan Microsoft Azure omurga ağını kullanmasına izin verir. Hizmet uç noktalarını zorlamalı bir tünel oluşturma API Yönetimi ile birlikte kullanıyorsanız, yukarıdaki Azure hizmetleri trafiğine Zorlamalı tünel sağlanmaz. Diğer API Management hizmet bağımlılığı trafiği Zorlamalı tünel yapılabilir ve kaybedilmez veya API Management hizmeti düzgün şekilde çalışmaz.
    
  * Internet 'ten gelen tüm denetim düzlemi trafiği, API Management hizmetinizin yönetim uç noktasına, API Management tarafından barındırılan belirli bir gelen IP kümesiyle yönlendirilir. Trafiğe Zorlamalı tünel yapıldığında yanıtlar, bu gelen kaynak IP 'lerine simetrik bir şekilde geri eşleşmeyecektir. Sınırlamanın üstesinden gelmek için, bu konak yollarının hedefini "Internet" olarak ayarlayarak aşağıdaki Kullanıcı tanımlı yolları ([udrs][UDRs]) Azure 'a geri ekleyebilmemiz gerekir. Denetim düzlemi trafiği için gelen IP 'lerin kümesi, [Denetim DÜZLEMI IP adresleri](#control-plane-ips) olarak belgelenmiştir

  * Zorlamalı tünel sağlayan diğer API Management hizmet bağımlılıkları için, ana bilgisayar adını çözümlemek ve uç noktaya ulaşmak için bir yol olmalıdır. Bunlar şunlardır
      - Ölçümler ve sistem durumu Izleme
      - Azure portal tanılama
      - SMTP geçişi
      - Geliştirici portalı CAPTCHA

## <a name="troubleshooting"></a><a name="troubleshooting"> </a>Sorun giderme
* **Ilk kurulum**: API Management hizmetinin bir alt ağa ilk dağıtımı başarısız olduğunda, önce bir sanal makineyi aynı alt ağa dağıtmanız önerilir. Sanal makineye sonraki uzak masaüstü ve Azure aboneliğinizde aşağıdaki her bir kaynaktan birine bağlantı olduğunu doğrulama
    * Azure Depolama Blobu
    * Azure SQL Veritabanı
    * Azure depolama tablosu

  > [!IMPORTANT]
  > Bağlantıyı doğruladıktan sonra, alt ağa API Management dağıtılmadan önce alt ağda dağıtılan tüm kaynakları kaldırdığınızdan emin olun.

* **Artımlı güncelleştirmeler**: ağınızda değişiklik yaparken, API Management hizmetinin bağlı olduğu kritik kaynaklara erişimi kaybetmediğinden emin olmak Için [networkstatus API](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/networkstatus)'sine bakın. Bağlantı durumu 15 dakikada bir güncelleştirilir.

* **Kaynak Gezinti bağlantıları**: Kaynak Yöneticisi Style VNET alt ağına dağıtım yaparken, API Management bir kaynak gezinti bağlantısı oluşturarak alt ağı ayırır. Alt ağ, farklı bir sağlayıcıdan bir kaynağı zaten içeriyorsa, dağıtım **başarısız**olur. Benzer şekilde, bir API Management hizmetini farklı bir alt ağa taşıdığınızda veya sildiğinizde, bu kaynak gezinti bağlantısını kaldıracağız.

## <a name="subnet-size-requirement"></a><a name="subnet-size"> </a> Alt ağ boyutu gereksinimi
Azure, bazı IP adreslerini her alt ağ içinde ayırır ve bu adresler kullanılamaz. Alt ağların ilk ve son IP adresleri protokol uyumu için ayrılmıştır ve Azure hizmetleri için kullanılan üç adres daha vardır. Daha fazla bilgi için bkz. [Bu alt AĞLARDAKI IP adreslerini kullanma konusunda herhangi bir kısıtlama var mı?](../virtual-network/virtual-networks-faq.md#are-there-any-restrictions-on-using-ip-addresses-within-these-subnets)

Azure VNET altyapısı tarafından kullanılan IP adreslerine ek olarak, alt ağdaki her API Yönetimi örneği, geliştirici SKU 'su için her bir Premium SKU veya bir IP adresi birimi başına iki IP adresi kullanır. Her örnek, dış yük dengeleyici için ek bir IP adresi ayırır. Iç sanal ağa dağıtım yaparken, iç yük dengeleyici için ek bir IP adresi gerektirir.

Dört adet kullanılabilir IP adresi sağlayan API Management dağıtılabilecek olan alt ağın en düşük boyutunun üzerinde hesaplama verilme.

Her ek ölçek birimi API Management için iki IP adresi gerekir.

## <a name="routing"></a><a name="routing"> </a> Yönlendirme
+ Tüm hizmet uç noktalarına erişim sağlamak için, yük dengeli bir genel IP adresi (VIP) ayrılır.
+ VNET içindeki kaynaklara erişmek için bir alt ağ IP aralığından (DIP) bir IP adresi kullanılır ve VNET dışındaki kaynaklara erişmek için bir genel IP adresi (VIP) kullanılır.
+ Yük dengeli genel IP adresi, Azure portal genel bakış/Essentials dikey penceresinde bulunabilir.

## <a name="limitations"></a><a name="limitations"> </a>Sınırlamalar
* API Management örnekleri içeren bir alt ağ başka hiçbir Azure Kaynak türünü içeremez.
* Alt ağ ve API Management hizmeti aynı abonelikte olmalıdır.
* API Management örnekleri içeren bir alt ağ abonelikler arasında taşınamaz.
* Dahili sanal ağ modunda yapılandırılan çok bölgeli API Management dağıtımlarda, kullanıcılar, yönlendirmeye sahip oldukları gibi birden çok bölgede yük dengelemeyi yönetmekten sorumludur.
* Başka bir bölgedeki genel olarak eşlenmiş VNET 'teki bir kaynağın Iç moddaki API Management hizmete bağlantısı, platform sınırlaması nedeniyle çalışmayacak. Daha fazla bilgi için bkz. [bir sanal ağdaki kaynaklar, eşlenen sanal ağdaki Azure iç yük dengeleyici ile iletişim](../virtual-network/virtual-network-manage-peering.md#requirements-and-constraints) kuramıyor

## <a name="control-plane-ip-addresses"></a><a name="control-plane-ips"> </a> DENETIM düzlemi IP adresleri

IP adresleri **Azure ortamı**tarafından bölünür. Gelen isteklere izin verilmesi durumunda **genel** Ile işaretlenen IP adresinin, **bölgeye** özgü IP adresiyle birlikte beyaz listelenmesi gerekir.

| **Azure ortamı**|   **Geli**|  **IP adresi**|
|-----------------|-------------------------|---------------|
| Azure Genel| Orta Güney ABD (genel)| 104.214.19.224|
| Azure Genel| Orta Kuzey ABD (genel)| 52.162.110.80|
| Azure Genel| Orta Batı ABD| 52.253.135.58|
| Azure Genel| Güney Kore - Orta| 40.82.157.167|
| Azure Genel| Batı Birleşik Krallık| 51.137.136.0|
| Azure Genel| Batı Japonya| 40.81.185.8|
| Azure Genel| Orta Kuzey ABD| 40.81.47.216|
| Azure Genel| Güney Birleşik Krallık| 51.145.56.125|
| Azure Genel| Batı Hindistan| 40.81.89.24|
| Azure Genel| Doğu ABD| 52.224.186.99|
| Azure Genel| Batı Avrupa| 51.145.179.78|
| Azure Genel| Doğu Japonya| 52.140.238.179|
| Azure Genel| Orta Fransa| 40.66.60.111|
| Azure Genel| Doğu Kanada| 52.139.80.117|
| Azure Genel| BAE Kuzey| 20.46.144.85|
| Azure Genel| Güney Brezilya| 191.233.24.179|
| Azure Genel| Güneydoğu Asya| 40.90.185.46|
| Azure Genel| Güney Afrika Kuzey| 102.133.130.197|
| Azure Genel| Orta Kanada| 52.139.20.34|
| Azure Genel| Güney Kore - Güney| 40.80.232.185|
| Azure Genel| Orta Hindistan| 13.71.49.1|
| Azure Genel| Batı ABD| 13.64.39.16|
| Azure Genel| Güneydoğu Avustralya| 20.40.160.107|
| Azure Genel| Orta Avustralya| 20.37.52.67|
| Azure Genel| Güney Hindistan| 20.44.33.246|
| Azure Genel| Orta ABD| 13.86.102.66|
| Azure Genel| Doğu Avustralya| 20.40.125.155|
| Azure Genel| Batı ABD 2| 51.143.127.203|
| Azure Genel| EUAP Doğu ABD 2| 52.253.229.253|
| Azure Genel| EUAP Orta ABD| 52.253.159.160|
| Azure Genel| Orta Güney ABD| 20.188.77.119|
| Azure Genel| Doğu ABD 2| 20.44.72.3|
| Azure Genel| Kuzey Avrupa| 52.142.95.35|
| Azure Genel| Doğu Asya| 52.139.152.27|
| Azure Genel| Güney Fransa| 20.39.80.2|
| Azure Genel| İsviçre Batı| 51.107.96.8|
| Azure Genel| Orta Avustralya 2| 20.39.99.81|
| Azure Genel| BAE Orta| 20.37.81.41|
| Azure Genel| İsviçre Kuzey| 51.107.0.91|
| Azure Genel| Güney Afrika Batı| 102.133.0.79|
| Azure Genel| Almanya Orta Batı| 51.116.96.0|
| Azure Genel| Almanya Kuzey| 51.116.0.0|
| Azure Genel| Norveç Doğu| 51.120.2.185|
| Azure Genel| Norveç Batı| 51.120.130.134|
| Azure China 21Vianet| Çin Kuzey (genel)| 139.217.51.16|
| Azure China 21Vianet| Çin Doğu (genel)| 139.217.171.176|
| Azure China 21Vianet| Kuzey Çin| 40.125.137.220|
| Azure China 21Vianet| Doğu Çin| 40.126.120.30|
| Azure China 21Vianet| Çin Kuzey 2| 40.73.41.178|
| Azure China 21Vianet| Çin Doğu 2| 40.73.104.4|
| Azure Kamu| USGov Virginia (genel)| 52.127.42.160|
| Azure Kamu| USGov Texas (genel)| 52.127.34.192|
| Azure Kamu| USGov Virginia| 52.227.222.92|
| Azure Kamu| USGov Iowa| 13.73.72.21|
| Azure Kamu| USGov Arizona| 52.244.32.39|
| Azure Kamu| USGov Texas| 52.243.154.118|
| Azure Kamu| USDoD orta| 52.182.32.132|
| Azure Kamu| USDoD Doğu| 52.181.32.192|

## <a name="related-content"></a><a name="related-content"> </a>İlgili içerik
* [VPN ağ geçidi kullanarak bir sanal ağı arka uca bağlama](../vpn-gateway/design.md#s2smulti)
* [Farklı dağıtım modellerinden bir sanal ağ bağlama](../vpn-gateway/vpn-gateway-connect-different-deployment-models-powershell.md)
* [Azure API Management çağrıları izlemek için API denetçisini kullanma](api-management-howto-api-inspector.md)
* [Sanal ağ hakkında sık sorulan sorular](../virtual-network/virtual-networks-faq.md)
* [Hizmet etiketleri](../virtual-network/security-overview.md#service-tags)

[api-management-using-vnet-menu]: ./media/api-management-using-with-vnet/api-management-menu-vnet.png
[api-management-setup-vpn-select]: ./media/api-management-using-with-vnet/api-management-using-vnet-select.png
[api-management-setup-vpn-add-api]: ./media/api-management-using-with-vnet/api-management-using-vnet-add-api.png
[api-management-vnet-private]: ./media/api-management-using-with-vnet/api-management-vnet-internal.png
[api-management-vnet-public]: ./media/api-management-using-with-vnet/api-management-vnet-external.png

[Enable VPN connections]: #enable-vpn
[Connect to a web service behind VPN]: #connect-vpn
[Related content]: #related-content

[UDRs]: ../virtual-network/virtual-networks-udr-overview.md
[Network Security Group]: ../virtual-network/security-overview.md
[ServiceEndpoints]: ../virtual-network/virtual-network-service-endpoints-overview.md
[ServiceTags]: ../virtual-network/security-overview.md#service-tags
