---
title: Azure HDInsight kümeleri için giden ağ trafiği kısıtlamasını yapılandırma
description: Azure HDInsight kümeleri için giden ağ trafiği kısıtlamasını yapılandırma hakkında bilgi edinin.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 829f3e730b4993a6a7f32a9224d3c6c38bd4c06e
ms.sourcegitcommit: fa4852cca8644b14ce935674861363613cf4bfdf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70811950"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall-preview"></a>Güvenlik duvarını kullanarak Azure HDInsight kümeleri için giden ağ trafiği yapılandırma (Önizleme)

Bu makalede, Azure Güvenlik Duvarı 'nı kullanarak HDInsight kümenizdeki giden trafiği güvenli hale getirmeye yönelik adımlar sağlanmaktadır. Aşağıdaki adımlarda, var olan bir küme için bir Azure Güvenlik Duvarı yapılandırdığınız varsayılmaktadır. Yeni bir küme dağıtıyorsanız ve güvenlik duvarının arkasındaysa, önce HDInsight kümenizi ve alt ağını oluşturun ve ardından bu kılavuzdaki adımları izleyin.

## <a name="background"></a>Arka Plan

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

### <a name="create-a-new-firewall-for-your-cluster"></a>Kümeniz için yeni bir güvenlik duvarı oluşturma

1. Kümenizin bulunduğu sanal ağda **AzureFirewallSubnet** adlı bir alt ağ oluşturun. 
1. [Öğreticideki adımları kullanarak yeni bir güvenlik duvarı **Test-FW01** oluşturun: Azure portal](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall)kullanarak Azure Güvenlik Duvarı 'nı dağıtın ve yapılandırın.

### <a name="configure-the-firewall-with-application-rules"></a>Uygulama kurallarıyla güvenlik duvarını yapılandırma

Kümenin önemli iletişimleri göndermesini ve almasını sağlayan bir uygulama kuralı koleksiyonu oluşturun.

Azure portal yeni güvenlik duvarını **Test-FW01** seçin. **Ayarlar** **uygulama kuralı koleksiyonu** **uygulama kuralı koleksiyonu Ekle**altında Kurallar ' a tıklayın. >  > 

![Başlığın Uygulama kuralı koleksiyonu ekle](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

**Uygulama kuralı koleksiyonu Ekle** ekranında, aşağıdaki adımları izleyin:

1. Bir **ad**, **Öncelik**girin ve **eylem** açılan menüsünden **Izin ver** ' e tıklayın ve **FQDN Etiketleri bölümüne** aşağıdaki kuralları girin:

   | **Name** | **Kaynak adres** | **FQDN etiketi** | **Notlar** |
   | --- | --- | --- | --- |
   | Rule_1 | * | HDInsight ve WindowsUpdate | HDI Hizmetleri için gerekli |

1. **Hedef FQDN 'Ler bölümüne** aşağıdaki kuralları ekleyin:

   | **Name** | **Kaynak adres** | **Protokol: bağlantı noktası** | **Hedef FQDN 'ler** | **Notlar** |
   | --- | --- | --- | --- | --- |
   | Rule_2 | * | https: 443 | login.windows.net | Windows oturum açma etkinliğine izin verir |
   | Rule_3 | * | https: 443, http: 80 | < storage_account_name. blob. Core. Windows. net > | Kümeniz, ile desteklenir ve ardından, için bir kural ekleyin. YALNıZCA HTTPS bağlantılarını kullanmak için depolama hesabında ["güvenli aktarım gerekli"](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) özelliğinin etkinleştirildiğinden emin olun. |

1. **Ekle**'yi tıklatın.

   ![Başlığın Uygulama kuralı koleksiyonu ayrıntılarını girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

### <a name="configure-the-firewall-with-network-rules"></a>Ağ kurallarıyla güvenlik duvarını yapılandırma

HDInsight kümenizi doğru şekilde yapılandırmak için ağ kuralları oluşturun.

1. Azure portal yeni güvenlik duvarını **Test-FW01** seçin.
1. **Ayarlar** **ağ kuralı koleksiyonu** **ağ kuralı koleksiyonu Ekle**altında Kurallar ' a tıklayın. >  > 
1. **Ağ kuralı koleksiyonu Ekle** ekranında, bir **ad**, **Öncelik**girin ve **eylem** açılan menüsünden **izin ver** ' e tıklayın.
1. **IP adresleri** bölümünde aşağıdaki kuralları oluşturun:

   | **Name** | **Protokolü** | **Kaynak adres** | **Hedef adres** | **Hedef bağlantı noktası** | **Notlar** |
   | --- | --- | --- | --- | --- | --- |
   | Rule_1 | UDP | * | * | `123` | Zaman hizmeti |
   | Rule_2 | Any | * | DC_IP_Address_1, DC_IP_Address_2 | `*` | Kurumsal Güvenlik Paketi (ESP) kullanıyorsanız, IP adresleri bölümüne, ESP kümeleri için AAD-DS ile iletişime izin veren bir ağ kuralı ekleyin. Etki alanı denetleyicilerinin IP adreslerini portaldaki AAD-DS bölümünde bulabilirsiniz | 
   | Rule_3 | TCP | * | Data Lake Storage hesabınızın IP adresi | `*` | Azure Data Lake Storage kullanıyorsanız, ADLS 1. ve Gen2 ile ilgili bir SNı sorunu gidermek için IP adresleri bölümüne bir ağ kuralı ekleyebilirsiniz. Bu seçenek, trafiği büyük veri yükleri için daha yüksek maliyetlere neden olabilecek güvenlik duvarı 'na yönlendirir, ancak trafik günlüğe kaydedilir ve güvenlik duvarı günlüklerinde denetlenebilir. Data Lake Storage hesabınızın IP adresini belirleme. FQDN 'yi bir IP adresine çözümlemek `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` için gibi bir PowerShell komutu kullanabilirsiniz.|
   | Rule_4 | TCP | * | * | `12000` | Seçim Log Analytics kullanıyorsanız, Log Analytics çalışma alanınız ile iletişimi etkinleştirmek için IP adresleri bölümünde bir ağ kuralı oluşturun. |

1. **Hizmet etiketleri** bölümünde aşağıdaki kuralları oluşturun:

   | **Name** | **Protokolü** | **Kaynak adres** | **Hizmet etiketleri** | **Hedef bağlantı noktası** | **Notlar** |
   | --- | --- | --- | --- | --- | --- |
   | Rule_7 | TCP | * | SQL | `1433` | HDInsight alt ağında güvenlik duvarını atlayacak SQL Server için hizmet uç noktaları yapılandırmadığınız müddetçe SQL için hizmet etiketleri bölümünde, SQL trafiğini günlüğe kaydetme ve denetleme izni veren bir ağ kuralı yapılandırın. |

1. Ağ kuralı koleksiyonunuzun oluşturulmasını tamamladıktan sonra **Ekle** ' ye tıklayın.

   ![Başlığın Uygulama kuralı koleksiyonu girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

### <a name="create-and-configure-a-route-table"></a>Rota tablosu oluşturma ve yapılandırma

Aşağıdaki girişlerle bir yol tablosu oluşturun:

1. [Bu, gerekli HDInsight YÖNETIM IP adresleri listesinden](../hdinsight/hdinsight-management-ip-addresses.md) **Internet**'in bir sonraki atlamada altı Adres:
    1. Tüm bölgelerde tüm kümeler için dört IP adresi
    1. Kümenin oluşturulduğu bölgeye özgü iki IP adresi
1. 0\.0.0.0/0 IP adresi için bir Sanal Gereç yolu, sonraki atlama olan Azure Güvenlik Duvarı özel IP adresiniz.

Örneğin, "Orta ABD" ABD bölgesinde oluşturulan bir kümenin yol tablosunu yapılandırmak için aşağıdaki adımları kullanın:

1. Azure Portal’da oturum açın.
1. Azure Güvenlik Duvarı **testinizi seçin-FW01**. **Genel bakış** sayfasında LISTELENEN **özel IP adresini** kopyalayın. Bu örnekte **, 10.1.1.4 örnek adresini** kullanacağız
1. Yeni bir yol tablosu oluşturun.
1. **Ayarlar**altında **rotalar** ' ı tıklatın.
1. Aşağıdaki tabloda IP adresleri için yollar oluşturmak üzere **Ekle** ' ye tıklayın.

| Rota adı | Adres ön eki | Sonraki atlama türü | Sonraki atlama adresi |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | İnternet | NA |
| 23.99.5.239 | 23.99.5.239/32 | İnternet | NA |
| 168.61.48.131 | 168.61.48.131/32 | İnternet | NA |
| 138.91.141.162 | 138.91.141.162/32 | İnternet | NA |
| 13.67.223.215 | 13.67.223.215/32 | İnternet | NA |
| 40.86.83.253 | 40.86.83.253/32 | İnternet | NA |
| 0.0.0.0 | 0.0.0.0/0 | Sanal gereç | 10.1.1.4 |

Yol tablosu yapılandırmasını doldurun:

1. **Ayarlar** altında **alt ağlar** ' a ve ardından **ilişkilendir**' e tıklayarak HDInsight alt ağına oluşturduğunuz yol tablosunu atayın.
1. **Alt ağı ilişkilendir** ekranında, kümenizin oluşturulduğu sanal ağı ve HDInsight kümeniz Için kullandığınız **HDInsight alt ağını** seçin.
1. **Tamam**'ı tıklatın.

## <a name="edge-node-or-custom-application-traffic"></a>Edge-node veya özel uygulama trafiği

Yukarıdaki adımlar kümenin sorun olmadan çalışmasına izin verir. Yine de, varsa kenar düğümleri üzerinde çalışan özel uygulamalarınıza uyum sağlamak için bağımlılıkları yapılandırmanız gerekir.

Uygulama bağımlılıkları tanımlanmalıdır ve Azure Güvenlik duvarı ya da yol tablosuna eklenmelidir.

Asimetrik yönlendirme sorunlarından kaçınmak için uygulama trafiği için yolların oluşturulması gerekir.

Uygulamalarınızın başka bağımlılıkları varsa, bunların Azure güvenlik duvarınızdan eklenmesi gerekir. Diğer her şey için HTTP/HTTPS trafiğine ve ağ kurallarına izin vermek üzere uygulama kuralları oluşturun.

## <a name="logging"></a>Günlüğe Kaydetme

Azure Güvenlik Duvarı, günlükleri birkaç farklı depolama sistemine gönderebilir. Güvenlik duvarınız için günlüğe kaydetmeyi yapılandırma yönergeleri için [öğreticideki adımları izleyin: Azure Güvenlik Duvarı günlüklerini ve ölçümlerini](../firewall/tutorial-diagnostics.md)izleyin.

Günlüğe kaydetme kurulumunu tamamladıktan sonra, Log Analytics verileri günlüğe kaydetmek için aşağıdaki gibi bir sorgu ile engellenen trafiği görüntüleyebilirsiniz:

```
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Azure Güvenlik duvarını Azure Izleyici günlükleriyle tümleştirmek, uygulama bağımlılıklarının tümünün farkında olmadığında ilk olarak bir uygulama çalışırken yararlıdır. Azure izleyici günlükleri hakkında daha fazla bilgi edinmek için Azure izleyici ['de günlük verilerini çözümleme](../azure-monitor/log-query/log-query-overview.md)

## <a name="access-to-the-cluster"></a>Kümeye erişim
Güvenlik Duvarı kurulumundan başarıyla karşılaşduktan sonra, VNET 'in içinden ambarı 'na erişmek`https://<clustername>-int.azurehdinsight.net`için iç uç noktasını () kullanabilirsiniz. Genel uç nokta (`https://<clustername>.azurehdinsight.net`) veya SSH uç noktasını (`<clustername>-ssh.azurehdinsight.net`) kullanmak için, [burada](https://docs.microsoft.com/azure/firewall/integrate-lb)açıklanan assymetric yönlendirme sorununu önlemek için yol tablosu ve NSG kuralları kurulumunda doğru yollara sahip olduğunuzdan emin olun.

## <a name="configure-another-network-virtual-appliance"></a>Başka bir ağ sanal gereci yapılandırma

>[!Important]
> Aşağıdaki bilgiler **yalnızca** Azure Güvenlik Duvarı dışında bir ağ sanal gereci (NVA) yapılandırmak istiyorsanız gereklidir.

Önceki yönergeler, HDInsight kümenizdeki giden trafiği kısıtlamak için Azure Güvenlik duvarını yapılandırmanıza yardımcı olur. Azure Güvenlik Duvarı, yaygın olarak karşılaşılan birçok önemli senaryo için trafiğe izin verecek şekilde otomatik olarak yapılandırılır. Başka bir ağ sanal gereci kullanmak istiyorsanız, bir dizi ek özelliği el ile yapılandırmanız gerekecektir. Ağınızı yapılandırın sanal gereciniz olarak aşağıdakileri aklınızda bulundurun:

* Hizmet uç noktası özellikli Hizmetleri, hizmet uç noktaları ile yapılandırılmalıdır.
* IP adresi bağımlılıkları HTTP/sn olmayan trafiğe yöneliktir (TCP ve UDP trafiği).
* FQDN HTTP/HTTPS uç noktaları, NVA cihazınıza yerleştirilebilir.
* Joker karakter HTTP/HTTPS uç noktaları, bir dizi niteleyicilere göre değişebilen bağımlılıklardır.
* Oluşturduğunuz yol tablosunu HDInsight alt ağına atayın.

### <a name="service-endpoint-capable-dependencies"></a>Hizmet uç noktası özellikli bağımlılıklar

| **Uç noktası** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları

| **Uç noktası** | **Ayrıntılar** |
|---|---|
| \*: 123 | NTP saat denetimi. Trafik, 123 numaralı bağlantı noktasında birden çok uç noktaya denetlenir |
| [Burada](hdinsight-management-ip-addresses.md) Yayınlanan IP 'ler | Bunlar HDInsight hizmetidir |
| ESP kümeleri için AAD-DS özel IP 'Leri |
| \*: KMS Windows etkinleştirmesi 16800 |
| \*Log Analytics için 12000 |

#### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları

>[!Important]
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
| azure.archive.ubuntu.com:80                                                |
| ocsp.msocsp.com:80                                                |
| ocsp.digicert.com:80                                                |

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight sanal ağ mimarisi](hdinsight-virtual-network-architecture.md)
