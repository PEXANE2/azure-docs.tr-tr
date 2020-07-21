---
title: Azure Cloud Shell hızlı başlangıç-Bash
description: Azure Cloud Shell tarayıcınızda Bash komut satırını nasıl kullanacağınızı öğrenin.
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 03/12/2018
ms.author: damaerte
ms.openlocfilehash: 3001e814a5f31725bfc78385d2435bbb0e971d40
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86513030"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Bash için hızlı başlangıç Azure Cloud Shell

Bu belge [Azure portal](https://ms.portal.azure.com/)Azure Cloud Shell Bash 'in nasıl kullanılacağını açıklamaktadır.

> [!NOTE]
> Azure Cloud Shell hızlı başlangıçta bir [PowerShell](quickstart-powershell.md) de mevcuttur.

## <a name="start-cloud-shell"></a>Cloud Shell'i Başlatma
1. Azure portal üst gezintiden **Cloud Shell** başlatın. <br>
![Azure portal Azure Cloud Shell başlatmayı gösteren ekran görüntüsü.](media/quickstart/shell-icon.png)

2. Depolama hesabı ve Microsoft Azure dosyaları paylaşma oluşturmak için bir abonelik seçin.
3. "Depolama oluştur" u seçin

> [!TIP]
> Azure CLı için her oturumda otomatik olarak kimliğiniz doğrulanır.

### <a name="select-the-bash-environment"></a>Bash ortamını seçin
Ortam açılan pencerenin sol tarafındaki ortamın açılır penceresinin olduğunu kontrol edin `Bash` . <br>
![Azure Cloud Shell için bash ortamının nasıl seçileceğini gösteren ekran görüntüsü.](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Aboneliğinizi ayarlama
1. Erişiminiz olan abonelikleri listeleyin.
   ```azurecli-interactive
   az account list
   ```

2. Tercih ettiğiniz aboneliği ayarlayın:

   ```azurecli-interactive
   az account set --subscription 'my-subscription-name'
   ```

> [!TIP]
> Aboneliğiniz, kullanılarak gelecekteki oturumlarınız için hatırlanır `/home/<user>/.azure/azureProfile.json` .

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
WestUS içinde "MyRG" adlı yeni bir kaynak grubu oluşturun.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Linux VM oluşturma
Yeni kaynak grubunuzda bir Ubuntu sanal makinesi oluşturun. Azure CLı, SSH anahtarları oluşturacak ve VM 'leri bu dosyalarla ayarlamaya çalışacak. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Kullanarak `--generate-ssh-keys` , sanal makinenizde ve dizininizde ortak ve özel anahtarlar oluşturup ayarlamak Için Azure CLI bildirir `$Home` . Varsayılan olarak anahtarlar ve ' de Cloud Shell yerleştirilir `/home/<user>/.ssh/id_rsa` `/home/<user>/.ssh/id_rsa.pub` . `.ssh`Klasörünüz, bağlı dosya paylaşımınızda kalıcı olarak kullanılan 5 GB resimde kalıcıdır `$Home` .

Bu VM 'deki Kullanıcı adınız, Cloud Shell ($) ' de kullanılan Kullanıcı adınız olacaktır User@Azure: .

### <a name="ssh-into-your-linux-vm"></a>Linux sanal makinenize SSH
1. Azure portal arama çubuğunda VM adınızı arayın.
2. VM adınızı ve genel IP adresini almak için "Bağlan" a tıklayın. <br>
   ![S S H kullanarak bir Linux V M 'ye bağlanmayı gösteren ekran görüntüsü.](media/quickstart/sshcmd-copy.png)

3. Cmd ile sanal makinenize SSH `ssh` .
   ```
   ssh username@ipaddress
   ```

SSH bağlantısı kurulduktan sonra Ubuntu karşılama istemi ' ni görmeniz gerekir. <br>
![S S H bağlantısını kurduktan sonra Ubuntu başlatma ve hoş geldiniz isteminin gösterildiği ekran görüntüsü.](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Temizleme 
1. SSH oturumunuzla çıkış yapın.
   ```
   exit
   ```

2. Kaynak grubunuzu ve içindeki kaynakları silin.
   ```azurecli-interactive
   az group delete -n MyRG
   ```

## <a name="next-steps"></a>Sonraki adımlar
[Bash için kalıcı dosyalar hakkında bilgi edinin Cloud Shell](persisting-shell-storage.md) <br>
[Azure CLı hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure dosyaları depolama hakkında bilgi edinin](../storage/files/storage-files-introduction.md) <br>
