---
title: Güvenilir hizmet programlama modeline genel bakış
description: Service Fabric güvenilir hizmet programlama modeli hakkında bilgi edinin ve kendi hizmetlerinizi yazmaya başlayın.
author: masnider
ms.topic: conceptual
ms.date: 3/9/2018
ms.author: masnider
ms.custom: sfrev
ms.openlocfilehash: 58259b0d19d68c468779a579bd9c86e77106c18d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77083509"
---
# <a name="reliable-services-overview"></a>Reliable Services özelliğine genel bakış

Azure Service Fabric durum bilgisiz ve durum bilgisi olan hizmetlerin yazılmasını ve yönetilmesini basitleştirir. Bu konu şunları içerir:

* Durum bilgisiz ve durum bilgisi olan hizmetler için Reliable Services programlama modeli.
* Güvenilir bir hizmeti yazarken yapmanız istediğiniz seçimler.
* Bazı senaryolar ve ne zaman Reliable Services kullanılacağı ve bunların nasıl yazıldığı hakkında örnekler.

*Reliable Services* , Service Fabric kullanılabilen programlama modellerinden biridir. Diğeri, Reliable Services modelinin üstünde [sanal aktör](https://research.microsoft.com/en-us/projects/orleans/) uygulama çerçevesi sağlayan *güvenilir aktör* programlama modelidir. Reliable Actors hakkında daha fazla bilgi için bkz. [Service Fabric Reliable Actors giriş](service-fabric-reliable-actors-introduction.md).

Service Fabric, hazırlama ve dağıtım aracılığıyla [Service Fabric uygulama yönetimi](service-fabric-deploy-remove-applications.md)aracılığıyla hizmet ömrünü yönetir.

## <a name="what-are-reliable-services"></a>Reliable Services nedir?

Reliable Services, uygulamanız için önemli olanları ifade etmenize yardımcı olmak için basit ve güçlü, en üst düzey bir programlama modeli sağlar. Reliable Services programlama modeliyle şunları alırsınız:

* Service Fabric API 'Lerine erişin. [Konuk yürütülebilir dosyaları](service-fabric-guest-executables-introduction.md)olarak modellenen Service Fabric hizmetlerinden farklı olarak Reliable Services Service Fabric API 'leri doğrudan kullanabilir. Bu, hizmetlerin şunları yapmasına izin verir:
  * Sistemi sorgulama
  * Kümedeki varlıklar hakkında durum bildir
  * Yapılandırma ve kod değişiklikleri hakkında bildirim alın
  * Diğer Hizmetleri bulma ve bunlarla iletişim kurma,
  * [Güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md) kullanın
  * Birçok farklı özelliğe, hepsi de birkaç programlama dilinde birinci sınıf programlama modelinden erişin.
* Diğer tanıdık programlama modelleri gibi kendi kodunuzu çalıştırmak için basit bir model. Kodunuzun iyi tanımlanmış bir giriş noktası ve kolayca yönetilen yaşam döngüsü vardır.
* Takılabilir iletişim modeli. [Web API 'si](service-fabric-reliable-services-communication-webapi.md), WebSockets, özel TCP protokolleri veya başka herhangi bir şeyı içeren http gibi istediğiniz taşımayı kullanın. Reliable Services, kullanabileceğiniz bazı harika kullanıma hazır seçenekler sağlar veya kendi kendinize sağlayabilirsiniz.
* Durum bilgisi olan hizmetler için Reliable Services programlama modeli, [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)kullanarak durumlarınızın durumunu tutarlı ve güvenilir bir şekilde depolamanıza olanak tanır. Güvenilir koleksiyonlar, C# koleksiyonları kullanan herkese tanıdık olan, yüksek oranda kullanılabilir ve güvenilir bir koleksiyon sınıfları kümesidir. Geleneksel olarak, hizmetler, güvenilir durum yönetimi için dış sistemler için gerekli. Güvenilir koleksiyonlar sayesinde, yüksek oranda kullanılabilir dış depolardan beklemeniz için kullandığınız aynı yüksek kullanılabilirliğe ve güvenilirliğe sahip olarak, durumunuzu daha sonra saklayabilirsiniz. Bu model, çalışması gereken işlem ve durumu birlikte konumlandırdığı için gecikmeyi de artırır.

## <a name="what-makes-reliable-services-different"></a>Ne Reliable Services farklı yapar

Reliable Services, Service Fabric aşağıdakileri sağladığından daha önce yazmış olduğunuz hizmetlerden farklıdır:

* **Güvenilirlik** -hizmetiniz, makinelerinizin başarısız olduğu veya ağ sorunları üzerinde karşılaştığı ve hizmetlerin hata ve kilitlenme veya başarısız olduğu durumlarda güvenilir olmayan ortamlarda bile kalır. Durum bilgisi olan hizmetler için, durumunuz ağ veya diğer hataların olması durumunda bile korunur.
* **Kullanılabilirlik** -hizmetiniz erişilebilir ve yanıt veriyor. Service Fabric istediğiniz sayıda çalışan kopyayı korur.
* **Ölçeklenebilirlik** -hizmetler belirli donanımlardan ayrılır ve donanım veya diğer kaynakların eklenmesi ya da kaldırılması sırasında gerektiğinde büyüyebilir veya küçülebilir. Hizmetler, hizmetin kısmi hataların ölçeğini ve işlemesini sağlamak için kolayca bölümlenir (özellikle de durum bilgisi olan durumda). Hizmetler, kod aracılığıyla dinamik olarak oluşturulup silinebilir, örneğin müşteri isteklerine yanıt olarak daha fazla örnek olması gerekir. Son olarak, Service Fabric Hizmetleri hafif olarak önerir. Service Fabric, tüm işletim sistemi örneklerinin veya işlemlerinin bir hizmetin tek bir örneğine sağlanması veya ayrılması yerine, tek bir işlem içinde binlerce hizmet verilmesini sağlar.
* **Tutarlılık** -güvenilir bir hizmette depolanan bilgilerin tutarlı olması garantilenebilir. Bu, bir hizmet içindeki birden çok güvenilir koleksiyonda bile geçerlidir. Bir hizmet içindeki koleksiyonlar genelinde yapılan değişiklikler işlem için atomik bir şekilde yapılabilir.

## <a name="service-lifecycle"></a>Hizmet yaşam döngüsü

Hizmetinizin durum bilgisiz veya durumsuz olup olmadığı Reliable Services, kodunuzu hızlıca takmanızı ve başlamanıza olanak tanıyan basit bir yaşam döngüsü sağlar.  Yeni bir hizmet alma ve çalıştırma için iki yöntem uygulamanız gerekir:

* **CreateServiceReplicaListeners/Createserviceınstancelisteners** -bu yöntem, hizmetin kullanmak istediği iletişim yığınlarını tanımladığı yerdir. [Web API 'si](service-fabric-reliable-services-communication-webapi.md)gibi iletişim yığını, hizmet için dinleme uç noktasını veya uç noktalarını tanımlar (istemcilerin hizmete nasıl ulaştıklarında). Ayrıca, görüntülenen iletilerin hizmet kodunun geri kalanı ile nasıl etkileşime gireceğini tanımlar.
* **RunAsync** -bu yöntem, hizmetinizin iş mantığını çalıştırdığı ve hizmetin kullanım ömrü boyunca çalışması gereken arka plan görevlerinin çalıştırılacağı yerdir. Belirtilen iptal belirteci, işin ne zaman durması gerektiğini belirten bir sinyaltir. Örneğin, hizmetin iletileri güvenilir bir kuyruktan çekmek ve onları işlemesi gerekiyorsa, bu işin gerçekleştiği yerdir.

İlk kez güvenilir hizmetler hakkında bilgi edinmek istiyorsanız, okumaya devam edin! Güvenilir hizmetlerin yaşam döngüsüne yönelik ayrıntılı bir anlatım arıyorsanız [Reliable Services yaşam döngüsüne genel bakış](service-fabric-reliable-services-lifecycle.md)' a bakın.

## <a name="example-services"></a>Örnek hizmetler

Reliable Services modelinin hem durum bilgisi olmayan hem de durum bilgisi olan hizmetlerle nasıl çalıştığını daha yakından inceleyelim.

### <a name="stateless-reliable-services"></a>Durum bilgisi olmayan Reliable Services

*Durum bilgisi* olmayan bir hizmet, çağrılar genelinde hizmette hiçbir durum tutulmamaktadır. Var olan herhangi bir durum tamamen atılabilir ve eşitleme, çoğaltma, kalıcılık veya yüksek kullanılabilirlik gerektirmez.

Örneğin, belleği olmayan bir Hesaplayıcı düşünün ve aynı anda gerçekleştirilecek tüm hüküm ve işlemleri alır.

Bu durumda, hizmetin yapması `RunAsync()` gereken arka plan görev `runAsync()` işlemesi olmadığından hizmetin (C#) veya (Java) boş olabilir. Hesaplayıcı hizmeti oluşturulduğunda, bazı bağlantı noktaları üzerinde bir `ICommunicationListener` dinleme uç noktası açan `CommunicationListener` bir (C#) veya (Java) (örneğin, [Web API](service-fabric-reliable-services-communication-webapi.md)) döndürür. Bu dinleme uç noktası, farklı hesaplama yöntemlerine (örneğin, "Ekle (N1, N2)") kadar ana bilgisayarın ortak API 'sini tanımlar.

İstemciden bir çağrı yapıldığında, uygun yöntem çağrılır ve Hesaplayıcı hizmeti, belirtilen verilerde işlemleri gerçekleştirir ve sonucu döndürür. Herhangi bir durum depolamaz.

Herhangi bir iç durumu depolamamadan Bu örnek Hesaplayıcı basit hale gelir. Ancak hizmetlerin çoğu gerçekten durum bilgisiz değildir. Bunun yerine, diğer mağazalarına durumlarını externalize. (Örneğin, oturum durumunu bir yedekleme deposunda veya önbellekte tutmaya bağlı olan herhangi bir Web uygulaması durum bilgisiz değildir.)

Bir Web uygulaması için genel kullanıma yönelik API 'YI kullanıma sunan bir ön uç olarak, durum bilgisi olmayan hizmetlerin Service Fabric nasıl kullanıldığına ilişkin yaygın bir örnek. Ön uç hizmeti, bir Kullanıcı isteğini tamamlamaya yönelik bir durum bilgisi olan hizmetlerden daha fazla konuşuyor. Bu durumda, istemcilerden gelen çağrılar, durum bilgisi olmayan hizmetin dinlediği 80 gibi bilinen bir bağlantı noktasına yönlendirilir. Bu durum bilgisiz hizmeti, çağrıyı alır ve çağrının güvenilen bir taraftan mı yoksa hedeflenen hizmetten mi olduğunu belirler.  Ardından, durum bilgisi olmayan hizmet çağrıyı durum bilgisiz hizmetin doğru bölümüne iletir ve bir yanıt bekler. Durum bilgisi olmayan hizmet bir yanıt aldığında, özgün istemciye yanıt verir. Bu tür bir hizmete örnek olarak, söz konusu depodaki diğer Service Fabric örnekleri arasında *Service Fabric* Başlarken örneği ([C#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) / [Java](https://github.com/Azure-Samples/service-fabric-java-getting-started)) bulunur.

### <a name="stateful-reliable-services"></a>Durum bilgisi olan Reliable Services

Durum *bilgisi* olan bir hizmet, hizmetin çalışması için tutarlı ve mevcut durumda tutulan bir durum bölümünün olması gereken bir hizmettir. Aldığı güncelleştirmelere göre bazı bir değerin hareketli ortalamasını sürekli olarak hesaplayan bir hizmeti göz önünde bulundurun. Bunu yapmak için, işlem yapması gereken geçerli gelen istek kümesine ve geçerli ortalamaya sahip olmalıdır. Bir dış depodaki (bugün bir Azure Blob veya tablo deposu gibi) bilgileri alan, işleyen ve depolayan tüm hizmetler durum bilgisi değildir. Yalnızca dış durum deposunda durumunu tutar.

Dış mağaza bu durum için güvenilirlik, kullanılabilirlik, ölçeklenebilirlik ve tutarlılık sağlar. Service Fabric, hizmetlerinin durumlarını dışarıdan depolaması için gerekli değildir. Service Fabric, bu gereksinimlerin her ikisi de hizmet kodu ve hizmet durumu için ele alınır.

Görüntüleri işleyen bir hizmet yazmak istiyoruz. Bunu yapmak için, hizmet bir görüntüyü ve bu görüntüde gerçekleştirilecek dönüştürme dizilerini alır. Bu hizmet, gibi `ConvertImage(Image i, IList<Conversion> conversions)`bir API 'yi kullanıma sunan bir iletişim dinleyicisi (bir WebAPI olduğunu varsayalım) döndürür. Bir istek aldığında, hizmet onu bir içinde depolar ve isteği izleyebilmek `IReliableQueue`için ISTEMCIYE bir kimlik döndürür.

Bu hizmette `RunAsync()` daha karmaşık olabilir. Hizmetin, isteklerini aldığı `RunAsync()` `IReliableQueue` ve istenen dönüştürmeleri gerçekleştiren içindeki bir döngüsü vardır. Sonuçlar, istemci geri geldiğinde dönüştürülen `IReliableDictionary` görüntülerini elde ettikleri zaman içinde depolanır. Görüntü kaybolmamasını sağlamak için, bu güvenilir hizmet sıradan çıkar, dönüşümleri gerçekleştirir ve sonucu tek bir işlemde depolar. Bu durumda, ileti kuyruktan kaldırılır ve sonuçlar yalnızca dönüştürmeler tamamlandığında sonuç sözlüğünde depolanır. Alternatif olarak, hizmet, görüntüyü kuyruktan çekerek ve bir uzak depoda hemen depolayabileceği. Bu, hizmetin yönetmesi gereken durum miktarını azaltır, ancak hizmetin, uzak mağazayı yönetmek için gerekli meta verileri tutması gerektiğinden karmaşıklığı artırır. Her iki yaklaşım da, ortadaki bir hata başarısız olursa, istek işlenmek üzere bekleyen kuyrukta kalır.

Bu hizmet tipik bir .NET hizmeti gibi gelse de aradaki fark, kullanılan veri yapılarının (`IReliableQueue` ve `IReliableDictionary`) Service Fabric tarafından sağlandığı ve yüksek oranda güvenilir, kullanılabilir ve tutarlı olması gerektiğidir.

## <a name="when-to-use-reliable-services-apis"></a>Reliable Services API 'Leri ne zaman kullanılır

Reliable Services API 'Leri göz önünde bulundurun:

* Hizmetinizin kodunun (ve isteğe bağlı olarak) yüksek oranda kullanılabilir ve güvenilir olmasını istiyorsunuz.
* Birden çok durum birimi (örneğin, siparişler ve sipariş satırı öğeleri) boyunca işlem garantisi almanız gerekir.
* Uygulamanızın durumu doğal olarak güvenilir sözlükler ve kuyruklar olarak modellenebilir.
* Uygulamalarınızın kodunuzun veya durumlarınızın, düşük gecikmeli okuma ve yazma işlemleri ile yüksek oranda kullanılabilir olması gerekir.
* Uygulamanızın, bir veya daha fazla güvenilir koleksiyonda gerçekleştirilen tamamlanmış işlemlerin eşzamanlılık veya ayrıntı düzeyi denetimine sahip olması gerekir.
* İletişimleri yönetmek veya hizmetiniz için bölümleme şemasını denetlemek istiyorsunuz.
* Kodunuzun ücretsiz bir iş parçacıklı çalışma zamanı ortamı olması gerekir.
* Uygulamanızın, çalışma zamanında güvenilir sözlükler veya kuyrukları veya tüm hizmetleri dinamik olarak oluşturması veya yok etmek gerekir.
* Hizmetinizin durumu için Service Fabric tarafından sunulan yedekleme ve geri yükleme özelliklerini programlı olarak kontrol etmeniz gerekir.
* Uygulamanızın durum birimleri için değişiklik geçmişini koruması gerekir.
* Üçüncü taraf tarafından geliştirilen özel durum sağlayıcılarını geliştirmek veya kullanmak istiyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar

* [Hızlı başlangıç Reliable Services](service-fabric-reliable-services-quick-start.md)
* [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)
* [Reliable Actors programlama modeli](service-fabric-reliable-actors-introduction.md)
