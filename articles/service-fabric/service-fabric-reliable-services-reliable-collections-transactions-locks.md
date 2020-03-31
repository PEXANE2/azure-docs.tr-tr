---
title: Güvenilir Koleksiyonlarda İşlemler ve Kilit Modları
description: Azure Servis Kumaşı Güvenilir Devlet Yöneticisi ve Güvenilir Koleksiyon İşlemleri ve Kilitleme.
ms.topic: conceptual
ms.date: 5/1/2017
ms.custom: sfrev
ms.openlocfilehash: 5f7b3a4d43d35f0d2965dd33c8f69143f4b3a8f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76938920"
---
# <a name="transactions-and-lock-modes-in-azure-service-fabric-reliable-collections"></a>Azure Hizmet Kumaşı Güvenilir Koleksiyonlarında Işlemler ve kilit modları

## <a name="transaction"></a>İşlem

Hareket, tek bir mantıksal çalışma birimi olarak gerçekleştirilen işlem dizisidir. Veritabanı işlemlerinin ortak [ASİt](https://en.wikipedia.org/wiki/ACID) *(atomiklik,* *tutarlılık,* *izolasyon,* *dayanıklılık)* özelliklerini sergiler:

* **Atomiklik**: Bir işlem atomik bir iş birimi olmalıdır. Başka bir deyişle, tüm veri değişiklikleri gerçekleştirilir veya bunların hiçbiri gerçekleştirilmemektedir.
* **Tutarlılık**: Tamamlandığında, bir işlem tüm verileri tutarlı bir durumda bırakmalıdır. Tüm iç veri yapıları işlemin sonunda doğru olmalıdır.
* **İzolasyon**: Eşzamanlı işlemlerle yapılan değişiklikler, diğer eşzamanlı işlemler tarafından yapılan değişikliklerden izole edilmelidir. [Bir ITransaction](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.itransaction?view=azure-dotnet) içinde bir işlem için kullanılan yalıtım düzeyi, işlemi gerçekleştiren [IReliableState](https://docs.microsoft.com/dotnet/api/microsoft.servicefabric.data.ireliablestate?view=azure-dotnet) tarafından belirlenir.
* **Dayanıklılık**: Bir işlem tamamlandıktan sonra, etkileri sistemde kalıcı olarak yer alır. Değişiklikler, sistem hatası durumunda bile devam eder.

### <a name="isolation-levels"></a>Yalıtım düzeyleri

Yalıtım düzeyi, hareketin diğer hareketler tarafından yapılan değişikliklerden ne derece yalıtılması gerektiğini tanımlar.
Güvenilir Koleksiyonlarda desteklenen iki yalıtım düzeyi vardır:

* **Yinelenebilir Okuma**: İfadelerin değiştirilmiş ancak henüz başka işlemler tarafından işlenmemiş verileri okuyamadığını ve geçerli işlem bitene kadar geçerli işlem tarafından okunan verileri başka hiçbir işlemin değiştiremeyeceğini belirtir.
* **Anlık Görüntü**: Bir hareketteki herhangi bir ekstre tarafından okunan verilerin, hareketin başlangıcında var olan verilerin işlem açısından tutarlı sürümü olduğunu belirtir.
  Hareket, yalnızca işlem başlamadan önce işlenen veri değişikliklerini tanıyabilir.
  Geçerli hareket in başlamasından sonra diğer hareketler tarafından yapılan veri değişiklikleri, geçerli harekette çalıştırılatan ekstreler tarafından görünmez.
  Etki, bir hareketteki deyimler, işlemin başlangıcında var olduğu gibi işlenen verilerin anlık görüntüsünü alır gibi olur.
  Anlık görüntüler Güvenilir Koleksiyonlar arasında tutarlıdır.

Güvenilir Koleksiyonlar, işlem ve işlemin oluşturuldurduğu anda yinelemenin rolüne bağlı olarak belirli bir okuma işlemi için kullanılacak yalıtım düzeyini otomatik olarak seçer.
Aşağıda, Güvenilir Sözlük ve Sıra işlemleri için yalıtım düzeyi varsayılanlarını gösteren tablo ve tablo yer alır.

| İşlem \ Rol | Birincil | İkincil |
| --- |:--- |:--- |
| Tek Varlık Okuma |Tekrarlanabilir Okuma |Anlık Görüntü |
| Numaralandırma, Sayım |Anlık Görüntü |Anlık Görüntü |

> [!NOTE]
> Tek Varlık İşlemleri için `IReliableDictionary.TryGetValueAsync` `IReliableQueue.TryPeekAsync`yaygın örnekler , .
> 

Hem Güvenilir Sözlük hem de Güvenilir Sıra desteği *Yazılarınızı Okuyun.*
Başka bir deyişle, bir hareket içindeki herhangi bir yazı, aynı işleme ait aşağıdaki okumatarafından görülebilir.

## <a name="locks"></a>Kilitler

Güvenilir Koleksiyonlarda, tüm işlemler titiz iki aşamalı kilitleme uygular: bir hareket, bir iptal veya taahhütle sona erene kadar edindiği kilitleri serbest bırakmaz.

Güvenilir Sözlük, tüm tek varlık işlemleri için satır düzeyinde kilitleme kullanır.
Güvenilir Kuyruk, sıkı işlemsel FIFO özelliği için eşzamanlılığı kapatır.
Güvenilir Kuyruk, aynı anda bir `TryPeekAsync` işlem ve/veya `TryDequeueAsync` bir `EnqueueAsync` işlemle bir işleme izin veren işlem düzeyinde kilitler kullanır.
FIFO'yu korumak için, `TryPeekAsync` `TryDequeueAsync` Güvenilir Sıra'nın boş olduğunu veya hiç `EnqueueAsync`gözlemlediği takdirde, bu sıranın da kilitleyeceğini unutmayın.

Yazma işlemleri her zaman Özel kilitleri alır.
Okuma işlemleri için kilitleme birkaç etkene bağlıdır:

- Anlık Görüntü yalıtımı kullanılarak yapılan herhangi bir okuma işlemi kilitsizdir.
- Varsayılan olarak tekrarlanabilir Okuma işlemi Paylaşılan kilitleri alır.
- Ancak, Yinelenebilir Okuma'yı destekleyen herhangi bir okuma işlemi için, kullanıcı Paylaşılan kilit yerine Güncelleştirme kilidi isteyebilir.
Güncelleştirme kilidi, birden çok işlemin kaynakları daha sonraki bir zamanda olası güncelleştirmeler için kilitlediğinde oluşan ortak bir kilitlenme biçimini önlemek için kullanılan asimetrik bir kilittir.

Kilit uyumluluk matrisi aşağıdaki tabloda bulunabilir:

| İstek \ Verilen | None | Paylaşımlı | Güncelleştirme | Özel |
| --- |:--- |:--- |:--- |:--- |
| Paylaşımlı |Çakışma yok |Çakışma yok |Çakışma |Çakışma |
| Güncelleştirme |Çakışma yok |Çakışma yok |Çakışma |Çakışma |
| Özel |Çakışma yok |Çakışma |Çakışma |Çakışma |

Güvenilir Koleksiyonlar API'lerinde zaman aşım bağımsız değişkeni kilitlenme algılamaiçin kullanılır.
Örneğin, iki işlem (T1 ve T2) K1'i okumaya ve güncelleştirmeye çalışır.
Kilitlenmeleri mümkündür, çünkü her ikisi de Paylaşılan kilidi netti.
Bu durumda, işlemlerden biri veya her ikisi zaman dolacak. Ben bu senaryo, bir Güncelleştirme kilidi böyle bir kilitlenme önleyebilir.

## <a name="next-steps"></a>Sonraki adımlar

* [Güvenilir Koleksiyonlar ile çalışma](service-fabric-work-with-reliable-collections.md)
* [Güvenilir Hizmetler bildirimleri](service-fabric-reliable-services-notifications.md)
* [Güvenilir Hizmetler yedekleme ve geri yükleme (olağanüstü durum kurtarma)](service-fabric-reliable-services-backup-restore.md)
* [Güvenilir Durum Yöneticisi yapılandırması](service-fabric-reliable-services-configuration.md)
* [Güvenilir Koleksiyonlar için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
