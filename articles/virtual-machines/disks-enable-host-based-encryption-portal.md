---
title: Konak Azure portal yönetilen disklerde şifrelemeyi kullanarak uçtan uca şifrelemeyi etkinleştirme
description: Azure yönetilen disklerinizde (Azure portal uçtan uca şifrelemeyi etkinleştirmek için konaktaki şifrelemeyi kullanın.
author: roygara
ms.service: virtual-machines
ms.topic: how-to
ms.date: 08/24/2020
ms.author: rogarana
ms.subservice: disks
ms.custom: references_regions
ms.openlocfilehash: ba7d6d8deb2034f8b2a853cf74635687561c41ea
ms.sourcegitcommit: 1f1d29378424057338b246af1975643c2875e64d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2021
ms.locfileid: "99573611"
---
# <a name="use-the-azure-portal-to-enable-end-to-end-encryption-using-encryption-at-host"></a>Konakta şifrelemeyi kullanarak uçtan uca şifrelemeyi etkinleştirmek için Azure portal kullanma

Konakta şifrelemeyi etkinleştirdiğinizde, VM konağında depolanan veriler, REST ve depolama hizmetine şifrelenen akışlara şifrelenir. Konakta şifreleme ve diğer yönetilen disk şifreleme türleri hakkında kavramsal bilgiler için bkz.:

* Linux: [VM verileriniz için ana bilgisayardan uca şifreleme](./disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).

* Windows: [VM verileriniz için ana bilgisayardan uca şifreleme Ile şifreleme](./disk-encryption.md#encryption-at-host---end-to-end-encryption-for-your-vm-data).

## <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](../../includes/virtual-machines-disks-encryption-at-host-restrictions.md)]

### <a name="supported-regions"></a>Desteklenen bölgeler

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](../../includes/virtual-machines-disks-encryption-at-host-regions.md)]

### <a name="supported-vm-sizes"></a>Desteklenen VM boyutları

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](../../includes/virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="prerequisites"></a>Önkoşullar

VM 'niz veya sanal makine ölçek kümeleriniz için konakta şifrelemeyi kullanabilmeniz için, özelliği aboneliğinizde etkinleştirilmiş olarak almanız gerekir. encryptionAtHost@microsoft.comAbonelikleriniz için etkin özelliği sağlamak üzere abonelik kimliklerinizle e-posta gönderin.

Azure portal, [belirtilen bağlantıyı](https://aka.ms/diskencryptionupdates)kullanarak oturum açın.

> [!IMPORTANT]
> Azure portal erişmek için, [sağlanmış bağlantıyı](https://aka.ms/diskencryptionupdates) kullanmanız gerekir. Ana bilgisayardaki şifreleme, bağlantıyı kullanmadan Genel Azure portal Şu anda görünür değil.

### <a name="create-an-azure-key-vault-and-disk-encryption-set"></a>Azure Key Vault ve disk şifreleme kümesi oluşturma

Özellik etkinleştirildikten sonra, henüz yapmadıysanız bir Azure Key Vault ve bir disk şifreleme kümesi ayarlamanız gerekir.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-portal](../../includes/virtual-machines-disks-encryption-create-key-vault-portal.md)]

## <a name="deploy-a-vm"></a>VM'yi dağıtma

Konakta şifrelemeyi etkinleştirmek için yeni bir VM dağıtmanız gerekir, bu, mevcut VM 'lerde etkinleştirilemez.

1. **Sanal makineleri** arayın ve **+ Ekle** ' yi seçerek bir VM oluşturun.
1. Yeni bir sanal makine oluşturun, uygun bir bölge ve desteklenen bir VM boyutu seçin.
1. **Temel** dikey penceredeki diğer değerleri istediğiniz gibi doldurup **diskler** dikey penceresine geçin.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-basic-blade.png" alt-text="Sanal makine oluşturma temelleri dikey penceresinin ekran görüntüsü, bölge ve V d boyutu vurgulanır.":::

1. **Diskler** dikey penceresinde, **konakta şifreleme** için **Evet** ' i seçin.
1. Diğer seçimleri istediğiniz gibi yapın.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-disk-blade.png" alt-text="Sanal makine oluşturma diskleri dikey penceresinin ekran görüntüsü, konaktaki şifreleme vurgulanır.":::

1. VM dağıtım işlemini tamamlayın ortamınıza uyan seçimler yapın.

Konakta şifreleme özelliği etkinleştirilmiş bir VM 'yi dağıttıysanız, ana bilgisayar üzerinde şifreleme kullanılarak tüm ilişkili disklerin şifrelenmesi gerekir.

## <a name="next-steps"></a>Sonraki adımlar

[Azure Resource Manager şablonu örnekleri](https://github.com/Azure-Samples/managed-disks-powershell-getting-started/tree/master/EncryptionAtHost)
