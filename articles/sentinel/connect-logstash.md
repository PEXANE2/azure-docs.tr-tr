---
title: Azure Sentinel 'e Logstash aracılığıyla veri kaynaklarını bağlama | Microsoft Docs
description: Günlükleri dış veri kaynaklarından Azure Sentinel 'e iletmek için Logstash 'i nasıl kullanacağınızı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/27/2020
ms.author: yelevin
ms.openlocfilehash: 79d29ef228fc27655da30edbeb64abcb01e45d5e
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2020
ms.locfileid: "84237181"
---
# <a name="use-logstash-to-connect-data-sources-to-azure-sentinel"></a>Veri kaynaklarını Azure Sentinel 'e bağlamak için Logstash 'i kullanma

> [!IMPORTANT]
> Logstash çıkış eklentisi kullanılarak veri alımı Şu anda genel önizlemededir. Bu özellik, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

**Logstash veri toplama altyapısı**Için Azure Sentinel 'in yeni çıkış eklentisini kullanarak, artık Azure sentinel 'de Log Analytics çalışma alanınıza doğrudan logstash aracılığıyla istediğiniz her türlü günlüğü gönderebilirsiniz. Günlüklerinizi, çıkış eklentisini kullanarak tanımlayacağınızı belirten özel bir tabloya gönderilir.

Logstash veri toplama altyapısıyla çalışma hakkında daha fazla bilgi edinmek için bkz. [Logstash ile çalışmaya başlama](https://www.elastic.co/guide/en/logstash/current/getting-started-with-logstash.html).

## <a name="overview"></a>Genel Bakış

### <a name="architecture-and-background"></a>Mimari ve arka plan

   ![Logstash mimarisi](./media/connect-logstash/logstash-architecture.png)

Logstash altyapısı üç bileşenden oluşur:

- Giriş eklentileri-çeşitli kaynaklardan özelleştirilmiş veri koleksiyonu
- Eklentileri filtrele-belirtilen ölçütlere göre verilerin işlenmesi ve normalleştirilmesi
- Çıkış eklentileri-toplanan ve işlenmiş verilerin çeşitli hedeflere gönderilmesi özelleştirilmiş

> [!NOTE]
> Azure Sentinel yalnızca kendi sağlanmış çıkış eklentisini destekler. Azure Sentinel için üçüncü taraf çıkış eklentilerini veya herhangi bir türdeki başka bir Logstash eklentisini desteklemez.

Logstash için Azure Sentinel çıktı eklentisi, Log Analytics çalışma alanınıza JSON biçimli verileri gönderir Log Analytics HTTP veri toplayıcı REST API ' nı kullanarak. Veriler özel günlüklere alınır.

- [Log Analytics REST API hakkında daha fazla bilgi edinin](https://docs.microsoft.com/rest/api/loganalytics/create-request).
- [Özel Günlükler hakkında daha fazla bilgi edinin](https://docs.microsoft.com/azure/azure-monitor/platform/data-sources-custom-logs).

## <a name="installing-and-configuring-the-azure-sentinel-output-plugin-in-logstash"></a>Logstash 'te Azure Sentinel çıkış eklentisini yükleme ve yapılandırma

1. **Yükleme**

    Azure Sentinel çıktı eklentisi, Logstash koleksiyonunda kullanılabilir.

    - ***Microsoft-logstash-output-Azure-loganalytics*** eklentisini yüklemek için [Eklentiler belgesi Ile çalışan](https://www.elastic.co/guide/en/logstash/current/working-with-plugins.html) logstash içindeki yönergeleri izleyin.
    - Logstash sisteminizin Internet erişimi yoksa, bir çevrimdışı eklenti paketi hazırlamak ve kullanmak için Logstash [çevrimdışı eklenti yönetimi](https://www.elastic.co/guide/en/logstash/current/offline-plugins.html) belgesindeki yönergeleri izleyin. (Bu, Internet erişimi ile başka bir Logstash sistemi oluşturmanızı gerektirir.)

1. **Yapılandırma**

    [Bir yapılandırma dosyası](https://www.elastic.co/guide/en/logstash/current/configuration-file-structure.html) belgesinin Logstash yapısındaki bilgileri kullanarak, yapılandırmaya aşağıdaki anahtarlar ve değerlerle Azure Sentinel çıktı eklentisini ekleyin (tablonun altındaki uygun yapılandırma dosyası sözdizimine bakın):

    | **Alan adı** | **Veri türü** | **Açıklama** |
    |----------------|---------------|-----------------|
    | `workspace_id` | string | Çalışma alanı KIMLIĞI GUID 'nizi girin |
    | `workspace_key` | string | Çalışma alanınızın birincil anahtar GUID 'nizi girin |
    | `custom_log_table_name` | string | Günlüklerin alınacağı tablonun adını ayarlayın. Her çıkış eklentisi için yalnızca bir tablo adı yapılandırılabilir. Günlük tablosu, **özel Günlükler** kategorisindeki **tablolarda** bulunan **Günlükler**altında Azure Sentinel 'de, bir sonek ile görünür `_CL` . |
    | `endpoint` | string | İsteğe bağlı alan. Bu, varsayılan olarak Log Analytics uç noktasıdır. Alternatif bir uç nokta ayarlamak için bu alanı kullanın. |
    | `time_generated_field` | string | İsteğe bağlı alan. Bu özellik Log Analytics varsayılan **TimeGenerated** alanını geçersiz kılar. Veri kaynağındaki zaman damgası alanının adını girin. Alandaki verilerin ISO 8601 biçimine () uyması gerekir `YYYY-MM-DDThh:mm:ssZ` |
    | `key_names` | array | Log Analytics çıktı şeması alanlarının bir listesini girin. Her liste öğesi tek tırnak içine, virgülle ayrılmış öğeleri ve köşeli parantez içine alınmış tüm listeyi kapsamalıdır. Aşağıdaki örneğe bakın. |
    | `plugin_flush_interval` | sayı | İsteğe bağlı alan. İleti aktarımları arasındaki en büyük aralığı (saniye olarak) Log Analytics olarak tanımlamak için ayarlayın. Varsayılan değer 5 ' tir. |
    | `amount_resizing` | boole | True/false. İleti arabellek boyutunu alınan günlük verisi hacmine göre ayarlayan otomatik ölçeklendirme mekanizmasını etkinleştirin veya devre dışı bırakın. |
    | `max_items` | sayı | İsteğe bağlı alan. Yalnızca `amount_resizing` "false" olarak ayarlandıysa geçerlidir. İleti arabellek boyutunda bir üst sınır ayarlamak için kullanın (kayıtlar). Varsayılan değer 2000’dir.  |

    **Örnek yapılandırma**

        input {
            tcp {
                port => 514
                type => syslog
            }
        }

        filter {
            grok {
                match => { "message" => "<%{NUMBER:PRI}>1 (?<TIME_TAG>[0-9]{4}-[0-9]{1,2}-[0-9]{1,2}T[0-9]{1,2}:[0-9]{1,2}:[0-9]{1,2})[^ ]* (?<HOSTNAME>[^ ]*) %{GREEDYDATA:MSG}" }
            }
        }

        output {
            logstash-output-azure-loganalytics {
                workspace_id => "<WS_ID>"
                workspace_key => "${WS_KEY}"
                custom_log_table_name => "logstashCustomTable"
                key_names => ['PRI','TIME_TAG','HOSTNAME','MSG']
                plugin_flush_interval => 5
            }
        } 

    > [!NOTE]
    > İç işleyişi, yapılandırması ve performans ayarları hakkında daha fazla bilgi edinmek için çıkış eklentisinin [GitHub](https://github.com/Azure/Azure-Sentinel/tree/master/DataConnectors/microsoft-logstash-output-azure-loganalytics) ' ı ziyaret edin.

1. **Logstash 'i yeniden Başlat**

1. **Azure Sentinel 'de gelen günlükleri görüntüleme**

    1. İletilerin çıkış eklentisine gönderildiğini doğrulayın.

    1. Azure Sentinel gezinti menüsünde **Günlükler**' e tıklayın. **Tablolar** başlığı altında **özel Günlükler** kategorisini genişletin. Yapılandırmada belirttiğiniz tablonun adını bulun ve tıklayın (bir `_CL` sonek ile).

        ![Logstash özel günlükleri](./media/connect-logstash/logstash-custom-logs-menu.png)

    1. Tablodaki kayıtları görmek için tablo adını şema olarak kullanarak tabloyu sorgulayın.

        ![Logstash özel günlük sorgusu](./media/connect-logstash/logstash-custom-logs-query.png)

## <a name="monitor-output-plugin-audit-logs"></a>Çıktı eklentisi denetim günlüklerini izleme

Azure Sentinel çıkış eklentisinin bağlantısını ve etkinliğini izlemek için, uygun Logstash günlük dosyasını etkinleştirin. Günlük dosyasının konumu için [Logstash Dizin düzen](https://www.elastic.co/guide/en/logstash/current/dir-layout.html#dir-layout) belgesine bakın.

Bu günlük dosyasında herhangi bir veri görmüyorsanız, çıkış eklentisinin veri aldığından emin olmak için bazı olayları yerel olarak oluşturup gönderin (giriş ve filtre eklentileri aracılığıyla). Azure Sentinel yalnızca çıkış eklentisine ilişkin sorunları destekleyecektir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Sentinel 'e dış veri kaynakları bağlamak için Logstash 'in nasıl kullanılacağını öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Yerleşik](tutorial-detect-threats-built-in.md) veya [özel](tutorial-detect-threats-custom.md) kurallar kullanarak Azure Sentinel ile tehditleri algılamaya başlayın.