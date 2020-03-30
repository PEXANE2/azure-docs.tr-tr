---
title: Hizmet Kumaşında Sağlık Takibi
description: Kümenin ve uygulamalarının ve hizmetlerinin izlenmesini sağlayan Azure Hizmet Dokusu sistem durumu izleme modeline giriş.
author: oanapl
ms.topic: conceptual
ms.date: 2/28/2018
ms.author: oanapl
ms.openlocfilehash: 473aa2b9a74193a857390cd3e29b2b559b6084d3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282425"
---
# <a name="introduction-to-service-fabric-health-monitoring"></a>Service Fabric sistem durumu izlemeye giriş
Azure Hizmet Kumaşı, zengin, esnek ve genişletilebilir sağlık değerlendirmesi ve raporlaması sağlayan bir sistem durumu modeli sunar. Model, kümenin durumunun ve içinde çalışan hizmetlerin neredeyse gerçek zamanlı olarak izlenmesine olanak tanır. Kolayca sağlık bilgileri elde edebilir ve onlar basamaklı önce potansiyel sorunları düzeltmek ve büyük kesintilere neden. Tipik modelde, hizmetler yerel görünümlerine göre raporlar gönderir ve bu bilgiler genel küme düzeyinde bir görünüm sağlamak için toplanır.

Hizmet Kumaş bileşenleri, geçerli durumlarını bildirmek için bu zengin sistem durumu modelini kullanır. Uygulamalarınızdan gelen sağlık durumunu bildirmek için aynı mekanizmayı kullanabilirsiniz. Özel koşullarınızı yakalayan yüksek kaliteli sağlık raporlaması için yatırım yaparsanız, çalışan uygulamanız için sorunları çok daha kolay algılayabilir ve düzeltebilirsiniz.

> [!NOTE]
> İzlenen yükseltmelere yönelik bir ihtiyacı gidermek için sağlık alt sistemini başlattık. Service Fabric, tam kullanılabilirlik, kapalı kalma süresi ve kullanıcı müdahalesi olmadan en az düzeyde olmasını sağlayan izlenen uygulama ve küme yükseltmeleri sağlar. Bu hedeflere ulaşmak için yükseltme, yapılandırılan yükseltme ilkelerini temel alan durumu denetler. Yükseltme yalnızca sağlık istenilen eşiklere saygı duyduğunda devam edebilir. Aksi takdirde, yükseltme otomatik olarak geri alınır veya yöneticilere sorunları düzeltme şansı vermek için duraklatılır. Uygulama yükseltmeleri hakkında daha fazla bilgi edinmek için [bu makaleye](service-fabric-application-upgrade.md)bakın.
> 
> 

## <a name="health-store"></a>Sağlık deposu
Sistem durumu deposu, kolay alma ve değerlendirme için kümedeki varlıklar la ilgili sağlıkla ilgili bilgileri tutar. Yüksek kullanılabilirlik ve ölçeklenebilirlik sağlamak için hizmet kumaş ıstayıltmalı devlet hizmeti olarak uygulanır. Sistem deposu kumaşın bir **parçasıdır:/Sistem** uygulaması ve küme çalışır durumdayken kullanılabilir.

## <a name="health-entities-and-hierarchy"></a>Sağlık varlıkları ve hiyerarşi
Sistem durumu varlıkları, farklı varlıklar arasındaki etkileşimleri ve bağımlılıkları yakalayan mantıksal bir hiyerarşi içinde düzenlenir. Sistem durumu deposu, Service Fabric bileşenlerinden alınan raporlara göre sistem durumu varlıklarını ve hiyerarşisini otomatik olarak oluşturur.

Sağlık kuruluşları Hizmet Kumaşı varlıklarını yansıtmaktadır. (Örneğin, **sistem durumu uygulama varlığı** kümede dağıtılan bir uygulama örneğiyle eşleşirken, **sistem durumu düğümü varlığı** bir Hizmet Dokusu küme düğümüyle eşleşir.) Sistem hiyerarşisi sistem varlıklarının etkileşimlerini yakalar ve gelişmiş sistem değerlendirmesi için temelidir. [Servis Kumaşı teknik genel bakışta](service-fabric-technical-overview.md)anahtar Servis Kumaşı kavramları hakkında bilgi edinebilirsiniz. Uygulama hakkında daha fazla bilgi için [Service Fabric uygulama modeline](service-fabric-application-model.md)bakın.

Sistem durumu varlıkları ve hiyerarşisi kümenin ve uygulamaların etkili bir şekilde raporedilmesine, ayrıştırılmasına ve izlenmesine olanak sağlar. Sistem durumu modeli, kümedeki birçok hareketli parçanın sağlığının doğru ve *ayrıntılı* bir temsilini sağlar.

![Sağlık kuruluşları.][1]
Üst-alt ilişkilerine dayalı bir hiyerarşi içinde düzenlenen sağlık varlıkları.

[1]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy.png

Sağlık kuruluşları şunlardır:

* **Küme.** Service Fabric kümesinin durumunu temsil eder. Küme sağlık raporları, tüm kümeyi etkileyen koşulları açıklar. Bu koşullar kümedeki veya kümenin kendisini birden çok varlığı etkiler. Duruma göre, muhabir sorunu bir veya daha fazla sağlıksız çocuğa indirgeyemez. Örnekler ağ bölümleme veya iletişim sorunları nedeniyle küme bölünmesi beyin içerir.
* **Düğüm**. Servis Kumaş düğümünün durumunu temsil eder. Düğüm sistem durumu raporları düğüm işlevselliğini etkileyen koşulları açıklar. Bunlar genellikle üzerinde çalışan tüm dağıtılan varlıkları etkiler. Örnekler arasında disk alanı dışındaki düğüm (veya bellek, bağlantılar gibi makine çapındaki diğer özellikler) ve düğüm kapandığında verilebilir. Düğüm varlığı düğüm adı (string) ile tanımlanır.
* **Uygulama**. Kümede çalışan bir uygulama örneğinin sistem durumunu temsil eder. Uygulama sağlık raporları, uygulamanın genel durumunu etkileyen koşulları açıklar. Bunlar tek tek çocuklara (hizmetler veya dağıtılan uygulamalar) kadar daraltılamaz. Örnekler, uygulamadaki farklı hizmetler arasındaki uç-uç etkileşimi içerir. Uygulama varlığı uygulama adı (URI) ile tanımlanır.
* **Hizmet**. Kümede çalışan bir hizmetin durumunu temsil eder. Hizmet sağlık raporları, hizmetin genel durumunu etkileyen koşulları açıklar. Muhabir sorunu sağlıksız bir bölümveya yinelemeye indirgeyemez. Örnekler, tüm bölümler için sorunlara neden olan bir hizmet yapılandırması (bağlantı noktası veya dış dosya paylaşımı gibi) içerir. Hizmet varlığı hizmet adı (URI) ile tanımlanır.
* **Bölüm .** Hizmet bölümü durumunu temsil eder. Bölüm sistem durumu raporları, tüm yineleme kümesini etkileyen koşulları açıklar. Bunlara örnek olarak, yineleme sayısıhedef sayısının altında olduğunda ve bir bölüm çoğunluk kaybıolduğunda verilebilir. Bölüm varlığı bölüm kimliği (GUID) ile tanımlanır.
* **Çoğaltma**. Bir devlet hizmeti yinelemesinin veya devletsiz hizmet örneğinin sistem durumunu temsil eder. Yineleme, izleme örgütlerinin ve sistem bileşenlerinin bir uygulama için rapor edebileceği en küçük birimdir. Özel hizmetler için örnekler, işlemleri ikinci ve yavaş çoğaltma işlemleri kopyalayamayan birincil çoğaltma içerir. Ayrıca, durum bildirmemiş bir örnek, kaynakları tükendiğinde veya bağlantı sorunları olduğunda rapor verebilir. Çoğaltma varlığı bölüm kimliği (GUID) ve çoğaltma veya örnek kimliği (uzun) ile tanımlanır.
* **DeployedApplication**. Düğüm üzerinde çalışan bir *uygulamanın*durumunu temsil eder. Dağıtılan uygulama durumu raporları, düğümdeki uygulamaya özgü koşulları açıklar ve aynı düğümüzerinde dağıtılan hizmet paketlerine indirgenemez. Örnek olarak, uygulama paketinin bu düğüme indirilemediğinde yapılan hatalar ve düğümde uygulama güvenlik ilkelerinin ayarlanması yla ilgili sorunlar verilebilir. Dağıtılan uygulama, uygulama adı (URI) ve düğüm adı (string) ile tanımlanır.
* **DeployedServicePackage**. Kümedeki bir düğüm üzerinde çalışan bir hizmet paketinin durumunu temsil eder. Aynı uygulama için aynı düğümdeki diğer hizmet paketlerini etkilemeyen bir hizmet paketine özgü koşulları açıklar. Örnekler, hizmet paketinde başlatılamayan bir kod paketi ve okunamayan bir yapılandırma paketi içerir. Dağıtılan hizmet paketi uygulama adı (URI), düğüm adı (dize), hizmet bildirimi adı (dize) ve hizmet paketi etkinleştirme kimliği (string) ile tanımlanır.

Sistem durumu modelinin parçalılığı, sorunları algılamayı ve düzeltmeyi kolaylaştırır. Örneğin, bir hizmet yanıt vermiyorsa, uygulama örneğinin sağlıksız olduğunu bildirmek mümkündür. Ancak, sorun bu uygulamadaki tüm hizmetleri etkilemeyebileceğinden, bu düzeyde raporlama ideal değildir. Daha fazla bilgi bu bölüme işaret ederse, rapor sağlıksız hizmete veya belirli bir alt bölüme uygulanmalıdır. Veriler hiyerarşi boyunca otomatik olarak yüzeye çıkar ve sağlıksız bir bölüm hizmet ve uygulama düzeylerinde görünür hale getirilir. Bu toplama, sorunun temel nedenini daha hızlı bir şekilde belirlemeye ve çözmeye yardımcı olur.

Sistem durumu hiyerarşisi üst-alt ilişkilerinden oluşur. Bir küme düğümler ve uygulamalar oluşur. Uygulamalar da hizmetlere ve dağıtılmış uygulamalara sahiptir. Dağıtılan uygulamalar hizmet paketlerini dağıttı. Hizmetlerin bölümleri vardır ve her bölümün bir veya daha fazla yinelemesi vardır. Düğümler ve dağıtılmış varlıklar arasında özel bir ilişki vardır. Yetki sistemi bileşeni olan Failover Manager hizmeti tarafından bildirilen sağlıksız bir düğüm, dağıtılan uygulamaları, hizmet paketlerini ve üzerinde dağıtılan yinelemeleri etkiler.

Sistem durumu hiyerarşisi, neredeyse gerçek zamanlı bilgi olan en son sağlık raporlarına dayalı olarak sistemin en son durumunu temsil eder.
Dahili ve harici izleme örgütleri, uygulamaya özgü mantığa veya özel izlenen koşullara göre aynı varlıklar hakkında rapor verebilir. Kullanıcı raporları sistem raporlarıyla birlikte bulunur.

Büyük bir bulut hizmetinin tasarımı sırasında sağlık durumunu nasıl bildirecek ve yanıt verecek lerine yatırım yapmayı planlayın. Bu ön yatırım, hizmetin hata ayıklamasını, izlenmesini ve çalışmasını kolaylaştırır.

## <a name="health-states"></a>Sağlık durumları
Service Fabric, bir varlığın sağlıklı olup olmadığını açıklamak için üç sistem durumu kullanır: Tamam, uyarı ve hata. Sağlık deposuna gönderilen herhangi bir rapor bu durumlardan birini belirtmelidir. Sağlık değerlendirme sonucu bu durumlardan biridir.

Olası [sağlık durumları](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthstate) şunlardır:

* **Tamam**. Varlık sağlıklı. Bu konuda veya çocukları hakkında bildirilen bilinen bir sorun yoktur (varsa).
* **Uyarı**. Varlığın bazı sorunları vardır, ancak yine de düzgün çalışabilir. Örneğin, gecikmeler vardır, ancak henüz işlevsel sorunlara neden olmaz. Bazı durumlarda, uyarı durumu dış müdahale olmadan kendini düzeltebilir. Bu gibi durumlarda, sağlık raporları farkındalığı artırmak ve neler olup bittiğini görünürlük sağlar. Diğer durumlarda, uyarı durumu kullanıcı müdahalesi olmadan ciddi bir soruna dönüşebilir.
* **Hata**. Varlık sağlıksız. Düzgün çalışamadığından, varlığın durumunu düzeltmek için işlem yapılmalıdır.
* **Bilinmiyor.** Varlık sağlık deposunda yok. Bu sonuç, birden çok bileşenden sonuçları birleştiren dağıtılmış sorgulardan elde edilebilir. Örneğin, get düğüm listesi sorgusu **FailoverManager,** **ClusterManager**ve **HealthManager'a**gider; uygulama listesi sorgusu almak **ClusterManager** ve **HealthManager**gider. Bu sorgular, birden çok sistem bileşeninin sonuçlarını birleştirir. Başka bir sistem bileşeni sistem deposunda bulunmayan bir varlığı döndürürse, birleştirilen sonucun bilinmeyen sistem durumu vardır. Sağlık raporları henüz işlenmedi veya varlık silindikten sonra temizlenmiş olduğundan, bir varlık depoda değil.

## <a name="health-policies"></a>Sağlık politikaları
Sağlık deposu, bir varlığın raporlarına ve çocuklarına göre sağlıklı olup olmadığını belirlemek için sağlık ilkeleri uygular.

> [!NOTE]
> Sistem durumu ilkeleri küme manifestosunda (küme ve düğüm sistem durumu değerlendirmesi için) veya uygulama bildiriminde (uygulama değerlendirmesi ve herhangi bir alt kümesi için) belirtilebilir. Sağlık değerlendirme istekleri, yalnızca bu değerlendirme için kullanılan özel sağlık değerlendirme ilkelerinde de geçebilir.
> 
> 

Varsayılan olarak, Hizmet Kumaşı üst-alt hiyerarşik ilişki için katı kurallar (her şey sağlıklı olmalıdır) uygular. Çocuklardan birinde bile sağlıksız bir olay varsa, ebeveyn sağlıksız olarak kabul edilir.

### <a name="cluster-health-policy"></a>Küme durumu ilkesi
[Küme durumu ilkesi](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy) küme durumu ve düğüm durumu durumları değerlendirmek için kullanılır. İlke küme manifestosunda tanımlanabilir. Yoksa, varsayılan ilke (sıfır tolere hataları) kullanılır.
Küme sağlık ilkesi şunları içerir:

* [WarningasError'ı düşünün.](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror) Uyarı sağlık raporlarının sağlık değerlendirmesi sırasında hata olarak değerlendirilip değerlendirilmeyeceğini belirtir. Varsayılan: false.
* [MaxPercentSağlıksızApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthyapplications). Küme hata olarak kabul edilmeden önce sağlıksız olabilecek uygulamaların en fazla tolere edilen yüzdesini belirtir.
* [MaxPercentUnhealthyNodes](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.maxpercentunhealthynodes). Küme hata olarak kabul edilmeden önce sağlıksız olabilecek düğümlerin en fazla tolere edilen yüzdesini belirtir. Büyük kümelerde, bazı düğümler her zaman aşağı veya onarım için dışarı, bu nedenle bu yüzde bunu tolere etmek için yapılandırılmalıdır.
* [UygulamaTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.applicationtypehealthpolicymap). Uygulama türü sistem durumu ilkesi eşlemi, özel uygulama türlerini tanımlamak için küme durumu değerlendirmesi sırasında kullanılabilir. Varsayılan olarak, tüm uygulamalar bir havuza konur ve MaxPercentUnhealthyApplications ile değerlendirilir. Bazı uygulama türleri farklı şekilde ele alınırsa, bunlar genel havuzdan çıkarılabilir. Bunun yerine, bunlar haritadaki uygulama türü adlarıyla ilişkili yüzdelere göre değerlendirilir. Örneğin, bir kümede farklı türlerde binlerce uygulama ve özel uygulama türüne ait birkaç denetim uygulama örneği vardır. Denetim uygulamaları hiçbir zaman hata olmamalıdır. Bazı hataları tolere etmek için %20'ye küresel MaxPercentUnhealthyApplications belirtebilirsiniz, ancak uygulama türü için "ControlApplicationType" MaxPercentUnhealthyApplications'ı 0 olarak ayarlayın. Bu şekilde, birçok uygulamadan bazıları sağlıksız, ancak genel sağlıksız yüzdenin altındaysa, küme Uyarı olarak değerlendirilir. Uyarı durumu durumu küme yükseltmeveya Hata durumu durumu tarafından tetiklenen diğer izleme etkilemez. Ancak hata daki tek bir denetim uygulaması bile kümeyi sağlıksız hale getirerek, yükseltme yapılandırmasına bağlı olarak geri alma tetikler veya küme yükseltmesini duraklatabilir.
  Haritada tanımlanan uygulama türleri için, tüm uygulama örnekleri genel uygulama havuzundan çıkarılır. Bunlar, haritadan belirli MaxPercentUnhealthyApplications kullanılarak, uygulama türünün toplam uygulama sayısına göre değerlendirilir. Tüm uygulamalar küresel havuzda kalır ve MaxPercentUnhealthyApplications ile değerlendirilir.

Aşağıdaki örnek, küme manifestosundan bir alıntıdır. Uygulama türü haritasındaki girişleri tanımlamak için parametre adını "ApplicationTypeMaxPercentUnhealthyApplications-" ile önek ve ardından uygulama türü adı.

```xml
<FabricSettings>
  <Section Name="HealthManager/ClusterHealthPolicy">
    <Parameter Name="ConsiderWarningAsError" Value="False" />
    <Parameter Name="MaxPercentUnhealthyApplications" Value="20" />
    <Parameter Name="MaxPercentUnhealthyNodes" Value="20" />
    <Parameter Name="ApplicationTypeMaxPercentUnhealthyApplications-ControlApplicationType" Value="0" />
  </Section>
</FabricSettings>
```

### <a name="application-health-policy"></a>Uygulama durumu ilkesi
[Uygulama sağlık ilkesi,](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy) uygulamalar ve çocukları için olayların ve alt devletlerin toplanmasının değerlendirilmesinin nasıl yapıldığını açıklar. Uygulama manifestosunda, **ApplicationManifest.xml'** de, uygulama paketinde tanımlanabilir. Hiçbir ilke belirtilmemişse, Service Fabric, uyarı veya hata durumu durumunda bir sağlık raporu veya bir çocuk varsa varlığın sağlıksız olduğunu varsayar.
Yapılandırılabilir ilkeler şunlardır:

* [WarningasError'ı düşünün.](https://docs.microsoft.com/dotnet/api/system.fabric.health.clusterhealthpolicy.considerwarningaserror) Uyarı sağlık raporlarının sağlık değerlendirmesi sırasında hata olarak değerlendirilip değerlendirilmeyeceğini belirtir. Varsayılan: false.
* [MaxPercentUnhealthyDeployedApplications](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.maxpercentunhealthydeployedapplications). Uygulama hata olarak kabul edilmeden önce sağlıksız olabilecek dağıtılan uygulamaların en fazla tolere edilen yüzdesini belirtir. Bu yüzde, sağlıksız dağıtılan uygulama sayısının, uygulamaların kümede şu anda dağıtılmakta olduğu düğüm sayısına bölünmesiyle hesaplanır. Hesaplama, küçük sayıda düğümdeki bir başarısızlığa tolerans göstermek için yuvarlar. Varsayılan yüzde: sıfır.
* [VarsayılanServiceTypeHealthPolicy](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.defaultservicetypehealthpolicy). Uygulamadaki tüm hizmet türleri için varsayılan sistem durumu ilkesinin yerini alan varsayılan hizmet türü sistem durumu ilkesini belirtir.
* [ServiceTypeHealthPolicyMap](https://docs.microsoft.com/dotnet/api/system.fabric.health.applicationhealthpolicy.servicetypehealthpolicymap). Hizmet türüne göre hizmet durumu ilkelerinin bir haritasını sağlar. Bu ilkeler, belirtilen her hizmet türü için varsayılan hizmet türü sistem durumu ilkelerinin yerini alır. Örneğin, bir uygulamanın durumsuz bir ağ geçidi hizmet türü ve durumlu bir altyapı hizmet türü varsa, bunların değerlendirilmesi için sistem durumu ilkelerini farklı şekilde yapılandırabilirsiniz. Hizmet türüne göre ilke belirttiğinizde, hizmetin durumu üzerinde daha ayrıntılı denetim elde edebilirsiniz.

### <a name="service-type-health-policy"></a>Hizmet türü sistem durumu ilkesi
[Hizmet türü sağlık ilkesi,](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy) hizmetlerin ve hizmetlerin çocuklarının nasıl değerlendirilip toplanıp toplaştırılacaklarını belirtir. İlke şunları içerir:

* [MaxPercentUnhealthyPartitionsPerService](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthypartitionsperservice). Bir hizmet sağlıksız olarak kabul edilmeden önce sağlıksız bölümlerin en fazla tolere edilen yüzdesini belirtir. Varsayılan yüzde: sıfır.
* [MaxPercentUnhealthyReplicasPerPartition](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyreplicasperpartition). Bir bölüm sağlıksız olarak kabul edilmeden önce sağlıksız yinelemelerin en fazla tolere edilen yüzdesini belirtir. Varsayılan yüzde: sıfır.
* [MaxPercentUnhealthyServices](https://docs.microsoft.com/dotnet/api/system.fabric.health.servicetypehealthpolicy.maxpercentunhealthyservices). Uygulama sağlıksız olarak kabul edilmeden önce sağlıksız hizmetlerin en fazla tolere edilen yüzdesini belirtir. Varsayılan yüzde: sıfır.

Aşağıdaki örnek, bir uygulama bildiriminden bir alıntıdır:

```xml
    <Policies>
        <HealthPolicy ConsiderWarningAsError="true" MaxPercentUnhealthyDeployedApplications="20">
            <DefaultServiceTypeHealthPolicy
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="10"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="FrontEndServiceType"
                   MaxPercentUnhealthyServices="0"
                   MaxPercentUnhealthyPartitionsPerService="20"
                   MaxPercentUnhealthyReplicasPerPartition="0"/>
            <ServiceTypeHealthPolicy ServiceTypeName="BackEndServiceType"
                   MaxPercentUnhealthyServices="20"
                   MaxPercentUnhealthyPartitionsPerService="0"
                   MaxPercentUnhealthyReplicasPerPartition="0">
            </ServiceTypeHealthPolicy>
        </HealthPolicy>
    </Policies>
```

## <a name="health-evaluation"></a>Sağlık değerlendirmesi
Kullanıcılar ve otomatik leştirilmiş hizmetler herhangi bir kuruluşun durumunu istediği zaman değerlendirebilir. Bir kuruluşun durumunu değerlendirmek için, sağlık deposu varlıkla ilgili tüm sağlık raporlarını toplar ve tüm çocuklarını değerlendirir (varsa). Sistem durumu toplama algoritması, sağlık raporlarının nasıl değerlendirileceklerini ve çocuk sağlığı durumlarının nasıl toplanıp topyapılacağını belirten sağlık ilkeleri kullanır (gerektiğinde).

### <a name="health-report-aggregation"></a>Sağlık raporu toplama
Bir varlığın farklı özellikleri farklı muhabirler (sistem bileşenleri veya watchdogs) tarafından gönderilen birden çok sağlık raporları olabilir. Toplama, ilişkili sistem durumu ilkelerini, özellikle de uygulamanın veya küme durumu ilkesinin ConsiderWarningAsError üyesini kullanır. ConsiderWarningAsError uyarıların nasıl değerlendirileceklerini belirtir.

Toplu sağlık durumu, varlık la ilgili *en kötü* sağlık raporları tarafından tetiklenir. En az bir hata sistem durumu raporu varsa, toplanan sistem durumu bir hatadır.

![Hata raporu ile sağlık raporu toplama.][2]

Bir veya daha fazla hata sağlık raporu olan bir sağlık kuruluşu Hata olarak değerlendirilir. Aynı durum, sağlık durumu ne olursa olsun, süresi dolmuş bir sağlık raporu için de geçerlidir.

[2]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-error.png

Hata raporları ve bir veya daha fazla uyarı yoksa, birleştirilmiş sistem durumu, ConsiderWarningAsError ilkesi bayrağına bağlı olarak uyarı veya hatadır.

![Uyarı raporu ve ConsiderWarningAsError yanlış ile Sağlık raporu toplama.][3]

Uyarı raporu ve ConsiderWarningAsError ile sistem durumu raporu toplama yanlış (varsayılan) olarak ayarlanır.

[3]: ./media/service-fabric-health-introduction/servicefabric-health-report-eval-warning.png

### <a name="child-health-aggregation"></a>Çocuk sağlığı toplama
Bir varlığın toplu sağlık durumu, çocuk sağlığı durumlarını yansıtır (varsa). Çocuk sağlığı durumlarını toplama algoritması, varlık türüne göre geçerli olan sistem durumu ilkelerini kullanır.

![Çocuk kuruluşları sağlık toplama.][4]

Sağlık politikalarına dayalı çocuk toplama.

[4]: ./media/service-fabric-health-introduction/servicefabric-health-hierarchy-eval.png

Sağlık deposu tüm çocukları değerlendirdikten sonra, sağlıksız çocukların yapılandırılan maksimum yüzdesini temel alan sağlık durumlarını toplar. Bu yüzde, varlık ve alt türüne göre ilkeden alınır.

* Tüm çocukların Tamam durumları varsa, çocuk toplu sağlık durumu tamamdır.
* Çocukların hem Tamam hem de uyarı durumları varsa, çocuk toplu sağlık durumu uyarıdır.
* İzin verilen en fazla izin verilen sağlıksız çocukların yüzdesini saymayan hata durumları olan çocuklar varsa, toplanan üst sistem durumu bir hatadır.
* Hata durumları olan çocuklar, izin verilen en yüksek sağlıksız çocukların yüzdelerine saygı duyuyorsa, toplanan ebeveyn sağlık durumu uyarıdır.

## <a name="health-reporting"></a>Sağlık raporlama
Sistem bileşenleri, Sistem Kumaşuygulamaları ve dahili/harici watchdogs Service Fabric varlıklarına karşı rapor verebilir. Gazeteciler, izlenecekleri koşullara göre izlenen kuruluşların sağlık durumlarının *yerel* tespitlerini yapıyorlar. Herhangi bir küresel devlet e veya toplu verilere bakmaları gerekmez. İstenilen davranış basit muhabirler ve hangi bilgi göndermek için çıkarmak için birçok şey bakmak gerekir karmaşık organizmalar değil.

Sağlık verilerini sağlık deposuna göndermek için, bir muhabirin etkilenen varlığı tanımlaması ve bir sağlık raporu oluşturması gerekir. Raporu göndermek için [FabricClient.HealthClient.ReportHealth](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.healthclient.reporthealth) API'sını kullanın, powershell `Partition` `CodePackageActivationContext` cmdlets veya REST'te açıkta kalan sağlık API'lerini bildirin.

### <a name="health-reports"></a>Sağlık raporları
Kümedeki varlıkların her biri için [sistem durumu raporları](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthreport) aşağıdaki bilgileri içerir:

* **SourceId**. Sağlık olayının muhabirini benzersiz bir şekilde tanımlayan bir dize.
* **Varlık tanımlayıcısı**. Raporun uygulandığı varlığı tanımlar. Varlık [türüne](service-fabric-health-introduction.md#health-entities-and-hierarchy)göre farklılık gösterir:
  
  * Küme. Yok.
  * Düğüm. Düğüm adı (string).
  * Uygulama. Uygulama adı (URI). Kümede dağıtılan uygulama örneğinin adını temsil eder.
  * Hizmet. Servis adı (URI). Kümede dağıtılan hizmet örneğinin adını gösterir.
  * Bölüm. Bölüm Kimliği (GUID). Bölüm benzersiz tanımlayıcıyı temsil eder.
  * Çoğaltma. Devlet hizmeti çoğaltma kimliği veya devletsiz hizmet örneği kimliği (INT64).
  * DeployedApplication. Uygulama adı (URI) ve düğüm adı (string).
  * DeployedServicePackage. Uygulama adı (URI), düğüm adı (dize) ve hizmet bildirimi adı (string).
* **Özellik**. Muhabirin varlığın belirli bir özelliği için sistem durumu olayını kategorilere ayırmasına olanak tanıyan bir *dize* (sabit numaralandırma değil). Örneğin, muhabir A Node01 "Depolama" özelliğinin durumunu bildirebilir ve Muhabir B, Node01 "Bağlantı" özelliğinin durumunu bildirebilir. Sağlık deposunda, bu raporlar Node01 varlığı için ayrı sağlık olayları olarak değerlendirilir.
* **Açıklama**. Bir muhabirin sağlık olayı hakkında ayrıntılı bilgi sağlamasına olanak tanıyan bir dize. **SourceId**, **Özellik**ve **HealthState** raporu tam olarak açıklamalıdır. Açıklama, rapor hakkında insan tarafından okunabilir bilgiler ekler. Metin, yöneticilerin ve kullanıcıların sağlık raporunu anlamasını kolaylaştırır.
* **HealthState**. Raporun sağlık durumunu açıklayan [bir numaralandırma.](service-fabric-health-introduction.md#health-states) Kabul edilen değerler Tamam, Uyarı ve Hata'dır.
* **TimeToLive**. Sağlık raporunun ne kadar süreyle geçerli olduğunu gösteren bir zaman zaman açıklığı. **RemoveWhenExpired**ile birleştiğinde, sağlık deposunun süresi dolmuş olayları nasıl değerlendireceklerini bilmesini sağlar. Varsayılan olarak, değer sonsuzdur ve rapor sonsuza kadar geçerlidir.
* **Süresi Dolmuş**olarak kaldır. Bir boolean. Doğru olarak ayarlanırsa, süresi dolan sağlık raporu otomatik olarak sistem durumu deposundan kaldırılır ve rapor varlık sağlık değerlendirmesini etkilemez. Rapor yalnızca belirli bir süre için geçerli olduğunda kullanılır ve muhabirin raporu açıkça temizlemesi gerekmez. Ayrıca, sağlık deposundaki raporları silmek için de kullanılır (örneğin, bir izleme örgütü değiştirilir ve önceki kaynak ve özellik içeren rapor göndermeyi durdurur). Sağlık deposundan önceki durumları temizlemek için RemoveWhenExpired ile birlikte kısa bir TimeToLive içeren bir rapor gönderebilir. Değer yanlış olarak ayarlanırsa, süresi dolan rapor sistem durumu değerlendirmesinde bir hata olarak kabul edilir. Yanlış değer, kaynağın bu özellik üzerinde düzenli olarak rapor vermesi gerektiğini sağlık deposuna bildirir. Eğer işe yaramazsa, bekçi köpeğinde bir sorun olmalı. İzleme örgütünün sağlığı, olay bir hata olarak dikkate alınarak yakalanır.
* **Sıra Numarası**. Sürekli artması gereken pozitif bir tamsayı, raporların sırasını temsil eder. Ağ gecikmeleri veya diğer sorunlar nedeniyle geç alınan eski raporları algılamak için sistem durumu deposu tarafından kullanılır. Sıra numarası aynı varlık, kaynak ve özellik için en son uygulanan sayıdan az veya eşitse rapor reddedilir. Belirtilmemişse, sıra numarası otomatik olarak oluşturulur. Yalnızca durum geçişleri hakkında raporlama yaparken sıra numarasını koymak gerekir. Bu durumda, kaynağın hangi raporları gönderdiğini hatırlaması ve kurtarma için gereken bilgileri saklaması gerekir.

Bu dört bilgi parçası --SourceId, varlık tanımlayıcısı, Özellik ve HealthState-- her sağlık raporu için gereklidir. SourceId dizesi, sistem raporları için ayrılmış olan "**System"** önekiyle başlanmaz. Aynı varlık için, aynı kaynak ve özellik için yalnızca bir rapor vardır. Aynı kaynak ve özellik için birden çok rapor, sağlık istemcisi tarafında (toplu olarak) veya sağlık deposu tarafında birbirini geçersiz kılar. Değiştirme sıra numaralarına dayanır; daha yeni raporlar (daha yüksek sıra numaralarıyla) eski raporların yerini alır.

### <a name="health-events"></a>Sağlık etkinlikleri
Dahili olarak, sistem durumu deposu raporlardaki tüm bilgileri ve ek meta verileri içeren [sağlık olaylarını](https://docs.microsoft.com/dotnet/api/system.fabric.health.healthevent)tutar. Meta veriler, raporun sistem durumu istemcisine verildiği zamanı ve sunucu tarafında değiştirilme süresini içerir. Sistem durumu [olayları, sağlık sorguları](service-fabric-view-entities-aggregated-health.md#health-queries)tarafından döndürülür.

Eklenen meta veriler şunları içerir:

* **KaynakUtcTimestamp**. Raporun sağlık istemcisine (Eşgüdümlü Evrensel Süre) verildiği süre.
* **LastModifiedUtcTimestamp**. Raporun sunucu tarafında en son değiştirilme zamanı (Eşgüdümlü Evrensel Zaman).
* **Süresi Dolmuş**. Sorgu sistem durumu deposu tarafından yürütüldüğünde raporun süresinin dolup dolmadığını belirten bir bayrak. Bir olayın süresi yalnızca RemoveWhenExpired yanlışsa dolabilir. Aksi takdirde, olay sorgu tarafından döndürülür ve mağazadan kaldırılır.
* **Lastoktransitionat**, **LastWarningtransitionat**, **lasterrortransitionat**. Ok/warning/error geçişleri için son kez. Bu alanlar, olay için sağlık durumu geçişlerinin tarihini verir.

Durum geçiş alanları daha akıllı uyarılar veya "geçmiş" sağlık olayı bilgileri için kullanılabilir. Bunlar gibi senaryoları etkinleştirin:

* Bir özellik X dakikadan uzun süredir uyarı/hata olduğunda uyarı verin. Koşulun belirli bir süre için denetlenmesi, geçici koşullarla ilgili uyarıları önler. Örneğin, beş dakikadan uzun süredir sistem durumu uyarı ysa bir uyarı (HealthState == Warning and Now - LastWarningTransitionTime > 5 dakika) olarak çevrilebilir.
* Yalnızca son X dakikalarında değişen koşullar hakkında uyarı. Bir rapor belirtilen süreden önce zaten hatalıysa, daha önce sinyal verdiği için yoksayılabilir.
* Bir özellik uyarı ve hata arasında gidip gelmekse, ne kadar süredir sağlıksız olduğunu belirleyin (yani tamam değil). Örneğin, özellik beş dakikadan uzun süredir sağlıklı değilse bir uyarı (HealthState != Ok and Now - LastOkTransitionTime > 5 dakika) olarak çevrilebilir.

## <a name="example-report-and-evaluate-application-health"></a>Örnek: Uygulama durumunu bildirme ve değerlendirme
Aşağıdaki örnek uygulama kumaş PowerShell üzerinden bir sağlık raporu **gönderir:/WordCount** kaynak **MyWatchdog**. Sağlık raporu sonsuz TimeToLive ile, bir hata durumu durumu "kullanılabilirlik" sağlık özelliği hakkında bilgi içerir. Ardından, toplu sağlık durumu hatalarını ve bildirilen sağlık olayları listesini sağlık olayları listesinde döndüren uygulama durumunu sorgular.

```powershell
PS C:\> Send-ServiceFabricApplicationHealthReport –ApplicationName fabric:/WordCount –SourceId "MyWatchdog" –HealthProperty "Availability" –HealthState Error

PS C:\> Get-ServiceFabricApplicationHealth fabric:/WordCount -ExcludeHealthStatistics


ApplicationName                 : fabric:/WordCount
AggregatedHealthState           : Error
UnhealthyEvaluations            :
                                  Error event: SourceId='MyWatchdog', Property='Availability'.

ServiceHealthStates             :
                                  ServiceName           : fabric:/WordCount/WordCountService
                                  AggregatedHealthState : Error

                                  ServiceName           : fabric:/WordCount/WordCountWebService
                                  AggregatedHealthState : Ok

DeployedApplicationHealthStates :
                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_0
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_2
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_3
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_4
                                  AggregatedHealthState : Ok

                                  ApplicationName       : fabric:/WordCount
                                  NodeName              : _Node_1
                                  AggregatedHealthState : Ok

HealthEvents                    :
                                  SourceId              : System.CM
                                  Property              : State
                                  HealthState           : Ok
                                  SequenceNumber        : 360
                                  SentAt                : 3/22/2016 7:56:53 PM
                                  ReceivedAt            : 3/22/2016 7:56:53 PM
                                  TTL                   : Infinite
                                  Description           : Application has been created.
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Error->Ok = 3/22/2016 7:56:53 PM, LastWarning = 1/1/0001 12:00:00 AM

                                  SourceId              : MyWatchdog
                                  Property              : Availability
                                  HealthState           : Error
                                  SequenceNumber        : 131032204762818013
                                  SentAt                : 3/23/2016 3:27:56 PM
                                  ReceivedAt            : 3/23/2016 3:27:56 PM
                                  TTL                   : Infinite
                                  Description           :
                                  RemoveWhenExpired     : False
                                  IsExpired             : False
                                  Transitions           : Ok->Error = 3/23/2016 3:27:56 PM, LastWarning = 1/1/0001 12:00:00 AM
```

## <a name="health-model-usage"></a>Sağlık modeli kullanımı
Sistem durumu modeli bulut hizmetlerinin ve altta yatan Hizmet Dokusu platformunun ölçeklenmesine izin verir, çünkü izleme ve sistem durumu belirlemeleri kümedeki farklı monitörler arasında dağıtılır.
Diğer sistemler, hizmetler tarafından yayılan *tüm yararlı olabilecek* bilgileri ayrıştıran küme düzeyinde tek ve merkezi bir hizmete sahiptir. Bu yaklaşım ölçeklenebilirliklerini engeller. Ayrıca, sorunları ve olası sorunları mümkün olduğunca temel nedene yakın belirlemenize yardımcı olmak için belirli bilgileri toplamalarına da izin vermez.

Sistem durumu modeli, küme ve uygulama durumunu değerlendirmek ve izlenen yükseltmeler için izleme ve tanı lama için kullanılır. Diğer hizmetler, otomatik onarımlar gerçekleştirmek, küme sistem durumu geçmişi oluşturmak ve belirli koşullar hakkında uyarılar vermek için sistem durumu verilerini kullanır.

## <a name="next-steps"></a>Sonraki adımlar
[Hizmet Kumaşı sağlık raporlarını görüntüleyin](service-fabric-view-entities-aggregated-health.md)

[Sorun giderme için sistem sistem sistem raporlarıkullanma](service-fabric-understand-and-troubleshoot-with-system-health-reports.md)

[Hizmet durumunu bildirme ve kontrol etme](service-fabric-diagnostics-how-to-report-and-check-service-health.md)

[Özel Hizmet Kumaşı sağlık raporları ekleme](service-fabric-report-health.md)

[Hizmetleri yerel olarak izleme ve tanılama](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Hizmet Kumaş uygulama yükseltme](service-fabric-application-upgrade.md)

