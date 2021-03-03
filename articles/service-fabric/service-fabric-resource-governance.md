---
title: Kapsayıcılar ve hizmetler için kaynak idaresi
description: Azure Service Fabric, işlem veya kapsayıcı olarak çalışan hizmetler için kaynak isteklerini ve sınırlarını belirtmenize olanak tanır.
ms.topic: conceptual
ms.date: 8/9/2017
ms.openlocfilehash: d760766870c8c2be0a2d2384f6d012b75bc92fbd
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101735667"
---
# <a name="resource-governance"></a>Kaynak idaresi

Aynı düğüm veya kümede birden çok hizmet çalıştırırken, bir hizmetin daha fazla kaynak tüketmesi ve bu da işlemdeki diğer hizmetlerden kaynaklanabilir. Bu sorun, "gürültülü komşu" sorunu olarak adlandırılır. Azure Service Fabric, kaynak kullanımını sınırlamak üzere hizmet başına istekleri ve sınırları belirterek geliştiricinin bu davranışı denetlemesini sağlar.

> Bu makaleye devam etmeden önce [Service Fabric uygulama modeli][application-model-link] ve [Service Fabric barındırma modeli][hosting-model-link]hakkında bilgi sahibi olmanız önerilir.
>

## <a name="resource-governance-metrics"></a>Kaynak idare ölçümleri

Kaynak İdaresi, [hizmet paketine][application-model-link]uygun olarak Service Fabric desteklenir. Hizmet paketine atanan kaynaklar kod paketleri arasında daha da ayrılabilir. Service Fabric, iki yerleşik [ölçüm](service-fabric-cluster-resource-manager-metrics.md)ile hizmet PAKETI başına CPU ve bellek yönetimini destekler:

* *CPU* (ölçüm adı `servicefabric:/_CpuCores` ): konak makinede bulunan mantıksal çekirdek. Tüm düğümlerdeki tüm çekirdekler aynı şekilde ağırlıklı olarak dağıtılır.

* *Bellek* (ölçüm adı `servicefabric:/_MemoryInMB` ): bellek megabayt cinsinden ifade edilir ve makinede bulunan fiziksel bellekle eşlenir.

Bu iki ölçüm için, [küme kaynak yöneticisi (CRM)][cluster-resource-manager-description-link] toplam küme kapasitesini, kümedeki her düğümdeki yükü ve kümedeki kalan kaynakları izler. Bu iki ölçüm, diğer Kullanıcı veya özel ölçümle eşdeğerdir. Var olan tüm özellikler bunlarla birlikte kullanılabilir:

* Küme, bu iki ölçümlere (varsayılan davranış) göre [dengelenebilir](service-fabric-cluster-resource-manager-balancing.md) .
* Küme, bu iki ölçümlere göre [birleştirilebilirler](service-fabric-cluster-resource-manager-defragmentation-metrics.md) .
* [Bir kümeyi açıkladığında][cluster-resource-manager-description-link], bu iki ölçüm için arabelleğe alınan kapasite ayarlanabilir.

> [!NOTE]
> [Dinamik yük raporlama](service-fabric-cluster-resource-manager-metrics.md) bu ölçümler için desteklenmez; Bu ölçümler için yük oluşturma sırasında tanımlanmıştır.

## <a name="resource-governance-mechanism"></a>Kaynak idare mekanizması

Sürüm 7,2 ' den başlayarak Service Fabric çalışma zamanı, CPU ve bellek kaynakları için isteklerin ve limitlerin belirtimini destekler.

> [!NOTE]
> 7,2 'den eski Service Fabric çalışma zamanı sürümleri yalnızca tek bir değerin **istek** olarak ve belirli bir kaynak için **sınır** (CPU veya bellek) olarak hizmet verdiği bir modeli destekler. Bu belgede **Requestsonly** belirtimi olarak açıklanmaktadır.

* *Istek sayısı:* CPU ve bellek isteği değerleri, ve ölçümleri için [küme kaynak yöneticisi (CRM)][cluster-resource-manager-description-link] tarafından kullanılan yükleri temsil `servicefabric:/_CpuCores` eder `servicefabric:/_MemoryInMB` . Diğer bir deyişle, CRM bir hizmetin kaynak tüketimini istek değerlerine eşit olacak şekilde değerlendirir ve yerleştirme kararları verirken bu değerleri kullanır.

* *Sınırlar:* CPU ve bellek sınırı değerleri, bir düğüm üzerinde bir işlem veya kapsayıcı etkinleştirildiğinde uygulanan gerçek kaynak sınırlarını temsil eder.

Service Fabric, CPU ve bellek için **requestsonly, limitsonly** ve her Iki **requestsandlimit** belirtimlerine izin verir.
* RequestsOnly belirtimi kullanıldığında, Service Fabric istek değerlerini de sınırlar olarak kullanır.
* LimitsOnly belirtimi kullanıldığında, Service Fabric istek değerlerini 0 olarak değerlendirir.
* Requestsandlimit belirtimi kullanıldığında, sınır değerleri istek değerlerinden büyük veya ona eşit olmalıdır.

Kaynak idare mekanizmasını daha iyi anlamak için, CPU kaynağı için **Requestsonly** belirtimine sahip örnek yerleştirme senaryosuna bakalım (bellek yönetimi mekanizması eşdeğerdir). İki CPU çekirdeği ve üzerine yerleştirilecek iki hizmet paketi içeren bir düğüm düşünün. Yerleştirilecek ilk hizmet paketi yalnızca bir kapsayıcı kodu paketinden oluşur ve yalnızca bir CPU çekirdeği isteği belirtir. Yerleştirilecek ikinci hizmet paketi yalnızca tek bir işlem tabanlı kod paketinden oluşur ve aynı zamanda yalnızca bir CPU çekirdeği isteği belirtir. Her iki hizmet paketinde de bir RequestsOnly belirtimi olduğundan, sınır değerleri kendi istek değerlerine ayarlanır.

1. İlk olarak, bir CPU çekirdeği isteyen kapsayıcı tabanlı hizmet paketi düğüme yerleştirilir. Çalışma zamanı kapsayıcıyı etkinleştirir ve CPU sınırını bir çekirdekli olarak ayarlar. Kapsayıcı birden fazla çekirdek kullanamaz.

2. Ardından, bir CPU çekirdeği isteyen işlem tabanlı hizmet paketi düğüme yerleştirilir. Çalışma zamanı, hizmet sürecini etkinleştirir ve CPU sınırını bir çekirdekli olarak ayarlar.

Bu noktada, isteklerin toplamı düğümün kapasitesine eşittir. CRM, bu düğümdeki CPU istekleriyle daha fazla kapsayıcı veya hizmet işlemi yerleştirmeyecektir. Düğümde, bir işlem ve kapsayıcı her biri çekirdekle çalışır ve CPU için birbirleriyle devam etmez.

Şimdi de **Requestsandlimit** belirtimi ile örneğimizi tekrar ziyaret edelim. Bu kez, kapsayıcı tabanlı hizmet paketi bir CPU çekirdeği isteği ve iki CPU çekirdeği sınırı belirtir. İşlem tabanlı hizmet paketi hem isteği hem de bir CPU çekirdeği sınırını belirtir.
  1. Önce kapsayıcı tabanlı hizmet paketi düğüme yerleştirilir. Çalışma zamanı kapsayıcıyı etkinleştirir ve CPU sınırını iki çekirdeğe ayarlar. Kapsayıcı ikiden fazla çekirdek kullanamaz.
  2. Sonra, işlem tabanlı hizmet paketi düğüme yerleştirilir. Çalışma zamanı, hizmet sürecini etkinleştirir ve CPU sınırını bir çekirdekli olarak ayarlar.

  Bu noktada, düğüme yerleştirilmiş olan hizmet paketlerinin CPU isteklerinin toplamı, düğümün CPU kapasitesine eşittir. CRM, bu düğümdeki CPU istekleriyle daha fazla kapsayıcı veya hizmet işlemi yerleştirmeyecektir. Ancak, düğümünde limitlerin toplamı (kapsayıcı için iki çekirdek + işlem için bir çekirdek) iki çekirdeğin kapasitesini aşıyor. Kapsayıcı ve işlem aynı anda veri bloğu varsa, CPU kaynağı için çekişme olasılığı vardır. Bu çekişme, platformun temel alınan işletim sistemi tarafından kullanılacaktır. Bu örnekte, kapsayıcı iki CPU çekirdeğini patlama ve bu da işlemin bir CPU çekirdeği isteği garantisi oluşmasına neden olabilir.

> [!NOTE]
> Önceki örnekte gösterildiği gibi, CPU ve bellek için istek değerleri **bir düğümdeki kaynakların rezervasyonuna yol açabilir**. Bu değerler, yerleştirme kararları verirken Küme Kaynak Yöneticisi göz önünde bulundurulan kaynak tüketimini temsil eder. Sınır değerleri, bir düğüm üzerinde bir işlem veya kapsayıcı etkinleştirildiğinde uygulanan gerçek kaynak sınırlarını temsil eder.


CPU için çekişme olabilecek bazı durumlar vardır. Bu durumlarda, örneğimizin işlem ve kapsayıcısı gürültülü komşu sorunu yaşayabilirler:

* *Yönetilen ve yönetilmeyen Hizmetleri ve kapsayıcıları karıştırma*: bir Kullanıcı hiçbir kaynak İdaresi olmadan bir hizmet oluşturursa, çalışma zamanı bu dosyayı kaynak olmadan görür ve örneğimizdeki düğüme yerleştirebilir. Bu durumda, bu yeni işlem düğümde zaten çalışmakta olan hizmetlerin ücretine göre bazı CPU kullanımını etkili bir şekilde tüketir. Bu sorunun iki çözümü vardır. Aynı kümede yönetilen ve yönetilmeyen Hizmetleri karışmayın ya da bu iki hizmet türünün aynı düğüm kümesine bitmesi için [yerleştirme kısıtlamalarını](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) kullanın.

* *Düğüm üzerinde başka bir işlem başlatıldığında Service Fabric dışında (örneğin, bir işletim sistemi hizmeti)*: Bu durumda, Service Fabric dışındaki işlem, var olan hizmetlerle CPU için de devam ediyor. Bu sorunun çözümü, sonraki bölümde gösterildiği gibi, işletim sistemi ek yükünü hesaba eklemek için düğüm kapasitelerinin doğru şekilde ayarlanmadır.

* *İstekler sınırlara eşit olmadığında*: Requestsandlimit örneğinde daha önce açıklandığı gibi, istekler bir düğümdeki kaynakların rezervasyonuna yol açabilir. İsteklerle daha büyük sınırlara sahip bir hizmet bir düğüme yerleştirildiğinde, kaynakları (varsa) bu sınıra kadar kullanabilir. Bu gibi durumlarda, düğümdeki diğer hizmetler, istek değerlerine kadar kaynakları tüketemeyebilir.

## <a name="cluster-setup-for-enabling-resource-governance"></a>Kaynak yönetimini etkinleştirmek için küme kurulumu

Bir düğüm başlatıldığında ve kümeye katıldığında, Service Fabric kullanılabilir bellek miktarını ve kullanılabilir çekirdek sayısını algılar ve bu iki kaynak için düğüm kapagruplarını ayarlar.

İşletim sistemi için arabellek alanını bırakmak ve düğümde çalışıyor olabilecek diğer süreçler için, Service Fabric düğümde kullanılabilir kaynakların yalnızca %80 ' unu kullanır. Bu yüzde yapılandırılabilir ve küme bildiriminde değiştirilebilir.

Aşağıda, kullanılabilir CPU 'nun %50 ' i ve kullanılabilir belleğin %70 ' i kullanmak için Service Fabric nasıl talimat alınacağını gösteren bir örnek verilmiştir:

```xml
<Section Name="PlacementAndLoadBalancing">
    <!-- 0.0 means 0%, and 1.0 means 100%-->
    <Parameter Name="CpuPercentageNodeCapacity" Value="0.5" />
    <Parameter Name="MemoryPercentageNodeCapacity" Value="0.7" />
</Section>
```

Çoğu müşteri ve senaryo için, CPU ve bellek için düğüm kapasitelerinin otomatik algılanması önerilen yapılandırmadır (otomatik algılama varsayılan olarak açıktır). Ancak, düğüm kapasitelerinin tam el ile kurulumunu yapmanız gerekiyorsa, kümedeki düğümleri açıklama mekanizmasını kullanarak düğüm türü başına yapılandırabilirsiniz. Düğüm türünü dört çekirdekli ve 2 GB bellek ile ayarlamaya yönelik bir örnek aşağıda verilmiştir:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="servicefabric:/_CpuCores" Value="4"/>
        <Capacity Name="servicefabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```

Kullanılabilir kaynakların otomatik algılanması etkin olduğunda ve düğüm kapasiteleri küme bildiriminde el ile tanımlandığında Service Fabric, düğümün kullanıcının tanımladığı kapasiteyi desteklemek için yeterli kaynağa sahip olup olmadığını denetler:

* Bildirimde tanımlanan düğüm kapasiteleri, düğümdeki kullanılabilir kaynaklardan azsa veya buna eşitse, Service Fabric bildirimde belirtilen kapasiteleri kullanır.

* Bildirimde tanımlanan düğüm kapasiteleri kullanılabilir kaynaklardan fazlaysa, Service Fabric kullanılabilir kaynakları düğüm kapasiteleri olarak kullanır.

Gerekli değilse, kullanılabilir kaynakların otomatik algılanması kapatılabilir. Devre dışı bırakmak için aşağıdaki ayarı değiştirin:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="AutoDetectAvailableResources" Value="false" />
</Section>
```

En iyi performans için, küme bildiriminde aşağıdaki ayar de açılmalıdır:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" />
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```

> [!IMPORTANT]
> Service Fabric sürüm 7,0 ' den başlayarak, düğüm kaynak kapasitelerinin Kullanıcı tarafından el ile değer sağladığı durumlarda düğüm kaynak kapasitelerinin nasıl hesaplandığını gösteren kuralı güncelleştirdik. Aşağıdaki senaryoyu ele alalım:
>
> * Düğümde toplam 10 CPU çekirdeği vardır
> * SF, düğüm üzerinde çalışan diğer hizmetler için %20 ' nin (System Services Service Fabric dahil) bir arabelleğini atan Kullanıcı Hizmetleri (varsayılan ayar) için toplam kaynakların %80 ' sini kullanacak şekilde yapılandırılmıştır.
> * Kullanıcı, CPU çekirdeği ölçümü için düğüm kaynak kapasitesini el ile geçersiz kılmayı ve 5 çekirdeğe ayarlıyor
>
> Service Fabric Kullanıcı Hizmetleri için kullanılabilir kapasitenin aşağıdaki şekilde nasıl hesaplanacağını gösteren kuralı değiştirdik:
>
> * 7,0 Service Fabric önce, Kullanıcı Hizmetleri için kullanılabilir kapasite **5 çekirdeğe** hesaplanacak (kapasite arabelleği %20 ' si yoksayılır)
> * Service Fabric 7,0 ' den başlayarak, Kullanıcı Hizmetleri için kullanılabilir kapasite **4 çekirdeğe** hesaplanacak (kapasite arabelleği %20 ' si yok sayılır)

## <a name="specify-resource-governance"></a>Kaynak yönetimini belirtin

Kaynak idare istekleri ve limitleri uygulama bildiriminde (Servicemanifestımport bölümünde) belirtilir. Bazı örneklere bakalım:

**Örnek 1: RequestsOnly belirtimi**
```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1024" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1024" />
    </Policies>
  </ServiceManifestImport>
```

Bu örnekte, `CpuCores` özniteliği **servicepackagea** için 1 CPU çekirdekli bir istek belirtmek için kullanılır. CPU sınırı ( `CpuCoresLimit` özniteliği) belirtilmediğinden Service Fabric, hizmet PAKETININ CPU sınırı olarak belirtilen 1 çekirdekli istek değerini de kullanır.

**Servicepackagea** yalnızca, **bu düğüme yerleştirilmiş tüm hizmet paketlerinin CPU isteklerinin toplamı** çıkarıldıktan sonra kalan CPU kapasitesinin, 1 çekirdekden büyük veya ona eşit olduğu bir düğüme yerleştirilir. Düğümde, hizmet paketi bir çekirdekle sınırlı olacaktır. Hizmet paketi iki kod paketi (**CodeA1** ve **CodeA2**) içerir ve her ikisi de özniteliğini belirtir `CpuShares` . CpuShares 512:256 oranı, bireysel kod paketlerinin CPU sınırlarını hesaplamak için kullanılır. Bu nedenle CodeA1, bir çekirdekli iki buçuk ile sınırlandırılır ve CodeA2, bir çekirdeğin bir üçte biri ile sınırlanır. Tüm kod paketleri için CpuShares belirtilmemişse, Service Fabric CPU limitini aralarında eşit olarak böler.

Kod paketleri için belirtilen CpuShares, hizmet paketinin genel CPU sınırının göreli oranını temsil ettiğinden, kod paketlerinin bellek değerleri mutlak koşullarda belirtilir. Bu örnekte, `MemoryInMB` özniteliği hem CodeA1 hem de CodeA2 için 1024 MB bellek isteklerini belirtmek için kullanılır. Bellek sınırı ( `MemoryInMBLimit` öznitelik) belirtilmediğinden Service Fabric Ayrıca, belirtilen istek değerlerini kod paketlerinin sınırları olarak kullanır. Hizmet paketi için bellek isteği (ve sınırı), kendi bileşen kod paketlerinin bellek isteği (ve sınırı) değerlerinin toplamı olarak hesaplanır. Bu nedenle, **Servicepackagea** için bellek isteği ve SıNıRı 2048 MB olarak hesaplanır.

**Servicepackagea** yalnızca, **bu düğüme yerleştirilmiş tüm hizmet paketlerinin bellek isteklerinin toplamı** çıkarıldıktan sonra kalan bellek kapasitesinin, 2048 MB 'den büyük veya buna eşit olduğu bir düğüme yerleştirilir. Düğümde, her iki kod paketi de 1024 MB bellek ile sınırlandırılır. Kod paketleri (kapsayıcılar veya süreçler) Bu sınırdan daha fazla bellek ayıramayacak ve bunun denenmemesi, bellek dışı özel durumlara neden olur.

**Örnek 2: LimitsOnly belirtimi**
```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCoresLimit="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMBLimit="1024" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMBLimit="1024" />
    </Policies>
  </ServiceManifestImport>
```
Bu örnek `CpuCoresLimit` , ve `MemoryInMBLimit` özniteliklerini kullanır ve yalnızca SF sürümleri 7,2 ve üzeri sürümlerde kullanılabilir. **Service packagea** için 1 çekirdekli CPU sınırı belirtmek Için CpuCoresLimit özniteliği kullanılır. CPU isteği ( `CpuCores` özniteliği) belirtilmediği için 0 olarak kabul edilir. `MemoryInMBLimit` özniteliği, CodeA1 ve CodeA2 için 1024 MB bellek sınırlarını belirtmek için kullanılır ve istekler ( `MemoryInMB` Attribute) belirtilmediği için 0 olarak değerlendirilir. Bu nedenle, **Servicepackagea** için bellek isteği ve sınırı sırasıyla 0 ve 2048 olarak hesaplanır. **Servicepackagea** IÇIN hem CPU hem de bellek istekleri 0 olduğundan, ve ölçümleri Için, bir CRM 'nin yerleştirme için göz önünde bulundurulması gereken bir yük yoktur `servicefabric:/_CpuCores` `servicefabric:/_MemoryInMB` . Bu nedenle, bir kaynak idare perspektifinden, **Servicepackagea** , **kalan kapasiteden bağımsız** olarak herhangi bir düğüme yerleştirilebilir. Örnek 1 ' e benzer şekilde, düğüm üzerinde CodeA1, çekirdek ve 1024 MB 'lık belleğin iki ikisi ile sınırlandırılır ve CodeA2, çekirdek ve 1024 MB 'lık bir bellekle sınırlı olur.

**Örnek 3: Requestsandlimit belirtimi**
```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1" CpuCoresLimit="2"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1024" MemoryInMBLimit="3072" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="2048" MemoryInMBLimit="4096" />
    </Policies>
  </ServiceManifestImport>
```
İlk iki örneğe derleme yaparken bu örnek, CPU ve bellek için hem isteklerin hem de limitlerin belirtilmesini gösterir. **Servicepackagea** 'da CPU ve bellek istekleri 1 core ve 3072 (1024 + 2048) MB vardır. Düğümün toplam CPU (ve bellek) kapasitesinden düğüme yerleştirilmiş tüm hizmet paketlerinin tüm CPU (ve bellek) isteklerinin toplamı çıkarıldıktan sonra, yalnızca en az 1 çekirdek (ve 3072 MB) kapasiteye sahip bir düğüme yerleştirilebilir. Düğüm üzerinde, CodeA1 2 çekirdekle ve 3072 MB bellekle sınırlı olur ancak CodeA2, 2 çekirdekten ve 4096 MB bellekle sınırlı olacaktır.

### <a name="using-application-parameters"></a>Uygulama parametrelerini kullanma

Kaynak idare ayarlarını belirtirken, birden çok uygulama yapılandırmasını yönetmek için [Uygulama parametrelerini](service-fabric-manage-multiple-environment-app-configuration.md) kullanmak mümkündür. Aşağıdaki örnek, uygulama parametrelerinin kullanımını gösterir:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>

  <Parameters>
    <Parameter Name="CpuCores" DefaultValue="4" />
    <Parameter Name="CpuSharesA" DefaultValue="512" />
    <Parameter Name="CpuSharesB" DefaultValue="512" />
    <Parameter Name="MemoryA" DefaultValue="2048" />
    <Parameter Name="MemoryB" DefaultValue="2048" />
  </Parameters>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="[CpuSharesA]" MemoryInMB="[MemoryA]" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="[CpuSharesB]" MemoryInMB="[MemoryB]" />
    </Policies>
  </ServiceManifestImport>
```

Bu örnekte, her bir hizmet paketinin 4 çekirdek ve 2 GB bellek aldığı, üretim ortamı için varsayılan parametre değerleri ayarlanır. Varsayılan değerleri uygulama parametre dosyalarıyla değiştirmek mümkündür. Bu örnekte, uygulamayı yerel olarak test etmek için bir parametre dosyası kullanılabilir; burada üretimden daha az kaynak alınır:

```xml
<!-- ApplicationParameters\Local.xml -->

<Application Name="fabric:/TestApplication1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="CpuSharesA" DefaultValue="512" />
    <Parameter Name="CpuSharesB" DefaultValue="512" />
    <Parameter Name="MemoryA" DefaultValue="1024" />
    <Parameter Name="MemoryB" DefaultValue="1024" />
  </Parameters>
</Application>
```

> [!IMPORTANT]
> Uygulama parametreleriyle kaynak yönetimini belirtmek, Service Fabric sürüm 6,1 ' den başlayarak kullanılabilir.<br>
>
> Kaynak yönetimini belirtmek için uygulama parametreleri kullanıldığında, Service Fabric sürüm 6,1 ' den önceki bir sürüme düşürülemez.

## <a name="enforcing-the-resource-limits-for-user-services"></a>Kullanıcı Hizmetleri için kaynak sınırlarını zorlama

Kaynak İdaresi Service Fabric hizmetlerinize uygulanırken kaynak tarafından yönetilen hizmetlerin kaynak kotasını aşmayacağından emin olmaya devam ederken, pek çok kullanıcının bazı Service Fabric hizmetlerini yönetilmeyen modda çalıştırması gerekir. Yönetilmeyen Service Fabric Hizmetleri kullanılırken, "ard arda" yönetilmeyen hizmetlerin, Service Fabric düğümlerinde kullanılabilir tüm kaynakları tükettiği durumlarda, örneğin:

* Düğümlerde çalışan diğer hizmetlerin kaynakları (Service Fabric sistem hizmetleri dahil)
* Uygun olmayan bir durumda biten düğümler
* Service Fabric küme yönetimi API 'Leri yanıt vermiyor

Bu durumların oluşmasını önlemek için Service Fabric, *düğüm üzerinde çalışan tüm Service Fabric Kullanıcı Hizmetleri (yönetilen ve yönetilmeyen) için kaynak sınırlarını zorunlu kılabilir* . bu sayede, Kullanıcı hizmetlerinin belirtilen kaynak miktarından daha fazla kullanmayacağı garanti altına alınır. Bu, ClusterManifest 'in Placementandloaddengeleme bölümündeki Enforceuserservicemetrickapasiteler yapılandırmasının değeri true olarak ayarlanarak elde edilir. Bu ayar varsayılan olarak kapalıdır.

```xml
<SectionName="PlacementAndLoadBalancing">
    <ParameterName="EnforceUserServiceMetricCapacities" Value="false"/>
</Section>
```

Ek açıklamalar:

* Kaynak sınırı zorlaması yalnızca `servicefabric:/_CpuCores` ve `servicefabric:/_MemoryInMB` kaynak ölçümleri için geçerlidir
* Kaynak sınırı zorlaması yalnızca, kaynak ölçümlerinin düğüm kapasiteleri Service Fabric, otomatik algılama mekanizmasıyla veya Kullanıcı aracılığıyla düğüm kapasitelerinin el ile ( [kaynak yönetimini etkinleştirme](service-fabric-resource-governance.md#cluster-setup-for-enabling-resource-governance) bölümünde açıklandığı gibi) tarafından kullanılabilir olduğunda geçerlidir. Düğüm kapasiteleri yapılandırılmamışsa, Kullanıcı Hizmetleri için ne kadar kaynak ayrılacağını bilemediğinden Service Fabric kaynak sınırı zorlama özelliği kullanılamaz. Service Fabric, "Enforceuserservicemetrickapasiteler" true ise ancak düğüm kapasiteleri yapılandırılmamışsa bir sistem durumu uyarısı verir.

## <a name="other-resources-for-containers"></a>Kapsayıcılar için diğer kaynaklar

CPU ve belleğin yanı sıra, kapsayıcılar için diğer kaynak sınırlarını belirtmek mümkündür. Bu sınırlar, kod paketi düzeyinde belirtilir ve kapsayıcı başlatıldığında uygulanır. CPU ve bellek ile farklı olarak, Cluster Kaynak Yöneticisi bu kaynakları bilmez ve hiçbir kapasite denetimi veya yük dengelemesi yapamayacaktır.

* *Memoryswapınmb*: bir kapsayıcının kullanabileceği takas belleği miktarı.
* *Memoryrezervationınmb*: yalnızca düğümde bellek çakışması algılandığında uygulanan bellek yönetimi için geçici sınır.
* *Cpupercent*: KAPSAYıCıNıN kullanabileceği CPU yüzdesi. Hizmet paketi için CPU istekleri veya sınırları belirtilmişse, bu parametre etkin bir şekilde yok sayılır.
* *Maximumıops*: bir kapsayıcının KULLANABILECEĞI Maksimum IOPS (okuma ve yazma).
* *Maximumiobytesps*: bir kapsayıcının kullanabileceği en yüksek GÇ (saniye başına bayt) (okuma ve yazma).
* *Blockioweight*: diğer kapsayıcılara göreli olarak blok GÇ ağırlığı.

Bu kaynaklar, CPU ve bellekle birleştirilebilir. Kapsayıcılar için ek kaynakların nasıl belirtilbir örneği aşağıda verilmiştir:

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuPercent="5"
            MemorySwapInMB="4084" MemoryReservationInMB="1024" MaximumIOPS="20" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="next-steps"></a>Sonraki adımlar

* Küme Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için [Service Fabric Cluster Resource Manager 'A giriş](service-fabric-cluster-resource-manager-introduction.md)konusunu okuyun.
* Uygulama modeli, hizmet paketleri ve kod paketleri hakkında daha fazla bilgi edinmek ve çoğaltmaların bunlara nasıl eşlendiğini öğrenin-- [Service Fabric bir uygulamanın modelini][application-model-link]okuyun.

<!-- Links -->
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md
[cluster-resource-manager-description-link]: service-fabric-cluster-resource-manager-cluster-description.md
