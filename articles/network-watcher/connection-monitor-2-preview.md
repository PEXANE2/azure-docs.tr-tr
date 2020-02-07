---
title: Bağlantı Izleyicisi (Önizleme) | Microsoft Docs
description: Dağıtılmış bir ortamdaki ağ iletişimini izlemek için bağlantı Izleyicisi 'ni (Önizleme) nasıl kullanacağınızı öğrenin
services: network-watcher
documentationcenter: na
author: vinynigam
manager: agummadi
editor: ''
tags: azure-resource-manager
Customer intent: I need to monitor communication between a VM and another VM. If the communication fails, I need to know why, so that I can resolve the problem.
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/27/2020
ms.author: vinigam
ms.custom: mvc
ms.openlocfilehash: 5dc705fbd17a12ee001e1e8de15b49e841f08b81
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/06/2020
ms.locfileid: "77049620"
---
# <a name="unified-connectivity-monitoring-with-connection-monitor-preview"></a>Bağlantı Izleyicisi ile Birleşik bağlantı izleme (Önizleme)

Bağlantı Izleyicisi (Önizleme), karma ve Azure bulut dağıtımları için Azure ağ Izleyicisi 'nde birleştirilmiş uçtan uca bağlantı izleme olanakları sağlar. Azure ağ Izleyicisi, Azure dağıtımlarınız için bağlantı ile ilgili ölçümleri izlemek, tanılamak ve görüntülemek için araçlar sağlar.

Anahtar kullanım örnekleri:

- Çok katmanlı bir uygulamadaki veritabanı sunucusu VM 'si ile iletişim kuran bir ön uç Web sunucusu sanal makinenizin olması gerekir. İki VM arasındaki ağ bağlantısını kontrol etmek istiyorsunuz.
- Doğu ABD bölgesindeki VM 'Lerin Orta ABD bölgedeki VM 'Lere ping ve çapraz bölge ağ gecikmelerinin karşılaştırmasını istiyorsunuz
- Seattle gibi şehirlerde birden çok şirket içi Office siteniz var. Office 365 URL 'Lerine bağlanılıyor. Seattle ve Ashburn 'tan Office 365 URL 'Lerini kullanarak kullanıcıların yaşadığı gecikme sürelerini karşılaştırmak istiyorsunuz.
- Bir Azure depolama uç noktası için bağlantı gerektiren bir karma uygulama ayarlamış olmanız gerekir. Şirket içi bir site ile Azure uygulaması arasındaki gecikmeleri, her ikisi de aynı Azure depolama uç noktasına bağlanarak karşılaştırmak istiyorsunuz.
- Bulut uygulamanızı barındıran Azure VM 'lerinin şirket içi kurulumlarınız için bağlantısını kontrol etmek istiyorsunuz.

Bu önizleme aşamasında, çözüm iki temel özellik olan ağ [Izleyicisinin Bağlantı İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) ve ağ performansı İzleyicisi (NPM) [hizmet bağlantı İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity)' ne en iyi şekilde bir araya getirir.

Lamaları

* Azure ve karma izleme ihtiyaçlarına yönelik Birleşik, sezgisel deneyim
* Bölgeler arası, çapraz çalışma alanı bağlantısı izleme
* Daha yüksek yoklama sıklıkları ve ağ performansına daha iyi görünürlük
* Karma dağıtımlarınız için daha hızlı uyarı
* HTTP, TCP ve ıCMP tabanlı bağlantı denetimleri için destek
* Hem Azure hem de Azure olmayan test kurulumları için ölçümler ve Log Analytics desteği

![Bağlantı İzleyicisi](./media/connection-monitor-2-preview/hero-graphic.png)

Bağlantı Izleyicisi 'ni (Önizleme) kullanarak izlemeye başlamak için aşağıda bahsedilen adımları izleyin

## <a name="step-1-install-monitoring-agents"></a>1\. Adım: izleme aracılarını Install

Bağlantı Izleyicisi, bağlantı denetimlerini çalıştırmak için hafif yürütülebilir dosyaları kullanır.  Hem Azure hem de şirket içi ortamlarından gelen bağlantı denetimlerini destekliyoruz. Kullanılacak belirli yürütülebilir dosya, sanal makinenizin Azure 'da mı yoksa şirket içinde mi barındırıldığından bağlıdır.

### <a name="agents-for-azure-virtual-machines"></a>Azure sanal makineleri için aracılar

Bağlantı Izleyicisinin, Azure VM 'lerinizi izleme kaynağı olarak tanıması için, ağ Izleyicisi Aracısı sanal makine uzantısını (ağ Izleyicisi uzantısı olarak da bilinir) bunlar üzerinde yüklemeniz gerekir. Ağ Izleyicisi Aracısı uzantısı, Azure sanal makinelerinde uçtan uca izlemeyi ve diğer gelişmiş işlevleri tetiklemenin bir gereksinimidir. [BIR VM oluşturabilir ve ağ izleyicisi uzantısını bu uzantıya yükleyebilirsiniz](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm)[](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm).  Ayrıca, [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows) için ağ izleyicisi uzantısını ayrı olarak yükleyebilir, yapılandırabilir ve sorun giderebilirsiniz.

NSG veya güvenlik duvarı kuralları kaynak ve hedef arasındaki iletişimi engelliyorsa, bağlantı Izleyicisi sorunu algılar ve topolojide bir tanılama iletisi olarak gösterir. Bağlantı izlemeyi etkinleştirmek için NSG ve Güvenlik Duvarı kurallarının, kaynak ve hedef arasında TCP veya ıCMP üzerinden paketlere izin verildiğinden emin olun.

### <a name="agents-for-on-premise-machines"></a>Şirket içi makineler için aracılar

Bağlantı Izleyicisinin şirket içi makinelerinizi izlemek üzere kaynak olarak tanıması için, makinelere Log Analytics aracısını yüklemeniz ve ağ performansı Izleme çözümünü etkinleştirmeniz gerekir. Bu aracılar Log Analytics çalışma alanlarına bağlanır ve izlemeye başlayabilmeleri için çalışma alanı KIMLIĞI ve birincil anahtar ayarlanmalıdır.

Windows makineleri için Log Analytics Aracısı 'nı yüklemek için [Bu bağlantıda](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows) bahsedilen yönergeleri izleyin

Yolda güvenlik duvarları veya sanal ağ gereçleri (NVA) varsa, hedefin erişilebilir olduğundan emin olun.

## <a name="step-2-enable-network-watcher-on-your-subscription"></a>2\. Adım: aboneliğinizde Ağ İzleyicisini etkinleştirme

VNET içeren tüm abonelikler ağ Izleyicisi ile etkinleştirilir. Aboneliğinizde bir sanal ağ oluşturduğunuzda, bu sanal ağın bölgesinde ve aboneliğinde ağ Izleyicisi otomatik olarak etkinleştirilir. Ağ Izleyicisi 'ni otomatik olarak etkinleştirmek için kaynaklarınız veya ilişkili ücretler üzerinde hiçbir etkisi yoktur. Ağ Izleyicisi 'nin aboneliğinizde açık olarak devre dışı olmadığından emin olun. Daha fazla bilgi için bkz. [ağ Izleyicisini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="step-3-create-connection-monitor"></a>3\. Adım: bağlantı Izleyicisi oluşturma 

_Bağlantı İzleyicisi_ , düzenli aralıklarla iletişimi izler ve kaynak aracıları ile hedef uç noktalar arasındaki ulaşımlar, gecikme süresi ve ağ topolojisi değişiklikleri hakkında bilgilendirir. Kaynaklar Azure sanal makineleri veya bir izleme aracısının yüklü olduğu şirket içi makineler olabilir. Hedef uç noktalar Office 365 URL 'Leri, Dynamics 365 URL 'Leri, özel URL 'ler, Azure VM kaynak kimlikleri, IPv4, IPv6, FQDN veya herhangi bir etki alanı adı olabilir.

### <a name="accessing-connection-monitor-preview"></a>Bağlantı Izleyicisi 'ne erişme (Önizleme)

1. Azure portal giriş sayfasından ağ Izleyicisi ' ni ziyaret edin.
2. Ağ Izleyicisi 'nin sol bölmesindeki Izleme bölümünde "bağlantı Izleyicisi (Önizleme)" sekmesine tıklayın.
3. Bağlantı Izleyicisi (Önizleme) deneyimi kullanılarak oluşturulan tüm bağlantı Izleyicilerini görebilirsiniz. Bağlantı Izleyicisi sekmesinin klasik deneyimi kullanılarak oluşturulan tüm bağlantı Izleyicileri, bağlantı Izleyicisi sekmesinde görünür.

    ![Bağlantı Izleyicisi oluşturma](./media/connection-monitor-2-preview/cm-resource-view.png)


### <a name="creating-a-connection-monitor"></a>Bağlantı Izleyicisi oluşturma

Bağlantı Izleyicisi (Önizleme) kullanılarak oluşturulan bağlantı Izleyicileri, hem şirket içi hem de Azure sanal makinelerini kaynak olarak ekleme ve Azure 'a ya da başka bir URL/IP 'ye yayılabilen uç noktalara bağlantı olanağı sağlar.

Bir bağlantı Izleyicisinin varlıkları aşağıda verilmiştir:

* Bağlantı Izleyicisi kaynağı – bölgeye özgü Azure kaynağı. Aşağıda bahsedilen tüm varlıklar bir bağlantı Izleyicisi kaynağının özellikleridir.
* Uç noktalar – bağlantı denetimlerine katılan tüm kaynaklar ve hedefler bitiş noktaları olarak çağırılır. Uç nokta örnekleri: Azure VM 'Leri, şirket Içi aracılar, URL 'Ler, IP 'ler
* Test yapılandırması – her test yapılandırması protokole özeldir. Seçilen protokole bağlı olarak, bağlantı noktası, eşikler, sınama sıklığı ve diğer parametreleri tanımlayabilirsiniz
* Test grubu – her test grubu kaynak uç noktaları, hedef uç noktaları ve test yapılandırması içerir. Her bağlantı Izleyicisinde birden fazla test grubu bulunabilir
* Test – bir kaynak uç noktası, hedef uç noktası ve test yapılandırmasının birleşimi bir test yapar. Test, izleme verilerinin (başarısız olan% ve RTT) kullanılabildiği en düşük düzeydir

 ![Bağlantı Izleyicisi oluşturma](./media/connection-monitor-2-preview/cm-tg-2.png)

#### <a name="from-portal"></a>Portaldan

Bir bağlantı Izleyicisi oluşturmak için aşağıdaki adımları izleyin:

1. Bağlantı Izleyicisi (Önizleme) panosunda sol üst köşedeki "Oluştur" düğmesine tıklayın.
2. Temel sekmede bağlantı monitörünüz için bilgi girin
   1. Bağlantı Izleyicisi adı – bağlantı monitörünüzün adı. Azure kaynakları için standart adlandırma kuralları burada geçerlidir.
   2. Abonelik: bağlantı monitörünüz için bir abonelik seçin.
   3. Region: bağlantı Izleyicisi kaynağınız için bir bölge seçin. Yalnızca bu bölgede oluşturulan kaynak VM 'Leri seçebilirsiniz.
   4. Çalışma alanı yapılandırması-izleme verilerinizi varsayılan onay kutusuna tıklayarak depolamak için bağlantı Izleyicisi tarafından oluşturulan varsayılan çalışma alanını kullanabilirsiniz. Özel bir çalışma alanı seçmek için bu kutunun işaretini kaldırın. Çalışma alanını seçmek için abonelik ve bölgeyi seçin; Bu işlem izleme verilerinizi tutar.
   5. Test grupları eklemek için "Ileri: test grupları" seçeneğine tıklayın

      ![Bağlantı Izleyicisi oluşturma](./media/connection-monitor-2-preview/create-cm-basics.png)

3. Test grupları sekmesinde, bir test grubu eklemek için "+ test grubu" düğmesine tıklayın. Test grupları eklemek için _bağlantı Izleyicisinde test grupları oluşturma '_ yı kullanın. Bağlantı monitörünüzü gözden geçirmek için "gözden geçir + oluştur" seçeneğine tıklayın.

   ![Bağlantı Izleyicisi oluşturma](./media/connection-monitor-2-preview/create-tg.png)

4. "Gözden geçir + oluştur" sekmesinde, bağlantı Izleyicisini oluşturmadan önce temel bilgiler ve test grupları ' nı gözden geçirin. "Gözden geçir + oluştur" görünümünden bağlantı Izleyicisini düzenlemek için:
   1. Temel ayrıntıları düzenlemek için, görüntü 2 ' de 1. kutu tarafından belirtilen kurşun kalem simgesini kullanın
   2. Bireysel test gruplarını düzenlemek için, düzenlemek istediğiniz test grubuna tıklayarak, düzenleme modunda test grubunu açın.
   3. Geçerli maliyet/ay önizleme sırasında maliyeti belirtti. Şu anda bağlantı Izleyicisi 'ni kullanmaya yönelik ücret alınmaz, bu nedenle bu sütunda sıfır görünür. Gerçek maliyet/ay, genel kullanıma alındıktan sonra ücretlendirilebilecek fiyatı gösterdi. Bu şekilde, önizleme sırasında bile Günlük Analizi alma ücretleri uygulanır.

5. "Gözden geçir + oluştur" sekmesinde, bağlantı Izleyicisini oluşturmak için "Oluştur" düğmesine tıklayın.

   ![Bağlantı Izleyicisi oluşturma](./media/connection-monitor-2-preview/review-create-cm.png)

6.  Bağlantı Izleyicisi (Önizleme) arka planda bağlantı Izleyicisi kaynağını oluşturur.

#### <a name="from-armclient"></a>Armclient 'dan

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

address: '\&lt;FQDN of your on-premise agent'

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

Dağıtım komutu:
```
armclient PUT $ARM/$SUB/$NW/connectionMonitors/$connectionMonitorName/?api-version=2019-07-01 $body -verbose
```

### <a name="creating-test-groups-in-connection-monitor"></a>Bağlantı Izleyicisinde test grupları oluşturma

Bağlantı Izleyicisinde bulunan her bir test grubu, başarısız olan denetimlerin ağ parametrelerini test eden kaynakları ve hedefi ve test yapılandırması üzerinden RTT 'yi içerir.

#### <a name="from-portal"></a>Portaldan

Bir bağlantı Izleyicisinde bir test grubu oluşturmak için, yukarıda bahsedilen alanlarla ilgili değeri belirtirsiniz:

1. Test grubunu devre dışı bırak – bu alanın Işaretlenmesi, test grubunda belirtilen tüm kaynaklar ve hedefler için izlemeyi devre dışı bırakır. Bu seçeneği varsayılan olarak işaretlenmemiş olarak görürsünüz.
2. Ad – test grubunuzun adı
3. Kaynaklar: aracılar bunlara yüklenmişse hem Azure VM 'lerini hem de şirket Içi makineleri kaynak olarak belirtebilirsiniz. Kaynağınıza özgü aracıyı yüklemek için adım 1 ' e bakın.
   1. Azure aracılarını seçmek için "Azure Agents" sekmesine tıklayın. Yalnızca, bağlantı Izleyicisini oluşturma sırasında belirttiğiniz bölgeye bağlı olan VM 'Leri görürsünüz. VM 'Ler varsayılan olarak ait oldukları abonelikte gruplandırılır ve gruplar daraltılmıştır. Abonelikler düzeyinden hiyerarşideki diğer düzeylere ayrıntıya gidebilirsiniz:

      ```Subscription -\&gt; resource groups -\&gt; VNETs -\&gt; Subnets -\&gt; VMs with agents Y```

      Ayrıca, ağacı başka bir düzeyden başlatmak için "gruplama ölçütü" alanının değerini de değiştirebilirsiniz. Örneğin: Group By – VNET, sanal ağları hiyerarşi içinde aracılarla gösterir-\&gt; Alt ağlar-\&gt; Aracılarla VM 'Ler.

      ![Kaynak ekleme](./media/connection-monitor-2-preview/add-azure-sources.png)

   2. Şirket içi aracıları seçmek için "Azure dışı aracılar" sekmesine tıklayın. Varsayılan olarak, aracıları bir bölgedeki çalışma alanları halinde gruplanmış olarak görürsünüz. Yalnızca Ağ Performansı İzleyicisi çözümü yapılandırılan çalışma alanları listelenecektir. Ağ Performansı İzleyicisi çözümünü [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview)' nden çalışma alanınıza ekleyin. Ayrıca, [Çözüm Galerisi Azure izleyici çözümlerini ekleme](https://docs.microsoft.com/azure/azure-monitor/insights/solutions) bölümünde açıklanan süreci de kullanabilirsiniz. Varsayılan olarak, bağlantı Izleyicisi görünümü oluşturma görünümündeki temel bilgiler sekmesinde seçili bölgeyi görürsünüz. Bölgeyi değiştirebilir ve yeni seçilen bölgeden çalışma alanlarından aracılar seçebilirsiniz. Ayrıca "gruplandırma ölçütü" alanının değerini alt ağlardaki gruplar olarak değiştirebilirsiniz.

      ![Azure dışı kaynaklar](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   3. Azure ve Azure dışı aracıların seçili olduğunu gözden geçirmek için "gözden geçir" e tıklayın.

      ![Kaynakları gözden geçir](./media/connection-monitor-2-preview/review-sources.png)

   4. Kaynakları seçmeyi tamamladıktan sonra "bitti" ye tıklayın.

4. Hedefler: Azure VM 'lerine veya herhangi bir uç noktaya (genel IP, URL, FQDN) yönelik bağlantıyı, hedefler olarak belirterek izleyebilirsiniz. Tek bir test grubunda Azure VM 'leri, O365 URL 'Leri, D365 URL 'Leri veya özel uç noktaları ekleyebilirsiniz.

   1. Azure VM 'lerini hedef olarak seçmek için "Azure VM 'Ler" sekmesine tıklayın. Varsayılan olarak, Azure VM 'Leri bağlantı Izleyicisi görünümü oluşturma görünümündeki temel bilgiler sekmesinde seçilen bölgede bulunan aynı bölgedeki abonelik hiyerarşisinde gruplanmış olarak görürsünüz. Bölgeyi değiştirebilir ve yeni seçilen bölgeden Azure VM 'Leri seçebilirsiniz. Abonelikler düzeyinden Azure aracıları gibi hiyerarşideki diğer düzeylere gidebilirsiniz.

      ![Hedef Ekle](./media/connection-monitor-2-preview/add-azure-dests1.png)<br>

      ![Hedef Ekle 2](./media/connection-monitor-2-preview/add-azure-dests2.png)

   2. Azure VM 'lerini hedef olarak seçmek için "uç noktalar" sekmesine tıklayın. Uç nokta listesi O365 ve D365 test URL 'Leriyle doldurulacak, ada göre gruplandırılır.  Aynı bağlantı Izleyicisinde diğer test gruplarında oluşturulan bir uç noktasını da seçebilirsiniz. Yeni bir uç nokta eklemek için, ekranın sağ üst köşesinden "+ uç nokta" düğmesine tıklayın ve uç nokta URL 'SI/IP/FQDN ve ad sağlayın

      ![Uç nokta Ekle](./media/connection-monitor-2-preview/add-endpoints.png)

   3. Azure ve Azure dışı aracıların seçili olduğunu gözden geçirmek için "gözden geçir" e tıklayın.
   4. Kaynakları seçmeyi tamamladıktan sonra "bitti" ye tıklayın.

5. Test yapılandırması – belirli bir test grubundaki herhangi bir sayıda test yapılandırmasını ilişkilendirebilirsiniz. Portal, test grubu başına bir test yapılandırmasıyla kısıtlar, ancak daha fazla eklemek için Armclient kullanın.
   1. Ad – test yapılandırması için ad
   2. Protokol: TCP, ıCMP veya HTTP arasında seçim yapabilirsiniz. HTTP 'yi HTTPS olarak değiştirmek için, bağlantı noktası olarak HTTP as Protocol ve 443 ' i seçin
   3. Ağ test yapılandırması oluşturma – bu onay kutusunu yalnızca protokol alanında HTTP ' yi seçerseniz görürsünüz. 3\. adım ve TCP/ıCMP üzerinden 4. adımda belirtilen kaynakları ve hedefleri kullanarak başka bir test yapılandırması oluşturmak için bu alanı etkinleştirin. Yeni oluşturulan test yapılandırması, 5 ' te belirtilen "\&lt; name adı" olarak adlandırılmıştır. a\&gt;\_networkTestConfig "
   4. İzleme Oute 'yi devre dışı bırak – bu alan, TCP veya ıCMP as Protocol ile test grupları için geçerli olacaktır.  Kaynakları, topoloji ve atlama atlama gidiş dönüş süresini bulmaktan durdurmak için bu alanı işaretleyin.
   5. Hedef bağlantı noktası: bu alanı, tercih ettiğiniz bir hedef bağlantı noktasına koymak için özelleştirebilirsiniz.
   6. Sınama sıklığı – bu alan, kaynakların yukarıda belirtilen protokol ve bağlantı noktasındaki hedeflere ne sıklıkta ping uygulayacağına karar verir. 30 saniye, 1 dakika, 5 dakika, 15 dakika ve 30 dakika arasında seçim yapabilirsiniz. Kaynaklar, belirlediğiniz değere göre hedeflere bağlantıyı test edecektir.  Örneğin, 30 saniye seçerseniz, kaynaklar en az 30 saniyelik dönemde hedefe bağlantıyı kontrol eder.
   7. Sistem durumu eşikleri: aşağıda belirtilen ağ parametrelerinde eşikler ayarlayabilirsiniz
      1. Denetimler, yukarıda belirtilen ölçütlere göre hedefe bağlantıyı denetlediğinde denetim yüzdesi-denetim yüzdesi başarısız oldu. TCP/ıCMP protokolü için,% içinde başarısız olan denetimler% paket kaybı ile eşit olabilir. HTTP protokolü için bu alan, yanıt almayan http isteklerinin sayısını temsil eder.
      2. Milisaniye cinsinden RTT – kaynakların, yukarıda belirtilen test yapılandırması üzerinden hedefe bağlanması durumunda milisaniye cinsinden gidiş dönüş süresi.

      ![TG Ekle](./media/connection-monitor-2-preview/add-test-config.png)

Test yapılandırması belirtilen test grubuna eklenen tüm kaynaklar ve hedefler, bireysel testlere bölünmez. Örnek:

* Test grubu: TG1
* Kaynaklar: 3 (A, B, C)
* Hedefler: 2 (D, E)
* Test yapılandırması: 2 (yapılandırma 1, yapılandırma 2)
* Oluşturulan testler: toplam = 12

| **Sınama numarası** | **Kaynak** | **Hedefine** | **Test yapılandırması adı** |
| --- | --- | --- | --- |
| 1 | A | D | Yapılandırma 1 |
| 2 | A | D | Yapılandırma 2 |
| 3 | A | E | Yapılandırma 1 |
| 4 | A | E | Yapılandırma 2 |
| 5 | B | D | Yapılandırma 1 |
| 6 | B | D | Yapılandırma 2 |
| 7 | B | E | Yapılandırma 1 |
| 8 | B | E | Yapılandırma 2 |
| 9 | C | D | Yapılandırma 1 |
| 10 | C | D | Yapılandırma 2 |
| 11 | C | E | Yapılandırma 1 |
| 12 | C | E | Yapılandırma 2 |

### <a name="scale-limits"></a>Ölçek sınırları

* Her bölge için abonelik başına en fazla bağlantı Izleyicisi: 100
* Bağlantı Izleyicisi başına en fazla test grubu sayısı-20
* Bağlantı Izleyicisi başına en fazla # kaynak + hedef: 100
* Bağlantı Izleyicisi başına en fazla test yapılandırması sayısı – Armclient aracılığıyla 20. 2 portal üzerinden.

## <a name="step-4--data-analysis-and-alerts"></a>4\. Adım: veri analizi ve uyarılar

Bir bağlantı Izleyicisi oluşturulduktan sonra, kaynaklar belirtilen test yapılandırmasına göre hedeflere bağlantıyı denetler.

### <a name="checks-in-a-test"></a>Bir testte denetimler

Bağlantı Izleyicisi (Önizleme), test yapılandırmasındaki bir kullanıcı tarafından seçilen protokole bağlı olarak, kaynak hedef çifti için seçilen test sıklığı üzerinden bir dizi denetim çalıştırır.

HTTP seçilirse hizmet, başarısız olan denetimleri belirlemede yanıt kodu döndüren HTTP yanıtlarının sayısını hesaplar.  RTT 'yi hesaplamak için, bir HTTP çağrısının yanıtını almak için geçen süreyi ölçyoruz.

TCP veya ıCMP seçilirse, hizmet% başarısız olan denetimleri belirlemede% paketini hesaplar. RTT 'yi hesaplamak için gönderilen paketlerin ACK 'i almak için geçen süreyi ölçyoruz. Ağ testleriniz için izleme yolu verilerini etkinleştirdiyseniz, şirket içi ağınız için atlama kaybını ve gecikme süresini görebilirsiniz.

### <a name="states-of-a-test"></a>Bir testin durumları

Testteki denetimler tarafından döndürülen verilere bağlı olarak, her bir test aşağıdaki durumlara sahip olabilir:

* Pass = denetim için gerçek değerler başarısız oldu% ve RTT belirtilen eşiklerin içinde
* Başarısız = denetim için gerçek değerler başarısız oldu% veya RTT belirtilen eşiklerden fazla. Hiçbir eşik belirtilmemişse, Denetim başarısız olduğunda bir test başarısız olarak işaretlenir% = %100
* Uyarı:% başarısız denetimler için ölçüt belirtilmemiş. Böyle bir durumda, bağlantı Izleyicisi (Önizleme), bir otomatik ayarlama ölçütünü eşik olarak kullanır ve bu eşiğe ihlal edildiğinde testin durumu "uyarı" olarak ayarlanır

### <a name="data-collection-analysis-and-alerts"></a>Veri toplama, analiz ve uyarılar

Bağlantı Izleyicisi (Önizleme) tarafından toplanan tüm veriler, bağlantı Izleyicisi oluşturma sırasında yapılandırılan Log Analytics çalışma alanında depolanır. Azure Izleyici ölçümlerinde izleme verileri de mevcuttur. İzleme verilerinizi istediğiniz uzunlukta tutmak için Log Analytics kullanabilirsiniz, ancak Azure Izleyici ölçümleri varsayılan olarak 30 gün boyunca depolar **.** Daha sonra [veri üzerinde ölçüm tabanlı uyarılar ayarlayabilirsiniz](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/).

#### <a name="monitoring-dashboards-in-connection-monitor-solution"></a>Bağlantı Izleyicisi çözümünde panoları izleme

Belirli bir abonelik, bölge, zaman damgası, kaynak ve hedef türü seçimi için erişiminizin olduğu bağlantı Izleyicinizle ilgili bir liste görürsünüz.

Ağ Izleyicisi hizmetinden bağlantı Izleyicisi 'ne (Önizleme) gittiğinizde, şu şekilde **görüntülemeyi**seçebilirsiniz:

* Bağlantı Izleyicisi (varsayılan) – seçili abonelikler, bölgeler, zaman damgası, kaynak ve hedef türleri için oluşturulan tüm bağlantı Izleyicilerinin listesi
* Test grupları – seçili abonelikler, bölgeler, zaman damgası, kaynak ve hedef türleri için oluşturulan tüm test gruplarının listesi. Bu test grupları bağlantı Izleyicisinde filtrelenmez
* Testler – seçili abonelikler, bölgeler, zaman damgası, kaynak ve hedef türleri için çalışan tüm testlerin listesi. Bu testler bağlantı Izleyicisi veya test grupları üzerinde filtrelenmez.

Her bir bağlantı Izleyicisini, panoda çalışan çeşitli ayrı testlerde test gruplarına ve her test grubuna genişletebilirsiniz. Aşağıdaki görüntüde [1] olarak işaretlendi.

Bu listeye göre filtre uygulayabilirsiniz:

* En üst düzey filtreler-abonelikler, bölgeler, zaman damgası kaynağı ve hedef türleri. Aşağıdaki görüntüde [2] olarak işaretlendi.
* Durum tabanlı filtreler-bağlantı Izleyici/test grubu/test durumunda Ikinci düzey filtre. Aşağıdaki görüntüde [3] olarak işaretlendi.
* Arama alanı: genel arama yapmak için "tümü" seçeneğini belirleyin. Belirli bir varlıkta arama yapmak için açılan listeyi kullanarak arama sonuçlarını daraltın. Aşağıdaki görüntüde [4] olarak işaretlendi.

![Testleri filtrele](./media/connection-monitor-2-preview/cm-view.png)

Örnek:

1. Kaynak IP = 10.192.64.56 tüm bağlantı Izleyici (Önizleme) genelinde tüm testlere bakmak için:
   1. Görünümü "test" olarak değiştirin
   2. Arama dosyalanmış = 10.192.64.56
   3. Değer ' in yanındaki açılan menüyü kullanarak "kaynaklar" ı seçin
2. Kaynak IP = 10.192.64.56 'te yalnızca tüm bağlantı Izleyicisinde (Önizleme) başarısız testleri filtrelemek için
   1. Görünümü "test" olarak değiştirin
   2. Durum tabanlı filtrelerden "başarısız" seçeneğini belirleyin.
   3. Arama alanı = 10.192.64.56
   4. Değer ' in yanındaki açılan menüyü kullanarak "kaynaklar" ı seçin
3. Yalnızca hedefin outlook.office365.com olduğu tüm bağlantı Izleyicisinde (Önizleme) başarısız testleri filtrelemek için
   1. Görünümü "test" olarak değiştirin
   2. Durum tabanlı filtrelerden "başarısız" seçeneğini belirleyin.
   3. Arama alanı = outlook.office365.com
   4. Değer ' in yanındaki açılan menüsünü kullanarak "hedefler" i seçin

   ![Başarısız testler](./media/connection-monitor-2-preview/tests-view.png)

% Başarısız denetimlerinin ve RTT 'nin eğilimlerini görüntülemek için:

1. Bağlantı İzleyicisi
   1. Ayrıntılı incelemek istediğiniz bağlantı Izleyicisine tıklayın
   2. Varsayılan olarak, izleme verilerini "test grupları" ile görüntüleyebilirsiniz.

      ![Ölçümleri görüntüleme ölçütü](./media/connection-monitor-2-preview/cm-drill-landing.png)

   3. Ayrıntılı incelemek istediğiniz test grubunu seçin

      ![TG 'ye göre ölçümler](./media/connection-monitor-2-preview/cm-drill-select-tg.png)

   4. Önceki adımda seçtiğiniz test grubu için başarısız olan% veya RTT milisaniye olan denetimler üzerinde ilk 5 başarısız test görürsünüz. Her test için,% başarısız denetimler ve RTT için eğilim çizgilerini görürsünüz
   5. Yukarıdaki listeden bir test seçin veya ayrıntılı incelemek için başka bir test seçin.
   6. Seçilen zaman aralığı için,% başarısız denetimler için eşik ve gerçek değerler görürsünüz. RTT için, Threshold, Ort, min ve Max değerlerini görürsünüz.

      ![RTT](./media/connection-monitor-2-preview/cm-drill-charts.png)

  7. Daha fazla veri görüntülemek için zaman aralığını değiştirin
  8. B adımındaki görünümü değiştirebilir ve kaynakları, hedefleri veya test yapılandırmalarına göre görüntülemeyi seçebilirsiniz. Sonra başarısız testleri temel alarak bir kaynak seçin ve başarısız olan 5 testi inceleyin.  Örneğin: Seçili bağlantı Izleyicisinde Bu birleşim arasında çalışan tüm testleri araştırmak için görünüm ölçütü: kaynaklar ve hedefler ' i seçin.

      ![RTT2](./media/connection-monitor-2-preview/cm-drill-select-source.png)

2. Test grubu
   1. Ayrıntılı incelemek istediğiniz test grubuna tıklayın
   2. Varsayılan olarak, izleme verilerini "kaynak + hedef + test yapılandırması (test)" ile görüntüleyebilirsiniz.

      ![RTT3](./media/connection-monitor-2-preview/tg-drill.png)

   3. Ayrıntılı incelemek istediğiniz testi seçin
   4. Seçilen zaman aralığı için,% başarısız denetimler için eşik ve gerçek değerler görürsünüz. RTT için, Threshold, Ort, min ve Max değerlerini görürsünüz. Ayrıca, seçtiğiniz teste özel tetiklenen uyarıları da görürsünüz.
   5. Daha fazla veri görüntülemek için zaman aralığını değiştirin
   6. B adımındaki görünümü değiştirebilir ve kaynakları, hedefleri veya test yapılandırmalarına göre görüntülemeyi seçebilirsiniz. Ardından, ilk 5 başarısız testi araştırmak için bir varlık seçin.  Örneğin: Seçili bağlantı Izleyicisinde Bu birleşim arasında çalışan tüm testleri araştırmak için görünüm ölçütü: kaynaklar ve hedefler ' i seçin.

3. Test etme
   1. Ayrıntılı incelemek istediğiniz kaynak + hedef + test yapılandırmasına tıklayın
   2. Seçilen zaman aralığı için,% başarısız denetimler için eşik ve gerçek değerler görürsünüz. RTT için, Threshold, Ort, min ve Max değerlerini görürsünüz. Ayrıca, seçtiğiniz teste özel tetiklenen uyarıları da görürsünüz.

      ![Test1](./media/connection-monitor-2-preview/test-drill.png)

   3. Ağ topolojisini istediğiniz zaman görmek için "topoloji" seçeneğine de tıklayabilirsiniz.

      ![Yazıp](./media/connection-monitor-2-preview/test-topo.png)

   4. Bağlantı Izleyicisi tarafından tanımlanan sorunları görmek için, herhangi bir Azure ağı bağlantısı atlamada tıklayabilirsiniz. Bu yetenek, şirket içi ağlarda Şu anda kullanılamaz.

       ![Test3](./media/connection-monitor-2-preview/test-topo-hop.png)

#### <a name="log-queries-in-azure-monitor-log-analytics"></a>Azure Izleyici 'de günlük sorguları Log Analytics

İzleme verilerinizin özel görünümlerini oluşturmak için Log Analytics kullanın. Kullanıcı arabiriminde görünen tüm veriler Log Analytics doldurulur. Depodaki verilerin etkileşimli analizini yapabilir ve aracı sistem durumu ve diğer Log Analytics tabanlı uygulamalar gibi farklı kaynaklardaki verileri ilişkilendirebilir. Ayrıca, verileri Excel 'e, Power BI veya paylaşılabilir bir bağlantıya de aktarabilirsiniz.

#### <a name="metrics-in-azure-monitor"></a>Azure İzleyicisi'nde ölçümler

Bağlantı Izleyicisi (Önizleme) deneyiminden önce oluşturulan bağlantı Izleyicisi için 4 ölçüm kullanılabilir. Bağlantı Izleyicisi (Önizleme) deneyimiyle oluşturulan bağlantı Izleyicilerinde, veriler yalnızca "(Önizleme)" ile etiketlenmiş ölçümler için kullanılabilir.

Kaynak türü-Microsoft. Network/networkWatchers/Connectionmonitörleri

| Ölçüm | Ölçüm görünen adı | Birim | Toplama Türü | Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | % Yoklama başarısız oldu | Yüzde | Ortalama | Bağlantı İzleme Araştırmaları yüzdesi başarısız oldu | Boyut yok |
| AverageRoundtripMs | Ort. gidiş dönüş süresi (MS) | Mayacak | Ortalama | Kaynak ve hedef arasında gönderilen bağlantı izleme araştırmaları için Ortalama ağ gidiş dönüş süresi (MS) |             Boyut yok |
| ChecksFailedPercent (Önizleme) | % Denetim başarısız oldu (Önizleme) | Yüzde | Ortalama | test için başarısız olan denetim yüzdesi | * Connectionmonitorresourceıd <br> * SourceAddress <br> * SourceName <br> * Sourceresourceıd <br> * SourceType <br> * Protokolü <br> * Hedef adres <br> * DestinationName <br> * Hedef RESOURCEID <br> * DestinationType <br> * Hedef bağlantı noktası <br> * TestGroupName <br> * TestConfigurationName <br> * Bölge |
| Roundüçlü MS (Önizleme) | Gidiş dönüş süresi (MS) (Önizleme) | Milisaniye | Ortalama | Kaynak ve hedef arasında gönderilen denetimler için gidiş dönüş süresi (MS). Bu değer ortalama değil | * Connectionmonitorresourceıd <br> * SourceAddress <br> * SourceName <br> * Sourceresourceıd <br> * SourceType <br> * Protokolü <br> * Hedef adres <br> * DestinationName <br> * Hedef RESOURCEID <br> * DestinationType <br> * Hedef bağlantı noktası <br> * TestGroupName <br> * TestConfigurationName <br> * Bölge |

 ![Ölçümleri izleme](./media/connection-monitor-2-preview/monitor-metrics.png)

#### <a name="metric-alerts-in-azure-monitor"></a>Azure Izleyici 'de ölçüm uyarıları

Uyarı oluşturmak için:

1. Bağlantı İzleyicisi (Önizleme) kullanılarak oluşturulan bağlantı Izleyici kaynağını seçme
2. Önceki adımda seçilen kaynak için "ölçüm" ın sinyal türü olarak görüntülendiğinden emin olun
3. Koşul Ekle ' de, sinyal adı ' nı ChecksFailedPercent (Önizleme) veya Roundüçlü MS (Önizleme) ve sinyal türü olarak ölçüm olarak seçin. Örn: ChecksFailedPercent (Önizleme) seçeneğini belirleyin
4. Ölçüm başına uygulanan tüm boyutlar listelenecektir.  Boyut adı ve boyut değerini seçin. Ör: kaynak adresini seçin ve 1. adımda seçilen bağlantı Izleyicisi kaynağı ile ilgili herhangi bir kaynağın IP adresini sağlayın
5. Uyarı mantığındaki şunları seçin:
   1. Koşul türü – statik
   2. Koşul ve eşik
   3. Toplama ayrıntı düzeyi ve değerlendirme sıklığı-bağlantı Izleyicisi (Önizleme) 1 dakikada bir verileri güncelleştirir.
6.  Eylemler ' de eylem grubunuzu seçin
7. Uyarı ayrıntıları sağla
8. Uyarı kuralı oluştur

   ![Uyarılar](./media/connection-monitor-2-preview/mdm-alerts.jpg)

## <a name="step-5-diagnose-issues-in-your-network"></a>5\. Adım: ağınızdaki sorunları tanılayın

Bağlantı Izleyicisi, bağlantı Izleyicisi kaynağına ve ağınızda ilgili sorunları tanılamanıza yardımcı olur. Karma ağınızdaki sorunlar, 1. adımda yüklediğiniz Log Analytics aracıları tarafından algılanır ve Azure 'daki sorunlar ağ Izleyicisi uzantısı tarafından algılanır.  Karma ağdaki sorunlar tanılama sayfasında görünür ve Azure ağındaki sorunlar ağ topolojisinde görünür olacaktır.

Kaynak olarak şirket içi VM 'Leri olan ağlarda şunları tespit ediyoruz:

* İstek zaman aşımına uğradı
* Uç nokta DNS tarafından çözümlenmedi-geçici veya kalıcı. URL geçersiz.
* Hiçbir ana bilgisayar bulunamadı.
* Kaynak, hedefle bağlantı kuramıyor. Hedefe ıCMP üzerinden ulaşılamıyor.
* Sertifikayla ilgili sorun-aracının kimliğini doğrulamak için gereken Istemci sertifikası, sertifika konum değiştirme listesi erişilebilir değil, uç noktanın ana bilgisayar adı, sertifikanın konu veya konu diğer adı ile eşleşmiyor, kaynağın yerel bilgisayar güvenilen sertifika yetkilileri deposunda kök sertifika yok, SSL sertifikası kullanım dışı/geçersiz/iptal edildi

Azure VM 'Leri olan ağlarda kaynaklar bulunur, şunları tespit ediyoruz:

* Aracı sorunları – aracı durdu, başarısız DNS çözümlemesi, hedef bağlantı noktasında dinleme yapan uygulama/dinleyici yok, yuva açılamadı
* VM durumu sorunları – başlatma, durdurma, durdurma, serbest ayırma, serbest bırakma, yeniden başlatma, ayrılmamış
* ARP tablosu girdisi eksik
* Yerel güvenlik duvarı sorunları nedeniyle trafik engellendi, NSG kuralları
* VNET ağ geçidi – eksik yollar, iki ağ geçidi arasındaki tünel bağlantısı kesildi veya eksik ya da ikinci ağ geçidi tünelle bulunmadı, hiçbir eşleme bilgisi bulunamadı
* MS Edge 'de yol eksik.
* Sistem yolları veya UDR nedeniyle trafik durduruldu
* Ağ Geçidi bağlantısında BGP etkinleştirilmemiş
* Load Balancer DIP araştırması
