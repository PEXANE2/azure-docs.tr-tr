---
title: Azure'da Linux VM'leri ile ilgili ilkelerle güvenliği uygulayın
description: Bir Azure Kaynak Yöneticisi Linux Sanal Makine'ye ilke nasıl uygulanır?
services: virtual-machines-linux
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 06778ab4-f8ff-4eed-ae10-26a276fc3faa
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 08/02/2017
ms.author: mimckitt
ms.openlocfilehash: b585b4d0aef06b01cba07ab8ef016b59f9b4f61b
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80878927"
---
# <a name="apply-policies-to-linux-vms-with-azure-resource-manager"></a>Azure Kaynak Yöneticisi ile Linux VM'lerine ilkeler uygulayın
İlkeleri kullanarak, bir kuruluş kuruluş kuruluş genelinde çeşitli kuralları ve kuralları uygulayabilir. İstenilen davranışın uygulanması, kuruluşun başarısına katkıda bulunmakla birlikte riski azaltmaya yardımcı olabilir. Bu makalede, kuruluşunuzun Sanal Makineleri için istenen davranışı tanımlamak için Azure Kaynak Yöneticisi ilkelerini nasıl kullanabileceğinizi açıklıyoruz.

İlkelere giriş için Azure [İlkesi nedir'e bakın.](../../governance/policy/overview.md)

## <a name="permitted-virtual-machines"></a>İzin Verilen Sanal Makineler
Kuruluşunuz için sanal makinelerin bir uygulamayla uyumlu olduğundan emin olmak için izin verilen işletim sistemlerini kısıtlayabilirsiniz. Aşağıdaki ilke örneğinde, yalnızca Ubuntu 14.04.2-LTS Sanal Makinelerin oluşturulmasına izin verirsiniz.

```json
{
  "if": {
    "allOf": [
      {
        "field": "type",
        "in": [
          "Microsoft.Compute/disks",
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
                "Canonical"
              ]
            },
            {
              "field": "Microsoft.Compute/imageOffer",
              "in": [
                "UbuntuServer"
              ]
            },
            {
              "field": "Microsoft.Compute/imageSku",
              "in": [
                "14.04.2-LTS"
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

Herhangi bir Ubuntu LTS görüntüsüne izin vermek için önceki ilkeyi değiştirmek için joker karakter kullanın: 

```json
{
  "field": "Microsoft.Compute/virtualMachines/imageSku",
  "like": "*LTS"
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


## <a name="next-steps"></a>Sonraki adımlar
* Bir ilke kuralı tanımladıktan sonra (önceki örneklerde gösterildiği gibi), ilke tanımını oluşturmanız ve bir kapsama atamanız gerekir. Kapsam bir abonelik, kaynak grubu veya kaynak olabilir. İlkeler atamak için kaynak [ilkeleri atamak ve yönetmek için Azure portalını kullan](../../governance/policy/assign-policy-portal.md)'ı kullanma [(ilkeleri atamak için PowerShell'i kullanın](../../governance/policy/assign-policy-powershell.md)veya ilkeleri [atamak için Azure CLI'yi kullanın.](../../governance/policy/assign-policy-azurecli.md)
* Kaynak ilkelerine giriş için Azure [İlkesi nedir'](../../governance/policy/overview.md)e bakın.
* Kuruluşların abonelikleri etkili bir şekilde yönetmek için Resource Manager'ı nasıl kullanabileceği hakkında yönergeler için bkz. [Azure kurumsal iskelesi: öngörücü abonelik idaresi](/azure/architecture/cloud-adoption-guide/subscription-governance).
