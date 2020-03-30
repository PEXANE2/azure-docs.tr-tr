---
title: Azure Fonksiyonları tetikleyici ve bağlama örneği
description: Azure İşlev bağlamalarını yapılandırmayı öğrenin
author: craigshoemaker
ms.topic: reference
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: 8685c0fe02ad6c68918736e857c2015e2bfb4595
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74227237"
---
# <a name="azure-functions-trigger-and-binding-example"></a>Azure Fonksiyonları tetikleyici ve bağlama örneği

Bu makalede, bir Azure İşlevi'nde [tetikleyici ve bağlamaların](./functions-triggers-bindings.md) nasıl yapılandırılabildiğini gösterin.

Azure Sıra depolama alanında yeni bir ileti göründüğünde Azure Tablo depolama alanına yeni bir satır yazmak istediğinizi varsayalım. Bu senaryo bir Azure Kuyruk depolaması tetikleyicisi ve bir de Azure Tablo depolamasıçıkış bağlayıcısı kullanılarak gerçekleştirilir. 

Bu senaryo için bir *function.json* dosyası aşağıda verilmiştir. 

```json
{
  "bindings": [
    {
      "type": "queueTrigger",
      "direction": "in",
      "name": "order",
      "queueName": "myqueue-items",
      "connection": "MY_STORAGE_ACCT_APP_SETTING"
    },
    {
      "type": "table",
      "direction": "out",
      "name": "$return",
      "tableName": "outTable",
      "connection": "MY_TABLE_STORAGE_ACCT_APP_SETTING"
    }
  ]
}
```

Dizideki `bindings` ilk öğe Sıra depolama tetikleyicisidir. Ve `type` `direction` özellikleri tetikleyiciyi tanımlar. Özellik, `name` sıra iletisi içeriğini alan işlev parametresini tanımlar. İzlenecek sıranın adı içinde `queueName`ve bağlantı dizesi tarafından `connection`tanımlanan uygulama ayarında.

Dizideki `bindings` ikinci öğe Azure Tablo Depolama çıktısı bağlayıcısidır. Ve `type` `direction` özellikleri bağlama yı tanımlar. Özellik, `name` işlev inanın değerini kullanarak bu durumda işlevin yeni tablo satırını nasıl sağladığını belirtir. Tablonun adı içinde `tableName`ve bağlantı dizesi tarafından `connection`tanımlanan uygulama ayarında.

Azure portalında *function.json* içeriğini görüntülemek ve görüntülemek için, işlevinizin **Tümleştir** sekmesinde **Gelişmiş düzenleyici** seçeneğini tıklatın.

> [!NOTE]
> `connection` Değeri, bağlantı dizesini içeren bir uygulama ayarının adıdır, bağlantı dizesinin kendisini değil. *Bağlamalar, function.json'Un* hizmet sırlarını içermediği en iyi uygulamayı uygulamak için uygulama ayarlarında depolanan bağlantı dizelerini kullanır.

## <a name="c-script-example"></a>C# komut dosyası örneği

İşte bu tetikleyici ve bağlama ile çalışan C# komut dosyası kodu. Sıra iletisi içeriğini sağlayan parametrenin adının; `order` bu ad, `name` *function.json'daki* özellik değeri`order` 

```cs
#r "Newtonsoft.Json"

using Microsoft.Extensions.Logging;
using Newtonsoft.Json.Linq;

// From an incoming queue message that is a JSON object, add fields and write to Table storage
// The method return value creates a new row in Table Storage
public static Person Run(JObject order, ILogger log)
{
    return new Person() { 
            PartitionKey = "Orders", 
            RowKey = Guid.NewGuid().ToString(),  
            Name = order["Name"].ToString(),
            MobileNumber = order["MobileNumber"].ToString() };  
}
 
public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

## <a name="javascript-example"></a>JavaScript örneği

Aynı *function.json* dosyası JavaScript işlevi ile kullanılabilir:

```javascript
// From an incoming queue message that is a JSON object, add fields and write to Table Storage
// The second parameter to context.done is used as the value for the new row
module.exports = function (context, order) {
    order.PartitionKey = "Orders";
    order.RowKey = generateRandomId(); 

    context.done(null, order);
};

function generateRandomId() {
    return Math.random().toString(36).substring(2, 15) +
        Math.random().toString(36).substring(2, 15);
}
```

## <a name="class-library-example"></a>Sınıf kitaplığı örneği

Sınıf kitaplığında, aynı tetikleyici &mdash; ve bağlama bilgi sırası ve tablo adları, &mdash; depolama hesapları, giriş ve çıktı için işlev parametreleri, bir function.json dosyası yerine öznitelikler tarafından sağlanır. Bir örneği aşağıda verilmiştir:

```csharp
public static class QueueTriggerTableOutput
{
    [FunctionName("QueueTriggerTableOutput")]
    [return: Table("outTable", Connection = "MY_TABLE_STORAGE_ACCT_APP_SETTING")]
    public static Person Run(
        [QueueTrigger("myqueue-items", Connection = "MY_STORAGE_ACCT_APP_SETTING")]JObject order,
        ILogger log)
    {
        return new Person() {
                PartitionKey = "Orders",
                RowKey = Guid.NewGuid().ToString(),
                Name = order["Name"].ToString(),
                MobileNumber = order["MobileNumber"].ToString() };
    }
}

public class Person
{
    public string PartitionKey { get; set; }
    public string RowKey { get; set; }
    public string Name { get; set; }
    public string MobileNumber { get; set; }
}
```

Artık bir Azure Kuyruğu tarafından tetiklenen ve verileri Azure Tablo depolamasına aktaran bir çalışma işleviniz var.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure İşlevler bağlama ifade desenleri](./functions-bindings-expressions-patterns.md)
