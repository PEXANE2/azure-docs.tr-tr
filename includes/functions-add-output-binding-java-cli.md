---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/25/2020
ms.author: glenga
ms.openlocfilehash: 2b2c043e70aac14c7fc6f0b58aae257624b05d13
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "80673289"
---
Java projesinde bağlamalar, işlev yönteminde bağlama ek açıklamaları olarak tanımlanır. Dosyadaki *function.js* bu ek açıklamaları temel alınarak otomatik olarak oluşturulur.

_Src/Main/Java_ altındaki işlev kodunuzun konumuna gidin, *function. Java* proje dosyasını açın ve Yöntem tanımına aşağıdaki parametreyi ekleyin `run` :

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

`msg`Parametresi [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding) , işlev tamamlandığında çıkış bağlamaya ileti olarak yazılmış dizeler koleksiyonunu temsil eden bir türdür. Bu durumda, çıktı adlı bir depolama kuyruğudur `outqueue` . Depolama hesabı için bağlantı dizesi yöntemi tarafından ayarlanır `connection` . Bağlantı dizesinin kendisi yerine, depolama hesabı bağlantı dizesini içeren uygulama ayarını geçirirsiniz.

`run`Yöntem tanımı şimdi aşağıdaki örnekteki gibi görünmelidir:  

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