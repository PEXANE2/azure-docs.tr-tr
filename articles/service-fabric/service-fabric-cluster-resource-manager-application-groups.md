---
title: Service Fabric kümesi Kaynak Yöneticisi-uygulama grupları
description: Service Fabric kümesindeki uygulama grubu işlevlerine genel bakış Kaynak Yöneticisi
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 988c7ce52125800c16aa785d5b1458604a927ecd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75452143"
---
# <a name="introduction-to-application-groups"></a>Uygulama gruplarına giriş
Service Fabric Küme Kaynak Yöneticisi genellikle kümenin tamamında yükü ( [ölçümler](service-fabric-cluster-resource-manager-metrics.md)aracılığıyla temsil edilen) dağıtarak küme kaynaklarını yönetir. Service Fabric, küme ve kümedeki düğümlerin kapasitesini [kapasiteye](service-fabric-cluster-resource-manager-cluster-description.md)göre bir bütün olarak yönetir. Ölçümler ve kapasite birçok iş yükü için harika çalışır, ancak farklı Service Fabric uygulama örneklerinin ağır kullanımını sağlayan desenler bazen ek gereksinimleri getirir. Örneğin, şunları yapmak isteyebilirsiniz:

- Bazı adlandırılmış uygulama örnekleri içindeki hizmetler için kümedeki düğümlerde bazı kapasiteyi ayırın
- Adlandırılmış bir uygulama örneği içindeki hizmetlerin üzerinde çalıştığı toplam düğüm sayısını sınırlayın (bunları kümenin tamamında yayma yerine)
- Hizmet sayısını veya BT içindeki hizmetlerin toplam kaynak tüketimini sınırlamak için adlandırılmış uygulama örneği üzerinde kapasiteler tanımlayın

Bu gereksinimleri karşılamak için Service Fabric kümesi Kaynak Yöneticisi uygulama grupları adlı bir özelliği destekler.

## <a name="limiting-the-maximum-number-of-nodes"></a>En fazla düğüm sayısını sınırlandırma
Uygulama kapasitesi için en basit kullanım örneği, bir uygulama örneğinin belirli bir maksimum düğüm sayısıyla sınırlı olması gerektiği durumdur. Bu, bu uygulama örneğindeki tüm hizmetleri bir dizi makineye birleştirir. Birleştirme, bu adlandırılmış uygulama örneği içindeki hizmetler tarafından fiziksel kaynak kullanımını tahmin etmeye veya üst sınıra denerken faydalıdır. 

Aşağıdaki görüntüde, en fazla tanımlı düğüm sayısı ile birlikte ve içeren bir uygulama örneği gösterilmektedir:

<center>

![En fazla düğüm sayısını tanımlayan uygulama örneği][Image1]
</center>

Sol örnekte, uygulamanın tanımlı en fazla düğüm sayısı ve üç hizmeti vardır. Küme Kaynak Yöneticisi, kümedeki en iyi dengeyi elde etmek için, altı kullanılabilir düğüm genelinde tüm çoğaltmaları yaymıştır (varsayılan davranış). Doğru örnekte, aynı uygulamanın üç düğüm ile sınırlı olduğunu görüyoruz.

Bu davranışı denetleyen parametreye MaximumNodes adı verilir. Bu parametre uygulama oluşturma sırasında ayarlanabilir veya zaten çalışmakta olan bir uygulama örneği için güncelleştirilir.

PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MaximumNodes 3
Update-ServiceFabricApplication –ApplicationName fabric:/AppName –MaximumNodes 5
```

C#

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MaximumNodes = 3;
await fc.ApplicationManager.CreateApplicationAsync(ad);

ApplicationUpdateDescription adUpdate = new ApplicationUpdateDescription(new Uri("fabric:/AppName"));
adUpdate.MaximumNodes = 5;
await fc.ApplicationManager.UpdateApplicationAsync(adUpdate);

```

Düğüm kümesi içinde, Küme Kaynak Yöneticisi hangi hizmet nesnelerinin birlikte yerleştirileceğini veya hangi düğümlerin kullanıldığını garanti etmez.

## <a name="application-metrics-load-and-capacity"></a>Uygulama ölçümleri, yük ve kapasite
Uygulama grupları ayrıca, belirli bir adlandırılmış uygulama örneğiyle ilişkili ölçümleri ve bu uygulama örneğinin bu ölçümler için kapasitesini tanımlamanızı sağlar. Uygulama ölçümleri, bu uygulama örneği içindeki hizmetlerin kaynak tüketimini izlemenize, ayırmanızı ve sınırlamanızı sağlar.

Her uygulama ölçümü için, ayarlanabilir iki değer vardır:

- **Toplam uygulama kapasitesi** – Bu ayar, belirli bir ölçüm için uygulamanın toplam kapasitesini temsil eder. Küme Kaynak Yöneticisi, bu uygulama örneği içinde Toplam yükün bu değeri aşmasına neden olacak yeni hizmetlerin oluşturulmasına izin vermez. Örneğin, uygulama örneğinin 10 kapasiteye sahip olduğunu ve zaten beş tane yüklü olduğunu varsayalım. Toplam 10 varsayılan yüküne sahip bir hizmet oluşturulmasına izin verilmez.
- **Maksimum düğüm kapasitesi** – Bu ayar, tek bir düğümdeki uygulamanın en fazla toplam yükünü belirtir. Yük bu kapasitenin üzerinden geçerse, yük azaldıkça, Küme Kaynak Yöneticisi çoğaltmaları diğer düğümlere taşıdığında.


PowerShell

``` posh
New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -Metrics @("MetricName:Metric1,MaximumNodeCapacity:100,MaximumApplicationCapacity:1000")
```

C#:

``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.TotalApplicationCapacity = 1000;
appMetric.MaximumNodeCapacity = 100;
ad.Metrics.Add(appMetric);
await fc.ApplicationManager.CreateApplicationAsync(ad);
```

## <a name="reserving-capacity"></a>Kapasiteyi ayırma
Uygulama grupları için başka bir yaygın kullanım, küme içindeki kaynakların belirli bir uygulama örneği için ayrılmış olmasını sağlamaktır. Alan, uygulama örneği oluşturulduğunda her zaman ayrılır.

Uygulamanın kümedeki alanı ayırma, şu durumlarda hemen gerçekleşir:
- uygulama örneği oluşturuldu ancak henüz içinde hiç hizmet yok
- uygulama örneği içindeki hizmetlerin sayısı her seferinde değişir 
- hizmetler var, ancak kaynakları tüketmiyor 

Bir uygulama örneği için kaynak ayırma, iki ek parametre belirtilmesini gerektirir: *Minimumnodes* ve *Nodereservationcapacity*

- **Minimumnodes** -uygulama örneğinin üzerinde çalışması gereken en az düğüm sayısını tanımlar.  
- **Nodereservationcapacity** -Bu ayar, uygulama için ölçüm başına olur. Bu değer, bu uygulama içindeki hizmetlerin çalıştırıldığı herhangi bir düğümdeki uygulama için ayrılan ölçüm miktarıdır.

**Minimumnodes** ve **Nodereservationcapacity** birleştirilirken, küme içindeki uygulama için en düşük yük ayırması garanti altına alır. Kümede, gereken toplam ayırmayı aşan daha az kalan kapasite varsa, uygulamanın oluşturulması başarısız olur. 

Kapasite ayırma örneğine göz atalım:

<center>

![Ayrılmış kapasiteyi tanımlayan uygulama örnekleri][Image2]
</center>

Sol örnekte, uygulamalarda hiçbir uygulama kapasitesi tanımlı değildir. Küme Kaynak Yöneticisi her şeyi normal kurallara göre dengeler.

Sağdaki örnekte, Application1 aşağıdaki ayarlarla oluşturulduğunu varsayalım:

- MinimumNodes iki olarak ayarlandı
- İle tanımlanmış bir uygulama ölçümü
  - NodeReservationCapacity 20

PowerShell

 ``` posh
 New-ServiceFabricApplication -ApplicationName fabric:/AppName -ApplicationTypeName AppType1 -ApplicationTypeVersion 1.0.0.0 -MinimumNodes 2 -Metrics @("MetricName:Metric1,NodeReservationCapacity:20")
 ```

C#

 ``` csharp
ApplicationDescription ad = new ApplicationDescription();
ad.ApplicationName = new Uri("fabric:/AppName");
ad.ApplicationTypeName = "AppType1";
ad.ApplicationTypeVersion = "1.0.0.0";
ad.MinimumNodes = 2;

var appMetric = new ApplicationMetricDescription();
appMetric.Name = "Metric1";
appMetric.NodeReservationCapacity = 20;

ad.Metrics.Add(appMetric);

await fc.ApplicationManager.CreateApplicationAsync(ad);
```

Service Fabric, kapasiteyi iki düğüm üzerinde ayırır ve aynı zamanda Application1 içindeki hizmetler tarafından tüketilmekte olmasa bile Application1 ' deki hizmetlerin bu kapasiteyi kullanmasına izin vermez. Bu ayrılmış uygulama kapasitesi tüketilen ve bu düğümde ve küme içindeki kalan kapasiteye göre sayımlar olarak değerlendirilir.  Rezervasyon, kalan küme kapasitesinden hemen düşülür, ancak ayrılmış tüketim yalnızca en az bir hizmet nesnesi yerleştirildiğinde belirli bir düğümün kapasitesinden düşülür. Daha sonra bu rezervasyon, kaynaklar yalnızca gerektiğinde düğümlerde ayrıldığından esneklik ve daha iyi kaynak kullanımına izin verir.

## <a name="obtaining-the-application-load-information"></a>Uygulama yükleme bilgilerini alma
Bir veya daha fazla ölçüm için tanımlanmış bir uygulama kapasitesi olan her uygulama için, hizmetlerinin çoğaltmaları tarafından bildirilen toplam yük hakkındaki bilgileri elde edebilirsiniz.

PowerShell

``` posh
Get-ServiceFabricApplicationLoadInformation –ApplicationName fabric:/MyApplication1
```

C#

``` csharp
var v = await fc.QueryManager.GetApplicationLoadInformationAsync("fabric:/MyApplication1");
var metrics = v.ApplicationLoadMetricInformation;
foreach (ApplicationLoadMetricInformation metric in metrics)
{
    Console.WriteLine(metric.ApplicationCapacity);  //total capacity for this metric in this application instance
    Console.WriteLine(metric.ReservationCapacity);  //reserved capacity for this metric in this application instance
    Console.WriteLine(metric.ApplicationLoad);  //current load for this metric in this application instance
}
```

ApplicationLoad sorgusu, uygulama için belirtilen uygulama kapasitesi hakkında temel bilgileri döndürür. Bu bilgiler, en düşük düğümleri ve en fazla düğüm bilgilerini ve uygulamanın Şu anda işgal eden numarasını içerir. Ayrıca, aşağıdakiler de dahil olmak üzere her bir uygulama yük ölçümü hakkında bilgiler içerir:

* Ölçüm adı: ölçümün adı.
* Ayırma kapasitesi: Bu uygulama için kümede ayrılan küme kapasitesi.
* Uygulama yükü: Bu uygulamanın alt çoğaltmalarının toplam yüklemesi.
* Uygulama kapasitesi: uygulama yükünün izin verilen en yüksek değeri.

## <a name="removing-application-capacity"></a>Uygulama kapasitesini kaldırma
Uygulama kapasitesi parametreleri bir uygulama için ayarlandıktan sonra, uygulama API 'Lerini veya PowerShell cmdlet 'lerini kullanarak bu uygulamalar kaldırılabilir. Örneğin:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Bu komut uygulama örneğinden tüm uygulama kapasite yönetimi parametrelerini kaldırır. Bu, MinimumNodes, Maximumnode ve varsa uygulamanın ölçümlerini içerir. Komutun etkisi anında gerçekleşir. Bu komut tamamlandıktan sonra Küme Kaynak Yöneticisi, uygulamaları yönetmek için varsayılan davranışı kullanır. Uygulama kapasitesi parametreleri aracılığıyla `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`yeniden belirtilebilir.

### <a name="restrictions-on-application-capacity"></a>Uygulama kapasitesinde kısıtlamalar
Uygulama kapasitesi parametrelerinde kullanılması gereken çeşitli kısıtlamalar vardır. Doğrulama hataları varsa, hiçbir değişiklik gerçekleşmiyor.

- Tüm tamsayı parametreleri negatif olmayan sayılar olmalıdır.
- MinimumNodes hiçbir şekilde Maximumnode değerinden büyük olmamalıdır.
- Bir yük ölçümü için kapasiteler tanımlanmışsa, bu kurallara uymalıdır:
  - Düğüm ayırma kapasitesi, maksimum düğüm kapasitesinden büyük olmamalıdır. Örneğin, düğümdeki "CPU" ölçüsünün kapasitesini iki birim olarak SINIRLANDIRAMAZ ve her düğümde üç birim ayırmaya çalışırsınız.
  - MaximumNodes belirtilmişse, MaximumNodes ve maksimum düğüm kapasitesi çarpımı Toplam uygulama kapasitesinden büyük olmamalıdır. Örneğin, "CPU" yükleme ölçümü için maksimum düğüm kapasitesi sekiz olarak ayarlanır. Ayrıca, en fazla düğümü 10 olarak ayarlayadığınızı varsayalım. Bu durumda, bu yük ölçümü için toplam uygulama kapasitesi 80 ' den büyük olmalıdır.

Kısıtlamalar, hem uygulama oluşturma hem de güncelleştirmeler sırasında zorlanır.

## <a name="how-not-to-use-application-capacity"></a>Uygulama kapasitesini kullanma
- Uygulamayı _belirli_ bir düğüm alt kümesiyle kısıtlamak Için uygulama grubu özelliklerini kullanmayı denemeyin. Diğer bir deyişle, uygulamanın en fazla beş düğüm üzerinde çalıştığını ancak kümedeki belirli beş düğümü belirtmez. Bir uygulamayı belirli düğümlere kısıtlama, hizmetler için yerleştirme kısıtlamaları kullanılarak elde edilebilir.
- Aynı uygulamadan iki hizmetin aynı düğümlere yerleştirildiğinden emin olmak için uygulama kapasitesini kullanmayı denemeyin. Bunun yerine [benzeşim](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) veya [yerleştirme kısıtlamalarını](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi için [Hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Kaynak Yöneticisi kümenin, kümedeki yükü nasıl yönettiğini ve dengelemediğini öğrenmek için [Yük Dengeleme](service-fabric-cluster-resource-manager-balancing.md) sayfasındaki makaleye göz atın
- Baştan başlayın ve [Service Fabric kümesine giriş yapın Kaynak Yöneticisi](service-fabric-cluster-resource-manager-introduction.md)
- Ölçümlerin genel olarak nasıl çalıştığı hakkında daha fazla bilgi için [Service Fabric yük ölçümlerine](service-fabric-cluster-resource-manager-metrics.md) göre okuyun
- Küme Kaynak Yöneticisi, kümeyi açıklamak için birçok seçenek içerir. Bunlarla ilgili daha fazla bilgi edinmek için [Service Fabric kümesini açıklama](service-fabric-cluster-resource-manager-cluster-description.md) konusunda bu makaleye göz atın

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
