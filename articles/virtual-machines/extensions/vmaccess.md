---
title: Azure Linux VM'ye erişimi sıfırlama
description: VMAccess Uzantısı ve Azure CLI'yi kullanarak linux vm'lerinde yönetim kullanıcıları nın yönetimi ve erişimini sıfırlama
services: virtual-machines-linux
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 261a9646-1f93-407e-951e-0be7226b3064
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/10/2018
ms.author: akjosh
ms.openlocfilehash: bd9dc05a84a4ee54fce40e6c88e87ac90bfee8a5
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79250367"
---
# <a name="manage-administrative-users-ssh-and-check-or-repair-disks-on-linux-vms-using-the-vmaccess-extension-with-the-azure-cli"></a>Azure CLI ile VMAccess Uzantısını kullanarak Linux VM'lerinde yönetim kullanıcılarını, SSH'yi ve diskleri kontrol edin veya onarın
## <a name="overview"></a>Genel Bakış
Linux VM'nizdeki disk hatalar gösteriyor. Bir şekilde Linux VM'nizin kök parolasını sıfırlar veya yanlışlıkla SSH özel anahtarınızı sildiniz. Bu veri merkezi gün geri oldu, orada sürücü ve daha sonra sunucu konsolu almak için KVM açmanız gerekir. Azure VMAccess uzantısını, Linux'a erişimi sıfırlamak veya disk düzeyinde bakım gerçekleştirmek için konsola erişmenize olanak tanıyan KVM anahtarı olarak düşünün.

Bu makalede, bir diski denetlemek veya onarmak, kullanıcı erişimini sıfırlamak, yönetimkullanıcı hesaplarını yönetmek veya Azure Kaynak Yöneticisi sanal makineleri olarak çalışırken Linux'taki SSH yapılandırmasını güncelleştirmek için Azure VMAccess Uzantısı'nı nasıl kullanacağınızı gösterir. Klasik sanal makineleri yönetmeniz gerekiyorsa, [klasik VM belgelerinde](../linux/classic/reset-access-classic.md)bulunan talimatları takip edebilirsiniz. 
 
> [!NOTE]
> AAD Giriş Uzantısı'nı yükledikten sonra VM'nizin parolasını sıfırlamak için VMAccess Uzantısı'nı kullanırsanız, makineniz için AAD Girişini yeniden etkinleştirmek için AAD Giriş Uzantısını yeniden çalıştırmanız gerekir.

## <a name="prerequisites"></a>Ön koşullar
### <a name="operating-system"></a>İşletim sistemi

VM Access uzantısı bu Linux dağıtımlarına karşı çalıştırılabilir:

| Dağıtım | Sürüm |
|---|---|
| Ubuntu | 16.04 LTS, 14.04 LTS ve 12.04 LTS |
| Debian | Debian 7.9+, 8.2+ |
| Red Hat | RHEL 6.7+, 7.1+ |
| Oracle Linux | 6.4+, 7.0+ |
| Suse | 11 ve 12 |
| Opensuse | openSUSE Leap 42.2+ |
| CentOS | CentOS 6.3+, 7.0+ |
| CoreOS | 494.4.0+ |

## <a name="ways-to-use-the-vmaccess-extension"></a>VMAccess Uzantısını kullanmanın yolları
Linux VM'lerinizde VMAccess Uzantısı'nı kullanmanın iki yolu vardır:

* Azure CLI'yi ve gerekli parametreleri kullanın.
* [VMAccess Uzantı işlemi ham JSON dosyaları kullanın](#use-json-files-and-the-vmaccess-extension) ve sonra hareket.

Aşağıdaki [örneklerde az vm kullanıcı](/cli/azure/vm/user) komutları kullanılır. Bu adımları gerçekleştirmek için, en son [Azure CLI'nin](/cli/azure/install-az-cli2) [az girişi](/cli/azure/reference-index)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir.

## <a name="update-ssh-key"></a>SSH anahtarını güncelleştirme
Aşağıdaki örnek, VM'de `azureuser` `myVM`kullanıcı için SSH tuşunu güncelleştirir:

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

> **NOT:** Komut, `az vm user update` VM'deki yönetici kullanıcı `~/.ssh/authorized_keys` için dosyaya yeni ortak anahtar metnini ekler. Bu, varolan SSH anahtarlarının yerini almaz veya kaldırmaz. Bu, VMAccess Uzantısı aracılığıyla dağıtım zamanında ayarlanan önceki anahtarları veya sonraki güncelleştirmeleri kaldırmaz.

## <a name="reset-password"></a>Parola sıfırlama
Aşağıdaki örnek, VM'deki `azureuser` `myVM`kullanıcının parolasını sıfırlar:

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username azureuser \
  --password myNewPassword
```

## <a name="restart-ssh"></a>SSH'yi yeniden başlatma
Aşağıdaki örnek, SSH daemon'u yeniden başlatır ve SSH yapılandırmasını `myVM`bir VM adlı varsayılan değerlere sıfırlar:

```azurecli-interactive
az vm user reset-ssh \
  --resource-group myResourceGroup \
  --name myVM
```

## <a name="create-an-administrativesudo-user"></a>Yönetici/sudo kullanıcısı oluşturma
Aşağıdaki örnek, **sudo** `myNewUser` izinleri ile adlı bir kullanıcı oluşturur. Hesap, VM adlı `myVM`kimlik doğrulama için bir SSH tuşu kullanır. Bu yöntem, geçerli kimlik bilgilerinin kaybolması veya unutulması durumunda bir VM'ye yeniden erişmenize yardımcı olmak için tasarlanmıştır. En iyi uygulama olarak, **sudo** izinleri olan hesaplar sınırlı olmalıdır.

```azurecli-interactive
az vm user update \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser \
  --ssh-key-value ~/.ssh/id_rsa.pub
```

## <a name="delete-a-user"></a>Kullanıcı silme
Aşağıdaki örnek, VM `myNewUser` adlı `myVM`bir kullanıcıyı siler:

```azurecli-interactive
az vm user delete \
  --resource-group myResourceGroup \
  --name myVM \
  --username myNewUser
```

## <a name="use-json-files-and-the-vmaccess-extension"></a>JSON uzantılarını ve VMAccess Uzantısını kullanma
Aşağıdaki örneklerde ham JSON dosyaları kullanılır. JSON dosyalarınızı aramak için [az vm uzantısı ayarını](/cli/azure/vm/extension) kullanın. Bu JSON dosyaları Azure şablonlarından da çağrılabilir. 

### <a name="reset-user-access"></a>Kullanıcı erişimini sıfırlama
Linux VM'nizde kök erişiminizi kaybettiyseniz, bir kullanıcının SSH anahtarını veya parolasını güncellemek için bir VMAccess komut dosyası başlatabilirsiniz.

Bir kullanıcının SSH ortak anahtarını güncelleştirmek `update_ssh_key.json` için, adlı bir dosya oluşturun ve aşağıdaki biçimde ayarlar ekleyin. Kendi değerlerinizi ve `username` `ssh_key` parametreleri değiştirin:

```json
{
  "username":"azureuser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNGxxxxxx2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7xxxxxxwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5wxxtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== azureuser@myVM"
}
```

VMAccess komut dosyasını aşağıdakilerle çalıştırın:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings update_ssh_key.json
```

Kullanıcı parolasını sıfırlamak için, `reset_user_password.json` adlandırılmış bir dosya oluşturun ve aşağıdaki biçimde ayarlar ekleyin. Kendi değerlerinizi ve `username` `password` parametreleri değiştirin:

```json
{
  "username":"azureuser",
  "password":"myNewPassword" 
}
```

VMAccess komut dosyasını aşağıdakilerle çalıştırın:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_user_password.json
```

### <a name="restart-ssh"></a>SSH'yi yeniden başlatma
SSH daemon'u yeniden başlatmak ve SSH yapılandırmasını varsayılan değerlere sıfırlamak için . `reset_sshd.json` Aşağıdaki içeriği ekleyin:

```json
{
  "reset_ssh": true
}
```

VMAccess komut dosyasını aşağıdakilerle çalıştırın:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings reset_sshd.json
```

### <a name="manage-administrative-users"></a>İdari kullanıcıları yönetme

Kimlik doğrulaması için SSH anahtarı kullanan **sudo** izinlerine sahip bir `create_new_user.json` kullanıcı oluşturmak için, adlı bir dosya oluşturun ve aşağıdaki biçimde ayarlar ekleyin. Kendi değerlerinizi ve `username` `ssh_key` parametreleri değiştirin. Bu yöntem, geçerli kimlik bilgilerinin kaybolması veya unutulması durumunda bir VM'ye yeniden erişmenize yardımcı olmak için tasarlanmıştır. En iyi uygulama olarak, **sudo** izinleri olan hesaplar sınırlı olmalıdır.

```json
{
  "username":"myNewUser",
  "ssh_key":"ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQCZ3S7gGp3rcbKmG2Y4vGZFMuMZCwoUzZNG1vHY7P2XV2x9FfAhy8iGD+lF8UdjFX3t5ebMm6BnnMh8fHwkTRdOt3LDQq8o8ElTBrZaKPxZN2thMZnODs5Hlemb2UX0oRIGRcvWqsd4oJmxsXa/Si98Wa6RHWbc9QZhw80KAcOVhmndZAZAGR+Wq6yslNo5TMOr1/ZyQAook5C4FtcSGn3Y+WczaoGWIxG4ZaWk128g79VIeJcIQqOjPodHvQAhll7qDlItVvBfMOben3GyhYTm7k4YwlEdkONm4yV/UIW0la1rmyztSBQIm9sZmSq44XXgjVmDHNF8UfCZ1ToE4r2SdwTmZv00T2i5faeYnHzxiLPA3Enub7iUo5IdwFArnqad7MO1SY1kLemhX9eFjLWN4mJe56Fu4NiWJkR9APSZQrYeKaqru4KUC68QpVasNJHbuxPSf/PcjF3cjO1+X+4x6L1H5HTPuqUkyZGgDO4ynUHbko4dhlanALcriF7tIfQR9i2r2xOyv5gxJEW/zztGqWma/d4rBoPjnf6tO7rLFHXMt/DVTkAfn5woYtLDwkn5FMyvThRmex3BDf0gujoI1y6cOWLe9Y5geNX0oj+MXg/W0cXAtzSFocstV1PoVqy883hNoeQZ3mIGB3Q0rIUm5d9MA2bMMt31m1g3Sin6EQ== myNewUser@myVM",
  "password":"myNewUserPassword"
}
```

VMAccess komut dosyasını aşağıdakilerle çalıştırın:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings create_new_user.json
```

Bir kullanıcıyı silmek için, adlı `delete_user.json` bir dosya oluşturun ve aşağıdaki içeriği ekleyin. Parametre için kendi `remove_user` değerinizi değiştirin:

```json
{
  "remove_user":"myNewUser"
}
```

VMAccess komut dosyasını aşağıdakilerle çalıştırın:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings delete_user.json
```

### <a name="check-or-repair-the-disk"></a>Diski denetleme veya onarma
VMAccess'i kullanarak Linux VM'ye eklediğiniz bir diski kontrol edebilir ve onarabilirsiniz.

Diski denetlemek ve onarmak için, `disk_check_repair.json` adlı bir dosya oluşturun ve aşağıdaki biçimde ayarlar ekleyin. Kendi değerinizi aşağıdaki ler `repair_disk`için değiştirin:

```json
{
  "check_disk": "true",
  "repair_disk": "true, mydiskname"
}
```

VMAccess komut dosyasını aşağıdakilerle çalıştırın:

```azurecli-interactive
az vm extension set \
  --resource-group myResourceGroup \
  --vm-name myVM \
  --name VMAccessForLinux \
  --publisher Microsoft.OSTCExtensions \
  --version 1.4 \
  --protected-settings disk_check_repair.json
```
## <a name="troubleshoot-and-support"></a>Sorun giderme ve destek

### <a name="troubleshoot"></a>Sorun giderme

Uzantı lı dağıtımların durumuyla ilgili veriler Azure portalından ve Azure CLI kullanılarak alınabilir. Belirli bir VM uzantılarının dağıtım durumunu görmek için Azure CLI'yi kullanarak aşağıdaki komutu çalıştırın.

```azurecli
az vm extension list --resource-group myResourceGroup --vm-name myVM -o table
```

### <a name="support"></a>Destek

Bu makalenin herhangi bir noktasında daha fazla yardıma ihtiyacınız varsa, [MSDN Azure ve Yığın Taşma forumlarında](https://azure.microsoft.com/support/forums/)Azure uzmanlarıyla iletişime geçebilirsiniz. Alternatif olarak, bir Azure destek olayı dosyalayabilirsiniz. [Azure destek sitesine](https://azure.microsoft.com/support/options/) gidin ve destek al'ı seçin. Azure Desteği'ni kullanma hakkında daha fazla bilgi için [Microsoft Azure destek SSS'sini](https://azure.microsoft.com/support/faq/)okuyun.
