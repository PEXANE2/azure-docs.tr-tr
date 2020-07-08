---
title: Sanal makine ölçek kümeleriyle Istenen durum yapılandırmasını kullanma
description: Sanal makineleri yapılandırmak için, Azure Istenen durum yapılandırması Uzantısı ile sanal makine ölçek kümelerini kullanma.
author: ju-shim
ms.author: jushiman
ms.topic: how-to
ms.service: virtual-machine-scale-sets
ms.subservice: extensions
ms.date: 6/25/2020
ms.reviewer: mimckitt
ms.custom: mimckitt
ms.openlocfilehash: 6939339ce8d0f324fbd9e14ba31d23a6febcfcbe
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85373906"
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Azure DSC Uzantısı ile sanal makine ölçek kümelerini kullanma
[Sanal Makine Ölçek Kümeleri](virtual-machine-scale-sets-overview.md) , [Azure istenen durum yapılandırması (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) uzantı işleyicisiyle birlikte kullanılabilir. Sanal Makine Ölçek Kümeleri, büyük miktarlarda sanal makine dağıtmak ve yönetmek için bir yol sağlar ve yükleme yanıtı olarak esnek ve out edebilir. DSC, üretim yazılımını çalıştırdığından VM 'Leri çevrimiçi olarak yapılandırmak için kullanılır.

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a>Sanal makinelere ve sanal makine ölçek kümelerine dağıtma arasındaki farklar
Bir sanal makine ölçek kümesi için temel alınan şablon yapısı, tek bir VM 'den biraz farklıdır. Özellikle, tek bir VM "virtualMachines" düğümü altında uzantıları dağıtır. DSC 'nin şablona eklendiği "Uzantılar" türünde bir giriş vardır

```
"resources": [
          {
              "name": "Microsoft.Powershell.DSC",
              "type": "extensions",
              "location": "[resourceGroup().location]",
              "apiVersion": "2015-06-15",
              "dependsOn": [
                  "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
              ],
              "tags": {
                  "displayName": "dscExtension"
              },
              "properties": {
                  "publisher": "Microsoft.Powershell",
                  "type": "DSC",
                  "typeHandlerVersion": "2.20",
                  "autoUpgradeMinorVersion": false,
                  "forceUpdateTag": "[parameters('dscExtensionUpdateTagVersion')]",
                  "settings": {
                      "configuration": {
                          "url": "[concat(parameters('_artifactsLocation'), '/', variables('dscExtensionArchiveFolder'), '/', variables('dscExtensionArchiveFileName'))]",
                          "script": "DscExtension.ps1",
                          "function": "Main"
                      },
                      "configurationArguments": {
                          "nodeName": "[variables('vmName')]"
                      }
                  },
                  "protectedSettings": {
                      "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                  }
              }
          }
      ]
```

Bir sanal makine ölçek kümesi düğümünde "VirtualMachineProfile", "extensionProfile" özniteliği içeren bir "Özellikler" bölümü vardır. DSC "Uzantılar" altına eklenir

```
"extensionProfile": {
            "extensions": [
                {
                    "name": "Microsoft.Powershell.DSC",
                    "properties": {
                        "publisher": "Microsoft.Powershell",
                        "type": "DSC",
                        "typeHandlerVersion": "2.20",
                        "autoUpgradeMinorVersion": false,
                        "forceUpdateTag": "[parameters('DscExtensionUpdateTagVersion')]",
                        "settings": {
                            "configuration": {
                                "url": "[concat(parameters('_artifactsLocation'), '/', variables('DscExtensionArchiveFolder'), '/', variables('DscExtensionArchiveFileName'))]",
                                "script": "DscExtension.ps1",
                                "function": "Main"
                            },
                            "configurationArguments": {
                                "nodeName": "localhost"
                            }
                        },
                        "protectedSettings": {
                            "configurationUrlSasToken": "[parameters('_artifactsLocationSasToken')]"
                        }
                    }
                }
            ]
```

## <a name="behavior-for-a-virtual-machine-scale-set"></a>Bir sanal makine ölçek kümesi davranışı
Bir sanal makine ölçek kümesi davranışı, tek bir VM 'nin davranışıyla aynıdır. Yeni bir VM oluşturulduğunda, DSC uzantısıyla otomatik olarak sağlanır. Uzantı için WMF 'in daha yeni bir sürümü gerekliyse, sanal makine çevrimiçi duruma gelmeden önce yeniden başlatılır. Bu, çevrimiçi olduktan sonra DSC Configuration. zip ' i indirir ve VM 'de temin edilir. Daha fazla ayrıntı [Için Azure DSC uzantısına genel bakış](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)'da ulaşabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[DSC uzantısının Azure Resource Manager şablonunu](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)inceleyin.

[DSC uzantısının kimlik bilgilerini güvenli bir şekilde nasıl işlediğini](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)öğrenin. 

Azure DSC uzantı işleyicisi hakkında daha fazla bilgi için bkz. [Azure Istenen durum yapılandırması uzantı Işleyicisine giriş](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

PowerShell DSC hakkında daha fazla bilgi için [PowerShell belge merkezini ziyaret edin](/powershell/scripting/dsc/overview/overview). 

