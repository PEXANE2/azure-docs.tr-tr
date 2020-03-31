---
title: Hizmet Kumaş Güvenilir Aktörler Genel Bakış
description: Hizmet Kumaş Güvenilir Aktörler programlama modeli, Sanal Aktör desen dayalı Giriş.
author: vturecek
ms.topic: conceptual
ms.date: 11/01/2017
ms.author: vturecek
ms.openlocfilehash: 6aafa2a3372c431f8afa7fad41051c26c3fe5fcd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645574"
---
# <a name="introduction-to-service-fabric-reliable-actors"></a>Hizmet kumaş güvenilir aktörler giriş
Güvenilir Aktörler [Sanal Aktör](https://research.microsoft.com/en-us/projects/orleans/) desen dayalı bir Hizmet Kumaş uygulama çerçevesidir. Güvenilir Aktörler API, Service Fabric tarafından sağlanan ölçeklenebilirlik ve güvenilirlik garantileri üzerine oluşturulmuş tek dişli bir programlama modeli sağlar.

## <a name="what-are-actors"></a>Aktörler nedir?
Aktör, tek iş parçacığı yürütme ile işlem ve devlet izole, bağımsız bir birimdir. [Aktör deseni,](https://en.wikipedia.org/wiki/Actor_model) eşzamanlı veya dağıtılmış sistemler için, bu aktörlerin çok sayıda aynı anda ve birbirinden bağımsız olarak yürütülebilir bir hesaplama modelidir. Aktörler birbirleriyle iletişim kurabilir ve daha fazla aktör yaratabilirler.

### <a name="when-to-use-reliable-actors"></a>Güvenilir Aktörler ne zaman kullanılır
Hizmet Kumaş Güvenilir Aktörler aktör tasarım desen bir uygulamadır. Herhangi bir yazılım tasarım deseninde olduğu gibi, belirli bir deseni kullanıp kullanmama kararı, bir yazılım tasarım sorununun desene uygun olup olmadığına bağlı olarak verilir.

Aktör tasarım deseni bir dizi dağıtılmış sistem sorunları ve senaryoları için iyi bir uyum olabilir, desen kısıtlamaları ve bunu uygulayan çerçeve dikkatli dikkate yapılmalıdır. Genel kılavuz olarak, aşağıdakiler varsa, sorununuzu veya senaryonuzu modellemek için aktör deseni düşünün:

* Sorun alanınız çok sayıda (binlerce veya daha fazla) küçük, bağımsız ve yalıtılmış durum ve mantık birimlerini içerir.
* Bir dizi aktör arasında durum sorgulama dahil olmak üzere dış bileşenlerden önemli etkileşim gerektirmeyen tek iş parçacığı nesneleri ile çalışmak istiyorsunuz.
* Aktör örnekleriniz, G/Ç işlemleri düzenleyerek öngörülemeyen gecikmelerle arayanları engellemez.

## <a name="actors-in-service-fabric"></a>Hizmet KumaşAktörler
Hizmet Kumaş, aktörler Güvenilir Aktörler çerçevesinde uygulanmaktadır: [Hizmet Kumaş Güvenilir Hizmetler](service-fabric-reliable-services-introduction.md)üstüne inşa edilmiş bir aktör desen tabanlı uygulama çerçevesi. Yazdığınız her Güvenilir Aktör hizmeti aslında bölümlenmiş, devlete ait güvenilir bir hizmettir.

Her aktör, .NET nesnesinin .NET türünün bir örneği olmasıyla aynı olan bir aktör türü örneği olarak tanımlanır. Örneğin, bir hesap makinesinin işlevselliğini uygulayan bir aktör türü olabilir ve bu türde küme deki çeşitli düğümlere dağıtılan birçok aktör olabilir. Bu tür aktörlerin her biri benzersiz bir aktör kimliği ile tanımlanır.

## <a name="actor-lifetime"></a>Aktör Ömür Boyu
Service Fabric aktörleri sanaldır, yani ömürleri hafıza içi gösterimlerine bağlı değildir. Sonuç olarak, açıkça oluşturulmalarına veya yok edilmelerine gerek yoktur. Güvenilir Aktörler çalışma süresi, bir aktör kimliği için ilk isteği aldığında bir aktörü otomatik olarak etkinleştirir. Bir aktör bir süre için kullanılmazsa, Güvenilir Aktörler çalışma zamanı çöp-bellek nesnesini toplar. Ayrıca daha sonra yeniden etkinleştirilmesi gerekirse aktörün varlığı hakkında bilgi koruyacaktır. Daha fazla bilgi için [Aktör yaşam döngüsü ve çöp toplama](service-fabric-reliable-actors-lifecycle.md)ya da çöp toplama bilgisine bakın.

Bu sanal aktör yaşam boyu soyutlama sanal aktör modelinin bir sonucu olarak bazı uyarılar taşır, ve aslında Güvenilir Aktörler uygulama bu modelden zaman zaman sapma.

* Bir aktör, aktör kimliğine ilk kez bir ileti gönderildiğinde otomatik olarak etkinleştirilir (bir aktör nesnesinin oluşturulmasına neden olur). Bir süre sonra, aktör nesne çöp toplanır. Gelecekte, aktör kimliğini yeniden kullanmak, yeni bir aktör nesnesinin inşa edilmesine neden olur. Bir aktörün durumu, devlet müdüründe depolandığında nesnenin ömründen daha uzun yaşar.
* Herhangi bir aktör kimliği için herhangi bir aktör yöntemini çağırmak, o aktörü etkinleştirir. Bu nedenle, aktör türleri kendi oluşturucuları çalışma süresine göre dolaylı olarak çağrılır. Bu nedenle, parametreler hizmetin kendisi tarafından aktörün oluşturucuya geçirilse de, istemci kodu parametreleri aktör türü oluşturucuya geçiremez. Sonuç olarak, aktör istemciden başlatma parametreleri gerektiriyorsa, aktörler diğer yöntemler çağrıldığında kısmen başharflerhaline getirilebilir. İstemciden bir aktörün aktivasyonu için tek bir giriş noktası yoktur.
* Her ne kadar Güvenilir Aktörler dolaylı olarak aktör nesneleri oluşturmak; bir aktörü ve durumunu açıkça silme yeteneğine sahipsiniz.

## <a name="distribution-and-failover"></a>Dağıtım ve arıza
Ölçeklenebilirlik ve güvenilirlik sağlamak için Service Fabric aktörleri kümeye dağıtır ve başarısız düğümlerden gerektiğinde sağlıklı düğümlere otomatik olarak geçirir. Bu, [bölümlenmiş, durumlu Güvenilir Hizmet](service-fabric-concepts-partitioning.md)üzerinde bir soyutlamadır. Dağıtım, ölçeklenebilirlik, güvenilirlik ve otomatik failover tüm aktörler *aktör Hizmeti*olarak adlandırılan bir devlet Güvenilir Hizmet içinde çalışan gerçeği sayesinde sağlanmaktadır.

Aktörler Aktör Hizmeti'nin bölümlerine dağıtılır ve bu bölümler Bir Hizmet Kumaşı kümesindeki düğümler arasında dağıtılır. Her hizmet bölümü bir dizi aktör içerir. Service Fabric, hizmet bölümlerinin dağıtımını ve başarısızlığı yönetir.

Örneğin, varsayılan aktör bölüm yerleşimini kullanarak üç düğüme dağıtılan dokuz bölümü olan bir aktör hizmeti böylece dağıtılır:

![Güvenilir Aktörler dağılımı][2]

Aktör Çerçevesi sizin için bölüm düzeni ve anahtar aralığı ayarlarını yönetir. Bu, bazı seçenekleri basitleştirir, ancak aynı zamanda bazı dikkate taşır:

* Güvenilir Hizmetler bir bölümleme düzeni, anahtar aralığı (bir aralık bölümleme düzeni kullanırken) ve bölüm sayısı seçmenize olanak sağlar. Güvenilir Aktörler aralık bölümleme düzeni (üniforma Int64 düzeni) ile sınırlıdır ve tam Int64 anahtar aralığını kullanmanız gerekir.
* Varsayılan olarak, aktörler rasgele tek tip dağıtım ile sonuçlanan bölümlere yerleştirilir.
* Aktörler rasgele yerleştirildiği için, aktör işlemlerinin her zaman yöntem çağrı verilerinin serileştirilmesi ve deserialization, gecikme ve ek yük tahakkuk dahil olmak üzere ağ iletişimi gerektirecek olması beklenmelidir.
* Gelişmiş senaryolarda, belirli bölümlere eşgösteren Int64 aktör işllerini kullanarak oyuncu bölünme sınırlaşTırımını denetlemek mümkündür. Ancak, bunu yapmak aktörlerin bölümler arasında dengesiz bir şekilde dağıtılmasına neden olabilir.

Aktör hizmetlerinin nasıl bölümlendirildiklerine ilişkin daha fazla bilgi için [aktörler için bölümleme kavramlarına](service-fabric-reliable-actors-platform.md#service-fabric-partition-concepts-for-actors)bakın.

## <a name="actor-communication"></a>Aktör iletişimi
Aktör etkileşimleri, arabirimi uygulayan aktör tarafından paylaşılan bir arabirimde ve aynı arabirim aracılığıyla bir aktöre proxy alan istemcide tanımlanır. Bu arabirim aktör yöntemlerini eşit olarak çağırmak için kullanıldığından, arabirimdeki her yöntem Görev döndürme olmalıdır.

Yöntem çağrıları ve yanıtları sonuçta küme genelinde ağ isteklerine neden olur, bu nedenle bağımsız değişkenler ve döndürdikleri görevlerin sonuç türleri platform tarafından serileştirilebilir olmalıdır. Özellikle, [onlar veri sözleşmesi serializable](service-fabric-reliable-actors-notes-on-actor-type-serialization.md)olmalıdır.

### <a name="the-actor-proxy"></a>Aktör vekil
Güvenilir Aktörler istemci API bir aktör örnek ve bir aktör istemci sayılsın. Bir aktörle iletişim kurmak için, istemci aktör arabirimini uygulayan bir aktör proxy nesnesi oluşturur. İstemci, proxy nesnesi üzerinde yöntemleri çağırarak aktörle etkileşime girerek. Aktör proxy istemci-aktör ve aktör-aktör iletişim için kullanılabilir.

```csharp
// Create a randomly distributed actor ID
ActorId actorId = ActorId.CreateRandom();

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
IMyActor myActor = ActorProxy.Create<IMyActor>(actorId, new Uri("fabric:/MyApp/MyActorService"));

// This will invoke a method on the actor. If an actor with the given ID does not exist, it will be activated by this method call.
await myActor.DoWorkAsync();
```

```java
// Create actor ID with some name
ActorId actorId = new ActorId("Actor1");

// This only creates a proxy object, it does not activate an actor or invoke any methods yet.
MyActor myActor = ActorProxyBase.create(actorId, new URI("fabric:/MyApp/MyActorService"), MyActor.class);

// This will invoke a method on the actor. If an actor with the given ID does not exist, it will be activated by this method call.
myActor.DoWorkAsync().get();
```


Aktör proxy nesnesini oluşturmak için kullanılan iki bilgi parçasının aktör kimliği ve uygulama adı olduğunu unutmayın. Aktör kimliği, uygulama adı aktörün dağıtıldığı [Hizmet Dokusu uygulamasını](service-fabric-reliable-actors-platform.md#application-model) tanımlarken, aktör kimliği benzersiz olarak aktörü tanımlar.

İstemci tarafındaki `ActorProxy` `ActorProxyBase`(C#) / (Java) sınıfı, aktörü nismana göre bulmak ve onunla bir iletişim kanalı açmak için gerekli kararı alır. Ayrıca iletişim hataları ve başarısızlıkları durumunda aktör bulmak için yeniden çalışır. Sonuç olarak, ileti teslimi aşağıdaki özelliklere sahiptir:

* İleti teslimi en iyi çabadır.
* Aktörler aynı istemciden yinelenen iletiler alabilir.

## <a name="concurrency"></a>Eşzamanlılık
Güvenilir Aktörler çalışma süresi aktör yöntemlerine erişmek için basit bir sıra tabanlı erişim modeli sağlar. Bu, bir aktör nesnenin kodu içinde herhangi bir zamanda birden fazla iş parçacığının etkin olabileceği anlamına gelir. Sıra tabanlı erişim, veri erişimi için eşitleme mekanizmalarına gerek olmadığı için eşzamanlı sistemleri büyük ölçüde basitleştirir. Ayrıca, sistemlerin her aktör örneğinin tek dişli erişim yapısı için özel hususlarla tasarlanması gerektiği anlamına gelir.

* Tek bir aktör örneği aynı anda birden fazla isteği işleyemez. Aktör örneği, eşzamanlı istekleri işlemesi bekleniyorsa, bir iş aktüsudur.
* Aktörler, iki aktör arasında dairesel bir istek olduğunda, aktörlerden birine aynı anda harici bir istekte bulunulurken, aktörler birbirlerine kilitlenebilirler. Aktör çalışma süresi otomatik olarak aktör aramaları zaman dışarı ve olası kilitlenme durumları kesmek için arayan için bir özel durum atar.

![Güvenilir Aktörler iletişim][3]

### <a name="turn-based-access"></a>Sıra tabanlı erişim
Dönüş, diğer aktörlerden veya istemcilerden gelen bir talebe yanıt olarak bir aktör yönteminin tam olarak yürütülmesinden veya [zamanlayıcı/anımsatıcı](service-fabric-reliable-actors-timers-reminders.md) geri çağırmasının tam olarak yürütülmesinden oluşur. Bu yöntemler ve geri aramalar eşzamanlı olsa da, Aktörler çalışma zamanı onları terk etmez. Yeni bir dönüşe izin verilmeden önce dönüş tamamen tamamlanmalıdır. Başka bir deyişle, şu anda yürütülmakta olan bir aktör yöntemi veya zamanlayıcı/anımsatıcı geri araması, yönteme yeni bir çağrıya veya geri çağırmaya izin verilmeden önce tamamen tamamlanmış olmalıdır. Yürütme yöntemden veya geri aramadan döndüyse ve yöntem veya geri arama tarafından döndürülen görev tamamlanmışsa, yöntem veya geri arama tamamlanmış olarak kabul edilir. Bu sıra tabanlı eşzamanlılık bile farklı yöntemler, zamanlayıcılar ve geri aramalar arasında saygı olduğunu vurgulamak için değer.

Actors çalışma süresi, bir dönüşün başında aktör başına kilit alarak ve dönüşün sonunda kilidi serbest bırakarak sıra tabanlı eşzamanlılık zorlar. Böylece, sıra tabanlı eşzamanlılık aktörler arasında değil, aktör başına olarak uygulanır. Aktör yöntemleri ve zamanlayıcı/anımsatıcı geri aramaları farklı aktörler adına aynı anda yürütülebilir.

Aşağıdaki örnekte yukarıdaki kavramlar gösterilmektedir. İki eşzamanlı yöntem (örneğin, *Yöntem1* ve *Yöntem2),* bir zamanlayıcı ve bir anımsatıcı uygulayan bir aktör türü düşünün. Aşağıdaki diyagram, bu aktör türüne ait iki aktör *(ActorId1* ve *ActorId2)* adına bu yöntemlerin ve geri çağırmaların yürütülmesi için bir zaman çizelgesi örneği gösterir.

![Güvenilir Aktörler çalışma zamanı sıra tabanlı eşzamanlılık ve erişim][1]

Bu diyagram aşağıdaki kuralları izler:

* Her dikey çizgi, belirli bir aktör adına bir yöntemin veya geri aramanın mantıksal akışını gösterir.
* Her dikey çizgide işaretlenen olaylar kronolojik sırada gerçekleşir ve eskilerin altında yeni olaylar meydana gelir.
* Farklı aktörlere karşılık gelen zaman çizelgeleri için farklı renkler kullanılır.
* Vurgulama, bir yöntem veya geri arama adına aktör başına kilidin tutulduğu süreyi belirtmek için kullanılır.

Göz önünde bulundurulması gereken bazı önemli noktalar:

* *Method1* müşteri isteği *xyz789*yanıt olarak *ActorId2* adına yürütülürken, başka bir istemci isteği *(abc123)* de *Method1* *ActorId2*tarafından yürütülmesi ni gerektirir geldi . Ancak, *Yöntem1'in* ikinci yürütmesi önceki yürütme tamamlanana kadar başlamaz. Benzer şekilde, *Method1* istemci isteği *xyz789*yanıt olarak yürütülmektedir ise *ActorId2* yangınları tarafından kayıtlı bir hatırlatma . Anımsatıcı geri araması yalnızca *Yöntem1'in* her iki yürütmesi tamamlandıktan sonra yürütülür. Tüm bu sıra tabanlı eşzamanlılık *ActorId2*için yürürlüğe sürülmektedir kaynaklanmaktadır.
* Benzer şekilde, sıra tabanlı eşzamanlılık da *ActorId1*için uygulanır , *Yöntem1*yürütülmesi ile gösterildiği gibi , *Yöntem2*, ve *ActorId1* adına zamanlayıcı geri arama bir seri moda oluyor.
* *ActorId1* adına *Method1'in* yürütülmesi *ActorId2*adına yürütülmesiile örtüşmektedir. Bunun nedeni, sıra tabanlı eşzamanlılık aktörler arasında değil, yalnızca bir aktör içinde uygulanır olmasıdır.
* Yöntem/geri arama yürütmelerinin bazılarında, `Task`yöntem döndükten `CompletableFuture`sonra yöntem/geri arama bitişleri ile döndürülen (C#) / (Java) döndürülür. Bazılarında, eşzamanlı işlem yöntem/geri arama döndürülmeden tamamlanmıştır. Her iki durumda da, aktör başına kilit yalnızca hem yöntem/geri arama döndükten hem de eşzamanlı işlem bittikten sonra serbest bırakılır.

### <a name="reentrancy"></a>Yeniden giriş
Aktörler çalışma süresi varsayılan olarak yeniden canlandırma sağlar. Bu, *Aktör A'nın* bir aktör metodu *Aktör B'de*bir yöntem çağırırsa, bu yöntemin Aktör *A'da*başka bir yöntem le ilgili olduğu anlamına gelir, bu yöntemin çalışmasına izin verilir. Bunun nedeni, aynı mantıksal çağrı zinciri bağlamının bir parçası olmasıdır. Tüm zamanlayıcı ve anımsatıcı çağrıları yeni mantıksal çağrı bağlamıyla başlar. Daha fazla bilgi için [Güvenilir Aktörler reentrancy](service-fabric-reliable-actors-reentrancy.md) bakın.

### <a name="scope-of-concurrency-guarantees"></a>Eşzamanlılık garantilerinin kapsamı
Aktörler çalışma süresi, bu yöntemlerin çağrılması kontrol durumlarda bu eşzamanlılık garantileri sağlar. Örneğin, istemci isteğine yanıt olarak yapılan yöntem çağrılarının yanı sıra zamanlayıcı ve anımsatıcı geri aramaları için bu garantileri sağlar. Ancak, aktör kodu bu yöntemleri Aktörler çalışma zamanı tarafından sağlanan mekanizmaların dışında doğrudan çağırırsa, çalışma zamanı eşzamanlılık garantisi sağlayamaz. Örneğin, yöntem aktör yöntemleri tarafından döndürülen görevle ilişkili olmayan bazı görev bağlamında çağrılmışsa, çalışma zamanı eşzamanlılık garantileri sağlayamaz. Yöntem, aktörün kendi başına oluşturduğu bir iş parçacığından çağrılırsa, çalışma süresi eşzamanlılık garantileri de sağlayamaz. Bu nedenle, arka plan işlemlerini gerçekleştirmek için aktörler, sıra tabanlı eşzamanlılık saygı [aktör zamanlayıcıları ve aktör anımsatıcıları](service-fabric-reliable-actors-timers-reminders.md) kullanmalıdır.

## <a name="next-steps"></a>Sonraki adımlar
İlk Güvenilir Aktörler hizmetinizi oluşturarak başlayın:
   * [.NET'te Güvenilir Aktörler ile başlarken](service-fabric-reliable-actors-get-started.md)
   * [Java'da Güvenilir Aktörler ile başlarken](service-fabric-reliable-actors-get-started-java.md)

<!--Image references-->
[1]: ./media/service-fabric-reliable-actors-introduction/concurrency.png
[2]: ./media/service-fabric-reliable-actors-introduction/distribution.png
[3]: ./media/service-fabric-reliable-actors-introduction/actor-communication.png
