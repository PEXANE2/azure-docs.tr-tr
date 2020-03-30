---
title: Linux Sanal Makineleri Azure'da yeniden dağıtın | Microsoft Dokümanlar
description: SSH bağlantı sorunlarını azaltmak için Azure'da Linux sanal makinelerini yeniden dağıtma.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: genlin
manager: dcscontentpm
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 18e96f9463176b0fce04252492eea6dbede416c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79531116"
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a>Linux sanal makinesini yeni bir Azure düğümüne yeniden dağıtma
Azure'daki bir Linux sanal makinesine (VM) SSH veya uygulama erişiminde sorun gidermede güçlük yaşıyorsanız, VM'yi yeniden dağıtmak yardımcı olabilir. Bir VM'yi yeniden dağıttığınızda, VM'yi Azure altyapısı içinde yeni bir düğüme taşır ve ardından yeniden güç kullanır. Tüm yapılandırma seçenekleriniz ve ilişkili kaynaklarınız korunur. Bu makalede, Azure CLI veya Azure portalı kullanarak bir VM'yi nasıl yeniden dağıtabileceğiniz gösterilmektedir.

> [!NOTE]
> Bir VM'yi yeniden dağıttıktan sonra geçici disk kaybolur ve sanal ağ arabirimiyle ilişkili dinamik IP adresleri güncelleştirilir. 


## <a name="use-the-azure-cli"></a>Azure CLI kullanma
En son [Azure CLI'yi](/cli/azure/install-az-cli2) yükleyin ve az giriş 'i kullanarak Azure hesabınızda oturum [açın.](/cli/azure/reference-index)

[Az vm yeniden dağıtın](/cli/azure/vm)vM yeniden dağıtın. Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myVM* adlı VM'yi yeniden dağıtır:

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-classic-cli"></a>Azure klasik CLI'yi kullanma

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]


En [son Azure klasik CLI'sini](../../cli-install-nodejs.md) yükleyin ve Azure hesabınızda oturum açın. Kaynak Yöneticisi modunda olduğundan emin`azure config mode arm`olun ( ).

Aşağıdaki örnek, *myResourceGroup*adlı kaynak grubunda *myVM* adlı VM'yi yeniden dağıtır:

```console
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a>Sonraki adımlar
VM'nize bağlanmada sorun yaşıyorsanız, [Sorun giderme SSH bağlantıları](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) veya [ayrıntılı SSH sorun giderme adımlarında](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)özel yardım bulabilirsiniz. VM'nizde çalışan bir uygulamaya erişemiyorsanız, [uygulama sorun giderme sorunlarını](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)da okuyabilirsiniz.


