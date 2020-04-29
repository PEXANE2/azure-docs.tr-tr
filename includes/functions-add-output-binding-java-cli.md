---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 2b2c043e70aac14c7fc6f0b58aae257624b05d13
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80673289"
---
Java projesinde bağlamalar, işlev yönteminde bağlama ek açıklamaları olarak tanımlanır. *Function. JSON* dosyası daha sonra bu ek açıklamaları temel alınarak otomatik olarak oluşturulur.

_Src/Main/Java_altındaki işlev kodunuzun konumuna gidin, *function. Java* proje dosyasını açın ve `run` Yöntem tanımına aşağıdaki parametreyi ekleyin:

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

`msg` Parametresi, işlev tamamlandığında [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding) çıkış bağlamaya ileti olarak yazılmış dizeler koleksiyonunu temsil eden bir türdür. Bu durumda, çıktı adlı `outqueue`bir depolama kuyruğudur. Depolama hesabı için bağlantı dizesi `connection` yöntemi tarafından ayarlanır. Bağlantı dizesinin kendisi yerine, depolama hesabı bağlantı dizesini içeren uygulama ayarını geçirirsiniz.

`run` Yöntem tanımı şimdi aşağıdaki örnekteki gibi görünmelidir:  

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