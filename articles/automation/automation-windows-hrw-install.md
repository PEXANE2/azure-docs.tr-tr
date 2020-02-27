---
title: Azure Otomasyonu Windows Karma Runbook Çalışanı
description: Bu makale, yerel veri merkezinizdeki veya bulut ortamınızdaki Windows tabanlı bilgisayarlarda runbook 'ları çalıştırmak için kullanabileceğiniz bir Azure Otomasyonu karma Runbook Worker yükleme hakkında bilgi sağlar.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: 9f3e06f66996be4a2b43b64e6100c62a2fa41381
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77649968"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Windows karma runbook çalışanı dağıtma

Runbook 'u doğrudan rolü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamdaki kaynaklara karşı çalıştırmak için Azure Otomasyonu 'nun karma Runbook Worker özelliğini kullanabilirsiniz. Runbook 'lar Azure Otomasyonu 'nda depolanır ve yönetilir ve ardından bir veya daha fazla belirlenen bilgisayara gönderilir. Bu makalede karma Runbook Worker 'ın bir Windows makinesinde nasıl dağıtılacağı açıklanır.

Runbook Worker 'ı başarıyla dağıttıktan sonra, runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatikleştirmek üzere nasıl yapılandıracağınızı öğrenmek için [karma Runbook Worker 'daki runbook 'Ları Çalıştır](automation-hrw-run-runbooks.md) ' ı inceleyin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

>[!NOTE]
>Bu makale yeni Azure PowerShell Az modülünü kullanacak şekilde güncelleştirilmiştir. En azından Aralık 2020'ye kadar hata düzeltmeleri almaya devam edecek olan AzureRM modülünü de kullanmaya devam edebilirsiniz. Yeni Az modülüyle AzureRM'nin uyumluluğu hakkında daha fazla bilgi edinmek için bkz. [Yeni Azure PowerShell Az modülüne giriş](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-3.5.0). Karma runbook çalışanınız hakkında az Module yükleme yönergeleri için bkz. [Azure PowerShell modülünü yükleme](https://docs.microsoft.com/powershell/azure/install-az-ps?view=azps-3.5.0). Otomasyon hesabınız için, [Azure Otomasyonu 'nda Azure PowerShell modüllerini güncelleştirme](automation-update-azure-modules.md)' yi kullanarak modüllerinizi en son sürüme güncelleştirebilirsiniz.

## <a name="windows-hybrid-runbook-worker-installation-and-configuration"></a>Windows karma Runbook Worker yüklemesi ve yapılandırması

Bir Windows karma runbook çalışanı yüklemek ve yapılandırmak için aşağıdaki yöntemlerden birini kullanabilirsiniz.

* Azure VM 'Leri için Windows için [sanal makine uzantısı](../virtual-machines/extensions/oms-windows.md)' nı kullanarak windows için Log Analytics aracısını yüklersiniz. Uzantı Log Analytics aracısını Azure sanal makinelerine yükledikten sonra bir Azure Resource Manager şablonunu veya PowerShell 'i kullanarak sanal makineleri mevcut bir Log Analytics çalışma alanına kaydeder. Aracı yüklendikten sonra, aşağıdaki [el ile dağıtım](#manual-deployment) bölümünde **4. adımda** Otomasyon hesabınızdaki karma Runbook Worker grubuna sanal makine eklenebilir.

* Bir Windows bilgisayarı yapılandırma işlemini tamamen otomatik hale getirmek için bir Otomasyon Runbook 'u kullanın. Bu, veri merkezinizdeki veya diğer bulut ortamınızdaki makineler için önerilen yöntemdir.

* Azure dışı sanal makinenize karma Runbook Worker rolünü el ile yüklemek ve yapılandırmak için adım adım prosedürü izleyin.

> [!NOTE]
> Istenen durum yapılandırması (DSC) ile karma runbook çalışanı rolünü destekleyen sunucularınızın yapılandırmalarını yönetmek için, bunları DSC düğümleri olarak eklemeniz gerekir.

### <a name="minimum-requirements-for-windows-hybrid-runbook-worker"></a>Windows karma Runbook Worker için minimum gereksinimler

Bir Windows karma Runbook Worker için en düşük gereksinimler şunlardır:

* Windows Server 2012 veya üzeri
* Windows PowerShell 5,1 veya üzeri ([WMF 5,1 indirin](https://www.microsoft.com/download/details.aspx?id=54616))
* .NET Framework 4.6.2 veya sonraki sürümü
* İki çekirdek
* 4 GB RAM
* Bağlantı noktası 443 (giden)

### <a name="network-configuration"></a>Ağ yapılandırması

Karma Runbook Worker için daha fazla ağ gereksinimi almak üzere [ağınızı yapılandırma](automation-hybrid-runbook-worker.md#network-planning)konusuna bakın.

### <a name="server-onboarding-for-management-with-automation-dsc"></a>Automation DSC ile yönetim için sunucu ekleme

Sunucuları DSC ile yönetime ekleme hakkında daha fazla bilgi için bkz. [Azure Automation DSC yönetim için makineleri ekleme](automation-dsc-onboarding.md).

[Güncelleştirme yönetimi çözümünü](../operations-management-suite/oms-solution-update-management.md)etkinleştirirseniz, Log Analytics çalışma alanınıza bağlı tüm Windows bilgisayarları, bu çözüme dahil olan runbook 'ları destekleyecek şekilde otomatik olarak bir karma runbook çalışanı olarak yapılandırılır. Ancak, Otomasyon hesabınızda önceden tanımlanmış olan karma çalışan gruplarıyla kayıtlı değildir.

### <a name="adding-the-computer-to-a-hybrid-runbook-worker-group"></a>Bilgisayar karma Runbook Worker grubuna ekleniyor

Bilgisayar, hem çözüm hem de karma runbook çalışanı grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon Runbook 'larını desteklemek üzere otomasyon hesabınızdaki karma Runbook Worker grubuna eklenebilir. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

## <a name="automated-deployment"></a>Otomatik dağıtım

Hedef makinede, Windows hibrit çalışan rolünün yükleme ve yapılandırmasını otomatik hale getirmek için aşağıdaki adımları gerçekleştirin.

### <a name="1-download-the-powershell-script"></a>1. PowerShell betiğini indirin

Karma Runbook Worker rolünü çalıştıran bilgisayardan veya ortamınızda bulunan başka bir bilgisayardan doğrudan [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker) New-OnPremiseHybridWorker. ps1 betiğini indirin. Betiği çalışana kopyalayın. New-OnPremiseHybridWorker. ps1 betiği yürütme sırasında aşağıdaki parametreleri gerektirir:

* *Aaresourcegroupname* (zorunlu): Otomasyon hesabınızla ilişkili kaynak grubunun adı.
* *Omsresourcegroupname* (isteğe bağlı): Log Analytics çalışma alanı için kaynak grubunun adı. Bu kaynak grubu belirtilmemişse, *Aaresourcegroupname* kullanılır.
* *SubscriptionID* (zorunlu): Otomasyon hesabınızın bulunduğu Azure abonelik kimliği.
* *Tenantıd* (isteğe bağlı): Otomasyon hesabınızla ilişkili kiracı kuruluşun tanıtıcısı.
* *Çalışmaalanıadı* (isteğe bağlı): Log Analytics çalışma alanı adı. Log Analytics çalışma alanınız yoksa, betik bir tane oluşturur ve yapılandırır.
* *Automationaccountname* (zorunlu): Otomasyon hesabınızın adı.
* *Hybridgroupname* (zorunlu): Bu senaryoyu destekleyen runbook 'lar için hedef olarak belirttiğiniz bir karma Runbook Worker grubunun adı.
* *Kimlik bilgileri* (isteğe bağlı): Azure ortamında oturum açarken kullanılacak kimlik bilgileri.
  
> [!NOTE]
> Çözümleri etkinleştirirken, bir Log Analytics çalışma alanı ve Otomasyon hesabı bağlamak için yalnızca belirli bölgeler desteklenir. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

### <a name="2-open-windows-powershell-command-line-shell"></a>2. Windows PowerShell komut satırı kabuğu 'nu açın

**Windows PowerShell** 'i yönetici modundaki **Başlangıç** ekranından açın.

### <a name="3-run-the-powershell-script"></a>3. PowerShell betiğini çalıştırın

PowerShell komut satırı kabuğundan, indirdiğiniz betiği içeren klasöre gidin. *-Automationaccountname*, *-aaresourcegroupname*, *-omsresourcegroupname*, *-hybridgroupname*, *-SubscriptionID*ve *-çalışmaparametreleri*için değerleri değiştirin. Sonra betiği çalıştırın.

Betiği çalıştırdıktan sonra Azure ile kimlik doğrulaması yapmanız istenir. Abonelik yöneticileri rolünün üyesi ve aboneliğin ortak Yöneticisi olan bir hesapla oturum açmalısınız.

```powershell-interactive
.\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
-OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
-SubscriptionId <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
```

### <a name="4-install-nuget"></a>4. NuGet 'i yükler

NuGet 'i yüklemek ve Azure kimlik bilgilerinizle kimlik doğrulaması yapmak için kabul etmeniz istenir. En son NuGet sürümüne sahip değilseniz, [kullanılabilir NuGet dağıtım sürümlerinden](https://www.nuget.org/downloads)edinebilirsiniz.

### <a name="5-verify-the-deployment"></a>5. dağıtımı doğrulama

Betik tamamlandıktan sonra, **karma çalışan grupları** sayfasında yeni grup ve üye sayısı gösterilir. Mevcut bir gruptur, üye sayısı artırılır. **Karma çalışan grupları** sayfasında listeden grubu seçebilir ve **hibrit çalışanlar** kutucuğunu seçebilirsiniz. **Karma çalışanlar** sayfasında, grubun her bir üyesini listede görürsünüz.

## <a name="manual-deployment"></a>El ile dağıtım

Hedef makinede, Otomasyon ortamınız için bir kez ilk iki adımı gerçekleştirin. Ardından her çalışan bilgisayar için kalan adımları gerçekleştirin.

### <a name="1-create-a-log-analytics-workspace"></a>1. Log Analytics çalışma alanı oluşturma

Zaten bir Log Analytics çalışma alanınız yoksa, önce bir çalışma alanı oluşturmadan önce [Azure Izleyici günlüğü tasarım kılavuzunu](../azure-monitor/platform/design-logs-deployment.md) gözden geçirin.

### <a name="2-add-the-automation-solution-to-the-log-analytics-workspace"></a>2. Log Analytics çalışma alanına otomasyon çözümünü ekleyin

Otomasyon çözümü, karma Runbook Worker desteği de dahil olmak üzere Azure Otomasyonu için işlevsellik ekler. Çözümü Log Analytics çalışma alanınıza eklediğinizde, çalışan bileşenleri otomatik olarak bir sonraki adımda yükleyeceğiniz Aracı bilgisayara iter.

**Otomasyon** çözümünü çalışma alanınıza eklemek Için aşağıdaki PowerShell cmdlet 'ini çalıştırın.

```powershell-interactive
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true -DefaultProfile <IAzureContextContainer>
```

### <a name="3-install-the-log-analytics-agent-for-windows"></a>3. Windows için Log Analytics aracısını yükler

Windows için Log Analytics Aracısı Bilgisayarları bir Azure Izleyici Log Analytics çalışma alanına bağlar. Aracıyı bilgisayarınıza yükleyip çalışma alanınıza bağladığınızda, karma Runbook Worker için gereken bileşenleri otomatik olarak indirir.

Aracıyı bilgisayara yüklemek için [Windows bilgisayarları Azure izleyici günlüklerine bağlama](../log-analytics/log-analytics-windows-agent.md)bölümündeki yönergeleri izleyin. Bu işlemi, ortamınıza birden çok çalışan eklemek için birden çok bilgisayar için yineleyebilirsiniz.

Aracı birkaç dakika sonra Log Analytics çalışma alanınıza başarıyla bağlandığında, çalışma alanına sinyal verisi gönderdiğini doğrulamak için aşağıdaki sorguyu çalıştırabilirsiniz:

```kusto
Heartbeat 
| where Category == "Direct Agent" 
| where TimeGenerated > ago(30m)
```

Arama sonuçlarında, bilgisayara bağlı olduğunu ve hizmete rapor olduğunu belirten, bilgisayar için sinyal kayıtları görmeniz gerekir. Varsayılan olarak, her aracı atanmış çalışma alanına bir sinyal kaydını iletir. Aracının, C:\Program Files\Microsoft Monitoring Sservice içinde AzureAutomationFiles adlı bir klasör olduğunda, otomasyon çözümünü doğru bir şekilde indirdiğini doğrulayabilirsiniz. Karma Runbook Worker sürümünü doğrulamak için, C:\Program Files\Microsoft Monitoring Ise T\azureautomation\ konumuna gidin ve \\*Sürüm* alt klasörünü aklınızda olun.

### <a name="4-install-the-runbook-environment-and-connect-to-azure-automation"></a>4. runbook ortamını yükleyip Azure Otomasyonu 'na bağlanın

Bir aracıyı Log Analytics çalışma alanına rapor verecek şekilde yapılandırdığınızda, otomasyon çözümü **Add-HybridRunbookWorker** cmdlet 'Ini Içeren HybridRegistration PowerShell modülünü gönderir. Runbook ortamını bilgisayara yüklemek ve Azure Otomasyonu 'na kaydetmek için bu cmdlet 'i kullanın.

Yönetici modunda bir PowerShell oturumu açın ve aşağıdaki komutları çalıştırarak modülü içeri aktarın.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

Ardından aşağıdaki sözdizimini kullanarak **Add-HybridRunbookWorker** cmdlet 'ini çalıştırın.

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Bu cmdlet için gereken bilgileri, Azure portal "anahtarları Yönet" sayfasından alabilirsiniz. Otomasyon hesabınızdaki **Ayarlar** sayfasından **anahtarlar** seçeneğini belirleyerek bu sayfayı açın.

!["Anahtarları Yönet" sayfası](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* *GroupName* parametresi Için karma Runbook Worker grubunun adını kullanın. Bu grup Otomasyon hesabında zaten mevcutsa, geçerli bilgisayar bu gruba eklenir. Bu grup yoksa, eklenir.
* *Endpoint* parametresi Için anahtarları Yönet sayfasında **URL** girişini kullanın.
* *Belirteç* parametresi Için anahtarları Yönet SAYFASıNDA **birincil erişim anahtarı** girişini kullanın.

Yükleme hakkında ayrıntılı bilgi almak için, **Add-HybridRunbookWorker**ile *-verbose* anahtarını kullanın.

### <a name="5-install-powershell-modules"></a>5. PowerShell modüllerini yükler

Runbook 'lar, Azure Otomasyonu ortamınızda yüklü olan modüllerde tanımlanan etkinliklerin ve cmdlet 'lerden herhangi birini kullanabilir. Bu modüller şirket içi bilgisayarlara otomatik olarak dağıtılmadığından, el ile kurmanız gerekir. Özel durum Azure modülüdür. Bu modül varsayılan olarak yüklenir ve Azure Otomasyonu için tüm Azure hizmetleri ve etkinlikleri için cmdlet 'lere erişim sağlar.

Karma Runbook Worker özelliğinin birincil amacı yerel kaynakları yönettiğinden, büyük olasılıkla bu kaynakları destekleyen modülleri, özellikle PowerShellGet modülünü yüklemeniz gerekir. Windows PowerShell modüllerini yükleme hakkında bilgi için bkz. [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/developer/windows-powershell).

Yüklenen modüller, karma çalışanın otomatik olarak içe aktarabilmesi için PSModulePath ortam değişkeni tarafından başvurulan bir konumda olmalıdır. Daha fazla bilgi için bkz. [PSModulePath Içinde modül yüklemeleri](https://docs.microsoft.com/powershell/scripting/developer/module/installing-a-powershell-module?view=powershell-7).

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatik hale getirmek üzere nasıl yapılandıracağınızı öğrenmek için bkz. [runbook 'Ları karma Runbook Worker üzerinde çalıştırma](automation-hrw-run-runbooks.md).
* Karma runbook çalışanları kaldırma hakkında yönergeler için bkz. [Azure Otomasyonu karma runbook çalışanlarını kaldırma](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
* Karma runbook çalışanlarınızın sorunlarını giderme hakkında bilgi edinmek için bkz. [Windows karma runbook çalışanları sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#windows)
* Güncelleştirme Yönetimi sorunları gidermeye yönelik ek adımlar için, bkz. [güncelleştirme yönetimi: sorun giderme](troubleshoot/update-management.md).
