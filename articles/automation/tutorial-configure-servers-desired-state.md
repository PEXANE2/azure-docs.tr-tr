---
title: Azure Otomasyonu ile sunucuları istenen duruma göre yapılandırma ve kaymaları yönetme
description: Öğretici - Azure Otomasyon Durumu Yapılandırması ile sunucu yapılandırmalarını yönetme
services: automation
ms.subservice: dsc
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: a02c664ddf0802ad5ac306f98de14b7c0d5d7271
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678704"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Sunucuları istenilen duruma göre yapılandırma ve sürüklenme işlemlerini yönetme

Azure Otomasyon Durumu Yapılandırması, sunucularınız için yapılandırmalar belirtmenize ve bu sunucuların zaman içinde belirtilen durumda olduğundan emin olmaya olanak tanır.

> [!div class="checklist"]
> - Azure Automation DSC tarafından yönetilecek bir VM'de
> - Azure Otomasyonuna yapılandırma yükleme
> - Bir yapılandırmayı düğüm yapılandırmasına derleme
> - Yönetilen düğüme düğüm yapılandırması atama
> - Yönetilen düğümün uyumluluk durumunu denetleme

Bu öğretici için, IIS'nin VM'ye yüklenmesini sağlayan basit bir [DSC yapılandırması](/powershell/scripting/dsc/configurations/configurations) kullanıyoruz.

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
- Windows Server 2008 R2 veya daha yeni bir süre çalıştıran bir Azure Kaynak Yöneticisi VM (klasik değil). VM oluşturma yla ilgili talimatlar için [bkz.](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
- Azure PowerShell modülü sürümü 3.6 veya sonrası. Sürümü bulmak için `Get-Module -ListAvailable Az` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps).
- İstenilen Durum Yapılandırması (DSC) ile aşinalık. DSC hakkında daha fazla bilgi için [Windows PowerShell İstenen Durum Yapılandırmagenel Bakışı'na](/powershell/scripting/dsc/overview/overview)bakın.

## <a name="support-for-partial-configurations"></a>Kısmi yapılandırmalar için destek

Azure Otomasyon Durumu [Yapılandırması kısmi yapılandırmaların](/powershell/scripting/dsc/pull-server/partialconfigs)kullanımını destekler. Bu senaryoda, DSC birden çok yapılandırmayı bağımsız olarak yönetecek şekilde yapılandırılır ve her yapılandırma Azure Otomasyonundan alınır. Ancak, otomasyon hesabı başına düğüme yalnızca bir yapılandırma atanabilir. Bu, düğüm için iki yapılandırma kullanıyorsanız, iki Otomasyon hesabı gerektireceği anlamına gelir.

Çekme hizmetinden kısmi yapılandırmanın nasıl kaydedilenle ilgili ayrıntılar [için, kısmi yapılandırmalar](https://docs.microsoft.com/powershell/scripting/dsc/pull-server/partialconfigs#partial-configurations-in-pull-mode)için belgelere bakın.

Yapılandırmayı kod olarak kullanarak sunucuları işbirliği yle yönetmek için ekiplerin birlikte nasıl çalışabilecekleri hakkında daha fazla bilgi için, [DSC'nin CI/CD Pipeline'daki rolünü anlama'ya](/powershell/scripting/dsc/overview/authoringadvanced)bakın.

## <a name="log-in-to-azure"></a>Azure'da oturum açma

[Connect-AzAccount](https://docs.microsoft.com/powershell/module/Az.Accounts/Connect-AzAccount?view=azps-3.7.0) cmdlet ile Azure aboneliğinize giriş yapın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Azure Otomasyonu'na yapılandırma oluşturma ve yükleme


Bir metin düzenleyicisinde, aşağıdakileri yazın ve yerel olarak **TestConfig.ps1**olarak kaydedin.

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
> DSC Kaynakları sağlayan birden çok modülün içe aktarılmasını gerektiren daha gelişmiş senaryolarda, her modülün yapılandırmanızda benzersiz `Import-DscResource` bir çizgiye sahip olduğundan emin olun.

Yapılandırmayı Otomasyon hesabınıza yüklemek için [Import-AzAutomationDscConfiguration](https://docs.microsoft.com/powershell/module/Az.Automation/Import-AzAutomationDscConfiguration?view=azps-3.7.0) cmdlet'i arayın.

```powershell
 Import-AzAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Bir yapılandırmayı düğüm yapılandırmasına derleme

Bir düğüme atanabilmesi için dsc yapılandırması bir düğüm yapılandırması içine derlenmiş olmalıdır. [Bkz. DSC yapılandırmaları.](/powershell/scripting/dsc/configurations/configurations)

Yapılandırmayı Otomasyon hesabınızda adı geçen `TestConfig` `TestConfig.WebServer` bir düğüm yapılandırmasına derlemek için [Start-AzAutomationDscCompilationJob](https://docs.microsoft.com/powershell/module/Az.Automation/Start-AzAutomationDscCompilationJob?view=azps-3.7.0) cmdlet'i arayın.

```powershell
Start-AzAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>Devlet Yapılandırması tarafından yönetilecek bir VM kaydedin

Azure VM'leri (Hem Klasik hem de Kaynak Yöneticisi), şirket içi VM'leri, Linux makinelerini, AWS VM'leri ve şirket içi fiziksel makineleri yönetmek için Azure Otomasyon Durumu Yapılandırmasını kullanabilirsiniz. Bu konuda, yalnızca Azure Kaynak Yöneticisi VM'lerinin nasıl kaydedilebildiğini ele alıyoruz. Diğer makine türlerini kaydetme hakkında daha fazla bilgi için Azure [Otomasyon Durumu Yapılandırması tarafından yönetilen Onboarding makineleri](automation-dsc-onboarding.md)bölümüne bakın.

VM'inizi Azure Otomasyon Durumu Yapılandırması'na yönetilen bir düğüm olarak kaydetmek için [Register-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/Az.Automation/Register-AzAutomationDscNode?view=azps-3.7.0) cmdlet'i arayın. 

```powershell
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

### <a name="specify-configuration-mode-settings"></a>Yapılandırma modu ayarlarını belirtin

Bir VM'yi yönetilen düğüm olarak kaydetmek ve yapılandırma özelliklerini belirtmek için [Register-AzAutomationDscNode](/powershell/module/azurerm.automation/register-azurermautomationdscnode) cmdlet'i kullanın. Örneğin, `ApplyOnly` `ConfigurationMode` özelliğin değeri olarak belirterek makinenin durumunuyalnızca bir kez uygulanacak olduğunu belirtebilirsiniz. Durum Yapılandırması, yapılandırmayı ilk denetimden sonra uygulamaya çalışmaz.

```powershell
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

Ayrıca, Özelliği kullanarak DSC'nin yapılandırma durumunu `ConfigurationModeFrequencyMins` ne sıklıkta denetlediğinizi de belirtebilirsiniz. DSC yapılandırma ayarları hakkında daha fazla bilgi için [yerel yapılandırma yöneticisini yapılandırma ya](/powershell/scripting/dsc/managing-nodes/metaConfig)da yapılandırma ya da yapılandırma ya da yapılandırma ya da bkz.

```powershell
# Run a DSC check every 60 minutes
Register-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Yönetilen düğüme düğüm yapılandırması atama

Artık derlenmiş düğüm yapılandırmasını yapılandırmak istediğimiz VM'ye atayabiliriz.

```powershell
# Get the ID of the DSC node
$node = Get-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

Bu, kayıtlı DSC `TestConfig.WebServer` düğümüne adlandırılmış düğüm `DscVm`yapılandırmasını atar. Varsayılan olarak, DSC düğümü her 30 dakikada bir düğüm yapılandırmasıyla uyumluluk için denetlenir. Uyumluluk denetim aralığının nasıl değiştirilen hakkında bilgi için [bkz.](/powershell/scripting/dsc/managing-nodes/metaConfig)

## <a name="check-the-compliance-status-of-a-managed-node"></a>Yönetilen düğümün uyumluluk durumunu denetleme

Yönetilen düğümün uyumluluk durumu hakkında [Get-AzAutomationDscNodeReport](https://docs.microsoft.com/powershell/module/Az.Automation/Get-AzAutomationDscNodeReport?view=azps-3.7.0) cmdlet'i kullanarak raporlar alabilirsiniz.

```powershell
# Get the ID of the DSC node
$node = Get-AzAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeId $node.Id

# Display the most recent report
$reports[0]
```

## <a name="remove-nodes-from-service"></a>Düğümleri hizmetten kaldırma

Azure Otomasyon Durumu Yapılandırmasına düğüm eklediğinizde, Yerel Yapılandırma Yöneticisi'ndeki ayarlar hizmete kaydolacak ve yapılandırmaları ve makineyi yapılandırmak için gerekli modülleri çekecek şekilde ayarlanır.
Düğümü hizmetten kaldırmayı seçerseniz, bunu Azure portalını veya Az cmdlets'i kullanarak yapabilirsiniz.

> [!NOTE]
> Hizmetten bir düğümün silinmesi yalnızca Yerel Yapılandırma Yöneticisi ayarlarını ayarlar, böylece düğüm artık hizmete bağlanmaz.
> Bu, şu anda düğüme uygulanan yapılandırmayı etkilemez.
> Geçerli yapılandırmayı kaldırmak için [PowerShell'i](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1) kullanın veya yerel yapılandırma dosyasını silin (Linux düğümleri için tek seçenek budur).

### <a name="azure-portal"></a>Azure portal

Azure Otomasyonu'ndan, içindekiler tablosundaki **Durum yapılandırmasını (DSC)** tıklatın.
Hizmete kayıtlı düğümlerin listesini görüntülemek için sonraki tıklatma **düğümleri.**
Kaldırmak istediğiniz düğümün adını tıklatın.
Açılan Düğüm **görünümünde, Kaydı Nı Aç'ı**tıklatın.

### <a name="powershell"></a>PowerShell

PowerShell'i kullanarak Azure Automation State Configuration hizmetinden bir düğümün kaydını çıkarmak için cmdlet [Kayıt Dışı-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0)belgelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlarken](automation-dsc-getting-started.md)bkz.
- Düğümlere nasıl bindirilir öğrenince Azure [Otomasyon Durumu Yapılandırması tarafından yönetilen Yerleşik makinelere](automation-dsc-onboarding.md)bakın.
- DSC yapılandırmalarını hedef düğümlerine atayabilmeniz için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için Azure [Otomasyon Durumu Yapılandırma cmdlets'e](/powershell/module/azurerm.automation/#automation)bakın.
- Fiyatlandırma bilgileri için Azure [Otomasyon Durumu Yapılandırma fiyatlandırması'na](https://azure.microsoft.com/pricing/details/automation/)bakın.
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için [bkz.](automation-dsc-cd-chocolatey.md)
