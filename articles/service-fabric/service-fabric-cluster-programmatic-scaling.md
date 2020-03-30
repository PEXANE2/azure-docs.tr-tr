---
title: Azure Servis Kumaş Programlı Ölçekleme
description: Azure Hizmet Kumaşı kümesini özel tetikleyicilere göre programlı olarak veya dışında ölçeklendirin
author: mjrousos
ms.topic: conceptual
ms.date: 01/23/2018
ms.author: mikerou
ms.openlocfilehash: ffe07960c6d32bea8ec31b1fe8248b6abc2b63af
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75458294"
---
# <a name="scale-a-service-fabric-cluster-programmatically"></a>Hizmet Kumaşı kümesini programlı olarak ölçeklendirin 

Azure'da çalışan Service Fabric kümeleri, sanal makine ölçek kümelerinin üzerine inşa edilmiştir.  [Küme ölçeklendirme,](./service-fabric-cluster-scale-up-down.md) Service Fabric kümelerinin el ile veya otomatik ölçekkurallarıyla nasıl ölçeklendirilebildiğini açıklar. Bu makalede, daha gelişmiş bir senaryo olan akıcı Azure işlem SDK'sını kullanarak kimlik bilgilerini nasıl yönetip bir kümeyi nasıl veya dışarı ölçeklendirebilirsiniz. Genel bakış için, [Azure ölçekleme işlemlerini koordine etmek için programlı yöntemleri](service-fabric-cluster-scaling.md#programmatic-scaling)okuyun. 


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="manage-credentials"></a>Kimlik bilgilerini yönetme
Ölçeklendirme işlemek için bir hizmet yazma bir sorun, hizmet etkileşimli bir giriş olmadan sanal makine ölçeği ayarlanmış kaynaklara erişmek gerekir. Ölçeklendirme hizmeti kendi Service Fabric uygulamasını değiştiriyorsa, Hizmet Kumaşı kümesine erişmek kolaydır, ancak ölçek kümesine erişmek için kimlik bilgileri gereklidir. Oturum açabilmek için [Azure CLI](https://github.com/azure/azure-cli)ile oluşturulan bir [hizmet ilkesini](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli) kullanabilirsiniz.

Bir hizmet sorumlusu aşağıdaki adımlarla oluşturulabilir:

1. Sanal makine ölçeği kümesine`az login`erişimi olan bir kullanıcı olarak Azure CLI ' da oturum açın
2. Servis prensibini oluşturma`az ad sp create-for-rbac`
    1. AppId (başka bir yerde 'istemci kimliği' olarak adlandırılır), adını, parolasını ve kiracıyı daha sonra kullanmak üzere not alın.
    2. Ayrıca abonelik kimliğinizi de`az account list`

Akıcı bilgi işlem kitaplığı bu kimlik bilgilerini aşağıdaki gibi kullanarak oturum açabilir `IAzure` [(Microsoft.Azure.Management.Fluent](https://www.nuget.org/packages/Microsoft.Azure.Management.Fluent/) paketinde olduğu gibi akıcı azure türlerinin olduğunu unutmayın):

```csharp
var credentials = new AzureCredentials(new ServicePrincipalLoginInformation {
                ClientId = AzureClientId,
                ClientSecret = 
                AzureClientKey }, AzureTenantId, AzureEnvironment.AzureGlobalCloud);
IAzure AzureClient = Azure.Authenticate(credentials).WithSubscription(AzureSubscriptionId);

if (AzureClient?.SubscriptionId == AzureSubscriptionId)
{
    ServiceEventSource.Current.ServiceMessage(Context, "Successfully logged into Azure");
}
else
{
    ServiceEventSource.Current.ServiceMessage(Context, "ERROR: Failed to login to Azure");
}
```

Oturum açtıktan sonra, ölçek kümesi örnek `AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId).Capacity`sayısı üzerinden sorgulanabilir.

## <a name="scaling-out"></a>Ölçeği genişletme
Akıcı Azure bilgi işlem SDK'sı kullanılarak, örnekler sanal makine ölçeğine sadece birkaç çağrıyla eklenebilir -

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
``` 

Alternatif olarak, sanal makine ölçeği seti boyutu PowerShell cmdlets ile de yönetilebilir. [`Get-AzVmss`](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss)sanal makine ölçeği kümesi nesnesi alabilirsiniz. Geçerli kapasite `.sku.capacity` özellik üzerinden kullanılabilir. Kapasite yi istenilen değere değiştirdikten sonra, Azure'da ayarlanan [`Update-AzVmss`](https://docs.microsoft.com/powershell/module/az.compute/update-azvmss) sanal makine ölçeği komutuyla güncellenebilir.

Bir düğüm el ile eklerken olduğu gibi, ölçek kümesi şablonu Hizmet Kumaşı kümesine otomatik olarak yeni örnekleri birleştirmek için uzantılar içerdiğinden, yeni bir Hizmet Kumaş düğümü başlatmak için gereken tek ölçek kümesi örneği olmalıdır. 

## <a name="scaling-in"></a>Ölçekleme

Ölçekleme, ölçekleme ile benzer. Gerçek sanal makine ölçeği kümesi değişiklikleri hemen hemen aynıdır. Ancak, daha önce de tartışıldığı gibi, Service Fabric yalnızca kaldırılan düğümleri Altın veya Gümüş dayanıklılığıyla otomatik olarak temizler. Bu nedenle, Bronz dayanıklılık ölçeğinde, kaldırılacak düğümü kapatmak ve daha sonra durumunu kaldırmak için Service Fabric kümesi ile etkileşime girmeli.

Düğümü kapatmaya hazırlamak düğümün kaldırılacak (en son eklenen sanal makine ölçeği örneği) bulunmasını ve devre dışı bırakılmasını içerir. Sanal makine ölçeği kümesi örnekleri eklendikleri sıraya göre numaralandırılır, böylece düğümadlarının adlarında (altta yatan sanal makine ölçeği kümesi örnek adlarıyla eşleşen) sayı soneki karşılaştırılarak daha yeni düğümler bulunabilir. 

```csharp
using (var client = new FabricClient())
{
    var mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync())
        .Where(n => n.NodeType.Equals(NodeTypeToScale, StringComparison.OrdinalIgnoreCase))
        .Where(n => n.NodeStatus == System.Fabric.Query.NodeStatus.Up)
        .OrderByDescending(n =>
        {
            var instanceIdIndex = n.NodeName.LastIndexOf("_");
            var instanceIdString = n.NodeName.Substring(instanceIdIndex + 1);
            return int.Parse(instanceIdString);
        })
        .FirstOrDefault();
```

Kaldırılacak düğüm bulunduktan sonra, aynı `FabricClient` örnek ve önceki `IAzure` örnek kullanılarak devre dışı bırakılabilir ve kaldırılabilir.

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shutdown
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement VMSS capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

Ölçekleme dışında olduğu gibi, sanal makine ölçeği set kapasitesini değiştirmek için PowerShell cmdlets bir komut dosyası yaklaşımı tercih edilirse burada da kullanılabilir. Sanal makine örneği kaldırıldıktan sonra, Service Fabric düğümü durumu kaldırılabilir.

```csharp
await client.ClusterManager.RemoveNodeStateAsync(mostRecentLiveNode.NodeName);
```

## <a name="next-steps"></a>Sonraki adımlar

Kendi otomatik ölçekleme mantığınızı uygulamaya başlamak için, aşağıdaki kavramları ve yararlı API'leri edin:

- [El ile veya otomatik ölçek kurallarıyla ölçekleme](./service-fabric-cluster-scale-up-down.md)
- [.NET için akıcı Azure Yönetim Kitaplıkları](https://github.com/Azure/azure-sdk-for-net/tree/Fluent) (Service Fabric kümesinin altında yatan sanal makine ölçek kümeleriyle etkileşim kurmak için kullanışlıdır)
- [System.Fabric.FabricClient](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient) (Servis Kumaş ı küme ve düğümleri ile etkileşim için yararlıdır)
