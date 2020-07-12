---
title: Azure Işlevlerinde Tetikleyiciler ve bağlamalar
description: Azure işlevinizi çevrimiçi olaylara ve bulut tabanlı hizmetlere bağlamak için Tetikleyicileri ve bağlamaları kullanmayı öğrenin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: ddcf6758c8c648678c69070fa5b65ae6c4947018
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86252700"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure İşlevleri tetikleyicileri ve bağlama kavramları

Bu makalede, işlev Tetikleyicileri ve bağlamaları çevreleyen üst düzey kavramlar öğrenirsiniz.

Tetikleyiciler, bir işlevin çalışmasına neden olur. Bir tetikleyici, bir işlevin nasıl çağrılacağını ve bir işlevin tam olarak bir tetikleyicisine sahip olması gerektiğini tanımlar. Tetikleyiciler, genellikle işlevin yükü olarak sağlanan ilgili verilere sahiptir. 

İşleve bağlama, başka bir kaynağı işleve bildirimli olarak bağlama yöntemidir; bağlamalar, *Giriş bağlamaları*, *Çıkış bağlamaları*veya her ikisi için de bağlanabilir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır.

İhtiyaçlarınıza göre farklı bağlama türlerini bir arada kullanabilirsiniz. Bağlamalar isteğe bağlıdır ve bir işlevde bir veya daha fazla giriş ve/veya çıkış bağlaması bulunabilir.

Tetikleyiciler ve bağlamalar, diğer hizmetlere erişimi bir şekilde kodlamadan kaçınmanızı sağlar. İşleviniz verileri (bir kuyruk iletisinin içeriği gibi) işlev parametreleriyle alır. Verileri göndermek (bir kuyruk iletisi oluşturmak gibi) için işlevin dönüş değerini kullanırsınız. 

Farklı işlevleri nasıl uygulayabileceğinizi gösteren aşağıdaki örnekleri göz önünde bulundurun.

| Örnek senaryo | Tetikleyici | Giriş bağlama | Çıkış bağlama |
|-------------|---------|---------------|----------------|
| Başka bir kuyruğa yazmak için bir işlev çalıştıran yeni bir kuyruk iletisi ulaştı. | Sıradaki<sup>*</sup> | *Hiçbiri* | Sıradaki<sup>*</sup> |
|Zamanlanan bir iş BLOB depolama içeriğini okur ve yeni bir Cosmos DB belgesi oluşturur. | Zamanlayıcı | Blob Depolama | Cosmos DB |
|Event Grid, blob depolamadan bir görüntüyü okumak için ve Cosmos DB bir e-posta göndermek için kullanılır. | Event Grid | BLOB depolama ve Cosmos DB | SendGrid |
| Excel sayfasını güncelleştirmek için Microsoft Graph kullanan bir Web kancası. | HTTP | *Hiçbiri* | Microsoft Graph |

<sup>\*</sup>Farklı kuyrukları temsil eder

Bu örneklerin kapsamlı olması amaçlıyordu, ancak Tetikleyicileri ve bağlamaları birlikte nasıl kullanabileceğinizi göstermek için sağlanmıştır.

###  <a name="trigger-and-binding-definitions"></a>Tanımları tetikleme ve bağlama

Tetikleyiciler ve bağlamalar, geliştirme yaklaşımına göre farklı şekilde tanımlanır.

| Platform | Tetikleyiciler ve bağlamalar tarafından yapılandırılır... |
|-------------|--------------------------------------------|
| C# sınıf kitaplığı | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C# öznitelikleriyle yöntemleri ve parametreleri dekorasyon |
| Diğerlerinin tümü (Azure portal dahil) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;function.js([şema](http://json.schemastore.org/function)) [üzerinde](./functions-reference.md) güncelleştiriliyor |

Portal bu yapılandırma için bir kullanıcı arabirimi sağlar, ancak işlevinizin **tümleştirin** sekmesi aracılığıyla bulunan **Gelişmiş düzenleyiciyi** açarak dosyayı doğrudan düzenleyebilirsiniz.

.NET ' te, parametre türü giriş verileri için veri türünü tanımlar. Örneğin, `string` bir kuyruk tetikleyicisinin metnini bağlamak için kullanın, ikili olarak okunacak bir bayt dizisi ve bir nesneye serbest bırakmak için özel bir tür.

JavaScript gibi dinamik olarak yazılan diller için, `dataType` dosyadaki *function.js* özelliğini kullanın. Örneğin, bir HTTP isteğinin içeriğini ikili biçimde okumak için şu `dataType` şekilde ayarlayın `binary` :

```json
{
    "dataType": "binary",
    "type": "httpTrigger",
    "name": "req",
    "direction": "in"
}
```

İçin diğer seçenekler `dataType` `stream` ve ' dir `string` .

## <a name="binding-direction"></a>Bağlama yönü

Tüm tetikleyiciler ve bağlamalar `direction` dosyadaki [function.js](./functions-reference.md) bir özelliğe sahiptir:

- Tetikleyiciler için, yön her zaman`in`
- Giriş ve çıkış bağlamaları kullanımı `in` ve`out`
- Bazı bağlamalar özel bir yönü destekler `inout` . Kullanıyorsanız `inout` , portalda **tümleştir** sekmesi aracılığıyla yalnızca **Gelişmiş Düzenleyici** kullanılabilir.

Tetikleyicileri ve bağlamaları yapılandırmak için [bir sınıf kitaplığındaki öznitelikleri](functions-dotnet-class-library.md) kullandığınızda, yön bir öznitelik oluşturucusunda sağlanır veya parametre türünden çıkartılan olur.

## <a name="supported-bindings"></a>Desteklenen bağlamalar

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Önizleme aşamasında olan veya üretim kullanımı için onaylanan bağlamalar hakkında daha fazla bilgi için bkz. [desteklenen diller](supported-languages.md).

## <a name="resources"></a>Kaynaklar
- [Bağlama ifadeleri ve desenleri](./functions-bindings-expressions-patterns.md)
- [Azure Işlevi dönüş değerini kullanma](./functions-bindings-return-value.md)
- [Bağlama ifadesi kaydetme](./functions-bindings-register.md)
- Edici
  - [Azure İşlevleri'nde kodunuzu test etmeye yönelik stratejiler](functions-test-a-function.md)
  - [HTTP ile tetiklenmeyen bir işlevi el ile çalıştırma](functions-manually-run-non-http.md)
- [Bağlama hatalarını işleme](./functions-bindings-errors.md)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Işlevleri bağlama uzantılarını kaydetme](./functions-bindings-register.md)
