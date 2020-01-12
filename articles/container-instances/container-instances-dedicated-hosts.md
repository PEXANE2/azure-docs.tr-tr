---
title: Adanmış konaklarda dağıtın
description: İş yükleriniz için gerçek konak düzeyi yalıtımına ulaşmak üzere adanmış Konakları kullanın
ms.topic: article
ms.date: 01/10/2020
ms.author: danlep
ms.openlocfilehash: 619a39f4d08a4308cb0f566bc50860e9562bf9e4
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903762"
---
# <a name="deploy-on-dedicated-hosts"></a>Adanmış konaklarda dağıtın

"Adanmış", güvenli bir şekilde çalışan kapsayıcılar için yalıtılmış ve ayrılmış bir işlem ortamı sağlayan bir Azure Container Instances (acı) SKU 'su. Özel iş yükü yalıtımının, kuruluşunuzun güvenlik ve uyumluluk gereksinimlerini karşılamasına yardımcı olması için, her bir kapsayıcı grubunda adanmış bir fiziksel sunucuya sahip olan adanmış SKU sonuçları kullanılması. 

Adanmış SKU, fiziksel sunucu perspektifinden iş yükü yalıtımı gerektiren kapsayıcı iş yükleri için uygundur.

## <a name="using-the-dedicated-sku"></a>Adanmış SKU 'yu kullanma

> [!IMPORTANT]
> Adanmış SKU kullanımı, yalnızca şu anda kullanıma sunulan en son API sürümünde (2019-12-01) kullanılabilir. Dağıtım şablonunuzda bu API sürümünü belirtin. Ayrıca, adanmış SKU 'yu kullanmak için abonelik varsayılan sınırı 0 ' dır. Bu SKU 'yu üretim kapsayıcısı dağıtımlarınız için kullanmak isterseniz lütfen bir [Azure destek isteği][azure-support] oluşturun

API sürüm 2019-12-01 ' den başlayarak, bir acı dağıtımı için gerekli olan dağıtım şablonunun kapsayıcı grubu özellikleri bölümünde bir "SKU" özelliği bulunur. Şu anda, bu özelliği ACI için bir Azure Resource Manager dağıtım şablonunun parçası olarak kullanabilirsiniz. Öğreticide bir şablonla acı kaynaklarını dağıtma hakkında daha fazla bilgi edinebilirsiniz [: Kaynak Yöneticisi şablonu kullanarak çok kapsayıcılı bir grup dağıtma](https://docs.microsoft.com/azure/container-instances/container-instances-multi-container-group). 

SKU özelliği aşağıdaki değerlerden birine sahip olabilir:
* Standart-hiper yönetici düzeyinde güvenliği garanti eden standart acı dağıtımı seçeneği 
* Adanmış-kapsayıcı grubu için adanmış fiziksel konaklarla iş yükü düzeyi yalıtımı için kullanılır

## <a name="modify-your-json-deployment-template"></a>JSON dağıtım şablonunuzu değiştirme

Dağıtım şablonunuzda, kapsayıcı grubu kaynağı belirtildiğinde `"apiVersion": "2019-12-01",`emin olun. Kapsayıcı grubu kaynağının Özellikler bölümünde `"sku": "Dedicated",`ayarlayın.

Ayrılmış SKU 'yu kullanan bir kapsayıcı grubu dağıtım şablonunun kaynaklar bölümü için örnek bir kod parçacığı aşağıda verilmiştir:

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
                "osType": "Linux",
            },
            "location": "eastus2euap",
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

Birkaç saniye içinde Azure’dan bir ilk yanıt almanız gerekir. Dağıtım tamamlandıktan sonra ACI hizmeti tarafından kalıcı hale gelen tüm veriler, verdiğiniz anahtarla şifrelenir.

<!-- LINKS - Internal -->
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
