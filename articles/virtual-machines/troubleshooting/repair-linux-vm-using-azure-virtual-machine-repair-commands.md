---
title: Azure Sanal Makine onarım komutlarını kullanarak bir Linux VM'sini onarın | Microsoft Dokümanlar
description: Bu makalede, diski herhangi bir hatayı gidermek ve ardından orijinal VM'nizi yeniden oluşturmak için diski başka bir Linux VM'ye bağlamak için Azure Sanal Makine onarım komutlarının nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır.
services: virtual-machines-linux
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.date: 09/10/2019
ms.author: v-miegge
ms.openlocfilehash: 49fdfde402938ce8d0ee1b141a47e68c99c502e7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73796213"
---
# <a name="repair-a-linux-vm-by-using-the-azure-virtual-machine-repair-commands"></a>Azure Sanal Makine onarım komutlarını kullanarak Linux VM'sini onarma

Azure'daki Linux sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, diskin kendisinde azaltma gerçekleştirmeniz gerekebilir. Yaygın bir örnek, VM'nin başarılı bir şekilde önyükleme yapabilmesini engelleyen başarısız bir uygulama güncelleştirmesi olacaktır. Bu makalede, diski herhangi bir hatayı gidermek ve ardından orijinal VM'nizi yeniden oluşturmak için diski başka bir Linux VM'ye bağlamak için Azure Sanal Makine onarım komutlarının nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır.

> [!IMPORTANT]
> Bu makaledeki komut dosyaları yalnızca [Azure Kaynak Yöneticisi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)kullanan VM'ler için geçerlidir.

## <a name="repair-process-overview"></a>Onarım sürecine genel bakış

Artık Bir VM için işletim sistemi diskini değiştirmek için Azure Sanal Makine onarım komutlarını kullanabilirsiniz ve artık VM'yi silip yeniden oluşturmanız gerekmez.

VM sorununu gidermek için aşağıdaki adımları izleyin:

1. Azure Cloud Shell'i başlatma
2. Az uzantısı ekleme/güncelleştirme yi çalıştırma
3. Çalıştır az vm onarım oluşturma
4. Azaltma adımlarını gerçekleştirin
5. Az vm onarım geri çalıştırın

Ek dokümantasyon ve talimatlar için [az vm onarımına](https://docs.microsoft.com/cli/azure/ext/vm-repair/vm/repair)bakın.

## <a name="repair-process-example"></a>Onarım işlemi örneği

1. Azure Cloud Shell'i başlatma

   Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Hesabınızla birlikte kullanmak üzere önceden yüklenmiş ve yapılandırılan ortak Azure araçlarını içerir.

   Bulut Kabuğunu açmak için, kod bloğunun sağ üst köşesinden **deneyin'i** seçin. Ayrıca cloud shell'i ayrı bir tarayıcı [https://shell.azure.com](https://shell.azure.com)sekmesinde açabilirsiniz.

   Kod **Copy** bloklarını kopyalamak için Kopyala'yı seçin, ardından kodu Bulut Kabuğu'na yapıştırın ve çalıştırmak için **Enter'u** seçin.

   CLI'yi yerel olarak yükleyip kullanmayı tercih ediyorsanız bu hızlı başlangıç için Azure CLI 2.0.30 veya sonraki bir sürümü gerekir. Sürümü bulmak için ``az --version`` komutunu çalıştırın. Azure CLI'nizi yüklemeniz veya yükseltmeniz gerekiyorsa, [bkz.](https://docs.microsoft.com/cli/azure/install-azure-cli)

2. Komutları ilk kez kullandıysanız, `az vm repair` vm-repair CLI uzantısını ekleyin.

   ```azurecli-interactive
   az extension add -n vm-repair
   ```

   Komutları daha önce `az vm repair` kullandıysanız, vm onarım uzantısına herhangi bir güncelleştirme uygulayın.

   ```azurecli-interactive
   az extension update -n vm-repair
   ```

3. `az vm repair create` öğesini çalıştırın. Bu komut, işlevsiz VM için işletim sistemi diskinin bir kopyasını oluşturur, bir onarım VM oluşturur ve diski ataştırır.

   ```azurecli-interactive
   az vm repair create -g MyResourceGroup -n myVM --repair-username username --repair-password password!234 --verbose
   ```

4. Oluşturulan onarım VM üzerinde gerekli azaltma adımlarını gerçekleştirin ve ardından adım 5'e geçin.

5. `az vm repair restore` öğesini çalıştırın. Bu komut, onarılan işletim sistemi diskini VM'nin özgün işletim sistemi diskiyle değiştirir.

   ```azurecli-interactive
   az vm repair restore -g MyResourceGroup -n MyVM --verbose
   ```

## <a name="verify-and-enable-boot-diagnostics"></a>Önyükleme tanılamasını doğrulayın ve etkinleştirin

Aşağıdaki örnek, adlı ``myVMDeployed`` ``myResourceGroup``kaynak grubunda adı geçen VM'deki tanı uzantısını sağlar:

Azure CLI

```azurecli-interactive
az vm boot-diagnostics enable --name myVMDeployed --resource-group myResourceGroup --storage https://mystor.blob.core.windows.net/
```

## <a name="next-steps"></a>Sonraki adımlar

* VM'nize bağlanmada sorun yaşıyorsanız, [Bir Azure Sanal Makinesi'ne bağlı SORUN Giderme RDP bağlantılarına](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-rdp-connection)bakın.
* VM'nizde çalışan uygulamalara erişimle ilgili sorunlar için [Azure'daki sanal makinelerdeki Sorun Giderme uygulaması bağlantısı sorunlarına](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-app-connection)bakın.
* Kaynak Yöneticisi'ni kullanma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)bilgisine bakın.
