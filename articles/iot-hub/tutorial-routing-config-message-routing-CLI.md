---
title: Azure CLI'yi kullanarak Azure IoT Hub için ileti yönlendirmesini yapılandırma
description: Azure CLI'yi kullanarak Azure IoT Hub için ileti yönlendirmesini yapılandırın. İletideki özelliklere bağlı olarak, bir depolama hesabı veya Servis Veri Yolu kuyruğuna yönlendirin.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 056dac7977115f97892d8dbfde0710e00237804e
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78674338"
---
# <a name="tutorial-use-the-azure-cli-to-configure-iot-hub-message-routing"></a>Öğretici: IoT Hub ileti yönlendirmesini yapılandırmak için Azure CLI'yi kullanın

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="download-the-script-optional"></a>Komut dosyasını indirin (isteğe bağlı)

Bu öğreticinin ikinci bölümü için, IoT Hub'a ileti göndermek için bir Visual Studio uygulamasını indirip çalıştırAbilirsiniz. İndirmede Azure Kaynak Yöneticisi şablonu ve parametreler dosyasının yanı sıra Azure CLI ve PowerShell komut dosyalarını içeren bir klasör vardır.

Tamamlanan komut dosyasını görüntülemek istiyorsanız, [Azure IoT C# Örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)indirin. Master.zip dosyasının zip'ini açın. Azure CLI komut dosyası /iot-hub/Tutorials/Routing/SimulatedDevice/resources/ as **iothub_routing_cli.azcli'dedir.**

## <a name="use-the-azure-cli-to-create-your-resources"></a>Kaynaklarınızı oluşturmak için Azure CLI'yi kullanın

Aşağıdaki komut dosyasını Bulut Kabuğu'na kopyalayıp yapıştırın ve Enter tuşuna basın. Senaryoyu teker teksatır çalıştırAn bir senaryo. Komut dosyasının bu ilk bölümü, depolama hesabı, IoT Hub, Servis Veri Merkezi Ad Alanı ve Hizmet Veri Servisi kuyruğu da dahil olmak üzere bu öğretici için temel kaynakları oluşturur. Öğreticinin geri kalanında, komut dosyasının her bloğunu kopyalayın ve çalıştırmak için Cloud Shell'e yapıştırın.

> [!TIP]
> Hata ayıklama hakkında bir ipucu: Bu komut dosyası `\`komut dosyası daha okunabilir hale getirmek için devamı sembolü (backslash) kullanır. Komut dosyasını çalıştırmakta sorun varsa, Bulut Bulut `bash` oturumunuzun çalıştığından ve herhangi bir ters eğik çizgiden sonra boşluk olmadığından emin olun.
> 

IoT Hub adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adı vardır. Bunu kolaylaştırmak için, bu kaynak adları *randomValue*adı verilen rasgele alfasayısal bir değerle eklenir. RandomValue komut dosyasının en üstünde bir kez oluşturulur ve komut dosyası boyunca gerektiğinde kaynak adlarına eklenir. Rasgele olmasını istemiyorsanız, boş bir dize veya belirli bir değere ayarlayabilirsiniz. 

> [!IMPORTANT]
> İlk komut dosyasında ayarlanan değişkenler yönlendirme komut dosyası tarafından da kullanılır, bu nedenle tüm komut dosyasını aynı Bulut Bulut suşu oturumunda çalıştırın. Yönlendirmeyi ayarlamak için komut dosyasını çalıştırmak için yeni bir oturum açarsanız, değişkenlerden birkaçı eksik değerler olur.
>

```azurecli-interactive
# This command retrieves the subscription id of the current Azure account. 
# This field is used when setting up the routing queries.
subscriptionID=$(az account show --query id -o tsv)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# This command installs the IOT Extension for Azure CLI.
# You only need to install this the first time.
# You need it to create the device identity. 
az extension add --name azure-iot

# Set the values for the resource names that 
#   don't have to be globally unique.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique, 
#   so add a random value to the end.
iotHubName=ContosoTestHub$randomValue 
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# Add a consumer group to the IoT hub for the 'events' endpoint.
az iot hub consumer-group create --hub-name $iotHubName \
    --name $iotHubConsumerGroup

# The storage account name must be globally unique, 
#   so add a random value to the end.
storageAccountName=contosostorage$randomValue
echo "Storage account name = " $storageAccountName

# Create the storage account to be used as a routing destination.
az storage account create --name $storageAccountName \
    --resource-group $resourceGroup \
    --location $location \
    --sku Standard_LRS

# Get the primary storage account key. 
#    You need this to create the container.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"') 

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the container in the storage account. 
az storage container create --name $containerName \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# The Service Bus namespace must be globally unique, 
#   so add a random value to the end.
sbNamespace=ContosoSBNamespace$randomValue
echo "Service Bus namespace = " $sbNamespace

# Create the Service Bus namespace.
az servicebus namespace create --resource-group $resourceGroup \
    --name $sbNamespace \
    --location $location

# The Service Bus queue name must be globally unique, 
#   so add a random value to the end.
sbQueueName=ContosoSBQueue$randomValue
echo "Service Bus queue name = " $sbQueueName

# Create the Service Bus queue to be used as a routing destination.
az servicebus queue create --name $sbQueueName \
    --namespace-name $sbNamespace \
    --resource-group $resourceGroup

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName
```

Temel kaynaklar ayarlıolduğundan, ileti yönlendirmesini yapılandırabilirsiniz.

## <a name="set-up-message-routing"></a>İleti yönlendirmeyi ayarlama

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

Yönlendirme bitiş noktası oluşturmak için [az iot hub yönlendirme-bitiş noktası oluşturma'yı](/cli/azure/iot/hub/routing-endpoint?view=azure-cli-latest#az-iot-hub-routing-endpoint-create)kullanın. Bitiş noktası için ileti yolu oluşturmak için [az iot hub rotasını kullanın.](/cli/azure/iot/hub/route?view=azure-cli-latest#az-iot-hub-route-create)

### <a name="route-to-a-storage-account"></a>Depolama hesabına yönlendirme

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

Önce depolama hesabının bitiş noktasını ayarlayın, ardından rotayı ayarlayın. 

Bulut Bulut oturumunuzda ayarlanabilmesi gereken komut dosyası tarafından kullanılan değişkenler şunlardır:

**storageConnectionString**: Bu değer önceki komut dosyasında ayarlanan depolama hesabından alınır. Depolama hesabına erişmek için ileti yönlendirmesi tarafından kullanılır.

  **resourceGroup**: Kaynak grubunun iki olayı vardır - bunları kaynak grubunuza ayarlayın.

**endpoint subscriptionID**: Bu alan bitiş noktası için Azure subscriptionID olarak ayarlanır. 

**endpointType**: Bu alan bitiş noktası türüdür. Bu `azurestoragecontainer`değer , , `eventhub` `servicebusqueue`, `servicebustopic`veya . olarak ayarlanmalıdır. Buradaki amaçlarınız için, `azurestoragecontainer`bunu .

**iotHubName**: Bu alan yönlendirme yapacak hub adıdır.

**containerName**: Bu alan, verilerin yazılacağı depolama hesabındaki kapsayıcının adıdır.

**kodlama**: Bu alan `avro` ya `json`da . Bu, depolanan verilerin biçimini gösterir.

**routeName**: Bu alan, ayarladığınız rotanın adıdır. 

**endpointName**: Bu alan bitiş noktasını tanımlayan addır. 

**etkin**: Bu alan, ileti rotasıoluşturulduktan sonra etkinleştirilmesi gerektiğini belirten `true`, varsayılan.

**koşul**: Bu alan, bu bitiş noktasına gönderilen iletileri filtrelemek için kullanılan sorgudur. Depolamaya yönlendirilen iletilerin sorgu koşulu. `level="storage"`

Bu komut dosyasını kopyalayın ve Cloud Shell pencerenize yapıştırın ve çalıştırın.

```azurecli
##### ROUTING FOR STORAGE ##### 

endpointName="ContosoStorageEndpoint"
endpointType="azurestoragecontainer"
routeName="ContosoStorageRoute"
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)
```

Bir sonraki adım, depolama hesabı için yönlendirme bitiş noktasını oluşturmaktır. Ayrıca, sonuçların depolanacağı kapsayıcıyı da belirtirsiniz. Kapsayıcı daha önce depolama hesabı oluşturulduğunda oluşturuldu.

```azurecli
# Create the routing endpoint for storage.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName \
  --resource-group $resourceGroup \
  --encoding avro
```

Ardından, depolama bitiş noktası için rota oluşturun. İleti rotası, sorgu belirtimini karşılayan iletilerin nereye gönderilen yeri belirler. 

```azurecli
# Create the route for the storage endpoint.
az iot hub route create \
  --name $routeName \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName \
  --enabled \
  --condition $condition
```

### <a name="route-to-a-service-bus-queue"></a>Servis Veri Yolu kuyruğuna giden yol

Şimdi Service Bus kuyruğu için yönlendirmeyi ayarlayın. Hizmet Veri Servisi kuyruğunun bağlantı dizesini almak için, doğru hakları tanımlayan bir yetkilendirme kuralı oluşturmanız gerekir. Aşağıdaki komut dosyası, Hizmet Veri Servisi sırasına `sbauthrule`göre bir yetkilendirme `Listen Manage Send`kuralı oluşturur ve '' adlı bir yetkilendirme kuralı oluşturur. Bu yetkilendirme kuralı tanımlandıktan sonra, sıranın bağlantı dizesini almak için kullanabilirsiniz.

```azurecli
# Create the authorization rule for the Service Bus queue.
az servicebus queue authorization-rule create \
  --name "sbauthrule" \
  --namespace-name $sbNamespace \
  --queue-name $sbQueueName \
  --resource-group $resourceGroup \
  --rights Listen Manage Send \
  --subscription $subscriptionID
```

Şimdi Hizmet Veri Servisi sırasına bağlantı dizesini almak için yetkilendirme kuralını kullanın.

```azurecli
# Get the Service Bus queue connection string.
# The "-o tsv" ensures it is returned without the default double-quotes.
sbqConnectionString=$(az servicebus queue authorization-rule keys list \
  --name "sbauthrule" \
  --namespace-name $sbNamespace \
  --queue-name $sbQueueName \
  --resource-group $resourceGroup \
  --subscription $subscriptionID \
  --query primaryConnectionString -o tsv)

# Show the Service Bus queue connection string.
echo "service bus queue connection string = " $sbqConnectionString
```

Şimdi yönlendirme bitiş noktasını ve Servis Veri Yolu kuyruğu için ileti rotasını ayarlayın. Bulut Bulut oturumunuzda ayarlanabilmesi gereken komut dosyası tarafından kullanılan değişkenler şunlardır:

**endpointName**: Bu alan bitiş noktasını tanımlayan addır. 

**endpointType**: Bu alan bitiş noktası türüdür. Bu `azurestoragecontainer`değer , , `eventhub` `servicebusqueue`, `servicebustopic`veya . olarak ayarlanmalıdır. Buradaki amaçlarınız için, `servicebusqueue`bunu .

**routeName**: Bu alan, ayarladığınız rotanın adıdır. 

**koşul**: Bu alan, bu bitiş noktasına gönderilen iletileri filtrelemek için kullanılan sorgudur. Servis Veri Yolu sırasına yönlendirilen iletilerin sorgu `level="critical"`koşulu.

Yönlendirme bitiş noktası için Azure CLI ve Servis Veri Yolu kuyruğu için ileti rotası aşağıda verebistir.

```azurecli
endpointName="ContosoSBQueueEndpoint"
endpointType="ServiceBusQueue"
routeName="ContosoSBQueueRoute"
condition='level="critical"'

# Set up the routing endpoint for the Service Bus queue.
# This uses the Service Bus queue connection string.
az iot hub routing-endpoint create \
  --connection-string $sbqConnectionString \
  --endpoint-name $endpointName \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --resource-group $resourceGroup 

# Set up the message route for the Service Bus queue endpoint.
az iot hub route create --name $routeName \
  --hub-name $iotHubName \
  --source-type devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName \
  --enabled \
  --condition $condition
  ```

### <a name="view-message-routing-in-the-portal"></a>İleti yönlendirmesini portalda görüntüleme

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kaynakların ayarlı olduğuna ve ileti rotalarının yapılandırıldığına göre, IoT hub'ına ileti göndermeyi öğrenmek ve farklı hedeflere yönlendirilmelerini görmek için bir sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Bölüm 2 - İleti yönlendirme sonuçlarını görüntüleyin](tutorial-routing-view-message-routing-results.md)
