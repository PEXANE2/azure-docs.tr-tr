---
title: Azure Service Fabric Reliable Services yaşam döngüsü
description: Durum bilgisiz ve durum bilgisi olmayan hizmetler için Java kullanarak bir Azure Service Fabric Reliable Services uygulamasındaki yaşam döngüsü olayları hakkında bilgi edinin.
author: PavanKunapareddyMSFT
ms.topic: conceptual
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 1d3be958a0649ed3e80df2d63adbdf0b91831dbd
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75639607"
---
# <a name="reliable-services-lifecycle"></a>Reliable Services yaşam döngüsü
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Reliable Services Azure Service Fabric kullanılabilen programlama modellerinden biridir. Reliable Services yaşam döngüsü hakkında bilgi edinirken temel yaşam döngüsü olaylarının anlaşılması önemlidir. Olayların tam sıralaması yapılandırma ayrıntılarına bağlıdır. 

Genel olarak, Reliable Services yaşam döngüsü aşağıdaki olayları içerir:

* Başlangıç sırasında:
  * Hizmetler oluşturulur.
  * Hizmetler, sıfır veya daha fazla dinleyici oluşturma ve döndürme fırsatına sahiptir.
  * Hizmet ile iletişim için döndürülen tüm dinleyiciler açılır.
  * Hizmetin `runAsync` yöntemi çağrılır, bu nedenle hizmet uzun süre çalışan veya arka plan işi gerçekleştirebilir.
* Kapatılırken:
  * `runAsync` geçirilen iptal belirteci iptal edilir ve dinleyiciler kapalıdır.
  * Hizmet nesnesinin kendisi kaldırılabilir.

Reliable Services içindeki olayların sırası, güvenilir hizmetin durum bilgisiz veya durum bilgisiz olmasına bağlı olarak biraz değişebilir. 

Ayrıca, durum bilgisi olan hizmetler için birincil takas senaryosunu ele almanız gerekir. Bu sıra sırasında, birincil rolü başka bir çoğaltmaya aktarılır (veya geri gelir) hizmet kapatılmadan önce. 

Son olarak, hata veya hata koşulları hakkında düşünmeniz gerekir.

## <a name="stateless-service-startup"></a>Durum bilgisi olmayan hizmet başlangıcı
Durum bilgisi olmayan bir hizmetin yaşam döngüsü oldukça basittir. Olayların sırası aşağıda verilmiştir:

1. Hizmet oluşturulur.
2. Bu olaylar paralel olarak oluşur:
    - `StatelessService.createServiceInstanceListeners()` çağrılır ve döndürülen tüm dinleyiciler açılır. `CommunicationListener.openAsync()` her dinleyicide çağrılır.
    - Hizmetin `runAsync` yöntemi (`StatelessService.runAsync()`) çağırılır.
3. Varsa, hizmetin kendi `onOpenAsync` yöntemi çağırılır. Özellikle, `StatelessService.onOpenAsync()` çağrılır. Bu, yaygın olmayan bir geçersiz kılma, ancak kullanılabilir.

Dinleyicileri oluşturma ve açma çağrısı ile `runAsync`çağrısı arasında bir sıralama olmadığını unutmayın. `runAsync` başlatılmadan önce dinleyiciler açılabilir. Benzer şekilde, `runAsync`, iletişim dinleyicileri açılmadan önce ya da oluşturulmadan önce çağrılabilir. Herhangi bir eşitleme gerekliyse, uygulayan tarafından yapılması gerekir. Yaygın olarak karşılaşılan bazı çözümler şunlardır:

* Bazen, diğer bilgiler oluşturuluncaya veya diğer iş tamamlanana kadar dinleyiciler çalışamaz. Durum bilgisi olmayan hizmetler için bu iş genellikle hizmetin oluşturucusunda yapılabilir. `createServiceInstanceListeners()` çağrısı sırasında veya dinleyicinin oluşturulması kapsamında yapılabilir.
* Bazen `runAsync` kod, dinleyiciler açık olana kadar başlamaz. Bu durumda, ek koordinasyon gereklidir. Yaygın bir çözüm, dinleyicilerde bayrak eklemektir. Bayrak, dinleyicilerinin ne zaman bittiğini gösterir. `runAsync` yöntemi, asıl çalışmaya devam etmeden önce bunu denetler.

## <a name="stateless-service-shutdown"></a>Durum bilgisi olmayan hizmet kapatması
Durum bilgisi olmayan bir hizmetin kapatılması sırasında, aynı model izlenir, ancak tersi durumda:

1. Bu olaylar paralel olarak oluşur:
    - Açık tüm dinleyiciler kapanır. `CommunicationListener.closeAsync()` her dinleyicide çağrılır.
    - `runAsync()` geçirilen iptal belirteci iptal edildi. İptal belirtecinin `isCancelled` özelliği `true`döndürür ve çağrılırsa, belirtecin `throwIfCancellationRequested` yöntemi bir `CancellationException`atar.
2. Her dinleyicide `closeAsync()` bittiğinde ve `runAsync()` tamamlandığında, varsa hizmetin `StatelessService.onCloseAsync()` yöntemi çağrılır. Bu, ortak bir geçersiz kılma değildir, ancak kaynakları güvenli bir şekilde kapatmak, arka planda işlemeyi durdurmak, dış durumu kaydetmek veya mevcut bağlantıları kapatmak için kullanılabilir.
3. `StatelessService.onCloseAsync()` tamamlandıktan sonra, hizmet nesnesi kaldırılabilir.

## <a name="stateful-service-startup"></a>Durum bilgisi olan hizmet başlatma
Durum bilgisi olan hizmetlerde, az sayıda değişiklikle durum bilgisi olmayan hizmetlere benzer bir model vardır.  Durum bilgisi olan bir hizmeti başlatmak için olayların sırası aşağıda verilmiştir:

1. Hizmet oluşturulur.
2. `StatefulServiceBase.onOpenAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.
3. Bu olaylar paralel olarak oluşur:
    - `StatefulServiceBase.createServiceReplicaListeners()` çağrılır. 
      - Hizmet bir birincil hizmet ise, döndürülen tüm dinleyiciler açılır. `CommunicationListener.openAsync()` her dinleyicide çağrılır.
      - Hizmet ikincil bir hizmet ise, yalnızca `listenOnSecondary = true` olarak işaretlenen dinleyiciler açılır. İkincil öğeler üzerinde açık olan dinleyicilerinin olması daha az yaygındır.
    - Hizmet şu anda birincil ise, hizmetin `StatefulServiceBase.runAsync()` yöntemi çağrılır.
4. Tüm çoğaltma dinleyicisinin `openAsync()` sonuna çağrı yapıldıktan sonra `runAsync()` çağrıldığında `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

Durum bilgisi olmayan hizmetlere benzer şekilde, durum bilgisi olan hizmette, dinleyicilerinin oluşturulduğu ve açıldığı sırada ve `runAsync` çağrıldığında hiçbir koordinasyon yoktur. Koordine etmeniz gerekiyorsa, çözümler çok aynıdır. Ancak durum bilgisi olan hizmet için bir ek durum daha vardır. İletişim dinleyicilerine gelen çağrıların bazı [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)içinde bilgi gerektirdiğini varsayalım. İletişim dinleyicileri, güvenilir koleksiyonlar okunabilir veya yazılabilir olmadan önce açılabileceğinden ve `runAsync` başlamadan önce, bazı ek koordinasyon gereklidir. En basit ve en yaygın çözüm, iletişim dinleyicilerinin bir hata kodu döndürmesi içindir. İstemci, isteği yeniden denemeyi bilmemiz için hata kodunu kullanır.

## <a name="stateful-service-shutdown"></a>Durum bilgisi olan hizmet kapatması
Durum bilgisi olmayan hizmetler gibi, kapatılırken yaşam döngüsü olayları başlangıç sırasında ve ters çevrilme sırasında aynıdır. Durum bilgisi olan bir hizmet kapatıldığında, aşağıdaki olaylar gerçekleşir:

1. Bu olaylar paralel olarak oluşur:
    - Açık tüm dinleyiciler kapanır. `CommunicationListener.closeAsync()` her dinleyicide çağrılır.
    - `runAsync()` geçirilen iptal belirteci iptal edildi. İptal belirtecinin `isCancelled()` yöntemine bir çağrı `true`döndürür ve çağrılırsa, belirtecin `throwIfCancellationRequested()` yöntemi bir `OperationCanceledException`oluşturur.
2. Her dinleyicide `closeAsync()` tamamlandıktan sonra ve `runAsync()` tamamlandığında hizmetin `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

   > [!NOTE]  
   > `runAsync` bitmesi için, yalnızca bu çoğaltma birincil bir çoğaltmaysa gereklidir.

3. `StatefulServiceBase.onChangeRoleAsync()` yöntemi bittikten sonra, `StatefulServiceBase.onCloseAsync()` yöntemi çağrılır. Bu çağrı yaygın olmayan bir geçersiz kılma, ancak kullanılabilir.
3. `StatefulServiceBase.onCloseAsync()` tamamlandıktan sonra, hizmet nesnesi kaldırılabilir.

## <a name="stateful-service-primary-swaps"></a>Durum bilgisi olan hizmet birincil takas
Durum bilgisi olan bir hizmet çalışırken, iletişim dinleyicileri açılır ve `runAsync` yöntemi yalnızca durum bilgisi olan hizmetlerin birincil çoğaltmaları için çağırılır. İkincil çoğaltmalar oluşturulur, ancak daha fazla çağrı yapılmaz. Durum bilgisi olan bir hizmet çalışırken birincil olan çoğaltma değişebilir. Durum bilgisi olan bir çoğaltmanın görebileceği yaşam döngüsü olayları, değişim sırasında çoğaltmanın indirgenme veya yükseltilme olmasına bağlıdır.

### <a name="for-the-demoted-primary"></a>İndirgenen birincil için
Service Fabric, ileti işlemeyi durdurmak ve arka plan işlerini durdurmak için indirgenen birincil çoğaltmaya ihtiyaç duyuyor. Bu adım hizmetin kapatıldığı zaman ile benzerdir. Bir farklılık, hizmetin ikincil olarak kaldığı için çıkarlanmadığı veya kapatılmadığı bir farktır. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar paralel olarak oluşur:
    - Açık tüm dinleyiciler kapanır. `CommunicationListener.closeAsync()` her dinleyicide çağrılır.
    - `runAsync()` geçirilen iptal belirteci iptal edildi. İptal belirtecinin `isCancelled()` yönteminin bir denetimi `true`döndürür. Çağrılırsa, belirtecin `throwIfCancellationRequested()` yöntemi bir `OperationCanceledException`oluşturur.
2. Her dinleyicide `closeAsync()` tamamlandıktan sonra ve `runAsync()` tamamlandığında hizmetin `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

### <a name="for-the-promoted-secondary"></a>Yükseltilen ikincil için
Benzer şekilde, Service Fabric, hattaki iletileri dinlemeye başlamak için yükseltilen ikincil çoğaltmanın yanı sıra tamamlaması gereken tüm arka plan görevlerini başlatır. Bu işlem, hizmetin oluşturulduğu zaman ile benzerdir. Fark, çoğaltmanın kendisi zaten var. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar paralel olarak oluşur:
    - `StatefulServiceBase.createServiceReplicaListeners()` çağrılır ve döndürülen tüm dinleyiciler açılır. `CommunicationListener.openAsync()` her dinleyicide çağrılır.
    - Hizmetin `StatefulServiceBase.runAsync()` yöntemi çağrılır.
2. Tüm çoğaltma dinleyicisinin `openAsync()` sonuna çağrı yapıldıktan sonra `runAsync()` çağrıldığında `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Durum bilgisi olan hizmet kapanışı ve birincil indirgeme sırasında sık karşılaşılan sorunlar
Service Fabric, birden çok nedenden dolayı durum bilgisi olan bir hizmetin birincili olarak değiştirir. En yaygın nedenler [küme yeniden dengeleme](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltmeleridir](service-fabric-application-upgrade.md). Bu işlemler sırasında, hizmetin `cancellationToken`olması önemlidir. Bu, hizmetin silinmiş olması gibi normal hizmet kapatmaları sırasında da geçerlidir.

İptali sorunsuz bir şekilde işlemeyen hizmetler, birkaç sorunla karşılaşabilir. Bu işlemler, Service Fabric hizmetlerin düzgün şekilde durdurulmasını beklediği için yavaştır. Bu, son olarak zaman aşımına uğrar ve geri alma işlemi başarısız yükseltmelere neden olabilir. İptal belirtecini karşılamamanın başarısız olmasına neden olabilir. Düğümler sık kullanıldığından, kümeler dengesiz hale gelir. Ancak, hizmetler başka bir yere taşınamayacak kadar uzun sürdüğü için yeniden dengelenemeyecek. 

Hizmetler durum bilgisi olduğundan, hizmetlerin [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)kullanması da olasıdır. Service Fabric, birincil bir indirgendiğinde, temel alınan duruma Yazma erişiminin iptal edilmesi gerekir. Bu, hizmet yaşam döngüsünü etkileyebilecek ikinci bir sorun kümesine yol açar. Koleksiyonlar, zamanlamaya göre özel durumlar, çoğaltmanın taşınıp taşınmakta veya kapatılmakta olup olmadığı için döndürülür. Bu özel durumları doğru bir şekilde işlemek önemlidir. 

Service Fabric tarafından oluşturulan özel durumlar kalıcı [(`FabricException`)](https://docs.microsoft.com/java/api/system.fabric.exception) ya da geçici [(`FabricTransientException`)](https://docs.microsoft.com/java/api/system.fabric.exception.fabrictransientexception). Kalıcı özel durumlar günlüğe kaydedilir ve oluşturulmalıdır. Geçici özel durumlar, yeniden deneme mantığına göre yeniden denenebilir.

Test ve doğrulama Reliable Services önemli bir parçası, hizmet yaşam döngüsü olaylarıyla birlikte `ReliableCollections` kullanmaktan gelen özel durumları işledir. Hizmetinizi her zaman yükleme altında çalıştırmanızı öneririz. Ayrıca üretime dağıtım yapmadan önce yükseltmeler ve [Chaos testi](service-fabric-controlled-chaos.md) gerçekleştirmeniz gerekir. Bu temel adımlar, hizmetinizin doğru şekilde uygulandığından ve yaşam döngüsü olaylarını doğru şekilde işleyeceğinden emin olmanıza yardımcı olur.

## <a name="notes-on-service-lifecycle"></a>Hizmet yaşam döngüsü notları
* Hem `runAsync()` yöntemi hem de `createServiceInstanceListeners/createServiceReplicaListeners` çağrıları isteğe bağlıdır. Bir hizmette tek bir, her ikisi birden olabilir veya hiçbiri olabilir. Örneğin, hizmet tüm işini Kullanıcı çağrılarına yanıt olarak alıyorsa, `runAsync()`uygulaması için gerekli değildir. Yalnızca iletişim dinleyicileri ve bunlarla ilişkili kod gereklidir.  Benzer şekilde, iletişim dinleyicileri oluşturma ve döndürme isteğe bağlıdır. Hizmetin yalnızca bir arka plan işi olabilir, bu nedenle yalnızca `runAsync()`uygulaması gerekir.
* Bir hizmetin başarıyla `runAsync()` tamamlaması ve bundan geri dönmesi için geçerlidir. Bu bir hata koşulu olarak kabul edilmez. Hizmet bitirme 'nin arka plan çalışmasını temsil eder. Durum bilgisi olmayan Reliable Services için, hizmet birincili indirgendiyse ve sonra birincil olarak geri yükseltildiğinde `runAsync()` yeniden çağırılır.
* Bir hizmet beklenmeyen bir özel durum vererek `runAsync()` 'tan çıkılırken bu bir hata olur. Hizmet nesnesi kapatılır ve bir sistem durumu hatası bildirilir.
* Bu yöntemlerin döndürülmesinin zaman sınırı olmasa da, anında yazma özelliğini kaybedersiniz. Bu nedenle, herhangi bir gerçek çalışmayı tamamlayamazsınız. İptal isteğini aldıktan sonra mümkün olduğunca hızlı bir şekilde dönebilmenizi öneririz. Hizmetiniz bu API çağrılarına makul bir süre içinde yanıt vermezse, Service Fabric hizmetinizi zorla sonlandırabilirler. Genellikle bu durum yalnızca uygulama yükseltmeleri sırasında veya bir hizmetin silindiği zaman gerçekleşir. Bu zaman aşımı varsayılan olarak 15 dakikadır.
* `onCloseAsync()` yolundaki hatalara `onAbort()` çağrılmakta. Bu çağrı, hizmetin talep ettikleri tüm kaynakları temizlemesi ve serbest bırakmak için en iyi çaba bir fırsattır. Bu, genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric iç hatalardan dolayı hizmet örneğinin yaşam döngüsünü güvenilir bir şekilde yönetebilmeniz durumunda çağrılır.
* `OnChangeRoleAsync()`, durum bilgisi olan hizmet çoğaltması rolü değiştirirken (örneğin, birincil veya ikincil) çağrılır. Birincil çoğaltmalara yazma durumu verilir (güvenilir koleksiyonlar oluşturma ve bunlara yazma izni verilir). İkincil çoğaltmalara okuma durumu verilir (yalnızca mevcut güvenilir koleksiyonlardan okunabilir). Durum bilgisi olan bir hizmette çoğu iş, birincil çoğaltmada gerçekleştirilir. İkincil çoğaltmalar salt okuma doğrulaması, rapor oluşturma, veri madenciliği veya diğer salt okuma işleri gerçekleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services giriş](service-fabric-reliable-services-introduction.md)
* [Hızlı başlangıç Reliable Services](service-fabric-reliable-services-quick-start-java.md)

