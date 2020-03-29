---
title: Azure veri fabrikasını programlı olarak izleme
description: Farklı yazılım geliştirme kitleri (SDK'lar) kullanarak veri fabrikasındaki bir ardışık hattı nasıl izleyeceğizi öğrenin.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/16/2018
author: djpmsft
ms.author: daperlov
manager: anandsub
ms.openlocfilehash: 43a31d588ff6616d7200d9773883ce5da570b100
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74927352"
---
# <a name="programmatically-monitor-an-azure-data-factory"></a>Azure veri fabrikasını programlı olarak izleme
Bu makalede, farklı yazılım geliştirme kitleri (SDK) kullanarak bir veri fabrikasında bir ardışık izleme nasıl açıklanır. 

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="data-range"></a>Veri aralığı

Veri Fabrikası yalnızca 45 gün boyunca boru hattı çalıştıran verileri depolar. Veri Fabrikası ardışık işakleri ile ilgili verileri programlı olarak `Get-AzDataFactoryV2PipelineRun` sorguladiğinizde (örneğin, PowerShell komutuyla) isteğe bağlı `LastUpdatedAfter` ve `LastUpdatedBefore` parametreler için maksimum tarih yoktur. Ancak, örneğin, son bir yıla ait verileri sorgularsanız, sorgu bir hata döndürmez, ancak yalnızca son 45 güne ait ardışık ardışık işveri verir.

Ardışık hatlar denetim verilerini 45 günden fazla sürdürmek istiyorsanız, [Azure Monitor](monitor-using-azure-monitor.md)ile kendi tanılama günkaydinizi ayarlayın.

## <a name="net"></a>.NET
.NET SDK kullanarak bir ardışık hatlar oluşturma ve izleme konusunda tam [bir](quickstart-create-data-factory-dot-net.md)gözden geçirme için bkz.

1. Verileri kopyalamayı bitirene kadar ardışık hatlar çalışmasının durumunu sürekli olarak denetlemek için aşağıdaki kodu ekleyin.

    ```csharp
    // Monitor the pipeline run
    Console.WriteLine("Checking pipeline run status...");
    PipelineRun pipelineRun;
    while (true)
    {
        pipelineRun = client.PipelineRuns.Get(resourceGroup, dataFactoryName, runResponse.RunId);
        Console.WriteLine("Status: " + pipelineRun.Status);
        if (pipelineRun.Status == "InProgress")
            System.Threading.Thread.Sleep(15000);
        else
            break;
    }
    ```

2. Kopyalama etkinliği çalıştırayrıntılarını ( örneğin, okunan/yazılan verilerin boyutu) alan anize aşağıdaki kodu ekleyin.

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

.NET SDK ile ilgili tüm belgeler için [bkz.](/dotnet/api/microsoft.azure.management.datafactory?view=azure-dotnet)

## <a name="python"></a>Python
Python SDK kullanarak bir ardışık hatlar oluşturma ve izleme tam bir walkthrough için, [Python kullanarak bir veri fabrikası ve ardışık alma](quickstart-create-data-factory-python.md)bakın.

Ardışık hatlar çalışmasını izlemek için aşağıdaki kodu ekleyin:

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

Python SDK ile ilgili tüm belgeler için [Veri Fabrikası Python SDK referansına](/python/api/overview/azure/datafactory?view=azure-python)bakın.

## <a name="rest-api"></a>REST API
REST API kullanarak bir ardışık hatlar oluşturma ve izleme tam bir walkthrough için, [REST API kullanarak bir veri fabrikası ve boru hattı oluşturun](quickstart-create-data-factory-rest-api.md)bakın.
 
1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki betiği çalıştırın.

    ```powershell
    $request = "https://management.azure.com/subscriptions/${subsId}/resourceGroups/${resourceGroup}/providers/Microsoft.DataFactory/factories/${dataFactoryName}/pipelineruns/${runId}?api-version=${apiVersion}"
    while ($True) {
        $response = Invoke-RestMethod -Method GET -Uri $request -Header $authHeader
        Write-Host  "Pipeline run status: " $response.Status -foregroundcolor "Yellow"

        if ($response.Status -eq "InProgress") {
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

REST API ile ilgili tüm belgeler için [Veri Fabrikası REST API başvurusuna](/rest/api/datafactory/)bakın.

## <a name="powershell"></a>PowerShell
PowerShell kullanarak bir boru hattı oluşturma ve izleme konusunda tam bir yol için [powershell kullanarak bir veri fabrikası ve boru hattı oluşturma](quickstart-create-data-factory-powershell.md)bölümüne bakın.

1. İşlem hattı çalıştırma durumunu, verileri kopyalama işlemi tamamlanıncaya kadar sürekli olarak denetlemek için aşağıdaki betiği çalıştırın.

    ```powershell
    while ($True) {
        $run = Get-AzDataFactoryV2PipelineRun -ResourceGroupName $resourceGroupName -DataFactoryName $DataFactoryName -PipelineRunId $runId

        if ($run) {
            if ($run.Status -ne 'InProgress') {
                Write-Host "Pipeline run finished. The status is: " $run.Status -foregroundcolor "Yellow"
                $run
                break
            }
            Write-Host  "Pipeline is running...status: InProgress" -foregroundcolor "Yellow"
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

PowerShell cmdlets hakkında tam belgeler için Data [Factory PowerShell cmdlet referansına](/powershell/module/az.datafactory)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Veri Fabrikası ardışık hatlarını izlemek için Azure Monitor'u kullanma hakkında bilgi edinmek için Azure Monitor makalesini [kullanarak veri hatlarını](monitor-using-azure-monitor.md) izleyin. 

