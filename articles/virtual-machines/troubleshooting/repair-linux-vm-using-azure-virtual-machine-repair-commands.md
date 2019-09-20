---
title: Azure sanal makine onarım komutlarını kullanarak bir Linux VM 'yi onarma | Microsoft Docs
description: Bu makalede, tüm hataları gidermek için Azure sanal makine onarım komutlarının, diski başka bir Linux VM 'ye bağlamak için nasıl kullanılacağı açıklanır ve ardından özgün VM 'nizi yeniden derleyin.
services: virtual-machines-linux
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 09/10/2019
ms.author: v-miegge
ms.openlocfilehash: 56e420f9641638bfa79ff077be73132b00b934ab
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71132097"
---
# <a name="repair-a-linux-vm-by-using-the-azure-virtual-machine-repair-commands"></a>Azure sanal makine onarım komutlarını kullanarak bir Linux VM 'yi onarma

Azure 'daki Linux sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, diskin kendisi üzerinde risk azaltma yapmanız gerekebilir. Ortak bir örnek, VM 'nin başarıyla önyükleme yapabilmesini önleyen başarısız bir uygulama güncelleştirmesidir. Bu makalede, tüm hataları gidermek için Azure sanal makine onarım komutlarının, diski başka bir Linux VM 'ye bağlamak için nasıl kullanılacağı açıklanır ve ardından özgün VM 'nizi yeniden derleyin.

> [!IMPORTANT]
> Bu makaledeki betikler yalnızca [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)kullanan VM 'ler için geçerlidir.

## <a name="repair-process-overview"></a>İşleme genel bakış

Artık bir VM 'nin işletim sistemi diskini değiştirmek için Azure sanal makine onarım komutlarını kullanabilir ve artık VM 'yi silip yeniden oluşturmanız gerekmez.

VM sorununun giderilmesi için aşağıdaki adımları izleyin:

1. Azure Cloud Shell'i başlatma
2. Çalıştır az Extension Add/Update
3. Çalıştır az VM Repair Create
4. Risk azaltma adımlarını gerçekleştirin
5. Çalıştır az VM Repair restore

Ek belgeler ve yönergeler için bkz. [az VM Repair](https://docs.microsoft.com/cli/azure/ext/vm-repair/vm/repair).

## <a name="repair-process-example"></a>İşlemi Onar örneği

1. Azure Cloud Shell'i başlatma

   Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Bu, önceden yüklenmiş ve hesabınızla kullanılmak üzere yapılandırılmış ortak Azure araçları içerir.

   Cloud Shell açmak için, bir kod bloğunun sağ üst köşesinden **dene** ' yi seçin. Ayrıca, ' a giderek [https://shell.azure.com](https://shell.azure.com)ayrı bir tarayıcı sekmesinde Cloud Shell de açabilirsiniz.

   Kod bloklarını kopyalamak için **Kopyala** ' yı seçin, sonra kodu Cloud Shell yapıştırın ve çalıştırmak için **ENTER** ' u seçin.

   CLI'yi yerel olarak yükleyip kullanmayı tercih ediyorsanız bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümü gerekir. Sürümü bulmak için ``az --version`` komutunu çalıştırın. Azure CLı 'nizi yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 'Yı yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli).

2. `az vm repair` Komutları ilk kez kullandıysanız VM-Repair CLI uzantısını ekleyin.

   ```azurecli-interactive
   az extension add -n vm-repair
   ```

   `az vm repair` Komutları daha önce kullandıysanız, tüm güncelleştirmeleri VM-onarım uzantısına uygulayın.

   ```azurecli-interactive
   az extension update -n vm-repair
   ```

3. `az vm repair create` öğesini çalıştırın. Bu komut, işlevsel olmayan VM için işletim sistemi diskinin bir kopyasını oluşturur, bir onarım sanal makinesi oluşturur ve diski ekler.

   ```azurecli-interactive
   az vm repair create -g MyResourceGroup -n myVM --repair-username username --repair-password password!234 --verbose
   ```

4. Oluşturulan onarım VM 'sinde gerekli risk azaltma adımlarını gerçekleştirin ve 5. adıma geçin.

5. `az vm repair restore` öğesini çalıştırın. Bu komut, onarılan işletim sistemi diskini VM 'nin orijinal işletim sistemi diskiyle takas eder.

   ```azurecli-interactive
   az vm repair restore -g MyResourceGroup -n MyVM --verbose
   ```

## <a name="verify-and-enable-boot-diagnostics"></a>Önyükleme tanılamayı doğrulama ve etkinleştirme

Aşağıdaki örnek, adlı ``myVMDeployed`` ``myResourceGroup``kaynak grubunda adlı sanal makinede tanılama uzantısını sunar:

Azure CLI

```azurecli-interactive
az vm boot-diagnostics enable --name myVMDeployed --resource-group myResourceGroup --storage https://mystor.blob.core.windows.net/
```

## <a name="next-steps"></a>Sonraki adımlar

* Sanal makinenize bağlanırken sorun yaşıyorsanız bkz. [Azure sanal makinesine YÖNELIK RDP bağlantılarında sorun giderme](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-rdp-connection).
* VM 'niz üzerinde çalışan uygulamalara erişme sorunları için bkz. [Azure 'da sanal makinelerde uygulama bağlantı sorunlarını giderme](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-app-connection).
* Kaynak Yöneticisi kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
