---
title: Cosmos DB bağlantı ilkesi için Azure Işlevleri tetikleyicisi
description: Cosmos DB için Azure Işlevleri tetikleyicisi tarafından kullanılan bağlantı ilkesini yapılandırma hakkında bilgi edinin
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 953121a9a15d4fef56d381e3aab85329fadacce2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77604956"
---
# <a name="how-to-configure-the-connection-policy-used-by-azure-functions-trigger-for-cosmos-db"></a>Cosmos DB için Azure Işlevleri tetikleyicisi tarafından kullanılan bağlantı ilkesini yapılandırma

Bu makalede, Azure Cosmos hesabınıza bağlanmak üzere Cosmos DB için Azure Işlevleri tetikleyicisi 'ni kullanırken bağlantı ilkesini nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="why-is-the-connection-policy-important"></a>Bağlantı ilkesi neden önemlidir?

İki bağlantı modu vardır-doğrudan mod ve ağ geçidi modu. Bu bağlantı modları hakkında daha fazla bilgi için bkz. [Performans ipuçları](./performance-tips.md#networking) makalesi. Varsayılan olarak, **ağ geçidi** , Cosmos DB Için Azure işlevleri tetikleyicisinde tüm bağlantıları oluşturmak için kullanılır. Ancak, performans odaklı senaryolar için en iyi seçenek olmayabilir.

## <a name="changing-the-connection-mode-and-protocol"></a>Bağlantı modunu ve protokolünü değiştirme

İstemci bağlantı ilkesini yapılandırmak için kullanılabilecek iki temel yapılandırma ayarı vardır: **bağlantı modu** ve **bağlantı protokolü**. Cosmos DB için Azure Işlevleri tetikleyicisi tarafından kullanılan varsayılan bağlantı modunu ve protokolünü ve tüm [Azure Cosmos DB bağlamalarını](../azure-functions/functions-bindings-cosmosdb-v2-output.md)değiştirebilirsiniz. Varsayılan ayarları değiştirmek için Azure Işlevleri projenizde veya Azure Işlevleri uygulamanızda `host.json` dosyayı bulmanız ve aşağıdaki [ek ayarı](../azure-functions/functions-bindings-cosmosdb-v2-output.md#hostjson-settings)eklemeniz gerekir:

```js
{
  "cosmosDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

Burada `connectionMode` istenen bağlantı moduna (doğrudan veya ağ geçidine) ve `protocol` Istenen bağlantı protokolüne (TCP veya https) sahip olmanız gerekir. 

Azure Işlevleri projeniz Azure Işlevleri v1 çalışma zamanı ile çalışıyorsa, yapılandırmanın küçük bir ad farkı vardır; bunun yerine şunu kullanmalısınız `documentDB` `cosmosDB`:

```js
{
  "documentDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

> [!NOTE]
> Azure Işlevleri tüketim planı barındırma planı ile çalışırken her örnek, sürdüreme soket bağlantısı miktarına göre bir sınıra sahiptir. Doğrudan/TCP moduyla çalışırken, Tasarım yaparak daha fazla bağlantı oluşturulur ve [Tüketim planı sınırına](../azure-functions/manage-connections.md#connection-limit)ulaşarak, ağ geçidi modunu kullanabilir veya Azure işlevlerinizi [App Service modunda](../azure-functions/functions-scale.md#app-service-plan)çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Işlevlerinde bağlantı sınırları](../azure-functions/manage-connections.md#connection-limit)
* [Azure Cosmos DB performans ipuçları](./performance-tips.md)
* [Kod örnekleri](https://github.com/ealsur/serverless-recipes/tree/master/connectionmode)
