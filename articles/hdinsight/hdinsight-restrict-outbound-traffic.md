---
title: Giden ağ trafiği kısıtlamasını Yapılandırma-Azure HDInsight
description: Azure HDInsight kümeleri için giden ağ trafiği kısıtlamasını yapılandırma hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/23/2019
ms.openlocfilehash: 6771cdb206920c8e3b746e28573de1742543b4c8
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75646702"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall"></a>Güvenlik duvarını kullanarak Azure HDInsight kümeleri için giden ağ trafiği yapılandırma

Bu makalede, Azure Güvenlik Duvarı 'nı kullanarak HDInsight kümenizdeki giden trafiği güvenli hale getirmeye yönelik adımlar sağlanmaktadır. Aşağıdaki adımlarda, var olan bir küme için bir Azure Güvenlik Duvarı yapılandırdığınızı varsaymaktadır. Yeni bir küme dağıtıyorsanız ve güvenlik duvarının arkasındaysa, önce HDInsight kümenizi ve alt ağını oluşturun ve ardından bu kılavuzdaki adımları izleyin.

## <a name="background"></a>Arka plan

Azure HDInsight kümeleri normalde kendi sanal ağınıza dağıtılır. Kümede, ağ erişiminin düzgün çalışması için bu sanal ağın dışındaki hizmetlere bağımlılıkları vardır.

Gelen trafik gerektiren birkaç bağımlılık vardır. Gelen yönetim trafiği bir güvenlik duvarı cihazından gönderilemez. Bu trafiğin kaynak adresleri bilinmektedir ve [burada](hdinsight-management-ip-addresses.md)yayımlanır. Ayrıca, kümelerdeki gelen trafiğin güvenliğini sağlamak için bu bilgilerle ağ güvenlik grubu (NSG) kuralları da oluşturabilirsiniz.

HDInsight giden trafik bağımlılıkları, her geride statik IP adreslerine sahip olmayan FQDN 'Ler ile neredeyse tamamen tanımlanmıştır. Statik adreslerin olmaması, ağ güvenlik gruplarının (NSG 'ler) bir kümeden giden trafiği kilitlemek için kullanılamayacağı anlamına gelir. Adresler, geçerli ad çözümlemesine göre kuralları ayarlayamayacak ve NSG kurallarını ayarlamak için bu kuralın kullanılmasına yetecek kadar sık değişir.

Giden adreslerin güvenliğini sağlamaya yönelik çözüm, etki alanı adlarına göre giden trafiği denetleyebilirler bir güvenlik duvarı cihazı kullanmaktır. Azure Güvenlik Duvarı, giden HTTP ve HTTPS trafiğini hedef veya [FQDN ETIKETLERININ](https://docs.microsoft.com/azure/firewall/fqdn-tags)FQDN 'sine göre kısıtlayabilir.

## <a name="configuring-azure-firewall-with-hdinsight"></a>HDInsight ile Azure Güvenlik duvarını yapılandırma

Azure Güvenlik Duvarı ile mevcut HDInsight 'ınızdan çıkış kilitleme adımlarının Özeti şunlardır:

1. Güvenlik duvarı oluşturun.
1. Güvenlik duvarına uygulama kuralları ekleme
1. Güvenlik duvarına ağ kuralları ekleyin.
1. Bir yönlendirme tablosu oluşturun.

### <a name="create-new-subnet"></a>Yeni alt ağ oluştur

Kümenizin bulunduğu sanal ağda **AzureFirewallSubnet** adlı bir alt ağ oluşturun.

### <a name="create-a-new-firewall-for-your-cluster"></a>Kümeniz için yeni bir güvenlik duvarı oluşturma

**Test-FW01** adlı bir güvenlik duvarı oluşturarak **güvenlik** duvarını [öğreticiden dağıtma: Azure Portal kullanarak Azure Güvenlik Duvarı dağıtma ve yapılandırma](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall)adımlarını uygulayın.

### <a name="configure-the-firewall-with-application-rules"></a>Uygulama kurallarıyla güvenlik duvarını yapılandırma

Kümenin önemli iletişimleri göndermesini ve almasını sağlayan bir uygulama kuralı koleksiyonu oluşturun.

1. Azure portal yeni güvenlik duvarını **Test-FW01** seçin.

1. **Ayarlar** > **kurallar** > **uygulama kuralı koleksiyonu** >  **+ uygulama kuralı koleksiyonu Ekle**' ye gidin.

    ![Başlık: uygulama kuralı koleksiyonu Ekle](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

1. **Uygulama kuralı koleksiyonu Ekle** ekranında, aşağıdaki bilgileri sağlayın:

    **Üst bölüm**

    | Özellik|  Değer|
    |---|---|
    |Ad| FwAppRule|
    |Öncelik|200|
    |Eylem|Allow|

    **FQDN etiketleri bölümü**

    | Ad | Kaynak adres | FQDN etiketi | Notlar |
    | --- | --- | --- | --- |
    | Rule_1 | * | WindowsUpdate ve HDInsight | HDI Hizmetleri için gerekli |

    **Hedef FQDN bölümü**

    | Ad | Kaynak adresler | Protokol: bağlantı noktası | Hedef FQDN 'ler | Notlar |
    | --- | --- | --- | --- | --- |
    | Rule_2 | * | https: 443 | login.windows.net | Windows oturum açma etkinliğine izin verir |
    | Rule_3 | * | https: 443 | login.microsoftonline.com | Windows oturum açma etkinliğine izin verir |
    | Rule_4 | * | https: 443, http: 80 | storage_account_name. blob. Core. Windows. net | `storage_account_name` değerini gerçek depolama hesabı adınızla değiştirin. Kümeniz, ile desteklenir ve ardından, için bir kural ekleyin. YALNıZCA HTTPS bağlantılarını kullanmak için depolama hesabında ["güvenli aktarım gerekli"](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) özelliğinin etkinleştirildiğinden emin olun. |

   ![Başlık: uygulama kuralı koleksiyonu ayrıntılarını girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

1. **Add (Ekle)** seçeneğini belirleyin.

### <a name="configure-the-firewall-with-network-rules"></a>Ağ kurallarıyla güvenlik duvarını yapılandırma

HDInsight kümenizi doğru şekilde yapılandırmak için ağ kuralları oluşturun.

1. Önceki adımdan devam etmek için **ağ kuralı koleksiyonu** >  **+ ağ kuralı koleksiyonu Ekle**' ye gidin.

1. **Ağ kuralı koleksiyonu Ekle** ekranında, aşağıdaki bilgileri sağlayın:

    **Üst bölüm**

    | Özellik|  Değer|
    |---|---|
    |Ad| FwNetRule|
    |Öncelik|200|
    |Eylem|Allow|

    **IP adresleri bölümü**

    | Ad | Protokol | Kaynak adresler | Hedef adresler | Hedef bağlantı noktaları | Notlar |
    | --- | --- | --- | --- | --- | --- |
    | Rule_1 | UDP | * | * | 123 | Zaman hizmeti |
    | Rule_2 | Herhangi biri | * | DC_IP_Address_1, DC_IP_Address_2 | * | Kurumsal Güvenlik Paketi (ESP) kullanıyorsanız, IP adresleri bölümüne, ESP kümeleri için AAD-DS ile iletişime izin veren bir ağ kuralı ekleyin. Etki alanı denetleyicilerinin IP adreslerini portaldaki AAD-DS bölümünde bulabilirsiniz |
    | Rule_3 | TCP | * | Data Lake Storage hesabınızın IP adresi | * | Azure Data Lake Storage kullanıyorsanız, ADLS 1. ve Gen2 ile ilgili bir SNı sorunu gidermek için IP adresleri bölümüne bir ağ kuralı ekleyebilirsiniz. Bu seçenek, trafiği, büyük veri yükleri için daha yüksek maliyetlere neden olabilecek güvenlik duvarı 'na yönlendirir, ancak trafik günlüğe kaydedilir ve güvenlik duvarı günlüklerinde denetlenebilir. Data Lake Storage hesabınızın IP adresini belirleme. FQDN 'yi bir IP adresine çözümlemek için `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` gibi bir PowerShell komutu kullanabilirsiniz.|
    | Rule_4 | TCP | * | * | 12000 | Seçim Log Analytics kullanıyorsanız, Log Analytics çalışma alanınız ile iletişimi etkinleştirmek için IP adresleri bölümünde bir ağ kuralı oluşturun. |

    **Hizmet etiketleri bölümü**

    | Ad | Protokol | Kaynak Adresler | Hizmet Etiketleri | Hedef bağlantı noktaları | Notlar |
    | --- | --- | --- | --- | --- | --- |
    | Rule_7 | TCP | * | SQL | 1433 | Configure a network rule in the Service Tags section for SQL that will allow you to log and audit SQL traffic, unless you configured Service Endpoints for SQL Server on the HDInsight subnet, which will bypass the firewall. |

   ![Title: Enter application rule collection](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

1. **Add (Ekle)** seçeneğini belirleyin.

### <a name="create-and-configure-a-route-table"></a>Create and configure a route table

Create a route table with the following entries:

* All IP addresses from [Health and management services: All regions](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-all-regions) with a next hop type of **Internet**.

* Two IP addresses for the region where the cluster is created from [Health and management services: Specific regions](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-specific-regions) with a next hop type of **Internet**.

* One Virtual Appliance route for IP address 0.0.0.0/0 with the next hop being your Azure Firewall private IP address.

For example, to configure the route table for a cluster created in the US region of "East US", use following steps:

1. Select your Azure firewall **Test-FW01**. Copy the **Private IP address** listed on the **Overview** page. For this example, we'll use a **sample address of 10.0.2.4**.

1. Then navigate to **All services** > **Networking** > **Route tables** and **Create Route Table**.

1. From your new route, navigate to **Settings** > **Routes** >  **+ Add**. Add the following routes:

| Yönlendirme adı | Adres ön eki | Sonraki atlama türü | Sonraki atlama adresi |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | Yok |
| 23.99.5.239 | 23.99.5.239/32 | Internet | Yok |
| 168.61.48.131 | 168.61.48.131/32 | Internet | Yok |
| 138.91.141.162 | 138.91.141.162/32 | Internet | Yok |
| 13.82.225.233 | 13.82.225.233/32 | Internet | Yok |
| 40.71.175.99 | 40.71.175.99/32 | Internet | Yok |
| 0.0.0.0 | 0.0.0.0/0 | Sanal gereç | 10.0.2.4 |

Complete the route table configuration:

1. Assign the route table you created to your HDInsight subnet by selecting **Subnets** under **Settings**.

1. Select **+ Associate**.

1. On the **Associate subnet** screen, select the virtual network that your cluster was created into and the **Subnet** you used for your HDInsight cluster.

1. **Tamam**’ı seçin.

## <a name="edge-node-or-custom-application-traffic"></a>Edge-node or custom application traffic

The above steps will allow the cluster to operate without issues. You still need to configure dependencies to accommodate your custom applications running on the edge-nodes, if applicable.

Application dependencies must be identified and added to the Azure Firewall or the route table.

Routes must be created for the application traffic to avoid asymmetric routing issues.

If your applications have other dependencies, they need to be added to your Azure Firewall. Create Application rules to allow HTTP/HTTPS traffic and Network rules for everything else.

## <a name="logging-and-scale"></a>Logging and scale

Azure Firewall can send logs to a few different storage systems. For instructions on configuring logging for your firewall, follow the steps in [Tutorial: Monitor Azure Firewall logs and metrics](../firewall/tutorial-diagnostics.md).

Once you've completed the logging setup, if you're logging data to Log Analytics, you can view blocked traffic with a query such as the following:

```Kusto
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Integrating your Azure Firewall with Azure Monitor logs is useful when first getting an application working when you aren't aware of all of the application dependencies. You can learn more about Azure Monitor logs from [Analyze log data in Azure Monitor](../azure-monitor/log-query/log-query-overview.md)

To learn about the scale limits of Azure Firewall and request increases, see [this](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits) document or refer to the [FAQs](../firewall/firewall-faq.md).

## <a name="access-to-the-cluster"></a>Access to the cluster

After having the firewall set up successfully, you can use the internal endpoint (`https://CLUSTERNAME-int.azurehdinsight.net`) to access the Ambari from inside the VNET.

To use the public endpoint (`https://CLUSTERNAME.azurehdinsight.net`) or ssh endpoint (`CLUSTERNAME-ssh.azurehdinsight.net`), make sure you have the right routes in the route table and NSG rules to avoid the asymmetric routing issue explained [here](../firewall/integrate-lb.md). Specifically in this case, you need to allow the client IP address in the Inbound NSG rules and also add it to the user-defined route table with the next hop set as `internet`. If this isn't set up correctly, you'll see a timeout error.

## <a name="configure-another-network-virtual-appliance"></a>Configure another network virtual appliance

> [!Important]
> The following information is **only** required if you wish to configure a network virtual appliance (NVA) other than Azure Firewall.

The previous instructions help you configure Azure Firewall for restricting outbound traffic from your HDInsight cluster. Azure Firewall is automatically configured to allow traffic for many of the common important scenarios. Başka bir ağ sanal gereci kullanmak istiyorsanız, bir dizi ek özelliği el ile yapılandırmanız gerekir. Ağ sanal gerecinizi yapılandırırken şunları göz önünde bulundurun:

* Hizmet uç noktası özellikli Hizmetleri, hizmet uç noktaları ile yapılandırılmalıdır.
* IP adresi bağımlılıkları HTTP/sn olmayan trafiğe yöneliktir (TCP ve UDP trafiği).
* FQDN HTTP/HTTPS uç noktaları, NVA cihazınıza yerleştirilebilir.
* Joker karakter HTTP/HTTPS uç noktaları, bir dizi niteleyicilere göre değişebilen bağımlılıklardır.
* Oluşturduğunuz yol tablosunu HDInsight alt ağına atayın.

### <a name="service-endpoint-capable-dependencies"></a>Hizmet uç noktası özellikli bağımlılıklar

| **Uç noktası** |
|---|
| Azure SQL |
| Azure Depolama |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları

| **Uç noktası** | **Ayrıntılar** |
|---|---|
| \*: 123 | NTP saat denetimi. Trafik, 123 numaralı bağlantı noktasında birden çok uç noktaya denetlenir |
| [Burada](hdinsight-management-ip-addresses.md) Yayınlanan IP 'ler | Bunlar HDInsight hizmetidir |
| ESP kümeleri için AAD-DS özel IP 'Leri |
| \*: KMS Windows etkinleştirmesi için 16800 |
| Log Analytics için \*12000 |

#### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları

> [!Important]
> Aşağıdaki liste, en önemli FQDN 'lerin çoğunu sağlar. [Bu dosyada](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json)NVA 'nizi yapılandırmak için FQDN 'lerin tam listesini alabilirsiniz.

| **Uç noktası**                                                          |
|---|
| azure.archive.ubuntu.com:80                                           |
| security.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |
| wawsinfraprodbay063.blob.core.windows.net:443                         |
| registry-1.docker.io:443                                              |
| auth.docker.io:443                                                    |
| production.cloudflare.docker.com:443                                  |
| download.docker.com:443                                               |
| us.archive.ubuntu.com:80                                              |
| download.mono-project.com:80                                          |
| packages.treasuredata.com:80                                          |
| security.ubuntu.com:80                                                |
| azure.archive.ubuntu.com:80                                           |
| ocsp.msocsp.com:80                                                    |
| ocsp.digicert.com:80                                                  |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight sanal ağ mimarisi](hdinsight-virtual-network-architecture.md)
