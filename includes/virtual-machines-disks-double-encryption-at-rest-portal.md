---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/08/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: f96bc753d14d53584aec6d9661e52adcd21ab764
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87136298"
---
## <a name="supported-regions"></a>Desteklenen bölgeler

[!INCLUDE [virtual-machines-disks-double-encryption-at-rest-regions](virtual-machines-disks-double-encryption-at-rest-regions.md)]

## <a name="getting-started"></a>Başlarken

1. [Azure portalında](https://aka.ms/diskencryptionupdates) oturum açın.

    > [!IMPORTANT]
    > Azure portal erişmek için, [sağlanmış bağlantıyı](https://aka.ms/diskencryptionupdates) kullanmanız gerekir. Rest 'te Çift şifreleme, bağlantıyı kullanmadan Genel Azure portal Şu anda görünür değil.

1. **Disk şifreleme kümelerini**arayın ve seçin.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-disk-encryption-sets-search.png" alt-text="örnek metin":::

1. **+ Ekle**'yi seçin.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-add-disk-encryption-set.png" alt-text="örnek metin":::

1. Desteklenen bölgelerden birini seçin.
1. **Şifreleme türü**için, **platform tarafından yönetilen ve müşteri tarafından yönetilen anahtarlarla çift şifrelemeyi**seçin.

    > [!NOTE]
    > Belirli bir şifreleme türüyle bir disk şifreleme kümesi oluşturduktan sonra, bu değişiklik değiştirilemez. Farklı bir şifreleme türü kullanmak istiyorsanız, yeni bir disk şifreleme kümesi oluşturmanız gerekir.

1. Kalan bilgileri girin.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-create-disk-encryption-set-blade.png" alt-text="örnek metin":::

1. Bir Azure Key Vault ve anahtar seçin ya da gerekirse yeni bir tane oluşturun.

    > [!NOTE]
    > Key Vault bir örnek oluşturursanız, geçici silme ve Temizleme korumasını etkinleştirmeniz gerekir. Bu ayarlar, yönetilen diskleri şifrelemek için bir Key Vault kullanılırken zorunludur ve yanlışlıkla silme nedeniyle veri kaybını korumanıza karşı koruma sağlar.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-select-key-vault.png" alt-text="örnek metin":::

1. **Oluştur**’u seçin.
1. Oluşturduğunuz disk şifreleme kümesine gidin ve görüntülenen hatayı seçin. Bu işlem, disk şifreleme kümesini çalışacak şekilde yapılandırır.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-disk-set-error.png" alt-text="örnek metin":::

    Bir bildirim açılır ve başarılı olmalıdır. Bunun yapılması, anahtar kasanızın bulunduğu disk şifreleme kümesini kullanmanıza olanak sağlar.
    
    ![Anahtar kasanızın başarılı izninin ve rol atamasının ekran görüntüsü.](media/virtual-machines-disk-encryption-portal/disk-encryption-notification-success.png)

1. Diskinize gidin.
1. **Şifrelemeyi**seçin.
1. **Şifreleme türü**için, **platform tarafından yönetilen ve müşteri tarafından yönetilen anahtarlarla çift şifrelemeyi**seçin.
1. Disk şifreleme kümesini seçin.
1. **Kaydet**' i seçin.

    :::image type="content" source="media/virtual-machines-disks-double-encryption-at-rest-portal/double-encryption-enable-disk-blade.png" alt-text="örnek metin":::

Artık yönetilen diskinizde bekleyen ' de çift şifrelemeyi etkinleştirdiniz.
