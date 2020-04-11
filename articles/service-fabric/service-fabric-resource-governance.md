---
title: Kapsayıcılar ve hizmetler için kaynak idaresi
description: Azure Hizmet Kumaşı, kapsayıcıların içinde veya dışında çalışan hizmetler için kaynak sınırları belirtmenize olanak tanır.
ms.topic: conceptual
ms.date: 8/9/2017
ms.openlocfilehash: 11ca6e29829d911717a829b3e4dee0a190856a52
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115140"
---
# <a name="resource-governance"></a>Kaynak idaresi

Aynı düğüm veya küme üzerinde birden çok hizmet çalıştırıyorsanız, bir hizmetin daha fazla kaynak tüketerek bu süreçte diğer hizmetleri aç bırakması mümkündür. Bu sorun "gürültülü komşu" sorunu olarak adlandırılır. Azure Hizmet Kumaşı, kaynakları garanti altına almak ve kaynak kullanımını sınırlamak için geliştiricinin hizmet başına rezervasyonları ve sınırları belirtmesini sağlar.

> Bu makaleye devam etmeden önce Service Fabric [uygulama modeli](service-fabric-application-model.md) ve [Service Fabric hosting modeline](service-fabric-hosting-model.md)aşina olmamızı öneririz.
>

## <a name="resource-governance-metrics"></a>Kaynak yönetim ölçümleri

Kaynak yönetimi Hizmet Kumaşı'nda hizmet [paketine](service-fabric-application-model.md)uygun olarak desteklenir. Hizmet paketine atanan kaynaklar kod paketleri arasında daha fazla bölünebilir. Belirtilen kaynak sınırları aynı zamanda kaynakların çekiyi de ifade ediyor. Service Fabric, iki yerleşik ölçümle hizmet paketi başına CPU ve bellek [belirtmeyi](service-fabric-cluster-resource-manager-metrics.md)destekler:

* *CPU* (metrik `servicefabric:/_CpuCores`ad): Ana bilgisayar makinesinde bulunan mantıksal bir çekirdektir. Tüm düğümler üzerinde tüm çekirdekler aynı ağırlıklıdır.

* *Bellek* (metrik `servicefabric:/_MemoryInMB`ad): Bellek megabaytcinsinden ifade edilir ve makinede bulunan fiziksel belleğe eşlenir.

Bu iki ölçüm için [Cluster Resource Manager](service-fabric-cluster-resource-manager-cluster-description.md) toplam küme kapasitesini, kümedeki her düğümdeki yükü ve kümedeki kalan kaynakları izler. Bu iki ölçüm, başka bir kullanıcı ya da özel ölçüme eşdeğerdir. Varolan tüm özellikler bunlarla birlikte kullanılabilir:

* Küme bu iki ölçüme (varsayılan davranış) göre [dengelenebilir.](service-fabric-cluster-resource-manager-balancing.md)
* Küme bu iki ölçüme göre [parçalanabilir.](service-fabric-cluster-resource-manager-defragmentation-metrics.md)
* [Bir küme açıklanırken,](service-fabric-cluster-resource-manager-cluster-description.md)arabelleğe alan kapasite bu iki ölçüm için ayarlanabilir.

> [!NOTE]
> [Dinamik yük raporlaması](service-fabric-cluster-resource-manager-metrics.md) bu ölçümler için desteklenmez; bu ölçümler için yükler oluşturma zamanında tanımlanır.

## <a name="resource-governance-mechanism"></a>Kaynak yönetim mekanizması

Hizmet Kumaşı çalışma zamanı şu anda kaynaklar için rezervasyon sağlamaz. Bir işlem veya kapsayıcı açıldığında, çalışma zamanı kaynak sınırlarını oluşturma zamanında tanımlanan yüklere ayarlar. Ayrıca, çalışma zamanı, kaynaklar aşıldığında kullanılabilen yeni hizmet paketlerinin açılmasını reddeder. İşlemin nasıl çalıştığını daha iyi anlamak için, iki CPU çekirdeği olan bir düğüm örneği alalım (bellek yönetişim mekanizması eşdeğerdir):

1. İlk olarak, bir CPU çekirdeği isteyen bir kapsayıcı düğümüzerine yerleştirilir. Çalışma süresi kapsayıcıyı açar ve CPU sınırını tek bir çekirdek olarak ayarlar. Konteyner birden fazla çekirdek kullanamaz.

2. Daha sonra, bir hizmetin bir kopyası düğüme yerleştirilir ve ilgili hizmet paketi bir CPU çekirdeğinin sınırını belirtir. Çalışma süresi kod paketini açar ve CPU sınırını tek bir çekirdek olarak ayarlar.

Bu noktada, sınırların toplamı düğümün kapasitesine eşittir. Bir işlem ve bir kapsayıcı her biri bir çekirdekile çalışıyor ve birbirine müdahale etmiyor. Service Fabric, CPU sınırını belirtirken başka kapsayıcı veya yineleme yerleştirmez.

Ancak, diğer işlemlerIN CPU için uğraşabileceği iki durum vardır. Bu gibi durumlarda, örneğimizden bir işlem ve bir kapsayıcı gürültülü komşu sorunu yaşayabilir:

* *Yönetilen ve yönetilmeyen hizmetleri ve kapsayıcıları karıştırma*: Bir kullanıcı herhangi bir kaynak yönetişimi belirtilmeden bir hizmet oluşturursa, çalışma zamanı bunu kaynak tüketen bir madde olarak görür ve örneğimizdeki düğüme yerleyebilir. Bu durumda, bu yeni işlem, düğüm üzerinde çalışan hizmetler pahasına bazı CPU'yu etkin bir şekilde tüketir. Bu sorunun iki çözümü vardır. Yönetilen ve yönetilmeyen hizmetleri aynı kümede karıştırmayın veya bu iki hizmet türünün aynı düğüm kümesinde sona ermesin diye [yerleşim kısıtlamaları](service-fabric-cluster-resource-manager-advanced-placement-rules-placement-policies.md) kullanın.

* *Düğümde başka bir işlem başlatıldığında, Hizmet Kumaşı dışında (örneğin, bir işletim sistemi hizmeti)*: Bu durumda, Service Fabric dışındaki işlem de varolan hizmetlerle CPU için mücadele eder. Bu sorunun çözümü, bir sonraki bölümde gösterildiği gibi, işletim sistemi genel giderini hesaba katmak için düğüm kapasitelerini doğru şekilde ayarlamaktır.

## <a name="cluster-setup-for-enabling-resource-governance"></a>Kaynak yönetimini etkinleştirmek için küme kurulumu

Bir düğüm kümeyi başlatıp birleştirdiğinde, Service Fabric kullanılabilir bellek miktarını ve kullanılabilir çekirdek sayısını algılar ve sonra bu iki kaynak için düğüm kapasitelerini ayarlar.

İşletim sistemi için arabellek alanı bırakmak ve düğümüzerinde çalışan diğer işlemler için Service Fabric düğümdeki kullanılabilir kaynakların yalnızca %80'ini kullanır. Bu yüzde yapılandırılabilir ve küme bildiriminde değiştirilebilir.

Service Fabric'e kullanılabilir CPU'nun %50'sini ve kullanılabilir belleğin %70'ini kullanmasını nasıl öğretenbir örnek aşağıda verilmiştir:

```xml
<Section Name="PlacementAndLoadBalancing">
    <!-- 0.0 means 0%, and 1.0 means 100%-->
    <Parameter Name="CpuPercentageNodeCapacity" Value="0.5" />
    <Parameter Name="MemoryPercentageNodeCapacity" Value="0.7" />
</Section>
```

Çoğu müşteri ve senaryo için, CPU ve bellek için düğüm kapasitelerinin otomatik olarak algılanması önerilen yapılandırmadır (varsayılan olarak otomatik algılama açıktır). Ancak, düğüm kapasitelerinin tam manuel kurulumu gerekiyorsa, kümedeki düğümleri açıklamak için mekanizmayı kullanarak düğüm türü başına bu yapılandırma yapabilirsiniz. Burada dört çekirdek ve 2 GB bellek ile düğüm türünü ayarlamak için nasıl bir örnektir:

```xml
    <NodeType Name="MyNodeType">
      <Capacities>
        <Capacity Name="servicefabric:/_CpuCores" Value="4"/>
        <Capacity Name="servicefabric:/_MemoryInMB" Value="2048"/>
      </Capacities>
    </NodeType>
```

Kullanılabilir kaynakların otomatik algılaması etkinleştirildiğinde ve düğüm kapasiteleri küme manifestosunda el ile tanımlandığında, Service Fabric düğümün kullanıcının tanımladığı kapasiteyi desteklemek için yeterli kaynağa sahip olduğunu denetler:

* Bildirimde tanımlanan düğüm kapasiteleri düğümdeki kullanılabilir kaynaklardan daha az veya eşitse, Service Fabric bildirimde belirtilen kapasiteleri kullanır.

* Bildirimde tanımlanan düğüm kapasiteleri kullanılabilir kaynaklardan daha büyükse, Service Fabric kullanılabilir kaynakları düğüm kapasitesi olarak kullanır.

Gerekli değilse kullanılabilir kaynakların otomatik algılaması kapatılabilir. Kapatmak için aşağıdaki ayarı değiştirin:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="AutoDetectAvailableResources" Value="false" />
</Section>
```

En iyi performans için küme manifestosunda aşağıdaki ayarı n için açık olmalıdır:

```xml
<Section Name="PlacementAndLoadBalancing">
    <Parameter Name="PreventTransientOvercommit" Value="true" />
    <Parameter Name="AllowConstraintCheckFixesDuringApplicationUpgrade" Value="true" />
</Section>
```

> [!IMPORTANT]
> Service Fabric sürüm 7.0 ile başlayarak, kullanıcının düğüm kaynak kapasiteleri için değerleri el ile sağladığı durumlarda düğüm kaynak kapasitelerinin nasıl hesaplandığı kuralını güncelledik. Aşağıdaki senaryoyu ele alalım:
>
> * Düğümde toplam 10 cpu çekirdeği vardır
> * SF, kullanıcı hizmetleri (varsayılan ayar) için toplam kaynakların %80'ini kullanacak şekilde yapılandırılmıştır ve bu da düğümüzerinde çalışan diğer hizmetler için %20'lik bir arabellek bırakır (Hizmet Kumaşı sistem hizmetleri dahil)
> * Kullanıcı, cpu çekirdekölçümü için düğüm kaynak kapasitesini el ile geçersiz kılmaya karar verir ve bunu 5 çekirdek olarak ayarlar
>
> Service Fabric kullanıcı hizmetleri için kullanılabilir kapasitenin nasıl hesaplandığına ilişkin kuralı aşağıdaki şekilde değiştirdik:
>
> * Service Fabric 7.0'dan önce, kullanıcı hizmetleri için kullanılabilir kapasite **5 çekirdek** olarak hesaplanır (%20'lik kapasite arabelleği göz ardı edilir)
> * Service Fabric 7.0 ile başlayarak, kullanıcı hizmetleri için kullanılabilir kapasite **4 çekirdek** olarak hesaplanacaktır (%20'lik kapasite arabelleği göz ardı edilmez)

## <a name="specify-resource-governance"></a>Kaynak yönetimini belirtin

Kaynak yönetim sınırları aşağıdaki örnekte gösterildiği gibi uygulama bildiriminde (ServiceManifestImport bölümü) belirtilir:

```xml
<?xml version='1.0' encoding='UTF-8'?>
<ApplicationManifest ApplicationTypeName='TestAppTC1' ApplicationTypeVersion='vTC1' xsi:schemaLocation='http://schemas.microsoft.com/2011/01/fabric ServiceFabricServiceModel.xsd' xmlns='http://schemas.microsoft.com/2011/01/fabric' xmlns:xsi='https://www.w3.org/2001/XMLSchema-instance'>

  <!--
  ServicePackageA has the number of CPU cores defined, but doesn't have the MemoryInMB defined.
  In this case, Service Fabric sums the limits on code packages and uses the sum as 
  the overall ServicePackage limit.
  -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName='ServicePackageA' ServiceManifestVersion='v1'/>
    <Policies>
      <ServicePackageResourceGovernancePolicy CpuCores="1"/>
      <ResourceGovernancePolicy CodePackageRef="CodeA1" CpuShares="512" MemoryInMB="1000" />
      <ResourceGovernancePolicy CodePackageRef="CodeA2" CpuShares="256" MemoryInMB="1000" />
    </Policies>
  </ServiceManifestImport>
```

Bu örnekte **ServicePackageA** adlı hizmet paketi, yerleştirildiği düğümlerde bir çekirdek alır. Bu hizmet paketi iki kod paketi **(CodeA1** ve **CodeA2)** içerir ve her ikisi de parametreyi `CpuShares` belirtir. CpuShares 512:256 oranı iki kod paketleri arasında çekirdek böler.

Böylece, bu örnekte, CodeA1 bir çekirdeğin üçte ikisini alır ve CodeA2 çekirdeğin üçte birini alır (ve aynı yumuşak garantili rezervasyon). CpuShares kod paketleri için belirtilmemişse, Service Fabric çekirdekleri aralarında eşit olarak böler.

Bellek sınırları mutlakolduğundan, her iki kod paketi de 1024 MB bellekle sınırlıdır (ve aynı yumuşak garantili rezervasyon). Kod paketleri (kapsayıcılar veya işlemler) bu sınırdan daha fazla bellek ayıramaz ve bunu yapmaya çalışmak bellek dışı bir özel durumla sonuçlanır. Kaynak sınırı zorlamasının çalışması için, hizmet paketi içindeki tüm kod paketlerinin bellek sınırlarının belirtilmiş olması gerekir.

### <a name="using-application-parameters"></a>Uygulama parametrelerini kullanma

Kaynak yönetişim ayarlarını belirtirken, birden çok uygulama yapılandırmasını yönetmek için [uygulama parametrelerini](service-fabric-manage-multiple-environment-app-configuration.md) kullanmak mümkündür. Aşağıdaki örnek, uygulama parametrelerinin kullanımını gösterir:

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

Bu örnekte, her Hizmet Paketinin 4 çekirdek ve 2 GB bellek alacağı üretim ortamı için varsayılan parametre değerleri ayarlanır. Uygulama parametresi dosyaları ile varsayılan değerleri değiştirmek mümkündür. Bu örnekte, bir parametre dosyası, uygulamanın üretimden daha az kaynak alacağı yerel olarak sınanması için kullanılabilir:

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
> Hizmet Kumaşı sürüm 6.1'den başlayarak uygulama parametreleri ile kaynak yönetişiminin belirtilmesi kullanılabilir.<br>
>
> Uygulama parametreleri kaynak yönetimini belirtmek için kullanıldığında, Hizmet Dokusu sürüm 6.1'den önce bir sürüme indirgenemez.

## <a name="enforcing-the-resource-limits-for-user-services"></a>Kullanıcı hizmetleri için kaynak sınırlarını zorlama

Hizmet Kumaşı hizmetlerinize kaynak yönetimi uygulamak, bu kaynak tarafından yönetilen hizmetlerin kaynak kotasını aşamamasını garanti ederken, birçok kullanıcının hizmet kumaşı hizmetlerinin bazılarını yönetilmeyen modda çalıştırması gerekir. Yönetilmeyen Service Fabric hizmetlerini kullanırken, "kaçak" yönetilmeyen hizmetlerin Hizmet Kumaşı düğümlerinde mevcut tüm kaynakları tükettiği ve aşağıdakiler gibi ciddi sorunlara yol açabileceği durumlara rastlamak mümkündür:

* Düğümler üzerinde çalışan diğer hizmetlerin kaynak açlığı (Service Fabric sistem hizmetleri dahil)
* Düğümler sağlıksız bir durumda sona eriyor
* Yanıt vermeyen Hizmet Kumaş küme yönetimi API'leri

Bu durumların oluşmasını önlemek için Service Fabric, kullanıcı hizmetlerinin belirtilen kaynak miktarından fazlasını asla kullanmamasını garanti etmek *için düğümüzerinde çalışan (hem* yönetilen hem de yönetilmeyen) tüm Service Fabric kullanıcı hizmetleri için kaynak sınırlarını zorlamanıza olanak tanır. Bu, ClusterManifest'in PlacementAndLoadBalancing bölümündeki EnforceUserServiceMetricCapacities config değerinin doğru olarak ayarlanmasıyla elde edilir. Bu ayar varsayılan olarak kapatılır.

```xml
<SectionName="PlacementAndLoadBalancing">
    <ParameterName="EnforceUserServiceMetricCapacities" Value="false"/>
</Section>
```

Ek açıklamalar:

* Kaynak sınırı zorlama yalnızca `servicefabric:/_CpuCores` ve `servicefabric:/_MemoryInMB` kaynak ölçümleri için geçerlidir
* Kaynak sınırı zorlama, yalnızca kaynak ölçümleri için düğüm kapasiteleri Service Fabric'te otomatik algılama mekanizması aracılığıyla veya kullanıcılar aracılığıyla düğüm kapasitelerini el ile belirterek kullanılabilirse [(kaynak yönetimini etkinleştirmek için Küme kurulumunda](service-fabric-resource-governance.md#cluster-setup-for-enabling-resource-governance) açıklandığı gibi) çalışır.Düğüm kapasiteleri yapılandırılmamışsa, Hizmet Kumaşı kullanıcı hizmetleri için ne kadar kaynak ayıracağını bilmediğinden kaynak sınırı zorlama özelliği kullanılamaz.Service Fabric, "EnforceUserServiceMetricCapacities" doğruysa ancak düğüm kapasiteleri yapılandırılmamışsa bir sistem durumu uyarısı yayınlayacaktır.

## <a name="other-resources-for-containers"></a>Kapsayıcılar için diğer kaynaklar

CPU ve bellek yanı sıra kapsayıcılar için diğer kaynak sınırları belirtmek mümkündür. Bu sınırlar kod paketi düzeyinde belirtilir ve kapsayıcı başlatıldığında uygulanır. CPU ve bellekten farklı olarak, Cluster Resource Manager bu kaynaklardan haberdar değildir ve bunlar için herhangi bir kapasite denetimi veya yük dengelemesi yapmaz.

* *MemorySwapInMB*: Bir kapsayıcının kullanabileceği takas belleği miktarı.
* *MemoryReservationInMB*: Yalnızca düğümüzerinde bellek çekişmesi algılandığında uygulanan bellek yönetimi için yumuşak sınırdır.
* *CpuPercent*: Kapsayıcının kullanabileceği CPU yüzdesi. Hizmet paketi için CPU sınırları belirtilirse, bu parametre etkin bir şekilde yoksayılır.
* *MaximumIOps*: Bir kabın kullanabileceği maksimum IOPS (okuma ve yazma).
* *MaximumIOBytesps*: Bir kabın kullanabileceği maksimum IO (saniyede bayt) (okuma ve yazma).
* *BlockIOWeight*: Diğer kaplara göre blok IO ağırlığı.

Bu kaynaklar CPU ve bellek ile birleştirilebilir. Kapsayıcılar için ek kaynakların nasıl belirtilen bir örneği aşağıda verilmiştir:

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

* Küme Kaynak Yöneticisi hakkında daha fazla bilgi edinmek için [Hizmet Dokusu küme kaynak yöneticisitanıtMa'yı](service-fabric-cluster-resource-manager-introduction.md)okuyun.
* Uygulama modeli, hizmet paketleri ve kod paketleri ve çoğaltmaların bunlarla nasıl eşleşiş olduğu hakkında daha fazla bilgi edinmek için [Service Fabric'te bir uygulama modeli](service-fabric-application-model.md)okuyun.
