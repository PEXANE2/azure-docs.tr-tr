---
title: Azure CLı kullanarak bir VHD 'yi Azure 'a yükleme
description: Azure CLı kullanarak bir VHD 'yi Azure yönetilen diskine yüklemeyi öğrenin.
services: virtual-machines-linux,storage
author: roygara
ms.author: rogarana
ms.date: 09/20/2019
ms.topic: article
ms.service: virtual-machines-linux
ms.tgt_pltfrm: linux
ms.subservice: disks
ms.openlocfilehash: bc8932a9904a3e4e671edc3e624ff15e7253e1ed
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71326813"
---
# <a name="upload-a-vhd-to-azure-using-azure-cli"></a>Azure CLı kullanarak bir VHD 'yi Azure 'a yükleme

Bu makalede, yerel makinenizden bir Azure yönetilen diskine bir VHD yükleme işleminin nasıl yapılacağı açıklanır. Daha önce, verilerinizi bir depolama hesabına hazırlama ve bu depolama hesabını yönetme dahil olmak üzere daha fazla ilgili bir işlemi izlemeniz gerekiyordu. Artık bir depolama hesabını yönetmeniz veya bir VHD 'yi karşıya yüklemek için içindeki verileri aşamalandırmanız gerekmez. Bunun yerine, boş bir yönetilen disk oluşturur ve doğrudan buna bir VHD yüklersiniz. Bu, şirket içi VM 'Leri Azure 'a yüklemeyi basitleştirir ve bir VHD 'yi 32 TiB 'ye kadar doğrudan büyük bir yönetilen diske yüklemenizi sağlar.

Azure 'da IaaS sanal makineleri için bir yedekleme çözümü sağlıyorsanız, müşteri yedeklemelerini yönetilen disklere geri yüklemek için doğrudan karşıya yükleme kullanmanızı öneririz. Bir VHD 'yi Azure 'a harici bir makineden yüklüyorsanız, hızları yerel bant genişliğine göre değişir. Azure VM kullanıyorsanız, bant genişliğiniz standart HDD 'Ler ile aynı olacaktır.

Şu anda, standart HDD, standart SSD ve Premium SSD tarafından yönetilen diskler için doğrudan karşıya yükleme desteklenir. Henüz Ultra SSD 'Ler için desteklenmez.

## <a name="prerequisites"></a>Önkoşullar

- [AzCopy ile v10 arasındaki 'ın](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy)en son sürümünü indirin.
- [Azure CLI 'Yı yükler](/cli/azure/install-azure-cli).
- Yerel olarak depolanan bir VHD dosyası

## <a name="create-an-empty-managed-disk"></a>Boş bir yönetilen disk oluşturma

VHD 'nizi Azure 'a yüklemek için, bu karşıya yükleme işlemi için özel olarak yapılandırılmış boş bir yönetilen disk oluşturmanız gerekir. Bir tane oluşturmadan önce, bu diskler hakkında bilmeniz gereken bazı ek bilgiler vardır.

Bu tür yönetilen disklerin iki benzersiz durumu vardır:

- ReadToUpload, diskin karşıya yüklemeyi almaya çalıştığı ancak [güvenli erişim imzası](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) (SAS) üretilmediği anlamına gelir.
- ActiveUpload, bu, diskin karşıya yükleme almaya ve SAS üretilmeye hazırlanmasıdır.

Bu durumlardan birinde, yönetilen disk, gerçek disk türünden bağımsız olarak [Standart HDD fiyatlandırmasına](https://azure.microsoft.com/pricing/details/managed-disks/)göre faturalandırılır. Örneğin, bir P10 S10 olarak faturalandırılacaktır. Bu, diski bir VM `revoke-access` 'ye eklemek için gerekli olan yönetilen diskte çağrılana kadar doğru olacaktır.

Karşıya yüklemek üzere boş bir standart HDD oluşturabilmeniz için önce, karşıya yüklemek istediğiniz VHD 'nin dosya boyutunu bayt cinsinden yapmanız gerekir. Bunu sağlamak için `wc -c <yourFileName>.vhd` veya `ls -al <yourFileName>.vhd` kullanabilirsiniz. Bu değer, **--karşıya yükleme-boyut-bayt** parametresi belirtildiğinde kullanılır.

Bir [disk oluşturma](/cli/azure/disk#az-disk-create) cmdlet 'inde **--for-upload** parametresini ve **--upload-size-bytes** parametresini belirterek KARŞıYA yüklemek üzere boş bir standart HDD oluşturun:

```azurecli-interactive
az disk create -n mydiskname -g resourcegroupname -l westus2 --for-upload --upload-size-bytes 34359738880 --sku standard_lrs
```

Premium SSD veya standart SSD yüklemek isterseniz, **standard_lrs** değerini **premium_LRS** veya **standardssd_lrs**ile değiştirin. Ultra SSD henüz desteklenmiyor.

Şimdi karşıya yükleme işlemi için yapılandırılmış boş bir yönetilen disk oluşturdunuz. Bir VHD 'yi diske yüklemek için, bir yazılabilir SAS gerekir, bu sayede karşıya yüklemenizin hedefi olarak başvurabilirsiniz.

Boş yönetilen diskinizin yazılabilir bir SAS oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az disk grant-access -n mydiskname -g resourcegroupname --access-level Write --duration-in-seconds 86400
```

Örnek döndürülen değer:

```
{
  "accessSas": "https://md-impexp-t0rdsfgsdfg4.blob.core.windows.net/w2c3mj0ksfgl/abcd?sv=2017-04-17&sr=b&si=600a9281-d39e-4cc3-91d2-923c4a696537&sig=xXaT6mFgf139ycT87CADyFxb%2BnPXBElYirYRlbnJZbs%3D"
}
```

## <a name="upload-vhd"></a>VHD 'yi karşıya yükle

Boş yönetilen diskiniz için bir SAS olduğuna göre, bunu kullanarak, yükleme Komutunuz için yönetilen diskinizi hedef olarak ayarlayabilirsiniz.

Yerel VHD dosyanızı oluşturduğunuz SAS URI 'sini belirterek yönetilen bir diske yüklemek için AzCopy ile v10 arasındaki kullanın.

Bu karşıya yükleme, eşdeğer [Standart HDD](disks-types.md#standard-hdd)ile aynı aktarım hızına sahiptir. Örneğin, S4 'e karşılık gelen bir boyutunuz varsa, 60 MIB/sn 'ye kadar bir aktarım hızına sahip olursunuz. Ancak, S70 'e karşılık gelen bir boyutunuz varsa, 500 MIB/sn 'ye kadar bir aktarım hızına sahip olursunuz.

```
AzCopy.exe copy "c:\somewhere\mydisk.vhd" "sas-URI" --blob-type PageBlob
```

Yükleme sırasında SAS süresi dolarsa ve henüz çağırmadıysanız `revoke-access` , kullanarak `grant-access`karşıya yüklemeye devam etmek için yeni bir SAS alabilirsiniz.

Karşıya yükleme tamamlandıktan sonra ve diske daha fazla veri yazmanıza gerek kalmadığında, SAS 'yi iptal edin. SAS iptal edildiğinde, yönetilen diskin durumu değişir ve diski bir VM 'ye eklemenize olanak tanır.

```azurecli-interactive
az disk revoke-access -n mydiskname -g resourcegroupname
```

## <a name="next-steps"></a>Sonraki adımlar

Bir VHD 'yi yönetilen diske başarıyla yüklediğinize göre, diski bir sanal makineye iliştirebilir ve kullanmaya başlayabilirsiniz.

Bir sanal makineye nasıl disk iliştirileyeceğinizi öğrenmek için, konusunun makalesindeki makaleye bakın: [LINUX VM 'ye bir disk ekleyin](add-disk.md).
