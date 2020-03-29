---
title: Sorun giderme uygulama yükseltmeleri
description: Bu makalede, bir Hizmet Kumaşı uygulamasını yükseltme ve bunları nasıl çözeceği yle ilgili bazı yaygın sorunlar ele alLanmıştır.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: d462f2c2482e0fbb4d252967754a9675ed362674
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75377931"
---
# <a name="troubleshoot-application-upgrades"></a>Uygulama yükseltme ile ilgili sorunları giderme

Bu makalede, bir Azure Hizmet Kumaşı uygulamasını yükseltme ve bunların nasıl çözüleceği ile ilgili bazı yaygın sorunlar ele allanmıştır.

## <a name="troubleshoot-a-failed-application-upgrade"></a>Başarısız bir uygulama yükseltmesi sorun giderme

Yükseltme başarısız olduğunda, **Get-ServiceFabricApplicationUpgrade** komutunun çıktısı hata ayıklama için ek bilgiler içerir.  Aşağıdaki liste, ek bilgilerin nasıl kullanılabileceğini belirtir:

1. Hata türünü tanımlayın.
2. Başarısızlık nedenini belirleyin.
3. Daha fazla araştırma için bir veya daha fazla başarısız bileşeni yalıtın.

Bu bilgiler, Service **Fabric, Hata Eyleminin** geri alıp almayacağına veya yükseltmeyi askıya alıp almayacağına bakılmaksızın başarısızlığı algıladığında kullanılabilir.

### <a name="identify-the-failure-type"></a>Hata türünü tanımlama

**Get-ServiceFabricApplicationUpgrade**çıktısında, **FailureTimestampUtc,** Service Fabric tarafından bir yükseltme hatasının algılandığı zaman damgasını (UTC'de) tanımlar ve **FailureAction** tetiklenir. **FailureReason,** hatanın üç olası üst düzey nedeninden birini tanımlar:

1. UpgradeDomainTimeout - Belirli bir yükseltme etki alanının tamamlanmasının çok uzun sürdüğünü ve **UpgradeDomainTimeout'un** süresinin dolduğunu gösterir.
2. OverallUpgradeTimeout - Genel yükseltmenin tamamlanmasının çok uzun sürdüğünü ve **UpgradeTimeout'ın** süresinin dolduğunu gösterir.
3. HealthCheck - Bir güncelleştirme etki alanını yükselttikten sonra, uygulamanın belirtilen sağlık ilkeleri ve **HealthCheckRetryTimeout** süresi dolmasına göre sağlıksız kaldığını gösterir.

Bu girişler yalnızca yükseltme başarısız olduğunda ve geri dönmeye başladığında çıktıda gösterir. Daha fazla bilgi, hatanın türüne bağlı olarak görüntülenir.

### <a name="investigate-upgrade-timeouts"></a>Yükseltme zaman larını araştırma

Yükseltme zaman arızası hataları en sık hizmet kullanılabilirliği sorunları kaynaklanır. Bu paragrafı izleyen çıktı, hizmet yinelemelerinin veya örneklerinin yeni kod sürümünde başlatılamadığı yükseltmelere tipiktir. **UpgradeDomainProgressAtFailure** alanı, hata sırasında bekleyen yükseltme çalışmalarının anlık görüntüsünü yakalar.

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/DemoApp
```

```Output
ApplicationName                : fabric:/DemoApp
ApplicationTypeName            : DemoAppType
TargetApplicationTypeVersion   : v2
ApplicationParameters          : {}
StartTimestampUtc              : 4/14/2015 9:26:38 PM
FailureTimestampUtc            : 4/14/2015 9:27:05 PM
FailureReason                  : UpgradeDomainTimeout
UpgradeDomainProgressAtFailure : MYUD1

                                 NodeName            : Node4
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 744c8d9f-1d26-417e-a60e-cd48f5c098f0

                                 NodeName            : Node1
                                 UpgradePhase        : PostUpgradeSafetyCheck
                                 PendingSafetyChecks :
                                     WaitForPrimaryPlacement - PartitionId: 4b43f4d8-b26b-424e-9307-7a7a62e79750
UpgradeState                   : RollingBackCompleted
UpgradeDuration                : 00:00:46
CurrentUpgradeDomainDuration   : 00:00:00
NextUpgradeDomain              :
UpgradeDomainsStatus           : { "MYUD1" = "Completed";
                                 "MYUD2" = "Completed";
                                 "MYUD3" = "Completed" }
UpgradeKind                    : Rolling
RollingUpgradeMode             : UnmonitoredAuto
ForceRestart                   : False
UpgradeReplicaSetCheckTimeout  : 00:00:00
```

Bu örnekte, yükseltme etki *MYUD1* başarısız oldu ve iki bölüm *(744c8d9f-1d26-417e-a60e-cd48f5c098f0* ve *4b43f4d8-b26b-424e-9307-7a7a62e79750)* sıkışmış. Çalışma zamanı birincil yinelemeleri *(WaitForPrimaryPlacement)* hedef *düğümdüğüm1* ve *Düğüm4'e*yerleştiremediği için bölümler sıkışmıştı.

**Get-ServiceFabricNode** komutu, bu iki düğümün *MYUD1*yükseltme etki alanında olduğunu doğrulamak için kullanılabilir. *UpgradePhase* *PostUpgradeSafetyCheck*diyor , hangi bu güvenlik kontrolleri yükseltme etki alanında tüm düğümleri yükseltme bittikten sonra meydana geldiğini anlamına gelir. Tüm bu bilgiler, uygulama kodunun yeni sürümüyle ilgili olası bir soruna işaret eder. En sık karşılaşılan sorunlar, birincil kod yollarında açık veya promosyondaki hizmet hatalarıdır.

Yükseltme *Aşaması* *PreUpgradeSafetyCheck,* yükseltme etki alanını gerçekleştirilmeden önce hazırlarken sorunlar olduğu anlamına gelir. Bu durumda en sık karşılaşılan sorunlar, birincil kod yollarından kapanış veya indirgemedeki hizmet hatalarıdır.

Geçerli **UpgradeState** *RollingBackCompleted*, bu nedenle orijinal yükseltme otomatik olarak başarısızlık üzerine yükseltme geri aldı bir geri alma **FailureAction**ile gerçekleştirilmiş olmalıdır. Özgün yükseltme manuel **FailureAction**ile gerçekleştirildiyse, yükseltme bunun yerine uygulamanın canlı hata ayıklanmasına izin vermek için askıya alınmış durumda olacaktır.

Nadir durumlarda, genel yükseltme, sistem geçerli yükseltme etki alanı için tüm çalışmaları tamamlar gibi zamanları dışarı olursa **YükseltmeDomainProgressAtFailure** alanı boş olabilir. Bu durumda, **UpgradeTimeout** ve **UpgradeDomainTimeout** yükseltme parametre değerlerini artırmayı deneyin ve yükseltmeyi yeniden deneyin.

### <a name="investigate-health-check-failures"></a>Sistem durumu denetimi hatalarını araştırma

Sistem durumu denetimi hataları, yükseltme etki alanındaki tüm düğümlerin yükseltilmesi ve tüm güvenlik denetimlerinin geçirilmesinden sonra meydana gelebilecek çeşitli sorunlar tarafından tetiklenebilir. Bu paragrafı izleyen çıktı, başarısız sistem durumu denetimleri nedeniyle bir yükseltme hatasının tipik bir nedenidir. **SağlıksızDeğerlendirmeler** alanı, belirtilen [sağlık ilkesine](service-fabric-health-introduction.md)göre yükseltme sırasında başarısız olan sağlık denetimlerinin anlık görüntüsünü yakalar.

```powershell
Get-ServiceFabricApplicationUpgrade fabric:/DemoApp
```

```Output
ApplicationName                         : fabric:/DemoApp
ApplicationTypeName                     : DemoAppType
TargetApplicationTypeVersion            : v4
ApplicationParameters                   : {}
StartTimestampUtc                       : 4/24/2015 2:42:31 AM
UpgradeState                            : RollingForwardPending
UpgradeDuration                         : 00:00:27
CurrentUpgradeDomainDuration            : 00:00:27
NextUpgradeDomain                       : MYUD2
UpgradeDomainsStatus                    : { "MYUD1" = "Completed";
                                          "MYUD2" = "Pending";
                                          "MYUD3" = "Pending" }
UnhealthyEvaluations                    :
                                          Unhealthy services: 50% (2/4), ServiceType='PersistedServiceType', MaxPercentUnhealthyServices=0%.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc3', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='3a9911f6-a2e5-452d-89a8-09271e7e49a8', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

                                          Unhealthy service: ServiceName='fabric:/DemoApp/Svc2', AggregatedHealthState='Error'.

                                              Unhealthy partitions: 100% (1/1), MaxPercentUnhealthyPartitionsPerService=0%.

                                              Unhealthy partition: PartitionId='744c8d9f-1d26-417e-a60e-cd48f5c098f0', AggregatedHealthState='Error'.

                                                  Error event: SourceId='Replica', Property='InjectedFault'.

UpgradeKind                             : Rolling
RollingUpgradeMode                      : Monitored
FailureAction                           : Manual
ForceRestart                            : False
UpgradeReplicaSetCheckTimeout           : 49710.06:28:15
HealthCheckWaitDuration                 : 00:00:00
HealthCheckStableDuration               : 00:00:10
HealthCheckRetryTimeout                 : 00:00:10
UpgradeDomainTimeout                    : 10675199.02:48:05.4775807
UpgradeTimeout                          : 10675199.02:48:05.4775807
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
```

Sağlık kontrolü hatalarının araştırılması, öncelikle Service Fabric sağlık modelinin anlaşılmasını gerektirir. Ama böyle bir derinlemesine anlayış olmadan bile, biz iki hizmet sağlıksız olduğunu görebilirsiniz: *kumaş:/DemoApp/Svc3* ve *kumaş:/DemoApp/Svc2*, hata sağlık raporları ile birlikte ("InjectedFault" bu durumda). Bu örnekte, dört hizmetten ikisi sağlıksızdır ve varsayılan %0 sağlıksız hedefinin altındadır *(MaxPercentUnhealthyServices).*

Yükseltme, yükseltmeyi başlatırken bir **FailureAction** el kitabı belirterek başarısız olduktan sonra askıya alındı. Bu mod, başka bir işlem yapmadan önce başarısız durumdaki canlı sistemi araştırmamıza olanak tanır.

### <a name="recover-from-a-suspended-upgrade"></a>Askıya alınan yükseltmeden kurtarma

Bir geri alma **FailureAction**ile, yükseltme otomatik olarak başarısız üzerine geri rulo beri gerekli hiçbir kurtarma yoktur. El ile **FailureAction**ile birkaç kurtarma seçeneği vardır:

1.  bir geri alma tetikleme
2. Yükseltmenin geri kalan kısmını el ile devam edin
3. İzlenen yükseltmeye devam edin

**Start-ServiceFabricApplicationRollback** komutu, uygulamayı geri almaya başlamak için her zaman kullanılabilir. Komut başarıyla döndükten sonra, geri alma isteği sisteme kaydedilir ve kısa bir süre sonra başlar.

**Resume-ServiceFabricApplicationUpgrade** komutu, yükseltmenin geri kalan kısmını el ile, bir seferde bir yükseltme etki alanında devam etmek için kullanılabilir. Bu modda, sistem tarafından yalnızca güvenlik kontrolleri gerçekleştirilir. Daha fazla sağlık denetimi yapılmaz. Bu komut yalnızca *UpgradeState* *RollingForwardPending'u*gösterdiğinde kullanılabilir, bu da geçerli yükseltme etki alanının yükseltmeyi tamamladığı ancak bir sonrakinin başlatılmadığını (beklemede) anlamına gelir.

**Update-ServiceFabricApplicationUpgrade** komutu, hem güvenlik hem de sistem durumu kontrolleri gerçekleştirilirken izlenen yükseltmeyi devam ettirmek için kullanılabilir.

```powershell
Update-ServiceFabricApplicationUpgrade fabric:/DemoApp -UpgradeMode Monitored
```

```Output
UpgradeMode                             : Monitored
ForceRestart                            :
UpgradeReplicaSetCheckTimeout           :
FailureAction                           :
HealthCheckWaitDuration                 :
HealthCheckStableDuration               :
HealthCheckRetryTimeout                 :
UpgradeTimeout                          :
UpgradeDomainTimeout                    :
ConsiderWarningAsError                  :
MaxPercentUnhealthyPartitionsPerService :
MaxPercentUnhealthyReplicasPerPartition :
MaxPercentUnhealthyServices             :
MaxPercentUnhealthyDeployedApplications :
ServiceTypeHealthPolicyMap              :
```

Yükseltme, en son askıya alındığının bulunduğu yükseltme etki alanından devam eder ve öncekiyle aynı yükseltme parametrelerini ve sistem durumu ilkelerini kullanır. Gerekirse, yükseltme devam ettiğinde, önceki çıktıda gösterilen yükseltme parametreleri ve sistem durumu ilkelerinden herhangi biri aynı komutla değiştirilebilir. Bu örnekte, yükseltme, parametreler ve sistem durumu ilkeleri değişmeden İzlenen modda sürdürüldü.

## <a name="further-troubleshooting"></a>Daha fazla sorun giderme

### <a name="service-fabric-is-not-following-the-specified-health-policies"></a>Service Fabric belirtilen sağlık politikalarına uymaz

Olası Neden 1:

Service Fabric, tüm yüzdeleri sistem durumu değerlendirmesi için gerçek varlık sayısına (örneğin, yinelemeler, bölümler ve hizmetler) çevirir ve her zaman tüm varlıkları birer birime yuvarlar. Örneğin, maksimum *MaxPercentUnhealthyReplicasPerPartition%* 21 ve beş kopyaları varsa, o zaman Hizmet Kumaş en fazla`Math.Ceiling (5*0.21)`iki sağlıksız yinelemeler (yani, ) sağlar. Bu nedenle, sağlık politikaları buna göre ayarlanmalıdır.

Olası Neden 2:

Sistem durumu ilkeleri, belirli hizmet örnekleri açısından değil, toplam hizmetlerin yüzdeleri açısından belirtilir. Örneğin, yükseltmeden önce, bir uygulamada D hizmetinin sağlıksız olduğu ancak uygulamaya çok az etkisi olan dört hizmet örneği varsa, A, B, C ve D hizmeti vardır. Yükseltme sırasında bilinen sağlıksız hizmet D'yi yok saymak ve *maxPercentUnhealthyServices* parametresini yalnızca A, B ve C'nin sağlıklı olması gerektiğini varsayarak %25 olarak ayarlamak istiyoruz.

Ancak, yükseltme sırasında, C sağlıksız hale gelirken D sağlıklı hale gelebilir. Hizmetlerin yalnızca %25'i sağlıksız olduğu için yükseltme yine de başarılı olacaktır. Ancak, C'nin D yerine beklenmeyen sağlıksız olması nedeniyle beklenmeyen hatalara neden olabilir. Bu durumda, D A, B ve C farklı bir hizmet türü olarak modellenmelidir. Sistem durumu ilkeleri hizmet türüne göre belirtildiğinden, farklı hizmetlere farklı sağlıksız yüzde eşikleri uygulanabilir. 

### <a name="i-did-not-specify-a-health-policy-for-application-upgrade-but-the-upgrade-still-fails-for-some-time-outs-that-i-never-specified"></a>Uygulama yükseltmesi için bir sistem durumu ilkesi belirtmedim, ancak yükseltme hala belirtemediğim bazı zaman aşımlarında başarısız oluyor

Yükseltme isteğine sistem durumu ilkeleri sağlanamadığında, geçerli uygulama sürümünün *ApplicationManifest.xml'inden* alınır. Örneğin, Application X'i sürüm 1.0'dan sürüm 2.0'a yükseltiyorsanız, sürüm 1.0'da belirtilen uygulama durumu ilkeleri kullanılır. Yükseltme için farklı bir sistem durumu ilkesi kullanılacaksa, ilkenin uygulama yükseltme API çağrısının bir parçası olarak belirtilmesi gerekir. API çağrısının bir parçası olarak belirtilen ilkeler yalnızca yükseltme sırasında geçerlidir. Yükseltme tamamlandıktan sonra *ApplicationManifest.xml'de* belirtilen ilkeler kullanılır.

### <a name="incorrect-time-outs-are-specified"></a>Yanlış zaman zaman dilimleri belirtilir

Zaman aşımları tutarsız bir şekilde ayarlandığında ne olacağını merak etmiş olabilirsiniz. Örneğin, *YükseltmeEtki AlanıTimeout*daha az bir *YükseltmeTimeout* olabilir. Yanıt, bir hata döndürülür olmasıdır. *YükseltmeDomainTimeout* *HealthCheckWaitDuration* ve *HealthCheckRetryTimeout*toplamından daha az ise hatalar döndürülür , veya *YükseltmeDomainTimeout* *HealthCheckWaitDuration* ve *HealthCheckStableDuration*toplamından daha az ise .

### <a name="my-upgrades-are-taking-too-long"></a>Yükseltmelerim çok uzun sürüyor.

Yükseltmenin tamamlanması için gereken süre, belirtilen sağlık kontrollerine ve zaman çıkışlarına bağlıdır. Sistem durumu denetimleri ve zaman zaman ları, uygulamanın kopyalanması, dağıtılması ve stabilize edileb'sinin ne kadar sürtüldünye bağlı olduğuna bağlıdır. Zaman aşımı konusunda çok agresif olmak daha başarısız yükseltmeler anlamına gelebilir, bu nedenle daha uzun zaman aşımı ile konservatif olarak başlamanızı öneririz.

Zaman zamanlarının yükseltme süreleri ile nasıl etkileşimde olduğu hakkında hızlı bir yenileme aşağıda vereb

Yükseltme etki alanı yükseltmeleri *HealthCheckWaitDuration* + *HealthCheckStableDuration*daha hızlı tamamlayamaz.

Yükseltme hatası *HealthCheckWaitDuration* + *HealthCheckRetryTimeout*daha hızlı oluşamaz.

Yükseltme etki alanının yükseltme süresi *UpgradeDomainTimeout*ile sınırlıdır.  *HealthCheckRetryTimeout* ve *HealthCheckStableDuration* hem sıfır olmayan ve uygulamanın sağlık ileri geri geçiş tutar, daha sonra yükseltme sonunda *YükseltmeDomainTimeout*üzerinde kez dışarı . *YükseltmeDomainTimeout* geçerli yükseltme etki alanı için yükseltme başladığında geri sayma başlar.

## <a name="next-steps"></a>Sonraki adımlar

[Visual Studio kullanarak Uygulama yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme ile size yol.

[Powershell kullanarak Uygulamayükseltme PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) kullanarak bir uygulama yükseltme ile size yol.

[Yükseltme](service-fabric-application-upgrade-parameters.md)Parametreleri'ni kullanarak uygulamanızın nasıl yükselttikini kontrol edin.

[Veri Serileştirme'nin](service-fabric-application-upgrade-data-serialization.md)nasıl kullanılacağını öğrenerek uygulama yükseltmelerinizi uyumlu hale getirin.

[Gelişmiş Konular'a](service-fabric-application-upgrade-advanced.md)atıfta bulunarak uygulamanızı yükseltirken gelişmiş işlevselliği nasıl kullanacağınızı öğrenin.