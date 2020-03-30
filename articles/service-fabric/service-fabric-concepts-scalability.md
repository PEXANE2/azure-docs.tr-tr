---
title: Hizmet Kumaş ı hizmetlerinin ölçeklenebilirliği
description: Azure Hizmet Kumaşı'nda ölçekleme ve uygulamaları ölçeklendirmek için kullanılan çeşitli teknikler hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/26/2019
ms.author: masnider
ms.openlocfilehash: 17827342b67d37d9fbeb56654824e004367823ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282568"
---
# <a name="scaling-in-service-fabric"></a>Hizmet Kumaşında Ölçekleme
Azure Hizmet Kumaşı, bir kümenin düğümlerinde hizmetleri, bölümleri ve yinelemeleri yöneterek ölçeklenebilir uygulamalar oluşturmayı kolaylaştırır. Aynı donanımda birçok iş yükü çalıştırmak maksimum kaynak kullanımını sağlar, ancak iş yüklerinizi nasıl ölçeklendirmeyi seçtiğiniz açısından da esneklik sağlar. Bu Kanal 9 videosu ölçeklenebilir mikrohizmetler uygulamalarını nasıl oluşturabileceğinizi açıklar:

> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T116/player]

Hizmet KumaşÖlçekleme birkaç farklı şekilde gerçekleştirilir:

1. Durum suz hizmet örnekleri oluşturarak veya kaldırarak ölçekleme
2. Yeni adlandırılmış hizmetler oluşturarak veya kaldırarak ölçekleme
3. Yeni adlandırılmış uygulama örnekleri oluşturarak veya kaldırarak ölçekleme
4. Bölümlenmiş hizmetleri kullanarak ölçekleme
5. Kümeden düğümler ekleyerek ve kaldırarak ölçekleme 
6. Küme Kaynak Yöneticisi ölçümlerini kullanarak ölçekleme

## <a name="scaling-by-creating-or-removing-stateless-service-instances"></a>Durum suz hizmet örnekleri oluşturarak veya kaldırarak ölçekleme
Service Fabric içinde ölçeklendirmenin en basit yollarından biri, devletsiz hizmetlerle çalışır. Bir devletsiz hizmet oluşturduğunuzda, bir `InstanceCount`. `InstanceCount`hizmet başlatıldığında, bu hizmetin kodunun kaç çalışan kopyasının oluşturulduğunu tanımlar. Örneğin, kümede 100 düğüm olduğunu varsayalım. Ayrıca bir hizmetin `InstanceCount` 10'un üzerinde bir hizmetle oluşturulduğunu da söyleyebilir. Çalışma süresi boyunca, kodun çalışan 10 kopyası çok meşgul olabilir (veya yeterince meşgul olmayabilir). Bu iş yükünü ölçeklendirmenin bir yolu, örnek sayısını değiştirmektir. Örneğin, bazı izleme veya yönetim kodu, iş yükünün yüke bağlı olarak ölçeklenmesi gerekip gerekmediğine bağlı olarak varolan örnek sayısını 50 veya 5 olarak değiştirebilir. 

C#:

```csharp
StatelessServiceUpdateDescription updateDescription = new StatelessServiceUpdateDescription(); 
updateDescription.InstanceCount = 50;
await fabricClient.ServiceManager.UpdateServiceAsync(new Uri("fabric:/app/service"), updateDescription);
```

Powershell:

```posh
Update-ServiceFabricService -Stateless -ServiceName $serviceName -InstanceCount 50
```
### <a name="using-dynamic-instance-count"></a>Dinamik Örnek Sayısını Kullanma
Özellikle devletsiz hizmetler için Service Fabric, örnek sayısını değiştirmenin otomatik bir yolunu sunar. Bu, hizmetin kullanılabilir düğüm sayısıyla dinamik olarak ölçeklendirmesine olanak tanır. Bu davranışı seçmenin yolu örnek sayısını = -1 olarak ayarlamaktır. InstanceCount = -1, Service Fabric'e "Bu devletsiz hizmeti her düğümde çalıştırın" diyen bir yönergedir. Düğüm sayısı değişirse, Hizmet Kumaşı örnek sayısını otomatik olarak eşleşecek şekilde değiştirerek hizmetin tüm geçerli düğümlerde çalışmasını sağlar. 

C#:

```csharp
StatelessServiceDescription serviceDescription = new StatelessServiceDescription();
//Set other service properties necessary for creation....
serviceDescription.InstanceCount = -1;
await fc.ServiceManager.CreateServiceAsync(serviceDescription);
```

Powershell:

```posh
New-ServiceFabricService -ApplicationName $applicationName -ServiceName $serviceName -ServiceTypeName $serviceTypeName -Stateless -PartitionSchemeSingleton -InstanceCount "-1"
```

## <a name="scaling-by-creating-or-removing-new-named-services"></a>Yeni adlandırılmış hizmetler oluşturarak veya kaldırarak ölçekleme
Adlandırılmış hizmet örneği, kümedeki bazı adlandırılmış uygulama örneği içinde bir hizmet türünün belirli bir örneğidir [(bkz. Hizmet Kumaşı uygulama yaşam döngüsü).](service-fabric-application-lifecycle.md) 

Hizmetler az çok meşgul hale geldikçe yeni adlandırılmış hizmet örnekleri oluşturulabilir (veya kaldırılabilir). Bu, isteklerin daha fazla hizmet örneğine yayılmasını sağlayarak genellikle varolan hizmetlerdeki yükün azaltılmasına olanak tanır. Hizmet oluştururken, Hizmet Kumaş Kümesi Kaynak Yöneticisi hizmetleri kümedeki dağıtılmış bir şekilde yerleştirir. Kesin kararlar kümedeki [ölçümlere](service-fabric-cluster-resource-manager-metrics.md) ve diğer yerleşim kurallarına tabidir. Hizmetler birkaç farklı şekilde oluşturulabilir, ancak en yaygın ları [`New-ServiceFabricService`](https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice?view=azureservicefabricps)ya arayan biri [`CreateServiceAsync`](https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync?view=azure-dotnet)gibi yönetim eylemleri yoluyla ya da kod arama yoluyla. `CreateServiceAsync`kümede çalışan diğer hizmetler içinden de çağrılabilir.

Hizmetleri dinamik olarak oluşturmak her türlü senaryoda kullanılabilir ve yaygın bir desendir. Örneğin, belirli bir iş akışını temsil eden durumlu bir hizmet düşünün. İşi temsil eden çağrılar bu hizmete gelecek ve bu hizmet bu iş akışına giden adımları yürütecek ve ilerleme kaydedecek. 

Bu özel hizmet ölçeğini nasıl yaparsınız? Hizmet, bir şekilde çok kiracılı olabilir ve aramaları kabul edebilir ve aynı iş akışının birçok farklı örneği için aynı anda adımları başlatabilir. Ancak, şimdi aynı iş akışının birçok farklı örnekleri hakkında endişelenmenize gerek olduğundan, kodu daha karmaşık hale getirebilir, tüm farklı aşamalarında ve farklı müşterilerden. Ayrıca, birden çok iş akışını aynı anda işlemek ölçek sorununu çözmez. Bunun nedeni, bir noktada bu hizmetin belirli bir makineye sığmayacak kadar çok kaynak tüketir olmasıdır. İlk etapta bu desen için inşa edilmedi birçok hizmet de kendi kodbazı doğal darboğaz veya yavaşlama nedeniyle zorluk la karşılaşır. Bu tür sorunlar, izlediği eşzamanlı iş akışları sayısı büyüyünce hizmetin de çalışmamasını neden olur.  

Çözüm, izlemek istediğiniz iş akışının her farklı örneği için bu hizmetin bir örneğini oluşturmaktır. Bu büyük bir desen ve hizmet stateless veya stateful olup olmadığını çalışır. Bu kalıbın çalışması için genellikle "İş Yükü Yöneticisi Hizmeti" olarak davranan başka bir hizmet vardır. Bu hizmetin görevi istek almak ve bu istekleri diğer hizmetlere yönlendirmektir. Yönetici, iletiyi aldığında iş yükü hizmetinin bir örneğini dinamik olarak oluşturabilir ve ardından istekleri bu hizmetlere aktarabilir. Yönetici hizmeti, belirli bir iş akışı hizmeti işini tamamladığında geri çağrı alabilir. Yönetici bu geri aramaları aldığında, iş akışı hizmetinin bu örneğini silebilir veya daha fazla arama bekleniyorsa bırakabilir. 

Bu tür yöneticinin gelişmiş sürümleri, yönettiği hizmetlerin havuzlarını bile oluşturabilir. Havuz, yeni bir istek geldiğinde hizmetin dönmesini beklememelerini sağlamaya yardımcı olur. Bunun yerine, yönetici havuzdan şu anda meşgul olmayan bir iş akışı hizmeti seçebilir veya rasgele rota. Hizmet havuzunu kullanılabilir tutmak, isteğin yeni bir hizmetin döndürülmesini beklemeolasılığı daha düşük olduğundan, yeni istekleri işlemeyi hızlandırır. Yeni hizmetler oluşturmak hızlıdır, ancak ücretsiz veya anlık değildir. Havuz, isteğin hizmete girmeden önce beklemesi gereken süreyi en aza indirmeye yardımcı olur. Yanıt süreleri en önemli olduğunda bu yöneticiyi ve havuz deseni sık sık görürsünüz. İsteği sıralamak ve hizmeti arka planda oluşturmak ve _sonra_ aktarmak da popüler bir yönetici desenidir, çünkü hizmetin şu anda bekleyen iş miktarının bazı izlemelerine dayalı olarak hizmetler oluşturmak ve siler. 

## <a name="scaling-by-creating-or-removing-new-named-application-instances"></a>Yeni adlandırılmış uygulama örnekleri oluşturarak veya kaldırarak ölçekleme
Tüm uygulama örnekleri oluşturma ve silme, hizmet oluşturma ve silme desenine benzer. Bu desen için, küme içindeki diğer hizmetlerden aldığı isteklere ve bilgilere dayanarak karar veren bazı yönetici hizmeti vardır. 

Varolan bazı uygulamalarda yeni adlandırılmış hizmet örnekleri oluşturmak yerine yeni bir adlandırılmış uygulama örneği oluşturma ne zaman kullanılmalıdır? Birkaç dava var:

  * Yeni uygulama örneği, kodu belirli bir kimlik veya güvenlik ayarları altında çalışması gereken bir müşteri içindir.
    * Service Fabric, belirli kimlikler altında çalışacak farklı kod paketlerinin tanımlanmasına olanak tanır. Aynı kod paketini farklı kimlikler altında başlatmak için etkinleştirmelerin farklı uygulama örneklerinde gerçekleşmesi gerekir. Varolan bir müşterinin iş yüklerinin dağıtıldığı bir servis talebi düşünün. Bunlar, uzak veritabanları veya diğer sistemler gibi diğer kaynaklara erişimlerini izleyebilmeniz ve denetleyebilmeniz için belirli bir kimlik altında çalışıyor olabilir. Bu durumda, yeni bir müşteri kaydolduğunda, büyük olasılıkla kodlarını aynı bağlamda (işlem alanı) etkinleştirmek istemezsiniz. Bunu yaparken, bu durum hizmet kodunuzu belirli bir kimlik bağlamında hareket etmesini zorlaştırır. Genellikle daha fazla güvenlik, yalıtım ve kimlik yönetimi kodunuz olmalıdır. Aynı uygulama örneği içinde ve dolayısıyla aynı işlem alanı içinde farklı adlandırılmış hizmet örnekleri kullanmak yerine, farklı adlandırılmış Hizmet Kumaş Uygulaması örneklerini kullanabilirsiniz. Bu, farklı kimlik bağlamlarının tanımlanmasını kolaylaştırır.
  * Yeni uygulama örneği de bir yapılandırma aracı olarak hizmet vermektedir
    * Varsayılan olarak, bir uygulama örneği içinde belirli bir hizmet türü adlandırılmış hizmet örneklerinin tümü belirli bir düğüm üzerinde aynı işlemde çalışacaktır. Bunun anlamı, her hizmet örneğini farklı şekilde yapılandırabilmenize veya bunu yapmanın karmaşık olduğudur. Hizmetler, yapılandırma paketi içinde config'lerini aramak için kullandıkları bazı belirteçlere sahip olmalıdır. Genellikle bu sadece servisin adıdır. Bu iyi çalışır, ancak bu uygulama örneği içinde bireysel adlı hizmet örneklerinin adlarını yapılandırma çiftler. Yapılandırma normalde uygulama örneğine özgü değerlere sahip bir tasarım zamanı yapı olduğundan, bu kafa karıştırıcı ve yönetilmesi zor olabilir. Daha fazla hizmet oluşturmak, config paketlerindeki bilgileri değiştirmek veya yeni hizmetlerin kendi özel bilgilerini arayabilmeleri için yenilerini dağıtmak için her zaman daha fazla uygulama yükseltmesi anlamına gelir. Yepyeni bir uygulama örneği oluşturmak genellikle daha kolaydır. Ardından, hizmetler için gerekli olan yapılandırmayı ayarlamak için uygulama parametrelerini kullanabilirsiniz. Bu şekilde, adlandırılmış uygulama örneğinde oluşturulan tüm hizmetler belirli yapılandırma ayarlarını devralabilir. Örneğin, sırlar veya müşteri başına kaynak sınırları gibi her müşteri için ayarlar ve özelleştirmeler içeren tek bir yapılandırma dosyasına sahip olmak yerine, bu ayarlara sahip her müşteri için farklı bir uygulama örneğine sahip olursunuz Geçersiz kılın -mış. 
  * Yeni uygulama bir yükseltme sınırı olarak hizmet vermektedir
    * Service Fabric içinde, farklı adlandırılmış uygulama örnekleri yükseltme için sınır olarak hizmet vermektedir. Adlandırılmış bir uygulama örneğinin yükseltimi, başka bir adlandırılmış uygulama örneğinin çalıştırdığı kodu etkilemez. Farklı uygulamalar aynı düğümlerde aynı kodun farklı sürümlerini çalıştırarak sona erer. Yeni kodun başka bir hizmetle aynı yükseltmeleri izleyip izlememesi gerektiğini seçebileceğinizden, ölçekleme kararı almanız gerektiğinde bu bir etken olabilir. Örneğin, hizmetleri dinamik olarak oluşturarak ve silerek belirli bir müşterinin iş yüklerini ölçeklemekten sorumlu yönetici hizmetine bir çağrı geldiğini varsay. Ancak bu durumda, arama _yeni_ bir müşteriyle ilişkili bir iş yükü içindir. Çoğu müşteri yalnızca daha önce listelenen güvenlik ve yapılandırma nedenleriyle değil, yazılımın belirli sürümlerini çalıştırma ve ne zaman yükseltildiklerini seçme açısından daha fazla esneklik sağladığı için birbirlerinden izole edilmeyi sever. Ayrıca yeni bir uygulama örneği oluşturabilir ve herhangi bir yükseltmenin dokunacağı hizmetlerinizin miktarını daha fazla bölmek için hizmeti orada oluşturabilirsiniz. Ayrı uygulama örnekleri, uygulama yükseltmeleri yaparken daha fazla parçalılık sağlar ve Ayrıca A/B sınamasını ve Mavi/Yeşil dağıtımlarını etkinleştirin. 
  * Varolan uygulama örneği tam
    * Hizmet Kumaşı'nda [uygulama kapasitesi,](service-fabric-cluster-resource-manager-application-groups.md) belirli uygulama örnekleri için kullanılabilir kaynak miktarını denetlemek için kullanabileceğiniz bir kavramdır. Örneğin, belirli bir hizmetin ölçeklendirmek için başka bir örneğin oluşturulması gerektiğine karar verebilirsiniz. Ancak, bu uygulama örneği belirli bir metrik için kapasite dışındadır. Bu özel müşteriye veya iş yüküne hala daha fazla kaynak verilmesi gerekiyorsa, bu uygulama için varolan kapasiteyi artırabilir veya yeni bir uygulama oluşturabilirsiniz. 

## <a name="scaling-at-the-partition-level"></a>Bölüm düzeyinde ölçekleme
Service Fabric bölümleme desteklemektedir. Bölümleme, bir hizmeti her biri bağımsız olarak çalışan birkaç mantıksal ve fiziksel bölüme böler. Bu, hiç bir yineleme kümesinin tüm aramaları işlemek ve tüm durumu aynı anda işlemek zorunda olmadığından, durum hizmetleri için yararlıdır. [Bölümleme genel bakış](service-fabric-concepts-partitioning.md) desteklenen bölümleme düzenleri türleri hakkında bilgi sağlar. Her bölümün yinelemeleri bir kümedeki düğümlere yayılır, bu hizmetin yükünü dağıtır ve bir bütün olarak hizmetin veya herhangi bir bölümün tek bir hata noktası olmamasını sağlar. 

0'ın düşük anahtarı, 99'luk yüksek anahtarı ve 4 bölüm sayısıyla aralıklı bölümleme düzeni kullanan bir hizmet düşünün. Üç düğümlü bir kümede, hizmet, burada gösterildiği gibi her düğümdeki kaynakları paylaşan dört yinelemeyle birlikte dizilebilir:

<center>

![Üç düğümlü bölüm düzeni](./media/service-fabric-concepts-scalability/layout-three-nodes.png)
</center>

Düğüm sayısını artırırsanız, Hizmet Kumaşı varolan yinelemelerin bazılarını oraya taşır. Örneğin, düğüm sayısının dörde çıkarıldığını ve yinelemelerin yeniden dağıtıldığını varsayalım. Şimdi hizmet şimdi her düğüm üzerinde çalışan üç yineleme, her biri farklı bölümlere ait vardır. Bu, yeni düğüm soğuk olmadığından daha iyi kaynak kullanımına olanak tanır. Genellikle, her hizmetin kullanabileceği daha fazla kaynağa sahip olduğundan performansı da artırır.

<center>

![Dört düğümlü bölüm düzeni](./media/service-fabric-concepts-scalability/layout-four-nodes.png)
</center>

## <a name="scaling-by-using-the-service-fabric-cluster-resource-manager-and-metrics"></a>Service Fabric Cluster Resource Manager ve ölçümleri kullanarak ölçekleme
[Ölçümler,](service-fabric-cluster-resource-manager-metrics.md) hizmetlerin kaynak tüketimini Service Fabric'e nasıl ifade ettikleridir. Ölçümleri kullanmak, Küme Kaynak Yöneticisi'ne kümenin düzenini yeniden düzenleme ve en iyi duruma getirme fırsatı verir. Örneğin, kümede çok sayıda kaynak olabilir, ancak bunlar şu anda çalışmakta olan hizmetlere tahsis edilmemiş olabilir. Ölçümlerin kullanılması, hizmetlerin kullanılabilir kaynaklara erişmesini sağlamak için kümeyi yeniden düzenlemesine olanak tanır. 


## <a name="scaling-by-adding-and-removing-nodes-from-the-cluster"></a>Kümeden düğümler ekleyerek ve kaldırarak ölçekleme 
Service Fabric ile ölçekleme için başka bir seçenek kümenin boyutunu değiştirmektir. Kümenin boyutunu değiştirmek, kümedeki düğüm türlerinden biri veya daha fazlası için düğüm eklemek veya kaldırmak anlamına gelir. Örneğin, kümedeki tüm düğümlerin sıcak olduğu bir durum düşünün. Bu, kümenin kaynaklarının hemen hemen tamamının tüketilmeanlamına gelir. Bu durumda, kümeye daha fazla düğüm eklemek ölçeklendirmenin en iyi yoludur. Yeni düğümler kümeye katıldıktan sonra Hizmet Kumaş Küme Kaynak Yöneticisi hizmetleri onlara taşır ve varolan düğümler üzerinde daha az toplam yük sağlar. Örnek sayısı = -1 olan devletsiz hizmetler için otomatik olarak daha fazla hizmet örneği oluşturulur. Bu, bazı çağrıların varolan düğümlerden yeni düğümlere taşınmasına olanak tanır. 

Daha fazla bilgi için [küme ölçekleme](service-fabric-cluster-scaling.md)bakın.

## <a name="choosing-a-platform"></a>Platform seçme

İşletim sistemleri arasındaki uygulama farklılıkları nedeniyle, Service Fabric'i Windows veya Linux ile kullanmayı seçmek, uygulamanızı ölçeklemenin önemli bir parçası olabilir. Olası engellerden biri, aşamalı günlüğe kaydetmenin nasıl gerçekleştirildiğidir. Windows'daki Service Fabric, tek başına bir makine günlüğü için bir çekirdek sürücüsü kullanır ve bu durum durumlu hizmet yinelemeleri arasında paylaşılır. Bu günlük yaklaşık 8 GB ağırlığındadır. Linux, diğer taraftan, her yineleme için 256 MB evreleme günlüğü kullanır, belirli bir düğüm üzerinde çalışan hafif hizmet çoğaltmaları sayısını en üst düzeye çıkarmak isteyen uygulamalar için daha az ideal hale. Geçici depolama gereksinimlerindeki bu farklılıklar, Service Fabric küme dağıtımı için istenen platformu bilgilendirebilir.

## <a name="putting-it-all-together"></a>Hepsini bir araya getirme
Burada tartıştığımız tüm fikirleri ele alalım ve bir örnek üzerinden konuşalım. Aşağıdaki hizmeti göz önünde bulundurun: Adlara ve iletişim bilgilerine tutunarak adres defteri gibi davranan bir hizmet oluşturmaya çalışıyorsunuz. 

Ön tarafta ölçekle ilgili bir sürü sorunuz var: Kaç kullanıcınız olacak? Her kullanıcı kaç kişi saklayacak? Eğer ilk kez hizmet ayakta zaman tüm bu anlamaya çalışıyorum zordur. Belirli bir bölüm sayısıile tek bir statik hizmet ile gitmek için gittiğini varsayalım. Yanlış bölüm sayısını seçmenin sonuçları daha sonra ölçek sorunlarına neden olabilir. Benzer şekilde, doğru sayıyı seçseniz bile ihtiyacınız olan tüm bilgilere sahip olmayabilirsiniz. Örneğin, hem düğüm sayısı hem de boyutları açısından, kümenin boyutunu önden belirlemeniz gerekir. Bir hizmetin ömrü boyunca kaç kaynak tüketeceğini tahmin etmek genellikle zordur. Ayrıca, hizmetin gerçekte gördüğü trafik modelini önceden bilmek de zor olabilir. Örneğin, belki insanlar kişilerini yalnızca sabah ilk iş olarak ekleyip kaldırırlar veya belki de gün boyunca eşit olarak dağıtılır. Buna dayanarak ölçeklendirmeniz gerekebilir ve dinamik olarak. Belki ne zaman ölçeklendirmeniz ve içinde yapmanız gerektiğini tahmin etmeyi öğrenebilirsiniz, ancak her iki şekilde de muhtemelen hizmetinizin kaynak tüketimini değiştirmesine tepki vermeniz gerekecektir. Bu, varolan kaynakların kullanımını yeniden düzenleme yeterli olmadığında daha fazla kaynak sağlamak için kümenin boyutunu değiştirmeyi içerebilir. 

Ama neden bile tüm kullanıcılar için tek bir bölüm düzeni dışarı almaya çalışın? Neden kendinizi bir hizmet ve bir statik kümeyle sınırlandırın? Gerçek durum genellikle daha dinamiktir. 

Ölçek için oluştururken, aşağıdaki dinamik deseni göz önünde bulundurun. Durumunuza uyarlamanız gerekebilir:

1. Öndeki herkes için bir bölümleme şeması seçmeye çalışmak yerine, bir "yönetici hizmeti" oluşturun.
2. Yönetici hizmetinin görevi, hizmetinize kaydolurken müşteri bilgilerine bakmaktır. Daha sonra bu bilgilere bağlı olarak yönetici _hizmeti, yalnızca o müşteri için_ _gerçek_ kişi depolama hizmetinizin bir örneğini oluşturur. Belirli yapılandırma, yalıtım veya yükseltme gerektiriyorsa, bu müşteri için bir Uygulama örneğini döndürmeye de karar verebilirsiniz. 

Bu dinamik oluşturma deseni birçok yararları:

  - Öndeki tüm kullanıcılar için doğru bölüm sayısını tahmin etmeye veya kendi başına sonsuz ölçeklenebilir tek bir hizmet oluşturmaya çalışmıyorsunuz. 
  - Farklı kullanıcıların aynı bölüm sayısına, yineleme sayısına, yerleşim kısıtlamalarına, ölçümlere, varsayılan yüklere, hizmet adlarına, dns ayarlarına veya hizmet veya uygulama düzeyinde belirtilen diğer özelliklere sahip olması gerekmez. 
  - Ek veri segmentasyonu kazanırsınız. Her müşterinin hizmetin kendi kopyası vardır
    - Her müşteri hizmeti farklı şekilde yapılandırılabilir ve beklenen ölçeklere göre gerektiğinde daha fazla veya daha az bölüm veya yinelemeyle daha fazla veya daha az kaynak verilebilir.
      - Örneğin, müşterinin "Gold" katmanı için ödeme aldığını varsayılabilir - daha fazla yineleme veya daha fazla bölüm sayısı ve ölçümler ve uygulama kapasiteleri aracılığıyla hizmetlerine ayrılmış potansiyel kaynaklar alabilir.
      - Ya da ihtiyaç duydukları kişi sayısını gösteren bilgi sağladığını söylüyorlar "Küçük" oldu - onlar sadece birkaç bölüm alacak, hatta diğer müşterilerle paylaşılan bir hizmet havuzuna konabilir.
  - Müşterilerin gelmesini beklerken bir sürü hizmet örneği veya yineleme çalıştırmatmamanız
  - Bir müşteri ayrılırsa, bilgilerini hizmetinizden kaldırmak, yöneticinin oluşturduğu hizmeti veya uygulamayı silmesi kadar kolaydır.

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Hizmet Kumaş ı hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
* [Bölümleme Servisi Kumaş hizmetleri](service-fabric-concepts-partitioning.md)
