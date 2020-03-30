---
title: Quickstart - Azure PowerShell'i kullanarak Akış Analizi işi oluşturun
description: Bu hızlı başlangıç, bir Azure Akış Analizi işini dağıtmak ve çalıştırmak için Azure PowerShell modülünün nasıl kullanılacağını gösterir.
author: mamccrea
ms.author: mamccrea
ms.date: 12/20/2018
ms.topic: quickstart
ms.service: stream-analytics
ms.custom: mvc
ms.openlocfilehash: 84c132c333e4d6ba052029350f275ebf499a906f
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79536811"
---
# <a name="quickstart-create-a-stream-analytics-job-using-azure-powershell"></a>Hızlı başlangıç: Azure PowerShell'i kullanarak Bir Akış Analizi işi oluşturun

Azure PowerShell modülü, PowerShell cmdlets veya komut dosyalarını kullanarak Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıçta Azure PowerShell modülü kullanarak bir Azure Stream Analytics işi dağıtma ve çalıştırma işleminin ayrıntıları verilmektedir.

Örnek iş, bir IoT Hub aygıtından veri akışı okur. Giriş verileri Raspberry Pi çevrimiçi simülatörü tarafından oluşturulur. Ardından, Stream Analytics işi, iletileri 27°'den büyük bir sıcaklıkla filtrelemek için Akış Analizi sorgu dilini kullanarak verileri dönüştürür. Son olarak, ortaya çıkan çıktı olaylarını blob depolama sındaki bir dosyaya yazar.

## <a name="before-you-begin"></a>Başlamadan önce

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* Azure aboneliğiniz yoksa, ücretsiz bir [hesap oluşturun.](https://azure.microsoft.com/free/)

* Bu hızlı başlatma için Azure PowerShell modülü gerekir. Yerel makinenizde yüklü sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell Modülü yükleme](https://docs.microsoft.com/powershell/azure/install-Az-ps).

* Bazı IoT Hub eylemleri Azure PowerShell tarafından desteklenmez ve Azure CLI sürüm 2.0.70 veya sonraki sürüm ve Azure CLI için IoT uzantısı kullanılarak tamamlanmalıdır. [Azure CLI'yi yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) ve IoT uzantısını yüklemek için kullanın. `az extension add --name azure-iot`


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

`Connect-AzAccount` Komutla Azure aboneliğinizde oturum açın ve açılan tarayıcıya Azure kimlik bilgilerinizi girin:

```powershell
# Connect to your Azure account
Connect-AzAccount
```

Birden fazla aboneliğiniz varsa, aşağıdaki cmdlets'i çalıştırarak bu hızlı başlangıç için kullanmak istediğiniz aboneliği seçin. `<your subscription name>` değerini aboneliğinizin adıyla değiştirdiğinizden emin olun:

```powershell
# List all available subscriptions.
Get-AzSubscription

# Select the Azure subscription you want to use to create the resource group and resources.
Get-AzSubscription -SubscriptionName "<your subscription name>" | Select-AzSubscription
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[Yeni-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup)ile bir Azure kaynak grubu oluşturun. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

```powershell
$resourceGroup = "StreamAnalyticsRG"
$location = "WestUS2"
New-AzResourceGroup `
    -Name $resourceGroup `
    -Location $location
```

## <a name="prepare-the-input-data"></a>Girdi verilerini hazırlama

Stream Analytics işini tanımlamadan önce, işe girdi olarak yapılandırılan verileri hazırlayın.

Aşağıdaki Azure CLI kod bloğu, işin gerektirdiği giriş verilerini hazırlamak için birçok komut yapar. Kodu anlamak için bölümleri gözden geçirin.

1. PowerShell pencerenizde, Azure hesabınızda oturum açmak için [az giriş](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest) komutunu çalıştırın.

    Başarılı bir şekilde oturum açabildiğinizde, Azure CLI aboneliklerinizin bir listesini döndürür. Bu hızlı başlangıç için kullandığınız aboneliği kopyalayın ve bu aboneliği seçmek için [az hesap kümesi komutunu](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#change-the-active-subscription) çalıştırın. PowerShell ile önceki bölümde seçtiğiniz aboneliği seçin. Aboneliğinizin `<your subscription name>` adıyla değiştirdiğinizden emin olun.

    ```azurecli
    az login

    az account set --subscription "<your subscription>"
    ```

2. [Az iot hub oluşturma](../iot-hub/iot-hub-create-using-cli.md#create-an-iot-hub) komutunu kullanarak bir IoT Hub oluşturun. Bu örnek, **MyASAIoTHub**adında bir IoT Hub'ı oluşturur. IoT Hub adları benzersiz olduğundan, kendi IoT Hub adınızı bulabilmeniz gerekir. Aboneliğinizde kullanılabilen ücretsiz katmanı kullanmak için SKU'yu F1 olarak ayarlayın. Değilse, bir sonraki en alt katmanı seçin.

    ```azurecli
    az iot hub create --name "<your IoT Hub name>" --resource-group $resourceGroup --sku S1
    ```

    IoT hub'ı oluşturulduktan sonra, [az iot hub show-connection-string](https://docs.microsoft.com/cli/azure/iot/hub?view=azure-cli-latest) komutunu kullanarak IoT Hub bağlantı dizesini alın. Bağlantı dizesinin tamamını kopyalayın ve IoT Hub'ını Stream Analytics işinize girdi olarak eklediğinizde kaydedin.

    ```azurecli
    az iot hub show-connection-string --hub-name "MyASAIoTHub"
    ```

3. [Az iothub aygıt kimliği oluşturma](../iot-hub/quickstart-send-telemetry-c.md#register-a-device) komutunu kullanarak IoT Hub'a bir aygıt ekleyin. Bu örnek, **MyASAIoTDevice**adlı bir aygıt oluşturur.

    ```azurecli
    az iot hub device-identity create --hub-name "MyASAIoTHub" --device-id "MyASAIoTDevice"
    ```

4. [Az iot hub aygıt-kimlik show-connection-string](/cli/azure/ext/azure-cli-iot-ext/iot/hub/device-identity#ext-azure-cli-iot-ext-az-iot-hub-device-identity-show-connection-string) komutunu kullanarak aygıt bağlantı dizesini alın. Tüm bağlantı dizesini kopyalayın ve Raspberry Pi simülatörü oluştururken kaydedin.

    ```azurecli
    az iot hub device-identity show-connection-string --hub-name "MyASAIoTHub" --device-id "MyASAIoTDevice" --output table
    ```

    **Çıktı örneği:**

    ```output
    HostName=MyASAIoTHub.azure-devices.net;DeviceId=MyASAIoTDevice;SharedAccessKey=a2mnUsg52+NIgYudxYYUNXI67r0JmNubmfVafojG8=
    ```

## <a name="create-blob-storage"></a>Blob depolama oluşturma

Aşağıdaki Azure PowerShell kod bloğu, iş çıktısı için kullanılan blob depolama sını oluşturmak için komutları kullanır. Kodu anlamak için bölümleri gözden geçirin.

1. [New-AzStorageAccount](https://docs.microsoft.com/powershell/module/az.storage/New-azStorageAccount) cmdlet kullanarak standart bir genel amaçlı depolama hesabı oluşturun.  Bu örnek, yerel olarak yedekli depolama (LRS) ve blob şifreleme (varsayılan olarak etkin) ile **myasaquickstartstorage** adlı bir depolama hesabı oluşturur.

2. Kullanılacak depolama hesabını tanımlayan `$storageAccount.Context` depolama hesabı bağlamını alın. Depolama hesaplarıyla çalışırken kimlik bilgilerini tekrar tekrar sağlamak yerine bağlama başvurursunuz.

3. [New-AzStorageContainer'ı](https://docs.microsoft.com/powershell/module/az.storage/new-azstoragecontainer)kullanarak bir depolama kapsayıcısı oluşturun.

4. Kod tarafından çıktı edilen depolama anahtarını kopyalayın ve daha sonra akış işinin çıktısını oluşturmak için bu anahtarı kaydedin.

    ```powershell
    $storageAccountName = "myasaquickstartstorage"
    $storageAccount = New-AzStorageAccount `
      -ResourceGroupName $resourceGroup `
      -Name $storageAccountName `
      -Location $location `
      -SkuName Standard_LRS `
      -Kind Storage

    $ctx = $storageAccount.Context
    $containerName = "container1"

    New-AzStorageContainer `
      -Name $containerName `
      -Context $ctx

    $storageAccountKey = (Get-AzStorageAccountKey `
      -ResourceGroupName $resourceGroup `
      -Name $storageAccountName).Value[0]

    Write-Host "The <storage account key> placeholder needs to be replaced in your output json files with this key value:"
    Write-Host $storageAccountKey -ForegroundColor Cyan
    ```

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

[New-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/new-azstreamanalyticsjob) cmdlet ile Bir Stream Analytics işi oluşturun. Bu cmdlet, iş adını, kaynak grubu adını ve iş tanımını parametre olarak alır. İş adı, işinizi tanımlayan herhangi bir kolay ad olabilir. Alfasayısal karakterler, tireler ve yalnızca alt çizerler olabilir ve 3 ile 63 karakter uzunluğunda olmalıdır. İş tanımı bir iş oluşturmak için gereken özellikleri içeren bir JSON dosyasıdır. Yerel makinenizde `JobDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin:

```json
{
  "location":"WestUS2",
  "properties":{
    "sku":{
      "name":"standard"
    },
    "eventsOutOfOrderPolicy":"adjust",
    "eventsOutOfOrderMaxDelayInSeconds":10,
    "compatibilityLevel": 1.1
  }
}
```

Ardından, `New-AzStreamAnalyticsJob` cmdlet'ini çalıştırın. Değişkenin `jobDefinitionFile` değerini iş tanımı JSON dosyasını depoladığınız yol ile değiştirin.

```powershell
$jobName = "MyStreamingJob"
$jobDefinitionFile = "C:\JobDefinition.json"
New-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -File $jobDefinitionFile `
  -Name $jobName `
  -Force
```

## <a name="configure-input-to-the-job"></a>İş girdisini yapılandırma

[New-AzStreamAnalyticsInput](https://docs.microsoft.com/powershell/module/az.streamanalytics/new-azstreamanalyticsinput) cmdlet'i kullanarak işinize bir giriş ekleyin. Bu cmdlet iş adı, iş girdisi adı, kaynak grubu adı ve iş girdisi tanımını parametre olarak alır. İş girdisi tanımı işin girdisini yapılandırmak için gereken özellikleri içeren bir JSON dosyasıdır. Bu örnekte, giriş olarak bir blob depolama oluşturursunuz.

Yerel makinenizde `JobInputDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. Değeri, önceki bir `accesspolicykey` bölüme `SharedAccessKey` kaydettiğiniz IoT Hub bağlantı dizesinin bölümüyle değiştirdiğinden emin olun.

```json
{
    "properties": {
        "type": "Stream",
        "datasource": {
            "type": "Microsoft.Devices/IotHubs",
            "properties": {
                "iotHubNamespace": "MyASAIoTHub",
                "sharedAccessPolicyName": "iothubowner",
                "sharedAccessPolicyKey": "accesspolicykey",
                "endpoint": "messages/events",
                "consumerGroupName": "$Default"
                }
        },
        "compression": {
            "type": "None"
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8"
            }
        }
    },
    "name": "IoTHubInput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/inputs"
}
```

`New-AzStreamAnalyticsInput` Ardından, cmdlet çalıştırın, iş giriş `jobDefinitionFile` tanımı JSON dosyasını depoladınız yolu ile değişkenin değerini değiştirmek için emin olun.

```powershell
$jobInputName = "IoTHubInput"
$jobInputDefinitionFile = "C:\JobInputDefinition.json"
New-AzStreamAnalyticsInput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobInputDefinitionFile `
  -Name $jobInputName
```

## <a name="configure-output-to-the-job"></a>İş çıktısını yapılandırma

[New-AzStreamAnalyticsOutput](https://docs.microsoft.com/powershell/module/az.streamanalytics/new-azstreamanalyticsoutput) cmdlet'i kullanarak işinize bir çıktı ekleyin. Bu cmdlet iş adı, iş çıktısı adı, kaynak grubu adı ve iş çıktısı tanımını parametre olarak alır. İş çıktısı tanımı işin çıktısını yapılandırmak için gereken özellikleri içeren bir JSON dosyasıdır. Bu örnek, çıktı olarak blob depolama kullanır.

Yerel makinenizde `JobOutputDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. `accountKey` değerini, depolama hesabınızın $storageAccountKey değerinde depolanmış değeri olan erişim anahtarıyla değiştirdiğinizden emin olun.

```json
{
    "properties": {
        "datasource": {
            "type": "Microsoft.Storage/Blob",
            "properties": {
                "storageAccounts": [
                    {
                      "accountName": "asaquickstartstorage",
                      "accountKey": "<storage account key>"
                    }
                ],
                "container": "container1",
                "pathPattern": "output/",
                "dateFormat": "yyyy/MM/dd",
                "timeFormat": "HH"
            }
        },
        "serialization": {
            "type": "Json",
            "properties": {
                "encoding": "UTF8",
                "format": "LineSeparated"
            }
        }
    },
    "name": "BlobOutput",
    "type": "Microsoft.StreamAnalytics/streamingjobs/outputs"
}
```

Ardından, `New-AzStreamAnalyticsOutput` cmdlet'ini çalıştırın. `jobOutputDefinitionFile` değişkeninin değerini, iş çıktısı tanımı JSON dosyasını depoladığınız yol ile değiştirdiğinizden emin olun.

```powershell
$jobOutputName = "BlobOutput"
$jobOutputDefinitionFile = "C:\JobOutputDefinition.json"
New-AzStreamAnalyticsOutput `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobOutputDefinitionFile `
  -Name $jobOutputName -Force
```

## <a name="define-the-transformation-query"></a>Dönüşüm sorgusunu tanımlama

[New-AzStreamAnalyticsTransformation](https://docs.microsoft.com/powershell/module/az.streamanalytics/new-azstreamanalyticstransformation) cmdlet'i kullanarak işinizde bir dönüşüm ekleyin. Bu cmdlet iş adı, iş dönüşümü adı, kaynak grubu adı ve iş dönüşümü tanımını parametre olarak alır. Yerel makinenizde `JobTransformationDefinition.json` adlı bir dosya oluşturun ve içine aşağıdaki JSON verilerini ekleyin. JSON dosyası, dönüşüm sorgusunu tanımlayan bir sorgu parametresi içerir:

```json
{
    "name":"MyTransformation",
    "type":"Microsoft.StreamAnalytics/streamingjobs/transformations",
    "properties":{
        "streamingUnits":1,
        "script":null,
        "query":" SELECT * INTO BlobOutput FROM IoTHubInput HAVING Temperature > 27"
    }
}
```

Ardından, `New-AzStreamAnalyticsTransformation` cmdlet'ini çalıştırın. Değişkenin `jobTransformationDefinitionFile` değerini iş dönüşümü tanımı JSON dosyasını depoladığınız yolile değiştirdiğinden emin olun.

```powershell
$jobTransformationName = "MyJobTransformation"
$jobTransformationDefinitionFile = "C:\JobTransformationDefinition.json"
New-AzStreamAnalyticsTransformation `
  -ResourceGroupName $resourceGroup `
  -JobName $jobName `
  -File $jobTransformationDefinitionFile `
  -Name $jobTransformationName -Force
```

## <a name="run-the-iot-simulator"></a>IoT simülatörünü çalıştırın

1. [Raspberry Pi Azure IoT Online Simülatörü'nü](https://azure-samples.github.io/raspberry-pi-web-simulator/)açın.

2. Satır 15'teki yer tutucuyu önceki bölümde kaydettiğiniz tüm Azure IoT Hub Aygıtı bağlantı dizesiyle değiştirin.

3. **Çalıştır**’a tıklayın. Çıktı, IoT Hub'ınıza gönderilen sensör verilerini ve iletilerini göstermelidir.

    ![Raspberry Pi Azure IoT Çevrimiçi Simülatörü](./media/stream-analytics-quick-create-powershell/ras-pi-connection-string.png)

## <a name="start-the-stream-analytics-job-and-check-the-output"></a>Stream Analytics işini başlatıp çıktıyı denetleyin

[Start-AzStreamAnalyticsJob](https://docs.microsoft.com/powershell/module/az.streamanalytics/start-azstreamanalyticsjob) cmdlet'i kullanarak işe başlayın. Bu cmdlet iş adı, kaynak grubu adı, çıktı başlangıç modu ve başlangıç saatini parametre olarak alır. `OutputStartMode`; `JobStartTime`, `CustomTime` veya `LastOutputEventTime` değerlerini kabul eder. Bu değerlerin her birinin ne anlama geldiği hakkında daha fazla bilgi için PowerShell belgelerindeki [parametreler](https://docs.microsoft.com/powershell/module/az.streamanalytics/start-azstreamanalyticsjob) bölümüne bakın.

Aşağıdaki cmdlet’i çalıştırdıktan sonra iş başlarsa çıktı olarak `True` değeri döndürülür. Depolama kapsayıcısında, dönüştürülmüş verilerle birlikte bir çıktı klasörü oluşturulur.

```powershell
Start-AzStreamAnalyticsJob `
  -ResourceGroupName $resourceGroup `
  -Name $jobName `
  -OutputStartMode 'JobStartTime'
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, akış işini ve tüm ilgili kaynakları silin. İşin silinmesi, iş tarafından kullanılan akış birimlerinin faturalanmasını önler. İşi gelecekte kullanmayı planlıyorsanız, silme işlemini atlayıp işi şimdilik durdurabilirsiniz. Bu işi kullanmaya devam etmeyecekseniz, aşağıdaki cmdlet'i çalıştırarak bu hızlı başlatmanın oluşturduğu tüm kaynakları silin:

```powershell
Remove-AzResourceGroup `
  -Name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta PowerShell kullanarak basit bir Stream Analytics işi dağıttınız. Stream Analytics işlerini [Azure portalı](stream-analytics-quick-create-portal.md) ve [Visual Studio](stream-analytics-quick-create-vs.md)’yu kullanarak da dağıtabilirsiniz.

Diğer girdi kaynaklarını yapılandırma ve gerçek zamanlı algılama hakkında bilgi almak için aşağıdaki makaleye geçin:

> [!div class="nextstepaction"]
> [Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama](stream-analytics-real-time-fraud-detection.md)
