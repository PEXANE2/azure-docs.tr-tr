---
title: Bağlantı Izleyicisi önizlemesi-ARMClient
titleSuffix: Azure Network Watcher
description: ARMClient kullanarak bağlantı Izleyicisi 'ni (Önizleme) oluşturmayı öğrenin.
services: network-watcher
documentationcenter: na
author: vinigam
ms.service: network-watcher
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2020
ms.author: vinigam
ms.openlocfilehash: 7d35799cd73ff4d065cb58189f2325dc4dac6840
ms.sourcegitcommit: 97a0d868b9d36072ec5e872b3c77fa33b9ce7194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87567996"
---
# <a name="create-a-connection-monitor-preview-using-the-armclient"></a>ARMClient kullanarak bağlantı Izleyicisi (Önizleme) oluşturma

ARMClient kullanarak kaynaklarınız arasındaki iletişimi izlemek için bağlantı Izleyicisi 'ni (Önizleme) oluşturmayı öğrenin. Karma ve Azure bulut dağıtımlarını destekler.

## <a name="before-you-begin"></a>Başlamadan önce 

Bağlantı Izleyici (Önizleme) bölümünde oluşturduğunuz bağlantı izleyicilerinde, hem şirket içi makineleri hem de Azure VM 'lerini kaynak olarak ekleyebilirsiniz. Bu bağlantı izleyicileri, uç noktalara bağlantıyı da izleyebilir. Uç noktalar Azure veya başka bir URL veya IP üzerinde olabilir.

Bağlantı Izleyicisi (Önizleme) aşağıdaki varlıkları içerir:

* **Bağlantı İzleyicisi kaynağı** : bölgeye özgü bir Azure kaynağı. Aşağıdaki varlıkların hepsi bir bağlantı İzleyicisi kaynağının özellikleridir.
* **Uç nokta** : bağlantı denetimlerine katılan kaynak veya hedef. Uç noktalara örnek olarak Azure VM 'Leri, şirket içi aracılar, URL 'Ler ve IP 'Ler verilebilir.
* **Test yapılandırması** – bir test için protokole özgü bir yapılandırma. Seçtiğiniz protokole bağlı olarak, bağlantı noktası, eşikler, sınama sıklığı ve diğer parametreleri tanımlayabilirsiniz.
* **Test grubu** : kaynak uç noktaları, hedef uç noktaları ve test yapılandırmalarının bulunduğu grup. Bir bağlantı İzleyicisi birden fazla test grubu içerebilir.
* **Test** : kaynak uç noktası, hedef uç noktası ve test yapılandırmasının birleşimi. Test, izleme verilerinin kullanılabildiği en ayrıntılı düzeydir. İzleme verileri, başarısız olan denetim yüzdesini ve gidiş dönüş süresini (RTT) içerir.

    ![Test grupları ve testler arasındaki ilişkiyi tanımlayan bir bağlantı izleyicisini gösteren diyagram](./media/connection-monitor-2-preview/cm-tg-2.png)

## <a name="steps-to-create-with-sample-arm-template"></a>Örnek ARM şablonuyla oluşturma adımları

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

## <a name="description-of-properties"></a>Özelliklerin açıklaması

* connectionMonitorName-Bağlantı İzleyicisi kaynağının adı

* Bağlantı İzleyicisi oluşturmak istediğiniz aboneliğin alt abonelik KIMLIĞI

* NW-ağ Izleyicisi kaynak KIMLIĞI, CM 'nin oluşturulacağı 

* Bağlantı İzleyicisi 'nin oluşturulacağı konum bölgesi

* Uç Noktalar
    * ad – her uç nokta için benzersiz ad
    * RESOURCEID – Azure uç noktaları Için kaynak KIMLIĞI, sanal makineler için Azure Resource Manager (ARM) kaynak KIMLIĞINE başvurur. Azure dışı uç noktalar için kaynak KIMLIĞI, Azure olmayan aracılara bağlı Log Analytics çalışma alanı için ARM kaynak KIMLIĞI anlamına gelir.
    * Adres: yalnızca kaynak KIMLIĞI belirtilmediğinde veya kaynak KIMLIĞI Log Analytics çalışma alanı ise geçerlidir. Log Analytics kaynak KIMLIĞIYLE birlikte kullanılırsa, bu, izleme için kullanılabilen aracının FQDN 'sine başvurur. Kaynak KIMLIĞI olmadan kullanılırsa bu, herhangi bir genel bitiş noktasının URL veya IP 'SI olabilir.
    * Filtre: Azure dışı uç noktalar Için, Bağlantı İzleyicisi kaynağında izlemek üzere kullanılacak Log Analytics çalışma alanından aracıları seçmek için filtre kullanın. Filtreler ayarlanmamışsa, Log Analytics çalışma alanına ait olan tüm aracılar izleme için kullanılabilir
        * tür – türü "aracı adresi" olarak ayarla
        * Adres: adresi şirket içi aracınızın FQDN 'SI olarak ayarlayın

* Test grupları
    * ad-test grubunuzu adlandırın.
    * testConfigurations-kaynak uç noktaların hedef uç noktalara bağlandığı test yapılandırması
    * kaynaklar-yukarıda oluşturulan uç noktalardan seçin. Azure tabanlı kaynak uç noktalarında Azure ağ Izleyicisi uzantısının yüklü olması ve Azure tabanlı kaynak uç noktalarında Azure Log Analytics aracısının yüklü olması gerekir. Kaynağınıza yönelik bir aracı yüklemek için bkz. [izleme aracılarını yüklemek](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#install-monitoring-agents).
    * hedefler-yukarıda oluşturulan uç noktalardan seçin. Azure VM 'lerine veya herhangi bir uç noktaya (genel IP, URL veya FQDN) yönelik bağlantıyı, bunları hedefler olarak belirterek izleyebilirsiniz. Tek bir test grubunda Azure VM 'leri, Office 365 URL 'Leri, Dynamics 365 URL 'Leri ve özel uç noktaları ekleyebilirsiniz.
    * devre dışı bırak-bu alanı, test grubunun belirttiği tüm kaynaklar ve hedefler için izlemeyi devre dışı bırakmak için kullanın.

* Test yapılandırması
    * ad-test yapılandırmasının adı.
    * testFrequencySec-kaynakların, belirttiğiniz protokol ve bağlantı noktasındaki hedeflere ne sıklıkta ping olarak alınacağını belirtin. 30 saniye, 1 dakika, 5 dakika, 15 dakika veya 30 dakika seçebilirsiniz. Kaynaklar, seçtiğiniz değere göre hedeflere bağlantıyı test edecektir. Örneğin, 30 saniye seçerseniz, kaynaklar en az 30 saniyelik bir dönemde hedefe bağlantıyı kontrol eder.
    * protokol-TCP, ıCMP, HTTP veya HTTPS seçeneklerini belirleyebilirsiniz. Protokolüne bağlı olarak, bazı protokole özgü yapılandırmaları yapabilirsiniz
        * preferHTTPS-HTTP üzerinden HTTPS kullanıp kullanmayacağınızı belirtin
        * bağlantı noktası-tercih ettiğiniz hedef bağlantı noktasını belirtin.
        * Disableizleme Oute-bu, protokolü TCP veya ıCMP olan test grupları için geçerlidir. Topoloji ve atlama-atlama RTT bulma kaynaklarını durdurur.
    * Başarılı eşik-aşağıdaki ağ parametrelerinde eşikler ayarlayabilirsiniz:
        * checksFailedPercent-belirttiğiniz ölçütlere göre kaynaklar hedeflere bağlantı denetlediğinde başarısız olan denetim yüzdesini ayarlayın. TCP veya ıCMP protokolü için, başarısız denetimlerin yüzdesi, paket kaybı yüzdesine eşit hale getirilmiş olabilir. HTTP protokolü için, bu alan yanıt olmadan alınan HTTP isteklerinin yüzdesini temsil eder.
        * Roundroundtimems-kaynakların test yapılandırması üzerinden hedefe bağlanması için geçen süreyi milisaniye olarak ayarlayın.

## <a name="scale-limits"></a>Ölçek sınırları

Bağlantı izleyicileri aşağıdaki ölçek sınırlarına sahiptir:

* Her bölge için abonelik başına en fazla bağlantı İzleyicisi: 100
* Bağlantı İzleyicisi başına en fazla test grubu: 20
* Bağlantı İzleyicisi başına en fazla kaynak ve hedef sayısı: 100
* Bağlantı İzleyicisi başına en fazla test yapılandırması: ARMClient aracılığıyla 20

## <a name="next-steps"></a>Sonraki adımlar

* [İzleme verilerini çözümlemeyi ve uyarıları ayarlamayı](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#analyze-monitoring-data-and-set-alerts) öğrenin
* [Ağınızdaki sorunları tanılamayı](https://docs.microsoft.com/azure/network-watcher/connection-monitor-preview#diagnose-issues-in-your-network) öğrenin
