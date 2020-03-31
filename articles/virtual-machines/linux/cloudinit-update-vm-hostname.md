---
title: Linux VM için ana bilgisayar adını ayarlamak için bulut init'i kullanma
description: Azure CLI ile oluşturma sırasında bir Linux VM'yi özelleştirmek için bulut init nasıl kullanılır?
author: rickstercdn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: 631b8ef83d5fbf10ec401df7432b23238f2ae2e6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969159"
---
# <a name="use-cloud-init-to-set-hostname-for-a-linux-vm-in-azure"></a>Azure'da bir Linux VM için ana bilgisayar adı ayarlamak için bulut init'i kullanma
Bu makalede, Azure'da belirli bir ana bilgisayar adını sanal makine (VM) veya sanal makine ölçeği kümelerinde (VMSS) yapılandırmak için [bulut init'in](https://cloudinit.readthedocs.io) nasıl kullanılacağı gösterilmektedir. Bu bulut init komut dosyaları, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalışır. Azure'da ve desteklenen Linux dağıtımlarında bulut init'in yerel olarak nasıl çalıştığı hakkında daha fazla bilgi için [bulut girişe genel bakış](using-cloud-init.md)

## <a name="set-the-hostname-with-cloud-init"></a>Ana bilgisayar adını bulut-init ile ayarlama
Varsayılan olarak, Azure'da yeni bir sanal makine oluşturduğunuzda ana bilgisayar adı VM adı ile aynıdır.  [Az vm oluştur](/cli/azure/vm)ile Azure'da bir VM oluştururken bu varsayılan ana bilgisayar adını değiştirmek için bir `--custom-data` bulut init komut dosyası çalıştırmak için, anahtarile bulut-init dosyasını belirtin.  

Yükseltme işlemini iş başında görmek için geçerli *kabuğunuzda cloud_init_hostname.txt* adlı bir dosya oluşturun ve aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil, Bulut Kabuğu'nda oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_hostname.txt` adını girin. **Nano** düzenleyiciyi kullanmak için #1 seçin. Bulut-init dosyasının tamamının, özellikle de ilk satırın doğru kopyalandığından emin olun.  

```yaml
#cloud-config
hostname: myhostname
```

Bu görüntüyü dağıtmadan önce, az grubu oluşturma komutuna sahip bir kaynak grubu [oluşturmanız](/cli/azure/group) gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, [az vm oluşturmak](/cli/azure/vm) ve aşağıdaki gibi bulut-init `--custom-data cloud_init_hostname.txt` dosyası belirtin bir VM oluşturun:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_hostname.txt \
  --generate-ssh-keys 
```

Oluşturulduktan sonra, Azure CLI VM hakkındaki bilgileri gösterir. VM'iniz `publicIpAddress` için SSH'yi kullanın. Kendi adresinizi aşağıdaki gibi girin:

```bash
ssh <publicIpAddress>
```

VM adını görmek için `hostname` aşağıdaki komutu kullanın:

```bash
hostname
```

VM, aşağıdaki örnek çıktıda gösterildiği gibi, ana bilgisayar adını bulut-init dosyasında ayarlanan değer olarak bildirmelidir:

```bash
myhostname
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik ek bulut bindirme örnekleri için aşağıdakileri görün:
 
- [VM'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede varolan paketleri güncelleştirmek için bir paket yöneticisi çalıştırma](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirme ve anahtar ekleme](tutorial-automate-vm-deployment.md)
