---
title: Azure Otomasyonu Windows Karma Runbook Çalışanı
description: Bu makalede, yerel veri merkezinizde veya bulut ortamınızda Windows tabanlı bilgisayarlarda runbook çalıştırmak için kullanabileceğiniz bir Azure Otomasyon Karma Runbook Worker yükleme hakkında bilgi sağlar.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: 53dfe07ebd4925c96290db140b6e613c38eef564
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617331"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Windows Karma Runbook Worker dağıtma

Azure Otomasyonu'nun Karma Runbook Worker özelliğini, bu yerel kaynakları yönetmek için rolü barındıran bilgisayarda doğrudan ve çevredeki kaynaklara karşı runbook'ları çalıştırmak için kullanabilirsiniz. Azure Otomasyon, runbook'ları depolar ve yönetir ve bunları bir veya daha fazla belirlenmiş bilgisayara teslim eder. Bu makalede, Bir Windows makinesinde bir Karma Runbook Worker nasıl dağıtılanın açıklar.

Bir runbook çalışanını başarıyla dağıttıktan sonra, çalışma kitaplarınızı şirket içi veri merkezinizdeki veya diğer bulut ortamınızdaki işlemleri otomatikleştirmek üzere nasıl yapılandırabileceğinizi öğrenmek için [Bir Karma Runbook Çalışanı'nda Çalıştır runbook'larını](automation-hrw-run-runbooks.md) gözden geçirin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma Runbook Worker'ınızdaki Az modül yükleme yönergeleri için Azure [PowerShell Modül'üne](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0)bakın. Otomasyon hesabınız için, Azure Otomasyonu'nda Azure [PowerShell modüllerini nasıl güncelleştirebileceğinizi](automation-update-azure-modules.md)kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="windows-hybrid-runbook-worker-installation-and-configuration"></a>Windows Hybrid Runbook Worker yükleme ve yapılandırma

Bir Windows Karma Runbook Worker yüklemek ve yapılandırmak için aşağıdaki yöntemlerden birini kullanabilirsiniz.

* Azure Sanal M'ler için, Windows için [sanal makine uzantısını](../virtual-machines/extensions/oms-windows.md)kullanarak Windows için Günlük Analizi aracısını yükleyin. Uzantı, Azure sanal makinelerinde Log Analytics aracısını yükler ve azure kaynak yöneticisi şablonu veya PowerShell kullanarak sanal makineleri varolan bir Log Analytics çalışma alanına kaydeder. Aracı yüklendikten sonra, VM Otomasyon hesabınızdaki Karma Runbook İşçi grubuna eklenebilir. [El Ile dağıtım](#manual-deployment) bölümündeki 3 ve 4 adımlarına bakın.

* Windows bilgisayar yapılandırma işlemini tamamen otomatikleştirmek için Bir Otomasyon runbook'u kullanın. Bu, veri merkezinizdeki veya başka bir bulut ortamındaki makineler için önerilen yöntemdir.

* Azure olmayan VM'nizde Karma Runbook İşçisi rolünü el ile yüklemek ve yapılandırmak için adım adım yordamı izleyin.

> [!NOTE]
> İstenilen Durum Yapılandırması (DSC) ile Karma Runbook Worker rolünü destekleyen sunucuların yapılandırmasını yönetmek için sunucuları DSC düğümleri olarak eklemeniz gerekir.

### <a name="minimum-requirements-for-windows-hybrid-runbook-worker"></a>Windows Karma Runbook Worker için minimum gereksinimler

Windows Karma Runbook Worker için minimum gereksinimler şunlardır:

* Windows Server 2012 veya üzeri
* Windows PowerShell 5.1 veya sonrası ([WMF 5.1 indir](https://www.microsoft.com/download/details.aspx?id=54616))
* .NET Framework 4.6.2 veya sonraki sürümü
* İki çekirdek
* 4 GB RAM
* Bağlantı noktası 443 (giden)

### <a name="network-configuration"></a>Ağ yapılandırması

Karma Runbook Çalışması için daha fazla ağ gereksinimi almak için [ağınızı yapılandırma](automation-hybrid-runbook-worker.md#network-planning)ya da yapılandırma'ya bakın.

### <a name="server-onboarding-for-management-with-automation-dsc"></a>Otomasyon DSC ile yönetim için sunucu onboarding

DSC ile yönetim için onboarding sunucuları hakkında daha fazla bilgi için Azure [Automation DSC tarafından yönetilen Onboarding makineleri](automation-dsc-onboarding.md)ne büründü.

[Güncelleştirme Yönetimi çözümünüzü](../operations-management-suite/oms-solution-update-management.md) etkinleştirmek, Log Analytics çalışma alanınıza bağlı tüm Windows bilgisayarlarını, çözüme dahil olan runbook'ları desteklemek için Karma Runbook Çalışanı olarak otomatik olarak yapılandırır. Ancak, bu çalışan Otomasyon hesabınızda zaten tanımlanmış olan Hibrit Runbook İşçi gruplarına kayıtlı değildir.

### <a name="addition-of-the-computer-to-a-hybrid-runbook-worker-group"></a>Bilgisayarın Karma Runbook Worker grubuna eklenmesi

Çalışan bilgisayarı Otomasyon hesabınızdaki Karma Runbook İşçi grubuna ekleyebilirsiniz. Hem çözüm hem de Karma Runbook İşçi grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon runbook'larını desteklemeniz gerektiğini unutmayın. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

## <a name="automated-deployment"></a>Otomatik dağıtım

Hedef makinede, Windows Karma Çalışması rolünün yüklenmesini ve yapılandırmasını otomatikleştirmek için aşağıdaki adımları gerçekleştirin.

### <a name="step-1---download-the-powershell-script"></a>Adım 1 - PowerShell komut dosyasını indirin

[PowerShell](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker) **Galerisi'nden Yeni-OnPremiseHybridWorker.ps1** komut indirin. İndirme doğrudan Karma Runbook Worker rolünü çalıştıran bilgisayardan veya ortamınızdaki başka bir bilgisayardan olmalıdır. Komut dosyasını indirdiğinizde, dosyayı çalışanınıza kopyalayın. **New-OnPremiseHybridWorker.ps1** komut dosyası yürütme sırasında aşağıda açıklanan parametreleri kullanır.

| Parametre | Durum | Açıklama |
| --------- | ------ | ----------- |
| `AAResourceGroupName` | Zorunlu | Otomasyon hesabınızla ilişkili kaynak grubunun adı. |
| `AutomationAccountName` | Zorunlu | Otomasyon hesabınızın adı.
| `Credential` | İsteğe bağlı | Azure ortamında oturum açarken kullanılacak kimlik bilgileri. |
| `HybridGroupName` | Zorunlu | Bu senaryoyu destekleyen runbook'lar için hedef olarak belirttiğiniz Karma Runbook İşçi grubunun adı. |
| `OMSResourceGroupName` | İsteğe bağlı | Log Analytics çalışma alanı için kaynak grubunun adı. Bu kaynak grubu belirtilmemişse, `AAResourceGroupName` değeri kullanılır. |
| `SubscriptionID` | Zorunlu | Otomasyon hesabınızla ilişkili Azure aboneliğinin tanımlayıcısı. |
| `TenantID` | İsteğe bağlı | Otomasyon hesabınızla ilişkili kiracı kuruluşun tanımlayıcısı. |
| `WorkspaceName` | İsteğe bağlı | Log Analytics çalışma alanı adı. Log Analytics çalışma alanınız yoksa, komut dosyası bir tane oluşturur ve yapılandırır. |

> [!NOTE]
> Azure Otomasyonu, çözümleri etkinleştirirken yalnızca bir Log Analytics çalışma alanı ve Otomasyon hesabını bağlamak için belirli bölgeleri destekler. Desteklenen eşleme çiftleri listesi [için, Otomasyon hesabı ve Log Analytics çalışma alanı için Bölge eşleme](how-to/region-mappings.md)bölümüne bakın.

### <a name="step-2---open-windows-powershell-command-line-shell"></a>Adım 2 - Windows PowerShell komut satırı kabuğunu aç

**Windows PowerShell'i** Yönetici modunda **Başlangıç** ekranından açın.

### <a name="step-3---run-the-powershell-script"></a>Adım 3 - PowerShell komut dosyasını çalıştırın

PowerShell komut satırı kabuğunda, indirdiğiniz komut dosyasını içeren klasöre göz atın. Parametrelerin `AutomationAccountName`değerlerini değiştirin `AAResourceGroupName` `OMSResourceGroupName`, `HybridGroupName` `SubscriptionID`, `WorkspaceName`, , ve . O zaman senaryoyu çalıştır.

Komut dosyasını çalıştırdıktan sonra Azure ile kimlik doğrulamanız istenir. Abonelik Yöneticileri rolünün üyesi ve aboneliğin eş yöneticisi olan bir hesapla oturum açmanız gerekir.

```powershell-interactive
.\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
-OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
-SubscriptionID <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
```

### <a name="step-4---install-nuget"></a>Adım 4 - NuGet yükle

NuGet'i yüklemeyi ve Azure kimlik bilgilerinizle kimlik doğrulamayı kabul etmeniz istenir. En son NuGet sürümüne sahip değilseniz, [kullanılabilir NuGet Dağıtım Sürümlerinden](https://www.nuget.org/downloads)edinebilirsiniz.

### <a name="step-5---verify-the-deployment"></a>Adım 5 - Dağıtımı doğrulayın

Komut dosyası tamamlandıktan sonra, Karma İşçi Grupları sayfası yeni grubu ve üye sayısını gösterir. Varolan bir grupsa, üye sayısı artımlanır. Hibrit İşçi Grupları sayfasındaki listeden grubu seçebilir ve **Karma İşçi** döşemesini seçebilirsiniz. Karma İşçiler sayfasında, listelenen grubun her üyesini görebilirsiniz.

## <a name="manual-deployment"></a>El ile dağıtım

Hedef makinede, Otomasyon ortamınız için ilk iki adımı bir kez gerçekleştirin. Ardından, her alt bilgisayar için kalan adımları gerçekleştirin.

### <a name="step-1---create-a-log-analytics-workspace"></a>Adım 1 - Günlük Analizi çalışma alanı oluşturma

Log Analytics çalışma alanınız yoksa, çalışma alanını oluşturmadan önce [Azure Monitörü Günlüğü tasarım kılavuzunu](../azure-monitor/platform/design-logs-deployment.md) inceleyin.

### <a name="step-2---add-the-automation-solution-to-the-log-analytics-workspace"></a>Adım 2 - Log Analytics çalışma alanına Otomasyon çözümlerini ekleyin

Otomasyon çözümü, Karma Runbook Worker desteği de dahil olmak üzere Azure Otomasyonu için işlevsellik ekler. Çözümü Log Analytics çalışma alanınıza eklediğinizde, otomatik olarak aracı bilgisayara yüklediğiniz alt bileşenleri bir sonraki adımda açıklandığı gibi iter.

Çalışma alanınıza Otomasyon çözümlerini eklemek için aşağıdaki PowerShell cmdlet'i çalıştırın.

```powershell-interactive
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true -DefaultProfile <IAzureContextContainer>
```

### <a name="step-3---install-the-log-analytics-agent-for-windows"></a>Adım 3 - Windows için Log Analytics aracısını yükleyin

Windows için Log Analytics aracısı, bilgisayarları bir Azure Monitor Log Analytics çalışma alanına bağlar. Aracıyı bilgisayarınıza yüklediğinizde ve çalışma alanınıza bağladığınızda, Karma Runbook Worker için gereken bileşenleri otomatik olarak karşıdan yükler.

Aracıyı bilgisayara yüklemek [için, Windows bilgisayarlarını Azure Monitor günlüklerine bağlayın](../log-analytics/log-analytics-windows-agent.md)yönergeleri izleyin. Birden çok bilgisayarın ortamınıza birden çok iş için eklemesi için bu işlemi yineleyebilirsiniz.

Aracı birkaç dakika sonra Log Analytics çalışma alanınıza başarıyla bağlandığında, çalışma alanına sinyal verileri gönderdiğini doğrulamak için aşağıdaki sorguyu çalıştırabilirsiniz.

```kusto
Heartbeat 
| where Category == "Direct Agent" 
| where TimeGenerated > ago(30m)
```

Arama sonuçlarında, bilgisayarın bağlı olduğunu belirten ve hizmete rapor veren sinyal kayıtlarını görmeniz gerekir. Varsayılan olarak, her aracı, atanan çalışma alanına bir sinyal kaydı iler. 

Aracı yüklemesini ve kurulumunu tamamlamak için aşağıdaki adımları kullanın.

1. Çözeltinin ajan makinesine inmesini etkinleştirin. [Çalışma alanında Yerleşik makinelere](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account#onboard-machines-in-the-workspace)bakın.
2. Aracının Otomasyon çözümünü doğru şaindirdığını doğrulayın. 
3. Karma Runbook Worker sürümünü onaylamak için **C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation'a** göz atın ve **sürüm** alt klasörüne dikkat edin.

### <a name="step-4---install-the-runbook-environment-and-connect-to-azure-automation"></a>Adım 4 - Runbook ortamını yükleyin ve Azure Otomasyonuna bağlanın

Bir aracıyı Log Analytics çalışma alanına rapor yapacak şekilde yapılandırdığınızda, Otomasyon çözümü cmdlet içeren `HybridRegistration` PowerShell modüllerini `Add-HybridRunbookWorker` aşağı iter. Runbook ortamını bilgisayara yüklemek ve Azure Otomasyonu'na kaydetmek için bu cmdlet'i kullanın.

Administrator modunda bir PowerShell oturumu açın ve modülü almak için aşağıdaki komutları çalıştırın.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

Şimdi aşağıdaki `Add-HybridRunbookWorker` sözdizimini kullanarak cmdlet çalıştırın.

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Bu cmdlet için gerekli bilgileri Azure portalındaki Anahtarları Yönet sayfasından alabilirsiniz. Otomasyon hesabınızdaki Ayarlar **sayfasındaki Anahtarları** seçerek bu sayfayı açın.

![Anahtarları yönet sayfası](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* `GroupName` Parametre için, Karma Runbook Worker grubunun adını kullanın. Bu grup Otomasyon hesabında zaten varsa, geçerli bilgisayar bu gruba eklenir. Eğer bu grup yoksa, eklenir.
* `EndPoint` Parametre için, Anahtarları Yönet sayfasındaki **URL** girişini kullanın.
* `Token` Parametre için, Anahtarları Yönet sayfasındaki **BIRINCIL ERİşİm ANAHTARI** girişini kullanın.
* Gerekirse, yükleme `Verbose` hakkında ayrıntıları almak için parametreyi ayarlayın.

### <a name="step-5----install-powershell-modules"></a>Adım 5 - PowerShell modüllerini yükleyin

Runbook'lar Azure Otomasyon ortamınızda yüklü modüllerde tanımlanan etkinlikleri ve cmdlet'leri kullanabilir. Bu modüller şirket içi bilgisayarlara otomatik olarak dağıtılmamalıdır, bunları el ile yüklemeniz gerekir. Bunun istisnası Azure modülüdür. Bu modül varsayılan olarak yüklenir ve Azure Otomasyonu için tüm Azure hizmetleri ve etkinlikleri için cmdlets'e erişim sağlar.

Karma Runbook Worker özelliğinin birincil amacı yerel kaynakları yönetmek olduğundan, büyük olasılıkla bu kaynakları destekleyen modülleri, özellikle de modülü yüklemeniz `PowerShellGet` gerekir. Windows PowerShell modüllerini yükleme hakkında daha fazla bilgi için [Windows PowerShell'e](https://docs.microsoft.com/powershell/scripting/developer/windows-powershell)bakın.

Yüklenen modüller, karma işçinin otomatik olarak `PSModulePath` içe aktarabilmesi için ortam değişkeni tarafından başvurulan bir konumda olmalıdır. Daha fazla bilgi için [PSModulePath'deki Modülleri Yükle'ye](https://docs.microsoft.com/powershell/scripting/developer/module/installing-a-powershell-module?view=powershell-7)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitaplarınızı şirket içi veri merkezinizdeki veya diğer bulut ortamınızdaki işlemleri otomatikleştirmek için nasıl yapılandırılabildiğini öğrenmek için, [Karma Runbook Çalışanı'ndaki runbook'lara](automation-hrw-run-runbooks.md)bakın.
* Karma Runbook İşçilerini kaldırma yla ilgili talimatlar [için](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker)bkz.
* Karma Runbook Çalışanlarınızı nasıl gidererinizi öğrenmek için [Windows Karma Runbook Çalışanları Sorun Giderme'ye](troubleshoot/hybrid-runbook-worker.md#windows)bakın.
* Sorun giderme güncelleştirme yönetimi sorunları için ek adımlar için [bkz: Güncelleştirme Yönetimi: sorun giderme.](troubleshoot/update-management.md)
