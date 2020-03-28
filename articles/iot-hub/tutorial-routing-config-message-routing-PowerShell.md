---
title: Azure PowerShell ile Azure IoT Hub için ileti yönlendirmeyi yapılandırma
description: Azure PowerShell'i kullanarak Azure IoT Hub için ileti yönlendirmeyi yapılandırın. İletideki özelliklere bağlı olarak, bir depolama hesabı veya Servis Veri Yolu kuyruğuna yönlendirin.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 68338c56419316e561bb072c1a0555e89d3de85b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74084432"
---
# <a name="tutorial-use-azure-powershell-to-configure-iot-hub-message-routing"></a>Öğretici: IoT Hub ileti yönlendirmesini yapılandırmak için Azure PowerShell'i kullanın

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="download-the-script-optional"></a>Komut dosyasını indirin (isteğe bağlı)

Bu öğreticinin ikinci bölümü için, IoT Hub'a ileti göndermek için bir Visual Studio uygulamasını indirip çalıştırAbilirsiniz. İndirmede Azure Kaynak Yöneticisi şablonu ve parametreler dosyasının yanı sıra Azure CLI ve PowerShell komut dosyalarını içeren bir klasör vardır. 

Tamamlanan komut dosyasını görüntülemek istiyorsanız, [Azure IoT C# Örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)indirin. Master.zip dosyasının zip'ini açın. Azure CLI komut dosyası /iot-hub/Tutorials/Routing/SimulatedDevice/resources/ as **iothub_routing_psh.ps1'dedir.**

## <a name="create-your-resources"></a>Kaynaklarınızı oluşturun

PowerShell ile kaynakları oluşturarak başlayın.

### <a name="use-powershell-to-create-your-base-resources"></a>Temel kaynaklarınızı oluşturmak için PowerShell'i kullanın

Aşağıdaki komut dosyasını Bulut Kabuğu'na kopyalayıp yapıştırın ve Enter tuşuna basın. Senaryoyu teker teksatır çalıştırAn bir senaryo. Komut dosyasının bu ilk bölümü, depolama hesabı, IoT Hub, Servis Veri Merkezi Ad Alanı ve Hizmet Veri Servisi kuyruğu da dahil olmak üzere bu öğretici için temel kaynakları oluşturur. Öğreticiye girerken, komut dosyasının her bloğunu kopyalayın ve çalıştırmak için Cloud Shell'e yapıştırın.

IoT Hub adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adı vardır. Bunu kolaylaştırmak için, bu kaynak adları *randomValue*adı verilen rasgele alfasayısal bir değerle eklenir. RandomValue komut dosyasının en üstünde bir kez oluşturulur ve komut dosyası boyunca gerektiğinde kaynak adlarına eklenir. Rasgele olmasını istemiyorsanız, boş bir dize veya belirli bir değere ayarlayabilirsiniz. 

> [!IMPORTANT]
> İlk komut dosyasında ayarlanan değişkenler yönlendirme komut dosyası tarafından da kullanılır, bu nedenle tüm komut dosyasını aynı Bulut Bulut suşu oturumunda çalıştırın. Yönlendirmeyi ayarlamak için komut dosyasını çalıştırmak için yeni bir oturum açarsanız, değişkenlerden birkaçı eksik değerler olur. 
>

```azurepowershell-interactive
# This command retrieves the subscription id of the current Azure account.
# This field is used when setting up the routing queries.
$subscriptionID = (Get-AzContext).Subscription.Id

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves the first 6 digits of a random value.
$randomValue = "$(Get-Random)".Substring(0,6)

# Set the values for the resource names that don't have to be globally unique.
$location = "West US"
$resourceGroup = "ContosoResources"
$iotHubConsumerGroup = "ContosoConsumers"
$containerName = "contosoresults"

# Create the resource group to be used 
#   for all resources for this tutorial.
New-AzResourceGroup -Name $resourceGroup -Location $location

# The IoT hub name must be globally unique, 
#   so add a random value to the end.
$iotHubName = "ContosoTestHub" + $randomValue
Write-Host "IoT hub name is " $iotHubName

# Create the IoT hub.
New-AzIotHub -ResourceGroupName $resourceGroup `
    -Name $iotHubName `
    -SkuName "S1" `
    -Location $location `
    -Units 1 

# Add a consumer group to the IoT hub for the 'events' endpoint.
Add-AzIotHubEventHubConsumerGroup -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EventHubConsumerGroupName $iotHubConsumerGroup `
  -EventHubEndpointName "events"

# The storage account name must be globally unique, so add a random value to the end.
$storageAccountName = "contosostorage" + $randomValue
Write-Host "storage account name is " $storageAccountName

# Create the storage account to be used as a routing destination.
# Save the context for the storage account 
#   to be used when creating a container.
$storageAccount = New-AzStorageAccount -ResourceGroupName $resourceGroup `
    -Name $storageAccountName `
    -Location $location `
    -SkuName Standard_LRS `
    -Kind Storage
# Retrieve the connection string from the context. 
$storageConnectionString = $storageAccount.Context.ConnectionString
Write-Host "storage connection string = " $storageConnectionString 

# Create the container in the storage account.
New-AzStorageContainer -Name $containerName `
    -Context $storageAccount.Context

# The Service Bus namespace must be globally unique,
#   so add a random value to the end.
$serviceBusNamespace = "ContosoSBNamespace" + $randomValue
Write-Host "Service Bus namespace is " $serviceBusNamespace

# Create the Service Bus namespace.
New-AzServiceBusNamespace -ResourceGroupName $resourceGroup `
    -Location $location `
    -Name $serviceBusNamespace 

# The Service Bus queue name must be globally unique,
#  so add a random value to the end.
$serviceBusQueueName  = "ContosoSBQueue" + $randomValue
Write-Host "Service Bus queue name is " $serviceBusQueueName 

# Create the Service Bus queue to be used as a routing destination.
New-AzServiceBusQueue -ResourceGroupName $resourceGroup `
    -Namespace $serviceBusNamespace `
    -Name $serviceBusQueueName  `
    -EnablePartitioning $False 
```

### <a name="create-a-simulated-device"></a>Sanal cihaz oluşturma

[!INCLUDE [iot-hub-include-create-simulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

Temel kaynaklar ayarlıolduğundan, ileti yönlendirmesini yapılandırabilirsiniz.

## <a name="set-up-message-routing"></a>İleti yönlendirmeyi ayarlama

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

Yönlendirme bitiş noktası oluşturmak için [Add-AzIotHubRoutingEndpoint'i](/powershell/module/az.iothub/Add-AzIotHubRoutingEndpoint)kullanın. Bitiş noktası için ileti rotasını oluşturmak için [Add-AzIotHubRoute'u](/powershell/module/az.iothub/Add-AzIoTHubRoute)kullanın.

### <a name="route-to-a-storage-account"></a>Depolama hesabına yönlendirme 

Önce depolama hesabının bitiş noktasını ayarlayın, ardından ileti rotasını oluşturun.

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

Bulut Bulut oturumunuzda ayarlanabilmesi gereken komut dosyası tarafından kullanılan değişkenler şunlardır:

**resourceGroup**: Bu alanın iki olayı vardır - her ikisini de kaynak grubunuza ayarlayın.

**ad**: Bu alan, yönlendirmenin uygulanacağı IoT Hub'ının adıdır.

**endpointName**: Bu alan bitiş noktasını tanımlayan addır. 

**endpointType**: Bu alan bitiş noktası türüdür. Bu `azurestoragecontainer`değer , , `eventhub` `servicebusqueue`, `servicebustopic`veya . olarak ayarlanmalıdır. Buradaki amaçlarınız için, `azurestoragecontainer`bunu .

**subscriptionID**: Bu alan Azure hesabınız için subscriptionID olarak ayarlanır.

**storageConnectionString**: Bu değer önceki komut dosyasında ayarlanan depolama hesabından alınır. Depolama hesabına erişmek için yönlendirme tarafından kullanılır.

**containerName**: Bu alan, verilerin yazılacağı depolama hesabındaki kapsayıcının adıdır.

**Kodlama**: Bu alanı `AVRO` ya `JSON`da . Bu, depolanan verilerin biçimini belirtir. Varsayılan avrodur.

**routeName**: Bu alan, ayarladığınız rotanın adıdır. 

**koşul**: Bu alan, bu bitiş noktasına gönderilen iletileri filtrelemek için kullanılan sorgudur. Depolamaya yönlendirilen iletilerin sorgu koşulu. `level="storage"`

**etkin**: Bu alan, ileti rotasıoluşturulduktan sonra etkinleştirilmesi gerektiğini belirten `true`, varsayılan.

Bu komut dosyasını kopyalayın ve Cloud Shell pencerenize yapıştırın.

```powershell
##### ROUTING FOR STORAGE #####

$endpointName = "ContosoStorageEndpoint"
$endpointType = "azurestoragecontainer"
$routeName = "ContosoStorageRoute"
$condition = 'level="storage"'
```

Bir sonraki adım, depolama hesabı için yönlendirme bitiş noktasını oluşturmaktır. Ayrıca, sonuçların depolanacağı kapsayıcıyı da belirtirsiniz. Kapsayıcı, depolama hesabı oluşturulduğunda oluşturuldu.

```powershell
# Create the routing endpoint for storage.
# Specify 'AVRO' or 'JSON' for the encoding of the data.
Add-AzIotHubRoutingEndpoint `
  -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EndpointName $endpointName `
  -EndpointType $endpointType `
  -EndpointResourceGroup $resourceGroup `
  -EndpointSubscriptionId $subscriptionId `
  -ConnectionString $storageConnectionString  `
  -ContainerName $containerName `
  -Encoding AVRO
```

Ardından, depolama bitiş noktası için ileti rotası oluşturun. İleti rotası, sorgu belirtimini karşılayan iletilerin nereye gönderilen yeri belirler.

```powershell
# Create the route for the storage endpoint.
Add-AzIotHubRoute `
   -ResourceGroupName $resourceGroup `
   -Name $iotHubName `
   -RouteName $routeName `
   -Source DeviceMessages `
   -EndpointName $endpointName `
   -Condition $condition `
   -Enabled 
```

### <a name="route-to-a-service-bus-queue"></a>Servis Veri Yolu kuyruğuna giden yol

Şimdi Service Bus kuyruğu için yönlendirmeyi ayarlayın. Hizmet Veri Servisi kuyruğunun bağlantı dizesini almak için, doğru hakları tanımlayan bir yetkilendirme kuralı oluşturmanız gerekir. Aşağıdaki komut dosyası, Hizmet Veri Servisi sırasına `sbauthrule`göre bir yetkilendirme `Listen Manage Send`kuralı oluşturur ve '' adlı bir yetkilendirme kuralı oluşturur. Bu yetkilendirme kuralı ayarlandıktan sonra, sıranın bağlantı dizesini almak için kullanabilirsiniz.

```powershell
##### ROUTING FOR SERVICE BUS QUEUE #####

# Create the authorization rule for the Service Bus queue.
New-AzServiceBusAuthorizationRule `
  -ResourceGroupName $resourceGroup `
  -NamespaceName $serviceBusNamespace `
  -Queue $serviceBusQueueName `
  -Name "sbauthrule" `
  -Rights @("Manage","Listen","Send")
```

Şimdi Hizmet Veri Servisi sıra anahtarını almak için yetkilendirme kuralını kullanın. Bu yetkilendirme kuralı, daha sonra komut dosyasında bağlantı dizesini almak için kullanılır.

```powershell
$sbqkey = Get-AzServiceBusKey `
    -ResourceGroupName $resourceGroup `
    -NamespaceName $serviceBusNamespace `
    -Queue $servicebusQueueName `
    -Name "sbauthrule"
```

Şimdi yönlendirme bitiş noktasını ve Servis Veri Yolu kuyruğu için ileti rotasını ayarlayın. Bulut Bulut oturumunuzda ayarlanabilmesi gereken komut dosyası tarafından kullanılan değişkenler şunlardır:

**endpointName**: Bu alan bitiş noktasını tanımlayan addır. 

**endpointType**: Bu alan bitiş noktası türüdür. Bu `azurestoragecontainer`değer , , `eventhub` `servicebusqueue`, `servicebustopic`veya . olarak ayarlanmalıdır. Buradaki amaçlarınız için, `servicebusqueue`bunu .

**routeName**: Bu alan, ayarladığınız rotanın adıdır. 

**koşul**: Bu alan, bu bitiş noktasına gönderilen iletileri filtrelemek için kullanılan sorgudur. Servis Veri Yolu sırasına yönlendirilen iletilerin sorgu `level="critical"`koşulu.

Hizmet Veri Servisi kuyruğu için ileti yönlendirmesi için Azure PowerShell burada.

```powershell
$endpointName = "ContosoSBQueueEndpoint"
$endpointType = "servicebusqueue"
$routeName = "ContosoSBQueueRoute"
$condition = 'level="critical"'

# Add the routing endpoint, using the connection string property from the key.
Add-AzIotHubRoutingEndpoint `
  -ResourceGroupName $resourceGroup `
  -Name $iotHubName `
  -EndpointName $endpointName `
  -EndpointType $endpointType `
  -EndpointResourceGroup $resourceGroup `
  -EndpointSubscriptionId $subscriptionId `
  -ConnectionString $sbqkey.PrimaryConnectionString

# Set up the message route for the Service Bus queue endpoint.
Add-AzIotHubRoute `
   -ResourceGroupName $resourceGroup `
   -Name $iotHubName `
   -RouteName $routeName `
   -Source DeviceMessages `
   -EndpointName $endpointName `
   -Condition $condition `
   -Enabled 
```

### <a name="view-message-routing-in-the-portal"></a>İleti yönlendirmesini portalda görüntüleme

[!INCLUDE [iot-hub-include-view-routing-in-portal](../../includes/iot-hub-include-view-routing-in-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kaynakların ayarlı olduğuna ve ileti rotalarının yapılandırıldığına göre, IoT hub'ına ileti göndermeyi öğrenmek ve farklı hedeflere yönlendirilmelerini görmek için bir sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Bölüm 2 - İleti yönlendirme sonuçlarını görüntüleyin](tutorial-routing-view-message-routing-results.md)