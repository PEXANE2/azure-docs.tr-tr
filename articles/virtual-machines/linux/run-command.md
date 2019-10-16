---
title: Azure 'da bir Linux VM 'de kabuk betikleri çalıştırma
description: Bu konuda, Çalıştır komutu kullanılarak Azure Linux sanal makinesi içinde betikleri çalıştırma açıklanmaktadır
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 04/26/2019
ms.topic: article
manager: carmonm
ms.openlocfilehash: 6550b6e3f59ff7e6bac39dfc1abcf829256122d4
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376349"
---
# <a name="run-shell-scripts-in-your-linux-vm-with-run-command"></a>Çalıştır komutuyla Linux sanal makinenizde kabuk betikleri çalıştırma

Çalıştır komutu, bir Azure Linux VM 'de kabuk betikleri çalıştırmak için VM aracısını kullanır. Bu betikler genel makine veya uygulama yönetimi için kullanılabilir ve VM erişimi ve ağ sorunlarını hızla tanılamak ve düzeltmek ve VM 'yi iyi bir duruma getirmek için kullanılabilir.

## <a name="benefits"></a>Avantajlar

Sanal makinelerinize erişmek için kullanılabilecek birden çok seçenek vardır. Çalıştır komutu, sanal makinelerinizdeki betikleri VM aracısını kullanarak Uzaktan çalıştırabilir. Run komutu, Linux VM 'Leri için Azure portal, [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand)veya [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) aracılığıyla kullanılabilir.

Bu özellik, bir sanal makine içinde bir komut dosyası çalıştırmak istediğiniz tüm senaryolarda faydalıdır ve hatalı ağ veya yönetici kullanıcı nedeniyle RDP veya SSH bağlantı noktası açık olmayan bir sanal makineyi düzeltme ve düzeltme yöntemlerinden biridir yapılandırmada.

## <a name="restrictions"></a>Kısıtlamalar

Aşağıda, Çalıştır komutu kullanılırken mevcut olan kısıtlamaların bir listesi verilmiştir.

* Çıktı, Son 4096 bayt ile sınırlıdır
* 20 saniye yaklaşık bir betiği çalıştırmak için gereken süre
* Betikler varsayılan olarak Linux üzerinde yükseltilmiş kullanıcı olarak çalışır
* Tek seferde bir betik çalıştırılabilir
* Bilgi isteyen betikler (etkileşimli mod) desteklenmez.
* Çalışan bir betiği iptal edemezsiniz
* Bir betiğin çalışacağı en uzun süre 90 dakikadır ve bu süre sonra zaman aşımına uğrar
* Betiğin sonuçlarını döndürmek için VM 'den giden bağlantı gereklidir.

> [!NOTE]
> Run komutu doğru çalışması için, Azure genel IP adresleri için bağlantı (bağlantı noktası 443) gerekir. Uzantının bu uç noktalara erişimi yoksa betikler başarıyla çalıştırılabilir, ancak sonuçları döndürmeyebilir. Sanal makinede trafiği engelliyorsanız, `AzureCloud` etiketini kullanarak Azure genel IP adreslerine giden trafiğe izin vermek için [hizmet etiketlerini](../../virtual-network/security-overview.md#service-tags) kullanabilirsiniz.

## <a name="available-commands"></a>Kullanılabilir komutlar

Bu tabloda, Linux VM 'Leri için kullanılabilen komutların listesi gösterilmektedir. **Runshellscript** komutu istediğiniz herhangi bir özel betiği çalıştırmak için kullanılabilir. Bir komutu çalıştırmak için Azure CLı veya PowerShell kullanırken, `--command-id` veya `-CommandId` parametresi için sağladığınız değer aşağıda listelenen değerlerden biri olmalıdır. Kullanılabilir bir komut olmayan bir değer belirttiğinizde, bu hatayı alırsınız.

```error
The entity was not found in this Azure location
```

|**Adı**|**Açıklama**|
|---|---|
|**RunShellScript**|Bir Linux kabuğu betiği yürütür.|
|**ifconfig**| Tüm ağ arabirimlerinin yapılandırmasını alın.|

## <a name="azure-cli"></a>Azure CLI

Aşağıda, bir Azure Linux sanal makinesinde kabuk betiği çalıştırmak için [az VM Run-Command](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) komutunu kullanan bir örnek verilmiştir.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

> [!NOTE]
> Komutları farklı bir kullanıcı olarak çalıştırmak için, kullanmak üzere bir kullanıcı hesabı belirtmek için `sudo -u` kullanabilirsiniz.

## <a name="azure-portal"></a>Azure portalı

[Azure](https://portal.azure.com) 'DA bir VM 'ye gidin ve **Işlemler**altında **komutu Çalıştır** ' ı seçin. SANAL makinede çalıştırılacak kullanılabilir komutların bir listesi görüntülenir.

![Komut listesini Çalıştır](./media/run-command/run-command-list.png)

Çalıştırmak için bir komut seçin. Bazı komutlardan isteğe bağlı veya gerekli giriş parametreleri olabilir. Bu komutlar için parametreler, giriş değerlerini sağlamanız için metin alanları olarak sunulur. Her komut için, **Görünüm betiği**genişleterek çalıştırılmakta olan betiği görüntüleyebilirsiniz. **Runshellscript** , kendi özel betiğinizi sağlamanıza olanak sağladığı diğer komutlardan farklıdır.

> [!NOTE]
> Yerleşik komutlar düzenlenemez.

Komut seçildikten sonra **Çalıştır** ' a tıklayarak betiği çalıştırın. Betik çalışır ve tamamlandığında çıkış penceresindeki çıktıyı ve hataları döndürür. Aşağıdaki ekran görüntüsünde **ifconfig** komutunu çalıştırmanın örnek bir çıkışı gösterilmektedir.

![Komut betiği çıkışını Çalıştır](./media/run-command/run-command-script-output.png)

### <a name="powershell"></a>PowerShell

Aşağıda, bir Azure VM üzerinde PowerShell Betiği çalıştırmak için [Invoke-AzVMRunCommand](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) cmdlet 'i kullanılan bir örnek verilmiştir. Cmdlet 'i, `-ScriptPath` parametresinde başvurulan betiğin, cmdlet 'in çalıştırıldığı yere yerelde olmasını bekler.

```powershell-interactive
Invoke-AzVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Çalıştır komutuna erişimi sınırlandırma

Çalıştırma komutlarının listelenmesi veya bir komutun ayrıntılarını göstermek, yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü ve üzeri olan abonelik düzeyinde `Microsoft.Compute/locations/runCommands/read` izni gerektirir.

Bir komutun çalıştırılması, abonelik düzeyinde, [sanal makine katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolü ve üst sürümü olan `Microsoft.Compute/virtualMachines/runCommand/action` iznini gerektirir.

Çalıştır komutunu kullanmak için [yerleşik](../../role-based-access-control/built-in-roles.md) rollerden birini kullanabilir veya [özel](../../role-based-access-control/custom-roles.md) bir rol oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Komut dosyalarını ve komutları sanal makinenizde uzaktan çalıştırmanın diğer yolları hakkında bilgi edinmek için bkz. [LINUX sanal makinenizde betikleri çalıştırın](run-scripts-in-vm.md) .
