---
title: Azure sanal makine ölçek kümeleri eklenen veri diskleri
description: Sanal makine ölçek kümeleriyle eklenen veri disklerini, belirli kullanım durumlarının anahatları aracılığıyla nasıl kullanacağınızı öğrenin.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: disks
ms.date: 4/25/2017
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 9e4bdf868d3f8ddf3a049509ead30a4b1ba341b7
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86527447"
---
# <a name="azure-virtual-machine-scale-sets-and-attached-data-disks"></a>Azure sanal makine ölçek kümeleri ve bağlı veri diskleri
Kullanılabilir depolama alanınızı genişletmek için Azure [sanal makine ölçek kümeleri](./index.yml), bağlı veri diskleri içeren sanal makine örneklerini destekler. Ölçek kümesi oluşturulduğunda veya mevcut bir ölçek kümesine veri diskleri ekleyebilirsiniz.

> [!NOTE]
> Bağlı veri diskleri içeren bir ölçek kümesi oluşturduğunuzda tek başına Azure sanal makinelerinde olduğu gibi, diskleri kullanabilmek için bir sanal makine içinde takmanız ve biçimlendirmeniz gerekir. Bu işlemi tamamlamanın uygun bir yolu, bir sanal makine üzerindeki tüm verileri bölümlemek ve biçimlendirmek için bir betik çağıran Özel Betik Uzantısı kullanılmasıdır. Bununla ilgili örnekler için bkz. [Azure CLI](tutorial-use-disks-cli.md#prepare-the-data-disks) [Azure PowerShell](tutorial-use-disks-powershell.md#prepare-the-data-disks).


## <a name="create-and-manage-disks-in-a-scale-set"></a>Ölçek kümesinde diskler oluşturma ve yönetme
Bağlı veri diskleri içeren bir ölçek kümesi oluşturma, veri diskleri hazırlayıp biçimlendirme veya veri diskleri ekleme ve kaldırma işlemlerine ilişkin ayrıntılı bilgiler için aşağıdaki öğreticilerden birine bakın:

- [Azure CLI](tutorial-use-disks-cli.md)
- [Azure PowerShell](tutorial-use-disks-powershell.md)

Bu makalenin geri kalanında, veri diskleri gerektiren Service Fabric kümeleri gibi belirli kullanım senaryoları veya bir ölçek kümesine içerik barındıran mevcut veri disklerinin eklenmesi açıklanmaktadır.


## <a name="create-a-service-fabric-cluster-with-attached-data-disks"></a>Eklenen veri diskleri ile bir Service Fabric kümesi oluşturma
Azure’da çalışan bir [Service Fabric](../service-fabric/index.yml) kümesindeki her [düğüm türü](../service-fabric/service-fabric-cluster-nodetypes.md), bir sanal makine ölçek kümesi tarafından desteklenir. Azure Resource Manager şablonunu kullanarak, Service Fabric kümesini oluşturan ölçek kümelerine veri diskleri ekleyebilirsiniz. Başlangıç noktası olarak [mevcut bir şablonu](https://github.com/Azure-Samples/service-fabric-cluster-templates) kullanabilirsiniz. Şablonda, _Microsoft.Compute/virtualMachineScaleSets_ kaynaklarının _storageProfile_ seçeneğine _dataDisks_ bölümü ekleyin ve şablonu dağıtın. Aşağıdaki örnekte 128 GB veri diski kullanıma açılır:

```json
"dataDisks": [
    {
    "diskSizeGB": 128,
    "lun": 0,
    "createOption": "Empty"
    }
]
```

Küme dağıtıldığında veri disklerini otomatik olarak bölebilir, biçimlendirebilir ve bağlayabilirsiniz. Ölçek kümelerinin _virtualMachineProfile_ bölümünün _extensionProfile_ öğesine özel bir betik uzantısı ekleyin.

Bir Windows kümesinde veri disklerini otomatik olarak hazırlamak için şunları ekleyin:

```json
{
    "name": "customScript",
    "properties": {
        "publisher": "Microsoft.Compute",
        "type": "CustomScriptExtension",
        "typeHandlerVersion": "1.8",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.ps1"
        ],
        "commandToExecute": "powershell -ExecutionPolicy Unrestricted -File prepare_vm_disks.ps1"
        }
    }
}
```
Bir Linux kümesinde veri disklerini otomatik olarak hazırlamak için şunları ekleyin:
```json
{
    "name": "lapextension",
    "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
        "fileUris": [
            "https://raw.githubusercontent.com/Azure-Samples/compute-automation-configurations/master/prepare_vm_disks.sh"
        ],
        "commandToExecute": "bash prepare_vm_disks.sh"
        }
    }
}
```


## <a name="adding-pre-populated-data-disks-to-an-existing-scale-set"></a>Önceden doldurulmuş veri disklerini mevcut bir ölçek kümesine ekleme
Ölçek kümesi modelinde belirtilen veri diskleri her zaman boştur. Ancak bir ölçek kümesindeki belirli bir VM’ye var olan bir veri diski ekleyebilirsiniz. Ölçek kümesindeki tüm VM 'Ler arasında veri yaymak isterseniz, veri diskinizi çoğaltabilir ve ölçek kümesindeki her bir VM 'ye ekleyebilir ya da verileri içeren özel bir görüntü oluşturabilir veya Azure dosyalarını veya benzer bir veri depolama teklifini kullanabilirsiniz.


## <a name="additional-notes"></a>Ek notlar
Azure Yönetilen diskleri ve ölçek kümesi bağlı veri diskleri için destek, Microsoft.Compute API’sinin [_2016-04-30-preview_](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/compute/resource-manager/Microsoft.Compute/preview/2016-04-30-preview/compute.json) veya üstü sürümlere eklenmiştir.

Ölçek kümelerinde eklenen veri diskleri için Azure portal desteği sınırlıdır. Gereksinimlerinize bağlı olarak, bağlı diskleri yönetmek için Azure şablonları, CLI, PowerShell, SDK’lar ve REST API kullanabilirsiniz.
