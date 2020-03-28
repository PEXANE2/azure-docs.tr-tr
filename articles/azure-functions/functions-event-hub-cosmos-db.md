---
title: "Öğretici: Azure Cosmos DB ve Etkinlik Hub'larıyla Java işlevlerini kullanın"
description: Bu öğretici, Java'da yazılmış bir işlevi kullanarak Azure Cosmos DB'de güncelleştirmeler yapmak için Etkinlik Hub'larından etkinlikleri nasıl tükettiğinizi gösterir.
author: KarlErickson
ms.topic: tutorial
ms.date: 11/04/2019
ms.author: karler
ms.openlocfilehash: b6d7b2c60e777266b1cab578b8970c1fa1c6bc50
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "77425332"
---
# <a name="tutorial-create-a-function-in-java-with-an-event-hub-trigger-and-an-azure-cosmos-db-output-binding"></a>Öğretici: Etkinlik Hub tetikleyicisi ve Azure Cosmos DB çıktı bağlama ile Java'da bir işlev oluşturun

Bu öğretici, sürekli sıcaklık ve basınç veri akışını analiz eden bir Java işlevi oluşturmak için Azure İşlevlerini nasıl kullanacağınızı gösterir. Sensör okumalarını temsil eden olay hub olayları işlevi tetikler. İşlev olay verilerini işler, ardından bir Azure Cosmos DB'ye durum girişleri ekler.

Bu öğreticide şunları yapacaksınız:

> [!div class="checklist"]
> * Azure CLI'yi kullanarak Azure kaynaklarını oluşturun ve yapılandırın.
> * Bu kaynaklarla etkileşimde olan Java işlevlerioluşturun ve test edin.
> * İşlevlerinizi Azure'a dağıtın ve Uygulama Öngörüleri ile izleyin.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakileri yüklemiş olmalısınız:

* [Java Developer Kit](https://aka.ms/azure-jdks), sürüm 8
* [Apache Maven](https://maven.apache.org), sürüm 3.0 veya üzeri
* Cloud Shell'i kullanmamayı tercih ederseniz [Azure CLI](/cli/azure/install-azure-cli)
* [Azure İşlevler Çekirdek Araçları](https://www.npmjs.com/package/azure-functions-core-tools) sürüm 2.6.666 veya üzeri

> [!IMPORTANT]
> Bu `JAVA_HOME` öğreticiyi tamamlamak için ortam değişkeni JDK'nın yükleme konumuna ayarlanmalıdır.

Bu öğretici için kodu doğrudan kullanmayı tercih ederseniz, [java-functions-eventhub-cosmosdb](https://github.com/Azure-Samples/java-functions-eventhub-cosmosdb) örnek repo'ya bakın.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

Bu eğitimde, bu kaynaklara ihtiyacınız olacak:

* Diğer kaynakları içerecek bir kaynak grubu
* Olay Hub'ları ad alanı, olay hub'ı ve yetkilendirme kuralı
* Cosmos DB hesabı, veritabanı ve toplama
* Barındırmak için bir işlev uygulaması ve depolama hesabı

Aşağıdaki bölümlerde Azure CLI'yi kullanarak bu kaynakların nasıl oluşturulabileceğinizgösteriz.

### <a name="log-in-to-azure"></a>Azure'da oturum açma

Cloud Shell kullanmıyorsanız, hesabınıza erişmek için Azure CLI'yi yerel olarak kullanmanız gerekir. `az login` Tarayıcı tabanlı oturum açma deneyimini başlatmak için Bash komutunu kullanın. Birden fazla Azure aboneliğine erişiminiz varsa, `az account set --subscription` varsayılanı abonelik kimliğiyle birlikte ayarlayın.

### <a name="set-environment-variables"></a>Ortam değişkenlerini belirleme

Ardından, oluşturacağınız kaynakların adları ve konumu için bazı ortam değişkenleri oluşturun. `<value>` Yer tutucuları seçtiğiniz değerlerle değiştirerek aşağıdaki komutları kullanın. Değerler, Azure [kaynakları için adlandırma kurallarına ve kısıtlamalarına](/azure/architecture/best-practices/resource-naming)uygun olmalıdır. `LOCATION` Değişken için `az functionapp list-consumption-locations` komut tarafından üretilen değerlerden birini kullanın.

```azurecli-interactive
RESOURCE_GROUP=<value>
EVENT_HUB_NAMESPACE=<value>
EVENT_HUB_NAME=<value>
EVENT_HUB_AUTHORIZATION_RULE=<value>
COSMOS_DB_ACCOUNT=<value>
STORAGE_ACCOUNT=<value>
FUNCTION_APP=<value>
LOCATION=<value>
```

Bu öğreticinin geri kalanı bu değişkenleri kullanır. Bu değişkenlerin yalnızca geçerli Azure CLI veya Cloud Shell oturumunuz süresince devam ettiğini unutmayın. Farklı bir yerel terminal penceresi veya Cloud Shell oturum süreniz dışında bu komutları yeniden çalıştırmanız gerekir.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure, hesabınızdaki ilgili tüm kaynakları toplamak için kaynak gruplarını kullanır. Bu şekilde, bunları bir birim olarak görüntüleyebilir ve onlarla yaptığınız işi bitirdiğinizde tek bir komutla silebilirsiniz.

Kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az group create \
    --name $RESOURCE_GROUP \
    --location $LOCATION
```

### <a name="create-an-event-hub"></a>Olay hub’ı oluşturma

Ardından, aşağıdaki komutları kullanarak bir Azure Etkinlik Hub'ları ad alanı, olay merkezi ve yetkilendirme kuralı oluşturun:

```azurecli-interactive
az eventhubs namespace create \
    --resource-group $RESOURCE_GROUP \
    --name $EVENT_HUB_NAMESPACE
az eventhubs eventhub create \
    --resource-group $RESOURCE_GROUP \
    --name $EVENT_HUB_NAME \
    --namespace-name $EVENT_HUB_NAMESPACE \
    --message-retention 1
az eventhubs eventhub authorization-rule create \
    --resource-group $RESOURCE_GROUP \
    --name $EVENT_HUB_AUTHORIZATION_RULE \
    --eventhub-name $EVENT_HUB_NAME \
    --namespace-name $EVENT_HUB_NAMESPACE \
    --rights Listen Send
```

Olay Hub'ları ad alanı, gerçek olay hub'ını ve yetkilendirme kuralını içerir. Yetkilendirme kuralı, işlevlerinizin hub'a ileti göndermesine ve ilgili olayları dinlemesine olanak tanır. Bir işlev, telemetri verilerini temsil eden iletiler gönderir. Başka bir işlev olayları dinler, olay verilerini analiz eder ve sonuçları Azure Cosmos DB'de depolar.

### <a name="create-an-azure-cosmos-db"></a>Azure Cosmos DB oluşturma

Ardından, aşağıdaki komutları kullanarak bir Azure Cosmos DB hesabı, veritabanı ve koleksiyon oluşturun:

```azurecli-interactive
az cosmosdb create \
    --resource-group $RESOURCE_GROUP \
    --name $COSMOS_DB_ACCOUNT
az cosmosdb database create \
    --resource-group-name $RESOURCE_GROUP \
    --name $COSMOS_DB_ACCOUNT \
    --db-name TelemetryDb
az cosmosdb collection create \
    --resource-group-name $RESOURCE_GROUP \
    --name $COSMOS_DB_ACCOUNT \
    --collection-name TelemetryInfo \
    --db-name TelemetryDb \
    --partition-key-path '/temperatureStatus'
```

Değer, `partition-key-path` verilerinizi her öğenin değerine `temperatureStatus` göre bölümlere ayırır. Bölüm anahtarı, Verilerinizi bağımsız olarak erişebileceği farklı alt kümelere bölerek Cosmos DB'nin performansı artırmasına olanak tanır.

### <a name="create-a-storage-account-and-function-app"></a>Depolama hesabı ve işlev uygulaması oluşturma

Ardından, Azure İşlevleri tarafından gerekli olan bir Azure Depolama hesabı oluşturun ve ardından işlev uygulamasını oluşturun. Aşağıdaki komutları kullanın:

```azurecli-interactive
az storage account create \
    --resource-group $RESOURCE_GROUP \
    --name $STORAGE_ACCOUNT \
    --sku Standard_LRS
az functionapp create \
    --resource-group $RESOURCE_GROUP \
    --name $FUNCTION_APP \
    --storage-account $STORAGE_ACCOUNT \
    --consumption-plan-location $LOCATION \
    --runtime java
```

`az functionapp create` Komut işlev uygulamanızı oluşturduğunda, aynı ada sahip bir Application Insights kaynağı da oluşturur. İşlev uygulaması, uygulama öngörülerine bağlanan adlandırılmış `APPINSIGHTS_INSTRUMENTATIONKEY` bir ayarla otomatik olarak yapılandırılır. Bu öğreticide daha sonra açıklandığı gibi, işlevlerinizi Azure'a dağıttıktan sonra uygulama telemetrisini görüntüleyebilirsiniz.

## <a name="configure-your-function-app"></a>İşlev uygulamanızı yapılandırın

İşlev uygulamanızın düzgün çalışması için diğer kaynaklara erişmeniz gerekir. Aşağıdaki bölümler, işlev uygulamanızı yerel makinenizde çalışabilmesi için nasıl yapılandırabileceğinizi gösterir.

### <a name="retrieve-resource-connection-strings"></a>Kaynak bağlantı dizelerini alma

Depolama, olay hub'ı ve Cosmos DB bağlantı dizelerini almak ve bunları ortam değişkenlerine kaydetmek için aşağıdaki komutları kullanın:

```azurecli-interactive
AZURE_WEB_JOBS_STORAGE=$( \
    az storage account show-connection-string \
        --name $STORAGE_ACCOUNT \
        --query connectionString \
        --output tsv)
echo $AZURE_WEB_JOBS_STORAGE
EVENT_HUB_CONNECTION_STRING=$( \
    az eventhubs eventhub authorization-rule keys list \
        --resource-group $RESOURCE_GROUP \
        --name $EVENT_HUB_AUTHORIZATION_RULE \
        --eventhub-name $EVENT_HUB_NAME \
        --namespace-name $EVENT_HUB_NAMESPACE \
        --query primaryConnectionString \
        --output tsv)
echo $EVENT_HUB_CONNECTION_STRING
COSMOS_DB_CONNECTION_STRING=$( \
    az cosmosdb keys list \
        --resource-group $RESOURCE_GROUP \
        --name $COSMOS_DB_ACCOUNT \
        --type connection-strings \
        --query connectionStrings[0].connectionString \
        --output tsv)
echo $COSMOS_DB_CONNECTION_STRING
```

Bu değişkenler Azure CLI komutlarından alınan değerlere göre ayarlanır. Her komut, döndürülen JSON yükünden bağlantı dizesini ayıklamak için bir JMESPath sorgusu kullanır. Bağlantı dizeleri de, `echo` başarılı bir şekilde alındığını onaylamak için kullanılarak görüntülenir.

### <a name="update-your-function-app-settings"></a>İşlev uygulama ayarlarınızı güncelleyin

Ardından, bağlantı dize değerlerini Azure İşlevler hesabınızdaki uygulama ayarlarına aktarmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az functionapp config appsettings set \
    --resource-group $RESOURCE_GROUP \
    --name $FUNCTION_APP \
    --settings \
        AzureWebJobsStorage=$AZURE_WEB_JOBS_STORAGE \
        EventHubConnectionString=$EVENT_HUB_CONNECTION_STRING \
        CosmosDBConnectionString=$COSMOS_DB_CONNECTION_STRING
```

Azure kaynaklarınız artık oluşturuldu ve birlikte düzgün çalışacak şekilde yapılandırıldı.

## <a name="create-and-test-your-functions"></a>İşlevlerinizi oluşturun ve test edin

Ardından, yerel makinenizde bir proje oluşturur, Java kodu ekler ve test elersiniz. Maven için Azure İşlevler Eklentisi ve Azure İşlevler Temel Araçları ile çalışan komutları kullanırsınız. Işlevleriniz yerel olarak çalışır, ancak oluşturduğunuz bulut tabanlı kaynakları kullanır. Işlevleri yerel olarak çalıştırdıktan sonra, Bunları buluta dağıtmak ve verilerinizin ve analizlerinizin birikmesini izlemek için Maven'i kullanabilirsiniz.

Kaynaklarınızı oluşturmak için Cloud Shell'i kullandıysanız, Azure'a yerel olarak bağlanmazsınız. Bu durumda, tarayıcı `az login` tabanlı oturum açma işlemini başlatmak için komutu kullanın. Daha sonra gerekirse, varsayılan `az account set --subscription` aboneliği abonelik kimliğiyle birlikte ayarlayın. Son olarak, yerel makinenizde bazı ortam değişkenlerini yeniden oluşturmak için aşağıdaki komutları çalıştırın. Yer `<value>` tutucuları daha önce kullandığınız değerlerle değiştirin.

```bash
RESOURCE_GROUP=<value>
FUNCTION_APP=<value>
```

### <a name="create-a-local-functions-project"></a>Yerel işlevler projesi oluşturma

Bir işlev ler projesi oluşturmak ve gerekli bağımlılıkları eklemek için aşağıdaki Maven komutunu kullanın.

```bash
mvn archetype:generate --batch-mode \
    -DarchetypeGroupId=com.microsoft.azure \
    -DarchetypeArtifactId=azure-functions-archetype \
    -DappName=$FUNCTION_APP \
    -DresourceGroup=$RESOURCE_GROUP \
    -DgroupId=com.example \
    -DartifactId=telemetry-functions
```

Bu komut, bir `telemetry-functions` klasör içinde birkaç dosya oluşturur:

* Maven ile kullanım için bir `pom.xml` dosya
* Yerel `local.settings.json` sınama için uygulama ayarlarını tutmak için bir dosya
* Veri `host.json` analizi işlevinizde Cosmos DB çıktısı bağlaması için gerekli olan Azure İşlerinden Uzatma Paketi'ni sağlayan bir dosya
* Varsayılan `Function.java` işlev uygulaması içeren bir dosya
* Bu öğreticinin gerekmediği birkaç test dosyası

Derleme hatalarını önlemek için test dosyalarını silmeniz gerekir. Yeni proje klasörüne gitmek ve test klasörünü silmek için aşağıdaki komutları çalıştırın:

```bash
cd telemetry-functions
rm -r src/test
```

### <a name="retrieve-your-function-app-settings-for-local-use"></a>Yerel kullanım için işlev uygulama ayarlarınızı alın

Yerel sınama için, işlev projenizin bu eğitimde Azure'daki işlev uygulamanıza eklediğiniz bağlantı dizelerine gereksinim ilerler. Bulutta depolanan tüm işlev uygulama ayarlarını alan ve bunları dosyanıza `local.settings.json` ekleyen aşağıdaki Azure İşlevler Temel Araçları komutunu kullanın:

```bash
func azure functionapp fetch-app-settings $FUNCTION_APP
```

### <a name="add-java-code"></a>Java kodu ekle

Ardından, dosyayı `Function.java` açın ve içindekileri aşağıdaki kodla değiştirin.

```java
package com.example;

import com.example.TelemetryItem.status;
import com.microsoft.azure.functions.annotation.Cardinality;
import com.microsoft.azure.functions.annotation.CosmosDBOutput;
import com.microsoft.azure.functions.annotation.EventHubOutput;
import com.microsoft.azure.functions.annotation.EventHubTrigger;
import com.microsoft.azure.functions.annotation.FunctionName;
import com.microsoft.azure.functions.annotation.TimerTrigger;
import com.microsoft.azure.functions.ExecutionContext;
import com.microsoft.azure.functions.OutputBinding;

public class Function {

    @FunctionName("generateSensorData")
    @EventHubOutput(
        name = "event",
        eventHubName = "", // blank because the value is included in the connection string
        connection = "EventHubConnectionString")
    public TelemetryItem generateSensorData(
        @TimerTrigger(
            name = "timerInfo",
            schedule = "*/10 * * * * *") // every 10 seconds
            String timerInfo,
        final ExecutionContext context) {

        context.getLogger().info("Java Timer trigger function executed at: "
            + java.time.LocalDateTime.now());
        double temperature = Math.random() * 100;
        double pressure = Math.random() * 50;
        return new TelemetryItem(temperature, pressure);
    }

    @FunctionName("processSensorData")
    public void processSensorData(
        @EventHubTrigger(
            name = "msg",
            eventHubName = "", // blank because the value is included in the connection string
            cardinality = Cardinality.ONE,
            connection = "EventHubConnectionString")
            TelemetryItem item,
        @CosmosDBOutput(
            name = "databaseOutput",
            databaseName = "TelemetryDb",
            collectionName = "TelemetryInfo",
            connectionStringSetting = "CosmosDBConnectionString")
            OutputBinding<TelemetryItem> document,
        final ExecutionContext context) {

        context.getLogger().info("Event hub message received: " + item.toString());

        if (item.getPressure() > 30) {
            item.setNormalPressure(false);
        } else {
            item.setNormalPressure(true);
        }

        if (item.getTemperature() < 40) {
            item.setTemperatureStatus(status.COOL);
        } else if (item.getTemperature() > 90) {
            item.setTemperatureStatus(status.HOT);
        } else {
            item.setTemperatureStatus(status.WARM);
        }

        document.setValue(item);
    }
}
```

Gördüğünüz gibi, bu dosya iki `generateSensorData` işlev `processSensorData`içerir ve . İşlev, `generateSensorData` olay merkezine sıcaklık ve basınç okumaları gönderen bir sensörü simüle eder. Zamanlayıcı tetikleyicisi işlevi her 10 saniyede bir çalıştırR ve olay merkezi çıktısı bağlama, iade değerini olay merkezine gönderir.

Olay hub'ı iletiyi aldığında, bir olay oluşturur. Olay `processSensorData` aldığında işlev çalışır. Daha sonra olay verilerini işler ve sonuçları Azure Cosmos DB'ye göndermek için bir Azure Cosmos DB çıktısı bağlayıcısı kullanır.

Bu işlevler tarafından kullanılan veriler, uygulamanız gereken bir sınıf kullanılarak `TelemetryItem`depolanır. Aynı konumda adı `TelemetryItem.java` verilen yeni `Function.java` bir dosya oluşturun ve aşağıdaki kodu ekleyin:

```java
package com.example;

public class TelemetryItem {

    private String id;
    private double temperature;
    private double pressure;
    private boolean isNormalPressure;
    private status temperatureStatus;
    static enum status {
        COOL,
        WARM,
        HOT
    }

    public TelemetryItem(double temperature, double pressure) {
        this.temperature = temperature;
        this.pressure = pressure;
    }

    public String getId() {
        return id;
    }

    public double getTemperature() {
        return temperature;
    }

    public double getPressure() {
        return pressure;
    }

    @Override
    public String toString() {
        return "TelemetryItem={id=" + id + ",temperature="
            + temperature + ",pressure=" + pressure + "}";
    }

    public boolean isNormalPressure() {
        return isNormalPressure;
    }

    public void setNormalPressure(boolean isNormal) {
        this.isNormalPressure = isNormal;
    }

    public status getTemperatureStatus() {
        return temperatureStatus;
    }

    public void setTemperatureStatus(status temperatureStatus) {
        this.temperatureStatus = temperatureStatus;
    }
}
```

### <a name="run-locally"></a>Yerel olarak çalıştırma

Artık işlevleri yerel olarak oluşturabilir ve çalıştırabilir ve Azure Cosmos DB'nizde verilerin göründüğünü görebilirsiniz.

Işlevleri oluşturmak ve çalıştırmak için aşağıdaki Maven komutlarını kullanın:

```bash
mvn clean package
mvn azure-functions:run
```

Bazı yapı ve başlangıç iletilerinden sonra, işlevler her çalıştırışta aşağıdaki örneğe benzer çıktı görürsünüz:

```output
[10/22/19 4:01:30 AM] Executing 'Functions.generateSensorData' (Reason='Timer fired at 2019-10-21T21:01:30.0016769-07:00', Id=c1927c7f-4f70-4a78-83eb-bc077d838410)
[10/22/19 4:01:30 AM] Java Timer trigger function executed at: 2019-10-21T21:01:30.015
[10/22/19 4:01:30 AM] Function "generateSensorData" (Id: c1927c7f-4f70-4a78-83eb-bc077d838410) invoked by Java Worker
[10/22/19 4:01:30 AM] Executed 'Functions.generateSensorData' (Succeeded, Id=c1927c7f-4f70-4a78-83eb-bc077d838410)
[10/22/19 4:01:30 AM] Executing 'Functions.processSensorData' (Reason='', Id=f4c3b4d7-9576-45d0-9c6e-85646bb52122)
[10/22/19 4:01:30 AM] Event hub message received: TelemetryItem={id=null,temperature=32.728691307527015,pressure=10.122563042388165}
[10/22/19 4:01:30 AM] Function "processSensorData" (Id: f4c3b4d7-9576-45d0-9c6e-85646bb52122) invoked by Java Worker
[10/22/19 4:01:38 AM] Executed 'Functions.processSensorData' (Succeeded, Id=1cf0382b-0c98-4cc8-9240-ee2a2f71800d)
```

Daha sonra Azure [portalına](https://portal.azure.com) gidip Azure Cosmos DB hesabınıza gidebilirsiniz. **Veri Gezgini'ni**seçin, **TelemetryInfo'u**genişletin, ardından verilerinizi geldiğinde görüntülemek için **Öğeler'i** seçin.

![Cosmos DB Veri Gezgini](media/functions-event-hub-cosmos-db/data-explorer.png)

## <a name="deploy-to-azure-and-view-app-telemetry"></a>Azure'a dağıtın ve uygulama telemetrisini görüntüleyin

Son olarak, uygulamanızı Azure'a dağıtabilir ve uygulamanın yerel olarak çalıştığı şekilde çalışmaya devam ettiğini doğrulayabilirsiniz.

Aşağıdaki komutu kullanarak projenizi Azure'a dağıtın:

```bash
mvn azure-functions:deploy
```

İşlevleriniz artık Azure'da çalışır ve Azure Cosmos DB'nizde veri biriktirmeye devam eder. Dağıtılmış işlev uygulamanızı Azure portalında görüntüleyebilir ve aşağıdaki ekran görüntülerinde gösterildiği gibi bağlı Application Insights kaynağı ndan uygulama telemetrisini görüntüleyebilirsiniz:

**Canlı Ölçümler Akışı:**

![Uygulama Öngörüleri Canlı Ölçümler Akışı](media/functions-event-hub-cosmos-db/application-insights-live-metrics-stream.png)

**Performans:**

![Uygulama Öngörüleri Performans bıçağı](media/functions-event-hub-cosmos-db/application-insights-performance.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz Azure kaynaklarıyla işiniz bittiğinde aşağıdaki komutu kullanarak silebilirsiniz:

```azurecli-interactive
az group delete --name $RESOURCE_GROUP
```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Olay Hub olaylarını işleyen ve Bir Cosmos DB'yi güncelleyen bir Azure İşlevi oluşturmayı öğrendiniz. Daha fazla bilgi için [Azure İşlevler Java geliştirici kılavuzuna](/azure/azure-functions/functions-reference-java)bakın. Kullanılan ek açıklamalar hakkında bilgi için [com.microsoft.azure.functions.ek açıklama](/java/api/com.microsoft.azure.functions.annotation) başvurusuna bakın.

Bu öğretici, bağlantı dizeleri gibi sırları depolamak için ortam değişkenlerini ve uygulama ayarlarını kullanmıştır. Bu sırları Azure Key Vault'ta depolama hakkında daha fazla bilgi için, [Uygulama Hizmeti ve Azure İşlevleri için Anahtar Kasası başvurularını kullanın'a](/azure/app-service/app-service-key-vault-references)bakın.

Ardından, otomatik dağıtım için Azure Pipelines CI/CD'yi nasıl kullanacağınızı öğrenin:

> [!div class="nextstepaction"]
> [Azure İşlevleri için Java oluşturma ve dağıtma](/azure/devops/pipelines/ecosystems/java-function)
