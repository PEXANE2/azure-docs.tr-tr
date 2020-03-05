---
title: Java işlevinizi Azure depolama 'ya bağlama
description: Bir kuyruk depolama çıkış bağlaması kullanarak HTTP ile tetiklenen bir Java işlevini Azure depolama 'ya bağlamayı öğrenin.
author: KarlErickson
ms.author: karler
ms.date: 10/14/2019
ms.topic: quickstart
zone_pivot_groups: java-build-tools-set
ms.openlocfilehash: 8ae69bfa7ed00e310205332e05c071158c5fc9a3
ms.sourcegitcommit: d45fd299815ee29ce65fd68fd5e0ecf774546a47
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2020
ms.locfileid: "78272799"
---
# <a name="connect-your-java-function-to-azure-storage"></a>Java işlevinizi Azure depolama 'ya bağlama

[!INCLUDE [functions-add-storage-binding-intro](../../includes/functions-add-storage-binding-intro.md)]

Bu makalede, [önceki hızlı başlangıç makalesinde](functions-create-first-java-maven.md) oluşturduğunuz Işlevi bir Azure depolama kuyruğu ile tümleştirme işlemi gösterilmektedir. Bu işleve eklediğiniz çıkış bağlaması, verileri bir HTTP isteğinden kuyruktaki bir iletiye yazar.

Çoğu bağlamanın, bağlı hizmete erişmek için kullandığı depolanan bir bağlantı dizesi gerekir. Bu bağlantıyı daha kolay hale getirmek için, işlev uygulamanızla oluşturduğunuz depolama hesabını kullanırsınız. Bu hesap bağlantısı zaten `AzureWebJobsStorage`adlı bir uygulama ayarında depolanıyor.  

## <a name="prerequisites"></a>Önkoşullar

Bu makaleye başlamadan önce, [Java hızlı](functions-create-first-java-maven.md)başlangıcının 1. bölümündeki adımları uygulayın.

## <a name="download-the-function-app-settings"></a>İşlev uygulaması ayarlarını indirin

[!INCLUDE [functions-app-settings-download-cli](../../includes/functions-app-settings-download-local-cli.md)]

## <a name="enable-extension-bundles"></a>Uzantı paketlerini etkinleştir

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

Artık, depolama çıkış bağlamasını projenize ekleyebilirsiniz.

## <a name="add-an-output-binding"></a>Çıktı bağlaması ekleme

Java projesinde bağlamalar, işlev yönteminde bağlama ek açıklamaları olarak tanımlanır. *Function. JSON* dosyası daha sonra bu ek açıklamaları temel alınarak otomatik olarak oluşturulur.

_Src/Main/Java_altındaki işlev kodunuzun konumuna gidin, *function. Java* proje dosyasını açın ve `run` yöntemi tanımına aşağıdaki parametreyi ekleyin:

```java
@QueueOutput(name = "msg", queueName = "outqueue", connection = "AzureWebJobsStorage") OutputBinding<String> msg
```

`msg` parametresi, işlev tamamlandığında çıkış bağlamasına ileti olarak yazılmış dizelerin koleksiyonunu temsil eden bir [`OutputBinding<T>`](/java/api/com.microsoft.azure.functions.outputbinding) türüdür. Bu durumda, çıktı `outqueue`adlı bir depolama kuyruğudur. Depolama hesabı için bağlantı dizesi `connection` yöntemi tarafından ayarlanır. Bağlantı dizesinin kendisi yerine, depolama hesabı bağlantı dizesini içeren uygulama ayarını geçirirsiniz.

`run` yöntemi tanımı aşağıdaki örnekteki gibi görünmelidir:  

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

Şimdi, işlev kodunuzun çıkış bağlamasına yazmak için yeni `msg` parametresini kullanabilirsiniz. `name` değerini `msg` çıkış bağlamasına eklemek için başarı yanıtından önce aşağıdaki kod satırını ekleyin.

```java
msg.setValue(name);
```

Çıkış bağlamayı kullandığınızda, kimlik doğrulaması, kuyruk başvurusu alma veya veri yazma için Azure depolama SDK kodunu kullanmanız gerekmez. Işlevler çalışma zamanı ve kuyruk çıkış bağlaması bu görevleri sizin için işler.

`run` yönteminiz aşağıdaki örnekteki gibi görünmelidir:

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

Ayrıca, aynı zamanda bir test kümesi oluşturduğundan, bu testleri `run` yöntemi imzasında yeni `msg` parametresini işleyecek şekilde güncelleştirmeniz gerekir.  

_Src/test/Java_altındaki test kodunuzun konumuna gidin, *function. Java* proje dosyasını açın ve `//Invoke` altındaki kod satırını aşağıdaki kodla değiştirin.

```java
@SuppressWarnings("unchecked")
final OutputBinding<String> msg = (OutputBinding<String>)mock(OutputBinding.class);

// Invoke
final HttpResponseMessage ret = new Function().run(req, msg, context);
``` 

Artık yeni çıkış bağlamasını yerel olarak denemeye hazırsınız.

## <a name="run-the-function-locally"></a>İşlevi yerel olarak çalıştırma

Daha önce olduğu gibi, projeyi derlemek ve Işlevler çalışma zamanını yerel olarak başlatmak için aşağıdaki komutu kullanın:

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
> Host. JSON içindeki uzantı paketlerini etkinleştirdiyseniz, [depolama bağlama uzantısı](functions-bindings-storage-blob.md#add-to-your-functions-app) diğer Microsoft bağlama uzantılarıyla birlikte başlangıç sırasında sizin için indirilir ve yüklenir.

Daha önce olduğu gibi, yeni bir Terminal penceresinde kıvrımlı kullanarak işlevi komut satırından tetikleyin:

```CMD
curl -w "\n" http://localhost:7071/api/HttpTrigger-Java --data AzureFunctions
```

Bu kez, çıkış bağlama aynı zamanda depolama hesabınızda `outqueue` adlı bir kuyruk oluşturur ve bu dizeyi içeren bir ileti ekler.

Ardından, yeni kuyruğu görüntülemek ve bir iletinin eklendiğini doğrulamak için Azure CLı 'yi kullanırsınız. Kuyruğunuzu, [Microsoft Azure Depolama Gezgini][Azure Storage Explorer] veya [Azure Portal](https://portal.azure.com)kullanarak da görüntüleyebilirsiniz.

[!INCLUDE [functions-storage-account-set-cli](../../includes/functions-storage-account-set-cli.md)]

[!INCLUDE [functions-query-storage-cli](../../includes/functions-query-storage-cli.md)]

### <a name="redeploy-the-project"></a>Projeyi yeniden dağıtın 

Yayınlanan uygulamanızı güncelleştirmek için aşağıdaki komutu yeniden çalıştırın:  

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

Yine, dağıtılan işlevi test etmek için kıvrımlı kullanabilirsiniz. Daha önce olduğu gibi, aşağıdaki örnekte olduğu gibi POST isteğinin gövdesinde `AzureFunctions` değerini URL 'ye geçirin:

```bash
curl -w "\n" https://fabrikam-functions-20190929094703749.azurewebsites.net/api/HttpTrigger-Java?code=zYRohsTwBlZ68YF.... --data AzureFunctions
```

Çıkış bağlamasının, kuyrukta beklendiği gibi yeni bir ileti üretdiğini doğrulamak için [depolama kuyruğu iletisini yeniden inceleyebilirsiniz](#query-the-storage-queue) .

[!INCLUDE [functions-cleanup-resources](../../includes/functions-cleanup-resources.md)]

## <a name="next-steps"></a>Sonraki adımlar

HTTP ile tetiklenen işlevinizi bir depolama kuyruğuna veri yazmak için güncelleştirdiniz. Java ile Azure Işlevleri geliştirme hakkında daha fazla bilgi edinmek için bkz. [Azure Işlevleri Java geliştirici kılavuzu](functions-reference-java.md) ve [Azure işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md). Java 'daki tüm Işlev projelerinin örnekleri için bkz. [Java işlevleri örnekleri](/samples/browse/?products=azure-functions&languages=Java). 

Sonra, işlev uygulamanız için Application Insights izlemeyi etkinleştirmelisiniz:

> [!div class="nextstepaction"]
> [Application Insights tümleştirmesini etkinleştirme](functions-monitoring.md#manually-connect-an-app-insights-resource)


[Azure Storage Explorer]: https://storageexplorer.com/
