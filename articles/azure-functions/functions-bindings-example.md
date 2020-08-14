---
title: Azure İşlevleri tetikleyici ve bağlama örneği
description: Azure Işlev bağlamalarını yapılandırmayı öğrenin
author: craigshoemaker
ms.topic: reference
ms.custom: devx-track-csharp
ms.date: 02/18/2019
ms.author: cshoe
ms.openlocfilehash: c95524a5de6696bd0ffe7463451d152a9d3a19b4
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88205968"
---
# <a name="azure-functions-trigger-and-binding-example"></a>Azure İşlevleri tetikleyici ve bağlama örneği

Bu makalede bir Azure Işlevinde [tetikleyici ve bağlamaların](./functions-triggers-bindings.md) nasıl yapılandırılacağı gösterilir.

Azure kuyruk depolamada yeni bir ileti göründüğünde Azure Tablo depolama alanına yeni bir satır yazmak istediğinizi varsayalım. Bu senaryo bir Azure Kuyruk depolaması tetikleyicisi ve bir de Azure Tablo depolamasıçıkış bağlayıcısı kullanılarak gerçekleştirilir. 

Bu senaryo için dosyada bir *function.js* . 

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

Dizideki ilk öğe `bindings` kuyruk depolama tetikleyicisinin bir öğesidir. `type`Ve `direction` özellikleri tetikleyiciyi belirler. `name`Özelliği, kuyruk iletisi içeriğini alan işlev parametresini tanımlar. İzlenecek kuyruğun adı `queueName` ' nda, bağlantı dizesi tarafından tanımlanan uygulama ayarıdır `connection` .

Dizideki ikinci öğe, `bindings` Azure Tablo depolama çıkış bağlamadır. `type`Ve `direction` özellikleri bağlamayı belirler. `name`Özelliği, işlevin yeni tablo satırını nasıl sağladığını belirtir, bu durumda işlev dönüş değeri ' ni kullanarak. Tablonun adı `tableName` ve bağlantı dizesi tarafından tanımlanan uygulama ayarıdır `connection` .

Azure portal *function.js* içeriğini görüntülemek ve düzenlemek için Işlevinizin **tümleştirin** sekmesinde **Gelişmiş Düzenleyici** seçeneğine tıklayın.

> [!NOTE]
> Değeri bağlantı dizesinin `connection` kendisini değil bağlantı dizesini içeren bir uygulama ayarının adıdır. Bağlamalar, *function.json* 'un hizmet gizli dizileri içermediğinden en iyi uygulamaları zorlamak için uygulama ayarlarında depolanan bağlantı dizelerini kullanır.

## <a name="c-script-example"></a>C# betiği örneği

Bu tetikleyici ve bağlama ile birlikte çalışarak C# betik kodu aşağıda verilmiştir. Kuyruk iletisi içeriğini sağlayan parametre adının olduğunu ve `order` `name` *function.jsüzerinde* Özellik değeri olduğundan, bu adın gerekli olduğunu unutmayın. `order` 

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

Dosyadaki aynı *function.js* bir JavaScript işleviyle birlikte kullanılabilir:

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

Bir sınıf kitaplığında, aynı tetikleyici ve bağlama bilgileri &mdash; kuyruğu ve tablo adları, depolama hesapları, giriş ve çıkış için işlev parametreleri, &mdash; bir function.jsdosyası yerine öznitelikler tarafından sağlanır. Aşağıda bir örnek verilmiştir:

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

Artık bir Azure kuyruğu tarafından tetiklenen ve Azure Tablo depolama alanına veri çıktısı veren bir çalışma işleviniz var.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Işlevleri bağlama ifadesi desenleri](./functions-bindings-expressions-patterns.md)
