---
title: Azure Data Factory içinde atlayan pencere Tetikleyicileri oluşturma
description: Bir ardışık düzen penceresinde bir işlem hattı çalıştıran Azure Data Factory tetikleyici oluşturmayı öğrenin.
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
ms.openlocfilehash: 964190108bb53a349fa1cb1301e2a554c1e32b26
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83996695"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Atlayan pencerede işlem hattı çalıştıran bir tetikleyici oluşturma
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, bir atlayan pencere tetikleyicisi oluşturma, başlatma ve izlemeye yönelik adımlar sağlanmaktadır. Tetikleyiciler ve desteklenen türler hakkında genel bilgi için bkz. işlem [hattı yürütme ve Tetikleyiciler](concepts-pipeline-execution-triggers.md).

Atlayan pencere tetikleyicileri, durumu korurken belirtilen bir başlangıç zamanından itibaren periyodik bir zaman aralığında başlatılan bir tetikleyici türüdür. Atlayan pencereler sabit boyutlu, çakışmayan ve bitişik zaman aralıkları dizisidir. Atlayan pencere tetikleyicisinin bir işlem hattı ile bire bir ilişkisi vardır ve yalnızca tekil bir işlem hattına başvurabilir. Atlayan pencere tetikleyicisi, zamanlama tetikleyicisinin karmaşık senaryolara yönelik bir özellik paketi ([diğer pencere tetikleyicilerine bağımlılığı](#tumbling-window-trigger-dependency), [başarısız bir işi yeniden çalıştırma ve işlem](tumbling-window-trigger-dependency.md#monitor-dependencies) [hatları için Kullanıcı yeniden denemesi ayarlama](#user-assigned-retries-of-pipelines)) için daha ağır bir alternatiftir. Zamanlama tetikleyicisi ve atlayan pencere tetikleyicisi arasındaki farkı daha fazla anlamak için lütfen [buraya](concepts-pipeline-execution-triggers.md#trigger-type-comparison)göz atın.

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)

1. Data Factory Kullanıcı arabiriminde bir atlayan pencere tetikleyicisi oluşturmak için **Tetikleyiciler** sekmesini seçin ve ardından **Yeni**' yi seçin. 
1. Tetikleyici yapılandırma bölmesi açıldıktan sonra, atlayan **pencere penceresi**' ni seçin ve ardından, çıkış penceresi tetikleyicisi özelliklerini tanımlayın. 
1. İşiniz bittiğinde **Kaydet**'i seçin.

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

Aşağıdaki tabloda, atlayan bir pencere tetikleyicisinin yinelenme ve zamanlama ile ilgili büyük JSON öğelerine yönelik üst düzey bir genel bakış sunulmaktadır:

| JSON öğesi | Açıklama | Tür | İzin verilen değerler | Gerekli |
|:--- |:--- |:--- |:--- |:--- |
| **türüyle** | Tetikleyicinin türü. Tür, "TumblingWindowTrigger" sabit değeridir. | Dize | "TumblingWindowTrigger" | Evet |
| **runtimeState** | Tetikleyici çalışma zamanının geçerli durumu.<br/>**Note**: Bu öğe \<readOnly> . | Dize | "Başlatıldı," "durduruldu," "devre dışı" | Evet |
| **lemiyor** | Tetikleyicinin yineleneceği sıklık birimini (dakika veya saat) temsil eden bir dize. **StartTime** tarih değerleri **Sıklık** değerinden daha ayrıntılı ise, pencere sınırları hesaplandıktan sonra **StartTime** tarihleri kabul edilir. Örneğin **Sıklık** değeri saat Ise ve **StartTime** değeri 2017-09-01T10:10:10z ise, ilk pencere (2017-09-01T10:10:10z, 2017-09-01T11:10:10z). | Dize | "dakika", "saat"  | Evet |
| **aralığında** | Tetikleyicinin çalışma sıklığını belirten **frequency** değerinin aralığını gösteren bir pozitif tamsayı. Örneğin, **Aralık** 3, **Sıklık** ise "saat" ise, tetikleyici her 3 saatte bir yinelenir. <br/>**Note**: en düşük pencere aralığı 5 dakikadır. | Tamsayı | Pozitif bir tamsayı. | Evet |
| **startTime**| Geçmişte olabilecek ilk oluşum. İlk tetikleyici aralığı (**başlangıçsaati**, **başlangıçsaati**  +  **aralığı**). | DateTime | Bir tarih saat değeri. | Evet |
| **endTime**| Geçmişte olabilecek son oluşum. | DateTime | Bir tarih saat değeri. | Evet |
| **ilir** | Pencere için veri işleme başlangıcını geciktirmek için geçen süre. İşlem hattı çalıştırması beklenen yürütme süresi artı **gecikme**miktarı ile başlatılır. **Gecikme** , tetikleyicinin yeni bir çalıştırmayı tetiklemeden önce geçen süreyi ne kadar bekleyeceğini tanımlar. **Gecikme** , pencerenin **StartTime**öğesini değiştirmez. Örneğin, 00:10:00 **gecikme** değeri 10 dakikalık bir gecikme anlamına gelir. | Timespan<br/>(SS: DD: SS)  | Varsayılan değer 00:00:00 olan bir TimeSpan değeri. | Hayır |
| **maxConcurrency** | Kullanılabilir olan Windows için tetiklenen eşzamanlı tetikleyici çalışmalarının sayısı. Örneğin, saat başı için saatlik çalıştırmaları, 24 Windows ile sonuçlarınıza geri dönmek için. **MaxConcurrency** = 10 ise, tetikleyici olayları yalnızca ilk 10 pencere için tetiklenir (00:00-01:00-09:00-10:00). İlk 10 tetiklenen işlem hattı çalıştıktan sonra, sonraki 10 Windows için tetikleyici çalıştırmaları tetiklenir (10:00-11:00-19:00-20:00). **MaxConcurrency** = 10 ' un bu örneğine devam ederseniz, 10 Windows varsa, toplam 10 işlem hattı çalıştırması vardır. Yalnızca 1 pencere hazırlandıysanız yalnızca 1 işlem hattı çalıştırması vardır. | Tamsayı | 1 ile 50 arasında bir tamsayı. | Evet |
| **retryPolicy: Count** | İşlem hattı çalıştırılmadan önceki yeniden deneme sayısı "başarısız" olarak işaretlenmemiştir.  | Tamsayı | Varsayılan değer 0 olan (yeniden deneme yok) bir tamsayı. | Hayır |
| **retryPolicy: ıntervalınseconds** | Saniyeler içinde belirtilen yeniden deneme girişimleri arasındaki gecikme. | Tamsayı | Saniye sayısı, varsayılan değer 30 ' dur. | Hayır |
| **Bağımlıdson: tür** | TumblingWindowTriggerReference türü. Bir bağımlılık ayarlandıysa gereklidir. | Dize |  "TumblingWindowTriggerDependencyReference", "SelfDependencyTumblingWindowTriggerReference" | Hayır |
| **Bağımlıdson: boyut** | Bağımlılık penceresinin boyutu. | Timespan<br/>(SS: DD: SS)  | Varsayılan değeri, alt tetikleyicisinin pencere boyutu olan pozitif bir TimeSpan değeri  | Hayır |
| **Bağımlıdson: konum** | Bağımlılık tetikleyicisinin boşluğu. | Timespan<br/>(SS: DD: SS) |  Kendinden bağımlılıkta negatif olması gereken bir TimeSpan değeri. Değer belirtilmemişse pencere, tetikleyiciyle aynı olur. | Kendinden bağımlılık: Evet<br/>Diğer: Hayır  |

> [!NOTE]
> Atlayan bir pencere tetikleyicisi yayımlandıktan sonra **Aralık** ve **Sıklık** düzenlenemez.

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

Tetikleyicinin startTime öğesi geçmişte ise, bu formüle dayalı olarak, z = (CurrentTime-TriggerStartTime)/Triggerdilimleyicesize, tetikleyici, gelecekteki çalıştırmaları yürütmeden önce {z} geri doldurma (geçmiş) ile paralel olarak çalışır. Windows için yürütme sırası, en eskiden en yeni aralıklarda belirleyici olur. Şu anda bu davranışın değiştirilmesi mümkün değildir.

### <a name="existing-triggerresource-elements"></a>Mevcut TriggerResource öğeleri

Aşağıdaki noktaları var olan **Triggerresource** öğelerinin güncelleştirilmesi için geçerlidir:

* Tetikleyici oluşturulduktan sonra Trigger öğesinin **Sıklık** öğesi (veya pencere boyutu) değeri, **Aralık** öğesiyle birlikte değiştirilemez. Bu, triggerRun yeniden yönlendirme ve bağımlılık değerlendirmelerinin düzgün çalışması için gereklidir
* Tetikleyicinin **bitişi** öğesi için değer değişirse (eklenmiş veya güncelleştirilmiş), zaten işlenmiş olan pencerelerin *durumu sıfırlanmaz.* Tetikleyici, yeni **bitişsaati** değerini verir. Yeni bir **bitişsaati** değeri zaten yürütülmüş olan pencerelerin önünde ise, tetikleyici duraklar. Aksi takdirde tetikleyici, yeni **bitişsaati** değerine rastlana kadar duraklar.

### <a name="user-assigned-retries-of-pipelines"></a>Kullanıcı ardışık düzenleri yeniden denemeye atandı

Ardışık düzen hatalarında, iç içe geçmiş pencere tetikleyicisi, Kullanıcı müdahalesi olmadan aynı giriş parametrelerini kullanarak başvurulan işlem hattının yürütülmesini otomatik olarak yeniden deneyebilir. Bu, tetikleyici tanımında "retryPolicy" özelliği kullanılarak belirtilebilir.

### <a name="tumbling-window-trigger-dependency"></a>Atlayan pencere tetikleme bağımlılığı

Bir atlayan pencere tetikleyicisinin yalnızca, veri fabrikasında başka bir dönüştürme penceresi tetikleyicisi başarıyla yürütüldükten sonra yürütüldüğünden emin olmak istiyorsanız, atlayan [bir pencere tetikleme bağımlılığı oluşturun](tumbling-window-trigger-dependency.md). 

## <a name="sample-for-azure-powershell"></a>Azure PowerShell için örnek

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu bölümde, bir tetikleyiciyi oluşturmak, başlatmak ve izlemek için Azure PowerShell nasıl kullanılacağı gösterilmektedir.

1. C:\ADFv2QuickStartPSH\ klasöründe aşağıdaki içeriğe sahip **MyTrigger.js** ADLı bir JSON dosyası oluşturun:

    > [!IMPORTANT]
    > JSON dosyasını kaydetmeden önce, **StartTime** öğesinin DEĞERINI geçerli UTC saatine ayarlayın. **BitişZamanı** öğesinin DEĞERINI geçerli UTC saatinden bir saat olarak ayarlayın.

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

* Tetikleyiciler hakkında ayrıntılı bilgi için bkz. işlem [hattı yürütme ve Tetikleyiciler](concepts-pipeline-execution-triggers.md#trigger-execution).
* [Atlayan pencere tetikleyici bağımlılığı oluşturma](tumbling-window-trigger-dependency.md)
