---
title: include dosyası
description: include dosyası
services: storage
author: jboeshart
ms.service: storage
ms.topic: include
ms.date: 06/05/2018
ms.author: jaboes
ms.custom: include file
ms.openlocfilehash: 126b488d2bb59e2904bee646301240efe6fe71a4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76037557"
---
Bu belge, sanal makineler sağlamak için Azure Kaynak Yöneticisi şablonlarını kullanırken yönetilen ve yönetilmeyen diskler arasındaki farkları gözden geçirir. Örnekler, yönetilen disklere yönetilmeyen Diskler kullanan varolan şablonları güncelleştirmenize yardımcı olur. Referans olarak, [101-vm-simple-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) şablonu bir kılavuz olarak kullanıyoruz. Şablonu, doğrudan karşılaştırmak [istiyorsanız, yönetilen diskleri](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) kullanarak hem [yönetilen Diskleri](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) hem de önceki bir sürümü kullanarak görebilirsiniz.

## <a name="unmanaged-disks-template-formatting"></a>Yönetilmeyen Diskler şablon biçimlendirme

Başlamak için, yönetilmeyen disklerin nasıl dağıtıldıklarına bir göz atalım. Yönetilmeyen diskler oluştururken, VHD dosyalarını tutmak için bir depolama hesabı gerekir. Yeni bir depolama hesabı oluşturabilir veya zaten var olan bir depoyu kullanabilirsiniz. Bu makalede, yeni bir depolama hesabı oluşturmak için nasıl gösterir. Aşağıda gösterildiği gibi kaynaklar bloğunda bir depolama hesabı kaynağı oluşturun.

```json
{
    "type": "Microsoft.Storage/storageAccounts",
    "apiVersion": "2018-07-01",
    "name": "[variables('storageAccountName')]",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

Sanal makine nesnesi içinde, sanal makineden önce oluşturulduğundan emin olmak için depolama hesabına bir bağımlılık ekleyin. `storageProfile` Bu bölümde, depolama hesabına başvuran ve işletim sistemi diski ve tüm veri diskleri için gerekli olan VHD konumunun tam URI'sini belirtin.

```json
{
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2018-10-01",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a>Yönetilen diskler şablon biçimlendirme

Azure Yönetilen Diskler ile disk üst düzey bir kaynak haline gelir ve artık kullanıcı tarafından oluşturulacak bir depolama hesabı gerektirmez. Yönetilen diskler ilk `2016-04-30-preview` OLARAK API sürümünde ortaya çıkarıldı, sonraki tüm API sürümlerinde kullanılabilir ve artık varsayılan disk türüdür. Aşağıdaki bölümler varsayılan ayarlar arasında yürür ve disklerinizi nasıl daha fazla özelleştirebilirsiniz ayrıntılarıyla anlatır.

> [!NOTE]
> ApI sürümü arasında kırılma değişiklikleri `2016-04-30-preview` olduğu gibi `2016-04-30-preview` daha sonra `2017-03-30`kullanılması tavsiye edilir .
>
>

### <a name="default-managed-disk-settings"></a>Varsayılan yönetilen disk ayarları

Yönetilen disklere sahip bir VM oluşturmak için artık depolama hesabı kaynağını oluşturmanız gerekmez. Aşağıdaki şablon örneğine atıfta bulunarak, önceki unmanged disk örneklerinden bazı farklılıklar dikkate almak için vardır:

- Yönetilen `apiVersion` diskleri destekleyen bir sürümdür.
- `osDisk`ve `dataDisks` artık VHD için belirli bir URI bakın.
- Ek özellikler belirtmeden dağıtırken, disk VM boyutuna dayalı bir depolama türü kullanır. Örneğin, premium depolama alanını destekleyen bir VM boyutu kullanıyorsanız (Standard_D2s_v3 gibi adlarında "s" boyutları) premium diskler varsayılan olarak yapılandırılır. Bir depolama türü belirtmek için diskin sku ayarını kullanarak bunu değiştirebilirsiniz.
- Disk için ad belirtilmemişse, işletim `<VMName>_OsDisk_1_<randomstring>` sistemi diski `<VMName>_disk<#>_<randomstring>` ve her veri diski için biçimini alır.
  - Özel bir görüntüden Bir VM oluşturuluyorsa, depolama hesabı türü ve disk adı için varsayılan ayarlar özel görüntü kaynağında tanımlanan disk özelliklerinden alınır. Bunlar, şablondaki değerler belirtilerek geçersiz kılınabilir.
- Varsayılan olarak, Azure disk şifrelemesi devre dışı bırakılır.
- Varsayılan olarak, disk önbelleğe alma işletim sistemi diski için Oku/Yaz ve veri diskleri için Yok'dur.
- Aşağıdaki örnekte hala bir depolama hesabı bağımlılığı vardır, ancak bu yalnızca tanılama depolama içindir ve disk depolama için gerekli değildir.

```json
{
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2018-10-01",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a>Üst düzey yönetilen disk kaynağını kullanma

Sanal makine nesnesinde disk yapılandırmasını belirtmeye alternatif olarak, üst düzey bir disk kaynağı oluşturabilir ve sanal makine oluşturmanın bir parçası olarak ekleyebilirsiniz. Örneğin, veri diski olarak kullanmak üzere aşağıdaki gibi bir disk kaynağı oluşturabilirsiniz.

```json
{
    "type": "Microsoft.Compute/disks",
    "apiVersion": "2018-06-01",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

VM nesnesi içinde, eklenecek disk nesnesine başvurun. `managedDisk` Özellikte oluşturulan yönetilen diskin kaynak kimliğinin belirtilmesi, VM oluşturulurken diskin ekine izin verir. VM kaynağı `apiVersion` için `2017-03-30`ayarlanır. VM oluşturmadan önce başarıyla oluşturulduğundan emin olmak için disk kaynağına bağımlılık eklenir. 

```json
{
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2018-10-01",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>Yönetilen diskleri kullanarak VM'lerle yönetilen kullanılabilirlik kümeleri oluşturma

Yönetilen diskleri kullanarak VM'lerle yönetilen kullanılabilirlik kümeleri oluşturmak için nesneyi `sku` kullanılabilirlik kümesi kaynağına ekleyin ve `name` özelliği `Aligned`. Bu özellik, tek bir hata noktasını önlemek için her VM için disklerin birbirinden yeterince izole edilmesini sağlar. Ayrıca kullanılabilirlik `apiVersion` kümesi kaynağı için ayarlanır `2018-10-01`unutmayın.

```json
{
    "type": "Microsoft.Compute/availabilitySets",
    "apiVersion": "2018-10-01",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="standard-ssd-disks"></a>Standart SSD diskler

Standart SSD Diskleri oluşturmak için Kaynak Yöneticisi şablonunda gerekli parametreler aşağıda verilmiştir:

* Microsoft.Compute için *apiVersion* olarak `2018-04-01` ayarlanmalıdır (veya daha sonra)
* *managedDisk.storageAccountType'ı*`StandardSSD_LRS`

Aşağıdaki örnekte, Standart SSD Diskler kullanan bir VM için *properties.storageProfile.osDisk* bölümü gösterilmektedir:

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

Şablonlu bir Standart SSD diskinin nasıl oluşturulaca açık tam bir şablon örneği için [bkz.](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/)

### <a name="additional-scenarios-and-customizations"></a>Ek senaryolar ve özelleştirmeler

REST API belirtimleri hakkında tam bilgi bulmak için, lütfen [yönetilen bir disk REST API dokümantasyon oluşturun](/rest/api/manageddisks/disks/disks-create-or-update)gözden geçirin. Şablon dağıtımları aracılığıyla API'ye gönderilebilen varsayılan ve kabul edilebilir değerlerin yanı sıra ek senaryolar da bulacaksınız. 

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen diskleri kullanan tam şablonlar için aşağıdaki Azure Hızlı Başlatma Repo bağlantılarını ziyaret edin.
    * [Yönetilen diskli Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [Yönetilen diskile Linux VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
* Yönetilen [diskler](../articles/virtual-machines/windows/managed-disks-overview.md) hakkında daha fazla bilgi edinmek için Azure Yönetilen Disklere Genel Bakış belgesini ziyaret edin.
* [Microsoft.Compute/virtualMachines şablon başvuru](/azure/templates/microsoft.compute/virtualmachines) belgesini ziyaret ederek sanal makine kaynakları için şablon başvuru belgelerini gözden geçirin.
* [Microsoft.Compute/disks şablonu başvuru](/azure/templates/microsoft.compute/disks) belgesini ziyaret ederek disk kaynakları için şablon başvuru belgelerini gözden geçirin.
* Azure sanal makine ölçek kümelerinde yönetilen disklerin nasıl kullanılacağı hakkında bilgi için [ölçek kümeleri belgesiyle veri disklerini kullan'ı](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-attached-disks) ziyaret edin.
