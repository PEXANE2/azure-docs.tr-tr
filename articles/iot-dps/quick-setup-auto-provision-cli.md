---
title: Azure CLI kullanarak Azure IoT Hub Aygıt Sağlama Hizmeti ayarlama
description: Quickstart - Azure CLI kullanarak Azure IoT Hub Aygıt Sağlama Hizmeti'ni (DPS) ayarlama
author: wesmc7777
ms.author: wesmc
ms.date: 11/08/2019
ms.topic: quickstart
ms.service: iot-dps
services: iot-dps
ms.custom: mvc
ms.openlocfilehash: ea1cae1f5a30d4cd76df39fec43f3818178fc213
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77484205"
---
# <a name="quickstart-set-up-the-iot-hub-device-provisioning-service-with-azure-cli"></a>Quickstart: Azure CLI ile IoT Hub Aygıt Sağlama Hizmetini Ayarlama

Azure CLI, komut satırından veya betik içindeki Azure kaynaklarını oluşturmak ve yönetmek için kullanılır. Bu hızlı başlangıç, bir IoT hub'ı ve Bir IoT Hub Aygıt Sağlama Hizmeti oluşturmak ve iki hizmeti birbirine bağlamak için Azure CLI'yi kullanarak ayrıntıları kullanır. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

> [!IMPORTANT]
> Hem IoT hub'ı hem de bu hızlı başlatmada oluşturduğunuz sağlama hizmeti, DNS uç noktaları olarak genel olarak keşfedilebilir. Bu kaynaklar için kullanılan adları değiştirmeye karar verirseniz, hassas bilgiler kullanmaktan kaçının.
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

## <a name="create-an-iot-hub"></a>IoT hub oluşturma

[az iot hub create](/cli/azure/iot/hub#az-iot-hub-create) komutuyla bir IoT hub oluşturun.

Aşağıdaki örnekte, *westus* konumunda *my-sample-hub* adlı bir IoT hub oluşturulur. IoT hub adı Azure'da genel olarak benzersiz olmalıdır, bu nedenle örnek ada benzersiz bir önek veya sonek eklemek veya tamamen yeni bir ad seçmek isteyebilirsiniz. Adınızın bir IoT hub'ı için uygun adlandırma kurallarını izlediğinden emin olun: 3-50 karakter uzunluğunda olmalıdır ve yalnızca büyük veya küçük harfleralfaerik karakterler veya tireler ('-') içerebilir. 

```azurecli-interactive 
az iot hub create --name my-sample-hub --resource-group my-sample-resource-group --location westus
```

## <a name="create-a-device-provisioning-service"></a>Aygıt Sağlama Hizmeti Oluşturma

[az iot dps create](/cli/azure/iot/dps#az-iot-dps-create) komutu ile bir Aygıt Sağlama Hizmeti oluşturun. 

Aşağıdaki örnek, *westus* konumunda *my-sample-dps* adlı bir sağlama hizmeti oluşturur. Ayrıca, kendi sağlama hizmetiniz için benzersiz bir ad seçmeniz gerekir. Bir IoT Hub Aygıt Sağlama Hizmeti için uygun adlandırma kurallarını izlediğinden emin olun: 3-64 karakter uzunluğunda olmalıdır ve yalnızca büyük veya küçük harfleralfaerik karakterler veya tireler ('-') içerebilir.

```azurecli-interactive 
az iot dps create --name my-sample-dps --resource-group my-sample-resource-group --location westus
```

> [!TIP]
> Örnekte sağlama hizmeti Batı ABD konumunda oluşturulur. `az provider show --namespace Microsoft.Devices --query "resourceTypes[?resourceType=='ProvisioningServices'].locations | [0]" --out table` komutunu çalıştırarak veya [Azure Durumu](https://azure.microsoft.com/status/) sayfasına gidip "Cihaz Sağlama Hizmeti" için arama yaparak kullanılabilir konumların listesini görüntüleyebilirsiniz. Komutlarda, konumlar tek bir sözcük veya çok sözcük biçiminde belirtilebilir; örneğin: westus, Batı ABD, WEST US, vb. Değer büyük/küçük harf duyarlı değildir. Konumu belirtirken birden çok sözcük biçimini kullanırsanız, değeri çift tırnak içine alın; örneğin, `--location "West US"`.
>

## <a name="get-the-connection-string-for-the-iot-hub"></a>IoT hub için bağlantı dizesini alma

Cihaz Sağlama Hizmeti ile arasında bağlantı kurmak için IoT hub'ınızın bağlantı dizesine ihtiyacınız vardır. Bağlantı dizesini almak için [az iot hub show-connection-string](/cli/azure/iot/hub#az-iot-hub-show-connection-string) komutunu kullanın ve komutunu çıkışını kullanarak iki kaynağı bağlarken kullanacağınız değişkeni ayarlayın. 

Aşağıdaki örnek, *hub'ın* *iothubowner* ilkesinin birincil anahtarı için bağlantı dizesinin değerine `--policy-name` hubConnectionString değişkenini ayarlar (parametre farklı bir ilke belirtmek için kullanılabilir). Daha önce seçtiğiniz benzersiz IoT hub adı için *örnek hub'ımı* değiştirin. Komut, komut çıkışından bağlantı dizesini ayıklamak için Azure CLI [query](/cli/azure/query-azure-cli) ve [output](/cli/azure/format-output-azure-cli#tsv-output-format) seçeneklerini kullanır.

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

Aşağıdaki örnek, *westus* *konumundamy-sample-hub* adlı bir IoT hub'ını ve *my-sample-dps*adlı bir Aygıt Sağlama Hizmeti'ni bağlar. Bu adları, daha önce seçtiğiniz benzersiz IoT hub'ı ve Aygıt Sağlama Hizmeti adları için değiştirin. Komut, önceki adımda *hubConnectionString* değişkeninde depolanan IoT hub'ınızın bağlantı dizesini kullanır.

```azurecli-interactive 
az iot dps linked-hub create --dps-name my-sample-dps --resource-group my-sample-resource-group --connection-string $hubConnectionString --location westus
```

Komutun tamamlanması birkaç dakika sürebilir.

## <a name="verify-the-provisioning-service"></a>Sağlama hizmetini doğrulama

[az iot dps show](/cli/azure/iot/dps#az-iot-dps-show) komutuyla sağlama hizmetinizin ayrıntılarını alın.

Aşağıdaki örnekte *my-sample-dps* adlı sağlama hizmetinin ayrıntıları alınır. Bu adı kendi Cihaz Sağlama Hizmeti adınız için değiştirin.

```azurecli-interactive
az iot dps show --name my-sample-dps
```
Bağlı IoT hub, *properties.iotHubs* koleksiyonunda gösterilir.

![Sağlama Hizmetini Doğrula](./media/quick-setup-auto-provision-cli/verify-provisioning-service.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu koleksiyondaki diğer hızlı başlangıçlar, bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıçlar veya öğreticilerle devam etmeyi planlıyorsanız, bu hızlı başlangıçta oluşturulan kaynakları temizlemeyin. Devam etmeyi planlamıyorsanız, aşağıdaki komutları kullanarak sağlama hizmetini, IoT hub'ı veya kaynak grubuyla bu grubun tüm kaynaklarını silebilirsiniz. Aşağıda yazılan kaynakların adlarını kendi kaynaklarınızın adlarıyla değiştirin.

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

Bu hızlı başlangıçta, bir IoT hub'ı ve aygıt sağlama hizmeti örneğini dağıttınız ve iki kaynağı birbirine bağladınız. Bu kurulumu niçin simüle edilmiş bir aygıt sağlamak için kullanacağınızı öğrenmek için, benzetilen bir aygıt oluşturmak için hızlı başlatmaya devam edin.

> [!div class="nextstepaction"]
> [Simüle edilmiş bir aygıt oluşturmak için hızlı başlatma](./quick-create-simulated-device.md)
