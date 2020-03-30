---
title: Azure Hizmet Kumaşı için kapasite planlama ve ölçekleme
description: Hizmet Kumaş kümeleri ve uygulamaları planlama ve ölçeklendirme için en iyi uygulamalar.
author: peterpogorski
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: pepogors
ms.openlocfilehash: bf228e17ca24df9833f96f0c6fd3ef232cdf7ae6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258999"
---
# <a name="capacity-planning-and-scaling-for-azure-service-fabric"></a>Azure Hizmet Kumaşı için kapasite planlama ve ölçekleme

Herhangi bir Azure Hizmet Kumaşı kümesi oluşturmadan veya kümenizi barındıran bilgi işlem kaynaklarını ölçeklendirmeden önce, kapasite için plan yapmak önemlidir. Kapasite planlaması hakkında daha fazla bilgi için hizmet [kumaşı küme kapasitesini planlama ya](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)da bkz. Küme ölçeklenebilirliği için daha iyi uygulama kılavuzu için [Service Fabric ölçeklenebilirlik hususlarına](https://docs.microsoft.com/azure/architecture/reference-architectures/microservices/service-fabric#scalability-considerations)bakın.

Düğüm türü ve küme özelliklerini göz önünde bulundurmanın yanı sıra, ölçekleme işlemlerinin bir üretim ortamı için tamamlanması bir saatten uzun sürer beklemeniz gerekir. Bu husus, eklediğiniz VM sayısına bakılmaksızın doğrudur.

## <a name="autoscaling"></a>Otomatik ölçeklendirme
[Kaynak yapılandırmalarını kod olarak]( https://docs.microsoft.com/azure/service-fabric/service-fabric-best-practices-infrastructure-as-code)ele almak en iyi uygulama olduğundan, Azure Kaynak Yöneticisi şablonları aracılığıyla ölçekleme işlemleri gerçekleştirmeniz gerekir. 

Sanal makine ölçek kümeleri aracılığıyla otomatik ölçeklendirme kullanmak, sürümlenmiş Kaynak Yöneticisi şablonunuzun sanal makine ölçek kümeleri için örnek sayılarınızı yanlış tanımlamasına neden olur. Hatalı tanım, gelecekteki dağıtımların istenmeyen ölçekleme işlemlerine neden olması riskini artırır. Genel olarak, şu durumlarda otomatik ölçekleme kullanmalısınız:

* Kaynak Yöneticisi şablonlarınızı uygun kapasiteyle dağıtmak kullanım durumunuzu desteklemez.
     
   El ile ölçeklemenin yanı sıra, [Azure kaynak grubu dağıtım projelerini kullanarak Azure DevOps Hizmetlerinde Sürekli tümleştirme ve teslim alma ardışık bir yapılandırma](https://docs.microsoft.com/azure/vs-azure-tools-resource-groups-ci-in-vsts)yapabilirsiniz. Bu ardışık işlem genellikle [Azure Monitor REST API'den](https://docs.microsoft.com/azure/azure-monitor/platform/rest-api-walkthrough)sorgulanan sanal makine performans ölçümlerini kullanan bir mantık uygulaması tarafından tetiklenir. Kaynak Yöneticisi şablonlarını optimize ederken ardışık alan, istediğiniz ölçümleri temel alarak etkili bir şekilde otomatik ölçeklendirin.
* Bir defada yalnızca bir sanal makine ölçeği kümesi düğümlerini yatay olarak ölçeklendirmeniz gerekir.
   
   Aynı anda üç veya daha fazla düğüme göre ölçeklendirmek için, [sanal makine ölçek kümesi ekleyerek Bir Hizmet Kumaş ı kümesini ölçeklendirmeniz](virtual-machine-scale-set-scale-node-type-scale-out.md)gerekir. Sanal makine ölçeği kümelerini yatay olarak ölçeklendirmek ve ölçeklendirmek en güvenlisi, her seferinde bir düğüm.
* Service Fabric kümeniz için Gümüş güvenilirliğine veya daha yüksek bir güvenliğe ve otomatik ölçeklendirme kurallarını yapılandırdığınız herhangi bir ölçekte Gümüş dayanıklılığa veya daha yüksek olansilver dayanıklılığına sahipsiniz.
  
   Otomatik ölçekleme kuralları için minimum kapasite, beş sanal makine örneğine eşit veya daha büyük olmalıdır. Ayrıca, birincil düğüm türünüz için Güvenilirlik Katmanı minimumuna eşit veya daha büyük olmalıdır.

> [!NOTE]
> Service Fabric stateful servis kumaşı:/System/InfastructureService/<NODE_TYPE_NAME>, Gümüş veya daha yüksek dayanıklılığa sahip her düğüm türünde çalışır. Küme düğüm türlerinin herhangi birinde Azure'da çalıştırılmak üzere desteklenen tek sistem hizmetidir.

## <a name="vertical-scaling-considerations"></a>Dikey ölçekleme hususları

Azure Hizmet Kumaşı'nda bir düğüm türünü [dikey ölçekleme](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out) için birkaç adım ve dikkat edilmesi gereken ler vardır. Örnek:

* Küme ölçekleme önce sağlıklı olmalıdır. Aksi takdirde, kümenin daha da dengesini bozarsınız.
* Gümüş dayanıklılık düzeyi veya daha büyük tüm Service Fabric küme düğüm türleri için gerekli olan stateful hizmetleri barındıran.

> [!NOTE]
> Stateful Service Fabric sistem hizmetlerini barındıran birincil düğüm türünüz Gümüş dayanıklılık düzeyi veya daha büyük olmalıdır. Gümüş dayanıklılığını etkinleştirdikten sonra, sistem işlem hızında veri güvenliği için optimizasyon yaptığından, yükseltmeler, düğümlerin eklenmesi veya kaldırılması gibi küme işlemleri daha yavaş olacaktır.

Sanal makine ölçek kümesinin dikey ölçeklendirmesi yıkıcı bir işlemdir. Bunun yerine, istediğiniz SKU ile yeni bir ölçek kümesi ekleyerek kümenizi yatay olarak ölçeklendirin. Ardından, güvenli bir dikey ölçekleme işlemini tamamlamak için hizmetlerinizi istediğiniz SKU'ya geçirin. Sanal makine ölçeği kümesi kaynağı SKU'nun değiştirilmesi yıkıcı bir işlemdir, çünkü ana bilgisayarlarınızı yeniden görüntüler ve bu işlem yerel olarak kalıcı olan tüm durumu ortadan kaldırır.

Kümeniz, uygulamanızın hizmetlerini nerede barındıracağınıza karar vermek için Service Fabric [düğüm özelliklerini ve yerleşim kısıtlamalarını](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#node-properties-and-placement-constraints) kullanır. Birincil düğüm türünüzü dikey olarak ölçeklediğinizde, aynı özellik `"nodeTypeRef"`değerlerini ' için bildirin. Bu değerleri sanal makine ölçek kümeleri için Hizmet Kumaşı uzantısında bulabilirsiniz. 

Kaynak Yöneticisi şablonunun aşağıdaki snippet'i bildireceğiniz özellikleri gösterir. Ölçeklediğiniz yeni sağlanan ölçek kümeleri için aynı değere sahiptir ve yalnızca kümeniz için geçici bir durum hizmeti olarak desteklenir.

```json
"settings": {
   "nodeTypeRef": ["[parameters('primaryNodetypeName')]"]
}
```

> [!NOTE]
> Başarılı bir dikey ölçeklendirme işlemini tamamlamak için `nodeTypeRef` gerekenden daha uzun süre aynı özellik değerini kullanan birden çok ölçek kümesiyle kümenizi çalışır durumda bırakmayın.
>
> Üretim ortamında değişiklik denemeden önce her zaman test ortamlarında işlemleri doğrulayın. Varsayılan olarak, Service Fabric küme sistemi hizmetlerinin yalnızca hedef birincil düğüm türüne yönelik bir yerleşim kısıtlaması vardır.

Düğüm özellikleri ve yerleşim kısıtlamaları beyan edilegeldiğinde, aşağıdaki adımları bir defada bir VM örneği yapın. Bu, sistem hizmetlerinin (ve devlet hizmetlerinin) başka bir yerde yeni yinelemeler oluşturulduğunda kaldırdığınız VM örneğinde incelikle kapatılmasını sağlar.

1. PowerShell'den, `Disable-ServiceFabricNode` çıkaracağınız düğümü devre dışı kaldırmak amacıyla `RemoveNode` çalıştırın. En yüksek sayıya sahip düğüm türünü kaldırın. Örneğin, altı düğümlü bir kümeniz varsa, "MyNodeType_5" sanal makine örneğini kaldırın.
2. Düğümün devre dışı bırakıldığından emin olmak için çalıştırın. `Get-ServiceFabricNode` Değilse, düğüm devre dışı bırakılana kadar bekleyin. Bu, her düğüm için birkaç saat sürebilir. Düğüm devre dışı bırakılana kadar devam etmeyin.
3. Bu düğüm türünde VM sayısını bir er olarak azaltın. En yüksek VM örneği şimdi kaldırılır.
4. Gerektiğinde 1'den 3'e kadar olan adımları yineleyin, ancak birincil düğüm türlerindeki örnek sayısını hiçbir zaman güvenilirlik katmanının gerektirdiğinden daha az küçültmeyin. Önerilen örneklerin listesi için [Hizmet Kumaşı küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) bölümüne bakın.
5. Tüm VM'ler gittikten sonra ("Aşağı" olarak temsil edilir) kumaş:/System/InfrastructureService/[düğüm adı] bir Hata durumu gösterir. Ardından, düğüm türünü kaldırmak için küme kaynağını güncelleştirebilirsiniz. ARM şablon dağıtımını kullanabilir veya [Azure kaynak yöneticisi](https://resources.azure.com)aracılığıyla küme kaynağını edebilirsiniz. Bu, hata durumundaolan kumaş:/System/InfrastructureService/[düğüm türü] hizmetini kaldıracak bir küme yükseltmesi başlatacaktır.
 6. Bundan sonra isteğe bağlı olarak VMScaleSet silebilir, yine de Hizmet Kumaş Explorer görünümünden "Aşağı" olarak düğümleri göreceksiniz. Son adım onları komutla `Remove-ServiceFabricNodeState` temizlemek olacaktır.

## <a name="horizontal-scaling"></a>Yatay ölçekleme

Yatay ölçekleme işlemi [el ile](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down) veya [programlı olarak](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-programmatic-scaling)yapabilirsiniz.

> [!NOTE]
> Gümüş veya Altın dayanıklılığına sahip bir düğüm türünü ölçekleiyorsanız, ölçekleme yavaş olacaktır.

### <a name="scaling-out"></a>Ölçeği genişletme

Belirli bir sanal makine ölçeği kümesinin örnek sayısını artırarak Hizmet Kumaşı kümesini ölçeklendirin. Kapasiteyi artırmak için istenilen `AzureClient` ölçek kümesini kullanarak ve kimliği kullanarak programlı olarak ölçeklendirebilirsiniz.

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);
var newCapacity = (int)Math.Min(MaximumNodeCount, scaleSet.Capacity + 1);
scaleSet.Update().WithCapacity(newCapacity).Apply(); 
```

El ile ölçeklendirmek için, istenen sanal makine [ölçeği kümesi](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) kaynağının SKU özelliğindeki kapasiteyi güncelleştirin.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

### <a name="scaling-in"></a>Ölçekleme

Ölçekleme, ölçeklemeden daha fazla dikkate alınmasını gerektirir. Örneğin:

* Service Fabric sistem hizmetleri kümenizdeki birincil düğüm türünde çalışır. Güvenilirlik katmanının gerektirdiğinden daha az örneğe sahip olmak için bu düğüm türüne ait örnek sayısını asla kapatmayın veya küçültmeyin. 
* Devlet hizmeti için, kullanılabilirliği korumak ve hizmetinizin durumunu korumak için her zaman hazır olan belirli sayıda düğüme ihtiyacınız vardır. En azından, bölüm veya hizmetin hedef yineleme kümesi sayısına eşit bir düğüm sayısına ihtiyacınız vardır.

El ile ölçeklendirmek için aşağıdaki adımları izleyin:

1. PowerShell'den, `Disable-ServiceFabricNode` çıkaracağınız düğümü devre dışı kaldırmak amacıyla `RemoveNode` çalıştırın. En yüksek sayıya sahip düğüm türünü kaldırın. Örneğin, altı düğümlü bir kümeniz varsa, "MyNodeType_5" sanal makine örneğini kaldırın.
2. Düğümün devre dışı bırakıldığından emin olmak için çalıştırın. `Get-ServiceFabricNode` Değilse, düğüm devre dışı bırakılana kadar bekleyin. Bu, her düğüm için birkaç saat sürebilir. Düğüm devre dışı bırakılana kadar devam etmeyin.
3. Bu düğüm türünde VM sayısını bir er olarak azaltın. En yüksek VM örneği şimdi kaldırılır.
4. İstediğiniz kapasiteyi sağlanana kadar gerektiğinde 1'den 3'e kadar adımları yineleyin. Birincil düğüm türlerindeki örnek sayısını, güvenilirlik katmanının gerektirdiğinden daha azına küçültmeyin. Önerilen örneklerin listesi için [Hizmet Kumaşı küme kapasitesini planlama](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) bölümüne bakın.

El ile ölçeklendirmek için, istenen sanal makine [ölçeği kümesi](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile) kaynağının SKU özelliğindeki kapasiteyi güncelleştirin.

```json
"sku": {
    "name": "[parameters('vmNodeType0Size')]",
    "capacity": "[parameters('nt0InstanceCount')]",
    "tier": "Standard"
}
```

Programlı olarak ölçeklendirmek için düğümü kapatmaya hazırlamanız gerekir. Kaldırılacak düğümü bulun (en yüksek örnek düğüm). Örnek:

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

Önceki kodda kullandığınız aynı `FabricClient` örneği (bu`client` durumda) ve düğüm örneğini (bu`instanceIdString` durumda) kullanarak düğümü devre dışı bırakın ve kaldırın:

```csharp
var scaleSet = AzureClient.VirtualMachineScaleSets.GetById(ScaleSetId);

// Remove the node from the Service Fabric cluster
ServiceEventSource.Current.ServiceMessage(Context, $"Disabling node {mostRecentLiveNode.NodeName}");
await client.ClusterManager.DeactivateNodeAsync(mostRecentLiveNode.NodeName, NodeDeactivationIntent.RemoveNode);

// Wait (up to a timeout) for the node to gracefully shut down
var timeout = TimeSpan.FromMinutes(5);
var waitStart = DateTime.Now;
while ((mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Up || mostRecentLiveNode.NodeStatus == System.Fabric.Query.NodeStatus.Disabling) &&
        DateTime.Now - waitStart < timeout)
{
    mostRecentLiveNode = (await client.QueryManager.GetNodeListAsync()).FirstOrDefault(n => n.NodeName == mostRecentLiveNode.NodeName);
    await Task.Delay(10 * 1000);
}

// Decrement virtual machine scale set capacity
var newCapacity = (int)Math.Max(MinimumNodeCount, scaleSet.Capacity - 1); // Check min count 

scaleSet.Update().WithCapacity(newCapacity).Apply();
```

> [!NOTE]
> Bir kümeyi küçülttüyseniz, Hizmet Kumaş ı Gezgini'nde sağlıksız bir durumda görüntülenen kaldırılan düğüm/VM örneğini görürsünüz. Bu davranışın açıklaması için Service [Fabric Explorer'da gözlemlenebilecek Davranışlar bölümüne](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-scale-up-down#behaviors-you-may-observe-in-service-fabric-explorer)bakın. Şunları yapabilirsiniz:
> * Uygun düğüm adı ile [Remove-ServiceFabricNodeState komutunu](https://docs.microsoft.com/powershell/module/servicefabric/remove-servicefabricnodestate?view=azureservicefabricps) arayın.
> * Service [Fabric otomatik ölçeklendirme yardımcı uygulamasını](https://github.com/Azure/service-fabric-autoscale-helper/) kümenizde dağıtın. Bu uygulama, küçültülmüş düğümlerin Service Fabric Explorer'dan temizlenmesini sağlar.

## <a name="reliability-levels"></a>Güvenilirlik düzeyleri

[Güvenilirlik düzeyi,](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity) Service Fabric küme kaynağınızın bir özelliğidir. Tek tek düğüm türleri için farklı yapılandırılamaz. Küme için sistem hizmetlerinin çoğaltma faktörlerini denetler ve küme kaynak düzeyinde bir ayardır. 

Güvenilirlik düzeyi, birincil düğüm türünizin sahip olması gereken en az düğüm sayısını belirler. Güvenilirlik katmanı aşağıdaki değerleri alabilir:

* Platin: Sistem hizmetlerini yedi ve dokuz tohum düğümü sayısı yla bir hedef çoğaltma kümesi yle çalıştırıyor.
* Altın: Sistem hizmetlerini yedi ve yedi tohum düğümü sayısı yla bir hedef çoğaltma kümesi yle çalıştırın.
* Gümüş: Sistem hizmetlerini beş ve beş tohum düğümü nden oluşan bir hedef çoğaltma kümesi sayısıyla çalıştırın.
* Bronz: Üç ve üç tohum düğümleri bir hedef çoğaltma kümesi sayısı ile sistem hizmetlerini çalıştırın.

Önerilen minimum güvenilirlik düzeyi Gümüş'tir.

Güvenilirlik [düzeyi, Microsoft.ServiceFabric/clusters kaynağının](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters)özellikleri bölümünde şu şekilde ayarlanır:

```json
"properties":{
    "reliabilityLevel": "Silver"
}
```

## <a name="durability-levels"></a>Dayanıklılık seviyeleri

> [!WARNING]
> Bronz dayanıklılık ile çalışan düğüm türleri _hiçbir ayrıcalık_elde. Durum dolmayan iş yüklerinizi etkileyen altyapı işleri durdurulamaz veya geciktirilmez ve bu da iş yüklerinizi etkileyebilir. 
>
> Yalnızca durum ifadesüz iş yüklerini çalıştıran düğüm türleri için Bronz dayanıklılık kullanın. Üretim iş yükleri için, durum tutarlılığı sağlamak için Gümüş veya daha yüksek çalıştırın. [Kapasite planlama belgelerindeki](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity)kılavuza göre doğru güvenilirliği seçin.

Dayanıklılık düzeyi iki kaynak ta ayarlanmalıdır. Bir [sanal makine ölçeği kümesi kaynağının](https://docs.microsoft.com/rest/api/compute/virtualmachinescalesets/createorupdate#virtualmachinescalesetosprofile)uzantısı profili:

```json
"extensionProfile": {
    "extensions":          {
        "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
        "properties": {
            "settings": {
                "durabilityLevel": "Bronze"
            }
        }
    }
}
```

Diğer kaynak `nodeTypes` [Microsoft.ServiceFabric/clusters kaynağında](https://docs.microsoft.com/azure/templates/microsoft.servicefabric/2018-02-01/clusters)altındadır: 

```json
"nodeTypes": [
    {
        "name": "[variables('vmNodeType0Name')]",
        "durabilityLevel": "Bronze"
    }
]
```

## <a name="next-steps"></a>Sonraki adımlar

* Windows Server çalıştıran VM'lerde veya bilgisayarlarda küme [oluşturma: Windows Server için Hizmet Kumaşı küme oluşturma.](service-fabric-cluster-creation-for-windows-server.md)
* Linux çalıştıran VM'lerde veya bilgisayarlarda bir küme oluşturun: [Linux kümesi oluşturun.](service-fabric-cluster-creation-via-portal.md)
* Service [Fabric destek seçenekleri](service-fabric-support.md)hakkında bilgi edinin.

[Image1]: ./media/service-fabric-best-practices/generate-common-name-cert-portal.png
