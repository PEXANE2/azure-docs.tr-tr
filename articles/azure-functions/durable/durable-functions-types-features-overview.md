---
title: Azure Dayanıklı İşlevlerde İşlev türleri
description: Azure İşlevlerinde Dayanıklı İşlevler düzenlemesinde işlevden işleve iletişimi destekleyen işlev ve rol türleri hakkında bilgi edinin.
author: cgillum
ms.topic: conceptual
ms.date: 08/22/2019
ms.author: azfuncdf
ms.openlocfilehash: 35ef9d8731e169e890f5985ce01215fec5d6e3de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79277888"
---
# <a name="durable-functions-types-and-features"></a>Dayanıklı Fonksiyonlar türleri ve özellikleri

Dayanıklı Işlevler, [Azure İşlevlerinin](../functions-overview.md)bir uzantısıdır. İşlev yürütmenin durum lu düzenlemesi için Dayanıklı İşlevler'i kullanabilirsiniz. Dayanıklı işlev uygulaması, farklı Azure işlevlerinden oluşan bir çözümdür. Fonksiyonlar dayanıklı bir işlev orkestrasyonunda farklı roller oynayabilir. 

Şu anda Azure İşlevlerinde dört dayanıklı işlev türü vardır: etkinlik, orkestratör, varlık ve istemci. Bu bölümün geri kalanı, bir orkestrasyonda yer alan işlev türleri hakkında daha fazla ayrıntıya girer.

## <a name="orchestrator-functions"></a>Orkestratör fonksiyonları

Orchestrator işlevleri, eylemlerin nasıl yürütüldedildiğini ve eylemlerin yürütülme sırasını açıklar. Orchestrator işlevleri, dayanıklı [işlevler uygulama desenlerinde](durable-functions-overview.md#application-patterns)gösterildiği gibi, düzenlemeyi kodda (C# veya JavaScript) açıklar. Bir orkestrasyon [etkinlik fonksiyonları,](#activity-functions) [alt orkestrasyonlar,](durable-functions-orchestrations.md#sub-orchestrations) [dış olaylar,](durable-functions-orchestrations.md#external-events) [HTTP](durable-functions-http-features.md)ve [zamanlayıcılar](durable-functions-orchestrations.md#durable-timers)için bekleyen dahil olmak üzere eylemlerin birçok farklı türde olabilir. Orchestrator işlevleri de [varlık işlevleri](#entity-functions)ile etkileşime girebilirsiniz.

> [!NOTE]
> Orchestrator işlevleri sıradan kod kullanılarak yazılır, ancak kodun nasıl yazılalacaksı konusunda katı gereksinimler vardır. Özellikle, orchestrator fonksiyon kodu *deterministik*olmalıdır. Bu determinizm gereksinimlerini n uyulmaması, orkestratör işlevlerinin düzgün çalışmasına neden olabilir. Bu gereksinimler ve bunların nasıl çözülebileceği hakkında ayrıntılı bilgi [kod kısıtlamaları](durable-functions-code-constraints.md) konusuna eklenebilir.

Orkestratör işlevleri ve özellikleri hakkında daha ayrıntılı bilgi için [Dayanıklı orkestrasyon](durable-functions-orchestrations.md) makalesine bakın.

## <a name="activity-functions"></a>Etkinlik fonksiyonları

Etkinlik işlevleri, dayanıklı bir fonksiyon orkestrasyonundaki temel çalışma birimidir. Etkinlik işlevleri, işlemde düzenlenmiş işlevler ve görevlerdir. Örneğin, bir siparişi işlemek için bir orkestratör işlevi oluşturabilirsiniz. Görevler envanteri denetlemeyi, müşteriyi şarj etmeyi ve sevkiyat oluşturmayı içerir. Her görev ayrı bir etkinlik işlevi olacaktır. Bu etkinlik işlevleri seri olarak, paralel olarak veya her ikisinin bir birleşimi olarak yürütülebilir.

Orchestrator işlevlerinin aksine, etkinlik işlevleri, bu işlevlerde yapabileceğiniz iş türünde kısıtlanmaz. Etkinlik işlevleri sık sık ağ aramaları yapmak veya CPU yoğun işlemleri çalıştırmak için kullanılır. Bir etkinlik işlevi de verileri orkestratör işlevine geri döndürebilir. Dayanıklı Görev Çerçevesi, çağrılan her etkinlik işlevinin bir orkestrasyonun yürütülmesi sırasında *en az bir kez* yürütüleceğini garanti eder.

> [!NOTE]
> Etkinlik işlevleri *yalnızca bir kez* yürütmeyi garanti ettiğinden, etkinlik işlevmantığınızı mümkün olduğunca *iktidara* getirmenizi öneririz.

Bir [etkinlik](durable-functions-bindings.md#activity-trigger) işlevini tanımlamak için bir etkinlik tetikleyicisi kullanın. .NET işlevleri `DurableActivityContext` parametre olarak alır. Ayrıca, tetikleyiciyi işleve girişler geçirmek için başka bir JSON serihale getirilebilir nesneye bağlayabilirsiniz. JavaScript'te, `<activity trigger binding name>` [ `context.bindings` nesneüzerindeki](../functions-reference-node.md#bindings)özellik üzerinden bir girişe erişebilirsiniz. Etkinlik işlevleri yalnızca tek bir değere geçmiş olabilir. Birden çok değer geçirmek için tuples, diziler veya karmaşık türleri kullanmanız gerekir.

> [!NOTE]
> Bir etkinlik işlevini yalnızca bir orkestratör işlevinden tetikleyebilirsiniz.

## <a name="entity-functions"></a>Varlık işlevleri

Varlık işlevleri, küçük durum parçalarını okuma ve güncelleştirme işlemlerini tanımlar. Bu durum lu varlıklara sık sık dayanıklı varlıklar atıfta *bulunuruz.* Orchestrator işlevleri gibi, varlık işlevleri özel bir tetikleyici türü, *varlık tetikleyici*si ile işlevlerdir. İstemci işlevlerinden veya orkestratör işlevlerinden de çağrılabilirler. Orchestrator işlevlerinin aksine, varlık işlevlerinin belirli bir kod kısıtlaması yoktur. Varlık işlevleri de açıkça yerine örtülü kontrol akışı yoluyla devlet temsil devlet yönetir.

> [!NOTE]
> Varlık işlevleri ve ilgili işlevler yalnızca Dayanıklı Fonksiyonlar 2.0 ve üzeri işlevlerde kullanılabilir.

Varlık işlevleri hakkında daha fazla bilgi için [Dayanıklı Varlıklar](durable-functions-entities.md) makalesine bakın.

## <a name="client-functions"></a>İstemci işlevleri

Orchestrator işlevleri bir [orchestration tetikleme bağlama](durable-functions-bindings.md#orchestration-trigger) tarafından tetiklenir ve varlık işlevleri bir [varlık tetikleyici bağlama](durable-functions-bindings.md#entity-trigger)tarafından tetiklenir. Bu tetikleyicilerin her ikisi de, görev hub'ına [task hub](durable-functions-task-hubs.md)sıralanmış iletilere tepki vererek çalışır. Bu iletileri teslim etmenin birincil yolu, bir [orchestrator istemci bağlama](durable-functions-bindings.md#orchestration-client) veya bir istemci işlevi içinden bağlayıcı bir [varlık istemcisi](durable-functions-bindings.md#entity-client) *kullanarak.* Herhangi bir non-orchestrator işlevi bir *istemci işlevi*olabilir. Örneğin, orkestratörü HTTP tarafından tetiklenen bir işlevden, bir Azure Olay Hub'ı tetikleyen işlevden vb. tetikleyebilirsiniz. Bir işlevi *istemci işlevi* yapan şey, dayanıklı istemci çıktısı bağlamasını kullanmasıdır.

> [!NOTE]
> Diğer işlev türlerinin aksine, orkestratör ve varlık işlevleri doğrudan Azure Portalı'ndaki düğmeler kullanılarak tetiklenemez. Azure Portalı'nda bir orkestratör veya varlık işlevini sınamak istiyorsanız, bunun yerine, bir orkestratör veya varlık işlevini uygulamanın bir parçası olarak başlatan bir *istemci işlevi* çalıştırmanız gerekir. En basit test deneyimi için *manuel tetikleme* işlevi önerilir.

Orchestrator veya varlık işlevlerini tetiklemenin yanı sıra, *dayanıklı istemci* bağlaması çalışan orkestrasyonlarla ve varlıklarla etkileşimkurmak için kullanılabilir. Örneğin, orkestrasyonlar sorgulanabilir, sonlandırılabilir ve olaylar bunlara yükseltilebilir. Orkestrasyon ve varlıkları yönetme hakkında daha fazla bilgi için [Örnek yönetimi](durable-functions-instance-management.md) makalesine bakın.

## <a name="next-steps"></a>Sonraki adımlar

Başlamak için [C#](durable-functions-create-first-csharp.md) veya [JavaScript'te](quickstart-js-vscode.md)ilk dayanıklı işlevinizi oluşturun.

> [!div class="nextstepaction"]
> [Dayanıklı Fonksiyonlar orkestrasyonları hakkında daha fazla bilgi edinin](durable-functions-orchestrations.md)