---
title: Yuvarlanan pencere tetikleyici bağımlılıkları oluşturma
description: Azure Veri Fabrikası'nda yuvarlanan bir pencere tetikleyicisine nasıl bağımlılık yaratılabildiğini öğrenin.
services: data-factory
ms.author: daperlov
author: djpmsft
manager: anandsub
ms.service: data-factory
ms.workload: data-services
ms.devlang: na
ms.topic: conceptual
ms.custom: seo-lt-2019
ms.date: 07/29/2019
ms.openlocfilehash: 39ea8dda0fd823d3061b2cb29e1c548f99281c82
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81418805"
---
# <a name="create-a-tumbling-window-trigger-dependency"></a>Atlayan pencere tetikleyici bağımlılığı oluşturma
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, yuvarlanan bir pencere tetikleyicibir bağımlılık oluşturmak için adımlar sağlar. Yuvarlanan Pencere tetikleyicileri hakkında genel bilgi [için, yuvarlanan pencere tetikleyicisini nasıl oluşturura](how-to-create-tumbling-window-trigger.md)bakın.

Bir bağımlılık zinciri oluşturmak ve bir tetikleyicinin yalnızca veri fabrikasında başka bir tetikleyicinin başarılı bir şekilde yürütülmesinden sonra yürütülmesini sağlamak için, bu gelişmiş özelliği yuvarlanan bir pencere bağımlılığı oluşturmak için kullanın.

## <a name="create-a-dependency-in-the-data-factory-ui"></a>Veri Fabrikası UI'sinde bağımlılık oluşturma

Tetikleyiciye bağımlılık oluşturmak **için, Yeni > Gelişmiş > >** Tetikle'yi seçin ve ardından uygun uzaklık ve büyüklüğe bağlı olarak tetikleyiciyi seçin. **Bağımlılıkların** etkili olması için Veri Fabrikası değişikliklerini Bitir'i seçin ve yayımlayın.

![Bağımlılık Oluşturma](media/tumbling-window-trigger-dependency/tumbling-window-dependency01.png "Bağımlılık Oluşturma")

## <a name="tumbling-window-dependency-properties"></a>Yuvarlanan pencere bağımlılık özellikleri

Bağımlılık içeren yuvarlanan pencere tetikleyicisi aşağıdaki özelliklere sahiptir:

```json
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": <<Started/Stopped/Disabled - readonly>>,
        "typeProperties": {
            "frequency": <<Minute/Hour>>,
            "interval": <<int>>,
            "startTime": <<datetime>>,
            "endTime": <<datetime – optional>>,
            "delay": <<timespan – optional>>,
            "maxConcurrency": <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count": <<int - optional, default: 0>>,
                "intervalInSeconds": <<int>>,
            },
            "dependsOn": [
                {
                    "type": "TumblingWindowTriggerDependencyReference",
                    "size": <<timespan – optional>>,
                    "offset": <<timespan – optional>>,
                    "referenceTrigger": {
                        "referenceName": "MyTumblingWindowDependency1",
                        "type": "TriggerReference"
                    }
                },
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": <<timespan – optional>>,
                    "offset": <<timespan>>
                }
            ]
        }
    }
}
```

Aşağıdaki tablo, Yuvarlanan Pencere bağımlılığını tanımlamak için gereken özniteliklerin listesini sağlar.

| **Özellik Adı** | **Açıklama**  | **Tür** | **Gerekli** |
|---|---|---|---|
| type  | Varolan tüm yuvarlanan pencere tetikleyicileri bu açılır pencerede görüntülenir. Bağımlılık yapmak için tetikleyiciyi seçin.  | TumblingWindowTriggerDependencyReference veya SelfDependencyTumblingWindowTriggerReference | Evet |
| uzaklık | Bağımlılık tetikleyicisinin mahsup uyruşu. Zaman aralığı biçiminde bir değer sağlayın ve hem negatif hem de pozitif uzaklıklara izin verilir. Tetikleyici kendisine bağlıysa ve diğer tüm durumlarda isteğe bağlıysa, bu özellik zorunludur. Kendine bağımlılık her zaman olumsuz bir dengeleme olmalıdır. Değer belirtilmemişse, pencere tetikleyicinin kendisiyle aynıdır. | Timespan<br/>(hh:mm:ss) | Kendine Bağımlılık: Evet<br/>Diğer: Hayır |
| size | Bağımlılık yuvarlanan pencerenin boyutu. Pozitif bir zaman önceliği değeri sağlayın. Bu özellik isteğe bağlıdır. | Timespan<br/>(hh:mm:ss) | Hayır  |

> [!NOTE]
> Yuvarlanan bir pencere tetikleyicisi en fazla iki diğer tetikleyiciye bağlı olabilir.

## <a name="tumbling-window-self-dependency-properties"></a>Yuvarlanan pencere kendine bağımlılık özellikleri

Tetikleyicinin önceki pencere başarıyla tamamlanana kadar sonraki pencereye geçmemesi gereken senaryolarda, kendine bağımlılık oluşturun. Önceki hr içinde kendi önceki çalıştırmalarının başarısına bağlı bir kendine bağımlılık tetikleyicisi aşağıdaki özelliklere sahip olacaktır:

```json
{
    "name": "DemoSelfDependency",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00Z",
            "delay": "00:01:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": "01:00:00",
                    "offset": "-01:00:00"
                }
            ]
        }
    }
}
```
## <a name="usage-scenarios-and-examples"></a>Kullanım senaryoları ve örnekleri

Aşağıda senaryoların çizimleri ve yuvarlanan pencere bağımlılığı özelliklerinin kullanımı verilmiştir.

### <a name="dependency-offset"></a>Bağımlılık mahsup

![Mahsup Örneği](media/tumbling-window-trigger-dependency/tumbling-window-dependency02.png "Mahsup Örneği")

### <a name="dependency-size"></a>Bağımlılık boyutu

![Boyut örneği](media/tumbling-window-trigger-dependency/tumbling-window-dependency03.png "Boyut örneği")

### <a name="self-dependency"></a>Kendine bağımlılık

![Kendine bağımlılık](media/tumbling-window-trigger-dependency/tumbling-window-dependency04.png "Kendine bağımlılık")

### <a name="dependency-on-another-tumbling-window-trigger"></a>Başka bir yuvarlanan pencere tetikleyicisine bağımlılık

Son yedi günlük çıktıyı bir araya alan ve yedi günlük haddeleme penceresi akışları oluşturan başka bir günlük işe bağlı olarak günlük bir telemetri işleme işi:

![Bağımlılık örneği](media/tumbling-window-trigger-dependency/tumbling-window-dependency05.png "Bağımlılık örneği")

### <a name="dependency-on-itself"></a>Kendine bağımlılık

İşin çıktı akışlarında boşluk olmayan günlük bir iş:

![Kendine bağımlılık örneği](media/tumbling-window-trigger-dependency/tumbling-window-dependency06.png "Kendine bağımlılık örneği")

Yuvarlanan pencere tetikleyicisini kullanarak Azure Veri Fabrikanızda bağımlı ardışık hatlar oluşturma hakkında bir gösteri için aşağıdaki videoyu izleyin:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Create-dependent-pipelines-in-your-Azure-Data-Factory/player]

## <a name="monitor-dependencies"></a>Bağımlılıkları izleme

Bağımlılık zincirini ve ilgili pencereleri tetikleyici çalıştırma izleme sayfasından izleyebilirsiniz. İzleme **> Tetikleyici Çalışır**gidin. Eylemler sütununaltında, tetikleyiciyi yeniden çalıştırabilir veya bağımlılıklarını görüntüleyebilirsiniz.

![Tetikleme çalıştırmalarını izleme](media/tumbling-window-trigger-dependency/tumbling-window-dependency07.png "Tetikleme çalıştırmalarını izleme")

'Tetikleyici Bağımlılıkları Görüntüle'yi tıklattığınızda, bağımlılıkların durumunu görebilirsiniz. Bağımlılık tetikleyicilerinden biri başarısız olursa, bağımlı tetikleyicinin çalışması için bunu başarıyla yeniden başlatmanız gerekir. Yuvarlanan bir pencere tetikleyicisi, zamanlamanın dolmadan önce bağımlılıkları yedi gün bekler.

![Bağımlılıkları izleme](media/tumbling-window-trigger-dependency/tumbling-window-dependency08.png "Bağımlılıkları izleme")

Tetikleyici bağımlılık zamanlamasını görüntülemek için daha görsel bir görünüm için Gantt görünümünü seçin.

![Bağımlılıkları izleme](media/tumbling-window-trigger-dependency/tumbling-window-dependency09.png "Bağımlılıkları izleme")

## <a name="next-steps"></a>Sonraki adımlar

* [Yuvarlanan pencere tetikleyicisi oluşturma yı](how-to-create-tumbling-window-trigger.md) kalım
