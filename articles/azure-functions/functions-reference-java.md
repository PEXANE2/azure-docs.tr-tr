---
title: Azure Işlevleri için Java geliştirici başvurusu
description: Java ile işlevleri geliştirmeyi anlayın.
ms.topic: conceptual
ms.date: 09/14/2018
ms.openlocfilehash: 19a290fe7717d7838e8fcd1d1f5cddb3f54eb812
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82145321"
---
# <a name="azure-functions-java-developer-guide"></a>Azure Işlevleri Java geliştirici kılavuzu

Azure Işlevleri çalışma zamanı, [Java SE 8 LTS 'yi (Zulu 8.31.0.2-JRE 8.0.181-win_x64)](https://repos.azul.com/azure-only/zulu/packages/zulu-8/8u181/)destekler. Bu kılavuzda, Java ile Azure Işlevleri yazma hakkında bilgiler yer alır.

Diğer dillerde olduğu gibi, bir İşlev Uygulaması bir veya daha fazla işleve sahip olabilir. Java işlevi, ek açıklamayla `public` `@FunctionName`donatılmış bir yöntemdir. Bu yöntem, bir Java işlevinin girişini tanımlar ve belirli bir pakette benzersiz olmalıdır. Java 'da yazılan bir İşlev Uygulaması birden fazla ortak yöntemi olan birden çok sınıfa sahip olabilir `@FunctionName`.

Bu makalede, [Azure işlevleri geliştirici başvurusunu](functions-reference.md)zaten okuduğunuzu varsaymış olursunuz. Ayrıca, [Visual Studio Code](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-java) veya [Maven](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java)kullanarak ilk Işlevinizi oluşturmak için işlevler hızlı başlangıcını de tamamlamalısınız.

## <a name="programming-model"></a>Programlama modeli 

[Tetikleyiciler ve bağlamaların](functions-triggers-bindings.md) kavramları Azure işlevleri için temel bir araçlardır. Tetikleyiciler, kodunuzun yürütülmesini başlatır. Bağlamalar, özel veri erişim kodu yazmak zorunda kalmadan bir işleve veri geçirmek ve veri döndürmek için bir yol sağlar.

## <a name="create-java-functions"></a>Java işlevleri oluşturma

Java işlevlerinin oluşturulmasını kolaylaştırmak için, belirli bir işlev tetikleyicisine sahip projeler oluşturmanıza yardımcı olması için önceden tanımlanmış Java şablonlarını kullanan Maven tabanlı araç ve arşiv türleri vardır.    

### <a name="maven-based-tooling"></a>Maven tabanlı araç

Aşağıdaki geliştirici ortamlarında, Java işlev projeleri oluşturmanıza olanak sağlayan Azure Işlevleri araçları vardır: 

+ [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions)
+ [Küreler](functions-create-maven-eclipse.md)
+ [IntelliJ](functions-create-maven-intellij.md)

Yukarıdaki makale bağlantılarında, tercih ettiğiniz IDE 'yi kullanarak ilk işlevlerinizi nasıl oluşturacağınız gösterilmektedir. 

### <a name="project-scaffolding"></a>Proje yapı Iskelesi

Terminalden komut satırı geliştirmeyi tercih ediyorsanız, Java tabanlı işlev projelerini dolandırmanın en kolay yolu, arşiv türleri kullanmaktır `Apache Maven` . Azure Işlevleri için Java Maven arşiv ETYPE şu _GroupID_altında yayımlanır:_ArtifactId_: [com. Microsoft. Azure: Azure-Functions-arşiv ETYPE](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-archetype/). 

Aşağıdaki komut, bu arşiv ETYPE kullanarak yeni bir Java işlev projesi oluşturur:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype 
```

Bu arşiv ETYPE kullanmaya başlamak için bkz. [Java hızlı başlangıç](/azure/azure-functions/functions-create-first-azure-function-azure-cli?pivots=programming-language-java). 

## <a name="create-kotlin-functions-preview"></a>Kotlin işlevleri oluşturma (Önizleme)

Ayrıca, Kotlin işlevleri oluşturmak için bir Maven arşiv ETYPE vardır. Şu anda önizleme aşamasında olan bu arşiv ETYPE şu _GroupID_altında yayımlanır:_ArtifactId_: [com. Microsoft. Azure: Azure-Functions-Kotlin-](https://search.maven.org/artifact/com.microsoft.azure/azure-functions-kotlin-archetype/),. 

Aşağıdaki komut, bu arşiv ETYPE kullanarak yeni bir Java işlev projesi oluşturur:

```
mvn archetype:generate \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-kotlin-archetype
```

Bu arşiv ETYPE kullanmaya başlamak için, bkz. [Kotlin hızlı başlangıç](functions-create-first-kotlin-maven.md).

## <a name="folder-structure"></a>Klasör yapısı

Bir Azure Işlevleri Java projesinin klasör yapısı aşağıda verilmiştir:

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

_* Kotlin projesi hala Maven olduğundan çok benzer görünüyor_

İşlev uygulamasını yapılandırmak için paylaşılan bir [Host. JSON](functions-host-json.md) dosyası kullanabilirsiniz. Her işlevin kendi kod dosyası (. Java) ve bağlama yapılandırma dosyası (Function. JSON) vardır.

Bir projeye birden fazla işlev yerleştirebilirsiniz. İşlevlerinizi ayrı jliler 'e yerleştirmekten kaçının. Hedef `FunctionApp` dizindeki, Azure 'da işlev uygulamanıza dağıtılan şeydir.

## <a name="triggers-and-annotations"></a>Tetikleyiciler ve ek açıklamalar

 İşlevler, bir HTTP isteği, süreölçer veya veri güncelleştirmesi gibi bir tetikleyici tarafından çağrılır. İşlevinizin bir veya daha fazla çıkış üretmek için bu tetikleyiciyi ve diğer girişleri işlemesi gerekir.

Yöntemlerinizi giriş ve çıkışları bağlamak için [com. Microsoft. Azure. Functions. Annotation. *](/java/api/com.microsoft.azure.functions.annotation) paketinde bulunan Java ek açıklamalarını kullanın. Daha fazla bilgi için bkz. [Java başvuru belgeleri](/java/api/com.microsoft.azure.functions.annotation).

> [!IMPORTANT] 
> [Yerel. Settings. JSON](/azure/azure-functions/functions-run-local#local-settings-file) ' da Azure Blob depolama, Azure kuyruk depolaması veya Azure Tablo depolama alanı tetiklerinizi yerel olarak çalıştırmak Için bir Azure depolama hesabı yapılandırmanız gerekir.

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

`function.json` [Azure-Functions-Maven-Plugin](https://mvnrepository.com/artifact/com.microsoft.azure/azure-functions-maven-plugin)tarafından buna karşılık gelen oluşturulan:

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

Java işlev uygulamalarının yerel geliştirmesi için Azul [sistemlerinden](https://www.azul.com/downloads/azure-only/zulu/)Azure Java 8 JDKs [Için Azul Zulu kurumsal](https://assets.azul.com/files/Zulu-for-Azure-FAQ.pdf) 'ı indirin ve kullanın. Azure Işlevleri, işlev uygulamalarınızı buluta dağıtırken Azul Java 8 JDK çalışma zamanını kullanır.

JDKs ve Function uygulamalarıyla ilgili sorunlar için [Azure desteği](https://azure.microsoft.com/support/) , [nitelikli bir destek planıyla](https://azure.microsoft.com/support/plans/)sunulmaktadır.

## <a name="customize-jvm"></a>JVM 'YI özelleştirme

İşlevler, Java işlevlerinizi çalıştırmak için kullanılan Java sanal makinesini (JVM) özelleştirmenizi sağlar. [Aşağıdaki JVM seçenekleri](https://github.com/Azure/azure-functions-java-worker/blob/master/worker.config.json#L7) varsayılan olarak kullanılır:

* `-XX:+TieredCompilation`
* `-XX:TieredStopAtLevel=1`
* `-noverify` 
* `-Djava.net.preferIPv4Stack=true`
* `-jar`

Adlı `JAVA_OPTS`bir uygulama ayarında ek bağımsız değişkenler sağlayabilirsiniz. Azure portal veya Azure CLı 'de Azure 'a dağıtılan işlev uygulamanıza uygulama ayarları ekleyebilirsiniz.

> [!IMPORTANT]  
> Tüketim planında, özelleştirmenin çalışması için değeri 0 olan WEBSITE_USE_PLACEHOLDER ayarını da eklemeniz gerekir. Bu ayar, Java işlevleri için soğuk başlangıç sürelerini artırır.

### <a name="azure-portal"></a>Azure portal

[Azure Portal](https://portal.azure.com), `JAVA_OPTS` ayarı eklemek için [uygulama ayarları sekmesini](functions-how-to-use-azure-function-app-settings.md#settings) kullanın.

### <a name="azure-cli"></a>Azure CLI

Aşağıdaki örnekte olduğu gibi, ayarlamak `JAVA_OPTS`için [az functionapp config appSettings set](/cli/azure/functionapp/config/appsettings) komutunu kullanabilirsiniz:

#### <a name="consumption-plan"></a>[Tüketim planı](#tab/consumption)
```azurecli-interactive
az functionapp config appsettings set \
--settings "JAVA_OPTS=-Djava.awt.headless=true" \
"WEBSITE_USE_PLACEHOLDER=0" \
--name <APP_NAME> --resource-group <RESOURCE_GROUP>
```
#### <a name="dedicated-plan--premium-plan"></a>[Adanmış plan/Premium plan](#tab/dedicated+premium)
```azurecli-interactive
az functionapp config appsettings set \
--settings "JAVA_OPTS=-Djava.awt.headless=true" \
--name <APP_NAME> --resource-group <RESOURCE_GROUP>
```
---

Bu örnek, gözetimsiz moda izin vermez. İşlev `<APP_NAME>` uygulamanızın adıyla ve `<RESOURCE_GROUP>` kaynak grubuyla değiştirin. 

## <a name="third-party-libraries"></a>Üçüncü taraf kitaplıkları 

Azure Işlevleri, üçüncü taraf kitaplıkların kullanımını destekler. Varsayılan olarak, proje `pom.xml` dosyanızda belirtilen tüm bağımlılıklar, [`mvn package`](https://github.com/Microsoft/azure-maven-plugins/blob/master/azure-functions-maven-plugin/README.md#azure-functionspackage) hedef sırasında otomatik olarak paketlenmiştir. `pom.xml` Dosyada bağımlılıklar olarak belirtilmeyen kitaplıklar için, bunları işlevin kök dizinindeki bir `lib` dizine yerleştirin. `lib` Dizine yerleştirilmiş bağımlılıklar, çalışma zamanında sistem sınıfı yükleyicilerine eklenir.

`com.microsoft.azure.functions:azure-functions-java-library` Bağımlılık, varsayılan olarak Sınıfyoluna sağlanır ve `lib` dizine eklenmesi gerekmez. Ayrıca, [Azure-Functions-Java-Worker](https://github.com/Azure/azure-functions-java-worker) , [burada](https://github.com/Azure/azure-functions-java-worker/wiki/Azure-Java-Functions-Worker-Dependencies) listelenen bağımlılıkları Sınıfyoluna ekler.

## <a name="data-type-support"></a>Veri türü desteği

Giriş veya çıkış bağlamalarına bağlamak için düz eski Java nesnelerini (POJOs) `azure-functions-java-library`, içinde tanımlanan türleri veya dize ve tamsayı gibi temel veri türlerini kullanabilirsiniz.

### <a name="pojos"></a>POJOs

Giriş verilerini POJO 'ya dönüştürmek için [Azure-Functions-Java-Worker](https://github.com/Azure/azure-functions-java-worker) [gson](https://github.com/google/gson) kitaplığını kullanır. İşlevlere giriş olarak kullanılan POJO türleri olmalıdır `public`.

### <a name="binary-data"></a>İkili veriler

Function. json ' daki `byte[]` `dataType` alanı ' e `binary`ayarlayarak, ikili girdileri veya çıkışları öğesine bağlayın:

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

Null değerler bekleliyorsanız kullanın `Optional<T>`.

## <a name="bindings"></a>Bağlamalar

Giriş ve çıkış bağlamaları, kodunuzun içinden verilere bağlanmak için bildirim temelli bir yol sağlar. Bir işlevde birden fazla giriş ve çıkış bağlaması olabilir.

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

Bu işlevi bir HTTP isteğiyle çağırır. 
- HTTP istek yükü, bağımsız değişken `String` `inputReq`için bir olarak geçirilir.
- Tablo depolamadan bir giriş alınır ve bağımsız değişkenine `TestInputData` `inputData`olarak geçirilir.

Bir toplu işlem almak için `String[]`,, veya `POJO[]` `List<String>` `List<POJO>`' a bağlayabilirsiniz.

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

Bu işlev, yapılandırılan olay hub 'ında her yeni veri olduğunda tetiklenir. `cardinality` , Olarak ayarlandığı için `MANY`, işlevi olay hub 'ından bir ileti toplu işi alır. `EventData`Olay Hub 'ı, işlev yürütmesi `TestEventData` için öğesine dönüştürülür.

### <a name="output-binding-example"></a>Çıkış bağlama örneği

' I kullanarak `$return`, dönüş değerine bir çıkış bağlaması bağlayabilirsiniz. 

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

Birden çok çıkış bağlaması varsa, bunlardan yalnızca biri için dönüş değerini kullanın.

Birden çok çıkış değeri göndermek için, `OutputBinding<T>` `azure-functions-java-library` pakette tanımlanan öğesini kullanın. 

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

Bu işlevi bir HttpRequest üzerinde çağırılır. Kuyruk depolamaya birden çok değer yazar.

## <a name="httprequestmessage-and-httpresponsemessage"></a>HttpRequestMessage ve HttpResponseMessage

 Bunlar ' de `azure-functions-java-library`tanımlanmıştır. HttpTrigger işlevleriyle çalışmak için yardımcı türlerdir.

| Özelleştirilmiş tür      |       Hedef        | Tipik kullanım                  |
| --------------------- | :-----------------: | ------------------------------ |
| `HttpRequestMessage<T>`  |    HTTP Tetikleyicisi     | Yöntemi, üstbilgileri veya sorguları alır |
| `HttpResponseMessage` | HTTP çıkış bağlama | 200 dışında bir durum döndürür   |

## <a name="metadata"></a>Meta Veriler

Birkaç tetikleyici, giriş verileriyle birlikte [tetikleyici meta verilerini](/azure/azure-functions/functions-triggers-bindings) gönderir. Daha fazla açıklama `@BindingName` kullanarak tetikleyici meta verilerine bağlayabilirsiniz.


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
Yukarıdaki örnekte,, `queryValue` http istek URL 'sindeki sorgu dizesi parametresine `name` bağlanır. `http://{example.host}/api/metadata?name=test` Sıra tetikleyicisi meta `Id` verilerinden nasıl bağlanılacağını gösteren başka bir örnek aşağıda verilmiştir.

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
> Ek açıklamada belirtilen adın meta veri özelliğiyle eşleşmesi gerekir.

## <a name="execution-context"></a>Yürütme bağlamı

`ExecutionContext`, içinde `azure-functions-java-library`tanımlanan, işlevler çalışma zamanı ile iletişim kurmak için yardımcı yöntemleri içerir.

### <a name="logger"></a>Medi

İşlev `getLogger`kodundan Günlükler yazmak `ExecutionContext`için ' de tanımlanan ' ı kullanın.

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

## <a name="view-logs-and-trace"></a>Günlükleri görüntüleme ve izleme

Azure CLı 'yi kullanarak Java stdout ve stderr günlük kaydını ve diğer uygulama günlüğünü de yazdırabilirsiniz. 

Azure CLı kullanarak uygulama günlüğü yazmak için işlev uygulamanızı yapılandırma:

```azurecli-interactive
az webapp log config --name functionname --resource-group myResourceGroup --application-logging true
```

Azure CLı kullanarak işlev uygulamanıza ait günlük çıktısını akışa almak için yeni bir komut istemi, bash veya terminal oturumu açın ve aşağıdaki komutu girin:

```azurecli-interactive
az webapp log tail --name webappname --resource-group myResourceGroup
```
[Az WebApp log tail](/cli/azure/webapp/log) komutu, `--provider` seçeneğini kullanarak çıktıyı filtrelemeye yönelik seçeneklere sahiptir. 

Azure CLı kullanarak günlük dosyalarını tek bir ZIP dosyası olarak indirmek için yeni bir komut istemi, bash veya terminal oturumu açın ve aşağıdaki komutu girin:

```azurecli-interactive
az webapp log download --resource-group resourcegroupname --name functionappname
```

Bu komutu çalıştırmadan önce Azure portal veya Azure CLı 'de dosya sistemi günlüğünü etkinleştirmiş olmanız gerekir.

## <a name="environment-variables"></a>Ortam değişkenleri

Işlevlerde, hizmet bağlantı dizeleri gibi [uygulama ayarları](functions-app-settings.md), yürütme sırasında ortam değişkenleri olarak sunulur. Bu ayarlara,, `System.getenv("AzureWebJobsStorage")`kullanarak erişebilirsiniz.

Aşağıdaki örnek, adlı `myAppSetting`anahtar ile [uygulama ayarını](functions-how-to-use-azure-function-app-settings.md#settings)alır:

```java

public class Function {
    public String echo(@HttpTrigger(name = "req", methods = {"post"}, authLevel = AuthorizationLevel.ANONYMOUS) String req, ExecutionContext context) {
        context.getLogger().info("My app setting value: "+ System.getenv("myAppSetting"));
        return String.format(req);
    }
}

```

> [!NOTE]
> En iyileştirilmiş soğuk başlangıç deneyimi için FUNCTIONS_EXTENSION_VERSION AppSetting değeri ~ 2 veya ~ 3 olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

Azure Işlevleri Java geliştirmesi hakkında daha fazla bilgi için aşağıdaki kaynaklara bakın:

* [Azure İşlevleri için en iyi uygulamalar](functions-best-practices.md)
* [Azure İşlevleri geliştirici başvurusu](functions-reference.md)
* [Azure Işlevleri Tetikleyicileri ve bağlamaları](functions-triggers-bindings.md)
* [Visual Studio Code](https://code.visualstudio.com/docs/java/java-azurefunctions), [IntelliJ](functions-create-maven-intellij.md)ve [tutulma](functions-create-maven-eclipse.md) ile yerel geliştirme ve hata ayıklama
* [Visual Studio Code ile Java Azure Işlevleri ile uzaktan hata ayıklama](https://code.visualstudio.com/docs/java/java-serverless#_remote-debug-functions-running-in-the-cloud)
* [Azure Işlevleri için Maven eklentisi](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-functions-maven-plugin/README.md) 
* İşlev oluşturmayı `azure-functions:add` amaç aracılığıyla kolaylaştırın ve [ZIP dosya dağıtımı](deployment-zip-push.md)için bir hazırlama dizini hazırlayın.
