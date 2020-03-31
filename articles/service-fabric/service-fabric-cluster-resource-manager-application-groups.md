---
title: Hizmet Kumaş Küme Kaynak Yöneticisi - Uygulama Grupları
description: Hizmet Kumaş Küme Kaynak Yöneticisi'ndeki Uygulama Grubu işlevine genel bakış
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 988c7ce52125800c16aa785d5b1458604a927ecd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75452143"
---
# <a name="introduction-to-application-groups"></a>Başvuru Gruplarına Giriş
Service Fabric'in Küme Kaynak Yöneticisi genellikle yükü [(Ölçümler](service-fabric-cluster-resource-manager-metrics.md)aracılığıyla temsil edilen) küme tüm kümeye eşit olarak yayarak küme kaynaklarını yönetir. Service Fabric, kümedeki düğümlerin kapasitesini ve kümeyi kapasite [ile](service-fabric-cluster-resource-manager-cluster-description.md)bir bütün olarak yönetir. Ölçümler ve kapasite birçok iş yükü için harika çalışır, ancak farklı Hizmet Kumaş Uygulama Örneklerinden yoğun olarak kullanan desenler bazen ek gereksinimler getirir. Örneğin şunları yapmak isteyebilirsiniz:

- Kümedeki düğümlerde bazı kapasiteleri bazı adlandırılmış uygulama örneklerindeki hizmetler için ayırın
- Adlandırılmış bir uygulama örneği içindeki hizmetlerin çalıştırdığı toplam düğüm sayısını sınırlandırın (bunları tüm kümeye yaymak yerine)
- Adlandırılmış uygulama örneğinin kendisindeki kapasiteleri, hizmet sayısını veya içindeki hizmetlerin toplam kaynak tüketimini sınırlamak için tanımlayın

Bu gereksinimleri karşılamak için, Hizmet Kumaş Küme Kaynak Yöneticisi Uygulama Grupları adlı bir özelliği destekler.

## <a name="limiting-the-maximum-number-of-nodes"></a>En fazla düğüm sayısını sınırlama
Uygulama kapasitesi için en basit kullanım örneği, bir uygulama örneğinin belirli bir maksimum düğüm sayısıyla sınırlanması gerektiğinde yapılır. Bu, bu uygulama örneğindeki tüm hizmetleri belirli sayıda makinede birleştirir. Konsolidasyon, bu adlandırılmış uygulama örneğindeki hizmetlerin fiziksel kaynak kullanımını tahmin etmeye veya kapatmaya çalışırken yararlıdır. 

Aşağıdaki resimde, tanımlanan en fazla sayıda düğüm içeren ve olmayan bir uygulama örneği gösterilmektedir:

<center>

![Maksimum Düğüm Sayısını Tanımlayan Uygulama Örneği][Image1]
</center>

Soldaki örnekte, uygulama tanımlanan en fazla düğüm sayısına sahip değildir ve üç hizmeti vardır. Küme Kaynak Yöneticisi kümedeki en iyi dengeyi (varsayılan davranış) elde etmek için tüm yinelemeleri kullanılabilir düğümlere yaydı. Doğru örnekte, aynı uygulamayı üç düğümle sınırlı olarak görüyoruz.

Bu davranışı denetleyen parametreye MaximumNodes denir. Bu parametre uygulama oluşturma sırasında ayarlanabilir veya zaten çalışan bir uygulama örneği için güncelleştirildi.

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

Küme Kaynak Yöneticisi, düğüm kümesi içinde hangi hizmet nesnelerinin bir araya getirilip yerleştirildiğini veya hangi düğümlerin kullanılacağını garanti etmez.

## <a name="application-metrics-load-and-capacity"></a>Uygulama Ölçümleri, Yük ve Kapasite
Uygulama Grupları ayrıca, belirli bir adlandırılmış uygulama örneğiyle ilişkili ölçümleri ve bu ölçümler için bu uygulama örneğinin kapasitesini tanımlamanıza da olanak sağlar. Uygulama ölçümleri, bu uygulama örneğindeki hizmetlerin kaynak tüketimini izlemenize, rezerve etmenize ve sınırlamanıza olanak sağlar.

Her uygulama ölçümü için ayarlanabilecek iki değer vardır:

- **Toplam Uygulama Kapasitesi** – Bu ayar, belirli bir metrik için uygulamanın toplam kapasitesini temsil eder. Küme Kaynak Yöneticisi, bu uygulama örneği içinde toplam yükün bu değeri aşmasına neden olacak yeni hizmetlerin oluşturulmasına izin verir. Örneğin, uygulama örneğinin 10 kapasiteye sahip olduğunu ve zaten beş yük olduğunu varsayalım. Toplam varsayılan yükü 10 olan bir hizmetin oluşturulmasına izin verilmez.
- **Maksimum Düğüm Kapasitesi** – Bu ayar, uygulama için tek bir düğümdeki maksimum toplam yükü belirtir. Yük bu kapasitenin üzerine çıkarsa, Küme Kaynak Yöneticisi yinelemeleri diğer düğümlere taşır, böylece yük azalır.


Powershell:

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

## <a name="reserving-capacity"></a>Kapasite Rezerve Etme
Uygulama grupları için başka bir yaygın kullanım küme içindeki kaynakların belirli bir uygulama örneği için ayrılmış olduğundan emin olmaktır. Uygulama örneği oluşturulduğunda alan her zaman ayrılmıştır.

Uygulama için kümede boşluk ayırma, aşağıdaki durumlarda bile hemen gerçekleşir:
- uygulama örneği oluşturulur, ancak henüz içinde herhangi bir hizmet yok
- uygulama örneği içindeki hizmet sayısı her seferinde değişir 
- hizmetler var, ancak kaynakları tüketmiyor 

Bir uygulama örneği için kaynak ayırma iki ek parametre belirtilmesi gerektirir: *Minimum Düğümler* ve *DüğümRezervasyon Kapasitesi*

- **Minimum Düğümler** - Uygulama örneğinin çalışması gereken en az düğüm sayısını tanımlar.  
- **NodeReservationCapacity** - Bu ayar uygulama için metrik başına. Değer, bu uygulamadaki hizmetlerin çalıştığı herhangi bir düğümdeki uygulama için ayrılmış olan metrik miktarıdır.

**MinimumDüğümler** ve **Düğümlerin BirleştirilmesiReservationCapacity** küme içindeki uygulama için minimum yük rezervasyonu garanti eder. Kümede gereken toplam rezervasyondan daha az kalan kapasite varsa, uygulamanın oluşturulması başarısız olur. 

Kapasite rezervasyonu örneğine bakalım:

<center>

![Ayrılmış Kapasiteyi Tanımlayan Uygulama Örnekleri][Image2]
</center>

Soldaki örnekte, uygulamaların tanımlı herhangi bir Uygulama Kapasitesi yoktur. Küme Kaynak Yöneticisi her şeyi normal kurallara göre dengeler.

Sağdaki örnekte, Application1'in aşağıdaki ayarlarla oluşturulduğunu varsayalım:

- MinimumDüğümler iki olarak ayarlanmış
- Bir uygulama Metrik ile tanımlanan
  - NodeReservationKapasitesi 20

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

Service Fabric, Application1 için iki düğüm üzerinde kapasite ayırır ve Application2'deki hizmetlerin, o anda Application1'deki hizmetler tarafından herhangi bir yük tüketilmese bile bu kapasiteyi tüketmesine izin vermez. Bu ayrılmış uygulama kapasitesi tüketilen olarak kabul edilir ve bu düğüm ve küme içinde kalan kapasite karşı sayar.  Rezervasyon kalan küme kapasitesinden hemen düşülür, ancak ayrılmış tüketim yalnızca en az bir hizmet nesnesi yerleştirildiğinde belirli bir düğümün kapasitesinden düşülür. Bu daha sonra rezervasyon, kaynaklar yalnızca gerektiğinde düğümler üzerinde rezerve edildiğinden esneklik ve daha iyi kaynak kullanımı sağlar.

## <a name="obtaining-the-application-load-information"></a>Uygulama yük bilgilerinin alınması
Bir veya daha fazla ölçüm için tanımlanmış bir Uygulama Kapasitesi olan her uygulama için, hizmetlerinin yinelemeleri tarafından bildirilen toplam yük hakkında bilgi edinebilirsiniz.

Powershell:

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

ApplicationLoad sorgusu, uygulama için belirtilen Uygulama Kapasitesi ile ilgili temel bilgileri döndürür. Bu bilgiler, Minimum Düğümler ve Maksimum Düğümler bilgilerini ve uygulamanın şu anda içinde bulunduğu sayıyı içerir. Ayrıca, her uygulama yük ölçümü hakkında bilgi içerir:

* Metrik Adı: Metrik adı.
* Rezervasyon Kapasitesi: Küme de bu Uygulama için ayrılmış küme kapasitesi.
* Uygulama Yükü: Bu Uygulamanın alt yinelemelerinin toplam yükü.
* Uygulama Kapasitesi: Uygulama Yükünün izin verilen maksimum değeri.

## <a name="removing-application-capacity"></a>Uygulama Kapasitesini Kaldırma
Uygulama Kapasitesi parametreleri bir uygulama için ayarlandıktan sonra, Uygulama API'lerini veya PowerShell cmdlets'i güncelleştir kullanılarak kaldırılabilir. Örnek:

``` posh
Update-ServiceFabricApplication –Name fabric:/MyApplication1 –RemoveApplicationCapacity

```

Bu komut, uygulama örneğindeki tüm Uygulama kapasitesi yönetimi parametrelerini kaldırır. Buna Minimum Düğümler, MaximumNode'lar ve varsa Uygulamanın ölçümleri dahildir. Komutun etkisi hemen olur. Bu komut tamamlandıktan sonra, Küme Kaynak Yöneticisi uygulamaları yönetmek için varsayılan davranışı kullanır. Uygulama Kapasitesi parametreleri tekrar `Update-ServiceFabricApplication` / `System.Fabric.FabricClient.ApplicationManagementClient.UpdateApplicationAsync()`.

### <a name="restrictions-on-application-capacity"></a>Uygulama Kapasitesindeki Kısıtlamalar
Uygulama Kapasitesi parametrelerinde uyulması gereken çeşitli kısıtlamalar vardır. Doğrulama hataları varsa hiçbir değişiklik gerçekleşmez.

- Tüm tüm sonda parametreleri negatif olmayan sayılar olmalıdır.
- MinimumDüğümler hiçbir zaman MaximumNode'lardan büyük olmamalıdır.
- Bir yük ölçümü için kapasiteler tanımlanırsa, aşağıdaki kurallara uymaları gerekir:
  - Düğüm Rezervasyon Kapasitesi Maksimum Düğüm Kapasitesinden büyük olmamalıdır. Örneğin, düğümdeki metrik "CPU" kapasitesini iki birimle sınırlayamaz sınız ve her düğümde üç birim ayırmayı deneyemezsiniz.
  - MaximumNodes belirtilirse, MaximumNodes ve Maksimum Düğüm Kapasitesi'nin ürünü Toplam Uygulama Kapasitesinden büyük olmamalıdır. Örneğin, yük metrik "CPU" için Maksimum Düğüm Kapasitesi sekiz olarak ayarlanmış olduğunu varsayalım. Maksimum Düğümleri 10 olarak ayarladığınızı da varsayalım. Bu durumda, bu yük ölçümü için Toplam Uygulama Kapasitesi 80'den büyük olmalıdır.

Kısıtlamalar hem uygulama oluşturma hem de güncelleştirmeler sırasında uygulanır.

## <a name="how-not-to-use-application-capacity"></a>Uygulama Kapasitesi Nasıl Kullanılmaz?
- Uygulamayı _belirli_ bir düğüm alt kümesiyle kısıtlamak için Uygulama Grubu özelliklerini kullanmayı denemayın. Başka bir deyişle, uygulamanın en fazla beş düğümde çalıştığını, ancak kümedeki belirli beş düğümün üzerinde çalışmadığını belirtebilirsiniz. Belirli düğümlere bir uygulama kısıtlama hizmetleri için yerleşim kısıtlamaları kullanılarak elde edilebilir.
- Aynı uygulamadan iki hizmetin aynı düğümlere yerleştirildiğinden emin olmak için Uygulama Kapasitesini kullanmayı niçin kullanmayın. Bunun yerine [yakınlık](service-fabric-cluster-resource-manager-advanced-placement-rules-affinity.md) veya [yerleşim kısıtlamaları](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)kullanın.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmetleri yapılandırma hakkında daha fazla bilgi [için, Hizmetleri yapılandırma hakkında bilgi edinin](service-fabric-cluster-resource-manager-configure-services.md)
- Küme Kaynak Yöneticisi'nin kümedeki yükü nasıl yönettiği ve dengeler yaptığı hakkında bilgi edinmek [için, yükü dengeleme](service-fabric-cluster-resource-manager-balancing.md) makalesine göz atın
- Baştan başlayın ve [Hizmet Kumaş Küme Kaynak Yöneticisi'ne Giriş alın](service-fabric-cluster-resource-manager-introduction.md)
- Ölçümlerin genel olarak nasıl çalıştığı hakkında daha fazla bilgi için [Servis Kumaş Yük Ölçümleri'ni](service-fabric-cluster-resource-manager-metrics.md) okuyun
- Küme Kaynak Yöneticisi kümeyi açıklamak için birçok seçenek vardır. Onlar hakkında daha fazla bilgi edinmek için, [Bir Hizmet Kumaş kümesi açıklayan](service-fabric-cluster-resource-manager-cluster-description.md) bu makaleye göz atın

[Image1]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-max-nodes.png
[Image2]:./media/service-fabric-cluster-resource-manager-application-groups/application-groups-reserved-capacity.png
