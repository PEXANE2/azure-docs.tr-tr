---
title: Azure Otomasyonu ile sunucuları istenen duruma göre yapılandırma ve kaymaları yönetme
description: Öğretici - Azure Otomasyon Durumu Yapılandırması ile sunucu yapılandırmalarını yönetme
services: automation
ms.subservice: dsc
ms.topic: conceptual
ms.date: 08/08/2018
ms.openlocfilehash: 9e2f04f59a56be6c516eb90de45fdf7327673086
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75416587"
---
# <a name="configure-servers-to-a-desired-state-and-manage-drift"></a>Sunucuları istenilen duruma göre yapılandırma ve sürüklenme işlemlerini yönetme

Azure Otomasyon Durumu Yapılandırması, sunucularınız için yapılandırmalar belirtmenize ve bu sunucuların zaman içinde belirtilen durumda olduğundan emin olmaya olanak tanır.

> [!div class="checklist"]
> - Azure Automation DSC tarafından yönetilecek bir VM'de
> - Azure Otomasyonuna yapılandırma yükleme
> - Bir yapılandırmayı düğüm yapılandırmasına derleme
> - Yönetilen düğüme düğüm yapılandırması atama
> - Yönetilen düğümün uyumluluk durumunu denetleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için aşağıdakiler gerekir:

- Azure Otomasyonu hesabı. Bir Azure Otomasyonu Garklı Çalıştır hesabı oluşturma yönergeleri için bkz. [Azure Farklı Çalıştır Hesabı](automation-sec-configure-azure-runas-account.md).
- Windows Server 2008 R2 veya daha yeni bir süre çalıştıran bir Azure Kaynak Yöneticisi VM (Klasik değil). VM oluşturma yönergeleri için bkz. [Azure portalında ilk Windows sanal makinenizi oluşturma](../virtual-machines/virtual-machines-windows-hero-tutorial.md)
- Azure PowerShell modülü sürümü 3.6 veya sonrası. Sürümü bulmak için `Get-Module -ListAvailable AzureRM` komutunu çalıştırın. Yükseltmeniz gerekirse, bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/azurerm/install-azurerm-ps).
- İstenilen Durum Yapılandırması (DSC) ile aşinalık. DSC hakkında daha fazla bilgi için [Windows PowerShell İstenen Durum Yapılandırması Yapılandırmagenel Bakış'a](/powershell/scripting/dsc/overview/overview) bakın

## <a name="log-in-to-azure"></a>Azure'da oturum açma

`Connect-AzureRmAccount` komutuyla Azure aboneliğinizde oturum açın ve ekrandaki yönergeleri izleyin.

```powershell
Connect-AzureRmAccount
```

## <a name="create-and-upload-a-configuration-to-azure-automation"></a>Azure Otomasyonu'na yapılandırma oluşturma ve yükleme

Bu öğretici için, IIS'nin VM'ye yüklenmesini sağlayan basit bir DSC yapılandırması kullanacağız.

DSC yapılandırmaları hakkında bilgi edinmek için bkz. [DSC yapılandırmaları](/powershell/scripting/dsc/configurations/configurations).

Bir metin düzenleyicisine aşağıdakileri yazıp `TestConfig.ps1` adıyla yerel ortamda kaydedin.

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

Yapılandırmayı `Import-AzureRmAutomationDscConfiguration` Otomasyon hesabınıza yüklemek için cmdlet'i arayın:

```powershell
 Import-AzureRmAutomationDscConfiguration -SourcePath 'C:\DscConfigs\TestConfig.ps1' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Published
```

## <a name="compile-a-configuration-into-a-node-configuration"></a>Bir yapılandırmayı düğüm yapılandırmasına derleme

Bir düğüme atanabilmesi için dsc yapılandırması bir düğüm yapılandırması içine derlenmiş olmalıdır.

Yapılandırmaları derleme hakkında daha fazla bilgi için Bkz. [DSC yapılandırmaları.](/powershell/scripting/dsc/configurations/configurations)

Yapılandırmayı `Start-AzureRmAutomationDscCompilationJob` bir düğüm yapılandırmasına `TestConfig` derlemek için cmdlet'i arayın:

```powershell
Start-AzureRmAutomationDscCompilationJob -ConfigurationName 'TestConfig' -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount'
```

Bu, Otomasyon hesabınızda `TestConfig.WebServer` adı geçen bir düğüm yapılandırması oluşturur.

## <a name="register-a-vm-to-be-managed-by-state-configuration"></a>Devlet Yapılandırması tarafından yönetilecek bir VM kaydedin

Azure VM'leri (Hem Klasik hem de Kaynak Yöneticisi), şirket içi VM'leri, Linux makinelerini, AWS VM'leri ve şirket içi fiziksel makineleri yönetmek için Azure Otomasyon Durumu Yapılandırmasını kullanabilirsiniz. Bu konuda, yalnızca Azure Kaynak Yöneticisi VM'lerinin nasıl kaydedilebildiğini ele alıyoruz. Diğer makine türlerini kaydetme hakkında daha fazla bilgi için Azure [Otomasyon Durumu Yapılandırması tarafından yönetilen Onboarding makineleri](automation-dsc-onboarding.md)bölümüne bakın.

VM'inizi `Register-AzureRmAutomationDscNode` Azure Otomasyon Durumu Yapılandırması'na kaydettirmek için cmdlet'i arayın.

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm'
```

Bu, belirtilen VM'yi Durum Yapılandırmasında yönetilen bir düğüm olarak kaydeder.

### <a name="specify-configuration-mode-settings"></a>Yapılandırma modu ayarlarını belirtin

Bir VM'yi yönetilen düğüm olarak kaydettiğinizde, yapılandırmanın özelliklerini de belirtebilirsiniz. Örneğin, **ConfigurationMode** özelliğinin değerini belirterek makinenin durumunun yalnızca bir kez uygulanacağı belirtebilirsiniz (DSC `ApplyOnly` ilk denetimden sonra yapılandırmayı uygulamaya çalışmaz) aşağıdakileri belirtebilirsiniz:

```powershell
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationMode 'ApplyOnly'
```

**ConfigurationModeFrequencyMins** özelliğini kullanarak DSC'nin yapılandırma durumunu ne sıklıkta denetlediğinizi de belirtebilirsiniz:

```powershell
# Run a DSC check every 60 minutes
Register-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -AzureVMName 'DscVm' -ConfigurationModeFrequencyMins 60
```

Yönetilen bir düğüm için yapılandırma özelliklerini ayarlama hakkında daha fazla bilgi için [bkz.](/powershell/module/azurerm.automation/register-azurermautomationdscnode)

DSC yapılandırma ayarları hakkında daha fazla bilgi için [yerel yapılandırma yöneticisini yapılandırma ya](/powershell/scripting/dsc/managing-nodes/metaConfig)da yapılandırma ya da yapılandırma ya da yapılandırma ya da bkz.

## <a name="assign-a-node-configuration-to-a-managed-node"></a>Yönetilen düğüme düğüm yapılandırması atama

Artık derlenmiş düğüm yapılandırmasını yapılandırmak istediğimiz VM'ye atayabiliriz.

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Assign the node configuration to the DSC node
Set-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeConfigurationName 'TestConfig.WebServer' -NodeId $node.Id
```

Bu, adlandırılmış `DscVm`kayıtlı `TestConfig.WebServer` DSC düğümüne adlandırılmış düğüm yapılandırmasını atar.
Varsayılan olarak, DSC düğümü her 30 dakikada bir düğüm yapılandırmasıyla uyumluluk için denetlenir.
Uyumluluk denetim aralığının nasıl değiştirilen hakkında bilgi için [bkz.](/powershell/scripting/dsc/managing-nodes/metaConfig)

## <a name="working-with-partial-configurations"></a>Kısmi Yapılandırmalarla Çalışma

Azure Otomasyon Durumu [Yapılandırması kısmi yapılandırmaların](/powershell/scripting/dsc/pull-server/partialconfigs)kullanımını destekler.
Bu senaryoda, DSC birden çok yapılandırmayı bağımsız olarak yönetecek şekilde yapılandırılır ve her yapılandırma Azure Otomasyonundan alınır.
Ancak, otomasyon hesabı başına düğüme yalnızca bir yapılandırma atanabilir.
Bu, düğüm için iki yapılandırma kullanıyorsanız, iki otomasyon hesabı gerektireceği anlamına gelir.

Çekme hizmetinden kısmi yapılandırmanın nasıl kaydedilenhakkında ayrıntılı bilgi [için, kısmi yapılandırmalar](https://docs.microsoft.com/powershell/scripting/dsc/pull-server/partialconfigs#partial-configurations-in-pull-mode)için belgelere bakın.

Ekiplerin kod olarak yapılandırmayı kullanarak sunucuları işbirliği içinde yönetmek için birlikte nasıl çalışabilecekleri hakkında daha fazla bilgi [için](/powershell/scripting/dsc/overview/authoringadvanced)bkz.

## <a name="check-the-compliance-status-of-a-managed-node"></a>Yönetilen düğümün uyumluluk durumunu denetleme

Cmdlet'i arayarak yönetilen düğümün uyumluluk `Get-AzureRmAutomationDscNodeReport` durumu hakkında raporlar alabilirsiniz:

```powershell
# Get the ID of the DSC node
$node = Get-AzureRmAutomationDscNode -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -Name 'DscVm'

# Get an array of status reports for the DSC node
$reports = Get-AzureRmAutomationDscNodeReport -ResourceGroupName 'MyResourceGroup' -AutomationAccountName 'myAutomationAccount' -NodeId $node.Id

# Display the most recent report
$reports[0]
```

## <a name="removing-nodes-from-service"></a>Düğümleri hizmetten kaldırma

Azure Otomasyon Durumu Yapılandırmasına düğüm eklediğinizde, Yerel Yapılandırma Yöneticisi'ndeki ayarlar hizmete kaydolacak ve yapılandırmaları ve makineyi yapılandırmak için gerekli modülleri çekecek şekilde ayarlanır.
Düğümü hizmetten kaldırmayı seçerseniz, bunu Azure portalını veya Az cmdlets'i kullanarak yapabilirsiniz.

> [!NOTE]
> Hizmetten bir düğümün silinmesi yalnızca Yerel Yapılandırma Yöneticisi ayarlarını ayarlar, böylece düğüm artık hizmete bağlanmaz.
> Bu, şu anda düğüme uygulanan yapılandırmayı etkilemez.
> Geçerli yapılandırmayı kaldırmak için [PowerShell'i](https://docs.microsoft.com/powershell/module/psdesiredstateconfiguration/remove-dscconfigurationdocument?view=powershell-5.1) kullanın veya yerel yapılandırma dosyasını silin (Linux düğümleri için tek seçenek budur).

### <a name="azure-portal"></a>Azure portalında

Azure Otomasyonu'ndan, içindekiler tablosundaki **Durum yapılandırmasını (DSC)** tıklatın.
Hizmete kayıtlı düğümlerin listesini görüntülemek için sonraki tıklatma **düğümleri.**
Kaldırmak istediğiniz düğümün adını tıklatın.
Açılan Düğüm **görünümünde, Kaydı Nı Aç'ı**tıklatın.

### <a name="powershell"></a>PowerShell

PowerShell'i kullanarak Azure Automation State Configuration hizmetinden bir düğümün kaydını çıkarmak için cmdlet [Kayıt Dışı-AzAutomationDscNode](https://docs.microsoft.com/powershell/module/az.automation/unregister-azautomationdscnode?view=azps-2.0.0)belgelerini izleyin.

## <a name="next-steps"></a>Sonraki adımlar

- Başlamak için Azure [Otomasyon Durumu Yapılandırması'na başlarken](automation-dsc-getting-started.md)
- Düğümlere nasıl bindirilir öğrenince Azure [Otomasyon Durumu Yapılandırması ile yönetim için Onboarding makineleri](automation-dsc-onboarding.md) bölümüne bakın
- DSC yapılandırmalarını hedef düğümlerine atamak için derleme hakkında bilgi edinmek için [bkz.](automation-dsc-compile.md)
- PowerShell cmdlet başvurusu için Azure [Otomasyon Durumu Yapılandırma cmdlet'ine](/powershell/module/azurerm.automation/#automation) bakın
- Fiyatlandırma bilgileri için Bkz. [Azure Otomasyon Durumu Yapılandırma fiyatlandırması](https://azure.microsoft.com/pricing/details/automation/)
- Sürekli dağıtım ardışık bir şekilde Azure Otomasyon Durumu Yapılandırmasını kullanma örneğini görmek için [bkz.](automation-dsc-cd-chocolatey.md)
