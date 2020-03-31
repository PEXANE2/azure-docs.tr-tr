---
title: 'Uygulama yükseltme: yükseltme parametreleri'
description: Gerçekleştirilmesi gereken sistem durumu denetimleri ve yükseltmeyi otomatik olarak geri alma ilkeleri de dahil olmak üzere, Hizmet Dokusu uygulamasını yükseltmeyle ilgili parametreleri açıklar.
ms.topic: conceptual
ms.date: 11/08/2018
ms.openlocfilehash: 42b5c52181cfb006ae57e43c183b96a059a9c63a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75377982"
---
# <a name="application-upgrade-parameters"></a>Uygulama yükseltme parametreleri
Bu makalede, bir Azure Hizmet Kumaşı uygulamasının yükseltimi sırasında uygulanan çeşitli parametreler açıklanmaktadır. Uygulama yükseltme parametreleri yükseltme sırasında uygulanan zaman zaman larını ve sistem durumu denetimlerini denetler ve yükseltme başarısız olduğunda uygulanması gereken ilkeleri belirtirler. Uygulama parametreleri aşağıdakileri kullanarak yükseltmeler için geçerlidir:
- PowerShell
- Visual Studio
- SFCTL
- [Geri kalanı](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-startapplicationupgrade)

Uygulama yükseltmeleri, kullanıcı tarafından seçilebilen üç yükseltme modundan biri aracılığıyla başlatılır. Her modun kendi uygulama parametreleri kümesi vardır:
- Izlenen
- İzlenmeyen Otomatik
- İzlenmeyen Kılavuz

Uygulanabilir gerekli ve isteğe bağlı parametreler her bölümde aşağıdaki gibi açıklanmıştır.

## <a name="visual-studio-and-powershell-parameters"></a>Visual Studio ve PowerShell parametreleri

PowerShell kullanarak Hizmet Kumaş uygulama yükseltmeleri [Başlat-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) komutunu kullanın. Yükseltme modu, [Başlat-ServiceFabricApplicationUpgrade'e](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade)İzlenen , **İzlenmeyen** **Otomatik**veya **İzlenmemiş Manuel** parametreyi geçirerek seçilir.

Visual Studio Service Fabric uygulama yükseltme parametreleri Visual Studio Yükseltme Ayarları iletişim kutusu aracılığıyla ayarlanır. Visual Studio yükseltme modu, İzlenen , **İzlenmeyen**Otomatik veya **UnmonitoredManual**için **Yükseltme Modu** açılır kutusu kullanılarak seçilir. **UnmonitoredManual** Daha fazla bilgi için Visual [Studio'daki Hizmet Kumaşı uygulamasının yükseltmesini yapılandırın.](service-fabric-visualstudio-configure-upgrade.md)

### <a name="required-parameters"></a>Gerekli parametreler
(PS=PowerShell, VS=Visual Studio)

| Parametre | Uygulanan Öğe | Açıklama |
| --- | --- | --- |
ApplicationName |PS| Yükseltilmekte olan uygulamanın adı. Örnekler: kumaş:/VisualObjects, kumaş:/ClusterMonitor. |
UygulamaTypeVersion|PS|Yükseltmenin hedeflenen uygulama türünün sürümü. |
FailureAction |PS, VS|İzin verilen değerler **Rollback**, **Manuel**ve **Geçersiz**. *İzlenen* bir yükseltme izleme ilkesi veya sistem durumu ilkesi ihlalleriyle karşılaştığında gerçekleştirecek telafi eylemi. <br>**Geri alma,** yükseltmenin otomatik olarak yükseltme öncesi sürüme geri döneceğini belirtir. <br>**El kitabı,** yükseltmenin *UnmonitoredManual* yükseltme moduna geçeceğini gösterir. <br>**Geçersiz,** hata eyleminin geçersiz olduğunu gösterir.|
Izlenen |PS|Yükseltme modunun izlendiğini gösterir. Cmdlet yükseltme etki alanı için yükseltmeyi tamamladıktan sonra, yükseltme etki alanının ve kümenin durumu tanımladığınız sistem durumu ilkelerini karşılıyorsa, Service Fabric bir sonraki yükseltme etki alanını yükseltir. Yükseltme etki alanı veya küme sistem durumu ilkelerini karşılamazsa, yükseltme başarısız olur ve Service Fabric yükseltme etki alanının yükseltmesini geri getirir veya belirtilen ilke başına el ile moda döner. Bu, üretim ortamında uygulama yükseltmeleri için önerilen moddur. |
Yükseltme Modu | VS | İzin verilen değerler **İzlenir** (varsayılan), **İzlenmeyenOtomatik**veya **İzlenmemiş El Ile.** Ayrıntılar için bu makalede her mod için PowerShell parametrelerine bakın. |
İzlenmeyenOtomatik | PS | Yükseltme modunun izlenmeden otomatik olduğunu gösterir. Service Fabric yükseltme etki alanını yükselttikten sonra, Service Fabric uygulama durumu durumuna bakılmaksızın bir sonraki yükseltme etki alanını yükseltir. Bu mod üretim için önerilmez ve yalnızca bir uygulamanın geliştirilmesi sırasında yararlıdır. |
İzlenmeyen Kılavuz | PS | Yükseltme modunun izlenmemiş el kitabı olduğunu gösterir. Service Fabric bir yükseltme etki alanını yükselttikten sonra, *Özgeçmiş-ServiceFabricApplicationUpgrade* cmdlet'i kullanarak bir sonraki yükseltme etki alanını yükseltmenizi bekler. |

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

Sistem durumu değerlendirme parametreleri isteğe bağlıdır. Yükseltme başladığında sistem durumu değerlendirme ölçütleri belirtilmemişse, Service Fabric uygulama örneğinin ApplicationManifest.xml'inde belirtilen uygulama durumu ilkelerini kullanır.

> [!div class="mx-tdBreakAll"]
> | Parametre | Uygulanan Öğe | Açıklama |
> | --- | --- | --- |
> | UygulamaParametresi |PS, VS| Uygulama parametreleri için geçersiz kılmaları belirtir.<br>PowerShell uygulama parametreleri karma ad/değer çiftleri olarak belirtilir. Örneğin, @{ "VotingData_MinReplicaSetSize" = "3"; "VotingData_PartitionCount" = "1" }.<br>Visual Studio uygulama **parametreleri, Uygulama Parametreleri Dosya** alanındaki Yayın Hizmeti Kumaş Uygulaması iletişim kutusunda belirtilebilir.
> | Confirm |PS| İzin verilen değerler **Doğru** ve **Yanlış'tır.** Cmdlet çalıştırmadan önce onay ister. |
> | WarningasError'ı Düşünün |PS, VS |İzin verilen değerler **Doğru** ve **Yanlış'tır.** Varsayılan ayar, **False** değeridir. Yükseltme sırasında uygulamanın durumunu değerlendirirken uygulama için uyarı durumu olaylarını hata olarak değerlendirin. Varsayılan olarak, Hizmet Dokusu uyarı durumu olaylarını hata (hata) olarak değerlendirmez, bu nedenle uyarı olayları olsa bile yükseltme devam edebilir. |
> | VarsayılanHizmetTypeHealthPolicy | PS, VS |MaxPercentUnhealthyPartitionsPerService, MaxPercentUnhealthyReplicasPerPartition, MaxPercentUnhealthyServices biçiminde izlenen yükseltme için kullanmak için varsayılan hizmet türü için sistem durumu ilkesi belirtir. Örneğin, 5,10,15 aşağıdaki değerleri gösterir: MaxPercentUnhealthyPartitionsPerService = 5, MaxPercentUnhealthyReplicasPerPartition = 10, MaxPercentUnhealthyServices = 15. |
> | Force | PS, VS | İzin verilen değerler **Doğru** ve **Yanlış'tır.** Yükseltme işleminin uyarı iletisini atladığını ve sürüm numarası değişmemiş olsa bile yükseltmeyi zorladığını gösterir. Bu, yerel sınama için yararlıdır, ancak çalışma makarasına ve olası veri kaybına neden olan varolan dağıtımın kaldırılmasını gerektirdiğinden üretim ortamında kullanılması önerilmez. |
> | ForceRestart |PS, VS |Bir yapılandırmayı veya veri paketini hizmet kodunu güncelleştirmeden güncellerseniz, hizmet yalnızca ForceRestart özelliği **True**olarak ayarlanırsa yeniden başlatılır. Güncelleştirme tamamlandığında, Service Fabric hizmeti yeni bir yapılandırma paketinin veya veri paketinin kullanılabilir olduğunu fark edin. Hizmet, değişikliklerin uygulanmasından sorumludur. Gerekirse, hizmet kendisini yeniden başlatabilir. |
> | HealthCheckRetryTimeoutSec |PS, VS |Service Fabric'in yükseltmeyi başarısız olarak bildirmeden önce sistem durumu değerlendirmesi yapmaya devam ettiği süre (saniye cinsinden). Varsayılan değer 600 saniyedir. Bu süre *HealthCheckWaitDurationSec* ulaştıktan sonra başlar. Bu *HealthCheckRetryTimeout*içinde , Hizmet Kumaş uygulama sağlık birden fazla sağlık kontrolleri gerçekleştirebilir. Varsayılan değer 10 dakikadır ve uygulamanız için uygun şekilde özelleştirilmelidir. |
> | HealthCheckStableDurationSec |PS, VS |Bir sonraki yükseltme etki alanına geçmeden veya yükseltmeyi tamamlamadan önce uygulamanın kararlı olduğunu doğrulamak için süre (saniye cinsinden). Bu bekleme süresi, sağlık denetimi yapıldıktan hemen sonra fark edilmeyen sağlık değişikliklerini önlemek için kullanılır. Varsayılan değer 120 saniyedir ve uygulamanız için uygun şekilde özelleştirilmelidir. |
> | HealthCheckWaitDurationSec |PS, VS | Service Fabric uygulamanın durumunu değerlendirmeden önce yükseltme nin yükseltme etki alanında tamamlanmasından sonra bekleme süresi (saniye cinsinden). Bu süre, bir uygulamanın sağlıklı kabul edilememesi için çalışma süresi olarak da kabul edilebilir. Sistem durumu denetimi geçerse, yükseltme işlemi bir sonraki yükseltme etki alanına devam eder.  Sağlık denetimi başarısız olursa, Service Fabric *HealthCheckRetryTimeoutSec* ulaşıncaya kadar yeniden sağlık denetimi yeniden denemeden önce [UpgradeHealthCheckInterval](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-fabric-settings#clustermanager) için bekler. Varsayılan ve önerilen değer 0 saniyedir. |
> | MaxPercentUnhealthyDağıtım Uygulamaları|PS, VS |Varsayılan ve önerilen değer 0'dır. Uygulama sağlıksız olarak kabul edilmeden önce sağlıksız olabilecek ve yükseltmede başarısız olabilecek en fazla dağıtılmış uygulama sayısını [(Bkz. Sistem Durumu bölümüne](service-fabric-health-introduction.md)bakın). Bu parametre düğümdeki uygulama durumunu tanımlar ve yükseltme sırasındaki sorunları algılamaya yardımcı olur. Genellikle, uygulamanın yinelemeleri, uygulamanın sağlıklı görünmesini sağlayarak yükseltmenin devam etmesine olanak tanıyan diğer düğüme yük dengeli olur. Sıkı bir *MaxPercentUnhealthyDeployedApplications* sağlık belirterek, Hizmet Kumaş hızlı bir şekilde uygulama paketi ile ilgili bir sorun algılayabilir ve başarısız bir hızlı yükseltme üretmek yardımcı olur. |
> | MaxPercentSağlıksızHizmetler |PS, VS |*DefaultServiceTypeHealthPolicy* ve *ServiceTypeHealthPolicy*için bir parametre. Varsayılan ve önerilen değer 0'dır. Uygulama sağlıksız kabul edilmeden önce sağlıksız olabilecek ve yükseltmede başarısız olabilecek en fazla hizmet sayısını uygulama örneğinde belirtin. |
> | MaxPercentUnhealthyPartitionsPerService|PS, VS |*DefaultServiceTypeHealthPolicy* ve *ServiceTypeHealthPolicy*için bir parametre. Varsayılan ve önerilen değer 0'dır. Hizmet sağlıksız kabul edilmeden önce sağlıksız olabilecek bir hizmetteki en fazla bölüm sayısını belirtin. |
> | MaxPercentUnhealthyReplicasPerPartition|PS, VS |*DefaultServiceTypeHealthPolicy* ve *ServiceTypeHealthPolicy*için bir parametre. Varsayılan ve önerilen değer 0'dır. Bölüm sağlıksız olarak kabul edilmeden önce sağlıksız olabilecek en fazla yineleme sayısını belirtin. |
> | ServiceTypeHealthPolicyMap | PS, VS | Bir hizmet türüne ait hizmetlerin durumunu değerlendirmek için kullanılan sistem durumu ilkesini temsil eder. Aşağıdaki biçimde bir karma tablo girişi alır: @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"} Örneğin: @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" } |
> | Zaman OutSec | PS, VS | Zaman ödeme süresini işlem için saniyeler içinde belirtir. |
> | UpgradeDomainTimeoutSec |PS, VS |Tek bir yükseltme etki alanını yükseltmek için maksimum süre (saniye cinsinden). Bu zaman ayarına ulaşılırsa, yükseltme durur ve *FailureAction*ayarını temel alan devam eder. Varsayılan değer hiçbir zaman (Sonsuz) değildir ve uygulamanız için uygun şekilde özelleştirilmelidir. |
> | YükseltmeReplicaSetCheckTimeoutSec |PS, VS |Saniyecinsinden ölçülür.<br>**Devletsiz hizmet**--Tek bir yükseltme etki alanı içinde, Service Fabric hizmetin ek örneklerinin kullanılabilir olmasını sağlamaya çalışır. Hedef örnek sayısı birden fazlaysa, Service Fabric en fazla zaman sonu değerine kadar birden fazla örneğin kullanılabilir olmasını bekler. Bu zaman-out *UpgradeReplicaSetCheckTimeoutSec* özelliği kullanılarak belirtilir. Zaman aşımı süresi dolduğunda, Hizmet Kumaşı, hizmet örneklerinin sayısına bakılmaksızın yükseltmeye devam eder. Hedef örnek sayısı birse, Hizmet Kumaşı beklemez ve yükseltmeye hemen devam eder.<br><br>**Stateful service**--Tek bir yükseltme etki alanı içinde, Service Fabric yineleme kümesinin bir çoğunluk olduğundan emin olmaya çalışır. Service Fabric, maksimum zaman sonu değerine *(UpgradeReplicaSetCheckTimeoutSec* özelliği tarafından belirtilir) kadar bir çoğunluk kullanılabilir olmasını bekler. Zaman aşımı süresi dolduğunda, Hizmet Kumaşı yeter sayısıne bakılmaksızın yükseltmeye devam eder. Bu ayar, ileri doğru yuvarlanırken asla (sonsuz) ve geri dönerken 1200 saniye olarak ayarlanır. |
> | UpgradeTimeoutSec |PS, VS |Tüm yükseltme için geçerli olan bir zaman-out (saniye cinsinden). Bu zaman ayarı yapılırsa, yükseltme durur ve *FailureAction* tetiklenir. Varsayılan değer hiçbir zaman (Sonsuz) değildir ve uygulamanız için uygun şekilde özelleştirilmelidir. |
> | WhatIf | PS | İzin verilen değerler **Doğru** ve **Yanlış'tır.** Cmdlet çalıştırılıyorsa ne olacağını gösterir. Cmdlet çalıştırılmaz. |

*MaxPercentUnhealthyServices*, *MaxPercentUnhealthyPartitionsPerService*, ve *MaxPercentUnhealthyReplicasPerPartition* kriterleri bir uygulama örneği için hizmet türü başına belirtilebilir. Bu parametrelerin hizmet başına ayarlanması, bir uygulamanın farklı değerlendirme ilkeleriyle farklı hizmet türleri içermesine olanak tanır. Örneğin, bir devlet ağ geçidi hizmet türü belirli bir uygulama örneği için bir stateful motor hizmet türü farklı bir *MaxPercentUnhealthyPartitionsPerService* olabilir.

## <a name="sfctl-parameters"></a>SFCTL parametreleri

Service Fabric CLI kullanarak Hizmet Kumaşı uygulama yükseltmeleri, aşağıdaki gerekli ve isteğe bağlı parametrelerle birlikte [sfctl uygulama yükseltme](https://docs.microsoft.com/azure/service-fabric/service-fabric-sfctl-application#sfctl-application-upgrade) komutunu kullanır.

### <a name="required-parameters"></a>Gerekli parametreler

| Parametre | Açıklama |
| --- | --- |
| uygulama-id  |Yükseltilmekte olan uygulamanın kimliği. <br> Bu genellikle 'kumaş olmadan uygulamanın tam adıdır:' URI düzeni. Sürüm 6.0'dan başlayarak hiyerarşik adlar\~' ' karakteriyle sınırlandırılır. Örneğin, uygulama adı 'kumaş:/myapp/app1' ise, uygulama kimliği 6.0+ ve önceki sürümlerde 'myapp/app1' olarak 'myapp/app1'\~olacaktır.|
uygulama sürümü |Yükseltmenin hedeflenen uygulama türünün sürümü.|
parametreler  |Uygulamayı yükseltirken uygulanacak uygulama parametresi geçersiz kılmalarının JSON kodlanmış listesi.|

### <a name="optional-parameters"></a>İsteğe bağlı parametreler

| Parametre | Açıklama |
| --- | --- |
varsayılan hizmet-sağlık ilkesi | [JSON,](https://docs.microsoft.com/rest/api/servicefabric/sfclient-model-servicetypehealthpolicy) bir hizmet türünün durumunu değerlendirmek için varsayılan olarak kullanılan sistem durumu ilkesinin belirtimini kodladı. Harita varsayılan olarak boştur. |
hata-eylem | İzin verilen değerler **Rollback**, **Manuel**ve **Geçersiz**. *İzlenen* bir yükseltme izleme ilkesi veya sistem durumu ilkesi ihlalleriyle karşılaştığında gerçekleştirecek telafi eylemi. <br>**Geri alma,** yükseltmenin otomatik olarak yükseltme öncesi sürüme geri döneceğini belirtir. <br>**El kitabı,** yükseltmenin *UnmonitoredManual* yükseltme moduna geçeceğini gösterir. <br>**Geçersiz,** hata eyleminin geçersiz olduğunu gösterir.|
kuvvet yeniden başlatma | Bir yapılandırmayı veya veri paketini hizmet kodunu güncelleştirmeden güncellerseniz, hizmet yalnızca ForceRestart özelliği **True**olarak ayarlanırsa yeniden başlatılır. Güncelleştirme tamamlandığında, Service Fabric hizmeti yeni bir yapılandırma paketinin veya veri paketinin kullanılabilir olduğunu fark edin. Hizmet, değişikliklerin uygulanmasından sorumludur. Gerekirse, hizmet kendisini yeniden başlatabilir. |
sağlık-check-retry-zaman- zaman | *FailureAction* yürütülmeden önce uygulama veya küme sağlıksız olduğunda sistem durumu değerlendirmesini yeniden deneme süresi. İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan: PT0H10M0S. |
sağlık-check-kararlı-süresi | Yükseltme bir sonraki yükseltme etki alanına geçmeden önce uygulamanın veya kümenin sağlıklı kalması gereken süre. İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan: PT0H2M0S. |
sağlık-kontrol-bekleme süresi | Sistem durumu ilkeleri uygulamadan önce bir yükseltme etki alanını tamamladıktan sonra bekleme süresi. İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan: 0.|
max-sağlıksız-uygulamalar | Varsayılan ve önerilen değer 0'dır. Uygulama sağlıksız olarak kabul edilmeden önce sağlıksız olabilecek ve yükseltmede başarısız olabilecek en fazla dağıtılmış uygulama sayısını [(Bkz. Sistem Durumu bölümüne](service-fabric-health-introduction.md)bakın). Bu parametre düğümdeki uygulama durumunu tanımlar ve yükseltme sırasındaki sorunları algılamaya yardımcı olur. Genellikle, uygulamanın yinelemeleri, uygulamanın sağlıklı görünmesini sağlayarak yükseltmenin devam etmesine olanak tanıyan diğer düğüme yük dengeli olur. Katı bir *max-sağlıksız uygulamalar* sağlık belirterek, Hizmet Kumaş hızlı bir şekilde uygulama paketi ile ilgili bir sorun algılayabilir ve başarısız bir hızlı yükseltme üretmek yardımcı olur. 0 ile 100 arasında bir sayı olarak temsil edilir. |
mod | İzin verilen değerler **İzlenir**, **UpgradeMode**, **UnmonitoredAuto , UnmonitoredManual**. **UnmonitoredManual** Varsayılan olarak **UnmonitoredAuto'dur.** Bu değerlerin açıklamaları için Visual Studio ve PowerShell *Gerekli Parametreler* bölümüne bakın.|
çoğaltma-ayar-çek-zaman ayarı |Saniyecinsinden ölçülür. <br>**Devletsiz hizmet**--Tek bir yükseltme etki alanı içinde, Service Fabric hizmetin ek örneklerinin kullanılabilir olmasını sağlamaya çalışır. Hedef örnek sayısı birden fazlaysa, Service Fabric en fazla zaman sonu değerine kadar birden fazla örneğin kullanılabilir olmasını bekler. Bu zaman-out *çoğaltma-ayarlı-çek-zaman ödeme* özelliği kullanılarak belirtilir. Zaman aşımı süresi dolduğunda, Hizmet Kumaşı, hizmet örneklerinin sayısına bakılmaksızın yükseltmeye devam eder. Hedef örnek sayısı birse, Hizmet Kumaşı beklemez ve yükseltmeye hemen devam eder.<br><br>**Stateful service**--Tek bir yükseltme etki alanı içinde, Service Fabric yineleme kümesinin bir çoğunluk olduğundan emin olmaya çalışır. Service Fabric, maksimum zaman çıkış değerine *(çoğaltma ayarlı-denetleme-zaman sonu* özelliği tarafından belirtilir) kadar bir çoğunluk kullanılabilir olmasını bekler. Zaman aşımı süresi dolduğunda, Hizmet Kumaşı yeter sayısıne bakılmaksızın yükseltmeye devam eder. Bu ayar, ileri doğru yuvarlanırken asla (sonsuz) ve geri dönerken 1200 saniye olarak ayarlanır. |
hizmet-sağlık politikası | JSON, hizmet türü adı başına hizmet türü sağlık ilkesine sahip haritayı kodladı. Harita boş varsayılan olmak. [Parametre JSON formatı. .](https://docs.microsoft.com/rest/api/servicefabric/sfclient-model-applicationhealthpolicy#servicetypehealthpolicymap) "Değer" bölümü için JSON **MaxPercentUnhealthyServices**içerir , **MaxPercentUnhealthyPartitionsPerService**, ve **MaxPercentUnhealthyReplicasPerPartition**. Bu parametrelerin açıklamaları için Visual Studio ve PowerShell İsteğe Bağlı Parametreler bölümüne bakın.
timeout | Zaman ödeme süresini işlem için saniyeler içinde belirtir. Varsayılan: 60. |
upgrade-etki alanı-zaman ayarı | *FailureAction* yürütülmeden önce her yükseltme etki alanının tamamlanması gereken süre. İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan değer hiçbir zaman (Sonsuz) değildir ve uygulamanız için uygun şekilde özelleştirilmelidir. Varsayılan: P10675199DT02H48M05.4775807S. |
yükseltme-zaman ayarı | *FailureAction* yürütülmeden önce her yükseltme etki alanının tamamlanması gereken süre. İlk olarak ISO 8601 süresini temsil eden bir dize olarak yorumlanır. Bu başarısız olursa, o zaman milisaniye toplam sayısını temsil eden bir sayı olarak yorumlanır. Varsayılan değer hiçbir zaman (Sonsuz) değildir ve uygulamanız için uygun şekilde özelleştirilmelidir. Varsayılan: P10675199DT02H48M05.4775807S.|
uyarı-as-hata | İzin verilen değerler **Doğru** ve **Yanlış'tır.** Varsayılan ayar, **False** değeridir. Bayrak olarak geçirilebilir. Yükseltme sırasında uygulamanın durumunu değerlendirirken uygulama için uyarı durumu olaylarını hata olarak değerlendirin. Varsayılan olarak, Hizmet Dokusu uyarı durumu olaylarını hata (hata) olarak değerlendirmez, bu nedenle uyarı olayları olsa bile yükseltme devam edebilir. |

## <a name="next-steps"></a>Sonraki adımlar
[Visual Studio kullanarak Uygulama yükseltme](service-fabric-application-upgrade-tutorial.md) Visual Studio kullanarak bir uygulama yükseltme ile size yol.

[Powershell kullanarak Uygulamayükseltme PowerShell](service-fabric-application-upgrade-tutorial-powershell.md) kullanarak bir uygulama yükseltme ile size yol.

[Linux'ta Service Fabric CLI kullanarak Uygulamanızı yükseltme,](service-fabric-application-lifecycle-sfctl.md#upgrade-application) Service Fabric CLI'yi kullanarak bir uygulama yükseltmesi ile size yol verir.

[Service Fabric Eclipse Plugin kullanarak uygulamanızı yükseltme](service-fabric-get-started-eclipse.md#upgrade-your-service-fabric-java-application)

[Veri Serileştirme'nin](service-fabric-application-upgrade-data-serialization.md)nasıl kullanılacağını öğrenerek uygulama yükseltmelerinizi uyumlu hale getirin.

[Gelişmiş Konular'a](service-fabric-application-upgrade-advanced.md)atıfta bulunarak uygulamanızı yükseltirken gelişmiş işlevselliği nasıl kullanacağınızı öğrenin.

[Sorun Giderme Uygulama Yükseltmeleri'ndeki](service-fabric-application-upgrade-troubleshooting.md)adımlara atıfta bulunarak uygulama yükseltmelerinde sık karşılaşılan sorunları giderin.
