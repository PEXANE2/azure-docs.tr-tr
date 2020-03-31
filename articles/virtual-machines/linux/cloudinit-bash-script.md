---
title: Azure'daki bir Linux VM'de bash komut dosyası çalıştırmak için bulut init'i kullanma
description: Azure CLI ile oluşturma sırasında Linux VM'de bash komut dosyası çalıştırmak için bulut init nasıl kullanılır?
author: rickstercdn
ms.service: virtual-machines-linux
ms.topic: article
ms.date: 11/29/2017
ms.author: rclaus
ms.openlocfilehash: e2f19ceb6c7f19ba749b46a3553036587be6a71a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78969224"
---
# <a name="use-cloud-init-to-run-a-bash-script-in-a-linux-vm-in-azure"></a>Azure'da bir Linux VM'de bash komut dosyası çalıştırmak için bulut init'i kullanma
Bu makalede, Azure'da sağlama zamanında bir Linux sanal makinede (VM) veya sanal makine ölçeği kümelerinde (VMSS) varolan bir bash komut dosyasını çalıştırmak için [bulut init'i](https://cloudinit.readthedocs.io) nasıl kullanacağınızı gösterilmektedir. Bu bulut init komut dosyaları, kaynaklar Azure tarafından sağlandıktan sonra ilk önyüklemede çalışır. Azure'da ve desteklenen Linux dağıtımlarında bulut init'in yerel olarak nasıl çalıştığı hakkında daha fazla bilgi için [bulut girişe genel bakış](using-cloud-init.md)

## <a name="run-a-bash-script-with-cloud-init"></a>Bulut tabanlı bir bash komut dosyası çalıştırma
Bulut girişi ile varolan komut dosyalarınızı bir bulut config'ine dönüştürmeniz gerekmez, cloud-init birden çok giriş türünü kabul eder ve bunlardan biri bash komut dosyasıdır.

Komut dosyalarınızı çalıştırmak için Linux Custom Script Azure Uzantısı'nı kullanıyorsanız, bunları bulut eklentisini kullanmak üzere geçirebilirsiniz. Ancak, Azure Uzantıları komut dosyası hatalarına karşı uyarmak için raporlamayı tümleşik hale getirmekiçin, komut dosyası başarısız olursa bulut içerisindeki görüntü dağıtımı başarısız olmaz.

Bu işlevselliği iş başında görmek için, sınama için basit bir bash komut dosyası oluşturun. Bulut-init `#cloud-config` dosyası gibi, bu komut dosyası da sanal makinenizi sağlamak için AzureCLI komutlarını çalıştıracağınız yerin yerel olması gerekir.  Bu örnekte, dosyayı yerel makinenizde değil, Bulut Kabuğu'nda oluşturun. İstediğiniz düzenleyiciyi kullanabilirsiniz. Dosyayı oluşturmak ve kullanılabilir düzenleyicilerin listesini görmek için `sensible-editor simple_bash.sh` adını girin. **Nano** düzenleyiciyi kullanmak için #1 seçin. Bulut-init dosyasının tamamının, özellikle de ilk satırın doğru kopyalandığından emin olun.  

```bash
#!/bin/sh
echo "this has been written via cloud-init" + $(date) >> /tmp/myScript.txt
```

Bu görüntüyü dağıtmadan önce, az grubu oluşturma komutuna sahip bir kaynak grubu [oluşturmanız](/cli/azure/group) gerekir. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır. Aşağıdaki örnek *eastus* konumunda *myResourceGroup* adlı bir kaynak grubu oluşturur.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

Şimdi, [az vm oluşturmak](/cli/azure/vm) ve aşağıdaki gibi bash `--custom-data simple_bash.sh` komut dosyası dosyası belirtin bir VM oluşturun:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroup \
  --name centos74 \
  --image OpenLogic:CentOS:7-CI:latest \
  --custom-data simple_bash.sh \
  --generate-ssh-keys 
```
## <a name="verify-bash-script-has-run"></a>Bash komut dosyasının çalıştırılıp çalıştırıldın
SSH, önceki komuttan çıktıda gösterilen VM'nizin genel IP adresine. Kendi **genel İplik Adresinizi** aşağıdaki gibi girin:

```bash
ssh <publicIpAddress>
```

**/tmp** dizinini değiştirin ve myScript.txt dosyasının var olduğunu ve içinde uygun metin olduğunu doğrulayın.  Yoksa, daha fazla bilgi için **/var/log/cloud-init.log'u** kontrol edebilirsiniz.  Aşağıdaki girişi arayın:

```bash
Running config-scripts-user using lock Running command ['/var/lib/cloud/instance/scripts/part-001']
```

## <a name="next-steps"></a>Sonraki adımlar
Yapılandırma değişikliklerine yönelik ek bulut bindirme örnekleri için aşağıdakileri görün:
 
- [VM'ye ek bir Linux kullanıcısı ekleme](cloudinit-add-user.md)
- [İlk önyüklemede varolan paketleri güncelleştirmek için bir paket yöneticisi çalıştırma](cloudinit-update-vm.md)
- [VM yerel ana bilgisayar adını değiştirme](cloudinit-update-vm-hostname.md) 
- [Uygulama paketi yükleme, yapılandırma dosyalarını güncelleştirme ve anahtar ekleme](tutorial-automate-vm-deployment.md)
