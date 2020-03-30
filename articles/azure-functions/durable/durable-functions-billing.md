---
title: Dayanıklı işlevler faturalandırma - Azure Fonksiyonları
description: Dayanıklı İşlevlerin iç davranışları ve Azure İşlevleri için faturalandırmayı nasıl etkilediği hakkında bilgi edinin.
author: cgillum
ms.topic: overview
ms.date: 08/31/2019
ms.author: azfuncdf
ms.openlocfilehash: 504ef93a0002895bc5662d95ad269c8593170ee2
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74233006"
---
# <a name="durable-functions-billing"></a>Dayanıklı Fonksiyonlar faturalandırma

[Dayanıklı Işlevler,](durable-functions-overview.md) Azure İşlevler ile aynı şekilde faturalandırılır. Daha fazla bilgi için Azure [İşlevler fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/)'na bakın.

Azure İşlevler [Tüketim planında](../functions-scale.md#consumption-plan)orkestratör işlevlerini yürütürken, bazı faturalandırma davranışlarına dikkat etseniz gerekir. Aşağıdaki bölümlerde bu davranışları ve bunların etkileri daha ayrıntılı olarak açıklayınız.

## <a name="orchestrator-function-replay-billing"></a>Orchestrator işlevi yeniden faturalandırma

[Orchestrator işlevleri](durable-functions-orchestrations.md) bir orkestrasyon ömrü boyunca birkaç kez yeniden çalabilir. Her tekrar, Azure İşlevleri çalışma zamanı tarafından ayrı bir işlev çağırması olarak görüntülenir. Bu nedenle, Azure İşlevler Tüketim planında bir orkestratör işlevinin her tekrarı için faturalandırılırsınız. Diğer plan türleri orchestrator işlevi tekrarı için ücret lendirmez.

## <a name="awaiting-and-yielding-in-orchestrator-functions"></a>Orkestratör işlevlerini beklemek ve verimli olmak

Bir orkestratör işlevi C# veya JavaScript'te **verim** için **bekleme** yi kullanarak asynchronous eyleminin bitmesini beklediğinde, çağrıda belirli bir işletmenin tamamlanmışolması dikkate Orkestratör işlevinin faturalandırması bu noktada durur. Bir sonraki orkestratör işlevi tekrarı kadar devam etmez. Bir orkestratör işlevini beklerken veya vermek için harcanan herhangi bir zaman için faturalandırılmamanız.

> [!NOTE]
> Diğer işlevleri çağıran işlevler bazıları tarafından bir antipattern olarak kabul edilir. Bunun _nedeni, çift faturalama_olarak bilinen bir sorundur. Bir işlev başka bir işlevi doğrudan aradığında, her ikisi de aynı anda çalışır. Çağrı işlevi yanıt beklerken çağrı işlevi etkin bir şekilde kod çalıştırıyor. Bu durumda, arama işlevinin çağrılan işlevin çalışmasını bekleyerek harcadığı süreyi ödemeniz gerekir.
>
> Orkestratör işlevlerinde çift faturalama yoktur. Bir orchestratör işlevinin faturalandırması, bir etkinlik işlevinin veya alt orkestrasyonun sonucunu beklerken durur.

## <a name="durable-http-polling"></a>Dayanıklı HTTP yoklama

Orchestrator [işlevleri, HTTP özellikleri makalesinde](durable-functions-http-features.md)açıklandığı gibi harici uç noktalara uzun süreli HTTP çağrıları yapabilir. C#'daki **CallHttpAsync** yöntemi ve JavaScript'teki **callHttp** yöntemi, [eşzamanlı 202 desenini](durable-functions-http-features.md#http-202-handling)takip ederken bir HTTP bitiş noktasını dahili olarak yoklayababilir.

Şu anda dahili HTTP yoklama işlemleri için doğrudan faturalama yoktur. Ancak, dahili yoklama orkestratör işlevinin düzenli olarak yeniden çalmasına neden olabilir. Bu dahili işlev tekrarları için standart ücretler faturalandırılırsınız.

## <a name="azure-storage-transactions"></a>Azure Depolama işlemleri

Dayanıklı Işlevler, durumları kalıcı tutmak, iletileri işlemek ve bölme kiralama yoluyla bölümleri yönetmek için varsayılan olarak Azure Depolama'yı kullanır. Bu depolama hesabına sahip olduğunuz için, tüm işlem maliyetleri Azure aboneliğinize faturalandırılır. Dayanıklı İşlevler tarafından kullanılan Azure Depolama yapıları hakkında daha fazla bilgi için [Görev hub'ları makalesine](durable-functions-task-hubs.md)bakın.

Dayanıklı İşlevler uygulamanızın neden olduğu gerçek Azure Depolama maliyetlerine çeşitli etkenler katkıda bulunur:

* Tek bir işlevli uygulama, bir dizi Azure Depolama kaynaklarını paylaşan tek bir görev hub'ı ile ilişkilidir. Bu kaynaklar, bir işlev uygulamasındaki tüm dayanıklı işlevler tarafından kullanılır. İşlev uygulamasındaki gerçek işlev sayısının Azure Depolama işlem maliyetleri üzerinde hiçbir etkisi yoktur.
* Her işlev uygulaması örneği, üstel bir geri tepme yoklama algoritması kullanarak depolama hesabındaki birden çok sırayı içten olarak yoklar. Boşta bulunan bir uygulama örneği, kuyrukları etkin bir uygulamadan daha az eşler ve bu da daha az işlem maliyetiyle sonuçlanır. Dayanıklı Işlevler sıra yoklama davranışı hakkında daha fazla bilgi [için, Performans ve Ölçek makalesinin sıra yoklama bölümüne](durable-functions-perf-and-scale.md#queue-polling)bakın.
* Azure İşlevler Tüketimi veya Premium planlarında çalışırken, [Azure İşlevler ölçeği denetleyicisi](../functions-scale.md#how-the-consumption-and-premium-plans-work) arka plandaki tüm görev merkezi kuyruklarını düzenli olarak yoklar. Bir işlev uygulaması ışık altında orta ölçekli ise, yalnızca tek bir ölçek denetleyici örneği bu kuyrukları yoklar. İşlev uygulaması çok sayıda örneğe ölçeklendirilebilirse, daha fazla ölçek denetleyici örnekleri eklenebilir. Bu ek ölçek denetleyici örnekleri toplam sıra hareket maliyetlerini artırabilir.
* Her işlev uygulaması örneği, bir dizi blob kiralaması için rekabet eder. Bu örnekler, tutulan kiraları yenilemek veya yeni kiralamalar elde etmeye çalışmak için Azure Blob hizmetini düzenli aralıklarla arar. Görev merkezinin yapılandırılmış bölüm sayısı, blob kiralama sayısını belirler. Daha fazla sayıda işlev uygulaması örneğine ölçekleme, büyük olasılıkla bu kiralama işlemleriyle ilişkili Azure Depolama işlem maliyetlerini artırır.

Azure Depolama [fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) hakkında daha fazla bilgiyi Azure Depolama fiyatlandırma belgelerinde bulabilirsiniz. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevleri fiyatlandırması hakkında daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/functions/)
