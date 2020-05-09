---
title: Azure Otomasyonu Linux Karma Runbook Çalışanı
description: Bu makalede, Azure Otomasyonu karma Runbook Worker yükleme hakkında bilgi sağlanır. böylece, runbook 'ları yerel veri merkezinizdeki veya bulut ortamınızda Linux tabanlı bilgisayarlarda çalıştırabilirsiniz.
services: automation
ms.subservice: process-automation
ms.date: 03/02/2020
ms.topic: conceptual
ms.openlocfilehash: 82f6d9e56e5d5745077ef512cb3392c16b95961f
ms.sourcegitcommit: f57297af0ea729ab76081c98da2243d6b1f6fa63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82872173"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Linux karma runbook çalışanı dağıtma

Runbook 'u doğrudan rolü barındıran bilgisayarda ve bu yerel kaynakları yönetmek için ortamdaki kaynaklara karşı çalıştırmak için Azure Otomasyonu 'nun karma Runbook Worker özelliğini kullanabilirsiniz. Linux hibrit Runbook Worker, runbook 'ları, yükseltme gerektiren komutları çalıştırmak için yükseltilebilir özel bir kullanıcı olarak yürütür. Runbook 'lar Azure Otomasyonu 'nda depolanır ve yönetilir ve ardından bir veya daha fazla belirlenen bilgisayara gönderilir.

Bu makalede karma Runbook Worker 'ın bir Linux makinesine nasıl yükleneceği, çalışanın nasıl kaldırılacağı ve karma Runbook Worker grubunun nasıl kaldırılacağı açıklanır.

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Karma Runbook Worker özelliği aşağıdaki dağıtımları destekler:

* Amazon Linux 2012,09-2015,09 (x86/x64)
* CentOS Linux 5, 6 ve 7 (x86/x64)
* Oracle Linux 5, 6 ve 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 ve 7 (x86/x64)
* De, GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12,04 LTS, 14,04 LTS, 16,04 LTS ve 18,04 (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

## <a name="supported-runbook-types"></a>Desteklenen runbook türleri

Linux hibrit runbook çalışanları, Azure Automation 'da runbook türlerinin tam kümesini desteklemez.

Aşağıdaki runbook türleri bir Linux karma çalışanı üzerinde çalışır:

* Python 2
* PowerShell

  > [!NOTE]
  > PowerShell runbook 'ları, Linux makinesinde PowerShell Core 'un yüklü olmasını gerektirir. Yükleme hakkında bilgi edinmek için bkz. [Linux üzerinde PowerShell Core yükleme](/powershell/scripting/install/installing-powershell-core-on-linux) .

Aşağıdaki runbook türleri bir Linux karma çalışanı üzerinde çalışmıyor:

* PowerShell İş Akışı
* Grafik
* Grafik PowerShell Iş akışı

## <a name="deployment-requirements"></a>Dağıtım gereksinimleri

Linux karma Runbook Worker için en düşük gereksinimler şunlardır:

* İki çekirdek
* 4 GB RAM
* Bağlantı noktası 443 (giden)

### <a name="package-requirements"></a>Paket gereksinimleri

| **Gerekli paket** | **Açıklama** | **En düşük sürüm**|
|--------------------- | --------------------- | -------------------|
|GLIBC |GNU C Kitaplığı| 2.5-12 |
|Openssl| OpenSSL kitaplıkları | 1,0 (TLS 1,1 ve TLS 1,2 desteklenir)|
|Curl | Web istemcisini kıvır | 7.15.5|
|Python-ctypes | Python 2. x gereklidir |
|PAM | Eklenebilir Kimlik Doğrulaması Modülleri|
| **İsteğe bağlı paket** | **Açıklama** | **En düşük sürüm**|
| PowerShell Core | PowerShell runbook 'larını çalıştırmak için PowerShell 'in yüklenmesi gerekir, bkz. yükleme hakkında bilgi edinmek için [Linux 'Ta PowerShell Core 'U yükleme](/powershell/scripting/install/installing-powershell-core-on-linux) .  | 6.0.0 |

## <a name="install-a-linux-hybrid-runbook-worker"></a>Linux karma Runbook Worker 'ı yükler

Linux bilgisayarınızda bir karma runbook çalışanı yüklemek ve yapılandırmak için, doğrudan el ile gerçekleştirilen bir işlemi izleyin. Azure Log Analytics çalışma alanınızda Otomasyon Karma Çalışanı çözümünün etkinleştirilmesini ve sonra bilgisayarı bir çalışan olarak kaydetmek ve bir gruba eklemek için bir komut kümesi çalıştırmayı gerektirir.

Devam etmeden önce Otomasyon hesabınızın bağlı olduğu Log Analytics çalışma alanını unutmayın. Ayrıca Otomasyon hesabınızın birincil anahtarını da aklınızda bulabilirsiniz. Hem Otomasyon hesabınızı seçip, çalışma alanı KIMLIĞI için **çalışma alanı** ' nı seçip birincil anahtar için **anahtarlar** ' ı seçerek Azure Portal her ikisini de bulabilirsiniz. Karma Runbook Worker için ihtiyaç duyduğunuz bağlantı noktaları ve adresler hakkında daha fazla bilgi için bkz. [ağınızı yapılandırma](automation-hybrid-runbook-worker.md#network-planning).

>[!NOTE]
> Linux hibrit Worker yüklemesi sırasında karşılık gelen sudo izinleri olan [nxautomation hesabı](automation-runbook-execution.md#log-analytics-agent-for-linux) bulunmalıdır. Çalışanı yüklemeye çalışırsanız ve hesap yoksa veya uygun izinlere sahip değilse, yükleme başarısız olur.

1. Aşağıdaki yöntemlerden birini kullanarak Azure 'da Otomasyon Karma Çalışanı çözümü etkinleştirin:

   * [Azure İzleyici günlüğü çözümlerini çalışma alanınıza ekleme](../log-analytics/log-analytics-add-solutions.md)yordamını kullanarak aboneliğinize Otomasyon karma çalışanı çözümünü ekleyin.
   * Aşağıdaki cmdlet'i çalıştırın:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Aşağıdaki komutu çalıştırarak Linux için Log Analytics aracısını yükler. Workspace \<ID\> ve \<WorkspaceKey\> değerlerini çalışma alanınızdan uygun değerlerle değiştirin.

   [!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. *-W*, *-k*, *-g*ve *-e*parametrelerinin değerlerini değiştirerek aşağıdaki komutu çalıştırın. *-G* parametresi için, değeri, yeni Linux hibrit Runbook Worker 'ın katılması gereken karma Runbook Worker grubunun adıyla değiştirin. Otomasyon hesabınızda ad yoksa, bu adla yeni bir karma Runbook Worker grubu oluşturulur.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Komut tamamlandıktan sonra, Azure portal karma çalışan grupları sayfasında yeni grup ve üye sayısı gösterilir. Bu, var olan bir gruptur, üye sayısı artırılır. Karma çalışan grupları sayfasında listeden grubu seçebilir ve **hibrit çalışanlar** kutucuğunu seçebilirsiniz. Karma çalışanlar sayfasında, grubun her bir üyesini listede görürsünüz.

> [!NOTE]
> Bir Azure VM için Linux için Azure Izleyici sanal makine uzantısını kullanıyorsanız, otomatik yükseltme sürümleri karma Runbook Worker `autoUpgradeMinorVersion` ile sorun oluşmasına neden olabileceği için false olarak ayarlamayı öneririz. Uzantıyı el ile yükseltme hakkında bilgi edinmek için bkz. [Azure CLI dağıtımı](../virtual-machines/extensions/oms-linux.md#azure-cli-deployment).

## <a name="turn-off-signature-validation"></a>İmza doğrulamasını kapat

Varsayılan olarak, Linux karma runbook çalışanları imza doğrulaması gerektirir. Bir çalışan için imzasız runbook çalıştırırsanız bir `Signature validation failed` hata görürsünüz. İmza doğrulamasını devre dışı bırakmak için aşağıdaki komutu çalıştırın. İkinci parametreyi Log Analytics çalışma alanı KIMLIĞINIZLE değiştirin.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="remove-the-hybrid-runbook-worker-from-an-on-premises-linux-computer"></a><a name="remove-linux-hybrid-runbook-worker"></a>Karma runbook çalışanını şirket içi Linux bilgisayarından kaldırma

Çalışma alanı KIMLIĞINI almak için `ls /var/opt/microsoft/omsagent` karma Runbook Worker üzerinde komutunu kullanabilirsiniz. Çalışma alanı KIMLIĞI ile adlandırılan bir klasör oluşturulur.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Bu kod, Linux için Log Analytics aracısını bilgisayardan kaldırmaz. Yalnızca karma Runbook Worker rolünün işlevselliğini ve yapılandırmasını kaldırır.

## <a name="remove-a-hybrid-worker-group"></a>Karma Çalışanı grubunu kaldırma

Linux bilgisayarların karma Runbook Worker grubunu kaldırmak için bir Windows karma çalışanı grubu ile aynı adımları kullanırsınız. Bkz. [karma çalışanı grubunu kaldırma](automation-windows-hrw-install.md#remove-a-hybrid-worker-group).

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatik hale getirmek üzere nasıl yapılandıracağınızı öğrenmek için bkz. [runbook 'Ları karma Runbook Worker üzerinde çalıştırma](automation-hrw-run-runbooks.md).
* Karma runbook çalışanlarınızın sorunlarını giderme hakkında bilgi edinmek için bkz. [Linux karma runbook çalışanları sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#linux)
