---
title: Azure İşlevlerinde tetikleyiciler ve bağlamalar
description: Azure İşlevinizi çevrimiçi etkinliklere ve bulut tabanlı hizmetlere bağlamak için tetikleyicileri ve bağlamaları kullanmayı öğrenin.
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: d41fd7f66ecef3a563345424d7dc4366e47d3f0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79276510"
---
# <a name="azure-functions-triggers-and-bindings-concepts"></a>Azure İşlevleri kavramları tetikler ve bağlar

Bu makalede, işlevleri tetikler ve bağlamaları çevreleyen üst düzey kavramları öğrenirsiniz.

Tetikleyiciler bir işlevin çalışmasına neden olan şeydir. Tetikleyici, bir işlevin nasıl çağrıldığını ve bir işlevin tam olarak bir tetikleyiciye sahip olması gerektiğini tanımlar. Tetikleyiciler, genellikle işlevin yükü olarak sağlanan ilgili verilere sahiptir. 

Bir işleve bağlama, başka bir kaynağı işleve bildirimsel olarak bağlamanın bir yoludur; bağlayıcılar *giriş bağlamaları,* çıkış *bağlamaları*veya her ikisi olarak bağlanabilir. Bağlamalardan alınan veriler işleve parametre olarak sağlanır.

İhtiyaçlarınıza göre farklı bağlama türlerini bir arada kullanabilirsiniz. Bağlamalar isteğe bağlıdır ve bir işlevde bir veya daha fazla giriş ve/veya çıkış bağlaması bulunabilir.

Tetikleyiciler ve bağlamalar, diğer hizmetlere zor kodlama erişiminden kaçınmanızı sağlar. İşleviniz verileri (bir kuyruk iletisinin içeriği gibi) işlev parametreleriyle alır. Verileri göndermek (bir kuyruk iletisi oluşturmak gibi) için işlevin dönüş değerini kullanırsınız. 

Farklı işlevleri nasıl uygulayabileceğinize ilgili aşağıdaki örnekleri göz önünde bulundurun.

| Örnek senaryo | Tetikleyici | Giriş bağlama | Çıkış bağlama |
|-------------|---------|---------------|----------------|
| Başka bir kuyruğa yazmak için bir işlev çalıştıran yeni bir sıra iletisi gelir. | Sıra<sup>*</sup> | *Yok* | Sıra<sup>*</sup> |
|Zamanlanmış bir iş Blob Depolama içeriğini okur ve yeni bir Cosmos DB belgesi oluşturur. | Zamanlayıcı | Blob Depolama | Cosmos DB |
|Olay Izgara blob Depolama bir görüntü ve Cosmos DB bir belge okumak için bir e-posta göndermek için kullanılır. | Event Grid | Blob Depolama ve Cosmos DB | SendGrid |
| Excel sayfasını güncelleştirmek için Microsoft Graph kullanan bir webhook. | HTTP | *Yok* | Microsoft Graph |

<sup>\*</sup>Farklı kuyrukları temsil eder

Bu örnekler ayrıntılı olması amaçlanmaz, ancak tetikleyicileri ve bağlamaları birlikte nasıl kullanabileceğinizi göstermek için sağlanır.

###  <a name="trigger-and-binding-definitions"></a>Tetikleme ve bağlama tanımları

Tetikleyiciler ve bağlamalar geliştirme yaklaşımına bağlı olarak farklı tanımlanır.

| Platform | Tetikleyiciler ve bağlamalar tarafından yapılandırılır ... |
|-------------|--------------------------------------------|
| C# sınıfı kitaplığı | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;C# öznitelikleri ile dekorasyon yöntemleri ve parametreleri |
| Diğerleri (Azure portalı dahil) | &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;güncelleme [fonksiyonu.json](./functions-reference.md) ([şema](http://json.schemastore.org/function)) |

Portal bu yapılandırma için bir Kullanıcı Arabirimi sağlar, ancak işlevinizin **Tümleştir** sekmesi aracılığıyla kullanılabilir **Gelişmiş düzenleyiciyi** açarak dosyayı doğrudan düzenleme yapabilirsiniz.

.NET'te parametre türü giriş verileri için veri türünü tanımlar. Örneğin, bir `string` sıra tetikleyicisi metnine bağlamak için kullanın, ikili olarak okunacak bir bayt dizisi ve bir nesneye seriden arındırmak için özel bir tür.

JavaScript gibi dinamik olarak yazılmış diller için `dataType` *function.json* dosyasındaki özelliği kullanın. Örneğin, bir HTTP isteğinin içeriğini ikili biçimde `dataType` `binary`okumak için:

```json
{
    "dataType": "binary",
    "type": "httpTrigger",
    "name": "req",
    "direction": "in"
}
```

Diğer seçenekler `dataType` `stream` için `string`ve .

## <a name="binding-direction"></a>Bağlama yönü

Tüm tetikleyiciler ve bağlamalar `direction` [function.json](./functions-reference.md) dosyasında bir özelliğe sahiptir:

- Tetikleyiciler için, yön her zaman`in`
- Giriş ve çıkış bağlamaları kullanımı `in` ve`out`
- Bazı bağlamalar özel `inout`bir yönü destekler. Kullanıyorsanız, portaldaki **Tümleştir** sekmesi aracılığıyla yalnızca Gelişmiş düzenleyici kullanılabilir. **Advanced editor** `inout`

Tetikleyicileri ve bağlamaları yapılandırmak için [sınıf kitaplığındaki öznitelikleri](functions-dotnet-class-library.md) kullandığınızda, yön bir öznitelik oluşturucusunda veya parametre türünden çıkarılır.

## <a name="supported-bindings"></a>Desteklenen bağlamalar

[!INCLUDE [Full bindings table](../../includes/functions-bindings.md)]

Hangi bağlamaların önizlemede olduğu veya üretim kullanımı için onaylandığı hakkında bilgi için desteklenen [dillere](supported-languages.md)bakın.

## <a name="resources"></a>Kaynaklar
- [İfadeleri ve desenleri bağlama](./functions-bindings-expressions-patterns.md)
- [Azure İşlevi return değerini kullanma](./functions-bindings-return-value.md)
- [Bağlayıcı bir ifade nasıl kaydedilir?](./functions-bindings-register.md)
- Test:
  - [Azure İşlevleri'nde kodunuzu test etmeye yönelik stratejiler](functions-test-a-function.md)
  - [HTTP ile tetiklenmeyen bir işlevi el ile çalıştırma](functions-manually-run-non-http.md)
- [Bağlama hatalarını işleme](./functions-bindings-errors.md)

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure İşlevlerini bağlama uzantılarını kaydedin](./functions-bindings-register.md)
