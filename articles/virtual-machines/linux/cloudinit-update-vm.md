---
title: Azure'da bir Linux VM'de bulut init'i kullanma
description: Azure CLI ile oluşturma sırasında paketleri Linux VM'de güncelleştirmek ve yüklemek için bulut init nasıl kullanılır?
author: cynthn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 04/20/2018
ms.author: cynthn
ms.openlocfilehash: 7b7a03572a001fc6d5114635b33510f1a4b1bc70
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969149"
---
# <a name="use-cloud-init-to-update-and-install-packages-in-a-linux-vm-in-azure"></a>Azure'daki bir Linux VM'deki paketleri güncelleştirmek ve yüklemek için bulut init'i kullanma
Bu makalede, Azure'da sağlama zamanında Linux sanal makine (VM) veya sanal makine ölçeği kümelerinde paketleri güncelleştirmek için [bulut init'i](https://cloudinit.readthedocs.io) nasıl kullanacağınızı gösterilmektedir. Bu bulut init komut dosyaları, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalışır. Azure'da ve desteklenen Linux dağıtımlarında bulut init'in yerel olarak nasıl çalıştığı hakkında daha fazla bilgi için [bulut girişe genel bakış](using-cloud-init.md)

## <a name="update-a-vm-with-cloud-init"></a>Bulut init ile bir VM güncelleştirme
Güvenlik amacıyla, ilk önyüklemede en son güncelleştirmeleri uygulamak için bir VM yapılandırmak isteyebilirsiniz. Bulut-init farklı Linux dağıtımları üzerinde çalıştığından, `apt` paket `yum` yöneticisini belirtmeye veya buna gerek yoktur. Bunun yerine, `package_upgrade` bulut giriş işleminin dağıtım için kullanılan uygun mekanizmayı belirlemesine izin verebilirsiniz. Bu iş akışı, dağıtımlar arasında aynı bulut giriş komut dosyalarını kullanmanıza olanak tanır.

Yükseltme işlemini iş başında görmek için geçerli kabuğunuzda *cloud_init_upgrade.txt* adlı bir dosya oluşturun ve aşağıdaki yapılandırmayı yapıştırın. Bu örnekte, dosyayı yerel makinenizde değil, Bulut Kabuğu'nda oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor cloud_init_upgrade.txt` adını girin. **Nano** düzenleyiciyi kullanmak için #1 seçin. Bulut-init dosyasının tamamının, özellikle de ilk satırın doğru kopyalandığından emin olun.  

```yaml
#cloud-config
package_upgrade: true
packages:
- httpd
```

Bu görüntüyü dağıtmadan önce, az grubu oluşturma komutuna sahip bir kaynak grubu [oluşturmanız](/cli/azure/group) gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, [az vm oluşturmak](/cli/azure/vm) ve aşağıdaki gibi bulut-init `--custom-data cloud_init_upgrade.txt` dosyası belirtin bir VM oluşturun:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data cloud_init_upgrade.txt \
  --generate-ssh-keys 
```

SSH, önceki komuttan çıktıda gösterilen VM'nizin genel IP adresine. Kendi **genel İplik Adresinizi** aşağıdaki gibi girin:

```bash
ssh <publicIpAddress>
```

Paket yönetim aracını çalıştırın ve güncelleştirmeleri denetleyin.

```bash
sudo yum update
```

Bulut giriş denetlendiğinden ve güncelleştirmeleri önyüklemeye yükledikçe, uygulanacak ek güncelleştirme olmamalıdır.  Güncelleştirme işlemini, değiştirilen paket sayısını ve aşağıdakine `httpd` benzer `yum history` çıktıyı çalıştırarak ve gözden geçirerek yüklemeyi görürsünüz.

```bash
Loaded plugins: fastestmirror, langpacks
ID     | Command line             | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
     3 | -t -y install httpd      | 2018-04-20 22:42 | Install        |    5
     2 | -t -y upgrade            | 2018-04-20 22:38 | I, U           |   65
     1 |                          | 2017-12-12 20:32 | Install        |  522
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik ek bulut bindirme örnekleri için aşağıdakileri görün:
 
- [VM'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede varolan paketleri güncelleştirmek için bir paket yöneticisi çalıştırma](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirme ve anahtar ekleme](tutorial-automate-vm-deployment.md)
