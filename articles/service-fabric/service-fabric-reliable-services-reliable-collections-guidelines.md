---
title: Güvenilir koleksiyonlar için yönergeler
description: Azure Service Fabric uygulamasında güvenilir Service Fabric koleksiyonları kullanmaya yönelik yönergeler ve öneriler.
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: db37067069b2a9eb08009eb6bb373f6fce1cafa9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81398534"
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Azure Service Fabric güvenilir koleksiyonlar için yönergeler ve öneriler
Bu bölüm, güvenilir durum Yöneticisi ve güvenilir koleksiyonlar kullanımına ilişkin yönergeler sağlar. Amaç, kullanıcıların ortak bir şekilde kaçınmasına yardımcı olmaktır.

Yönergeler *, koşullara göre*ön koşul sunan basit öneriler olarak düzenlenir, *göz önünde bulundurun*, *önleyin* ve *Not*edin.

* Okuma işlemleri tarafından döndürülen özel türdeki bir nesneyi değiştirmeyin (örneğin, `TryPeekAsync` veya `TryGetValueAsync`). Eşzamanlı Koleksiyonlar gibi güvenilir koleksiyonlar, bir kopya değil, nesnelere bir başvuru döndürür.
* Değişiklik yapmadan önce özel bir türün döndürülen nesnesini derin olarak kopyalayın. Yapılar ve yerleşik türler değere göre geçiş yaptığından, bunları değiştirmek istediğiniz başvuru türü alanları veya özellikleri içermedikleri takdirde, bunlarda derin bir kopya yapmanız gerekmez.
* Zaman aşımları için `TimeSpan.MaxValue` kullanmayın. Kilitlenmeleri algılamak için zaman aşımları kullanılmalıdır.
* İşlem kaydedildikten, durdurulduktan veya atıldıktan sonra bir işlem kullanmayın.
* İçinde oluşturulduğu işlem kapsamı dışında bir numaralandırma kullanmayın.
* Kilitlenmeleri neden olabileceğinden, başka bir işlemin `using` deyimindeki bir işlem oluşturmayın.
* İle `IReliableStateManager.GetOrAddAsync` güvenilir durum oluşturmayın ve aynı işlemde güvenilir durumu kullanın. Bu, bir InvalidOperationException ile sonuçlanır.
* `IComparable<TKey>` Uygulamanızın doğru olduğundan emin olun. Sistem, denetim noktaları ve `IComparable<TKey>` satırları birleştirmek için bağımlılığı alır.
* Belirli bir kilitlenme sınıfına engel olmak için bir öğeyi güncelleştirmek amacıyla bir öğe okurken güncelleştirme kilidi kullanın.
* Bölüm başına güvenilir koleksiyonların sayısını 1000 ' den küçük olacak şekilde tutmayı göz önünde bulundurun. Daha az öğe ile daha güvenilir koleksiyonlar üzerinde daha fazla öğe ile güvenilir koleksiyonlar tercih edin.
* Öğelerinizi (örneğin, güvenilir sözlük için TKey + TValue) aşağıdaki 80 Kbayt: daha az küçültün. Bu, büyük nesne yığın kullanımının yanı sıra disk ve ağ GÇ gereksinimlerinin miktarını azaltır. Genellikle, değerin yalnızca bir kısmı güncelleştirilirken yinelenen verilerin çoğaltılmasını azaltır. Bunu güvenilir sözlükte elde etmenin yaygın yolu, satırlarınızı birden çok satıra bölmek olur.
* Olağanüstü durum kurtarma için yedekleme ve geri yükleme işlevlerini kullanmayı düşünün.
* Tek bir varlık işlemlerini ve çoklu varlık işlemlerini (örn `GetCountAsync`. `CreateEnumerableAsync`) farklı yalıtım düzeyleri nedeniyle aynı işlemde karıştırmaktan kaçının.
* InvalidOperationException 'yi işleyin. Kullanıcı işlemleri, çeşitli nedenlerle sistem tarafından iptal edilebilir. Örneğin, güvenilir durum Yöneticisi rolünü birincil dışında değiştirirken veya uzun süre çalışan bir işlem işlem günlüğünün kesilmesinin engellendiği zaman. Bu gibi durumlarda, Kullanıcı işleminin zaten sonlandırıldığı belirten InvalidOperationException alabilir. Varsayıldığında, işlem sonlandırmasının Kullanıcı tarafından istenmediğini, bu özel durumu işlemenin en iyi yolu işlemi atmak, iptal belirtecinin sinyal (veya çoğaltmanın rolü değiştirilmiş) olup olmadığını kontrol etmek ve yeni bir işlem oluşturup yeniden denenmemesi durumunda.  

Göz önünde bulundurmanız gereken bazı şeyler aşağıda verilmiştir:

* Varsayılan zaman aşımı, tüm güvenilir koleksiyon API 'Leri için dört saniyedir. Çoğu kullanıcının varsayılan zaman aşımını kullanması gerekir.
* Varsayılan iptal belirteci, tüm `CancellationToken.None` güvenilir koleksiyonlar API 'lerinde bulunur.
* Güvenilir bir sözlük için anahtar türü parametresi (*TKey*), ve ' `GetHashCode()` `Equals()`nin doğru bir şekilde uygulanması gerekir. Anahtarlar sabit olmalıdır.
* Güvenilir koleksiyonlar için yüksek kullanılabilirlik elde etmek üzere her hizmetin en az bir hedefi ve en düşük çoğaltma kümesi boyutu 3 ' ü içermelidir.
* İkincil üzerinde okuma işlemleri, çekirdek kaydedilmemiş sürümleri okuyabilir.
  Bu, tek bir ikincil sunucudan okunan bir veri sürümünün yanlış ilerlemedi olabileceği anlamına gelir.
  Birincili okuma her zaman kararlı değildir: hiçbir zaman yanlış ilerlemedi olamaz.
* Güvenilir bir koleksiyonda uygulamanız tarafından kalıcı olan verilerin güvenlik/gizliliği, sizin kararınız ve depolama yönetimi tarafından sağlanmış olan korumalardan tabidir; Yani. İşletim sistemi disk şifrelemesi, verilerinizi bekleyen bir şekilde korumak için kullanılabilir.  

## <a name="volatile-reliable-collections"></a>Geçici güvenilir Koleksiyonlar
Geçici güvenilir koleksiyonları kullanmaya karar verirken aşağıdakileri göz önünde bulundurun:

* ```ReliableDictionary```geçici desteğe sahiptir
* ```ReliableQueue```geçici desteğe sahiptir
* ```ReliableConcurrentQueue```geçici desteğe sahip DEĞIL
* Kalıcı hizmetler geçici yapılamaz. Bayrağını değiştirme ```HasPersistedState``` , tüm ```false``` hizmetin sıfırdan yeniden oluşturulmasını gerektiriyor
* Geçici hizmetler kalıcı yapılamıyor. Bayrağını değiştirme ```HasPersistedState``` , tüm ```true``` hizmetin sıfırdan yeniden oluşturulmasını gerektiriyor
* ```HasPersistedState```, bir hizmet düzeyi yapılandırması. Bu, **Tüm** koleksiyonların kalıcı ya da geçici olacağı anlamına gelir. Geçici ve kalıcı koleksiyonları karıştırılamaz
* Geçici bir bölümün çekirdek kaybı, veri kaybına neden olur
* Yedekleme ve geri yükleme geçici hizmetler için kullanılamaz

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [İşlemler ve kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Verileri yönetme
  * [Yedekleme ve geri yükleme](service-fabric-reliable-services-backup-restore.md)
  * [Bildirimler](service-fabric-reliable-services-notifications.md)
  * [Serileştirme ve yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* Diğer
  * [Hızlı başlangıç Reliable Services](service-fabric-reliable-services-quick-start.md)
  * [Güvenilir koleksiyonlar için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
