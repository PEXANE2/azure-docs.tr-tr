---
title: Azure 'da Güncelleştirme Yönetimi çözümü
description: Bu makalede, Windows ve Linux makineleriniz için güncelleştirmeleri yönetmek üzere Azure Güncelleştirme Yönetimi çözümünün nasıl kullanılacağı açıklanır.
services: automation
ms.subservice: update-management
ms.date: 02/27/2020
ms.topic: conceptual
ms.openlocfilehash: c76b14e4f08ec930159498da4a35fdad0341929e
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79278512"
---
# <a name="update-management-solution-in-azure"></a>Azure 'da Güncelleştirme Yönetimi çözümü

Azure 'da, şirket içi ortamlarda ve diğer bulut ortamlarında Windows ve Linux makineleriniz için işletim sistemi güncelleştirmelerini yönetmek üzere Azure Otomasyonu 'nda Güncelleştirme Yönetimi çözümünü kullanabilirsiniz. Tüm aracı makinelerde kullanılabilir güncelleştirmelerin durumunu hızlı bir şekilde değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

Aşağıdaki yöntemleri kullanarak sanal makineler (VM) için Güncelleştirme Yönetimi etkinleştirebilirsiniz:

- Azure [Otomasyonu hesabınızdan](automation-onboard-solutions-from-automation-account.md) bir veya daha fazla Azure makinesi Için ve Azure dışı makineler için el ile.

- Tek bir Azure VM için Azure portal sanal makine sayfasından. Bu senaryo, [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) ve [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management) VM 'leri için kullanılabilir.

- [Birden çok Azure VM](manage-update-multi.md) Için Azure Portal **sanal makineler** sayfasında bunları seçerek. 

> [!NOTE]
> Güncelleştirme Yönetimi çözümü, Otomasyon hesabınıza bir Log Analytics çalışma alanı bağlamayı gerektirir. Desteklenen bölgelerin kesin listesi için bkz. [Azure çalışma alanı eşlemeleri](./how-to/region-mappings.md). Bölge eşlemeleri, Otomasyon hesabınızdan ayrı bir bölgedeki VM 'Leri yönetme özelliğini etkilemez.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Güncelleştirme Yönetimi çözümünü yeni veya mevcut bir Otomasyon hesabına dağıtmanıza ve aboneliğinizdeki Log Analytics çalışma alanına dağıtmanızı sağlayan bir Azure [Kaynak Yöneticisi şablonu](automation-update-management-deploy-template.md) kullanılabilir.

## <a name="solution-overview"></a>Çözüme genel bakış

Güncelleştirme Yönetimi tarafından yönetilen makineler, değerlendirme gerçekleştirmek ve dağıtımları güncelleştirmek için aşağıdaki konfigürasyonları kullanır:

* Windows veya Linux için Log Analytics Aracısı
* Linux için PowerShell İstenen Durum Yapılandırması (DSC)
* Otomasyon Karma Runbook Çalışanı
* Windows makineleri için Microsoft Update veya Windows Server Update Services (WSUS)

Aşağıdaki diyagramda, çözümün çalışma alanındaki tüm bağlı Windows Server ve Linux makinelerine nasıl değerlendirir ve güvenlik güncelleştirmeleri uyguladığı gösterilmektedir:

![İşlem akışı Güncelleştirme Yönetimi](./media/automation-update-management/update-mgmt-updateworkflow.png)

Güncelleştirme Yönetimi, makineleri aynı kiracıda birden çok aboneliğe yerel olarak eklemek için kullanılabilir.

Bir paket yayımlandıktan sonra, düzeltme ekinin değerlendirme için Linux makinelere gösterilmesi 2 ile 3 saat sürer. Windows makinelerinde, düzeltme ekinin yayımlandıktan sonra değerlendirmesi göstermesi için 12-15 saat sürer.

Bir makine güncelleştirme uyumluluğu taramasını tamamladıktan sonra, aracı bilgileri toplu olarak Azure Izleyici günlüklerine iletir. Bir Windows makinesinde, uyumluluk taraması varsayılan olarak her 12 saatte bir çalıştırılır.

Tarama zamanlamasının yanı sıra, güncelleştirme uyumluluğu taraması, Log Analytics aracısının yeniden başlatılmasından sonra, güncelleştirme yüklenmeden önce ve güncelleştirme yüklemesinden sonra 15 dakika içinde başlatılır.

Bir Linux makinesi için, uyumluluk taraması her saat varsayılan olarak gerçekleştirilir. Log Analytics Aracısı yeniden başlatılırsa, 15 dakika içinde bir uyumluluk taraması başlatılır.

Çözüm, makinenin ne kadar güncel olduğunu, ile eşitlemek için yapılandırdığınız kaynağı temel alır. Windows makinesi WSUS 'e rapor verecek şekilde yapılandırıldıysa, WSUS Microsoft Update ile en son ne zaman eşitlendiğine bağlı olarak sonuçlar Microsoft Update gösterdiği gibi farklılık gösterebilir. Bu davranış, bir genel depoya değil yerel depoya raporlamak üzere yapılandırılmış Linux makineleri için aynıdır.

> [!NOTE]
> Hizmete doğru bir şekilde raporlamak için, Güncelleştirme Yönetimi belirli URL 'Lerin ve bağlantı noktalarının etkinleştirilmesini gerektirir. Bu gereksinimler hakkında daha fazla bilgi edinmek için bkz. [karma çalışanlar Için ağ planlaması](automation-hybrid-runbook-worker.md#network-planning).

Zamanlanmış bir dağıtım oluşturarak güncelleştirmeleri gerektiren makinelere yazılım güncelleştirmeleri dağıtabilir ve yükleyebilirsiniz. *Isteğe bağlı* olarak sınıflandırılan güncelleştirmeler Windows makineler için dağıtım kapsamına dahil edilmez. Dağıtım kapsamında yalnızca gerekli güncelleştirmeler bulunur.

Zamanlanan dağıtım, hangi hedef makinelerin geçerli güncelleştirmeleri alacağını tanımlar. Bu, belirli makineleri açıkça belirterek ya da belirli bir makine kümesinin (ya da belirtilen ölçütlere göre dinamik olarak Azure VM 'Leri seçen bir [Azure sorgusundaki](automation-update-management-query-logs.md) ) günlük aramalarını temel alan bir [bilgisayar grubu](../azure-monitor/platform/computer-groups.md) seçerek gerçekleştirir. Bu gruplar, yalnızca çözümü etkinleştiren yönetim paketlerini hangi makinelerin almasını belirleyen [kapsam yapılandırmasından](../azure-monitor/insights/solution-targeting.md)farklıdır.

Ayrıca, güncelleştirmelerin yüklenebileceği bir zaman aralığını onaylamak ve ayarlamak için bir zamanlama da belirtirsiniz. Bu döneme bakım penceresi denir. Bakım penceresinin 20 dakikalık bir yayılımı yeniden başlatmalar için ayrılmıştır, bir tane gereklidir ve uygun yeniden başlatma seçeneğini seçmiş olursunuz. Düzeltme Eki beklenenden uzun sürüyorsa ve bakım penceresinde 20 dakikadan kısa bir süre sonra yeniden başlatma gerçekleşmez.

Güncelleştirmeler Azure Automation’daki runbook'lar tarafından yüklenir. Bu runbook 'ları görüntüleyemezsiniz ve herhangi bir yapılandırma gerektirmez. Bir güncelleştirme dağıtımı oluşturulduğunda, dahil edilen makineler için belirtilen zamanda ana güncelleştirme runbook 'unu Başlatan bir zamanlama oluşturur. Ana runbook, gerekli güncelleştirmeleri yüklemek için her bir aracıda bir alt runbook başlatır.

Güncelleştirme dağıtımında belirtilen tarih ve saatte, hedef makineler dağıtımı paralel olarak yürütür. Yüklemeden önce, güncelleştirmelerin hala gerekli olduğunu doğrulamak üzere bir tarama çalıştırılır. WSUS istemci makineleri için güncelleştirmeler WSUS 'ta onaylanmamışsa güncelleştirme dağıtımı başarısız olur.

Birden fazla Log Analytics çalışma alanında (aynı zamanda çoklu kayıt olarak da adlandırılır) Güncelleştirme Yönetimi için bir makinenin kayıtlı olması desteklenmez.

## <a name="clients"></a>İstemciler

### <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda güncelleştirme değerlendirmeleri için desteklenen işletim sistemleri listelenmektedir. Düzeltme eki uygulama karma Runbook Worker gerektirir. Karma Runbook Worker gereksinimleri hakkında bilgi için, bkz. [Windows karma Runbook Worker](automation-windows-hrw-install.md) ve [Linux hibrit Runbook Worker](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)yüklemek için Yükleme Kılavuzu.

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2019 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2016 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2012 R2 (Datacenter/Standard)<br><br>Windows Server 2012 || 
|Windows Server 2008 R2 (RTM ve SP1 standart)| Güncelleştirme Yönetimi yalnızca bu işletim sistemi için değerlendirme gerçekleştirmeyi destekler, [karma Runbook Worker](automation-windows-hrw-install.md) Windows Server 2008 R2 için desteklenmediğinden düzeltme eki uygulama desteklenmez. |
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme deposuna erişmesi gerekir. Sınıflandırma tabanlı düzeltme eki uygulama `yum`, CentOS 'ın RTM sürümlerindeki güvenlik verilerini döndürmesini gerektirir. CentOS üzerinde sınıflandırma tabanlı düzeltme eki uygulama hakkında daha fazla bilgi için bkz. [Linux üzerinde sınıflandırmaları güncelleştirme](automation-view-update-assessments.md#linux-2).          |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme deposuna erişmesi gerekir.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme deposuna erişmesi gerekir.        |
|Ubuntu 14,04 LTS, 16,04 LTS ve 18,04 (x86/x64)      |Linux aracılarının bir güncelleştirme deposuna erişmesi gerekir.         |

> [!NOTE]
> Azure sanal makine ölçek kümeleri, Güncelleştirme Yönetimi aracılığıyla yönetilebilir. Güncelleştirme Yönetimi, temel görüntüde değil örneklerin kendileri üzerinde işe yarar. Tüm sanal makine örneklerinin aynı anda güncelleştirilebilmesi için güncelleştirmeleri artımlı bir şekilde zamanlamanız gerekir.
> [Azure dışı bir makine](automation-tutorial-installed-software.md#onboard-a-non-azure-machine)ekleme altındaki adımları izleyerek sanal makine ölçek kümeleri için düğüm ekleyebilirsiniz.

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Aşağıdaki tabloda desteklenmeyen işletim sistemleri listelenmektedir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows istemcisi     | İstemci işletim sistemleri (örneğin, Windows 7 ve Windows 10) desteklenmez.        |
|Windows Server 2016 Nano sunucu     | Desteklenmiyor.       |
|Azure Kubernetes hizmet düğümleri | Desteklenmiyor. [Azure Kubernetes Service (AKS) Içindeki Linux düğümlerine güvenlik ve çekirdek güncelleştirmelerini uygulama](../aks/node-updates-kured.md) bölümünde açıklanan düzeltme eki uygulama sürecini kullanın|

### <a name="client-requirements"></a>İstemci gereksinimleri

Aşağıdaki bilgiler, işletim sistemine özgü istemci gereksinimlerini açıklamaktadır. Ek rehberlik için bkz. [ağ planlaması](#ports).

#### <a name="windows"></a>Windows

Windows aracılarının bir WSUS sunucusuyla iletişim kuracak şekilde yapılandırılması veya Microsoft Update erişmesi gerekir.

Configuration Manager Güncelleştirme Yönetimi kullanabilirsiniz. Tümleştirme senaryoları hakkında daha fazla bilgi için bkz. [Güncelleştirme Yönetimi Configuration Manager tümleştirme](oms-solution-updatemgmt-sccmintegration.md#configuration). [Log Analytics Windows Aracısı](../azure-monitor/platform/agent-windows.md) gereklidir. Bir Azure VM 'yi oluşturuyorsanız, aracı otomatik olarak yüklenir.

Varsayılan olarak, Azure Marketi 'nden dağıtılan Windows VM 'Leri Windows Update hizmetinden otomatik güncelleştirmeleri alacak şekilde ayarlanır. Bu çözümü eklediğinizde veya çalışma alanınıza Windows VM 'Leri eklediğinizde bu davranış değişmez. Bu çözümü kullanarak güncelleştirmeleri etkin bir şekilde yönetmezseniz varsayılan davranış (güncelleştirmeleri otomatik olarak uygulamak için) geçerlidir.

> [!NOTE]
> Bir Kullanıcı, makine yeniden başlatmaları yalnızca Kullanıcı tarafından değil, sistem tarafından gerçekleştirilebileceği şekilde grup ilkesi değiştirebilir. Güncelleştirme Yönetimi, kullanıcıdan el ile etkileşim kurmadan makinenin yeniden başlatılması için haklara sahip değilse, yönetilen makineler kalabilir.
>
> Daha fazla bilgi için bkz. [Otomatik Güncelleştirmeler için Grup İlkesi ayarlarını yapılandırma](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

#### <a name="linux"></a>Linux

Linux için, makinenin bir güncelleştirme deposuna erişmesi gerekir. Güncelleştirme deposu özel veya genel olabilir. Güncelleştirme Yönetimi etkileşimde bulunmak için TLS 1,1 veya TLS 1,2 gereklidir. Linux için birden fazla Log Analytics çalışma alanına raporlamak üzere yapılandırılmış bir Log Analytics Aracısı bu çözümle desteklenmez. Makinede Python 2. x de yüklü olmalıdır.

Linux için Log Analytics aracısını yükleme ve en son sürümü indirme hakkında daha fazla bilgi için bkz. [Linux için Log Analytics Aracısı](../azure-monitor/platform/agent-linux.md). Windows için Log Analytics aracısının nasıl yükleneceği hakkında bilgi için bkz. [Windows bilgisayarlarını Azure izleyici 'ye bağlama](../log-analytics/log-analytics-windows-agent.md).

Azure Marketi 'nde bulunan isteğe bağlı Red Hat Enterprise Linux (RHEL) görüntülerinden oluşturulan VM 'Ler, Azure 'da dağıtılan [Red Hat güncelleştirme altyapısına (rhuı)](../virtual-machines/workloads/redhat/redhat-rhui.md) erişmek için kaydedilir. Diğer tüm Linux dağıtımı, dağıtımın desteklenen yöntemleri kullanılarak dağıtımın çevrimiçi dosya deposundan güncelleştirilmeleri gerekir.

## <a name="permissions"></a>İzinler

Güncelleştirme dağıtımları oluşturmak ve yönetmek için belirli izinlere sahip olmanız gerekir. Bu izinler hakkında bilgi edinmek için bkz. [rol tabanlı erişim – güncelleştirme yönetimi](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Çözüm bileşenleri

Çözüm aşağıdaki kaynaklardan oluşur. Bu kaynaklar, çözümü etkinleştirdiğinizde Otomasyon hesabınıza otomatik olarak eklenir. 

### <a name="hybrid-worker-groups"></a>Karma Çalışanı grupları

Bu çözümü etkinleştirdikten sonra, Log Analytics çalışma alanınıza doğrudan bağlı olan tüm Windows makineleri, bu çözüme dahil olan runbook 'ları destekleyecek bir karma runbook çalışanı olarak otomatik olarak yapılandırılır.

Çözüm tarafından yönetilen her bir Windows makinesi, Otomasyon hesabı için bir **sistem karma çalışanı grubu** olarak **karma çalışan grupları** bölmesinde listelenir. Çözümler, *ana bilgisayar adı FQDN_GUID* adlandırma kuralını kullanır. Bu grupları hesabınızdaki runbook 'lar ile hedefleyebilirsiniz. Denerseniz, deneme başarısız olur. Bu gruplar yalnızca bu yönetim çözümünü desteklemek üzere tasarlanmıştır.

Hem çözüm hem de karma runbook çalışanı grup üyeliği için aynı hesabı kullanırsanız Otomasyon Runbook 'larını desteklemek için Otomasyon hesabınızdaki karma Runbook Worker grubuna Windows makinesini ekleyebilirsiniz. Bu işlev, karma Runbook Worker 'ın 7.2.12024.0 sürümüne eklenmiştir.

### <a name="management-packs"></a>Yönetim paketleri

System Center Operations Manager yönetim grubunuz [bir Log Analytics çalışma alanına bağlıysa](../azure-monitor/platform/om-agents.md), aşağıdaki yönetim paketleri Operations Manager yüklenir. Bu yönetim paketleri, çözümü ekledikten sonra doğrudan bağlı Windows makinelerine da yüklenir. Bu yönetim paketlerini yapılandırmanıza veya yönetmenize gerek yoktur.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* MP Dağıtımını güncelleştirme

> [!NOTE]
> Günlük verilerini toplamak için yönetim grubunda yapılandırılmış aracılarla bir Log Analytics çalışma alanına bağlı bir Operations Manager 1807 veya 2019 yönetim grubunuz varsa, bunları Güncelleştirme Yönetimi ile yönetmek için aşağıdaki kuralı geçersiz kılmanız gerekir: **ısotomatik RegistrationEnabled** parametresini geçersiz kılın ve **Microsoft. ıntelligencepacks. AzureAutomation. Hybridadgent. Init** kuralında **true** olarak ayarlayın.

Çözüm Yönetimi paketlerinin nasıl güncelleştirildiği hakkında daha fazla bilgi için bkz. [Azure izleyici günlüklerine bağlanma Operations Manager](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Operations Manager aracısı olan makineler Güncelleştirme Yönetimi tarafından tam olarak yönetilmek üzere, aracının Windows veya Linux için Log Analytics aracısına güncelleştirilmeleri gerekir. Aracıyı güncelleştirme hakkında bilgi edinmek için bkz. [Operations Manager aracısını yükseltme](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). Operations Manager kullanan ortamlarda, System Center Operations Manager 2012 R2 UR 14 veya sonraki bir sürümünü çalıştırıyor olmanız gerekir.

## <a name="data-collection"></a>Veri toplama

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| Windows aracıları |Yes |Çözüm, Windows aracılarından sistem güncelleştirmeleri hakkında bilgi toplar ve gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Yes |Çözüm, Linux aracılarından sistem güncelleştirmeleri hakkında bilgi toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Yes |Çözüm, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar.<br/><br/>Operations Manager aracısından Azure Izleyici günlüklerine doğrudan bağlantı gerekli değildir. Verileri yönetim grubundan Log Analytics çalışma alanına iletilir. |

### <a name="collection-frequency"></a>Toplama sıklığı

Her bir yönetilen Windows makinesi için günde iki kez tarama gerçekleştirilir. Her 15 dakikada bir Windows API 'si, durumun değişip değişmediğini belirlemekte son güncelleştirme zamanını sorgulamak için çağırılır. Durum değiştiyse, bir uyumluluk taraması başlatılır.

Her yönetilen Linux makinesi için saatte bir tarama gerçekleştirilir.

Panonun yönetilen makinelerden güncelleştirilmiş verileri görüntülemesi 30 dakika ile 6 saat arasında sürebilir.

Güncelleştirme Yönetimi kullanan bir makineye yönelik Azure Izleyici günlüklerinin ortalama veri kullanımı yaklaşık olarak ayda 25 megabayt (MB). Bu değer yalnızca bir yaklaşık değerdir ve ortamınıza bağlı olarak değişebilir. Tam kullanımınızın izlenmesini sağlamak için ortamınızı izlemenizi öneririz. Veri kullanımını çözümleme hakkında daha fazla bilgi için bkz. [kullanımı ve maliyeti yönetme](../azure-monitor/platform/manage-cost-storage.md).

## <a name="ports"></a>Ağ planlama

Aşağıdaki adresler Güncelleştirme Yönetimi için özel olarak gereklidir. 443 numaralı bağlantı noktası üzerinden bu adreslerle iletişim oluşur.

|Azure genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com    | *.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net | *.blob.core.usgovcloudapi.net|
|*.azure-automation.net | *. azure-automation.us|

Windows makinelerinde, Windows Update için gereken tüm uç noktalara giden trafiğe de izin vermeniz gerekir. [Http/proxy ile Ilgili sorunlarda](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy), gerekli uç noktaların güncelleştirilmiş bir listesini bulabilirsiniz. Yerel bir [Windows Update sunucunuz](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment)varsa, [WSUS anahtarınıza](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry)belirtilen sunucuya giden trafiğe de izin vermeniz gerekir.

Red Hat Linux makineleri için, gerekli uç noktalar için [rhuı içerik teslim sunucuları Için IP 'ler](../virtual-machines/workloads/redhat/redhat-rhui.md#the-ips-for-the-rhui-content-delivery-servers) bölümüne bakın. Diğer Linux dağıtımları için sağlayıcı belgelerinize bakın.

Karma Runbook Worker için gereken bağlantı noktaları hakkında daha fazla bilgi için bkz. [karma çalışan rolü bağlantı noktaları](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Özel durumları tanımlarken listelenen adresleri kullanmanızı öneririz. IP adresleri için [Microsoft Azure veri MERKEZI IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)indirebilirsiniz. Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıkları ve IP aralıklarında yaklaşan değişiklikleri yansıtır.

İnternet erişimi olmayan makineleri yapılandırmak için [İnternet erişimi olmadan bilgisayarları bağlama](../azure-monitor/platform/gateway.md) bölümündeki yönergeleri izleyin.

## <a name="update-classifications"></a>Güncelleştirme sınıflandırmaları

Aşağıdaki tablolar, Güncelleştirme Yönetimi ' deki güncelleştirme sınıflandırmalarını her sınıflandırma için bir tanım ile listeler.

### <a name="windows"></a>Windows

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler     | Kritik, güvenlikle ilgili olmayan bir hatayı ele alan belirli bir sorun için güncelleştirme.        |
|Güvenlik güncelleştirmeleri     | Ürüne özgü, güvenlikle ilgili bir sorun için bir güncelleştirme.        |
|Güncelleştirme paketleri     | Kolay dağıtım için bir arada paketlenmiş toplu bir düzeltme kümesi.        |
|Özellik paketleri     | Ürün sürümü dışında dağıtılan yeni ürün özellikleri.        |
|Hizmet paketleri     | Bir uygulamaya uygulanan toplu bir düzeltme kümesi.        |
|Tanım güncelleştirmeleri     | Virüs veya diğer tanım dosyalarına yönelik bir güncelleştirme.        |
|Araçlar     | Bir veya daha fazla görevi tamamlamaya yardımcı olan bir yardımcı program veya özellik.        |
|Güncelleştirmeler     | Şu anda yüklü olan bir uygulama veya dosyaya yönelik bir güncelleştirme.        |

### <a name="linux-2"></a>'Un

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri     | Belirli bir sorun veya ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirmeler.         |
|Diğer güncelleştirmeler     | Doğası gereği önemli olmayan veya güvenlik güncelleştirmeleri olmayan diğer tüm güncelleştirmeler.        |

Linux için Güncelleştirme Yönetimi, bulutta veri zenginleştirme nedeniyle değerlendirme verilerini görüntülerken buluttaki kritik güncelleştirmeler ve güvenlik güncelleştirmeleri arasında ayrım yapabilir. Düzeltme eki uygulama Güncelleştirme Yönetimi makinede bulunan sınıflandırma verilerine bağımlıdır. Diğer dağıtımlardan farklı olarak, CentOS bu bilgileri RTM sürümünde kullanılamaz. Aşağıdaki komut için güvenlik verilerini döndürecek şekilde yapılandırılmış CentOS makineleriniz varsa, Güncelleştirme Yönetimi sınıflandırmalara göre düzeltme eki uygulanabilir.

```bash
sudo yum -q --security check-update
```

Şu anda yerel sınıflandırmanın etkinleştirilmesi için desteklenen bir yöntem yok-CentOS üzerinde veri kullanılabilirliği. Şu anda, bu özelliği kendi kendilerine etkinleştirmiş olabilecek müşterilere yalnızca en iyi çaba desteği sağlanır. 

Red Hat Enterprise sürüm 6 ' da güncelleştirmeleri sınıflandırmak için, yıum-güvenlik eklentisini yüklemeniz gerekir. Red Hat Enterprise Linux 7 ' de, eklenti zaten bir yalnızca bir parçası olduğundan, herhangi bir şey yüklemeye gerek yoktur. Daha fazla bilgi için aşağıdaki Red hat [Bilgi Bankası makalesine](https://access.redhat.com/solutions/10021)bakın.

## <a name="integrate-with-configuration-manager"></a>Configuration Manager ile tümleştirme

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için Microsoft uç nokta Configuration Manager yatırım yapmış müşteriler, yazılım güncelleştirmelerini yönetmeye yardımcı olmak için Configuration Manager kuvvetini ve vadede yararlanır. Configuration Manager, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçasıdır.

Yönetim çözümünü Configuration Manager ile tümleştirmeyi öğrenmek için bkz. [Configuration Manager güncelleştirme yönetimi Ile tümleştirme](oms-solution-updatemgmt-sccmintegration.md).

### <a name="third-party-updates-on-windows"></a>Windows 'da üçüncü taraf güncelleştirmeleri

Güncelleştirme Yönetimi, desteklenen Windows sistemlerini güncelleştirmek için yerel olarak yapılandırılmış güncelleştirme deposuna bağımlıdır. Bu, WSUS veya Windows Update. [System Center Updates Publisher](https://docs.microsoft.com/configmgr/sum/tools/updates-publisher) (Updates Publisher) gibi araçlar, WSUS ile özel güncelleştirmeleri içeri aktarıp yayımlamanıza olanak sağlar. Bu senaryo, Configuration Manager kullanan makineleri üçüncü taraf yazılımlarla güncelleştirme deposu olarak güncelleştirmesine Güncelleştirme Yönetimi olanak tanır. Updates Publisher 'ı yapılandırma hakkında bilgi edinmek için bkz. [Updates Publisher 'ı yüklemek](https://docs.microsoft.com/configmgr/sum/tools/install-updates-publisher).

## <a name="onboard"></a>Güncelleştirme Yönetimi etkinleştir

Sistemleri güncelleştirmeye başlamak için Güncelleştirme Yönetimi çözümünü etkinleştirmeniz gerekir. Çözümü eklemek için önerilen ve desteklenen yöntemler aşağıda verilmiştir:

- [Bir sanal makineden](automation-onboard-solutions-from-vm.md)

- [Birden çok makineye göz atmaya](automation-onboard-solutions-from-browse.md)

- [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md)

- [Bir Azure Otomasyonu runbook 'u ile](automation-onboard-solutions.md)

- [Azure Resource Manager şablonuyla](automation-update-management-deploy-template.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu çözümle ilgili sık sorulan soruları gözden geçirmek için Azure Otomasyonu [SSS](automation-faq.md) 'yi gözden geçirin.
