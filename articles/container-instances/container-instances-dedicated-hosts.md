---
title: Adanmış konakta dağıt
description: Azure Container Instances iş yükleriniz için doğru konak düzeyi yalıtımına ulaşmak üzere adanmış bir konak kullanın
ms.topic: article
ms.date: 01/17/2020
author: dkkapur
ms.author: dekapur
ms.openlocfilehash: adad0ddfc78530b3a3a7c139d9a95ec4790c8053
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76934142"
---
# <a name="deploy-on-dedicated-hosts"></a>Ayrılmış konaklara dağıtma

"Adanmış", güvenli bir şekilde çalışan kapsayıcılar için yalıtılmış ve ayrılmış bir işlem ortamı sağlayan bir Azure Container Instances (acı) SKU 'su. Özel iş yükü yalıtımının, kuruluşunuzun güvenlik ve uyumluluk gereksinimlerini karşılamasına yardımcı olması için, her bir kapsayıcı grubunda adanmış bir fiziksel sunucuya sahip olan adanmış SKU sonuçları kullanılması. 

Adanmış SKU, fiziksel sunucu perspektifinden iş yükü yalıtımı gerektiren kapsayıcı iş yükleri için uygundur.

## <a name="prerequisites"></a>Ön koşullar

* Adanmış SKU 'yu kullanmak için abonelik varsayılan sınırı 0 ' dır. Bu SKU 'yu üretim kapsayıcısı dağıtımlarınız için kullanmak isterseniz, sınırı artırmak için bir [Azure destek isteği][azure-support] oluşturun.

## <a name="use-the-dedicated-sku"></a>Adanmış SKU 'yu kullanma

> [!IMPORTANT]
> Adanmış SKU kullanımı, yalnızca şu anda kullanıma sunulan en son API sürümünde (2019-12-01) kullanılabilir. Dağıtım şablonunuzda bu API sürümünü belirtin.
>

API sürüm 2019-12-01 ' den başlayarak, bir acı dağıtımı için gerekli olan dağıtım şablonunun kapsayıcı grubu özellikleri bölümünde bir `sku` özelliği vardır. Şu anda, bu özelliği ACI için bir Azure Resource Manager dağıtım şablonunun parçası olarak kullanabilirsiniz. Öğreticide bir şablonla acı kaynaklarını dağıtma hakkında daha fazla bilgi edinin [: Kaynak Yöneticisi şablonu kullanarak çok kapsayıcılı bir grup dağıtın](https://docs.microsoft.com/azure/container-instances/container-instances-multi-container-group). 

`sku` özelliği aşağıdaki değerlerden birine sahip olabilir:
* `Standard`-hiper yönetici düzeyinde güvenliği güvence altına almaya devam eden standart acı dağıtımı seçeneği 
* `Dedicated`-kapsayıcı grubu için adanmış fiziksel konaklarla iş yükü düzeyi yalıtımı için kullanılır

## <a name="modify-your-json-deployment-template"></a>JSON dağıtım şablonunuzu değiştirme

Dağıtım şablonunuzda, aşağıdaki özellikleri değiştirin veya ekleyin:
* `resources`altında `apiVersion` `2012-12-01`olarak ayarlayın.
* Kapsayıcı grubu özellikleri altında, değer `Dedicated`sahip bir `sku` özelliği ekleyin.

Ayrılmış SKU 'yu kullanan bir kapsayıcı grubu dağıtım şablonunun kaynaklar bölümü için örnek bir kod parçacığı aşağıda verilmiştir:

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

Tek bir kapsayıcı örneği çalıştıran örnek bir kapsayıcı grubunu dağıtan bir şablon aşağıda verilmiştir:

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

## <a name="deploy-your-container-group"></a>Kapsayıcı grubunuzu dağıtın

Masaüstünüzde dağıtım şablonu dosyasını oluşturup düzenlediyseniz, dosyayı içine sürükleyerek Cloud Shell dizininize yükleyebilirsiniz... 

[az group create][az-group-create] komutuyla bir kaynak grubu oluşturun.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

[Az Group Deployment Create][az-group-deployment-create] komutuyla şablonu dağıtın.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file deployment-template.json
```

Birkaç saniye içinde Azure’dan bir ilk yanıt almanız gerekir. Ayrılmış bir konakta başarılı bir dağıtım gerçekleşir.

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
