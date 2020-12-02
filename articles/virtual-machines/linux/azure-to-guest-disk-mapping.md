---
title: Azure disklerini Linux VM Konuk disklerine eşleme
description: Linux VM 'nin Konuk disklerini içeren Azure disklerini belirleme.
author: timbasham
ms.service: virtual-machines-linux
ms.subservice: disks
ms.workload: infrastructure-services
ms.topic: how-to
ms.date: 11/17/2020
ms.author: tibasham
ms.openlocfilehash: 4f0e48bf1c14728c54d4e89f30700017b0420d7d
ms.sourcegitcommit: 84e3db454ad2bccf529dabba518558bd28e2a4e6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96523703"
---
# <a name="how-to-map-azure-disks-to-linux-vm-guest-disks"></a>Azure disklerini Linux VM Konuk disklerine eşleme

Bir sanal makinenin Konuk disklerini geri yükleyen Azure disklerini belirlemeniz gerekebilir. Bazı senaryolarda, disk veya birim boyutunu bağlı Azure disklerinin boyutuyla karşılaştırabilirsiniz. SANAL makineye bağlı aynı boyutta birden çok Azure diskinin bulunduğu senaryolarda, veri disklerinin mantıksal birim numarasını (LUN) kullanmanız gerekir. 

## <a name="what-is-a-lun"></a>LUN nedir?

Mantıksal birim numarası (LUN), belirli bir depolama cihazını tanımlamak için kullanılan bir sayıdır. Her depolama cihazına, sıfırdan başlayan benzersiz bir sayısal tanımlayıcı atanır. Bir cihazın tam yolu, veri yolu numarası, hedef KIMLIK numarası ve mantıksal birim numarası (LUN) ile temsil edilir. 

Örneğin: ***Bus numarası 0, hedef kimliği 0, LUN 3** _

Alıştırmada yalnızca LUN 'yi kullanmanız gerekir.

## <a name="finding-the-lun"></a>LUN bulma

Aşağıda, Linux 'ta bir diskin LUN 'unu bulmak için iki yöntem listelenmektedir.

### <a name="lsscsi"></a>lsscsı

1. VM’ye bağlanma
1. `sudo lsscsi`

Listelenen ilk sütunda LUN bulunur, biçim [Host: Channel: target: _ * LUN * *] olur.

### <a name="listing-block-devices"></a>Blok cihazları listeleme

1. VM’ye bağlanma
1. `sudo ls -l /sys/block/*/device`

Listelenen son sütun LUN 'yi içerir, biçim [Host: Channel: target:**LUN**] olur

## <a name="finding-the-lun-for-the-azure-disks"></a>Azure diskleri için LUN bulma

Azure disk için LUN 'U Azure portal, Azure CLı 'yi kullanarak bulabilirsiniz.

### <a name="finding-an-azure-disks-lun-in-the-azure-portal"></a>Azure portal bir Azure diskinin LUN 'unu bulma

1. Azure portal sanal makinelerinizin listesini göstermek için "sanal makineler" i seçin
1. Sanal makineyi seçin
1. "Diskler" i seçin
1. Eklenen diskler listesinden bir veri diski seçin.
1. Diskin LUN 'U, disk ayrıntısı bölmesinde görüntülenir. Burada görüntülenen LUN, lsscsı kullanarak Konukta bakmış olduğunuz LUN 'Lara ve blok cihazlarını listelemesine göre bağıntılı.

### <a name="finding-an-azure-disks-lun-using-azure-cli"></a>Azure CLı kullanarak bir Azure diskinin LUN 'unu bulma

```azurecli-interactive
az vm show -g myResourceGroup -n myVM --query "storageProfile.dataDisks"
```
