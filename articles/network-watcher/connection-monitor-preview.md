---
title: Bağlantı Izleyicisi (Önizleme) | Microsoft Docs
description: Dağıtılmış bir ortamdaki ağ iletişimini izlemek için bağlantı Izleyicisi 'ni (Önizleme) nasıl kullanacağınızı öğrenin.
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
ms.openlocfilehash: d926a9f686f0f4c39203b8a217a7c608cfad926e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80548113"
---
# <a name="network-connectivity-monitoring-with-connection-monitor-preview"></a>Bağlantı Izleyicisi ile ağ bağlantısı Izleme (Önizleme)

Bağlantı Izleyicisi (Önizleme), Azure ağ Izleyicisi 'nde birleştirilmiş uçtan uca bağlantı izleme sağlar. Bağlantı Izleyicisi (Önizleme) özelliği karma ve Azure bulut dağıtımlarını destekler. Ağ Izleyicisi, Azure dağıtımlarınız için bağlantı ile ilgili ölçümleri izlemek, tanılamak ve görüntülemek için araçlar sağlar.

Bağlantı Izleyicisi (Önizleme) için bazı kullanım durumları aşağıda verilmiştir:

- Ön uç Web sunucusu sanal makinesi, çok katmanlı bir uygulamadaki bir veritabanı sunucusu VM 'si ile iletişim kurar. İki VM arasındaki ağ bağlantısını kontrol etmek istiyorsunuz.
- Doğu ABD bölgesindeki VM 'Lerin Orta ABD bölgesindeki sanal makinelere ping işlemi olmasını ve bölgeler arası ağ gecikmelerinin karşılaştırmasını istiyorsunuz.
- Seattle, Washington ve Ashyakma, Virginia 'de birden çok şirket içi Office siteniz var. Office siteleriniz Office 365 URL 'Lerine bağlanır. Office 365 URL 'nizin kullanıcıları için Seattle ve Ashburn arasındaki gecikme sürelerini karşılaştırın.
- Karma uygulamanızın bir Azure depolama uç noktasına bağlanması gerekiyor. Şirket içi siteniz ve Azure uygulamanız aynı Azure depolama uç noktasına bağlanır. Şirket içi sitenin gecikme sürelerini Azure uygulamasının gecikme süreleriyle karşılaştırmak istiyorsunuz.
- Şirket içi kurulumlarınız ile bulut uygulamanızı barındıran Azure VM 'Ler arasındaki bağlantıyı denetlemek istiyorsunuz.

Bağlantı Izleyicisi, önizleme aşamasında iki özelliği en iyi şekilde birleştirir: ağ Izleyicisi [Bağlantı İzleyicisi](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview#monitor-communication-between-a-virtual-machine-and-an-endpoint) özelliği ve ağ performansı İzleyicisi (NPM) [hizmet bağlantısı İzleyicisi](https://docs.microsoft.com/azure/azure-monitor/insights/network-performance-monitor-service-connectivity) özelliği.

Bağlantı Izleyicisi 'nin (Önizleme) bazı avantajları aşağıda verilmiştir:

* Azure ve karma izleme ihtiyaçlarına yönelik Birleşik, sezgisel deneyim
* Bölgeler arası, çapraz çalışma alanı bağlantısı izleme
* Daha yüksek yoklama sıklıkları ve ağ performansına daha iyi görünürlük
* Karma dağıtımlarınız için daha hızlı uyarı
* HTTP, TCP ve ıCMP tabanlı bağlantı denetimleri için destek 
* Hem Azure hem de Azure olmayan test kurulumları için ölçümler ve Log Analytics desteği

![Bağlantı Izleyicisinin Azure VM 'Leri, Azure dışı Konakları, uç noktaları ve veri depolama konumları ile nasıl etkileşime gireceğini gösteren diyagram](./media/connection-monitor-2-preview/hero-graphic.png)

İzleme için bağlantı Izleyicisi 'ni (Önizleme) kullanmaya başlamak için şu adımları izleyin: 

1. İzleme aracılarını yükler.
1. Aboneliğinizde ağ Izleyicisi 'Ni etkinleştirin.
1. Bağlantı İzleyicisi oluşturun.
1. Veri analizini ve uyarıları ayarlayın.
1. Ağınızdaki sorunları tanılayın.

Aşağıdaki bölümlerde bu adımlarla ilgili ayrıntılar sağlanmaktadır.

## <a name="install-monitoring-agents"></a>İzleme aracılarını yükler

Bağlantı Izleyicisi, bağlantı denetimlerini çalıştırmak için hafif yürütülebilir dosyaları kullanır.  Azure ortamlarından ve şirket içi ortamların bağlantı denetimlerini destekler. Kullandığınız yürütülebilir dosya, sanal makinenizin Azure 'da mı yoksa şirket içinde mi barındırıldığından bağlıdır.

### <a name="agents-for-azure-virtual-machines"></a>Azure sanal makineleri için aracılar

Bağlantı Izleyicisi 'nin Azure VM 'lerinizi izleme kaynakları olarak tanımasını sağlamak için, ağ Izleyicisi Aracısı sanal makine uzantısını bunlara yükler. Bu uzantı, *Ağ İzleyicisi uzantısı*olarak da bilinir. Azure sanal makineleri, uzantının uçtan uca izlemeyi ve diğer gelişmiş işlevleri tetiklemesine gerek duyar. 

[BIR VM oluşturduğunuzda](https://docs.microsoft.com/azure/network-watcher/connection-monitor#create-the-first-vm)ağ izleyicisi uzantısını yükleyebilirsiniz. Ayrıca, [Linux](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-linux) ve [Windows](https://docs.microsoft.com/azure/virtual-machines/extensions/network-watcher-windows)için ağ izleyicisi uzantısını ayrı olarak yükleyebilir, yapılandırabilir ve sorun giderebilirsiniz.

Bir ağ güvenlik grubu (NSG) veya güvenlik duvarı için kurallar, kaynak ve hedef arasındaki iletişimi engelleyebilirler. Bağlantı Izleyicisi bu sorunu algılar ve topolojide bir tanılama iletisi olarak gösterir. Bağlantı izlemeyi etkinleştirmek için NSG ve Güvenlik Duvarı kurallarının, kaynak ve hedef arasında TCP veya ıCMP üzerinden paketlere izin verildiğinden emin olun.

### <a name="agents-for-on-premises-machines"></a>Şirket içi makineler için aracılar

Bağlantı Izleyicisi 'ni izlemek için kaynak olarak şirket içi makinelerinizi tanımasını sağlamak için, makinelere Log Analytics aracısını yüklersiniz. Ağ Performansı İzleyicisi çözümü etkinleştirin. Bu aracılar Log Analytics çalışma alanlarına bağlanır, bu nedenle aracıların izlemeye başlamadan önce çalışma alanı KIMLIĞI ve birincil anahtarı ayarlamanız gerekir.

Windows makineleri için Log Analytics Aracısı 'nı yüklemek için bkz. [Windows Için Azure izleyici sanal makine uzantısı](https://docs.microsoft.com/azure/virtual-machines/extensions/oms-windows).

Yol güvenlik duvarları veya ağ sanal gereçler (NVA 'lar) içeriyorsa, hedefin erişilebilir olduğundan emin olun.

## <a name="enable-network-watcher-on-your-subscription"></a>Aboneliğinizde ağ Izleyicisi 'Ni etkinleştirin

Sanal ağı olan tüm abonelikler ağ Izleyicisi ile etkinleştirilir. Aboneliğinizde bir sanal ağ oluşturduğunuzda, sanal ağın bölgesinde ve aboneliğinde ağ Izleyicisi otomatik olarak etkinleştirilir. Bu otomatik etkinleştirme, kaynaklarınızı etkilemez veya ücretlendirir. Ağ Izleyicisi 'nin aboneliğinizde açık olarak devre dışı olmadığından emin olun. 

Daha fazla bilgi için bkz. [ağ Izleyicisini etkinleştirme](https://docs.microsoft.com/azure/network-watcher/network-watcher-create).

## <a name="create-a-connection-monitor"></a>Bağlantı izleyicisi oluşturma 

Bağlantı Izleyicisi, iletişimi düzenli aralıklarla izler. Ulaşılabilirlik ve gecikme süresi içindeki değişiklikleri bilgilendirir. Ayrıca, kaynak aracıları ile hedef uç noktalar arasındaki geçerli ve geçmiş ağ topolojisini kontrol edebilirsiniz.

Kaynaklar, yüklü bir izleme aracısına sahip Azure sanal makineleri veya şirket içi makineler olabilir. Hedef uç noktalar Office 365 URL 'Leri, Dynamics 365 URL 'Leri, özel URL 'ler, Azure VM kaynak kimlikleri, IPv4, IPv6, FQDN veya herhangi bir etki alanı adı olabilir.

### <a name="access-connection-monitor-preview"></a>Erişim bağlantısı Izleyicisi (Önizleme)

1. Azure portal giriş sayfasında **Ağ İzleyicisi**' ne gidin.
1. Sol tarafta, **izleme** bölümünde **Bağlantı İzleyicisi (Önizleme)** öğesini seçin.
1. Bağlantı Izleyicisinde (Önizleme) oluşturulan tüm bağlantı izleyicilerini görürsünüz. Klasik bağlantı Izleyici deneyiminde oluşturulan bağlantı izleyicilerini görmek için **Bağlantı İzleyicisi** sekmesine gidin.

    ![Bağlantı Izleyicisinde oluşturulan bağlantı izleyicilerini gösteren ekran görüntüsü (Önizleme)](./media/connection-monitor-2-preview/cm-resource-view.png)


### <a name="create-a-connection-monitor"></a>Bağlantı izleyicisi oluşturma

Bağlantı Izleyici (Önizleme) bölümünde oluşturduğunuz bağlantı izleyicilerinde, hem şirket içi makineleri hem de Azure VM 'lerini kaynak olarak ekleyebilirsiniz. Bu bağlantı izleyicileri, uç noktalara bağlantıyı da izleyebilir. Uç noktalar Azure veya başka bir URL veya IP üzerinde olabilir.

Bağlantı Izleyicisi (Önizleme) aşağıdaki varlıkları içerir:

* **Bağlantı İzleyicisi kaynağı** : bölgeye özgü bir Azure kaynağı. Aşağıdaki varlıkların hepsi bir bağlantı İzleyicisi kaynağının özellikleridir.
* **Uç nokta** : bağlantı denetimlerine katılan kaynak veya hedef. Uç noktalara örnek olarak Azure VM 'Leri, şirket içi aracılar, URL 'Ler ve IP 'Ler verilebilir.
* **Test yapılandırması** – bir test için protokole özgü bir yapılandırma. Seçtiğiniz protokole bağlı olarak, bağlantı noktası, eşikler, sınama sıklığı ve diğer parametreleri tanımlayabilirsiniz.
* **Test grubu** : kaynak uç noktaları, hedef uç noktaları ve test yapılandırmalarının bulunduğu grup. Bir bağlantı İzleyicisi birden fazla test grubu içerebilir.
* **Test** : kaynak uç noktası, hedef uç noktası ve test yapılandırmasının birleşimi. Test, izleme verilerinin kullanılabildiği en ayrıntılı düzeydir. İzleme verileri, başarısız olan denetim yüzdesini ve gidiş dönüş süresini (RTT) içerir.

 ![Test grupları ve testler arasındaki ilişkiyi tanımlayan bir bağlantı izleyicisini gösteren diyagram](./media/connection-monitor-2-preview/cm-tg-2.png)

#### <a name="create-a-connection-monitor-from-the-azure-portal"></a>Azure portal bir bağlantı izleyicisi oluşturun

Azure portal bir bağlantı İzleyicisi oluşturmak için aşağıdaki adımları izleyin:

1. **Bağlantı İzleyicisi (Önizleme)** panosunda, sol üst köşedeki **Oluştur**' u seçin.
1. **Temel bilgiler** sekmesinde, bağlantı monitörünüz için bilgi girin:
   * **Bağlantı Izleyicisi adı** – bağlantı monitörünüzün adını ekleyin. Azure kaynakları için standart adlandırma kurallarını kullanın.
   * **Abonelik** : bağlantı monitörünüz için bir abonelik seçin.
   * **Region** : bağlantı monitörünüz için bir bölge seçin. Yalnızca bu bölgede oluşturulan kaynak VM 'Leri seçebilirsiniz.
   * **Çalışma alanı yapılandırması** -çalışma alanınız izleme verilerinizi tutar. Özel bir çalışma alanı veya varsayılan çalışma alanı kullanabilirsiniz. 
       * Varsayılan çalışma alanını kullanmak için onay kutusunu işaretleyin. 
       * Özel bir çalışma alanı seçmek için onay kutusunu temizleyin. Ardından özel çalışma alanınız için abonelik ve bölge seçin. 
1. Sekmenin alt kısmındaki **İleri: test grupları**' nı seçin.

   ![Bağlantı Izleyicisinde temel bilgiler sekmesini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/create-cm-basics.png)

1. **Test grupları** sekmesinde **+ test grubu**' nu seçin. Test gruplarınızı ayarlamak için, bkz. [bağlantı Izleyicisinde test grupları oluşturma](#create-test-groups-in-a-connection-monitor). 
1. Sekmenin en altında, Ileri ' yi seçin ve bağlantı monitörünüzü gözden geçirmek için **Oluştur** ' a tıklayın.

   ![Test grupları sekmesini ve test grubu ayrıntılarını eklediğiniz bölmeyi gösteren ekran görüntüsü](./media/connection-monitor-2-preview/create-tg.png)

1. **Gözden geçir + oluştur** sekmesinde, bağlantı izleyicisini oluşturmadan önce temel bilgileri ve test gruplarını gözden geçirin. Bağlantı izleyicisini düzenlemeniz gerekiyorsa:
   * Temel ayrıntıları düzenlemek için kalem simgesini seçin.
   * Bir test grubunu düzenlemek için seçin.

   > [!NOTE] 
   > **Gözden geçir + oluştur** sekmesi, Bağlantı İzleyicisi önizleme aşamasında aylık maliyeti gösterir. Şu anda **GEÇERLI maliyet** sütunu hiçbir ücret göstermez. Bağlantı Izleyicisi genel kullanıma sunulduğunda, bu sütunda aylık bir ücret gösterilir. 
   > 
   > Bağlantı Izleyicisi önizleme aşamasında bile, Log Analytics alma ücretleri uygulanır.

1. Bağlantı izleyicisini oluşturmaya hazırsanız, **gözden geçir + oluştur** sekmesinin altındaki **Oluştur**' u seçin.

   ![Inceleme + Oluştur sekmesini gösteren bağlantı Izleyicisinin ekran görüntüsü](./media/connection-monitor-2-preview/review-create-cm.png)

Bağlantı Izleyicisi (Önizleme) arka planda Bağlantı İzleyicisi kaynağını oluşturur.

#### <a name="create-a-connection-monitor-by-using-armclient"></a>ARMClient kullanarak bağlantı İzleyicisi oluşturma

ARMClient kullanarak bir bağlantı İzleyicisi oluşturmak için aşağıdaki kodu kullanın.

```armclient
$connectionMonitorName = "sampleConnectionMonitor"

$ARM = "https://management.azure.com"

$SUB = "subscriptions/<subscription id 1>;"

$NW = "resourceGroups/NetworkWatcherRG/providers/Microsoft.Network/networkWatchers/NetworkWatcher\_<region>"

$body =

"{

location: '<region>',

properties: {

endpoints: [{

name: 'workspace',

resourceId: '/subscriptions/<subscription id>/resourcegroups/<resource group>/providers/Microsoft.OperationalInsights/workspaces/sampleWorkspace',

filter: {

 items: [{

type: 'AgentAddress',

address: '<FQDN of your on-premises agent>'

}]

}

          },

 {

name: 'vm1',

resourceId: '/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<vm-name>'

},

 {

name: 'vm2',

resourceId: '/subscriptions/<subscription id>/resourceGroups/<resource group>/providers/Microsoft.Compute/virtualMachines/<vm-name>'

   },

{

name: 'azure portal'

address: '<URL>'

   },

 {

    name: 'ip',

     address: '<IP>'

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

   // Choose your protocol
   
    testConfigurations: ['http', 'https', 'tcpDisabled', 'icmpDisabled'],

    sources: ['vm1'],

    destinations: ['vm2'],

    disable: true

   }

  ],

  testConfigurations: [{

    name: 'http',

    testFrequencySec: <frequency>,

    protocol: 'HTTP',

    successThreshold: {

     checksFailedPercent: <threshold for checks failed %>,

     roundTripTimeMs: <threshold for RTT>

    }

   }, {

    name: 'https',

    testFrequencySec: <frequency>,

    protocol: 'HTTP',

    httpConfiguration: {

     preferHTTPS: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'tcpEnabled',

    testFrequencySec: <frequency>,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'icmpEnabled',

    testFrequencySec: <frequency>,

    protocol: 'ICMP',

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'icmpDisabled',

    testFrequencySec: <frequency>,

    protocol: 'ICMP',

    icmpConfiguration: {

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }, {

    name: 'tcpDisabled',

    testFrequencySec: <frequency>,

    protocol: 'TCP',

    tcpConfiguration: {

     port: 80,

     disableTraceRoute: true

    },

    successThreshold: {

     checksFailedPercent: <choose your checks failed threshold>,

     roundTripTimeMs: <choose your RTT threshold>

    }

   }

  ]

 }

} "
```

Dağıtım komutu aşağıda verilmiştir:
```
armclient PUT $ARM/$SUB/$NW/connectionMonitors/$connectionMonitorName/?api-version=2019-07-01 $body -verbose
```

### <a name="create-test-groups-in-a-connection-monitor"></a>Bağlantı izleyicisinde test grupları oluşturma

Bir bağlantı izleyicisinde bulunan her bir test grubu, ağ parametrelerini test eden kaynakları ve hedefleri içerir. Başarısız olan denetim yüzdesi ve test yapılandırmalarının RTT üzerinden test edilmiştir.

Azure portal, bir bağlantı izleyicisinde bir test grubu oluşturmak için aşağıdaki alanlar için değerler belirtirsiniz:

* **Test grubunu devre dışı bırak** – test grubunun belirttiği tüm kaynaklar ve hedefler için izlemeyi devre dışı bırakmak üzere bu alanı seçebilirsiniz. Bu seçim varsayılan olarak temizlenir.
* **Ad** – test grubunuzu adlandırın.
* **Kaynaklar** : aracılar bunlara yüklenmişse hem Azure VM 'lerini hem de şirket içi makineleri kaynak olarak belirtebilirsiniz. Kaynağınıza yönelik bir aracı yüklemek için bkz. [izleme aracılarını yüklemek](#install-monitoring-agents).
   * Azure aracılarını seçmek için **Azure aracıları** sekmesini seçin. Burada yalnızca bağlantı izleyicisini oluştururken belirttiğiniz bölgeye bağlı olan VM 'Leri görürsünüz. Varsayılan olarak VM 'Ler, ait oldukları abonelikte gruplandırılır. Bu gruplar daraltılmıştır. 
   
       Abonelik düzeyinden hiyerarşideki diğer düzeylere ayrıntıya gidebilirsiniz:

      **Abonelik** > **kaynak** > **VNETs**grupları > **aracıları olan sanal** **ağlar alt ağları** > 

      Ayrıca, ağacı başka bir düzeyden başlatmak için **Group By** alanı değerini de değiştirebilirsiniz. Örneğin, sanal ağ ile gruplandırma yaparsanız,**aracılarla**sanal **VNETs** > **ağlar alt ağları** > VM 'lerinde aracıların bulunduğu VM 'leri görürsünüz.

      ![Kaynak ekleme paneli ve Azure aracıları sekmesini gösteren bağlantı Izleyicisinin ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-sources.png)

   * Şirket içi aracıları seçmek için, **Azure dışı aracılar** sekmesini seçin. Varsayılan olarak, aracılar bölgelere göre çalışma alanları halinde gruplandırılır. Bu çalışma alanlarının tümünde Ağ Performansı İzleyicisi çözümü yapılandırıldı. 
   
       Çalışma alanınıza Ağ Performansı İzleyicisi eklemeniz gerekiyorsa [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.NetworkMonitoringOMS?tab=Overview)'nden alın. Ağ Performansı İzleyicisi ekleme hakkında daha fazla bilgi için bkz. [Azure izleyici 'de çözümleri izleme](https://docs.microsoft.com/azure/azure-monitor/insights/solutions). 
   
       **Bağlantı Izleyicisi oluştur** görünümünde, **temel bilgiler** sekmesinde varsayılan bölge seçilidir. Bölgeyi değiştirirseniz, yeni bölgedeki çalışma alanlarından aracılar seçebilirsiniz. Ayrıca **Group By** alanı değerini, alt ağlara göre gruplandırma olarak değiştirebilirsiniz.

      ![Kaynak ekleme paneli ve Azure dışı aracılar sekmesini gösteren bağlantı Izleyicisinin ekran görüntüsü](./media/connection-monitor-2-preview/add-non-azure-sources.png)


   * Seçtiğiniz Azure ve Azure olmayan aracıları gözden geçirmek için **İnceleme** sekmesine gidin.

      ![Kaynak Ekle panelini ve gözden geçirme sekmesini gösteren bağlantı Izleyicisinin ekran görüntüsü](./media/connection-monitor-2-preview/review-sources.png)

   * Kaynakları ayarlamayı tamamladığınızda, **Kaynak Ekle** panelinin alt kısmındaki **bitti**' yi seçin.

* **Hedefler** : Azure VM 'lerine veya herhangi bir uç noktaya (genel IP, URL veya FQDN) yönelik bağlantıyı, hedefler olarak belirterek izleyebilirsiniz. Tek bir test grubunda Azure VM 'leri, Office 365 URL 'Leri, Dynamics 365 URL 'Leri ve özel uç noktaları ekleyebilirsiniz.

    * Azure VM 'lerini hedef olarak seçmek için **Azure VM 'leri** sekmesini seçin. Varsayılan olarak, Azure VM 'Leri, **temel bilgiler** sekmesinde **Bağlantı İzleyicisi oluştur** görünümünde seçtiğiniz bölgede bulunan bir abonelik hiyerarşisinde gruplandırılır. Bölgeyi değiştirebilir ve yeni seçilen bölgeden Azure VM 'Leri seçebilirsiniz. Daha sonra, Azure Agents düzeyi gibi, abonelik düzeyinden hiyerarşideki diğer düzeylere gidebilirsiniz.

       ![Azure VM 'Leri sekmesini gösteren hedefler ekle bölmesinin ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-dests1.png)

       ![Abonelik düzeyini gösteren hedefler ekle bölmesinin ekran görüntüsü](./media/connection-monitor-2-preview/add-azure-dests2.png)

    * Uç noktaları hedef olarak seçmek için **bitiş noktaları** sekmesini seçin. Uç noktalar listesi, Office 365 test URL 'Leri ve Dynamics 365 test URL 'Lerini ada göre gruplanmış olarak içerir. Bu uç noktalara ek olarak, aynı bağlantı izleyicisinde diğer test gruplarında oluşturulmuş bir uç nokta seçebilirsiniz. 
    
        Yeni bir uç nokta eklemek için sağ üst köşede **+ uç noktalar**' ı seçin. Ardından bir uç nokta adı ve URL, IP veya FQDN belirtin.

       ![Bağlantı Izleyicisinde uç noktaların hedef olarak nereye ekleneceğini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/add-endpoints.png)

    * Seçtiğiniz Azure sanal makinelerini ve uç noktalarını gözden geçirmek için **İnceleme** sekmesini seçin.
    * Hedefleri seçme işini bitirdiğinizde **bitti**' yi seçin.

* **Test yapılandırması** – test yapılandırmasını bir test grubunda ilişkilendirebilirsiniz. Azure portal, test grubu başına yalnızca bir test yapılandırmasına izin verir, ancak daha fazla eklemek için ARMClient kullanabilirsiniz.

    * **Ad** – test yapılandırmasını adlandırın.
    * **Protokol** : TCP, ıCMP veya http ' yi seçin. HTTP 'yi HTTPS olarak değiştirmek için protokol olarak **http** ' yi seçin ve bağlantı noktası olarak **443** ' yı seçin.
        * **Ağ test yapılandırması oluştur** – bu onay kutusu yalnızca **protokol** alanında **http** ' yi seçtiğinizde görünür. Yapılandırmanızda başka bir yerde belirttiğiniz kaynakları ve hedefleri kullanan başka bir test yapılandırması oluşturmak için bu kutuyu seçin. Yeni oluşturulan test yapılandırması olarak adlandırılmıştır `<the name of your test configuration>_networkTestConfig`.
        * İzleme hareketini **devre dışı bırak** – Bu alan, protokolü TCP veya ICMP olan test grupları için geçerlidir. Kaynakları, topolojiyi ve Atlama RTT 'yi bulmaktan durdurmak için bu kutuyu seçin.
    * **Hedef bağlantı noktası** : bu alanı, seçtiğiniz bir hedef bağlantı noktasıyla özelleştirebilirsiniz.
    * **Sınama sıklığı** – kaynakların, belirttiğiniz protokol ve bağlantı noktasındaki hedeflere ne sıklıkta ping uygulayabileceğini belirlemek için bu alanı kullanın. 30 saniye, 1 dakika, 5 dakika, 15 dakika veya 30 dakika seçebilirsiniz. Kaynaklar, seçtiğiniz değere göre hedeflere bağlantıyı test edecektir.  Örneğin, 30 saniye seçerseniz, kaynaklar en az 30 saniyelik bir dönemde hedefe bağlantıyı kontrol eder.
    * **Başarı eşiği** – aşağıdaki ağ parametrelerinde eşikler ayarlayabilirsiniz:
       * **Denetim başarısız** : kaynaklar, belirttiğiniz kriterleri kullanarak hedeflere bağlantı denetlediğinde başarısız olan denetim yüzdesini ayarlayın. TCP veya ıCMP protokolü için, başarısız denetimlerin yüzdesi, paket kaybı yüzdesine eşit hale getirilmiş olabilir. HTTP protokolü için, bu alan yanıt olmadan alınan HTTP isteklerinin yüzdesini temsil eder.
       * **Gidiş dönüş süresi** – bir test yapılandırması üzerinden hedefe bağlanmak için kaynakların ne kadar süreceğine ILIŞKIN olarak RTT 'yi milisaniye olarak ayarlayın.
    
       ![Bağlantı Izleyicisinde bir test yapılandırmasının nerede ayarlanacağını gösteren ekran görüntüsü](./media/connection-monitor-2-preview/add-test-config.png)

Bir test grubuna eklediğiniz tüm kaynaklar, hedefler ve test yapılandırması, bireysel testlere bölünmüştür. Kaynak ve hedeflerin nasıl bölündüğü hakkında bir örnek aşağıda verilmiştir:

* Test grubu: TG1
* Kaynaklar: 3 (A, B, C)
* Hedefler: 2 (D, E)
* Test yapılandırması: 2 (yapılandırma 1, yapılandırma 2)
* Oluşturulan toplam test sayısı: 12

| Sınama numarası | Kaynak | Hedef | Test yapılandırması |
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

Bağlantı izleyicileri aşağıdaki ölçek sınırlarına sahiptir:

* Her bölge için abonelik başına en fazla bağlantı İzleyicisi: 100
* Bağlantı İzleyicisi başına en fazla test grubu: 20
* Bağlantı İzleyicisi başına en fazla kaynak ve hedef sayısı: 100
* Bağlantı İzleyicisi başına en fazla test yapılandırması: 
    * 20 ile ARMClient
    * 2 Azure portal aracılığıyla

## <a name="analyze-monitoring-data-and-set-alerts"></a>İzleme verilerini çözümleme ve uyarıları ayarlama

Bir bağlantı İzleyicisi oluşturduktan sonra, kaynaklar test yapılandırmanıza göre hedeflere bağlantıyı denetler.

### <a name="checks-in-a-test"></a>Bir testte denetimler

Bağlantı Izleyicisi (Önizleme), test yapılandırmasında seçtiğiniz protokole bağlı olarak, kaynak-hedef çifti için bir dizi denetim çalıştırır. Denetimler, seçtiğiniz test sıklığına göre çalışır.

HTTP kullanırsanız, hizmet yanıt kodu döndüren HTTP yanıtlarının sayısını hesaplar. Sonuç, başarısız denetimlerin yüzdesini belirler. RTT 'yi hesaplamak için, hizmet bir HTTP çağrısıyla yanıt arasındaki süreyi ölçer.

TCP veya ıCMP kullanıyorsanız, hizmet başarısız denetimlerin yüzdesini öğrenmek için paket kaybı yüzdesini hesaplar. RTT 'yi hesaplamak için, hizmet gönderilen paketlerin onay (ACK) alınması için geçen süreyi ölçer. Ağ testleriniz için izleme yolu verilerini etkinleştirdiyseniz, şirket içi ağınız için atlama kaybını ve gecikme süresini görebilirsiniz.

### <a name="states-of-a-test"></a>Bir testin durumları

Denetimlerin döndürdüğü verilere bağlı olarak, testler aşağıdaki durumlara sahip olabilir:

* **Pass** : başarısız denetimlerin yüzdesi için gerçek değerler ve RTT belirtilen eşiklerin içindedir.
* **Başarısız** – başarısız DENETIMLERIN veya RTT yüzdesinin gerçek değerleri belirtilen eşikleri aştı. Eşik belirtilmemişse, başarısız denetimlerin yüzdesi 100 olduğunda bir test başarısız durumuna ulaşır.
* **Uyarı** – başarısız denetimlerin yüzdesi için hiçbir ölçüt belirtilmemiş. Belirtilen ölçüt yokluğunda, bağlantı Izleyicisi (Önizleme) otomatik olarak bir eşik atar. Bu eşik aşıldığında, test durumu uyarı olarak değişir.

### <a name="data-collection-analysis-and-alerts"></a>Veri toplama, analiz ve uyarılar

Bağlantı Izleyicisi (Önizleme) tarafından toplanan veriler Log Analytics çalışma alanında depolanır. Bu çalışma alanını bağlantı izleyicisini oluştururken ayarlarsınız. 

Azure Izleyici ölçümlerinde izleme verileri de mevcuttur. İzleme verilerinizi istediğiniz uzunlukta tutmak için Log Analytics kullanabilirsiniz. Azure Izleyici, ölçümleri varsayılan olarak yalnızca 30 gün boyunca depolar. 

[Veride ölçüm tabanlı uyarılar ayarlayabilirsiniz](https://azure.microsoft.com/blog/monitor-at-scale-in-azure-monitor-with-multi-resource-metric-alerts/).

#### <a name="monitoring-dashboards"></a>Panoları izleme

İzleme Panoları ' nde, abonelikleriniz, bölgeler, zaman damgaları, kaynaklarınız ve hedef türleri için erişebileceğiniz bağlantı izleyicilerinin bir listesini görürsünüz.

Ağ Izleyicisinden bağlantı Izleyicisi 'ne (Önizleme) gittiğinizde, verileri şu şekilde görüntüleyebilirsiniz:

* **Bağlantı İzleyicisi** – abonelikleriniz, bölgeler, zaman damgaları, kaynaklarınız ve hedef türleri için oluşturulan tüm bağlantı izleyicilerinin listesi. Bu görünüm varsayılandır.
* **Test grupları** – abonelikleriniz, bölgelerleriniz, zaman damgaları, kaynaklarınız ve hedef türleri için oluşturulan tüm test gruplarının listesidir. Bu test grupları bağlantı izleyicilerine göre filtrelenmez.
* **Test** – abonelikleriniz, bölgeler, zaman damgaları, kaynaklarınız ve hedef türleriniz için çalışan tüm testlerin listesi. Bu testler bağlantı izleyicilerine veya test gruplarına göre filtrelenmez.

Aşağıdaki görüntüde, üç veri görünümü, ok 1 ile belirtilmiştir.

Panoda, her bir bağlantı izleyicisini genişleterek test gruplarını görüntüleyebilirsiniz. Ardından, içinde çalışan testleri görmek için her bir test grubunu genişletebilirsiniz. 

Bir listeyi temelinde filtreleyebilirsiniz:

* **Üst düzey filtreler** – abonelikler, bölgeler, zaman damgası kaynakları ve hedef türleri seçin. Aşağıdaki görüntüde Box 2 ' ye bakın.
* **Durum tabanlı filtreler** : Bağlantı İzleyicisi, test grubu veya test durumuna göre filtreleyin. Aşağıdaki görüntüde ok 3 ' ü inceleyin.
* **Özel filtreler** – genel arama yapmak Için **Tümünü Seç ' i** seçin. Belirli bir varlığa göre arama yapmak için, açılan listeden öğesini seçin. Aşağıdaki görüntüde ok 4 ' ü inceleyin.

![Bağlantı Izleyicilerindeki bağlantı izleyicilerinin, test gruplarının ve testlerin nasıl filtreleneceğini gösteren ekran görüntüsü (Önizleme)](./media/connection-monitor-2-preview/cm-view.png)

Örneğin, kaynak IP 'nin 10.192.64.56 olduğu bağlantı Izleyicisi 'ndeki (Önizleme) tüm testlere bakmak için:
1. Görünümü **Test**olacak şekilde değiştirin.
1. Arama alanına *10.192.64.56* yazın.
1. Açılan listede **kaynaklar**' ı seçin.

Kaynak IP 'nin 10.192.64.56 olduğu bağlantı Izleyicisinde (Önizleme) yalnızca başarısız testleri göstermek için:
1. Görünümü **Test**olacak şekilde değiştirin.
1. Durum tabanlı filtre için **başarısız**' ı seçin.
1. Arama alanına *10.192.64.56* yazın.
1. Açılan listede **kaynaklar**' ı seçin.

Yalnızca hedef outlook.office365.com olan bağlantı Izleyicisinde (Önizleme) başarısız testleri göstermek için:
1. Görünümü **Test**olarak değiştirin.
1. Durum tabanlı filtre için **başarısız**' ı seçin.
1. Arama alanına *Outlook.office365.com* girin.
1. Açılan listede **hedefler**' i seçin.

   ![Outlook.Office365.com hedefi için yalnızca başarısız testleri göstermek üzere filtrelenmiş bir görünümü gösteren ekran görüntüsü](./media/connection-monitor-2-preview/tests-view.png)

RTT içindeki eğilimleri ve bir bağlantı izleyicisinin başarısız olan denetim yüzdesini görüntülemek için:
1. Araştırmak istediğiniz bağlantı izleyicisini seçin. Varsayılan olarak, izleme verileri test grubuna göre düzenlenir.

   ![Test grubuna göre gösterilen bir bağlantı izleyicisinin ölçümlerini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/cm-drill-landing.png)

1. Araştırmak istediğiniz test grubunu seçin.

   ![Bir test grubunun nerede seçileceğini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/cm-drill-select-tg.png)

    Başarısız denetimlerin RTT veya yüzdesine göre test grubunuzun en çok başarısız olan beş testi görürsünüz. Her test için, başarısız denetimlerin yüzdesi için RTT ve eğilim çizgilerini görürsünüz.
1. Listeden bir test seçin veya araştırmak için başka bir test seçin. Zaman aralığı ve başarısız denetimlerin yüzdesi için eşik ve gerçek değerler görürsünüz. RTT için eşik, ortalama, minimum ve maksimum değerlerini görürsünüz.

   ![Bir testin RTT ve başarısız denetimlerin yüzdesi için sonuçlarını gösteren ekran görüntüsü](./media/connection-monitor-2-preview/cm-drill-charts.png)

1. Daha fazla veri görüntülemek için zaman aralığını değiştirin.
1. Kaynakları, hedefleri veya test yapılandırmasını görmek için görünümü değiştirin. 
1. Başarısız testler temelinde bir kaynak seçin ve başarısız olan ilk beş testi araştırın. Örneğin, bağlantı izleyicisinde ilgili testleri araştırmak için**kaynaklara** **göre** > görüntüle ve > **hedeflere** **göre görüntüle**' yi seçin.

   ![İlk beş başarısız test için performans ölçümlerini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/cm-drill-select-source.png)

RTT içindeki eğilimleri ve bir test grubu için başarısız denetimlerin yüzdesini görüntülemek için:

1. Araştırmak istediğiniz test grubunu seçin. 

    Varsayılan olarak, izleme verileri kaynaklar, hedefler ve test yapılandırmalarına (testler) göre düzenlenir. Daha sonra, test gruplarından kaynakları, hedefleri veya test yapılandırmalarına kadar görünümü değiştirebilirsiniz. Ardından, ilk beş başarısız testi araştırmak için bir varlık seçin. Örneğin, seçilen bağlantı izleyicisinde ilgili testleri araştırmak için görünümü kaynaklar ve hedefler olarak değiştirin.
1. Araştırmak istediğiniz testi seçin.

   ![Testin nerede seçileceğini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/tg-drill.png)

    Zaman aralığı ve başarısız denetimlerin yüzdesi için eşik değerlerini ve gerçek değerleri görürsünüz. RTT için eşik, ortalama, minimum ve maksimum değerlerini görürsünüz. Seçtiğiniz test için tetiklenen uyarıları da görürsünüz.
1. Daha fazla veri görüntülemek için zaman aralığını değiştirin.

RTT içindeki eğilimleri ve test için başarısız denetimlerin yüzdesini görüntülemek için:
1. Araştırmak istediğiniz kaynak, hedef ve test yapılandırmasını seçin.

    Zaman aralığı ve başarısız denetimlerin yüzdesi için eşik değerlerini ve gerçek değerleri görürsünüz. RTT için eşik, ortalama, minimum ve maksimum değerlerini görürsünüz. Seçtiğiniz test için tetiklenen uyarıları da görürsünüz.

   ![Bir test için ölçümleri gösteren ekran görüntüsü](./media/connection-monitor-2-preview/test-drill.png)

1. Ağ topolojisini görmek için **topoloji**' yi seçin.

   ![Ağ topolojisi sekmesini gösteren ekran görüntüsü](./media/connection-monitor-2-preview/test-topo.png)

1. Tanımlanan sorunları görmek için, topolojide, yoldaki herhangi bir atlama seçin. (Bu atlamalar Azure kaynaklarıdır.) Bu işlevsellik şu anda şirket içi ağlarda kullanılamıyor.

   ![Topoloji sekmesindeki seçili atlama bağlantısını gösteren ekran görüntüsü](./media/connection-monitor-2-preview/test-topo-hop.png)

#### <a name="log-queries-in-log-analytics"></a>Log Analytics 'de günlük sorguları

İzleme verilerinizin özel görünümlerini oluşturmak için Log Analytics kullanın. Kullanıcı arabiriminin gösterdiği tüm veriler Log Analytics. Depodaki verileri etkileşimli olarak analiz edebilirsiniz. Aracı Durumu veya Log Analytics tabanlı diğer çözümlerdeki verilerin ilişkilendirilmesi. Verileri Excel 'e aktarın veya Power BI veya paylaşılabilir bir bağlantı oluşturun.

#### <a name="metrics-in-azure-monitor"></a>Azure İzleyicisi'nde ölçümler

Bağlantı Izleyicisi (Önizleme) deneyiminden önce oluşturulan bağlantı izleyicilerinde, dört ölçüm de kullanılabilir:% araştırmaları başarısız, AverageRoundtripMs, ChecksFailedPercent (Önizleme) ve Roundüçlü zaman MS (Önizleme). Bağlantı Izleyicisi (Önizleme) deneyiminde oluşturulan bağlantı izleyicilerinde, veriler yalnızca ile etiketlenmiş ölçümler *(Önizleme)* için kullanılabilir.

![Bağlantı Izleyicisinde ölçümleri gösteren ekran görüntüsü (Önizleme)](./media/connection-monitor-2-preview/monitor-metrics.png)

Ölçümleri kullandığınızda, kaynak türünü Microsoft. Network/networkWatchers/Connectionmonitörleri olarak ayarlayın

| Ölçüm | Görünen ad | Birim | Toplama türü | Açıklama | Boyutlar |
| --- | --- | --- | --- | --- | --- |
| ProbesFailedPercent | % Yoklama başarısız oldu | Yüzde | Ortalama | Bağlantı İzleme Araştırmaları yüzdesi başarısız oldu. | Boyut yok |
| AverageRoundtripMs | Ort. gidiş dönüş süresi (MS) | Mayacak | Ortalama | Kaynak ve hedef arasında gönderilen bağlantı izleme araştırmaları için Ortalama ağ RTT. |             Boyut yok |
| ChecksFailedPercent (Önizleme) | % Denetim başarısız oldu (Önizleme) | Yüzde | Ortalama | Bir test için başarısız denetimlerin yüzdesi. | Connectionmonitorresourceıd <br>SourceAddress <br>Kaynak <br>Sourceresourceıd <br>KaynakTürü <br>Protokol <br>Hedef adres <br>Hedef adı <br>Hedef RESOURCEID <br>Hedef türü <br>DestinationPort <br>TestGroupName <br>TestConfigurationName <br>Bölge |
| Roundüçlü MS (Önizleme) | Gidiş dönüş süresi (MS) (Önizleme) | Mayacak | Ortalama | Kaynak ve hedef arasında gönderilen denetimler için RTT. Bu değer, ortalama değildir. | Connectionmonitorresourceıd <br>SourceAddress <br>Kaynak <br>Sourceresourceıd <br>KaynakTürü <br>Protokol <br>Hedef adres <br>Hedef adı <br>Hedef RESOURCEID <br>Hedef türü <br>DestinationPort <br>TestGroupName <br>TestConfigurationName <br>Bölge |

#### <a name="metric-alerts-in-azure-monitor"></a>Azure Izleyici 'de ölçüm uyarıları

Azure Izleyici 'de bir uyarı oluşturmak için:

1. Bağlantı Izleyicisi (Önizleme) bölümünde oluşturduğunuz Bağlantı İzleyicisi kaynağını seçin.
1. **Ölçümün** Bağlantı İzleyicisi için sinyal türü olarak görüntülendiğinden emin olun.
1. **Koşul**adı bölümünde, **sinyal adı**Için, **checksfailedpercent (Önizleme)** veya **roundüçlü MS (Önizleme)** öğesini seçin.
1. **Sinyal türü**için **ölçümler**' i seçin. Örneğin, **Checksfailedpercent (Önizleme)** öğesini seçin.
1. Ölçümün tüm boyutları listelenir. Boyut adı ve boyut değerini seçin. Örneğin, **kaynak adresi** ' ni seçin ve ardından bağlantı izleyicinizdeki herhangi BIR kaynağın IP adresini girin.
1. **Uyarı mantığı**' nda aşağıdaki ayrıntıları girin:
   * **Koşul türü**: **statik**.
   * **Koşul** ve **eşik**.
   * **Toplama ayrıntı düzeyi ve değerlendirme sıklığı**: bağlantı Izleyicisi (Önizleme) her dakikada verileri güncelleştirir.
1. **Eylemler**' de eylem grubunuzu seçin.
1. Uyarı ayrıntılarını belirtin.
1. Uyarı kuralını oluşturun.

   ![Azure Izleyici 'de kural oluştur alanını gösteren ekran görüntüsü; "Kaynak adresi" ve "kaynak uç noktası adı" vurgulanır](./media/connection-monitor-2-preview/mdm-alerts.jpg)

## <a name="diagnose-issues-in-your-network"></a>Ağınızdaki sorunları tanılayın

Bağlantı Izleyicisi (Önizleme), bağlantı izleyicinizdeki ve ağınızdaki sorunları tanılamanıza yardımcı olur. Karma ağınızdaki sorunlar, daha önce yüklediğiniz Log Analytics aracıları tarafından algılanır. Azure 'daki sorunlar ağ Izleyicisi uzantısı tarafından algılanır. 

Ağ topolojisinde Azure ağı 'ndaki sorunları görüntüleyebilirsiniz.

Kaynakları şirket içi VM 'Ler olan ağlarda aşağıdaki sorunlar tespit edilebilir:

* İstek zaman aşımına uğradı.
* Uç nokta DNS tarafından çözümlenmedi-geçici veya kalıcı. URL geçersiz.
* Hiçbir ana bilgisayar bulunamadı.
* Kaynak, hedefle bağlantı kuramıyor. Hedefe ıCMP üzerinden ulaşılamıyor.
* Sertifikayla ilgili sorunlar: 
    * Aracının kimliğini doğrulamak için istemci sertifikası gereklidir. 
    * Sertifika konum değiştirme listesine erişilemiyor. 
    * Uç noktanın ana bilgisayar adı, sertifikanın konu veya konu diğer adı ile eşleşmiyor. 
    * Kaynağın yerel bilgisayar güvenilen sertifika yetkilileri deposunda kök sertifika yok. 
    * SSL sertifikasının zaman aşımına uğradı, geçersiz, iptal edildi veya uyumsuz.

Kaynakları Azure VM 'Leri olan ağlarda aşağıdaki sorunlar tespit edilebilir:

* Aracı sorunları:
    * Aracı durdu.
    * DNS çözümlemesi başarısız oldu.
    * Hedef bağlantı noktasını dinleyen uygulama veya dinleyici yok.
    * Yuva açılamadı.
* VM durumu sorunları: 
    * Başlatılıyor
    * Durduruluyor
    * Durduruldu
    * Serbest bırakılıyor
    * Serbest bırakıldı
    * Yeniden başlatılıyor
    * Ayrılmadı
* ARP tablosu girdisi eksik.
* Yerel güvenlik duvarı sorunları veya NSG kuralları nedeniyle trafik engellendi.
* Sanal ağ geçidi sorunları: 
    * Eksik yollar.
    * İki ağ geçidi arasındaki tünelin bağlantısı kesildi veya yok.
    * İkinci ağ geçidi, tünel tarafından bulunamadı.
    * Eşleme bilgisi bulunamadı.
* Microsoft Edge 'de yol eksikti.
* Sistem yolları veya UDR nedeniyle trafik durduruldu.
* Ağ Geçidi bağlantısında BGP etkin değil.
* DIP araştırması, yük dengeleyicide çalışıyor.
