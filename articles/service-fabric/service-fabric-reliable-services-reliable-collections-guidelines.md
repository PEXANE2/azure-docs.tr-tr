---
title: Güvenilir Koleksiyonlar Için Yönergeler
description: Hizmet Kumaşı Güvenilir Koleksiyonları bir Azure Hizmet Kumaşı uygulamasında kullanma yönergeleri ve Önerileri.
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: db37067069b2a9eb08009eb6bb373f6fce1cafa9
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81398534"
---
# <a name="guidelines-and-recommendations-for-reliable-collections-in-azure-service-fabric"></a>Azure Hizmet Kumaşında Güvenilir Koleksiyonlar için yönergeler ve öneriler
Bu bölümde, Güvenilir Devlet Yöneticisi ve Güvenilir Koleksiyonlar'ın kullanılmasına ilişkin yönergeler verilmektedir. Amaç, kullanıcıların yaygın tuzaklardan kaçınmalarına yardımcı olmaktır.

Kurallar, *Do*, *Göz at,* *Kaçın* ve *Yapma*" terimleri ile önceden belirlenmiş basit öneriler olarak düzenlenir.

* Okuma işlemleriyle döndürülen özel türdeki bir nesneyi değiştirmeyin (örneğin, `TryPeekAsync` veya). `TryGetValueAsync` Eşzamanlı Koleksiyonlar gibi Güvenilir Koleksiyonlar, bir kopya değil nesnelere bir başvuru döndürün.
* Değiştirmeden önce özel bir türün döndürülen nesnesini derin kopyalayın. Yapı ve yerleşik türler geçiş değeri olduğundan, değiştirmek istediğiniz referans türüne sahip alanlar veya özellikler içermedikçe, bunlar üzerinde derin bir kopya yapmanız gerekmez.
* Zaman zaman `TimeSpan.MaxValue` ları kullanmayın. Zaman aşımları kilitlenmeleri algılamak için kullanılmalıdır.
* Bir hareketi işlendikten, iptal edildikten veya elden çıkarıldıktan sonra kullanmayın.
* Oluşturulduğu işlem kapsamı nın dışında numaralandırma yapmayın.
* Kilitlenmelere neden olabileceğinden, `using` başka bir işlemin ekstresi içinde bir hareket oluşturmayın.
* Aynı işlemde güvenilir `IReliableStateManager.GetOrAddAsync` durum la güvenilir bir durum oluşturmayın ve güvenilir durumu kullanmayın. Bu, Bir Geçersiz Çalışma Özel Durum ile sonuçlanır.
* Uygulamanızın `IComparable<TKey>` doğru olduğundan emin olun. Sistem, denetim noktalarını ve satırları birleştirmek `IComparable<TKey>` için bağımlılık alır.
* Belirli bir açmazı önlemek için bir öğeyi güncelleştirmek amacıyla bir öğeyi okurken Güncelleştir kilidi kullanın.
* Bölüm başına Güvenilir Koleksiyon sayısını 1000'den az tutmayı düşünün. Daha az öğeyle daha güvenilir koleksiyonlara göre daha fazla öğeiçeren Güvenilir Koleksiyonlar'ı tercih edin.
* Öğelerinizi (örneğin, TKey + TValue for Reliable Dictionary) 80 KByte'un altında tutmayı düşünün: daha küçük türde. Bu, Büyük Nesne Yığını kullanımının yanı sıra disk ve ağ IO gereksinimlerini azaltır. Genellikle, değerin yalnızca küçük bir bölümü güncelleştirildiğinde yinelenen verileri çoğaltmayı azaltır. Güvenilir Sözlük'te bunu başarabilmek için ortak yol, satırlarınızı birden çok satıra ayırmaktır.
* Olağanüstü durum kurtarma için yedekleme ve geri yükleme işlevlerini kullanmayı düşünün.
* Farklı yalıtım düzeyleri nedeniyle tek varlık işlemlerini ve `GetCountAsync` `CreateEnumerableAsync`çok varlıklı işlemleri (örn. ) aynı işlemde karıştırmaktan kaçının.
* GeçersizOperasyonException'ı işleyin. Kullanıcı hareketleri çeşitli nedenlerle sistem tarafından iptal edilebilir. Örneğin, Güvenilir Durum Yöneticisi birincil rolünü değiştirirken veya uzun süren bir işlem işlem günlüğünün kesilmesini engelliyorsa. Bu gibi durumlarda, kullanıcı, işlemlerinin zaten sonlandırıldığını belirten Geçersiz Operasyon Özel Durum alabilir. İşlemin sonlandırılmasının kullanıcı tarafından istenmediğini varsayarsak, bu istisnayı işlemenin en iyi yolu hareketi elden çıkarmak, iptal jetonunun sinyal olup olmadığını (veya yinelemenin rolünün değiştirilip değiştirilmediğini) kontrol etmek ve yeni bir işlem oluşturup yeniden denememektir.  

Akılda tutulması gereken bazı şeyler şunlardır:

* Varsayılan zaman-out tüm Güvenilir Toplama API'leri için dört saniyedir. Çoğu kullanıcı varsayılan zaman ayarı kullanmalıdır.
* Varsayılan iptal belirteci tüm Güvenilir KoleksiyonAPI'lerindedir. `CancellationToken.None`
* Güvenilir Sözlük için anahtar türü parametresi *(TKey)* doğru şekilde uygulanmalıdır `GetHashCode()` ve. `Equals()` Anahtarlar değişmez olmalı.
* Güvenilir Koleksiyonlar için yüksek kullanılabilirlik elde etmek için, her hizmetin en az bir hedef ve en az çoğaltma kümesi boyutu 3 olmalıdır.
* İkincil işlemler, çoğunluk taahhüdünde olmayan sürümleri okuyabilir.
  Bu, tek bir ikincil den okunan verilerin bir sürümünün yanlış ilerletilmiş olabileceği anlamına gelir.
  Birincil okumalar her zaman kararlıdır: yanlış ilerletilmiş asla.
* Uygulamanızın güvenilir bir koleksiyonda ısrar ettiği verilerin güvenliği/Gizliliği sizin kararınızdır ve depolama yönetiminiz tarafından sağlanan korumalara tabidir; Yani. İşletim Sistemi disk şifrelemesi, verilerinizi istirahatte korumak için kullanılabilir.  

## <a name="volatile-reliable-collections"></a>Uçucu Güvenilir Koleksiyonlar
Geçici güvenilir koleksiyonları kullanmaya karar verirken aşağıdakileri göz önünde bulundurun:

* ```ReliableDictionary```uçucu desteği var
* ```ReliableQueue```uçucu desteği var
* ```ReliableConcurrentQueue```uçucu desteği YOK
* Kalıcı hizmetler uçucu yapılamaz. Bayrağı ```HasPersistedState``` sıfırdan ```false``` tüm hizmeti yeniden oluşturmayı gerektirir şekilde değiştirme
* Uçucu hizmetler kalıcı yapılamaz. Bayrağı ```HasPersistedState``` sıfırdan ```true``` tüm hizmeti yeniden oluşturmayı gerektirir şekilde değiştirme
* ```HasPersistedState```bir hizmet düzeyi config olduğunu. Bu, **TÜM** koleksiyonların kalıcı veya geçici olacağı anlamına gelir. Uçucu ve kalıcı koleksiyonları karıştıramazsınız
* Geçici bir bölümün çoğunluk kaybı, tam veri kaybına neden
* Yedekleme ve geri yükleme geçici hizmetler için KULLANıLABILIR DeğİlDIR

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [İşlemler ve Kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Verileri Yönetme
  * [Yedekleme ve Geri Yükleme](service-fabric-reliable-services-backup-restore.md)
  * [Bildirimler](service-fabric-reliable-services-notifications.md)
  * [Serileştirme ve Yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir Durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* Diğer
  * [Güvenilir Hizmetler hızlı başlat](service-fabric-reliable-services-quick-start.md)
  * [Güvenilir Koleksiyonlar için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
