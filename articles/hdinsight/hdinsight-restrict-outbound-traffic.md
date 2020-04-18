---
title: Giden ağ trafiği kısıtlaması yapılandırma - Azure HDInsight
description: Azure HDInsight kümeleri için giden ağ trafiği kısıtlamasını nasıl yapılandırılabildiğini öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/17/2020
ms.openlocfilehash: d4bf2d1d4beeb00325d54e091a00438073509eef
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641305"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall"></a>Güvenlik Duvarı'nı kullanarak Azure HDInsight kümeleri için giden ağ trafiğini yapılandırma

Bu makalede, Azure Güvenlik Duvarı'nı kullanarak HDInsight kümenizden giden trafiği güvenli hale getirmek için adımlar sağlanmaktadır. Aşağıdaki adımlar, varolan bir küme için bir Azure Güvenlik Duvarı yapılandırdığınızı varsayar. Bir güvenlik duvarının arkasına yeni bir küme dağıtıyorsanız, önce HDInsight kümenizi ve alt ağınızı oluşturun. Ardından bu kılavuzdaki adımları izleyin.

## <a name="background"></a>Arka plan

HDInsight kümeleri normalde sanal ağda dağıtılır. Küme, bu sanal ağın dışındaki hizmetlere bağımlıdır.

Gelen trafik gerektiren çeşitli bağımlılıklar vardır. Gelen yönetim trafiği bir güvenlik duvarı aygıtı ndan gönderilemez. Bu trafiğin kaynak adresleri bilinmektedir ve [burada](hdinsight-management-ip-addresses.md)yayınlanır. Kümelere gelen trafiği güvence altına almak için bu bilgilerle Ağ Güvenlik Grubu (NSG) kuralları da oluşturabilirsiniz.

HDInsight giden trafik bağımlılıkları neredeyse tamamen FQDNs ile tanımlanır. Arkalarında statik IP adresleri yok. Statik adreslerin olmaması, Ağ Güvenlik Grupları'nın (NSG'ler) bir kümeden giden trafiği kilitleyememesi anlamına gelir. Adresler, geçerli ad çözümlemesi ve kullanımına göre kurallar ayarlayamadan yeterince sık değişir.

Alan adlarına göre giden trafiği denetlenebilen bir güvenlik duvarıyla giden adresleri güvenli hale getirebilir. Azure Güvenlik Duvarı, hedefin FQDN'sini veya [FQDN etiketlerini](../firewall/fqdn-tags.md)temel alan giden trafiği kısıtlar.

## <a name="configuring-azure-firewall-with-hdinsight"></a>Azure Güvenlik Duvar'ı HDInsight ile yapılandırma

Azure Güvenlik Duvarı ile mevcut HDInsight'ınızdan çıkışları kilitleme adımlarının özeti şunlardır:

1. Bir alt ağ oluşturun.
1. Bir güvenlik duvarı oluşturun.
1. Güvenlik duvarına uygulama kuralları ekleme
1. Güvenlik duvarına ağ kuralları ekleyin.
1. Yönlendirme tablosu oluşturun.

### <a name="create-new-subnet"></a>Yeni alt ağ oluşturma

Kümenizin bulunduğu sanal ağda **AzureFirewallSubnet** adında bir alt ağ oluşturun.

### <a name="create-a-new-firewall-for-your-cluster"></a>Kümeniz için yeni bir güvenlik duvarı oluşturma

Güvenlik duvarını Öğretici'den **dağıt'** taki adımları kullanarak **Test-FW01** adlı bir güvenlik duvarı [oluşturun: Azure portalını kullanarak Azure Güvenlik Duvarını dağıtın ve yapılandırın.](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall)

### <a name="configure-the-firewall-with-application-rules"></a>Güvenlik duvarını uygulama kurallarıyla yapılandırma

Kümenin önemli iletişimleri göndermesine ve almasını sağlayan bir uygulama kuralı koleksiyonu oluşturun.

1. Azure portalından yeni güvenlik duvarı **Testi-FW01'i** seçin.

1. **Ayarlar** > **Kuralları** > **Uygulama kural koleksiyonu** > **+ Uygulama kuralı toplama ekle**gidin.

    ![Başlık: Uygulama kuralı koleksiyonu ekleme](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

1. Uygulama **kuralı toplama toplama** ekranında aşağıdaki bilgileri sağlayın:

    **Üst bölüm**

    | Özellik|  Değer|
    |---|---|
    |Adı| FwAppRule|
    |Öncelik|200|
    |Eylem|İzin Ver|

    **FQDN etiketleri bölümü**

    | Adı | Kaynak adresi | FQDN etiketi | Notlar |
    | --- | --- | --- | --- |
    | Rule_1 | * | WindowsUpdate ve HDInsight | HDI hizmetleri için gerekli |

    **Hedef FQDNs bölümü**

    | Adı | Kaynak adresleri | `Protocol:Port` | Hedef FQDNS | Notlar |
    | --- | --- | --- | --- | --- |
    | Rule_2 | * | https:443 | login.windows.net | Windows oturum açma etkinliğine izin verir |
    | Rule_3 | * | https:443 | login.microsoftonline.com | Windows oturum açma etkinliğine izin verir |
    | Rule_4 | * | https:443,http:80 | storage_account_name.blob.core.windows.net | Gerçek `storage_account_name` depolama hesabı adınızı değiştirin. Kümeniz WASB tarafından desteklenense, WASB için bir kural ekleyin. YALNıZCA https bağlantılarını kullanmak için depolama hesabında ["güvenli aktarım gerekli"](../storage/common/storage-require-secure-transfer.md) olduğundan emin olun. |

   ![Başlık: Uygulama kuralı toplama ayrıntılarını girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

1. **Add (Ekle)** seçeneğini belirleyin.

### <a name="configure-the-firewall-with-network-rules"></a>Güvenlik duvarını ağ kurallarıyla yapılandırma

HDInsight kümenizi doğru şekilde yapılandırmak için ağ kurallarını oluşturun.

1. Önceki adımdan devam ederek, **Ağ kuralı koleksiyonu** > + Ağ kuralı**koleksiyonu ekle'ye**gidin.

1. Ağ **kuralı toplama toplama** ekranında aşağıdaki bilgileri sağlayın:

    **Üst bölüm**

    | Özellik|  Değer|
    |---|---|
    |Adı| FwNetRule|
    |Öncelik|200|
    |Eylem|İzin Ver|

    **IP Adresleri bölümü**

    | Adı | Protokol | Kaynak adresleri | Hedef adresleri | Hedef bağlantı noktaları | Notlar |
    | --- | --- | --- | --- | --- | --- |
    | Rule_1 | UDP | * | * | 123 | Zaman hizmeti |
    | Rule_2 | Herhangi biri | * | DC_IP_Address_1, DC_IP_Address_2 | * | Kurumsal Güvenlik Paketi 'ni (ESP) kullanıyorsanız, IP Adresleri bölümüne ESP kümeleri için AAD-DS ile iletişime izin veren bir ağ kuralı ekleyin. Portaldaki AAD-DS bölümünde etki alanı denetleyicilerinin IP adreslerini bulabilirsiniz |
    | Rule_3 | TCP | * | Veri Gölü Depolama hesabınızın IP Adresi | * | Azure Veri Gölü Depolama'yı kullanıyorsanız, ADLS Gen1 ve Gen2 ile ilgili bir SNI sorununu gidermek için IP Adresleri bölümüne bir ağ kuralı ekleyebilirsiniz. Bu seçenek trafiği güvenlik duvarına yönlendirir. Bu da büyük veri yükleri için daha yüksek maliyetlere neden olabilir, ancak trafik günlüğe kaydedilir ve güvenlik duvarı günlüklerinde denetlenebilir. Veri Gölü Depolama hesabınızın IP adresini belirleyin. FQDN'yi bir `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")` IP adresine çözümlemek gibi bir PowerShell komutu kullanabilirsiniz.|
    | Rule_4 | TCP | * | * | 12000 | (İsteğe bağlı) Log Analytics kullanıyorsanız, Log Analytics çalışma alanınızla iletişimi etkinleştirmek için IP Adresleri bölümünde bir ağ kuralı oluşturun. |

    **Hizmet Etiketleri bölümü**

    | Adı | Protokol | Kaynak Adresler | Hizmet Etiketleri | Hedef Limanlar | Notlar |
    | --- | --- | --- | --- | --- | --- |
    | Rule_7 | TCP | * | SQL | 1433 | SQL için Hizmet Etiketleri bölümünde, SQL trafiğini günlüğe kaydetmenize ve denetlemenize olanak tanıyan bir ağ kuralını yapılandırın. HDInsight alt netinde SQL Server için Hizmet Bitiş Noktaları yapılandırılmadığı sürece, güvenlik duvarını atlayacaktır. |

   ![Başlık: Uygulama kuralı koleksiyonunu girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

1. **Add (Ekle)** seçeneğini belirleyin.

### <a name="create-and-configure-a-route-table"></a>Rota tablosu oluşturma ve yapılandırma

Aşağıdaki girişlerden bir rota tablosu oluşturun:

* [Sağlık ve yönetim hizmetlerinden](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-all-regions) tüm IP adresleri: Bir sonraki **internet**atlama türüne sahip tüm bölgeler.

* Kümenin [Sistem Durumu ve yönetim hizmetlerinden](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-specific-regions) oluşturulduğu bölge için iki IP adresi: Bir sonraki **internet**atlama türüne sahip belirli bölgeler.

* IP adresi 0.0.0.0/0 için bir Sanal Cihaz rotası ve bir sonraki atlama Azure Güvenlik Duvarı özel IP adresiniz dir.

Örneğin, ABD'nin "Doğu ABD" bölgesinde oluşturulan bir küme için rota tablosunu yapılandırmak için aşağıdaki adımları kullanın:

1. Azure güvenlik duvarı **Test-FW01'inizi**seçin. **Genel Bakış** sayfasında listelenen Özel **IP adresini** kopyalayın. Bu örnek için, **10.0.2.4 örnek adresi**kullanacağız.

1. Ardından **Tüm hizmetler** > **Ağ** > **Rotası tablolarına** gidin ve Rota Tablosu **Oluştur'** una gidin.

1. Yeni rotanızdan **Ayarlar** > **Rotaları** > **+ Ekle'ye**gidin. Aşağıdaki yolları ekleyin:

| Yönlendirme adı | Adres ön eki | Sonraki atlama türü | Sonraki atlama adresi |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | NA |
| 23.99.5.239 | 23.99.5.239/32 | Internet | NA |
| 168.61.48.131 | 168.61.48.131/32 | Internet | NA |
| 138.91.141.162 | 138.91.141.162/32 | Internet | NA |
| 13.82.225.233 | 13.82.225.233/32 | Internet | NA |
| 40.71.175.99 | 40.71.175.99/32 | Internet | NA |
| 0.0.0.0 | 0.0.0.0/0 | Sanal gereç | 10.0.2.4 |

Rota tablosu yapılandırmasını tamamlayın:

1. **Ayarlar**altında **Alt Ağları** seçerek oluşturduğunuz rota tablosunu HDInsight alt ağınıza atayın.

1. + **Ilişkilendir'i**seçin.

1. Ortak **alt ağ** ekranında, kümenizin oluşturulduğu sanal ağı seçin. HDInsight kümeniz için kullandığınız **Subnet.**

1. **Tamam'ı**seçin.

## <a name="edge-node-or-custom-application-traffic"></a>Kenar düğümü veya özel uygulama trafiği

Yukarıdaki adımlar kümenin sorunsuz çalışmasına izin verir. Varsa, kenar düğümlerinde çalışan özel uygulamalarınızı barındıracak bağımlılıkları yapılandırmanız gerekir.

Uygulama bağımlılıkları tanımlanmalı ve Azure Güvenlik Duvarı'na veya rota tablosuna eklenmelidir.

Asimetrik yönlendirme sorunlarını önlemek için uygulama trafiği için yollar oluşturulmalıdır.

Uygulamalarınızın başka bağımlılıkları varsa, Azure Güvenlik Duvarınıza eklenmesi gerekir. Her şey için HTTP/HTTPS trafiğine ve Ağ kurallarına izin vermek için Uygulama kuralları oluşturun.

## <a name="logging-and-scale"></a>Günlüğe kaydetme ve ölçeklendirme

Azure Güvenlik Duvarı günlükleri birkaç farklı depolama sistemine gönderebilir. Güvenlik duvarınız için günlük işlemlerini yapılandırma yla ilgili talimatlar [için, Öğretici: Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleyin.](../firewall/tutorial-diagnostics.md)

Günlük kurulumuna başladıktan sonra, Log Analytics kullanıyorsanız, engellenen trafiği aşağıdaki ler gibi bir sorguyla görüntüleyebilirsiniz:

```Kusto
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Azure Güvenlik Duvarı'nı Azure Monitor günlükleriyle tümleştirmek, bir uygulamayı ilk çalışırken yararlıdır. Özellikle tüm uygulama bağımlılıklarının farkında değilseniz. Azure Monitör'deki günlük verilerini [çözümleyerek](../azure-monitor/log-query/log-query-overview.md) Azure Monitor günlükleri hakkında daha fazla bilgi edinebilirsiniz

Azure Güvenlik Duvarı'nın ölçek sınırları ve istek artışları hakkında bilgi edinmek için [bu](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits) belgeye bakın veya [SSS'lere](../firewall/firewall-faq.md)bakın.

## <a name="access-to-the-cluster"></a>Kümeye erişim

Güvenlik duvarının başarıyla ayarlatmasını sonra, ambari'ye sanal ağ içinden erişmek için dahili uç noktasını ()`https://CLUSTERNAME-int.azurehdinsight.net`kullanabilirsiniz.

Genel bitiş noktası (`https://CLUSTERNAME.azurehdinsight.net`) veya ssh`CLUSTERNAME-ssh.azurehdinsight.net`bitiş noktası ( ), [burada](../firewall/integrate-lb.md)açıklanan asimetrik yönlendirme sorunu önlemek için rota tablosu ve NSG kuralları doğru yolları olduğundan emin olun . Özellikle bu durumda, Gelen NSG kurallarında istemci IP adresine izin vermeniz ve bir sonraki atlama kümesi `internet`ile kullanıcı tarafından tanımlanan rota tablosuna eklemeniz gerekir. Yönlendirme doğru ayarlanmazsa, bir zaman arahatası görürsünüz.

## <a name="configure-another-network-virtual-appliance"></a>Başka bir ağ sanal cihazını yapılandırma

> [!Important]
> Aşağıdaki bilgiler **yalnızca** Azure Güvenlik Duvarı dışında bir ağ sanal cihazını (NVA) yapılandırmak istiyorsanız gereklidir.

Önceki yönergeler, HDInsight kümenizden giden trafiği kısıtlamak için Azure Güvenlik Duvarı'nı yapılandırmanıza yardımcı olur. Azure Güvenlik Duvarı, sık karşılaşılan önemli senaryoların çoğu için trafiğe izin verecek şekilde otomatik olarak yapılandırılır. Başka bir ağ sanal cihaz kullanarak ek özellikler bir dizi yapılandırmak gerekir. Ağınızı sanal cihazınızı yapılandırırken aşağıdaki faktörleri aklınızda bulundurun:

* Hizmet Bitiş Noktası özellikli hizmetler, hizmet bitiş noktalarıyla yapılandırılmalıdır.
* IP Adresi bağımlılıkları HTTP/S olmayan trafik içindir (hem TCP hem de UDP trafiği).
* FQDN HTTP/HTTPS uç noktaları NVA cihazınıza yerleştirilebilir.
* Joker KARAKTER HTTP/HTTPS bitiş noktaları, bir dizi niteleyiciye göre değişebilen bağımlılıklardır.
* Oluşturduğunuz rota tablosunu HDInsight alt ağınıza atayın.

### <a name="service-endpoint-capable-dependencies"></a>Hizmet uç noktası yeteneğine sahip bağımlılıklar

| **Uç Nokta** |
|---|
| Azure SQL |
| Azure Storage |
| Azure Active Directory |

#### <a name="ip-address-dependencies"></a>IP adresi bağımlılıkları

| **Uç Nokta** | **Şey** |
|---|---|
| \*:123 | NTP saat kontrolü. Trafik, 123 bağlantı noktasındabirden fazla uç noktada denetlenir |
| [IP'ler burada](hdinsight-management-ip-addresses.md) yayınlandı | Bu IP'ler HDInsight hizmetidir |
| ESP kümeleri için AAD-DS özel IP'leri |
| \*:KMS Windows Etkinleştirme için 16800 |
| \*Log Analytics için 12000 |

#### <a name="fqdn-httphttps-dependencies"></a>FQDN HTTP/HTTPS bağımlılıkları

> [!Important]
> Aşağıdaki liste sadece en önemli FQDNs birkaç verir. [Bu dosyada](https://github.com/Azure-Samples/hdinsight-fqdn-lists/blob/master/HDInsightFQDNTags.json)NVA'nızı yapılandırmak için ek FQDN'ler (çoğunlukla Azure Depolama ve Azure Hizmet Veri Mes'leri) alabilirsiniz.

| **Uç Nokta**                                                          |
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
