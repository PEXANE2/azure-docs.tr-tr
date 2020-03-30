---
title: Azure CLI'yi kullanarak VHD yükleme
description: Azure yönetilen bir diske vhd yüklemeyi ve azure CLI'yi kullanarak bölgeler arasında yönetilen bir diski doğrudan yükleme yoluyla nasıl kopyalayamanızı öğrenin.
services: virtual-machines,storage
author: roygara
ms.author: rogarana
ms.date: 03/13/2020
ms.topic: article
ms.service: virtual-machines
ms.subservice: disks
ms.openlocfilehash: d89a4279d425e4b12e92aae81edfd6c1514c3eef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062659"
---
# <a name="upload-a-vhd-to-azure-using-azure-cli"></a>Azure CLI'yi kullanarak Azure'a vhd yükleme

Bu makalede, yerel makinenizden Azure yönetilen bir diske vhd yükleme nin nasıl olduğu açıklanmaktadır. Daha önce, verilerinizi bir depolama hesabında düzenlemeyi ve bu depolama hesabını yönetmeyi içeren daha ilgili bir işlemi izlemeniz gerekiyordu. Şimdi, artık bir depolama hesabı yönetmek veya bir vhd yüklemek için veri sahne gerekir. Bunun yerine, boş yönetilen bir disk oluşturun ve doğrudan bir vhd yükleyin. Bu, şirket içi VM'lerin Azure'a yüklenmesini kolaylaştırır ve 32 TiB'ye kadar bir vhd'yi doğrudan yönetilen büyük bir diske yüklemenize olanak tanır.

Azure'daki IaaS VM'ler için bir yedekleme çözümü sağlıyorsanız, müşteri yedeklemelerini yönetilen disklere geri yüklemek için doğrudan yükleme kullanmanızı öneririz. Azure dışındaki bir makineden Bir VHD yüklüyorsanız, hızlar yerel bant genişliğinize bağlıdır. Azure VM kullanıyorsanız, bant genişliğiniz standart HDD'lerle aynı olacaktır.

Şu anda, doğrudan yükleme standart HDD, standart SSD ve premium SSD yönetilen diskler için desteklenir. Henüz ultra SSD'ler için desteklenmez.

## <a name="prerequisites"></a>Ön koşullar

- [AzCopy v10](../../storage/common/storage-use-azcopy-v10.md#download-and-install-azcopy)en son sürümünü indirin.
- [Azure CLI'yi yükleyin.](/cli/azure/install-azure-cli)
- Yerel olarak depolanan bir vhd dosyası
- Şirket içinde bir vhd yüklemeyi planlıyorsanız: [Azure için hazırlanmış](../windows/prepare-for-upload-vhd-image.md)sabit boyutlu bir vhd, yerel olarak depolanır.
- Veya bir kopyalama eylemi gerçekleştirmek istiyorsanız Azure'da yönetilen bir disk.

## <a name="create-an-empty-managed-disk"></a>Boş yönetilen disk oluşturma

VHD'nizi Azure'a yüklemek için, bu yükleme işlemi için yapılandırılan boş bir yönetilen disk oluşturmanız gerekir. Bir tane oluşturmadan önce, bu diskler hakkında bilmeniz gereken bazı ek bilgiler vardır.

Yönetilen disk bu tür iki benzersiz durumları vardır:

- ReadToUpload, diskin yükleme almaya hazır olduğu ancak [güvenli erişim imzası](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1) (SAS) oluşturulmadı anlamına gelir.
- ActiveUpload, bu da diskin yükleme almaya hazır olduğu ve SAS'ın oluşturulduğu anlamına gelir.

Bu durumların herhangi birinde, yönetilen disk, gerçek disk türüne bakılmaksızın [standart HDD fiyatlandırmasında](https://azure.microsoft.com/pricing/details/managed-disks/)faturalandırılır. Örneğin, Bir P10 S10 olarak faturalandırılır. Bu, diski `revoke-access` VM'ye takmak için gerekli olan yönetilen diske çağrılana kadar geçerli olacaktır.

Yüklemek için boş bir standart HDD oluşturabilmeniz için önce, baytolarak yüklemek istediğiniz vhd'nin dosya boyutuna sahip olmanız gerekir. Bunu elde etmek için, `wc -c <yourFileName>.vhd` `ls -al <yourFileName>.vhd`ya da kullanabilirsiniz . Bu değer **--upload boyutu-bayt** parametresi belirtilirken kullanılır.

Bir [disk oluşturma](/cli/azure/disk#az-disk-create) cmdletinde hem **-yükleme** için parametrehem de **--upload boyutu-bayt** parametresini belirterek yükleme için boş bir standart HDD oluşturun:

```azurecli
az disk create -n mydiskname -g resourcegroupname -l westus2 --for-upload --upload-size-bytes 34359738880 --sku standard_lrs
```

Premium SSD veya standart bir SSD yüklemek istiyorsanız, **standard_lrs** **premium_LRS** veya **standardssd_lrs**ile değiştirin. Ultra SSD henüz desteklenmedi.

Şimdi yükleme işlemi için yapılandırılan boş bir yönetilen disk oluşturdunuz. Diske bir vhd yüklemek için, yüklemeniz için hedef olarak başvuruda bulunabilmeniz için yazılabilir bir SAS'ye ihtiyacınız vardır.

Boş yönetilen diskinizin yazılabilir SAS'ını oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az disk grant-access -n mydiskname -g resourcegroupname --access-level Write --duration-in-seconds 86400
```

Örnek döndürülen değer:

```output
{
  "accessSas": "https://md-impexp-t0rdsfgsdfg4.blob.core.windows.net/w2c3mj0ksfgl/abcd?sv=2017-04-17&sr=b&si=600a9281-d39e-4cc3-91d2-923c4a696537&sig=xXaT6mFgf139ycT87CADyFxb%2BnPXBElYirYRlbnJZbs%3D"
}
```

## <a name="upload-vhd"></a>VHD yükle

Artık boş yönetilen diskiniz için bir SAS'ınız olduğuna göre, yönetilen diskinizi yükleme komutunuzun hedefi olarak ayarlamak için kullanabilirsiniz.

Oluşturduğunuz SAS URI'yi belirterek yerel VHD dosyanızı yönetilen bir diske yüklemek için AzCopy v10'u kullanın.

Bu yükleme eşdeğer [standart HDD](disks-types.md#standard-hdd)ile aynı üretim throughite sahiptir. Örneğin, S4'e eşit bir boyutunuz varsa, 60 MiB/s'ye kadar bir çıktınız olacaktır. Ancak, S70'e eşit bir boyuta sahipseniz, 500 MiB/s'ye kadar bir çıktınız olacaktır.

```bash
AzCopy.exe copy "c:\somewhere\mydisk.vhd" "sas-URI" --blob-type PageBlob
```

Yükleme tamamlandıktan sonra ve artık diske daha fazla veri yazmanız gerekmez, SAS'ı iptal edin. SAS'ı iptal etmek yönetilen diskin durumunu değiştirir ve diski VM'ye eklemenize olanak sağlar.

```azurecli
az disk revoke-access -n mydiskname -g resourcegroupname
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

Artık yönetilen bir diske başarılı bir vhd yüklediğinize göre, diski [varolan bir VM'ye veri diski](add-disk.md) olarak ekleyebilir veya yeni bir VM oluşturmak için [diski işletim sistemi olarak VM'ye ekleyebilirsiniz.](upload-vhd.md#create-the-vm) 

