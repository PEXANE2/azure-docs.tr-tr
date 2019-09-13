---
title: Dayanıklı işlevler faturalandırma-Azure Işlevleri
description: Dayanıklı işlevlerin iç davranışları ve Azure Işlevleri için faturalandırmayı nasıl etkilediği hakkında bilgi edinin.
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: overview
ms.date: 08/31/2019
ms.author: azfuncdf
ms.openlocfilehash: b79d50adf932bd5c316fbda9d0964eea7c0484ca
ms.sourcegitcommit: f3f4ec75b74124c2b4e827c29b49ae6b94adbbb7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70935890"
---
# <a name="durable-functions-billing"></a>Dayanıklı İşlevler faturalandırma

[Dayanıklı işlevler](durable-functions-overview.md) Azure işlevleri ile aynı faturalandırılır. Daha fazla bilgi için [Azure işlevleri fiyatlandırması](https://azure.microsoft.com/pricing/details/functions/). Azure Işlevleri [Tüketim planında](../functions-scale.md#consumption-plan)Orchestrator işlevleri çalıştırıldığında, bazı fatura davranışları göz önünde bulundurulmalıdır. Aşağıdaki bölümlerde bu davranışlar ve bunların etkileri daha ayrıntılı olarak açıklanır.

## <a name="orchestrator-function-replay-billing"></a>Orchestrator işlevi yeniden yürütme faturalandırma

[Orchestrator işlevleri](durable-functions-orchestrations.md) , düzenleme ömrü boyunca birkaç kez yeniden kullanılabilir. Her yeniden yürütme işlemi, Azure Işlevleri çalışma zamanı tarafından ayrı bir işlev çağrısı olarak görüntülenir. Bu nedenle, Azure Işlevleri tüketim planında, Orchestrator işlevinin her yeniden oynaması için faturalandırılırsınız. Diğer plan türleri Orchestrator işlevi yeniden yürütme için ücret alınmaz.

## <a name="awaiting-and-yielding-in-orchestrator-functions"></a>Orchestrator işlevleri bekleniyor ve sürüyor

Bir Orchestrator işlevi, ( `await` C#) veya `yield` (JavaScript) kullanarak zaman uyumsuz bir eylemin tamamlanmasını bekliyorsa, çalışma zamanı belirli yürütmenin tamamlandığını kabul eder. Orchestrator işlevinin faturalandırması bu noktada sona erer ve sonraki Orchestrator işlevi yeniden yürütmeden önce bu işlemi sürdürmez. Orchestrator işlevinde bekleyen veya olmayan herhangi bir zaman faturalandırılmaz.

> [!NOTE]
> Diğer işlevleri çağıran işlevler bazıları bir kenar yumuşatma olarak değerlendirilir. Bunun nedeni, _Double faturalandırma_olarak bilinen bir sorundur. Bir işlev doğrudan başka bir işlevi çağırdığında, her ikisi de aynı anda yürütülür. Çağıran bir yanıt beklerken, çağrılan kodu etkin bir şekilde yürütüyor. Bu durumda, arayanın yürütme işleminin yürütülmesi için harcadığı süre için ödeme yapmanız gerekir. Orchestrator işlevleri, bir etkinlik işlevinin (veya alt düzenleme) sonucunu beklerken, Orchestrator işlevinin faturalandırılması nedeniyle bu çift Faturalandırmadan dolayı görünmez.

## <a name="durable-http-polling"></a>Kalıcı HTTP yoklaması

Orchestrator işlevleri, [http özellikleri](durable-functions-http-features.md) makalesinde açıklandığı gibi dış uç noktalara uzun süre çalışan http çağrıları yapabilir. Yöntemi (C#) ve `callHttp` yöntemi (JavaScript), [zaman uyumsuz 202 modelini](durable-functions-http-features.md#http-202-handling)takip eden bir HTTP uç noktasını dahili olarak yoklayamayabilir. `CallHttpAsync` Şu anda iç HTTP yoklama işlemleri için doğrudan faturalandırma yoktur. Bununla birlikte, iç yoklama Orchestrator işlevinin düzenli olarak yeniden oynamasına neden olabilir ve bu iç işlev için standart ücretler faturalandırılacaksınız.

## <a name="azure-storage-transactions"></a>Azure depolama işlemleri

Dayanıklı İşlevler durumu kalıcı hale getirmek, iletileri işlemek ve BLOB kiraları aracılığıyla bölümleri yönetmek için Azure Storage 'ı varsayılan olarak kullanır. Bu depolama hesabı size ait olduğundan, tüm işlem maliyetleri Azure aboneliğinize faturalandırılır. Dayanıklı İşlevler tarafından kullanılan Azure depolama yapıtları hakkında daha fazla bilgi için bkz. [görev hub 'ları](durable-functions-task-hubs.md) makalesi.

Dayanıklı İşlevler uygulamanız tarafından tahakkuk eden gerçek Azure depolama maliyetlerine birçok etken katkıda bulunur.

* Tek bir işlev uygulaması, bir dizi Azure depolama kaynağını paylaşan tek bir görev hub 'ı ile ilişkilendirilir. Bu kaynaklar, bir işlev uygulamasındaki tüm dayanıklı işlevler tarafından kullanılır. İşlev uygulamasındaki gerçek işlev sayısı, Azure depolama işlem maliyetlerini etkilemez.
* Her işlev uygulaması örneği, bir üstel geri alma yoklama algoritması kullanarak depolama hesabındaki birden çok kuyruğu dahili olarak yoklar. Boş bir uygulama örneği, kuyrukları etkin bir uygulamadan daha az sıklıkta yoklayacak ve daha az işlem maliyetine neden olur. Dayanıklı İşlevler kuyruğu-yoklama davranışı hakkında daha fazla bilgi için [performans ve ölçek makalesinin sıra yoklaması bölümüne](durable-functions-perf-and-scale.md#queue-polling) bakın.
* Azure işlevleri tüketimine veya Premium planlarında çalışırken, [Azure Işlevleri ölçek denetleyicisi](../functions-scale.md#how-the-consumption-and-premium-plans-work) arka plandaki tüm görev hub kuyruklarını düzenli olarak yoklar. Hafif ve orta ölçekli ölçekte, bu kuyrukları yalnızca tek bir ölçek denetleyicisi örneği yoklayacak. İşlev uygulaması çok sayıda örneğe ölçeklendirebilir, daha fazla ölçek denetleyicisi örneği eklenebilir. Bu ek ölçek denetleyicisi örnekleri, toplam kuyruk işlem maliyetlerini artırabilir.
* Her işlev uygulama örneği bir blob kiraları kümesi için. Bu örnekler, tutulan kiralamaları yenilemek için düzenli aralıklarla Azure Blob hizmetine çağrı yapar ve yeni kiralamalar elde etmeye çalışır. Blob kiralamaları sayısı, görev hub 'ının yapılandırılan bölüm sayısı tarafından belirlenir. Çok sayıda işlev uygulaması örneğine ölçekleme, büyük olasılıkla bu kira işlemleriyle ilişkili Azure depolama işlem maliyetlerini artırır.

Azure Depolama fiyatlandırması hakkında daha fazla bilgi için [Azure depolama fiyatlandırma](https://azure.microsoft.com/pricing/details/storage/) belgelerinde bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Işlevleri fiyatlandırması hakkında daha fazla bilgi edinin](https://azure.microsoft.com/pricing/details/functions/)
