---
title: Azure Izleyici 'de CollectD 'den veri toplayın | Microsoft Docs
description: CollectD, uygulamalardan ve sistem düzeyi bilgilerden düzenli olarak veri toplayan açık kaynaklı bir Linux Daemon.  Bu makalede, Azure Izleyici 'de CollectD 'den veri toplama hakkında bilgi sağlanır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/27/2018
ms.openlocfilehash: b8c09d4ac5d0856eb0d448a1cabd9adc567850c4
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77670619"
---
# <a name="collect-data-from-collectd-on-linux-agents-in-azure-monitor"></a>Azure Izleyici 'de Linux aracılarında CollectD 'den veri toplama
[Collectd](https://collectd.org/) , uygulamalardan ve sistem düzeyi bilgilerden düzenli olarak performans ölçümlerini toplayan açık kaynaklı bir Linux Daemon. Örnek uygulamalar Java Sanal Makinesi (JVM), MySQL Server ve NGINX ' i içerir. Bu makalede, Azure Izleyici 'de CollectD 'den performans verilerinin toplanması hakkında bilgi sağlanır.

Kullanılabilir eklentilerin tam listesi, [Eklentiler tablosunda](https://collectd.org/wiki/index.php/Table_of_Plugins)bulunabilir.

![CollectD genel bakış](media/data-sources-collectd/overview.png)

Aşağıdaki CollectD yapılandırması, Linux için Log Analytics aracısına, CollectD verilerini Linux için Log Analytics aracısına yönlendirmenize dahildir.

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

    LoadPlugin write_http

    <Plugin write_http>
         <Node "oms">
         URL "127.0.0.1:26000/oms.collectd"
         Format "JSON"
         StoreRates true
         </Node>
    </Plugin>

Ayrıca, 5,5 öncesinde bir collectD sürümü kullanılıyorsa, bunun yerine aşağıdaki yapılandırmayı kullanın.

    LoadPlugin write_http

    <Plugin write_http>
       <URL "127.0.0.1:26000/oms.collectd">
        Format "JSON"
         StoreRates true
       </URL>
    </Plugin>

CollectD yapılandırması, 26000 numaralı bağlantı noktası üzerinden performans ölçümü verilerini Linux için Log Analytics aracısına göndermek için varsayılan`write_http` eklentisini kullanır. 

> [!NOTE]
> Bu bağlantı noktası, gerekirse özel tanımlı bir bağlantı noktasına yapılandırılabilir.

Linux için Log Analytics Aracısı Ayrıca, CollectD ölçümleri için 26000 numaralı bağlantı noktasını dinler ve ardından bunları Azure Izleyici şeması ölçümlerine dönüştürür. Linux yapılandırma `collectd.conf`için Log Analytics Aracısı aşağıda verilmiştir.

    <source>
      type http
      port 26000
      bind 127.0.0.1
    </source>

    <filter oms.collectd>
      type filter_collectd
    </filter>

> [!NOTE]
> CollectD varsayılan olarak 10 saniyelik bir [aralıkta](https://collectd.org/wiki/index.php/Interval)değerleri okumak üzere ayarlanır. Bu işlem, Azure Izleyici günlüklerine gönderilen veri hacmini doğrudan etkilediği için, izleme gereksinimleri ile ilişkili maliyetler ve Azure Izleyici günlüklerinin kullanımı arasında iyi bir denge sağlamak için CollectD yapılandırması dahilinde bu zaman aralığını ayarlamanız gerekebilir.

## <a name="versions-supported"></a>Desteklenen sürümler
- Azure Izleyici Şu anda CollectD sürüm 4,8 ve üstünü desteklemektedir.
- CollectD ölçüm koleksiyonu için Log Analytics Agent for Linux v 1.1.0-bir veya üzeri gereklidir.


## <a name="configuration"></a>Yapılandırma
Azure Izleyici 'de CollectD verilerinin toplanmasını yapılandırmak için temel adımlar aşağıda verilmiştir.

1. Write_http eklentisini kullanarak, CollectD 'yi Linux için Log Analytics aracısına veri gönderecek şekilde yapılandırın.  
2. Linux için Log Analytics Aracısı 'nı uygun bağlantı noktasındaki CollectD verilerini dinlemek üzere yapılandırın.
3. CollectD ve Linux için Log Analytics Aracısı 'nı yeniden başlatın.

### <a name="configure-collectd-to-forward-data"></a>Verileri iletmek için CollectD 'yi yapılandırma 

1. CollectD verilerini Linux için Log Analytics aracısına yönlendirmek için, `oms.conf` CollectD 'nin yapılandırma dizinine eklenmesi gerekir. Bu dosyanın hedefi, makinenizin Linux 'a göre değişir.

    CollectD yapılandırma dizininiz/etc/collectd.exe dizininde bulunuyorsa:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd.d/oms.conf

    CollectD yapılandırma dizininiz/etc/collectd/collectd.exe dizininde bulunuyorsa:

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/oms.conf /etc/collectd/collectd.conf.d/oms.conf

    >[!NOTE]
    >5,5 ' dan önceki CollectD sürümleri için `oms.conf` etiketleri yukarıda gösterildiği gibi değiştirmeniz gerekecektir.
    >

2. Collectd. conf dosyasını istenen çalışma alanının omsagent yapılandırma dizinine kopyalayın.

        sudo cp /etc/opt/microsoft/omsagent/sysconf/omsagent.d/collectd.conf /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/
        sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/collectd.conf

3. Aşağıdaki komutlarla, CollectD ve Linux için Log Analytics Aracısı 'nı yeniden başlatın.

    sudo hizmeti toplanan sudo/seçenek/Microsoft/omsagent/bin/service_control yeniden başlatma

## <a name="collectd-metrics-to-azure-monitor-schema-conversion"></a>CollectD ölçümlerini Azure Izleyici şemasına dönüştürme
Linux için Log Analytics Aracısı tarafından zaten toplanan altyapı ölçümleri ve CollectD tarafından toplanan yeni ölçümler arasında tanıdık bir modeli korumak için aşağıdaki şema eşlemesi kullanılır:

| CollectD ölçüm alanı | Azure Izleyici alanı |
|:--|:--|
| `host` | Bilgisayar |
| `plugin` | None |
| `plugin_instance` | Örnek adı<br>**Plugin_instance** *null* ise InstanceName = " *_Total*" |
| `type` | ObjectName |
| `type_instance` | CounterName<br>**Type_instance** *null* ise CounterName =**Blank** |
| `dsnames[]` | CounterName |
| `dstypes` | None |
| `values[]` | Onay değeri |

## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin. 
* Syslog kayıtlarından verileri tek tek alanlara ayrıştırmak için [özel alanları](custom-fields.md) kullanın.
