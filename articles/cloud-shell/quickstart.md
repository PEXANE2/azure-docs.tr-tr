---
title: Hızlı başlangıç Azure Cloud Shell | Microsoft Docs
description: Azure Cloud Shell için hızlı başlangıç
services: ''
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/12/2018
ms.author: damaerte
ms.openlocfilehash: 8151013f263c6cf2f90e89fa1c3b0b3025f2ea38
ms.sourcegitcommit: d060947aae93728169b035fd54beef044dbe9480
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2019
ms.locfileid: "68741981"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Bash için hızlı başlangıç Azure Cloud Shell

Bu belge [Azure portal](https://ms.portal.azure.com/)Azure Cloud Shell Bash 'in nasıl kullanılacağını açıklamaktadır.

> [!NOTE]
> Azure Cloud Shell hızlı başlangıçta bir [PowerShell](quickstart-powershell.md) de mevcuttur.

## <a name="start-cloud-shell"></a>Cloud Shell Başlat
1. Azure portal üst gezintiden **Cloud Shell** başlatın. <br>
![](media/quickstart/shell-icon.png)

2. Depolama hesabı ve Microsoft Azure dosyaları paylaşma oluşturmak için bir abonelik seçin.
3. "Depolama oluştur" u seçin

> [!TIP]
> Azure CLı için her oturumda otomatik olarak kimliğiniz doğrulanır.

### <a name="select-the-bash-environment"></a>Bash ortamını seçin
Ortam açılan pencerenin sol tarafındaki ortamın açılır penceresinin `Bash`olduğunu kontrol edin. <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Aboneliğinizi ayarlama
1. Erişiminiz olan abonelikleri listeleyin.
   ```azurecli-interactive
   az account list
   ```

2. Tercih ettiğiniz aboneliği ayarlayın: <br>
```azurecli-interactive
az account set --subscription 'my-subscription-name'
```

> [!TIP]
> Aboneliğiniz, kullanılarak `/home/<user>/.azure/azureProfile.json`gelecekteki oturumlarınız için hatırlanır.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
WestUS içinde "MyRG" adlı yeni bir kaynak grubu oluşturun.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Linux VM'i oluşturma
Yeni kaynak grubunuzda bir Ubuntu sanal makinesi oluşturun. Azure CLı, SSH anahtarları oluşturacak ve VM 'leri bu dosyalarla ayarlamaya çalışacak. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Kullanarak `--generate-ssh-keys` , sanal makinenizde ve `$Home` dizininizde ortak ve özel anahtarlar oluşturup ayarlamak için Azure CLI bildirir. Varsayılan olarak anahtarlar `/home/<user>/.ssh/id_rsa` ve `/home/<user>/.ssh/id_rsa.pub`' de Cloud Shell yerleştirilir. Klasörünüz, bağlı dosya paylaşımınızda kalıcı `$Home`olarak kullanılan 5 GB resimde kalıcıdır. `.ssh`

Bu VM 'deki Kullanıcı adınız, Cloud Shell ($User@Azure:) ' de kullanılan Kullanıcı adınız olacaktır.

### <a name="ssh-into-your-linux-vm"></a>Linux sanal makinenize SSH
1. Azure portal arama çubuğunda VM adınızı arayın.
2. VM adınızı ve genel IP adresini almak için "Bağlan" a tıklayın. <br>
   ![](media/quickstart/sshcmd-copy.png)

3. `ssh` Cmd ile sanal makinenize SSH.
   ```
   ssh username@ipaddress
   ```

SSH bağlantısı kurulduktan sonra Ubuntu karşılama istemi ' ni görmeniz gerekir. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Temizleme 
1. SSH oturumunuzla çıkış yapın.
   ```azurecli-interactive
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
