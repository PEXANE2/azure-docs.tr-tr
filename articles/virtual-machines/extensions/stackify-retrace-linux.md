---
title: Stackify Retrace Azure Linux Aracı uzantısı
description: Stackify Retrace Linux aracısını bir Linux sanal makinesinde dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: darinhoward
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 04/12/2018
ms.author: akjosh
ms.openlocfilehash: 5914947bd994ee405f253e34c3dd919dd6561898
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79253799"
---
# <a name="stackify-retrace-linux-agent-extension"></a>Stackify Retrace Linux Agent Uzantısı

## <a name="overview"></a>Genel Bakış

Stackify, sorunları hızla bulmanıza ve düzeltmenize yardımcı olmak için uygulamanızla ilgili ayrıntıları izleyen ürünler sağlar. Geliştirici ekipleri için Retrace, tam entegre, çok ortamlı, uygulama performansı süper gücüdür. Her geliştirme ekibinin ihtiyaç duyduğu çeşitli araçları birleştirir.

Retrace, tüm ortamlarda ki tüm özellikleri tek bir platformda sunan TEK araçtır.

* Uygulama performans yönetimi (APM)
* Uygulama ve sunucu günlüğü
* Hata izleme ve izleme
* Sunucu, uygulama ve özel ölçümler

**Stackify Linux Agent Extension Hakkında**

Bu uzantı, Linux Agent for Retrace için bir yükleme yolu sağlar. 

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi 

Retrace aracısı bu Linux dağıtımlarına karşı çalıştırılabilir

| Dağıtım | Sürüm |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS, 16.10 ve 17.04 |
| Debian | 7.9+ ve 8.2+, 9 |
| Red Hat | 6.7+, 7.1+ |
| CentOS | 6.3+, 7.0+ |

### <a name="internet-connectivity"></a>İnternet bağlantısı

Linux için Stackify Agent uzantısı hedef sanal makine internete bağlı olmasını gerektirir. 

Bağlantıların Stackify'ye bağlanmasına izin verecek şekilde https://support.stackify.com/hc/en-us/articles/207891903-Adding-Exceptions-to-a-Firewallağ yapılandırmanızı ayarlamanız gerekebilir, bkz. 


## <a name="extension-schema"></a>Uzantı şeması

---

Aşağıdaki JSON Stackify Retrace Agent uzantısı için şema gösterir. Uzantısı gerektirir `environment` ve `activationKey`.

```json
    {
      "type": "extensions",
      "name": "StackifyExtension",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Stackify.LinuxAgent.Extension",
        "type": "StackifyLinuxAgentExtension",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "environment": "myEnvironment"
        },
        "protectedSettings": {
          "activationKey": "myActivationKey"
        }
      }
    }      
```

## <a name="template-deployment"></a>Şablon dağıtımı 

Azure VM uzantıları Azure Kaynak Yöneticisi şablonlarıyla dağıtılabilir. Önceki bölümde ayrıntılı olarak ayrıntılı olarak kullanılan JSON şeması, Azure Kaynak Yöneticisi şablonu dağıtımı sırasında Stackify Retrace Linux Agent uzantısını çalıştırmak için bir Azure Kaynak Yöneticisi şablonunda kullanılabilir.  

Sanal makine uzantısı için JSON, sanal makine kaynağının içine yerleştirilebilir veya Kaynak Yöneticisi JSON şablonunun köküne veya üst seviyesine yerleştirilebilir. JSON'un yerleşimi kaynak adının ve türünün değerini etkiler. Daha fazla bilgi için bkz.

Aşağıdaki örnekstackify Retrace Linux uzantısı sanal makine kaynağı içinde iç içe olduğunu varsayar. Uzantı kaynağını iç içe yerleştirirken, JSON sanal makinenin "kaynaklar"ına yerleştirilir: [] sanal makinenin nesnesi.

Uzantısı gerektirir `environment` ve `activationKey`.

```json
    {
      "type": "extensions",
      "name": "StackifyExtension",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[resourceId('Microsoft.Compute/virtualMachines',variables('vmName'))]"
      ],
      "properties": {
        "publisher": "Stackify.LinuxAgent.Extension",
        "type": "StackifyLinuxAgentExtension",
        "typeHandlerVersion": "1.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "environment": "myEnvironment"
        },
        "protectedSettings": {
          "activationKey": "myActivationKey"
        }
      }
    }      
```

UzantıJSON'u şablonun köküne yerleştirirken, kaynak adı üst sanal makineye bir başvuru içerir ve tür iç içe yapılan yapılandırmayı yansıtır.

```json
    {
        "type": "Microsoft.Compute/virtualMachines/extensions",
        "name": "<parentVmResource>/StackifyExtension",
        "apiVersion": "[variables('apiVersion')]",
        "location": "[resourceGroup().location]",
        "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
        ],
        "properties": {
            "publisher": "Stackify.LinuxAgent.Extension",
            "type": "StackifyLinuxAgentExtension",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "environment": "myEnvironment"
            },
            "protectedSettings": {
              "activationKey": "myActivationKey"
            }
        }
    }
```


## <a name="powershell-deployment"></a>PowerShell dağıtımı

Komut, `Set-AzVMExtension` Stackify Retrace Linux Agent sanal makine uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir. Komutu çalıştırmadan önce, genel ve özel yapılandırmaların PowerShell karma tablosunda depolanmış olması gerekir.

Uzantısı gerektirir `environment` ve `activationKey`.

```powershell
$PublicSettings = @{"environment" = "myEnvironment"}
$ProtectedSettings = @{"activationKey" = "myActivationKey"}

Set-AzVMExtension -ExtensionName "Stackify.LinuxAgent.Extension" `
    -ResourceGroupName "myResourceGroup" `
    -VMName "myVM" `
    -Publisher "Stackify.LinuxAgent.Extension" `
    -ExtensionType "StackifyLinuxAgentExtension" `
    -TypeHandlerVersion 1.0 `
    -Settings $PublicSettings `
    -ProtectedSettings $ProtectedSettings `
    -Location WestUS `
```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı 

Azure CLI aracı, Stackify Retrace Linux Agent sanal makine uzantısını varolan bir sanal makineye dağıtmak için kullanılabilir.  

Uzantısı gerektirir `environment` ve `activationKey`.

```azurecli
az vm extension set --publisher 'Stackify.LinuxAgent.Extension' --version 1.0 --name 'StackifyLinuxAgentExtension' --protected-settings '{"activationKey":"myActivationKey"}' --settings '{"environment":"myEnvironment"}'  --resource-group 'myResourceGroup' --vm-name 'myVmName'
```

## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="error-codes"></a>Hata kodları

| Hata kodu | Anlamı | Olası eylem |
| :---: | --- | --- |
| 10 | Yükleme Hatası | wget gereklidir |
| 20 | Yükleme Hatası | python gereklidir |
| 30 | Yükleme Hatası | sudo gereklidir |
| 40 | Yükleme Hatası | activationKey gereklidir |
| 51 | Yükleme Hatası | Os dağıtım desteklenmiyor |
| 60 | Yükleme Hatası | çevre gereklidir |
| 70 | Yükleme Hatası | Bilinmiyor |
| 80 | Hatayı Etkinleştir | Servis kurulumu başarısız oldu |
| 90 | Hatayı Etkinleştir | Hizmet başlatma başarısız oldu |
| 100 | Hatayı Devre Dışı | Hizmet Durdurma Başarısız Oldu |
| 110 | Hatayı Devre Dışı | Hizmet Kaldırma Başarısız Oldu |
| 120 | Kaldırma Hatası | Hizmet Durdurma Başarısız Oldu |

Daha fazla yardıma ihtiyacınız varsa Stackify https://support.stackify.comdesteğine '.