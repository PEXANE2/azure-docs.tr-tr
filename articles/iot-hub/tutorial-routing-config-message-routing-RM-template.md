---
title: Azure Kaynak Yöneticisi şablonu kullanarak Azure IoT Hub için ileti yönlendirmeyi yapılandırma
description: Azure Kaynak Yöneticisi şablonu kullanarak Azure IoT Hub için ileti yönlendirmeyi yapılandırma
author: robinsh
manager: philmeagit st
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 8f245653a8b84944e1e8a3f48a49992f0065be58
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74084400"
---
# <a name="tutorial-use-an-azure-resource-manager-template-to-configure-iot-hub-message-routing"></a>Öğretici: IoT Hub ileti yönlendirmesini yapılandırmak için Azure Kaynak Yöneticisi şablonu kullanın

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="message-routing"></a>İleti yönlendirme

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

## <a name="download-the-template-and-parameters-file"></a>Şablon ve parametreler dosyasını indirin

Bu öğreticinin ikinci bölümü için, IoT Hub'a ileti göndermek için bir Visual Studio uygulamasını indirip çalıştırAbilirsiniz. Bu indirmede Azure Kaynak Yöneticisi şablonu ve parametreler dosyasının yanı sıra Azure CLI ve PowerShell komut dosyalarını içeren bir klasör vardır.

Azure [IoT C# Örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) hemen indirin. Master.zip dosyasının zip'ini açın. Kaynak Yöneticisi şablonu ve parametreler dosyası /iot-hub/Tutorials/Routing/SimulatedDevice/resources/ as **template_iothub.json** ve **template_iothub_parameters.json**bulunmaktadır.

## <a name="create-your-resources"></a>Kaynaklarınızı oluşturun

Tüm kaynaklarınızı oluşturmak için bir Azure Kaynak Yöneticisi (RM) şablonu kullanacaksınız. Azure CLI ve PowerShell komut dosyaları aynı anda birkaç satır çalıştırılabilir. Bir RM şablonu tek adımda dağıtılır. Bu makalede, her birini anlamanıza yardımcı olmak için bölümleri ayrı ayrı gösterir. Ardından şablonu nasıl dağıtacağınızı gösterir ve sınama için sanal aygıtı oluşturur. Şablon dağıtıldıktan sonra, portaldaki ileti yönlendirme yapılandırmasını görüntüleyebilirsiniz.

IoT Hub adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adı vardır. Kaynakları adlandırmayı kolaylaştırmak için, bu kaynak adları geçerli tarih/saat oluşturulan rasgele bir alfasayısal değeri eklemek için ayarlanır. 

Şablona bakarsanız, parametreyi alan ve *rasgele Değer'i* parametreye oluşturan bu kaynaklar için değişkenlerin nerede ayarlandığı görürsünüz. 

Aşağıdaki bölümde kullanılan parametreler açıklanmaktadır.

### <a name="parameters"></a>Parametreler

Bu parametrelerin çoğu varsayılan değerlere sahiptir. **_in** ile bitenler *randomValue* ile küresel olarak benzersiz hale getirmek için birleştirilmiştir. 

**randomValue**: Bu değer, şablonu dağıttığınızda geçerli tarihten/saatlerde oluşturulur. Bu alan, şablonun kendisinde oluşturulduğu için parametreler dosyasında bulunmaz.

**subscriptionId**: Bu alan, şablonu dağıttığınız abonelik için ayarlanır. Bu alan sizin için ayarlı olduğundan parametreler dosyasında değildir.

**IoTHubName_in**: Bu alan, randomValue ile küresel olarak benzersiz olarak kısıtlanan temel IoT Hub adıdır.

**konum**: Bu alan, "westus" gibi dağıtdığınız Azure bölgesidir.

**consumer_group**: Bu alan, yönlendirme bitiş noktasından gelen iletiler için ayarlanan tüketici grubudur. Azure Akış Analizi'ndeki sonuçları filtrelemek için kullanılır. Örneğin, her şeyi aldığınız tüm akış vardır veya **Contoso'ya**ayarlanmış consumer_group ile gelen verileriniz varsa, yalnızca bu girişleri göstermek için bir Azure Akışı Analizi akışı (ve Power BI raporu) ayarlayabilirsiniz. Bu alan, bu öğreticinin bölüm 2'sinde kullanılır.

**sku_name**: Bu alan IoT Hub'ı için ölçeklemedir. Bu değer S1 veya üzeri olmalıdır; birden fazla uç noktaya izin vermemektedir, çünkü ücretsiz bir katman bu öğretici için çalışmıyor.

**sku_units**: Bu alan **sku_name**ile gider ve kullanılabilecek IoT Hub birimlerinin sayısıdır.

**d2c_partitions**: Bu alan olay akışı için kullanılan bölüm sayısıdır.

**storageAccountName_in**: Bu alan oluşturulacak depolama hesabının adıdır. İletiler depolama hesabındaki bir kapsayıcıya yönlendirilir. Bu alan randomValue ile küresel olarak benzersiz hale getirmek için birleştirilmiştir.

**storageContainerName**: Bu alan, depolama hesabına yönlendirilen iletilerin depolandığı kapsayıcının adıdır.

**storage_endpoint**: Bu alan, ileti yönlendirmesi tarafından kullanılan depolama hesabı bitiş noktasının adıdır.

**service_bus_namespace_in**: Bu alan oluşturulacak Servis Veri Günü ad alanının adıdır. Bu değer randomValue ile küresel olarak benzersiz hale getirmek için birleştirilmiştir.

**service_bus_queue_in**: Bu alan, iletileri yönlendirmek için kullanılan Servis Veri Servisi kuyruğunun adıdır. Bu değer randomValue ile küresel olarak benzersiz hale getirmek için birleştirilmiştir.

**AuthRules_sb_queue**: Bu alan, sıranın bağlantı dizesini almak için kullanılan servis veri servisi sırası için yetkilendirme kurallarıdır.

### <a name="variables"></a>Değişkenler

Bu değerler şablonda kullanılır ve çoğunlukla parametrelerden türetilmiştir.

**queueAuthorizationRuleResourceId**: Bu alan, Hizmet Veri Servisi sırası için yetkilendirme kuralı için ResourceId'dir. ResourceId sırayla sırayla sırayla bağlantı dizesini almak için kullanılır.

**iotHubName**: Bu alan randomValue concatenated sonra IoT Hub adıdır. 

**storageAccountName**: Bu alan randomValue'in birleştirilmiş olması sonrasında depolama hesabının adıdır. 

**service_bus_namespace**: Bu alan randomValue concatenated sonra namespace olduğunu.

**service_bus_queue**: Bu alan randomValue concatenated sonra Servis Veri Servisi sıra adıdır.

**sbVersion**: Kullanılacak Servis Veri Servisi API'sinin THe sürümü. Bu durumda, "2017-04-01".

### <a name="resources-storage-account-and-container"></a>Kaynaklar: Depolama hesabı ve kapsayıcı

Oluşturulan ilk kaynak, iletilerin yönlendirildiği kapsayıcıyla birlikte depolama hesabıdır. Kapsayıcı depolama hesabı altında bir kaynaktır. Depolama hesabı `dependsOn` için, depolama hesabının kapsayıcıdan önce oluşturulmasını gerektiren bir yan tümcesi vardır.

Bu bölüm şu şekilde görünür:

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2018-07-01",
    "location": "[parameters('location')]",
    "sku": {
        "name": "Standard_LRS",
        "tier": "Standard"
    },
    "kind": "Storage",
    "properties": {},
    "resources": [
        {
        "type": "blobServices/containers",
        "apiVersion": "2018-07-01",
        "name": "[concat('default/', parameters('storageContainerName'))]",
        "properties": {
            "publicAccess": "None"
            } ,
        "dependsOn": [
            "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]"
            ]
        }
    ]
}
```

### <a name="resources-service-bus-namespace-and-queue"></a>Kaynaklar: Servis Veri Günü ad alanı ve sıra

Oluşturulan ikinci kaynak, iletilerin yönlendirildiği Servis Veri Yolu kuyruğuyla birlikte Servis Veri Yolu ad alanıdır. SKU standart olarak ayarlanır. API sürümü değişkenlerden alınır. Ayrıca, bu bölümü dağıttığında Hizmet Veri Servisi ad alanını etkinleştirmek için ayarlanır (durum:Etkin). 

```json
{
    "type": "Microsoft.ServiceBus/namespaces",
    "comments": "The Sku should be 'Standard' for this tutorial.",
    "sku": {
        "name": "Standard",
        "tier": "Standard"
    },
    "name": "[variables('service_bus_namespace')]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "properties": {
        "provisioningState": "Succeeded",
        "metricId": "[concat('a4295411-5eff-4f81-b77e-276ab1ccda12:', variables('service_bus_namespace'))]",
        "serviceBusEndpoint": "[concat('https://', variables('service_bus_namespace'),'.servicebus.windows.net:443/')]",
        "status": "Active"
    },
    "dependsOn": []
}
```

Bu bölümde Servis Veri Servisi kuyruğu oluşturulur. Komut dosyasının bu `dependsOn` bölümünde, ad alanının kuyruktan önce oluşturulmasını sağlayan bir yan tümce vardır.

```json
{
    "type": "Microsoft.ServiceBus/namespaces/queues",
    "name": "[concat(variables('service_bus_namespace'), '/', variables('service_bus_queue'))]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "scale": null,
    "properties": {},
    "dependsOn": [
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]"
    ]
}
```

### <a name="resources-iot-hub-and-message-routing"></a>Kaynaklar: Iot Hub ve ileti yönlendirme

Depolama hesabı ve Servis Veri Yolu kuyruğu oluşturulduğuna göre, iletileri onlara yollayan IoT Hub'ını oluşturursunuz. RM şablonu, Hizmet Veri Servisi kaynakları ve depolama hesabı oluşturulmadan önce hub oluşturmaya çalışmaması için yan tümceleri kullanır. `dependsOn` 

İşte IoT Hub bölümünün ilk bölümü. Şablonun bu bölümü bağımlılıkları ayarlar ve özellikleriyle başlar.

```json
{
    "apiVersion": "2018-04-01",
    "type": "Microsoft.Devices/IotHubs",
    "name": "[variables('IoTHubName')]",
    "location": "[parameters('location')]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces/queues', variables('service_bus_namespace'), variables('service_bus_queue'))]"
    ],
    "properties": {
        "eventHubEndpoints": {}
            "events": {
                "retentionTimeInDays": 1,
                "partitionCount": "[parameters('d2c_partitions')]"
                }
            },
```

Sonraki bölüm, Iot Hub için ileti yönlendirme yapılandırması bölümüdür. Birincisi uç noktaların bölümüdür. Şablonun bu bölümü, Bağlantı dizeleri de dahil olmak üzere Servis Veri Servisi kuyruğu ve depolama hesabı için yönlendirme uç noktalarını ayarlar.

Sıranın bağlantı dizesini oluşturmak için satır içinde alınan YetkilendirmeKurallarıResourcedId'e ihtiyacınız var. Depolama hesabı için bağlantı dizesi oluşturmak için birincil depolama anahtarını alır ve ardından bağlantı dizesi için biçimde kullanırsınız.

Uç nokta yapılandırması, blob biçimini `AVRO` veya `JSON`.

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

 ```json
"routing": {
    "endpoints": {
        "serviceBusQueues": [
        {
            "connectionString": "[Concat('Endpoint=sb://',variables('service_bus_namespace'),'.servicebus.windows.net/;SharedAccessKeyName=',parameters('AuthRules_sb_queue'),';SharedAccessKey=',listkeys(variables('queueAuthorizationRuleResourceId'),variables('sbVersion')).primaryKey,';EntityPath=',variables('service_bus_queue'))]",
            "name": "[parameters('service_bus_queue_endpoint')]",
            "subscriptionId": "[parameters('subscriptionId')]", 
            "resourceGroup": "[resourceGroup().Name]"
        }
        ],
        "serviceBusTopics": [],
        "eventHubs": [],
        "storageContainers": [
            {
                "connectionString": 
                "[Concat('DefaultEndpointsProtocol=https;AccountName=',variables('storageAccountName'),';AccountKey=',listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('storageAccountName')), providers('Microsoft.Storage', 'storageAccounts').apiVersions[0]).keys[0].value)]",
                "containerName": "[parameters('storageContainerName')]",
                "fileNameFormat": "{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}",
                "batchFrequencyInSeconds": 100,
                "maxChunkSizeInBytes": 104857600,
                "encoding": "avro",
                "name": "[parameters('storage_endpoint')]",
                "subscriptionId": "[parameters('subscriptionId')]",
                "resourceGroup": "[resourceGroup().Name]"
            }
        ]
    },
```

Bir sonraki bölüm, uç noktalara ileti yolları içindir. Her bitiş noktası için bir ayar vardır, bu nedenle Servis Veri Servisi sırası için bir tane ve depolama hesabı kapsayıcısı için bir tane vardır.

Depolamaya yönlendirilen iletilerin sorgu koşulunun `level="storage"`ve Servis Veri Yolu kuyruğuna yönlendirilen iletilerin sorgu `level="critical"`koşulu olduğunu unutmayın.

```json
"routes": [
    {
        "name": "contosoStorageRoute",
        "source": "DeviceMessages",
        "condition": "level=\"storage\"",
        "endpointNames": [
            "[parameters('storage_endpoint')]"
            ],
        "isEnabled": true
    },
    {
        "name": "contosoSBQueueRoute",
        "source": "DeviceMessages",
        "condition": "level=\"critical\"",
        "endpointNames": [
            "[parameters('service_bus_queue_endpoint')]"
            ],
        "isEnabled": true
    }
],
```

Bu json varsayılan bilgileri ve hub için SKU içeren IoT Hub bölümünün geri kalanını gösterir.

```json
            "fallbackRoute": {
                "name": "$fallback",
                "source": "DeviceMessages",
                "condition": "true",
                "endpointNames": [
                    "events"
                ],
                "isEnabled": true
            }
        },
        "storageEndpoints": {
            "$default": {
                "sasTtlAsIso8601": "PT1H",
                "connectionString": "",
                "containerName": ""
            }
        },
        "messagingEndpoints": {
            "fileNotifications": {
                "lockDurationAsIso8601": "PT1M",
                "ttlAsIso8601": "PT1H",
                "maxDeliveryCount": 10
            }
        },
        "enableFileUploadNotifications": false,
        "cloudToDevice": {
            "maxDeliveryCount": 10,
            "defaultTtlAsIso8601": "PT1H",
            "feedback": {
                "lockDurationAsIso8601": "PT1M",
                "ttlAsIso8601": "PT1H",
                "maxDeliveryCount": 10
            }
        }
    },
    "sku": {
        "name": "[parameters('sku_name')]",
        "capacity": "[parameters('sku_units')]"
    }
}
```

### <a name="resources-service-bus-queue-authorization-rules"></a>Kaynaklar: Servis Veri Servisi sıra yetkilendirme kuralları

Hizmet Veri Servisi sıra yetkilendirme kuralı, Hizmet Veri Servisi kuyruğunun bağlantı dizesini almak için kullanılır. Hizmet Veri `dependsOn` Servisi ad alanı ve Servis Veri Servisi sırasını önce oluşturulmamasını sağlamak için bir yan tümce kullanır.

```json
{
    "type": "Microsoft.ServiceBus/namespaces/queues/authorizationRules",
    "name": "[concat(variables('service_bus_namespace'), '/', variables('service_bus_queue'), '/', parameters('AuthRules_sb_queue'))]",
    "apiVersion": "[variables('sbVersion')]",
    "location": "[parameters('location')]",
    "scale": null,
    "properties": {
        "rights": [
            "Send"
        ]
    },
    "dependsOn": [
        "[resourceId('Microsoft.ServiceBus/namespaces', variables('service_bus_namespace'))]",
        "[resourceId('Microsoft.ServiceBus/namespaces/queues', variables('service_bus_namespace'), variables('service_bus_queue'))]"
    ]
},
```

### <a name="resources-consumer-group"></a>Kaynaklar: Tüketici grubu

Bu bölümde, bu öğreticinin ikinci bölümünde Azure Akış Analizi tarafından kullanılacak IoT Hub verileri için bir Tüketici Grubu oluşturursunuz.

```json
{
    "type": "Microsoft.Devices/IotHubs/eventHubEndpoints/ConsumerGroups",
    "name": "[concat(variables('iotHubName'), '/events/',parameters('consumer_group'))]",
    "apiVersion": "2018-04-01",
    "dependsOn": [
        "[concat('Microsoft.Devices/IotHubs/', variables('iotHubName'))]"
    ]
}
```

### <a name="resources-outputs"></a>Kaynaklar: Çıktılar

Görüntülenmek üzere dağıtım komut dosyasına bir değer geri göndermek istiyorsanız, bir çıktı bölümü kullanırsınız. Şablonun bu bölümü, Servis Veri Servisi kuyruğunun bağlantı dizesini döndürür. Bir değeri döndürmek gerekli değildir, sonuçları arama komut dosyasına nasıl döndüreceklerine bir örnek olarak dahildir.

```json
"outputs": {
    "sbq_connectionString": {
      "type": "string",
      "value": "[Concat('Endpoint=sb://',variables('service_bus_namespace'),'.servicebus.windows.net/;SharedAccessKeyName=',parameters('AuthRules_sb_queue'),';SharedAccessKey=',listkeys(variables('queueAuthorizationRuleResourceId'),variables('sbVersion')).primaryKey,';EntityPath=',variables('service_bus_queue'))]"
    }
  }
```

## <a name="deploy-the-rm-template"></a>RM şablonu dağıtma

Şablonu Azure'a dağıtmak için şablonu ve parametreler dosyasını Azure Bulut BulutU'na yükleyin ve ardından şablonu dağıtmak için bir komut dosyası çalıştırın. Azure Bulut Kabuğu'nu açın ve oturum açın. Bu örnekpowershell kullanır.

Dosyaları yüklemek için menü çubuğundaki **Dosyaları Yükle/İndir** simgesini seçin ve ardından Yükle'yi seçin.

![Yükleme/İndir dosyaları vurgulanan Bulut Kabuğu menü çubuğu](media/tutorial-routing-config-message-routing-RM-template/CloudShell_upload_files.png)

Yerel diskinizdeki dosyaları bulmak ve seçmek için açılan Dosya Gezgini'ni kullanın ve ardından **Aç'ı**seçin.

Dosyalar yüklendikten sonra, sonuç iletişim kutusu aşağıdaki resim gibi bir şey gösterir.

![Yükleme/İndir dosyaları vurgulanan Bulut Kabuğu menü çubuğu](media/tutorial-routing-config-message-routing-RM-template/CloudShell_upload_results.png)

Dosyalar Bulut Kabuğu örneğinizin kullandığı paylaşıma yüklenir. 

Dağıtımı gerçekleştirmek için komut dosyasını çalıştırın. Bu komut dosyasının son satırı, döndürülecek şekilde ayarlanan değişkeni alır -- Servis Veri Servisi sıra bağlantı dizesi.

Komut dosyası bu değişkenleri ayarlar ve kullanır:

**$RGName,** şablonu dağıtacak kaynak grubu adıdır. Bu alan şablonu dağıtmadan önce oluşturulur.

**$location** şablon için kullanılacak Azure konumudur( örneğin"westus").

**deploymentname,** dönen değişken değerini almak için dağıtıma atadığınız bir addır.

İşte PowerShell komut dosyası. Bu PowerShell komut dosyasını kopyalayın ve Cloud Shell penceresine yapıştırın ve çalıştırmak için Enter tuşuna basın.

```powershell
$RGName="ContosoResources"
$location = "westus"
$deploymentname="contoso-routing"

# Remove the resource group if it already exists. 
#Remove-AzResourceGroup -name $RGName 
# Create the resource group.
New-AzResourceGroup -name $RGName -Location $location 

# Set a path to the parameter file. 
$parameterFile = "$HOME/template_iothub_parameters.json"
$templateFile = "$HOME/template_iothub.json"

# Deploy the template.
New-AzResourceGroupDeployment `
    -Name $deploymentname `
    -ResourceGroupName $RGName `
    -TemplateParameterFile $parameterFile `
    -TemplateFile $templateFile `
    -verbose

# Get the returning value of the connection string.
(Get-AzResourceGroupDeployment -ResourceGroupName $RGName -Name $deploymentname).Outputs.sbq_connectionString.value
```

Komut dosyası hatalarınız varsa, komut dosyasını yerel olarak dinleyebilir, Bulut Kabuğu'na yeniden yükleyebilir ve komut dosyasını yeniden çalıştırabilirsiniz. Komut dosyası başarıyla çalıştırıladıktan sonra bir sonraki adıma devam edin.

## <a name="create-simulated-device"></a>Sanal cihaz oluşturma

[!INCLUDE [iot-hub-include-create-simulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

## <a name="view-message-routing-in-the-portal"></a>İleti yönlendirmesini portalda görüntüleme

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık tüm kaynaklar ayarladığınız ve ileti yolları yapılandırıldığınızda, yönlendirilen iletilerle ilgili bilgileri nasıl işleyip görüntülediğinizi öğrenmek için bir sonraki öğreticiye ilerleyin.

> [!div class="nextstepaction"]
> [Bölüm 2 - İleti yönlendirme sonuçlarını görüntüleyin](tutorial-routing-view-message-routing-results.md)
