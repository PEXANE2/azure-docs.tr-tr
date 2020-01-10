---
title: Reliable Services yaşam döngüsüne genel bakış
description: Durum bilgisiz ve durum bilgisi olmayan hizmetler için Azure Service Fabric Reliable Services uygulamasındaki yaşam döngüsü olayları hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: fe338ca3f25cd606da7f95f6c9437a3cd3dc4e69
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75645506"
---
# <a name="reliable-services-lifecycle-overview"></a>Reliable Services yaşam döngüsüne genel bakış
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Azure Service Fabric Reliable Services yaşam döngüsünün ne zaman yaşadığınızı düşünürken yaşam döngüsünün temelleri en önemli öneme sahiptir. Genel olarak, yaşam döngüsü şunları içerir:

- Başlangıç sırasında:
  - Hizmetler oluşturulur.
  - Hizmetler, sıfır veya daha fazla dinleyici oluşturma ve döndürme fırsatına sahiptir.
  - Döndürülen tüm dinleyiciler, hizmetle iletişime izin verecek şekilde açılır.
  - Hizmetin **RunAsync** yöntemi çağrılır ve hizmetin uzun süre çalışan görevler veya arka plan çalışması yapmasına izin verir.
- Kapatılırken:
  - **RunAsync** öğesine geçirilen iptal belirteci iptal edilir ve dinleyiciler kapalıdır.
  - Dinleyiciler kapatıldıktan sonra, hizmet nesnesinin kendisi parçalanmıştır.

Bu olayların tam sıralaması hakkında ayrıntılı bilgi vardır. Olayların sırası, güvenilir hizmetin durum bilgisiz veya durum bilgisiz olmasına bağlı olarak biraz değişebilir. Ayrıca, durum bilgisi olan hizmetler için, birincil takas senaryosuyla uğraşmalıdır. Bu sıra sırasında, birincil rolü başka bir çoğaltmaya aktarılır (veya geri gelir) hizmet kapatılmadan önce. Son olarak, hata veya hata koşullarını düşünmemiz gerekir.

## <a name="stateless-service-startup"></a>Durum bilgisi olmayan hizmet başlangıcı
Durum bilgisi olmayan bir hizmetin yaşam döngüsü basittir. Olayların sırası aşağıda verilmiştir:

1. Hizmet oluşturulur.
2. Ardından, paralel olarak iki şey meydana gelir:
    - `StatelessService.CreateServiceInstanceListeners()` çağrılır ve döndürülen tüm dinleyiciler açılır. `ICommunicationListener.OpenAsync()` her dinleyicide çağrılır.
    - Hizmetin `StatelessService.RunAsync()` yöntemi çağrılır.
3. Varsa, hizmetin `StatelessService.OnOpenAsync()` yöntemi çağrılır. Bu çağrı yaygın olmayan bir geçersiz kılma, ancak kullanılabilir. Genişletilmiş hizmet başlatma görevleri şu anda başlatılabilir.

Dinleyicileri ve **RunAsync**'yi oluşturma ve açma çağrıları arasında bir sıralama olmadığını unutmayın. **RunAsync** başlatılmadan önce dinleyiciler açılabilir. Benzer şekilde, iletişim dinleyicileri açık veya oluşturulmuş olmadan önce **RunAsync** 'i çağırabilirsiniz. Herhangi bir eşitleme gerekliyse, uygulayıcısı için bir alıştırma olarak kalır. Yaygın olarak karşılaşılan bazı çözümler şunlardır:

  - Bazı durumlarda, bazı bilgiler oluşturuluncaya veya iş tamamlanana kadar dinleyiciler çalışamaz. Durum bilgisi olmayan hizmetler için, bu iş genellikle aşağıdaki gibi diğer konumlarda yapılabilir: 
    - Hizmetin oluşturucusunda.
    - `CreateServiceInstanceListeners()` çağrısı sırasında.
    - Dinleyicinin kendisini oluşturma bir parçası olarak.
  - Bazen **RunAsync** içindeki kod, dinleyiciler açık olana kadar başlamaz. Bu durumda, ek koordinasyon gereklidir. Yaygın olarak karşılaşılan bir çözüm, dinleyicilerde ne zaman bittiğini gösteren bir bayrak olduğu anlamına gelir. Bu bayrak daha sonra, fiili çalışmaya devam etmeden önce **RunAsync** içinde denetlenir.

## <a name="stateless-service-shutdown"></a>Durum bilgisi olmayan hizmet kapatması
Durum bilgisi olmayan bir hizmeti kapatmak için, yalnızca ters ' de olmak üzere aynı model izlenir:

1. Paralel:
    - Açık tüm dinleyiciler kapanır. `ICommunicationListener.CloseAsync()` her dinleyicide çağrılır.
    - `RunAsync()` geçilen iptal belirteci iptal edildi. İptal belirtecinin `IsCancellationRequested` özelliğinin bir denetimi true döndürür ve çağrılırsa, belirtecin `ThrowIfCancellationRequested` yöntemi bir `OperationCanceledException`oluşturur.
2. Her dinleyicide `CloseAsync()` tamamlandıktan sonra `RunAsync()` da tamamlandığında, varsa hizmetin `StatelessService.OnCloseAsync()` yöntemi çağrılır.  Durum bilgisi olmayan hizmet örneği düzgün şekilde kapatıcaksında OnCloseAsync çağrılır. Bu durum hizmetin kodu yükseltildiğinde, hizmet örneğinin yük dengelemesi nedeniyle taşınması ya da geçici bir hata algılanıyorsa meydana gelir. `StatelessService.OnCloseAsync()`geçersiz kılmak, ancak kaynakları güvenli bir şekilde kapatmak, arka planda işlemeyi durdurmak, dış durum kaydetmeyi durdurmak veya mevcut bağlantıları kapatmak için kullanılabilir.
3. `StatelessService.OnCloseAsync()` tamamlandıktan sonra, hizmet nesnesi kaldırılabilir.

## <a name="stateful-service-startup"></a>Durum bilgisi olan hizmet başlatma
Durum bilgisi olan hizmetlerin, az sayıda değişiklikle durum bilgisi olmayan hizmetlere benzer bir kalıbı vardır. Durum bilgisi olan bir hizmeti başlatmak için olayların sırası aşağıdaki gibidir:

1. Hizmet oluşturulur.
2. `StatefulServiceBase.OnOpenAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.
3. Paralel olarak aşağıdaki şeyler gerçekleşir:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` çağrılır. 
      - Hizmet bir birincil hizmet ise, döndürülen tüm dinleyiciler açılır. `ICommunicationListener.OpenAsync()` her dinleyicide çağrılır.
      - Hizmet Ikincil bir hizmet ise, yalnızca `ListenOnSecondary = true` olarak işaretlenen dinleyiciler açılır. İkincil öğeler üzerinde açık olan dinleyicilerinin olması daha az yaygındır.
    - Hizmet şu anda birincil ise, hizmetin `StatefulServiceBase.RunAsync()` yöntemi çağrılır.
4. Tüm çoğaltma dinleyicisinin `OpenAsync()` sonuna çağrı yapıldıktan sonra `RunAsync()` çağrıldığında `StatefulServiceBase.OnChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

Durum bilgisi olmayan hizmetlere benzer şekilde, dinleyicilerinin oluşturulduğu ve açıldığı sırada ve **RunAsync** çağrıldığında bir düzenleme yoktur. Koordine etmeniz gerekiyorsa, çözümler çok aynıdır. Durum bilgisi olan hizmet için bir ek durum daha vardır. İletişim dinleyicilerine gelen çağrıların bazı [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)içinde bilgi gerektirdiğini varsayalım.

   > [!NOTE]  
   > İletişim dinleyicileri güvenilir koleksiyonlar okunabilir veya yazılabilir olmadan önce açılabilir ve **RunAsync** başlamadan önce, bazı ek koordinasyon gereklidir. En basit ve en yaygın çözüm, iletişim dinleyicilerinin, isteği yeniden denemek için istemcinin kullandığı bir hata kodu döndürmesi içindir.

## <a name="stateful-service-shutdown"></a>Durum bilgisi olan hizmet kapatması
Durum bilgisi olmayan hizmetler gibi, kapatılırken yaşam döngüsü olayları başlangıç sırasında ve ters çevrilme sırasında aynıdır. Durum bilgisi olan bir hizmet kapatıldığında, aşağıdaki olaylar gerçekleşir:

1. Paralel:
    - Açık tüm dinleyiciler kapanır. `ICommunicationListener.CloseAsync()` her dinleyicide çağrılır.
    - `RunAsync()` geçilen iptal belirteci iptal edildi. İptal belirtecinin `IsCancellationRequested` özelliğinin bir denetimi true döndürür ve çağrılırsa, belirtecin `ThrowIfCancellationRequested` yöntemi bir `OperationCanceledException`oluşturur.
2. Her dinleyicide `CloseAsync()` tamamlandıktan sonra ve `RunAsync()` tamamlandığında hizmetin `StatefulServiceBase.OnChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

   > [!NOTE]  
   > **RunAsync** işleminin bitmesini beklemeniz gereken yalnızca, bu çoğaltma birincil bir çoğaltmaysa gereklidir.

3. `StatefulServiceBase.OnChangeRoleAsync()` yöntemi bittikten sonra, `StatefulServiceBase.OnCloseAsync()` yöntemi çağrılır. Bu çağrı yaygın olmayan bir geçersiz kılma, ancak kullanılabilir.
3. `StatefulServiceBase.OnCloseAsync()` tamamlandıktan sonra, hizmet nesnesi kaldırılabilir.

## <a name="stateful-service-primary-swaps"></a>Durum bilgisi olan hizmet birincil takas
Durum bilgisi olan bir hizmet çalışırken, yalnızca bu durum bilgisi olan hizmetlerin birincil çoğaltmalarının iletişim dinleyicileri açık ve **RunAsync** yöntemi çağırılır. İkincil çoğaltmalar oluşturulur, ancak daha fazla çağrı yapılmaz. Durum bilgisi olan bir hizmet çalışırken, birincil olan çoğaltma, hata veya küme Dengeleme iyileştirmesinin sonucu olarak değişebilir. Bu, bir çoğaltmanın görebilecekleri yaşam döngüsü olayları açısından ne anlama geliyor? Durum bilgisi olan çoğaltmanın gördüğü davranış, değişim sırasında çoğaltmanın indirgenme veya yükseltilme olmasına bağlıdır.

### <a name="for-the-primary-thats-demoted"></a>İndirgenen birincil için
İndirgenen birincil çoğaltma için Service Fabric, bu çoğaltmanın ileti işlemeyi durdurmasına ve yaptığı tüm arka planda çalışmaya çıkmasına ihtiyaç duyuyor. Sonuç olarak, bu adım hizmet kapatılırken olduğu gibi görünüyor. Bir farklılık, hizmetin Ikincil olarak kaldığı için çıkarılması veya kapatılmadığı bir farktır. Aşağıdaki API 'Ler çağrılır:

1. Paralel:
    - Açık tüm dinleyiciler kapanır. `ICommunicationListener.CloseAsync()` her dinleyicide çağrılır.
    - `RunAsync()` geçilen iptal belirteci iptal edildi. İptal belirtecinin `IsCancellationRequested` özelliğinin bir denetimi true döndürür ve çağrılırsa, belirtecin `ThrowIfCancellationRequested` yöntemi bir `OperationCanceledException`oluşturur.
2. Her dinleyicide `CloseAsync()` tamamlandıktan sonra ve `RunAsync()` tamamlandığında hizmetin `StatefulServiceBase.OnChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

### <a name="for-the-secondary-thats-promoted"></a>Yükseltilen Ikincil için
Benzer şekilde, Service Fabric, hattaki iletileri dinlemeye başlamak ve tamamlaması gereken tüm arka plan görevlerini başlatmak için yükseltilen Ikincil çoğaltmanın olması gerekir. Sonuç olarak, bu işlem hizmetin oluşturulduğu sırada olduğu gibi görünür, ancak çoğaltmanın kendisi de vardır. Aşağıdaki API 'Ler çağrılır:

1. Paralel:
    - `StatefulServiceBase.CreateServiceReplicaListeners()` çağrılır ve döndürülen tüm dinleyiciler açılır. `ICommunicationListener.OpenAsync()` her dinleyicide çağrılır.
    - Hizmetin `StatefulServiceBase.RunAsync()` yöntemi çağrılır.
2. Tüm çoğaltma dinleyicisinin `OpenAsync()` sonuna çağrı yapıldıktan sonra `RunAsync()` çağrıldığında `StatefulServiceBase.OnChangeRoleAsync()` çağrılır. Bu çağrı, hizmette genellikle geçersiz kılınmaz.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Durum bilgisi olan hizmet kapanışı ve birincil indirgeme sırasında sık karşılaşılan sorunlar
Service Fabric, çok çeşitli nedenlerle durum bilgisi olan bir hizmetin birincili olarak değiştirir. En yaygın olarak, [küme yeniden dengeleme](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltme](service-fabric-application-upgrade.md)bulunur. Bu işlemler sırasında (hizmetin silinip silinmediğini gördüğünüz gibi), hizmetin `CancellationToken`uygun olması önemlidir. 

İptali sorunsuz bir şekilde işlemeyen hizmetler, birkaç sorunla karşılaşabilir. Bu işlemler, Service Fabric hizmetlerin düzgün şekilde durdurulmasını beklediği için yavaştır. Bu, sonunda zaman aşımına uğrar ve geri dönerek başarısız olan yükseltmelere neden olabilir. İptal belirtecini karşılamamak, ayrıca imale kümelerin oluşmasına neden olabilir. Düğümler çalışır durumda olduğundan kümeler dengesiz hale gelir, ancak başka bir yere taşınamayacak kadar uzun sürdüğü için hizmetler yeniden dengelenemeyecek. 

Hizmetler durum bilgisi olduğundan, [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md)da kullanmaları olasıdır. Service Fabric, birincil bir indirgendiğinde, temel alınan duruma Yazma erişiminin iptal edilmesi gerekir. Bu, hizmet yaşam döngüsünü etkileyebilecek ikinci bir sorun kümesine yol açar. Koleksiyonlar, zamanlamaya göre özel durumlar, çoğaltmanın taşınıp taşınmakta veya kapatılmakta olup olmadığı için döndürülür. Bu özel durumlar doğru şekilde işlenmelidir. Service Fabric tarafından oluşturulan özel durumlar kalıcı [(`FabricException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) ve geçici [(`FabricTransientException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) kategorilere ayrılır. Geçici özel durumlar, bazı yeniden deneme mantığına göre yeniden denenirken kalıcı özel durumlar günlüğe kaydedilir ve oluşturulmalıdır.

Hizmet yaşam döngüsü olaylarıyla birlikte `ReliableCollections` kullanımı ile gelen özel durumların işlenmesi, güvenilir bir hizmetin sınanması ve doğrulanması önemli bir parçasıdır. Üretime dağıtım yapmadan önce yükseltmeler ve [Chaos testi](service-fabric-controlled-chaos.md) gerçekleştirirken hizmetinizi her zaman yükleme altında çalıştırmanızı öneririz. Bu temel adımlar, hizmetinizin doğru şekilde uygulandığından ve yaşam döngüsü olaylarını doğru bir şekilde işleymesinin sağlanmasına yardımcı olur.


## <a name="notes-on-the-service-lifecycle"></a>Hizmet yaşam döngüsünün notları
  - Hem `RunAsync()` yöntemi hem de `CreateServiceReplicaListeners/CreateServiceInstanceListeners` çağrıları isteğe bağlıdır. Bir hizmette, her ikisi birden olabilir veya hiçbiri olabilir. Örneğin, hizmet tüm işini Kullanıcı çağrılarına yanıt olarak alıyorsa, `RunAsync()`uygulaması için gerekli değildir. Yalnızca iletişim dinleyicileri ve bunlarla ilişkili kod gereklidir. Benzer şekilde, iletişim dinleyicilerinin oluşturulması ve döndürülmesi isteğe bağlıdır, çünkü hizmet yalnızca bir arka plan çalışmasına sahip olabilir ve bu nedenle yalnızca `RunAsync()`uygulaması gerekir.
  - Bir hizmetin başarıyla `RunAsync()` tamamlaması ve bundan geri dönmesi geçerlidir. Tamamlama bir hata koşulu değildir. `RunAsync()` tamamlamak, hizmetin arka plan işinin tamamlandığını gösterir. Durum bilgisi olan güvenilir hizmetler için, çoğaltma birincili Ikinciye indirgendiyse ve sonra birinciye yükseltilerek `RunAsync()` yeniden çağrılır.
  - Bir hizmet beklenmeyen bir özel durum vererek `RunAsync()` 'tan çıkılırken bu bir hata oluşturur. Hizmet nesnesi kapatılmış ve bir sistem durumu hatası bildirildi.
  - Bu yöntemlerin döndürülmesinin zaman sınırı olmasa da, güvenilir koleksiyonlara yazma özelliğini hemen kaybedersiniz ve bu nedenle gerçek bir çalışmayı tamamlayamazsınız. İptal isteğini aldıktan sonra mümkün olduğunca hızlı bir şekilde geri dönmenize tavsiye ederiz. Hizmetiniz bu API çağrılarına makul bir süre içinde yanıt vermezse, Service Fabric hizmetinizi zorla sonlandırabilirler. Genellikle bu yalnızca uygulama yükseltmeleri sırasında veya bir hizmet silindiğinde gerçekleşir. Bu zaman aşımı varsayılan olarak 15 dakikadır.
  - `OnCloseAsync()` yolundaki başarısızlıklar, hizmetin talep ettikleri tüm kaynakları temizlemesi ve serbest bırakmak için en iyi çaba bir fırsat olan `OnAbort()` çağrılmasıyla sonuçlanır. Bu, genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric iç hatalardan dolayı hizmet örneğinin yaşam döngüsünü güvenilir bir şekilde yönetebilmeniz durumunda çağrılır.
  - `OnChangeRoleAsync()`, durum bilgisi olan hizmet çoğaltması rolü değiştirirken (örneğin, birincil veya ikincil) çağrılır. Birincil çoğaltmalara yazma durumu verilir (güvenilir koleksiyonlar oluşturma ve bunlara yazma izni verilir). İkincil çoğaltmalara okuma durumu verilir (yalnızca mevcut güvenilir koleksiyonlardan okunabilir). Durum bilgisi olan bir hizmette çoğu iş, birincil çoğaltmada gerçekleştirilir. İkincil çoğaltmalar salt okuma doğrulaması, rapor oluşturma, veri madenciliği veya diğer salt okuma işleri gerçekleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
- [Reliable Services giriş](service-fabric-reliable-services-introduction.md)
- [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
- [Çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md)
