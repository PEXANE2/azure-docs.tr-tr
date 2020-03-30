---
title: Azure'da Windows VM'de PowerShell komut dosyalarını çalıştırma
description: Bu konu, Çalıştır Komutu özelliğini kullanarak Bir Azure Windows sanal makine içinde PowerShell komut larının nasıl çalıştırılabildiğini açıklar
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 04/26/2019
ms.topic: article
manager: carmonm
ms.openlocfilehash: fa7f72989d47499127714eddfa6b5e98aa80178c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73749234"
---
# <a name="run-powershell-scripts-in-your-windows-vm-by-using-run-command"></a>Windows VM'inizde Çalıştır Komutu'nu kullanarak PowerShell komutlarını çalıştırın

Çalıştır Komutu özelliği, Bir Azure Windows VM içinde PowerShell komut ları çalıştırmak için sanal makine (VM) aracısını kullanır. Bu komut dosyalarını genel makine veya uygulama yönetimi için kullanabilirsiniz. VM erişimini ve ağ sorunlarını hızlı bir şekilde tanılamanıza ve düzeltmenize ve VM'yi iyi bir duruma geri almanıza yardımcı olabilir.

 

## <a name="benefits"></a>Avantajlar

Sanal makinelerinize birden fazla şekilde erişebilirsiniz. Run Komutu, VM aracısını kullanarak sanal makinelerinizdeki komut dosyalarını uzaktan çalıştırabilir. Windows VM'ler için Azure portalı, [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand)veya [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) üzerinden Çalıştır Komutu'nu kullanırsınız.

Bu özellik, sanal bir makine içinde bir komut dosyası çalıştırmak istediğiniz tüm senaryolarda yararlıdır. Uygun olmayan ağ veya yönetimkullanıcı yapılandırması nedeniyle RDP veya SSH bağlantı noktası açık olmayan sanal bir makinenin sorun giderin ve düzeltilmesinin tek yollarından biridir.

## <a name="restrictions"></a>Kısıtlamalar

Run Komutu'nu kullanırken aşağıdaki kısıtlamalar geçerlidir:

* Çıktı son 4.096 bayt ile sınırlıdır.
* Komut dosyasını çalıştırmak için en az süre yaklaşık 20 saniyedir.
* Komut dosyaları Windows'da Sistem olarak çalıştırılır.
* Her seferinde bir komut dosyası çalıştırılabilir.
* Bilgi için istekte olan komut dosyaları (etkileşimli mod) desteklenmez.
* Çalışan bir komut dosyalarını iptal edemezsin.
* Bir komut dosyasının çalıştırabileceği maksimum süre 90 dakikadır. Ondan sonra, zaman dolacak.
* Komut dosyasının sonuçlarını döndürmek için VM'den giden bağlantı gereklidir.

> [!NOTE]
> Düzgün çalışması için Run Komutu, Azure ortak IP adreslerine bağlantı (bağlantı noktası 443) gerektirir. Uzantının bu uç noktalara erişimi yoksa, komut dosyaları başarılı bir şekilde çalışabilir, ancak sonuçları döndürmeyebilir. Sanal makinedeki trafiği engelliyorsanız, `AzureCloud` etiketi kullanarak Azure genel IP adreslerine trafik izni vermek için hizmet [etiketlerini](../../virtual-network/security-overview.md#service-tags) kullanabilirsiniz.

## <a name="available-commands"></a>Kullanılabilir komutlar

Bu tablo, Windows VM'ler için kullanılabilen komutların listesini gösterir. İstediğiniz özel komut dosyasını çalıştırmak için **RunPowerShellScript** komutunu kullanabilirsiniz. Bir komutu çalıştırmak için Azure CLI veya PowerShell'i kullanırken, `--command-id` `-CommandId` komut u veya parametre için sağladığınız değer aşağıdaki listelenen değerlerden biri olmalıdır. Kullanılabilir bir komut olmayan bir değer belirttiğiniz zaman, bu hatayı alırsınız:

```error
The entity was not found in this Azure location
```

|**Adı**|**Açıklama**|
|---|---|
|**RunPowerShellScript**|PowerShell komut dosyası çalıştırın.|
|**Uzaktan PS'yi Etkinleştir**|Makineyi uzaktan PowerShell'i etkinleştirecek şekilde yapılandırır.|
|**EnableAdminAccount**|Yerel yönetici hesabının devre dışı bırakıldığını ve etkinleştirilmesini sağlarsa denetler.|
|**Ipconfig**| TCP/IP'ye bağlı her bağdaştırıcı için IP adresi, alt ağ maskesi ve varsayılan ağ geçidi için ayrıntılı bilgiler gösterir.|
|**RDPAyarları**|Kayıt defteri ayarlarını ve etki alanı ilkesi ayarlarını denetler. Makine bir etki alanının parçasıysa veya ayarları varsayılan değerlere göre değiştirirse ilke eylemleri önerir.|
|**SıfırlamaRDPCert**|RDP dinleyicisine bağlı SSL sertifikasını kaldırır ve RDP dinleyici güvenliğini varsayılan olarak geri yükler. Sertifikayla ilgili herhangi bir sorun görürseniz bu komut dosyasını kullanın.|
|**SetRDPPort**|Uzak Masaüstü bağlantıları için varsayılan veya kullanıcı tarafından belirtilen bağlantı noktası numarasını ayarlar. Bağlantı noktasına gelen erişim için güvenlik duvarı kuralları sağlar.|

## <a name="azure-cli"></a>Azure CLI

Aşağıdaki örnekte, Azure Windows VM'de bir kabuk komut dosyası çalıştırmak için [az vm run-command komutu](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) kullanır.

```azurecli-interactive
# script.ps1
#   param(
#       [string]$arg1,
#       [string]$arg2
#   )
#   Write-Host This is a sample script with parameters $arg1 and $arg2

az vm run-command invoke  --command-id RunPowerShellScript --name win-vm -g my-resource-group \
    --scripts @script.ps1 --parameters "arg1=somefoo" "arg2=somebar"
```

## <a name="azure-portal"></a>Azure portalında

[Azure portalındaki](https://portal.azure.com) bir VM'ye gidin ve **OPERATIONS**altında **Çalıştır komutunu** seçin. VM'de çalıştırılabilen kullanılabilir komutların listesini görürsünüz.

![Komutlar listesi](./media/run-command/run-command-list.png)

Çalıştırmak için bir komut seçin. Komutlardan bazıları isteğe bağlı veya gerekli giriş parametrelerine sahip olabilir. Bu komutlar için parametreler, giriş değerlerini sağlamanız için metin alanları olarak sunulur. Her komut **için, Görünüm komut dosyasını**genişleterek çalıştırılan komut dosyasını görüntüleyebilirsiniz. **RunPowerShellScript** diğer komutlardan farklıdır, çünkü kendi özel komut unuzu sağlamanızı sağlar.

> [!NOTE]
> Yerleşik komutlar edinilemez.

Komutu seçtikten sonra, komut dosyasını çalıştırmak için **Çalıştır'ı** seçin. Komut dosyası bittikten sonra çıktıyı ve çıktı penceresindeki hataları döndürür. Aşağıdaki ekran görüntüsü **RDPSettings** komutunu çalıştıran bir örnek çıktı gösterir.

![Komut komut dosyası çıktısını çalıştırma](./media/run-command/run-command-script-output.png)

## <a name="powershell"></a>PowerShell

Aşağıdaki örnekte, Bir Azure VM'de PowerShell komut dosyası çalıştırmak için [Invoke-AzVMRunKomut](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) cmdlet'i kullanır. Cmdlet, parametrede başvurulan `-ScriptPath` komut dosyasının cmdlet'in çalıştırıldığı yere yerel olmasını bekler.

```azurepowershell-interactive
Invoke-AzVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Run Komutu'na erişimi sınırlama

Çalıştırma komutlarını listelemek veya bir komutun `Microsoft.Compute/locations/runCommands/read` ayrıntılarını göstermek için abonelik düzeyinde izin gerekir. Yerleşik [Reader](../../role-based-access-control/built-in-roles.md#reader) rolü ve daha yüksek düzeylerde bu izin vardır.

Bir komutu `Microsoft.Compute/virtualMachines/runCommand/action` çalıştırmak için abonelik düzeyinde izin gerekir. [Sanal Makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolü ve daha yüksek düzeylerde bu izne sahiptir.

[Yerleşik rollerden](../../role-based-access-control/built-in-roles.md) birini kullanabilir veya Run Komutu'nu kullanmak için özel bir [rol](../../role-based-access-control/custom-roles.md) oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

VM'nizde komut dosyalarını ve komutları uzaktan çalıştırmanın diğer yolları hakkında bilgi edinmek için [Windows VM'nizde komut ları çalıştır'a](run-scripts-in-vm.md)bakın.
