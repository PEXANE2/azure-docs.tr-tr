---
title: Linux için Azure Network Watcher Agent sanal makine uzantısı
description: Sanal makine uzantısı nı kullanarak Network Watcher Agent'ı Linux sanal makinesine dağıtın.
services: virtual-machines-linux
documentationcenter: ''
author: mimckitt
manager: vashan
editor: ''
tags: azure-resource-manager
ms.assetid: 5c81e94c-e127-4dd2-ae83-a236c4512345
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/14/2017
ms.author: mimckitt
ms.openlocfilehash: 476241ad3d4077f2da0c513e3c989218701232ba
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81255762"
---
# <a name="network-watcher-agent-virtual-machine-extension-for-linux"></a>Linux için Ağ İzleyicisi Aracısı sanal makine uzantısı

## <a name="overview"></a>Genel Bakış

[Azure Ağ İzleyicisi,](/azure/network-watcher/) Azure ağlarıiçin izleme sağlayan bir ağ performansı izleme, tanılama ve analiz hizmetidir. Ağ İzleyicisi sanal makine (VM) uzantısı, Azure VM'lerde ağ trafiğini isteğe bağlı olarak yakalama ve diğer gelişmiş işlevsellik gibi bazı Ağ İzleyicisi özellikleri için bir gerekliliktir.

Bu makalede, Linux için Network Watcher Agent VM uzantısı için desteklenen platformlar ve dağıtım seçenekleri ayrıntılı olarak belirtilmiştir. Aracının yüklenmesi VM'nin yeniden başlatılmasını bozmaz veya yeniden başlatmayı gerektirmez. Uzantıyı dağıttığınız sanal makinelere dağıtabilirsiniz. Sanal makine bir Azure hizmeti tarafından dağıtılırsa, sanal makineye uzantı yüklemeye izin verip vermediğini belirlemek için hizmetiçin belgeleri denetleyin.

## <a name="prerequisites"></a>Ön koşullar

### <a name="operating-system"></a>İşletim sistemi

Network Watcher Agent uzantısı aşağıdaki Linux dağıtımları için yapılandırılabilir:

| Dağıtım | Sürüm |
|---|---|
| Ubuntu | 12 |
| Debian | 7 ve 8 |
| Red Hat | 6 ve 7 |
| Oracle Linux | 6.8+ ve 7 |
| SUSE Linux Enterprise Server | 11 ve 12 |
| OpenSUSE Sıçraması | 42.3+ |
| CentOS | 6.5+ ve 7 |
| CoreOS | 899.17.0+ |


### <a name="internet-connectivity"></a>İnternet bağlantısı

Ağ İzleyicisi Aracısı işlevlerinden bazıları, Bir VM'nin Internet'e bağlı olmasını gerektirir. Giden bağlantılar kurma özelliği olmadan, Ağ İzleyicisi Temsilcisi özelliklerinden bazıları arızalanabilir veya kullanılamayabilir. Aracıgerektiren Ağ İzleyicisi işlevi hakkında daha fazla bilgi için[Ağ İzleyicisi belgelerine](/azure/network-watcher/)bakın.

## <a name="extension-schema"></a>Uzantı şeması

Aşağıdaki JSON Ağ İzleyicisi Uzantısı için şema gösterir. Uzantı, kullanıcı tarafından sağlanan ayarları gerektirmez veya desteklemez. Uzantı varsayılan yapılandırmasına dayanır.

```json
{
    "type": "extensions",
    "name": "Microsoft.Azure.NetworkWatcher",
    "apiVersion": "[variables('apiVersion')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "properties": {
        "publisher": "Microsoft.Azure.NetworkWatcher",
        "type": "NetworkWatcherAgentLinux",
        "typeHandlerVersion": "1.4",
        "autoUpgradeMinorVersion": true
    }
}
```

### <a name="property-values"></a>Özellik değerleri

| Adı | Değer / Örnek |
| ---- | ---- |
| apiVersion | 2015-06-15 |
| yayımcı | Microsoft.Azure.NetworkWatcher |
| type | Ağ WatcherAgentLinux |
| typeHandlerVersion | 1.4 |

## <a name="template-deployment"></a>Şablon dağıtımı

Azure Kaynak Yöneticisi şablonuyla Azure VM uzantılarını dağıtabilirsiniz. Ağ İzleyici Aracısı uzantısını dağıtmak için şablonunuzda önceki json şemasını kullanın.

## <a name="azure-classic-cli-deployment"></a>Azure klasik CLI dağıtımı

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Aşağıdaki örnek, Network Watcher Agent VM uzantısını klasik dağıtım modeli aracılığıyla dağıtılan varolan bir VM'ye dağıtur:

```console
azure config mode asm
azure vm extension set myVM1 NetworkWatcherAgentLinux Microsoft.Azure.NetworkWatcher 1.4
```

## <a name="azure-cli-deployment"></a>Azure CLI dağıtımı

Aşağıdaki örnek, Ağ İzleyicisi VM uzantısını Kaynak Yöneticisi aracılığıyla dağıtılan varolan bir VM'ye dağıtır:

```azurecli
az vm extension set --resource-group myResourceGroup1 --vm-name myVM1 --name NetworkWatcherAgentLinux --publisher Microsoft.Azure.NetworkWatcher --version 1.4
```

## <a name="troubleshooting-and-support"></a>Sorun giderme ve destek

### <a name="troubleshooting"></a>Sorun giderme

Azure portalı veya Azure CLI'yi kullanarak uzantı dağıtımlarının durumu yla ilgili verileri alabilirsiniz.

Aşağıdaki örnek, Azure CLI'yi kullanarak Kaynak Yöneticisi aracılığıyla dağıtılan bir VM için NetworkWatcherAgentLinux uzantısının dağıtım durumunu gösterir:

```azurecli
az vm extension show --name NetworkWatcherAgentLinux --resource-group myResourceGroup1 --vm-name myVM1
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [Ağ İzleyicisi belgelerine](/azure/network-watcher/)başvurabilir veya [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarına başvurabilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek **al'ı**seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'si'ne](https://azure.microsoft.com/support/faq/)bakın.
