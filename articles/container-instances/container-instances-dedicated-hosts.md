---
title: Özel ana bilgisayarda dağıtma
description: Azure Kapsayıcı Örnekleri iş yükleriniçin gerçek ana bilgisayar düzeyinde yalıtım elde etmek için özel bir ana bilgisayar kullanın
ms.topic: article
ms.date: 01/17/2020
author: dkkapur
ms.author: dekapur
ms.openlocfilehash: a614d6b5d0cf5c6c1df5ffcb90e56960d6b8a2a9
ms.sourcegitcommit: 75089113827229663afed75b8364ab5212d67323
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "82025042"
---
# <a name="deploy-on-dedicated-hosts"></a>Ayrılmış konaklara dağıtma

"Adanmış", güvenli bir şekilde çalışan kapsayıcılar için yalıtılmış ve özel bir işlem ortamı sağlayan bir Azure Kapsayıcı Örnekleri (ACI) sku'sudur. Özel sku sonuçlarını kullanmak, her kapsayıcı grubunun azure veri merkezinde özel bir fiziksel sunucuya sahip olmasına bağlı olarak, kuruluşunuzun güvenlik ve uyumluluk gereksinimlerini karşılamaya yardımcı olmak için tam iş yükü yalıtımı sağlar. 

Özel sku fiziksel sunucu açısından iş yükü yalıtımı gerektiren kapsayıcı iş yükleri için uygundur.

## <a name="prerequisites"></a>Önkoşullar

* Herhangi bir aboneliğin özel sku'yu kullanması için varsayılan sınır 0'dır. Bu sku'yu üretim kapsayıcısı dağıtımlarınız için kullanmak istiyorsanız, sınırı artırmak için bir [Azure Destek isteği][azure-support] oluşturun.

## <a name="use-the-dedicated-sku"></a>Özel sku kullanın

> [!IMPORTANT]
> Özel sku kullanarak şu anda yayılıyor en son API sürümü (2019-12-01) kullanılabilir. Dağıtım şablonunuzda bu API sürümünü belirtin.
>

API sürümü 2019-12-01'den başlayarak, bir Dağıtım şablonunun kapsayıcı grubu özellikleri bölümünün altında, ACI dağıtımı için gerekli olan bir `sku` özellik vardır. Şu anda, bu özelliği ACI için bir Azure Kaynak Yöneticisi dağıtım şablonunun bir parçası olarak kullanabilirsiniz. Öğretici'de şablonla ACI kaynaklarını dağıtma hakkında daha fazla bilgi [edinin: Kaynak Yöneticisi şablonu kullanarak çok kapsayıcılı bir grubu dağıtın.](https://docs.microsoft.com/azure/container-instances/container-instances-multi-container-group) 

Özellik `sku` aşağıdaki değerlerden birine sahip olabilir:
* `Standard`- hala hipervizör düzeyinde güvenliği garanti eden standart ACI dağıtım seçeneği 
* `Dedicated`- konteyner grubu için özel fiziksel ana bilgisayarları ile iş yükü düzeyi yalıtımı için kullanılır

## <a name="modify-your-json-deployment-template"></a>JSON dağıtım şablonunuzu değiştirme

Dağıtım şablonunuzda aşağıdaki özellikleri değiştirin veya ekleyin:
* Altında `resources`, `apiVersion` `2019-12-01`ayarlayın .
* Kapsayıcı grup özelliklerinin altında, `sku` değeri `Dedicated`olan bir özellik ekleyin.

Aşağıda, özel sku'yu kullanan kapsayıcı grubu dağıtım şablonunun kaynaklar bölümüiçin örnek bir bölüm verilmiştir:

```json
[...]
"resources": [
    {
        "name": "[parameters('containerGroupName')]",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2019-12-01",
        "location": "[resourceGroup().location]",    
        "properties": {
            "sku": "Dedicated",
            "containers": {
                [...]
            }
        }
    }
]
```

Aşağıda, tek bir kapsayıcı örneği çalıştıran örnek bir kapsayıcı grubunu dağıtan tam bir şablon verilmiştir:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "containerGroupName": {
        "type": "string",
        "defaultValue": "myContainerGroup",
        "metadata": {
          "description": "Container Group name."
        }
      }
    },
    "resources": [
        {
            "name": "[parameters('containerGroupName')]",
            "type": "Microsoft.ContainerInstance/containerGroups",
            "apiVersion": "2019-12-01",
            "location": "[resourceGroup().location]",
            "properties": {
                "sku": "Dedicated",
                "containers": [
                    {
                        "name": "container1",
                        "properties": {
                            "image": "nginx",
                            "command": [
                                "/bin/sh",
                                "-c",
                                "while true; do echo `date`; sleep 1000000; done"
                            ],
                            "ports": [
                                {
                                    "protocol": "TCP",
                                    "port": 80
                                }
                            ],
                            "environmentVariables": [],
                            "resources": {
                                "requests": {
                                    "memoryInGB": 1.0,
                                    "cpu": 1
                                }
                            }
                        }
                    }
                ],
                "restartPolicy": "Always",
                "ipAddress": {
                    "ports": [
                        {
                            "protocol": "TCP",
                            "port": 80
                        }
                    ],
                    "type": "Public"
                },
                "osType": "Linux"
            },
            "tags": {}
        }
    ]
}
```

## <a name="deploy-your-container-group"></a>Konteyner grubunuzu dağıtma

Masaüstünüzde dağıtım şablonu dosyasını oluşturduysanız ve düzenlediyseniz, dosyayı içine sürükleyerek dosyayı Bulut Kabuğu dizininize yükleyebilirsiniz. 

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[Az grubu dağıtım oluşturma][az-group-deployment-create] komutu ile şablonu dağıtın.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file deployment-template.json
```

Birkaç saniye içinde Azure’dan bir ilk yanıt almanız gerekir. Başarılı bir dağıtım özel bir ana bilgisayarda gerçekleşir.

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
