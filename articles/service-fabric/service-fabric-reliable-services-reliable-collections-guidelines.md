---
title: Güvenilir Koleksiyonlar Için Yönergeler
description: Hizmet Kumaşı Güvenilir Koleksiyonları bir Azure Hizmet Kumaşı uygulamasında kullanma yönergeleri ve Önerileri.
ms.topic: conceptual
ms.date: 12/10/2017
ms.openlocfilehash: 37c734205877f9e0cb98ef2834462691e8e483d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75645489"
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
* GeçersizOperasyonException'ı işleyin. Kullanıcı hareketleri çeşitli nedenlerle sistem tarafından iptal edilebilir. Örneğin, Güvenilir Durum Yöneticisi birincil rolünü değiştirirken veya uzun süren bir işlem işlem günlüğünün kesilmesini engelliyorsa. Bu gibi durumlarda, kullanıcı, işlemlerinin zaten sonlandırıldığını belirten Geçersiz Operasyon Özel Durum alabilir. İşlemin sonlandırılmasının kullanıcı tarafından talep edilmediğini varsayarsak, bu istisnayı işleetmenin en iyi yolu hareketi elden çıkmak, iptal belirteci sinyalinin alınıp sinyal alınmamasını kontrol etmek (veya yinelemenin rolü değiştirildi) ve yeni bir işlem ve yeniden deneme.  

Akılda tutulması gereken bazı şeyler şunlardır:

* Varsayılan zaman-out tüm Güvenilir Toplama API'leri için dört saniyedir. Çoğu kullanıcı varsayılan zaman ayarı kullanmalıdır.
* Varsayılan iptal belirteci tüm Güvenilir KoleksiyonAPI'lerindedir. `CancellationToken.None`
* Güvenilir Sözlük için anahtar türü parametresi *(TKey)* doğru şekilde uygulanmalıdır `GetHashCode()` ve. `Equals()` Anahtarlar değişmez olmalı.
* Güvenilir Koleksiyonlar için yüksek kullanılabilirlik elde etmek için, her hizmetin en az bir hedef ve en az çoğaltma kümesi boyutu 3 olmalıdır.
* İkincil işlemler, çoğunluk taahhüdünde olmayan sürümleri okuyabilir.
  Bu, tek bir ikincil den okunan verilerin bir sürümünün yanlış ilerletilmiş olabileceği anlamına gelir.
  Birincil okumalar her zaman kararlıdır: yanlış ilerletilmiş asla.
* Uygulamanızın güvenilir bir koleksiyonda ısrar ettiği verilerin güvenliği/Gizliliği sizin kararınızdır ve depolama yönetiminiz tarafından sağlanan korumalara tabidir; Yani. İşletim Sistemi disk şifrelemesi, verilerinizi istirahatte korumak için kullanılabilir.  

### <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [İşlemler ve Kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Verileri Yönetme
  * [Yedekleme ve Geri Yükleme](service-fabric-reliable-services-backup-restore.md)
  * [Bildirimler](service-fabric-reliable-services-notifications.md)
  * [Serileştirme ve Yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir Durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* Diğer
  * [Güvenilir Hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
  * [Güvenilir Koleksiyonlar için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
