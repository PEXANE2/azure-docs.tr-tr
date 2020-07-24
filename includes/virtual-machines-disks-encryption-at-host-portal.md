---
title: include dosyası
description: include dosyası
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 07/07/2020
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: c5ea31217bd3088ec123281b36f8578b08ea25b2
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87136335"
---
## <a name="restrictions"></a>Kısıtlamalar

[!INCLUDE [virtual-machines-disks-encryption-at-host-restrictions](virtual-machines-disks-encryption-at-host-restrictions.md)]

### <a name="supported-regions"></a>Desteklenen bölgeler

[!INCLUDE [virtual-machines-disks-encryption-at-host-regions](virtual-machines-disks-encryption-at-host-regions.md)]

### <a name="supported-vm-sizes"></a>Desteklenen VM boyutları

[!INCLUDE [virtual-machines-disks-encryption-at-host-suported-sizes](virtual-machines-disks-encryption-at-host-suported-sizes.md)]

## <a name="prerequisites"></a>Önkoşullar

VM 'niz veya sanal makine ölçek kümeleriniz için konakta şifrelemeyi kullanabilmeniz için, özelliği aboneliğinizde etkinleştirilmiş olarak almanız gerekir. encryptionAtHost@microsoft adresine e-posta gönderin. abonelik Kimliklerinizle birlikte, özelliği abonelikleriniz için etkin olarak alın.

Azure portal, [belirtilen bağlantıyı](https://aka.ms/diskencryptionupdates)kullanarak oturum açın.

> [!IMPORTANT]
> Azure portal erişmek için, [sağlanmış bağlantıyı](https://aka.ms/diskencryptionupdates) kullanmanız gerekir. Ana bilgisayardaki şifreleme, bağlantıyı kullanmadan Genel Azure portal Şu anda görünür değil.

### <a name="create-an-azure-key-vault-and-disk-encryption-set"></a>Azure Key Vault ve disk şifreleme kümesi oluşturma

Özellik etkinleştirildikten sonra, henüz yapmadıysanız bir Azure Key Vault ve bir disk şifreleme kümesi ayarlamanız gerekir.

[!INCLUDE [virtual-machines-disks-encryption-create-key-vault-portal](virtual-machines-disks-encryption-create-key-vault-portal.md)]

## <a name="deploy-a-vm"></a>VM'yi dağıtma

Konakta şifrelemeyi etkinleştirmek için yeni bir VM dağıtmanız gerekir, bu, mevcut VM 'lerde etkinleştirilemez.

1. **Sanal makineleri** arayın ve **+ Ekle** ' yi seçerek bir VM oluşturun.
1. Yeni bir sanal makine oluşturun, uygun bir bölge ve desteklenen bir VM boyutu seçin.
1. **Temel** dikey penceredeki diğer değerleri istediğiniz gibi doldurup **diskler** dikey penceresine geçin.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-basic-blade.png" alt-text="örnek metin":::

1. **Diskler** dikey penceresinde, **konakta şifreleme**için **Evet** ' i seçin.
1. Diğer seçimleri istediğiniz gibi yapın.

    :::image type="content" source="media/virtual-machines-disks-encryption-at-host-portal/disks-encryption-at-host-disk-blade.png" alt-text="örnek metin":::

1. VM dağıtım işlemini tamamlayın ortamınıza uyan seçimler yapın.

Konakta şifreleme özelliği etkinleştirilmiş bir VM 'yi dağıttıysanız, ana bilgisayar üzerinde şifreleme kullanılarak tüm ilişkili disklerin şifrelenmesi gerekir.