---
title: Azure Izleyici 'de özel JSON verileri toplama | Microsoft Docs
description: Özel JSON veri kaynakları, Linux için Log Analytics Aracısı kullanılarak Azure Izleyici 'ye toplanabilir.  Bu özel veri kaynakları, bir dizi gibi JSON döndüren basit betikler veya Floentıd 'nin 300 + eklentilerinden biridir. Bu makalede, bu veri koleksiyonu için gereken yapılandırma açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: 49eb3fa22bc9afffb9e93f3152cdc00323b76d41
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77662170"
---
# <a name="collecting-custom-json-data-sources-with-the-log-analytics-agent-for-linux-in-azure-monitor"></a>Azure Izleyici 'de Linux için Log Analytics Aracısı ile özel JSON veri kaynakları toplama
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Özel JSON veri kaynakları, Linux için Log Analytics Aracısı kullanılarak [Azure izleyici](data-platform.md) 'ye toplanabilir.  Bu özel veri kaynakları, bir dizi gibi JSON döndüren basit betikler [veya](https://curl.haxx.se/) [floentıd 'nin 300 + eklentilerinden](https://www.fluentd.org/plugins/all)biridir. Bu makalede, bu veri koleksiyonu için gereken yapılandırma açıklanmaktadır.


> [!NOTE]
> Özel JSON verileri için Log Analytics Agent for Linux v 1.1.0-TII + gereklidir

## <a name="configuration"></a>Yapılandırma

### <a name="configure-input-plugin"></a>Giriş eklentisini yapılandırma

Azure Izleyici 'de JSON verileri toplamak için, giriş `oms.api.` eklentisindeki bir Floentd etiketinin başlangıcına ekleyin.

Örneğin, aşağıdaki ayrı bir yapılandırma dosyasıdır `exec-json.conf` `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`.  Bu, her 30 saniyede bir kıvrımlı `exec` komutu çalıştırmak Için Floentd eklentisini kullanır.  Bu komutun çıktısı, JSON çıktı eklentisi tarafından toplanır.

```
<source>
  type exec
  command 'curl localhost/json.output'
  format json
  tag oms.api.httpresponse
  run_interval 30s
</source>

<match oms.api.httpresponse>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api_httpresponse*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```
Altına `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` eklenen yapılandırma dosyası, aşağıdaki komutla onun sahipliğinin değiştirilmesini gerektirir.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Çıkış eklentisini yapılandırma 
' Deki `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` ana yapılandırmaya veya içindeki ayrı bir yapılandırma dosyasına yerleştirilmiş olarak aşağıdaki çıkış eklentisi yapılandırmasını ekleyin`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`

```
<match oms.api.**>
  type out_oms_api
  log_level info

  buffer_chunk_limit 5m
  buffer_type file
  buffer_path /var/opt/microsoft/omsagent/<workspace id>/state/out_oms_api*.buffer
  buffer_queue_limit 10
  flush_interval 20s
  retry_limit 10
  retry_wait 30s
</match>
```

### <a name="restart-log-analytics-agent-for-linux"></a>Linux için Log Analytics aracısını yeniden başlatma
Linux hizmeti için Log Analytics aracısını aşağıdaki komutla yeniden başlatın.

    sudo /opt/microsoft/omsagent/bin/service_control restart 

## <a name="output"></a>Çıktı
Veriler Azure Izleyici 'de bir kayıt türüyle toplanacaktır `<FLUENTD_TAG>_CL`.

Örneğin, Azure Izleyici 'de bir `tag oms.api.tomcat` kayıt türü içeren özel etiket `tomcat_CL`.  Bu türün tüm kayıtlarını aşağıdaki günlük sorgusuyla alabilirsiniz.

    Type=tomcat_CL

İç içe geçmiş JSON veri kaynakları desteklenir, ancak üst alanı temel alan dizine alınır. Örneğin, aşağıdaki JSON verileri bir günlük sorgusundan olarak `tag_s : "[{ "a":"1", "b":"2" }]`döndürülür.

```
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin. 
