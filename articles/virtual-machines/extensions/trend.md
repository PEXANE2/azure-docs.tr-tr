---
title: Bir VM 'de eğilim mikro derin güvenliği yüklemesi
description: Bu makalede, Azure 'da klasik dağıtım modeliyle oluşturulan bir VM 'de Trend Micro Security 'nin nasıl yükleneceği ve yapılandırılacağı açıklanmaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: axayjo
manager: gwallace
editor: ''
tags: azure-service-management
ms.assetid: e991b635-f1e2-483f-b7ca-9d53e7c22e2a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.topic: article
ms.date: 04/20/2018
ms.author: akjosh
ms.openlocfilehash: c4a4f641283a9a4771a236ba844cfe296b31c28c
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87082687"
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Windows VM 'de bir hizmet olarak Trend mikro derin güvenliği 'ni yüklemek ve yapılandırmak

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

[!INCLUDE [virtual-machines-extensions-deprecation-statement](../../../includes/virtual-machines-extensions-deprecation-statement.md)]
Bu makalede, Windows Server çalıştıran yeni veya mevcut bir sanal makinede (VM) bir hizmet olarak Trend mikro derin güvenliği 'nin nasıl yükleneceği ve yapılandırılacağı gösterilmektedir. Hizmet olarak derin güvenlik, kötü amaçlı yazılımdan koruma, güvenlik duvarı, yetkisiz giriş önleme sistemi ve bütünlük izleme içerir.

İstemci, VM Aracısı aracılığıyla bir güvenlik uzantısı olarak yüklenir. Yeni bir sanal makinede, VM Aracısı Azure portal tarafından otomatik olarak oluşturulduğu için derin güvenlik aracısını yüklersiniz.

Azure portal, Azure CLı veya PowerShell kullanılarak oluşturulan mevcut bir VM, bir VM aracısına sahip olmayabilir. VM aracısına sahip olmayan var olan bir sanal makine için, önce indirmeniz ve yüklemeniz gerekir. Bu makalede her iki durum ele alınmaktadır.

Bir şirket içi çözüm için Trend Micro 'den geçerli bir aboneliğiniz varsa, Azure sanal makinelerinizi korumanıza yardımcı olması için kullanabilirsiniz. Henüz bir müşteriyseniz, bir deneme aboneliği için kaydolabilirsiniz. Bu çözüm hakkında daha fazla bilgi için, ayrıntılı güvenlik Için bkz. eğilim mikro blog gönderisi [MICROSOFT Azure VM Aracısı uzantısı](https://go.microsoft.com/fwlink/p/?LinkId=403945).

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Derin güvenlik aracısını yeni bir VM 'ye yükler

[Azure Portal](https://portal.azure.com) , sanal makineyi oluşturmak için **Market** 'Ten bir görüntü kullandığınızda eğilim mikro güvenlik uzantısını yüklemenizi sağlar. Tek bir sanal makine oluşturuyorsanız, portalı kullanmak Trend Micro 'ten koruma eklemenin kolay bir yoludur.

**Market** 'ten bir giriş kullanmak, sanal makineyi ayarlamanıza yardımcı olan bir sihirbaz açar. Trend mikro güvenlik uzantısını yüklemek için sihirbazın üçüncü paneli olan **Ayarlar** dikey penceresini kullanın.  Genel yönergeler için [Azure Portal Windows çalıştıran bir sanal makine oluşturma](../windows/quick-create-portal.md)bölümüne bakın.

Sihirbazın **Ayarlar** dikey penceresine geldiğinizde, aşağıdaki adımları uygulayın:

1. **Uzantılar**' a ve ardından sonraki bölmede **uzantı Ekle** ' ye tıklayın.

   ![Uzantıyı eklemeye başlayın][1]

2. **Yeni kaynak** bölmesinde **derin güvenlik Aracısı** ' nı seçin. Derin güvenlik Aracısı bölmesinde **Oluştur**' a tıklayın.

   ![Derin güvenlik aracısını tanımla][2]

3. Uzantı için **kiracı tanımlayıcısı** ve **kiracı etkinleştirme parolasını** girin. İsteğe bağlı olarak, bir **Güvenlik Ilkesi tanımlayıcı**girebilirsiniz. Ardından, istemciyi eklemek için **Tamam** ' ı tıklatın.

   ![Uzantı ayrıntılarını sağlayın][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Derin güvenlik aracısını var olan bir VM 'ye yükler
Aracıyı var olan bir VM 'ye yüklemek için aşağıdaki öğeler gereklidir:

* Yerel bilgisayarınızda yüklü olan Azure PowerShell modülü, sürüm 0.8.2 veya daha yeni bir sürüm. Yüklediğiniz Azure PowerShell sürümünü **Get-Module Azure | Format-Table sürümü** komutunu kullanarak kontrol edebilirsiniz. Yönergeler ve en son sürüme bağlantı için bkz. [Azure PowerShell nasıl yüklenir ve yapılandırılır](/powershell/azure/). Kullanarak Azure aboneliğinizde oturum açın `Add-AzureAccount` .
* Hedef sanal makinede yüklü VM Aracısı.

İlk olarak, VM aracısının zaten yüklü olduğunu doğrulayın. Bulut hizmeti adını ve sanal makine adını doldurup yönetici düzeyinde bir Azure PowerShell komut isteminde aşağıdaki komutları çalıştırın. < ve > karakterler dahil olmak üzere tırnak içindeki her şeyi değiştirin.

```azurepowershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

Bulut hizmeti ve sanal makine adını bilmiyorsanız, geçerli aboneliğinizdeki tüm sanal makineler için bu bilgileri göstermek üzere **Get-AzureVM** ' i çalıştırın.

**Write-Host** komutu **true**dönerse, VM Aracısı yüklenir. **Yanlış**döndürürse, Azure Blog Post [VM Aracısı ve uzantıları-Bölüm 2](https://go.microsoft.com/fwlink/p/?LinkId=403947)' de bulunan yönergeler ve indirme bağlantısına bakın.

VM Aracısı yüklüyse, bu komutları çalıştırın.

```azurepowershell
$Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM
```

## <a name="next-steps"></a>Sonraki adımlar
Aracının yüklendiğinde çalışmaya başlaması birkaç dakika sürer. Bundan sonra, derin Güvenlik Yöneticisi tarafından yönetilebilmesi için sanal makinede derin güvenliği etkinleştirmeniz gerekir. Ek yönergeler için aşağıdaki makalelere bakın:

* Bu çözümle ilgili eğilim makalesi, [Microsoft Azure Için anında Bulut güvenliği](https://go.microsoft.com/fwlink/?LinkId=404101)
* Sanal makineyi yapılandırmak için [örnek bir Windows PowerShell betiği](https://go.microsoft.com/fwlink/?LinkId=404100)
* Örnek için [yönergeler](https://go.microsoft.com/fwlink/?LinkId=404099)

## <a name="additional-resources"></a>Ek kaynaklar
[Windows Server çalıştıran bir sanal makinede oturum açma]

[Azure VM uzantıları ve özellikleri]

<!-- Image references -->
[1]: ./media/trend/new_vm_Blade3.png
[2]: ./media/trend/find_SecurityAgent.png
[3]: ./media/trend/SecurityAgentDetails.png

<!-- Link references -->
[Windows Server çalıştıran bir sanal makinede oturum açma]:../windows/classic/connect-logon.md
[Azure VM uzantıları ve özellikleri]: https://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
