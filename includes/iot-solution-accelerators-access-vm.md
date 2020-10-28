---
title: include dosyası
description: include dosyası
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 817c41a969f03ad04d372c516a16ef6b770f3e18
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755324"
---
## <a name="access-the-virtual-machine"></a>Sanal makineye erişin

Aşağıdaki adımlarda Azure Cloud Shell ' de Azure CLı kullanılır. Tercih ederseniz, [Azure CLI](/cli/azure/install-azure-cli) 'yi geliştirme makinenize yükleyebilir ve komutları yerel olarak çalıştırabilirsiniz.

Aşağıdaki adımlarda, Azure sanal makinesinin **SSH** erişimine izin verecek şekilde nasıl yapılandırılacağı gösterilmektedir. Gösterilen adımlarda, Çözüm Hızlandırıcısı için seçtiğiniz adın **contoso-simülasyon** olduğunu varsaymaktadır. bu değeri dağıtımınızın adıyla değiştirin:

1. Çözüm Hızlandırıcısı kaynaklarını içeren kaynak grubunun içeriğini listeleyin:

    ```azurecli-interactive
    az resource list -g contoso-simulation -o table
    ```

    Sanal makinenin, genel IP adresinin ve ağ güvenlik grubunun adını, daha sonra bu değerlere ihtiyacınız olduğunu unutmayın.

1. Ağ güvenlik grubunu SSH erişimine izin verecek şekilde güncelleştirin. Aşağıdaki komut, ağ güvenlik grubunun adının **contoso-simülasyon-NSG** olduğunu varsayar; bu değeri ağ güvenlik grubunuzun adıyla değiştirin:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name contoso-simulation-nsg -g contoso-simulation --access Allow -o table
    ```

    Yalnızca test ve geliştirme sırasında SSH erişimini etkinleştirin. SSH 'yi etkinleştirirseniz, [mümkün olan en kısa sürede yeniden devre dışı bırakmanız gerekir](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices#disable-rdpssh-access-to-virtual-machines).

1. Sanal makinede **azureuser** hesabının parolasını bildiğiniz bir parolayla güncelleştirin. Aşağıdaki komutu çalıştırdığınızda kendi parolanızı seçin:

    ```azurecli-interactive
    az vm user update --name vm-vikxv --username azureuser --password YOURSECRETPASSWORD  -g contoso-simulation
    ```

1. Sanal makinenizin genel IP adresini bulun. Aşağıdaki komut, sanal makinenin adının **VM-vikxv** olduğunu varsayar; bu değeri, daha önce bir örneği yaptığınız sanal makinenin adıyla değiştirin:

    ```azurecli-interactive
    az vm list-ip-addresses --name vm-vikxv -g contoso-simulation -o table
    ```

    Sanal makinenizin genel IP adresini bir yere getirin.
