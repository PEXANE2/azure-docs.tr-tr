---
title: Set up Azure IoT Hub Device Provisioning Service using Azure CLI
description: Quickstart - Set up the Azure IoT Hub Device Provisioning Service using Azure CLI
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: 6406929c3abc3612da2c27edc45e10fd84883d73
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74228555"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-azure-cli"></a>Quickstart: Set up the IoT Hub Device Provisioning Service with Azure CLI

Azure CLI, komut satırından veya betik içinden Azure kaynakları oluşturmak ve yönetmek için kullanılır. This quickstart details using the Azure CLI to create an IoT hub and an IoT Hub Device Provisioning Service, and to link the two services together. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

> [!IMPORTANT]
> Both the IoT hub and the provisioning service you create in this quickstart will be publicly discoverable as DNS endpoints. Bu kaynaklar için kullanılan adları değiştirmeye karar verirseniz, hassas bilgiler kullanmaktan kaçının.
>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutuyla bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. 

Aşağıdaki örnek *westus* konumunda *my-sample-resource-group* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name my-sample-resource-group --location westus
```

> [!TIP]
> Örnekte kaynak grubu Batı ABD konumunda oluşturulur. `az account list-locations -o table` komutunu çalıştırarak kullanılabilir konumların listesini görüntüleyebilirsiniz.
>
>

## <a name="create-an-iot-hub"></a>Bir IoT Hub oluşturma

[az iot hub create](/cli/azure/iot/hub#az-iot-hub-create) komutuyla bir IoT hub oluşturun.

Aşağıdaki örnekte, *westus* konumunda *my-sample-hub* adlı bir IoT hub oluşturulur. An IoT hub name must be globally unique in Azure, so you may want to add a unique prefix or suffix to the example name, or choose a new name altogether. Make sure your name follows proper naming conventions for an IoT hub: it should be 3-50 characters in length, and can contain only upper or lower case alphanumeric characters or hyphens ('-'). 

```azurecli-interactive 
az iot hub create --name my-sample-hub --resource-group my-sample-resource-group --location westus
```

## <a name="create-a-device-provisioning-service"></a>Create a Device Provisioning Service

Create a Device Provisioning Service with the [az iot dps create](/cli/azure/iot/dps#az-iot-dps-create) command. 

The following example creates a provisioning service named *my-sample-dps* in the *westus* location. You will also need to choose a globally unique name for your own provisioning service. Make sure it follows proper naming conventions for an IoT Hub Device Provisioning Service: it should be 3-64 characters in length and can contain only upper or lower case alphanumeric characters or hyphens ('-').

```azurecli-interactive 
az iot dps create --name my-sample-dps --resource-group my-sample-resource-group --location westus
```

> [!TIP]
> Örnekte sağlama hizmeti Batı ABD konumunda oluşturulur. `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` komutunu çalıştırarak veya [Azure Durumu](https://azure.microsoft.com/status/) sayfasına gidip "Cihaz Sağlama Hizmeti" için arama yaparak kullanılabilir konumların listesini görüntüleyebilirsiniz. In commands, locations can be specified either in one word or multi-word format; for example: westus, West US, WEST US, etc. The value is not case sensitive. Konumu belirtirken birden çok sözcük biçimini kullanırsanız, değeri çift tırnak içine alın; örneğin, `-- location "West US"`.
>

## <a name="get-the-connection-string-for-the-iot-hub"></a>IoT hub için bağlantı dizesini alma

Cihaz Sağlama Hizmeti ile arasında bağlantı kurmak için IoT hub'ınızın bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini almak için [az iot hub show-connection-string](/cli/azure/iot/hub#az-iot-hub-show-connection-string) komutunu kullanın ve komutunu çıkışını kullanarak iki kaynağı bağlarken kullanacağınız değişkeni ayarlayın. 

The following example sets the *hubConnectionString* variable to the value of the connection string for the primary key of the hub's *iothubowner* policy (the `--policy-name` parameter can be used to specify a different policy). Trade out *my-sample-hub* for the unique IoT hub name you chose earlier. Komut, komut çıkışından bağlantı dizesini ayıklamak için Azure CLI [query](/cli/azure/query-azure-cli) ve [output](/cli/azure/format-output-azure-cli#tsv-output-format) seçeneklerini kullanır.

```azurecli-interactive 
hubConnectionString=$(az iot hub show-connection-string --name my-sample-hub --key primary --query connectionString -o tsv)
```

Bağlantı dizesini görmek için `echo` komutunu kullanabilirsiniz.

```azurecli-interactive 
echo $hubConnectionString
```

> [!NOTE]
> Bu iki komut, Bash altında çalışan konaklar için geçerlidir. Yerel bir Windows/CMD kabuğu veya PowerShell konağı kullanıyorsanız, söz konusu ortama yönelik doğru söz dizimini kullanacak şekilde komutları değiştirmeniz gerekir.
>

## <a name="link-the-iot-hub-and-the-provisioning-service"></a>IoT hub ile cihaz sağlama hizmetini bağlama

IoT hub ile sağlama hizmetinizi bağlamak için [az iot dps linked-hub create](/cli/azure/iot/dps/linked-hub#az-iot-dps-linked-hub-create) komutunu kullanın. 

The following example links an IoT hub named *my-sample-hub* in the *westus* location and a Device Provisioning Service named *my-sample-dps*. Trade out these names for the unique IoT hub and Device Provisioning Service names you chose earlier. The command uses the connection string for your IoT hub that was stored in the *hubConnectionString* variable in the previous step.

```azurecli-interactive 
az iot dps linked-hub create --dps-name my-sample-dps --resource-group my-sample-resource-group --connection-string $hubConnectionString --location westus
```

The command may take a few minutes to complete.

## <a name="verify-the-provisioning-service"></a>Sağlama hizmetini doğrulama

[az iot dps show](/cli/azure/iot/dps#az-iot-dps-show) komutuyla sağlama hizmetinizin ayrıntılarını alın.

Aşağıdaki örnekte *my-sample-dps* adlı sağlama hizmetinin ayrıntıları alınır. Trade out this name for your own Device Provisioning Service name.

```azurecli-interactive
az iot dps show --name my-sample-dps
```
Bağlı IoT hub, *properties.iotHubs* koleksiyonunda gösterilir.

![Verify Provisioning Service](./media/quick-setup-auto-provision-cli/verify-provisioning-service.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutları kullanarak sağlama hizmetini, IoT hub'ı veya kaynak grubuyla bu grubun tüm kaynaklarını silebilirsiniz. Replace the names of the resources written below with the names of your own resources.

Sağlama hizmetini silmek için [az iot dps delete](/cli/azure/iot/dps#az-iot-dps-delete) komutunu çalıştırın:

```azurecli-interactive
az iot dps delete --name my-sample-dps --resource-group my-sample-resource-group
```
IoT hub'ı silmek için [az iot hub delete](/cli/azure/iot/hub#az-iot-hub-delete) komutunu çalıştırın:

```azurecli-interactive
az iot hub delete --name my-sample-hub --resource-group my-sample-resource-group
```

Kaynak grubuyla birlikte bu kaynak grubunun tüm kaynaklarını silmek için [az group delete](/cli/azure/group#az-group-delete) komutunu çalıştırın:

```azurecli-interactive
az group delete --name my-sample-resource-group
```

## <a name="next-steps"></a>Sonraki adımlar

In this quickstart, you’ve deployed an IoT hub and a Device Provisioning Service instance, and linked the two resources. To learn how to use this setup to provision a simulated device, continue to the quickstart for creating a simulated device.

> [!div class="nextstepaction"]
> [Quickstart to create a simulated device](./quick-create-simulated-device.md)
