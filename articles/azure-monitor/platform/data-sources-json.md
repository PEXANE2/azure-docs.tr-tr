---
title: Azure Izleyici 'de özel JSON verileri toplama | Microsoft Docs
description: Özel JSON veri kaynakları, Linux için Log Analytics Aracısı kullanılarak Azure Izleyici 'ye toplanabilir.  Bu özel veri kaynakları, bir dizi gibi JSON döndüren basit betikler veya Floentıd 'nin 300 + eklentilerinden biridir. Bu makalede, bu veri koleksiyonu için gereken yapılandırma açıklanmaktadır.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/28/2018
ms.openlocfilehash: 407257dbe9fbfa560153d5044263fc4c947cb05c
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86111941"
---
# <a name="collecting-custom-json-data-sources-with-the-log-analytics-agent-for-linux-in-azure-monitor"></a>Azure Izleyici 'de Linux için Log Analytics Aracısı ile özel JSON veri kaynakları toplama
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)]

Özel JSON veri kaynakları, Linux için Log Analytics Aracısı kullanılarak [Azure izleyici](data-platform.md) 'ye toplanabilir.  Bu özel veri kaynakları, bir dizi gibi JSON döndüren basit betikler [veya](https://curl.haxx.se/) [floentıd 'nin 300 + eklentilerinden](https://www.fluentd.org/plugins/all)biridir. Bu makalede, bu veri koleksiyonu için gereken yapılandırma açıklanmaktadır.


> [!NOTE]
> Özel JSON verileri için Log Analytics Agent for Linux v 1.1.0-TII + gereklidir

## <a name="configuration"></a>Yapılandırma

### <a name="configure-input-plugin"></a>Giriş eklentisini yapılandırma

Azure Izleyici 'de JSON verileri toplamak için, `oms.api.` giriş eklentisindeki bir Floentd etiketinin başlangıcına ekleyin.

Örneğin, aşağıdaki ayrı bir yapılandırma dosyasıdır `exec-json.conf` `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` .  Bu, `exec` her 30 saniyede bir kıvrımlı komutu çalıştırmak Için Floentd eklentisini kullanır.  Bu komutun çıktısı, JSON çıktı eklentisi tarafından toplanır.

```xml
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

Altına eklenen yapılandırma dosyası, `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/` aşağıdaki komutla onun sahipliğinin değiştirilmesini gerektirir.

`sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/conf/omsagent.d/exec-json.conf`

### <a name="configure-output-plugin"></a>Çıkış eklentisini yapılandırma 
' Deki ana yapılandırmaya `/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.conf` veya içindeki ayrı bir yapılandırma dosyasına yerleştirilmiş olarak aşağıdaki çıkış eklentisi yapılandırmasını ekleyin`/etc/opt/microsoft/omsagent/<workspace id>/conf/omsagent.d/`

```xml
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

```console
sudo /opt/microsoft/omsagent/bin/service_control restart 
```

## <a name="output"></a>Çıktı
Veriler Azure Izleyici 'de bir kayıt türüyle toplanacaktır `<FLUENTD_TAG>_CL` .

Örneğin, `tag oms.api.tomcat` Azure izleyici 'de bir kayıt türü içeren özel etiket `tomcat_CL` .  Bu türün tüm kayıtlarını aşağıdaki günlük sorgusuyla alabilirsiniz.

```console
Type=tomcat_CL
```

İç içe geçmiş JSON veri kaynakları desteklenir, ancak üst alanı temel alan dizine alınır. Örneğin, aşağıdaki JSON verileri bir günlük sorgusundan olarak döndürülür `tag_s : "[{ "a":"1", "b":"2" }]` .

```json
{
    "tag": [{
        "a":"1",
        "b":"2"
    }]
}
```


## <a name="next-steps"></a>Sonraki adımlar
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin. 
