---
title: Azure Otomasyonu 'nda Windows karma runbook çalışanı dağıtma
description: Bu makalede, yerel veri merkezinizdeki veya bulut ortamınızda Windows tabanlı makinelerde runbook 'lar çalıştırmak için kullanabileceğiniz bir karma Runbook Worker dağıtımı açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 06/24/2020
ms.topic: conceptual
ms.openlocfilehash: 6141440cdb5b232145417c6051cb340e806f9352
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85338006"
---
# <a name="deploy-a-windows-hybrid-runbook-worker"></a>Windows karma runbook çalışanı dağıtma

Runbook 'u doğrudan rolü barındıran makinede ve bu yerel kaynakları yönetmek için ortamdaki kaynaklara karşı çalıştırmak için Azure Automation 'ın karma Runbook Worker özelliğini kullanabilirsiniz. Azure Otomasyonu runbook 'ları depolar ve yönetir ve ardından bunları bir veya daha fazla belirlenen makineye gönderir. Bu makalede bir Windows makinesine karma Runbook Worker dağıtımı, çalışan kaldırma ve karma Runbook Worker grubunun nasıl kaldırılacağı açıklanır.

Runbook Worker 'ı başarıyla dağıttıktan sonra, runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatikleştirmek üzere nasıl yapılandıracağınızı öğrenmek için [karma Runbook Worker 'daki runbook 'Ları Çalıştır](automation-hrw-run-runbooks.md) ' ı inceleyin.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdakilere sahip olduğunuzdan emin olun.

### <a name="a-log-analytics-workspace"></a>Log Analytics çalışma alanı

Karma Runbook Worker rolü, rolü yüklemek ve yapılandırmak için bir Azure Izleyici Log Analytics çalışma alanına bağlıdır. [Azure Resource Manager](../azure-monitor/samples/resource-manager-workspace.md#create-a-log-analytics-workspace), [PowerShell](../azure-monitor/scripts/powershell-sample-create-workspace.md?toc=/powershell/module/toc.json)aracılığıyla veya [Azure Portal](../azure-monitor/learn/quick-create-workspace.md)aracılığıyla oluşturabilirsiniz.

Azure Izleyici Log Analytics çalışma alanınız yoksa, çalışma alanını oluşturmadan önce [Azure Izleyici günlüğü tasarım kılavuzunu](../azure-monitor/platform/design-logs-deployment.md) gözden geçirin.

Bir çalışma alanınız varsa, ancak Otomasyon hesabınıza bağlı değilse, Otomasyon özelliğinin etkinleştirilmesi, karma Runbook Worker desteği de dahil olmak üzere Azure Otomasyonu için işlevsellik ekler. Log Analytics çalışma alanınızda Azure Otomasyonu özelliklerinden birini etkinleştirdiğinizde, özellikle [güncelleştirme yönetimi](automation-update-management.md) veya [değişiklik izleme ve envanterinde](change-tracking.md), çalışan bileşenleri otomatik olarak aracı makinesine gönderilir.

   Güncelleştirme Yönetimi özelliğini çalışma alanınıza eklemek için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

    ```powershell-interactive
    Set-AzOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <logAnalyticsWorkspaceName> -IntelligencePackName "Updates" -Enabled $true
    ```

   Değişiklik İzleme ve envanter özelliğini çalışma alanınıza eklemek için aşağıdaki PowerShell cmdlet 'ini çalıştırın:

    ```powershell-interactive
    Set-AzOperationalInsightsIntelligencePack -ResourceGroupName <logAnalyticsResourceGroup> -WorkspaceName <logAnalyticsWorkspaceName> -IntelligencePackName "ChangeTracking" -Enabled $true
    ```

### <a name="log-analytics-agent"></a>Log Analytics aracısı

Karma Runbook Worker rolü, desteklenen Windows işletim sistemi için [Log Analytics aracısına](../azure-monitor/platform/log-analytics-agent.md) gerek duyar.

### <a name="supported-windows-operating-system"></a>Desteklenen Windows işletim sistemi

Windows işletim sisteminin aşağıdaki sürümleri resmi olarak Windows karma Runbook Worker için desteklenir:

* Windows Server 2019
* Windows Server 2016, sürüm 1709 ve 1803
* Windows Server 2012, 2012 R2
* Windows Server 2008 SP2 (x64), 2008 R2
* Windows 10 Enterprise (çoklu oturum dahil) ve Pro
* Windows 8 Enterprise ve Pro
* Windows 7 SP1

### <a name="minimum-requirements"></a>Minimum gereksinimler

Bir Windows karma Runbook Worker için en düşük gereksinimler şunlardır:

* Windows PowerShell 5,1 veya üzeri ([WMF 5,1 indirin](https://www.microsoft.com/download/details.aspx?id=54616))
* .NET Framework 4.6.2 veya sonraki sürümü
* İki çekirdek
* 4 GB RAM
* Bağlantı noktası 443 (giden)

### <a name="network-configuration"></a>Ağ yapılandırması

Karma Runbook Worker için daha fazla ağ gereksinimi almak üzere [ağınızı yapılandırma](automation-hybrid-runbook-worker.md#network-planning)konusuna bakın.

### <a name="adding-a-machine-to-a-hybrid-runbook-worker-group"></a>Karma Runbook Worker grubuna makine ekleme

Çalışan makineyi Otomasyon hesabınızdaki bir karma Runbook Worker grubuna ekleyebilirsiniz. Hem Azure Otomasyonu özelliği hem de karma runbook çalışanı grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon Runbook 'larını desteketmeniz gerektiğini unutmayın. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

>[!NOTE]
>Azure Otomasyonu [güncelleştirme yönetimi](automation-update-management.md) etkinleştirilmesi, işletim sistemi güncelleştirmelerini yönetmeyi desteklemek için Log Analytics çalışma alanınıza karma runbook çalışanı olarak bağlı tüm Windows makineleri otomatik olarak yapılandırır. Ancak bu çalışan, Otomasyon hesabınızda zaten tanımlanmış olan karma Runbook Worker gruplarına kayıtlı değildir.

## <a name="enabling-machines-for-management-with-azure-automation-state-configuration"></a>Azure Otomasyonu durum yapılandırmasıyla yönetimi için makineleri etkinleştirme

Azure Otomasyonu durum yapılandırmasıyla yönetim için makinelerin etkinleştirilmesi hakkında bilgi için bkz. [Azure Otomasyonu durum yapılandırması tarafından yönetim için makineleri etkinleştirme](automation-dsc-onboarding.md).

> [!NOTE]
> Istenen durum yapılandırması (DSC) ile karma Runbook Worker rolünü destekleyen makinelerin yapılandırmasını yönetmek için, makineleri DSC düğümleri olarak eklemeniz gerekir.

## <a name="windows-hybrid-runbook-worker-installation-options"></a>Windows karma Runbook Worker yükleme seçenekleri

Bir Windows karma runbook çalışanı yüklemek ve yapılandırmak için aşağıdaki yöntemlerden birini kullanabilirsiniz.

* Azure VM 'Leri için [Windows için sanal makine uzantısı](../virtual-machines/extensions/oms-windows.md)' nı kullanarak windows için Log Analytics aracısını yüklersiniz. Uzantı Log Analytics aracısını Azure sanal makinelerine yükledikten sonra bir Azure Resource Manager şablonunu veya PowerShell 'i kullanarak var olan bir Log Analytics çalışma alanına sanal makineler kaydeder. Aracı yüklendikten sonra, sanal makine Otomasyon hesabınızdaki bir karma Runbook Worker grubuna eklenebilir.

* Azure dışı VM 'Ler için [Windows bilgisayarları Azure 'A bağlama](../azure-monitor/platform/agent-windows.md) ' da açıklanan dağıtım seçeneklerini kullanarak windows için Log Analytics aracısını yüklersiniz. Bu işlemi, ortamınıza birden fazla çalışan eklemek için birden çok makine için yineleyebilirsiniz. Aracı yüklendikten sonra, VM 'Ler Otomasyon hesabınızdaki bir karma Runbook Worker grubuna eklenebilir.

* Bir veya daha fazla Windows makinesini yapılandırma işlemini tamamen [otomatik hale getirmek](#automated-deployment) için bir belirtilen PowerShell betiği kullanın. Bu, veri merkezinizdeki veya başka bir bulut ortamındaki makineler için önerilen yöntemdir.

## <a name="automated-deployment"></a>Otomatik dağıtım

Hedef makinede, PowerShell betiği **New-OnPremiseHybridWorker.ps1**kullanarak Windows hibrit çalışan rolünün yükleme ve yapılandırmasını otomatik hale getirmek için aşağıdaki adımları gerçekleştirin. Betik aşağıdaki adımları gerçekleştirir:

* Gerekli modülleri yükleme
* Azure hesabınızla oturum açar
* Belirtilen kaynak grubu ve otomasyon hesabının varlığını doğrular
* Otomasyon hesabı özniteliklerine başvurular oluşturur
* Belirtilmemişse bir Azure Izleyici Log Analytics çalışma alanı oluşturur
* Çalışma alanında Azure Otomasyonu çözümünü etkinleştirme
* Windows için Log Analytics aracısını indirme ve yükleme
* Makineyi karma Runbook Worker olarak kaydet

### <a name="step-1---download-the-powershell-script"></a>1. adım-PowerShell betiğini Indirme

**New-OnPremiseHybridWorker.ps1** betiğini [PowerShell Galerisi](https://www.powershellgallery.com/packages/New-OnPremiseHybridWorker)indirin. Betiği indirdikten sonra hedef makinede kopyalayın veya çalıştırın. **New-OnPremiseHybridWorker.ps1** betiği yürütme sırasında aşağıda açıklanan parametreleri kullanır.

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

**Başlat menüsünde** **Başlat**' a tıklayın, **PowerShell**yazın, **Windows PowerShell**' e sağ tıklayın ve ardından **yönetici olarak çalıştır**' a tıklayın.

### <a name="step-3---run-the-powershell-script"></a>3. adım-PowerShell betiğini çalıştırma

PowerShell komut satırı kabuğu 'nda indirdiğiniz betiği içeren klasöre gidin. ,,,, Ve parametreleri için değerleri değiştirin `AutomationAccountName` `AAResourceGroupName` `OMSResourceGroupName` `HybridGroupName` `SubscriptionID` `WorkspaceName` . Sonra betiği çalıştırın.

Betiği çalıştırdıktan sonra Azure ile kimlik doğrulaması yapmanız istenir. Abonelik yöneticileri rolünün üyesi ve aboneliğin ortak Yöneticisi olan bir hesapla oturum açmalısınız.

```powershell-interactive
.\New-OnPremiseHybridWorker.ps1 -AutomationAccountName <nameOfAutomationAccount> -AAResourceGroupName <nameOfResourceGroup>`
-OMSResourceGroupName <nameOfOResourceGroup> -HybridGroupName <nameOfHRWGroup> `
-SubscriptionID <subscriptionId> -WorkspaceName <nameOfLogAnalyticsWorkspace>
```

### <a name="step-4---install-nuget"></a>4. adım-NuGet 'i yükler

NuGet 'i yüklemek ve Azure kimlik bilgilerinizle kimlik doğrulaması yapmak için kabul etmeniz istenir. En son NuGet sürümüne sahip değilseniz, [mevcut NuGet dağıtım sürümlerinden](https://www.nuget.org/downloads)indirebilirsiniz.

### <a name="step-5---verify-the-deployment"></a>5. adım-dağıtımı doğrulama

Betik tamamlandıktan sonra, Otomasyon hesabınızdaki karma çalışan grupları sayfasında yeni grup ve üye sayısı gösterilir. Mevcut bir gruptur, üye sayısı artırılır. Karma çalışan grupları sayfasında listeden grubu seçebilir ve **hibrit çalışanlar** kutucuğunu seçebilirsiniz. Karma çalışanlar sayfasında, grubun her bir üyesini listelendiğini görebilirsiniz.

## <a name="manual-deployment"></a>El ile dağıtım

Bir Windows karma runbook çalışanı yüklemek ve yapılandırmak için aşağıdaki adımları gerçekleştirin.

### <a name="step-1---verify-agent-is-reporting-to-workspace"></a>1. adım-aracının çalışma alanına raporluyor olduğunu doğrulama

Windows için Log Analytics Aracısı, makineleri bir Azure Izleyici Log Analytics çalışma alanına bağlar. Aracıyı makinenize yükleyip çalışma alanınıza bağladığınızda, karma Runbook Worker için gereken bileşenleri otomatik olarak indirir.

Aracı birkaç dakika sonra Log Analytics çalışma alanınıza başarıyla bağlandığında, çalışma alanına sinyal verisi gönderdiğini doğrulamak için aşağıdaki sorguyu çalıştırabilirsiniz.

```kusto
Heartbeat 
| where Category == "Direct Agent"
| where TimeGenerated > ago(30m)
```

Arama sonuçlarında, makineye bağlı olduğunu ve hizmete raporlanmasını belirten, makinenin sinyal kayıtlarını görmeniz gerekir. Varsayılan olarak, her aracı atanmış çalışma alanına bir sinyal kaydını iletir. Aracı yüklemesini ve kurulumunu gerçekleştirmek için aşağıdaki adımları kullanın.

1. Aracı makinesini eklemek için özelliği etkinleştirin. Güncelleştirme Yönetimi ve Azure VM 'Leri için bkz. [Azure VM 'Leri etkinleştirme](automation-onboard-solutions-from-automation-account.md#enable-azure-vms)ve Azure dışı VM 'ler için bkz. [çalışma alanındaki makineleri etkinleştirme](automation-onboard-solutions-from-automation-account.md#enable-machines-in-the-workspace). Değişiklik İzleme ve Azure VM 'Leri için bkz. [Azure VM 'Leri etkinleştirme](automation-enable-changes-from-auto-acct.md#enable-azure-vms)ve Azure dışı VM 'ler için bkz. [çalışma alanındaki makineleri etkinleştirme](automation-enable-changes-from-auto-acct.md#enable-machines-in-the-workspace).

2. Karma Runbook Worker sürümünü onaylamak için, `C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\` **Sürüm** alt klasörüne göz atarak göz atabilirsiniz.

### <a name="step-3---install-the-runbook-environment-and-connect-to-azure-automation"></a>3. adım-runbook ortamını yükleyip Azure Otomasyonu 'na bağlanma

Bir aracıyı bir Log Analytics çalışma alanına rapor verecek şekilde yapılandırdığınızda, Azure Otomasyonu özelliği `HybridRegistration` cmdlet 'ini içeren PowerShell modülünü aşağı iter `Add-HybridRunbookWorker` . Runbook ortamını makineye yüklemek ve Azure Otomasyonu 'na kaydetmek için bu cmdlet 'i kullanın.

Yönetici modunda bir PowerShell oturumu açın ve aşağıdaki komutları çalıştırarak modülü içeri aktarın.

```powershell-interactive
cd "C:\Program Files\Microsoft Monitoring Agent\Agent\AzureAutomation\<version>\HybridRegistration"
Import-Module .\HybridRegistration.psd1
```

Şimdi `Add-HybridRunbookWorker` aşağıdaki söz dizimini kullanarak cmdlet 'i çalıştırın.

```powershell-interactive
Add-HybridRunbookWorker –GroupName <String> -EndPoint <Url> -Token <String>
```

Parametreler için gereken bilgileri `EndPoint` ve `Token` Otomasyon hesabınızdaki **anahtarlar** sayfasını alabilirsiniz. Sayfanın sol tarafındaki **Hesap ayarları** bölümünün altında bulunan **anahtarlar** ' ı seçin.

![Anahtarları Yönet sayfası](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

* Parametresi için `EndPoint` , **URL**değerini kopyalayın.

* Parametresi için `Token` , **BIRINCIL erişim anahtarı**değerini kopyalayın.

* Parametresi için `GroupName` karma Runbook Worker grubunun adını kullanın. Bu grup Otomasyon hesabında zaten mevcutsa, geçerli makine buna eklenir. Bu grup yoksa, eklenir.

* Gerekirse, `Verbose` yükleme hakkındaki ayrıntıları almak için parametresini ayarlayın.

### <a name="step-4----install-powershell-modules"></a>4. adım-PowerShell modüllerini Install

Runbook 'lar, Azure Otomasyonu ortamınızda yüklü olan modüllerde tanımlanan etkinlikleri ve cmdlet 'leri kullanabilir. Bu modüller şirket içi makinelere otomatik olarak dağıtılmadığından, el ile kurmanız gerekir. Özel durum Azure modülüdür. Bu modül varsayılan olarak yüklenir ve Azure Otomasyonu için tüm Azure hizmetleri ve etkinlikleri için cmdlet 'lere erişim sağlar.

Karma Runbook Worker 'ın birincil amacı yerel kaynakları yönettiğinden, büyük olasılıkla bu kaynakları destekleyen modülleri, özellikle modülünü yüklemeniz gerekir `PowerShellGet` . Windows PowerShell modüllerini yükleme hakkında bilgi için bkz. [Windows PowerShell](https://docs.microsoft.com/powershell/scripting/developer/windows-powershell).

Yüklenen modüller, `PSModulePath` karma çalışanın otomatik olarak içe aktarabilmesi için ortam değişkeni tarafından başvurulan bir konumda olmalıdır. Daha fazla bilgi için bkz. [PSModulePath Içinde modül yüklemeleri](https://docs.microsoft.com/powershell/scripting/developer/module/installing-a-powershell-module?view=powershell-7).

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-windows-machine"></a><a name="remove-windows-hybrid-runbook-worker"></a>Karma runbook çalışanını şirket içi bir Windows makinesinden kaldırma

1. Azure portal Otomasyon hesabınıza gidin.

2. **Hesap ayarları**altında **anahtarlar** ' ı seçin ve **URL** ve **birincil erişim anahtarı**değerlerini aklınızda yapın.

3. Yönetici modunda bir PowerShell oturumu açın ve URL 'niz ile birincil erişim anahtarı değerlerinizle aşağıdaki komutu çalıştırın. `Verbose`Kaldırma işleminin ayrıntılı günlüğü için parametresini kullanın. Eski makineleri karma çalışanı grubundan kaldırmak için isteğe bağlı `machineName` parametresini kullanın.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <primaryAccessKey> -machineName <computerName>
```

## <a name="remove-a-hybrid-worker-group"></a>Karma Çalışanı grubunu kaldırma

Karma runbook çalışanı grubunu kaldırmak için, önce karma runbook çalışanını grubun üyesi olan her makineden kaldırmanız gerekir. Sonra, grubu kaldırmak için aşağıdaki adımları kullanın:

1. Azure portal Otomasyon hesabını açın.

2. **Işlem Otomasyonu**altında **karma çalışan grupları** ' nı seçin. Silmek istediğiniz grubu seçin. Bu grubun Özellikler sayfası görüntülenir.

   ![Özellikler sayfası](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. Seçili grubun Özellikler sayfasında **Sil**' i seçin. Bu eylemi onaylamanızı isteyen bir ileti. Devam etmek istediğinizden emin değilseniz **Evet** ' i seçin.

   ![Onay iletisi](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Bu işlemin tamamlanması birkaç saniye sürebilir. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatik hale getirmek üzere nasıl yapılandıracağınızı öğrenmek için bkz. [runbook 'Ları karma Runbook Worker üzerinde çalıştırma](automation-hrw-run-runbooks.md).

* Karma runbook çalışanlarınızın sorunlarını giderme hakkında bilgi edinmek için bkz. [karma Runbook Worker sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#general).
