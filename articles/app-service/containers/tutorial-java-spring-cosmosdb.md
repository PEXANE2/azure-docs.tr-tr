---
title: 'Öğretici: MongoDB ile Linux Java uygulaması'
description: Azure'da çalışan bir MongoDB'ye (Cosmos DB) bağlanarak Azure Uygulama Hizmeti'nde çalışan veri odaklı bir Linux Java uygulamasını nasıl elde edebilirsiniz öğrenin.
author: rloutlaw
ms.author: routlaw
ms.devlang: java
ms.topic: tutorial
ms.date: 12/10/2018
ms.custom: mvc, seodec18, seo-java-july2019, seo-java-august2019, seo-java-september2019
ms.openlocfilehash: e5dcb39430158db1ee9a18524d0214335a2bbbba
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80045369"
---
# <a name="tutorial-build-a-java-spring-boot-web-app-with-azure-app-service-on-linux-and-azure-cosmos-db"></a>Öğretici: Linux ve Azure Cosmos DB'de Azure Uygulama Hizmeti ile Java Bahar Önyükleme web uygulaması oluşturun

Bu öğretici, Azure'da Java web uygulamalarını oluşturma, yapılandırma, dağıtma ve ölçekleme sürecinde size yol sunar. İşi bittiğinde, [Linux'ta Azure Uygulama Hizmeti'nde](/azure/app-service/containers)çalışan [Azure Cosmos DB'de](/azure/cosmos-db) veri depolayan bir [Bahar Önyükleme](https://projects.spring.io/spring-boot/) uygulamasına sahip olursunuz.

![Azure Cosmos DB'de veri depolama bahar önyükleme uygulaması](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Bir Cosmos DB veritabanı oluşturun.
> * Örnek bir uygulamayı veritabanına bağlayın ve yerel olarak test edin
> * Örnek uygulamayı Azure'a dağıtma
> * Uygulama Hizmeti'nden tanılama günlüklerini akışla
> * Örnek uygulamayı ölçeklendirmek için ek örnekler ekleme

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

* [Azure CLI](https://docs.microsoft.com/cli/azure/overview), kendi bilgisayarınızda yüklü. 
* [Git](https://git-scm.com/)
* [Java JDK](https://aka.ms/azure-jdks)
* [Maven](https://maven.apache.org)

## <a name="clone-the-sample-todo-app-and-prepare-the-repo"></a>Örnek TODO uygulamasını klonlayın ve repo'yu hazırlayın

Bu öğretici, [Bahar Veri Azure Cosmos DB](https://github.com/Microsoft/spring-data-cosmosdb)tarafından desteklenen bir Bahar REST API çağıran bir web UI ile örnek bir TODO liste uygulaması kullanır. Uygulamanın kodu [GitHub'da](https://github.com/Microsoft/spring-todo-app)kullanılabilir. Bahar ve Cosmos DB kullanarak Java uygulamaları yazma hakkında daha fazla bilgi edinmek için [Azure Cosmos DB SQL API öğreticisini](https://docs.microsoft.com/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db ) ve [Bahar Verisi Azure Cosmos DB hızlı başlangıç](https://github.com/Microsoft/spring-data-cosmosdb#quick-start)ile Bahar Önyükleme Başlatıcısı'na bakın.


Örnek repo'yu klonlamak ve örnek uygulama ortamını ayarlamak için terminalinizdeki aşağıdaki komutları çalıştırın.

```bash
git clone --recurse-submodules https://github.com/Azure-Samples/e2e-java-experience-in-app-service-linux-part-2.git
cd e2e-java-experience-in-app-service-linux-part-2
yes | cp -rf .prep/* .
```

## <a name="create-an-azure-cosmos-db"></a>Azure Cosmos DB oluşturma

Aboneliğinizde bir Azure Cosmos DB veritabanı oluşturmak için aşağıdaki adımları izleyin. TODO listesi uygulaması bu veritabanına bağlanır ve çalışırken verilerini depolar ve uygulamayı nerede çalıştırdığınızdan önemli olmayan uygulama durumunu devam ettir.

1. Azure CLI'nize giriş yapın ve oturum açma kimlik bilgilerinize birden fazla bağlıysanız aboneliğinizi isteğe bağlı olarak ayarlayın.

    ```bash
    az login
    az account set -s <your-subscription-id>
    ```   

2. Kaynak grubu adını belirterek bir Azure Kaynak Grubu oluşturun.

    ```bash
    az group create -n <your-azure-group-name> \
        -l <your-resource-group-region>
    ```

3. Azure Cosmos DB'yi türle `GlobalDocumentDB` oluşturun. Cosmos DB adı yalnızca küçük harfler kullanmalıdır. Komutun `documentEndpoint` yanıtında alanı not edin.

    ```bash
    az cosmosdb create --kind GlobalDocumentDB \
        -g <your-azure-group-name> \
        -n <your-azure-COSMOS-DB-name-in-lower-case-letters>
    ```

4. Uygulamaya bağlanmak için Azure Cosmos DB anahtarınızı alın. `primaryMasterKey`Bir sonraki `documentEndpoint` adımda ihtiyacınız olacak gibi yakın tutun.

    ```bash
    az cosmosdb list-keys -g <your-azure-group-name> -n <your-azure-COSMOSDB-name>
    ```

## <a name="configure-the-todo-app-properties"></a>TODO uygulama özelliklerini yapılandırma

Bilgisayarınızda bir terminal açın. Yeni oluşturduğunuz Cosmos DB veritabanınız için özelleştirebilmeniz için örnek komut dosyası dosyasını klonlanmış repo'da kopyalayın.

```bash
cd initial/spring-todo-app
cp set-env-variables-template.sh .scripts/set-env-variables.sh
```
 
En `.scripts/set-env-variables.sh` sevdiğiniz düzenleyiciyi edin ve Azure Cosmos DB bağlantı bilgilerini tedarik edin. App Service Linux yapılandırması için Cosmos`your-resource-group-region`DB veritabanını`your-azure-group-name`oluştururken kullanılan önceki bölge ( ) ve kaynak grubu () kullanın. Azure dağıtımında hiçbir web uygulaması adını yineleyemediğinden benzersiz bir WEBAPP_NAME seçin.

```bash
export COSMOSDB_URI=<put-your-COSMOS-DB-documentEndpoint-URI-here>
export COSMOSDB_KEY=<put-your-COSMOS-DB-primaryMasterKey-here>
export COSMOSDB_DBNAME=<put-your-COSMOS-DB-name-here>

# App Service Linux Configuration
export RESOURCEGROUP_NAME=<put-your-resource-group-name-here>
export WEBAPP_NAME=<put-your-Webapp-name-here>
export REGION=<put-your-REGION-here>
```

Sonra komut dosyası çalıştırın:

```bash
source .scripts/set-env-variables.sh
```
   
Bu ortam değişkenleri `application.properties` TODO liste uygulamasında kullanılır. Özellikler dosyasındaki alanlar, Yay Verileri için varsayılan bir depo yapılandırması ayarlar:

```properties
azure.cosmosdb.uri=${COSMOSDB_URI}
azure.cosmosdb.key=${COSMOSDB_KEY}
azure.cosmosdb.database=${COSMOSDB_DBNAME}
```

```java
@Repository
public interface TodoItemRepository extends DocumentDbRepository<TodoItem, String> {
}
```

Daha sonra örnek `@Document` uygulama, Cosmos `com.microsoft.azure.spring.data.cosmosdb.core.mapping.Document` DB tarafından depolanacak ve yönetilecek bir varlık türünü ayarlamak için alınan ek açıklamayı kullanır:

```java
@Document
public class TodoItem {
    private String id;
    private String description;
    private String owner;
    private boolean finished;
```

## <a name="run-the-sample-app"></a>Örnek uygulamayı çalıştırma

Örneği çalıştırmak için Maven'i kullan.

```bash
mvn package spring-boot:run
```

Çıktı aşağıdaki gibi görünmelidir.

```bash
bash-3.2$ mvn package spring-boot:run
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 


[INFO] SimpleUrlHandlerMapping - Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] SimpleUrlHandlerMapping - Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
[INFO] WelcomePageHandlerMapping - Adding welcome page: class path resource [static/index.html]
2018-10-28 15:04:32.101  INFO 7673 --- [           main] c.m.azure.documentdb.DocumentClient      : Initializing DocumentClient with serviceEndpoint [https://sample-cosmos-db-westus.documents.azure.com:443/], ConnectionPolicy [ConnectionPolicy [requestTimeout=60, mediaRequestTimeout=300, connectionMode=Gateway, mediaReadMode=Buffered, maxPoolSize=800, idleConnectionTimeout=60, userAgentSuffix=;spring-data/2.0.6;098063be661ab767976bd5a2ec350e978faba99348207e8627375e8033277cb2, retryOptions=com.microsoft.azure.documentdb.RetryOptions@6b9fb84d, enableEndpointDiscovery=true, preferredLocations=null]], ConsistencyLevel [null]
[INFO] AnnotationMBeanExporter - Registering beans for JMX exposure on startup
[INFO] TomcatWebServer - Tomcat started on port(s): 8080 (http) with context path ''
[INFO] TodoApplication - Started TodoApplication in 45.573 seconds (JVM running for 76.534)
```

Uygulama başlatıldıktan sonra Bu Bağlantıyı kullanarak Bahar TODO [http://localhost:8080/](http://localhost:8080/)Uygulamasına yerel olarak erişebilirsiniz: .

 ![Bahar TODO uygulamasına yerel olarak erişin](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-locally.jpg)

"Started TodoApplication" iletisi yerine özel durumlar görürseniz, önceki adımdaki komut dosyasının `bash` ortam değişkenlerini düzgün bir şekilde dışa aktarıp aktardığını ve oluşturduğunuz Azure Cosmos DB veritabanı için değerlerin doğru olup olmadığını denetleyin.

## <a name="configure-azure-deployment"></a>Azure dağıtımını yapılandırma

Dosyayı `pom.xml` dizinde `initial/spring-boot-todo` açın ve [Maven](https://github.com/Microsoft/azure-maven-plugins/blob/develop/azure-webapp-maven-plugin/README.md) yapılandırması için aşağıdaki Azure Web Uygulama Eklentisini ekleyin.

```xml    
<plugins> 

    <!--*************************************************-->
    <!-- Deploy to Java SE in App Service Linux           -->
    <!--*************************************************-->
       
    <plugin>
        <groupId>com.microsoft.azure</groupId>
        <artifactId>azure-webapp-maven-plugin</artifactId>
        <version>1.9.0</version>
        <configuration>
            <schemaVersion>v2</schemaVersion>

            <!-- Web App information -->
            <resourceGroup>${RESOURCEGROUP_NAME}</resourceGroup>
            <appName>${WEBAPP_NAME}</appName>
            <region>${REGION}</region>

            <!-- Java Runtime Stack for Web App on Linux-->
            <runtime>
                 <os>linux</os>
                 <javaVersion>jre8</javaVersion>
                 <webContainer>jre8</webContainer>
             </runtime>
             <deployment>
                 <resources>
                 <resource>
                     <directory>${project.basedir}/target</directory>
                     <includes>
                     <include>*.jar</include>
                     </includes>
                 </resource>
                 </resources>
             </deployment>

            <appSettings>
                <property>
                    <name>COSMOSDB_URI</name>
                    <value>${COSMOSDB_URI}</value>
                </property> 
                <property>
                    <name>COSMOSDB_KEY</name>
                    <value>${COSMOSDB_KEY}</value>
                </property>
                <property>
                    <name>COSMOSDB_DBNAME</name>
                    <value>${COSMOSDB_DBNAME}</value>
                </property>
                <property>
                    <name>JAVA_OPTS</name>
                    <value>-Dserver.port=80</value>
                </property>
            </appSettings>

        </configuration>
    </plugin>           
    ...
</plugins>
```

## <a name="deploy-to-app-service-on-linux"></a>Linux'ta Uygulama Hizmetine Dağılın

TODO `azure-webapp:deploy` uygulamasını Linux'taki Azure Uygulama Hizmeti'ne dağıtmak için Maven hedefini kullanın.

```bash

# Deploy
bash-3.2$ mvn azure-webapp:deploy
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building spring-todo-app 2.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- azure-webapp-maven-plugin:1.9.0:deploy (default-cli) @ spring-todo-app ---
[INFO] Target Web App doesn't exist. Creating a new one...
[INFO] Creating App Service Plan 'ServicePlanb6ba8178-5bbb-49e7'...
[INFO] Successfully created App Service Plan.
[INFO] Successfully created Web App.
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] Copying 1 resource to /home/test/e2e-java-experience-in-app-service-linux-part-2/initial/spring-todo-app/target/azure-webapp/spring-todo-app-61bb5207-6fb8-44c4-8230-c1c9e4c099f7
[INFO] Trying to deploy artifact to spring-todo-app...
[INFO] Renaming /home/test/e2e-java-experience-in-app-service-linux-part-2/initial/spring-todo-app/target/azure-webapp/spring-todo-app-61bb5207-6fb8-44c4-8230-c1c9e4c099f7/spring-todo-app-2.0-SNAPSHOT.jar to app.jar
[INFO] Deploying the zip package spring-todo-app-61bb5207-6fb8-44c4-8230-c1c9e4c099f7718326714198381983.zip...
[INFO] Successfully deployed the artifact to https://spring-todo-app.azurewebsites.net
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 02:19 min
[INFO] Finished at: 2019-11-06T15:32:03-07:00
[INFO] Final Memory: 50M/574M
[INFO] ------------------------------------------------------------------------
```

Çıktı, dağıtılmış uygulamanızın URL'sini içerir `https://spring-todo-app.azurewebsites.net` (bu örnekte). Bu URL'yi web tarayıcınıza kopyalayabilir veya uygulamanızı yüklemek için Terminal pencerenizde aşağıdaki komutu çalıştırabilirsiniz.

```bash
open https://spring-todo-app.azurewebsites.net
```

Adres çubuğunda uzak URL ile çalışan uygulamayı görmeniz gerekir:

 ![Uzak bir URL ile çalışan Bahar Önyükleme uygulaması](./media/tutorial-java-spring-cosmosdb/spring-todo-app-running-in-app-service.jpg)

## <a name="stream-diagnostic-logs"></a>Tanılama günlüklerini akışla aktarma

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]


## <a name="scale-out-the-todo-app"></a>TODO Uygulamasını Ölçeklendirin

Başka bir alt ekleyerek uygulamayı ölçeklendirin:

```bash
az appservice plan update --number-of-workers 2 \
   --name ${WEBAPP_PLAN_NAME} \
   --resource-group <your-azure-group-name>
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu kaynaklara başka bir öğretici (bkz. [Sonraki adımlar](#next)) için gereksinim duymuyorsanız, Cloud Shell'de aşağıdaki komutu çalıştırarak bu kaynakları silebilirsiniz: 
  
```bash
az group delete --name <your-azure-group-name>
```

<a name="next"></a>

## <a name="next-steps"></a>Sonraki adımlar

[Azure Java Geliştiricilerbahar](/java/azure/)
[Önyükleme](https://spring.io/projects/spring-boot), [Cosmos DB için Bahar Verileri](/java/azure/spring-framework/configure-spring-boot-starter-java-app-with-cosmos-db?view=azure-java-stable), Azure [Cosmos DB](/azure/cosmos-db/sql-api-introduction) ve App Service [Linux](app-service-linux-intro.md).

Geliştirici kılavuzunda Linux'ta Uygulama Hizmeti'nde Java uygulamalarını çalıştırma hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"] 
> [App Service Linux’ta Java geliştirme kılavuzu](configure-language-java.md)
