---
title: Dayanıklı Fonksiyonlar sürümlerine genel bakış - Azure Fonksiyonları
description: Dayanıklı Fonksiyonlar sürümleri hakkında bilgi edinin.
author: cgillum
ms.topic: conceptual
ms.date: 10/30/2019
ms.author: azfuncdf
ms.openlocfilehash: 4a117e7f69647af3ad82f9013bfa40556ccc0dbd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77152899"
---
# <a name="durable-functions-versions-overview"></a>Dayanıklı Fonksiyonlar sürümlerine genel bakış

*Dayanıklı Işlevler,* [Azure İşlevlerinin](../functions-overview.md) ve [Azure Web İşlerinin](../../app-service/web-sites-create-web-jobs.md) bir uzantısıdır ve sunucusuz bir ortamda durum lu işlevler yazmanızı sağlar. Uzantı sizin için durumu, denetim noktalarını ve yeniden başlatmaları yönetir. Dayanıklı İşlevler'e zaten aşina değilseniz, [genel bakış belgelerine](durable-functions-overview.md)bakın.

## <a name="new-features-in-2x"></a>2.x yeni özellikler

Bu bölümde, sürüm 2.x'e eklenen Dayanıklı İşlevler'in özellikleri açıklanmaktadır.

### <a name="durable-entities"></a>Dayanıklı varlıklar

Dayanıklı Fonksiyonlar 2.x'te yeni bir [varlık fonksiyonları](durable-functions-entities.md) kavramını tanıttık.

Varlık işlevleri, *dayanıklı varlıklar*olarak bilinen küçük durum parçalarını okuma ve güncelleştirme işlemlerini tanımlar. Orchestrator işlevleri gibi, varlık işlevleri özel bir tetikleyici türü, *varlık tetikleyici*si ile işlevlerdir. Orchestrator işlevlerinin aksine, varlık işlevlerinin belirli bir kod kısıtlaması yoktur. Varlık işlevleri de açıkça yerine örtülü kontrol akışı yoluyla devlet temsil devlet yönetir.

Daha fazla bilgi edinmek için [dayanıklı varlıklar](durable-functions-entities.md) makalesine bakın.

### <a name="durable-http"></a>Dayanıklı HTTP

Dayanıklı Fonksiyonlar 2.x'te, şunları yapmanızı sağlayan yeni bir [Dayanıklı HTTP](durable-functions-http-features.md#consuming-http-apis) özelliği ni kullanıma sunduk:

* HTTP API'leri doğrudan düzenleme işlevlerinden (bazı belgelenmiş sınırlamalarla) arayın.
* Otomatik istemci tarafı HTTP 202 durum yoklama uygulayın.
* [Azure Yönetilen Kimlikler](../../active-directory/managed-identities-azure-resources/overview.md)için yerleşik destek.

Daha fazla bilgi için [HTTP özellikleri](durable-functions-http-features.md#consuming-http-apis) makalesine bakın.

## <a name="migrate-from-1x-to-2x"></a>1.x'ten 2.x'e geçirin

Bu bölümde, yeni özelliklerden yararlanmak için varolan sürüm 1.x Dayanıklı Fonksiyonlar sürüm 2.x'e nasıl geçirilir açıklanmaktadır.

### <a name="upgrade-the-extension"></a>Uzantıyı yükseltme

Projenizdeki [Dayanıklı Fonksiyonlar bağlama uzantısısürüm](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask) 2.x'i yükleyin. Daha fazla bilgi için [Azure İşlerini kaydedin](../functions-bindings-register.md) uzantılarını kaydedin.

### <a name="update-your-code"></a>Kodunuzu güncelleştirin

Dayanıklı Fonksiyonlar 2.x birkaç kırılma değişiklikleri tanır. Dayanıklı Fonksiyonlar 1.x uygulamaları kod değişikliği olmadan Dayanıklı Fonksiyonlar 2.x ile uyumlu değildir. Bu bölümde, sürüm 1.x işlevlerinizi 2.x'e yükseltirken yaptığınız değişikliklerden bazıları listelemektedir.

#### <a name="hostjson-schema"></a>Host.json şema

Dayanıklı Fonksiyonlar 2.x yeni bir host.json şeması kullanır. 1.x'ten itibaren yapılan ana değişiklikler şunlardır:

* `"storageProvider"`(ve `"azureStorage"` alt bölüm) depolamaya özgü yapılandırma için.
* `"tracing"`izleme ve günlük yapılandırması için.
* `"notifications"`(ve `"eventGrid"` alt bölüm) olay ızgarabildirim yapılandırması için.

Ayrıntılar için [Dayanıklı Fonksiyonlar host.json başvuru belgelerine](durable-functions-bindings.md#durable-functions-2-0-host-json) bakın.

#### <a name="default-taskhub-name-changes"></a>Varsayılan taskhub adı değişiklikleri

Sürüm 1.x'te, host.json'da bir görev hub adı belirtilmemişse, varsayılan olarak "DurableFunctionsHub" olarak belirlenmiştir. Sürüm 2.x'te varsayılan görev merkezi adı artık işlev uygulamasının adından türetilmiştir. Bu nedenle, 2.x'e yükseltirken bir görev hub adı belirtmediyseniz, kodunuz yeni görev hub'ı ile çalışacaktır ve tüm uçuş içi orkestrasyonlar artık bunları işleyen bir uygulama olmaz. Bunu aşmak için, görev hub adınızı açıkça "DurableFunctionsHub" v1.x varsayılanına ayarlayabilir veya uçuş içi orkestrasyonlar için son değişiklikleri nasıl işleyeceğiniz hakkında ayrıntılar için [sıfır kesinti zamanı dağıtım kılavuzumuzu](durable-functions-zero-downtime-deployment.md) izleyebilirsiniz.

#### <a name="public-interface-changes-net-only"></a>Ortak arabirim değişiklikleri (yalnızca.NET)

Sürüm 1.x'te, Dayanıklı Işlevler tarafından desteklenen çeşitli _bağlam_ nesneleri birim testinde kullanılmak üzere tasarlanmış soyut temel sınıflara sahiptir. Dayanıklı Fonksiyonlar 2.x'in bir parçası olarak, bu soyut temel sınıflar arabirimlerle değiştirilir.

Aşağıdaki tablo ana değişiklikleri temsil eder:

| 1.x | 2.x |
|----------|----------|
| `DurableOrchestrationClientBase` | `IDurableOrchestrationClient` veya `IDurableClient` |
| `DurableOrchestrationContext` veya `DurableOrchestrationContextBase` | `IDurableOrchestrationContext` |
| `DurableActivityContext` veya `DurableActivityContextBase` | `IDurableActivityContext` |
| `OrchestrationClientAttribute` | `DurableClientAttribute` |

Soyut bir taban sınıfının sanal yöntemler içerdiği durumlarda, bu sanal yöntemler `DurableContextExtensions`.

#### <a name="functionjson-changes-javascript-and-c-script"></a>function.json değişiklikleri (JavaScript ve C# Script)

Dayanıklı Fonksiyonlar 1.x'te, orkestrasyon istemcisi bağlama bir `type` . `orchestrationClient` Sürüm 2.x `durableClient` bunun yerine kullanır.
