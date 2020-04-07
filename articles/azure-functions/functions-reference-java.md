---
title: Azure İşlevler için Java geliştirici başvurusu
description: Java ile işlevlerin nasıl geliştirildiğini öğrenin.
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: 4b1f39ff4fd48a3ed99b34391e9cc6efdad86a5d
ms.sourcegitcommit: b129186667a696134d3b93363f8f92d175d51475
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80672996"
---
# <a name="azure-functions-java-developer-guide"></a>Azure İşlevler Java geliştirici kılavuzu

Azure Fonksiyonları çalışma süresi [Java SE 8 LTS 'yi (zulu8.31.0.2-jre8.0.181-win_x64)](https://repos.azul.com/azure-only/zulu/packages/zulu-8/8u181/)destekler. Bu kılavuz, Azure İşlevlerini Java ile yazmanın incelikleri hakkında bilgi içerir.

Diğer dillerde olduğu gibi, Bir İşlev Uygulamasının bir veya daha fazla işlevi olabilir. Java işlevi, `public` ek açıklama `@FunctionName`ile dekore edilmiş bir yöntemdir. Bu yöntem, java işlevinin girişini tanımlar ve belirli bir pakette benzersiz olmalıdır. Java'da yazılmış bir Fonksiyon Uygulaması, birden çok genel `@FunctionName`yöntemle açıklamalı birden çok sınıfa sahip olabilir.

Bu makalede, [Azure İşlevler geliştirici başvurularını](functions-reference.md)zaten okuduğunuz varsayar. Ayrıca [Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java) veya [Maven](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java)kullanarak, ilk işlevi oluşturmak için Fonksiyonları quickstart tamamlamanız gerekir.

## <a name="programming-model"></a>Programlama modeli 

[Tetikleyiciler ve bağlama kavramları](functions-triggers-bindings.md) Azure İşlevleri için temeldir. Tetikleyiciler kodunuzu yürütmeyi başlatın. Bağlamalar, özel veri erişim kodu yazmak zorunda kalmadan verileri bir işlevden veri aktarıp döndürmeniz için bir yol verir.

## <a name="create-java-functions"></a>Java işlevleri oluşturma

Java işlevlerinin oluşturulmasını kolaylaştırmak için, belirli bir işlev tetikleyicisi olan projeler oluşturmanıza yardımcı olmak için önceden tanımlanmış Java şablonları kullanan Maven tabanlı takım oluşturma ve arketipler vardır.    

### <a name="maven-based-tooling"></a>Maven tabanlı takım

Aşağıdaki geliştirici ortamlarında Java işlev projeleri oluşturmanıza olanak tanıyan Azure İşlevleri aracı vardır: 

+ [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions)
+ [Eclipse](functions-create-maven-eclipse.md)
+ [IntelliJ](functions-create-maven-intellij.md)

Yukarıdaki makale bağlantıları, seçtiğiniz IDE'yi kullanarak ilk işlevlerinizi nasıl oluşturabileceğinizi gösterir. 

### <a name="project-scaffolding"></a>Proje İskelesi

Terminalden komut satırı geliştirmeyi tercih ederseniz, Java tabanlı işlev projelerini iskeleye `Apache Maven` bağlamanın en basit yolu arketipler kullanmaktır. Azure Fonksiyonları için Java Maven arketipi aşağıdaki _groupId_altında yayınlanır:_artifactId_: [com.microsoft.azure:azure-functions-archetype](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-archetype/). 

Aşağıdaki komut, bu arketip kullanarak yeni bir Java işlev projesi oluşturur:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

Bu arketip kullanmaya başlamak için [Java quickstart'ına](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java)bakın. 

## <a name="create-kotlin-functions-preview"></a>Kotlin işlevleri oluşturma (önizleme)

Kotlin işlevleri ni oluşturmak için bir Maven arketipi de vardır. Bu arketip, şu anda önizleme, aşağıdaki _groupId_altında yayınlanır:_artifactId_: [com.microsoft.azure:azure-functions-kotlin-archetype](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-kotlin-archetype/). 

Aşağıdaki komut, bu arketip kullanarak yeni bir Java işlev projesi oluşturur:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-kotlin-archetype
```

Bu arketip kullanmaya başlamak için [Kotlin quickstart'a](functions-create-first-kotlin-maven.md)bakın.

## <a name="folder-structure"></a>Klasör yapısı

Azure İşlevler Java projesinin klasör yapısı aşağıda veda edilmiştir:

```
FunctionsProject
 | - src
 | | - main
 | | | - java
 | | | | - FunctionApp
 | | | | | - MyFirstFunction.java
 | | | | | - MySecondFunction.java
 | - target
 | | - azure-functions
 | | | - FunctionApp
 | | | | - FunctionApp.jar
 | | | | - host.json
 | | | | - MyFirstFunction
 | | | | | - function.json
 | | | | - MySecondFunction
 | | | | | - function.json
 | | | | - bin
 | | | | - lib
 | - pom.xml
```

_* Kotlin projesi hala Maven olduğu için çok benzer görünüyor_

İşlev uygulamasını yapılandırmak için paylaşılan bir [ana bilgisayar.json](functions-host-json.md) dosyası kullanabilirsiniz. Her işlevin kendi kod dosyası (.java) ve bağlama yapılandırma dosyası (function.json) vardır.

Bir projeye birden fazla işlev koyabilirsiniz. Fonksiyonlarınızı ayrı kavanozlara koymaktan kaçının. `FunctionApp` Hedef dizindeki, Azure'daki işlev uygulamanıza dağıtılan şeydir.

## <a name="triggers-and-annotations"></a>Tetikleyiciler ve ek açıklamalar

 Işlevler, http isteği, zamanlayıcı veya veri güncelleştirmesi gibi bir tetikleyici tarafından çağrılır. Bir veya daha fazla çıktı üretmek için işlevinizin bu tetikleyiciyi ve diğer girişleri işlemesi gerekir.

Giriş ve çıktıları yöntemlerinize bağlamak için [com.microsoft.azure.functions.annotation.*](/java/api/com.microsoft.azure.functions.annotation) paketinde yer alan Java ek açıklamalarını kullanın. Daha fazla bilgi için [Java başvuru dokümanlarına](/java/api/com.microsoft.azure.functions.annotation)bakın.

> [!IMPORTANT] 
> Azure Blob depolama, Azure Sıra depolama alanı veya Azure Tablo depolama sını yerel olarak çalıştırmak için [yerel ayarlar.json'unuzda](/azure/azure-functions/functions-run-local#local-settings-file) bir Azure Depolama hesabı yapılandırmanız gerekir.

Örnek:

```java
public class Function {
    public String echo(@HttpTrigger(name = "req", 
      methods = {"post"},  authLevel = AuthorizationLevel.ANONYMOUS) 
        String req, ExecutionContext context) {
        return String.format(req);
    }
}
```

Burada `function.json` [azure-functions-maven-plugin](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin)tarafından karşılık gelen oluşturulur:

```json
{
  "scriptFile": "azure-functions-example.jar",
  "entryPoint": "com.example.Function.echo",
  "bindings": [
    {
      "type": "httpTrigger",
      "name": "req",
      "direction": "in",
      "authLevel": "anonymous",
      "methods": [ "post" ]
    },
    {
      "type": "http",
      "name": "$return",
      "direction": "out"
    }
  ]
}

```

## <a name="jdk-runtime-availability-and-support"></a>JDK çalışma zamanı kullanılabilirliği ve desteği 

Java işlev uygulamalarının yerel gelişimi için, [Azul Systems'den](https://www.azul.com/downloads/azure-only/zulu/)Azure Java 8 JDK'ler [için Azul Zulu Enterprise'ı](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) indirin ve kullanın. Azure İşlevleri, işlev uygulamalarınızı buluta dağıttığınızda Azul Java 8 JDK çalışma zamanını kullanır.

JDK'lar ve işlev uygulamalarıyla ilgili sorunlar için [Azure desteği,](https://azure.microsoft.com/support/) nitelikli bir [destek planıyla](https://azure.microsoft.com/support/plans/)kullanılabilir.

## <a name="customize-jvm"></a>JVM'yi özelleştir

Fonksiyonlar, Java işlevlerinizi çalıştırmak için kullanılan Java sanal makinesini (JVM) özelleştirmenize olanak tanır. [Aşağıdaki JVM seçenekleri](https://github.com/Azure/azure-functions-java-worker/blob/master/worker.config.json#L7) varsayılan olarak kullanılır:

* `-XX:+TieredCompilation`
* `-XX:TieredStopAtLevel=1`
* `-noverify` 
* `-Djava.net.preferIPv4Stack=true`
* `-jar`

Adlandırılmış `JAVA_OPTS`bir uygulama ayarında ek bağımsız değişkenler sağlayabilirsiniz. Azure portalında veya Azure CLI'de Azure'a dağıtılan işlev uygulamanıza uygulama ayarları ekleyebilirsiniz.

### <a name="azure-portal"></a>Azure portal

Azure [portalında,](https://portal.azure.com)ayarı eklemek için `JAVA_OPTS` Uygulama Ayarları [sekmesini](functions-how-to-use-azure-function-app-settings.md#settings) kullanın.

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki örnekte olduğu gibi, ayarlamak `JAVA_OPTS`için az [functionapp config appsettings set](/cli/azure/functionapp/config/appsettings) komutunu kullanabilirsiniz:

```azurecli-interactive
az functionapp config appsettings set --name <APP_NAME> \
--resource-group <RESOURCE_GROUP> \
--settings "JAVA_OPTS=-Djava.awt.headless=true"
```
Bu örnek, başsız modu sağlar. İşlev uygulamanızın adı ve `<APP_NAME>` `<RESOURCE_GROUP>` kaynak grubuyla değiştirin.

> [!WARNING]  
> Tüketim [planında,](functions-scale.md#consumption-plan) `0`'' `WEBSITE_USE_PLACEHOLDER` değeri olan ayarı eklemeniz gerekir.  
Bu ayar, Java işlevleri için soğuk başlangıç sürelerini artırır.

## <a name="third-party-libraries"></a>Üçüncü taraf kitaplıkları 

Azure İşlevler üçüncü taraf kitaplıklarının kullanımını destekler. Varsayılan olarak, proje `pom.xml` dosyanızda belirtilen tüm bağımlılıklar [`mvn package`](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage) hedef sırasında otomatik olarak paketlenir. `pom.xml` Dosyada bağımlılık olarak belirtilmeyen kitaplıklar için, bunları işlevin kök dizininde bir `lib` dizine yerleştirin. `lib` Dizin yerleştirilen bağımlılıklar çalışma zamanında sistem sınıfı yükleyicisine eklenir.

Bağımlılık `com.microsoft.azure.functions:azure-functions-java-library` varsayılan olarak sınıf yolunda sağlanır ve `lib` dizine dahil edilmesi gerekmez. Ayrıca, [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) [burada](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) sınıf yoluna listelenen bağımlılıkları ekler.

## <a name="data-type-support"></a>Veri türü desteği

Giriş veya çıktı bağlamalarına bağlamak için Düz eski `azure-functions-java-library`Java nesnelerini (POJ'ler), string ve tamsayı gibi ilkel veri türlerinde tanımlanan türleri kullanabilirsiniz.

### <a name="pojos"></a>POJO'LAR

Giriş verilerini POJO'ya dönüştürmek için [azure-functions-java-worker](https://github.com/Azure/azure-functions-java-worker) [gson](https://github.com/google/gson) kitaplığını kullanır. İşlevlere giriş olarak kullanılan POJO tipleri olmalıdır. `public`

### <a name="binary-data"></a>İkili veriler

İkili girdileri veya çıktıları `byte[]`, function.json alanı `dataType` ayarlayarak `binary`bind:

```java
   @FunctionName("BlobTrigger")
    @StorageAccount("AzureWebJobsStorage")
     public void blobTrigger(
        @BlobTrigger(name = "content", path = "myblob/{fileName}", dataType = "binary") byte[] content,
        @BindingName("fileName") String fileName,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Blob trigger function processed a blob.\n Name: " + fileName + "\n Size: " + content.length + " Bytes");
    }
```

Null değerleri bekliyorsanız, `Optional<T>`kullanın.

## <a name="bindings"></a>Bağlamalar

Giriş ve çıktı bağlamaları, kodunuzdaki verilere bağlanmak için bildirimsel bir yol sağlar. Bir işlevin birden çok giriş ve çıkış bağlaması olabilir.

### <a name="input-binding-example"></a>Giriş bağlama örneği

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("echo")
    public static String echo(
        @HttpTrigger(name = "req", methods = { "put" }, authLevel = AuthorizationLevel.ANONYMOUS, route = "items/{id}") String inputReq,
        @TableInput(name = "item", tableName = "items", partitionKey = "Example", rowKey = "{id}", connection = "AzureWebJobsStorage") TestInputData inputData
        @TableOutput(name = "myOutputTable", tableName = "Person", connection = "AzureWebJobsStorage") OutputBinding<Person> testOutputData,
    ) {
        testOutputData.setValue(new Person(httpbody + "Partition", httpbody + "Row", httpbody + "Name"));
        return "Hello, " + inputReq + " and " + inputData.getKey() + ".";
    }

    public static class TestInputData {
        public String getKey() { return this.RowKey; }
        private String RowKey;
    }
    public static class Person {
        public String PartitionKey;
        public String RowKey;
        public String Name;

        public Person(String p, String r, String n) {
            this.PartitionKey = p;
            this.RowKey = r;
            this.Name = n;
        }
    }
}
```

Bu işlevi bir HTTP isteğiyle çağırırsınız. 
- HTTP istek yükü bağımsız `String` değişken `inputReq`için bir olarak geçirilir.
- Bir giriş Tablo depolamadan alınır ve `TestInputData` bağımsız değişkene `inputData`geçirilir.

Bir dizi girdi almak `String[]`için , , `POJO[]` `List<String>`, veya `List<POJO>`.

```java
@FunctionName("ProcessIotMessages")
    public void processIotMessages(
        @EventHubTrigger(name = "message", eventHubName = "%AzureWebJobsEventHubPath%", connection = "AzureWebJobsEventHubSender", cardinality = Cardinality.MANY) List<TestEventData> messages,
        final ExecutionContext context)
    {
        context.getLogger().info("Java Event Hub trigger received messages. Batch size: " + messages.size());
    }
    
    public class TestEventData {
    public String id;
}

```

Yapılandırılan olay hub'ında yeni veri olduğunda bu işlev tetiklenir. `cardinality` Çünkü , `MANY`işlev olay hub'ından bir toplu ileti toplu alır. `EventData`olay hub'ından işlev `TestEventData` yürütmeiçin dönüştürülür.

### <a name="output-binding-example"></a>Çıktı bağlama örneği

Bir çıktı bağlamayı return value'a `$return`kullanarak bağlayabilirsiniz. 

```java
package com.example;

import com.microsoft.azure.functions.annotation.*;

public class Function {
    @FunctionName("copy")
    @StorageAccount("AzureWebJobsStorage")
    @BlobOutput(name = "$return", path = "samples-output-java/{name}")
    public static String copy(@BlobTrigger(name = "blob", path = "samples-input-java/{name}") String content) {
        return content;
    }
}
```

Birden çok çıktı bağlaması varsa, bunlardan yalnızca biri için iade değerini kullanın.

Birden çok çıktı değeri `OutputBinding<T>` göndermek `azure-functions-java-library` için pakette tanımlanan kullanın. 

```java
@FunctionName("QueueOutputPOJOList")
    public HttpResponseMessage QueueOutputPOJOList(@HttpTrigger(name = "req", methods = { HttpMethod.GET,
            HttpMethod.POST }, authLevel = AuthorizationLevel.ANONYMOUS) HttpRequestMessage<Optional<String>> request,
            @QueueOutput(name = "itemsOut", queueName = "test-output-java-pojo", connection = "AzureWebJobsStorage") OutputBinding<List<TestData>> itemsOut, 
            final ExecutionContext context) {
        context.getLogger().info("Java HTTP trigger processed a request.");
       
        String query = request.getQueryParameters().get("queueMessageId");
        String queueMessageId = request.getBody().orElse(query);
        itemsOut.setValue(new ArrayList<TestData>());
        if (queueMessageId != null) {
            TestData testData1 = new TestData();
            testData1.id = "msg1"+queueMessageId;
            TestData testData2 = new TestData();
            testData2.id = "msg2"+queueMessageId;

            itemsOut.getValue().add(testData1);
            itemsOut.getValue().add(testData2);

            return request.createResponseBuilder(HttpStatus.OK).body("Hello, " + queueMessageId).build();
        } else {
            return request.createResponseBuilder(HttpStatus.INTERNAL_SERVER_ERROR)
                    .body("Did not find expected items in CosmosDB input list").build();
        }
    }

     public static class TestData {
        public String id;
    }
```

Bu işlevi bir HttpRequest'te çağırırsınız. Sıra depolamasına birden çok değer yazar.

## <a name="httprequestmessage-and-httpresponsemessage"></a>HttpRequestMessage ve HttpResponseMessage

 Bunlar ' `azure-functions-java-library`da tanımlanır. Bunlar, HttpTrigger işlevleriyle çalışmak için yardımcı türleridir.

| Özel tip      |       Hedef        | Tipik kullanım                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP Tetikleyicisi     | Yöntem, üstbilgi veya sorgu alır |
| `HttpResponseMessage` | HTTP Çıkış Bağlama | 200 dışındaki durum döndürür   |

## <a name="metadata"></a>Meta Veriler

Çok az tetikleyici, giriş verileriyle birlikte [tetikleyici meta veri](/azure/azure-functions/functions-triggers-bindings) gönderir. Meta verileri `@BindingName` tetiklemek için bağlamak için ek açıklama kullanabilirsiniz.


```Java
package com.example;

import java.util.Optional;
import com.microsoft.azure.functions.annotation.*;


public class Function {
    @FunctionName("metadata")
    public static String metadata(
        @HttpTrigger(name = "req", methods = { "get", "post" }, authLevel = AuthorizationLevel.ANONYMOUS) Optional<String> body,
        @BindingName("name") String queryValue
    ) {
        return body.orElse(queryValue);
    }
}
```
Önceki örnekte, `queryValue` HTTP istek URL'sinde sorgu `name` dize parametresine bağlıdır. `http://{example.host}/api/metadata?name=test` İşte başka bir örnek, `Id` kuyruktan meta veri tetikleme nasıl bağlanılmayı gösterir.

```java
 @FunctionName("QueueTriggerMetadata")
    public void QueueTriggerMetadata(
        @QueueTrigger(name = "message", queueName = "test-input-java-metadata", connection = "AzureWebJobsStorage") String message,@BindingName("Id") String metadataId,
        @QueueOutput(name = "output", queueName = "test-output-java-metadata", connection = "AzureWebJobsStorage") OutputBinding<TestData> output,
        final ExecutionContext context
    ) {
        context.getLogger().info("Java Queue trigger function processed a message: " + message + " with metadaId:" + metadataId );
        TestData testData = new TestData();
        testData.id = metadataId;
        output.setValue(testData);
    }
```

> [!NOTE]
> Ek açıklamasağlanan adı meta veri özelliği eşleşmesi gerekir.

## <a name="execution-context"></a>Yürütme bağlamı

`ExecutionContext`, tanımlanan `azure-functions-java-library`, çalışma zamanı işlevleri ile iletişim kurmak için yardımcı yöntemler içerir.

### <a name="logger"></a>Günlükçü

İşlev kodundan günlükleri yazmak için `getLogger`tanımlanan `ExecutionContext`, kullanılan kullanın.

Örnek:

```java

import com.microsoft.azure.functions.*;
import com.microsoft.azure.functions.annotation.*;

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        if (req.isEmpty()) {
            context.getLogger().warning("Empty request body received by function " + context.getFunctionName() + " with invocation " + context.getInvocationId());
        }
        return String.format(req);
    }
}
```

## <a name="view-logs-and-trace"></a>Günlükleri ve izlemeleri görüntüleme

Azure CLI'yi, Java stdout ve stderr günlüğe kaydetmenin yanı sıra diğer uygulama günlüğe kaydetmeyi de aktarmak için kullanabilirsiniz. 

Azure CLI'yi kullanarak uygulama günlüğü yazmak için işlev uygulamanızı şu şekilde yapılandırabilirsiniz:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Azure CLI'yi kullanarak işlev uygulamanız için günlük çıktısını aktarmak için yeni bir komut istemi, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
[az webapp günlük kuyruk](/cli/azure/webapp/log) komutu `--provider` seçeneği kullanarak çıktı filtre seçenekleri vardır. 

Azure CLI'yi kullanarak günlük dosyalarını tek bir ZIP dosyası olarak indirmek için yeni bir komut istemi, Bash veya Terminal oturumu açın ve aşağıdaki komutu girin:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Bu komutu çalıştırmadan önce Azure portalında veya Azure CLI'de dosya sistemi günlüğe kaydetmeyi etkinleştirmiş olmalısınız.

## <a name="environment-variables"></a>Ortam değişkenleri

İşlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md)yürütme sırasında ortam değişkenleri olarak ortaya çıkarır. Bu ayarlara kullanarak `System.getenv("AzureWebJobsStorage")`erişebilirsiniz.

Aşağıdaki örnek, adlı `myAppSetting`anahtar ile uygulama [ayarı](functions-how-to-use-azure-function-app-settings.md#settings)alır:

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("My app setting value: "+ System.getenv("myAppSetting"));
        return String.format(req);
    }
}

```

## <a name="next-steps"></a>Sonraki adımlar

Azure İşlevler Java geliştirme hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure Fonksiyonları tetikler ve bağlamalar](functions-triggers-bindings.md)
* [Görsel Stüdyo Kodu](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md)ve [Eclipse](functions-create-maven-eclipse.md) ile yerel geliştirme ve hata ayıklama
* [Visual Studio Code ile Uzaktan Hata Ayıklama Java Azure Fonksiyonları](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Azure Fonksiyonları için Maven eklentisi](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) 
* Amaç aracılığıyla işlev `azure-functions:add` oluşturmayı kolaylaştırın ve [ZIP dosya dağıtımı](deployment-zip-push.md)için bir hazırlama dizini hazırlayın.
