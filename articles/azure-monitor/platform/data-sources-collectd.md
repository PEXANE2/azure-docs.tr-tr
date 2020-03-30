---
title: Azure Monitör'de CollectD'den veri toplama | Microsoft Dokümanlar
description: CollectD, düzenli olarak uygulamalardan ve sistem düzeyi bilgilerinden veri toplayan açık kaynaklı bir Linux daemon'dur.  Bu makale, Azure Monitor'da CollectD'den veri toplama hakkında bilgi sağlar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/27/2018
ms.openlocfilehash: b8c09d4ac5d0856eb0d448a1cabd9adc567850c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77670619"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-azure-monitor"></a>Azure Monitor'da Linux aracıları üzerinde Toplanan'dan veri toplama
[CollectD,](https://collectd.org/) uygulamalardan ve sistem düzeyi bilgilerinden düzenli olarak performans ölçümleri toplayan bir açık kaynak Linux daemon'dur. Örnek uygulamalar Arasında Java Virtual Machine (JVM), MySQL Server ve Nginx sayılabilir. Bu makalede, Azure Monitor'da CollectD'den performans verileri toplama hakkında bilgi verilmektedir.

Mevcut eklentilerin tam listesini [Tablo Eklentileri](https://collectd.org/wiki/index.php/Table_of_Plugins)bulunabilir.

![Toplanan genel bakış](media/data-sources-collectd/overview.png)

Aşağıdaki CollectD yapılandırması, Toplanan verileri Linux için Log Analytics temsilcisine yönlendirmek için Linux için Log Analytics aracısına dahildir.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Ayrıca, 5.5'ten önce collectD sürümlerini kullanıyorsanız, bunun yerine aşağıdaki yapılandırmayı kullanın.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

CollectD yapılandırması,`write_http` performans metrik verilerini 26000 portu üzerinden Linux için Log Analytics temsilcisine göndermek için varsayılan eklentiyi kullanır. 

> [!NOTE]
> Bu bağlantı noktası, gerekirse özel tanımlı bir bağlantı noktasına yapılandırılabilir.

Linux için Log Analytics aracısı, CollectD ölçümleri için 26000 bağlantı noktasında da dinler ve bunları Azure Monitor şema ölçümlerine dönüştürür. Aşağıdaki Linux yapılandırması `collectd.conf`için Log Analytics aracısıdır.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>

> [!NOTE]
> Varsayılan olarak toplanan değerler 10 [saniyelik](https://collectd.org/wiki/index.php/Interval)aralıkta okunacak şekilde ayarlanır. Bu, Azure Monitör Günlükleri'ne gönderilen veri hacmini doğrudan etkilediğinden, izleme gereksinimleri ile ilişkili maliyetler ve Azure Monitör Günlükleri için kullanım arasında iyi bir denge oluşturmak için Bu aralığı CollectD yapılandırması içinde ayarlamanız gerekebilir.

## <a name="versions-supported"></a>Desteklenen sürümler
- Azure Monitor şu anda CollectD sürüm 4.8 ve üzeri destekler.
- Linux v1.1.0-217 veya üzeri için Log Analytics aracısı CollectD metrik koleksiyonu için gereklidir.


## <a name="configuration"></a>Yapılandırma
Azure Monitor'da Toplanan verilerin toplanmasını yapılandırmak için temel adımlar aşağıda veda edilebedilir.

1. write_http eklentisini kullanarak Linux için Log Analytics temsilcisine veri göndermek için CollectD'i yapılandırın.  
2. Linux için Log Analytics aracısını, toplanan verileri uygun bağlantı noktasında dinlemek üzere yapılandırın.
3. Linux için CollectD ve Log Analytics aracıyı yeniden başlatın.

### <a name="configure-collectd-to-forward-data"></a>Verileri iletmek için Toplananı yapılandır 

1. Toplanan verileri Linux için Log Analytics temsilcisine yönlendirmek için CollectD'in yapılandırma dizinine eklenmesi `oms.conf` gerekir. Bu dosyanın hedefi makinenizin Linux dağıtımına bağlıdır.

    CollectD config dizininiz /etc/collectd.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    CollectD config dizininiz /etc/collectd/collectd.conf.d/:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >5.5'ten önce Toplanan sürümler için, `oms.conf` yukarıda gösterildiği gibi etiketleri değiştirmeniz gerekir.
    >

2. Toplanan kopya.conf istenilen çalışma alanının omsagent yapılandırma dizinine.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Aşağıdaki komutlarla Linux için CollectD ve Log Analytics aracısını yeniden başlatın.

    sudo hizmeti toplanan yeniden başlatma sudo /opt/microsoft/omsagent/bin/service_control yeniden başlatma

## <a name="collectd-metrics-to-azure-monitor-schema-conversion"></a>Azure Monitör şema dönüştürmesine toplanan ölçümler
Linux için Log Analytics aracısı tarafından zaten toplanan altyapı ölçümleri ile CollectD tarafından toplanan yeni ölçümler arasında tanıdık bir model sağlamak için aşağıdaki şema eşleme kullanılır:

| Toplanan Metrik alan | Azure Monitör alanı |
|:--|:--|
| `host` | Bilgisayar |
| `plugin` | None |
| `plugin_instance` | Örnek Adı<br>Eğer **plugin_instance** *null* ise InstanceName="*_Total*" |
| `type` | ObjectName |
| `type_instance` | Countername<br>**type_instance** *null* ise CounterName=**boş** |
| `dsnames[]` | Countername |
| `dstypes` | None |
| `values[]` | Karşı Değer |

## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin. 
* Syslog kayıtlarından tek tek alanlara ayrıştamak için [Özel Alanlar'ı](custom-fields.md) kullanın.
