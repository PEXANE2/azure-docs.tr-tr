---
title: GPU özellikli kapsayıcı örneğini dağıtma
description: GPU kaynaklarını kullanarak bilgi işlem yoğun kapsayıcı uygulamalarını çalıştırmak için Azure kapsayıcı örneklerini nasıl dağıtılacağa öğrenin.
ms.topic: article
ms.date: 02/19/2020
ms.openlocfilehash: 0f1d21c62be5d7ae099faa2c6fcc440829bb451f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77525300"
---
# <a name="deploy-container-instances-that-use-gpu-resources"></a>GPU kaynaklarını kullanan kapsayıcı örneklerini dağıtma

Azure Kapsayıcı Örnekleri'nde belirli işlem yoğun iş yüklerini çalıştırmak [için, kapsayıcı gruplarınızı](container-instances-container-groups.md) *GPU kaynaklarıyla dağıtın.* Gruptaki konteyner örnekleri, CUDA ve derin öğrenme uygulamaları gibi konteyner iş yüklerini çalıştırırken bir veya daha fazla NVIDIA Tesla GPU'ya erişebilir.

Bu makalede, bir [YAML dosyası](container-instances-multi-container-yaml.md) veya Kaynak Yöneticisi şablonu kullanarak bir kapsayıcı grubu dağıtıldığında GPU kaynakları nın nasıl ekleyeceğiniz [gösterilmektedir.](container-instances-multi-container-group.md) Azure portalını kullanarak bir kapsayıcı örneğini dağıttığınızda GPU kaynaklarını da belirtebilirsiniz.

> [!IMPORTANT]
> Bu özellik şu anda önizlemede dir ve bazı [sınırlamalar geçerlidir.](#preview-limitations) Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="preview-limitations"></a>Önizleme sınırlamaları

Önizlemede, kapsayıcı gruplarında GPU kaynakları kullanırken aşağıdaki sınırlamalar uygulanır. 

[!INCLUDE [container-instances-gpu-regions](../../includes/container-instances-gpu-regions.md)]

Zaman içinde ek bölgeler için destek eklenecektir.

**Desteklenen işletim sistemi türleri**: Yalnızca Linux

**Ek sınırlamalar**: Bir kapsayıcı grubunu [sanal ağa](container-instances-vnet.md)dağıtırken GPU kaynakları kullanılamaz.

## <a name="about-gpu-resources"></a>GPU kaynakları hakkında

> [!IMPORTANT]
> GPU kaynakları yalnızca istek üzerine kullanılabilir. GPU kaynaklarına erişim isteğinde bulunmak için lütfen bir [Azure destek isteği][azure-support]gönderin.

### <a name="count-and-sku"></a>Kont ve SKU

Kapsayıcı örneğinde GPU kullanmak için, aşağıdaki bilgileri içeren bir *GPU kaynağı* belirtin:

* **Count** - GPU sayısı: **1**, **2**, veya **4**.
* **SKU** - GPU SKU: **K80**, **P100**, veya **V100**. Her SKU, NVIDIA Tesla GPU ile aşağıdaki Azure GPU özellikli VM ailelerinden biriyle eşler:

  | SKU | VM ailesi |
  | --- | --- |
  | K80 | [NC](../virtual-machines/nc-series.md) |
  | P100 | [NCv2](../virtual-machines/ncv2-series.md) |
  | V100 | [NCv3](../virtual-machines/ncv3-series.md) |

[!INCLUDE [container-instances-gpu-limits](../../includes/container-instances-gpu-limits.md)]

GPU kaynaklarını dağıtırken, cpu ve bellek kaynaklarını iş yüküne uygun olarak, bir önceki tabloda gösterilen en yüksek değerlere ayarlayın. Bu değerler şu anda CPU ve bellek kaynakları GPU kaynakları olmadan kapsayıcı gruplarında kullanılabilir daha büyüktür.  

### <a name="things-to-know"></a>Bilinmesi gerekenler

* **Dağıtım süresi** - GPU kaynaklarını içeren bir kapsayıcı grubunun oluşturulması **8-10 dakika**kadar sürer. Bunun nedeni, Azure'da bir GPU VM'nin sağlanması ve yapılandırılması için ek süredir. 

* **Fiyatlandırma** - GPU kaynakları olmayan kapsayıcı gruplarına benzer şekilde, GPU kaynaklarına sahip bir kapsayıcı grubu *süresi* boyunca tüketilen kaynaklar için Azure faturaları. Süre, konteyner grubu sona erene kadar ilk kabınızın görüntüsünü çekme saatinden itibaren hesaplanır. Kapsayıcı grubunu dağıtmak için zaman içermez.

  [Fiyatlandırma ayrıntılarına](https://azure.microsoft.com/pricing/details/container-instances/)bakın.

* **CUDA sürücüleri** - GPU kaynaklarına sahip konteyner örnekleri NVIDIA CUDA sürücüleri ve konteyner çalışma süreleri ile önceden sağlanmaktadır, böylece CUDA iş yükleri için geliştirilen konteyner görüntülerini kullanabilirsiniz.

  Bu aşamada CUDA 9.0'ı destekliyoruz. Örneğin, Docker dosyanız için aşağıdaki temel resimleri kullanabilirsiniz:
  * [nvidia/cuda:9.0-base-ubuntu16.04](https://hub.docker.com/r/nvidia/cuda/)
  * [tensorflow/tensorflow: 1.12.0-gpu-py3](https://hub.docker.com/r/tensorflow/tensorflow)
    
## <a name="yaml-example"></a>YAML örneği

GPU kaynakları eklemenin bir yolu, bir [YAML dosyası](container-instances-multi-container-yaml.md)kullanarak bir kapsayıcı grubu dağıtmaktır. Aşağıdaki YAML'yi *gpu-deploy-aci.yaml*adlı yeni bir dosyaya kopyalayın ve ardından dosyayı kaydedin. Bu YAML, *K80 GPU'lu* bir kapsayıcı örneğini belirten gpucontainergroup adlı bir kapsayıcı grubu oluşturur. Örnek bir örnek CUDA vektör ekleme uygulaması çalıştırın. Kaynak istekleri iş yükünü çalıştırmak için yeterlidir.

```YAML
additional_properties: {}
apiVersion: '2018-10-01'
name: gpucontainergroup
properties:
  containers:
  - name: gpucontainer
    properties:
      image: k8s-gcrio.azureedge.net/cuda-vector-add:v0.1
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
          gpu:
            count: 1
            sku: K80
  osType: Linux
  restartPolicy: OnFailure
```

Parametre için YAML dosya adını belirterek [az kapsayıcı][az-container-create] oluşturma `--file` komutu ile kapsayıcı grubunu dağıtın. GPU kaynaklarını destekleyen *eastus* gibi kapsayıcı grubu için bir kaynak grubunun adını ve konumunu sağlamanız gerekir.  

```azurecli
az container create --resource-group myResourceGroup --file gpu-deploy-aci.yaml --location eastus
```

Dağıtımın tamamlanması birkaç dakika sürer. Daha sonra, kapsayıcı başlar ve bir CUDA vektör ekleme işlemi çalıştırır. Günlük [çıktısını][az-container-logs] görüntülemek için az kapsayıcı günlükleri komutunu çalıştırın:

```azurecli
az container logs --resource-group myResourceGroup --name gpucontainergroup --container-name gpucontainer
```

Çıktı:

```Console
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

## <a name="resource-manager-template-example"></a>Kaynak Yöneticisi şablon örneği

GPU kaynakları ile bir kapsayıcı grubu dağıtmak için başka bir yolu bir [Kaynak Yöneticisi şablonu](container-instances-multi-container-group.md)kullanarak. Adlı `gpudeploy.json`bir dosya oluşturarak başlayın, ardından aşağıdaki JSON'u kopyalayın. Bu örnek, MNIST veri kümesine karşı [TensorFlow](https://www.tensorflow.org/) eğitim işi çalıştıran V100 GPU'lu bir kapsayıcı örneğini dağıtıyor. Kaynak istekleri iş yükünü çalıştırmak için yeterlidir.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "containerGroupName": {
        "type": "string",
        "defaultValue": "gpucontainergrouprm",
        "metadata": {
          "description": "Container Group name."
        }
      }
    },
    "variables": {
      "containername": "gpucontainer",
      "containerimage": "microsoft/samples-tf-mnist-demo:gpu"
    },
    "resources": [
      {
        "name": "[parameters('containerGroupName')]",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2018-10-01",
        "location": "[resourceGroup().location]",
        "properties": {
            "containers": [
            {
              "name": "[variables('containername')]",
              "properties": {
                "image": "[variables('containerimage')]",
                "resources": {
                  "requests": {
                    "cpu": 4.0,
                    "memoryInGb": 12.0,
                    "gpu": {
                        "count": 1,
                        "sku": "V100"
                  }
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "restartPolicy": "OnFailure"
        }
      }
    ]
}
```

[Az grubu dağıtım oluşturma][az-group-deployment-create] komutu ile şablonu dağıtın. Eastus gibi GPU kaynaklarını destekleyen bir bölgede oluşturulan bir kaynak grubunun adını *sağlamanız* gerekir.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file gpudeploy.json
```

Dağıtımın tamamlanması birkaç dakika sürer. Daha sonra, kapsayıcı başlar ve TensorFlow işini çalıştırır. Günlük [çıktısını][az-container-logs] görüntülemek için az kapsayıcı günlükleri komutunu çalıştırın:

```azurecli
az container logs --resource-group myResourceGroup --name gpucontainergrouprm --container-name gpucontainer
```

Çıktı:

```Console
2018-10-25 18:31:10.155010: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2018-10-25 18:31:10.305937: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: ccb6:00:00.0
totalMemory: 11.92GiB freeMemory: 11.85GiB
2018-10-25 18:31:10.305981: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: ccb6:00:00.0, compute capability: 3.7)
2018-10-25 18:31:14.941723: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.097
Accuracy at step 10: 0.6993
Accuracy at step 20: 0.8208
Accuracy at step 30: 0.8594
...
Accuracy at step 990: 0.969
Adding run metadata for 999
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

GPU kaynaklarını kullanmak pahalı olabileceğinden, kapsayıcılarınızın uzun süreler boyunca beklenmedik şekilde çalışmadığından emin olun. Azure portalında kapsayıcılarınızı izleyin veya [az kapsayıcı göster][az-container-show] komutuyla bir kapsayıcı grubunun durumunu kontrol edin. Örnek:

```azurecli
az container show --resource-group myResourceGroup --name gpucontainergroup --output table
```

Oluşturduğunuz kapsayıcı örnekleriyle çalışmayı bitirdiğinizde, bunları aşağıdaki komutlarla silin:

```azurecli
az container delete --resource-group myResourceGroup --name gpucontainergroup -y
az container delete --resource-group myResourceGroup --name gpucontainergrouprm -y
```

## <a name="next-steps"></a>Sonraki adımlar

* [YAML dosyaveya](container-instances-multi-container-yaml.md) [Kaynak Yöneticisi şablonu](container-instances-multi-container-group.md)kullanarak kapsayıcı grubunu dağıtma hakkında daha fazla bilgi edinin.
* Azure'da [GPU optimize edilmiş VM boyutları](../virtual-machines/linux/sizes-gpu.md) hakkında daha fazla bilgi edinin.


<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
