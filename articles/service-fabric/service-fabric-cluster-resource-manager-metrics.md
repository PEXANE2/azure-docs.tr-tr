---
title: Ölçümleri kullanarak Azure Hizmet Kumaşı uygulama yükünü yönetme
description: Hizmet kaynağı tüketimini yönetmek için Hizmet Kumaşı'ndaki ölçümleri nasıl yapılandıracağınız ve kullanacağınız hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: ea21502cdab35b261e20af7f23b7b522f77c6667
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451991"
---
# <a name="managing-resource-consumption-and-load-in-service-fabric-with-metrics"></a>Hizmet Kumaşında kaynak tüketimini ve yükü ölçümlerle yönetme
*Ölçümler,* hizmetlerinizin önemsediği ve kümedeki düğümler tarafından sağlanan kaynaklardır. Metrik, hizmetlerinizin performansını iyileştirmek veya izlemek için yönetmek istediğiniz her şeydir. Örneğin, hizmetinizin aşırı yüklenip yüklenmediğinizi öğrenmek için bellek tüketimini izleyebilirsiniz. Başka bir kullanım, daha iyi performans elde etmek için hizmetin belleğin daha az kısıtlandığı başka bir yere taşınıp taşınamayacağını anlamaktır.

Bellek, Disk ve CPU kullanımı gibi şeyler ölçümlere örnektir. Bu ölçümler, yönetilmesi gereken düğümdeki fiziksel kaynaklara karşılık gelen fiziksel ölçümlerdir. Ölçümler de (ve genellikle) mantıksal ölçümler olabilir. Mantıksal ölçümler "MyWorkQueueDepth" veya "MessagesToProcess" veya "TotalRecords" gibi şeylerdir. Mantıksal ölçümler uygulama tanımlı dır ve dolaylı olarak bazı fiziksel kaynak tüketimine karşılık gelir. Fiziksel kaynakların tüketimini hizmet başına olarak ölçmek ve raporlamak zor olabileceğinden, mantıksal ölçümler yaygındır. Service Fabric'in bazı varsayılan ölçümler sağlamasının nedeni, kendi fiziksel ölçümlerinizi ölçme nin ve raporlamanın karmaşıklığıdır.

## <a name="default-metrics"></a>Varsayılan ölçümler
Hizmetinizi yazmaya ve dağıtmaya başlamak istediğinizi varsayalım. Bu noktada hangi fiziksel veya mantıksal kaynakları tükettiğinizi bilemezsinz. Sorun değil! Hizmet Kumaş Küme Kaynak Yöneticisi, başka ölçümler belirtilmediğinde bazı varsayılan ölçümler kullanır. Bunlar:

  - PrimaryCount - düğümdeki Birincil yineleme sayısı 
  - ReplicaCount - düğümdeki toplam durum lu yinelemesayısı
  - Sayı - düğümdeki tüm hizmet nesnelerinin (durumsuz ve durumlu) sayısı

| Ölçüm | Stateless Instance Load | Stateful İkincil Yük | Stateful Birincil Yük | Ağırlık |
| --- | --- | --- | --- | --- |
| Birincil Sayma |0 |0 |1 |Yüksek |
| Çoğaltma Sayısı |0 |1 |1 |Orta |
| Sayı |1 |1 |1 |Düşük |


Temel iş yükleri için varsayılan ölçümler kümedeki düzgün bir çalışma dağılımı sağlar. Aşağıdaki örnekte, iki hizmet oluşturduğumuzda ve dengeleme için varsayılan ölçümlere güvendiğimizde neler olacağını görelim. İlk hizmet üç bölüm ve üç hedef çoğaltma kümesi boyutu ile devlet hizmetidir. İkinci hizmet, bir bölümve üç örnek sayısı ile devletsiz bir hizmettir.

İşte elde etmek:

<center>

![Varsayılan Ölçümlerle Küme Düzeni][Image1]
</center>

Dikkat edilecek bazı noktalar:
  - Devlet hizmeti için birincil yinelemeler birkaç düğüm e göre dağıtılır
  - Aynı bölüm için yinelemeler farklı düğümlerde
  - Toplam öncelik ve ikincilik sayısı kümede dağıtılır
  - Toplam hizmet nesnesi sayısı her düğüme eşit olarak ayrılır

Iyi!

Varsayılan ölçümler başlangıç olarak harika çalışır. Ancak, varsayılan ölçümler sizi yalnızca şimdiye kadar taşıyacaktır. Örneğin: Seçtiğiniz bölümleme şemasının tüm bölümler tarafından mükemmel bir şekilde kullanılması olasılığı nedir? Belirli bir hizmetin yükünün zaman içinde sabit olma, hatta şu anda birden fazla bölüm arasında aynı olma olasılığı nedir?

Varsayılan ölçümlerle çalıştırabilirsiniz. Ancak, bunu yapmak genellikle küme kullanımının istediğinizden daha düşük ve daha düzensiz olduğu anlamına gelir. Bunun nedeni, varsayılan ölçümlerin uyarlanabilir olmaması ve her şeyin eşdeğer olduğunu varsaymaolmasıdır. Örneğin, meşgul olan ve her ikisi de birincilsayı ölçümüne "1" katkıda bulunmayan bir Birincil. En kötü durumda, yalnızca varsayılan ölçümleri kullanmak da fazla zamanlanmış düğümlere neden olabilir ve performans sorunlarına neden olabilir. Kümenizden en iyi şekilde yararlanmak ve performans sorunlarından kaçınmak istiyorsanız, özel ölçümler ve dinamik yük raporlaması kullanmanız gerekir.

## <a name="custom-metrics"></a>Özel ölçümler
Ölçümler, hizmeti oluştururken hizmet başına örnek bazında yapılandırılır.

Herhangi bir metnin bunu açıklayan bazı özellikleri vardır: ad, ağırlık ve varsayılan yük.

* Metrik Adı: Metrik adı. Metrik ad, Kaynak Yöneticisi'nin bakış açısından küme içindeki metrik için benzersiz bir tanımlayıcıdır.
* Ağırlık: Metrik ağırlık, bu ölçümün bu hizmet için diğer ölçümlere göre ne kadar önemli olduğunu tanımlar.
* Varsayılan Yük: Hizmetin durumsuz veya durum dolu olup olmadığına bağlı olarak varsayılan yük farklı şekilde temsil edilir.
  * Durum disun hizmetler için her metnin Varsayılan Yük adında tek bir özelliği vardır
  * Devlet hizmetleri için şunları tanımlarsınız:
    * PrimaryDefaultLoad: Birincil olduğunda bu hizmetin tükettiği bu metnin varsayılan tutarı
    * İkincil Varsayılan Yük: Bu hizmetin ikincil olduğunda tükettiği bu ölçümün varsayılan tutarı

> [!NOTE]
> Özel ölçümler tanımlıyorsanız ve varsayılan ölçümleri _de_ kullanmak istiyorsanız, varsayılan ölçümleri _açıkça_ geri eklemeniz ve bunlar için ağırlıkları ve değerleri tanımlamanız gerekir. Bunun nedeni, varsayılan ölçümler ve özel ölçümleriniz arasındaki ilişkiyi tanımlamanız gerektiğidir. Örneğin, Belki De ConnectionCount veya WorkQueueDepth birincil dağıtım daha fazla bakım. Varsayılan olarak Birincil Sayım ölçümünün ağırlığı Yüksektir, bu nedenle önceliklerini sağlamak için diğer ölçümlerinizi eklediğinizde bu ölçüyü Orta'ya düşürmek istersiniz.
>

### <a name="defining-metrics-for-your-service---an-example"></a>Hizmetiniz için ölçümleri tanımlama - bir örnek
Aşağıdaki yapılandırmayı istediğinizi varsayalım:

  - Hizmetiniz "ConnectionCount" adlı bir metrik rapor
  - Varsayılan ölçümleri de kullanmak istiyorsunuz 
  - Bazı ölçümler yaptınız ve normalde bu hizmetin birincil bir kopyasının 20 birim "ConnectionCount" kapladığını biliyorsunuz.
  - İkinciller 5 adet "ConnectionCount" kullanır
  - "ConnectionCount"ın, bu hizmetin performansını yönetme açısından en önemli ölçüt olduğunu biliyorsunuz.
  - Yine de Birincil kopyaların dengede olmasını istiyorsunuz. Birincil kopyaları dengeleme genellikle ne olursa olsun iyi bir fikirdir. Bu, bazı düğüm veya hata etki alanının kaybolmasının birincil yinelemelerin büyük bir çoğunluğunu etkilemesini önlemeye yardımcı olur. 
  - Aksi takdirde, varsayılan ölçümler iyi

Bu metrik yapılandırmaya sahip bir hizmet oluşturmak için yazacağınız kod aşağıda veda edebilirsiniz:

Kod:

```csharp
StatefulServiceDescription serviceDescription = new StatefulServiceDescription();
StatefulServiceLoadMetricDescription connectionMetric = new StatefulServiceLoadMetricDescription();
connectionMetric.Name = "ConnectionCount";
connectionMetric.PrimaryDefaultLoad = 20;
connectionMetric.SecondaryDefaultLoad = 5;
connectionMetric.Weight = ServiceLoadMetricWeight.High;

StatefulServiceLoadMetricDescription primaryCountMetric = new StatefulServiceLoadMetricDescription();
primaryCountMetric.Name = "PrimaryCount";
primaryCountMetric.PrimaryDefaultLoad = 1;
primaryCountMetric.SecondaryDefaultLoad = 0;
primaryCountMetric.Weight = ServiceLoadMetricWeight.Medium;

StatefulServiceLoadMetricDescription replicaCountMetric = new StatefulServiceLoadMetricDescription();
replicaCountMetric.Name = "ReplicaCount";
replicaCountMetric.PrimaryDefaultLoad = 1;
replicaCountMetric.SecondaryDefaultLoad = 1;
replicaCountMetric.Weight = ServiceLoadMetricWeight.Low;

StatefulServiceLoadMetricDescription totalCountMetric = new StatefulServiceLoadMetricDescription();
totalCountMetric.Name = "Count";
totalCountMetric.PrimaryDefaultLoad = 1;
totalCountMetric.SecondaryDefaultLoad = 1;
totalCountMetric.Weight = ServiceLoadMetricWeight.Low;

serviceDescription.Metrics.Add(connectionMetric);
serviceDescription.Metrics.Add(primaryCountMetric);
serviceDescription.Metrics.Add(replicaCountMetric);
serviceDescription.Metrics.Add(totalCountMetric);

await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("ConnectionCount,High,20,5”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

> [!NOTE]
> Yukarıdaki örnekler ve bu belgenin geri kalanı, her hizmet için ayrı ayrı ölçüm yönetmeyi açıklar. Hizmetleriniz için ölçümleri hizmet türü düzeyinde tanımlamak da _mümkündür._ Bu, bunları hizmet bildirimlerinizde belirterek gerçekleştirilir. Tür düzeyi ölçümlerinin tanımlanması çeşitli nedenlerden dolayı önerilmez. İlk neden, metrik adlar genellikle ortama özgü olmasıdır. Yerinde sağlam bir sözleşme olmadığı sürece, bir ortamda metrik "Çekirdekler" diğerlerinde "MiliCores" veya "CoReS" olmadığından emin olamaz. Ölçümleriniz bildiriminizde tanımlanmışsa, ortam başına yeni bildirimler oluşturmanız gerekir. Bu genellikle yönetim zorluklarına yol açabilir sadece küçük farklılıklar ile farklı manifestolar, bir çoğalması yol açar.  
>
> Metrik yükler genellikle hizmet başına örnek olarak atanır. Örneğin, CustomerA için hizmetin yalnızca hafifçe kullanmayı planlayan bir örneğini oluşturduğunuzu varsayalım. Ayrıca, daha büyük bir iş yüküne sahip CustomerB için başka bir iş yükü oluşturduğunuzu da varsayalım. Bu durumda, büyük olasılıkla bu hizmetler için varsayılan yükleri tweak istiyorum. Bildirimler aracılığıyla tanımlanmış ölçümlerinve yüklerin varsa ve bu senaryoyu desteklemek istiyorsanız, her müşteri için farklı uygulama ve hizmet türleri gerektirir. Hizmet oluşturma zamanında tanımlanan değerler, bildirimde tanımlananları geçersiz kılar, böylece belirli varsayılanları ayarlamak için bunu kullanabilirsiniz. Ancak, bunu yapmak, bildirimlerde bildirilen değerlerin hizmetin gerçekte çalıştığı değerlerle eşleşmemesi için neden olur. Bu karışıklığa yol açabilir. 
>

Bir hatırlatma olarak: yalnızca varsayılan ölçümleri kullanmak istiyorsanız, yalnızca varsayılan ölçümleri kullanmak istiyorsanız, yalnızca metrik koleksiyonuna dokunmanız veya hizmetinizi oluştururken özel bir şey yapmanız gerekmez. Varsayılan ölçümler, başka sıyrık tanımlanmadığında otomatik olarak kullanılır. 

Şimdi, bu ayarların her birini daha ayrıntılı olarak gözden geçirelim ve etkilediği davranışlardan bahsedelim.

## <a name="load"></a>Yükleme
Ölçümleri tanımlamanın tüm amacı bazı yükü temsil etmektir. *Yük,* belirli bir metnin belirli bir düğümüzerinde belirli bir hizmet örneği veya yineleme tarafından ne kadarının tüketilme nedenidir. Yük hemen hemen her noktada yapılandırılabilir. Örnek:

  - Bir hizmet oluşturulduğunda yük tanımlanabilir. Buna _varsayılan yük_denir.
  - Hizmet oluşturulduktan sonra bir hizmet için varsayılan yükler de dahil olmak üzere metrik bilgiler güncelleştirilebilir. Buna _bir hizmeti güncelleştirme_denir. 
  - Belirli bir bölümün yükleri, bu hizmetin varsayılan değerlerine sıfırlanabilir. Buna _bölüm yükünü sıfırlama_denir.
  - Yük, çalışma süresi boyunca dinamik olarak hizmet nesnesi bazında raporlanabilir. Buna _raporlama yükü_denir. 
  
Tüm bu stratejiler ömrü boyunca aynı hizmet içinde kullanılabilir. 

## <a name="default-load"></a>Varsayılan yük
*Varsayılan yük,* bu hizmetin her hizmet nesnesinin (durum durumu belirteç örneği veya durum lu yineleme) ne kadarının tükettiğidir. Küme Kaynak Yöneticisi, dinamik yük raporu gibi başka bilgiler alana kadar hizmet nesnesinin yükü için bu numarayı kullanır. Daha basit hizmetler için varsayılan yük statik bir tanımdır. Varsayılan yük hiçbir zaman güncelleştirilemez ve hizmetin ömrü boyunca kullanılır. Varsayılan yükler, belirli miktarda kaynağın farklı iş yüklerine adandığı ve değişmediği basit kapasite planlama senaryoları için harika çalışır.

> [!NOTE]
> Kümenizdeki düğümler için kapasite yönetimi ve kapasiteleri tanımlama hakkında daha fazla bilgi için lütfen [bu makaleye](service-fabric-cluster-resource-manager-cluster-description.md#capacity)bakın.
> 

Küme Kaynak Yöneticisi, devlet hizmetlerinin Öncelikler ve İkinciler için farklı bir varsayılan yük belirtmesine olanak tanır. Devletsiz hizmetler yalnızca tüm örnekler için geçerli olan bir değer belirtebilir. Devlet hizmetleri için, birincil ve ikincil yinelemeler için varsayılan yük genellikle farklı, çünkü yinelemeler her rolde farklı türde iş yapar. Örneğin, Öncelikler genellikle hem okuma hem de yazma hizmeti ve hesaplama yükünün çoğunu ele, ikinci basamak ise yok. Genellikle birincil yineleme için varsayılan yük ikincil yinelemeler için varsayılan yükdaha yüksektir. Gerçek sayılar kendi ölçülerinize bağlı olmalıdır.

## <a name="dynamic-load"></a>Dinamik yük
Diyelim ki bir süredir hizmetini yürütüyorsun. Bazı izleme ile, fark ettik ki:

1. Belirli bir hizmetin bazı bölümleri veya örnekleri diğerlerinden daha fazla kaynak tüketir
2. Bazı hizmetlerin zaman içinde değişen yükü vardır.

Bu tür yük dalgalanmalarına neden olabilecek bir sürü şey var. Örneğin, farklı hizmetler veya bölümler farklı gereksinimleri olan farklı müşterilerle ilişkilidir. Hizmetin yaptığı iş miktarı gün boyunca değiştiğinden yük de değişebilir. Nedeni ne olursa olsun, genellikle varsayılan olarak kullanabileceğiniz tek bir sayı yoktur. Kümeden en iyi şekilde yararlanmak istiyorsanız, bu özellikle doğrudur. Varsayılan yük için seçtiğiniz herhangi bir değer bazen yanlıştır. Yanlış varsayılan yükler, Küme Kaynak Yöneticisi'nin kaynakların üzerinde veya altında ayrılmasıyla sonuçlanır. Sonuç olarak, Küme Kaynak Yöneticisi kümenin dengeli olduğunu düşünse de, üzerinde veya altında kullanılan düğümler vardır. Varsayılan yükler, ilk yerleşim için bazı bilgiler sağladıkları için hala iyidir, ancak gerçek iş yükleri için tam bir hikaye değildir. Değişen kaynak gereksinimlerini doğru bir şekilde yakalamak için Cluster Kaynak Yöneticisi, her hizmet nesnesinin çalışma süresi sırasında kendi yükünü güncelleştirmesine izin verir. Buna dinamik yük raporlaması denir.

Dinamik yük raporları, yinelemelerin veya örneklerin kullanım ömürleri boyunca ayırma/bildirilen metrik yükünü ayarlamalarına olanak sağlar. Soğuk olan ve herhangi bir iş yapmayan bir hizmet çoğaltma veya örneği genellikle belirli bir metrik düşük miktarda kullandığını bildirir. Meşgul bir yineleme veya örnek daha fazla kullandıklarını bildirir.

Yineleme veya örnek başına yük raporlama, Küme Kaynak Yöneticisi'nin kümedeki tek tek hizmet nesnelerini yeniden düzenlemesine olanak tanır. Hizmetleri yeniden düzenlemek, gereksinim duydukları kaynakları elde etmelerine yardımcı olur. Meşgul hizmetler, şu anda soğuk olan veya daha az iş yapan diğer yinelemelerden veya örneklerden kaynakları etkili bir şekilde "geri almak" için alır.

Güvenilir Hizmetler içinde, yükü dinamik olarak bildiren kod aşağıdaki gibi görünür:

Kod:

```csharp
this.Partition.ReportLoad(new List<LoadMetric> { new LoadMetric("CurrentConnectionCount", 1234), new LoadMetric("metric1", 42) });
```

Bir hizmet, oluşturma zamanında tanımlanan ölçümlerden herhangi birini raporedebilir. Bir hizmet, kullanmak üzere yapılandırılmamış bir metrik için yük bildiriyorsa, Service Fabric bu raporu yoksa. Geçerli olan aynı anda bildirilen başka ölçümler varsa, bu raporlar kabul edilir. Hizmet kodu, bildiği tüm ölçümleri ölçebilir ve bildirebilir ve operatörler hizmet kodunu değiştirmek zorunda kalmadan kullanılacak metrik yapılandırmayı belirtebilir. 

### <a name="updating-a-services-metric-configuration"></a>Bir hizmetin metrik yapılandırmasını güncelleştirme
Hizmetle ilişkili ölçümlerlistesi ve bu ölçümlerin özellikleri, hizmet canlıyken dinamik olarak güncelleştirilebilir. Bu deneme ve esneklik sağlar. Bu yararlı olduğunda bazı örnekler şunlardır:

  - belirli bir hizmet için bir buggy raporu ile bir metrik devre dışı bırakma
  - ölçümlerin ağırlıklarını istenilen davranışa göre yeniden yapılandırma
  - kod dağıtıldıktan ve diğer mekanizmalar aracılığıyla doğrulandıktan sonra yeni bir metriği etkinleştirme
  - gözlemlenen davranış ve tüketime dayalı olarak bir hizmetiçin varsayılan yükü değiştirme

Metrik yapılandırmayı değiştirmek için `FabricClient.ServiceManagementClient.UpdateServiceAsync` ana API'ler C# ve `Update-ServiceFabricService` PowerShell'dedir. Bu API'lerle belirttiğiniz bilgiler, hizmetiçin varolan metrik bilgilerin hemen yerini alır. 

## <a name="mixing-default-load-values-and-dynamic-load-reports"></a>Varsayılan yük değerlerini ve dinamik yük raporlarını karıştırma
Varsayılan yük ve dinamik yükler aynı hizmet için kullanılabilir. Bir hizmet hem varsayılan yük hem de dinamik yük raporlarını kullandığında, varsayılan yük dinamik raporlar ortaya çıkana kadar bir tahmin görevi gösterir. Varsayılan yük iyidir, çünkü Küme Kaynak Yöneticisi'ne çalışmak için bir şey verir. Varsayılan yük, Cluster Resource Manager'ın hizmet nesnelerini oluşturulduklarında iyi konumlara yerleştirmesine olanak tanır. Varsayılan yük bilgisi sağlanmadıysa, hizmetlerin yerleşimi etkin bir şekilde rasgele olur. Yük raporları daha sonra geldiğinde ilk rasgele yerleşim genellikle yanlıştır ve Küme Kaynak Yöneticisi hizmetleri taşımak zorundadır.

Önceki örneğimizi ele alalım ve bazı özel ölçümler ve dinamik yük raporlaması eklediğimizde neler olacağını görelim. Bu örnekte, örnek metrik olarak "MemoryInMb" kullanıyoruz.

> [!NOTE]
> Bellek, Service Fabric'in [yönetebileceği](service-fabric-resource-governance.md)sistem ölçümlerinden biridir ve bunu kendiniz bildirmek genellikle zordur. Bellek tüketimi hakkında rapor vermenizi beklemiyoruz; Bellek burada Küme Kaynak Yöneticisi'nin yetenekleri hakkında bilgi edinmeye yardımcı olarak kullanılır.
>

Devlet hizmetinin ilk olarak aşağıdaki komutla oluşturulduğunu varsayalım:

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName –Stateful -MinReplicaSetSize 3 -TargetReplicaSetSize 3 -PartitionSchemeSingleton –Metric @("MemoryInMb,High,21,11”,"PrimaryCount,Medium,1,0”,"ReplicaCount,Low,1,1”,"Count,Low,1,1”)
```

Bir hatırlatma olarak, bu sözdizimi ("MetricName, MetricWeight, PrimaryDefaultLoad, SecondaryDefaultLoad") olur.

Olası bir küme düzeninin nasıl görünebileceğini görelim:

<center>

![Küme Varsayılan ve Özel ölçümlerle dengelenmiş][Image2]
</center>

Kayda değer bazı şeyler:

* Bir bölüm içindeki ikincil yinelemelerin her birinin kendi yükü olabilir
* Genel ölçümler dengeli görünüyor. Bellek için, maksimum ve minimum yük arasındaki oran 1,75'tir (en fazla yüke sahip düğüm N3, en az N2 ve 28/16 = 1,75'tir).

Hala açıklamamız gereken bazı şeyler var:

* 1.75'lik bir oranın makul olup olmadığını ne belirledi? Küme Kaynak Yöneticisi bunun yeterli olup olmadığını veya yapılacak daha çok iş olup olmadığını nasıl anlar?
* Dengeleme ne zaman olur?
* Memory'nin "Yüksek" ağırlıklı olması ne anlama geliyor?

## <a name="metric-weights"></a>Metrik ağırlıklar
Farklı hizmetler arasında aynı ölçümleri izlemek önemlidir. Küme Kaynak Yöneticisi'nin kümedeki tüketimi izlemesine, düğümler arasında tüketimi dengelemesine ve düğümlerin kapasitenin üzerine çıkmamasını sağlamasına olanak tanıyan bu genel görünümdür. Ancak, hizmetler aynı ölçümün önemi konusunda farklı görünümlere sahip olabilir. Ayrıca, birçok ölçüm ve çok sayıda hizmetiçeren bir kümede, tüm ölçümler için mükemmel dengelenmiş çözümler olmayabilir. Küme Kaynak Yöneticisi bu durumları nasıl ele almalıdır?

Metrik ağırlıklar, cluster Kaynak Yöneticisi'nin mükemmel bir yanıt olmadığında kümeyi nasıl dengeleneceğe karar vermesine olanak sağlar. Metrik ağırlıklar, Küme Kaynak Yöneticisi'nin belirli hizmetleri farklı şekilde dengelemesine de olanak sağlar. Ölçümler dört farklı ağırlık düzeyine sahip olabilir: Sıfır, Düşük, Orta ve Yüksek. Sıfır ağırlığına sahip bir metrik, şeylerin dengeli olup olmadığını göz önünde bulundurarak hiçbir şey katkıda bulunmaz. Ancak, yükü hala kapasite yönetimine katkıda bulunur. Sıfır ağırlığı olan ölçümler hala yararlıdır ve sık sık hizmet davranışı ve performans izlemenin bir parçası olarak kullanılır. [Bu makalede,](service-fabric-diagnostics-event-generation-infra.md) hizmetlerinizin izlenmesi ve tanılanması için ölçümlerin kullanımı hakkında daha fazla bilgi verilmektedir. 

Kümedeki farklı metrik ağırlıkların gerçek etkisi, Küme Kaynak Yöneticisi'nin farklı çözümler üretmesidir. Metrik ağırlıklar Küme Kaynak Yöneticisi'ne belirli ölçümlerin diğerlerinden daha önemli olduğunu söyler. Mükemmel bir çözüm olmadığında Cluster Resource Manager, yüksek ağırlıklı ölçümleri daha iyi dengeleyen çözümleri tercih edebilir. Bir hizmet belirli bir ölçümün önemsiz olduğunu düşünüyorsa, bu ölçümün kullanımını dengesiz bulabilir. Bu, başka bir hizmetin, önemli olan bazı ölçümlerin eşit dağılımını elde etmesine olanak tanır.

Bazı yük raporlarının bir örneğine ve farklı metrik ağırlıkların kümedeki farklı ayırmalarla nasıl sonuçdoğurduğuna bakalım. Bu örnekte, ölçümlerin göreli ağırlığının değiştirilmesinin Küme Kaynak Yöneticisi'nin farklı hizmetler düzenlemeleri oluşturmasına neden olduğunu görüyoruz.

<center>

![Metrik Ağırlık Örneği ve Dengeleme Çözümlerine Etkisi][Image3]
</center>

Bu örnekte, tümü iki farklı ölçüm, MetricA ve MetricB için farklı değerler bildiren dört farklı hizmet vardır. Bir durumda, metrica tanımlamak tüm hizmetler en önemli (Ağırlık = Yüksek) ve MetricB önemsiz olarak (Ağırlık = Düşük). Sonuç olarak, Cluster Resource Manager'ın hizmetleri MetricA'nın MetricB'den daha dengeli olması için yerleştirdiğini görüyoruz. "Daha dengeli", MetricA'nın MetricB'den daha düşük standart sapmaya sahip olduğu anlamına gelir. İkinci durumda, metrik ağırlıkları tersine çeviriyoruz. Sonuç olarak, Cluster Resource Manager, MetricB'nin MetricA'dan daha dengeli olduğu bir ayırma bulmak için A ve B hizmetlerini değiştirir.

> [!NOTE]
> Metrik ağırlıklar Küme Kaynak Yöneticisi'nin nasıl dengelenmesi gerektiğini belirler, ancak dengeleme ne zaman gerçekleşmeyeceğini belirler. Dengeleme hakkında daha fazla bilgi için [bu makaleye](service-fabric-cluster-resource-manager-balancing.md) göz atın
>

### <a name="global-metric-weights"></a>Küresel metrik ağırlıklar
ServiceA'nın MetricA'yı Yüksek ağırlık olarak tanımladığını ve ServiceB'in MetricA'nın ağırlığını Düşük veya Sıfır olarak ayarlayıştı. Alıştıktan sonraki gerçek ağırlık nedir?

Her metrik için izlenen birden çok ağırlık vardır. İlk ağırlık, hizmet oluşturulduğunda metrik için tanımlanan ağırlıktır. Diğer ağırlık otomatik olarak hesaplanan küresel bir ağırlıktır. Küme Kaynak Yöneticisi, çözümleri puanlamada bu ağırlıkları her iki ağırlıkkullanır. Her iki ağırlığı da hesaba katmak önemlidir. Bu, Küme Kaynak Yöneticisi'nin her hizmeti kendi önceliklerine göre dengelemesine ve kümenin bir bütün olarak doğru şekilde ayrılmasını sağlamasına olanak tanır.

Küme Kaynak Yöneticisi hem küresel hem de yerel dengeyi önemsemezse ne olur? Küresel olarak dengeli, ancak bireysel hizmetler için kaynak dengesinin düşük olmasına neden olan çözümler oluşturmak kolaydır. Aşağıdaki örnekte, yalnızca varsayılan ölçümlerle yapılandırılan bir hizmete bakalım ve yalnızca genel bakiye dikkate alınınca neler olduğunu görelim:

<center>

![Küresel Tek Çözümün Etkisi][Image4]
</center>

Sadece küresel dengeye dayalı en iyi örnekte, küme bir bütün olarak gerçekten dengelidir. Tüm düğümler aynı sayıda öncelikler ve aynı sayı toplam yinelemeleri vardır. Ancak, bu ayırmanın gerçek etkisine bakarsanız o kadar da iyi değildir: herhangi bir düğümün kaybı belirli bir iş yükünü orantısız olarak etkiler, çünkü tüm önceliklerini devre dışı alır. Örneğin, ilk düğüm Circle hizmetinin üç farklı bölümü için üç ön öncelik başarısız olursa tüm kaybolur. Tersine, Üçgen ve Altıgen hizmetleri kendi bölümleri bir kopyasını kaybetmek var. Bu, aşağı yinelemekurtarmak zorunda dışında hiçbir bozulmaya neden olur.

Alttaki örnekte, Küme Kaynak Yöneticisi yinelemeleri hem genel hem de hizmet başına dengeyi temel alan dağıtmıştır. Çözümün puanını hesaplarken ağırlığın çoğunu küresel çözüme, (yapılandırılabilir) kısmı da tek tek hizmetlere verir. Bir metrin genel bakiyesi, her hizmetteki metrik ağırlıkların ortalamasına göre hesaplanır. Her hizmet kendi tanımlanmış metrik ağırlıklarına göre dengelenir. Bu, hizmetlerin kendi ihtiyaçlarına göre kendi içlerinde dengeli olmasını sağlar. Sonuç olarak, aynı ilk düğüm başarısız olursa, hata tüm hizmetlerin tüm bölümlerine dağıtılır. Her birinin etkisi aynıdır.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetlerin yapılandırılması hakkında daha fazla bilgi [için, Hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)(service-fabric-cluster-resource-manager-configure-services.md)
- Parçalanma Ölçümleri tanımlamak, düğümleri yaymak yerine yükü birleştirmenin bir yoludur. Parçalanmayı nasıl yapılandırıştırılabildiğini öğrenmek için [bu makaleye](service-fabric-cluster-resource-manager-defragmentation-metrics.md) bakın
- Küme Kaynak Yöneticisi'nin kümedeki yükü nasıl yönettiği ve dengeler yaptığı hakkında bilgi edinmek [için, yükü dengeleme](service-fabric-cluster-resource-manager-balancing.md) makalesine göz atın
- Baştan başlayın ve [Hizmet Kumaş Küme Kaynak Yöneticisi'ne Giriş alın](service-fabric-cluster-resource-manager-introduction.md)
- Hareket Maliyeti, Küme Kaynak Yöneticisi'ne belirli hizmetlerin taşınmasının diğerlerinden daha pahalı olduğunu bildiren bir yoldur. Hareket maliyeti hakkında daha fazla bilgi edinmek için [bu makaleye](service-fabric-cluster-resource-manager-movement-cost.md) bakın

[Image1]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-cluster-layout-with-default-metrics.png
[Image2]:./media/service-fabric-cluster-resource-manager-metrics/Service-Fabric-Resource-Manager-Dynamic-Load-Reports.png
[Image3]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-metric-weights-impact.png
[Image4]:./media/service-fabric-cluster-resource-manager-metrics/cluster-resource-manager-global-vs-local-balancing.png
