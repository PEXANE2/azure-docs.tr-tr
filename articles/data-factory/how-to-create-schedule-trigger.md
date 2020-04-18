---
title: Azure Veri Fabrikası'nda zamanlama tetikleyicileri oluşturma
description: Azure Veri Fabrikası'nda bir ardışık ardışık ardışık ardışık projeyi çalıştıran bir tetikleyiciyi nasıl oluşturabilirsiniz öğrenin.
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/23/2018
ms.openlocfilehash: a0a01dad5ae86cf20d57ade845326838f8fd686a
ms.sourcegitcommit: d791f8f3261f7019220dd4c2dbd3e9b5a5f0ceaf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2020
ms.locfileid: "81641460"
---
# <a name="create-a-trigger-that-runs-a-pipeline-on-a-schedule"></a>Zamanlamada ardışık hatlar hattını çalıştıran bir tetikleyici oluşturma
[!INCLUDE[appliesto-adf-asa-md](includes/appliesto-adf-asa-md.md)]

Bu makalede, zamanlama tetikleyicisi ve zamanlama tetikleyicisi oluşturma, başlatma ve izleme adımları hakkında bilgi verilmektedir. Diğer tetikleyici türleri [için, Bkz. Boru Hattı yürütme ve tetikleyiciler.](concepts-pipeline-execution-triggers.md)

Zamanlama tetikleyicisi oluştururken, tetikleyici için bir zamanlama (başlangıç tarihi, yineleme, bitiş tarihi vb.) belirtir ve bir ardışık sözcülükle ilişkilendirin. İşlem hatları ve tetikleyiciler çoka çok ilişkisine sahiptir. Birden çok tetikleyici tek bir işlem hattını başlatabilir. Tek bir tetikleyici birden fazla işlem hattını başlatabilir.

Aşağıdaki bölümlerde farklı şekillerde bir zamanlama tetikleyici oluşturmak için adımlar sağlar. 

## <a name="data-factory-ui"></a>Data Factory Kullanıcı Arabirimi (UI)
Düzenli aralıklarla çalışacak bir ardışık hatlar (saatlik, günlük vb.) zamanlamak için bir **zamanlama tetikleyicisi** oluşturabilirsiniz. 

> [!NOTE]
> Bir ardışık hatlar ve bir zamanlama tetikleyicisi oluşturma nın tam bir walkthrough için, hangi boru hattı ile tetikleyici ilişkilendirir ve çalışır ve boru hattı izler, [Bkz. Quickstart: Veri Fabrikası Kullanıcı Arabirimi kullanarak bir veri fabrikası oluşturun.](quickstart-create-data-factory-portal.md)

1. Kalem simgesiyle gösterilen **Edit** sekmesine geçin. 

    ![Düzen sekmesine geçin](./media/how-to-create-schedule-trigger/switch-edit-tab.png)

1. Menüde **Tetikleyici'yi** seçin, ardından **Yeni/Edit'i**seçin. 

    ![Yeni tetikleyici menüsü](./media/how-to-create-schedule-trigger/new-trigger-menu.png)

1. Tetikleyici **ekle** sayfasında **tetikleyiciyi seç'i seçin...** seçeneğini belirleyin, ardından **+Yeni'yi**seçin. 

    ![Tetikleyici ekle - yeni tetikleyici](./media/how-to-create-schedule-trigger/add-trigger-new-button.png)

1. Yeni **Tetikleyici** sayfasında aşağıdaki adımları yapın: 

    1. **Zamanlama'nın** **Tür**için seçildiğini doğrulayın. 
    1. **Başlangıç Tarihi (UTC)** için tetikleyicinin başlangıç tarih saatini belirtin. Varsayılan olarak geçerli datetime olarak ayarlanır. 
    1. Tetikleyici için **Yineleme** belirtin. Açılan listeden (Her dakika, Saatlik, Günlük, Haftalık ve Aylık) değerlerden birini seçin. Metin kutusuna çarpanı girin. Örneğin, tetikleyicinin her 15 dakikada bir çalışmasını istiyorsanız, **Her Dakika**'yı seçin ve metin kutusuna **15** girin. 
    1. **Bitiş** alanı için, tetikleyici için bir bitiş tarihi belirtmek istemiyorsanız, **Bitiş Sonu'nu**seçin. Bitiş tarihi saatini belirtmek **için, Tarih'i**seçin ve bitiş tarihini belirtin, ardından **Tamam'ı**seçin. Her işlem hattı çalıştırmasının bir maliyeti vardır. Test ediyorsanız, ardışık nedenin yalnızca birkaç kez tetiklendirildiğından emin olmak isteyebilirsiniz. Öte yandan, yayımlama saatiyle bitiş saati arasında işlem hattının çalıştırılmasına yetecek kadar zaman olduğundan emin olun. Tetikleyici siz çözümü kullanıcı arabiriminde kaydettiğinizde değil ancak Data Factory'de yayımladığınızda devreye girer.

        ![Tetikleyici ayarları](./media/how-to-create-schedule-trigger/trigger-settings.png)

1. Yeni **Tetikleyici** penceresinde, **Etkinleştirilen** seçenekte **Evet'i** seçin ve **ardından Tamam'ı**seçin. Tetikleyiciyi daha sonra devre dışı bırakmak için bu onay kutusunu kullanabilirsiniz. 

    ![Tetikleyici ayarları - İleri düğmesi](./media/how-to-create-schedule-trigger/trigger-settings-next.png)

1. Yeni **Tetikleyici** penceresinde, uyarı iletisini gözden geçirin ve ardından **Tamam'ı**seçin.

    ![Tetikleyici ayarları - Son düğmesi](./media/how-to-create-schedule-trigger/new-trigger-finish.png)

1. Değişiklikleri Veri Fabrikası'nda yayımlamak için **tümünü yayımla'yı** seçin. Veri Fabrikası'ndaki değişiklikleri yayımlayana kadar, tetikleyici ardışık hatlar çalışır çalışanlarını tetiklemeye başlamaz. 

    ![Yayımla düğmesi](./media/how-to-create-schedule-trigger/publish-2.png)

1. Soldaki **Pipeline çalışır** sekmesine geçin ve listeyi yenilemek için **Yenile'yi** seçin. Zamanlanan tetikleyici tarafından tetiklenen ardışık hatlar çalışır göreceksiniz. **Tetikleyen** sütunundaki değerlere dikkat edin. **Şimdi Tetikle** seçeneğini kullanırsanız, listede manuel tetikleyicinin çalıştırını görürsünüz. 

    ![Tetiklenen çalıştırmaları izleme](./media/how-to-create-schedule-trigger/monitor-triggered-runs.png)

1. **Tetikleyici Çalıştırmaları** görünümüne geçin. 

    ![Tetikleme çalıştırmalarını izleme](./media/how-to-create-schedule-trigger/monitor-trigger-runs.png)

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Bu bölümde, zamanlama tetikleyicisi oluşturmak, başlatmak ve izlemek için Azure PowerShell'i nasıl kullanacağınızı gösterir. Bu örneğin çalıştığını görmek için önce [Quickstart: Azure PowerShell'i kullanarak bir veri fabrikası oluşturun.](quickstart-create-data-factory-powershell.md) Ardından, her 15 dakikada bir çalışan bir zamanlama tetikleyicisi oluşturan ve başlatan ana yönteme aşağıdaki kodu ekleyin. Tetikleyici, Quickstart'ın bir parçası olarak oluşturduğunuz **Adfv2QuickStartPipeline** adlı bir ardışık yol ile ilişkilidir.

1. C:\ADFv2QuickStartPSH\ klasöründe Aşağıdaki içeriği içeren **MyTrigger.json** adlı bir JSON dosyası oluşturun:

    > [!IMPORTANT]
    > JSON dosyasını kaydetmeden **önce, başlangıç Zamanı** öğesinin değerini geçerli UTC saatine ayarlayın. **EndTime** öğesinin değerini geçerli UTC saatini bir saat geçmiş olarak ayarlayın.

    ```json   
    {
        "properties": {
            "name": "MyTrigger",
            "type": "ScheduleTrigger",
            "typeProperties": {
                "recurrence": {
                    "frequency": "Minute",
                    "interval": 15,
                    "startTime": "2017-12-08T00:00:00",
                    "endTime": "2017-12-08T01:00:00"
                }
            },
            "pipelines": [{
                    "pipelineReference": {
                        "type": "PipelineReference",
                        "referenceName": "Adfv2QuickStartPipeline"
                    },
                    "parameters": {
                        "inputPath": "adftutorial/input",
                        "outputPath": "adftutorial/output"
                    }
                }
            ]
        }
    }
    ```

    JSON snippet olarak:
    - Tetikleyicinin **tür** öğesi "ScheduleTrigger" olarak ayarlanır.
    - **Frekans** öğesi "Dakika" olarak, **aralık** öğesi 15 olarak ayarlanır. Bu nedenle, tetikleyici başlangıç ve bitiş saatleri arasında her 15 dakikada bir ardışık çalışır.
    - **Bitiş Zamanı** **öğesi, başlangıç Zamanı** öğesinin değerinden bir saat sonradır. Bu nedenle, tetikleyici, başlangıç saatinden 15 dakika, 30 dakika ve 45 dakika sonra ardışık hattı çalıştırın. Başlangıç saatini geçerli UTC saatine ve bitiş saatini başlangıç saatini bir saate güncelleştirmeyi unutmayın. 
    - Tetikleyici **Adfv2QuickStartPipeline** ardışık ile ilişkilidir. Birden çok ardışık bölümü bir tetikleyiciyle ilişkilendirmek için daha fazla **pipelineReference** bölümü ekleyin.
    - Quickstart'taki ardışık işlem iki **parametre** değeri alır: **inputPath** ve **outputPath.** Bu nedenle, bu parametreler için değerleri tetikleyiciden geçirirsiniz.

1. **Set-AzDataFactoryV2Trigger** cmdlet'i kullanarak bir tetikleyici oluşturun:

    ```powershell
    Set-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    ```

1. **Tetikleyicinin durumunuget-AzDataFactoryV2Trigger** cmdlet kullanarak **durdurulduğunu** doğrulayın:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

1. **Start-AzDataFactoryV2Trigger** cmdlet kullanarak tetikleyiciyi başlatın:

    ```powershell
    Start-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

1. Tetikleyicinin **durumunuget-AzDataFactoryV2Trigger** cmdlet kullanarak **başlatıldığından** onaylayın:

    ```powershell
    Get-AzDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```

1.  **Get-AzDataFactoryV2TriggerRun** cmdlet'ini kullanarak Azure PowerShell'de tetikleyici çalıştırmaları alın. Tetikleyici çalıştırmaları hakkında bilgi almak için aşağıdaki komutu düzenli aralıklarla uygulayın. **TriggerRunStartedAfter** ve **TriggerRunStartedBefore** değerlerini tetikleyici tanımınızdaki değerlerle eşleştirmek için güncelleştirin:

    ```powershell
    Get-AzDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
    
    Azure portalında tetik leme çalışanlarını ve ardışık hatlar çalışır çalışanlarını izlemek için [bkz.](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)


## <a name="net-sdk"></a>.NET SDK
Bu bölümde, bir tetikleyici oluşturmak, başlatmak ve izlemek için .NET SDK'yı nasıl kullanacağınızı gösterir. Bu örneğin çalıştığını görmek için önce [Quickstart'a gidin: .NET SDK'yı kullanarak bir veri fabrikası oluşturun.](quickstart-create-data-factory-dot-net.md) Ardından, her 15 dakikada bir çalışan bir zamanlama tetikleyicisi oluşturan ve başlatan ana yönteme aşağıdaki kodu ekleyin. Tetikleyici, Quickstart'ın bir parçası olarak oluşturduğunuz **Adfv2QuickStartPipeline** adlı bir ardışık yol ile ilişkilidir.

Her 15 dakikada bir çalışan bir zamanlama tetikleyicisi oluşturmak ve başlatmak için ana yönteme aşağıdaki kodu ekleyin:

```csharp
            // Create the trigger
            Console.WriteLine("Creating the trigger");

            // Set the start time to the current UTC time
            DateTime startTime = DateTime.UtcNow;

            // Specify values for the inputPath and outputPath parameters
            Dictionary<string, object> pipelineParameters = new Dictionary<string, object>();
            pipelineParameters.Add("inputPath", "adftutorial/input");
            pipelineParameters.Add("outputPath", "adftutorial/output");

            // Create a schedule trigger
            string triggerName = "MyTrigger";
            ScheduleTrigger myTrigger = new ScheduleTrigger()
            {
                Pipelines = new List<TriggerPipelineReference>()
                {
                    // Associate the Adfv2QuickStartPipeline pipeline with the trigger
                    new TriggerPipelineReference()
                    {
                        PipelineReference = new PipelineReference(pipelineName),
                        Parameters = pipelineParameters,
                    }
                },
                Recurrence = new ScheduleTriggerRecurrence()
                {
                    // Set the start time to the current UTC time and the end time to one hour after the start time
                    StartTime = startTime,
                    TimeZone = "UTC",
                    EndTime = startTime.AddHours(1),
                    Frequency = RecurrenceFrequency.Minute,
                    Interval = 15,
                }
            };

            // Now, create the trigger by invoking the CreateOrUpdate method
            TriggerResource triggerResource = new TriggerResource()
            {
                Properties = myTrigger
            };
            client.Triggers.CreateOrUpdate(resourceGroup, dataFactoryName, triggerName, triggerResource);

            // Start the trigger
            Console.WriteLine("Starting the trigger");
            client.Triggers.Start(resourceGroup, dataFactoryName, triggerName);
```

Tetikleyici çalışmasını izlemek için, örnekteki son `Console.WriteLine` ifadeden önce aşağıdaki kodu ekleyin:

```csharp
            // Check that the trigger runs every 15 minutes
            Console.WriteLine("Trigger runs. You see the output every 15 minutes");

            for (int i = 0; i < 3; i++)
            {
                System.Threading.Thread.Sleep(TimeSpan.FromMinutes(15));
                List<TriggerRun> triggerRuns = client.Triggers.ListRuns(resourceGroup, dataFactoryName, triggerName, DateTime.UtcNow.AddMinutes(-15 * (i + 1)), DateTime.UtcNow.AddMinutes(2)).ToList();
                Console.WriteLine("{0} trigger runs found", triggerRuns.Count);

                foreach (TriggerRun run in triggerRuns)
                {
                    foreach (KeyValuePair<string, string> triggeredPipeline in run.TriggeredPipelines)
                    {
                        PipelineRun triggeredPipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, triggeredPipeline.Value);
                        Console.WriteLine("Pipeline run ID: {0}, Status: {1}", triggeredPipelineRun.RunId, triggeredPipelineRun.Status);
                        List<ActivityRun> runs = client.ActivityRuns.ListByPipelineRun(resourceGroup, dataFactoryName, triggeredPipelineRun.RunId, run.TriggerRunTimestamp.Value, run.TriggerRunTimestamp.Value.AddMinutes(20)).ToList();
                    }
                }
            }
```

Azure portalında tetik leme çalışanlarını ve ardışık hatlar çalışır çalışanlarını izlemek için [bkz.](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)


## <a name="python-sdk"></a>Python SDK'sı
Bu bölümde, bir tetikleyici oluşturmak, başlatmak ve izlemek için Python SDK'yı nasıl kullanacağınızı gösterir. Bu örneğin çalıştığını görmek için önce [Quickstart:Python SDK'yı kullanarak bir veri fabrikası oluşturun.](quickstart-create-data-factory-python.md) Ardından, Python komut dosyasındaki "pipeline run"ı izleyin kod bloğundan sonra aşağıdaki kod bloğunu ekleyin. Bu kod, belirtilen başlangıç ve bitiş saatleri arasında her 15 dakikada bir çalışan bir zamanlama tetikleyicisi oluşturur. **start_time** değişkenini geçerli UTC saatine ve **end_time** değişkenini geçerli UTC saatini bir saat geçmiş olarak güncelleştirin.

```python
    # Create a trigger
    tr_name = 'mytrigger'
    scheduler_recurrence = ScheduleTriggerRecurrence(frequency='Minute', interval='15',start_time='2017-12-12T04:00:00', end_time='2017-12-12T05:00:00', time_zone='UTC')
    pipeline_parameters = {'inputPath':'adftutorial/input', 'outputPath':'adftutorial/output'}
    pipelines_to_run = []
    pipeline_reference = PipelineReference('copyPipeline')
    pipelines_to_run.append(TriggerPipelineReference(pipeline_reference, pipeline_parameters))
    tr_properties = ScheduleTrigger(description='My scheduler trigger', pipelines = pipelines_to_run, recurrence=scheduler_recurrence)    
    adf_client.triggers.create_or_update(rg_name, df_name, tr_name, tr_properties)

    # Start the trigger
    adf_client.triggers.start(rg_name, df_name, tr_name)
```

Azure portalında tetik leme çalışanlarını ve ardışık hatlar çalışır çalışanlarını izlemek için [bkz.](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="azure-resource-manager-template"></a>Azure Resource Manager şablonu
Tetikleyici oluşturmak için Azure Kaynak Yöneticisi şablonu kullanabilirsiniz. Adım adım yönergeler için kaynak [yöneticisi şablonu kullanarak Azure veri fabrikası oluşturma'ya](quickstart-create-data-factory-resource-manager-template.md)bakın.  

## <a name="pass-the-trigger-start-time-to-a-pipeline"></a>Tetikleme başlangıç saatini bir ardışık boru hattına geçirin
Azure Veri Fabrikası sürüm 1, sistem değişkenlerini kullanarak bölümlenmiş verileri okumayı veya yazmayı destekler: **SliceStart**, **SliceEnd**, **WindowStart**ve **WindowEnd**. Azure Veri Fabrikası'nın geçerli sürümünde, bir ardışık parametre kullanarak bu davranışı elde edebilirsiniz. Tetikleyicinin başlangıç saati ve zamanlanan saati, ardışık hatlar parametresi değeri olarak ayarlanır. Aşağıdaki örnekte, tetikleyici için zamanlanan saat, **zamanlanan RunTime** parametresine bir değer olarak geçirilir:

```json
"parameters": {
    "scheduledRunTime": "@trigger().scheduledTime"
}
```

## <a name="json-schema"></a>JSON şema
Aşağıdaki JSON tanımı, zamanlama ve yineleme ile zamanlama tetikleyicisi oluşturmayı gösterir:

```json
{
  "properties": {
    "type": "ScheduleTrigger",
    "typeProperties": {
      "recurrence": {
        "frequency": <<Minute, Hour, Day, Week, Month>>,
        "interval": <<int>>,             // Optional, specifies how often to fire (default to 1)
        "startTime": <<datetime>>,
        "endTime": <<datetime - optional>>,
        "timeZone": "UTC"
        "schedule": {                    // Optional (advanced scheduling specifics)
          "hours": [<<0-23>>],
          "weekDays": [<<Monday-Sunday>>],
          "minutes": [<<0-59>>],
          "monthDays": [<<1-31>>],
          "monthlyOccurrences": [
               {
                    "day": <<Monday-Sunday>>,
                    "occurrence": <<1-5>>
               }
           ]
        }
      }
    },
   "pipelines": [
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "<Name of your pipeline>"
                },
                "parameters": {
                    "<parameter 1 Name>": {
                        "type": "Expression",
                        "value": "<parameter 1 Value>"
                    },
                    "<parameter 2 Name>" : "<parameter 2 Value>"
                }
           }
      ]
  }
}
```

> [!IMPORTANT]
>  **parameters** özelliği, **pipelines** öğesinin zorunlu bir özelliğidir. İşlem hattınız herhangi bir parametre almasa bile, **parameters** özelliği için boş bir JSON tanımı eklemeniz gerekir.


### <a name="schema-overview"></a>Şemaya genel bakış
Aşağıdaki tabloda bir tetikleyicinin yinelenmesi ve zamanlanmasıyla ilgili ana şema öğelerinin genel bir özeti verilmiştir:

| JSON özelliği | Açıklama |
|:--- |:--- |
| **startTime** | Bir Tarih-Saat değeri. Basit zamanlamalar için **startTime** özelliğinin değeri ilk oluşum için geçerli olur. Karmaşık zamanlamalar için tetikleyici belirtilen **startTime** değerinden önce başlamaz. |
| **endTime** | Tetikleyicinin bitiş tarihi ve saati. Tetikleyici belirtilen bitiş tarihi ve saatinden sonra yürütülmez. Bu özelliğin değeri geçmişte olamaz. Bu özellik isteğe bağlıdır. |
| **timeZone** | Saat dilimi. Şu anda yalnızca UTC saat dilimi desteklenmektedir. |
| **Yineleme** | Tetikleyici için yinelenme kurallarını belirten bir yinelenme nesnesi. recurrence nesnesi şu öğeleri destekler: **frequency**, **interval**, **endTime**, **count** ve **schedule**. Bir yinelenme nesnesi tanımlanırken **frequency** öğesi gereklidir. Yinelenme nesnesinin diğer öğeleri isteğe bağlıdır. |
| **Frekans** | Tetikleyicinin yineleneceği sıklık birimi. “Minute”, “hour”, “day”, “week” ve “month” değerleri desteklenir. |
| **Aralığı** | Tetikleyicinin çalışma sıklığını belirten **frequency** değerinin aralığını gösteren bir pozitif tamsayı. Örneğin **interval** değeri 3, **frequency** değeri de "week" ise tetikleyici 3 haftada bir yinelenir. |
| **Zamanlama** | Tetikleyicinin yinelenme zamanlaması. **frequency** değeri belirtilen bir tetikleyici, yinelenmesini bir yinelenme zamanlamasına göre değiştirir. **schedule** özelliği, yinelenme için dakika, saat, haftanın günü, ayın günü ve hafta numarası tabanlı değişiklikleri içerir.


### <a name="schema-defaults-limits-and-examples"></a>Şema varsayılanları, sınırlar ve örnekler

| JSON özelliği | Tür | Gerekli | Varsayılan değer | Geçerli değerler | Örnek |
|:--- |:--- |:--- |:--- |:--- |:--- |
| **startTime** | Dize | Evet | Hiçbiri | ISO-8601 Tarih-Saatleri | `"startTime" : "2013-01-09T09:30:00-08:00"` |
| **Yineleme** | Nesne | Evet | Hiçbiri | Yinelenme nesnesi | `"recurrence" : { "frequency" : "monthly", "interval" : 1 }` |
| **Aralığı** | Sayı | Hayır | 1 | 1-1.000 | `"interval":10` |
| **endTime** | Dize | Evet | Hiçbiri | Gelecekteki bir zamanı temsil eden Tarih-Saat değeri. | `"endTime" : "2013-02-09T09:30:00-08:00"` |
| **Zamanlama** | Nesne | Hayır | Hiçbiri | Zamanlama nesnesi | `"schedule" : { "minute" : [30], "hour" : [8,17] }` |

### <a name="starttime-property"></a>startTime özelliği
Aşağıdaki tabloda **startTime** özelliğinin bir tetikleyici çalıştırmasını nasıl denetlediği gösterilmektedir:

| startTime değeri | Zamanlama olmadan yinelenme | Zamanlama ile yinelenme |
|:--- |:--- |:--- |
| Başlangıç zamanı geçmişte | Başlangıç zamanından sonraki ilk gelecek yürütme zamanını hesaplar ve bu zamanda çalıştırır.<br/><br/>Son yürütme zamanına göre yaptığı hesaplamalarla sonraki yürütmeleri çalıştırır.<br/><br/>Bu tablonun altındaki örneğe bakın. | Tetikleyici belirtilen başlangıç zamanından _önce_ başlamaz. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamayı temel alır.<br/><br/>Sonraki yürütmeleri yinelenme zamanlamasına göre çalıştırır. |
| Başlangıç zamanı gelecekte veya güncel | Belirtilen başlangıç zamanında bir kez çalışır.<br/><br/>Son yürütme zamanına göre yaptığı hesaplamalarla sonraki yürütmeleri çalıştırır. | Tetikleyici belirtilen başlangıç saatinden _daha erken_ başlar. İlk yinelenme, başlangıç zamanından hesaplanan zamanlamayı temel alır.<br/><br/>Sonraki yürütmeleri yinelenme zamanlamasına göre çalıştırır. |

Başlangıç zamanı geçmişte olduğunda ve bir yinelenme olmasına rağmen zamanlama olmadığında neler olacağını gösteren bir örneğe bakalım. Geçerli zamanın `2017-04-08 13:00`, başlangıç zamanının `2017-04-07 14:00` ve yinelenmenin iki günde bir olduğunu varsayalım. (Yineleme **recurrence** değeri **frekans** özelliği "gün" ve **aralık** özelliği 2 olarak ayarlayarak tanımlanır.) **Başlangıç Zamanı** değerinin geçmişte olduğuna ve geçerli zamandan önce oluştuğuna dikkat edin.

Bu şartlar altında ilk çalıştırma zamanı `2017-04-09 at 14:00` olacaktır. Zamanlayıcı altyapısı çalıştırma yinelenmelerini başlangıç zamanından itibaren hesaplar. Geçmişteki örnekler dikkate alınmaz. Altyapı gelecekte gerçekleşen bir sonraki örneği kullanır. Bu senaryoda startTime `2017-04-07 at 2:00pm` ve sonraki örnek bu zamandan iki gün sonrası olan `2017-04-09 at 2:00pm` olur.

İlk yürütme zamanı **startTime** değeri `2017-04-05 14:00` veya `2017-04-01 14:00` olsa bile değişmez. İlk yürütme sonrasındaki yürütmeler zamanlama kullanılarak hesaplanır. Bu nedenle, sonraki yürütmeler sırasıyla `2017-04-11 at 2:00pm`, `2017-04-13 at 2:00pm`, `2017-04-15 at 2:00pm` itibarıyla gerçekleşir ve bu düzende devam eder.

Son olarak, bir tetikleyicinin zamanlamasında saatler veya dakikalar ayarlanmazsa varsayılan olarak ilk yürütmenin saat veya dakikaları kullanılır.

### <a name="schedule-property"></a>schedule özelliği
Diğer yandan, zamanlama kullanımı tetikleyici yürütme sayısını sınırlayabilir. Örneğin, aylık sıklığa sahip bir tetikleyici 31. günde çalışacak şekilde zamanlanırsa, tetikleyici yalnızca 31. günü olan aylarda çalışır.

Diğer yandan zamanlama, tetikleyici çalıştırma sayısını da genişletebilir. Örneğin, aylık sıklığa sahip olan ve ayın 1. ve 2. günlerinde çalışacak şekilde zamanlanan bir tetikleyici, ayda bir kez çalışmak yerine ayın 1. ve 2. günlerinde çalışır.

Birden fazla **schedule** öğesi belirtilmişse değerlendirme sırası en büyük zamanlama ayarından en küçüğe doğru olur. Değerlendirme hafta numarasıyla başlar ve arkasından ayın günü, haftanın günü, saat ve son olarak dakika gelir.

Aşağıdaki tabloda **schedule** öğeleri ayrıntılı bir şekilde açıklanmıştır:


| JSON öğesi | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |
| **minutes** | Tetikleyicinin çalıştığı dakika değeri. | <ul><li>Tamsayı</li><li>Tamsayı dizisi</li></ul>
| **Saat** | Tetikleyicinin çalıştığı saat değeri. | <ul><li>Tamsayı</li><li>Tamsayı dizisi</li></ul> |
| **weekDays** | Tetikleyicinin çalıştığı hafta günleri. Bu değer yalnızca haftalık bir sıklık ile belirtilebilir. | <ul><li>Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday</li><li>Gün değerleri dizisi (en fazla dizi boyutu 7’dir)</li><li>Gün değerleri büyük/küçük harfe duyarlı değildir</li></ul> |
| **monthlyOccurrences** | Tetikleyicinin çalıştığı ay günleri. Bu değer yalnızca aylık bir sıklık ile belirtilebilir. | <ul><li>Aylık **Oluşum** nesneleri dizisi: `{ "day": day,  "occurrence": occurrence }`.</li><li>**day** özniteliği, tetikleyicinin çalıştığı gündür. Örneğin, **day** değeri `{Sunday}` olan bir **monthlyOccurrences** özelliği, ayın her Pazar günü anlamına gelir. **day** özniteliği gereklidir.</li><li>**occurrence** özniteliği, ay içinde belirtilen **day** değerinin gerçekleşmesidir. Örneğin, **day** ve **occurrence** değerleri `{Sunday, -1}` olan bir **monthlyOccurrences** özelliği, ayın son Pazar günü anlamına gelir. **occurrence** özniteliği isteğe bağlıdır.</li></ul> |
| **monthDays** | Tetikleyicinin çalıştığı ay günü. Bu değer yalnızca aylık bir sıklık ile belirtilebilir. | <ul><li><= -1 ve >= -31 şartlarını karşılayan herhangi bir değer</li><li>>= 1 ve <= 31 şartlarını karşılayan herhangi bir değer</li><li>Değer dizisi</li></ul> |


## <a name="examples-of-trigger-recurrence-schedules"></a>Tetikleyici yineleme zamanlaması örnekleri
Bu bölümde yinelenme zamanlaması örnekleri sağlanmış ve **schedule** nesnesi ile bunun öğelerine odaklanılmıştır.

Örneklerde **interval** değerinin 1 olduğu ve zamanlama tanımına göre **frequency** değerinin doğru olduğu varsayılmıştır. Örneğin, "gün" **frekans** değerine sahip olamaz ve **zamanlama** nesnesinde "monthDays" modifikasyonuna da sahip olamazsınız. Bu gibi kısıtlamalar önceki bölümde yer alan tabloda belirtilmiştir.

| Örnek | Açıklama |
|:--- |:--- |
| `{"hours":[5]}` | Her gün 05.00'te çalıştır. |
| `{"minutes":[15], "hours":[5]}` | Her gün 05.15'te çalıştır. |
| `{"minutes":[15], "hours":[5,17]}` | Her gün 05.15 ve 17.15’te çalıştır. |
| `{"minutes":[15,45], "hours":[5,17]}` | Her gün 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |
| `{"minutes":[0,15,30,45]}` | 15 dakikada bir çalıştır. |
| `{hours":[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23]}` | Saatte bir çalıştır. Bu tetikleyici saatte bir çalışır. **startTime** değeri belirtilirse dakikalar bu değer tarafından denetlenir. Bir değer belirtilmezse dakikalar, oluşturma zamanı tarafından denetlenir. Örneğin başlangıç zamanı veya oluşturma zamanı (hangisi geçerliyse) 12:25 ise tetikleyici 00.25, 01.25, 02.25, ... ve 23.25 saatlerinde çalışır.<br/><br/>Bu zamanlama, **frequency** değeri “hour”, **interval** değeri 1 olan ve **schedule** olmayan bir tetikleyiciye sahip olmakla eşdeğerdir.  Bu zamanlama farklı **frequency** ve **interval** değerleriyle kullanılarak başka tetikleyiciler oluşturulabilir. Örneğin, **frequency** değeri “day” olduğunda her gün çalışan zamanlama, **frequency** değeri “month” olduğunda yalnızca ayda bir kere çalışır. |
| `{"minutes":[0]}` | Her saat başı çalıştır. Bu tetikleyici 12.00, 01.00, 02.00 gibi bir saatte başlayarak saat başı çalışır.<br/><br/>Bu zamanlama, **frequency** değeri “hour” olup **startTime** değeri sıfır dakika olan ya da **schedule** değeri olmayıp **frequency** değeri “day” olan bir tetikleyiciye eşdeğerdir. **Sıklık** değeri "hafta" veya "ay" ise, zamanlama sırasıyla haftada bir gün veya yalnızca ayda bir gün yürütür. |
| `{"minutes":[15]}` | Her saat başını 15 dakika geçe çalıştır. Bu tetikleyici, 00.15, 01.15, 02.15, vb. bir saatte başlayıp 23.15’te bitecek şekilde her saat başını 15 dakika geçe çalışır. |
| `{"hours":[17], "weekDays":["saturday"]}` | Her hafta Cumartesi günleri saat 17.00'de çalıştır. |
| `{"hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17.00'de çalıştır. |
| `{"minutes":[15,45], "hours":[17], "weekDays":["monday", "wednesday", "friday"]}` | Her hafta Pazartesi, Çarşamba ve Cuma günleri saat 17.15 ve 17.45'te çalıştır. |
| `{"minutes":[0,15,30,45], "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Haftanın her günü 15 dakikada bir çalıştır. |
| `{"minutes":[0,15,30,45], "hours": [9, 10, 11, 12, 13, 14, 15, 16] "weekDays":["monday", "tuesday", "wednesday", "thursday", "friday"]}` | Haftanın her günü 09.00 ile 16.45 arasında 15 dakikada bir çalıştır. |
| `{"weekDays":["tuesday", "thursday"]}` | Salı ve Perşembe günleri belirtilen başlangıç saatinde çalıştır. |
| `{"minutes":[0], "hours":[6], "monthDays":[28]}` | Her ayın 28. gününde saat 06.00'da çalıştır (**frequency** değerinin “month” olduğu kabul edilir). |
| `{"minutes":[0], "hours":[6], "monthDays":[-1]}` | Ayın son günü saat 06.00'da çalıştır. Bir tetikleyiciyi ayın son gününde çalıştırmak için 28, 29, 30 veya 31 yerine -1 değerini kullanın. |
| `{"minutes":[0], "hours":[6], "monthDays":[1,-1]}` | Her ayın ilk ve son günü saat 06.00'da çalıştır. |
| `{monthDays":[1,14]}` | Her ayın birinci ve 14. gününde belirtilen başlangıç zamanında çalıştır. |
| `{"minutes":[0], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Her ayın ilk Cuma günü saat 05.00'te çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1}]}` | Her ayın ilk Cuma gününde belirtilen başlangıç zamanında çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":-3}]}` | Her ay, ayın sondan üçüncü Cuma gününde belirtilen zamanda çalıştır. |
| `{"minutes":[15], "hours":[5], "monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Her ayın ilk ve son Cuma günü saat 05.15'te çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":1},{"day":"friday", "occurrence":-1}]}` | Her ayın ilk ve son Cuma günü belirtilen başlangıç zamanında çalıştır. |
| `{"monthlyOccurrences":[{"day":"friday", "occurrence":5}]}` | Her ayın beşinci Cuma günü belirtilen başlangıç zamanında çalıştır. Bir ayda beşinci Cuma günü yoksa, işlem hattı yalnızca beşinci Cuma günlerinde çalışacak şekilde zamanlandığından çalışmaz. Tetikleyiciyi ayın son Cuma gününde çalıştırmak istiyorsanız **occurrence** değeri için 5 yerine -1 yazın. |
| `{"minutes":[0,15,30,45], "monthlyOccurrences":[{"day":"friday", "occurrence":-1}]}` | Ayın son Cuma günü 15 dakikada bir çalıştır. |
| `{"minutes":[15,45], "hours":[5,17], "monthlyOccurrences":[{"day":"wednesday", "occurrence":3}]}` | Her ayın üçüncü Çarşamba günü 05.15, 05.45, 17.15 ve 17.45’te çalıştır. |


## <a name="next-steps"></a>Sonraki adımlar
Tetikleyiciler hakkında ayrıntılı bilgi [için, Bkz. Boru Hattı yürütme ve tetikleyiciler.](concepts-pipeline-execution-triggers.md#trigger-execution)
