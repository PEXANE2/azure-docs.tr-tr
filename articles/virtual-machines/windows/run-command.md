---
title: Azure 'da Windows VM 'de PowerShell betikleri çalıştırma
description: Bu konuda, Çalıştır komutunu kullanarak bir Azure Windows sanal makinesi içinde PowerShell betikleri çalıştırma açıklanmaktadır
services: automation
ms.service: automation
author: bobbytreed
ms.author: robreed
ms.date: 04/26/2019
ms.topic: article
manager: carmonm
ms.openlocfilehash: 0a9a5e465e160da34a21f66fd7176a8fea5d1aac
ms.sourcegitcommit: 0576bcb894031eb9e7ddb919e241e2e3c42f291d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72376258"
---
# <a name="run-powershell-scripts-in-your-windows-vm-with-run-command"></a>Çalıştır komutuyla Windows sanal makinenizde PowerShell betikleri çalıştırma

Çalıştır komutu, bir Azure Windows sanal makinesi içinde PowerShell betikleri çalıştırmak için VM aracısını kullanır. Bu betikler genel makine veya uygulama yönetimi için kullanılabilir ve VM erişimi ve ağ sorunlarını hızla tanılamak ve düzeltmek ve VM 'yi iyi bir duruma getirmek için kullanılabilir.

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="benefits"></a>Avantajlar

Sanal makinelerinize erişmek için kullanılabilecek birden çok seçenek vardır. Çalıştır komutu, sanal makinelerinizdeki betikleri VM aracısını kullanarak Uzaktan çalıştırabilir. Çalıştır komutu, Windows VM 'Leri için Azure portal, [REST API](/rest/api/compute/virtual%20machines%20run%20commands/runcommand)veya [PowerShell](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) aracılığıyla kullanılabilir.

Bu özellik, bir sanal makine içinde bir komut dosyası çalıştırmak istediğiniz tüm senaryolarda faydalıdır ve hatalı ağ veya yönetici kullanıcı nedeniyle RDP veya SSH bağlantı noktası açık olmayan bir sanal makineyi düzeltme ve düzeltme yöntemlerinden biridir yapılandırmada.

## <a name="restrictions"></a>Kısıtlamalar

Çalıştır komutu kullanılırken aşağıdaki kısıtlamalar geçerlidir:

* Çıktı, Son 4096 bayt ile sınırlıdır
* Bir komut dosyasının çalıştırılacağı en kısa süre yaklaşık 20 saniyedir
* Windows 'da sistem olarak çalıştırılan betikler
* Tek seferde bir betik çalıştırılabilir
* Bilgi isteyen betikler (etkileşimli mod) desteklenmez.
* Çalışan bir betiği iptal edemezsiniz
* Bir betiğin çalışacağı en uzun süre 90 dakikadır ve bu süre sonra zaman aşımına uğrar
* Betiğin sonuçlarını döndürmek için VM 'den giden bağlantı gereklidir.

> [!NOTE]
> Run komutu doğru çalışması için, Azure genel IP adresleri için bağlantı (bağlantı noktası 443) gerekir. Uzantının bu uç noktalara erişimi yoksa betikler başarıyla çalıştırılabilir, ancak sonuçları döndürmeyebilir. Sanal makinede trafiği engelliyorsanız, `AzureCloud` etiketini kullanarak Azure genel IP adreslerine giden trafiğe izin vermek için [hizmet etiketlerini](../../virtual-network/security-overview.md#service-tags) kullanabilirsiniz.

## <a name="available-commands"></a>Kullanılabilir komutlar

Bu tabloda, Windows VM 'Leri için kullanılabilen komutların listesi gösterilmektedir. **Runpowershellscript** komutu istediğiniz herhangi bir özel betiği çalıştırmak için kullanılabilir. Bir komutu çalıştırmak için Azure CLı veya PowerShell kullanırken, `--command-id` veya `-CommandId` parametresi için sağladığınız değer aşağıda listelenen değerlerden biri olmalıdır. Kullanılabilir bir komut olmayan bir değer belirttiğinizde, bu hatayı alırsınız.

```error
The entity was not found in this Azure location
```

|**Adı**|**Açıklama**|
|---|---|
|**RunPowerShellScript**|Bir PowerShell betiği yürütür|
|**EnableRemotePS**|Makineyi uzak PowerShell 'i etkinleştirecek şekilde yapılandırır.|
|**EnableAdminAccount**|Yerel yönetici hesabının devre dışı olup olmadığını denetler ve bu durumda izin vermez.|
|**Komutunu**| TCP/IP 'ye erişen her bir bağdaştırıcı için IP adresi, alt ağ maskesi ve varsayılan ağ geçidi için ayrıntılı bilgileri gösterir.|
|**Rdpayarları**|Kayıt defteri ayarlarını ve etki alanı ilkesi ayarlarını denetler. Makine bir etki alanının parçasıysa veya ayarları varsayılan değerlere değiştirdiğinde ilke eylemlerini önerir.|
|**ResetRDPCert**|RDP dinleyicisine bağlı SSL sertifikasını kaldırır ve RDP dinleyicisi güvenliğini varsayılana geri yükler. Sertifikayla ilgili herhangi bir sorun görürseniz bu betiği kullanın.|
|**SetRDPPort**|Uzak Masaüstü bağlantıları için varsayılan veya Kullanıcı tarafından belirtilen bağlantı noktası numarasını ayarlar. Bağlantı noktasına gelen erişim için güvenlik duvarı kuralını sağlar.|

## <a name="azure-cli"></a>Azure CLI

Aşağıda, bir Azure Linux sanal makinesinde kabuk betiği çalıştırmak için [az VM Run-Command](/cli/azure/vm/run-command?view=azure-cli-latest#az-vm-run-command-invoke) komutunu kullanan bir örnek verilmiştir.

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

## <a name="azure-portal"></a>Azure portalı

[Azure](https://portal.azure.com) 'DA bir VM 'ye gidin ve **Işlemler**altında **komutu Çalıştır** ' ı seçin. SANAL makinede çalıştırılacak kullanılabilir komutların bir listesi görüntülenir.

![Komut listesini Çalıştır](./media/run-command/run-command-list.png)

Çalıştırmak için bir komut seçin. Bazı komutlardan isteğe bağlı veya gerekli giriş parametreleri olabilir. Bu komutlar için parametreler, giriş değerlerini sağlamanız için metin alanları olarak sunulur. Her komut için, **Görünüm betiği**genişleterek çalıştırılmakta olan betiği görüntüleyebilirsiniz. **Runpowershellscript** , kendi özel betiğinizi sağlamanıza olanak sağladığı diğer komutlardan farklıdır.

> [!NOTE]
> Yerleşik komutlar düzenlenemez.

Komut seçildikten sonra **Çalıştır** ' a tıklayarak betiği çalıştırın. Betik çalışır ve tamamlandığında çıkış penceresindeki çıktıyı ve hataları döndürür. Aşağıdaki ekran görüntüsünde, **Rdpsettings** komutunu çalıştırmanın örnek bir çıkışı gösterilmektedir.

![Komut betiği çıkışını Çalıştır](./media/run-command/run-command-script-output.png)

## <a name="powershell"></a>PowerShell

Aşağıda, bir Azure VM üzerinde PowerShell Betiği çalıştırmak için [Invoke-AzVMRunCommand](https://docs.microsoft.com/powershell/module/az.compute/invoke-azvmruncommand) cmdlet 'i kullanılan bir örnek verilmiştir. Cmdlet 'i, `-ScriptPath` parametresinde başvurulan betiğin, cmdlet 'in çalıştırıldığı yere yerelde olmasını bekler.

```azurepowershell-interactive
Invoke-AzVMRunCommand -ResourceGroupName '<myResourceGroup>' -Name '<myVMName>' -CommandId 'RunPowerShellScript' -ScriptPath '<pathToScript>' -Parameter @{"arg1" = "var1";"arg2" = "var2"}
```

## <a name="limiting-access-to-run-command"></a>Çalıştır komutuna erişimi sınırlandırma

Çalıştırma komutlarının listelenmesi veya bir komutun ayrıntılarını göstermek, yerleşik [okuyucu](../../role-based-access-control/built-in-roles.md#reader) rolü ve üzeri olan abonelik düzeyinde `Microsoft.Compute/locations/runCommands/read` izni gerektirir.

Bir komutun çalıştırılması, abonelik düzeyinde, [sanal makine katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) rolü ve üst sürümü olan `Microsoft.Compute/virtualMachines/runCommand/action` iznini gerektirir.

Çalıştır komutunu kullanmak için [yerleşik](../../role-based-access-control/built-in-roles.md) rollerden birini kullanabilir veya [özel](../../role-based-access-control/custom-roles.md) bir rol oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Komut dosyalarını ve komutları sanal makinenizde uzaktan çalıştırmanın diğer yolları hakkında bilgi edinmek için bkz. [WINDOWS sanal makinenizde betikleri çalıştırma](run-scripts-in-vm.md) .
