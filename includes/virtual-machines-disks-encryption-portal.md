---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/23/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 950fd82e14902c3a7a94fddb44d50ac372ebe119
ms.sourcegitcommit: d7bd8f23ff51244636e31240dc7e689f138c31f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87177068"
---
[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-portal](virtual-machines-disks-encryption-create-key-vault-portal.md)]

## <a name="deploy-a-vm"></a>VM'yi dağıtma

Anahtar Kasanızı ve disk şifreleme kümesini oluşturup ayarladığınıza göre, şifrelemeyi kullanarak bir VM dağıtabilirsiniz.
VM dağıtım işlemi standart dağıtım işlemine benzer, tek fark, VM 'yi diğer kaynaklarınızla aynı bölgeye dağıtmanız ve müşteri tarafından yönetilen anahtar kullanmayı tercih etmeniz gerekir.

1. **Sanal makineleri** arayın ve **+ Ekle** ' yi seçerek bir VM oluşturun.
1. **Temel** dikey pencerede, disk şifreleme kümesi ve Azure Key Vault aynı bölgeyi seçin.
1. **Temel** dikey penceredeki diğer değerleri istediğiniz şekilde girin.

    ![Bölge değeri vurgulanmış şekilde VM oluşturma deneyiminin ekran görüntüsü.](media/virtual-machines-disk-encryption-portal/server-side-encryption-create-a-vm-region.png)

1. **Diskler** dikey penceresinde, **müşteri tarafından yönetilen bir anahtarla bekleyen şifreleme**' yi seçin.
1. Disk **şifreleme kümesi** açılır penceresinde disk şifreleme kümesini seçin.
1. Diğer seçimleri istediğiniz gibi yapın.

    ![VM oluşturma deneyiminin, diskler dikey penceresinin ekran görüntüsü. Disk şifreleme kümesi açılır ve vurgulanmış olarak.](media/virtual-machines-disk-encryption-portal/server-side-encryption-create-vm-select-customer-managed-key-disk-encryption-set.png)

## <a name="enable-on-an-existing-disk"></a>Mevcut bir diskte etkinleştir

> [!CAUTION]
> Bir VM 'ye bağlı herhangi bir diskte disk şifrelemeyi etkinleştirmek, VM 'yi durdurmanız gerekir.
    
1. Disk şifreleme kümelerinizin biriyle aynı bölgede bulunan bir VM 'ye gidin.
1. VM 'yi açın ve **Durdur**' u seçin.

    ![Örnek sanal makinenizin ana kaplamanın, durdur düğmesi vurgulanmış şekilde ekran görüntüsü.](media/virtual-machines-disk-encryption-portal/server-side-encryption-stop-vm-to-encrypt-disk-fix.png)

1. VM 'yi durdurmayı tamamladıktan sonra, **diskler** ' i seçin ve ardından şifrelemek istediğiniz diski seçin.

    ![Örnek sanal makinenizin, diskler dikey penceresi açık olan ekran görüntüsü. İşletim sistemi diski, seçmeniz için örnek bir disk olarak vurgulanır.](media/virtual-machines-disk-encryption-portal/server-side-encryption-existing-disk-select.png)

1. **Şifreleme** ' yi seçin ve ardından **müşteri tarafından yönetilen bir anahtarla geri kalan şifrelemeyi** seçin ve ardından açılan listede disk şifreleme kümesini seçin.
1. **Kaydet**'i seçin.

    ![Örnek işletim sistemi diskinizin ekran görüntüsü. Şifreleme dikey penceresi açıktır, müşteri tarafından yönetilen bir anahtarla bekleyen şifreleme seçilidir ve ayrıca Azure Key Vault. Bu seçimleri yaptıktan sonra Kaydet düğmesi seçilidir.](media/virtual-machines-disk-encryption-portal/server-side-encryption-encrypt-existing-disk-customer-managed-key.png)

1. Şifrelemek istediğiniz sanal makineye bağlı diğer diskler için bu işlemi tekrarlayın.
1. Disklerinizde, müşteri tarafından yönetilen anahtarlara geçiş tamamlandığında, şifrelemek istediğiniz başka bir bağlı disk yoksa VM 'nizi başlatabilirsiniz.
