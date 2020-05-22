---
title: Giden ağ trafiği kısıtlamasını Yapılandırma-Azure HDInsight
description: Azure HDInsight kümeleri için giden ağ trafiği kısıtlamasını yapılandırma hakkında bilgi edinin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: seoapr2020
ms.date: 04/17/2020
ms.openlocfilehash: d3e5f99edb8043b563f37a1710c973bf925338db
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83745557"
---
# <a name="configure-outbound-network-traffic-for-azure-hdinsight-clusters-using-firewall"></a>Güvenlik duvarını kullanarak Azure HDInsight kümeleri için giden ağ trafiği yapılandırma

Bu makalede, Azure Güvenlik Duvarı 'nı kullanarak HDInsight kümenizdeki giden trafiği güvenli hale getirmeye yönelik adımlar sağlanmaktadır. Aşağıdaki adımlarda, var olan bir küme için bir Azure Güvenlik Duvarı yapılandırdığınızı varsayalım. Bir güvenlik duvarının arkasında yeni bir küme dağıtıyorsanız, önce HDInsight kümenizi ve alt ağını oluşturun. Ardından bu kılavuzdaki adımları izleyin.

## <a name="background"></a>Arka Plan

HDInsight kümeleri normalde bir sanal ağda dağıtılır. Kümede, bu sanal ağın dışındaki hizmetler üzerinde bağımlılıklar vardır.

Gelen trafik gerektiren birkaç bağımlılık vardır. Gelen yönetim trafiği bir güvenlik duvarı cihazından gönderilemez. Bu trafiğin kaynak adresleri bilinmektedir ve [burada](hdinsight-management-ip-addresses.md)yayımlanır. Ayrıca, kümelerdeki gelen trafiğin güvenliğini sağlamak için bu bilgilerle ağ güvenlik grubu (NSG) kuralları da oluşturabilirsiniz.

HDInsight giden trafik bağımlılıkları, FQDN 'Ler ile neredeyse tamamen tanımlanmıştır. Bunların arkasında statik IP adresleri yok. Statik adreslerin olmaması, ağ güvenlik gruplarının (NSG 'ler) bir kümeden giden trafiği kilitleyemeyeceği anlamına gelir. Adresler, en çok bir kez değişir ve geçerli ad çözümlemesi ve kullanımı temel alınarak kuralları ayarlayamamıştır.

Etki alanı adlarına göre giden trafiği denetleyesağlayan bir güvenlik duvarı ile giden adresleri güvenli hale getirin. Azure Güvenlik Duvarı, giden trafiği hedef veya [FQDN ETIKETLERININ](../firewall/fqdn-tags.md)FQDN 'sine göre kısıtlar.

## <a name="configuring-azure-firewall-with-hdinsight"></a>HDInsight ile Azure Güvenlik duvarını yapılandırma

Azure Güvenlik Duvarı ile mevcut HDInsight 'ınızdan çıkış kilitleme adımlarının Özeti şunlardır:

1. Bir alt ağ oluşturun.
1. Güvenlik duvarı oluşturun.
1. Güvenlik duvarına uygulama kuralları ekleme
1. Güvenlik duvarına ağ kuralları ekleyin.
1. Yönlendirme tablosu oluşturun.

### <a name="create-new-subnet"></a>Yeni alt ağ oluştur

Kümenizin bulunduğu sanal ağda **AzureFirewallSubnet** adlı bir alt ağ oluşturun.

### <a name="create-a-new-firewall-for-your-cluster"></a>Kümeniz için yeni bir güvenlik duvarı oluşturma

**Test-FW01** adlı bir güvenlik duvarı oluşturarak **güvenlik** duvarını [öğreticiden dağıtma: Azure Portal kullanarak Azure Güvenlik Duvarı dağıtma ve yapılandırma](../firewall/tutorial-firewall-deploy-portal.md#deploy-the-firewall)adımlarını uygulayın.

### <a name="configure-the-firewall-with-application-rules"></a>Uygulama kurallarıyla güvenlik duvarını yapılandırma

Kümenin önemli iletişimleri göndermesini ve almasını sağlayan bir uygulama kuralı koleksiyonu oluşturun.

1. Azure portal yeni güvenlik duvarını **Test-FW01** seçin.

1. **Ayarlar**  >  **kuralları**  >  **uygulama kuralı koleksiyonu**  >  **+ uygulama kuralı koleksiyonu Ekle**' ye gidin.

    ![Başlık: uygulama kuralı koleksiyonu Ekle](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection.png)

1. **Uygulama kuralı koleksiyonu Ekle** ekranında, aşağıdaki bilgileri sağlayın:

    **Üst bölüm**

    | Özellik|  Değer|
    |---|---|
    |Adı| FwAppRule|
    |Öncelik|200|
    |Eylem|İzin Ver|

    **FQDN etiketleri bölümü**

    | Name | Kaynak adres | FQDN etiketi | Notlar |
    | --- | --- | --- | --- |
    | Rule_1 | * | WindowsUpdate ve HDInsight | HDI Hizmetleri için gerekli |

    **Hedef FQDN bölümü**

    | Name | Kaynak adresler | `Protocol:Port` | Hedef FQDN 'ler | Notlar |
    | --- | --- | --- | --- | --- |
    | Rule_2 | * | https: 443 | login.windows.net | Windows oturum açma etkinliğine izin verir |
    | Rule_3 | * | https: 443 | login.microsoftonline.com | Windows oturum açma etkinliğine izin verir |
    | Rule_4 | * | https: 443, http: 80 | storage_account_name. blob. Core. Windows. net | `storage_account_name`Gerçek depolama hesabı adınızla değiştirin. Kümeniz, ile desteklenir ve ardından, için bir kural ekleyin. YALNıZCA HTTPS bağlantılarını kullanmak için depolama hesabında ["güvenli aktarım gerekli"](../storage/common/storage-require-secure-transfer.md) özelliğinin etkinleştirildiğinden emin olun. |

   ![Başlık: uygulama kuralı koleksiyonu ayrıntılarını girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-app-rule-collection-details.png)

1. **Add (Ekle)** seçeneğini belirleyin.

### <a name="configure-the-firewall-with-network-rules"></a>Ağ kurallarıyla güvenlik duvarını yapılandırma

HDInsight kümenizi doğru şekilde yapılandırmak için ağ kuralları oluşturun.

1. Önceki adımdan devam etmek için **ağ kuralı koleksiyonu**  >  **+ ağ kuralı koleksiyonu Ekle**' ye gidin.

1. **Ağ kuralı koleksiyonu Ekle** ekranında, aşağıdaki bilgileri sağlayın:

    **Üst bölüm**

    | Özellik|  Değer|
    |---|---|
    |Adı| FwNetRule|
    |Öncelik|200|
    |Eylem|İzin Ver|

    **IP adresleri bölümü**

    | Name | Protokol | Kaynak adresler | Hedef adresler | Hedef bağlantı noktaları | Notlar |
    | --- | --- | --- | --- | --- | --- |
    | Rule_1 | UDP | * | * | 123 | Zaman hizmeti |
    | Rule_2 | Herhangi biri | * | DC_IP_Address_1, DC_IP_Address_2 | * | Kurumsal Güvenlik Paketi (ESP) kullanıyorsanız, IP adresleri bölümüne, ESP kümeleri için AAD-DS ile iletişime izin veren bir ağ kuralı ekleyin. Etki alanı denetleyicilerinin IP adreslerini portaldaki AAD-DS bölümünde bulabilirsiniz |
    | Rule_3 | TCP | * | Data Lake Storage hesabınızın IP adresi | * | Azure Data Lake Storage kullanıyorsanız, ADLS 1. ve Gen2 ile ilgili bir SNı sorunu gidermek için IP adresleri bölümüne bir ağ kuralı ekleyebilirsiniz. Bu seçenek, trafiği güvenlik duvarıyla yönlendirmeyecektir. Bu, büyük veri yükleri için daha yüksek maliyetlere neden olabilir, ancak trafik günlüğe kaydedilir ve güvenlik duvarı günlüklerinde denetlenebilir. Data Lake Storage hesabınızın IP adresini belirleme. `[System.Net.DNS]::GetHostAddresses("STORAGEACCOUNTNAME.blob.core.windows.net")`FQDN 'yi BIR IP adresine çözümlemek için gibi bir PowerShell komutu kullanabilirsiniz.|
    | Rule_4 | TCP | * | * | 12000 | Seçim Log Analytics kullanıyorsanız, Log Analytics çalışma alanınız ile iletişimi etkinleştirmek için IP adresleri bölümünde bir ağ kuralı oluşturun. |

    **Hizmet etiketleri bölümü**

    | Name | Protokol | Kaynak Adresler | Hizmet Etiketleri | Hedef bağlantı noktaları | Notlar |
    | --- | --- | --- | --- | --- | --- |
    | Rule_7 | TCP | * | SQL | 1433 | SQL trafiğini günlüğe kaydetmek ve denetleyeceğinizi sağlayacak SQL için hizmet etiketleri bölümünde bir ağ kuralı yapılandırın. HDInsight alt ağında SQL Server için hizmet uç noktaları yapılandırmadığınız takdirde, güvenlik duvarını atlayacak olur. |
    | Rule_8 | TCP | * | Azure İzleyici | * | seçim Otomatik ölçeklendirme özelliğini kullanmayı planlayan müşterilerin bu kuralı eklemesi gerekir. |
    
   ![Başlık: uygulama kuralı koleksiyonu girin](./media/hdinsight-restrict-outbound-traffic/hdinsight-restrict-outbound-traffic-add-network-rule-collection.png)

1. **Add (Ekle)** seçeneğini belirleyin.

### <a name="create-and-configure-a-route-table"></a>Rota tablosu oluşturma ve yapılandırma

Aşağıdaki girişlerle bir yol tablosu oluşturun:

* [Sistem durumu ve yönetim hizmetlerinden](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-all-regions) gelen tüm IP adresleri: bir sonraki atlama türüne sahip tüm bölgeler **.**

* Kümenin [sistem durumu ve yönetim hizmetlerinden](../hdinsight/hdinsight-management-ip-addresses.md#health-and-management-services-specific-regions) oluşturulduğu bölge IÇIN iki IP adresi: **Internet**'in bir sonraki atlama türüne sahip belirli bölgeler.

* 0.0.0.0/0 IP adresi için bir Sanal Gereç yolu, sonraki atlama olan Azure Güvenlik Duvarı özel IP adresiniz.

Örneğin, "Doğu ABD" ABD bölgesinde oluşturulan bir kümenin yol tablosunu yapılandırmak için aşağıdaki adımları kullanın:

1. Azure Güvenlik Duvarı **testinizi seçin-FW01**. **Genel bakış** sayfasında LISTELENEN **özel IP adresini** kopyalayın. Bu örnekte, **10.0.2.4 örnek adresini**kullanacağız.

1. Ardından **tüm hizmetler**  >  **ağ**  >  **yolu tabloları** ' na gidin ve **rota tablosu oluşturun**.

1. Yeni yolınızdan **Ayarlar**  >  **rotalar**  >  **+ Ekle**' ye gidin. Aşağıdaki yolları ekleyin:

| Yönlendirme adı | Adres ön eki | Sonraki atlama türü | Sonraki atlama adresi |
|---|---|---|---|
| 168.61.49.99 | 168.61.49.99/32 | Internet | NA |
| 23.99.5.239 | 23.99.5.239/32 | Internet | NA |
| 168.61.48.131 | 168.61.48.131/32 | Internet | NA |
| 138.91.141.162 | 138.91.141.162/32 | Internet | NA |
| 13.82.225.233 | 13.82.225.233/32 | Internet | NA |
| 40.71.175.99 | 40.71.175.99/32 | Internet | NA |
| 0.0.0.0 | 0.0.0.0/0 | Sanal gereç | 10.0.2.4 |

Yol tablosu yapılandırmasını doldurun:

1. **Ayarlar**altında **alt ağlar** ' i seçerek HDInsight alt ağına oluşturduğunuz yol tablosunu atayın.

1. **+ İlişkilendir**' i seçin.

1. **Alt ağı ilişkilendir** ekranında, kümenizin oluşturulduğu sanal ağı seçin. Ve HDInsight kümeniz için kullandığınız **alt ağ** .

1. **Tamam**’ı seçin.

## <a name="edge-node-or-custom-application-traffic"></a>Edge-node veya özel uygulama trafiği

Yukarıdaki adımlar kümenin sorun olmadan çalışmasına izin verir. Yine de, varsa kenar düğümleri üzerinde çalışan özel uygulamalarınıza uyum sağlamak için bağımlılıkları yapılandırmanız gerekir.

Uygulama bağımlılıkları tanımlanmalıdır ve Azure Güvenlik duvarı ya da yol tablosuna eklenmelidir.

Asimetrik yönlendirme sorunlarından kaçınmak için uygulama trafiği için yolların oluşturulması gerekir.

Uygulamalarınızın başka bağımlılıkları varsa, bunların Azure güvenlik duvarınızdan eklenmesi gerekir. Diğer her şey için HTTP/HTTPS trafiğine ve ağ kurallarına izin vermek üzere uygulama kuralları oluşturun.

## <a name="logging-and-scale"></a>Günlüğe kaydetme ve ölçeklendirme

Azure Güvenlik Duvarı, günlükleri birkaç farklı depolama sistemine gönderebilir. Güvenlik duvarınız için günlüğe kaydetmeyi yapılandırma yönergeleri için [öğretici: Azure Güvenlik Duvarı günlüklerini ve ölçümlerini izleme](../firewall/tutorial-diagnostics.md)bölümündeki adımları izleyin.

Günlüğe kaydetme kurulumunu tamamladıktan sonra, Log Analytics kullanıyorsanız, engellenen trafiği şöyle bir sorgu ile görüntüleyebilirsiniz:

```Kusto
AzureDiagnostics | where msg_s contains "Deny" | where TimeGenerated >= ago(1h)
```

Azure Güvenlik Duvarı 'Nı Azure Izleyici günlükleriyle tümleştirme, ilk olarak bir uygulama çalışırken yararlıdır. Özellikle uygulama bağımlılıklarının tümünün farkında olmadığınız durumlarda. Azure izleyici günlükleri hakkında daha fazla bilgi edinmek için Azure izleyici ['de günlük verilerini çözümleme](../azure-monitor/log-query/log-query-overview.md)

Azure Güvenlik duvarının ölçek sınırları ve istek artışları hakkında bilgi edinmek için [Bu](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-firewall-limits) belgeye bakın veya [SSS](../firewall/firewall-faq.md)bölümüne bakın.

## <a name="access-to-the-cluster"></a>Kümeye erişim

Güvenlik duvarını başarıyla ayarladıktan sonra, `https://CLUSTERNAME-int.azurehdinsight.net` sanal ağın Içinden ambarı 'na erişmek için iç uç noktasını () kullanabilirsiniz.

Genel uç noktasını ( `https://CLUSTERNAME.azurehdinsight.net` ) veya SSH uç noktasını () kullanmak için `CLUSTERNAME-ssh.azurehdinsight.net` , [burada](../firewall/integrate-lb.md)açıklanan asimetrik yönlendirme sorununa engel olmak için yol tablosunda ve NSG kurallarında doğru yollara sahip olduğunuzdan emin olun. Özellikle bu durumda, gelen NSG kurallarında istemci IP adresine izin vermeniz ve sonraki atlama kümesiyle birlikte Kullanıcı tanımlı yol tablosuna de eklemeniz gerekir `internet` . Yönlendirme doğru ayarlanmamışsa bir zaman aşımı hatası görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure HDInsight sanal ağ mimarisi](hdinsight-virtual-network-architecture.md)
* [Ağ sanal gereci yapılandırma](./network-virtual-appliance.md)
