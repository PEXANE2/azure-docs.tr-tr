---
title: Programlı olarak bir Azure Data Factory izleme
description: Farklı yazılım geliştirme setleri (SDK 'lar) kullanarak bir veri fabrikasında bir işlem hattını izlemeyi öğrenin.
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/16/2018
author: dcstwh
ms.author: weetok
ms.custom: devx-track-python
ms.openlocfilehash: 038da033c2bdf78a0a2547cc713944bc11bf093d
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100379905"
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Programlı olarak bir Azure Data Factory izleme

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Bu makalede, farklı yazılım geliştirme setleri (SDK 'lar) kullanılarak bir veri fabrikasında bir işlem hattının nasıl izleneceği açıklanır. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="data-range"></a>Veri aralığı

Data Factory yalnızca 45 gün boyunca işlem hattı çalıştırma verilerini depolar. Data Factory işlem hattı çalıştırmaları hakkında daha fazla bilgi için bkz. Örneğin, PowerShell komutu ile `Get-AzDataFactoryV2PipelineRun` isteğe bağlı ve parametreler için en fazla tarih yok `LastUpdatedAfter` `LastUpdatedBefore` . Ancak, geçen yıla ait veriler için sorgulama yaparsanız bir hata almazsınız ancak yalnızca son 45 günden veri hattı çalıştırın.

İşlem hattı çalıştırma verilerini 45 günden uzun süre tutmak istiyorsanız, [Azure izleyici](monitor-using-azure-monitor.md)ile kendi tanılama günlük kaydını ayarlayın.

## <a name="pipeline-run-information"></a>İşlem hattı çalıştırma bilgileri

İşlem hattı çalıştırma özellikleri için ardışık düzen [EYLEMSIZLIK API başvurusuna](https://docs.microsoft.com/rest/api/datafactory/pipelineruns/get#pipelinerun)bakın. İşlem hattı çalıştırmasının yaşam döngüsü sırasında farklı durumları varsa, çalışma durumunun olası değerleri aşağıda listelenmiştir:

* Kuyruğa alındı
* Ediyor
* Başarılı
* Başarısız
* İptal Ediliyor
* İptal edildi

## <a name="net"></a>.NET
.NET SDK kullanarak bir işlem hattı oluşturup izlemenin tam bir Kılavuzu için bkz. [.NET kullanarak veri fabrikası ve işlem hattı oluşturma](quickstart-create-data-factory-dot-net.md).

1. Veri kopyalamayı bitirene kadar işlem hattı çalıştırmasının durumunu sürekli olarak denetlemek için aşağıdaki kodu ekleyin.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress" || pipelineRun.Status == "Queued")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Kopyalama etkinliği çalıştırma ayrıntılarını (örneğin, okunan/yazılan verilerin boyutu) alan aşağıdaki kodu ekleyin.

    ```csharp
    // Check the copy activity run details
    Console.WriteLine("Checking copy activity run details...");
   
    List<ActivityRun> activityRuns = client.ActivityRuns.ListByPipelineRun(
    resourceGroup, dataFactoryName, runResponse.RunId, DateTime.UtcNow.AddMinutes(-10), DateTime.UtcNow.AddMinutes(10)).ToList(); 
    if (pipelineRun.Status == "Succeeded")
        Console.WriteLine(activityRuns.First().Output);
    else
        Console.WriteLine(activityRuns.First().Error);
    Console.WriteLine("\nPress any key to exit...");
    Console.ReadKey();
    ```

.NET SDK ile ilgili tüm belgeler için, [Data Factory .NET SDK başvurusu](/dotnet/api/microsoft.azure.management.datafactory)' na bakın.

## <a name="python"></a>Python
Python SDK kullanarak bir işlem hattı oluşturma ve izleme hakkında tam bir adım adım için bkz. [Python kullanarak veri fabrikası ve işlem hattı oluşturma](quickstart-create-data-factory-python.md).

İşlem hattı çalıştırmasını izlemek için aşağıdaki kodu ekleyin:

```python
# Monitor the pipeline run
time.sleep(30)
pipeline_run = adf_client.pipeline_runs.get(
    rg_name, df_name, run_response.run_id)
print("\n\tPipeline run status: {}".format(pipeline_run.status))
activity_runs_paged = list(adf_client.activity_runs.list_by_pipeline_run(
    rg_name, df_name, pipeline_run.run_id, datetime.now() - timedelta(1),  datetime.now() + timedelta(1)))
print_activity_run_details(activity_runs_paged[0])
```

Python SDK ile ilgili tüm belgeler için, [Data Factory Python SDK başvurusu](/python/api/overview/azure/datafactory)' na bakın.

## <a name="rest-api"></a>REST API
REST API kullanarak bir işlem hattı oluşturup izlemenin tam bir Kılavuzu için, bkz. [REST API kullanarak veri fabrikası ve işlem hattı oluşturma](quickstart-create-data-factory-rest-api.md).
 
1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki betiği çalıştırın.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ( ($response.Status -eq "InProgress") -or ($response.Status -eq "Queued") ) {
            Start-Sleep -Seconds 15
        }
        else {
            $response | ConvertTo-Json
            break
        }
    }
    ```
2. Kopyalama etkinliği çalıştırma ayrıntılarını (örneğin, okunan/yazılan verilerin boyutu) almak için aşağıdaki betiği çalıştırın.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}/activityruns?api-version=${apiVersion}&startTime="+(Get-Date).ToString('yyyy-MM-dd')+"&endTime="+(Get-Date).AddDays(1).ToString('yyyy-MM-dd')+"&pipelineName=Adfv2QuickStartPipeline"
    $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
    $response | ConvertTo-Json
    ```

REST API hakkındaki tüm belgeler için bkz. [Data Factory REST API başvurusu](/rest/api/datafactory/).

## <a name="powershell"></a>PowerShell
PowerShell kullanarak bir işlem hattı oluşturma ve izleme hakkında tam bir adım adım için bkz. [PowerShell kullanarak veri fabrikası ve işlem hattı oluşturma](quickstart-create-data-factory-powershell.md).

1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki betiği çalıştırın.

    ```powershell
    while ($True) {
        $run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ( ($run.Status -ne "InProgress") -and ($run.Status -ne "Queued") ) {
                Write-Output ("Pipeline run finished. The status is: " +  $run.Status)
                $run
                break
            }
            Write-Output ("Pipeline is running...status: " + $run.Status)
        }

        Start-Sleep -Seconds 30
    }
    ```
2. Kopyalama etkinliği çalıştırma ayrıntılarını (örneğin, okunan/yazılan verilerin boyutu) almak için aşağıdaki betiği çalıştırın.

    ```powershell
    Write-Host "Activity run details:" -foregroundcolor "Yellow"
    $result = Get-AzDataFactoryV2ActivityRun -DataFactoryName $dataFactoryName -ResourceGroupName $resourceGroupName -PipelineRunId $runId -RunStartedAfter (Get-Date).AddMinutes(-30) -RunStartedBefore (Get-Date).AddMinutes(30)
    $result
    
    Write-Host "Activity 'Output' section:" -foregroundcolor "Yellow"
    $result.Output -join "`r`n"
    
    Write-Host "\nActivity 'Error' section:" -foregroundcolor "Yellow"
    $result.Error -join "`r`n"
    ```

PowerShell cmdlet 'leri hakkında tüm belgeler için bkz. [PowerShell cmdlet başvurusu Data Factory](/powershell/module/az.datafactory).

## <a name="next-steps"></a>Sonraki adımlar
Data Factory işlem hatlarını izlemek üzere Azure Izleyici 'yi kullanmayı öğrenmek için bkz. Azure izleyici 'yi [kullanarak işlem hatlarını izleme](monitor-using-azure-monitor.md) . 

