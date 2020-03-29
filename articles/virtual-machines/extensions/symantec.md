---
title: Azure'da Windows VM'de Symantec Uç Nokta Koruması'nı yükleme
description: Klasik dağıtım modeliyle oluşturulan yeni veya mevcut bir Azure VM'de Symantec Endpoint Protection güvenlik uzantısını nasıl yükleyip yapılandıracaksınız öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz
manager: gwallace
editor: ''
tags: azure-service-management
ms.assetid: 19dcebc7-da6b-4510-907b-d64088e81fa2
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.topic: article
ms.date: 03/31/2017
ms.author: akjosh
ms.openlocfilehash: 63f9441d4df9551405c2ab2bf8c0c67d7de5753c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919915"
---
# <a name="how-to-install-and-configure-symantec-endpoint-protection-on-a-windows-vm"></a>Windows VM'de Symantec Endpoint Protection nasıl yüklenir ve yapılandırılır?
[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

Azure'un kaynakları oluşturmak ve onlarla çalışmak için iki farklı dağıtım modeli vardır: [Kaynak Yöneticisi ve Klasik.](../../azure-resource-manager/management/deployment-models.md) Bu makalede, Klasik dağıtım modeli kullanılarak kapsar. Microsoft, yeni dağıtımların çoğunun Resource Manager modelini kullanmasını önerir.

Bu makalede, Windows Server çalıştıran mevcut bir sanal makine (VM) üzerinde Symantec Endpoint Protection istemcisi yüklemek ve yapılandırmak nasıl gösterir. Bu tam istemci, virüs ve casus yazılım koruması, güvenlik duvarı ve izinsiz giriş önleme gibi hizmetleri içerir. İstemci VM Aracısı kullanılarak bir güvenlik uzantısı olarak yüklenir.

Şirket içi bir çözüm için Symantec'ten varolan bir aboneliğiniz varsa, azure sanal makinelerinizi korumak için kullanabilirsiniz. Henüz müşteri değilseniz, deneme aboneliğiiçin kaydolabilirsiniz. Bu çözüm hakkında daha fazla bilgi için [Microsoft'un Azure platformunda Symantec Endpoint Protection'a][Symantec]bakın. Bu sayfada ayrıca, zaten bir Symantec müşterisiyseniz istemciyi yüklemek için lisans bilgileri ve yönergeleri ne kadar bağlantı vardır.

## <a name="install-symantec-endpoint-protection-on-an-existing-vm"></a>Symantec Uç Nokta Koruması'nı varolan bir VM'ye yükleme
Başlamadan önce aşağıdakileri yapmanız gerekir:

* Azure PowerShell modülü, sürüm 0.8.2 veya sonraki sürüm, iş bilgisayarınızda. Yükleme yaptığınız Azure PowerShell sürümünü **Get-Module azure | biçimtablosu sürüm** komutuyla kontrol edebilirsiniz. Talimatlar ve en son sürüme bağlantı için Azure [PowerShell'in nasıl yüklenir ve yapılandırılabildiğini][PS]öğrenin. Azure aboneliğinizde oturum `Add-AzureAccount`açarak oturum açın.
* Azure Sanal Makine'de çalışan VM Aracısı.

İlk olarak, VM Aracısı'nın sanal makineye zaten yüklenmiş olduğunu doğrulayın. Bulut hizmeti adını ve sanal makine adını doldurun ve ardından yönetici düzeyindeki Azure PowerShell komut isteminde aşağıdaki komutları çalıştırın. < ve > karakterler de dahil olmak üzere tırnak içindeki her şeyi değiştirin.

> [!TIP]
> Bulut hizmetini ve sanal makine adlarını bilmiyorsanız, geçerli aboneliğinizdeki tüm sanal makinelerin adlarını listelemek için **AzureVM'u** çalıştırın.

```powershell
$CSName = "<cloud service name>"
$VMName = "<virtual machine name>"
$vm = Get-AzureVM -ServiceName $CSName -Name $VMName
write-host $vm.VM.ProvisionGuestAgent
```

**Write-host** komutu **True'yu**görüntülerse, VM Aracısı yüklenir. **False**görüntüleniyorsa, Azure blog gönderisi VM Agent and [Extensions'da][Agent]yönergeleri ve indirme bağlantısına bakın - Bölüm 2 .

VM Aracısı yüklüyse, Symantec Endpoint Protection aracısını yüklemek için bu komutları çalıştırın.

```powershell
$Agent = Get-AzureVMAvailableExtension -Publisher Symantec -ExtensionName SymantecEndpointProtection

Set-AzureVMExtension -Publisher Symantec –Version $Agent.Version -ExtensionName SymantecEndpointProtection \
    -VM $vm | Update-AzureVM
```

Symantec güvenlik uzantısının yüklendiğini ve güncel olduğunu doğrulamak için:

1. Sanal makinede oturum açın. Talimatlar için, [Windows Server Çalıştıran Sanal Makinede Nasıl Oturum Açarınız'a][Logon]bakın.
2. Windows Server 2008 R2 **için Symantec Endpoint Protection> başlat'ı**tıklatın. Windows Server 2012 veya Windows Server 2012 R2 için başlangıç ekranından **Symantec**yazın ve ardından **Symantec Endpoint Protection'ı**tıklatın.
3. **Durum-Symantec Uç Nokta Koruması** penceresinin **Durum** sekmesinden güncelleştirmeleri uygulayın veya gerekirse yeniden başlatın.

## <a name="additional-resources"></a>Ek kaynaklar
[Windows Server Çalıştıran Sanal Makinede Nasıl Oturum Açar][Logon]

[Azure VM Uzantıları ve Özellikleri][Ext]

<!--Link references-->
[Symantec]: https://www.symantec.com/connect/blogs/symantec-endpoint-protection-now-microsoft-azure

[Create]:../windows/classic/tutorial.md

[PS]: /powershell/azureps-cmdlets-docs

[Agent]: https://go.microsoft.com/fwlink/p/?LinkId=403947

[Logon]:../windows/classic/connect-logon.md

[Ext]: https://go.microsoft.com/fwlink/p/?linkid=390493
