---
title: Öğretici - Azure IoT Hub ileti zenginleştirmelerini kullanma
description: Azure IoT Hub iletileri için ileti zenginleştirmelerinin nasıl kullanılacağını gösteren öğretici
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 12/20/2019
ms.author: robinsh
ms.openlocfilehash: c812e00699cd8f8cfbaf32feea1b43866ffb0990
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78674366"
---
# <a name="tutorial-use-azure-iot-hub-message-enrichments"></a>Öğretici: Azure IoT Hub ileti zenginleştirmelerini kullanma

*İleti zenginleştirmeleri,* Azure IoT Hub'ının iletiler belirlenen bitiş noktasına gönderilmeden önce iletileri ek bilgilerle *damgalayabilme* yeteneğini açıklar. İleti zenginleştirmelerini kullanmanın bir nedeni, akış aşağı işlemini kolaylaştırmak için kullanılabilecek verileri eklemektir. Örneğin, aygıt telemetri iletilerini aygıt ikiz etiketiyle zenginleştirmek, bu bilgiler için aygıt ikiz API çağrıları yapmak için müşterilerüzerindeki yükü azaltabilir. Daha fazla bilgi için [ileti zenginleştirmelerine genel bakış](iot-hub-message-enrichments-overview.md)bölümüne bakın.

Bu öğreticide, bir IoT hub'ı için ileti zenginleştirmelerini sınamak için gereken kaynakları oluşturmanın ve yapılandırmanın iki yolunu görürsünüz. Kaynaklar, iki depolama kapsayıcısı olan bir depolama hesabı içerir. Bir kapsayıcı zenginleştirilmiş iletileri, diğer kapsayıcı ise özgün iletileri tutar. Ayrıca, iletileri almak ve zenginleştirilmiş olup olmadıklarına bağlı olarak uygun depolama kabına yönlendirmek için bir IoT hub'ı da dahildir.

* İlk yöntem, kaynakları oluşturmak ve ileti yönlendirmesini yapılandırmak için Azure CLI'yi kullanmaktır. Daha sonra [Azure portalını](https://portal.azure.com)kullanarak zenginleştirmeleri el ile tanımlarsınız.

* İkinci yöntem, ileti yönlendirme ve ileti zenginleştirmeleri için hem kaynakları *hem* de yapılandırmaları oluşturmak için bir Azure Kaynak Yöneticisi şablonu kullanmaktır.

İleti yönlendirme ve ileti zenginleştirmeleri için yapılandırmalar tamamlandıktan sonra, IoT hub'ına ileti göndermek için bir uygulama kullanırsınız. Hub daha sonra bunları her iki depolama kapsayıcısına da yönlendirir. Yalnızca **zenginleştirilmiş** depolama kapsayıcısı için bitiş noktasına gönderilen iletiler zenginleştirilmiştir.

Bu öğreticiyi tamamlamak için gerçekleştirdiğiniz görevler şunlardır:

**IoT Hub ileti zenginleştirmelerini kullanma**
> [!div class="checklist"]
> * İlk yöntem: Azure CLI'yi kullanarak kaynakları oluşturun ve ileti yönlendirmesini yapılandırın. [Azure portalını](https://portal.azure.com)kullanarak ileti zenginleştirmelerini el ile yapılandırın.
> * İkinci yöntem: Kaynak Yöneticisi şablonu kullanarak kaynakları oluşturun ve ileti yönlendirme ve ileti zenginleştirmelerini yapılandırın. 
> * Hub'a ileti gönderen bir IoT aygıtını simüle eden bir uygulama çalıştırın.
> * Sonuçları görüntüleyin ve ileti zenginleştirmelerinin beklendiği gibi çalıştığını doğrulayın.

## <a name="prerequisites"></a>Ön koşullar

* Bir Azure aboneliğiniz olmalıdır. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.
* [Visual Studio'u](https://www.visualstudio.com/)yükleyin.

* 8883 bağlantı noktasının güvenlik duvarınızda açık olduğundan emin olun. Bu öğreticideki aygıt örneği, bağlantı noktası 8883 üzerinden iletişim sağlayan MQTT protokolünü kullanır. Bu bağlantı noktası, bazı kurumsal ve eğitim ağı ortamlarında engellenebilir. Daha fazla bilgi ve bu sorunu çözmenin yolları için [IoT Hub'ına Bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub)konusuna bakın.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="retrieve-the-iot-c-samples-repository"></a>IoT C# örnek deposunu alın

[GitHub'dan IoT C# örneklerini](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip) indirin ve zip'ini açın. Bu depoda çeşitli uygulamalar, komut dosyaları ve Kaynak Yöneticisi şablonları vardır. Bu öğretici için kullanılacak olanlar aşağıdaki gibidir:

* El ile yöntem için, kaynakları oluşturmak için kullanılan bir CLI komut dosyası vardır. Bu komut dosyası /azure-iot-samples-csharp/iot-hub/Tutorials/Routing/SimulatedDevice/resources/iothub_msgenrichment_cli.azcli içindedir. Bu komut dosyası kaynakları oluşturur ve ileti yönlendirmesini yapılandırır. Bu komut dosyasını çalıştırdıktan sonra, [Azure portalını](https://portal.azure.com)kullanarak ileti zenginleştirmelerini el ile oluşturun.
* Otomatik yöntem için bir Azure Kaynak Yöneticisi şablonu vardır. Şablon /azure-iot-samples-csharp/iot-hub/Tutorials/Routing/SimulatedDevice/resources/template_msgenrichments.json'dadır. Bu şablon kaynakları oluşturur, ileti yönlendirmesini yapılandırır ve ileti zenginleştirmelerini yapılandırır.
* Kullandığınız üçüncü uygulama, IoT hub'ına ileti göndermek ve ileti zenginleştirmelerini test etmek için kullandığınız Aygıt Simülasyonu uygulamasıdır.

## <a name="manually-set-up-and-configure-by-using-the-azure-cli"></a>Azure CLI'yi kullanarak el ile ayarlayın ve yapılandırın

Azure CLI komut dosyası, gerekli kaynakları oluşturmaya ek olarak, iki yolu ayrı depolama kapsayıcıları olan uç noktalara da yapılandırır. İleti yönlendirmesinin nasıl yapılandırılabildiğini hakkında daha fazla bilgi için [Yönlendirme öğreticisine](tutorial-routing.md)bakın. Kaynaklar ayarlandıktan sonra, her bitiş noktası için ileti zenginleştirmelerini yapılandırmak için [Azure portalını](https://portal.azure.com) kullanın. Daha sonra test adımına devam edin.

> [!NOTE]
> Tüm iletiler her iki uç noktaya da yönlendirilir, ancak yalnızca yapılandırılmış ileti zenginleştirmeleriyle bitiş noktasına giden iletiler zenginleştirilmiştir.
>

Aşağıdaki komut dosyasını kullanabilir veya komut dosyasını indirilen deponun /resources klasöründe açabilirsiniz. Betik aşağıdaki adımları gerçekleştirir:

* Bir IoT hub'ı oluşturun.
* Depolama hesabı oluşturma.
* Depolama hesabında iki kapsayıcı oluşturun. Bir kapsayıcı zenginleştirilmiş iletiler için, diğer kapsayıcı ise zenginleştirilmemiş iletiler içindir.
* İki farklı depolama hesabı için yönlendirme ayarlama:
    * Her depolama hesabı kapsayıcısı için bir bitiş noktası oluşturun.
    * Depolama hesabı kapsayıcı uç noktalarının her birine bir rota oluşturun.

IoT hub adı ve depolama hesabı adı gibi genel olarak benzersiz olması gereken birkaç kaynak adı vardır. Komut dosyasının çalıştırmasını kolaylaştırmak için, bu kaynak adları *randomValue*adı verilen rasgele alfasayısal bir değerle eklenir. Rasgele değer komut dosyasının en üstünde bir kez oluşturulur. Komut dosyası boyunca gerektiğinde kaynak adlarına eklenir. Değerin rasgele olmasını istemiyorsanız, onu boş bir dize veya belirli bir değere ayarlayabilirsiniz.

Henüz yapmadıysanız, bir Azure Bulut [BulutU penceresi](https://shell.azure.com) açın ve Bash olarak ayarlandığından emin olun. Sıkıştırılmamış depoda komut dosyasını açın, hepsini seçmek için Ctrl+A'yı seçin ve ardından kopyalamak için Ctrl+C'yi seçin. Alternatif olarak, aşağıdaki CLI komut dosyasını kopyalayabilir veya doğrudan Cloud Shell'de açabilirsiniz. Komut satırına sağ tıklayıp **Yapıştır'ı**seçerek komut dosyasını Bulut Kabuğu penceresine yapıştırın. Komut dosyası her seferinde bir deyimi çalıştırın. Komut dosyası nın çalışmasını durdurduktan sonra, son komutu çalıştırdığından emin olmak için **Enter'u** seçin. Aşağıdaki kod bloğu, ne yaptığını açıklayan yorumlarla birlikte kullanılan komut dosyasını gösterir.

Burada komut dosyası tarafından oluşturulan kaynaklar dır. *Zenginleştirilmiş,* kaynağın zenginleştirmeiçeren iletiler için olduğu anlamına gelir. *Özgün,* kaynağın zenginleştirilmemiş iletiler için olduğu anlamına gelir.

| Adı | Değer |
|-----|-----|
| resourceGroup | ContosoResourcesMsgEn |
| konteyner adı | Özgün  |
| konteyner adı | Zengin -leştirilmiş  |
| IoT cihaz adı | Contoso-Test Cihazı |
| IoT Hub adı | ContosoTestHubMsgEn |
| depolama Hesap Adı | kontosostorage |
| uç nokta Adı 1 | ContosoStorageEndpointOriginal |
| uç nokta Adı 2 | ContosoStorageEndpointZenginleştirilmiş|
| rota Adı 1 | ContosoStorageRouteOrijinal |
| rota Adı 2 | ContosoStorageRouteZenginleştirilmiş |

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
location=westus2
resourceGroup=ContosoResourcesMsgEn
containerName1=original
containerName2=enriched
iotDeviceName=Contoso-Test-Device

# Create the resource group to be used
#   for all the resources for this tutorial.
az group create --name $resourceGroup \
    --location $location

# The IoT hub name must be globally unique,
#   so add a random value to the end.
iotHubName=ContosoTestHubMsgEn$randomValue
echo "IoT hub name = " $iotHubName

# Create the IoT hub.
az iot hub create --name $iotHubName \
    --resource-group $resourceGroup \
    --sku S1 --location $location

# You need a storage account that will have two containers
#   -- one for the original messages and
#   one for the enriched messages.
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
#    You need this to create the containers.
storageAccountKey=$(az storage account keys list \
    --resource-group $resourceGroup \
    --account-name $storageAccountName \
    --query "[0].value" | tr -d '"')

# See the value of the storage account key.
echo "storage account key = " $storageAccountKey

# Create the containers in the storage account.
az storage container create --name $containerName1 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

az storage container create --name $containerName2 \
    --account-name $storageAccountName \
    --account-key $storageAccountKey \
    --public-access off

# Create the IoT device identity to be used for testing.
az iot hub device-identity create --device-id $iotDeviceName \
    --hub-name $iotHubName

# Retrieve the information about the device identity, then copy the primary key to
#   Notepad. You need this to run the device simulation during the testing phase.
# If you are using Cloud Shell, you can scroll the window back up to retrieve this value.
az iot hub device-identity show --device-id $iotDeviceName \
    --hub-name $iotHubName

##### ROUTING FOR STORAGE #####

# You're going to have two routes and two endpoints.
# One route points to the first container ("original") in the storage account
#   and includes the original messages.
# The other points to the second container ("enriched") in the same storage account
#   and includes the enriched versions of the messages.

endpointType="azurestoragecontainer"
endpointName1="ContosoStorageEndpointOriginal"
endpointName2="ContosoStorageEndpointEnriched"
routeName1="ContosoStorageRouteOriginal"
routeName2="ContosoStorageRouteEnriched"

# for both endpoints, retrieve the messages going to storage
condition='level="storage"'

# Get the connection string for the storage account.
# Adding the "-o tsv" makes it be returned without the default double quotes around it.
storageConnectionString=$(az storage account show-connection-string \
  --name $storageAccountName --query connectionString -o tsv)

# Create the routing endpoints and routes.
# Set the encoding format to either avro or json.

# This is the endpoint for the first container, for endpoint messages that are not enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName1 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName1 \
  --resource-group $resourceGroup \
  --encoding json

# This is the endpoint for the second container, for endpoint messages that are enriched.
az iot hub routing-endpoint create \
  --connection-string $storageConnectionString \
  --endpoint-name $endpointName2 \
  --endpoint-resource-group $resourceGroup \
  --endpoint-subscription-id $subscriptionID \
  --endpoint-type $endpointType \
  --hub-name $iotHubName \
  --container $containerName2 \
  --resource-group $resourceGroup \
  --encoding json

# This is the route for messages that are not enriched.
# Create the route for the first storage endpoint.
az iot hub route create \
  --name $routeName1 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName1 \
  --enabled \
  --condition $condition

# This is the route for messages that are enriched.
# Create the route for the second storage endpoint.
az iot hub route create \
  --name $routeName2 \
  --hub-name $iotHubName \
  --source devicemessages \
  --resource-group $resourceGroup \
  --endpoint-name $endpointName2 \
  --enabled \
  --condition $condition
```

Bu noktada, kaynakların tümü ayarlanır ve ileti yönlendirmesi yapılandırılır. Portaldaki ileti yönlendirme yapılandırmasını görüntüleyebilir ve zenginleştirilmiş depolama kapsayıcısına giden iletiler için ileti zenginleştirmelerini **ayarlayabilirsiniz.**

### <a name="manually-configure-the-message-enrichments-by-using-the-azure-portal"></a>Azure portalını kullanarak ileti zenginleştirmelerini el ile yapılandırın

1. **Kaynak gruplarını**seçerek IoT hub'ınıza gidin. Daha sonra bu öğretici için ayarlanan kaynak grubunu seçin **(ContosoResourcesMsgEn**). Listedeki IoT merkezini bulun ve seçin. IoT hub'ı için **İleti yönlendirmesini** seçin.

   ![İleti yönlendirmeyi seçin](./media/tutorial-message-enrichments/select-iot-hub.png)

   İleti yönlendirme bölmesinin **Rotalar,** Özel uç **noktaları**ve **zenginleştir iletileri**etiketli üç sekmesi vardır. Komut dosyası tarafından ayarlanan yapılandırmayı görmek için ilk iki sekmeye göz atın. İleti zenginleştirmeleri eklemek için üçüncü sekmeyi kullanın. **Zenginleştirilmiş**olarak adlandırılan depolama kapsayıcısı için bitiş noktasına giden iletileri zenginleştirelim. Adı ve değeri doldurun ve ardından açılan listeden Zenginleştirilmiş uç **nokta ContosoStorageEndpoint'i** seçin. İletiye IoT hub adını ekleyen bir zenginleştirme nasıl ayarlanabildiğini gösteren bir örnek aşağıda verilmiştir:

   ![İlk zenginleştirme ekle](./media/tutorial-message-enrichments/add-message-enrichments.png)

2. ContosoStorageEndpointEnriched bitiş noktası için listeye bu değerleri ekleyin.

   | Anahtar | Değer | Bitiş noktası (açılır liste) |
   | ---- | ----- | -------------------------|
   | myIotHub | $iothubname | AzureStorageContainers > ContosoStorageEndpointEnriched |
   | Cihaz Konumu | $twin.tags.location | AzureStorageContainers > ContosoStorageEndpointEnriched |
   |Customerıd | 6ce345b8-1e4a-411e-9398-d34587459a3a | AzureStorageContainers > ContosoStorageEndpointEnriched |

   > [!NOTE]
   > Aygıtınızın ikizi yoksa, buraya koyduğunuz değer, ileti zenginleştirmelerinde değer için bir dize olarak damgalanır. Aygıt ikiz bilgilerini görmek için portaldaki hub'ınıza gidin ve **IoT aygıtlarını**seçin. Cihazınızı seçin ve sayfanın üst kısmında **Aygıt ikizini** seçin.
   >
   > Konum gibi etiketler eklemek için ikiz bilgileri ve belirli bir değere ayarlayabilirsiniz. Daha fazla bilgi için [Bkz. IoT Hub'da aygıt ikizlerini anlayın ve kullanın.](iot-hub-devguide-device-twins.md)

3. Bitirdikten sonra bölmeniz bu görüntüye benzer olmalıdır:

   ![Tüm zenginleştirmelerin ekolduğu tablo](./media/tutorial-message-enrichments/all-message-enrichments.png)

4. Değişiklikleri kaydetmek için **Uygula'yı** seçin. [Test iletisi zenginleştirmeleri](#test-message-enrichments) bölümüne atlayın.

## <a name="create-and-configure-by-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu kullanarak oluşturma ve yapılandırma
Kaynakları, ileti yönlendirmeyi ve ileti zenginleştirmelerini oluşturmak ve yapılandırmak için Kaynak Yöneticisi şablonu kullanabilirsiniz.

1. Azure Portal’da oturum açın. Arama kutusunu açmak için **Kaynak Oluştur +** Seçin. *Şablon dağıtımını*girin ve arayın. Sonuç bölmesinde Şablon **dağıtımı 'nı (özel şablon kullanarak dağıt)** seçin.

   ![Azure portalında şablon dağıtımı](./media/tutorial-message-enrichments/template-select-deployment.png)

1. **Şablon dağıtım** bölmesinde **Oluştur'u** seçin.

1. Özel **dağıtım** bölmesinde, **düzenleyicide kendi şablonunuzu oluştur'u**seçin.

1. **Şablonu Edit** bölmesinde **Dosyayı Yükle'yi**seçin. Windows Gezgini görüntülenir. **template_messageenrichments.json** dosyasını **/iot-hub/Tutorials/Routing/SimulatedDevice/resources**adresindeki pımalanamayan repo dosyasında bulun. 

   ![Yerel makineden şablon seçme](./media/tutorial-message-enrichments/template-select.png)

1. Şablon dosyasını yerel makineden yüklemek için **Aç'ı** seçin. Yüklenir ve edit bölmesinde görünür.

   Bu şablon, varsayılan adların sonuna rasgele bir değer ekleyerek genel olarak benzersiz bir IoT hub adı ve depolama hesabı adı kullanacak şekilde ayarlanmıştır, böylece şablonu herhangi bir değişiklik yapmadan kullanabilirsiniz.

   Şablonu yükleyerek oluşturulan kaynaklar aşağıda verilmiştir. **Zenginleştirilmiş,** kaynağın zenginleştirmeiçeren iletiler için olduğu anlamına gelir. **Özgün,** kaynağın zenginleştirilmemiş iletiler için olduğu anlamına gelir. Bunlar Azure CLI komut dosyasında kullanılan değerlerle aynıdır.

   | Adı | Değer |
   |-----|-----|
   | resourceGroup | ContosoResourcesMsgEn |
   | konteyner adı | Özgün  |
   | konteyner adı | Zengin -leştirilmiş  |
   | IoT cihaz adı | Contoso-Test Cihazı |
   | IoT Hub adı | ContosoTestHubMsgEn |
   | depolama Hesap Adı | kontosostorage |
   | uç nokta Adı 1 | ContosoStorageEndpointOriginal |
   | uç nokta Adı 2 | ContosoStorageEndpointZenginleştirilmiş|
   | rota Adı 1 | ContosoStorageRouteOrijinal |
   | rota Adı 2 | ContosoStorageRouteZenginleştirilmiş |

1. **Kaydet'i**seçin. **Özel dağıtım** bölmesi görüntülenir ve şablon tarafından kullanılan tüm parametreleri gösterir. Ayarlamanız gereken tek alan **Kaynak grubudur.** Yeni bir tane oluşturun veya açılan listeden birini seçin.

   Özel **dağıtım** bölmesinin üst yarısı buradadır. Kaynak grubunu nerede doldurduğunuzu görebilirsiniz.

   ![Özel dağıtım bölmesinin üst yarısı](./media/tutorial-message-enrichments/template-deployment-top.png)

1. Özel dağıtım bölmesinin alt yarısı aşağıda veda **edin.** Parametrelerin geri kalanını ve şart ve koşulları görebilirsiniz. 

   ![Özel dağıtım bölmesinin alt yarısı](./media/tutorial-message-enrichments/template-deployment-bottom.png)

1. Hüküm ve koşulları kabul etmek için onay kutusunu seçin. Ardından şablon dağıtımına devam etmek için **Satın Al'ı** seçin.

1. Şablonun tam olarak dağıtılmasını bekleyin. İlerlemeyi kontrol etmek için ekranın üst kısmındaki çan simgesini seçin. Bittiğinde, [Test iletisi zenginleştirmeler](#test-message-enrichments) bölümüne devam edin.

## <a name="test-message-enrichments"></a>İleti zenginleştirmelerini test edin

İleti zenginleştirmelerini görüntülemek için **Kaynak gruplarını**seçin. Ardından, bu öğretici için kullandığınız kaynak grubunu seçin. Kaynaklar listesinden IoT merkezini seçin ve **Mesajlaşma'ya**gidin. İleti yönlendirme yapılandırması ve yapılandırılan zenginleştirmeler görüntülenir.

İleti zenginleştirmeleri bitiş noktası için yapılandırıldığına göre, IoT hub'ına ileti göndermek için Benzetilen Aygıt uygulamasını çalıştırın. Hub, aşağıdaki görevleri gerçekleştiren ayarlarla ayarlandı:

* Depolama bitiş noktası ContosoStorageEndpointOriginal yönlendirilen iletiler zenginleştirilmez ve depolama kapsayıcısında `original`depolanır.

* Depolama bitiş noktası ContosoStorageEndpointZenginleştirilmiş yönlendirilen iletiler zenginleştirilmiş ve depolama `enriched`kapsayıcısında saklanır.

Simüle Aygıt uygulaması, fermuarsız indirmedeki uygulamalardan biridir. Uygulama, Azure Depolama'yı içeren [Yönlendirme öğreticisinde](tutorial-routing.md)farklı ileti yönlendirme yöntemlerinin her biri için ileti gönderir.

Visual Studio'da kodu açmak için çözüm dosyası **IoT_SimulatedDevice.sln'ye** çift tıklayın ve **ardından Program.cs**açın. İşaretçi `{your hub name}`yerine IoT hub adını değiştirin. IoT hub ana bilgisayar adının biçimi **{hub adı}.azure-devices.net.** Bu öğretici için hub ana bilgisayar adı ContosoTestHubMsgEn.azure-devices.net. Ardından, komut dosyasını çalıştırdığınızda kaydettiğiniz aygıt anahtarını değiştirin ve işaretçi `{your device key}`için kaynakları oluşturun.

Aygıt anahtarınız yoksa, portaldan alabilirsiniz. Oturum açmadan sonra **Kaynak gruplarına**gidin, kaynak grubunuzu seçin ve ardından IoT hub'ınızı seçin. Test cihazınız için **IoT Cihazları'nın** altına bakın ve cihazınızı seçin. Panoya kopyalamak için **Birincil anahtarın** yanındaki kopya simgesini seçin.

   ```csharp
        private readonly static string s_myDeviceId = "Contoso-Test-Device";
        private readonly static string s_iotHubUri = "ContosoTestHubMsgEn.azure-devices.net";
        // This is the primary key for the device. This is in the portal.
        // Find your IoT hub in the portal > IoT devices > select your device > copy the key.
        private readonly static string s_deviceKey = "{your device key}";
   ```

### <a name="run-and-test"></a>Çalıştırma ve test etme

Konsol uygulamasını birkaç dakika çalıştırın. Gönderilen iletiler uygulamanın konsol ekranında görüntülenir.

Uygulama, IoT hub'ına her saniye yeni bir cihazdan buluta iletisi gönderir. İleti, cihaz kimliği, sıcaklık, nem düzeyi ve ileti düzeyi (varsayılan `normal` değeriyle) bilgileriyle bir JSON seri hale getirilmiş nesnesi içerir. İletinin `critical` depolama hesabına veya `storage`varsayılan bitiş noktasına yönlendirilmesine neden olan bir düzey rasgele atar. Depolama **hesabındaki zenginleştirilmiş** kapsayıcıya gönderilen iletiler zenginleştirilmiş tir.

Birkaç depolama iletisi gönderildikten sonra verileri görüntüleyin.

1. **Kaynak grupları**’nı seçin. Kaynak grubunuz **ContosoResourcesMsgEn'i**bulun ve seçin.

2. **Contosostorage**olan depolama hesabınızı seçin. Ardından sol bölmede **Depolama Gezgini'ni (önizleme)** seçin.

   ![Depolama Gezgini'ni seçin](./media/tutorial-message-enrichments/select-storage-explorer.png)

   Kullanılabilecek iki kapsayıcıyı görmek için **BLOB CONTAINERS'ı** seçin.

   ![Depolama hesabındaki kapsayıcılara bakın](./media/tutorial-message-enrichments/show-blob-containers.png)

**Zenginleştirilmiş** olarak adlandırılan kapsayıcıdaki iletiler iletilerde yer alan ileti zenginleştirmelerine sahiptir. **Orijinal** olarak adlandırılan kapsayıcıdaki iletilerde hiçbir zenginleştirme ile ham iletileri var. Alta gelene kadar kapsayıcılardan birine dalın ve en son ileti dosyasını açın. Ardından, bu kapsayıcıdaki iletilere hiçbir zenginleştirme eklenmediğini doğrulamak için diğer kapsayıcı için de aynısını yapın.

Zenginleştirilmiş iletilere baktığınızda, hub adı, konumu ve müşteri kimliği yle birlikte "IoT Hub'ım"ı görmeniz gerekir:

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage","my IoT Hub":"contosotesthubmsgen3276","devicelocation":"$twin.tags.location","customerID":"6ce345b8-1e4a-411e-9398-d34587459a3a"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

İşte zenginleşmemiş bir mesaj. "IoT Hub'ım", "aygıt konumum" ve "customerID"in burada görünmediğini, çünkü bu alanların zenginleştirmelerle eklenmediğini unutmayın. Bu bitiş noktasının zenginleşmeleri yoktur.

```json
{"EnqueuedTimeUtc":"2019-05-10T06:06:32.7220000Z","Properties":{"level":"storage"},"SystemProperties":{"connectionDeviceId":"Contoso-Test-Device","connectionAuthMethod":"{\"scope\":\"device\",\"type\":\"sas\",\"issuer\":\"iothub\",\"acceptingIpFilterRule\":null}","connectionDeviceGenerationId":"636930642531278483","enqueuedTime":"2019-05-10T06:06:32.7220000Z"},"Body":"eyJkZXZpY2VJZCI6IkNvbnRvc28tVGVzdC1EZXZpY2UiLCJ0ZW1wZXJhdHVyZSI6MjkuMjMyMDE2ODQ4MDQyNjE1LCJodW1pZGl0eSI6NjQuMzA1MzQ5NjkyODQ0NDg3LCJwb2ludEluZm8iOiJUaGlzIGlzIGEgc3RvcmFnZSBtZXNzYWdlLiJ9"}
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu eğitimde oluşturduğunuz tüm kaynakları kaldırmak için kaynak grubunu silin. Bu eylem grubun içerdiği tüm kaynakları siler. Bu durumda, IoT hub'ını, depolama hesabını ve kaynak grubunun kendisini kaldırır.

### <a name="use-the-azure-cli-to-clean-up-resources"></a>Kaynakları temizlemek için Azure CLI'yi kullanın

Kaynak grubunu kaldırmak için [az group delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanın. Bu `$resourceGroup` öğretici başında **ContosoResourcesMsgEn** için ayarlanmış olduğunu hatırlayın.

```azurecli-interactive
az group delete --name $resourceGroup
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki adımları kullanarak IoT Hub iletilerine ileti zenginleştirmelerini yapılandırmış ve sınamışsınız:

**IoT Hub ileti zenginleştirmelerini kullanma**
> [!div class="checklist"]
> * İlk yöntem: Azure CLI'yi kullanarak kaynakları oluşturun ve ileti yönlendirmesini yapılandırın. [Azure portalını](https://portal.azure.com)kullanarak ileti zenginleştirmelerini el ile yapılandırın.
> * İkinci yöntem: Azure Kaynak Yöneticisi şablonu kullanarak kaynakları oluşturun ve ileti yönlendirme ve ileti zenginleştirmelerini yapılandırın.
> * Hub'a ileti gönderen bir IoT aygıtını simüle eden bir uygulama çalıştırın.
> * Sonuçları görüntüleyin ve ileti zenginleştirmelerinin beklendiği gibi çalıştığını doğrulayın.

İleti zenginleştirmeleri hakkında daha fazla bilgi için ileti [zenginleştirmelerine genel bakış](iot-hub-message-enrichments-overview.md)bölümüne bakın.

İleti yönlendirme hakkında daha fazla bilgi için şu makalelere bakın:

* [Aygıttan buluta iletileri farklı uç noktalara göndermek için IoT Hub ileti yönlendirmesini kullanma](iot-hub-devguide-messages-d2c.md)
* [Öğretici: IoT Hub yönlendirme](tutorial-routing.md)
