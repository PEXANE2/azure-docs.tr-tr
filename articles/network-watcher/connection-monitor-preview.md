---
title: Bağlantı Monitörü (Önizleme) | Microsoft Dokümanlar
description: Dağıtılmış bir ortamda ağ iletişimini izlemek için Bağlantı İzleyicisi'ni (Önizleme) nasıl kullanacağınızı öğrenin.
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
tags: azure-resource-manager
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: vinigam
ms.custom: mvc
ms.openlocfilehash: 8f3a6f002fbebe215699c9b97a6dce63177c446f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77599333"
---
# <a name="network-connectivity-monitoring-with-connection-monitor-preview"></a>Bağlantı Monitörü ile Ağ Bağlantısı İzleme (Önizleme)

Bağlantı İzleyicisi (Önizleme), Azure Ağ İzleyicisi'nde birleştirilmiş uçdan uca bağlantı izleme sağlar. Bağlantı İzleyicisi (Önizleme) özelliği karma ve Azure bulut dağıtımlarını destekler. Ağ İzleyicisi, Azure dağıtımlarınız için bağlantıyla ilgili ölçümleri izlemek, tanılamak ve görüntülemek için araçlar sağlar.

Bağlantı İzleyicisi (Preview) için bazı kullanım örnekleri aşağıda veda edilmiştir:

- Ön uç web sunucusu VM çok katmanlı bir uygulamada bir veritabanı sunucusu VM ile iletişim kurar. İki VM arasındaki ağ bağlantısını denetlemek istiyorsunuz.
- Doğu ABD bölgesindeki VM'lerin Orta ABD bölgesindeki VM'leri pinglemasını ve bölgeler arası ağ gecikmelerini karşılaştırmak istiyorsunuz.
- Seattle, Washington ve Ashburn, Virginia'da birden fazla ofis var. Ofis siteleriniz Office 365 URL'lerine bağlanır. Office 365 URL'lerini kullananlarınız için Seattle ve Ashburn arasındaki gecikmeleri karşılaştırın.
- Karma uygulamanızın bir Azure Depolama bitiş noktasına bağlantı ihtiyacı vardır. Şirket içi siteniz ve Azure uygulamanız aynı Azure Depolama bitiş noktasına bağlanır. Şirket içi sitenin gecikmelerini Azure uygulamasının gecikmesi ile karşılaştırmak istiyorsunuz.
- Şirket içi kurulumlarınız ile bulut uygulamanızı barındıran Azure Sanal Ayarları arasındaki bağlantıyı denetlemek istiyorsunuz.

Önizleme aşamasında Connection Monitor en iyi iki özelliği bir leştirir: Ağ [İzleyicisi Bağlantı Monitörü](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) özelliği ve Ağ Performans Monitörü (NPM) [Service Connectivity Monitor](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity) özelliği.

Bağlantı İzleyicisi'nin bazı avantajları (Önizleme):

* Azure ve karma izleme ihtiyaçları için birleşik, sezgisel deneyim
* Bölgeler arası, çapraz çalışma alanı bağlantısı izleme
* Daha yüksek probing frekansları ve ağ performansında daha iyi görünürlük
* Karma dağıtımlarınız için daha hızlı uyarı
* HTTP, TCP ve ICMP tabanlı bağlantı denetimleri desteği 
* Hem Azure hem de Azure olmayan test kurulumları için Ölçümler ve Log Analytics desteği

![Bağlantı İzleyicisi'nin Azure Sanal Bilgisayarlar, Azure olmayan ana bilgisayarlar, uç noktalar ve veri depolama konumlarıyla nasıl etkileşimde bulundugünü gösteren diyagram](./media/connection-monitor-2-preview/hero-graphic.png)

İzleme için Bağlantı İzleyicisi 'ni (Önizleme) kullanmaya başlamak için aşağıdaki adımları izleyin: 

1. İzleme aracılarını yükleyin.
1. Aboneliğinizde Ağ İzleyicisi'ni etkinleştirin.
1. Bir bağlantı monitörü oluşturun.
1. Veri analizi ve uyarıları ayarlayın.
1. Abunuzdaki sorunları tanıla.

Aşağıdaki bölümlerde bu adımlar için ayrıntılar sağlayın.

## <a name="install-monitoring-agents"></a>İzleme aracılarını yükleme

Bağlantı İzleyicisi, bağlantı denetimlerini çalıştırmak için hafif yürütülebilir dosyalara güvenir.  Hem Azure ortamlarından hem de şirket içi ortamlardan gelen bağlantı denetimlerini destekler. Kullandığınız yürütülebilir dosya, VM'nizin Azure'da mı yoksa şirket içinde mi barındırılan olduğuna bağlıdır.

### <a name="agents-for-azure-virtual-machines"></a>Azure sanal makineleri için aracılar

Connection Monitor'un Azure VM'lerinizi izleme kaynağı olarak tanımasını sağlamak için, Ağ İzleyicisi Aracısı sanal makine uzantısını yükleyin. Bu uzantı, *Ağ İzleyicisi uzantısı*olarak da bilinir. Azure sanal makineleri, uçlardan uca izlemeyi ve diğer gelişmiş işlevleri tetiklemek için uzantı gerektirir. 

[Bir VM oluştururken](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm)Ağ İzleyicisi uzantısını yükleyebilirsiniz. Ayrıca [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows)için Ağ İzleyicisi uzantısını ayrı ayrı yükleyebilir, yapılandırabilir ve sorun giderebilirsiniz.

Ağ güvenlik grubu (NSG) veya güvenlik duvarı kuralları kaynak ve hedef arasındaki iletişimi engelleyebilir. Bağlantı İzleyicisi bu sorunu algılar ve topolojide tanısal bir ileti olarak gösterir. Bağlantı izlemeyi etkinleştirmek için, NSG ve güvenlik duvarı kurallarının, kaynak ve hedef arasında TCP veya ICMP üzerinden paketlere izin verdiğinden emin olun.

### <a name="agents-for-on-premises-machines"></a>Şirket içi makineler için aracılar

Connection Monitor'un şirket içi makinelerinizi izleme kaynağı olarak tanımasını sağlamak için, Log Analytics aracısını makinelere yükleyin. Ardından Ağ Performans İzleyicisi çözümünü etkinleştirin. Bu aracılar Log Analytics çalışma alanlarına bağlıdır, bu nedenle aracıların izlemeye başlamadan önce çalışma alanı kimliğini ve birincil anahtarı ayarlamanız gerekir.

Windows makineleri için Log Analytics aracısını yüklemek [için Windows için Azure Monitor sanal makine uzantısına](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows)bakın.

Yol güvenlik duvarları veya ağ sanal cihazları (NV'ler) içeriyorsa, hedefe ulaşılabildiğinizden emin olun.

## <a name="enable-network-watcher-on-your-subscription"></a>Aboneliğinizde Ağ İzleyicisini etkinleştirme

Sanal ağa sahip tüm abonelikler Network Watcher ile etkinleştirilir. Aboneliğinizde sanal ağ oluşturduğunuzda, Ağ İzleyicisi sanal ağın bölgesinde ve aboneliğinde otomatik olarak etkinleştirilir. Bu otomatik etkinleştirme kaynaklarınızı etkilemez veya bir ücrete tabi değildir. Ağ İzleyicisi'nin aboneliğinizde açıkça devre dışı bırakılmadığından emin olun. 

Daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/network-watcher/network-watcher-create)

## <a name="create-a-connection-monitor"></a>Bağlantı izleyicisi oluşturma 

Bağlantı Monitörü iletişimi düzenli aralıklarla izler. Ulaşılabilirlik ve gecikme deki değişiklikler hakkında sizi bilgilendirir. Kaynak aracılar ve hedef uç noktalar arasındaki geçerli ve geçmiş ağ topolojisini de kontrol edebilirsiniz.

Kaynaklar, Azure VM'leri veya yüklü bir izleme aracısı olan şirket içi makineler olabilir. Hedef uç noktalar Office 365 URL'leri, Dynamics 365 URL'leri, özel URL'ler, Azure VM kaynak adları, IPv4, IPv6, FQDN veya herhangi bir etki alanı adı olabilir.

### <a name="access-connection-monitor-preview"></a>Erişim Bağlantı Monitörü (Önizleme)

1. Azure portalı giriş **sayfasında, Ağ İzleyicisi'ne**gidin.
1. Solda, **İzleme** bölümünde **Bağlantı İzleyicisi (Önizleme) seçeneğini**belirleyin.
1. Bağlantı İzleyicisi'nde (Önizleme) oluşturulan tüm bağlantı monitörlerini görürsünüz. Bağlantı Monitörü'nün klasik deneyiminde oluşturulan bağlantı monitörlerini görmek için **Bağlantı İzleyicisi** sekmesine gidin.

    ![Bağlantı Monitörü'nde oluşturulan bağlantı monitörlerini gösteren ekran görüntüsü (Önizleme)](./media/connection-monitor-2-preview/cm-resource-view.png)


### <a name="create-a-connection-monitor"></a>Bağlantı izleyicisi oluşturma

Bağlantı İzleyicisi'nde (Önizleme) oluşturduğunuz bağlantı monitörlerinde, hem şirket içi makineleri hem de Azure VM'lerini kaynak olarak ekleyebilirsiniz. Bu bağlantı monitörleri uç noktalara bağlantı izleyebilir. Uç noktalar Azure'da veya başka bir URL veya IP'de olabilir.

Bağlantı İzleyicisi (Önizleme) aşağıdaki varlıkları içerir:

* **Bağlantı izleme kaynağı** – Bölgeye özgü bir Azure kaynağı. Aşağıdaki varlıkların tümü bir bağlantı izleme kaynağının özellikleridir.
* **Bitiş Noktası** – Bağlantı denetimlerine katılan bir kaynak veya hedef. Son noktalara örnek olarak Azure VM'ler, şirket içi aracılar, URL'ler ve IP'ler verilebilir.
* **Test yapılandırması** – Bir test için protokole özgü bir yapılandırma. Seçtiğiniz protokole bağlı olarak, bağlantı noktasını, eşikleri, test sıklığını ve diğer parametreleri tanımlayabilirsiniz.
* **Test grubu** – Kaynak uç noktaları, hedef uç noktaları ve test yapılandırmaları içeren grup. Bağlantı monitörü birden fazla test grubu içerebilir.
* **Test** – Kaynak bitiş noktası, hedef bitiş noktası ve test yapılandırmasının birleşimi. Test, izleme verilerinin kullanılabildiği en ayrıntılı düzeydir. İzleme verileri, başarısız olan çeklerin yüzdesini ve gidiş-dönüş süresini (RTT) içerir.

 ![Bağlantı monitörünü gösteren, test grupları ve testler arasındaki ilişkiyi tanımlayan diyagram](./media/connection-monitor-2-preview/cm-tg-2.png)

#### <a name="create-a-connection-monitor-from-the-azure-portal"></a>Azure portalından bağlantı monitörü oluşturma

Azure portalından bir bağlantı monitörü oluşturmak için aşağıdaki adımları izleyin:

1. Bağlantı **Monitörü (Önizleme)** panosunda, sol üst köşede **Oluştur'u**seçin.
1. Temel **Bilgiler** sekmesinde, bağlantı monitörünüz için bilgileri girin:
   * **Bağlantı Monitörü Adı** – Bağlantı monitörünüzün adını ekleyin. Azure kaynakları için standart adlandırma kurallarını kullanın.
   * **Abonelik** – Bağlantı monitörünüz için bir abonelik seçin.
   * **Bölge** – Bağlantı monitörünüz için bir bölge seçin. Yalnızca bu bölgede oluşturulan kaynak VM'leri seçebilirsiniz.
   * **Çalışma alanı yapılandırması** - Çalışma alanınız izleme verilerinizi tutar. Özel bir çalışma alanı veya varsayılan çalışma alanı kullanabilirsiniz. 
       * Varsayılan çalışma alanını kullanmak için onay kutusunu seçin. 
       * Özel bir çalışma alanı seçmek için onay kutusunu temizleyin. Ardından, özel çalışma alanınız için aboneliği ve bölgeyi seçin. 
1. Sekmenin alt kısmında **İleri: Test grupları'nı**seçin.

   ![Bağlantı Monitöründeki Temeller sekmesini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/create-cm-basics.png)

1. Test **grupları** sekmesinde **+ Test grubu'nu**seçin. Test gruplarınızı ayarlamak için [Bağlantı İzleyicisi'nde test grupları oluştur'a](#create-test-groups-in-a-connection-monitor)bakın. 
1. Sekmenin alt kısmında, Bağlantı monitörünüzü incelemek için **İleri: Gözden Geçir + oluştur'u** seçin.

   ![Test grupları sekmesini ve test grubu ayrıntılarını eklediğiniz bölmeyi gösteren ekran görüntüsü](./media/connection-monitor-2-preview/create-tg.png)

1. Gözden **Geçir + oluştur** sekmesinde, bağlantı monitörünü oluşturmadan önce temel bilgileri ve test gruplarını gözden geçirin. Bağlantı monitörünü delemniz gerekiyorsa:
   * Temel ayrıntıları nidaetmek için kalem simgesini seçin.
   * Bir test grubunu yeniden seçmek için seçin.

   > [!NOTE] 
   > **Gözden Geçir + oluştur** sekmesi, Bağlantı İzleyicisi önizleme aşamasında aylık maliyeti gösterir. Şu anda, **CARI MALİYET** sütununda ücret belirtilmektedir. Bağlantı İzleyicisi genel kullanıma sunulduğunda, bu sütun aylık bir ücret gösterir. 
   > 
   > Connection Monitor önizleme aşamasında bile Log Analytics yutma ücretleri uygulanır.

1. Bağlantı monitörünü oluşturmaya hazır olduğunuzda, Gözden Geçir **+ oluştur** sekmesinin en altında **Oluştur'u**seçin.

   ![Bağlantı Monitörü ekran görüntüsü, Gözden Geçirme + oluşturma sekmesini gösteren](./media/connection-monitor-2-preview/review-create-cm.png)

Bağlantı İzleyicisi (Önizleme), arka planda bağlantı izleme kaynağı oluşturur.

#### <a name="create-a-connection-monitor-by-using-armclient"></a>ARMClient kullanarak bağlantı monitörü oluşturma

ARMClient kullanarak bir bağlantı monitörü oluşturmak için aşağıdaki kodu kullanın.

```armclient
$connectionMonitorName = "sampleConnectionMonitor"

$ARM = "[https://](https://apac01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fbrazilus.management.azure.com&amp;data=02%7C01%7CManasi.Sant%40microsoft.com%7Cd900da4ed7f24366842108d68022159b%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C0%7C636837281231186904&amp;sdata=qHL8zWjkobY9MatRpAVbODwboKSQAqqEFOMnjmfyOnU%3D&amp;reserved=0)management.azure.com"

$SUB = "subscriptions/\&lt;subscription id 1\&gt;"

$NW = "resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher\_centraluseuap"

$body =

"{

location: 'eastus',

properties: {

endpoints: [{

name: 'workspace',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourcegroups/\&lt;resource group\&gt;/providers/Microsoft.OperationalInsights/workspaces/sampleWorkspace',

filter: {

 items: [{

type: 'AgentAddress',

address: '\&lt;FQDN of your on-premises agent'

}]

}

          },

 {

name: 'vm1',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourceGroups/\&lt;resource group\&gt;/providers/Microsoft.Compute/virtualMachines/\&lt;vm-name\&gt;'

},

 {

name: 'vm2',

resourceId: '/subscriptions/\&lt;subscription id\&gt;/resourceGroups/\&lt;resource group\&gt;/providers/Microsoft.Compute/virtualMachines/\&lt;vm-name\&gt;'

   },

{

name: 'azure portal'

address: '\&lt;URL\&gt;'

   },

 {

    name: 'ip',

     address: '\&lt;IP\&gt;'

 }

  ],

  testGroups: [{

    name: 'Connectivity to Azure Portal and Public IP',

    testConfigurations: ['http', 'https', 'tcpEnabled', 'icmpEnabled'],

    sources: ['vm1', 'workspace'],

    destinations: ['azure portal', 'ip']

   },

{

    name: 'Connectivty from Azure VM 1 to Azure VM 2',

    testConfigurations: ['http', 'https', 'tcpDisabled', 'icmpDisabled'],

    sources: ['vm1'],

    destinations: ['vm2'],

    disable: true

   }

  ],

  testConfigurations: [{

    name: 'http',

    testFrequencySec: 60,

    protocol: 'HTTP',

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'https',

    testFrequencySec: 60,

    protocol: 'HTTP',

    httpConfiguration: {

     preferHTTPS: true

    },

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'tcpEnabled',

    testFrequencySec: 30,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80

    },

    successThreshold: {

     checksFailedPercent: 30,

     roundTripTimeMs: 5.2

    }

   }, {

    name: 'icmpEnabled',

    testFrequencySec: 90,

    protocol: 'ICMP',

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'icmpDisabled',

    testFrequencySec: 120,

    protocol: 'ICMP',

    icmpConfiguration: {

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: 50,

     roundTripTimeMs: 3.4

    }

   }, {

    name: 'tcpDisabled',

    testFrequencySec: 45,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80,

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: 30,

     roundTripTimeMs: 5.2

    }

   }

  ]

 }

} "
```

Dağıtım komutu aşağıda veda eder:
```
armclient PUT $ARM/$SUB/$NW/connectionMonitors/$connectionMonitorName/?api-version=2019-07-01 $body -verbose
```

### <a name="create-test-groups-in-a-connection-monitor"></a>Bağlantı monitöründe test grupları oluşturma

Bağlantı monitöründeki her test grubu, ağ parametreleri üzerinde sınanan kaynakları ve hedefleri içerir. Bunlar, test yapılandırmaları üzerinden başarısız olan denetimyüzdesi ve RTT için test edilir.

Azure portalından, bağlantı monitöründe bir test grubu oluşturmak için aşağıdaki alanlar için değerler belirtirsiniz:

* **Test Grubunu Devre Dışı-** Test grubunun belirlediği tüm kaynaklar ve hedefler için izlemeyi devre dışı kalımiçin bu alanı seçebilirsiniz. Bu seçim varsayılan olarak temizlenir.
* **Ad** – Test grubunuzun adını adlandırın.
* **Kaynaklar** – Hem Azure VM'lerini hem de şirket içi makineleri, aracılar yüklüyse kaynak olarak belirtebilirsiniz. Kaynağınız için bir aracı yüklemek için [bkz.](#install-monitoring-agents)
   * Azure aracılarını seçmek için **Azure Aracıları** sekmesini seçin. Burada, bağlantı monitörünü oluşturduğunuzda belirttiğiniz bölgeye bağlı yalnızca VM'ler görürsünüz. Varsayılan olarak, VM'ler ait oldukları abonelik olarak gruplandırılır. Bu gruplar çöktü. 
   
       Abonelik düzeyinden hiyerarşideki diğer düzeylere kadar ayrıntılı olarak ayrıntılı olarak inebilirsiniz:

      **Abonelik** > **Kaynak grupları** > **Subnets** > VM'leri**aracılarla** > **gruplandırır**

      Ayrıca, ağacı başka bir düzeyden başlatmak için **Alana göre Grubun** değerini de değiştirebilirsiniz. Örneğin, sanal ağa göre gruplandırırsanız, hiyerarşide aracılar olan **VNETs** > **Subnets** > **VM'leri aracılarla**birlikte vm'ler görürsünüz.

      ![Kaynak Ekle panelini ve Azure Aracıları sekmesini gösteren Bağlantı Monitörü ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-sources.png)

   * Şirket içi aracıları seçmek için **Azure Olmayan Aracılar** sekmesini seçin. Varsayılan olarak, aracılar bölgeye göre çalışma alanlarına gruplandırılır. Tüm bu çalışma alanları, Ağ Performans İzleyicisi çözümünü yapılandırmış. 
   
       Çalışma alanınıza Ağ Performans Monitörü eklemeniz gerekiyorsa, [Azure Marketi'nden](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview)alın. Ağ Performans Monitörü nasıl ekleyeceğiniz hakkında bilgi için [Azure Monitor'da İzleme çözümlerine](https://docs.microsoft.com/azure/azure-monitor/insights/solutions)bakın. 
   
       Bağlantı **İzleyicisi Oluştur** görünümünde, **Temeller** sekmesinde varsayılan bölge seçilir. Bölgeyi değiştirirseniz, yeni bölgedeki çalışma alanlarından aracılar seçebilirsiniz. **Ayrıca, grubun** değerini alana göre alt ağlara göre gruplandırmaya da değiştirebilirsiniz.

      ![Kaynak Ekle panelini ve Azure Olmayan Aracılar sekmesini gösteren Bağlantı Monitörü ekran görüntüsü](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   * Seçtiğiniz Azure ve Azure olmayan aracıları gözden geçirmek için **Gözden Geçir** sekmesine gidin.

      ![Kaynak Ekle panelini ve Gözden Geçirme sekmesini gösteren Bağlantı Monitörü ekran görüntüsü](./media/connection-monitor-2-preview/review-sources.png)

   * Kaynak eklemeyi bitirdiğinizde, **Kaynak Ekle** panelinin alt **kısmında, Bitti'yi**seçin.

* **Hedefler** – Azure VM'lerine veya herhangi bir uç noktasına (genel IP, URL veya FQDN) bağlantılarını hedef olarak belirterek izleyebilirsiniz. Tek bir test grubunda Azure VM'leri, Office 365 URL'leri, Dynamics 365 URL'leri ve özel uç noktaları ekleyebilirsiniz.

    * Hedef olarak Azure VM'leri seçmek için **Azure VM** sekmesini seçin. Varsayılan olarak, Azure VM'leri **Temel Bilgiler** sekmesinde **Bağlantı İzleyicisi Oluştur** görünümünde seçtiğiniz aynı bölgede bulunan bir abonelik hiyerarşisi olarak gruplandırılır. Bölgeyi değiştirebilir ve yeni seçilen bölgeden Azure VM'lerini seçebilirsiniz. Ardından, Azure Aracıları düzeyi gibi hiyerarşideki abonelik düzeyinden diğer düzeylere kadar ayrıntıya inebilirsiniz.

       ![Azure VM'ler sekmesini gösteren Hedef Ekle bölmesinin ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-dests1.png)

       ![Hedef Ekle bölmesinin, Abonelik düzeyini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-dests2.png)

    * Hedef olarak uç noktaları seçmek için **Uç Noktalar** sekmesini seçin. Bitiş noktaları listesi, ada göre gruplanmış Office 365 test URL'leri ve Dynamics 365 test URL'lerini içerir. Bu uç noktalara ek olarak, aynı bağlantı monitöründeki diğer test gruplarında oluşturulan bir bitiş noktası seçebilirsiniz. 
    
        Yeni bir bitiş noktası eklemek için sağ üst köşede **+ Bitiş Noktaları'nı**seçin. Ardından bir bitiş noktası adı ve URL, IP veya FQDN sağlayın.

       ![Bağlantı İzleyicisi'nde hedef olarak uç noktaların nereye ekleyeceğini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/add-endpoints.png)

    * Seçtiğiniz Azure VM'lerini ve uç noktalarını gözden geçirmek için **Gözden Geçir** sekmesini seçin.
    * Hedef seçmeyi bitirdiğinizde, **Bitti'yi**seçin.

* **Test yapılandırmaları** – Test yapılandırmalarını bir test grubunda ilişkilendirebilirsiniz. Azure portalı test grubu başına yalnızca bir test yapılandırmasına izin verir, ancak daha fazlasını eklemek için ARMClient'ı kullanabilirsiniz.

    * **Ad** – Test yapılandırmasını adlandırın.
    * **Protokol** – TCP, ICMP veya HTTP'yi seçin. HTTP'yi HTTPS olarak değiştirmek için protokol olarak **HTTP'yi** seçin ve bağlantı noktası olarak **443'ü** seçin.
        * **Ağ testi yapılandırması oluşturun** – Bu onay kutusu yalnızca **Protokol** alanında **HTTP'yi** seçerseniz görüntülenir. Yapılandırmanızın başka bir yerinde belirttiğiniz aynı kaynakları ve hedefleri kullanan başka bir test yapılandırması oluşturmak için bu kutuyu seçin. Yeni oluşturulan test yapılandırması adı. `<the name of your test configuration>_networkTestConfig`
        * **İzleme rotasını devre dışı bırakma** – Bu alan, protokolü TCP veya ICMP olan test grupları için geçerlidir. Kaynakların topolojiyi ve hop-by-hop RTT'yi keşfetmesini engellemek için bu kutuyu seçin.
    * **Hedef bağlantı noktası** – Bu alanı seçtiğiniz bir hedef bağlantı noktasıyla özelleştirebilirsiniz.
    * **Test Sıklığı** – Kaynakların belirttiğiniz protokol ve bağlantı noktasında ki hedefleri ne sıklıkta pingleeceğini seçmek için bu alanı kullanın. 30 saniye, 1 dakika, 5 dakika, 15 dakika veya 30 dakika seçebilirsiniz. Kaynaklar, seçtiğiniz değere bağlı olarak hedeflere bağlantı test eder.  Örneğin, 30 saniye seçerseniz, kaynaklar 30 saniyelik bir dönemde hedefe olan bağlantıyı en az bir kez denetler.
    * **Başarı Eşiği** – Aşağıdaki ağ parametrelerinde eşikler ayarlayabilirsiniz:
       * **Denetimler başarısız oldu** – Kaynaklar belirttiğiniz ölçütleri kullanarak hedeflere bağlantıyı denetlediğinde başarısız olabilecek denetimlerin yüzdesini ayarlayın. TCP veya ICMP protokolü için, başarısız çeklerin yüzdesi paket kaybı yüzdesine eşit olabilir. HTTP protokolü için bu alan, yanıt alan http isteklerinin yüzdesini temsil eder.
       * **Gidiş-dönüş süresi** – RTT'yi, kaynakların test yapılandırması üzerinden hedefe bağlanmanın ne kadar sürebileceğini milisaniyecinsinden ayarlayın.
    
       ![Bağlantı Monitörü'nde test yapılandırması nerede ayarlandığıgösteren ekran görüntüsü](./media/connection-monitor-2-preview/add-test-config.png)

Bir test grubuna eklediğiniz tüm kaynaklar, hedefler ve test yapılandırmaları tek tek testlere bölünur. Kaynakların ve hedeflerin nasıl bozulduklarına bir örnek aşağıda verilmiştir:

* Test grubu: TG1
* Kaynaklar: 3 (A, B, C)
* Gidilecek Yerler: 2 (D, E)
* Test yapılandırmaları: 2 (Config 1, Config 2)
* Oluşturulan toplam test sayısı: 12

| Test numarası | Kaynak | Hedef | Test yapılandırması |
| --- | --- | --- | --- |
| 1 | A | D | Config 1 |
| 2 | A | D | Config 2 |
| 3 | A | E | Config 1 |
| 4 | A | E | Config 2 |
| 5 | B | D | Config 1 |
| 6 | B | D | Config 2 |
| 7 | B | E | Config 1 |
| 8 | B | E | Config 2 |
| 9 | C | D | Config 1 |
| 10 | C | D | Config 2 |
| 11 | C | E | Config 1 |
| 12 | C | E | Config 2 |

### <a name="scale-limits"></a>Ölçek sınırları

Bağlantı monitörleri aşağıdaki ölçek sınırlarına sahiptir:

* Bölge başına abonelik başına maksimum bağlantı monitörü: 100
* Bağlantı monitörü başına maksimum test grubu: 20
* Bağlantı monitörü başına maksimum kaynak ve varış noktaları: 100
* Bağlantı monitörü başına maksimum test yapılandırmaları: 
    * ARMClient ile 20
    * Azure portalı üzerinden 2

## <a name="analyze-monitoring-data-and-set-alerts"></a>İzleme verilerini analiz etme ve uyarıları ayarlama

Bir bağlantı monitörü oluşturduktan sonra, kaynaklar test yapılandırmanıza göre hedeflere bağlantı denetimini sağlar.

### <a name="checks-in-a-test"></a>Testte denetimler

Test yapılandırmasında seçtiğiniz protokole bağlı olarak, Bağlantı İzleyicisi (Önizleme) kaynak hedef çifti için bir dizi denetim çalıştırAr. Denetimler seçtiğiniz test sıklığına göre çalışır.

HTTP kullanıyorsanız, hizmet yanıt kodunu döndüren HTTP yanıtlarının sayısını hesaplar. Sonuç, başarısız olan denetimlerin yüzdesini belirler. RTT'yi hesaplamak için, hizmet bir HTTP çağrısı ile yanıt arasındaki süreyi ölçer.

TCP veya ICMP kullanıyorsanız, hizmet başarısız çeklerin yüzdesini belirlemek için paket kaybı yüzdesini hesaplar. RTT'yi hesaplamak için, hizmet gönderilen paketler için bildirim (ACK) almak için gereken süreyi ölçer. Ağ testleriniz için izleme yolu verilerini etkinleştirlediyseniz, şirket içi ağınız için atlama kaybı ve gecikme gecikmesi görebilirsiniz.

### <a name="states-of-a-test"></a>Bir testin durumları

Denetimlerin döndürdüğü verilere göre, testlerin şeyi şereleri olabilir:

* **Geç** – Başarısız çeklerin ve RTT'nin yüzdesi için gerçek değerler belirtilen eşikler içindedir.
* **Fail** – Başarısız çeklerin veya RTT'nin yüzdesi için gerçek değerler belirtilen eşikleri aştı. Eşik belirtilmemişse, başarısız denetimlerin yüzdesi 100 olduğunda bir test Başarısız durumuna ulaşır.
* **Uyarı** – Başarısız çekyüzdesi için ölçüt belirtilmedi. Belirtilen ölçütlerin yokluğunda, Bağlantı İzleyicisi (Önizleme) otomatik olarak bir eşik atar. Bu eşik aşıldığında, test durumu Uyarı olarak değişir.

### <a name="data-collection-analysis-and-alerts"></a>Veri toplama, analiz ve uyarılar

Bağlantı İzleyicisi'nin (Preview) topladığı veriler Log Analytics çalışma alanında depolanır. Bağlantı izleme sini oluşturduğunuzda bu çalışma alanını ayarlarsınız. 

İzleme verileri Azure Monitör Ölçümleri'nde de kullanılabilir. İzleme verilerinizi istediğiniz kadar tutmak için Log Analytics'i kullanabilirsiniz. Azure Monitor varsayılan olarak ölçümleri yalnızca 30 gün boyunca depolar. 

Veriler [üzerinde metrik tabanlı uyarılar](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/)ayarlayabilirsiniz.

#### <a name="monitoring-dashboards"></a>Panoları izleme

İzleme panolarında, abonelikleriniz, bölgeleriniz, zaman damgalarınız, kaynaklarınız ve hedef türleriniz için erişebileceğiniz bağlantı izleyicilerinin bir listesini görürsünüz.

Ağ İzleyicisi'nden Bağlantı İzleyicisi'ne (Önizleme) gittiğinizde, verileri şu şekilde görüntüleyebilirsiniz:

* **Bağlantı monitörü** – Abonelikleriniz, bölgeleriniz, zaman damgalarınız, kaynaklarınız ve hedef türleriniz için oluşturulan tüm bağlantı monitörlerinin listesi. Bu görünüm varsayılandır.
* **Test grupları** – Abonelikleriniz, bölgeleriniz, zaman damgalarınız, kaynaklarınız ve hedef türleriniz için oluşturulan tüm test gruplarının listesi. Bu test grupları bağlantı monitörleri tarafından filtrelenmez.
* **Test** – Abonelikleriniz, bölgeleriniz, zaman damgalarınız, kaynaklarınız ve hedef türleriniz için çalışan tüm testlerin listesi. Bu testler bağlantı monitörleri veya test grupları tarafından filtrelenmez.

Aşağıdaki resimde, üç veri görünümü ok 1 ile gösterilir.

Panoda, test gruplarını görmek için her bağlantı monitörünü genişletebilirsiniz. Ardından, içinde çalışan testleri görmek için her test grubunu genişletebilirsiniz. 

Bir listeyi şu temele göre filtreleyebilirsiniz:

* **Üst düzey filtreler** – Abonelikleri, bölgeleri, zaman damgası kaynaklarını ve hedef türlerini seçin. Aşağıdaki resimdeki kutu 2'ye bakın.
* **Durum tabanlı filtreler** – Bağlantı izleme, test grubu veya testdurumuna göre filtreuygulayın. Aşağıdaki resimdeki ok 3'e bakın.
* **Özel filtreler** – Genel arama yapmak için **Tümünü Seç'i** seçin. Belirli bir varlığa göre arama yapmak için açılan listeden seçim yapın. Aşağıdaki resimdeki ok 4'e bakın.

![Bağlantı Monitörü'nde bağlantı monitörlerinin, test gruplarının ve testlerin görünümlerine nasıl filtre uygulanış gösteriş görüntüleyici ekran görüntüsü (Önizleme)](./media/connection-monitor-2-preview/cm-view.png)

Örneğin, kaynak IP'nin 10.192.64.56 olduğu Bağlantı İzleyicisi'ndeki (Önizleme) tüm testlere bakmak için:
1. Görünümü **Test**olarak değiştirin.
1. Arama alanında, *10.192.64.56* yazın
1. Açılan **listede, Kaynaklar'ı**seçin.

Kaynak IP'nin 10.192.64.56 olduğu Bağlantı Monitörü'nde (Önizleme) yalnızca başarısız testleri göstermek için:
1. Görünümü **Test**olarak değiştirin.
1. Durum tabanlı filtre için **Başarısız'ı**seçin.
1. Arama alanında, *10.192.64.56* yazın
1. Açılan **listede, Kaynaklar'ı**seçin.

Yalnızca hedefin outlook.office365.com bağlantı monitöründe (Önizleme) yalnızca başarısız testleri göstermek için:
1. Görünümü **Test**olarak değiştirin.
1. Durum tabanlı filtre için **Başarısız'ı**seçin.
1. Arama *alanına, outlook.office365.com* girin
1. Açılan **listede, Hedefler'i**seçin.

   ![yalnızca Outlook.Office365.com hedef için başarısız testleri göstermek için filtre uygulanmış bir görünümü gösteren ekran görüntüsü](./media/connection-monitor-2-preview/tests-view.png)

RTT'deki eğilimleri ve bağlantı monitörü için başarısız denetimlerin yüzdesini görüntülemek için:
1. Araştırmak istediğiniz bağlantı monitörünü seçin. Varsayılan olarak, izleme verileri test grubuna göre düzenlenir.

   ![Test grubuna göre görüntülenen bağlantı monitörü ölçümlerini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/cm-drill-landing.png)

1. Araştırmak istediğiniz test grubunu seçin.

   ![Test grubunun seçilebileceği yeri gösteren ekran görüntüsü](./media/connection-monitor-2-preview/cm-drill-select-tg.png)

    TEST grubunuzun RTT'ye veya başarısız denetimlerin yüzdesini temel alan en iyi beş başarısız testini görürsünüz. Her test için, başarısız denetimlerin yüzdesi için RTT ve eğilim çizgilerini görürsünüz.
1. Listeden bir test seçin veya araştırmak için başka bir test seçin. Zaman aralığınız ve başarısız denetimlerin yüzdesi için eşik ve gerçek değerleri görürsünüz. RTT için eşik, ortalama, minimum ve maksimum değerleri görürsünüz.

   ![RTT için bir test sonuçlarını ve başarısız denetimlerin yüzdesini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/cm-drill-charts.png)

1. Daha fazla veri görüntülemek için zaman aralığını değiştirin.
1. Kaynakları, hedefleri veya test yapılandırmalarını görmek için görünümü değiştirin. 
1. Başarısız testlere dayalı bir kaynak seçin ve en iyi beş başarısız testi araştırın. Örneğin, bağlantı monitöründeki ilgili testleri araştırmak için**Kaynaklara** **Göre** > Görüntüle ve > **Hedeflere** **Göre Görüntüle'yi**seçin.

   ![Başarısız olan ilk beş testiçin performans ölçümlerini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/cm-drill-select-source.png)

RTT'deki eğilimleri ve bir test grubu için başarısız denetimlerin yüzdesini görüntülemek için:

1. Araştırmak istediğiniz test grubunu seçin. 

    Varsayılan olarak, izleme verileri kaynaklara, hedeflere ve test yapılandırmalarına (testler) göre düzenlenir. Daha sonra, görünümü test gruplarından kaynaklara, hedeflere veya test yapılandırmalarına değiştirebilirsiniz. Ardından, en iyi beş başarısız testi araştırmak için bir varlık seçin. Örneğin, seçili bağlantı monitöründeki ilgili testleri araştırmak için görünümü kaynaklar ve hedeflerle değiştirin.
1. Araştırmak istediğiniz testi seçin.

   ![Testin nerede seçilebileceğinizi gösteren ekran görüntüsü](./media/connection-monitor-2-preview/tg-drill.png)

    Zaman aralığınız ve başarısız denetim yüzdeniz için eşik değerlerini ve gerçek değerleri görürsünüz. RTT için eşik, ortalama, minimum ve maksimum değerler görürsünüz. Ayrıca seçtiğiniz test için uyarılarını da görürsünüz.
1. Daha fazla veri görüntülemek için zaman aralığını değiştirin.

RTT'deki eğilimleri ve bir test için başarısız denetimlerin yüzdesini görüntülemek için:
1. Araştırmak istediğiniz kaynak, hedef ve test yapılandırmasını seçin.

    Zaman aralığınız ve başarısız denetimlerin yüzdesi için eşik değerlerini ve gerçek değerleri görürsünüz. RTT için eşik, ortalama, minimum ve maksimum değerler görürsünüz. Ayrıca seçtiğiniz test için uyarılarını da görürsünüz.

   ![Test için ölçümleri gösteren ekran görüntüsü](./media/connection-monitor-2-preview/test-drill.png)

1. Ağ topolojisini görmek için **Topoloji'yi**seçin.

   ![Ağ Topoloji sekmesini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/test-topo.png)

1. Topolojide tanımlanan sorunları görmek için, yoldaki herhangi bir atlamayı seçin. (Bu atlamalar Azure kaynaklarıdır.) Bu işlevsellik şu anda şirket içi ağlar için kullanılamaz.

   ![Topoloji sekmesinde seçili bir atlama bağlantısını gösteren ekran görüntüsü](./media/connection-monitor-2-preview/test-topo-hop.png)

#### <a name="log-queries-in-log-analytics"></a>Log Analytics'te günlük sorguları

İzleme verilerinizin özel görünümlerini oluşturmak için Günlük Analizi'ni kullanın. UI'nin görüntülemesi tüm veriler Log Analytics'ten dir. Depodaki verileri etkileşimli olarak analiz edebilirsiniz. Aracı Durumu'ndan veya Log Analytics'te bulunan diğer çözümlerden gelen verileri ilişkilendirin. Verileri Excel veya Power BI'ye dışa aktarın veya paylaşılabilir bir bağlantı oluşturun.

#### <a name="metrics-in-azure-monitor"></a>Azure İzleyicisi'nde ölçümler

Bağlantı İzleyicisi (Önizleme) deneyiminden önce oluşturulan bağlantı monitörlerinde, dört ölçümün tümü de kullanılabilir: % Sondalar Başarısız, AverageRoundTripMs, ChecksFailedPercent (Önizleme) ve RoundTripTimeM'ler (Önizleme). Bağlantı İzleyicisi (Önizleme) deneyiminde oluşturulan bağlantı monitörlerinde veriler yalnızca *(Önizleme)* ile etiketlenen ölçümler için kullanılabilir.

![Bağlantı Monitörü'nde ölçümleri gösteren ekran görüntüsü (Önizleme)](./media/connection-monitor-2-preview/monitor-metrics.png)

Ölçümleri kullandığınızda, kaynak türünü Microsoft.Network/networkWatchers/connectionMonitors olarak ayarlayın

| Ölçüm | Görünen ad | Birim | Toplama türü | Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | % Sondalar Başarısız Oldu | Yüzde | Ortalama | Bağlantı izleme sondalarının yüzdesi başarısız oldu. | Boyut yok |
| AverageRoundtripMs | Avg. Gidiş-dönüş Saati (ms) | Milisaniye | Ortalama | Kaynak ve hedef arasında gönderilen bağlantı izleme probları için ortalama ağ RTT. |             Boyut yok |
| ChecksFailedPercent (Önizleme) | % Denetler Başarısız (Önizleme) | Yüzde | Ortalama | Bir test için başarısız denetimyüzdesi. | BağlantıMonitorResourceId <br>Kaynak Adresi <br>Kaynak Adı <br>SourceResourceId <br>KaynakTürü <br>Protokol <br>Hedef Adres <br>Hedef Adı <br>HedefKaynakId <br>Destinationtype <br>Hedef Port <br>TestGroupName <br>TestConfigurationName <br>Bölge |
| RoundTripTimeMs (Önizleme) | Gidiş-dönüş Süresi (ms) (Önizleme) | Milisaniye | Ortalama | Kaynak ve hedef arasında gönderilen denetimler için RTT. Bu değer ortalama değildir. | BağlantıMonitorResourceId <br>Kaynak Adresi <br>Kaynak Adı <br>SourceResourceId <br>KaynakTürü <br>Protokol <br>Hedef Adres <br>Hedef Adı <br>HedefKaynakId <br>Destinationtype <br>Hedef Port <br>TestGroupName <br>TestConfigurationName <br>Bölge |

#### <a name="metric-alerts-in-azure-monitor"></a>Azure Monitör'de metrik uyarılar

Azure Monitor'da bir uyarı oluşturmak için:

1. Bağlantı İzleyicisi'nde (Önizleme) oluşturduğunuz bağlantı izleme kaynağını seçin.
1. **Metrik'in** bağlantı monitörü için sinyal türü olarak ortaya çıktığından emin olun.
1. **İşaret Adı**Için **Koşul Ekle'de** **ChecksFailedPercent(Preview)** veya **RoundTripTimeMs(Preview)** seçeneğini belirleyin.
1. **Sinyal Türü**için **Ölçümler'i**seçin. Örneğin, **ChecksFailedPercent(Preview)** seçeneğini belirleyin.
1. Metrik için tüm boyutlar listelenir. Boyut adını ve boyut değerini seçin. Örneğin, **Kaynak Adresi** seçin ve ardından bağlantı monitörünüze herhangi bir kaynağın IP adresini girin.
1. **Alert Logic'te**aşağıdaki ayrıntıları doldurun:
   * **Koşul Türü**: **Statik**.
   * **Koşul** ve **Eşik**.
   * **Toplama Tanecikleri ve Değerlendirme Sıklığı**: Bağlantı Monitörü (Önizleme) verileri her dakika günceller.
1. **Eylemler'de**eylem grubunuzu seçin.
1. Uyarı ayrıntıları sağlayın.
1. Uyarı kuralını oluşturun.

   ![Azure Monitor'da Oluştur kuralı alanını gösteren ekran görüntüsü; "Kaynak adresi" ve "Kaynak uç nokta adı" vurgulanır](./media/connection-monitor-2-preview/mdm-alerts.jpg)

## <a name="diagnose-issues-in-your-network"></a>Abunuzdaki sorunları tanılama

Bağlantı İzleyicisi (Önizleme), bağlantı monitörünüzdeki ve ağınızdaki sorunları tanılamanıza yardımcı olur. Karma ağınızdaki sorunlar, daha önce yüklediğiniz Log Analytics aracıları tarafından algılanır. Azure'daki sorunlar Ağ İzleyicisi uzantısı tarafından algılanır. 

Ağ topolojisinde Azure ağındaki sorunları görüntüleyebilirsiniz.

Kaynakları şirket içi VM olan ağlar için aşağıdaki sorunlar algılanabilir:

* İstek zaman doldu.
* Bitiş noktası DNS tarafından çözülmedi – geçici veya kalıcı. URL geçersiz.
* Ev sahibi bulunamadı.
* Kaynak hedefe bağlanamıyor. HEDEF ICMP üzerinden ulaşılamıyor.
* Sertifika ile ilgili sorunlar: 
    * Aracının kimliğini doğrulamak için istemci sertifikası gereklidir. 
    * Sertifika yer değiştirme listesine erişilemez. 
    * Bitiş noktasının ana bilgisayar adı, sertifikanın öznesi veya özne alternatif adı ile eşleşmez. 
    * Kaynak'ın Yerel Bilgisayar Güvenilen Sertifika Yetkilileri deposunda kök sertifikası eksik. 
    * SSL sertifikasının süresi doldu, geçersiz, iptal edildi veya uyumsuz.

Kaynakları Azure VM olan ağlar için aşağıdaki sorunlar algılanabilir:

* Aracı sorunları:
    * Ajan durdu.
    * Başarısız DNS çözünürlüğü.
    * Hedef bağlantı noktasında dinleme uygulaması veya dinleyici yok.
    * Soket açılamadı.
* VM durum sorunları: 
    * Başlatılıyor
    * Durduruluyor
    * Durduruldu
    * Serbest bırakılıyor
    * Serbest bırakıldı
    * Yeni -den başlatma
    * Tahsis edilmedi
* ARP tablo girişi eksik.
* Yerel güvenlik duvarı sorunları veya NSG kuralları nedeniyle trafik engellendi.
* Sanal ağ ağ geçidi sorunları: 
    * Kayıp rotalar.
    * İki ağ geçidi arasındaki tünel bağlantısı kesilmiş veya eksik.
    * İkinci geçit tünelin yanında bulunamadı.
    * Hiçbir eşleme bilgisi bulunamadı.
* Microsoft Edge'de rota eksikti.
* Trafik sistem yolları veya UDR nedeniyle durduruldu.
* Ağ geçidi bağlantısında BGP etkin değil.
* DIP sondası yük dengeleyicisi aşağı.
