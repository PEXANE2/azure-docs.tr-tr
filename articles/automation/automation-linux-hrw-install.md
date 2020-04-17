---
title: Azure Otomasyonu Linux Karma Runbook Çalışanı
description: Bu makalede, yerel veri merkezinizde veya bulut ortamınızda Linux tabanlı bilgisayarlarda runbook'lar çalıştırabilmeniz için bir Azure Otomasyon Karma Runbook Worker yükleme hakkında bilgi verilmektedir.
services: automation
ms.subservice: process-automation
ms.date: 03/02/2020
ms.topic: conceptual
ms.openlocfilehash: f2584a8d4e68b7c16b3acdc29f64f0a19d83d735
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81457680"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Linux Hybrid Runbook Worker'ı dağıtma

Azure Otomasyonu'nun Karma Runbook Worker özelliğini, bu yerel kaynakları yönetmek için rolü barındıran bilgisayarda doğrudan ve çevredeki kaynaklara karşı runbook'ları çalıştırmak için kullanabilirsiniz. Linux Hybrid Runbook Worker, rünleri, yükseltilmesi gereken komutları çalıştırmak için yükseltilebilen özel bir kullanıcı olarak yürütür. Runbook'lar Azure Otomasyonu'nda depolanır ve yönetilir ve daha sonra bir veya daha fazla belirlenmiş bilgisayara teslim edilir.

Bu makalede, Bir Linux makinesine Hybrid Runbook Worker nasıl yüklenir.

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Karma Runbook Worker özelliği aşağıdaki dağılımları destekler:

* Amazon Linux 2012.09 için 2015.09 (x86/x64)
* CentOS Linux 5, 6 ve 7 (x86/x64)
* Oracle Linux 5, 6 ve 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 ve 7 (x86/x64)
* Debian GNU/Linux 6, 7 ve 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS ve 18.04 (x86/x64)
* SUSE Linux Enterprise Server 11 ve 12 (x86/x64)

## <a name="supported-runbook-types"></a>Desteklenen runbook türleri

Linux Hybrid Runbook Workers, Azure Otomasyonu'nda runbook türlerinin tam kümesini desteklemez.

Aşağıdaki runbook türleri bir Linux Karma İşçi üzerinde çalışır:

* Piton 2
* PowerShell

  > [!NOTE]
  > PowerShell runbook'lar PowerShell Core'un Linux makinesine yüklenmesini gerektirir. Nasıl yükleyirin öğrenmek için [Linux'a PowerShell Core yükleme'ye](/powershell/scripting/install/installing-powershell-core-on-linux) bakın.

Aşağıdaki runbook türleri Bir Linux Karma İşçisi üzerinde çalışmaz:

* PowerShell İş Akışı
* Grafik
* Grafiksel PowerShell İş Akışı

## <a name="installing-a-linux-hybrid-runbook-worker"></a>Linux Hybrid Runbook Worker yükleme

Linux bilgisayarınıza bir Karma Runbook Çalışanı yüklemek ve yapılandırmak için basit bir el ile işlemi izleyin. Azure Log Analytics çalışma alanınızda Otomasyon Karma İşçi çözümünün etkinleştirilmesi ve ardından bilgisayarı çalışan olarak kaydedip bir gruba eklemek için bir dizi komut çalıştırmanız gerekiyor.

Bir Linux Hybrid Runbook Worker için minimum gereksinimler şunlardır:

* İki çekirdek
* 4 GB RAM
* Bağlantı noktası 443 (giden)

### <a name="package-requirements"></a>Paket gereksinimleri

| **Gerekli paket** | **Açıklama** | **Minimum sürüm**|
|--------------------- | --------------------- | -------------------|
|Glibc |GNU C Kütüphanesi| 2.5-12 |
|Openssl| OpenSSL Kitaplıkları | 1.0 (TLS 1.1 ve TLS 1.2 desteklenir|
|Curl | cURL web istemcisi | 7.15.5|
|Python-ctypes | Python 2.x gereklidir |
|PAM | Eklenebilir Kimlik Doğrulaması Modülleri|
| **İsteğe bağlı paket** | **Açıklama** | **Minimum sürüm**|
| PowerShell Core | PowerShell runbook'ları çalıştırmak için PowerShell'in yüklü olması gerekir, nasıl yüklenir öğrenmek için [Linux'a PowerShell Core yükleme'ye](/powershell/scripting/install/installing-powershell-core-on-linux) bakın.  | 6.0.0 |

### <a name="installation"></a>Yükleme

Devam etmeden önce, Otomasyon hesabınızın bağlı olduğu Log Analytics çalışma alanına dikkat edin. Ayrıca Otomasyon hesabınız için birincil anahtarı da not edin. Otomasyon hesabınızı seçerek, çalışma alanı kimliği için **Çalışma Alanı'nı** seçerek ve birincil anahtar için **Anahtarlar'ı** seçerek her ikisini de Azure portalından bulabilirsiniz. Karma Runbook Worker için ihtiyacınız olan bağlantı noktaları ve adresler hakkında bilgi [için](automation-hybrid-runbook-worker.md#network-planning)bkz.

1. Aşağıdaki yöntemlerden birini kullanarak Azure'da Otomasyon Karma İşçisi çözümünün etkinleştirin:

   * [Çalışma alanınıza Azure Monitörü Ekle günlük çözümlerindeki](../log-analytics/log-analytics-add-solutions.md)yordamı kullanarak aboneliğinize Otomasyon Karma İşçi çözümünü ekleyin.
   * Aşağıdaki cmdlet'i çalıştırın:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Aşağıdaki komutu çalıştırarak Linux için Log Analytics aracısını yükleyin. WorkspaceID \<\> ve \<WorkspaceKey'i\> çalışma alanınızdaki uygun değerlerle değiştirin.

   [!INCLUDE [log-analytics-agent-note](../../includes/log-analytics-agent-note.md)]

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. *-w*, -k , *-g*ve *-k* *-e*parametrelerinin değerlerini değiştirerek aşağıdaki komutu çalıştırın. *-g* parametresi için, değeri yeni Linux Hybrid Runbook Worker'ın katılması gereken Karma Runbook Worker grubunun adı ile değiştirin. Otomasyon hesabınızda ad yoksa, bu adla yeni bir Karma Runbook İşçi grubu oluşur.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Komut tamamlandıktan sonra, Azure portalındaki Karma İşçi Grupları sayfası yeni grubu ve üye sayısını gösterir. Bu varolan bir grupsa, üye sayısı artımlanır. Hibrit İşçi Grupları sayfasındaki listeden grubu seçebilir ve **Karma İşçi** ler döşemesini seçebilirsiniz. Karma İşçiler sayfasında, listelenen grubun her üyesini görürsünüz.

> [!NOTE]
> Azure VM için Linux için Azure Monitor sanal makine uzantısını kullanıyorsanız, otomatik yükseltme sürümlerinin Karma Runbook Worker'ın sorunlara yol açabileceğinden yanlış ayarı `autoUpgradeMinorVersion` yapmanızı öneririz. Uzantıyı el ile yükseltmeyi öğrenmek için [Azure CLI dağıtımına](../virtual-machines/extensions/oms-linux.md#azure-cli-deployment)bakın.

## <a name="turning-off-signature-validation"></a>İmza doğrulamayı kapatma

Varsayılan olarak, Linux Hybrid Runbook Workers imza doğrulama gerektirir. İmzalanmamış bir runbook'u bir çalışana `Signature validation failed` karşı çalıştırıyorsanız, bir hata görürsünüz. İmza doğrulaması kapatmak için aşağıdaki komutu çalıştırın. İkinci parametreyi Log Analytics çalışma alanı kimliğinizle değiştirin.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitaplarınızı şirket içi veri merkezinizdeki veya diğer bulut ortamınızdaki işlemleri otomatikleştirmek için nasıl yapılandırılabildiğini öğrenmek için, [Karma Runbook Çalışanı'ndaki runbook'lara](automation-hrw-run-runbooks.md)bakın.
* Karma Runbook İşçileri'ni kaldırma yla ilgili talimatlar için azure [otomasyonu karma çalışma kitabı çalışanlarını kaldırın'a](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker)bakın.
* Hybrid Runbook Workers sorungidermeyi öğrenmek için, [Sorun Giderme Linux Hybrid Runbook Workers'a](troubleshoot/hybrid-runbook-worker.md#linux) bakın
