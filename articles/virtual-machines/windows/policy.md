---
title: Azure'da Windows VM'lerinde ilkelerle güvenliği uygulayın
description: Bir Azure Kaynak Yöneticisi Windows Sanal Makine'ye ilke nasıl uygulanır?
services: virtual-machines-windows
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 0b71ba54-01db-43ad-9bca-8ab358ae141b
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 08/02/2017
ms.author: mimckitt
ms.openlocfilehash: 407e5cefe9f7f60c86de6e80133ff1b3f8b9003d
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82086547"
---
# <a name="apply-policies-to-windows-vms-with-azure-resource-manager"></a>Azure Kaynak Yöneticisi ile Windows VM'lerine ilkeler uygulama
İlkeleri kullanarak, bir kuruluş kuruluş kuruluş genelinde çeşitli kuralları ve kuralları uygulayabilir. İstenilen davranışın uygulanması, kuruluşun başarısına katkıda bulunmakla birlikte riski azaltmaya yardımcı olabilir. Bu makalede, kuruluşunuzun Sanal Makineleri için istenen davranışı tanımlamak için Azure Kaynak Yöneticisi ilkelerini nasıl kullanabileceğinizi açıklıyoruz.

İlkelere giriş için Azure [İlkesi nedir'e bakın.](../../governance/policy/overview.md)

## <a name="permitted-virtual-machines"></a>İzin Verilen Sanal Makineler
Kuruluşunuz için sanal makinelerin bir uygulamayla uyumlu olduğundan emin olmak için izin verilen işletim sistemlerini kısıtlayabilirsiniz. Aşağıdaki ilke örneğinde, yalnızca Windows Server 2012 R2 Datacenter Sanal Makineleri'nin oluşturulmasına izin verirsiniz:

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/virtualMachines",
          "Microsoft.Compute/VirtualMachineScaleSets"
        ]
      },
      {
        "not": {
          "allOf": [
            {
              "field": "Microsoft.Compute/imagePublisher",
              "in": [
                "MicrosoftWindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "WindowsServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "2012-R2-Datacenter"
              ]
            },
            {
              "field": "Microsoft.Compute/imageVersion",
              "in": [
                "latest"
              ]
            }
          ]
        }
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

Önceki ilkeyi herhangi bir Windows Server Datacenter görüntüsüne izin vermek için değiştirmek için joker karakter kullanın:

```json
{
  "field": "Microsoft.Compute/imageSku",
  "like": "*Datacenter"
}
```

Herhangi bir Windows Server 2012 R2 Datacenter veya daha yüksek görüntü izin vermek için önceki ilkeyi değiştirmek için anyOf kullanın:

```json
{
  "anyOf": [
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2012-R2-Datacenter*"
    },
    {
      "field": "Microsoft.Compute/imageSku",
      "like": "2016-Datacenter*"
    }
  ]
}
```

İlke alanları hakkında bilgi [için, İlke takma adlarına](../../governance/policy/concepts/definition-structure.md#aliases)bakın.

## <a name="managed-disks"></a>Yönetilen diskler

Yönetilen disklerin kullanımını gerektirmek için aşağıdaki ilkeyi kullanın:

```json
{
  "if": {
    "anyOf": [
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/virtualMachines"
          },
          {
            "field": "Microsoft.Compute/virtualMachines/osDisk.uri",
            "exists": true
          }
        ]
      },
      {
        "allOf": [
          {
            "field": "type",
            "equals": "Microsoft.Compute/VirtualMachineScaleSets"
          },
          {
            "anyOf": [
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osDisk.vhdContainers",
                "exists": true
              },
              {
                "field": "Microsoft.Compute/VirtualMachineScaleSets/osdisk.imageUrl",
                "exists": true
              }
            ]
          }
        ]
      }
    ]
  },
  "then": {
    "effect": "deny"
  }
}
```

## <a name="images-for-virtual-machines"></a>Sanal Makineler için Görüntüler

Güvenlik nedenleriyle, ortamınızda yalnızca onaylı özel görüntülerin dağıtılmasını gerektirebilirsiniz. Onaylanan görüntüleri içeren kaynak grubunu veya belirli onaylı görüntüleri belirtebilirsiniz.

Aşağıdaki örnek, onaylanmış bir kaynak grubundan görüntüler gerektirir:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in": [
                    "Microsoft.Compute/virtualMachines",
                    "Microsoft.Compute/VirtualMachineScaleSets"
                ]
            },
            {
                "not": {
                    "field": "Microsoft.Compute/imageId",
                    "contains": "resourceGroups/CustomImage"
                }
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
} 
```

Aşağıdaki örnekte onaylı resim kimlikleri belirtin:

```json
{
    "field": "Microsoft.Compute/imageId",
    "in": ["{imageId1}","{imageId2}"]
}
```

## <a name="virtual-machine-extensions"></a>Sanal Makine uzantıları

Belirli türdeki uzantıların kullanımını yasaklamak isteyebilirsiniz. Örneğin, bir uzantı belirli özel sanal makine görüntüleriyle uyumlu olmayabilir. Aşağıdaki örnek, belirli bir uzantıntı nasıl engellenir gösterir. Hangi uzantıyı engelleyecek lerini belirlemek için yayımcı ve türü kullanır.

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "equals": "Microsoft.Compute/virtualMachines/extensions"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/publisher",
                "equals": "Microsoft.Compute"
            },
            {
                "field": "Microsoft.Compute/virtualMachines/extensions/type",
                "equals": "{extension-type}"

      }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```


## <a name="azure-hybrid-use-benefit"></a>Azure Hibrit Kullanım Teklifi

Şirket içi lisansınız olduğunda, lisans ücretini sanal makinelerinize kaydedebilirsiniz. Lisansın yoksa, seçeneği yasaklamanız gerekir. Aşağıdaki ilke, Azure Karma Kullanım Avantajı'nın (AHUB) kullanımını yasaklar:

```json
{
    "if": {
        "allOf": [
            {
                "field": "type",
                "in":[ "Microsoft.Compute/virtualMachines","Microsoft.Compute/VirtualMachineScaleSets"]
            },
            {
                "field": "Microsoft.Compute/licenseType",
                "exists": true
            }
        ]
    },
    "then": {
        "effect": "deny"
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar
* Bir ilke kuralı tanımladıktan sonra (önceki örneklerde gösterildiği gibi), ilke tanımını oluşturmanız ve bir kapsama atamanız gerekir. Kapsam bir abonelik, kaynak grubu veya kaynak olabilir. İlkeler atamak için kaynak [ilkeleri atamak ve yönetmek için Azure portalını kullan](../../governance/policy/assign-policy-portal.md)'ı kullanma [(ilkeleri atamak için PowerShell'i kullanın](../../governance/policy/assign-policy-powershell.md)veya ilkeleri [atamak için Azure CLI'yi kullanın.](../../governance/policy/assign-policy-azurecli.md)
* Kaynak ilkelerine giriş için Azure [İlkesi nedir'](../../governance/policy/overview.md)e bakın.
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).
