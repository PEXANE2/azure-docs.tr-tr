---
title: Azure Fonksiyonları güvenilir olay işleme
description: Azure İşlevleri'nde Olay Hub iletilerini kaçırmaktan kaçının
author: craigshoemaker
ms.topic: conceptual
ms.date: 09/12/2019
ms.author: cshoe
ms.openlocfilehash: e4f35495d8a01146068cffb9159c29c46c3c0d29
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75561876"
---
# <a name="azure-functions-reliable-event-processing"></a>Azure Fonksiyonları güvenilir olay işleme

Olay işleme, sunucusuz mimariyle ilişkili en yaygın senaryolardan biridir. Bu makalede, iletileri kaybetmemek için Azure İşlevleri ile güvenilir bir ileti işlemcisinin nasıl oluşturulacak olduğu açıklanmaktadır.

## <a name="challenges-of-event-streams-in-distributed-systems"></a>Dağıtılmış sistemlerde olay akışlarının zorlukları

Olayları saniyede 100 olay hızında gönderen bir sistem düşünün. Bu hızla, dakika içinde birden fazla paralel İşlev örneği gelen 100 olayı saniyede tüketebilir.

Ancak, aşağıdaki daha az en uygun koşullardan herhangi biri mümkündür:

- Olay yayımcısı bozuk bir olay gönderirse ne olur?
- İşlevler örneğiniz işlenmemiş özel durumlarla karşılaşırsa ne olur?
- Bir akış sistemi çevrimdışı olursa ne olur?

Uygulamanızın iş bilgililiği korurken bu durumlarla nasıl başa çıkıyorsunuz?

Kuyruklarda, güvenilir mesajlaşma doğal olarak gelir. İşlevler tetikleyicisi ile eşleştirildiğinde, işlev sıra iletisinde bir kilit oluşturur. İşlem başarısız olursa, kilit başka bir örneğin işlemeyi yeniden denemesine izin vermek için serbest bırakılır. İleti başarılı bir şekilde değerlendirilene veya zehirli sıraya eklenene kadar işleme devam eder.

Tek bir sıra iletisi yeniden deneme döngüsünde kalsa bile, diğer paralel yürütmeler kalan iletilerin sırasını çözmeye devam eder. Sonuç olarak, genel iş bilgili büyük ölçüde kötü bir iletiden etkilenmez. Ancak, depolama kuyrukları siparişi garanti etmez ve Olay Hub'ları tarafından gerekli olan yüksek iş yapma talepleri için optimize edideğildir.

Bunun aksine, Azure Etkinlik Hub'ları kilitleme konsepti içermez. Yüksek iş yapma, birden çok tüketici grubu ve yeniden oynatma yeteneği gibi özelliklere izin vermek için, Event Hubs etkinlikleri daha çok video oynatıcıgibi hareket eder. Olaylar, akışta bölüm başına tek bir noktadan okunur. İşaretçiden, o konumdan ileri veya geri okuyabilirsiniz, ancak olayların işlenmesi için işaretçiyi taşımayı seçmeniz gerekir.

Bir akışta hatalar oluştuğunda, işaretçiyi aynı noktada tutmaya karar verirseniz, işaretçi gelişmiş olana kadar olay işleme engellenir. Başka bir deyişle, işaretçi tek bir olayı işleyen sorunlarla başa çıkmak için durdurulursa, işlenmemiş olaylar birikmeye başlar.

Azure İşlevler, başarı veya başarısızlıktan bağımsız olarak akışı işaretçisini ilerleterek kilitlenmeleri önler. İşaretçi ilerlemeye devam ettiğiiçin, işlevlerinizin hataları uygun şekilde ele almalısınız.

## <a name="how-azure-functions-consumes-event-hubs-events"></a>Azure Fonksiyonları Olay Hub'larını nasıl tüketir?

Azure İşlevleri Etkinlik Hub olaylarını tüketirken aşağıdaki adımlardan geçer:

1. Olay merkezinin her bölümü için Azure Depolama'da bir işaretçi oluşturulur ve kalıcıdır.
2. Yeni iletiler (varsayılan olarak toplu iş başında) alındığı zaman, ana bilgisayar işlevi iletiler grubuyla tetiklemeye çalışır.
3. İşlev yürütmeyi tamamlarsa (istisnasız veya istisnasız) işaretçi ilerler ve depolama hesabına bir denetim noktası kaydedilir.
4. Koşullar işlev yürütmetamamlanmasını engellerse, ana bilgisayar işaretçi ilerleme başarısız olur. İşaretçi gelişmiş değilse, daha sonra denetimleri aynı iletileri işleme sonunda.
5. Adımları tekrarlayın 2-4

Bu davranış birkaç önemli noktayı ortaya çıkarır:

- *Ele alınamayan özel durumlar iletileri kaybetmenize neden olabilir.* Özel durumla sonuçlanan yürütmeler işaretçiyi ilerletmeye devam eder.
- *Fonksiyonlar en az bir kez teslimatı garanti eder.* Kodunuz ve bağımlı [sistemlerinizin, aynı iletinin iki kez alınabileceği gerçeğini hesaba katmak](./functions-idempotent.md)gerekebilir.

## <a name="handling-exceptions"></a>Özel durum işleme

Genel bir kural olarak, her işlev en yüksek kod düzeyinde bir [try/catch bloğu](./functions-bindings-error-pages.md) içermelidir. Özellikle, Olay Hub'ları olaylarını tüketen tüm işlevlerin bir `catch` bloğu olmalıdır. Bu şekilde, bir özel durum yükseltildiğinde, işaretçi ilerlemeden önce catch bloğu hatayı işler.

### <a name="retry-mechanisms-and-policies"></a>Mekanizmaları ve politikaları yeniden deneyin

Bazı özel durumlar geçicidir ve birkaç dakika sonra bir işlem yeniden denendiğinde yeniden görünmez. Bu nedenle ilk adım her zaman işlemi yeniden denemektir. Yeniden deneme işleme kurallarını kendiniz yazabilirsiniz, ancak bunlar o kadar olağandır ki, bir dizi araç kullanılabilir. Bu kitaplıkları kullanmak, işleme düzeninin korunmasına da yardımcı olabilecek güçlü yeniden deneme ilkeleri tanımlamanıza olanak sağlar.

Hataları işleme kitaplıklarını işlevlerinize tanıtmak, hem temel hem de gelişmiş yeniden deneme ilkelerini tanımlamanıza olanak sağlar. Örneğin, aşağıdaki kurallarla gösterilen bir iş akışını izleyen bir ilke uygulayabilirsiniz:

- Üç kez ileti eklemeyi deneyin (yeniden denemeler arasında bir gecikme olabilir).
- Tüm yeniden denemelerin nihai sonucu bir hataysa, işlemeakışında devam edebilirsiniz, böylece sıraya bir ileti ekleyin.
- Bozuk veya işlenmemiş iletiler daha sonra işlenir.

> [!NOTE]
> [Polly,](https://github.com/App-vNext/Polly) C# uygulamaları için esneklik ve geçici hata işleme kitaplığı örneğidir.

Önceden uyarılmış C# sınıf kitaplıklarıyla çalışırken, [özel durum filtreleri](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/try-catch) işlenmemiş bir özel durum oluştuğunda kodu çalıştırmanızı sağlar.

Özel durum filtrelerinin nasıl kullanılacağını gösteren örnekler [Azure Web İşleri SDK](https://github.com/Azure/azure-webjobs-sdk/wiki) reposunda kullanılabilir.

## <a name="non-exception-errors"></a>Özel durum dışı hatalar

Bir hata olmadığında bile bazı sorunlar ortaya çıkar. Örneğin, yürütmenin ortasında oluşan bir hata düşünün. Bu durumda, bir işlev yürütmetamamlanmazsa, ofset işaretçisi hiçbir zaman ilerletilir. İşaretçi ilerlemezse, başarısız bir yürütmeden sonra çalışan herhangi bir örnek aynı iletileri okumaya devam eder. Bu durum "en az bir kez" garantisi sağlar.

Her iletinin en az bir kez işlendiğigüvencesi, bazı iletilerin birden fazla kez işlenebileceği anlamına gelir. İşlev uygulamalarınız bu olasılığın farkında olmalı ve [idempotency ilkeleri](./functions-idempotent.md)etrafında oluşturulmalıdır.

## <a name="stop-and-restart-execution"></a>Yürütmeyi durdur ve yeniden başlat

Birkaç hata kabul edilebilir olsa da, uygulamanız önemli hatalarla karşılaşSa ne olur? Sistem sağlıklı bir duruma ulaşana kadar olayları tetiklemeyi durdurmak isteyebilirsiniz. İşlemi duraklatma fırsatına sahip olmak genellikle bir devre kesici deseni ile elde edilir. Devre kesici deseni, uygulamanızın olay sürecinin "devresini kırmasını" ve daha sonra devam etmesini sağlar.

Bir olay sürecinde bir devre kesici uygulamak için gerekli iki adet vardır:

- Devrenin durumunu izlemek ve izlemek için tüm örneklerde paylaşılan durum
- Devre durumunu yönetebilen ana işlem (açık veya kapalı)

Uygulama ayrıntıları değişebilir, ancak durumlar arasında durumu paylaşmak için bir depolama mekanizmasıgerekir. Durumu Azure Depolama'da, Redis önbelleğinde veya işlevler koleksiyonu tarafından erişilebilen başka bir hesapta depolamayı seçebilirsiniz.

[Azure Mantık Uygulamaları](../logic-apps/logic-apps-overview.md) veya [dayanıklı varlıklar,](./durable/durable-functions-overview.md) iş akışı ve devre durumunu yönetmek için doğal bir uyum sağlar. Diğer hizmetler de işe yarayabilir, ancak bu örnek için mantık uygulamaları kullanılır. Mantık uygulamalarını kullanarak, devre kesici deseni uygulamak için gereken denetimi sağlayarak bir işlevin yürütülmesini duraklatabilir ve yeniden başlatabilirsiniz.

### <a name="define-a-failure-threshold-across-instances"></a>Örnekler arasında bir hata eşiği tanımlama

Aynı anda birden çok örneği işleme olayları için hesap için, devrenin durumunu izlemek için paylaşılan dış durumu kalıcı olarak gereklidir.

Uygulamayı seçebileceğiniz bir kural şu kuralları uygulayabilir:

- Tüm örneklerde 30 saniye içinde 100'den fazla nihai hata varsa, devreyi kırın ve yeni iletileri tetiklemeyi durdurun.

Uygulama ayrıntıları ihtiyaçlarınıza göre değişir, ancak genel olarak aşağıdakileri yapan bir sistem oluşturabilirsiniz:

1. Depolama hesabında günlük hataları (Azure Depolama, Redis, vb.)
1. Yeni hata günlüğe kaydedildiğinde, eşiğin karşılanıp karşılanmadığı (örneğin, son 30 saniyede 100'den fazla) olup olmadığını görmek için yuvarlanma sayısını inceleyin.
1. Eşik karşılanırsa, Sisteme devreyi kırmasını söyleyen bir olay Azure Event Grid'e yayılın.

### <a name="managing-circuit-state-with-azure-logic-apps"></a>Azure Logic Apps ile devre durumunu yönetme

Aşağıdaki açıklama, Bir İşlevler uygulamasının işlenmesini durdurmak için bir Azure Mantık Uygulaması oluşturmanın bir yolunu vurgular.

Azure Logic Apps, farklı hizmetlere yerleşik konektörlerle birlikte gelir, devletsel orkestrasyonlar sunar ve devre durumunu yönetmek için doğal bir seçimdir. Devrenin kırılması gerektiğini tespit ettikten sonra, aşağıdaki iş akışını uygulamak için bir mantık uygulaması oluşturabilirsiniz:

1. Olay Ağı'nı tetikleme ve Azure İşi'ni durdurma (Azure Kaynak bağlayıcısıyla)
1. İş akışını yeniden başlatma seçeneğini içeren bir bildirim e-postası gönderme

E-posta alıcısı devrenin durumunu araştırabilir ve uygun olduğunda bildirim e-postasındaki bir bağlantı aracılığıyla devreyi yeniden başlatabilir. İş akışı işlevi yeniden başlatırken, iletiler son Olay Hub denetim noktasından işlenir.

Bu yaklaşımı kullanarak, hiçbir ileti kaybolur, tüm iletiler sırayla işlenir ve gerektiği sürece devre kırabilirsiniz.

## <a name="resources"></a>Kaynaklar

- [Güvenilir olay işleme örnekleri](https://github.com/jeffhollan/functions-csharp-eventhub-ordered-processing)
- [Azure Dayanıklı Fonksiyonlar Devre Kesici](https://github.com/jeffhollan/functions-durable-actor-circuitbreaker)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için aşağıdaki kaynaklara bakın:

- [Azure Fonksiyonları hata işleme](./functions-bindings-error-pages.md)
- [Karşıya yüklenen görüntüleri yeniden boyutlandırmayı Event Grid kullanarak otomatikleştirme](../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2Fazure%2Fazure-functions%2Ftoc.json&tabs=dotnet)
- [Azure Logic Apps ile tümleşen bir işlev oluşturma](./functions-twitter-email.md)
