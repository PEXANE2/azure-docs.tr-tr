---
title: Azure Service Fabric Reliable Services yaşam döngüsü
description: Durum bilgisiz ve durum bilgisi olmayan hizmetler için Java kullanarak bir Azure Service Fabric Reliable Services uygulamasındaki yaşam döngüsü olayları hakkında bilgi edinin.
author: PavanKunapareddyMSFT
ms.topic: conceptual
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 1d3be958a0649ed3e80df2d63adbdf0b91831dbd
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
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
  * Öğesine `runAsync` geçirilen iptal belirteci iptal edilir ve dinleyiciler kapalıdır.
  * Hizmet nesnesinin kendisi kaldırılabilir.

Reliable Services içindeki olayların sırası, güvenilir hizmetin durum bilgisiz veya durum bilgisiz olmasına bağlı olarak biraz değişebilir. 

Ayrıca, durum bilgisi olan hizmetler için birincil takas senaryosunu ele almanız gerekir. Bu sıra sırasında, birincil rolü başka bir çoğaltmaya aktarılır (veya geri gelir) hizmet kapatılmadan önce. 

Son olarak, hata veya hata koşulları hakkında düşünmeniz gerekir.

## <a name="stateless-service-startup"></a>Durum bilgisi olmayan hizmet başlangıcı
Durum bilgisi olmayan bir hizmetin yaşam döngüsü oldukça basittir. Olayların sırası aşağıda verilmiştir:

1. Hizmet oluşturulur.
2. Bu olaylar paralel olarak oluşur:
    - `StatelessService.createServiceInstanceListeners()`çağrılır ve döndürülen tüm dinleyiciler açılır. `CommunicationListener.openAsync()`Her dinleyicide çağrılır.
    - Hizmetin `runAsync` yöntemi (`StatelessService.runAsync()`) çağırılır.
3. Varsa, hizmetin kendi `onOpenAsync` yöntemi çağırılır. Özellikle, `StatelessService.onOpenAsync()` çağırılır. Bu, yaygın olmayan bir geçersiz kılma, ancak kullanılabilir.

Dinleyicileri ve çağrısını oluşturma ve açma çağrısı arasında bir sıralama olmadığını unutmayın `runAsync`. Dinleyicileri başlatılmadan önce `runAsync` açılabilir. Benzer şekilde `runAsync` , iletişim dinleyicileri açılmadan önce ya da oluşturulmadan önce çağrılabilir. Herhangi bir eşitleme gerekliyse, uygulayan tarafından yapılması gerekir. Yaygın olarak karşılaşılan bazı çözümler şunlardır:

* Bazen, diğer bilgiler oluşturuluncaya veya diğer iş tamamlanana kadar dinleyiciler çalışamaz. Durum bilgisi olmayan hizmetler için bu iş genellikle hizmetin oluşturucusunda yapılabilir. `createServiceInstanceListeners()` Çağrı sırasında ya da dinleyicinin oluşturulması kapsamında yapılabilir.
* Bazen, içindeki `runAsync` kod dinleyicileri açık olana kadar başlatılmaz. Bu durumda, ek koordinasyon gereklidir. Yaygın bir çözüm, dinleyicilerde bayrak eklemektir. Bayrak, dinleyicilerinin ne zaman bittiğini gösterir. Yöntemi `runAsync` , asıl çalışmaya devam etmeden önce bunu denetler.

## <a name="stateless-service-shutdown"></a>Durum bilgisi olmayan hizmet kapatması
Durum bilgisi olmayan bir hizmetin kapatılması sırasında, aynı model izlenir, ancak tersi durumda:

1. Bu olaylar paralel olarak oluşur:
    - Açık tüm dinleyiciler kapanır. `CommunicationListener.closeAsync()`Her dinleyicide çağrılır.
    - Geçirilen `runAsync()` iptal belirteci iptal edildi. İptal belirtecinin `isCancelled` özelliğinin özelliği `true`iade edilirken ve çağrılırsa, belirtecin `throwIfCancellationRequested` yöntemi bir `CancellationException`oluşturur.
2. Her `closeAsync()` dinleyicide bittiğinde ve `runAsync()` Ayrıca tamamlandığında hizmetin `StatelessService.onCloseAsync()` yöntemi çağrılır. Bu, ortak bir geçersiz kılma değildir, ancak kaynakları güvenli bir şekilde kapatmak, arka planda işlemeyi durdurmak, dış durumu kaydetmek veya mevcut bağlantıları kapatmak için kullanılabilir.
3. Tamamlandıktan `StatelessService.onCloseAsync()` sonra, hizmet nesnesi kaldırılabilir.

## <a name="stateful-service-startup"></a>Durum bilgisi olan hizmet başlatma
Durum bilgisi olan hizmetlerde, az sayıda değişiklikle durum bilgisi olmayan hizmetlere benzer bir model vardır.  Durum bilgisi olan bir hizmeti başlatmak için olayların sırası aşağıda verilmiştir:

1. Hizmet oluşturulur.
2. `StatefulServiceBase.onOpenAsync()`çağırılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.
3. Bu olaylar paralel olarak oluşur:
    - `StatefulServiceBase.createServiceReplicaListeners()`çağrılır. 
      - Hizmet bir birincil hizmet ise, döndürülen tüm dinleyiciler açılır. `CommunicationListener.openAsync()`Her dinleyicide çağrılır.
      - Hizmet ikincil bir hizmet ise, yalnızca olarak `listenOnSecondary = true` işaretlenen dinleyiciler açılır. İkincil öğeler üzerinde açık olan dinleyicilerinin olması daha az yaygındır.
    - Hizmet şu anda birincil ise, hizmetin `StatefulServiceBase.runAsync()` yöntemi çağrılır.
4. Tüm çoğaltma dinleyicisinin `openAsync()` çağrılarından ve `runAsync()` çağrıldıktan `StatefulServiceBase.onChangeRoleAsync()` sonra çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

Durum bilgisi olmayan hizmetlere benzer şekilde, durum bilgisi olan hizmette, dinleyicilerinin oluşturulduğu ve açıldığı ve ne zaman `runAsync` çağrıldığı sırası arasında bir düzenleme yoktur. Koordine etmeniz gerekiyorsa, çözümler çok aynıdır. Ancak durum bilgisi olan hizmet için bir ek durum daha vardır. İletişim dinleyicilerine gelen çağrıların bazı [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)içinde bilgi gerektirdiğini varsayalım. İletişim dinleyicileri, güvenilir koleksiyonlar okunabilir veya yazılabilir olmadan önce açılabileceğinden ve başlamadan önce `runAsync` , bazı ek koordinasyon gereklidir. En basit ve en yaygın çözüm, iletişim dinleyicilerinin bir hata kodu döndürmesi içindir. İstemci, isteği yeniden denemeyi bilmemiz için hata kodunu kullanır.

## <a name="stateful-service-shutdown"></a>Durum bilgisi olan hizmet kapatması
Durum bilgisi olmayan hizmetler gibi, kapatılırken yaşam döngüsü olayları başlangıç sırasında ve ters çevrilme sırasında aynıdır. Durum bilgisi olan bir hizmet kapatıldığında, aşağıdaki olaylar gerçekleşir:

1. Bu olaylar paralel olarak oluşur:
    - Açık tüm dinleyiciler kapanır. `CommunicationListener.closeAsync()`Her dinleyicide çağrılır.
    - Geçirilen `runAsync()` iptal belirteci iptal edildi. İptal belirtecinin `isCancelled()` yöntemine bir çağrı döner `true`ve çağrılırsa, belirtecin `throwIfCancellationRequested()` yöntemi bir `OperationCanceledException`oluşturur.
2. Her `closeAsync()` dinleyicide bittiğinde ve `runAsync()` Ayrıca tamamlandığında hizmet `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

   > [!NOTE]  
   > `runAsync` Yalnızca bu çoğaltma birincil bir çoğaltmaysa tamamlanmasını beklemek gereklidir.

3. `StatefulServiceBase.onChangeRoleAsync()` Yöntem bittikten sonra `StatefulServiceBase.onCloseAsync()` yöntemi çağrılır. Bu çağrı yaygın olmayan bir geçersiz kılma, ancak kullanılabilir.
3. Tamamlandıktan `StatefulServiceBase.onCloseAsync()` sonra, hizmet nesnesi kaldırılabilir.

## <a name="stateful-service-primary-swaps"></a>Durum bilgisi olan hizmet birincil takas
Durum bilgisi olan bir hizmet çalışırken, iletişim dinleyicileri açılır ve `runAsync` Yöntem yalnızca durum bilgisi olan hizmetlerin birincil çoğaltmaları için çağırılır. İkincil çoğaltmalar oluşturulur, ancak daha fazla çağrı yapılmaz. Durum bilgisi olan bir hizmet çalışırken birincil olan çoğaltma değişebilir. Durum bilgisi olan bir çoğaltmanın görebileceği yaşam döngüsü olayları, değişim sırasında çoğaltmanın indirgenme veya yükseltilme olmasına bağlıdır.

### <a name="for-the-demoted-primary"></a>İndirgenen birincil için
Service Fabric, ileti işlemeyi durdurmak ve arka plan işlerini durdurmak için indirgenen birincil çoğaltmaya ihtiyaç duyuyor. Bu adım hizmetin kapatıldığı zaman ile benzerdir. Bir farklılık, hizmetin ikincil olarak kaldığı için çıkarlanmadığı veya kapatılmadığı bir farktır. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar paralel olarak oluşur:
    - Açık tüm dinleyiciler kapanır. `CommunicationListener.closeAsync()`Her dinleyicide çağrılır.
    - Geçirilen `runAsync()` iptal belirteci iptal edildi. İptal belirtecinin `isCancelled()` yönteminin döndürdüğü `true`bir denetim. Çağrılırsa, belirtecin `throwIfCancellationRequested()` yöntemi bir `OperationCanceledException`oluşturur.
2. Her `closeAsync()` dinleyicide bittiğinde ve `runAsync()` Ayrıca tamamlandığında hizmet `StatefulServiceBase.onChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

### <a name="for-the-promoted-secondary"></a>Yükseltilen ikincil için
Benzer şekilde, Service Fabric, hattaki iletileri dinlemeye başlamak için yükseltilen ikincil çoğaltmanın yanı sıra tamamlaması gereken tüm arka plan görevlerini başlatır. Bu işlem, hizmetin oluşturulduğu zaman ile benzerdir. Fark, çoğaltmanın kendisi zaten var. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar paralel olarak oluşur:
    - `StatefulServiceBase.createServiceReplicaListeners()`çağrılır ve döndürülen tüm dinleyiciler açılır. `CommunicationListener.openAsync()`Her dinleyicide çağrılır.
    - Hizmetin `StatefulServiceBase.runAsync()` yöntemi çağrılır.
2. Tüm çoğaltma dinleyicisinin `openAsync()` çağrılarından ve `runAsync()` çağrıldıktan `StatefulServiceBase.onChangeRoleAsync()` sonra çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Durum bilgisi olan hizmet kapanışı ve birincil indirgeme sırasında sık karşılaşılan sorunlar
Service Fabric, birden çok nedenden dolayı durum bilgisi olan bir hizmetin birincili olarak değiştirir. En yaygın nedenler [küme yeniden dengeleme](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltmeleridir](service-fabric-application-upgrade.md). Bu işlemler sırasında, hizmetin ' ye saygı olması önemlidir `cancellationToken`. Bu, hizmetin silinmiş olması gibi normal hizmet kapatmaları sırasında da geçerlidir.

İptali sorunsuz bir şekilde işlemeyen hizmetler, birkaç sorunla karşılaşabilir. Bu işlemler, Service Fabric hizmetlerin düzgün şekilde durdurulmasını beklediği için yavaştır. Bu, son olarak zaman aşımına uğrar ve geri alma işlemi başarısız yükseltmelere neden olabilir. İptal belirtecini karşılamamanın başarısız olmasına neden olabilir. Düğümler sık kullanıldığından, kümeler dengesiz hale gelir. Ancak, hizmetler başka bir yere taşınamayacak kadar uzun sürdüğü için yeniden dengelenemeyecek. 

Hizmetler durum bilgisi olduğundan, hizmetlerin [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)kullanması da olasıdır. Service Fabric, birincil bir indirgendiğinde, temel alınan duruma Yazma erişiminin iptal edilmesi gerekir. Bu, hizmet yaşam döngüsünü etkileyebilecek ikinci bir sorun kümesine yol açar. Koleksiyonlar, zamanlamaya göre özel durumlar, çoğaltmanın taşınıp taşınmakta veya kapatılmakta olup olmadığı için döndürülür. Bu özel durumları doğru bir şekilde işlemek önemlidir. 

Service Fabric tarafından oluşturulan özel durumlar kalıcı [(`FabricException`)](https://docs.microsoft.com/java/api/system.fabric.exception) veya geçici [(`FabricTransientException`)](https://docs.microsoft.com/java/api/system.fabric.exception.fabrictransientexception)olabilir. Kalıcı özel durumlar günlüğe kaydedilir ve oluşturulmalıdır. Geçici özel durumlar, yeniden deneme mantığına göre yeniden denenebilir.

Test ve doğrulama Reliable Services önemli bir parçası, `ReliableCollections` hizmet yaşam döngüsü olaylarıyla birlikte kullanılarak gelen özel durumları işledir. Hizmetinizi her zaman yükleme altında çalıştırmanızı öneririz. Ayrıca üretime dağıtım yapmadan önce yükseltmeler ve [Chaos testi](service-fabric-controlled-chaos.md) gerçekleştirmeniz gerekir. Bu temel adımlar, hizmetinizin doğru şekilde uygulandığından ve yaşam döngüsü olaylarını doğru şekilde işleyeceğinden emin olmanıza yardımcı olur.

## <a name="notes-on-service-lifecycle"></a>Hizmet yaşam döngüsü notları
* Hem `runAsync()` Yöntem hem de `createServiceInstanceListeners/createServiceReplicaListeners` çağrılar isteğe bağlıdır. Bir hizmette tek bir, her ikisi birden olabilir veya hiçbiri olabilir. Örneğin, hizmet tüm işini Kullanıcı çağrılarına yanıt olarak alıyorsa, uygulamanız `runAsync()`gerekmez. Yalnızca iletişim dinleyicileri ve bunlarla ilişkili kod gereklidir.  Benzer şekilde, iletişim dinleyicileri oluşturma ve döndürme isteğe bağlıdır. Hizmetin yalnızca bir arka plan işi olabilir, bu nedenle yalnızca uygulaması `runAsync()`gerekir.
* Hizmetin başarıyla tamamlanabilmesi `runAsync()` ve bundan geri dönmesi geçerlidir. Bu bir hata koşulu olarak kabul edilmez. Hizmet bitirme 'nin arka plan çalışmasını temsil eder. Durum bilgisi olan Reliable Services `runAsync()` için, hizmet birincili indirgendiyse yeniden çağrılır ve sonra birincil olarak geri yükseltilir.
* Bir hizmet beklenmeyen bir özel `runAsync()` durum oluşturarak çıkış yaparken bu bir hata oluştu. Hizmet nesnesi kapatılır ve bir sistem durumu hatası bildirilir.
* Bu yöntemlerin döndürülmesinin zaman sınırı olmasa da, anında yazma özelliğini kaybedersiniz. Bu nedenle, herhangi bir gerçek çalışmayı tamamlayamazsınız. İptal isteğini aldıktan sonra mümkün olduğunca hızlı bir şekilde dönebilmenizi öneririz. Hizmetiniz bu API çağrılarına makul bir süre içinde yanıt vermezse, Service Fabric hizmetinizi zorla sonlandırabilirler. Genellikle bu durum yalnızca uygulama yükseltmeleri sırasında veya bir hizmetin silindiği zaman gerçekleşir. Bu zaman aşımı varsayılan olarak 15 dakikadır.
* `onCloseAsync()` Yol sonucunda `onAbort()` çağrılan başarısızlıklar. Bu çağrı, hizmetin talep ettikleri tüm kaynakları temizlemesi ve serbest bırakmak için en iyi çaba bir fırsattır. Bu, genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric iç hatalardan dolayı hizmet örneğinin yaşam döngüsünü güvenilir bir şekilde yönetebilmeniz durumunda çağrılır.
* `OnChangeRoleAsync()`durum bilgisi olan hizmet çoğaltması rolü değiştirirken (örneğin, birincil veya ikincil) çağrılır. Birincil çoğaltmalara yazma durumu verilir (güvenilir koleksiyonlar oluşturma ve bunlara yazma izni verilir). İkincil çoğaltmalara okuma durumu verilir (yalnızca mevcut güvenilir koleksiyonlardan okunabilir). Durum bilgisi olan bir hizmette çoğu iş, birincil çoğaltmada gerçekleştirilir. İkincil çoğaltmalar salt okuma doğrulaması, rapor oluşturma, veri madenciliği veya diğer salt okuma işleri gerçekleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services giriş](service-fabric-reliable-services-introduction.md)
* [Hızlı başlangıç Reliable Services](service-fabric-reliable-services-quick-start-java.md)

