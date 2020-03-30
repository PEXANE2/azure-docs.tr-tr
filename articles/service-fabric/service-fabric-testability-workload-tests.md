---
title: Azure Hizmet Kumaşı uygulamalarındaki hataları simüle edin
description: Azure Hizmet Kumaşı hizmetlerinizi zarif ve zarif hatalara karşı nasıl sertleştirmiş olabiliriz hakkında bilgi edinin.
author: anmolah
ms.topic: conceptual
ms.date: 06/15/2017
ms.author: anmola
ms.openlocfilehash: d3d9f6478336c59adb875bf21438d5ffa457b1d4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645999"
---
# <a name="simulate-failures-during-service-workloads"></a>Hizmet iş yükleri sırasında hataları simüle etmek
Azure Hizmet Kumaşı'ndaki sınayabilirlik senaryoları, geliştiricilerin tek tek hatalarla başa çıkma konusunda endişelenmemelerini sağlar. Ancak, istemci iş yükünün ve hataların açık bir şekilde ayrılmasının gerekli olabileceği senaryolar vardır. İstemci iş yükünün ve hatalarının ayrılması, hata olduğunda hizmetin gerçekten bazı eylemler gerçekleştirmesini sağlar. Sınanabilirliğin sağladığı denetim düzeyi göz önüne alındığında, bunlar iş yükü yürütmesinin kesin noktalarında olabilir. Uygulamada farklı durumlarda hataları bu indüksiyon hataları bulmak ve kalitesini artırabilir.

## <a name="sample-custom-scenario"></a>Örnek özel senaryo
Bu test, iş yükünü [zarif ve zarif olmayan hatalarla](service-fabric-testability-actions.md#graceful-vs-ungraceful-fault-actions)birbiriyle kesişen bir senaryo gösterir. Hatalar, en iyi sonuçlar için servis işlemlerinin ortasında veya hesaplamada indüklenmelidir.

Dört iş yükünü ortaya çıkaran bir hizmet örneğini gözden geçirelim: A, B, C ve D. Her biri bir iş akışı kümesine karşılık gelir ve işlem, depolama veya karışım olabilir. Basitlik adına, örneğimizdeki iş yüklerini özetleyeceğiz. Bu örnekte çalıştırılan farklı hatalar şunlardır:

* Yeniden Başlatma: Makinenin yeniden başlatılmasını simüle etmek için yapılan ungraceful hatası.
* Yeniden BaşlaDağıtılanCodePackage: Hizmet ana bilgisayar işlem çökmelerini simüle etmek için ungraceful hata.
* RemoveReplica: Çoğaltma kaldırma simüle etmek için zarif hata.
* MovePrimary: Service Fabric yük dengeleyicisi tarafından tetiklenen yineleme hareketlerini simüle etmek için zarif hata.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll.

using System;
using System.Fabric;
using System.Fabric.Testability.Scenario;
using System.Threading;
using System.Threading.Tasks;

class Test
{
    public static int Main(string[] args)
    {
        // Replace these strings with the actual version for your cluster and application.
        string clusterConnection = "localhost:19000";
        Uri applicationName = new Uri("fabric:/samples/PersistentToDoListApp");
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");

        Console.WriteLine("Starting Workload Test...");
        try
        {
            RunTestAsync(clusterConnection, applicationName, serviceName).Wait();
        }
        catch (AggregateException ae)
        {
            Console.WriteLine("Workload Test failed: ");
            foreach (Exception ex in ae.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("Workload Test completed successfully.");
        return 0;
    }

    public enum ServiceWorkloads
    {
        A,
        B,
        C,
        D
    }

    public enum ServiceFabricFaults
    {
        RestartNode,
        RestartCodePackage,
        RemoveReplica,
        MovePrimary,
    }

    public static async Task RunTestAsync(string clusterConnection, Uri applicationName, Uri serviceName)
    {
        // Create FabricClient with connection and security information here.
        FabricClient fabricClient = new FabricClient(clusterConnection);
        // Maximum time to wait for a service to stabilize.
        TimeSpan maxServiceStabilizationTime = TimeSpan.FromSeconds(120);

        // How many loops of faults you want to execute.
        uint testLoopCount = 20;
        Random random = new Random();

        for (var i = 0; i < testLoopCount; ++i)
        {
            var workload = SelectRandomValue<ServiceWorkloads>(random);
            // Start the workload.
            var workloadTask = RunWorkloadAsync(workload);

            // While the task is running, induce faults into the service. They can be ungraceful faults like
            // RestartNode and RestartDeployedCodePackage or graceful faults like RemoveReplica or MovePrimary.
            var fault = SelectRandomValue<ServiceFabricFaults>(random);

            // Create a replica selector, which will select a primary replica from the given service to test.
            var replicaSelector = ReplicaSelector.PrimaryOf(PartitionSelector.RandomOf(serviceName));
            // Run the selected random fault.
            await RunFaultAsync(applicationName, fault, replicaSelector, fabricClient);
            // Validate the health and stability of the service.
            await fabricClient.TestManager.ValidateServiceAsync(serviceName, maxServiceStabilizationTime);

            // Wait for the workload to finish successfully.
            await workloadTask;
        }
    }

    private static async Task RunFaultAsync(Uri applicationName, ServiceFabricFaults fault, ReplicaSelector selector, FabricClient client)
    {
        switch (fault)
        {
            case ServiceFabricFaults.RestartNode:
                await client.FaultManager.RestartNodeAsync(selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RestartCodePackage:
                await client.FaultManager.RestartDeployedCodePackageAsync(applicationName, selector, CompletionMode.Verify);
                break;
            case ServiceFabricFaults.RemoveReplica:
                await client.FaultManager.RemoveReplicaAsync(selector, CompletionMode.Verify, false);
                break;
            case ServiceFabricFaults.MovePrimary:
                await client.FaultManager.MovePrimaryAsync(selector.PartitionSelector);
                break;
        }
    }

    private static Task RunWorkloadAsync(ServiceWorkloads workload)
    {
        throw new NotImplementedException();
        // This is where you trigger and complete your service workload.
        // Note that the faults induced while your service workload is running will
        // fault the primary service. Hence, you will need to reconnect to complete or check
        // the status of the workload.
    }

    private static T SelectRandomValue<T>(Random random)
    {
        Array values = Enum.GetValues(typeof(T));
        T workload = (T)values.GetValue(random.Next(values.Length));
        return workload;
    }
}
```
