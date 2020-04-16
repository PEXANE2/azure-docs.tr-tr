---
title: Azure Otomasyonu Karma Runbook Çalışanı
description: Bu makalede, Azure Otomasyonu'nun yerel veri merkezinizdeki veya bulut sağlayıcınızdaki makinelerde runbook çalıştırmak için kullanabileceğiniz bir özelliği olan Karma Runbook Worker'ı yükleme ve kullanma hakkında bilgi verilmektedir.
services: automation
ms.subservice: process-automation
ms.date: 04/05/2019
ms.topic: conceptual
ms.openlocfilehash: 1e4470ce5ac69390cf8d361577b9ebf0013e4e51
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81405795"
---
# <a name="automate-resources-in-your-datacenter-or-cloud-by-using-hybrid-runbook-worker"></a>Karma Runbook Worker'ı kullanarak veri merkezinizdeki veya bulutunuzdaki kaynakları otomatikleştirin

Azure Otomasyonu'ndaki runbook'lar, Azure bulut platformunda çalıştıkları için diğer bulutlardaki veya şirket ortamınızdaki kaynaklara erişemeyebilir. Azure Otomasyonu'nun Karma Runbook Worker özelliğini, bu yerel kaynakları yönetmek için rolü barındıran bilgisayarda doğrudan ve çevredeki kaynaklara karşı runbook'ları çalıştırmak için kullanabilirsiniz. Runbook'lar Azure Otomasyonu'nda depolanır ve yönetilir ve daha sonra bir veya daha fazla atanmış bilgisayara teslim edilir.

Aşağıdaki resim bu işlevselliği göstermektedir:

![Karma Runbook Çalışanına genel bakış](media/automation-hybrid-runbook-worker/automation.png)

Her Karma Runbook Çalışanı, aracıyı yüklediğinizde belirttiğiniz karma runbook worker grubunun bir üyesidir. Bir grup tek bir aracı içerebilir, ancak yüksek kullanılabilirlik için bir gruba birden çok aracı yükleyebilirsiniz. Her makine, bir Otomasyon hesabına rapor bildiren bir Karma İşçi barındırabilir.

Bir Karma Runbook Çalışanı'nda bir runbook başlattığınızda, üzerinde çalıştığı grubu belirtirsiniz. Gruptaki her çalışan, kullanılabilir iş olup olmadığını görmek için Azure Otomasyonu'nu yoklar. Bir iş varsa, işi alan ilk işçi alır. İş sırasının işlem süresi Karma alt donanım profiline ve yüküne bağlıdır. Belirli bir işçiyi belirtemezsiniz. Karma Runbook Çalışanları, Azure kum kutularının sahip olduğu sınırların çoğunu paylaşmaz. Disk alanı, bellek veya ağ yuvalarında aynı sınırlara sahip değildirler. Karma Runbook Çalışanları yalnızca Karma Runbook Worker'ın kendisindeki kaynaklarla sınırlıdır. Buna ek olarak, Karma Runbook Çalışanları Azure ve zimtbox'larının sahip olduğu 180 dakikalık [adil paylaşım](automation-runbook-execution.md#fair-share) süresi sınırını paylaşmaz. Azure ve Karma Runbook Çalışanları için hizmet sınırları hakkında daha fazla bilgi edinmek için iş [sınırlarına](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits)bakın.

## <a name="install-a-hybrid-runbook-worker"></a>Karma Runbook Worker yükleme

Karma Runbook Worker yükleme işlemi işletim sistemine bağlıdır. Aşağıdaki tabloda dağıtım türleri tanımlanmektedir.

|İşletim Sistemi  |Dağıtım türleri  |
|---------|---------|
|Windows     | [PowerShell](automation-windows-hrw-install.md#automated-deployment)<br>[El ile](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)        |

Önerilen yükleme yöntemi, bir Windows bilgisayarını yapılandırma işlemini tamamen otomatikleştirmek için bir Otomasyon runbook kullanmaktır. İkinci yöntem, rolü el ile yüklemek ve yapılandırmak için adım adım yordamı izlemektir. Linux makineleri için aracıyı makineye yüklemek için bir Python komut dosyası çalıştırın.

> [!NOTE]
> İstenilen Durum Yapılandırması (DSC) ile Karma Runbook Worker rolünü destekleyen sunucularınızın yapılandırmasını yönetmek için sunucuları DSC düğümleri olarak eklemeniz gerekir. DSC ile yönetim için bunları onboarding hakkında daha fazla bilgi için, [Azure Automation DSC tarafından yönetimi için Onboarding makineleri](automation-dsc-onboarding.md)bakın.
>
>Güncelleştirme Yönetimi [çözümlüğünü](automation-update-management.md)etkinleştiriseniz, Azure Log Analytics çalışma alanınıza bağlı tüm bilgisayarlar, bu çözümde yer alan runbook'ları desteklemek üzere otomatik olarak Karma Runbook Çalışanı olarak yapılandırılır. Ancak, bilgisayar Otomasyon hesabınızda zaten tanımlanmış olan Hibrit İşçi gruplarına kayıtlı değildir. Bilgisayar, hem çözüm hem de Karma Runbook İşçi grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon runbook kitaplarını desteklemek için Otomasyon hesabınızdaki Bir Karma Runbook İşçisi grubuna eklenebilir. Bu işlev, Hybrid Runbook Worker'ın 7.2.12024.0 sürümüne eklendi.

Karma Runbook Worker dağıtmaya başlamadan önce [ağınızı planlamak için bilgileri](#network-planning) gözden geçirin. İşçiyi başarıyla dağıttıktan sonra, çalışma kitaplarınızı şirket içi veri merkezinizdeki veya diğer bulut ortamınızdaki işlemleri otomatikleştirmek üzere nasıl yapılandırabileceğinizi öğrenmek için [Bir Karma Runbook Çalışanı'nda Çalıştır runbook'larını](automation-hrw-run-runbooks.md) gözden geçirin.

Bilgisayar, hem çözüm hem de Karma Runbook İşçi grup üyeliği için aynı hesabı kullandığınız sürece Otomasyon runbook kitaplarını desteklemek için Otomasyon hesabınızdaki Bir Karma Runbook İşçisi grubuna eklenebilir. Bu işlev Karma Runbook Çalışanının 7.2.12024.0 sürümüne eklenmiştir.

## <a name="a-nameremove-a-hybrid-runbook-workerremove-a-hybrid-runbook-worker-from-an-on-premises-computer"></a><a name="remove-a-hybrid-runbook-worker">Karma Runbook İşçisi'ni şirket içi bilgisayardan kaldırma

Windows ve Linux için bu bölümde açıklandığı gibi, bir Karma Runbook İşçisi'ni şirket içi bir bilgisayardan kaldırabilirsiniz.

### <a name="remove-the-worker-on-windows"></a>Windows'da çalışan kaldırma

1. Azure portalında Otomasyon hesabınıza gidin.
2. **Hesap Ayarları** **altında, Anahtarlar'ı** seçin ve **URL** ve Birincil **Erişim Anahtarı**değerlerini not alın.

3. Administrator modunda bir PowerShell oturumu açın ve URL'niz ve birincil erişim anahtar değerlerinizle aşağıdaki komutu çalıştırın. Kaldırma `Verbose` işleminin ayrıntılı bir günlüğü için parametreyi kullanın. Karma İşçi grubunuzun eski makineleri kaldırmak için `machineName` isteğe bağlı parametreyi kullanın.

```powershell-interactive
Remove-HybridRunbookWorker -url <URL> -key <PrimaryAccessKey> -machineName <ComputerName>
```

### <a name="remove-the-worker-on-linux"></a>Linux'ta çalışanı kaldırın

Çalışma alanı kimliğini `ls /var/opt/microsoft/omsagent` almak için Karma Runbook İşçisi'ndeki komutu kullanabilirsiniz. Çalışma alanı kimliğiyle birlikte adlandırılmış bir klasör oluşturulur.

```bash
sudo python onboarding.py --deregister --endpoint="<URL>" --key="<PrimaryAccessKey>" --groupname="Example" --workspaceid="<workspaceId>"
```

> [!NOTE]
> Bu kod, Microsoft İzleme Aracısını bilgisayardan kaldırmaz. Yalnızca Karma Runbook Worker rolünün işlevselliğini ve yapılandırmasını kaldırır.

## <a name="remove-a-hybrid-worker-group"></a>Karma Çalışanı grubunu kaldırma

Karma Runbook Worker grubunu kaldırmak için öncelikle Grubun üyesi olan her bilgisayardan Karma Runbook İşçisi'ni kaldırmanız gerekir. Ardından grubu kaldırmak için aşağıdaki adımları kullanın:

1. Azure portalında Otomasyon hesabını açın.
2. **Proses Otomasyonu**altında **Karma işçi gruplarını** seçin. Silmek istediğiniz grubu seçin. Bu grubun özellikleri sayfası görüntülenir.

   ![Özellikler sayfası](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-group-properties.png)

3. Seçili grubun özellikleri sayfasında **Sil'i**seçin. Bir ileti, bu eylemi onaylamanızı ister. Devam etmek istediğinizden eminseniz **Evet'i** seçin.

   ![Onay iletisi](media/automation-hybrid-runbook-worker/automation-hybrid-runbook-worker-confirm-delete.png)

   Bu işlemin tamamlanması birkaç saniye sürebilir. Bu işlemin ilerleme durumunu menüdeki **Bildirimler**’in altından izleyebilirsiniz.

## <a name="configure-your-network"></a><a name="network-planning"></a>Ağınızı yapılandırma

### <a name="hybrid-worker-role"></a>Hibrit İşçi rolü

Karma Runbook Worker'ın Azure Otomasyonuna bağlanıp kaydolabilmesi için, bu bölümde açıklanan bağlantı noktası numarasına ve URL'lere erişimi olması gerekir. Bu erişim, Microsoft İzleme Aracısı'nın Azure Monitor günlüklerine bağlanması [için gereken bağlantı noktalarına ve URL'lere](../azure-monitor/platform/agent-windows.md) en üsttedir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Aracı ile Azure Otomasyonhizmeti arasındaki iletişim için bir proxy sunucusu kullanıyorsanız, uygun kaynakların erişilebilir olduğundan emin olun. Hybrid Runbook Worker ve Otomasyon hizmetlerinden gelen istekler için zaman dilimi 30 saniyedir. 3 denemeden sonra istek başarısız olur. Internet erişimini kısıtlamak için bir güvenlik duvarı kullanıyorsanız, güvenlik duvarınızı erişime izin vermek üzere yapılandırmanız gerekir. Log Analytics ağ geçidini proxy olarak kullanıyorsanız, bunun karma çalışanlar için yapılandırıldığından emin olun. Bunun nasıl yapılacağını anlatan talimatlar [için, Otomasyon Karma Çalışanları için Log Analytics ağ geçidini yapılandırın.](https://docs.microsoft.com/azure/log-analytics/log-analytics-oms-gateway)

Hibrit Runbook Worker rolünün Otomasyon ile iletişim kurması için aşağıdaki bağlantı noktası ve URL'ler gereklidir:

* Bağlantı noktası: Giden internet erişimi için yalnızca TCP 443 gereklidir.
* Genel URL: *.azure-automation.net
* US Gov Virginia'nın Genel URL'si: *.azure-automation.us
* Aracı hizmeti:\<https://\>çalışma alanıId .agentsvc.azure-automation.net

Özel durumları tanımlarken listelenen adreslerin kullanılması önerilir. IP adresleri için [Microsoft Azure Veri Merkezi IP Aralıklarını](https://www.microsoft.com/en-us/download/details.aspx?id=56519)indirebilirsiniz. Bu dosya haftalık olarak güncellenir ve şu anda dağıtılan aralıklara ve IP aralıklarında yapılacak değişikliklere sahiptir.

Belirli bir bölge için tanımlanmış bir Otomasyon hesabınız varsa, iletişimi o bölgesel veri merkeziyle sınırlandırabilirsiniz. Aşağıdaki tablo her bölge için DNS kaydısağlar:

| **Bölge** | **DNS kaydı** |
| --- | --- |
| Orta Batı ABD | wcus-jobruntimedata-prod-su1.azure-automation.net</br>wcus-agentservice-prod-1.azure-automation.net |
| Orta Güney ABD |scus-jobruntimedata-prod-su1.azure-automation.net</br>scus-agentservice-prod-1.azure-automation.net |
| Doğu ABD 2 |eus2-jobruntimedata-prod-su1.azure-automation.net</br>eus2-agentservice-prod-1.azure-automation.net |
| Batı ABD 2 |wus2-jobruntimedata-prod-su1.azure-automation.net</br>wus2-agentservice-prod-1.azure-automation.net |
| Orta Kanada |cc-jobruntimedata-prod-su1.azure-automation.net</br>cc-agentservice-prod-1.azure-automation.net |
| Batı Avrupa |we-jobruntimedata-prod-su1.azure-automation.net</br>we-agentservice-prod-1.azure-automation.net |
| Kuzey Avrupa |ne-jobruntimedata-prod-su1.azure-automation.net</br>ne-agentservice-prod-1.azure-automation.net |
| Güneydoğu Asya |sea-jobruntimedata-prod-su1.azure-automation.net</br>sea-agentservice-prod-1.azure-automation.net|
| Orta Hindistan |cid-jobruntimedata-prod-su1.azure-automation.net</br>cid-agentservice-prod-1.azure-automation.net |
| Doğu Japonya |jpe-jobruntimedata-prod-su1.azure-automation.net</br>jpe-agentservice-prod-1.azure-automation.net |
| Orta Avustralya |ac-jobruntimedata-prod-su1.azure-automation.net</br>ac-agentservice-prod-1.azure-automation.net |
| Doğu Avustralya |ae-jobruntimedata-prod-su1.azure-automation.net</br>ae-agentservice-prod-1.azure-automation.net |
| Avustralya Güneydoğu |ase-jobruntimedata-prod-su1.azure-automation.net</br>ase-agentservice-prod-1.azure-automation.net |
| Güney Birleşik Krallık | uks-jobruntimedata-prod-su1.azure-automation.net</br>uks-agentservice-prod-1.azure-automation.net |
| US Gov Virginia | usge-jobruntimedata-prod-su1.azure-automation.us<br>usge-agentservice-prod-1.azure-automation.us |

Bölge adları yerine bölge IP adresleri listesi için Microsoft İndirme Merkezi'nden [Azure Veri Merkezi IP adresi](https://www.microsoft.com/download/details.aspx?id=41653) XML dosyasını indirin.

> [!NOTE]
> Azure Veri Merkezi IP adresi XML dosyası, Microsoft Azure veri merkezlerinde kullanılan IP adres aralıklarını listeler. Dosya, işlem, SQL ve depolama aralıklarını içerir.
>
>Güncelleştirilmiş bir dosya haftalık olarak deftere nakledilir. Dosya, şu anda dağıtılan aralıkları ve IP aralıklarında yapılacak değişiklikleri yansıtır. Dosyada görünen yeni aralıklar veri merkezlerinde en az bir hafta boyunca kullanılmaz.
>
> Yeni XML dosyasını her hafta indirmek iyi bir fikirdir. Ardından, Azure'da çalışan hizmetleri doğru tanımlamak için sitenizi güncelleştirin. Azure ExpressRoute kullanıcıları, bu dosyanın her ayın ilk haftasında Azure alanının Kenarlık Ağ Geçidi Protokolü (BGP) reklamını güncelleştirmek için kullanıldığını belirtmelidir.

### <a name="update-management"></a>Güncelleştirme Yönetimi

Karma Runbook Worker'ın gerektirdiği standart adres ve bağlantı noktalarının yanı üstünde, güncelleştirme yönetimi için özel olarak aşağıdaki adresler gereklidir. Bu adreslere iletişim 443 portu üzerinden yapılır.

|Azure Genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|

## <a name="next-steps"></a>Sonraki adımlar

* Çalışma kitaplarınızı şirket içi veri merkezinizdeki veya diğer bulut ortamınızdaki işlemleri otomatikleştirmek için nasıl yapılandırılabildiğini öğrenmek için, [Karma Runbook Çalışanı'ndaki runbook'lara](automation-hrw-run-runbooks.md)bakın.
* Karma Runbook Çalışanlarınızı nasıl gidererinizi öğrenmek için, [Sorun Giderme Karma Runbook Çalışanları'na](troubleshoot/hybrid-runbook-worker.md#general)bakın.