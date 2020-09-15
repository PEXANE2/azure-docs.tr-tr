---
title: Azure Otomasyonu karma Runbook Worker genel bakış
description: Bu makalede, yerel veri merkezinizdeki veya bulut sağlayıcınızdaki makinelerde runbook 'ları çalıştırmak için kullanabileceğiniz karma Runbook Worker 'a genel bakış sunulmaktadır.
services: automation
ms.subservice: process-automation
ms.date: 09/14/2020
ms.topic: conceptual
ms.openlocfilehash: f5dc9305df8ce0e26e13738d605849fa75cc53a7
ms.sourcegitcommit: 07166a1ff8bd23f5e1c49d4fd12badbca5ebd19c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90087906"
---
# <a name="hybrid-runbook-worker-overview"></a>Karma Runbook Çalışanına genel bakış

Azure Otomasyonu 'ndaki runbook 'lar, Azure bulut platformunda çalıştıkları için diğer bulutlardaki veya şirket içi ortamınızdaki kaynaklara erişime sahip olmayabilir. Runbook 'u doğrudan rolü barındıran makinede ve bu yerel kaynakları yönetmek için ortamdaki kaynaklara karşı çalıştırmak için Azure Automation 'ın karma Runbook Worker özelliğini kullanabilirsiniz. Runbook 'lar Azure Otomasyonu 'nda depolanır ve yönetilir ve ardından bir veya daha fazla atanmış makineye dağıtılır.

Aşağıdaki görüntüde Bu işlevsellik gösterilmektedir:

![Karma Runbook Çalışanına genel bakış](media/automation-hybrid-runbook-worker/automation.png)

Karma Runbook Worker, Windows veya Linux işletim sisteminde çalışabilir. Azure Izleyici [Log Analytics çalışma alanına](../azure-monitor/platform/design-logs-deployment.md)raporlama [Log Analytics aracısına](../azure-monitor/platform/log-analytics-agent.md) bağlıdır. Çalışma alanı yalnızca desteklenen işletim sistemi için makine izlemek değildir, ancak karma Runbook Worker için gereken bileşenleri de indirir.

Her karma runbook çalışanı, aracıyı yüklerken belirttiğiniz bir karma Runbook Worker grubunun üyesidir. Bir grup tek bir aracı içerebilir, ancak yüksek kullanılabilirlik için bir gruba birden çok aracı yükleyebilirsiniz. Her makine, tek bir Otomasyon hesabına tek bir karma çalışanı raporlama barındırabilir.

Karma Runbook Worker üzerinde bir runbook başlattığınızda, üzerinde çalıştığı grubu belirtirsiniz. Gruptaki her çalışan, kullanılabilir işlerin olup olmadığını görmek için Azure Otomasyonu ' nu yoklar. Bir iş varsa, işi almak için ilk çalışan onu alır. İşler sırasının işlem süresi karma çalışan donanım profiline ve yüküne bağlıdır. Belirli bir çalışanı belirtemezsiniz.

Disk alanı, bellek veya ağ yuvaları üzerinde birçok korumalı alan [sınırı](../azure-resource-manager/management/azure-subscription-service-limits.md#automation-limits) olmadığından [Azure korumalı alanı](automation-runbook-execution.md#runbook-execution-environment) yerine karma runbook çalışanı kullanın. Karma çalışanındaki sınırlar yalnızca çalışan kaynakları ile ilgilidir.

> [!NOTE]
> Karma runbook çalışanları, Azure korumalı alan tarafından sahip olduğu [dengeli bir paylaşılan](automation-runbook-execution.md#fair-share) süre sınırı ile sınırlı değildir.

## <a name="hybrid-runbook-worker-installation"></a>Karma Runbook Worker yüklemesi

Karma Runbook Worker 'ı yüklemek için işlem işletim sistemine bağlıdır. Aşağıdaki tabloda dağıtım türleri tanımlanmaktadır.

|İşletim Sistemi  |Dağıtım türleri  |
|---------|---------|
|Windows     | [Otomatik](automation-windows-hrw-install.md#automated-deployment)<br>[El ile](automation-windows-hrw-install.md#manual-deployment)        |
|Linux     | [Python](automation-linux-hrw-install.md#install-a-linux-hybrid-runbook-worker)        |

Önerilen yükleme yöntemi, bir Windows makinesini yapılandırma işlemini tamamen otomatik hale getirmek için bir Azure Otomasyonu runbook 'unu kullanmaktır. Bu uygun değilse, rolü el ile yüklemek ve yapılandırmak için adım adım prosedürü izleyebilirsiniz. Linux makinelerinde aracıyı makineye yüklemek için bir Python betiği çalıştırırsınız.

## <a name="network-planning"></a><a name="network-planning"></a>Ağ planlama

Karma Runbook Worker 'ın Azure Otomasyonu 'na bağlanması ve kaydolmasının ardından bu bölümde açıklanan bağlantı noktası numarasına ve URL 'Lere erişimi olması gerekir. Çalışan Ayrıca Log Analytics aracısının Azure Izleyici Log Analytics çalışma alanına bağlanması için [gereken bağlantı noktalarına ve URL 'lere](../azure-monitor/platform/agent-windows.md) erişebilmelidir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Karma Runbook Worker için aşağıdaki bağlantı noktası ve URL 'Ler gereklidir:

* Bağlantı noktası: giden internet erişimi için yalnızca TCP 443 gerekir
* Genel URL: `*.azure-automation.net`
* US Gov Virginia genel URL 'SI: `*.azure-automation.us`
* Aracı hizmeti: `https://<workspaceId>.agentsvc.azure-automation.net`

Belirli bir bölge için tanımlanan bir Otomasyon hesabınız varsa, karma Runbook Worker iletişimini ilgili bölge veri merkezi ile kısıtlayabilirsiniz. Azure Otomasyonu tarafından gerekli DNS kayıtları için [kullanılan DNS kayıtlarını](how-to/automation-region-dns-records.md) gözden geçirin.

### <a name="proxy-server-use"></a>Proxy sunucusu kullanımı

Azure Otomasyonu ve Log Analytics aracısını çalıştıran makineler arasında iletişim için bir ara sunucu kullanıyorsanız, uygun kaynakların erişilebilir olduğundan emin olun. Karma runbook worker ve Automation hizmetlerinden gelen isteklerin zaman aşımı 30 saniyedir. Üç denemeden sonra bir istek başarısız olur.

### <a name="firewall-use"></a>Güvenlik duvarı kullanımı

İnternet 'e erişimi kısıtlamak için güvenlik duvarı kullanıyorsanız, güvenlik duvarını erişime izin verecek şekilde yapılandırmanız gerekir. Log Analytics ağ geçidini ara sunucu olarak kullanıyorsanız, karma runbook çalışanları için yapılandırıldığından emin olun. Bkz. [Otomasyon karma runbook çalışanları için Log Analytics ağ geçidini yapılandırma](../azure-monitor/platform/gateway.md).

### <a name="service-tags"></a>Hizmet etiketleri

Azure Otomasyonu, Azure sanal ağ hizmeti etiketlerini destekleyerek [Guestandhybridmanagement](../virtual-network/service-tags-overview.md)hizmet etiketiyle başlar. [Ağ güvenlik gruplarında](../virtual-network/security-overview.md#security-rules) veya [Azure Güvenlik duvarında](../firewall/service-tags.md)ağ erişim denetimleri tanımlamak için hizmet etiketlerini kullanabilirsiniz. Hizmet etiketleri, güvenlik kuralları oluştururken belirli IP adreslerinin yerine kullanılabilir. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adı **Guestandhybridmanagement**  ' i belirterek, Otomasyon Hizmeti için trafiğe izin verebilir veya bu trafiği reddedebilirsiniz. Bu hizmet etiketi, IP aralıklarını belirli bir bölgeyle kısıtlayarak daha ayrıntılı denetim yapılmasına izin vermeyi desteklemez.

Azure Otomasyonu hizmetinin hizmet etiketi yalnızca aşağıdaki senaryolar için kullanılan IP 'Leri sağlar:

* Sanal ağınızın içinden Web kancalarını tetikleme
* VNet 'teki karma runbook çalışanları veya durum yapılandırması aracılarının otomasyon hizmetiyle iletişim kurmasına izin verin

>[!NOTE]
>**Guestandhybridmanagement** hizmet etiketi şu anda yalnızca bir karma runbook çalışanında bulunan bir Azure korumalı alanında runbook işi yürütmeyi desteklememektedir.

## <a name="support-for-impact-level-5-il5"></a>Etki düzeyi 5 için destek (IL5)

Azure Otomasyonu karma Runbook Worker, Azure Kamu 'da aşağıdaki iki yapılandırmanın herhangi birindeki 5 iş yüklerini etkileyebilir:

* [Yalıtılmış sanal makine](../azure-government/documentation-government-impact-level-5.md#isolated-virtual-machines). Dağıtıldığında, IL5 iş yüklerini desteklemek için gereken yalıtım düzeyini sağlayan bu sanal makine için tüm fiziksel ana bilgisayarı tüketir.

* Tek bir Azure aboneliğine ayrılmış bir veya daha fazla sanal makineyi barındırabilen fiziksel sunucular sağlayan [Azure ayrılmış Konakları](../azure-government/documentation-government-impact-level-5.md#azure-dedicated-hosts).

>[!NOTE]
>Karma runbook çalışanı rolü aracılığıyla işlem yalıtımı, Azure ticari ve ABD kamu bulutları için kullanılabilir. 

## <a name="update-management-on-hybrid-runbook-worker"></a>Karma Runbook Worker üzerinde Güncelleştirme Yönetimi

Azure Otomasyonu [güncelleştirme yönetimi](update-management/update-mgmt-overview.md) etkinleştirildiğinde, Log Analytics çalışma alanınıza bağlı tüm makineler otomatik olarak karma runbook çalışanı olarak yapılandırılır. Her çalışan, güncelleştirme yönetimine hedeflenmiş runbook 'ları destekleyebilir.

Bu şekilde yapılandırılan bir makine, Otomasyon hesabınızda zaten tanımlanmış olan bir karma Runbook Worker grubuna kayıtlı değildir. Makineyi bir karma Runbook Worker grubuna ekleyebilirsiniz, ancak hem Güncelleştirme Yönetimi hem de karma runbook çalışanı grup üyeliği için aynı hesabı kullanmanız gerekir. Bu işlev karma Runbook Worker 'ın 7.2.12024.0 sürümüne eklenmiştir.

### <a name="update-management-addresses-for-hybrid-runbook-worker"></a>Karma Runbook Worker için Güncelleştirme Yönetimi adresleri

Karma Runbook Worker için gereken standart adreslerin ve bağlantı noktalarının üstünde, Güncelleştirme Yönetimi [ağ planlama](update-management/update-mgmt-overview.md#ports) bölümünde açıklanan ek ağ yapılandırması gereksinimleri vardır.

## <a name="azure-automation-state-configuration-on-a-hybrid-runbook-worker"></a>Karma Runbook Worker üzerinde Azure Otomasyonu durum yapılandırması

[Azure Otomasyonu durum yapılandırmasını](automation-dsc-overview.md) karma Runbook Worker üzerinde çalıştırabilirsiniz. Karma runbook çalışanını destekleyen sunucuların yapılandırmasını yönetmek için sunucuları DSC düğümleri olarak eklemeniz gerekir. Bkz. [Azure Otomasyonu durum yapılandırması ile yönetimi için makineleri etkinleştirme](automation-dsc-onboarding.md).

## <a name="runbooks-on-a-hybrid-runbook-worker"></a>Karma Runbook Worker 'daki runbook 'lar

Yerel makinedeki kaynakları yöneten veya karma Runbook Worker 'ın dağıtıldığı Yerel ortamdaki kaynaklara karşı çalışan runbook 'larınız olabilir. Bu durumda, runbook 'larınızı bir Otomasyon hesabı yerine karma çalışan üzerinde çalıştırmayı seçebilirsiniz. Karma Runbook Worker üzerinde çalışan runbook 'lar, Otomasyon hesabında çalıştıranlarla aynı yapıyla aynıdır. Bkz. [bir karma runbook çalışanında runbook 'Ları çalıştırma](automation-hrw-run-runbooks.md).

### <a name="hybrid-runbook-worker-jobs"></a>Karma Runbook Worker işleri

Karma Runbook Worker işleri Windows üzerinde yerel **sistem** hesabı veya Linux üzerinde [nxautomation hesabı](automation-runbook-execution.md#log-analytics-agent-for-linux) altında çalışır. Azure Otomasyonu, karma runbook çalışanları üzerinde işleri, Azure korumalı alanlar 'da çalıştırılan işlerden farklı bir şekilde işler. Bkz. [runbook yürütme ortamı](automation-runbook-execution.md#runbook-execution-environment).

Karma Runbook Worker konak makinesi yeniden başlatılırsa, çalışan runbook işi başlangıçtan itibaren veya PowerShell Iş akışı runbook 'ları için son denetim noktasından başlatılır. Bir runbook işi üç kez yeniden başlatıldıktan sonra askıya alınır.

### <a name="runbook-permissions-for-a-hybrid-runbook-worker"></a>Karma Runbook Worker için Runbook izinleri

Azure dışı kaynaklara erişirken, karma runbook çalışanı üzerinde çalışan runbook 'lar, genellikle Azure kaynaklarında kimlik doğrulaması yapan runbook 'lar tarafından kullanılan kimlik doğrulama mekanizmasını kullanamaz. Bir runbook yerel kaynaklara kendi kimlik doğrulamasını sağlar veya [Azure kaynakları için yönetilen kimlikleri](../active-directory/managed-identities-azure-resources/tutorial-windows-vm-access-arm.md#grant-your-vm-access-to-a-resource-group-in-resource-manager)kullanarak kimlik doğrulamasını yapılandırır. Ayrıca, tüm runbook 'lar için bir kullanıcı bağlamı sağlamak üzere bir farklı çalıştır hesabı belirtebilirsiniz.

## <a name="view-hybrid-runbook-workers"></a>Karma runbook çalışanlarını görüntüleme

Güncelleştirme Yönetimi özelliği Windows sunucularında veya VM 'lerde etkinleştirildikten sonra, Azure portal sistem karma runbook çalışanları grubunun listesini envantere kaydedebilirsiniz. Seçilen Otomasyon hesabı için sol bölmeden **karma çalışanlar grubu** seçeneğini belirleyerek, portalda en **System hybrid workers group** fazla 2.000 çalışanı görüntüleyebilirsiniz ve ardından,

:::image type="content" source="./media/automation-hybrid-runbook-worker/system-hybrid-workers-page.png" alt-text="Otomasyon hesabı sistemi karma çalışanı grupları sayfası" border="false" lightbox="./media/automation-hybrid-runbook-worker/system-hybrid-workers-page.png":::

2.000 ' den fazla karma çalışanı varsa, tümünün listesini almak için aşağıdaki PowerShell betiğini çalıştırabilirsiniz:

```powershell
"Get-AzSubscription -SubscriptionName "<subscriptionName>" | Set-AzContext
$workersList = (Get-AzAutomationHybridWorkerGroup -ResourceGroupName "<resourceGroupName>" -AutomationAccountName "<automationAccountName>").Runbookworker
$workersList | export-csv -Path "<Path>\output.csv" -NoClobber -NoTypeInformation"
```

## <a name="next-steps"></a>Sonraki adımlar

* Runbook 'larınızı şirket içi veri merkezinizde veya diğer bulut ortamınızda otomatik hale getirmek üzere nasıl yapılandıracağınızı öğrenmek için bkz. [runbook 'Ları karma Runbook Worker üzerinde çalıştırma](automation-hrw-run-runbooks.md).

* Karma runbook çalışanlarınızın sorunlarını giderme hakkında bilgi edinmek için bkz. [karma Runbook Worker sorunlarını giderme](troubleshoot/hybrid-runbook-worker.md#general).
