---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 2b2c043e70aac14c7fc6f0b58aae257624b05d13
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80673289"
---
Java projesinde, bağlamalar işlev yöntemine bağlama ek açıklamaları olarak tanımlanır. *Function.json* dosyası daha sonra bu ek açıklamalara göre otomatik olarak oluşturulur.

_Src/main/java_altında işlev kodunuzu konumuna göz atın, *Function.java* proje dosyasını açın `run` ve yöntem tanımına aşağıdaki parametreyi ekleyin:

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

`msg` Parametre, işlev [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding) tamamlandığında çıktı bağlamasına ileti olarak yazılan dizeler koleksiyonunu temsil eden bir türdür. Bu durumda, çıktı adlı `outqueue`bir depolama sırasıdır. Depolama hesabının bağlantı dizesi `connection` yönteme göre ayarlanır. Bağlantı dizesi yerine, Depolama hesabı bağlantı dizesini içeren uygulama ayarını geçersiniz.

`run` Yöntem tanımı şimdi aşağıdaki örnek gibi görünmelidir:  

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION)  
        HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    ...
}
```