---
title: Cosmos DB bağlantı ilkesi için Azure Fonksiyonları tetikleyici
description: Cosmos DB için Azure Fonksiyonları tetikleyicisi tarafından kullanılan bağlantı ilkesini nasıl yapılandırılamayın
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: maquaran
ms.openlocfilehash: 953121a9a15d4fef56d381e3aab85329fadacce2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77604956"
---
# <a name="how-to-configure-the-connection-policy-used-by-azure-functions-trigger-for-cosmos-db"></a>Cosmos DB için Azure Fonksiyonları tetikleyicisi tarafından kullanılan bağlantı ilkesi nasıl yapılandırılabilen

Bu makalede, Cosmos DB'nin Azure Cosmos hesabınıza bağlanması için Azure Fonksiyonları tetikleyicisini kullanırken bağlantı ilkesini nasıl yapılandırabileceğiniz açıklanmaktadır.

## <a name="why-is-the-connection-policy-important"></a>Bağlantı ilkesi neden önemlidir?

İki bağlantı modu vardır - Doğrudan mod ve Ağ Geçidi modu. Bu bağlantı modları hakkında daha fazla bilgi edinmek için [performans ipuçları](./performance-tips.md#networking) makalesine bakın. Varsayılan olarak, **Ağ Geçidi** Cosmos DB için Azure İşlevler tetikleyicisindeki tüm bağlantıları kurmak için kullanılır. Ancak, performans odaklı senaryolar için en iyi seçenek olmayabilir.

## <a name="changing-the-connection-mode-and-protocol"></a>Bağlantı modunu ve protokolü değiştirme

İstemci bağlantı ilkesini yapılandırmak için iki temel yapılandırma ayarı vardır – **bağlantı modu** ve **bağlantı protokolü.** Cosmos DB ve tüm [Azure Cosmos DB bağlamaları](../azure-functions/functions-bindings-cosmosdb-v2-output.md)için Azure İşlevler tetikleyicisi tarafından kullanılan varsayılan bağlantı modunu ve protokolünü değiştirebilirsiniz. Varsayılan ayarları değiştirmek için, dosyayı `host.json` Azure İşlevler projenizde veya Azure İşlevler Uygulamanızda bulmanız ve aşağıdaki [ek ayarı](../azure-functions/functions-bindings-cosmosdb-v2-output.md#hostjson-settings)eklemeniz gerekir:

```js
{
  "cosmosDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

İstenilen bağlantı modu (Doğrudan veya `protocol` Ağ Geçidi) ve istenilen bağlantı protokolü (Tcp veya Https) olması gereken yerlerde. `connectionMode` 

Azure İşlevler projeniz Azure İşlevleri V1 çalışma zamanı ile çalışıyorsa, `cosmosDB`yapılandırmanın küçük bir ad farkı varsa, aşağıdakiler yerine kullanmanız `documentDB` gerekir:

```js
{
  "documentDB": {
    "connectionMode": "Direct",
    "protocol": "Tcp"
  }
}
```

> [!NOTE]
> Azure İşlevler Tüketim Planı Barındırma planıyla çalışırken, her örnekte koruyabileceği Soket Bağlantısı miktarında bir sınır vardır. Direct / TCP modu ile çalışırken, tasarım gereği daha fazla bağlantı oluşturulur ve [Tüketim Planı sınırına](../azure-functions/manage-connections.md#connection-limit)ulaşabilirsiniz , bu durumda Ağ Geçidi modunu kullanabilir veya Azure İşlevlerinizi [Uygulama Hizmet Modu'nda](../azure-functions/functions-scale.md#app-service-plan)çalıştırabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure İşlevlerinde bağlantı sınırları](../azure-functions/manage-connections.md#connection-limit)
* [Azure Cosmos DB performans ipuçları](./performance-tips.md)
* [Kod örnekleri](https://github.com/ealsur/serverless-recipes/tree/master/connectionmode)
