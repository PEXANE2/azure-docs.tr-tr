---
title: Sistem durum raporlarıyla ilgili sorunları giderme
description: Azure Hizmet Dokusu bileşenleri tarafından gönderilen sistem durumu raporlarını ve sorun giderme kümesi veya uygulama sorunları için bunların kullanımını açıklar
author: oanapl
ms.topic: conceptual
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: a76ae803b1283ce50d2f4e259943ce5ffcf0274c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282022"
---
# <a name="use-system-health-reports-to-troubleshoot"></a>Sorun gidermek için sistem durum raporlarını kullanma
Azure Hizmet Dokusu bileşenleri, kümedeki tüm varlıkların sistem sistem durumu raporlarını kutunun hemen dışında sağlar. [Sistem deposu,](service-fabric-health-introduction.md#health-store) sistem raporlarına göre varlıkları oluşturur ve siler. Ayrıca, varlık etkileşimlerini yakalayan bir hiyerarşi içinde düzenler.

> [!NOTE]
> Sağlıkla ilgili kavramları anlamak için [Service Fabric sağlık modeli](service-fabric-health-introduction.md)hakkında daha fazla bilgi edinin.
> 
> 

Sistem sistem durumu raporları küme ve uygulama işlevselliği ve bayrak sorunları görünürlük sağlar. Uygulamalar ve hizmetler için sistem sağlık raporları, varlıkların uygulandığını ve Service Fabric perspektifinden doğru davrandığını doğrular. Raporlar, hizmetin iş mantığının veya yanıt vermeyen işlemlerin algılanmasının herhangi bir sistem durumu izlemesini sağlamaz. Kullanıcı hizmetleri, sistem durumu verilerini mantığına özgü bilgilerle zenginleştirebilir.

> [!NOTE]
> Kullanıcı izleme köpekleri tarafından gönderilen sistem raporları yalnızca sistem bileşenleri bir varlık *oluşturduktan sonra* görünür. Bir varlık silindiğinde, sistem durumu deposu onunla ilişkili tüm sistem durumu raporlarını otomatik olarak siler. Aynı varlık yeni bir örnek oluşturulduğunda da geçerlidir. Bir örnek, yeni bir durum lu kalıcı hizmet çoğaltma örneği oluşturulduğunda. Eski örnekle ilişkili tüm raporlar silinir ve mağazadan temizlenir.
> 
> 

Sistem bileşen raporları kaynak tarafından tanımlanır ve bu raporlar " Sistem " ile**başlar.** ekleyin. İzleme örgütleri, geçersiz parametrelere sahip raporlar reddedildiğinden, kaynakları için aynı öneki kullanamaz.

Onları neyin tetiklediğini anlamak ve temsil ettikleri olası sorunları nasıl düzelteceklerini öğrenmek için bazı sistem raporlarına bakalım.

> [!NOTE]
> Service Fabric, kümede ve uygulamalarda neler olup bittiğine ilişkin görünürlüğü artıran ilgi koşulları hakkında raporlar eklemeye devam ediyor. Varolan raporlar, sorunu daha hızlı gidermeye yardımcı olmak için daha fazla ayrıntıyla geliştirilebilir.
> 
> 

## <a name="cluster-system-health-reports"></a>Küme sistemi sistem sağlık raporları
Küme durumu varlığı sistem durumu deposunda otomatik olarak oluşturulur. Her şey düzgün çalışırsa, sistem raporu yoktur.

### <a name="neighborhood-loss"></a>Mahalle kaybı
**System.Federation,** bir mahalle kaybı algıladığında bir hata bildirir. Rapor tek tek düğümlerden gelir ve düğüm kimliği özellik adına dahildir. Bir mahalle tüm Hizmet Kumaşı halkasında kaybolursa, genellikle boşluk raporunun her iki tarafını temsil eden iki olay bekleyebilirsiniz. Daha fazla mahalle kaybedilirse, daha fazla olay vardır.

Rapor, genel kira zaman larını zaman-to-live (TTL) olarak belirtir. Rapor, durum etkin kaldığı sürece TTL süresinin her yarısına dargındır. Olay süresi dolduğunda otomatik olarak kaldırılır. Süresi dolduğunda kaldırma davranışı, raporlama düğümü aşağıda olsa bile raporun sistem durumu deposundan doğru şekilde temizlenmesini sağlar.

* **SourceId**: System.Federation
* **Özellik**: **Mahalle** ile başlar ve düğüm bilgilerini içerir.
* **Sonraki adımlar**: Mahallenin neden kaybolduğunu araştırın. Örneğin, küme düğümleri arasındaki iletişimi denetleyin.

### <a name="rebuild"></a>Yeniden derleme

Failover Manager (FM) hizmeti küme düğümleri hakkındaki bilgileri yönetir. FM verilerini kaybedip veri kaybına geçtiğinde, küme düğümleri hakkında en güncel bilgilere sahip olduğunu garanti edemez. Bu durumda, sistem yeniden oluşturma yoluyla gider ve System.FM durumunu yeniden oluşturmak için kümedeki tüm düğümlerden veri toplar. Bazen, ağ veya düğüm sorunları nedeniyle, yeniden sıkışmış veya duraklattı alabilirsiniz. Aynı failover Manager Master (FMM) hizmeti ile de olabilir. FMM, tüm FM'lerin kümede nerede olduğunu izleyen, devletsiz bir sistem hizmetidir. FMM birincil her zaman 0'a en yakın kimliği ile düğümdür. Bu düğüm düşerse, yeniden tetiklenir.
Önceki koşullardan biri gerçekleştiğinde, **System.FM** veya **System.FMM** bir hata raporu yla işaretler. Yeniden oluşturma iki aşamadan birinde sıkışmış olabilir:

* **Yayını beklemek**: FM/FMM diğer düğümlerden gelen yayın iletisi yanıtını bekler.

  * **Sonraki adımlar**: Düğümler arasında ağ bağlantısı sorunu olup olmadığını araştırın.
* **Düğümleri Bekliyorum**: FM/FMM zaten diğer düğümlerden bir yayın yanıtı aldı ve belirli düğümlerden yanıt bekliyor. Sistem durumu raporu, FM/FMM'in yanıt beklediği düğümleri listeler.
   * **Sonraki adımlar**: FM/FMM ve listelenen düğümler arasındaki ağ bağlantısını incele. Listelenen her düğümü diğer olası sorunlar için araştırın.

* **SourceID**: System.FM veya System.FMM
* **Özellik**: Yeniden.
* **Sonraki adımlar**: Düğümler arasındaki ağ bağlantısını ve sistem durumu raporunun açıklamasında listelenen belirli düğümlerin durumunu araştırın.

### <a name="seed-node-status"></a>Tohum Düğümü Durumu
**System.FM,** bazı tohum düğümleri sağlıksızsa küme düzeyi uyarısı bildirir. Tohum düğümleri, altta yatan kümenin kullanılabilirliğini koruyan düğümlerdir. Bu düğümler, diğer düğümlerle kiralama lar kurarak ve belirli ağ hatalarında tiebreaker olarak hizmet vererek kümenin ayakta kalmasını sağlamaya yardımcı olur. Tohum düğümlerinin çoğunluğu kümede aşağıysa ve geri getirilmezse, küme otomatik olarak kapanır. 

Bir tohum düğümü, düğüm durumu Aşağı, Kaldırıldı veya Bilinmiyor ise sağlıksızdır.
Tohum düğümü durumu için uyarı raporu ayrıntılı bilgi ile tüm sağlıksız tohum düğümleri listeleyecek.

* **SourceID**: System.FM
* **Özellik**: SeedNodeStatus
* **Sonraki adımlar**: Bu uyarı kümede gösteriliyorsa, düzeltmek için aşağıdaki yönergeleri izleyin: Cluster running Service Fabric sürüm 6.5 veya daha yüksek: Azure'da Hizmet Kumaşı kümesi için, tohum düğümü aşağı indikten sonra, Service Fabric bunu otomatik olarak tohum olmayan bir düğümolarak değiştirmeye çalışır. Bunu gerçekleştirmek için, birincil düğüm türündeki tohumsuz düğüm sayısının Down tohum düğümlerinin sayısına daha büyük veya eşit olduğundan emin olun. Gerekirse, bunu başarmak için birincil düğüm türüne daha fazla düğüm ekleyin.
Küme durumuna bağlı olarak, sorunu gidermek biraz zaman alabilir. Bu işlem yapıldıktan sonra, uyarı raporu otomatik olarak temizlenir.

Service Fabric bağımsız küme için, uyarı raporunu temizlemek için, tüm tohum düğümleri sağlıklı olmak gerekir. Tohum düğümlerinin neden sağlıksız olduğuna bağlı olarak, farklı eylemler yapılması gerekir: tohum düğümü Aşağı ise, kullanıcıların bu tohum düğümlerini yukarı getirmeleri gerekir; tohum düğümü Çıkarılırsa veya Bilinmiyorsa, bu tohum [düğümünün kümeden çıkarılması gerekir.](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-windows-server-add-remove-nodes)
Tüm tohum düğümleri sağlıklı olduğunda uyarı raporu otomatik olarak temizlenir.

Küme çalıştıran Service Fabric sürümü 6.5'ten eski: Bu durumda uyarı raporunun el ile temizlenmesi gerekir. **Kullanıcılar raporu temizlemeden önce tüm tohum düğümlerinin sağlıklı hale olduğundan emin olmalıdır:** tohum düğümü Aşağı ise, kullanıcıların bu tohum düğümyukarı getirmek gerekir;tohum düğümü Kaldırılır veya Bilinmiyorise, bu tohum düğümü kümeden kaldırılması gerekir.
Tüm tohum düğümleri sağlıklı hale geldikten sonra, [uyarı raporunu temizlemek](https://docs.microsoft.com/powershell/module/servicefabric/send-servicefabricclusterhealthreport)için Powershell aşağıdaki komutu kullanın:

```powershell
PS C:\> Send-ServiceFabricClusterHealthReport -SourceId "System.FM" -HealthProperty "SeedNodeStatus" -HealthState OK

## Node system health reports
System.FM, which represents the Failover Manager service, is the authority that manages information about cluster nodes. Each node should have one report from System.FM showing its state. The node entities are removed when the node state is removed. For more information, see [RemoveNodeStateAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.clustermanagementclient.removenodestateasync).

### Node up/down
System.FM reports as OK when the node joins the ring (it's up and running). It reports an error when the node departs the ring (it's down, either for upgrading or simply because it has failed). The health hierarchy built by the health store acts on deployed entities in correlation with System.FM node reports. It considers the node a virtual parent of all deployed entities. The deployed entities on that node are exposed through queries if the node is reported as up by System.FM, with the same instance as the instance associated with the entities. When System.FM reports that the node is down or restarted, as a new instance, the health store automatically cleans up the deployed entities that can exist only on the down node or on the previous instance of the node.

* **SourceId**: System.FM
* **Property**: State.
* **Next steps**: If the node is down for an upgrade, it should come back up after it's been upgraded. In this case, the health state should switch back to OK. If the node doesn't come back or it fails, the problem needs more investigation.

The following example shows the System.FM event with a health state of OK for node up:

```powershell
PS C:\> Get-ServiceFabricNodeHealth  _Node_0

NodeName              : _Node_0
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 8
                        SentAt                : 7/14/2017 4:54:51 PM
                        ReceivedAt            : 7/14/2017 4:55:14 PM
                        TTL                   : Infinite
                        Description           : Fabric node is up.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```


### <a name="certificate-expiration"></a>Sertifika nın süresi doldu
**System.FabricNode,** düğüm tarafından kullanılan sertifikalar son kullanma tarihine yakın olduğunda bir uyarı bildirir. Düğüm başına üç sertifika vardır: **Certificate_cluster**, **Certificate_server**ve **Certificate_default_client.** Son kullanma tarihine en az iki hafta kaldığınızda, rapor sağlık durumu tamamdır. Son kullanma tarihi iki hafta içinde olduğunda, rapor türü bir uyarıdır. Bu olayların TTL'si sonsuzdur ve bir düğüm kümeden ayrıldığında kaldırılır.

* **SourceId**: System.FabricNode
* **Özellik**: **Sertifika** ile başlar ve sertifika türü hakkında daha fazla bilgi içerir.
* **Sonraki adımlar**: Sertifikaları niçin sona ermeleri yakınsa güncelleştirin.

### <a name="load-capacity-violation"></a>Yük kapasitesi ihlali
Servis Kumaş Yük Dengeleyici, düğüm kapasitesi ihlali algıladığında bir uyarı bildirir.

* **SourceId**: System.PLB
* **Özellik**: **Kapasite**ile başlar.
* **Sonraki adımlar**: Sağlanan ölçümleri kontrol edin ve düğümdeki geçerli kapasiteyi görüntüleyin.

### <a name="node-capacity-mismatch-for-resource-governance-metrics"></a>Kaynak yönetim ölçümleri için düğüm kapasitesi uyuşmazlığı
System.Hosting, küme bildiriminde tanımlanan düğüm kapasiteleri kaynak yönetim ölçümleri (bellek ve CPU çekirdekleri) için gerçek düğüm kapasitelerinden daha büyükse bir uyarı bildirir. Kaynak [yönetişimini](service-fabric-resource-governance.md) kullanan ilk hizmet paketi belirli bir düğümde kaydolduğunda bir sistem durumu raporu görüntülenir.

* **SourceId**: System.Hosting
* **Özellik**: **Kaynak Yönetişim**.
* **Sonraki adımlar**: Yönetim hizmeti paketleri beklendiği gibi zorlanmadığından ve [kaynak yönetimi](service-fabric-resource-governance.md) düzgün çalışmadığından, bu sorun sorun olabilir. Küme bildirimini bu ölçümler için doğru düğüm kapasiteleriyle güncelleştirin veya belirtmeyin ve Hizmet Kumaşı'nın kullanılabilir kaynakları otomatik olarak algılamasına izin vermeyin.

## <a name="application-system-health-reports"></a>Uygulama sistemi sağlık raporları
Küme Yöneticisi hizmetini temsil eden System.CM, bir uygulama hakkındaki bilgileri yöneten makamdır.

### <a name="state"></a>Durum
System.CM, uygulama oluşturulduğunda veya güncelleştirildiğinde Tamam olarak raporlandı. Uygulama silindiğinde sağlık deposuna bilgi verir, böylece mağazadan kaldırılabilir.

* **SourceId**: System.CM
* **Özellik**: Devlet.
* **Sonraki adımlar**: Uygulama oluşturulduysa veya güncelleştirildiyse, Cluster Manager sistem durumu raporunu içermelidir. Aksi takdirde, bir sorgu düzenleyerek uygulamanın durumunu denetleyin. Örneğin, PowerShell cmdlet **Get-ServiceFabricApplication -ApplicationName applicationName** *applicationName*kullanın.

Aşağıdaki örnek, kumaştaki durum olayını **gösterir:/WordCount** uygulaması:

```powershell
PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ServicesFilter None -DeployedApplicationsFilter None -ExcludeHealthStatistics

ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Ok
ServiceHealthStates             : None
DeployedApplicationHealthStates : None
HealthEvents                    : 
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 282
                                  SentAt                : 7/13/2017 5:57:05 PM
                                  ReceivedAt            : 7/14/2017 4:55:10 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 7/13/2017 5:57:05 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="service-system-health-reports"></a>Hizmet sistemi sağlık raporları
Failover Manager hizmetini temsil eden System.FM, hizmetlerle ilgili bilgileri yöneten yetkilidir.

### <a name="state"></a>Durum
hizmet oluşturulduğunda System.FM Tamam olarak bildirir. Hizmet silindiğinde varlığı sistem durumu deposundan siler.

* **SourceId**: System.FM
* **Özellik**: Devlet.

Aşağıdaki örnek, hizmet dokusundaki durum olayını **gösterir:/WordCount/WordCountWebService:**

```powershell
PS C:\> Get-ServiceFabricServiceHealth fabric:/WordCount/WordCountWebService -ExcludeHealthStatistics


ServiceName           : fabric:/WordCount/WordCountWebService
AggregatedHealthState : Ok
PartitionHealthStates : 
                        PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
                        AggregatedHealthState : Ok
                        
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 14
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Service has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="service-correlation-error"></a>Hizmet korelasyon hatası
**System.PLB,** bir hizmeti güncelleştirmenin yakınlık zinciri oluşturan başka bir hizmetle ilişkili olduğunu algıladığında bir hata bildirir. Başarılı bir güncelleştirme gerçekleştiğinde rapor temizlenir.

* **SourceId**: System.PLB
* **Özellik**: **ServiceDescription**.
* **Sonraki adımlar**: İlişkili hizmet açıklamalarını denetleyin.

## <a name="partition-system-health-reports"></a>Bölüm sistemi sağlık raporları
Failover Manager hizmetini temsil eden System.FM, hizmet bölümleri hakkındaki bilgileri yöneten yetkilidir.

### <a name="state"></a>Durum
System.FM, bölüm oluşturulduğunda ve sağlıklı olduğunda Tamam olarak bildirir. Bölüm silindiğinde varlığı sistem durumu deposundan siler.

Bölüm en az yineleme sayısının altındaysa, bir hata bildirir. Bölüm minimum yineleme sayısının altında değilse, ancak hedef yineleme sayısının altındaysa, bir uyarı bildirir. Bölüm çoğunluk kaybı ise, System.FM bir hata bildirir.

Diğer önemli olaylar arasında, yeniden yapılandırma beklenenden uzun sürdüğünde ve yapı beklenenden daha uzun sürdüğünde bir uyarı yer alır. Yapı ve yeniden yapılandırma için beklenen süreler, hizmet senaryolarına göre yapılandırılabilir. Örneğin, bir hizmette Azure SQL Veritabanı gibi bir terabayt lık durum varsa, yapı az miktarda durum içeren bir hizmetten daha uzun sürer.

* **SourceId**: System.FM
* **Özellik**: Devlet.
* **Sonraki adımlar**: Sistem durumu iyi değilse, bazı yinelemelerin birincil veya ikincil olarak doğru şekilde oluşturulmadı, açılmamış veya birincil veya ikincil olarak yükseltilmemiş olabilir. 

Açıklama çoğunluk kaybını tanımlıyorsa, daha sonra aşağı olan yinelemeler için ayrıntılı sağlık raporunu incelemek ve bunları geri getirmek, bölümün yeniden çevrimiçi duruma getirilmesine yardımcı olur.

Açıklama, yeniden yapılandırmaya sıkışmış bir bölümü [açıklarsa,](service-fabric-concepts-reconfiguration.md)birincil yinelemedeki sistem durumu raporu ek bilgiler sağlar.

Diğer System.FM sağlık raporları için, yinelemeler veya diğer sistem bileşenlerinden bölüm veya hizmet raporları olacaktır. 

Aşağıdaki örneklerde bu raporlardan bazıları açıklayınız. 

Aşağıdaki örnekte sağlıklı bir bölüm gösterilmektedir:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountWebService | Get-ServiceFabricPartitionHealth -ExcludeHealthStatistics -ReplicasFilter None

PartitionId           : 8bbcd03a-3a53-47ec-a5f1-9b77f73c53b2
AggregatedHealthState : Ok
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 70
                        SentAt                : 7/13/2017 5:57:05 PM
                        ReceivedAt            : 7/14/2017 4:55:10 PM
                        TTL                   : Infinite
                        Description           : Partition is healthy.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/13/2017 5:57:18 PM, LastWarning = 1/1/0001 12:00:00 AM
```

Aşağıdaki örnek, hedef çoğaltma sayısının altında olan bir bölümün durumunu gösterir. Bir sonraki adım, nasıl yapılandırıldığı gösterir bölüm açıklaması almaktır: **MinReplicaSetSize** üç ve **TargetReplicaSetSize** yedi. Ardından kümedeki düğüm sayısını alın ve bu durumda beş tir. Yani, bu durumda, iki yineleme yerleştirilemez, çünkü yinelemelerin hedef sayısı kullanılabilir düğüm sayısından daha yüksektir.

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricPartitionHealth -ReplicasFilter None -ExcludeHealthStatistics


PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', ConsiderWarningAsError=false.
                        
ReplicaHealthStates   : None
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 123
                        SentAt                : 7/14/2017 4:55:39 PM
                        ReceivedAt            : 7/14/2017 4:55:44 PM
                        TTL                   : Infinite
                        Description           : Partition is below target replica or instance count.
                        fabric:/WordCount/WordCountService 7 2 af2e3e44-a8f8-45ac-9f31-4093eb897600
                          N/S Ready _Node_2 131444422260002646
                          N/S Ready _Node_4 131444422293113678
                          N/S Ready _Node_3 131444422293113679
                          N/S Ready _Node_1 131444422293118720
                          N/P Ready _Node_0 131444422293118721
                          (Showing 5 out of 5 replicas. Total available replicas: 5)
                        
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:55:44 PM, LastOk = 1/1/0001 12:00:00 AM
                        
                        SourceId              : System.PLB
                        Property              : ServiceReplicaUnplacedHealth_Secondary_af2e3e44-a8f8-45ac-9f31-4093eb897600
                        HealthState           : Warning
                        SequenceNumber        : 131445250939703027
                        SentAt                : 7/14/2017 4:58:13 PM
                        ReceivedAt            : 7/14/2017 4:58:14 PM
                        TTL                   : 00:01:05
                        Description           : The Load Balancer was unable to find a placement for one or more of the Service's Replicas:
                        Secondary replica could not be placed due to the following constraints and properties:  
                        TargetReplicaSetSize: 7
                        Placement Constraint: N/A
                        Parent Service: N/A
                        
                        Constraint Elimination Sequence:
                        Existing Secondary Replicas eliminated 4 possible node(s) for placement -- 1/5 node(s) remain.
                        Existing Primary Replica eliminated 1 possible node(s) for placement -- 0/5 node(s) remain.
                        
                        Nodes Eliminated By Constraints:
                        
                        Existing Secondary Replicas -- Nodes with Partition's Existing Secondary Replicas/Instances:
                        --
                        FaultDomain:fd:/4 NodeName:_Node_4 NodeType:NodeType4 UpgradeDomain:4 UpgradeDomain: ud:/4 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/3 NodeName:_Node_3 NodeType:NodeType3 UpgradeDomain:3 UpgradeDomain: ud:/3 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/2 NodeName:_Node_2 NodeType:NodeType2 UpgradeDomain:2 UpgradeDomain: ud:/2 Deactivation Intent/Status: None/None
                        FaultDomain:fd:/1 NodeName:_Node_1 NodeType:NodeType1 UpgradeDomain:1 UpgradeDomain: ud:/1 Deactivation Intent/Status: None/None
                        
                        Existing Primary Replica -- Nodes with Partition's Existing Primary Replica or Secondary Replicas:
                        --
                        FaultDomain:fd:/0 NodeName:_Node_0 NodeType:NodeType0 UpgradeDomain:0 UpgradeDomain: ud:/0 Deactivation Intent/Status: None/None
                        
                        
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 7/14/2017 4:56:14 PM, LastOk = 1/1/0001 12:00:00 AM

PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | select MinReplicaSetSize,TargetReplicaSetSize

MinReplicaSetSize TargetReplicaSetSize
----------------- --------------------
                2                    7                        

PS C:\> @(Get-ServiceFabricNode).Count
5
```

Aşağıdaki örnek, kullanıcının **RunAsync** yöntemindeki iptal belirteciyle onurlandırmamasıdır nedeniyle yeniden yapılandırmada sıkışmış bir bölümün durumunu gösterir. Birincil (P) olarak işaretlenmiş herhangi bir yinelemenin sistem durumu raporunu araştırmak, sorunun daha fazla araştırılmasına yardımcı olabilir.

```powershell
PS C:\utilities\ServiceFabricExplorer\ClientPackage\lib> Get-ServiceFabricPartitionHealth 0e40fd81-284d-4be4-a665-13bc5a6607ec -ExcludeHealthStatistics 


PartitionId           : 0e40fd81-284d-4be4-a665-13bc5a6607ec
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.FM', Property='State', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                                               
HealthEvents          : 
                        SourceId              : System.FM
                        Property              : State
                        HealthState           : Warning
                        SequenceNumber        : 7
                        SentAt                : 8/27/2017 3:43:09 AM
                        ReceivedAt            : 8/27/2017 3:43:32 AM
                        TTL                   : Infinite
                        Description           : Partition reconfiguration is taking longer than expected.
                        fabric:/app/test1 3 1 0e40fd81-284d-4be4-a665-13bc5a6607ec
                          P/S Ready Node1 131482789658160654
                          S/P Ready Node2 131482789688598467
                          S/S Ready Node3 131482789688598468
                          (Showing 3 out of 3 replicas. Total available replicas: 3)                        
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Ok->Warning = 8/27/2017 3:43:32 AM, LastError = 1/1/0001 12:00:00 AM
```
Bu sistem durumu raporu, yeniden yapılandırmadan geçen bölümün yinelemelerinin durumunu gösterir: 

```
  P/S Ready Node1 131482789658160654
  S/P Ready Node2 131482789688598467
  S/S Ready Node3 131482789688598468
```

Her yineleme için sistem durumu raporu şunları içerir:
- Önceki yapılandırma rolü
- Geçerli yapılandırma rolü
- [Yineleme durumu](service-fabric-concepts-replica-lifecycle.md)
- Yinelemenin üzerinde çalışan düğüm
- Çoğaltma Kimliği

Örnek gibi bir durumda, daha fazla araştırma gereklidir. Önceki örnekte ve `Primary` `Secondary` (131482789658160654 ve 131482789688598467) olarak işaretlenmiş yinelemeler ile başlayan her bir yinelemenin durumunu araştırın.

### <a name="replica-constraint-violation"></a>Yineleme kısıtlaması ihlali
**System.PLB,** yineleme kısıtlaması ihlali algılarsa ve tüm bölüm yinelemelerini yerleştiremezse bir uyarı bildirir. Rapor ayrıntıları, hangi kısıtlamaların ve özelliklerin yineleme yerleşimini engellediğini gösterir.

* **SourceId**: System.PLB
* **Özellik**: **ReplicaConstraintViolation**ile başlar.

## <a name="replica-system-health-reports"></a>Çoğaltma sistemi sağlık raporları
Yeniden yapılandırma aracısı bileşenini temsil eden **System.RA,** yineleme durumunun yetkisidir.

### <a name="state"></a>Durum
System.RA, yineleme oluşturulduğunda Tamam'ı bildirir.

* **SourceId**: System.RA
* **Özellik**: Devlet.

Aşağıdaki örnekte sağlıklı bir yineleme gösterilmektedir:

```powershell
PS C:\> Get-ServiceFabricPartition fabric:/WordCount/WordCountService | Get-ServiceFabricReplica | where {$_.ReplicaRole -eq "Primary"} | Get-ServiceFabricReplicaHealth

PartitionId           : af2e3e44-a8f8-45ac-9f31-4093eb897600
ReplicaId             : 131444422293118721
AggregatedHealthState : Ok
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131445248920273536
                        SentAt                : 7/14/2017 4:54:52 PM
                        ReceivedAt            : 7/14/2017 4:55:13 PM
                        TTL                   : Infinite
                        Description           : Replica has been created._Node_0
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 7/14/2017 4:55:13 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="replicaopenstatus-replicaclosestatus-replicachangerolestatus"></a>ÇoğaltmaOpen Durumu, ÇoğaltmaCloseStatus, ReplicaChangeRoleStatus
Bu özellik, yinelemeyi açmaya, yinelemeyi kapatmaya veya bir yinelemeyi bir rolden diğerine geçirmeye çalışırken uyarıları veya hataları belirtmek için kullanılır. Daha fazla bilgi için [Bkz. Çoğaltma yaşam döngüsü.](service-fabric-concepts-replica-lifecycle.md) Hatalar, bu süre içinde api aramalarından veya hizmet ana bilgisayar işleminin çökmelerinden atılan özel durumlar olabilir. C# kodundan gelen API çağrıları nedeniyle yapılan hatalar için Service Fabric, sistem durumu raporuna özel durum ve yığın izlemesini ekler.

Bu sistem durumu uyarıları, eylemi yerel olarak birkaç kez yeniden denedikten sonra (politikaya bağlı olarak) yükseltilir. Service Fabric eylemi en yüksek eşiğe kadar yeniden dener. Maksimum eşiğe ulaşıldıktan sonra, durumu düzeltmek için harekete geçmeye çalışabilir. Bu girişim, bu düğüm üzerindeki eylemden vazgeçtikçe bu uyarıların temizlenmesine neden olabilir. Örneğin, bir yineleme düğümde açılmıyorsa, Hizmet Kumaşı bir sistem durumu uyarısı yükseltir. Çoğaltma açılmamaya devam ederse, Service Fabric kendi kendini onarmak için hareket eder. Bu eylem, aynı işlemi başka bir düğüm üzerinde denemeyi içerebilir. Bu girişim, bu yinelemenin temizlenmesi için yükseltilen uyarıya neden olur. 

* **SourceId**: System.RA
* **Özellik**: **ReplicaOpenStatus**, **ReplicaCloseStatus**, ve **ReplicaChangeRoleStatus**.
* **Sonraki adımlar**: İşlemin neden başarısız olduğunu belirlemek için servis kodunu veya kilitlenme dökümlerini inceleştirin.

Aşağıdaki örnek, açık yönteminden atan `TargetInvocationException` bir yinelemenin durumunu gösterir. Açıklama hata noktası, **IStatefulServiceReplica.Open,** özel durum türü **TargetInvocationException**ve yığın izleme içerir.

```powershell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 337cf1df-6cab-4825-99a9-7595090c0b1b -ReplicaOrInstanceId 131483509874784794


PartitionId           : 337cf1df-6cab-4825-99a9-7595090c0b1b
ReplicaId             : 131483509874784794
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='ReplicaOpenStatus', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : ReplicaOpenStatus
                        HealthState           : Warning
                        SequenceNumber        : 131483510001453159
                        SentAt                : 8/27/2017 11:43:20 PM
                        ReceivedAt            : 8/27/2017 11:43:21 PM
                        TTL                   : Infinite
                        Description           : Replica had multiple failures during open on _Node_0 API call: IStatefulServiceReplica.Open(); Error = System.Reflection.TargetInvocationException (-2146232828)
Exception has been thrown by the target of an invocation.
   at Microsoft.ServiceFabric.Replicator.RecoveryManager.d__31.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.LoggingReplicator.d__137.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.DynamicStateManager.d__109.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.TransactionalReplicator.d__79.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Replicator.StatefulServiceReplica.d__21.MoveNext()
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.CompilerServices.TaskAwaiter.ThrowForNonSuccess(Task task)
   at System.Runtime.CompilerServices.TaskAwaiter.HandleNonSuccessAndDebuggerNotification(Task task)
   at Microsoft.ServiceFabric.Services.Runtime.StatefulServiceReplicaAdapter.d__0.MoveNext()

    For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/27/2017 11:43:21 PM, LastOk = 1/1/0001 12:00:00 AM                        
```

Aşağıdaki örnek, kapatma sırasında sürekli çöken bir yinelemeyi gösterir:

```powershell
C:>Get-ServiceFabricReplicaHealth -PartitionId dcafb6b7-9446-425c-8b90-b3fdf3859e64 -ReplicaOrInstanceId 131483565548493142


PartitionId           : dcafb6b7-9446-425c-8b90-b3fdf3859e64
ReplicaId             : 131483565548493142
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='ReplicaCloseStatus', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : ReplicaCloseStatus
                        HealthState           : Warning
                        SequenceNumber        : 131483565611258984
                        SentAt                : 8/28/2017 1:16:01 AM
                        ReceivedAt            : 8/28/2017 1:16:03 AM
                        TTL                   : Infinite
                        Description           : Replica had multiple failures during close on _Node_1. The application 
                        host has crashed.
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 1:16:03 AM, LastOk = 1/1/0001 12:00:00 AM
```

### <a name="reconfiguration"></a>Yeniden yapılandırma
Bu özellik, yeniden yapılandırma gerçekleştiren bir yinelemenin yeniden [yapılandırmanın](service-fabric-concepts-reconfiguration.md) durduğunu veya sıkıştığını algıladığını belirtmek için kullanılır. Bu sistem durumu raporu, birincil ikincil den etkin ikincil indirgenmiş olan yineleme üzerinde olabilir bir takas birincil yeniden yapılandırma durumları dışında, geçerli rolü birincil olan yineleme üzerinde olabilir.

Yeniden yapılandırma aşağıdaki nedenlerden biri için sıkışmış olabilir:

- Yeniden yapılandırmayı gerçekleştiren yinelemeyle aynı yineleme olan yerel yineleme üzerindeki bir eylem tamamlanamaz. Bu durumda, diğer bileşenlerden bu yineleme, System.RAP veya System.RE sağlık raporları nın incelenmesi ek bilgi sağlayabilir.

- Bir eylem uzak bir yineleme de tamamlanınmıyor. Eylemlerin beklemede olduğu yinelemeler sistem durumu raporunda listelenir. Daha fazla araştırma bu uzak kopyaları için sağlık raporları üzerinde yapılmalıdır. Bu düğüm ve uzak düğüm arasında iletişim sorunları da olabilir.

Nadir durumlarda, yeniden yapılandırma iletişim veya bu düğüm ve Failover Manager hizmeti arasındaki diğer sorunlar nedeniyle sıkışmış olabilir.

* **SourceId**: System.RA
* **Özellik**: Yeniden yapılandırma.
* **Sonraki adımlar**: Sistem durumu raporunun açıklamasına bağlı olarak yerel veya uzak yinelemeleri incele.

Aşağıdaki örnek, yeniden yapılandırmanın yerel yinelemeye takılıp kaldığı bir sistem durumu raporunu gösterir. Bu örnekte, iptal jetonuna uymayan bir hizmetten kaynaklanmaktadır.

```powershell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 9a0cedee-464c-4603-abbc-1cf57c4454f3 -ReplicaOrInstanceId 131483600074836703


PartitionId           : 9a0cedee-464c-4603-abbc-1cf57c4454f3
ReplicaId             : 131483600074836703
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', 
                        ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : Reconfiguration
                        HealthState           : Warning
                        SequenceNumber        : 131483600309264482
                        SentAt                : 8/28/2017 2:13:50 AM
                        ReceivedAt            : 8/28/2017 2:13:57 AM
                        TTL                   : Infinite
                        Description           : Reconfiguration is stuck. Waiting for response from the local replica
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 2:13:57 AM, LastOk = 1/1/0001 12:00:00 AM
```

Aşağıdaki örnek, yeniden yapılandırmanın iki uzak yinelemeden yanıt beklerken tıkandığı bir sistem durumu raporunu gösterir. Bu örnekte, geçerli birincil dahil olmak üzere bölüm üç yineleme vardır. 

```Powershell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId  579d50c6-d670-4d25-af70-d706e4bc19a2 -ReplicaOrInstanceId 131483956274977415


PartitionId           : 579d50c6-d670-4d25-af70-d706e4bc19a2
ReplicaId             : 131483956274977415
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', ConsiderWarningAsError=false.
                        
HealthEvents          : 
                        SourceId              : System.RA
                        Property              : Reconfiguration
                        HealthState           : Warning
                        SequenceNumber        : 131483960376212469
                        SentAt                : 8/28/2017 12:13:57 PM
                        ReceivedAt            : 8/28/2017 12:14:07 PM
                        TTL                   : Infinite
                        Description           : Reconfiguration is stuck. Waiting for response from 2 replicas
                        
                        Pending Replicas: 
                        P/I Down 40 131483956244554282
                        S/S Down 20 131483956274972403
                        
                        For more information see: https://aka.ms/sfhealth
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 12:07:37 PM, LastOk = 1/1/0001 12:00:00 AM
```

Bu sistem durumu raporu, yeniden yapılandırmanın iki yinelemeden yanıt beklerken sıkışıp kaldığını gösterir: 

```
    P/I Down 40 131483956244554282
    S/S Down 20 131483956274972403
```

Her yineleme için aşağıdaki bilgiler verilir:
- Önceki yapılandırma rolü
- Geçerli yapılandırma rolü
- [Yineleme durumu](service-fabric-concepts-replica-lifecycle.md)
- Düğüm Kimliği
- Çoğaltma Kimliği

Yeniden yapılandırmanın engelini kaldırmak için:
- **Aşağı** kopyaları gündeme getirilmelidir. 
- **Yapı kopyaları** yapıyı tamamlamalı ve hazıra geçiş yapmalıdır.

### <a name="slow-service-api-call"></a>Yavaş hizmet API çağrısı
**System.RAP** ve **System.Replicator,** kullanıcı hizmet koduna yapılandırılan süreden daha uzun sürerse bir uyarı bildirir. Arama tamamlandığında uyarı temizlenir.

* **SourceId**: System.RAP veya System.Replicator
* **Özellik**: Yavaş API'nin adı. Açıklama, API'nin beklemede olduğu saat hakkında daha fazla ayrıntı sağlar.
* **Sonraki adımlar**: Aramanın neden beklenenden uzun sürdüğünü araştırın.

Aşağıdaki örnek, **RunAsync'deki**iptal belirteciyle onurlandırmayan güvenilir bir hizmet için System.RAP'teki sağlık olayını gösterir:

```powershell
PS C:\> Get-ServiceFabricReplicaHealth -PartitionId 5f6060fb-096f-45e4-8c3d-c26444d8dd10 -ReplicaOrInstanceId 131483966141404693


PartitionId           : 5f6060fb-096f-45e4-8c3d-c26444d8dd10
ReplicaId             : 131483966141404693
AggregatedHealthState : Warning
UnhealthyEvaluations  : 
                        Unhealthy event: SourceId='System.RA', Property='Reconfiguration', HealthState='Warning', ConsiderWarningAsError=false.
                        
HealthEvents          :                         
                        SourceId              : System.RAP
                        Property              : IStatefulServiceReplica.ChangeRole(S)Duration
                        HealthState           : Warning
                        SequenceNumber        : 131483966663476570
                        SentAt                : 8/28/2017 12:24:26 PM
                        ReceivedAt            : 8/28/2017 12:24:56 PM
                        TTL                   : Infinite
                        Description           : The api IStatefulServiceReplica.ChangeRole(S) on _Node_1 is stuck. Start Time (UTC): 2017-08-28 12:23:56.347.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Warning = 8/28/2017 12:24:56 PM, LastOk = 1/1/0001 12:00:00 AM
                        
```

Özellik ve metin hangi API'nin takılıp kaldığını gösterir. Farklı sıkışmış API'ler için atılacak sonraki adımlar farklıdır. *IStatefulServiceReplica* veya *IStatelessServiceInstance'daki* tüm API'ler genellikle hizmet kodundaki bir hatadır. Aşağıdaki bölümde, bunların Güvenilir [Hizmetler modeline](service-fabric-reliable-services-lifecycle.md)nasıl çevrildiği açıklanmaktadır:

- **IStatefulServiceReplica.Open**: Bu `CreateServiceInstanceListeners`uyarı, bir `ICommunicationListener.OpenAsync`çağrının , veya `OnOpenAsync` geçersiz kılınmışsa, sıkışmış olduğunu gösterir.

- **IStatefulServiceReplica.Close** ve **IStatefulServiceReplica.Abort**: En yaygın durumda iptal belirteci onurlandıran olmayan bir `RunAsync`hizmettir . `ICommunicationListener.CloseAsync`Ayrıca, ya da geçersiz kılınmış, `OnCloseAsync` sıkışmış olabilir.

- **IStatefulServiceReplica.ChangeRole(S)** ve **IStatefulServiceReplica.ChangeRole(N)**: En yaygın durumda iptal belirteci onurlandıran olmayan bir `RunAsync`hizmettir . Bu senaryoda, en iyi çözüm çoğaltmayeniden başlatmaktır.

- **IStatefulServiceReplica.ChangeRole(P)**: En yaygın durumda hizmet `RunAsync`bir görev döndürmüş olmasıdır .

Takılıp kalanabilecek diğer API çağrıları **IReplicator** arabirimindedir. Örnek:

- **IReplicator.CatchupReplicaSet**: Bu uyarı iki şeyden birini gösterir. Yetersiz çoğaltmalar var. Bu durumda olup olmadığını görmek için, sıkışmış bir yeniden yapılandırma için bölüm veya System.FM sağlık raporu yineleme durumuna bakın. Ya da yinelemeler işlemleri kabul etmiyor. PowerShell cmdlet `Get-ServiceFabricDeployedReplicaDetail` tüm kopyaların ilerlemesini belirlemek için kullanılabilir. Sorun, birincil `LastAppliedReplicationSequenceNumber` `CommittedSequenceNumber` değerin arkasındaki değeri olan yinelemelerde yatıyor.

- **IReplicator.BuildReplica\<(Uzaktan Çoğaltma>)**: Bu uyarı yapı işleminde bir sorun gösterir. Daha fazla bilgi için [Bkz. Çoğaltma yaşam döngüsü.](service-fabric-concepts-replica-lifecycle.md) Çoğalıcı adresinin yanlış yapılandırılmasından kaynaklanıyor olabilir. Daha fazla bilgi için bkz: [Özel Güvenilir Hizmetleri Yapılandırın](service-fabric-reliable-services-configuration.md) ve [bir hizmet bildiriminde kaynakları belirtin.](service-fabric-service-manifest-resources.md) Ayrıca uzak düğümüzerinde bir sorun olabilir.

### <a name="replicator-system-health-reports"></a>Çoğaltıcı sistem sağlık raporları
**Çoğaltma sırası tam:**
**System.Replicator** çoğaltma sırası dolduğunda bir uyarı bildirir. Birincil, bir veya daha fazla ikincil yineleme işlemleri kabul etmek yavaş olduğundan çoğaltma sırası genellikle tam olur. İkincil olarak, bu genellikle hizmet işlemleri uygulamak için yavaş olduğunda olur. Sıra artık dolduğunda uyarı temizlenir.

* **SourceId**: System.Replicator
* **Özellik**: **PrimaryReplicationQueueStatus** veya **SecondaryReplicationQueueStatus**, çoğaltma rolüne bağlı olarak.
* **Sonraki adımlar**: Rapor birincil taraftaysa, kümedeki düğümler arasındaki bağlantıyı denetleyin. Tüm bağlantılar sağlıklıysa, işlemleri uygulamak için yüksek disk gecikmesi olan en az bir yavaş ikincil olabilir. Rapor ikincil taraftaysa, önce düğümdeki disk kullanımını ve performansını denetleyin. Ardından, giden bağlantıyı yavaş düğümden birincil düğüme doğru kontrol edin.

**RemoteReplicatorConnectionStatus:**
**System.Replicator** birincil çoğaltma bir uyarı bildirir ikincil (uzak) çoğaltıcı bağlantısı sağlıklı değil. Uzaktan çoğaltıcının adresi raporun iletisinde gösterilir, bu da yanlış yapılandırmanın geçirilip geçirilmediğini veya çoğalıcılar arasında ağ sorunları olup olmadığını algılamayı daha kullanışlı hale getirir.

* **SourceId**: System.Replicator
* **Özellik**: **RemoteReplicatorConnectionStatus**.
* **Sonraki adımlar**: Hata iletisini denetleyin ve uzak çoğaltıcı adresinin doğru şekilde yapılandırıldığından emin olun. Örneğin, uzaktan çoğaltıcı "localhost" dinleme adresiyle açılırsa, dışarıdan erişilemez. Adres doğru görünüyorsa, olası ağ sorunlarını bulmak için birincil düğüm ile uzak adres arasındaki bağlantıyı denetleyin.

### <a name="replication-queue-full"></a>Çoğaltma sırası tam
**System.Replicator** çoğaltma sırası dolduğunda bir uyarı bildirir. Birincil, bir veya daha fazla ikincil yineleme işlemleri kabul etmek yavaş olduğundan çoğaltma sırası genellikle tam olur. İkincil olarak, bu genellikle hizmet işlemleri uygulamak için yavaş olduğunda olur. Sıra artık dolduğunda uyarı temizlenir.

* **SourceId**: System.Replicator
* **Özellik**: **PrimaryReplicationQueueStatus** veya **SecondaryReplicationQueueStatus**, çoğaltma rolüne bağlı olarak.

### <a name="slow-naming-operations"></a>Yavaş Adlandırma işlemleri
**System.NamingService,** bir Adlandırma işlemi kabul edilebilirden daha uzun sürdüğünde, birincil yinelemesi üzerinde sağlık bildirir. Adlandırma işlemlerine örnek olarak [CreateServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync) veya [DeleteServiceAsync](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync)verilebilir. FabricClient altında daha fazla yöntem bulunabilir. Örneğin, [bunlar hizmet yönetimi yöntemleri](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient) veya mülkiyet yönetimi [yöntemleri](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.propertymanagementclient)altında bulunabilir.

> [!NOTE]
> Adlandırma hizmeti, hizmet adlarını kümedeki bir konuma giderir. Kullanıcılar bunu hizmet adlarını ve özelliklerini yönetmek için kullanabilir. Service Fabric bölümlenmiş-kalıcı bir hizmettir. Bölümlerden biri, tüm Hizmet Kumaşadları ve hizmetleri hakkında meta veriler içeren *Yetki Sahibini*temsil eder. Hizmet Kumaşı adları, *Ad Sahibi* bölümleri olarak adlandırılan farklı bölümlere eşlenir, bu nedenle hizmet genişletilebilir. [Adlandırma hizmeti](service-fabric-architecture.md)hakkında daha fazla bilgi edinin.
> 
> 

Bir Adlandırma işlemi beklenenden uzun sürdüğünde, işlem, işlem için hizmet veren Adlandırma hizmeti bölümü birincil yinelemesi hakkında bir uyarı raporuyla işaretlenir. İşlem başarıyla tamamlanırsa, uyarı temizlenir. İşlem bir hatayla tamamlanırsa, sistem durumu raporu hatayla ilgili ayrıntıları içerir.

* **SourceId**: System.NamingService
* **Özellik**: "**Duration_**" öneki ile başlar ve yavaş işlemi ve işlemin uygulandığı Servis Kumaşı adını tanımlar. Örneğin, ad kumaş hizmet **oluşturmak:/MyApp/MyService** çok uzun sürerse, özellik **Duration_AOCreateService.fabric:/MyApp/MyService**. "AO", bu ad ve işlem için Adlandırma bölmesinin rolüne işaret ediyor.
* **Sonraki adımlar**: Adlandırma işleminin neden başarısız olduğunu kontrol edin. Her işlemin farklı kök nedenleri olabilir. Örneğin, silme hizmeti sıkışmış olabilir. Uygulama ana bilgisayarı hizmet kodundaki bir kullanıcı hatası nedeniyle düğümüzerinde çökmeye devam ettiği için hizmet sıkışmış olabilir.

Aşağıdaki örnek, bir hizmet oluşturma işlemini gösterir. İşlem, yapılandırılan süreden daha uzun sürdü. "AO" yeniden çalışır ve "HAYR" için iş gönderir. "HAYRİ" TIMEOUT ile son işlemi tamamladı. Bu durumda, aynı yineleme hem "AO" hem de "NO" rolleri için birincildir.

```powershell
PartitionId           : 00000000-0000-0000-0000-000000001000
ReplicaId             : 131064359253133577
AggregatedHealthState : Warning
UnhealthyEvaluations  :
                        Unhealthy event: SourceId='System.NamingService', Property='Duration_AOCreateService.fabric:/MyApp/MyService', HealthState='Warning', ConsiderWarningAsError=false.

HealthEvents          :
                        SourceId              : System.RA
                        Property              : State
                        HealthState           : Ok
                        SequenceNumber        : 131064359308715535
                        SentAt                : 4/29/2016 8:38:50 PM
                        ReceivedAt            : 4/29/2016 8:39:08 PM
                        TTL                   : Infinite
                        Description           : Replica has been created.
                        RemoveWhenExpired     : False
                        IsExpired             : False
                        Transitions           : Error->Ok = 4/29/2016 8:39:08 PM, LastWarning = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_AOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064359526778775
                        SentAt                : 4/29/2016 8:39:12 PM
                        ReceivedAt            : 4/29/2016 8:39:38 PM
                        TTL                   : 00:05:00
                        Description           : The AOCreateService started at 2016-04-29 20:39:08.677 is taking longer than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM

                        SourceId              : System.NamingService
                        Property              : Duration_NOCreateService.fabric:/MyApp/MyService
                        HealthState           : Warning
                        SequenceNumber        : 131064360657607311
                        SentAt                : 4/29/2016 8:41:05 PM
                        ReceivedAt            : 4/29/2016 8:41:08 PM
                        TTL                   : 00:00:15
                        Description           : The NOCreateService started at 2016-04-29 20:39:08.689 completed with FABRIC_E_TIMEOUT in more than 30.000.
                        RemoveWhenExpired     : True
                        IsExpired             : False
                        Transitions           : Error->Warning = 4/29/2016 8:39:38 PM, LastOk = 1/1/0001 12:00:00 AM
```

## <a name="deployedapplication-system-health-reports"></a>DeployedApplication sistem sağlık raporları
**System.Hosting** dağıtılan varlıklar üzerinde yetkidir.

### <a name="activation"></a>Etkinleştirme
System.Hosting, düğümüzerinde bir uygulama başarıyla etkinleştirildiğinde Tamam olarak raporlar. Aksi takdirde, bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **Etkinleştirme**, rollout sürümü de dahil olmak üzere.
* **Sonraki adımlar**: Uygulama sağlıksızsa, etkinleştirme neden başarısız olduğunu araştırın.

Aşağıdaki örnekte başarılı bir etkinleştirme gösterilmektedir:

```powershell
PS C:\> Get-ServiceFabricDeployedApplicationHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ExcludeHealthStatistics

ApplicationName                    : fabric:/WordCount
NodeName                           : _Node_1
AggregatedHealthState              : Ok
DeployedServicePackageHealthStates : 
                                     ServiceManifestName   : WordCountServicePkg
                                     ServicePackageActivationId : 
                                     NodeName              : _Node_1
                                     AggregatedHealthState : Ok
                                     
HealthEvents                       : 
                                     SourceId              : System.Hosting
                                     Property              : Activation
                                     HealthState           : Ok
                                     SequenceNumber        : 131445249083836329
                                     SentAt                : 7/14/2017 4:55:08 PM
                                     ReceivedAt            : 7/14/2017 4:55:14 PM
                                     TTL                   : Infinite
                                     Description           : The application was activated successfully.
                                     RemoveWhenExpired     : False
                                     IsExpired             : False
                                     Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>İndirme
System.Hosting, uygulama paketi karşıdan yüklemesi başarısız olursa bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **İndir**, rollout sürümü de dahil olmak üzere.
* **Sonraki adımlar**: İndirme düğümünde neden başarısız olduğunu araştırın.

## <a name="deployedservicepackage-system-health-reports"></a>DeployedServicePackage sistem sağlık raporları
**System.Hosting** dağıtılan varlıklar üzerinde yetkidir.

### <a name="service-package-activation"></a>Servis paketi etkinleştirme
Düğümdeki servis paketi etkinleştirme başarılı olursa System.Hosting raporları tamam olarak. Aksi takdirde, bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: Aktivasyon.
* **Sonraki adımlar**: Etkinleştirme başarısız nedenini araştırın.

### <a name="code-package-activation"></a>Kod paketi etkinleştirme
Etkinleştirme başarılı olursa, System.Hosting her kod paketi için Tamam olarak raporlar. Etkinleştirme başarısız olursa, yapılandırılmış olarak bir uyarı bildirir. **CodePackage** etkinleştirmek için başarısız olursa veya yapılandırılan **CodePackageHealthErrorThreshold**daha büyük bir hata ile sona erer , barındırma bir hata raporlar. Bir hizmet paketi birden çok kod paketi içeriyorsa, her biri için bir etkinleştirme raporu oluşturulur.

* **SourceId**: System.Hosting
* **Özellik**: **CodePackageActivation** önekini kullanır ve kod paketinin adını ve giriş noktasını *CodePackageActivation:CodePackageName:SetupEntryPoint/EntryPoint*olarak içerir. Örneğin, **CodePackageActivation:Code:SetupEntryPoint**.

### <a name="service-type-registration"></a>Hizmet türü kaydı
System.Hosting, hizmet türü başarıyla kaydedilmişse tamam olarak raporlar. **ServiceTypeRegistrationTimeout**kullanılarak yapılandırıldığı gibi, kayıt zamanında yapılmazsa bir hata bildirir. Çalışma süresi kapalıysa, hizmet türü düğümden kaydedilmemiş ve barındırma bir uyarı bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **ServiceTypeRegistration** önekini kullanır ve hizmet türü adını içerir. Örneğin, **ServiceTypeRegistration:FileStoreServiceType**.

Aşağıdaki örnek, sağlıklı dağıtılan bir hizmet paketini gösterir:

```powershell
PS C:\> Get-ServiceFabricDeployedServicePackageHealth -NodeName _Node_1 -ApplicationName fabric:/WordCount -ServiceManifestName WordCountServicePkg


ApplicationName            : fabric:/WordCount
ServiceManifestName        : WordCountServicePkg
ServicePackageActivationId : 
NodeName                   : _Node_1
AggregatedHealthState      : Ok
HealthEvents               : 
                             SourceId              : System.Hosting
                             Property              : Activation
                             HealthState           : Ok
                             SequenceNumber        : 131445249084026346
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServicePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : CodePackageActivation:Code:EntryPoint
                             HealthState           : Ok
                             SequenceNumber        : 131445249084306362
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The CodePackage was activated successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
                             
                             SourceId              : System.Hosting
                             Property              : ServiceTypeRegistration:WordCountServiceType
                             HealthState           : Ok
                             SequenceNumber        : 131445249088096842
                             SentAt                : 7/14/2017 4:55:08 PM
                             ReceivedAt            : 7/14/2017 4:55:14 PM
                             TTL                   : Infinite
                             Description           : The ServiceType was registered successfully.
                             RemoveWhenExpired     : False
                             IsExpired             : False
                             Transitions           : Error->Ok = 7/14/2017 4:55:14 PM, LastWarning = 1/1/0001 12:00:00 AM
```

### <a name="download"></a>İndirme
System.Hosting, hizmet paketi karşıdan yüklemesi başarısız olursa bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **İndir**, rollout sürümü de dahil olmak üzere.
* **Sonraki adımlar**: İndirme düğümünde neden başarısız olduğunu araştırın.

### <a name="upgrade-validation"></a>Yükseltme doğrulaması
System.Hosting, yükseltme sırasında doğrulama başarısız olursa veya yükseltme düğümde başarısız olursa bir hata bildirir.

* **SourceId**: System.Hosting
* **Özellik**: **FabricUpgradeValidation** önekini kullanır ve yükseltme sürümünü içerir.
* **Açıklaması**: Karşılaşılan hataya işaret ediyor.

### <a name="undefined-node-capacity-for-resource-governance-metrics"></a>Kaynak yönetim ölçümleri için tanımlanmamış düğüm kapasitesi
System.Hosting, küme manifestosunda düğüm kapasiteleri tanımlanmamışsa ve otomatik algılama yapılandırması kapatılırsa bir uyarı bildirir. Hizmet Kumaşı, [kaynak yönetimini](service-fabric-resource-governance.md) kullanan hizmet paketi belirli bir düğümüzerinde kaydolduğunda bir sistem durumu uyarısı yükseltir.

* **SourceId**: System.Hosting
* **Özellik**: **Kaynak Yönetişim**.
* **Sonraki adımlar**: Bu sorunun üstesinden gelmek için tercih edilen yol, kullanılabilir kaynakların otomatik olarak algılanmasını sağlamak için küme bildirimini değiştirmektir. Başka bir yolu, küme bildirimini bu ölçümler için doğru belirtilen düğüm kapasiteleriyle güncelleştirmektir.

## <a name="next-steps"></a>Sonraki adımlar
* [Hizmet Kumaşı sağlık raporlarını görüntüleyin](service-fabric-view-entities-aggregated-health.md)

* [Hizmet durumunu bildirme ve kontrol etme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

* [Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

* [Hizmet Kumaş uygulama yükseltme](service-fabric-application-upgrade.md)

