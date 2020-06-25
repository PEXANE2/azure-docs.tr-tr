---
title: Azure Otomasyonu 'nda Linux karma runbook çalışanı dağıtma
description: Bu makalede, yerel veri merkezinizdeki veya bulut ortamınızdaki Linux tabanlı makinelerde runbook 'ları çalıştırmak için bir Azure Otomasyonu karma Runbook Worker nasıl yükleneceği açıklanır.
services: automation
ms.subservice: process-automation
ms.date: 06/24/2020
ms.topic: conceptual
ms.openlocfilehash: c569c83ed0bc5d78f0e5670c802188ee9fd8fd53
ms.sourcegitcommit: 61d92af1d24510c0cc80afb1aebdc46180997c69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85340805"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Linux karma runbook çalışanı dağıtma

Runbook 'u doğrudan rolü barındıran makinede ve bu yerel kaynakları yönetmek için ortamdaki kaynaklara karşı çalıştırmak için Azure Automation 'ın karma Runbook Worker özelliğini kullanabilirsiniz. Linux hibrit Runbook Worker, runbook 'ları, yükseltme gerektiren komutları çalıştırmak için yükseltilebilir özel bir kullanıcı olarak yürütür. Azure Otomasyonu runbook 'ları depolar ve yönetir ve ardından bunları bir veya daha fazla belirlenen makineye gönderir. Bu makalede karma Runbook Worker 'ın bir Linux makinesine nasıl yükleneceği, çalışanın nasıl kaldırılacağı ve karma Runbook Worker grubunun nasıl kaldırılacağı açıklanır.

Runbook Worker 'ı başarıyla dağıttıktan sonra, runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatikleştirmek üzere nasıl yapılandıracağınızı öğrenmek için [karma Runbook Worker 'daki runbook 'Ları Çalıştır](automation-hrw-run-runbooks.md) ' ı inceleyin.

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdakilere sahip olduğunuzdan emin olun:

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

Karma Runbook Worker rolü, desteklenen Linux işletim sistemi için [Log Analytics aracısına](../azure-monitor/platform/log-analytics-agent.md) gerek duyar.

### <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Karma Runbook Worker özelliği aşağıdaki dağıtımları destekler:

* Amazon Linux 2012,09-2015,09 (x86/x64)
* CentOS Linux 5, 6 ve 7 (x86/x64)
* Oracle Linux 5, 6 ve 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 ve 7 (x86/x64)
* De, GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12,04 LTS, 14,04 LTS, 16,04 LTS ve 18,04 (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

### <a name="minimum-requirements"></a>Minimum gereksinimler

Linux karma Runbook Worker için en düşük gereksinimler şunlardır:

* İki çekirdek
* 4 GB RAM
* Bağlantı noktası 443 (giden)

| **Gerekli paket** | **Açıklama** | **En düşük sürüm**|
|--------------------- | --------------------- | -------------------|
|GLIBC |GNU C Kitaplığı| 2.5-12 |
|Openssl| OpenSSL kitaplıkları | 1,0 (TLS 1,1 ve TLS 1,2 desteklenir)|
|Curl | Web istemcisini kıvır | 7.15.5|
|Python-ctypes | Python 2. x gereklidir |
|PAM | Eklenebilir Kimlik Doğrulaması Modülleri|
| **İsteğe bağlı paket** | **Açıklama** | **En düşük sürüm**|
| PowerShell Core | PowerShell runbook 'larını çalıştırmak için PowerShell Core 'un yüklenmesi gerekir. Yükleme hakkında bilgi edinmek için bkz. [Linux üzerinde PowerShell Core yükleme](/powershell/scripting/install/installing-powershell-core-on-linux) . | 6.0.0 |

## <a name="supported-runbook-types"></a>Desteklenen runbook türleri

Linux hibrit runbook çalışanları, Azure Otomasyonu 'nda sınırlı sayıda runbook türü kümesini destekler ve aşağıdaki tabloda açıklanmıştır.

|Runbook türü | Destekleniyor |
|-------------|-----------|
|Python 2 |Yes |
|PowerShell |Evet<sup>1</sup> |
|PowerShell İş Akışı |No |
|Grafik |No |
|Grafik PowerShell Iş akışı |No |

<sup>1</sup> PowerShell runbook 'ları, Linux makinesinde PowerShell Core 'un yüklü olmasını gerektirir. Yükleme hakkında bilgi edinmek için bkz. [Linux üzerinde PowerShell Core yükleme](/powershell/scripting/install/installing-powershell-core-on-linux) .

## <a name="install-a-linux-hybrid-runbook-worker"></a>Linux karma Runbook Worker 'ı yükler

Bir Linux karma runbook çalışanı yüklemek ve yapılandırmak için aşağıdaki adımları gerçekleştirin.

1. Log Analytics aracısını hedef makineye dağıtın.

    * Azure VM 'Leri için [Linux için sanal makine uzantısını](../virtual-machines/extensions/oms-linux.md)kullanarak linux için Log Analytics aracısını yüklersiniz. Uzantı Log Analytics aracısını Azure sanal makinelerine yükleyip, sanal makineleri bir Azure Resource Manager şablonu veya Azure CLı kullanarak mevcut bir Log Analytics çalışma alanına kaydeder. Aracı yüklendikten sonra, sanal makine Otomasyon hesabınızdaki bir karma Runbook Worker grubuna eklenebilir.

    * Azure dışı VM 'Ler için [Linux bilgisayarlarını Azure 'A bağlama](../azure-monitor/platform/agent-linux.md) bölümünde açıklanan dağıtım seçeneklerini kullanarak linux için Log Analytics aracısını yüklersiniz. Bu işlemi, ortamınızda birden fazla çalışan eklemek için birden çok makine için yineleyebilirsiniz. Aracı yüklendikten sonra, VM 'Ler Otomasyon hesabınızdaki bir karma Runbook Worker grubuna eklenebilir.

    > [!NOTE]
    > Istenen durum yapılandırması (DSC) ile karma Runbook Worker rolünü destekleyen makinelerin yapılandırmasını yönetmek için, makineleri DSC düğümleri olarak eklemeniz gerekir.

    > [!NOTE]
    > Linux hibrit Worker yüklemesi sırasında karşılık gelen sudo izinleri olan [nxautomation hesabı](automation-runbook-execution.md#log-analytics-agent-for-linux) bulunmalıdır. Çalışanı yüklemeye çalışırsanız ve hesap yoksa veya uygun izinlere sahip değilse, yükleme başarısız olur.

2. Aracının çalışma alanına rapor ettiğini doğrulayın.

    Linux için Log Analytics Aracısı, makineleri bir Azure Izleyici Log Analytics çalışma alanına bağlar. Aracıyı makinenize yükleyip çalışma alanınıza bağladığınızda, karma Runbook Worker için gereken bileşenleri otomatik olarak indirir.

    Aracı birkaç dakika sonra Log Analytics çalışma alanınıza başarıyla bağlandığında, çalışma alanına sinyal verisi gönderdiğini doğrulamak için aşağıdaki sorguyu çalıştırabilirsiniz.

    ```kusto
    Heartbeat 
    | where Category == "Direct Agent"
    | where TimeGenerated > ago(30m)
    ```

    Arama sonuçlarında, makineye bağlı olduğunu ve hizmete raporlanmasını belirten, makinenin sinyal kayıtlarını görmeniz gerekir. Varsayılan olarak, her aracı atanmış çalışma alanına bir sinyal kaydını iletir.

3. Makineyi karma Runbook Worker grubuna eklemek için aşağıdaki komutu çalıştırın,, ve parametrelerinin değerlerini belirtin `-w` `-k` `-g` `-e` .

    Parametreler için gereken bilgileri `-k` ve `-e` Otomasyon hesabınızdaki **anahtarlar** sayfasından alabilirsiniz. Sayfanın sol tarafındaki **Hesap ayarları** bölümünün altında bulunan **anahtarlar** ' ı seçin.

    ![Anahtarları Yönet sayfası](media/automation-hybrid-runbook-worker/elements-panel-keys.png)

    * Parametresi için `-e` , **URL**değerini kopyalayın.

    * Parametresi için `-k` , **BIRINCIL erişim anahtarı**değerini kopyalayın.

    * Parametresi için `-g` , yeni Linux hibrit Runbook Worker 'ın katılması gereken karma Runbook Worker grubunun adını belirtin. Bu grup Otomasyon hesabında zaten mevcutsa, geçerli makine buna eklenir. Bu grup yoksa, bu ad ile oluşturulur.

    * Parametresi için `-w` Log Analytics çalışma alanı kimliğinizi belirtin.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <logAnalyticsworkspaceId> -k <automationSharedKey> -g <hybridGroupName> -e <automationEndpoint>
   ```

4. Komut tamamlandıktan sonra, Otomasyon hesabınızdaki karma çalışan grupları sayfasında yeni grup ve üye sayısı gösterilir. Bu, var olan bir gruptur, üye sayısı artırılır. Karma çalışan grupları sayfasında listeden grubu seçebilir ve **hibrit çalışanlar** kutucuğunu seçebilirsiniz. Karma çalışanlar sayfasında, grubun her bir üyesini listede görürsünüz.

    > [!NOTE]
    > Bir Azure VM için Linux için Log Analytics sanal makine uzantısı kullanıyorsanız, `autoUpgradeMinorVersion` `false` otomatik yükseltme sürümleri için ayarı, karma Runbook Worker ile sorun oluşmasına neden olabilir. Uzantıyı el ile yükseltme hakkında bilgi edinmek için bkz. [Azure CLI dağıtımı](../virtual-machines/extensions/oms-linux.md#azure-cli-deployment).

## <a name="turn-off-signature-validation"></a>İmza doğrulamasını kapat

Varsayılan olarak, Linux karma runbook çalışanları imza doğrulaması gerektirir. Bir çalışan için imzasız runbook çalıştırırsanız bir `Signature validation failed` hata görürsünüz. İmza doğrulamasını devre dışı bırakmak için aşağıdaki komutu çalıştırın. İkinci parametreyi Log Analytics çalışma alanı KIMLIĞINIZLE değiştirin.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <logAnalyticsworkspaceId>
 ```

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-linux-machine"></a><a name="remove-linux-hybrid-runbook-worker"></a>Karma runbook çalışanını şirket içi Linux makinesinden kaldırma

`ls /var/opt/microsoft/omsagent`Çalışma alanı kimliğini almak Için karma Runbook Worker üzerinde komutunu kullanabilirsiniz. Çalışma alanı KIMLIĞI ile adlandırılan bir klasör oluşturulur.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Bu betik, Linux için Log Analytics aracısını makineden kaldırmaz. Yalnızca karma Runbook Worker rolünün işlevselliğini ve yapılandırmasını kaldırır.

## <a name="remove-a-hybrid-worker-group"></a>Karma Çalışanı grubunu kaldırma

Linux makinelerinin karma Runbook Worker grubunu kaldırmak için, bir Windows karma çalışanı grubu ile aynı adımları kullanırsınız. Bkz. [karma çalışanı grubunu kaldırma](automation-windows-hrw-install.md#remove-a-hybrid-worker-group).

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatik hale getirmek üzere nasıl yapılandıracağınızı öğrenmek için bkz. [runbook 'Ları karma Runbook Worker üzerinde çalıştırma](automation-hrw-run-runbooks.md).

* Karma runbook çalışanlarınızın sorunlarını giderme hakkında bilgi edinmek için bkz. [karma Runbook Worker sorunlarını giderme-Linux](troubleshoot/hybrid-runbook-worker.md#linux).
