---
title: Azure Veri Fabrikası'nda yuvarlanan pencere tetikleyicileri oluşturma
description: Azure Veri Fabrikası'nda, yuvarlanan bir pencerede bir ardışık pencereyi çalıştıran bir tetikleyiciyi nasıl oluşturabilirsiniz öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 09/11/2019
ms.openlocfilehash: 97c8f8a5bb2111264e9459a7d2128c1ab7c2503d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81414424"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Atlayan pencerede işlem hattı çalıştıran bir tetikleyici oluşturma
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, yuvarlanan bir pencere tetikleyicioluşturmak, başlatmak ve izlemek için adımlar sağlar. Tetikleyiciler ve desteklenen türler hakkında genel bilgi için [Bkz.](concepts-pipeline-execution-triggers.md)

Atlayan pencere tetikleyicileri, durumu korurken belirtilen bir başlangıç zamanından itibaren periyodik bir zaman aralığında başlatılan bir tetikleyici türüdür. Atlayan pencereler sabit boyutlu, çakışmayan ve bitişik zaman aralıkları dizisidir. Yuvarlanan pencere tetikleyicisi, bir ardışık pencere ile bire bir ilişkiye sahiptir ve yalnızca tekil bir ardışık pencere hattına başvurur.

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

1. Veri Fabrikası UI'sinde yuvarlanan bir pencere tetikleyicisi oluşturmak için **Tetikleyiciler** sekmesini seçin ve ardından **Yeni'yi**seçin. 
1. Tetikleyici yapılandırma bölmesi açıldıktan **sonra, Yuvarlanan Pencere'yi**seçin ve ardından yuvarlanan pencere tetikleme özelliklerini tanımlayın. 
1. İşiniz bittiğinde **Kaydet**'i seçin.

![Azure portalında yuvarlanan pencere tetikleyicisi oluşturma](media/how-to-create-tumbling-window-trigger/create-tumbling-window-trigger.png)

## <a name="tumbling-window-trigger-type-properties"></a>Yuvarlanan pencere tetik leme türü özellikleri

Yuvarlanan pencerede aşağıdaki tetikleme türü özellikleri vardır:

```
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": <<Minute/Hour>>,
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime: <<datetime – optional>>,
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
        },
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipelineName"
            },
            "parameters": {
                "parameter1": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "parameter2": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
            }
        }
    }
}
```

Aşağıdaki tablo, yuvarlanan bir pencere tetikleyicisinin tekrarı ve zamanlaması ile ilgili ana JSON öğelerinin üst düzey bir genel görünümünü sağlar:

| JSON öğesi | Açıklama | Tür | İzin verilen değerler | Gerekli |
|:--- |:--- |:--- |:--- |:--- |
| **Türü** | Tetikleyicinin türü. Tür sabit değer "TumblingWindowTrigger"tır. | Dize | "Yuvarlanan Pencere Tetikleme" | Evet |
| **runtimeState** | Tetikleyici çalışma zamanının geçerli durumu.<br/>**Not**: Bu \<öğe sadece> okunur. | Dize | "Başladı", "Durduruldu", "Devre Dışı" | Evet |
| **Frekans** | Tetikleyicinin yinelendiği frekans birimini (dakika veya saat) temsil eden dize. Başlangıç **Saati** tarih değerleri **sıklık** değerinden daha ayrıntılıysa, pencere sınırları hesaplandığında **başlangıç Saati** tarihleri dikkate alınır. Örneğin, **frekans** değeri saatlikse ve **başlangıç Saati** değeri 2017-09-01T10:10:10Z ise, ilk pencere (2017-09-01T10:10:10Z, 2017-09-01T11:10:10Z) olur. | Dize | "dakika", "saat"  | Evet |
| **Aralığı** | Tetikleyicinin çalışma sıklığını belirten **frequency** değerinin aralığını gösteren bir pozitif tamsayı. Örneğin, **aralık** 3 ve **sıklık** "saat" ise, tetikleyici her 3 saatte bir yinelenir. <br/>**Not**: Minimum pencere aralığı 5 dakikadır. | Tamsayı | Pozitif bir tamsayı. | Evet |
| **startTime**| Geçmişte olabilir ilk olay. İlk tetikleme aralığı (**startTime**, **startTime** + **aralığıdır).** | DateTime | DateTime değeri. | Evet |
| **endTime**| Geçmişte olabilir son olay. | DateTime | DateTime değeri. | Evet |
| **Gecikme** | Pencere için veri işlemenin başlatılmasını geciktirmek için gereken süre. Ardışık hatlar, beklenen yürütme süresi artı **gecikme**miktarından sonra başlatılır. **Gecikme,** tetikleyicinin yeni bir çalıştırmayı tetiklemeden önce son zamanı ne kadar beklediğini tanımlar. **Gecikme** pencere **başlangıç Zamanını**değiştirmez. Örneğin, 00:10:00 **gecikme** değeri 10 dakikalık bir gecikme anlamına gelir. | Timespan<br/>(hh:mm:ss)  | Varsayılan değerin 00:00:00 olduğu bir zaman önce değeri. | Hayır |
| **maxConcurrency** | Hazır pencereler için çalıştırılan eşzamanlı tetikleme çalıştırmalarının sayısı. Örneğin, dün için dolgu saatlik çalışır geri 24 pencere sonuçları. **MaxConcurrency** = 10 ise, tetikleme olayları yalnızca ilk 10 pencere için ateşlenir (00:00-01:00 - 09:00-10:00). Tetiklenen ilk 10 boru hattı hattı çalıştırıldıktan sonra, sonraki 10 pencere için tetikleme çalıştırmaları çalıştırılır (10:00-11:00 - 19:00-20:00). **MaxConcurrency** = 10 bu örnek ile devam, 10 pencere hazır ise, 10 toplam boru hattı çalışır vardır. Eğer sadece 1 pencere hazırsa, sadece 1 boru hattı çalışır. | Tamsayı | 1 ile 50 arasında bir sonda. | Evet |
| **retryPolicy: Sayma** | Ardışık hatlar çalıştırılmadan önceki yeniden deneme sayısı "Başarısız" olarak işaretlenir.  | Tamsayı | Varsayılan değerin 0 olduğu bir sonseda (yeniden deneme yok). | Hayır |
| **retryPolicy: intervalInSeconds** | Saniyeler içinde belirtilen yeniden deneme denemeleri arasındaki gecikme. | Tamsayı | Varsayılan ın 30 olduğu saniye sayısı. | Hayır |
| **dependsOn: türü** | TumblingWindowTriggerReference türü. Bağımlılık ayarlanmışsa gereklidir. | Dize |  "TumblingWindowTriggerDependencyReference", "SelfDependencyTumblingWindowTriggerReference" | Hayır |
| **dependsOn: boyutu** | Bağımlılık yuvarlanan pencerenin boyutu. | Timespan<br/>(hh:mm:ss)  | Varsayılan değerin alt tetikleyicinin pencere boyutu olduğu pozitif bir zaman dilimi değeri  | Hayır |
| **dependsOn: ofset** | Bağımlılık tetikleyicisinin mahsup. | Timespan<br/>(hh:mm:ss) |  Kendine bağımlılıkta negatif olması gereken bir zaman alanı değeri. Değer belirtilmemişse, pencere tetikleyicinin kendisiyle aynıdır. | Kendine Bağımlılık: Evet<br/>Diğer: Hayır  |

### <a name="windowstart-and-windowend-system-variables"></a>WindowStart ve WindowEnd sistem değişkenleri

**Pencere hattı** tanımınızda (diğer bir deyişle, sorgunun bir parçası için) yuvarlanan pencere tetikleyicisinin **WindowStart** ve **WindowEnd** sistem değişkenlerini kullanabilirsiniz. Sistem değişkenlerini **tetikleme** tanımında parametre olarak boru hattınıza aktarın. Aşağıdaki örnekte, bu değişkenlerin parametreler olarak nasıl geçirilmeniz gerektiğini gösterilmektedir:

```
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline": {
            "pipelineReference": {
                "type": "PipelineReference",
                "referenceName": "MyPipelineName"
            },
            "parameters": {
                "MyWindowStart": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                },
                "MyWindowEnd": {
                    "type": "Expression",
                    "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                }
            }
        }
    }
}
```

Pipeline tanımındaki **WindowStart** ve **WindowEnd** sistem değişken değerlerini kullanmak için buna göre "MyWindowStart" ve "MyWindowEnd" parametrelerinizi kullanın.

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>Geri doldurma senaryosunda pencerelerin yürütme sırası
Yürütme için birden çok pencere olduğunda (özellikle bir yedek doldurma senaryosunda), windows için yürütme sırası en eskiden en yeni aralıklara kadar belirleyicidir. Şu anda bu davranışın değiştirilmesi mümkün değildir.

### <a name="existing-triggerresource-elements"></a>Varolan TriggerResource öğeleri
Aşağıdaki noktalar varolan **TriggerResource** öğeleri için geçerlidir:

* Tetikleyicinin **frekans** öğesi (veya pencere boyutu) değeri değişirse, zaten işlenmiş olan pencerelerin durumu *sıfırlanmaz.* Tetikleyici, yeni pencere boyutunu kullanarak çalıştırdığı son pencereden pencereleriçin ateş etmeye devam ediyor.
* Tetikleyicinin bitiş **Zamanı** öğesinin değeri değişirse (eklenmiş veya güncelleştirilmişse), zaten işlenmiş olan pencerelerin durumu *sıfırlanmaz.* Tetikleyici, yeni **bitiş zamanı** değerini onurlandırıyor. Yeni **endTime** değeri zaten yürütülen pencerelerden önceyse, tetikleyici durur. Aksi takdirde, yeni **endTime** değeri ne zaman tetikleyici durur.

### <a name="tumbling-window-trigger-dependency"></a>Yuvarlanan pencere bağımlılık tetikleme

Yuvarlanan bir pencere tetikleyicisinin yalnızca veri fabrikasında başka bir yuvarlanan pencere tetikleyicisinin başarılı bir şekilde yürütülmesinden sonra yürütüleceğinden emin olmak istiyorsanız, [yuvarlanan bir pencere tetikleyicisi bağımlılık oluşturur.](tumbling-window-trigger-dependency.md) 

## <a name="sample-for-azure-powershell"></a>Azure PowerShell için örnek

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu bölümde, bir tetikleyici oluşturmak, başlatmak ve izlemek için Azure PowerShell'i nasıl kullanacağınızı gösterir.

1. C:\ADFv2QuickStartPSH\ klasöründe Aşağıdaki içeriği içeren **MyTrigger.json** adlı bir JSON dosyası oluşturun:

    > [!IMPORTANT]
    > JSON dosyasını kaydetmeden **önce, başlangıç Zamanı** öğesinin değerini geçerli UTC saatine ayarlayın. **EndTime** öğesinin değerini geçerli UTC saatini bir saat geçmiş olarak ayarlayın.

    ```json
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```

2. **Set-AzDataFactoryV2Trigger** cmdlet'i kullanarak bir tetikleyici oluşturun:

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
    
3. **Tetikleyicinin durumunuget-AzDataFactoryV2Trigger** cmdlet kullanarak **durdurulduğunu** doğrulayın:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. **Start-AzDataFactoryV2Trigger** cmdlet kullanarak tetikleyiciyi başlatın:

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Tetikleyicinin **durumunuget-AzDataFactoryV2Trigger** cmdlet kullanarak **başlatıldığından** onaylayın:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6. **Get-AzDataFactoryV2TriggerRun** cmdlet'ini kullanarak Azure PowerShell'de tetikleyici çalıştırmaları alın. Tetikleyici çalıştırmaları hakkında bilgi almak için aşağıdaki komutu düzenli aralıklarla uygulayın. **TriggerRunStartedAfter** ve **TriggerRunStartedBefore** değerlerini tetikleyici tanımınızdaki değerlerle eşleştirmek için güncelleştirin:

    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
Azure portalındaki tetikleyici çalıştırmaları ve ardışık hatlar denetimlerini izlemek için [bkz.](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="next-steps"></a>Sonraki adımlar

* Tetikleyiciler hakkında ayrıntılı bilgi [için, Bkz. Boru Hattı yürütme ve tetikleyiciler.](concepts-pipeline-execution-triggers.md#trigger-execution)
* [Atlayan pencere tetikleyici bağımlılığı oluşturma](tumbling-window-trigger-dependency.md)
