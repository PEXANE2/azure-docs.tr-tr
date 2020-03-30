---
title: Güvenilir Hizmet programlama modeline genel bakış
description: Service Fabric'in Güvenilir Servis programlama modeli hakkında bilgi edinin ve kendi hizmetlerinizi yazmaya başlayın.
author: masnider
ms.topic: conceptual
ms.date: 3/9/2018
ms.author: masnider
ms.custom: sfrev
ms.openlocfilehash: 58259b0d19d68c468779a579bd9c86e77106c18d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77083509"
---
# <a name="reliable-services-overview"></a>Reliable Services özelliğine genel bakış

Azure Hizmet Kumaşı, durum ifadebilmeyen ve kullanım durumu dolan hizmetleri yazmayı ve yönetmeyi kolaylaştırır. Bu konu şunları içerir:

* Durumsuz ve durumlu hizmetler için Güvenilir Hizmetler programlama modeli.
* Güvenilir Hizmet yazarken yapmak zorunda olduğunuz seçimler.
* Güvenilir Hizmetlerin ne zaman kullanılacağı ve nasıl yazıldığına dair bazı senaryolar ve örnekler.

*Güvenilir Hizmetler,* Service Fabric'te bulunan programlama modellerinden biridir. Başka bir Güvenilir Hizmetler modelinin üstüne sanal aktör uygulama çerçevesi sağlayan *Güvenilir* [Aktör](https://research.microsoft.com/en-us/projects/orleans/) programlama modelidir. Güvenilir Aktörler hakkında daha fazla bilgi için, [Hizmet Kumaş Güvenilir Aktörler Giriş](service-fabric-reliable-actors-introduction.md)bakın.

Service Fabric, [Service Fabric uygulama yönetimi](service-fabric-deploy-remove-applications.md)aracılığıyla, sağlama ve dağıtımdan yükseltme ve silme yoluyla hizmet ömrünü yönetir.

## <a name="what-are-reliable-services"></a>Güvenilir Hizmetler Nelerdir

Güvenilir Hizmetler, uygulamanız için neyin önemli olduğunu ifade etmenize yardımcı olacak basit, güçlü, üst düzey bir programlama modeli sunar. Güvenilir Hizmetler programlama modeli ile şunları elde elabilirsiniz:

* Servis Kumaş API'lerine erişim. [Misafir Çalıştırılabilir](service-fabric-guest-executables-introduction.md)olarak modellenen Hizmet Kumaşı hizmetlerinin aksine, Güvenilir Hizmetler Doğrudan Servis Kumaş API'lerini kullanabilir. Bu, hizmetlerin şunları sağlar:
  * Sistemi sorgulama
  * Kümedeki varlıklar hakkında sistem durumu bildirme
  * Yapılandırma ve kod değişiklikleri yle ilgili bildirimler alma
  * Diğer hizmetleri bulmak ve onlarla iletişim kurmak,
  * Güvenilir [Koleksiyonları](service-fabric-reliable-services-reliable-collections.md) Kullanın
  * Tümü çeşitli programlama dillerinde birinci sınıf bir programlama modelinden olmak üzere diğer birçok özellikten erişin.
* Diğer tanıdık programlama modelleri gibi hissediyor kendi kodu çalıştırmak için basit bir model. Kodunuz iyi tanımlanmış bir giriş noktasına ve kolayca yönetilen bir yaşam döngüsüne sahiptir.
* Takılabilir bir iletişim modeli. [Web API,](service-fabric-reliable-services-communication-webapi.md)WebSockets, özel TCP protokolleri veya başka bir şey ile HTTP gibi seçtiğiniz aktarım kullanın. Güvenilir Hizmetler, kullanabileceğiniz veya kendi hizmetinizi sağlayabileceğiniz harika kullanıma sunma seçenekleri sunar.
* Özel hizmetler için, Güvenilir Hizmetler programlama [modeli, Güvenilir Koleksiyonlar'ı](service-fabric-reliable-services-reliable-collections.md)kullanarak durumunuzu sürekli ve güvenilir bir şekilde hizmetinizin içinde saklamanızı sağlar. Güvenilir Koleksiyonlar, C# koleksiyonlarını kullanan herkes için tanıdık gelecek olan, son derece kullanılabilir ve güvenilir koleksiyon sınıfları kümesidir. Geleneksel olarak, hizmetler güvenilir devlet yönetimi için dış sistemlere ihtiyaç duydu. Güvenilir Koleksiyonlar ile, yüksek kullanılabilirlik ve yüksek kullanılabilirlik yüksek mağazalardan beklediğiniz güvenilirlikle durumunuzu işlem işleminizin yanında saklayabilirsiniz. İşlemin birlikte yerini ve çalışması için gereken durumu birlikte bu model de gecikmeyi artırır.

## <a name="what-makes-reliable-services-different"></a>Güvenilir Hizmetleri Farklı Kılan Nedir

Güvenilir Hizmetler, Hizmet Kumaşı şunları sağladığından, daha önce yazmış olabileceğiniz hizmetlerden farklıdır:

* **Güvenilirlik** - Hizmetiniz, makinelerinizin başarısız olduğu veya ağ sorunlarına neden olduğu güvenilir olmayan ortamlarda veya hizmetlerin hatalarla karşılaştığı ve çökmediği veya başarısız olduğu durumlarda bile çalışır durumda kalır. Devlet hizmetleri için, durumunuz ağ veya diğer hatalar varlığında bile korunur.
* **Kullanılabilirlik** - Hizmetiniz ulaşılabilir ve yanıt verir. Service Fabric istediğiniz sayıda çalışan kopyayı saklar.
* **Ölçeklenebilirlik** - Hizmetler belirli donanımlardan ayrılmış ve donanım veya diğer kaynakların eklenmesi veya kaldırılması yoluyla gerektiğinde büyüyebilir veya küçülebilir. Hizmetler, hizmetin kısmi hataları ölçeklendirebilmesini ve işleyebilmesini sağlamak için kolayca bölümlere ayırır (özellikle durum durumu yla). Hizmetler kod aracılığıyla dinamik olarak oluşturulabilir ve silinebilir ve örneğin müşteri isteklerine yanıt olarak daha fazla örneğin gerektiğinde döndürülmesini sağlar. Son olarak, Service Fabric hizmetleri hafif olmaya teşvik eder. Service Fabric, tüm işletim sistemi örneklerini veya işlemlerini bir hizmetin tek bir örneğine gerektirmek veya ayırmak yerine, binlerce hizmetin tek bir işlem içinde sağlanmasına olanak tanır.
* **Tutarlılık** - Güvenilir Bir Hizmette depolanan tüm bilgilerin tutarlı olacağı garanti edilebilir. Bu, bir hizmet içindeki birden çok Güvenilir Koleksiyon'da bile geçerlidir. Bir hizmet içindeki koleksiyonlar arasında değişiklikler işlematomik bir şekilde yapılabilir.

## <a name="service-lifecycle"></a>Servis yaşam döngüsü

Hizmetinizin durum dolu veya durumsuz olması, Güvenilir Hizmetler, kodunuzu hızla takmanızı ve başlamanızı sağlayan basit bir yaşam döngüsü sağlar.  Yeni bir hizmeti çalışır hale getirmek için iki yöntem uygulamanız gerekmektedir:

* **CreateServiceReplicaListeners/CreateServiceInstanceListeners** - Bu yöntem, hizmetin kullanmak istediği iletişim yığınını(lar) tanımladığı yöntemdir. [Web API](service-fabric-reliable-services-communication-webapi.md)gibi iletişim yığını, hizmetin dinleme bitiş noktasını veya bitiş noktasını (istemcilerin hizmete nasıl ulaştığını) tanımlayan şeydir. Ayrıca, görünen iletilerin hizmet kodunun geri kalanıyla nasıl etkileşimde olduğunu da tanımlar.
* **RunAsync** - Bu yöntem, hizmetinizin iş mantığını çalıştırdığı ve hizmetin ömrü boyunca çalışması gereken arka plan görevlerini başlatacağı yerdir. Sağlanan iptal belirteci, bu çalışmanın ne zaman durdurulması gerektiğine ilişkin bir sinyaldir. Örneğin, hizmetin iletileri Güvenilir Bir Sıra'dan çıkarıp işlemesi gerekiyorsa, bu çalışma burada gerçekleşir.

Güvenilir hizmetleri ilk kez öğreniyorsanız, okumaya devam edin! Güvenilir hizmetlerin yaşam döngüsünün ayrıntılı bir gözden geçirmesini arıyorsanız, [Güvenilir Hizmetler yaşam döngüsüne genel bakışa](service-fabric-reliable-services-lifecycle.md)göz atın.

## <a name="example-services"></a>Örnek hizmetler

Güvenilir Hizmetler modelinin hem devletsiz hem de durumlu hizmetlerle nasıl çalıştığına yakından bakalım.

### <a name="stateless-reliable-services"></a>Stateless Güvenilir Hizmetler

*Devletsiz hizmet,* aramalar arasında hizmet içinde tutulan bir devlet yoktur. Mevcut olan herhangi bir durum tamamen tek kullanımlıktır ve eşitleme, çoğaltma, kalıcılık veya yüksek kullanılabilirlik gerektirmez.

Örneğin, belleği olmayan ve aynı anda gerçekleştirmek için tüm terimleri ve işlemleri alan bir hesap makinesi düşünün.

Bu durumda, `RunAsync()` hizmetin yapması `runAsync()` gereken arka plan görev işleme olmadığından, hizmetin (C#) veya (Java) boş olabilir. Hesap makinesi hizmeti oluşturulduğunda, `ICommunicationListener` bazı bağlantı noktalarında `CommunicationListener` dinleme bitiş noktası açan bir (C#) veya (Java) (örneğin [Web API)](service-fabric-reliable-services-communication-webapi.md)döndürür. Bu dinleme bitiş noktası, hesap makinesinin genel API'sini tanımlayan farklı hesaplama yöntemlerine (örnek: "Add(n1, n2)") bağlanır.

Bir istemciden arama yapıldığında, uygun yöntem çağrılır ve hesap makinesi hizmeti sağlanan veriler üzerindeki işlemleri gerçekleştirir ve sonucu döndürür. Herhangi bir durumu saklamaz.

Herhangi bir iç durumu depolamamak bu örnek hesap makinesini kolaylaştırır. Ama çoğu hizmet gerçekten vatansız değildir. Bunun yerine, durumlarını başka bir mağazaya dışsallaştırırlar. (Örneğin, oturum durumunu bir destek deposunda veya önbellekte tutmaya dayanan herhangi bir web uygulaması durum ifade etmez.)

Hizmet Kumaşı'nda devletsiz hizmetlerin nasıl kullanıldığının yaygın bir örneği, bir web uygulaması için genel kullanıma açık API'yi ortaya çıkaran bir ön uç olarak dır. Ön uç hizmeti daha sonra kullanıcı isteğini tamamlamak için devlet hizmetleriyle konuşur. Bu durumda, istemcilerden gelen aramalar, devletsiz hizmetin dinlediği 80 gibi bilinen bir bağlantı noktasına yönlendirilir. Bu devletsiz hizmet aramayı alır ve aramanın güvenilir bir taraftan olup olmadığını ve hangi hizmetiçin görev aldığını belirler.  Daha sonra, devletsiz hizmet, aramayı devlet hizmetinin doğru bölümüne iletir ve yanıt bekler. Stateless hizmeti bir yanıt aldığında, özgün istemciye yanıt verir. Böyle bir hizmete örnek olarak, bu repodaki diğer Service Fabric örnekleri nin yanı sıra *Hizmet Kumaşı Başlarken* örneği[(C#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) / [Java)](https://github.com/Azure-Samples/service-fabric-java-getting-started)örneklenir.

### <a name="stateful-reliable-services"></a>DevletE Yönelik Güvenilir Hizmetler

*Hizmetin* çalışması için devletin bir kısmının tutarlı ve mevcut tutulması gereken bir hizmettir. Aldığı güncelleştirmeleri temel alan bir miktar değerin yuvarlanma ortalamalarını sürekli olarak hesaplayan bir hizmeti düşünün. Bunu yapmak için, işlemesi gereken geçerli gelen istek kümesine ve geçerli ortalamaya sahip olması gerekir. Bilgileri harici bir mağazada (bugün Azure blob veya tablo deposu gibi) alan, işleyen ve depolayan tüm hizmetler durum iyidir. Sadece dış devlet deposunda durumunu korur.

Dış mağaza bu durum için güvenilirlik, kullanılabilirlik, ölçeklenebilirlik ve tutarlılık sağlayan şey olduğundan, günümüzde çoğu hizmet durumlarını harici olarak saklar. Service Fabric'te, hizmetlerin durumlarını harici olarak depolaması gerekmez. Service Fabric, hem hizmet kodu hem de hizmet durumu için bu gereksinimleri karşılar.

Görüntüleri işleyen bir hizmet yazmak istediğimizi söyleyebiliriz. Bunu yapmak için, hizmet bir görüntü ve bu görüntü üzerinde gerçekleştirmek için dönüşümler serisi alır. Bu hizmet, bir ILETIŞIM dinleyicisi döndürür (bir WebAPI olduğunu varsayalım) gibi `ConvertImage(Image i, IList<Conversion> conversions)`bir API ortaya çıkarır. Bir istek aldığında, hizmet isteği izlemek `IReliableQueue`için bir 'de saklar ve istemciye bazı kimlik döndürür.

Bu hizmette, `RunAsync()` daha karmaşık olabilir. Hizmetin içinde `RunAsync()` istekleri çeken `IReliableQueue` ve istenen dönüşümleri gerçekleştiren bir döngü vardır. Sonuçlar, istemci geri `IReliableDictionary` geldiğinde dönüştürülmüş görüntülerini alabilmeleri için bir anda depolanır. Bir şey başarısız olsa bile görüntünün kaybolmadığından emin olmak için, bu Güvenilir Hizmet kuyruktan çekilir, dönüşümleri gerçekleştirir ve sonucu tek bir işlemde saklar. Bu durumda, ileti kuyruktan kaldırılır ve sonuçlar yalnızca dönüşümler tamamlandığında sonuç sözlüğünde depolanır. Alternatif olarak, hizmet görüntüyü kuyruktan çıkarıp hemen uzak bir mağazada depolayabilir. Bu, hizmetin yönetmesi gereken durum miktarını azaltır, ancak hizmetin uzak depoyu yönetmek için gerekli meta verileri tutması gerektiğinden karmaşıklığı artırır. Her iki yaklaşımda da, ortada bir şey başarısız olduysa, istek işlenmeyi bekleyen sırada kalır.

Bu hizmet tipik bir .NET hizmeti gibi görünse de, fark, `IReliableDictionary`kullanılan`IReliableQueue` veri yapılarının Service Fabric tarafından sağlanmış olması ve son derece güvenilir, kullanılabilir ve tutarlı olmasıdır.

## <a name="when-to-use-reliable-services-apis"></a>Güvenilir Hizmet API'leri ne zaman kullanılır?

Şu durumlarda Güvenilir Hizmet API'lerini göz önünde bulundurun:

* Hizmetinizin kodunun (ve isteğe bağlı olarak durum) yüksek kullanılabilir ve güvenilir olmasını istiyorsunuz.
* Birden çok durum birimi (örneğin, siparişler ve sipariş satırı maddeleri) arasında işlem garantilerine ihtiyacınız vardır.
* Uygulamanızın durumu doğal olarak Güvenilir Sözlükler ve Kuyruklar olarak modellenebilir.
* Uygulama kodunuz veya eyaletinizin düşük gecikme süresi okuma ve yazma yla yüksek oranda kullanılabilir olması gerekir.
* Uygulamanızın, bir veya daha fazla Güvenilir Koleksiyon'da işlem yapılan işlemlerin eşzamanlılığını veya parçalılığını denetlemesi gerekir.
* İletişimi yönetmek veya hizmetinizin bölümleme düzenini denetlemek istiyorsunuz.
* Kodunuz için serbest iş parçacığı çalışma zamanı ortamı gerekir.
* Uygulamanızın çalışma zamanında Güvenilir Sözlükler veya Kuyruklar veya tüm Hizmetleri dinamik olarak oluşturması veya yok etmesi gerekir.
* Hizmet Kumaşı tarafından sağlanan yedeklemeyi programlı bir şekilde denetlemeniz ve hizmetin izin durumu için özellikleri geri yüklemeniz gerekir.
* Uygulamanızın, devlet birimleri için değişiklik geçmişini koruması gerekir.
* Üçüncü taraf tarafından geliştirilen, özel devlet sağlayıcıları geliştirmek veya tüketmek istiyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenilir Hizmetler hızlı başlat](service-fabric-reliable-services-quick-start.md)
* [Güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)
* [Güvenilir Aktörler programlama modeli](service-fabric-reliable-actors-introduction.md)
