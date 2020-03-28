---
title: Azure CLI'yi kullanarak Azure IoT Hub için ileti yönlendirmeyi yapılandırma
description: Azure CLI ve Azure portalını kullanarak Azure IoT Hub için ileti yönlendirmesini yapılandırın
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: tutorial
ms.date: 03/12/2019
ms.author: robinsh
ms.custom: mvc
ms.openlocfilehash: 38a40d628b883c0e7ada824d47d3fdf3d29caf93
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "74084379"
---
# <a name="tutorial-use-the-azure-cli-and-azure-portal-to-configure-iot-hub-message-routing"></a>Öğretici: IoT Hub ileti yönlendirmesini yapılandırmak için Azure CLI ve Azure portalını kullanın

[!INCLUDE [iot-hub-include-routing-intro](../../includes/iot-hub-include-routing-intro.md)]

[!INCLUDE [iot-hub-include-routing-create-resources](../../includes/iot-hub-include-routing-create-resources.md)]

## <a name="use-the-azure-cli-to-create-the-base-resources"></a>Temel kaynakları oluşturmak için Azure CLI'yi kullanın

Bu öğretici, temel kaynakları oluşturmak için Azure CLI'yi kullanır, ardından ileti yönlendirmeyi nasıl yapılandıracağımı göstermek ve sanal aygıtı sınamak için ayarlamak için [Azure portalını](https://portal.azure.com) kullanır.

Aşağıdaki komut dosyasını Bulut Kabuğu'na kopyalayıp yapıştırın ve Enter tuşuna basın. Senaryoyu teker teksatır çalıştırAn bir senaryo. Bu, depolama hesabı, IoT Hub,Service Bus Namespace ve Service Bus sırası da dahil olmak üzere bu öğretici için temel kaynakları oluşturur.

IoT Hub adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adı vardır. Bunu kolaylaştırmak için, bu kaynak adları *randomValue*adı verilen rasgele alfasayısal bir değerle eklenir. RandomValue komut dosyasının en üstünde bir kez oluşturulur ve komut dosyası boyunca gerektiğinde kaynak adlarına eklenir. Rasgele olmasını istemiyorsanız, boş bir dize veya belirli bir değere ayarlayabilirsiniz.

> [!TIP]
> Hata ayıklama hakkında bir ipucu: Bu komut dosyası `\`komut dosyası daha okunabilir hale getirmek için devamı sembolü (backslash) kullanır. Komut dosyasını çalıştırmakta sorun varsa, Bulut Bulut `bash` oturumunuzun çalıştığından ve herhangi bir ters eğik çizgiden sonra boşluk olmadığından emin olun.
>

```azurecli-interactive
# This retrieves the subscription id of the account 
#   in which you're logged in.
# This field is used to set up the routing queries.
subscriptionID=$(az account show --query id)

# Concatenate this number onto the resources that have to be globally unique.
# You can set this to "" or to a specific value if you don't want it to be random.
# This retrieves a random value.
randomValue=$RANDOM

# Set the values for the resource names that 
#   don't have to be globally unique.
location=westus
resourceGroup=ContosoResources
iotHubConsumerGroup=ContosoConsumers
containerName=contosoresults

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

```

Temel kaynaklar ayarlıştınartık, ileti yönlendirmeyi [Azure portalında](https://portal.azure.com)yapılandırabilirsiniz.

## <a name="set-up-message-routing"></a>İleti yönlendirmeyi ayarlama

[!INCLUDE [iot-hub-include-create-routing-description](../../includes/iot-hub-include-create-routing-description.md)]

### <a name="route-to-a-storage-account"></a>Depolama hesabına yönlendirme

Şimdi depolama hesabı için yönlendirmeyi ayarlayın. İleti Yönlendirme bölmesine gider ve bir yol eklersiniz. Yolu eklerken, yol için yeni bir uç nokta tanımlayın. Bu yönlendirme ayarlandıktan sonra, **düzey** özelliğinin **depolamaya** ayarlandığı iletiler otomatik olarak bir depolama hesabına yazılır. 

[!INCLUDE [iot-hub-include-blob-storage-format](../../includes/iot-hub-include-blob-storage-format.md)]

1. Azure [portalında](https://portal.azure.com) **Kaynak Grupları'nı**seçin ve ardından kaynak grubunuzu seçin. Bu öğreticide **ContosoResources** kullanılır.

2. Kaynak listesinin altındaki IoT merkezini seçin. Bu öğreticide **ContosoTestHub** kullanılır.

3. **İleti Yönlendirme'yi**seçin. İleti **Yönlendirme** bölmesinde +**Ekle'yi**seçin. Rota **Ekle** bölmesinde, desteklenen uç noktaları aşağıdaki resimde gösterildiği gibi göstermek için Bitiş Noktası alanının yanına +**Ekle'yi** seçin:

   ![Rota için bitiş noktası eklemeye başlayın](./media/tutorial-routing/message-routing-add-a-route-w-storage-ep.png)

4. **Blob depolama**'yı seçin. **Depolama bitiş noktası** bölmesini ekleyin bakın.

   ![Bitiş noktası ekleme](./media/tutorial-routing/message-routing-add-storage-ep.png)

5. Uç nokta için bir ad girin. Bu öğretici **ContosoStorageEndpoint**kullanır.

6. **Kapsayıcı seç'i**seçin. Bu sizi depolama hesaplarınızın listesine götürür. Hazırlık adımlarında ayarladığınız hesabı seçin. Bu öğreticide **contosostorage** kullanılır. Söz konusu depolama hesabının içindeki kapsayıcıların listesi gösterilir. Hazırlama adımlarında ayarladığınız kapsayıcıyı **seçin.** Bu öğreticide **contosoresults** kullanılır. Depolama bitiş **noktası** bölmesine geri döner ve yaptığınız seçimleri görürsünüz.

7. Kodlamayı AVRO veya JSON olarak ayarlayın. Bu öğretici için kalan alanlarda varsayılan değerleri kullanın. Seçilen bölge JSON kodlamasını desteklemiyorsa, bu alan soluk olacaktır.

   > [!NOTE]
   > Blob adının biçimini **Blob dosya adı biçimi** ile ayarlayabilirsiniz. Varsayılan değer: `{iothub}/{partition}/{YYYY}/{MM}/{DD}/{HH}/{mm}`. Biçim {iothub}, {partition}, {YYYY}, {MM}, {DD}, {HH} ve {mm} öğelerini içermelidir, sıralama farklı olabilir.
   >
   > Örneğin varsayılan blob dosya adı biçimini kullandığınızda hub adı ContosoTestHub ve tarih/saat 30 Ekim 2018 10:56 ise blob adı şu şekilde olacaktır: `ContosoTestHub/0/2018/10/30/10/56`.
   > 
   > Bloblar Avro biçiminde yazılır.
   >

8. Depolama bitiş noktasını oluşturmak ve rotaya eklemek için **Oluştur'u** seçin. **Yol ekle** bölmesine dönersiniz.

9. Şimdi yönlendirme sorgusu bilgilerinin kalan kısmını tamamlayın. Bu sorgu, az önce uç nokta olarak eklediğiniz depolama kapsayıcısına ileti gönderme ölçütlerini belirtir. Ekrandaki alanları doldurun.

   **Ad**: Yönlendirme sorgunuz için bir ad girin. Bu öğretici **ContosoStorageRoute**kullanır.

   **Uç nokta**: Az önce ayarladığınız uç noktayı gösterir.

   **Veri kaynağı**: Açılan listeden **Cihaz Telemetri İletileri**'ni seçin.

   **Rotayı Etkinleştir**: Bu alanın `enabled`.
   
   **Yönlendirme sorgusu**: Sorgu dizesi olarak `level="storage"` girin.

   ![Depolama hesabı için yönlendirme sorgusu oluşturma](./media/tutorial-routing/message-routing-finish-route-storage-ep.png)  

   **Kaydet'i**seçin. İşlem bittiğinde İleti Yönlendirme bölmesine döner ve burada depolama için yeni yönlendirme sorgunuzu görebilirsiniz. Yollar bölmesini kapatın; Kaynak grubu sayfasına dönersiniz.

### <a name="route-to-a-service-bus-queue"></a>Servis Veri Yolu kuyruğuna giden yol

Şimdi Service Bus kuyruğu için yönlendirmeyi ayarlayın. İleti Yönlendirme bölmesine gider ve bir yol eklersiniz. Yolu eklerken, yol için yeni bir uç nokta tanımlayın. Bu yol ayarlandıktan sonra, **düzey** özelliğinin **kritik** olarak ayarlandığı iletiler, Bir Mantık Uygulaması tetikleyen ve ardından bilgileri içeren bir e-posta gönderen Hizmet Veri Yolu kuyruğuna yazılır.

1. Kaynak grubu sayfasında, IoT hub'ınızı seçin ve ardından **İleti Yönlendirme'yi**seçin.

2. İleti **Yönlendirme** bölmesinde +**Ekle'yi**seçin.

3. Rota **Ekle** bölmesine, Bitiş Noktası alanının yanına +**Ekle'yi** seçin. **Service Bus Kuyruğu**'nu seçin. **Service Bus Uç Noktası Ekle** bölmesini görürsünüz.

   ![Servis veri günü bitiş noktası ekleme](./media/tutorial-routing/message-routing-add-sbqueue-ep.png)

4. Şu alanları doldurun:

   **Uç Nokta Adı**: Uç nokta için bir ad girin. Bu öğretici **ContosoSBQueueEndpoint**kullanır.
   
   **Servis Veri Yeri Ad Alanı**: Hazırlık adımlarında ayarladığınız servis veri aracı ad alanını seçmek için açılır listeyi kullanın. Bu öğreticide **ContosoSBNamespace** kullanılır.

   **Servis Veri Servisi sırası**: Servis Veri Servisi sırasını seçmek için açılır listeyi kullanın. Bu öğreticide **contososbqueue** kullanılır.

5. Hizmet Veri Servisi sıra bitiş noktasını eklemek için **Oluştur'u** seçin. **Yol ekle** bölmesine dönersiniz.

6. Şimdi yönlendirme sorgusu bilgilerinin kalan kısmını tamamlarsınız. Bu sorgu, az önce uç nokta olarak eklediğiniz Service Bus kuyruğuna ileti gönderme ölçütlerini belirtir. Ekrandaki alanları doldurun. 

   **Ad**: Yönlendirme sorgunuz için bir ad girin. Bu öğretici **ContosoSBQueueRoute**kullanır. 

   **Uç nokta**: Az önce ayarladığınız uç noktayı gösterir.

   **Veri kaynağı**: Açılan listeden **Cihaz Telemetri İletileri**'ni seçin.

   **Yönlendirme sorgusu**: Sorgu dizesi olarak `level="critical"` girin. 

   ![Servis Veri Servisi sırası için yönlendirme sorgusu oluşturma](./media/tutorial-routing/message-routing-finish-route-sbq-ep.png)

7. **Kaydet'i**seçin. Yollar bölmesine dönüldüğünde, burada gösterildiği gibi yeni yönlendirme yollarınızın ikisini de görürsünüz.

   ![Az önce ayarladığınız rotalar](./media/tutorial-routing/message-routing-show-both-routes.png)

8. **Özel Uç Noktalar** sekmesini seçerek ayarladığınız özel uç noktaları görebilirsiniz.

   ![Yeni kurduğunuz özel bitiş noktası](./media/tutorial-routing/message-routing-show-custom-endpoints.png)

9. İleti yönlendirme bölmesini kapatın; Kaynak grubu bölmesine dönersiniz.

## <a name="create-a-simulated-device"></a>Sanal cihaz oluşturma

[!INCLUDE [iot-hub-include-create-simulated-device-portal](../../includes/iot-hub-include-create-simulated-device-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Artık kaynakların ayarlı olduğuna ve ileti rotalarının yapılandırıldığına göre, IoT hub'ına ileti göndermeyi öğrenmek ve farklı hedeflere yönlendirilmelerini görmek için bir sonraki öğreticiye ilerleyin. 

> [!div class="nextstepaction"]
> [Bölüm 2 - İleti yönlendirme sonuçlarını görüntüleyin](tutorial-routing-view-message-routing-results.md)
