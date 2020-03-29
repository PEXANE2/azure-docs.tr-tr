---
title: Trend Micro Deep Security'yi VM'ye yükleyin
description: Bu makalede, Azure'da Klasik dağıtım modeliyle oluşturulan bir VM'de Trend Micro güvenliğinin nasıl yüklenir ve yapılandırılatırılatır.
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
ms.openlocfilehash: cffd2eab3a616b4d16d847d0f2e1a26655f40459
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919932"
---
# <a name="how-to-install-and-configure-trend-micro-deep-security-as-a-service-on-a-windows-vm"></a>Trend Micro Deep Security'yi Windows VM'de Hizmet Olarak yükleme ve yapılandırma

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

[!INCLUDE [virtual-machines-extensions-deprecation-statement](../../../includes/virtual-machines-extensions-deprecation-statement.md)]
Bu makalede, Windows Server çalıştıran yeni veya varolan bir sanal makineye (VM) Hizmet olarak Trend Micro Deep Security'yi nasıl yükleyip yapılandırabileceğiniz gösterilmektedir. Hizmet Olarak Derin Güvenlik, kötü amaçlı yazılımdan koruma, güvenlik duvarı, izinsiz giriş önleme sistemi ve bütünlük izleme içerir.

İstemci VM Agent üzerinden bir güvenlik uzantısı olarak yüklenir. VM Aracısı Azure portalı tarafından otomatik olarak oluşturulduğundan, yeni bir sanal makinede Derin Güvenlik Aracısı'nı yüklersiniz.

Azure portalı, Azure CLI veya PowerShell kullanılarak oluşturulan varolan bir VM'de VM aracısı olmayabilir. VM Agent olmayan mevcut bir sanal makine için, önce karşıdan yüklemeniz ve yüklemeniz gerekir. Bu makale, her iki durumu da kapsar.

Şirket içi bir çözüm için Trend Micro'dan geçerli bir aboneliğiniz varsa, azure sanal makinelerinizi korumaya yardımcı olmak için bunu kullanabilirsiniz. Henüz müşteri değilseniz, deneme aboneliğiiçin kaydolabilirsiniz. Bu çözüm hakkında daha fazla bilgi için, Derin Güvenlik için Trend Micro blog yazısı [Microsoft Azure VM Agent Extension](https://go.microsoft.com/fwlink/p/?LinkId=403945)bakın.

## <a name="install-the-deep-security-agent-on-a-new-vm"></a>Derin Güvenlik Aracısını yeni bir VM'ye yükleme

[Azure portalı,](https://portal.azure.com) sanal makineyi oluşturmak için **Market'ten** bir görüntü kullandığınızda Trend Micro güvenlik uzantısını yüklemenize olanak tanır. Tek bir sanal makine oluşturuyorsanız, portalı kullanmak Trend Micro'ya karşı koruma eklemenin kolay bir yoludur.

**Market'ten** bir giriş kullanmak, sanal makineyi kurmanıza yardımcı olan bir sihirbaz açar. Trend Micro güvenlik uzantısını yüklemek için sihirbazın üçüncü paneli olan **Ayarlar** bıçağını kullanırsınız.  Genel yönergeler için bkz. [Azure portalında Windows çalıştıran sanal bir makine oluştur.](../windows/classic/tutorial.md)

Sihirbazın **Ayarlar** bıçağına ulaşıyorsanız, aşağıdaki adımları yapın:

1. **Uzantılar'ı**tıklatın, ardından bir sonraki bölmede **uzantı ekle'yi** tıklatın.

   ![Uzantıeklemeye başlayın][1]

2. **Yeni kaynak** bölmesinde Derin **Güvenlik Aracısı'nı** seçin. Derin Güvenlik Aracısı bölmesinde **Oluştur'u**tıklatın.

   ![Derin Güvenlik Aracısı'nı Tanımla][2]

3. Uzantı için **Kiracı Tanımlayıcısı** ve **Kiracı Etkinleştirme Parolasını** girin. İsteğe bağlı olarak, bir **Güvenlik İlkesi Tanımlayıcısı**girebilirsiniz. Ardından, istemciyi eklemek için **Tamam'ı** tıklatın.

   ![Uzantı ayrıntıları sağlama][3]

## <a name="install-the-deep-security-agent-on-an-existing-vm"></a>Derin Güvenlik Aracısını varolan bir VM'ye yükleme
Aracıyı varolan bir VM'ye yüklemek için aşağıdaki öğelere ihtiyacınız vardır:

* Azure PowerShell modülü, sürüm 0.8.2 veya daha yeni, yerel bilgisayarınızda yüklenir. Yüklemeyaptığınız Azure PowerShell **sürümünü, Get-Module azure | biçim-tablo sürümü** komutunu kullanarak kontrol edebilirsiniz. Talimatlar ve en son sürüme bağlantı için Azure [PowerShell'i nasıl yükleyip yapılandıracağınıza](/powershell/azure/overview)bakın. Azure aboneliğinizde oturum `Add-AzureAccount`açarak oturum açın.
* VM Agent hedef sanal makineyüklü.

İlk olarak, VM Aracısı'nın zaten yüklü olduğunu doğrulayın. Bulut hizmeti adını ve sanal makine adını doldurun ve ardından yönetici düzeyindeki Azure PowerShell komut isteminde aşağıdaki komutları çalıştırın. < ve > karakterler de dahil olmak üzere tırnak içindeki her şeyi değiştirin.

    $CSName = "<cloud service name>"
    $VMName = "<virtual machine name>"
    $vm = Get-AzureVM -ServiceName $CSName -Name $VMName
    write-host $vm.VM.ProvisionGuestAgent

Bulut hizmetini ve sanal makine adını bilmiyorsanız, geçerli aboneliğinizdeki tüm sanal makineler için bu bilgileri görüntülemek için **Get-AzureVM'u** çalıştırın.

**Write-host** komutu **True'yu**döndürürse, VM Aracısı yüklenir. **False**dönerse, Azure blog gönderisi [VM Agent](https://go.microsoft.com/fwlink/p/?LinkId=403947)and Extensions - Bölüm 2'de yönergeleri ve indirme bağlantısını görün.

VM Aracısı yüklüyse, bu komutları çalıştırın.

    $Agent = Get-AzureVMAvailableExtension TrendMicro.DeepSecurity -ExtensionName TrendMicroDSA

    Set-AzureVMExtension -Publisher TrendMicro.DeepSecurity –Version $Agent.Version -ExtensionName TrendMicroDSA -VM $vm | Update-AzureVM

## <a name="next-steps"></a>Sonraki adımlar
Aracının yüklendiğinde çalışmaya başlaması birkaç dakika sürer. Bundan sonra, bir Deep Security Manager tarafından yönetilebilmek için sanal makinede Deep Security etkinleştirmek gerekir. Ek talimatlar için aşağıdaki makalelere bakın:

* Trend'in bu çözümle ilgili makalesi, [Microsoft Azure için Anında Bulut Güvenliği](https://go.microsoft.com/fwlink/?LinkId=404101)
* Sanal makineyi yapılandırmak için örnek bir [Windows PowerShell komut dosyası](https://go.microsoft.com/fwlink/?LinkId=404100)
* Örnek için [talimatlar](https://go.microsoft.com/fwlink/?LinkId=404099)

## <a name="additional-resources"></a>Ek kaynaklar
[Windows Server çalıştıran sanal bir makinede oturum açma]

[Azure VM Uzantıları ve özellikleri]

<!-- Image references -->
[1]: ./media/trend/new_vm_Blade3.png
[2]: ./media/trend/find_SecurityAgent.png
[3]: ./media/trend/SecurityAgentDetails.png

<!-- Link references -->
[Windows Server çalıştıran sanal bir makinede oturum açma]:../windows/classic/connect-logon.md
[Azure VM Uzantıları ve özellikleri]: https://go.microsoft.com/fwlink/p/?linkid=390493&clcid=0x409
