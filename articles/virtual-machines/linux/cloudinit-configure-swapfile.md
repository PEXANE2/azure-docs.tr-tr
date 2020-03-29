---
title: Linux VM'de takas bölümü yapılandırmak için bulut init'i kullanma
description: Azure CLI ile oluşturma sırasında Linux VM'de bir takas bölümü yapılandırmak için bulut init nasıl kullanılır?
author: rickstercdn
manager: gwallace
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 1247652e536042ee249054d86aed3c3f8e7aa7bf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969197"
---
# <a name="use-cloud-init-to-configure-a-swap-partition-on-a-linux-vm"></a>Linux VM'de takas bölümü yapılandırmak için bulut init'i kullanma
Bu makalede, çeşitli Linux dağıtımlarında takas bölümü yapılandırmak için [bulut init](https://cloudinit.readthedocs.io) nasıl kullanılacağını gösterir. Takas bölümü geleneksel olarak Linux Agent (WALA) tarafından hangi dağıtımların gerekli olduğuna göre yapılandırıldı.  Bu belge, bulut init kullanarak sağlama süresi sırasında isteğe bağlı takas bölümü oluşturma işlemini ana hatlar.  Azure'da ve desteklenen Linux dağıtımlarında bulut init'in yerel olarak nasıl çalıştığı hakkında daha fazla bilgi için [bulut girişe genel bakış](using-cloud-init.md)

## <a name="create-swap-partition-for-ubuntu-based-images"></a>Ubuntu tabanlı görüntüler için takas bölümü oluşturma
Azure'da varsayılan olarak, Ubuntu galeri görüntüleri takas bölümleri oluşturmaz. Bulut init kullanarak VM sağlama süresi sırasında değiştirme bölümü yapılandırmasını etkinleştirmek için lütfen Ubuntu wiki'deki [AzureSwapBölümleri belgesine](https://wiki.ubuntu.com/AzureSwapPartitions) bakın.

## <a name="create-swap-partition-for-red-hat-and-centos-based-images"></a>Red Hat ve CentOS tabanlı görüntüler için takas bölümü oluşturma

Geçerli kabuğunuzda *cloud_init_swappart.txt* adlı bir dosya oluşturun ve aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil, Bulut Kabuğu'nda oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_swappart.txt` adını girin. **Nano** düzenleyiciyi kullanmak için #1 seçin. Bulut-init dosyasının tamamının, özellikle de ilk satırın doğru kopyalandığından emin olun.  

```yaml
#cloud-config
disk_setup:
  ephemeral0:
    table_type: gpt
    layout: [66, [33,82]]
    overwrite: true
fs_setup:
  - device: ephemeral0.1
    filesystem: ext4
  - device: ephemeral0.2
    filesystem: swap
mounts:
  - ["ephemeral0.1", "/mnt"]
  - ["ephemeral0.2", "none", "swap", "sw", "0", "0"]
```

Bu görüntüyü dağıtmadan önce, az grubu oluşturma komutuna sahip bir kaynak grubu [oluşturmanız](/cli/azure/group) gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, [az vm oluşturmak](/cli/azure/vm) ve aşağıdaki gibi bulut-init `--custom-data cloud_init_swappart.txt` dosyası belirtin bir VM oluşturun:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_swappart.txt \
  --generate-ssh-keys 
```

## <a name="verify-swap-partition-was-created"></a>Takas bölümü oluşturulduğunu doğrula
SSH, önceki komuttan çıktıda gösterilen VM'nizin genel IP adresine. Kendi **genel İplik Adresinizi** aşağıdaki gibi girin:

```bash
ssh <publicIpAddress>
```

SSH'ed'i vm'ye soktuktan sonra, takas bölümü oluşturulıp oluşturulmamasını kontrol edin

```bash
swapon -s
```

Bu komutun çıktısı aşağıdaki gibi görünmelidir:

```text
Filename                Type        Size    Used    Priority
/dev/sdb2  partition   2494440 0   -1
```

> [!NOTE] 
> Takas bölümü yapılandırılan varolan bir Azure resminiz varsa ve yeni görüntüler için takas bölümü yapılandırmasını değiştirmek istiyorsanız, varolan takas bölmesini kaldırmanız gerekir. Daha fazla bilgi için lütfen 'Görüntüleri bulut-init'e göre özelleştir' belgesine bakın.

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik ek bulut bindirme örnekleri için aşağıdakileri görün:
 
- [VM'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede varolan paketleri güncelleştirmek için bir paket yöneticisi çalıştırma](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirme ve anahtar ekleme](tutorial-automate-vm-deployment.md)
