---
title: Azure Sanal Makine onarım komutlarını kullanarak Bir Windows VM'i onarma | Microsoft Dokümanlar
description: Bu makalede, diski herhangi bir hatagidermek ve özgün VM'nizi yeniden oluşturmak için diski başka bir Windows VM'ye bağlamak için Azure VM onarım komutlarının nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: virtual-machines
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 09/10/2019
ms.author: v-miegge
ms.openlocfilehash: 2055558ef80a641084a7cf9d299281497d282936
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060683"
---
# <a name="repair-a-windows-vm-by-using-the-azure-virtual-machine-repair-commands"></a>Azure Sanal Makine onarım komutlarını kullanarak Windows VM'sini onarma

Azure'daki Windows sanal makineniz (VM) bir önyükleme veya disk hatasıyla karşılaşırsa, diskin kendisinde azaltma gerçekleştirmeniz gerekebilir. Yaygın bir örnek, VM'nin başarılı bir şekilde önyükleme yapabilmesini engelleyen başarısız bir uygulama güncelleştirmesi olacaktır. Bu makalede, diski herhangi bir hatagidermek ve özgün VM'nizi yeniden oluşturmak için diski başka bir Windows VM'ye bağlamak için Azure VM onarım komutlarının nasıl kullanılacağı ayrıntılı olarak açıklanmaktadır.

> [!IMPORTANT]
> Bu makaledeki komut dosyaları yalnızca [Azure Kaynak Yöneticisi](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)kullanan VM'ler için geçerlidir.

## <a name="repair-process-overview"></a>Onarım sürecine genel bakış

Artık Bir VM için işletim sistemi diskini değiştirmek için Azure VM onarım komutlarını kullanabilirsiniz ve artık VM'yi silip yeniden oluşturmanız gerekmez.

VM sorununu gidermek için aşağıdaki adımları izleyin:

1. Azure Cloud Shell'i başlatma
2. az uzantısı ekleme/güncelleştirmeyi çalıştırın.
3. Az vm onarım oluşturma çalıştırın.
4. Az vm onarım çalıştırın.
5. Az vm onarım geri çalıştırın.

Ek dokümantasyon ve talimatlar için [az vm onarımına](https://docs.microsoft.com/cli/azure/ext/vm-repair/vm/repair)bakın.

## <a name="repair-process-example"></a>Onarım işlemi örneği

> [!NOTE]
> * Komut dosyasının çalışması için VM'den (bağlantı noktası 443) giden bağlantı gereklidir.
> * Aynı anda yalnızca bir komut dosyası çalışabilir.
> * Çalışan bir komut dosyası iptal edilemez.
> * Bir komut dosyasının çalıştırabileceği maksimum süre 90 dakikadır ve ardından zaman ları dolacak.

1. Azure Cloud Shell'i başlatma

   Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur. Hesabınızla birlikte kullanmak üzere önceden yüklenmiş ve yapılandırılan ortak Azure araçlarını içerir.

   Bulut Kabuğunu açmak için, kod bloğunun sağ üst köşesinden **deneyin'i** seçin. Ayrıca bulut kabuğunu ayrı bir tarayıcı [https://shell.azure.com](https://shell.azure.com)sekmesinde ziyaret ederek açabilirsiniz.

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

4. `az vm repair run` öğesini çalıştırın. Bu komut, onarım VM üzerinden ekli diskte belirtilen onarım komutu çalıştıracaktır.

   ```azurecli-interactive
   az vm repair run  –g MyResourceGroup –n MyVM -–run-on-repair --run-id 2 --verbose
   ```

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

* VM'nize bağlanmada sorun yaşıyorsanız, [bir Azure VM'ye bağlı SORUN Giderme RDP bağlantılarına](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-rdp-connection)bakın.
* VM'nizde çalışan uygulamalara erişimle ilgili sorunlar için [Azure'daki sanal makinelerdeki Sorun Giderme uygulaması bağlantısı sorunlarına](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-app-connection)bakın.
* Kaynak Yöneticisi'ni kullanma hakkında daha fazla bilgi için [Azure Kaynak Yöneticisi'ne genel bakış](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview)bilgisine bakın.
