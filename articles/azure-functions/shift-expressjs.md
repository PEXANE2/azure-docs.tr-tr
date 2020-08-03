---
title: Express.js 'den Azure Işlevlerine kaydırma
description: Express.js uç noktalarını Azure Işlevleri 'ne yeniden düzenleme hakkında bilgi edinin.
author: craigshoemaker
ms.topic: conceptual
ms.date: 07/31/2020
ms.author: cshoe
ms.openlocfilehash: 3e38fae84f63f4867befbc3695242dc62dc8dbe8
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87502656"
---
# <a name="shifting-from-expressjs-to-azure-functions"></a>Express.js 'den Azure Işlevlerine kaydırma

Web geliştiricileri için en popüler Node.js çerçevelerinden biridir ve API uç noktalarına sunan uygulamalar oluşturmak için harika bir seçim olmaya devam etmektedir. Express.js

Kodu sunucusuz bir mimariye geçirirken Express.js uç noktaları yeniden düzenleme aşağıdaki alanı etkiler:

- **Ara yazılım**: Express.js özellikler, güçlü bir ara yazılım koleksiyonudur. Birçok ara yazılım modülü artık Azure Işlevleri ve [azure API Management](../api-management/api-management-key-concepts.md) özellikleri 'nin açık olması için gerekli değildir. Uç noktaları geçirmeden önce önemli ara yazılım tarafından işlenen herhangi bir mantığı çoğaltabilmeniz veya bunun yerine değiştirilmesini sağlayın.

- **Farklı API 'ler**: hem istekleri hem de yanıtları işlemek IÇIN kullanılan API, Azure işlevleri ve Express.js arasında farklılık gösterir. Aşağıdaki örnekte gerekli değişiklikler ayrıntılı olarak verilmiştir.

- **Varsayılan yol**: Azure işlevleri uç noktaları, varsayılan olarak yol altında gösterilir `api` . Yönlendirme kuralları [ `routePrefix` dosyadaki _host.js_ ](./functions-bindings-http-webhook-output.md#hostjson-settings)aracılığıyla yapılandırılabilir.

- **Yapılandırma ve kurallar**: bir IŞLEVLER uygulaması http fiillerini tanımlamak, güvenlik ilkelerini tanımlamak ve işlevin [giriş ve çıkışını](./functions-triggers-bindings.md)yapılandırmak için dosyadaki _function.js_ kullanır. Varsayılan olarak, işlev dosyalarını içeren klasör adı, uç nokta adını tanımlar, ancak adı `route` dosyadaki [function.js](./functions-bindings-http-webhook-trigger.md#customize-the-http-endpoint) özelliği aracılığıyla değiştirebilirsiniz.

> [!TIP]
> Etkileşimli öğretici [yeniden düzenleme Node.js ve Azure işlevleri Ile sunucusuz API 'Lere hızlı API 'ler](https://docs.microsoft.com/learn/modules/shift-nodejs-express-apis-serverless/)aracılığıyla daha fazla bilgi edinin.

## <a name="example"></a>Örnek

### <a name="expressjs"></a>Express.js

Aşağıdaki örnekte, tipik bir Express.js `GET` uç noktası gösterilmektedir.

```javascript
// server.js
app.get('/hello', (req, res) => {
  try {
    res.send("Success!");
  } catch(error) {
    const err = JSON.stringify(error);
    res.status(500).send(`Request error. ${err}`);
  }
});
```

Öğesine bir `GET` istek gönderildiğinde `/hello` , `HTTP 200` içeren bir yanıt `Success` döndürülür. Uç nokta bir hatayla karşılaşırsa, yanıt `HTTP 500` hata ayrıntılardır.

### <a name="azure-functions"></a>Azure İşlevleri

Azure Işlevleri yapılandırma ve kod dosyalarını her bir işlev için tek bir klasörde düzenler. Varsayılan olarak klasörün adı, işlev adını belirler.

Örneğin, adlı bir işlev `hello` aşağıdaki dosyaları içeren bir klasöre sahiptir.

``` files
| - hello
|  - function.json
|  - index.js
```

Aşağıdaki örnek, Azure Işlevleri ile birlikte yukarıdaki Express.js uç noktası ile aynı sonucu uygular.

# <a name="javascript"></a>[JavaScript](#tab/javascript)

```javascript
// hello/index.js
module.exports = async function (context, req) {
  try {
    context.res = { body: "Success!" };
  } catch(error) {
    const err = JSON.stringify(error);
    context.res = {
      status: 500,
      body: `Request error. ${err}`
    };
  }
};
```

# <a name="typescript"></a>[TypeScript](#tab/typescript)

```typescript
// hello/index.ts
import { AzureFunction, Context, HttpRequest } from "@azure/functions";

const httpTrigger: AzureFunction = async function (context: Context, req: HttpRequest): Promise<void> {
  try {
    context.res = { body: "Success!" };
  } catch (error) {
    const err = JSON.stringify(error);
    context.res = {
      status: 500,
      body: `Request error. ${err}`,
    };
  }
};

export default httpTrigger;
```

---

IŞLEVLERE taşırken aşağıdaki değişiklikler yapılır:

- **Modül:** İşlev kodu bir JavaScript modülü olarak uygulanır.

- **Bağlam ve yanıt nesnesi**:, [`context`](./functions-reference-node.md#context-object) işlevin çalışma zamanıyla iletişim kurmanızı sağlar. Bağlamdan, istek verilerini okuyabilir ve işlevin yanıtını ayarlayabilirsiniz. Zaman uyumlu kod `context.done()` , yürütmeyi tamamlamaya yönelik çağrı yapmanızı gerektirir, ancak `asyc` işlevler isteği örtülü olarak çözer.

- **Adlandırma kuralı**: Azure işlevleri dosyalarını içerecek şekilde kullanılan klasör adı, varsayılan olarak uç nokta adı olarak kullanılır (Bu, [üzerindefunction.js](./functions-bindings-http-webhook-trigger.md#customize-the-http-endpoint)geçersiz kılınabilir).

- **Yapılandırma**: http fiillerini, veya gibi dosyada [function.js](./functions-bindings-http-webhook-trigger.md#customize-the-http-endpoint) tanımlarsınız `POST` `PUT` .

Dosyasında aşağıdaki _function.js_ , işlevi için yapılandırma bilgilerini barındırır.

```json
{
  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": ["get"]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "res"
    }
  ]
}
```

Dizide tanımlayarak `get` `methods` , işlevi http istekleri için kullanılabilir `GET` . API 'nize destek isteklerini kabul etmek istiyorsanız `POST` , `post` Array 'e de ekleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Etkileşimli öğretici [yeniden düzenleme Node.js ve Azure işlevleri Ile sunucusuz API 'Lere hızlı API 'ler](https://docs.microsoft.com/learn/modules/shift-nodejs-express-apis-serverless/) hakkında daha fazla bilgi edinin
