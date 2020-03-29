---
title: Azure'daki bir Linux VM'ye kullanıcı eklemek için bulut init'i kullanma
description: Azure CLI ile oluşturma sırasında bir Linux VM'ye kullanıcı eklemek için bulut init nasıl kullanılır?
author: rickstercdn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: f1782bfe0c14e3b44703f89ec7f78590c1bb74c5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969233"
---
# <a name="use-cloud-init-to-add-a-user-to-a-linux-vm-in-azure"></a>Azure'daki bir Linux VM'ye kullanıcı eklemek için bulut init'i kullanma
Bu makalede, Azure'da sağlama zamanında sanal makine (VM) veya sanal makine ölçeği kümelerine (VMSS) kullanıcı eklemek için [bulut init'i](https://cloudinit.readthedocs.io) nasıl kullanacağınızı gösterilmektedir. Bu bulut init komut dosyası, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalışır. Azure'da ve desteklenen Linux dağıtımlarında bulut init'in yerel olarak nasıl çalıştığı hakkında daha fazla bilgi için [bulut girişe genel bakış](using-cloud-init.md)alabakın.

## <a name="add-a-user-to-a-vm-with-cloud-init"></a>Bulut eklentisi olan bir VM'ye kullanıcı ekleme
Herhangi bir yeni Linux VM ilk görevlerinden biri *kök*kullanımını önlemek için kendiniz için ek bir kullanıcı eklemektir. SSH tuşları güvenlik ve kullanılabilirlik için en iyi yöntemdir. Bu bulut init komut dosyasıile *~/.ssh/authorized_keys* dosyasına anahtarlar eklenir.

Bir Linux VM'sine kullanıcı eklemek için geçerli kabuğunuzda *cloud_init_add_user.txt* adlı bir dosya oluşturun ve aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil, Bulut Kabuğu'nda oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_add_user.txt` adını girin. **Nano** düzenleyiciyi kullanmak için #1 seçin. Bulut-init dosyasının tamamının, özellikle de ilk satırın doğru kopyalandığından emin olun.  Değeri için kendi ortak anahtarınızı *(~.ssh/id_rsa.pub*içeriği gibi) sağlamanız `ssh-authorized-keys:` gerekir - bu örnek basitleştirmek için burada kısaltılmıştır.

```yaml
#cloud-config
users:
  - default
  - name: myadminuser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>
```
> [!NOTE] 
> #cloud-config dosyası parametre dahil içerir. `- default` Bu, kullanıcıyı, sağlama sırasında oluşturulan varolan yönetici kullanıcıya ekler. `- default` Parametresi olmayan bir kullanıcı oluşturursanız , Azure platformu tarafından oluşturulan otomatik oluşturulan yönetici kullanıcı üzerine yazılır. 

Bu görüntüyü dağıtmadan önce, az grubu oluşturma komutuna sahip bir kaynak grubu [oluşturmanız](/cli/azure/group) gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, [az vm oluşturmak](/cli/azure/vm) ve aşağıdaki gibi bulut-init `--custom-data cloud_init_add_user.txt` dosyası belirtin bir VM oluşturun:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_add_user.txt \
  --generate-ssh-keys 
```

SSH, önceki komuttan çıktıda gösterilen VM'nizin genel IP adresine. Kendi **genel İplik Adresinizi** aşağıdaki gibi girin:

```bash
ssh <publicIpAddress>
```

Kullanıcınızın VM'ye ve belirtilen gruplara eklenmesini onaylamak *için/etc/grup* dosyasının içeriğini aşağıdaki gibi görüntüleyin:

```bash
cat /etc/group
```

Aşağıdaki örnek çıktı, *cloud_init_add_user.txt* dosyasındaki kullanıcının VM'ye ve uygun gruba eklendiğini gösterir:

```bash
root:x:0:
<snip />
sudo:x:27:myadminuser
<snip />
myadminuser:x:1000:
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik ek bulut bindirme örnekleri için aşağıdakileri görün:
 
- [VM'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede varolan paketleri güncelleştirmek için bir paket yöneticisi çalıştırma](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirme ve anahtar ekleme](tutorial-automate-vm-deployment.md)
