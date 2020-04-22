---
title: Azure'da Linux VM'de kabuk komut dosyaları çalıştırma
description: Bu konu, Run Command özelliğini kullanarak bir Azure Linux sanal makinesinde komut ların nasıl çalıştırılabildiğini açıklar
services: automation
ms.service: virtual-machines
author: bobbytreed
ms.author: robreed
ms.date: 04/26/2019
ms.topic: article
manager: carmonm
ms.openlocfilehash: 80fc33a93d4d83dad1e687b176b39728fc7e8807
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758612"
---
# <a name="run-shell-scripts-in-your-linux-vm-by-using-run-command"></a>Run Komutunu kullanarak Linux VM'nizde kabuk komutları çalıştırın

Run Command özelliği, bir Azure Linux VM içinde kabuk komutları çalıştırmak için sanal makine (VM) aracısını kullanır. Bu komut dosyalarını genel makine veya uygulama yönetimi için kullanabilirsiniz. VM erişimini ve ağ sorunlarını hızlı bir şekilde tanılamanıza ve düzeltmenize ve VM'yi iyi bir duruma geri almanıza yardımcı olabilir.

## <a name="benefits"></a>Yararları

Sanal makinelerinize birden fazla şekilde erişebilirsiniz. Run Komutu, VM aracısını kullanarak sanal makinelerinizdeki komut dosyalarını uzaktan çalıştırabilir. Linux VM'leri için Azure portalı, [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand)veya [Azure CLI](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) üzerinden Çalıştır Komutu'nu kullanırsınız.

Bu özellik, sanal bir makine içinde bir komut dosyası çalıştırmak istediğiniz tüm senaryolarda yararlıdır. Uygun olmayan ağ veya yönetimkullanıcı yapılandırması nedeniyle RDP veya SSH bağlantı noktası açık olmayan sanal bir makinenin sorun giderin ve düzeltilmesinin tek yollarından biridir.

## <a name="restrictions"></a>Kısıtlamalar

Run Komutu'nu kullanırken aşağıdaki kısıtlamalar geçerlidir:

* Çıktı son 4.096 bayt ile sınırlıdır.
* Komut dosyasını çalıştırmak için en az süre yaklaşık 20 saniyedir.
* Komut dosyaları varsayılan olarak Linux'ta yüksek bir kullanıcı olarak çalıştırılır.
* Aynı anda bir komut dosyası çalıştırabilirsiniz.
* Bilgi için istekte olan komut dosyaları (etkileşimli mod) desteklenmez.
* Çalışan bir komut dosyalarını iptal edemezsin.
* Bir komut dosyasının çalıştırabileceği maksimum süre 90 dakikadır. Bundan sonra, senaryo zaman dolacak.
* Komut dosyasının sonuçlarını döndürmek için VM'den giden bağlantı gereklidir.

> [!NOTE]
> Düzgün çalışması için Run Komutu, Azure ortak IP adreslerine bağlantı (bağlantı noktası 443) gerektirir. Uzantının bu uç noktalara erişimi yoksa, komut dosyaları başarılı bir şekilde çalışabilir, ancak sonuçları döndürmeyebilir. Sanal makinedeki trafiği engelliyorsanız, `AzureCloud` etiketi kullanarak Azure genel IP adreslerine trafik izni vermek için hizmet [etiketlerini](../../virtual-network/security-overview.md#service-tags) kullanabilirsiniz.

## <a name="available-commands"></a>Kullanılabilir komutlar

Bu tablo, Linux VM'leri için kullanılabilen komutların listesini gösterir. İstediğiniz özel komut dosyasını çalıştırmak için **RunShellScript** komutunu kullanabilirsiniz. Bir komutu çalıştırmak için Azure CLI veya PowerShell'i kullanırken, `--command-id` `-CommandId` komut u veya parametre için sağladığınız değer aşağıdaki listelenen değerlerden biri olmalıdır. Kullanılabilir bir komut olmayan bir değer belirttiğiniz zaman, bu hatayı alırsınız:

```error
The entity was not found in this Azure location
```

|**Adı**|**Açıklama**|
|---|---|
|**RunShellScript**|Bir Linux kabuk komut dosyası çalıştırın.|
|**Ifconfig**| Tüm ağ arabirimlerinin yapılandırmasını alır.|

## <a name="azure-cli"></a>Azure CLI

Aşağıdaki örnekte, Azure Linux VM'de bir kabuk komutdosyası çalıştırmak için [az vm run-command komutu](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) kullanır.

```azurecli-interactive
az vm run-command invoke -g myResourceGroup -n myVm --command-id RunShellScript --scripts "sudo apt-get update && sudo apt-get install -y nginx"
```

> [!NOTE]
> Komutları farklı bir kullanıcı olarak `sudo -u` çalıştırmak için bir kullanıcı hesabı belirtmek için girin.

## <a name="azure-portal"></a>Azure portal

[Azure portalındaki](https://portal.azure.com) bir VM'ye gidin ve **OPERATIONS**altında **Çalıştır komutunu** seçin. VM'de çalıştırılabilen kullanılabilir komutların listesini görürsünüz.

![Komutlar listesi](./media/run-command/run-command-list.png)

Çalıştırmak için bir komut seçin. Komutlardan bazıları isteğe bağlı veya gerekli giriş parametrelerine sahip olabilir. Bu komutlar için parametreler, giriş değerlerini sağlamanız için metin alanları olarak sunulur. Her komut **için, Görünüm komut dosyasını**genişleterek çalıştırılan komut dosyasını görüntüleyebilirsiniz. **RunShellScript** diğer komutlardan farklıdır, çünkü kendi özel komut unuzu sağlamanızı sağlar.

> [!NOTE]
> Yerleşik komutlar edinilemez.

Komutu seçtikten sonra, komut dosyasını çalıştırmak için **Çalıştır'ı** seçin. Komut dosyası bittikten sonra çıktıyı ve çıktı penceresindeki hataları döndürür. Aşağıdaki ekran görüntüsü **ifconfig** komutu çalıştıran bir örnek çıktı gösterir.

![Komut komut dosyası çıktısını çalıştırma](./media/run-command/run-command-script-output.png)

### <a name="powershell"></a>PowerShell

Aşağıdaki örnekte, Bir Azure VM'de PowerShell komut dosyası çalıştırmak için [Invoke-AzVMRunKomut](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) cmdlet'i kullanır. Cmdlet, parametrede başvurulan `-ScriptPath` komut dosyasının cmdlet'in çalıştırıldığı yere yerel olmasını bekler.

```powershell-interactive
Invoke-AzVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Run Komutu'na erişimi sınırlama

Çalıştırma komutlarını listelemek veya bir komutun `Microsoft.Compute/locations/runCommands/read` ayrıntılarını göstermek için abonelik düzeyinde izin gerekir. Yerleşik [Reader](../../role-based-access-control/built-in-roles.md#reader) rolü ve daha yüksek düzeylerde bu izin vardır.

Bir komutu `Microsoft.Compute/virtualMachines/runCommand/action` çalıştırmak için abonelik düzeyinde izin gerekir. [Sanal Makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolü ve daha yüksek düzeylerde bu izne sahiptir.

[Yerleşik rollerden](../../role-based-access-control/built-in-roles.md) birini kullanabilir veya Run Komutu'nu kullanmak için özel bir [rol](../../role-based-access-control/custom-roles.md) oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

VM'nizde komut dosyalarını ve komutları uzaktan çalıştırmanın diğer yolları hakkında bilgi edinmek için [Linux VM'nizde komutları çalıştır'a](run-scripts-in-vm.md)bakın.
