---
title: Azure Otomasyonu 'nda Windows karma runbook çalışanı dağıtma
description: Bu makalede, yerel veri merkezinizdeki veya bulut ortamınızda Windows tabanlı bilgisayarlarda runbook 'ları çalıştırmak için kullanabileceğiniz karma bir runbook worker dağıtımı açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 12/10/2019
ms.topic: conceptual
ms.openlocfilehash: e4b8bcf2b6ed5ab9c1160df49b1a6082aaf02f65
ms.sourcegitcommit: 0b80a5802343ea769a91f91a8cdbdf1b67a932d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/25/2020
ms.locfileid: "83830472"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Windows karma runbook çalışanı dağıtma

Runbook 'u doğrudan rolü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamdaki kaynaklara karşı çalıştırmak için Azure Otomasyonu 'nun karma Runbook Worker özelliğini kullanabilirsiniz. Azure Otomasyonu runbook 'ları depolar ve yönetir ve ardından bunları belirlenen bir veya daha fazla bilgisayara gönderir. Bu makalede bir Windows makinesinde karma Runbook Worker dağıtımı açıklanır.

Runbook Worker 'ı başarıyla dağıttıktan sonra, runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatikleştirmek üzere nasıl yapılandıracağınızı öğrenmek için [karma Runbook Worker 'daki runbook 'Ları Çalıştır](automation-hrw-run-runbooks.md) ' ı inceleyin.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="windows-hybrid-runbook-worker-installation-and-configuration"></a>Windows karma Runbook Worker yüklemesi ve yapılandırması

Bir Windows karma runbook çalışanı yüklemek ve yapılandırmak için aşağıdaki yöntemlerden birini kullanabilirsiniz.

* Azure VM 'Leri için [Windows için sanal makine uzantısı](../virtual-machines/extensions/oms-windows.md)' nı kullanarak windows için Log Analytics aracısını yüklersiniz. Uzantı Log Analytics aracısını Azure sanal makinelerine yükledikten sonra bir Azure Resource Manager şablonunu veya PowerShell 'i kullanarak var olan bir Log Analytics çalışma alanına sanal makineler kaydeder. Aracı yüklendikten sonra, sanal makine Otomasyon hesabınızdaki bir karma Runbook Worker grubuna eklenebilir. [El ile dağıtım](#manual-deployment) bölümünde 3. ve 4. adıma bakın.

* Bir Windows bilgisayarı yapılandırma işlemini tamamen otomatik hale getirmek için bir Otomasyon Runbook 'u kullanın. Bu, veri merkezinizdeki veya başka bir bulut ortamındaki makineler için önerilen yöntemdir.

* Azure dışı sanal makinenize karma Runbook Worker rolünü el ile yüklemek ve yapılandırmak için adım adım prosedürü izleyin.

> [!NOTE]
> Istenen durum yapılandırması (DSC) ile karma Runbook Worker rolünü destekleyen sunucuların yapılandırmalarını yönetmek için, sunucuları DSC düğümleri olarak eklemeniz gerekir.

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

### <a name="enabling-servers-for-management-with-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırmasıyla yönetimi için sunucuları etkinleştirme

Sunucuları Azure Otomasyonu durum yapılandırmasıyla yönetimi için etkinleştirme hakkında bilgi için bkz. [Azure Otomasyonu durum yapılandırması tarafından yönetim için makineleri etkinleştirme](automation-dsc-onboarding.md).

Azure Otomasyonu [güncelleştirme yönetimi](automation-update-management.md) etkinleştirilmesi, runbook güncelleştirmelerini desteklemek için Log Analytics çalışma alanınıza karma runbook çalışanı olarak bağlı tüm Windows bilgisayarları otomatik olarak yapılandırır. Ancak bu çalışan, Otomasyon hesabınızda zaten tanımlanmış olan karma Runbook Worker gruplarına kayıtlı değildir.

### <a name="addition-of-the-computer-to-a-hybrid-runbook-worker-group"></a>Bilgisayarın karma Runbook Worker grubuna eklenmesi

Çalışan bilgisayarı, Otomasyon hesabınızdaki bir karma Runbook Worker grubuna ekleyebilirsiniz. Hem Azure Otomasyonu özelliği hem de karma runbook çalışanı grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon Runbook 'larını desteketmeniz gerektiğini unutmayın. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

## <a name="automated-deployment"></a>Otomatik dağıtım

Hedef makinede, Windows hibrit çalışan rolünün yükleme ve yapılandırmasını otomatik hale getirmek için aşağıdaki adımları gerçekleştirin.

### <a name="step-1---download-the-powershell-script"></a>1. adım-PowerShell betiğini Indirme

[PowerShell Galerisi](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker) **New-OnPremiseHybridWorker. ps1** betiğini indirin. İndirmenin doğrudan karma Runbook Worker rolünü çalıştıran bilgisayardan veya ortamınızda başka bir bilgisayardan olması gerekir. Betiği indirdiyseniz, bunu çalışanınızdan kopyalayın. **New-OnPremiseHybridWorker. ps1** betiği yürütme sırasında aşağıda açıklanan parametreleri kullanır.

| Parametre | Durum | Açıklama |
| --------- | ------ | ----------- |
| `AAResourceGroupName` | Zorunlu | Otomasyon hesabınızla ilişkili kaynak grubunun adı. |
| `AutomationAccountName` | Zorunlu | Otomasyon hesabınızın adı.
| `Credential` | İsteğe Bağlı | Azure ortamında oturum açarken kullanılacak kimlik bilgileri. |
| `HybridGroupName` | Zorunlu | Bu senaryoyu destekleyen runbook 'lar için hedef olarak belirttiğiniz karma Runbook Worker grubunun adı. |
| `OMSResourceGroupName` | İsteğe Bağlı | Log Analytics çalışma alanı için kaynak grubunun adı. Bu kaynak grubu belirtilmemişse, değeri `AAResourceGroupName` kullanılır. |
| `SubscriptionID` | Zorunlu | Otomasyon hesabınızla ilişkili Azure aboneliğinin tanıtıcısı. |
| `TenantID` | İsteğe Bağlı | Otomasyon hesabınızla ilişkili kiracı kuruluşunun tanıtıcısı. |
| `WorkspaceName` | İsteğe Bağlı | Log Analytics çalışma alanı adı. Log Analytics çalışma alanınız yoksa, betik bir tane oluşturur ve yapılandırır. |

> [!NOTE]
> Özellikleri etkinleştirirken, Azure Otomasyonu yalnızca bir Log Analytics çalışma alanı ve Otomasyon hesabı bağlamak için belirli bölgeleri destekler. Desteklenen eşleme çiftlerinin bir listesi için bkz. [Otomasyon hesabı ve Log Analytics çalışma alanı Için bölge eşleme](how-to/region-mappings.md).

### <a name="step-2---open-windows-powershell-command-line-shell"></a>2. Adım-Windows PowerShell komut satırı kabuğunu açın

**Windows PowerShell** 'i yönetici modundaki **Başlangıç** ekranından açın.

### <a name="step-3---run-the-powershell-script"></a>3. adım-PowerShell betiğini çalıştırma

PowerShell komut satırı kabuğu 'nda indirdiğiniz betiği içeren klasöre gidin. ,,,, Ve parametreleri için değerleri değiştirin `AutomationAccountName` `AAResourceGroupName` `OMSResourceGroupName` `HybridGroupName` `SubscriptionID` `WorkspaceName` . Sonra betiği çalıştırın.

Betiği çalıştırdıktan sonra Azure ile kimlik doğrulaması yapmanız istenir. Abonelik yöneticileri rolünün üyesi ve aboneliğin ortak Yöneticisi olan bir hesapla oturum açmalısınız.

```powershell-interactive
.\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <NameofAutomationAccount> -AAResourceGroupName <NameofResourceGroup>`
-OMSResourceGroupName <NameofOResourceGroup> -HybridGroupName <NameofHRWGroup> `
-SubscriptionID <AzureSubscriptionId> -WorkspaceName <NameOfLogAnalyticsWorkspace>
```

### <a name="step-4---install-nuget"></a>4. adım-NuGet 'i yükler

NuGet 'i yüklemek ve Azure kimlik bilgilerinizle kimlik doğrulaması yapmak için kabul etmeniz istenir. En son NuGet sürümüne sahip değilseniz, [kullanılabilir NuGet dağıtım sürümlerinden](https://www.nuget.org/downloads)edinebilirsiniz.

### <a name="step-5---verify-the-deployment"></a>5. adım-dağıtımı doğrulama

Betik tamamlandıktan sonra, karma çalışan grupları sayfasında yeni grup ve üye sayısı gösterilir. Mevcut bir gruptur, üye sayısı artırılır. Karma çalışan grupları sayfasında listeden grubu seçebilir ve **hibrit çalışanlar** kutucuğunu seçebilirsiniz. Karma çalışanlar sayfasında, grubun her bir üyesini listelendiğini görebilirsiniz.

## <a name="manual-deployment"></a>El ile dağıtım

Hedef makinede, Otomasyon ortamınız için bir kez ilk iki adımı gerçekleştirin. Ardından her çalışan bilgisayar için kalan adımları gerçekleştirin.

### <a name="step-1---create-a-log-analytics-workspace"></a>1. adım-Log Analytics çalışma alanı oluşturma

Zaten bir Log Analytics çalışma alanınız yoksa, çalışma alanını oluşturmadan önce [Azure Izleyici günlüğü tasarım kılavuzunu](../azure-monitor/platform/design-logs-deployment.md) gözden geçirin.

### <a name="step-2---add-an-azure-automation-feature-to-the-log-analytics-workspace"></a>2. adım-Log Analytics çalışma alanına Azure Otomasyonu özelliği ekleme

Otomasyon özelliği, karma Runbook Worker desteği de dahil olmak üzere Azure Otomasyonu için işlevsellik ekler. Log Analytics çalışma alanınızda bir Azure Otomasyonu özelliğini etkinleştirdiğinizde, çalışan bileşenleri otomatik olarak Aracı bilgisayara gönderilir.

Azure Otomasyonu özelliğini, örneğin Güncelleştirme Yönetimi, çalışma alanınıza eklemek için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

```powershell-interactive
Set-AzOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <LogAnalyticsWorkspaceName> -IntelligencePackName "AzureAutomation" -Enabled $true -DefaultProfile <IAzureContextContainer>
```

### <a name="step-3---install-the-log-analytics-agent-for-windows"></a>3. Adım-Windows için Log Analytics aracısını yükler

Windows için Log Analytics Aracısı Bilgisayarları bir Azure Izleyici Log Analytics çalışma alanına bağlar. Aracıyı bilgisayarınıza yükleyip çalışma alanınıza bağladığınızda, karma Runbook Worker için gereken bileşenleri otomatik olarak indirir.

Aracıyı bilgisayara yüklemek için [Windows bilgisayarları Azure izleyici günlüklerine bağlama](../log-analytics/log-analytics-windows-agent.md)bölümündeki yönergeleri izleyin. Bu işlemi, ortamınıza birden çok çalışan eklemek için birden çok bilgisayar için yineleyebilirsiniz.

Aracı birkaç dakika sonra Log Analytics çalışma alanınıza başarıyla bağlandığında, çalışma alanına sinyal verisi gönderdiğini doğrulamak için aşağıdaki sorguyu çalıştırabilirsiniz.

```kusto
Heartbeat 
| where Category == "Direct Agent" 
| where TimeGenerated > ago(30m)
```

Arama sonuçlarında, bilgisayara bağlı olduğunu ve hizmete rapor olduğunu belirten, bilgisayar için sinyal kayıtları görmeniz gerekir. Varsayılan olarak, her aracı atanmış çalışma alanına bir sinyal kaydını iletir. Aracı yüklemesini ve kurulumunu gerçekleştirmek için aşağıdaki adımları kullanın.

1. Aracı makinesini eklemek için özelliği etkinleştirin. Bkz. [çalışma alanındaki makineleri etkinleştirme](https://docs.microsoft.com/azure/automation/automation-onboard-solutions-from-automation-account#onboard-machines-in-the-workspace).
2. Aracının Azure Otomasyonu özelliğini doğru bir şekilde indirdiğini doğrulayın. 
3. Karma Runbook Worker sürümünü doğrulamak için, **C:\Program Files\Microsoft Monitoring Tors T\\cmreautoma'e** gidin ve **Sürüm** alt klasörünü aklınızda olun.

### <a name="step-4---install-the-runbook-environment-and-connect-to-azure-automation"></a>4. adım-runbook ortamını yükleyip Azure Otomasyonu 'na bağlanma

Bir aracıyı bir Log Analytics çalışma alanına rapor verecek şekilde yapılandırdığınızda, Azure Otomasyonu özelliği `HybridRegistration` cmdlet 'ini içeren PowerShell modülünü aşağı iter `Add-HybridRunbookWorker` . Runbook ortamını bilgisayara yüklemek ve Azure Otomasyonu 'na kaydetmek için bu cmdlet 'i kullanın.

Yönetici modunda bir PowerShell oturumu açın ve aşağıdaki komutları çalıştırarak modülü içeri aktarın.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

Şimdi `Add-HybridRunbookWorker` aşağıdaki söz dizimini kullanarak cmdlet 'i çalıştırın.

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Bu cmdlet için gereken bilgileri Azure portal anahtarları Yönet sayfasından edinebilirsiniz. Otomasyon hesabınızdaki ayarlar sayfasında bulunan **anahtarlar** ' a tıklayarak bu sayfayı açın.

![Anahtarları Yönet sayfası](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* Parametresi için `GroupName` karma Runbook Worker grubunun adını kullanın. Bu grup Otomasyon hesabında zaten mevcutsa, geçerli bilgisayar bu gruba eklenir. Bu grup yoksa, eklenir.
* Parametresi için `EndPoint` , anahtarları Yönet sayfasında **URL** girişini kullanın.
* Parametresi için `Token` anahtarları Yönet sayfasında **BIRINCIL erişim anahtarı** girişini kullanın.
* Gerekirse, `Verbose` yükleme hakkındaki ayrıntıları almak için parametresini ayarlayın.

### <a name="step-5----install-powershell-modules"></a>5. adım-PowerShell modüllerini Install

Runbook 'lar, Azure Otomasyonu ortamınızda yüklü olan modüllerde tanımlanan etkinlikleri ve cmdlet 'leri kullanabilir. Bu modüller şirket içi bilgisayarlara otomatik olarak dağıtılmadığından, el ile kurmanız gerekir. Özel durum Azure modülüdür. Bu modül varsayılan olarak yüklenir ve Azure Otomasyonu için tüm Azure hizmetleri ve etkinlikleri için cmdlet 'lere erişim sağlar.

Karma Runbook Worker 'ın birincil amacı yerel kaynakları yönettiğinden, büyük olasılıkla bu kaynakları destekleyen modülleri, özellikle modülünü yüklemeniz gerekir `PowerShellGet` . Windows PowerShell modüllerini yükleme hakkında bilgi için bkz. [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/developer/windows-powershell).

Yüklenen modüller, `PSModulePath` karma çalışanın otomatik olarak içe aktarabilmesi için ortam değişkeni tarafından başvurulan bir konumda olmalıdır. Daha fazla bilgi için bkz. [PSModulePath Içinde modül yüklemeleri](https://docs.microsoft.com/powershell/scripting/developer/module/installing-a-powershell-module?view=powershell-7).

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-windows-computer"></a><a name="remove-windows-hybrid-runbook-worker"></a>Karma runbook çalışanını şirket içi bir Windows bilgisayarından kaldırma

1. Azure portal Otomasyon hesabınıza gidin.
2. **Hesap ayarları**altında **anahtarlar** ' ı seçin ve **URL** ve **birincil erişim anahtarı**değerlerini aklınızda yapın.

3. Yönetici modunda bir PowerShell oturumu açın ve URL 'niz ile birincil erişim anahtarı değerlerinizle aşağıdaki komutu çalıştırın. `Verbose`Kaldırma işleminin ayrıntılı günlüğü için parametresini kullanın. Eski makineleri karma çalışanı grubundan kaldırmak için isteğe bağlı `machineName` parametresini kullanın.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

## <a name="remove-a-hybrid-worker-group"></a>Karma Çalışanı grubunu kaldırma

Karma runbook çalışanı grubunu kaldırmak için, önce karma runbook çalışanını grubun üyesi olan her bilgisayardan kaldırmanız gerekir. Sonra, grubu kaldırmak için aşağıdaki adımları kullanın:

1. Azure portal Otomasyon hesabını açın.
2. **Işlem Otomasyonu**altında **karma çalışan grupları** ' nı seçin. Silmek istediğiniz grubu seçin. Bu grubun Özellikler sayfası görüntülenir.

   ![Özellikler sayfası](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. Seçili grubun Özellikler sayfasında **Sil**' i seçin. Bu eylemi onaylamanızı isteyen bir ileti. Devam etmek istediğinizden emin değilseniz **Evet** ' i seçin.

   ![Onay iletisi](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Bu işlemin tamamlanması birkaç saniye sürebilir. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatik hale getirmek üzere nasıl yapılandıracağınızı öğrenmek için bkz. [runbook 'Ları karma Runbook Worker üzerinde çalıştırma](automation-hrw-run-runbooks.md).
* Karma runbook çalışanlarınızın sorunlarını giderme hakkında bilgi edinmek için bkz. [karma Runbook Worker sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#general).