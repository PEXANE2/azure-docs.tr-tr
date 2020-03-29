---
title: Azure Hizmet Kumaşı'nda devlet hizmetlerini test etme
description: Birim test Hizmeti Kumaş Stateful Services kavramları ve uygulamaları hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 09/04/2018
ms.openlocfilehash: 12e8a47d9685dee12594f4e2afaa848d9688d185
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75433921"
---
# <a name="unit-testing-stateful-services-in-service-fabric"></a>Hizmet Kumaşı'nda devlet hizmetlerini test etme birimi

Bu makalede, birim test Hizmet Kumaş Stateful Services kavramları ve uygulamaları kapsar. Hizmet Kumaşı içindeki birim testi, uygulama kodunun birden çok farklı bağlam altında etkin bir şekilde çalıştırışla gerçekleştirilmesi nedeniyle kendi değerlendirmelerini hak eder. Bu makalede, uygulama kodunun çalıştırabileceği farklı bağlamların her birinin altında yer alan uygulamaları açıklar.

## <a name="unit-testing-and-mocking"></a>Birim testi ve alay etme
Bu makalebağlamında birim testi, MSTest veya NUnit gibi bir test koşucusu bağlamında yürütülebilen otomatik testtir. Bu makaledeki birim testleri, veritabanı veya RESTFul API gibi uzak bir kaynağa karşı işlem gerçekleştirmez. Bu uzak kaynaklarla alay edilmeli. Bu makalebağlamında alay etmek, uzak kaynakların iade değerlerini taklit edecek, kaydeder ve denetler.

### <a name="service-fabric-considerations"></a>Servis Kumaş ı hususlar
Birim test bir Hizmet Kumaş stateful hizmet çeşitli hususlar vardır. İlk olarak, hizmet kodu birden çok düğümde ancak farklı roller altında yürütülür. Birim testleri tam kapsama elde etmek için her rolü n altında kodu değerlendirmek gerekir. Farklı roller Birincil, Etkin İkincil, Boşta İkincil ve Bilinmeyen olacaktır. Hizmet Kumaşı bu rolü geçersiz veya geçersiz hizmet olarak gördüğü için, None rolü genellikle özel bir kapsama gerektirmez. İkinci olarak, her düğüm herhangi bir noktada rolünü değiştirir. Tam kapsama elde etmek için, kod yürütme yolunun rol değişiklikleri meydana gelen sınanması gerekir.

## <a name="why-unit-test-stateful-services"></a>Neden birim test stateful hizmetleri? 
Birim test durumlu hizmetleri mutlaka geleneksel uygulama veya etki alanına özgü birim testi tarafından yakalanmış olmaz yapılan bazı yaygın hataları ortaya çıkarmak için yardımcı olabilir. Örneğin, durum bilgisi hizmetinin bellek durumu varsa, bu tür bir sınama, bu bellek içi durumunun her yineleme de eşit tutulduğunu doğrulayabilir. Bu tür sınama, hizmet veren bir hizmetin Service Fabric orkestrasyonunun uygun şekilde geçtiği iptal belirteçlerine yanıt verdiğini de doğrulayabilir. İptaller tetiklendiğinde, hizmet uzun süren ve/veya eşzamanlı işlemleri durdurmalıdır.  

## <a name="common-practices"></a>Yaygın uygulamalar

Aşağıdaki bölümde, birim olarak kullanılan bir hizmeti test etmek için en yaygın uygulamalar hakkında tavsiyelerde bulunmaktadır. Ayrıca, alaycı bir katmanın Service Fabric orkestrasyon ve devlet yönetimine yakın bir şekilde hizalanması gerektiğini de önerir. [ServiceFabric.Mocks](https://www.nuget.org/packages/ServiceFabric.Mocks/) 3.3.0 veya daha sonra olarak önerilen alay işlevselliği sağlayan ve aşağıda özetlenen uygulamaları izleyen tür kütüphane biridir.

### <a name="arrangement"></a>Düzenleme

#### <a name="use-multiple-service-instances"></a>Birden çok hizmet örneği kullanma
Birim testleri, birden çok durumlu hizmet örneğini yürütmelidir. Bu, Service Fabric'in hizmetinizi farklı düğümler arasında çalıştıran birden çok yinelemeyi karşıladığı kümede gerçekte ne olduğunu simüle eder. Ancak, bu örneklerin her biri farklı bir bağlam altında yürütülecektir. Testi çalıştırırken, her örnek kümede beklenen rol yapılandırması ile astarlanmalıdır. Örneğin, hizmetin hedef yineleme boyutu3 olması bekleniyorsa, Hizmet Kumaşı farklı düğümlerde üç yineleme sağlar. Biri birincil, diğeri Aktif İkincil'dir.

Çoğu durumda, hizmet yürütme yolunun ki bu rollerin her biri için biraz değişir. Örneğin, hizmet etkin bir ikincil istekkabul etmiyorsa, hizmetin ikincil bir istek denendiğini gösteren bilgilendirici bir özel durumu geri atmak için bu servis için bir denetimi olabilir. Birden çok örneğin olması bu durumun sınanmasını sağlar.

Ayrıca, birden çok örneğin olması, testlerin rol değişikliklerine rağmen yanıtların tutarlı olduğunu doğrulamak için bu örneklerin her birinin rollerini değiştirmesine olanak tanır.

#### <a name="mock-the-state-manager"></a>Devlet müdürüyle alay et
Devlet Yöneticisi uzak bir kaynak olarak ele alınmalıdır ve bu nedenle alay. Devlet yöneticisiyle alay ederken, okunup doğrulanabilmesi için devlet yöneticisine kaydedilenleri izlemek için bazı temel bellek içi depolama alanı olması gerekir. Bunu başarabilmek için basit bir yol, Güvenilir Koleksiyon türlerinin her birinin sahte örnekleri oluşturmaktır. Bu mocks içinde, bu koleksiyona karşı gerçekleştirilen işlemleri ile yakından uyumlu bir veri türü kullanın. Aşağıda, her güvenilir koleksiyon için önerilen bazı veri türleri verilmiştir

- IReliableDictionary<TKey, TValue> -> System.Collections.Concurrent.ConcurrentDictionary<TKey, TValue>
- IReliableQueue\<T> -> System.Collections.Generic.Queue\<T>
- IReliableConcurrentQueue\<T> -> System.Collections.Concurrent.ConcurrentQueue\<T>

#### <a name="many-state-manager-instances-single-storage"></a>Birçok Devlet Yöneticisi Örnekleri, tek depolama
Daha önce de belirtildiği gibi, Devlet Yöneticisi ve Güvenilir Koleksiyonlar uzak bir kaynak olarak ele alınmalıdır. Bu nedenle, bu kaynaklar birim testleri içinde alay edilmelidir ve olacaktır. Ancak, birden çok durumlu hizmet örneği çalıştırırken, alay edilen her bir devlet yöneticisinin farklı durum lu hizmet örnekleri arasında eşittutulması zor olacaktır. Durum hizmeti kümeüzerinde çalışırken, Hizmet Dokusu her ikincil yinelemenin durum yöneticisini birincil yinelemeyle tutarlı tutmayı önemser. Bu nedenle, rol değişikliklerini simüle edebilmeleri için testlerin aynı şekilde olması gerekir.

Bu eşitlemenin elde edilebilmiş basit bir yolu, her Güvenilir Koleksiyon'a yazılan verileri depolayan temel nesne için bir singleton deseni kullanmaktır. Örneğin, devlet hizmeti bir `IReliableDictionary<string, string>`. Sahte devlet müdürü bir alay `IReliableDictionary<string, string>`iade etmelidir . Bu mock, `ConcurrentDictionary<string, string>` yazılan anahtar/değer çiftlerini izlemek için a'yı kullanabilir. Hizmete `ConcurrentDictionary<string, string>` geçirilen devlet yöneticilerinin tüm örnekleri tarafından kullanılan bir singleton olmalıdır.

#### <a name="keep-track-of-cancellation-tokens"></a>İptal belirteçlerini takip edin
İptal belirteçleri, devlet hizmetlerinin önemli ama yaygın olarak gözden kaçan bir yönüdur. Service Fabric, devlet hizmeti için birincil çoğaltma başlattığında, bir iptal belirteci sağlanır. Bu iptal belirteci, hizmet kaldırıldığında veya farklı bir role düşürüldüğünde hizmete sinyal vermek için tasarlanmıştır. Hizmet Kumaşı'nın rol değişikliği iş akışını tamamlayabilmesi için, devlet hizmeti uzun süren veya eşzamanlı işlemleri durdurmalıdır.

Birim testleri çalıştırılırken, Test yürütme sırasında RunAsync, ChangeRoleAsync, OpenAsync ve CloseAsync'e sağlanan iptal belirteçleri tutulmalıdır. Bu belirteçlere tutunmak, testin bir hizmet kapatmaveya indirgeme simülasyonunu simüle etmesine ve hizmetin uygun şekilde yanıt verdiğini doğrulamasına olanak sağlar.

#### <a name="test-end-to-end-with-mocked-remote-resources"></a>Alay edilen uzak kaynaklarla uçuca test etme
Birim testleri, devlet hizmetinin durumunu mümkün olduğunca değiştirebilecek uygulama kodunun çoğunu yürütmelidir. Testlerin doğada uçtan uca olması önerilir. Var olan tek alay, uzak kaynak etkileşimlerini kaydetmek, simüle etmek ve/veya doğrulamaktır. Bu, Devlet Yöneticisi ve Güvenilir Koleksiyonlar ile etkileşimleri içerir. Aşağıdaki parçacık, uçtan uca testi gösteren bir test için gherkin örneğidir:

```
    Given stateful service named "fabric:/MyApp/MyService" is created
    And a new replica is created as "Primary" with id "111"
    And a new replica is created as "IdleSecondary" with id "222"
    And a new replica is created as "IdleSecondary" with id "333"
    And all idle secondary replicas are promoted to active secondary
    When a request is made to add the an employee "John Smith"
    And the active secondary replica "222" is promoted to primary
    And a request is made to get all employees
    Then the request should should return the "John Smith" employee
```

Bu test, bir yinelemede yakalanan verilerin birincil yinelemeye yükseltildiğinde ikincil bir yineleme için kullanılabilir olduğunu ileri sürmektedir. Güvenilir bir koleksiyonun çalışan verileri için destek deposu olduğunu varsayarsak, uygulama kodunun yeni çalışanı kaydetmek `CommitAsync` için hareketi yürütmemesi durumunda, bu testle yakalanabilecek Aa olası hatasıdır. Bu durumda, çalışanları almak için ikinci istek ilk istek tarafından eklenen çalışan iade olmaz.

### <a name="acting"></a>Davranı
#### <a name="mimic-service-fabric-replica-orchestration"></a>Mimic Service Kumaş çoğaltma orkestrasyonu
Birden çok hizmet örneğini yönetirken, testler bu hizmetleri Service Fabric düzenlemesiyle aynı şekilde başlatmalı ve yıkmalıdır. Örneğin, yeni bir birincil yinelemede bir hizmet oluşturulduğunda, Hizmet Dokusu CreateServiceReplicaListener, OpenAsync, ChangeRoleAsync ve RunAsync'i çağırır. Yaşam döngüsü olayları aşağıdaki makalelerde belgelenmiştir:

- [Stateful Service Startup](service-fabric-reliable-services-lifecycle.md#stateful-service-startup)
- [Stateful Service Shutdown](service-fabric-reliable-services-lifecycle.md#stateful-service-shutdown)
- [Devlet Hizmeti Birincil Swaps](service-fabric-reliable-services-lifecycle.md#stateful-service-primary-swaps)

#### <a name="run-replica-role-changes"></a>Yineleme rol değişikliklerini çalıştırma
Birim testleri, Hizmet Kumaşı düzenlemesiile aynı şekilde hizmet örneklerinin rollerini değiştirmelidir. Rol durumu makinesi aşağıdaki makalede belgelenmiştir:

[Çoğaltma Rol Durum Makinesi](service-fabric-concepts-replica-lifecycle.md#replica-role)

Rol değişikliklerinin simüle edilmesi, sınamanın en kritik yönlerinden biridir ve yinelemenin durumunun birbiriyle tutarlı olmadığı sorunları ortaya çıkarabilir. Bellek durumu statik veya sınıf düzeyi örnek değişkenlerinde depolama nedeniyle tutarsız yineleme durumu oluşabilir. Buna örnek olarak iptal belirteçleri, enumlar ve yapılandırma nesneleri/değerleri verilebilir. Bu, hizmetin rol değişikliğinin gerçekleşmesine izin vermek için RunAsync sırasında sağlanan iptal belirteçlerine saygı göstermesini de sağlar. Rol değişikliklerinin simüle edilmesi, RunAsync'in birden çok kez çağrılması için kod yazılmazsa ortaya çıkabilecek sorunları da ortaya çıkarabilirsiniz.

#### <a name="cancel-cancellation-tokens"></a>İptal belirteçlerini iptal etme
RunAsync'e sağlanan iptal belirteci iptal edildiği birim testleri olmalıdır. Bu, testin hizmetin düzgün bir şekilde kapandığını doğrulamasına olanak sağlar. Bu kapatma sırasında uzun süren veya eşzamanlı işlemler durdurulmalıdır. Bir hizmette var olabilecek uzun süren bir işlem örneği, Güvenilir Sıra'daki iletileri dinleyen bir işlemdir. Bu doğrudan RunAsync veya bir arka plan iş parçacığı içinde bulunabilir. Bu iptal belirteci iptal edilirse, uygulama operasyondan çıkma mantığını içermelidir.

Devlet hizmetleri yalnızca birincil üzerinde bulunması gereken herhangi bir önbellek veya bellek durumu kullanırsanız, bu anda bertaraf edilmelidir. Bu, düğüm daha sonra birincil hale gelirse bu durum tutarlı olduğundan emin olmaktır. İptal testi, testin bu durumu doğru bir şekilde bertaraf edilmesini doğrulamasına olanak sağlar.

#### <a name="execute-requests-against-multiple-replicas"></a>Birden çok yinelemeye karşı istekleri yürütme
Assert testleri farklı yineleme karşı aynı isteği yürütmek gerekir. Rol değişiklikleriyle eşleştirildiğinde, tutarlılık sorunları ortaya çıkarılabilir. Örnek bir test aşağıdaki adımları gerçekleştirebilir:
1. Geçerli birincil karşı bir yazma isteği yürütme
2. 1. adımda yazılan verileri geçerli birincil eteklerine karşı döndüren bir okuma isteği yürütme
3. Birincil bir ikincil teşvik edin. Bu da ikincil geçerli birincil düşürmek gerekir
4. Yeni ikincil karşı adım 2 aynı okuma isteği yürütmek.

Son adımda, test döndürülen verilerin tutarlı olduğunu ileri sürebilir. Bunun ortaya çıkarabileceği olası bir sorun, hizmet tarafından döndürülen verilerin bellekte olması, ancak sonuçta güvenilir bir koleksiyon tarafından desteklenen olmasıdır. Bellek içi veriler, güvenilir koleksiyonda bulunanlarla düzgün bir şekilde eşit tutulmayabilir.

Bellek içi veriler genellikle güvenilir bir koleksiyonda bulunan ikincil dizinler veya veri toplamaları oluşturmak için kullanılır.

### <a name="asserting"></a>Iddia
#### <a name="ensure-responses-match-across-replicas"></a>Yanıtların yinelemeler arasında eşleştirilmesini sağlama
Birim testleri, belirli bir istek için verilen yanıtın birincil egeçtikten sonra birden çok yineleme arasında tutarlı olduğunu ileri sürmelidir. Bu, yanıtta sağlanan verilerin güvenilir bir koleksiyon tarafından yedeklenmediği veya bu verileri yinelemeler arasında eşitlemek için bir mekanizma olmadan bellekte tutulduğu olası sorunları ortaya çıkarabilir. Bu, Hizmet Kumaşı yeniden dengeledikten veya yeni bir birincil yinelemeye geçemedikten sonra hizmetin tutarlı yanıtları geri göndermesini sağlar.

#### <a name="verify-service-respects-cancellation"></a>Hizmetin iptale saygı gösterin
İptal jetonu iptal edildiğinde sonlandırılması gereken uzun süren veya eşzamanlı işlemler, iptal edildikten sonra fiilen sonlandırıldıkları doğrulanmalıdır. Bu, yineleme değiştirme rollerine rağmen, geçiş tamamlanmadan önce birincil olmayan yineleme durağında çalışmaya devam etmek için tasarlanmayan işlemlerin olmasını sağlar. Bu, böyle bir işlemin Hizmet Kumaşı'ndan gelen rol değiştirme veya kapatma isteğinin tamamlanmasını engellediği sorunları da ortaya çıkarabilir.

#### <a name="verify-which-replicas-should-serve-requests"></a>Hangi yinelemelerin isteklere hizmet etmesi gerektiğini doğrulayın
Bir istek birincil olmayan bir yinelemeye yönlendirilirse, testler beklenen davranışı ileri sürmelidir. Service Fabric, ikincil yinelemelerin isteklere hizmet etmesini sağlar. Ancak, güvenilir koleksiyonlara yazdığı yazmayalnızca birincil yinelemeden oluşabilir. Uygulamanız yalnızca istekleri sunmak için birincil yinelemeler için niyetinde yse veya yalnızca bir istek alt kümesi ikincil tarafından ele alınabilirse, testler hem olumlu hem de negatif durumlar için beklenen davranışı öne sürmelidir. İstek olan negatif durum, isteği işlememesi gereken bir yinelemeye yönlendirilir ve pozitif olan tersi.

## <a name="next-steps"></a>Sonraki adımlar
Durum hizmetlerini nasıl [birleştirin](service-fabric-how-to-unit-test-stateful-services.md)öğrenin.
