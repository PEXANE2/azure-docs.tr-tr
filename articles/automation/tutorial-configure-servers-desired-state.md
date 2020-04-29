---
title: Azure Otomasyonu ile sunucuları istenen duruma göre yapılandırma ve kaymaları yönetme
description: Öğretici-Azure Otomasyonu durum yapılandırması ile sunucu yapılandırmalarını yönetme
services: automation
ms.subservice: dsc
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: a02c664ddf0802ad5ac306f98de14b7c0d5d7271
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81678704"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Sunucuları istenen bir duruma göre yapılandırma ve DRFT 'yi yönetme

Azure Otomasyonu durum yapılandırması sunucularınız için yapılandırmalar belirtmenize ve bu sunucuların zaman içinde belirtilen durumda olduğundan emin olmanızı sağlar.

> [!div class="checklist"]
> - Azure Automation DSC tarafından yönetilecek bir VM ekleme
> - Azure Otomasyonu 'na bir yapılandırma yükleme
> - Düğüm yapılandırmasında yapılandırma derleme
> - Yönetilen düğüme düğüm yapılandırması atama
> - Yönetilen bir düğümün uyumluluk durumunu denetleme

Bu öğreticide, IIS 'nin VM 'de yüklü olmasını sağlayan basit bir [DSC yapılandırması](/powershell/scripting/dsc/configurations/configurations) kullanırız.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
- Windows Server 2008 R2 veya üstünü çalıştıran bir Azure Resource Manager VM (klasik değil). VM oluşturma yönergeleri için [Azure Portal Ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)bölümüne bakın.
- Azure PowerShell modülü 3,6 veya sonraki bir sürümü. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps).
- Istenen durum yapılandırması (DSC) ile benzerlik. DSC hakkında daha fazla bilgi için bkz. [Windows PowerShell Istenen durum yapılandırmasına genel bakış](/powershell/scripting/dsc/overview/overview).

## <a name="support-for-partial-configurations"></a>Kısmi yapılandırma desteği

Azure Otomasyonu durum yapılandırması, [kısmi yapılandırmaların](/powershell/scripting/dsc/pull-server/partialconfigs)kullanımını destekler. Bu senaryoda DSC, birden fazla yapılandırmayı bağımsız olarak yönetmek üzere yapılandırılmıştır ve her Yapılandırma Azure Otomasyonu 'ndan alınır. Ancak, Otomasyon hesabı başına bir düğüme yalnızca bir yapılandırma atanabilir. Bu, bir düğüm için iki yapılandırma kullanıyorsanız iki Otomasyon hesabı zorunlu kılmasıdır.

Bir çekme hizmetinden kısmi bir yapılandırmanın nasıl kaydedileceği hakkında ayrıntılı bilgi için, [kısmi yapılandırmalar](https://docs.microsoft.com/powershell/scripting/dsc/pull-server/partialconfigs#partial-configurations-in-pull-mode)belgelerine bakın.

Yapılandırma olarak Configuration 'ı kullanarak sunucuları yönetmek için ekiplerin birlikte nasıl çalıştığı hakkında daha fazla bilgi için bkz. [BIR CI/CD ardışık DÜZENINDE DSC 'nin rolünü anlama](/powershell/scripting/dsc/overview/authoringadvanced).

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount?view=azps-3.7.0) cmdlet 'Ini kullanarak Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Azure Otomasyonu 'na bir yapılandırma oluşturma ve yükleme


Bir metin düzenleyicisinde aşağıdakini yazın ve **Testconfig. ps1**olarak yerel olarak kaydedin.

```powershell
configuration TestConfig {
   Node WebServer {
      WindowsFeature IIS {
         Ensure               = 'Present'
         Name                 = 'Web-Server'
         IncludeAllSubFeature = $true
      }
   }
}
```

> [!NOTE]
> DSC kaynaklarını sağlayan birden çok modülün içeri aktarılmasını gerektiren daha Gelişmiş senaryolarda, her modülün yapılandırmanızda benzersiz `Import-DscResource` bir satıra sahip olduğundan emin olun.

Yapılandırmayı Otomasyon hesabınıza yüklemek için [Import-AzAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/Import-AzAutomationDscConfiguration?view=azps-3.7.0) cmdlet 'ini çağırın.

```powershell
 Import-AzAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Düğüm yapılandırmasında yapılandırma derleme

Bir DSC yapılandırmasının bir düğüme atanabilmesi için önce düğüm yapılandırmasına derlenmesi gerekir. Bkz. [DSC yapılandırması](/powershell/scripting/dsc/configurations/configurations).

Yapılandırmayı Otomasyon hesabınızda adlı `TestConfig.WebServer` bir düğüm yapılandırması olarak derlemek için [Start-AzAutomationDscCompilationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Start-AzAutomationDscCompilationJob?view=azps-3.7.0) cmdlet 'ini çağırın. `TestConfig`

```powershell
Start-AzAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>Bir VM 'yi durum yapılandırması tarafından yönetilecek şekilde kaydetme

Azure Otomasyonu durum yapılandırması 'nı kullanarak Azure VM 'lerini (hem klasik hem de Kaynak Yöneticisi), şirket içi VM 'Lere, Linux makinelere, AWS VM 'lerine ve şirket içi fiziksel makinelere yönetebilirsiniz. Bu konu başlığında yalnızca Azure Resource Manager VM 'lerinin nasıl kaydedileceği ele alınmaktadır. Diğer makine türlerini kaydetme hakkında daha fazla bilgi için bkz. [Azure Otomasyonu durum yapılandırmasına göre yönetim için makine ekleme](automation-dsc-onboarding.md).

VM 'nizi Azure Otomasyonu durum yapılandırması ile yönetilen bir düğüm olarak kaydettirmek için [register-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationDscNode?view=azps-3.7.0) cmdlet 'ini çağırın. 

```powershell
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

### <a name="specify-configuration-mode-settings"></a>Yapılandırma modu ayarlarını belirtin

Bir VM 'yi yönetilen bir düğüm olarak kaydetmek ve yapılandırma özelliklerini belirtmek için [register-AzAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet 'ini kullanın. Örneğin, makinenin durumunun, `ApplyOnly` `ConfigurationMode` özelliğin değeri olarak belirtilerek yalnızca bir kez uygulanacağını belirtebilirsiniz. Durum Yapılandırması, ilk denetiminden sonra yapılandırmayı uygulamaya çalışır.

```powershell
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

Bu `ConfigurationModeFrequencyMins` ÖZELLIĞI kullanarak DSC 'nin yapılandırma durumunu ne sıklıkta denetleyeceğini de belirtebilirsiniz. DSC yapılandırma ayarları hakkında daha fazla bilgi için bkz. [yerel Configuration Manager yapılandırma](/powershell/scripting/dsc/managing-nodes/metaConfig).

```powershell
# Run a DSC check every 60 minutes
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Yönetilen düğüme düğüm yapılandırması atama

Artık derlenen düğüm yapılandırmasını yapılandırmak istediğimiz sanal makineye atayabiliriz.

```powershell
# Get the ID of the DSC node
$node = Get-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

Bu, kayıtlı DSC düğümüne `TestConfig.WebServer` `DscVm`adlı düğüm yapılandırmasını atar. Varsayılan olarak, DSC düğümü her 30 dakikada bir düğüm yapılandırmasıyla uyumluluk için denetlenir. Uyumluluk denetimi aralığını değiştirme hakkında daha fazla bilgi için bkz. [yerel Configuration Manager yapılandırma](/powershell/scripting/dsc/managing-nodes/metaConfig).

## <a name="check-the-compliance-status-of-a-managed-node"></a>Yönetilen bir düğümün uyumluluk durumunu denetleme

[Get-AzAutomationDscNodeReport](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationDscNodeReport?view=azps-3.7.0) cmdlet 'ini kullanarak, yönetilen bir düğümün uyumluluk durumu hakkında raporlar alabilirsiniz.

```powershell
# Get the ID of the DSC node
$node = Get-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeId $node.Id

# Display the most recent report
$reports[0]
```

## <a name="remove-nodes-from-service"></a>Düğümleri hizmetten kaldır

Azure Otomasyonu durum yapılandırmasına bir düğüm eklediğinizde yerel Configuration Manager ayarları, makineyi yapılandırmak için hizmet ve çekme yapılandırmalarına ve gerekli modüllere kaydolmak üzere ayarlanır.
Düğümü hizmetten kaldırmayı seçerseniz, Azure portal ya da az cmdlet 'lerini kullanarak bunu yapabilirsiniz.

> [!NOTE]
> Bir düğümün hizmetten kaydı, düğümün artık hizmete bağlanmaması için yalnızca yerel Configuration Manager ayarlarını ayarlar.
> Bu, düğüme uygulanmış olan yapılandırmayı etkilemez.
> Geçerli yapılandırmayı kaldırmak için [PowerShell](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1) 'i kullanın veya yerel yapılandırma dosyasını silin (Bu, Linux düğümlerine yönelik tek seçenektir).

### <a name="azure-portal"></a>Azure portal

Azure Otomasyonu ' ndan içindekiler tablosunda **Durum Yapılandırması (DSC)** seçeneğine tıklayın.
Ardından, hizmete kayıtlı düğümlerin listesini görüntülemek için **düğümler** ' e tıklayın.
Kaldırmak istediğiniz düğümün adına tıklayın.
Açılan düğüm görünümünde, **kayıt Sil**' e tıklayın.

### <a name="powershell"></a>PowerShell

PowerShell kullanarak Azure Otomasyonu durum yapılandırma hizmeti 'nden bir düğümün kaydını silmek için, [Unregister-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0)cmdlet 'inin belgelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için bkz. [Azure Otomasyonu durum yapılandırması ile çalışmaya](automation-dsc-getting-started.md)başlama.
- Düğümlerin nasıl ekleneceğini öğrenmek için bkz. [Azure Otomasyonu durum yapılandırmasına göre yönetim için makineleri ekleme](automation-dsc-onboarding.md).
- Hedef düğümlere atayabilmeniz için DSC yapılandırmalarını derleme hakkında bilgi edinmek için bkz. [Azure Otomasyonu durum yapılandırmasında yapılandırmaları derleme](automation-dsc-compile.md).
- PowerShell cmdlet başvurusu için bkz. [Azure Otomasyonu durum yapılandırması cmdlet 'leri](/powershell/module/azurerm.automation/#automation).
- Fiyatlandırma bilgileri için bkz. [Azure Otomasyonu durum yapılandırması fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/).
- Azure Otomasyonu durum yapılandırması 'nı sürekli bir dağıtım ardışık düzeninde kullanmaya ilişkin bir örnek görmek için bkz. [Azure Otomasyonu durum yapılandırması ve Chocolatey kullanarak sürekli dağıtım](automation-dsc-cd-chocolatey.md)
