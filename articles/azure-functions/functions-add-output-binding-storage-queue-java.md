---
title: Java işlevinizi Azure Depolama'ya bağlayın
description: Bir Kuyruk depolama çıktısı bağlama kullanarak HTTP tarafından tetiklenen bir Java işlevini Azure Depolama'ya nasıl bağlayabilirsiniz öğrenin.
author: KarlErickson
ms.author: karler
ms.date: 10/14/2019
ms.topic: quickstart
zone_pivot_groups: java-build-tools-set
ms.openlocfilehash: 8ae69bfa7ed00e310205332e05c071158c5fc9a3
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78272799"
---
# <a name="connect-your-java-function-to-azure-storage"></a>Java işlevinizi Azure Depolama'ya bağlayın

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

Bu makalede, [önceki quickstart makalesinde](functions-create-first-java-maven.md) oluşturduğunuz işlevin Azure Depolama kuyruğuyla nasıl tümleştirilen ekidir. Bu işleve eklediğiniz çıktı bağlama, bir HTTP isteğindeki verileri kuyruktaki bir iletiye yazar.

Çoğu bağlama, Bağlı hizmete erişmek için Işlevlerin kullandığı depolanmış bir bağlantı dizesi gerektirir. Bu bağlantıyı kolaylaştırmak için, işlev uygulamanızla oluşturduğunuz Depolama hesabını kullanırsınız. Bu hesaba bağlantı zaten adlı `AzureWebJobsStorage`bir uygulama ayarında depolanır.  

## <a name="prerequisites"></a>Ön koşullar

Bu makaleyi başlatmadan önce, [Java quickstart bölüm 1](functions-create-first-java-maven.md)adımları tamamlayın.

## <a name="download-the-function-app-settings"></a>Fonksiyon uygulama ayarlarını indirin

[!INCLUDE [functions-app-settings-download-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Uzantı demetlerini etkinleştirme

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Artık projenize Depolama çıktısı bağlamayı ekleyebilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

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

## <a name="add-code-that-uses-the-output-binding"></a>Çıkış bağlaması kullanan kod ekleme

Şimdi, işlev kodunuzdan çıktı bağlama yazmak için yeni `msg` parametreyi kullanabilirsiniz. Çıktı bağlama değeri eklemek `name` için başarı yanıtı önce kod aşağıdaki satırı ekleyin. `msg`

```java
msg.setValue(name);
```

Çıktı bağlama kullandığınızda, kimlik doğrulaması, sıra başvurusu almak veya veri yazmak için Azure Depolama SDK kodunu kullanmanız gerekmez. İşlevler çalışma zamanı ve sıra çıktısı bağlama bu görevleri sizin için yapar.

Yönteminiz `run` şimdi aşağıdaki örnek gibi görünmelidir:

```java
@FunctionName("HttpTrigger-Java")
public HttpResponseMessage run(
        @HttpTrigger(name = "req", methods = {HttpMethod.GET, HttpMethod.POST}, authLevel = AuthorizationLevel.FUNCTION) HttpRequestMessage<Optional<String>> request, 
        @QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") 
        OutputBinding<String> msg, final ExecutionContext context) {
    context.getLogger().info("Java HTTP trigger processed a request.");

    // Parse query parameter
    String query = request.getQueryParameters().get("name");
    String name = request.getBody().orElse(query);

    if (name == null) {
        return request.createResponseBuilder(HttpStatus.BAD_REQUEST).body("Please pass a name on the query string or in the request body").build();
    } else {
        // Write the name to the message queue. 
        msg.setValue(name);

        return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + name).build();
    }
}
```

## <a name="update-the-tests"></a>Testleri güncelleştirme

Arketip aynı zamanda bir dizi test oluşturduğundan, yöntem imzasındaki `msg` yeni parametreyi işlemek için bu testleri güncelleştirmeniz `run` gerekir.  

_Src/test/java_altında test kodunuzun konumuna göz atın, *Function.java* proje dosyasını `//Invoke` açın ve aşağıdaki kodla kod satırını değiştirin.

```java
@SuppressWarnings("unchecked")
final OutputBinding<String> msg = (OutputBinding<String>)mock(OutputBinding.class);

// Invoke
final HttpResponseMessage ret = new Function().run(req, msg, context);
``` 

Artık yeni çıktı bağlamayı yerel olarak denemeye hazırsınız.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Daha önce olduğu gibi, projeyi oluşturmak ve Işlevleri yerel olarak çalıştırma zamanını başlatmak için aşağıdaki komutu kullanın:

::: zone pivot="java-build-tools-maven"  
```bash
mvn clean package 
mvn azure-functions:run
```
::: zone-end

::: zone pivot="java-build-tools-gradle"  
```bash
gradle jar --info
gradle azureFunctionsRun
```
::: zone-end

> [!NOTE]  
> Host.json'da uzantı demetlerini etkinleştirdiğiniz için, [Depolama bağlama uzantısı](functions-bindings-storage-blob.md#add-to-your-functions-app) başlangıç sırasında diğer Microsoft bağlama uzantılarıyla birlikte karşıdan yüklendi ve sizin için yüklendi.

Daha önce olduğu gibi, yeni bir terminal penceresinde cURL kullanarak komut satırından işlevi tetikle:

```CMD
curl -w "\n" http://localhost:7071/api/HttpTrigger-Java --data AzureFunctions
```

Bu kez, çıktı bağlama depolama hesabınızda adlı `outqueue` bir sıra oluşturur ve aynı dize ile bir ileti ekler.

Ardından, yeni kuyruğa görüntülemek ve bir iletinin eklandığını doğrulamak için Azure CLI'yi kullanırsınız. Microsoft [Azure Depolama Gezgini'ni][Azure Storage Explorer] kullanarak veya [Azure portalında](https://portal.azure.com)da sıranızı görüntüleyebilirsiniz.

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

### <a name="redeploy-the-project"></a>Projeyi yeniden dağıtın 

Yayınlanan uygulamanızı güncellemek için aşağıdaki komutu yeniden çalıştırın:  

::: zone pivot="java-build-tools-maven"  
```bash
mvn azure-functions:deploy
```
::: zone-end

::: zone pivot="java-build-tools-gradle"  
```bash
gradle azureFunctionsDeploy
```
::: zone-end

Yine, dağıtılan işlevi sınamak için cURL kullanabilirsiniz. Daha önce olduğu `AzureFunctions` gibi, post isteğinin gövdesindeki değeri aşağıdaki örnekte olduğu gibi URL'ye iletin:

```bash
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpTrigger-Java?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Çıktı bağlamanın beklendiği gibi kuyrukta yeni bir ileti oluşturduğunu doğrulamak için [Depolama sıra iletisini](#query-the-storage-queue) yeniden inceleyebilirsiniz.

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

Depolama kuyruğuna veri yazmak için HTTP tetiklenen işlevinizi güncellediniz. Java ile Azure İşlevleri geliştirme hakkında daha fazla bilgi edinmek için [Azure İşlevler Java geliştirici kılavuzuna](functions-reference-java.md) ve [Azure İşlevlerini tetikleyenlere ve bağlamalara](functions-triggers-bindings.md)bakın. Java'daki tam Fonksiyon projeleri örnekleri için [Java İşlevleri örneklerine](/samples/browse/?products=azure-functions&languages=Java)bakın. 

Ardından, işlev uygulamanız için Uygulama Öngörüleri izlemeetkinleştirmelisiniz:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)


[Azure Storage Explorer]: https://storageexplorer.com/
