---
title: Azure Data Factory 'de atlayan pencere Tetikleyicileri oluşturma | Microsoft Docs
description: Bir ardışık düzen penceresinde bir işlem hattı çalıştıran Azure Data Factory tetikleyici oluşturmayı öğrenin.
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 12/14/2018
ms.author: shlo
ms.openlocfilehash: 0f78136edf58e76ed478bef9c255791d256c34a5
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2019
ms.locfileid: "68678473"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Atlayan bir pencerede işlem hattı çalıştıran bir tetikleyici oluşturma
Bu makalede, bir atlayan pencere tetikleyicisi oluşturma, başlatma ve izlemeye yönelik adımlar sağlanmaktadır. Tetikleyiciler ve desteklenen türler hakkında genel bilgi için bkz. işlem [hattı yürütme ve Tetikleyiciler](concepts-pipeline-execution-triggers.md).

Atlayan pencere tetikleyicileri, durumu korurken belirtilen bir başlangıç zamanından itibaren periyodik bir zaman aralığında başlatılan bir tetikleyici türüdür. Atlayan pencereler sabit boyutlu, çakışmayan ve bitişik zaman aralıkları dizisidir. Atlayan pencere tetikleyicisinin bir işlem hattı ile bire bir ilişkisi vardır ve yalnızca tekil bir işlem hattına başvurabilir.

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

Azure portal bir pencere tetikleyicisi oluşturmak için, bir **sonraki pencere > > tetikleme**' yı seçin ve ardından, atlayan pencereyi tanımlayan özellikleri yapılandırın.

![Azure portal bir atlayan pencere tetikleyicisi oluşturun](media/how-to-create-tumbling-window-trigger/create-tumbling-window-trigger.png)

## <a name="tumbling-window-trigger-type-properties"></a>Atlayan pencere tetikleyicisi tür özellikleri
Atlayan bir pencere aşağıdaki tetikleyici türü özelliklerine sahiptir:

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
            “maxConcurrency”: <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count": <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
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

Aşağıdaki tabloda, atlayan bir pencere tetikleyicisinin yinelenme ve zamanlama ile ilgili büyük JSON öğelerine yönelik üst düzey bir genel bakış sunulmaktadır:

| JSON öğesi | Açıklama | Type | İzin verilen değerler | Gerekli |
|:--- |:--- |:--- |:--- |:--- |
| **type** | Tetikleyicinin türü. Tür, "TumblingWindowTrigger" sabit değeridir. | Dize | "TumblingWindowTrigger" | Evet |
| **runtimeState** | Tetikleyici çalışma zamanının geçerli durumu.<br/>**Not**: Bu öğe \<ReadOnly >. | Dize | "Başlatıldı," "durduruldu," "devre dışı" | Evet |
| **frequency** | Tetikleyicinin yineleneceği sıklık birimini (dakika veya saat) temsil eden bir dize. **StartTime** tarih değerleri **Sıklık** değerinden daha ayrıntılı ise, pencere sınırları hesaplandıktan sonra **StartTime** tarihleri kabul edilir. Örneğin **Sıklık** değeri saat Ise ve **StartTime** değeri 2017-09-01T10:10:10z ise, ilk pencere (2017-09-01T10:10:10z, 2017-09-01T11:10:10z). | Dize | "dakika", "saat"  | Evet |
| **interval** | Tetikleyicinin çalışma sıklığını belirten **frequency** değerinin aralığını gösteren bir pozitif tamsayı. Örneğin, **Aralık** 3, **Sıklık** ise "saat" ise, tetikleyici her 3 saatte bir yinelenir. | Tamsayı | Pozitif bir tamsayı. | Evet |
| **startTime**| Geçmişte olabilecek ilk oluşum. İlk tetikleyici aralığı (**başlangıçsaati**, **başlangıçsaati** + **aralığı**). | Datetime | Bir tarih saat değeri. | Evet |
| **endTime**| Geçmişte olabilecek son oluşum. | Datetime | Bir tarih saat değeri. | Evet |
| **ilir** | Pencere için veri işleme başlangıcını geciktirmek için geçen süre. İşlem hattı çalıştırması beklenen yürütme süresi artı **gecikme**miktarı ile başlatılır. **Gecikme** , tetikleyicinin yeni bir çalıştırmayı tetiklemeden önce geçen süreyi ne kadar bekleyeceğini tanımlar. **Gecikme** , pencerenin **StartTime**öğesini değiştirmez. Örneğin, 00:10:00 **gecikme** değeri 10 dakikalık bir gecikme anlamına gelir. | Zaman aralığı<br/>(SS: DD: SS)  | Varsayılan değer 00:00:00 olan bir TimeSpan değeri. | Hayır |
| **maxConcurrency** | Kullanılabilir olan Windows için tetiklenen eşzamanlı tetikleyici çalışmalarının sayısı. Örneğin, saat başı için saatlik çalıştırmaları, 24 Windows ile sonuçlarınıza geri dönmek için. **MaxConcurrency** = 10 ise, tetikleyici olayları yalnızca ilk 10 pencere için tetiklenir (00:00-01:00-09:00-10:00). İlk 10 tetiklenen işlem hattı çalıştıktan sonra, sonraki 10 Windows için tetikleyici çalıştırmaları tetiklenir (10:00-11:00-19:00-20:00). **MaxConcurrency** = 10 ' un bu örneğine devam ederseniz, 10 Windows varsa, toplam 10 işlem hattı çalıştırması vardır. Yalnızca 1 pencere hazırlandıysanız yalnızca 1 işlem hattı çalıştırması vardır. | Tamsayı | 1 ile 50 arasında bir tamsayı. | Evet |
| **RetryPolicy Count** | İşlem hattı çalıştırılmadan önceki yeniden deneme sayısı "başarısız" olarak işaretlenmemiştir.  | Tamsayı | Varsayılan değer 0 olan (yeniden deneme yok) bir tamsayı. | Hayır |
| **retryPolicy: intervalInSeconds** | Saniyeler içinde belirtilen yeniden deneme girişimleri arasındaki gecikme. | Tamsayı | Saniye sayısı, varsayılan değer 30 ' dur. | Hayır |
| **Bağımlıdson: tür** | TumblingWindowTriggerReference türü. Bir bağımlılık ayarlandıysa gereklidir. | Dize |  "TumblingWindowTriggerDependencyReference", "SelfDependencyTumblingWindowTriggerReference" | Hayır |
| **Bağımlıdson: boyut** | Bağımlılık penceresinin boyutu. | Zaman aralığı<br/>(SS: DD: SS)  | Varsayılan değeri, alt tetikleyicisinin pencere boyutu olan pozitif bir TimeSpan değeri  | Hayır |
| **Bağımlıdson: konum** | Bağımlılık tetikleyicisinin boşluğu. | Zaman aralığı<br/>(SS: DD: SS) |  Kendinden bağımlılıkta negatif olması gereken bir TimeSpan değeri. Değer belirtilmemişse pencere, tetikleyiciyle aynı olur. | Kendinden bağımlılık: Evet<br/>Diğer: Hayır  |

### <a name="windowstart-and-windowend-system-variables"></a>WindowStart ve WindowEnd sistem değişkenleri

İşlem **hattı** tanımınızda (bir sorgunun parçası olarak), dönerek pencere tetikleyicisinin **Windowstart** ve **windowend** sistem değişkenlerini kullanabilirsiniz. Sistem değişkenlerini **tetikleyici** tanımında işlem hattınıza parametre olarak geçirin. Aşağıdaki örnekte, bu değişkenlerin parametre olarak nasıl geçirileceğini gösterilmektedir:

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

İşlem hattı tanımında **Windowstart** ve **windowend** sistem değişkeni değerlerini kullanmak için, buna uygun olarak "mywindowstart" ve "mywindowend" parametrelerinizi kullanın.

### <a name="execution-order-of-windows-in-a-backfill-scenario"></a>Arka doldurma senaryosunda Windows 'un yürütme sırası
Yürütme için birden çok pencere (özellikle bir geri doldurma senaryosunda) olduğunda, Windows için yürütme sırası en eskiyi en eskiye ve en yeni aralıklara göre belirleyici olur. Şu anda bu davranışın değiştirilmesi mümkün değildir.

### <a name="existing-triggerresource-elements"></a>Mevcut TriggerResource öğeleri
Aşağıdaki noktaları var olan **Triggerresource** öğeleri için geçerlidir:

* Tetikleyicinin **Sıklık** öğesi (veya pencere boyutu) değeri değişirse, zaten işlenmiş olan pencerelerin *durumu sıfırlanmaz.* Tetikleyici, yeni pencere boyutunu kullanarak yürütüldüğü son pencereden Windows için çalışmaya devam eder.
* Tetikleyicinin bitişi öğesi için değer değişirse (eklenmiş veya güncelleştirilmiş), zaten işlenmiş olan pencerelerin *durumu sıfırlanmaz.* Tetikleyici, yeni **bitişsaati** değerini verir. Yeni bir **bitişsaati** değeri zaten yürütülmüş olan pencerelerin önünde ise, tetikleyici duraklar. Aksi takdirde tetikleyici, yeni **bitişsaati** değerine rastlana kadar duraklar.

### <a name="tumbling-window-trigger-dependency"></a>Atlayan pencere tetikleme bağımlılığı

Bir atlayan pencere tetikleyicisinin yalnızca, veri fabrikasında başka bir dönüştürme penceresi tetikleyicisi başarıyla yürütüldükten sonra yürütüldüğünden emin olmak istiyorsanız, atlayan [bir pencere tetikleme bağımlılığı oluşturun](tumbling-window-trigger-dependency.md). 

## <a name="sample-for-azure-powershell"></a>Azure PowerShell için örnek

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu bölümde, bir tetikleyiciyi oluşturmak, başlatmak ve izlemek için Azure PowerShell nasıl kullanılacağı gösterilmektedir.

1. C:\ADFv2QuickStartPSH\ klasöründe aşağıdaki içeriğe sahip **Mytrigger. JSON** ADLı bir JSON dosyası oluşturun:

    > [!IMPORTANT]
    > JSON dosyasını kaydetmeden önce, **StartTime** öğesinin DEĞERINI geçerli UTC saatine ayarlayın. BitişZamanı öğesinin değerini geçerli UTC saatinden bir saat olarak ayarlayın.

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

2. **Set-AzDataFactoryV2Trigger** cmdlet 'ini kullanarak bir tetikleyici oluşturun:

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```
    
3. Tetikleyici durumunun **Get-AzDataFactoryV2Trigger** cmdlet 'ı kullanılarak **durdurulduğunu** doğrulayın:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

4. **Start-AzDataFactoryV2Trigger** cmdlet 'ini kullanarak tetikleyiciyi başlatın:

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

5. Tetikleyici durumunun **Get-AzDataFactoryV2Trigger** cmdlet 'ı kullanılarak **başlatıldığını** onaylayın:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

6. **Get-AzDataFactoryV2TriggerRun** cmdlet 'ini kullanarak Azure PowerShell tetikleyici çalıştırmalarını alın. Tetikleyici çalıştırmaları hakkında bilgi almak için aşağıdaki komutu düzenli aralıklarla yürütün. **Triggerrunstartedadfter** ve **Triggerrunstartedbefore** değerlerini tetikleyici tanımınızdaki değerlerle eşleşecek şekilde güncelleştirin:

    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
Tetikleyici çalıştırmalarını izlemek ve Azure portal işlem hattı çalıştırmalarını izlemek için bkz. işlem [hattı çalıştırmalarını izleme](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline).

## <a name="next-steps"></a>Sonraki adımlar

* Tetikleyiciler hakkında ayrıntılı bilgi için bkz. işlem [hattı yürütme ve Tetikleyiciler](concepts-pipeline-execution-triggers.md#triggers).
* [Atlayan pencere tetikleme bağımlılığı oluşturma](tumbling-window-trigger-dependency.md)