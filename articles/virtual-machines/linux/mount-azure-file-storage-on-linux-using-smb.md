---
title: SMB kullanarak Linux VM'lerinde Azure Dosya depolamasına montaj
description: Azure CLI ile SMB kullanarak Linux VM'lerine Azure Dosyası depolaması nasıl monte edilebilen
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure
ms.date: 06/28/2018
ms.author: cynthn
ms.openlocfilehash: 0314095a053087a7d490926c41c6ae386c304919
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80066642"
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a>SMB kullanarak Linux VM'lerinde Azure Dosya depolamasına montaj

Bu makalede, Azure CLI ile bir Kobİ yuvası kullanarak Bir Linux VM'de Azure Dosyası depolama hizmetini nasıl kullanacağınızı gösterir. Azure Dosya depolama, standart Kobİ protokolünü kullanarak bulutta dosya paylaşımları sunar. 

Dosya depolama, bulutta standart Kobİ protokolünü kullanan dosya paylaşımları sunar. SMB 3.0'ı destekleyen herhangi bir işletim sistemi için dosya paylaşımı monte edebilirsiniz. Linux'ta bir Kobİ yuvası kullandığınızda, bir SLA tarafından desteklenen sağlam ve kalıcı arşivleme depolama konumuna kolay yedekleme ler elde elabilirsiniz.

Dosyaları VM'den Dosya depolamada barındırılan bir SMB yuvasına taşımak günlükleri ayıklamanın harika bir yoludur. Aynı SMB payı mac, Linux veya Windows iş istasyonunuza yerel olarak monte edilebilir. SMB, Linux veya uygulama günlüklerini gerçek zamanlı olarak aktarmak için en iyi çözüm değildir, çünkü Kobİ protokolü bu tür ağır günlük işlemlerini yerine getirmek için oluşturulmadığından. Fluentd gibi özel, birleşik bir günlük katmanı aracı Linux ve uygulama günlük çıktısını toplamak için SMB'den daha iyi bir seçim olacaktır.

Bu kılavuz, Azure CLI sürümü 2.0.4 veya sonraki sürümlerini çalıştırmanızı gerektirir. Sürümü bulmak için **az --sürümünü** çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli). 


## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

*Doğu ABD* konumunda *myResourceGroup* adında bir kaynak grubu oluşturun.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Oluşturduğunuz kaynak grubu içinde, [az depolama hesabı oluşturarak](/cli/azure/storage/account)yeni bir depolama hesabı oluşturun. Bu örnek, *mySTORAGEACCT\<rasgele sayı>* adlı bir depolama hesabı oluşturur ve bu depolama hesabının adını değişken **STORAGEACCT'ye**koyar. Depolama hesabı adları benzersiz `$RANDOM` olması gerekir, benzersiz yapmak için sonuna kadar bir sayı ekler kullanarak.

```azurecli
STORAGEACCT=$(az storage account create \
    --resource-group "myResourceGroup" \
    --name "mystorageacct$RANDOM" \
    --location eastus \
    --sku Standard_LRS \
    --query "name" | tr -d '"')
```

## <a name="get-the-storage-key"></a>Depolama anahtarını alın

Bir depolama hesabı oluşturduğunuzda, hesap anahtarları herhangi bir hizmet kesintisi olmaksızın döndürülebilmeleri için çiftler halinde oluşturulur. Çiftteki ikinci anahtara geçtiğinızda, yeni bir anahtar çifti oluşturursunuz. Yeni depolama hesabı anahtarları her zaman çiftler halinde oluşturulur, bu nedenle her zaman geçiş için hazır en az bir kullanılmayan depolama hesabı anahtarı var.

Az depolama hesap anahtarları listesini kullanarak depolama [hesabı anahtarlarını](/cli/azure/storage/account/keys)görüntüleyin. Bu örnek, **STORAGEKEY** değişkeninde anahtar 1 değerini depolar.

```azurecli
STORAGEKEY=$(az storage account keys list \
    --resource-group "myResourceGroup" \
    --account-name $STORAGEACCT \
    --query "[0].value" | tr -d '"')
```

## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

az depolama payı oluşturarak Dosya depolama [paylaşımını oluşturun.](/cli/azure/storage/share) 

Paylaşım adlarının tüm küçük harfler, sayılar ve tek tireler olması gerekir, ancak tire ile başlayamaz. Dosya paylaşımlarını ve dosyalarını adlandırma hakkında ayrıntılı bilgi için, [Paylaşımları, Dizinleri, Dosyaları ve Meta Verileri Adlandırma ve Başvurma bölümüne](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata)bakın.

Bu örnek, 10-GiB kotası ile *myshare* adlı bir pay oluşturur. 

```azurecli
az storage share create --name myshare \
    --quota 10 \
    --account-name $STORAGEACCT \
    --account-key $STORAGEKEY
```

## <a name="create-a-mount-point"></a>Montaj noktası oluşturma

Azure dosya paylaşımını Linux bilgisayarınıza monte etmek için **cifs-utils** paketinin yüklü olduğundan emin olmanız gerekir. Yükleme yönergeleri için, [Linux dağıtımınız için cifs-utils paketini yükleyin'](../../storage/files/storage-how-to-use-files-linux.md#install-cifs-utils)e bakın.

Azure Files, TCP bağlantı noktası 445 üzerinden iletişim sağlayan SMB protokolünü kullanır.  Azure dosya paylaşımınızı montajda sorun yaşıyorsanız, güvenlik duvarınızın TCP bağlantı noktası 445'i engellemediğinden emin olun.


```bash
mkdir -p /mnt/MyAzureFileShare
```

## <a name="mount-the-share"></a>Paylaşıma montaj

Azure dosya paylaşımını yerel dizine monte edin. 

```bash
sudo mount -t cifs //$STORAGEACCT.file.core.windows.net/myshare /mnt/MyAzureFileShare -o vers=3.0,username=$STORAGEACCT,password=$STORAGEKEY,dir_mode=0777,file_mode=0777,serverino
```

Yukarıdaki komut, Azure dosya paylaşımını ve [cif'lere](https://linux.die.net/man/8/mount.cifs)özgü seçenekleri monte etmek için [montaj](https://linux.die.net/man/8/mount) komutunu kullanır. Özellikle, file_mode ve dir_mode seçenekleri dosyaları ve `0777`dizinleri izin olarak ayarlar. İzin, `0777` tüm kullanıcılara okuma, yazma ve yürütme izinleri verir. Değerleri diğer [CHMod izinleriyle](https://en.wikipedia.org/wiki/Chmod)değiştirerek bu izinleri değiştirebilirsiniz. Gid veya uid gibi diğer [cifs](https://linux.die.net/man/8/mount.cifs) seçeneklerini da kullanabilirsiniz. 


## <a name="persist-the-mount"></a>Montaj devam

Linux VM'yi yeniden başlattığınızda, monte edilen SMB payı kapatma sırasında söküldü. Önyükleme de SMB payı yeniden monte etmek için, Linux / vb / fstab bir satır ekleyin. Linux, önyükleme işlemi sırasında monte etmesi gereken dosya sistemlerini listelemek için fstab dosyasını kullanır. Kobİ paylaşımının eklenmesi, Dosya depolama payının Linux VM için kalıcı olarak monte edilmiş bir dosya sistemi olmasını sağlar. Bulut init kullandığınızda Dosya depolama SMB payını yeni bir VM'ye eklemek mümkündür.

```bash
//myaccountname.file.core.windows.net/mystorageshare /mnt/mymountpoint cifs vers=3.0,username=mystorageaccount,password=myStorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

Üretim ortamlarında daha fazla güvenlik için kimlik bilgilerinizi fstab dışında saklamanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Oluşturma sırasında bir Linux VM'yi özelleştirmek için bulut init'i kullanma](using-cloud-init.md)
- [Linux VM'ye disk ekleme](add-disk.md)
- [Linux VM'leri için Azure Disk Şifrelemesi](disk-encryption-overview.md)

