---
title: Azure Servis Kumaş ı Güvenilir Hizmetler yaşam döngüsü
description: Stateful ve stateless hizmetleri için Java'yı kullanan Azure Hizmet Kumaşı Güvenilir Hizmetler uygulamasındaki yaşam döngüsü olayları hakkında bilgi edinin.
author: PavanKunapareddyMSFT
ms.topic: conceptual
ms.date: 06/30/2017
ms.author: pakunapa
ms.openlocfilehash: 1d3be958a0649ed3e80df2d63adbdf0b91831dbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75639607"
---
# <a name="reliable-services-lifecycle"></a>Reliable Services yaşam döngüsü
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Güvenilir Hizmetler, Azure Hizmet Kumaşı'nda bulunan programlama modellerinden biridir. Güvenilir Hizmetlerin yaşam döngüsünü öğrenirken, temel yaşam döngüsü olaylarını anlamak en önemlidir. Olayların tam sıralanması yapılandırma ayrıntılarına bağlıdır. 

Genel olarak, Güvenilir Hizmetler yaşam döngüsü aşağıdaki olayları içerir:

* Başlangıç sırasında:
  * Hizmetler oluşturulur.
  * Hizmetler, sıfır veya daha fazla dinleyici oluşturma ve döndürme fırsatına sahiptir.
  * İade edilen dinleyiciler, hizmetle iletişim için açılır.
  * Hizmetin `runAsync` yöntemi çağrılır, böylece hizmet uzun süren veya arka plan çalışması yapabilir.
* Kapatma sırasında:
  * Geçirilen iptal belirteci `runAsync` iptal edilir ve dinleyiciler kapatılır.
  * Hizmet nesnesinin kendisi destructed.

Güvenilir Hizmetlerdeki olayların sırası, güvenilir hizmetin durumsuz veya durumlu olup olmadığına bağlı olarak biraz değişebilir. 

Ayrıca, devlet hizmetleri için birincil takas senaryosunu ele almanız gerekir. Bu sıra sırasında, birincil rolü başka bir yinelemeaktarılır (veya geri geliyor) hizmet kapanmadan. 

Son olarak, hata veya hata koşulları hakkında düşünmek zorunda.

## <a name="stateless-service-startup"></a>Devletsiz hizmet başlatma
Bir devletsiz hizmetin yaşam döngüsü oldukça basittir. İşte olayların sırası:

1. Hizmet inşa edilmiştir.
2. Bu olaylar paralel olarak oluşur:
    - `StatelessService.createServiceInstanceListeners()`çağrılır ve döndürülen dinleyiciler açılır. `CommunicationListener.openAsync()`her dinleyiciye çağrılır.
    - Hizmetin `runAsync` yöntemi (`StatelessService.runAsync()`) denir.
3. Varsa, hizmetin kendi `onOpenAsync` yöntemi çağrılır. Özellikle, `StatelessService.onOpenAsync()` denir. Bu nadir bir geçersiz kılma, ancak kullanılabilir.

Dinleyicileri oluşturmak ve açmak için çağrı ile `runAsync`çağrı arasında bir sipariş olmadığını belirtmek önemlidir. Dinleyiciler başlamadan önce `runAsync` açılabilecek. Benzer şekilde, `runAsync` iletişim dinleyicileri açılmadan veya daha oluşturulmadan önce çağrılabilir. Herhangi bir eşitleme gerekiyorsa, uygulayıcı sı tarafından yapılmalıdır. Bazı yaygın çözümler şunlardır:

* Bazen dinleyiciler, diğer bilgiler oluşturulana veya başka bir iş yapılana kadar işlev göremez. Devletsiz hizmetler için, bu iş genellikle hizmetin oluşturucusunda yapılabilir. Bu `createServiceInstanceListeners()` arama sırasında yapılabilir, ya da dinleyici kendisi inşaat ın bir parçası olarak.
* Bazen dinleyiciler `runAsync` açılana kadar kod çalışmaya başlar. Bu durumda, ek koordinasyon gereklidir. Ortak bir çözüm dinleyicibir bayrak eklemektir. Bayrak, dinleyicilerin ne zaman bitirdiğini gösterir. Yöntem, `runAsync` fiili çalışmaya devam etmeden önce bunu denetler.

## <a name="stateless-service-shutdown"></a>Devletsiz hizmet kapatma
Bir devletsiz hizmeti kapatırken, aynı desen izlenir, ancak terste:

1. Bu olaylar paralel olarak oluşur:
    - Açık dinleyiciler kapalıdır. `CommunicationListener.closeAsync()`her dinleyiciye çağrılır.
    - Geçirilen iptal belirteci `runAsync()` iptal edilir. İptal belirteci `isCancelled` özelliğinin `true`iadesini denetlemek ve çağrıldığında `throwIfCancellationRequested` belirteç `CancellationException`yöntemi bir .
2. Her `closeAsync()` dinleyiciyi bitirip `runAsync()` de bitince, varsa `StatelessService.onCloseAsync()` hizmetin yöntemi çağrılır. Yine, bu yaygın bir geçersiz kılma değildir, ancak kaynakları güvenli bir şekilde kapatmak, arka plan işlemini durdurmak, dış durumu kaydetmeyi bitirmek veya varolan bağlantıları kapatmak için kullanılabilir.
3. Bittikten sonra, `StatelessService.onCloseAsync()` servis nesnesi destructed.

## <a name="stateful-service-startup"></a>Devlet hizmeti başlatma
Durum lu hizmetler, birkaç değişiklikle, devletsiz hizmetlere benzer bir desene sahiptir.  Aşağıda, devlet hizmeti başlatma etkinliklerinin sırası ve

1. Hizmet inşa edilmiştir.
2. `StatefulServiceBase.onOpenAsync()`denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.
3. Bu olaylar paralel olarak oluşur:
    - `StatefulServiceBase.createServiceReplicaListeners()`çağrılır. 
      - Hizmet birincil bir hizmetse, döndürülen tüm dinleyiciler açılır. `CommunicationListener.openAsync()`her dinleyiciye çağrılır.
      - Hizmet ikincil bir hizmetse, yalnızca `listenOnSecondary = true` dinleyiciler açıldığı gibi işaretlenir. İkincillerde açık olan dinleyicilere sahip olmak daha az yaygındır.
    - Hizmet şu anda birincil ise, `StatefulServiceBase.runAsync()` hizmetin yöntemi çağrılır.
4. Tüm çoğaltma dinleyici `openAsync()` aramaları bittikten `runAsync()` sonra ve `StatefulServiceBase.onChangeRoleAsync()` denir, denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.

Devletsiz hizmetlere benzer şekilde, devlet hizmetinde, dinleyicilerin oluşturulduğu ve açıldığı ve ne `runAsync` zaman çağrıldığı arasında bir koordinasyon yoktur. Koordinasyona ihtiyacınız varsa, çözümler hemen hemen aynıdır. Ama devlet hizmeti için bir dava daha var. İletişim dinleyicilerine gelen çağrıların bazı [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)içinde tutulan bilgileri gerektirdiğini söyleyin. Güvenilir Koleksiyonlar okunabilir veya yazılabilir önce ve başlamadan önce `runAsync` iletişim dinleyicileri açılabilir, bazı ek koordinasyon gereklidir. En basit ve en yaygın çözüm, iletişim dinleyicileri için bir hata kodu döndürmek için. İstemci, isteği yeniden denemek için hata kodunu kullanır.

## <a name="stateful-service-shutdown"></a>Devlet hizmeti kapatma
Durum suz hizmetler gibi, kapatma sırasındaki yaşam döngüsü olayları da başlangıç sırasındakiyle aynıdır, ancak tersine çevrilir. Devlet hizmeti kapatılırken aşağıdaki olaylar oluşur:

1. Bu olaylar paralel olarak oluşur:
    - Açık dinleyiciler kapalıdır. `CommunicationListener.closeAsync()`her dinleyiciye çağrılır.
    - Geçirilen iptal belirteci `runAsync()` iptal edilir. `isCancelled()` İptal belirteci yöntemine yapılan `true`bir çağrı, belirteç yöntemini döndürür ve çağrıldığında, belirteci `throwIfCancellationRequested()` yöntemi bir `OperationCanceledException`.
2. Her `closeAsync()` dinleyici üzerinde bitirir `runAsync()` ve aynı zamanda bitirir `StatefulServiceBase.onChangeRoleAsync()` sonra, hizmet denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.

   > [!NOTE]  
   > Bu `runAsync` yineleme birincil çoğaltma ise bitirmek için beklemek gereklidir.

3. `StatefulServiceBase.onChangeRoleAsync()` Yöntem bittikten sonra `StatefulServiceBase.onCloseAsync()` yöntem çağrılır. Bu çağrı nadir bir geçersiz kılma, ancak kullanılabilir.
3. Bittikten sonra, `StatefulServiceBase.onCloseAsync()` servis nesnesi destructed.

## <a name="stateful-service-primary-swaps"></a>Devlet hizmeti birincil takaslar
Durum lu bir hizmet çalışırken, iletişim dinleyicileri `runAsync` açılır ve yöntem yalnızca bu durum lu hizmetlerin birincil yinelemeleri için çağrılır. İkincil yinelemeler oluşturulur, ancak başka çağrı görmez. Bir devlet hizmeti çalışırken, şu anda birincil yineleme değişebilir. Durum lu bir yinelemenin görebileceği yaşam döngüsü olayları, değiştirme sırasında kopyanın düşürülüp düşürülmediğine veya yükseltilmesine bağlıdır.

### <a name="for-the-demoted-primary"></a>Düşüren birincil için
Service Fabric, iletileri işlemeyi durdurmak ve arka plan çalışmalarını durdurmak için indirgenmiş birincil yinelemeye ihtiyaç duyar. Bu adım, hizmetin kapatılmasına benzer. Bir fark, hizmetin ikincil olarak kalması nedeniyle kullanımdan önce veya kapatılmıyor olmasıdır. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar paralel olarak oluşur:
    - Açık dinleyiciler kapalıdır. `CommunicationListener.closeAsync()`her dinleyiciye çağrılır.
    - Geçirilen iptal belirteci `runAsync()` iptal edilir. İptal belirteci `isCancelled()` yönteminin bir `true`kontrol döndürür. Çağrılması halinde, belirteç `throwIfCancellationRequested()` yöntemi `OperationCanceledException`bir .
2. Her `closeAsync()` dinleyici üzerinde bitirir `runAsync()` ve aynı zamanda bitirir `StatefulServiceBase.onChangeRoleAsync()` sonra, hizmet denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.

### <a name="for-the-promoted-secondary"></a>Terfi eden ikincil
Benzer şekilde, Service Fabric'in kablodaki iletileri dinlemeye başlamak ve tamamlaması gereken arka plan görevlerini başlatmak için tanıtılan ikincil yinelemeye ihtiyacı vardır. Bu işlem, hizmet oluşturulduğu zamanbenzer. Fark, yinelemenin kendisinin zaten var olmasıdır. Aşağıdaki olaylar gerçekleşir:

1. Bu olaylar paralel olarak oluşur:
    - `StatefulServiceBase.createServiceReplicaListeners()`çağrılır ve döndürülen dinleyiciler açılır. `CommunicationListener.openAsync()`her dinleyiciye çağrılır.
    - Hizmetin `StatefulServiceBase.runAsync()` yöntemi denir.
2. Tüm çoğaltma dinleyici `openAsync()` aramaları bittikten `runAsync()` sonra ve `StatefulServiceBase.onChangeRoleAsync()` denir, denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Devlet hizmeti kapatma ve birincil indirgeme sırasında sık karşılaşılan sorunlar
Service Fabric, birden çok nedenden dolayı devlethizmetinin birincil birincil değiştirir. En yaygın nedenler [küme yeniden dengeleme](service-fabric-cluster-resource-manager-balancing.md) ve uygulama [yükseltme](service-fabric-application-upgrade.md)vardır. Bu işlemler sırasında, hizmetin `cancellationToken`. Bu, hizmet silinmiş gibi normal hizmet kapatma sırasında da geçerlidir.

İptal işlemlerini temiz bir şekilde işlemeyen hizmetler çeşitli sorunlarla karşılaşabilir. Service Fabric hizmetlerin zarif bir şekilde durmasını beklediğinden, bu işlemler yavaş. Bu sonuçta başarısız yükseltmeleri yol açabilir zaman dışarı ve geri alma. İptal belirteci nin yerine getirilmemesi de dengesiz kümelere neden olabilir. Düğümler ısındığı için kümeler dengesizleşir. Ancak, hizmetleri başka bir yere taşımak çok uzun sürdüğünden, hizmetler yeniden dengelendirilemez. 

Hizmetler durum dolu olduğundan, hizmetlerin [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)kullanması da olasıdır. Hizmet Kumaşı'nda, birincil bir ilke indirgendiğinde, gerçekleşen ilk şeylerden biri, temel duruma yazma erişiminin iptal edilmesidir. Bu, hizmet yaşam döngüsünü etkileyebilecek ikinci bir sorun kümesine yol açar. Koleksiyonlar zamanlamaya ve yinelemenin taşınıp taşınmadığına veya kapatılmadığına bağlı olarak özel durumları döndürer. Bu özel durumları doğru şekilde işlemek önemlidir. 

Service Fabric tarafından atılan istisnalar kalıcı [`FabricException`( )](https://docs.microsoft.com/java/api/system.fabric.exception) veya [geçicidir .`FabricTransientException`](https://docs.microsoft.com/java/api/system.fabric.exception.fabrictransientexception) Kalıcı özel durumlar günlüğe kaydedilmeli ve atılmalıdır. Geçici özel durumlar yeniden deneme mantığına göre yeniden denenebilir.

Güvenilir Hizmetleri test etme ve doğrulamanın önemli bir bölümü, `ReliableCollections` hizmet yaşam döngüsü olaylarıyla birlikte kullanılmasından kaynaklanan özel durumları işlemektir. Hizmetinizi her zaman yük altında çalıştırmanızı öneririz. Ayrıca üretime dağıtmadan önce yükseltmeleri ve [kaos testi](service-fabric-controlled-chaos.md) gerçekleştirmelisiniz. Bu temel adımlar, hizmetinizin doğru şekilde uygulanmasını ve yaşam döngüsü olaylarını doğru şekilde işlemesine yardımcı olur.

## <a name="notes-on-service-lifecycle"></a>Hizmet yaşam döngüsü ile ilgili notlar
* Hem `runAsync()` yöntem hem `createServiceInstanceListeners/createServiceReplicaListeners` de aramalar isteğe bağlıdır. Bir hizmette her ikisi de olabilir veya ikisi birden olabilir. Örneğin, hizmet tüm çalışmasını kullanıcı çağrılarına yanıt olarak yapıyorsa, bunu `runAsync()`uygulamasına gerek yoktur. Yalnızca iletişim dinleyicileri ve ilişkili kodları gereklidir.  Benzer şekilde, iletişim dinleyicileri oluşturma ve döndürme isteğe bağlıdır. Hizmetin yalnızca arka plan çalışması olabilir, bu `runAsync()`nedenle yalnızca uygulanması gerekir.
* Bir hizmetin başarıyla tamamlanması `runAsync()` ve servisten geri dönmesi için geçerlidir. Bu bir hata durumu olarak kabul edilir. Bu hizmet bitirme arka plan çalışmasını temsil eder. Durumlu Güvenilir Hizmetler `runAsync()` için, hizmet birincilden düşürülüp birincil liğe yükseltilirse yeniden çağrılır.
* Bir hizmet beklenmeyen `runAsync()` bir özel durum oluşturarak çıkarsa, bu bir hatadır. Hizmet nesnesi kapatılır ve bir sistem durumu hatası bildirilir.
* Bu yöntemlerden dönmenin bir zaman sınırı olmamasına rağmen, yazma yeteneğini hemen kaybedersiniz. Bu nedenle, herhangi bir gerçek işi tamamlayamaz. İptal talebini aldıktan sonra mümkün olan en kısa sürede dönmenizi öneririz. Hizmetiniz bu API çağrılarına makul bir süre içinde yanıt vermezse, Service Fabric hizmetinizi zorla sonlandırabilir. Genellikle, bu yalnızca uygulama yükseltmeleri sırasında veya bir hizmet silinirken gerçekleşir. Bu zaman dilimi varsayılan olarak 15 dakikadır.
* Yoldaki `onCloseAsync()` hatalar çağrılmasına `onAbort()` neden oluyor. Bu çağrı, hizmetin iddia ettikleri kaynakları temizlemesi ve serbest bırakması için son şans, en iyi çaba fırsatıdır. Bu genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric dahili hatalar nedeniyle hizmet örneğinin yaşam döngüsünü güvenilir bir şekilde yönetemediğinde çağrılır.
* `OnChangeRoleAsync()`durumlu hizmet yinelemesi, örneğin birincil veya ikincil rol değiştirdiğinde çağrılır. Birincil yinelemelere yazma durumu verilir (Güvenilir Koleksiyonlar'a yazma ve yazma izni verilir). İkincil yinelemelere okuma durumu verilir (yalnızca varolan Güvenilir Koleksiyonlardan okunabilir). Devlet hizmetindeki çalışmaların çoğu birincil yinelemede gerçekleştirilir. İkincil yinelemeler salt okunur doğrulama, rapor oluşturma, veri madenciliği veya diğer salt okunur işleri gerçekleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Hizmetlere Giriş](service-fabric-reliable-services-introduction.md)
* [Güvenilir Hizmetler hızlı başlat](service-fabric-reliable-services-quick-start-java.md)

