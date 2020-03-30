---
title: Güvenilir Hizmetlerin yaşam döngüsüne genel bakış
description: Devlet ve durum bilgisine ait hizmetler için Azure Hizmet Kumaşı Güvenilir Hizmetler uygulamasındayaşam döngüsü olayları hakkında bilgi edinin.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: fe338ca3f25cd606da7f95f6c9437a3cd3dc4e69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258284"
---
# <a name="reliable-services-lifecycle-overview"></a>Güvenilir Hizmetler yaşam döngüsüne genel bakış
> [!div class="op_single_selector"]
> * [Windows üzerinde C#](service-fabric-reliable-services-lifecycle.md)
> * [Linux üzerinde Java](service-fabric-reliable-services-lifecycle-java.md)
>
>

Azure Service Fabric Reliable Services'in yaşam döngülerini düşündüğünüzde, yaşam döngüsünün temelleri en önemlileridir. Genel olarak, yaşam döngüsü aşağıdakileri içerir:

- Başlangıç sırasında:
  - Hizmetler oluşturulur.
  - Hizmetler, sıfır veya daha fazla dinleyici oluşturma ve döndürme fırsatına sahiptir.
  - Döndürülen dinleyiciler açılır ve hizmetle iletişim sağlar.
  - Hizmetin **RunAsync** yöntemi, hizmetin uzun süren görevleri veya arka plan çalışmasını yapmasına olanak tanıyan çağrılır.
- Kapatma sırasında:
  - **RunAsync'e** geçen iptal belirteci iptal edilir ve dinleyiciler kapatılır.
  - Dinleyiciler kapandıktan sonra, servis nesnesinin kendisi destructed edilir.

Bu olayların tam sıralama etrafında ayrıntıları vardır. Olayların sırası, Güvenilir Hizmetin durumsuz veya durumlu olup olmadığına bağlı olarak biraz değişebilir. Buna ek olarak, devlet hizmetleri için, Birincil takas senaryosu ile uğraşmak gerekir. Bu sıra sırasında, Birincil rolü hizmet kapanmadan başka bir yinelemeye aktarılır (veya geri gelir). Son olarak, hata veya hata koşulları hakkında düşünmek gerekir.

## <a name="stateless-service-startup"></a>Devletsiz hizmet başlatma
Devletsiz bir hizmetin kullanım ömrü basittir. İşte olayların sırası:

1. Hizmet inşa edilmiştir.
2. Sonra, paralel olarak, iki şey olur:
    - `StatelessService.CreateServiceInstanceListeners()`çağrılır ve döndürülen dinleyiciler açılır. `ICommunicationListener.OpenAsync()`her dinleyiciye çağrılır.
    - Hizmetin `StatelessService.RunAsync()` yöntemi denir.
3. Varsa, hizmetin `StatelessService.OnOpenAsync()` yöntemi çağrılır. Bu çağrı nadir bir geçersiz kılma, ancak kullanılabilir. Genişletilmiş hizmet başlatma görevleri şu anda başlatılabilir.

Dinleyicileri ve **RunAsync'i**oluşturmak ve açmak için çağrılar arasında bir sipariş olmadığını unutmayın. **RunAsync** başlamadan önce dinleyiciler açılabilir. Benzer şekilde, iletişim dinleyicileri açılmadan veya daha da oluşturulmadan **runAsync'i** çağırabilirsiniz. Herhangi bir eşitleme gerekiyorsa, uygulayıcıya bir alıştırma olarak bırakılır. Bazı yaygın çözümler şunlardır:

  - Bazen dinleyiciler başka bilgiler oluşturulana veya iş bitene kadar çalışamaz. Devletsiz hizmetler için, bu iş genellikle aşağıdaki gibi diğer konumlarda yapılabilir: 
    - Hizmetin yapıcısında.
    - `CreateServiceInstanceListeners()` Arama sırasında.
    - Dinleyicinin kendi inşasının bir parçası olarak.
  - Bazen **RunAsync'deki** kod, dinleyiciler açılana kadar başlamaz. Bu durumda, ek koordinasyon gereklidir. Yaygın bir çözüm, dinleyiciler içinde ne zaman bitirdiklerini gösteren bir bayrak olmasıdır. Bu bayrak, fiili çalışmaya devam etmeden önce **RunAsync'de** işaretlenir.

## <a name="stateless-service-shutdown"></a>Devletsiz hizmet kapatma
Bir devletsiz hizmeti kapatmak için, aynı desen, tam tersi takip edilir:

1. Paralel olarak:
    - Açık dinleyiciler kapalıdır. `ICommunicationListener.CloseAsync()`her dinleyiciye çağrılır.
    - Geçirilen iptal belirteci iptal `RunAsync()` edilir. İptal belirteci `IsCancellationRequested` özelliğinin denetimi doğru döndürür ve çağrıldığında `ThrowIfCancellationRequested` belirteç `OperationCanceledException`yöntemi bir .
2. Her `CloseAsync()` dinleyiciyi bitirdikten `RunAsync()` ve aynı zamanda bittikten `StatelessService.OnCloseAsync()` sonra, varsa hizmetin yöntemi çağrılır.  OnCloseAsync, devletsiz hizmet örneği zarif bir şekilde kapatıldığında çağrılır. Bu durum, hizmetin kodu yükseltilirken, yük dengelemesi nedeniyle hizmet örneğinin taşınması veya geçici bir hata algılandığında oluşabilir. Geçersiz kılmak `StatelessService.OnCloseAsync()`için nadirdir, ancak kaynakları güvenli bir şekilde kapatmak, arka plan işlemini durdurmak, dış durumu kaydetmeyi bitirmek veya varolan bağlantıları kapatmak için kullanılabilir.
3. Bittikten sonra, `StatelessService.OnCloseAsync()` servis nesnesi destructed.

## <a name="stateful-service-startup"></a>Devlet hizmeti başlatma
Devlet hizmetleri, birkaç değişiklikle, devletsiz hizmetlere benzer bir modele sahiptir. Devlet hizmeti başlatmak için olayların sırası aşağıdaki gibidir:

1. Hizmet inşa edilmiştir.
2. `StatefulServiceBase.OnOpenAsync()`denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.
3. Aşağıdaki şeyler paralel olarak olur:
    - `StatefulServiceBase.CreateServiceReplicaListeners()`çağrılır. 
      - Hizmet birincil hizmetse, döndürülen tüm dinleyiciler açılır. `ICommunicationListener.OpenAsync()`her dinleyiciye çağrılır.
      - Hizmet İkincil bir hizmetse, yalnızca `ListenOnSecondary = true` bu dinleyiciler açılır olarak işaretlenir. İkincillerde açık olan dinleyicilere sahip olmak daha az yaygındır.
    - Hizmet şu anda birincil ise, `StatefulServiceBase.RunAsync()` hizmetin yöntemi çağrılır.
4. Tüm çoğaltma dinleyici `OpenAsync()` aramaları bittikten `RunAsync()` sonra ve `StatefulServiceBase.OnChangeRoleAsync()` denir, denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.

Devletsiz hizmetlere benzer şekilde, dinleyicilerin oluşturulduğu ve açıldığı sırayla **RunAsync** çağrıldığı sıra yla arasında bir koordinasyon yoktur. Koordinasyona ihtiyacınız varsa, çözümler hemen hemen aynıdır. Devlet hizmeti için ek bir servis daha vardır. İletişim dinleyicilerine gelen çağrıların bazı [Güvenilir Koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)içinde tutulan bilgileri gerektirdiğini söyleyin.

   > [!NOTE]  
   > İletişim dinleyicileri güvenilir koleksiyonlar okunabilir veya yazılabilir önce açılabilir ve **RunAsync** başlamadan önce, bazı ek koordinasyon gereklidir. En basit ve en yaygın çözüm, iletişim dinleyicileri için istemci isteği yeniden denemek için bilmek için kullandığı bir hata kodu döndürmek için.

## <a name="stateful-service-shutdown"></a>Devlet hizmeti kapatma
Durum suz hizmetler gibi, kapatma sırasındaki yaşam döngüsü olayları da başlangıç sırasındakiyle aynıdır, ancak tersine çevrilir. Devlet hizmeti kapatılırken aşağıdaki olaylar oluşur:

1. Paralel olarak:
    - Açık dinleyiciler kapalıdır. `ICommunicationListener.CloseAsync()`her dinleyiciye çağrılır.
    - Geçirilen iptal belirteci iptal `RunAsync()` edilir. İptal belirteci `IsCancellationRequested` özelliğinin denetimi doğru döndürür ve çağrıldığında `ThrowIfCancellationRequested` belirteç `OperationCanceledException`yöntemi bir .
2. Her `CloseAsync()` dinleyici üzerinde bitirir `RunAsync()` ve aynı zamanda bitirir `StatefulServiceBase.OnChangeRoleAsync()` sonra, hizmet denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.

   > [!NOTE]  
   > **RunAsync'in** tamamlanmasını bekleme gereksinimi yalnızca bu yineleme Birincil yinelemeyse gereklidir.

3. `StatefulServiceBase.OnChangeRoleAsync()` Yöntem bittikten sonra `StatefulServiceBase.OnCloseAsync()` yöntem çağrılır. Bu çağrı nadir bir geçersiz kılma, ancak kullanılabilir.
3. Bittikten sonra, `StatefulServiceBase.OnCloseAsync()` servis nesnesi destructed.

## <a name="stateful-service-primary-swaps"></a>Stateful service Birincil takaslar
Durum lu bir hizmet çalışırken, yalnızca bu durum lu hizmetlerin Birincil yinelemeleri iletişim dinleyicilerini açmış ve **RunAsync** yöntemi çağrılır. İkincil yinelemeler oluşturulur, ancak başka çağrı görmez. Durum lu bir hizmet çalışırken, şu anda Birincil olan yineleme hata veya küme dengeleme optimizasyonu sonucu değişebilir. Bu, bir yinelemenin görebileceği yaşam döngüsü olayları açısından ne anlama gelir? Durum yinelemesinin gördüğü davranış, yinelemenin değiştirme sırasında indirgenip veya tanıtılan yineleme olup olmadığına bağlıdır.

### <a name="for-the-primary-thats-demoted"></a>Düşürülen Birincil için
Düşürülen Birincil yineleme için, Hizmet Dokusunun iletileri işlemeyi durdurmak ve yaptığı arka plan çalışmasını bırakmak için bu yinelemeye ihtiyacı vardır. Sonuç olarak, bu adım hizmet kapatıldığında yapmış gibi görünür. Bir fark, hizmetin ikincil olarak kaldığı için kullanımdan önce veya kapatılmadığıdır. Aşağıdaki API'ler çağrılır:

1. Paralel olarak:
    - Açık dinleyiciler kapalıdır. `ICommunicationListener.CloseAsync()`her dinleyiciye çağrılır.
    - Geçirilen iptal belirteci iptal `RunAsync()` edilir. İptal belirteci `IsCancellationRequested` özelliğinin denetimi doğru döndürür ve çağrıldığında `ThrowIfCancellationRequested` belirteç `OperationCanceledException`yöntemi bir .
2. Her `CloseAsync()` dinleyici üzerinde bitirir `RunAsync()` ve aynı zamanda bitirir `StatefulServiceBase.OnChangeRoleAsync()` sonra, hizmet denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.

### <a name="for-the-secondary-thats-promoted"></a>Terfi eden İkincil için
Benzer şekilde, Service Fabric'in, kablodaki iletileri dinlemeye başlamak ve tamamlaması gereken arka plan görevlerini başlatmak için tanıtılan İkincil yinelemeye ihtiyacı vardır. Sonuç olarak, yinelemenin kendisi zaten var olması dışında, bu işlem hizmet oluşturulduğunda yapmış gibi görünür. Aşağıdaki API'ler çağrılır:

1. Paralel olarak:
    - `StatefulServiceBase.CreateServiceReplicaListeners()`çağrılır ve döndürülen dinleyiciler açılır. `ICommunicationListener.OpenAsync()`her dinleyiciye çağrılır.
    - Hizmetin `StatefulServiceBase.RunAsync()` yöntemi denir.
2. Tüm çoğaltma dinleyici `OpenAsync()` aramaları bittikten `RunAsync()` sonra ve `StatefulServiceBase.OnChangeRoleAsync()` denir, denir. Bu çağrı genellikle hizmette geçersiz kılınmadı.

### <a name="common-issues-during-stateful-service-shutdown-and-primary-demotion"></a>Devlet hizmeti kapatma ve Birincil indirgeme sırasında sık karşılaşılan sorunlar
Service Fabric, çeşitli nedenlerle devletli bir hizmetin Birincil'ini değiştirir. En yaygın [küme yeniden dengeleme](service-fabric-cluster-resource-manager-balancing.md) ve [uygulama yükseltme](service-fabric-application-upgrade.md)vardır. Bu işlemler sırasında (ve normal hizmet kapatma sırasında, hizmetin silinip silindiğini göreceğiniz gibi), hizmetin `CancellationToken`. 

İptal işlemlerini temiz bir şekilde işlemeyan hizmetler de çeşitli sorunlarla karşılaşabilir. Service Fabric hizmetlerin zarif bir şekilde durmasını beklediğinden, bu işlemler yavaş. Bu sonuçta başarısız yükseltmeleri yol açabilir zaman dışarı ve geri rulo. İptal belirteci nin yerine getirilmemesi de dengesiz kümelere neden olabilir. Düğümler ısındığı için kümeler dengesizleşir, ancak başka bir yere taşınması çok uzun sürdüğünden hizmetler yeniden dengelenemez. 

Hizmetler durum dolu olduğundan, [Güvenilir Koleksiyonları](service-fabric-reliable-services-reliable-collections.md)kullanmaları da olasıdır. Hizmet Kumaşı'nda, Birincil indirgendiğinde, ilk olan şeylerden biri, temel duruma yazma erişiminin iptal edilmesidir. Bu, hizmet yaşam döngüsünü etkileyebilecek ikinci bir sorun kümesine yol açar. Koleksiyonlar zamanlamaya ve yinelemenin taşınıp taşınmadığına veya kapatılmadığına bağlı olarak özel durumları döndürer. Bu özel durumlar doğru şekilde işlenmelidir. Service Fabric tarafından atılan özel durumlar kalıcı [(`FabricException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabricexception?view=azure-dotnet) ve geçici [(`FabricTransientException`)](https://docs.microsoft.com/dotnet/api/system.fabric.fabrictransientexception?view=azure-dotnet) kategorilere ayrılır. Geçici özel durumlar bazı yeniden deneme mantığına bağlı olarak yeniden denenebilirken kalıcı özel durumlar günlüğe kaydedilmeli ve atılmalıdır.

Hizmet yaşam döngüsü olayları ile `ReliableCollections` birlikte kullanımından kaynaklanan özel durumları işlemek, Güvenilir Bir Hizmeti test etmenin ve doğrulamanın önemli bir parçasıdır. Üretime başlamadan önce yükseltmeler ve [kaos testleri](service-fabric-controlled-chaos.md) gerçekleştirirken hizmetinizi her zaman yük altında çalıştırmanızı öneririz. Bu temel adımlar, hizmetinizin doğru şekilde uygulanmasını ve yaşam döngüsü olaylarını doğru şekilde işlemesine yardımcı olur.


## <a name="notes-on-the-service-lifecycle"></a>Hizmet yaşam döngüsü ile ilgili notlar
  - Hem `RunAsync()` yöntem hem `CreateServiceReplicaListeners/CreateServiceInstanceListeners` de aramalar isteğe bağlıdır. Bir hizmet, her ikisi de, ya da ikisi de bunlardan biri olabilir. Örneğin, hizmet tüm çalışmasını kullanıcı çağrılarına yanıt olarak yapıyorsa, bunu `RunAsync()`uygulamasına gerek yoktur. Yalnızca iletişim dinleyicileri ve ilişkili kodları gereklidir. Benzer şekilde, iletişim dinleyicileri oluşturma ve döndürme isteğe bağlıdır, çünkü hizmetin yalnızca arka `RunAsync()`plan çalışması olabilir ve bu nedenle yalnızca uygulanması gerekir.
  - Bir hizmetin başarıyla `RunAsync()` tamamlanması ve hizmetten dönmesi geçerlidir. Tamamlama bir hata koşulu değildir. Tamamlama, `RunAsync()` hizmetin arka plan çalışmasının tamamladığını gösterir. Stateful güvenilir hizmetler `RunAsync()` için, yineleme Birincil'den İkincil'e düşürülüp birincilliğe geri yükseltilirse yeniden çağrılır.
  - Bir hizmet beklenmeyen `RunAsync()` bir özel durum oluşturarak çıkarsa, bu bir hata oluşturur. Hizmet nesnesi kapatılır ve bir sistem durumu hatası bildirilir.
  - Bu yöntemlerden geri dönmenin bir zaman sınırı olmamasına rağmen, Güvenilir Koleksiyonlara yazma yeteneğini hemen kaybedersiniz ve bu nedenle gerçek bir çalışmayı tamamlayamazsınız. İptal talebini aldıktan sonra mümkün olan en kısa sürede geri dönmenizi tavsiye ettik. Hizmetiniz bu API çağrılarına makul bir süre içinde yanıt vermezse, Service Fabric hizmetinizi zorla sonlandırabilir. Genellikle bu yalnızca uygulama yükseltmeleri sırasında veya bir hizmet silinirken gerçekleşir. Bu zaman dilimi varsayılan olarak 15 dakikadır.
  - `OnCloseAsync()` Yoldaki hatalar `OnAbort()` çağrılmakla sonuçlanır ve bu da hizmetin iddia ettikleri kaynakları temizlemesi ve serbest bırakması için son şans için en iyi çaba fırsatıdır. Bu genellikle düğümde kalıcı bir hata algılandığında veya Service Fabric dahili hatalar nedeniyle hizmet örneğinin yaşam döngüsünü güvenilir bir şekilde yönetemediğinde çağrılır.
  - `OnChangeRoleAsync()`durumlu hizmet yinelemesi, örneğin birincil veya ikincil rol değiştirdiğinde çağrılır. Birincil yinelemelere yazma durumu verilir (Güvenilir Koleksiyonlar'a yazma ve yazma izni verilir). İkincil yinelemelere okuma durumu verilir (yalnızca varolan Güvenilir Koleksiyonlardan okunabilir). Devlet hizmetindeki çalışmaların çoğu birincil yinelemede gerçekleştirilir. İkincil yinelemeler salt okunur doğrulama, rapor oluşturma, veri madenciliği veya diğer salt okunur işleri gerçekleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar
- [Güvenilir Hizmetlere Giriş](service-fabric-reliable-services-introduction.md)
- [Güvenilir Hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
- [Çoğaltmalar ve örnekler](service-fabric-concepts-replica-lifecycle.md)
