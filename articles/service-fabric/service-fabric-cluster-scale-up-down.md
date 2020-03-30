---
title: Service Fabric kümesini içinde veya dışında ölçeklendirin
description: Her düğüm türü/sanal makine ölçeği kümesi için otomatik ölçek kuralları ayarlayarak talebi eşleşecek şekilde Hizmet Kumaşı kümesini ölçeklendirin. Service Fabric kümesine düğüm ekleme veya kaldırma
ms.topic: conceptual
ms.date: 03/12/2019
ms.openlocfilehash: 26ef13f38d525e4e493ad933bfb906dd36ed0070
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258739"
---
# <a name="scale-a-cluster-in-or-out"></a>Bir kümenin ölçeğini daraltma veya genişletme

> [!WARNING]
> Ölçeklendirmeden önce bu bölümü okuyun

Uygulama iş yükünü kaynak kaynak hesaplama küçşenmesi kasıtlı planlama gerektirir, neredeyse her zaman bir üretim ortamı için tamamlamak için bir saatten daha uzun sürer ve iş yükü ve iş bağlamını anlamak gerektirir; aslında bu etkinliği daha önce hiç yapmadıysanız, bu belgenin geri kalan kısmını devam etmeden önce [Service Fabric küme kapasite planlama hususlarını](service-fabric-cluster-capacity.md)okuyup anlamanız önerilir. Bu öneri, istenmeyen LiveSite sorunlarını önlemek içindir ve ayrıca gerçekleştirmeye karar verdiğiniz işlemleri üretim dışı bir ortama karşı başarıyla test etmeniz önerilir. İstediğiniz zaman [üretim sorunlarını bildirebilir veya Azure için ücretli destek isteyebilirsiniz.](service-fabric-support.md#report-production-issues-or-request-paid-support-for-azure) Uygun içeriğe sahip bu işlemleri gerçekleştirmek üzere ayrılan mühendisler için, bu makalede ölçekleme işlemleri açıklanır, ancak hangi işlemlerin kullanım durumunuz için uygun olduğuna karar vermeniz ve anlamanız gerekir; ölçeklendirecek kaynaklar (CPU, Depolama, Bellek), ölçeklendirecek hangi yön (Dikey veya Yatay) ve hangi işlemlerin gerçekleştirilmesi (Kaynak Şablonu dağıtımı, Portal, PowerShell/CLI) gibi.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="scale-a-service-fabric-cluster-in-or-out-using-auto-scale-rules-or-manually"></a>Bir Service Fabric kümesini otomatik ölçek kurallarını kullanarak veya el ile ölçeklendirin
Sanal makine ölçek kümeleri, sanal makine koleksiyonunu bir küme olarak dağıtmak ve yönetmek için kullanabileceğiniz bir Azure bilgi işlem kaynağıdır. Hizmet Kumaşı kümesinde tanımlanan her düğüm türü ayrı bir sanal makine ölçeği kümesi olarak ayarlanır. Her düğüm türü daha sonra bağımsız olarak ölçeklenebilir veya dışarı çıkarılabilir, farklı bağlantı noktaları kümeleri açık olabilir ve farklı kapasite ölçütlerine sahip olabilir. Service Fabric düğüm [türleri](service-fabric-cluster-nodetypes.md) belgesinde bu konuda daha fazla bilgi edinin. Kümenizdeki Servis Kumaş düğüm türleri arka uçtaki sanal makine ölçeği kümelerinden yapıldığından, her düğüm türü/sanal makine ölçeği kümesi için otomatik ölçek kuralları ayarlamanız gerekir.

> [!NOTE]
> Aboneliğiniz, bu kümeyi oluşturan yeni VM'leri eklemek için yeterli çekiye sahip olmalıdır. Şu anda model doğrulaması yoktur, bu nedenle kota sınırlarından herhangi biri vurulursa dağıtım süresi hatası alırsınız. Ayrıca tek bir düğüm türü sadece VMSS başına 100 düğüm geçemez. Hedeflenen ölçek elde etmek için VMSS eklemeniz gerekebilir ve otomatik ölçeklendirme Otomatik olarak VMSS's ekleyebilir. VMSS'nin yerinde canlı kümeye eklenmesi zor bir görevdir ve genellikle bu, kullanıcıların oluşturma zamanında sağlanan uygun düğüm türleri ile yeni kümeler sağlamasıyla sonuçlanır; buna göre [küme kapasitesini planlayın.](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) 
> 
> 

## <a name="choose-the-node-typevirtual-machine-scale-set-to-scale"></a>Ölçeklendirilecek düğüm türünü/Sanal Makine ölçeğini seçin
Şu anda, Bir Hizmet Kumaş Kümesi oluşturmak için portalı kullanarak sanal makine ölçeği kümeleri için otomatik ölçek kurallarını belirtemediğinizde, düğüm türlerini listelemek ve sonra bunlara otomatik ölçek kuralları eklemek için Azure PowerShell'i (1,0+) kullanmamıza izin verin.

Kümenizi oluşturan sanal makine ölçeği kümesinin listesini almak için aşağıdaki cmdlets'i çalıştırın:

```powershell
Get-AzResource -ResourceGroupName <RGname> -ResourceType Microsoft.Compute/VirtualMachineScaleSets

Get-AzVmss -ResourceGroupName <RGname> -VMScaleSetName <virtual machine scale set name>
```

## <a name="set-auto-scale-rules-for-the-node-typevirtual-machine-scale-set"></a>Düğüm türü/sanal makine ölçeği kümesi için otomatik ölçek kuralları ayarlama
Kümenizde birden çok düğüm türü varsa, ölçeklendirmek istediğiniz her düğüm türü/sanal makine ölçeği kümeleri (içinde veya dışında) için bunu yineleyin. Otomatik ölçeklendirmeyi ayarlamadan önce sahip olmanız gereken düğüm sayısını dikkate alın. Birincil düğüm türü için gereken düğüm sayısı alt sınırı seçmiş olduğunuz güvenilirlik düzeyi tarafından yönetilir. [Güvenilirlik düzeyleri](service-fabric-cluster-capacity.md)hakkında daha fazla bilgi edinin.

> [!NOTE]
> Birincil düğüm türünü en küçük sayıdan daha aza ölçeklemek kümeyi kararsız hale getirir veya aşağı indirir. Bu, uygulamalarınız ve sistem hizmetleri için veri kaybına neden olabilir.
> 
> 

Şu anda otomatik ölçek özelliği, uygulamalarınızın Service Fabric'e bildirdiği yükler tarafından yönlendirilmez. Yani şu anda aldığınız otomatik ölçek, sanal makine ölçeği kümesi örneklerinin her biri tarafından yayılan performans sayaçları tarafından yönlendirilir.  

[Her sanal makine ölçeği kümesi için otomatik ölçek ayarlamak için](../virtual-machine-scale-sets/virtual-machine-scale-sets-autoscale-overview.md)bu yönergeleri izleyin.

> [!NOTE]
> Bir ölçek küçültme senaryosunda, düğüm türünüz altın veya gümüş [dayanıklılık düzeyine][durability] sahip değilse, uygun düğüm adı ile [Remove-ServiceFabricNodeState cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) aramanız gerekir. Bronz dayanıklılık için, aynı anda birden fazla düğümü küçültmek önerilmez.
> 
> 

## <a name="manually-add-vms-to-a-node-typevirtual-machine-scale-set"></a>Düğüm türüne/sanal makine ölçeği kümesine vm'leri el ile ekleme

Ölçeği genişlettiğinizde, ölçek kümesine daha fazla sanal makine örneği eklersiniz. Bu örnekler, Service Fabric tarafından kullanılan düğümler haline gelir. Service Fabric, ölçek kümesine ne zaman daha fazla örnek eklendiğini bilir (ölçek genişletilerek) ve otomatik olarak tepki verir. 

> [!NOTE]
> VM'lerin eklenmesi zaman alır, bu nedenle eklemelerin anlık olmasını beklemeyin. Bu nedenle, vm kapasitesi, yinelemelerin/hizmet örneklerinin yerleştirilebilmesi için kullanılabilir hale gelmeden önce 10 dakikadan fazla bir süre için kapasite eklemeyi planlayın.
> 

### <a name="add-vms-using-a-template"></a>Şablon kullanarak VM ekleme
Her düğüm türündeki VM sayısını değiştirmek için [quickstart şablon galerisindeki](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) örnek/yönergeleri izleyin. 

### <a name="add-vms-using-powershell-or-cli-commands"></a>PowerShell veya CLI komutlarını kullanarak VM ekleme
Aşağıdaki kod, ada göre bir ölçek kümesi alır ve ölçek kümesinin **kapasitesini** 1 artırır.

```powershell
$scaleset = Get-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity += 1

Update-AzVmss -ResourceGroupName $scaleset.ResourceGroupName -VMScaleSetName $scaleset.Name -VirtualMachineScaleSet $scaleset
```

Bu kod, kapasiteyi 6 olarak ayarlar.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 6
```

## <a name="manually-remove-vms-from-a-node-typevirtual-machine-scale-set"></a>VM'leri düğüm türünden/sanal makine ölçeği kümesinden el ile kaldırma
Düğüm türünde ölçeklendirildiğinde, VM örneklerini ölçek kümesinden kaldırırsınız. Düğüm türü Bronz dayanıklılık düzeyi ise, Servis Kumaşı ne olduğundan habersizdir ve düğümün kaybolduğunu bildirir. Daha sonra Service Fabric, küme için kötü bir durum olduğunu bildirir. Bu kötü durumu önlemek için düğümü kümeden açıkça kaldırmanız ve düğüm durumunu kaldırmanız gerekir.

Hizmet kumaş sistemi hizmetleri kümenizdeki birincil düğüm türünde çalışır. Birincil düğüm türünü ölçeklendirirken, hiçbir zaman örnek sayısını [güvenilirlik katmanının](service-fabric-cluster-capacity.md) gerektirdiğinden daha azına ölçeklendirmeyin. 
 
Devlet hizmeti için, kullanılabilirliğini korumak ve hizmetinizin durumunu korumak için her zaman belirli sayıda düğüme ihtiyacınız vardır. En azından, bölüm/hizmetin hedef yineleme kümesi sayısına eşit düğüm sayısına ihtiyacınız vardır.

### <a name="remove-the-service-fabric-node"></a>Service Fabric düğümünü kaldırma

Düğüm durumunu el ile kaldırma adımları yalnızca *Bronz* dayanıklılık katmanına sahip düğüm türleri için geçerlidir.  *Gümüş* ve *Altın* dayanıklılık katmanı için bu adımlar platform tarafından otomatik olarak yapılır. Dayanıklılık hakkında daha fazla bilgi için bkz. [Service Fabric küme kapasitesi planlaması][durability].

Küme düğümlerini yükseltme ve hata etki alanlarına eşit olarak dağıtarak eşit bir şekilde kullanılmalarını sağlamak için önce en son oluşturulan düğümün kaldırılması gerekir. Başka bir deyişle düğümler, oluşturma sırasının tersine kaldırılmalıdır. En son oluşturulan düğüm, `virtual machine scale set InstanceId` özelliğinin değeri en yüksek olandır. Aşağıdaki kod örnekleri en son oluşturulan düğümü döndürür.

```powershell
Get-ServiceFabricNode | Sort-Object NodeInstanceId -Descending | Select-Object -First 1
```

```shell
sfctl node list --query "sort_by(items[*], &name)[-1]"
```

Service Fabric kümesinin, bu düğümün kaldırılacağını bilmesi gerekir. Uygulamanız gereken üç adım vardır:

1. Artık veriler için çoğaltma olmaması için düğümü devre dışı bırakın.  
PowerShell: `Disable-ServiceFabricNode`  
sfctl: `sfctl node disable`

2. Service Fabric çalışma zamanının düzgün şekilde kapanması ve uygulamanızın bir sonlandırma isteği alması için düğümü durdurun.  
PowerShell: `Start-ServiceFabricNodeTransition -Stop`  
sfctl: `sfctl node transition --node-transition-type Stop`

2. Kümeden düğümü kaldırın.  
PowerShell: `Remove-ServiceFabricNodeState`  
sfctl: `sfctl node remove-state`

Bu üç adım düğüme uygulandıktan sonra ölçek kümesinden kaldırılabilir. [Bronz][durability]’un yanı sıra dayanıklılık katmanları kullanıyorsanız ölçek kümesi örneği kaldırıldığında bu adımlar sizin için uygulanır.

Aşağıdaki kod bloğu, en son oluşturulan düğümü alır, düğümü devre dışı bırakır, durdurur ve kümeden kaldırır.

```powershell
#### After you've connected.....
# Get the node that was created last
$node = Get-ServiceFabricNode | Sort-Object { $_.NodeName.Substring($_.NodeName.LastIndexOf('_') + 1) } -Descending | Select-Object -First 1

# Node details for the disable/stop process
$nodename = $node.NodeName
$nodeid = $node.NodeInstanceId

$loopTimeout = 10

# Run disable logic
Disable-ServiceFabricNode -NodeName $nodename -Intent RemoveNode -TimeoutSec 300 -Force

$state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus

while (($state -ne [System.Fabric.Query.NodeStatus]::Disabled) -and ($loopTimeout -ne 0))
{
    Start-Sleep 5
    $loopTimeout -= 1
    $state = Get-ServiceFabricNode | Where-Object NodeName -eq $nodename | Select-Object -ExpandProperty NodeStatus
    Write-Host "Checking state... $state found"
}

# Exit if the node was unable to be disabled
if ($state -ne [System.Fabric.Query.NodeStatus]::Disabled)
{
    Write-Error "Disable failed with state $state"
}
else
{
    # Stop node
    $stopid = New-Guid
    Start-ServiceFabricNodeTransition -Stop -OperationId $stopid -NodeName $nodename -NodeInstanceId $nodeid -StopDurationInSeconds 300

    $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
    $loopTimeout = 10

    # Watch the transaction
    while (($state -eq [System.Fabric.TestCommandProgressState]::Running) -and ($loopTimeout -ne 0))
    {
        Start-Sleep 5
        $state = (Get-ServiceFabricNodeTransitionProgress -OperationId $stopid).State
        Write-Host "Checking state... $state found"
    }

    if ($state -ne [System.Fabric.TestCommandProgressState]::Completed)
    {
        Write-Error "Stop transaction failed with $state"
    }
    else
    {
        # Remove the node from the cluster
        Remove-ServiceFabricNodeState -NodeName $nodename -TimeoutSec 300 -Force
    }
}
```

Aşağıdaki **sfctl** kodunda, en son oluşturulan düğümün **node-name** değerini almak için şu komut kullanılır: `sfctl node list --query "sort_by(items[*], &name)[-1].name"`

```shell
# Inform the node that it is going to be removed
sfctl node disable --node-name _nt1vm_5 --deactivation-intent 4 -t 300

# Stop the node using a random guid as our operation id
sfctl node transition --node-instance-id 131541348482680775 --node-name _nt1vm_5 --node-transition-type Stop --operation-id c17bb4c5-9f6c-4eef-950f-3d03e1fef6fc --stop-duration-in-seconds 14400 -t 300

# Remove the node from the cluster
sfctl node remove-state --node-name _nt1vm_5
```

> [!TIP]
> Her bir adımın durumunu denetlemek için şu **sfctl** sorgularını kullanın
>
> **Devre dışı bırakma durumunu denetleme**
> `sfctl node list --query "sort_by(items[*], &name)[-1].nodeDeactivationInfo"`
>
> **Durdurma durumunu denetle**
> `sfctl node list --query "sort_by(items[*], &name)[-1].isStopped"`
>

### <a name="scale-in-the-scale-set"></a>Ölçek kümesinin ölçeğini daraltma

Şimdi Service Fabric düğümü kümeden kaldırıldığına göre sanal makine ölçek kümesinin ölçeği daraltılabilir. Aşağıdaki örnekte, ölçek kümesi kapasitesi 1 azaltılır.

```powershell
$scaleset = Get-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm
$scaleset.Sku.Capacity -= 1

Update-AzVmss -ResourceGroupName SFCLUSTERTUTORIALGROUP -VMScaleSetName nt1vm -VirtualMachineScaleSet $scaleset
```

Bu kod, kapasiteyi 5 olarak ayarlar.

```azurecli
# Get the name of the node with
az vmss list-instances -n nt1vm -g sfclustertutorialgroup --query [*].name

# Use the name to scale
az vmss scale -g sfclustertutorialgroup -n nt1vm --new-capacity 5
```

## <a name="behaviors-you-may-observe-in-service-fabric-explorer"></a>Service Fabric Explorer'da gözlemlenebilecek davranışlar
Bir kümeyi ölçeklendirdiğinizde, Hizmet Kumaş ı Gezgini kümenin bir parçası olan düğüm sayısını (sanal makine ölçeği kümesi örnekleri) yansıtır.  Ancak, bir kümeyi küçülttüyseniz, uygun düğüm adı ile [Remove-ServiceFabricNodeState cmd'yi](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate) aramadığınız sürece, sağlıksız bir durumda görüntülenen kaldırılan düğüm/VM örneğini görürsünüz.   

İşte bu davranışın açıklaması.

Service Fabric Explorer'da listelenen düğümler, Service Fabric sistem hizmetlerinin (özellikle FM) kümenin sahip olduğu/sahip olduğu düğüm sayısı hakkında ne bildiğini bir yansımasıdır. Sanal makine ölçeğini küçültttükten sonra VM silindi ancak FM sistem hizmeti yine de düğümün (silinen VM'ye eşlenen) geri geleceğini düşünüyor. Yani Hizmet Kumaş Explorer bu düğümü görüntülemeye devam ediyor (ancak sistem durumu hata veya bilinmiyor olabilir).

VM kaldırıldığında düğümün kaldırıldığından emin olmak için iki seçeneğiniz var:

1. Kümenizdeki düğüm türleri için altyapı tümleştirmesi sağlayan bir dayanıklılık düzeyi Altın veya Gümüş seçin. Bu da küçülttüldüğünde sistem hizmetlerimizden (FM) düğümleri otomatik olarak kaldırır.
Burada [dayanıklılık düzeyleri ile ilgili ayrıntılara](service-fabric-cluster-capacity.md) bakın

2. VM örneği küçültüldükten sonra [Remove-ServiceFabricNodeState cmdlet'i](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate)aramanız gerekir.

> [!NOTE]
> Service Fabric kümeleri kullanılabilirliğini korumak ve durumu korumak için her zaman belirli sayıda düğümün dolmasını gerektirir - "yeterli çoğunluk sağlamak" olarak adlandırılır. Bu nedenle, [ilk durum tam](service-fabric-reliable-services-backup-restore.md)bir yedekleme gerçekleştirmedikçe kümedeki tüm makineleri kapatmak için genellikle güvenli değildir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
Küme kapasitesini planlama, küme yükseltme ve hizmetleri bölümleme hakkında da bilgi edinmek için aşağıdakileri okuyun:

* [Küme kapasitenizi planlayın](service-fabric-cluster-capacity.md)
* [Küme yükseltmeleri](service-fabric-cluster-upgrade.md)
* [Maksimum ölçek için bölüm durumu hizmetleri](service-fabric-concepts-partitioning.md)

<!--Image references-->
[BrowseServiceFabricClusterResource]: ./media/service-fabric-cluster-scale-up-down/BrowseServiceFabricClusterResource.png
[ClusterResources]: ./media/service-fabric-cluster-scale-up-down/ClusterResources.png

[durability]: service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster
