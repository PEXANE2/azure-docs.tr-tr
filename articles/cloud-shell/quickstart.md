---
title: Azure Bulut Kabuğu Quickstart - Bash
description: Azure Cloud Shell ile tarayıcınızda Bash komut satırını nasıl kullanacağınızı öğrenin.
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 03/12/2018
ms.author: damaerte
ms.openlocfilehash: 58d795a5aee79e4149864a79a923ce34950b31d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79458078"
---
# <a name="quickstart-for-bash-in-azure-cloud-shell"></a>Azure Cloud Shell'de Bash için hızlı başlangıç

Bu belge, [Azure portalında](https://ms.portal.azure.com/)Azure Bulut Shell'de Bash'in nasıl kullanılacağını ayrıntılarıyla anlatır.

> [!NOTE]
> [Azure Cloud Cloud Cloud Quickstart'ta](quickstart-powershell.md) bir PowerShell de mevcuttur.

## <a name="start-cloud-shell"></a>Cloud Shell'i Başlatma
1. Azure portalının en üst gezintisinden **Cloud Shell'i** başlatın. <br>
![](media/quickstart/shell-icon.png)

2. Depolama hesabı oluşturmak ve Microsoft Azure Dosyaları paylaşmak için bir abonelik seçin.
3. "Depolama Alanı Oluştur" seçeneğini belirleyin

> [!TIP]
> Her oturumda Azure CLI için otomatik olarak kimlik doğrulanırsınız.

### <a name="select-the-bash-environment"></a>Bash ortamını seçin
Kabuk penceresinin sol tarafından açılan ortamın . `Bash` <br>
![](media/quickstart/env-selector.png)

### <a name="set-your-subscription"></a>Aboneliğinizi ayarlama
1. Erişiminiz olan abonelikleri listele.
   ```azurecli-interactive
   az account list
   ```

2. Tercih ettiğiniz aboneliği ayarlayın: <br>
```azurecli-interactive
az account set --subscription 'my-subscription-name'
```

> [!TIP]
> Aboneliğiniz gelecekteki oturumlar için `/home/<user>/.azure/azureProfile.json`hatırlanacaktır.

### <a name="create-a-resource-group"></a>Kaynak grubu oluşturma
WestUS'ta "MyRG" adında yeni bir kaynak grubu oluşturun.
```azurecli-interactive
az group create --location westus --name MyRG
```

### <a name="create-a-linux-vm"></a>Linux VM oluşturma
Yeni kaynak grubunuzda bir Ubuntu VM oluşturun. Azure CLI, SSH anahtarlarını oluşturur ve VM'yi onlarla birlikte ayarlar. <br>

```azurecli-interactive
az vm create -n myVM -g MyRG --image UbuntuLTS --generate-ssh-keys
```

> [!NOTE]
> Azure `--generate-ssh-keys` CLI'nin VM ve dizininizde genel ve `$Home` özel anahtarlar oluşturması ve ayarlaması talimatını kullanmak. Varsayılan olarak tuşlar Cloud `/home/<user>/.ssh/id_rsa` Shell'e yerleştirilir ve `/home/<user>/.ssh/id_rsa.pub`. Klasörünüz, `.ssh` devam etmek için kullanılan ekli dosya paylaşımınızın `$Home`5 GB görüntüsünde kalıcıdır.

Bu VM'deki kullanıcı adınız Cloud Shell'deUser@Azure:kullanılan kullanıcı adınız olacaktır ($).

### <a name="ssh-into-your-linux-vm"></a>Linux VM'nize SSH
1. Azure portalı arama çubuğunda VM adınızı arayın.
2. VM adınızı ve genel IP adresinizi almak için "Bağlan"ı tıklayın. <br>
   ![](media/quickstart/sshcmd-copy.png)

3. `ssh` Cmd ile VM içine SSH.
   ```
   ssh username@ipaddress
   ```

SSH bağlantısını kurduktan sonra, Ubuntu karşılama istemini görmeniz gerekir. <br>
![](media/quickstart/ubuntu-welcome.png)

## <a name="cleaning-up"></a>Temizleme 
1. Ssh oturumunuzdan çıkın.
   ```
   exit
   ```

2. Kaynak grubunuzu ve içindeki kaynakları silin.
   ```azurecli-interactive
   az group delete -n MyRG
   ```

## <a name="next-steps"></a>Sonraki adımlar
[Cloud Shell'de Bash için kalıcı dosyalar hakkında bilgi edinin](persisting-shell-storage.md) <br>
[Azure CLI hakkında bilgi edinin](https://docs.microsoft.com/cli/azure/) <br>
[Azure Dosyaları depolama hakkında bilgi edinin](../storage/files/storage-files-introduction.md) <br>
