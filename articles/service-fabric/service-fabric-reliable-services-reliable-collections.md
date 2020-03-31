---
title: Güvenilir Koleksiyonlara Giriş
description: Service Fabric stateful hizmetleri, yüksek kullanılabilir, ölçeklenebilir ve düşük gecikmeli bulut uygulamaları yazmanızı sağlayan güvenilir koleksiyonlar sağlar.
ms.topic: conceptual
ms.date: 1/3/2019
ms.openlocfilehash: 48fa682f4c017f66911729e1f581f3aa91cdc28d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75609732"
---
# <a name="introduction-to-reliable-collections-in-azure-service-fabric-stateful-services"></a>Azure Hizmet Kumaşı'nda Güvenilir Koleksiyonlara Giriş

Güvenilir Koleksiyonlar, tek bilgisayar uygulamaları yazıyormuş gibi yüksek kullanılabilir, ölçeklenebilir ve düşük gecikmeli bulut uygulamaları yazmanızı sağlar. **Microsoft.ServiceFabric.Data.Collections** ad alanındaki sınıflar, durumunuzu otomatik olarak kullanılabilir hale getiren bir koleksiyon kümesi sağlar. Geliştiricilerin yalnızca Güvenilir Koleksiyon API'lerine programlamaları ve Güvenilir Koleksiyonların çoğaltılan ve yerel durumu yönetmesine izin vermeleri gerekir.

Güvenilir Koleksiyonlar ve diğer yüksek kullanılabilirlik teknolojileri (Redis, Azure Table hizmeti ve Azure Kuyruk hizmeti gibi) arasındaki temel fark, devletin hizmet örneğinde yerel olarak tutulurken aynı zamanda yüksek oranda kullanılabilir hale getirilmiş olmasıdır. Bu şu anlama gelir:

* Tüm okumalar yereldir, bu da düşük gecikme gecikmesi ve yüksek iş bölümü okuması ile sonuçlanır.
* Tüm yazlar, düşük gecikme ve yüksek iş bölümü yazmaile sonuçlanan minimum ağ iOs sayısına neden olan en az sayıda dır.

![Koleksiyonların evrimimimim.](media/service-fabric-reliable-services-reliable-collections/ReliableCollectionsEvolution.png)

Güvenilir Koleksiyonlar **System.Collections** sınıflarının doğal evrimi olarak düşünülebilir: geliştirici için karmaşıklığı artırmadan bulut ve çoklu bilgisayar uygulamaları için tasarlanmış yeni bir koleksiyon kümesi. Bu nedenle, Güvenilir Koleksiyonlar şunlardır:

* Çoğaltılır: Durum değişiklikleri yüksek kullanılabilirlik için çoğaltılır.
* Devam eden: Veriler, büyük ölçekli kesintilere (örneğin, bir veri merkezi elektrik kesintisi) karşı dayanıklılık için diske kalıcıdır.
* Yazmalar kalıcı olduğundan ve çoğaltıldığından, yalnızca bellekte veri kalıcı olan geçici bir ReliableDictionary, ReliableQueue veya diğer güvenilir bir koleksiyon oluşturamazsınız.
* Asynchronous: API'ler, IO'ya maruz kalırken iş parçacıklarının engellenmemesini sağlamak için eşzamanlıdır.
* İşlemsel: API'ler, bir hizmet içinde birden çok Güvenilir Koleksiyonu kolayca yönetebilmeniz için işlemlerin soyutlanmasını kullanır.

Güvenilir Koleksiyonlar, uygulama durumu yla ilgili muhakeme yi kolaylaştırmak için kutunun dışında güçlü tutarlılık garantileri sağlar.
Güçlü tutarlılık, işlemin yalnızca tüm işlemin birincil yinelemeler de dahil olmak üzere çoğunluk çoğunluğu üzerinde günlüğe kaydedildikten sonra tamamlanmasını sağlayarak elde edilir.
Daha zayıf tutarlılık elde etmek için, uygulamalar asynchronous commit returns önce istemci / istekte geri kabul edebilirsiniz.

Güvenilir KoleksiyonAPI'leri eşzamanlı koleksiyon API'lerinin bir evrimidir **(System.Collections.Concurrent** namespace'te bulunur):

* Eş zamanlı koleksiyonlardan farklı olarak, işlemler çoğaltıldığından ve kalıcı olduğundan bir görevi döndürür.
* Çıkış parametreleri `ConditionalValue<T>` yok: `bool` Out parametreleri yerine bir ve bir değeri döndürmek için kullanılır. `ConditionalValue<T>`gibi `Nullable<T>` ama bir yapı olması T gerektirmez.
* Hareketler: Kullanıcının bir hareketteki birden çok Güvenilir Koleksiyondaki eylemleri gruplandırmasını sağlamak için bir hareket nesnesi kullanır.

Bugün, **Microsoft.ServiceFabric.Data.Collections** üç koleksiyon içerir:

* [Güvenilir Sözlük](https://msdn.microsoft.com/library/azure/dn971511.aspx): Anahtar/değer çiftlerinin çoğaltılmış, işlemsel ve eşzamanlı bir koleksiyonunu temsil eder. **EşzamanlıSözlük**benzer , hem anahtar ve değer herhangi bir tür olabilir.
* [Güvenilir Sıra](https://msdn.microsoft.com/library/azure/dn971527.aspx): Çoğaltılmış, işlemsel ve eşzamanlı bir ilk-in, ilk çıkan (FIFO) sırayı temsil eder. Eşzamanlı **Queue**benzer, değer herhangi bir türde olabilir.
* [Güvenilir Eşzamanlı Sıra](service-fabric-reliable-services-reliable-concurrent-queue.md): Yüksek iş için çoğaltılmış, işlemsel ve eşzamanlı en iyi çaba sıralama sırasını temsil eder. **Eşzamanlı Queue'ye**benzer şekilde, değer herhangi bir türde olabilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenilir Toplama Yönergeleri & Öneriler](service-fabric-reliable-services-reliable-collections-guidelines.md)
* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [İşlemler ve Kilitler](service-fabric-reliable-services-reliable-collections-transactions-locks.md)
* Verileri Yönetme
  * [Yedekleme ve Geri Yükleme](service-fabric-reliable-services-backup-restore.md)
  * [Bildirimler](service-fabric-reliable-services-notifications.md)
  * [Güvenilir Koleksiyonu serileştirme](service-fabric-reliable-services-reliable-collections-serialization.md)
  * [Serileştirme ve Yükseltme](service-fabric-application-upgrade-data-serialization.md)
  * [Güvenilir Durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* Diğer
  * [Güvenilir Hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
  * [Güvenilir Koleksiyonlar için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
