---
title: Sanal Makine Ölçek Setleri ile İstenilen Durum Yapılandırması Kullanma
description: Sanal makineleri yapılandırmak için Azure İstenilen Durum Yapılandırma Uzantısı ile Sanal Makine Ölçeği Kümelerini kullanma.
author: zjalexander
tags: azure-service-management,azure-resource-manager
ms.assetid: c8f047b5-0e6c-4ef3-8a47-f1b284d32942
ms.service: virtual-machine-scale-sets
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows
ms.date: 04/05/2017
ms.author: zachal
ms.openlocfilehash: a93a8a9c27be5a1736a50e6c4c4b830980b7d974
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76278065"
---
# <a name="using-virtual-machine-scale-sets-with-the-azure-dsc-extension"></a>Azure DSC Uzantısı ile Sanal Makine Ölçek Kümelerini Kullanma
[Sanal Makine Ölçek Setleri,](virtual-machine-scale-sets-overview.md) [Azure İstenilen Durum Yapılandırması (DSC)](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) uzantılı işleyicisi ile kullanılabilir. Sanal makine ölçek kümeleri, çok sayıda sanal makineyi dağıtmak ve yönetmek için bir yol sağlar ve yüke yanıt olarak elastik olarak ölçeklenebilir. DSC, çevrimiçi olduklarında VM'leri yapılandırmak için kullanılır, böylece üretim yazılımını çalıştırMaktadırlar.

## <a name="differences-between-deploying-to-virtual-machines-and-virtual-machine-scale-sets"></a>Sanal Makinelere dağıtım ile Sanal Makine Ölçek Kümeleri arasındaki farklar
Sanal makine ölçek kümesinin temel şablon yapısı tek bir VM'den biraz farklıdır. Özellikle, tek bir VM uzantıları "virtualMachines" düğümü altında dağıtır. Şablona DSC'nin eklendiği "uzantılar" türünde bir giriş vardır

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

Sanal makine ölçeği kümesi düğümünde "VirtualMachineProfile", "extensionProfile" özniteliği olan bir "özellikler" bölümü vardır. DSC "uzantıları" altında eklenir

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

## <a name="behavior-for-a-virtual-machine-scale-set"></a>Sanal Makine Ölçeği Kümesi Davranışı
Sanal makine ölçeği kümesinin davranışı, tek bir VM'nin davranışıyla aynıdır. Yeni bir VM oluşturulduğunda, otomatik olarak DSC uzantısı ile birlikte verilir. WMF'nin daha yeni bir sürümü uzantı tarafından gerekliyse, VM çevrimiçi olmadan önce yeniden başlatılır. Bir kez çevrimiçi, dsc yapılandırma .zip indirir ve VM üzerinde hükmü. Azure [DSC Uzantısı Genel Bakış'ta](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)daha fazla ayrıntı bulabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[DSC uzantısı için Azure Kaynak Yöneticisi şablonuna](../virtual-machines/windows/extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)göz atın.

[DSC uzantısının kimlik bilgilerini güvenli bir şekilde](../virtual-machines/windows/extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)nasıl işleyeceğini öğrenin. 

Azure DSC uzantı işleyicisi hakkında daha fazla bilgi için [bkz.](../virtual-machines/windows/extensions-dsc-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) 

PowerShell DSC hakkında daha fazla bilgi için [PowerShell dokümantasyon merkezini ziyaret edin.](/powershell/scripting/dsc/overview/overview) 

