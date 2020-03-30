---
title: Azure mikro hizmetlerinde hataları simüle edin
description: Bu makalede, Microsoft Azure Hizmet Dokusu'nda bulunan sınanabilirlik eylemleri hakkında bahsedilmiştir.
author: motanv
ms.topic: conceptual
ms.date: 06/07/2017
ms.author: motanv
ms.openlocfilehash: 4bdb00eec38addc0c9f88eba8b73185ec5721277
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282048"
---
# <a name="testability-actions"></a>Test edilebilirlik eylemleri
Azure Hizmet Dokusu, güvenilir olmayan bir altyapıyı simüle etmek için geliştirici olarak size çeşitli gerçek dünyadaki hataları ve devlet geçişlerini simüle etmenin yollarını sunar. Bunlar test edilebilirlik eylemleri olarak ortaya çıkarır. Eylemler, belirli bir hata enjeksiyonuna, durum geçişine veya doğrulamaya neden olan düşük düzeyli API'lerdir. Bu eylemleri birleştirerek, hizmetleriniz için kapsamlı test senaryoları yazabilirsiniz.

Hizmet Kumaşı, bu eylemlerden oluşan bazı yaygın test senaryoları sağlar. Ortak durum geçişlerini ve hata durumlarını test etmek için özenle seçilen bu yerleşik senaryoları kullanmanızı şiddetle öneririz. Ancak, henüz yerleşik senaryolar tarafından kapsanmayan veya uygulamanız için özel olarak hazırlanmış senaryolar için kapsam eklemek istediğinizde eylemler özel test senaryoları oluşturmak için kullanılabilir.

Eylemlerin C# uygulamaları System.Fabric.dll derlemesinde bulunur. System Fabric PowerShell modülü Microsoft.ServiceFabric.Powershell.dll derlemesinde bulunur. Çalışma zamanı kurulumunun bir parçası olarak ServiceFabric PowerShell modülü kullanım kolaylığı sağlamak için yüklenir.

## <a name="graceful-vs-ungraceful-fault-actions"></a>Zarif vs. zarif hata eylemleri
Test edilebilirlik eylemleri iki ana kovaya ayrılır:

* Ungraceful hataları: Bu hatalar makine yeniden başlatmave işlem çökmeleri gibi hataları simüle. Bu tür hatalarda, işlemin yürütme bağlamı aniden durur. Bu, uygulama yeniden başlatmadan önce durum temizliğinin çalıştırılamaması anlamına gelir.
* Zarif hatalar: Bu hatalar, yük dengelemetarafından tetiklenen çoğaltma hareketleri ve düşmeler gibi zarif eylemleri simüle eder. Bu gibi durumlarda, hizmet yakın bir bildirim alır ve çıkmadan önce durumu temizleyebilirsiniz.

Daha iyi kalite doğrulama için, çeşitli zarif ve zarif hatalar adüklerken hizmet ve iş yükünü çalıştırın. Hizmet işleminin bazı iş akışının ortasında aniden çıktığı kusursuz hatalar senaryoları çalıştırın. Bu, hizmet çoğaltma Service Fabric tarafından geri yüklendikten sonra kurtarma yolunu sınar. Bu, veri tutarlılığını ve hizmet durumunun hatalardan sonra doğru şekilde korunup korunmamasına yardımcı olur. Diğer hata kümesi (zarif hatalar) hizmetin Service Fabric tarafından taşınan yinelemelere doğru tepki verdiğini sınar. Bu, RunAsync yönteminde iptal işleme testleri. Hizmetin ayarlanan iptal belirteci olup olup dedeği olup olma durumunu doğru bir şekilde kaydetmesi ve RunAsync yönteminden çıkması gerekir.

## <a name="testability-actions-list"></a>Test edilebilirlik eylemleri listesi
| Eylem | Açıklama | Yönetilen API | PowerShell cmdlet | Zarif/zarif olmayan hatalar |
| --- | --- | --- | --- | --- |
| TemizlemeTestState |Test sürücüsünün kötü kapanması durumunda kümedeki tüm test durumunu kaldırır. |TemizlemeTestStateAsync |Remove-ServiceFabricTestState |Uygulanamaz |
| InvokeDataLoss |Veri kaybını bir hizmet bölümüne neden olur. |InvokeDataLossAsync |Invoke-ServiceFabricPartitionDataLoss |Zarif |
| InvokeQuorumKaybı |Belirli bir devlet hizmeti bölünmesini çoğunluk kaybına dönüştürür. |InvokeQuorumLossAsync |Invoke-ServiceFabricQuorumLoss |Zarif |
| MovePrimary |Stateful service'in belirtilen birincil kopyasını belirtilen küme düğümüne taşır. |MovePrimaryAsync |Move-ServiceFabricPrimaryReplica |Zarif |
| MoveSecondary |Durum lu bir hizmetin geçerli ikincil yinelemesini farklı bir küme düğümüne taşır. |MoveSecondaryAsync |Move-ServiceFabricSecondaryReplica |Zarif |
| RemoveReplica |Bir yinelemeyi kümeden kaldırarak yineleme hatalarını simüle eder. Bu yinelemeyi kapatır ve tüm durumunu kümeden kaldırarak 'Yok' rolüne geçirecektir. |RemoveReplicaAsync |Remove-ServiceFabricReplica |Zarif |
| Yeniden Dağıtılmış KodPaketini Yeniden Başlatın |Kümedeki bir düğümüzerinde dağıtılan bir kod paketini yeniden başlatarak kod paketi hatası benzetimi. Bu, bu işlemde barındırılan tüm kullanıcı hizmeti yinelemelerini yeniden başlatacak olan kod paketi işlemini iptal eder. |Yeniden BaşlatDağıtılmışCodePackageAsync |Yeniden Başlat-HizmetFabricDeployedCodePackage |Lütufkar |
| Yeniden BaşlatmaNode |Bir düğümü yeniden başlatarak Hizmet Kumaşı küme düğümü hatalarını simüle eder. |RestartNodeAsync |Yeniden Başlat-ServiceFabricNode |Lütufkar |
| Yeniden BaşlatmaBölüm |Bir bölümün bazı veya tüm yinelemelerini yeniden başlatarak veri merkezi karartma veya küme karartma senaryosubenzetimi. |Yeniden BaşlatmaPartitionAsync |Restart-ServiceFabricPartition |Zarif |
| Yeniden BaşlatmaÇoğaltma |Bir kümede kalıcı bir yinelemeyi yeniden başlatarak, yinelemeyi kapatarak ve sonra yeniden açarak yineleme hatasını simüle eder. |Yeniden BaşlatmaReplicaAsync |Yeniden Başlat-ServiceFabricReplica |Zarif |
| Başlangıç Düğümü |Zaten durdurulmuş bir kümede düğüm başlatır. |StartNodeAsync |Start-ServiceFabricNode |Uygulanamaz |
| StopNode |Kümedeki düğümü durdurarak düğüm hatalarını simüle eder. Başlangıç Düğümü çağrılana kadar düğüm aşağıda kalır. |StopNodeAsync |Stop-ServiceFabricNode |Lütufkar |
| Uygulamayı Doğrulama |Genellikle sisteme bazı hatalar neden olduktan sonra, bir uygulama içindeki tüm Service Fabric hizmetlerinin kullanılabilirliğini ve durumunu doğrular. |UygulamaDoğrulaAsync |Test-ServisKumaş Uygulaması |Uygulanamaz |
| Hizmeti Doğrulama |Genellikle sisteme bazı hatalar neden olduktan sonra Bir Hizmet Kumaşı hizmetinin kullanılabilirliğini ve durumunu doğrular. |DoğrulaServiceAsync |Test-ServisKumaş Servisi |Uygulanamaz |

## <a name="running-a-testability-action-using-powershell"></a>PowerShell'i kullanarak test edilebilirlik eylemi çalıştırma
Bu öğretici, PowerShell'i kullanarak bir test edilebilirlik eylemini nasıl çalıştırabileceğinizi gösterir. Yerel (tek kutulu) kümeye veya Azure kümesine karşı sınayabilirlik eylemini nasıl çalıştıracağınızı öğreneceksiniz. Microsoft.Fabric.Powershell.dll--Service Fabric PowerShell modülü-- Microsoft Service Fabric MSI'ı yüklediğinizde otomatik olarak yüklenir. PowerShell istemini açtığınızda modül otomatik olarak yüklenir.

Öğretici bölümler:

* [Tek kutulu kümeye karşı eylem çalıştırma](#run-an-action-against-a-one-box-cluster)
* [Azure kümesine karşı bir eylem çalıştırma](#run-an-action-against-an-azure-cluster)

### <a name="run-an-action-against-a-one-box-cluster"></a>Tek kutulu kümeye karşı eylem çalıştırma
Yerel bir kümeye karşı bir sınayabilirlik eylemi çalıştırmak için önce kümeye bağlanın ve PowerShell istemini yönetici modunda açın. **Yeniden Başlatma-ServiceFabricNode eylemine** bakalım.

```powershell
Restart-ServiceFabricNode -NodeName Node1 -CompletionMode DoNotVerify
```

Burada eylem **Yeniden Başlatma-ServiceFabricNode** "Düğüm1" adlı bir düğüm üzerinde çalıştırılıyor. Tamamlama modu, yeniden başlatma düğümü eyleminin gerçekten başarılı olup olmadığını doğrulamaması gerektiğini belirtir. Tamamlanma modunu "Doğrula" olarak belirtmek, yeniden başlatma eyleminin gerçekten başarılı olup olmadığını doğrulamasına neden olur. Düğümü doğrudan adına göre belirtmek yerine, bir bölüm anahtarı ve yineleme türü aracılığıyla aşağıdaki gibi belirtebilirsiniz:

```powershell
Restart-ServiceFabricNode -ReplicaKindPrimary  -PartitionKindNamed -PartitionKey Partition3 -CompletionMode Verify
```


```powershell
$connection = "localhost:19000"
$nodeName = "Node1"

Connect-ServiceFabricCluster $connection
Restart-ServiceFabricNode -NodeName $nodeName -CompletionMode DoNotVerify
```

**Yeniden Başlat-ServiceFabricNode** kümedeki bir Hizmet Kumaş düğümini yeniden başlatmak için kullanılmalıdır. Bu, tüm sistem hizmetini ve bu düğümde barındırılan kullanıcı hizmeti yinelemelerini yeniden başlatan Fabric.exe işlemini durdurur. Hizmetinizi sınamak için bu API'yi kullanmak, kurtarma yolları üzerindeki hataların ortaya çıkarımına yardımcı olur. Kümedeki düğüm hatalarını simüle yardımcı olur.

Aşağıdaki ekran görüntüsü, yeniden **başlat-ServiceFabricNode** test edilebilirlik komutunu iş başında gösterir.

![](media/service-fabric-testability-actions/Restart-ServiceFabricNode.png)

İlk **Get-ServiceFabricNode'un** (Service Fabric PowerShell modülünden bir cmdlet) çıkışı, yerel kümenin beş düğüme sahip olduğunu gösterir: Düğüm.1-Düğüm.5. Test edilebilirlik eylemi (cmdlet) **Restart-ServiceFabricNode** düğüm üzerinde yürütüldükten sonra, Düğüm.4 adlı, düğümün çalışma süresinin sıfırlandığını görürsünüz.

### <a name="run-an-action-against-an-azure-cluster"></a>Azure kümesine karşı bir eylem çalıştırma
Bir Azure kümesine karşı bir sınayabilirlik eylemi (PowerShell kullanarak) çalıştırmak, eylemi yerel bir kümeye karşı çalıştırmaya benzer. Tek fark, eylemi çalıştırmadan önce, yerel kümeye bağlanmak yerine önce Azure kümesine bağlanmanız gerekir.

## <a name="running-a-testability-action-using-c35"></a>C&#35; kullanarak bir test edilebilirlik eylemi çalıştırma
C#kullanarak bir sınama eylemi çalıştırmak için öncelikle FabricClient'ı kullanarak kümeye bağlanmanız gerekir. Ardından eylemi çalıştırmak için gereken parametreleri edinin. Aynı eylemi çalıştırmak için farklı parametreler kullanılabilir.
RestartServiceFabricNode eylemine bakıldığında, bunu çalıştırmanın bir yolu kümedeki düğüm bilgilerini (düğüm adı ve düğüm örneği kimliği) kullanmaktır.

```csharp
RestartNodeAsync(nodeName, nodeInstanceId, completeMode, operationTimeout, CancellationToken.None)
```

Parametre açıklaması:

* **CompleteMode,** modun yeniden başlatma eyleminin gerçekten başarılı olup olmadığını doğrulamaması gerektiğini belirtir. Tamamlanma modunu "Doğrula" olarak belirtmek, yeniden başlatma eyleminin gerçekten başarılı olup olmadığını doğrulamasına neden olur.  
* **OperationTimeout,** Timeout Exception özel durum atılmadan önce işlemin tamamlanması için gereken süreyi ayarlar.
* **İptalToken** bekleyen bir aramanın iptal edilmesini sağlar.

Düğümü doğrudan adına göre belirtmek yerine, bir bölüm anahtarı ve yineleme türü yle belirtebilirsiniz.

Daha fazla bilgi için PartitionSelector ve ReplicaSelector bölümüne bakın.

```csharp
// Add a reference to System.Fabric.Testability.dll and System.Fabric.dll
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Fabric.Testability;
using System.Fabric;
using System.Threading;
using System.Numerics;

class Test
{
    public static int Main(string[] args)
    {
        string clusterConnection = "localhost:19000";
        Uri serviceName = new Uri("fabric:/samples/PersistentToDoListApp/PersistentToDoListService");
        string nodeName = "N0040";
        BigInteger nodeInstanceId = 130743013389060139;

        Console.WriteLine("Starting RestartNode test");
        try
        {
            //Restart the node by using ReplicaSelector
            RestartNodeAsync(clusterConnection, serviceName).Wait();

            //Another way to restart node is by using nodeName and nodeInstanceId
            RestartNodeAsync(clusterConnection, nodeName, nodeInstanceId).Wait();
        }
        catch (AggregateException exAgg)
        {
            Console.WriteLine("RestartNode did not complete: ");
            foreach (Exception ex in exAgg.InnerExceptions)
            {
                if (ex is FabricException)
                {
                    Console.WriteLine("HResult: {0} Message: {1}", ex.HResult, ex.Message);
                }
            }
            return -1;
        }

        Console.WriteLine("RestartNode completed.");
        return 0;
    }

    static async Task RestartNodeAsync(string clusterConnection, Uri serviceName)
    {
        PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);
        ReplicaSelector primaryofReplicaSelector = ReplicaSelector.PrimaryOf(randomPartitionSelector);

        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(primaryofReplicaSelector, CompletionMode.Verify);
    }

    static async Task RestartNodeAsync(string clusterConnection, string nodeName, BigInteger nodeInstanceId)
    {
        // Create FabricClient with connection and security information here
        FabricClient fabricclient = new FabricClient(clusterConnection);
        await fabricclient.FaultManager.RestartNodeAsync(nodeName, nodeInstanceId, CompletionMode.Verify);
    }
}
```

## <a name="partitionselector-and-replicaselector"></a>PartitionSelector ve ReplicaSelector
### <a name="partitionselector"></a>PartitionSelector
PartitionSelector test edilebilirlik maruz bir yardımcı ve herhangi bir sınanabilirlik eylemleri gerçekleştirmek için belirli bir bölüm seçmek için kullanılır. Bölüm kimliği önceden biliniyorsa, belirli bir bölümü seçmek için kullanılabilir. Veya, bölüm anahtarını sağlayabilir ve işlem bölüm kimliğini dahili olarak çözer. Ayrıca rasgele bir bölüm seçme seçeneğiniz de var.

Bu yardımcıyı kullanmak için PartitionSelector nesnesini oluşturun ve Select* yöntemlerinden birini kullanarak bölümü seçin. Ardından PartitionSelector nesnesini gerektiren API'ye geçirin. Seçenek seçili değilse, varsayılan olarak rasgele bir bölüme bölünür.

```csharp
Uri serviceName = new Uri("fabric:/samples/InMemoryToDoListApp/InMemoryToDoListService");
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
string partitionName = "Partition1";
Int64 partitionKeyUniformInt64 = 1;

// Select a random partition
PartitionSelector randomPartitionSelector = PartitionSelector.RandomOf(serviceName);

// Select a partition based on ID
PartitionSelector partitionSelectorById = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);

// Select a partition based on name
PartitionSelector namedPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionName);

// Select a partition based on partition key
PartitionSelector uniformIntPartitionSelector = PartitionSelector.PartitionKeyOf(serviceName, partitionKeyUniformInt64);
```

### <a name="replicaselector"></a>ReplicaSelector
ReplicaSelector test edilebilirlik maruz bir yardımcı ve herhangi bir sınanabilirlik eylemleri gerçekleştirmek için bir yineleme seçmenize yardımcı olmak için kullanılır. Çoğaltma kimliği önceden biliniyorsa, belirli bir yinelemeyi seçmek için kullanılabilir. Buna ek olarak, birincil yineleme veya rasgele bir ikincil seçme seçeneğiniz var. ReplicaSelector PartitionSelector türetilmiştir, bu nedenle hem çoğaltma ve test edilebilirlik işlemi gerçekleştirmek istediğiniz bölümü seçmeniz gerekir.

Bu yardımcıyı kullanmak için bir ReplicaSelector nesnesi oluşturun ve yinelemeyi ve bölümü seçmek istediğiniz şekilde ayarlayın. Daha sonra bunu gerektiren API'ye aktarabilirsiniz. Hiçbir seçenek seçili değilse, varsayılan olarak rasgele çoğaltma ve rasgele bölümleme olur.

```csharp
Guid partitionIdGuid = new Guid("8fb7ebcc-56ee-4862-9cc0-7c6421e68829");
PartitionSelector partitionSelector = PartitionSelector.PartitionIdOf(serviceName, partitionIdGuid);
long replicaId = 130559876481875498;

// Select a random replica
ReplicaSelector randomReplicaSelector = ReplicaSelector.RandomOf(partitionSelector);

// Select the primary replica
ReplicaSelector primaryReplicaSelector = ReplicaSelector.PrimaryOf(partitionSelector);

// Select the replica by ID
ReplicaSelector replicaByIdSelector = ReplicaSelector.ReplicaIdOf(partitionSelector, replicaId);

// Select a random secondary replica
ReplicaSelector secondaryReplicaSelector = ReplicaSelector.RandomSecondaryOf(partitionSelector);
```

## <a name="next-steps"></a>Sonraki adımlar
* [Test edilebilirlik senaryoları](service-fabric-testability-scenarios.md)
* Hizmetinizi nasıl test edin?
  * [Hizmet iş yükleri sırasında hataları simüle etmek](service-fabric-testability-workload-tests.md)
  * [Servise servise iletişim hataları](service-fabric-testability-scenarios-service-communication.md)

