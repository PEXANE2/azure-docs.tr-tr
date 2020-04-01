---
title: Azure'a Bir VHD yükleme veya bölgeler arasında disk kopyalama - Azure CLI
description: Azure yönetilen bir diske Nasıl VHD yükleyip, azure cli'yi kullanarak bölgeler arasında yönetilen bir diski doğrudan yükleme yoluyla kopyalamayı öğrenin.
services: virtual-machines,storage
author: roygara
ms.author: rogarana
ms.date: 03/27/2020
ms.topic: article
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: 6813206aebe67e4e6d2afd0d9c78d03f0c20c952
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80420960"
---
# <a name="upload-a-vhd-to-azure-or-copy-a-managed-disk-to-another-region---azure-cli"></a>Azure'a Bir VHD yükleme veya yönetilen bir diski başka bir bölgeye kopyalama - Azure CLI

[!INCLUDE [disks-upload-vhd-to-disk-intro](../../../includes/disks-upload-vhd-to-disk-intro.md)]

## <a name="prerequisites"></a>Ön koşullar

- [AzCopy v10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy)en son sürümünü indirin.
- [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli)
- Şirket içinde bir VHD yüklemeyi planlıyorsanız: [Azure için hazırlanmış](../windows/prepare-for-upload-vhd-image.md)sabit boyutlu bir VHD, yerel olarak depolanır.
- Veya bir kopyalama eylemi gerçekleştirmek istiyorsanız Azure'da yönetilen bir disk.

## <a name="getting-started"></a>Başlarken

Bir GUI üzerinden disk yüklemeyi tercih ederseniz, bunu Azure Depolama Gezgini'ni kullanarak yapabilirsiniz. Ayrıntılar için: [Azure yönetilen diskleri yönetmek için Azure Depolama Gezgini'ni kullanın](disks-use-storage-explorer-managed-disks.md)

VHD'nizi Azure'a yüklemek için, bu yükleme işlemi için yapılandırılan boş bir yönetilen disk oluşturmanız gerekir. Bir tane oluşturmadan önce, bu diskler hakkında bilmeniz gereken bazı ek bilgiler vardır.

Yönetilen disk bu tür iki benzersiz durumları vardır:

- ReadToUpload, diskin yükleme almaya hazır olduğu ancak [güvenli erişim imzası](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) (SAS) oluşturulmadı anlamına gelir.
- ActiveUpload, bu da diskin yükleme almaya hazır olduğu ve SAS'ın oluşturulduğu anlamına gelir.

> [!NOTE]
> Bu durumların herhangi birinde, yönetilen disk, gerçek disk türüne bakılmaksızın [standart HDD fiyatlandırmasında](https://azure.microsoft.com/pricing/details/managed-disks/)faturalandırılır. Örneğin, Bir P10 S10 olarak faturalandırılır. Bu, diski `revoke-access` VM'ye takmak için gerekli olan yönetilen diske çağrılana kadar geçerli olacaktır.

## <a name="create-an-empty-managed-disk"></a>Boş yönetilen disk oluşturma

Yükleme için boş bir standart HDD oluşturabilmeniz için, yüklemek istediğiniz VHD'nin baytlar halindeki dosya boyutuna ihtiyacınız vardır. Bunu elde etmek için, `wc -c <yourFileName>.vhd` `ls -al <yourFileName>.vhd`ya da kullanabilirsiniz . Bu değer **--upload boyutu-bayt** parametresi belirtilirken kullanılır.

Bir [disk oluşturma](/cli/azure/disk#az-disk-create) cmdletinde hem **-yükleme** için parametrehem de **--upload boyutu-bayt** parametresini belirterek yükleme için boş bir standart HDD oluşturun:

`<yourdiskname>`Değiştirin `<yourresourcegroupname>` `<yourregion>` , seçtiğiniz değerlerle. `--upload-size-bytes` Parametre, sizin için `34359738880`uygun bir değerle değiştirin örnek bir değer içerir.

```azurecli
az disk create -n <yourdiskname> -g <yourresourcegroupname> -l <yourregion> --for-upload --upload-size-bytes 34359738880 --sku standard_lrs
```

Premium SSD veya standart bir SSD yüklemek istiyorsanız, **standard_lrs** **premium_LRS** veya **standardssd_lrs**ile değiştirin. Ultra diskler şimdilik desteklenmez.

Artık yükleme işlemi için yapılandırılan boş yönetilen bir disk oluşturduğunuza göre, bu diske bir VHD yükleyebilirsiniz. Diske bir VHD yüklemek için, yüklemeniz için hedef olarak başvuruda bulunabilmeniz için yazılabilir bir SAS'ye ihtiyacınız vardır.

Boş yönetilen diskinizin yazılabilir Bir SAS `<yourdiskname>` `<yourresourcegroupname>`oluşturmak için aşağıdaki komutu değiştirin ve ardından kullanın:

```azurecli
az disk grant-access -n <yourdiskname> -g <yourresourcegroupname> --access-level Write --duration-in-seconds 86400
```

Örnek döndürülen değer:

```output
{
  "accessSas": "https://md-impexp-t0rdsfgsdfg4.blob.core.windows.net/w2c3mj0ksfgl/abcd?sv=2017-04-17&sr=b&si=600a9281-d39e-4cc3-91d2-923c4a696537&sig=xXaT6mFgf139ycT87CADyFxb%2BnPXBElYirYRlbnJZbs%3D"
}
```

## <a name="upload-a-vhd"></a>VHD’yi karşıya yükleme

Artık boş yönetilen diskiniz için bir SAS'ınız olduğuna göre, yönetilen diskinizi yükleme komutunuzun hedefi olarak ayarlamak için kullanabilirsiniz.

Oluşturduğunuz SAS URI'yi belirterek yerel VHD dosyanızı yönetilen bir diske yüklemek için AzCopy v10'u kullanın.

Bu yükleme eşdeğer [standart HDD](disks-types.md#standard-hdd)ile aynı üretim throughite sahiptir. Örneğin, S4'e eşit bir boyutunuz varsa, 60 MiB/s'ye kadar bir çıktınız olacaktır. Ancak, S70'e eşit bir boyuta sahipseniz, 500 MiB/s'ye kadar bir çıktınız olacaktır.

```bash
AzCopy.exe copy "c:\somewhere\mydisk.vhd" "sas-URI" --blob-type PageBlob
```

Yükleme tamamlandıktan sonra ve artık diske daha fazla veri yazmanız gerekmez, SAS'ı iptal edin. SAS'ı iptal etmek yönetilen diskin durumunu değiştirir ve diski VM'ye eklemenize olanak sağlar.

`<yourdiskname>`Değiştirin `<yourresourcegroupname>`ve diski kullanılabilir hale getirmek için aşağıdaki komutu kullanın:

```azurecli
az disk revoke-access -n <yourdiskname> -g <yourresourcegroupname>
```

## <a name="copy-a-managed-disk"></a>Yönetilen diski çoğaltma

Doğrudan yükleme, yönetilen bir diski kopyalama işlemini de kolaylaştırır. Aynı bölge içinde veya bölgeler arası (başka bir bölgeye) kopyalayabilirsiniz.

Takip komut dosyası bunu sizin için yapar, işlem varolan bir diskle çalıştığınızdan beri bazı farklılıklarla birlikte daha önce açıklanan adımlara benzer.

> [!IMPORTANT]
> Azure'dan yönetilen bir diskin baytlarında disk boyutunu sağlarken 512'lik bir ofset eklemeniz gerekir. Bunun nedeni, Azure'un disk boyutunu döndürürken altbilgiden atlanmasıdır. Bunu yapmazsanız kopya başarısız olur. Aşağıdaki komut dosyası bunu zaten sizin için yapar.

Değerlerinizle `<sourceResourceGroupHere>` `<sourceDiskNameHere>`, `<targetDiskNameHere>` `<targetResourceGroupHere>`, `<yourTargetLocationHere>` , , , ve (konum değeri örneği uswest2 olacaktır) değiştirin, ardından yönetilen bir diski kopyalamak için aşağıdaki komut dosyasını çalıştırın.

```azurecli
sourceDiskName = <sourceDiskNameHere>
sourceRG = <sourceResourceGroupHere>
targetDiskName = <targetDiskNameHere>
targetRG = <targetResourceGroupHere>
targetLocale = <yourTargetLocationHere>

sourceDiskSizeBytes= $(az disk show -g $sourceRG -n $sourceDiskName --query '[uniqueId]' -o tsv)

az disk create -g $targetRG -n $targetDiskName -l $targetLocale --for-upload --upload-size-bytes $(($sourceDiskSizeBytes+512)) --sku standard_lrs

targetSASURI = $(az disk grant-access -n $targetDiskName -g $targetRG  --access-level Write --duration-in-seconds 86400 -o tsv)

sourceSASURI=$(az disk grant-access -n $sourceDiskName -g $sourceRG --duration-in-seconds 86400 --query [accessSas] -o tsv)

.\azcopy copy $sourceSASURI $targetSASURI --blob-type PageBlob

az disk revoke-access -n $sourceDiskName -g $sourceRG

az disk revoke-access -n $targetDiskName -g $targetRG
```

## <a name="next-steps"></a>Sonraki adımlar

Artık yönetilen bir diske başarılı bir VHD yüklediğinize göre, yeni bir VM oluşturmak için diski [varolan bir VM'ye veri diski](add-disk.md) olarak ekleyebilir veya [diski işletim sistemi olarak VM'ye ekleyebilirsiniz.](upload-vhd.md#create-the-vm) 

