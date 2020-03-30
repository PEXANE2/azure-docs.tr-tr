---
title: include dosyası
description: include dosyası
services: iot-accelerators
author: dominicbetts
ms.service: iot-accelerators
ms.topic: include
ms.date: 08/16/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: a58e408feadd10e6dbc9d6878b82a4d045918ea6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68781504"
---
## <a name="access-the-virtual-machine"></a>Sanal makineye erişin

Aşağıdaki adımlar, Azure Bulut BulutU'nda Azure CLI'yi kullanır. İsterseniz, Azure [CLI'yi](/cli/azure/install-azure-cli) geliştirme makinenize yükleyebilir ve komutları yerel olarak çalıştırabilirsiniz.

Aşağıdaki adımlar, Azure sanal makinesini **SSH** erişimine izin verecek şekilde nasıl yapılandırabileceğinizi gösterir. Gösterilen adımlar, çözüm hızlandırıcı için seçtiğiniz **adın contoso-simülasyon** olduğunu varsayalım -- bu değeri dağıtımınızın adı ile değiştirin:

1. Çözüm hızlandırıcı kaynaklarını içeren kaynak grubunun içeriğini listele:

    ```azurecli-interactive
    az resource list -g contoso-simulation -o table
    ```

    Sanal makinenin adını, genel IP adresini ve ağ güvenlik grubunu not alın - bu değerlere daha sonra ihtiyacınız var.

1. SSH erişimine izin verecek şekilde ağ güvenlik grubunu güncelleştirin. Aşağıdaki komut, ağ güvenlik grubunun adının **contoso-simulation-nsg** olduğunu varsayar — bu değeri ağ güvenlik grubunuzun adıyla değiştirin:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name contoso-simulation-nsg -g contoso-simulation --access Allow -o table
    ```

    Yalnızca test ve geliştirme sırasında SSH erişimine olanak sağlar. SSH'yi [etkinleştiriseniz, mümkün olan en kısa sürede yeniden devre dışı bırakmanız gerekir.](https://docs.microsoft.com/azure/security/fundamentals/network-best-practices#disable-rdpssh-access-to-virtual-machines)

1. Sanal makinedeki **azureuser** hesabının parolasını bildiğiniz bir parolayla güncelleştirin. Aşağıdaki komutu çalıştırdığınızda kendi parolanızı seçin:

    ```azurecli-interactive
    az vm user update --name vm-vikxv --username azureuser --password YOURSECRETPASSWORD  -g contoso-simulation
    ```

1. Sanal makinenizin genel IP adresini bulun. Aşağıdaki komut sanal makinenin adının **vm-vikxv** olduğunu varsayar -- bu değeri daha önce not aldığınız sanal makinenin adı ile değiştirin:

    ```azurecli-interactive
    az vm list-ip-addresses --name vm-vikxv -g contoso-simulation -o table
    ```

    Sanal makinenizin genel IP adresini not alın.
