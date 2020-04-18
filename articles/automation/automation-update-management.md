---
title: Azure'da Yönetim çözümlerini güncelleştir
description: Bu makalede, Windows ve Linux makinelerinizin güncelleştirmelerini yönetmek için Azure Güncelleştirme Yönetimi çözümünüzün nasıl kullanılacağı açıklanmaktadır.
services: automation
ms.subservice: update-management
ms.date: 02/27/2020
ms.topic: conceptual
ms.openlocfilehash: 9bdc8cf97513854cf6a92ffd078febca6302d35c
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617405"
---
# <a name="update-management-solution-in-azure"></a>Azure'da Yönetim çözümlerini güncelleştir

Azure Otomasyonu'ndaki **Güncelleştirme Yönetimi çözümlerini,** Azure'daki Windows ve Linux makineleriniz için işletim sistemi güncelleştirmelerini, şirket içi ortamlarda ve diğer bulut ortamlarında yönetmek için kullanabilirsiniz. Tüm aracı makinelerinde kullanılabilir güncelleştirmelerin durumunu hızlı bir şekilde değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

Aşağıdaki yöntemleri kullanarak sanal makineler (VM'ler) için Güncelleştirme Yönetimi'ni etkinleştirebilirsiniz:

- Azure [Otomasyon uyupunuzdan](automation-onboard-solutions-from-automation-account.md) bir veya daha fazla Azure makinesi ve Azure olmayan makineler için el ile.

- Azure portalındaki Sanal makine sayfasından tek bir Azure VM için. Bu senaryo [Linux](../virtual-machines/linux/tutorial-config-management.md#enable-update-management) ve [Windows](../virtual-machines/windows/tutorial-config-management.md#enable-update-management) VM'leri için kullanılabilir.

- Azure portalındaki Sanal makineler sayfasından seçerek [birden çok Azure VM'si](manage-update-multi.md) için. 

> [!NOTE]
> **Güncelleştirme Yönetimi çözümü,** Bir Log Analytics çalışma alanını Otomasyon hesabınıza bağlamayı gerektirir. Desteklenen bölgelerin kesin listesi için [Azure Çalışma Alanı eşlemelerine](./how-to/region-mappings.md)bakın. Bölge eşlemeleri, Otomasyon hesabınızdan ayrı bir bölgede VM'leri yönetme yeteneğini etkilemez.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

Azure [Kaynak Yöneticisi şablonu,](automation-update-management-deploy-template.md) Aboneliğinizde yeni veya varolan bir Otomasyon hesabına ve Log Analytics çalışma alanına **Güncelleştirme Yönetimi çözümünüzü** dağıtmanıza yardımcı olmak için kullanılabilir.

## <a name="solution-overview"></a>Çözüme genel bakış

Güncelleştirme Yönetimi tarafından yönetilen makineler, değerlendirme gerçekleştirmek ve dağıtımları güncelleştirmek için aşağıdaki yapılandırmaları kullanır:

* Windows veya Linux için Log Analytics aracısı
* Linux için PowerShell İstenen Durum Yapılandırması (DSC)
* Otomasyon Karma Runbook Çalışanı
* Windows makineleri için Microsoft Update veya Windows Server Update Services (WSUS)

Aşağıdaki diyagram, çözümün bir çalışma alanındaki bağlı tüm Windows Server ve Linux makinelerine güvenlik güncelleştirmelerini nasıl değerlendirdiğini ve uyguladığını göstermektedir:

![Yönetim süreç akışını güncelleştir](./media/automation-update-management/update-mgmt-updateworkflow.png)

Güncelleştirme Yönetimi, aynı kiracıdaki birden çok abonelikte makinelerin yerel olarak eklenmesi için kullanılabilir.

Bir paket yayımlandıktan sonra, yama değerlendirme için Linux makineleri için göstermek için 2 ila 3 saat sürer. Windows makineleri için, yamanın yayımlandıktan sonra değerlendirmeye sunması 12 ila 15 saat sürer.

Bir makine güncelleştirme uyumluluğu için bir taramaya çıktıktan sonra, aracı bilgileri toplu olarak Azure Monitor günlüklerine iletir. Windows makinesinde uyumluluk tonu varsayılan olarak her 12 saatte bir çalıştırılır.

Taramaya ek olarak, güncelleştirme uyumluluğu için taramaya, Log Analytics aracısının yeniden başlatılmasından sonraki 15 dakika içinde, güncelleştirme yüklemesinden önce ve güncelleştirme yüklemesinden sonra başlatılır.

Bir Linux makinesi için uyumluluk tonu varsayılan olarak her saat gerçekleştirilir. Log Analytics aracısı yeniden başlatılırsa, 15 dakika içinde bir uyumluluk tayini başlatılır.

Çözüm, makinenin hangi kaynakla eşitlenecek şekilde yapılandırıldığınızı temel alan ne kadar güncel olduğunu bildirir. Windows makinesi WSUS'un Microsoft Update ile en son ne zaman eşitlendiğinden bağlı olarak WSUS'a rapor vermek üzere yapılandırılırsa, sonuçlar Microsoft Update'in gösterdiğinden farklı olabilir. Bu davranış, genel repo yerine yerel bir repo'ya rapor vermek üzere yapılandırılan Linux makineleri için de geçerlidir.

> [!NOTE]
> Hizmete düzgün bir şekilde rapor vermek için Güncelleştirme Yönetimi'nin etkinleştirilmesi için belirli URL'lerin ve bağlantı noktalarının etkinleştirilmesi gerekir. Bu gereksinimler hakkında daha fazla bilgi edinmek [için, Karma Çalışanlar için Ağ planlamasına](automation-hybrid-runbook-worker.md#network-planning)bakın.

Zamanlanmış bir dağıtım oluşturarak güncelleştirmeleri gerektiren makinelere yazılım güncelleştirmeleri dağıtabilir ve yükleyebilirsiniz. *İsteğe Bağlı* olarak sınıflandırılan güncelleştirmeler, Windows makineleri için dağıtım kapsamına dahil değildir. Yalnızca gerekli güncelleştirmeler dağıtım kapsamına dahil edilir.

Zamanlanan dağıtım, geçerli güncelleştirmeleri hangi hedef makinelerin aldığını tanımlar. Bunu, belirli makineleri açıkça belirterek veya belirli bir makine kümesinin günlük aramalarını temel alan bir [bilgisayar grubu](../azure-monitor/platform/computer-groups.md) seçerek (veya belirtilen ölçütlere göre Azure VM'lerini dinamik olarak seçen bir [Azure sorgusunda)](automation-update-management-query-logs.md) yapar. Bu gruplar, yalnızca hangi makinelerin çözümü etkinleştiren yönetim paketlerini alacağını belirlemek için kullanılan [kapsam yapılandırmasından](../azure-monitor/insights/solution-targeting.md)farklıdır.

Ayrıca, güncelleştirmeleri n için onaylayacak ve zaman dilimi ayarlamak için bir zamanlama da belirtirsiniz. Bu döneme bakım penceresi denir. Bakım penceresinin 20 dakikalık bir süresi, gerekli olduğunu ve uygun yeniden başlatma seçeneğini seçtiğinizi varsayarak yeniden başlatmaiçin ayrılmıştır. Yama beklenenden daha uzun sürerse ve bakım penceresinde 20 dakikadan az bir süre varsa, yeniden başlatma gerçekleşmez.

Güncelleştirmeler Azure Automation’daki runbook'lar tarafından yüklenir. Bu runbook'ları görüntülenemeniz gerekir ve yapılandırma gerektirmezler. Bir güncelleştirme dağıtımı oluşturulduğunda, dahil edilen makineler için belirtilen zamanda bir ana güncelleştirme runbook başlatan bir zamanlama oluşturur. Ana runbook gerekli güncelleştirmeleri yüklemek için her aracıüzerinde bir alt runbook başlar.

Güncelleştirme dağıtımında belirtilen tarih ve saatte, hedef makineler dağıtımı paralel olarak yürütür. Yüklemeden önce, güncelleştirmelerin hala gerekli olduğunu doğrulamak için bir tçalışma çalıştırılır. WSUS istemci makineleri için, güncelleştirmeler WSUS'ta onaylanmadıysa, güncelleştirme dağıtımı başarısız olur.

Birden fazla Log Analytics çalışma alanında (çok eşleme olarak da adlandırılır) Güncelleştirme Yönetimi'ne kayıtlı bir makinenin olması desteklenmez.

## <a name="clients"></a>İstemciler

### <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda, güncelleştirme değerlendirmeleri için desteklenen işletim sistemleri listelenir. Yama bir Hybrid Runbook Worker gerektirir. Karma Runbook Worker gereksinimleri hakkında daha fazla bilgi için, bir [Windows Hybrid Runbook Worker](automation-windows-hrw-install.md) ve Linux Hybrid [Runbook Worker](automation-linux-hrw-install.md#installing-a-linux-hybrid-runbook-worker)yüklemek için yükleme kılavuzlarına bakın.

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2019 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2016 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2012 R2(Veri Merkezi/Standart)<br><br>Windows Server 2012 || 
|Windows Server 2008 R2 (RTM ve SP1 Standardı)| Update Management yalnızca bu işletim sistemi için değerlendirme yapmayı destekler, [Karma Runbook Worker](automation-windows-hrw-install.md) Windows Server 2008 R2 için desteklenmez gibi yama desteklenmez. |
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracıları bir güncelleştirme deposuna erişim gerektirir. Sınıflandırma tabanlı yama, CentOS'un RTM sürümlerinde olmayan güvenlik verilerini döndürmeyi gerektirir. `yum` CentOS'ta sınıflandırma tabanlı yama hakkında daha fazla bilgi için [bkz.](automation-view-update-assessments.md#linux-2)          |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracıları bir güncelleştirme deposuna erişim gerektirir.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracıları bir güncelleştirme deposuna erişim gerektirir.        |
|Ubuntu 14,04 LTS, 16,04 LTS ve 18,04 (x86/x64)      |Linux aracıları bir güncelleştirme deposuna erişim gerektirir.         |

> [!NOTE]
> Azure sanal makine ölçek kümeleri Güncelleştirme Yönetimi aracılığıyla yönetilebilir. Güncelleştirme Yönetimi, temel görüntüde değil, örneklerin kendileri üzerinde çalışır. Tüm VM örneklerinin aynı anda güncelleştirilemeyecek şekilde güncelleştirmeleri aşamalı olarak zamanlamanız gerekir.
> [Yerleşik olmayan bir makinenin](automation-tutorial-installed-software.md#onboard-a-non-azure-machine)altındaki adımları izleyerek sanal makine ölçek kümeleri için düğümler ekleyebilirsiniz.

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Aşağıdaki tabloda desteklenmeyen işletim sistemleri listelenir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows istemcisi     | İstemci işletim sistemleri (Windows 7 ve Windows 10 gibi) desteklenmez.        |
|Windows Server 2016 Nano Server     | Desteklenmiyor.       |
|Azure Kubernetes Servis Düğümleri | Desteklenmiyor. [Azure Kubernetes Hizmeti'nde (AKS) Linux düğümlerine güvenlik ve çekirdek güncelleştirmeleri uygula'da](../aks/node-updates-kured.md) açıklanan yama işlemini kullanın|

### <a name="client-requirements"></a>İstemci gereksinimleri

Aşağıdaki bilgiler, Işletim Sistemi'ne özgü istemci gereksinimlerini açıklar. Ek yönergeler için [Bkz. Ağ planlaması.](#ports)

#### <a name="windows"></a>Windows

Windows aracıları bir WSUS sunucusuyla iletişim kurmak üzere yapılandırılmalıdır veya Microsoft Update'e erişim gerektirirler.

Configuration Manager ile Güncelleştirme Yönetimi'ni kullanabilirsiniz. Tümleştirme senaryoları hakkında daha fazla bilgi edinmek için [Bkz. Güncelleştirme Yönetimi ile Configuration Manager'ı Tümleştir.](oms-solution-updatemgmt-sccmintegration.md#configuration) [Log Analytics Windows aracısı](../azure-monitor/platform/agent-windows.md) gereklidir. Bir Azure VM'ye biniyorsanız aracı otomatik olarak yüklenir.

Varsayılan olarak, Azure Marketi'nden dağıtılan Windows VM'leri Windows Update Hizmeti'nden otomatik güncelleştirmeler alacak şekilde ayarlanır. Bu çözümü eklediğinizde veya çalışma alanınıza Windows VM'leri eklediğinizde bu davranış değişmez. Bu çözümü kullanarak güncelleştirmeleri etkin olarak yönetmezseniz, varsayılan davranış (güncelleştirmeleri otomatik olarak uygulamak için) uygulanır.

> [!NOTE]
> Kullanıcı, makine yeniden başlatmasının sistem tarafından değil, yalnızca kullanıcı tarafından gerçekleştirilebileceği için Grup İlkesi'ni değiştirebilir. Update Management kullanıcıdan manuel etkileşim olmadan makineyi yeniden başlatma hakkına sahip değilse yönetilen makineler takılabilir.
>
> Daha fazla bilgi için, [Otomatik Güncelleştirmeler için Grup İlkesi ayarlarını yapılandır' a](https://docs.microsoft.com/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates)bakın.

#### <a name="linux"></a>Linux

Linux için makine bir güncelleştirme deposuna erişim gerektirir. Güncelleştirme deposu özel veya herkese açık olabilir. TlS 1.1 veya TLS 1.2, Güncelleme Yönetimi ile etkileşimde kalmak için gereklidir. Birden fazla Log Analytics çalışma alanına rapor vermek üzere yapılandırılan Linux için bir Log Analytics Aracısı bu çözümle desteklenmez. Makine de Python 2.x yüklü olmalıdır.

Linux için Log Analytics aracısını nasıl yükleyin ve en son sürümü indirmek [için, Linux için Log Analytics aracısına](../azure-monitor/platform/agent-linux.md)bakın. Windows için Günlük Analizi aracısını nasıl yükleyin hakkında bilgi için [windows bilgisayarlarını Azure Monitor'a bağlayın'a](../log-analytics/log-analytics-windows-agent.md)bakın.

Azure Marketi'nde bulunan isteğe bağlı Red Hat Enterprise Linux (RHEL) görüntülerinden oluşturulan VM'ler, Azure'da dağıtılan [Red Hat Update Infrastructure'a (RHUI)](../virtual-machines/workloads/redhat/redhat-rhui.md) erişmek için kaydedilir. Diğer Linux dağıtımı, dağıtımın desteklenen yöntemleri kullanılarak dağıtımın çevrimiçi dosya deposundan güncelleştirilmelidir.

## <a name="permissions"></a>İzinler

Güncelleştirme dağıtımları oluşturmak ve yönetmek için belirli izinlere ihtiyacınız vardır. Bu izinler hakkında bilgi edinmek için [Rol Tabanlı erişim – Yönetimi Güncelleştir'](automation-role-based-access-control.md#update-management-permissions)e bakın.

## <a name="solution-components"></a>Çözüm bileşenleri

Çözüm aşağıdaki kaynaklardan oluşur. Çözümü etkinleştirdiğinizde bu kaynaklar otomatik olarak Otomasyon hesabınıza eklenir. 

### <a name="hybrid-worker-groups"></a>Karma Çalışanı grupları

Bu çözümü etkinleştirdikten sonra, Log Analytics çalışma alanınıza doğrudan bağlanan tüm Windows makineleri, bu çözümde yer alan runbook'ları desteklemek üzere otomatik olarak Karma Runbook İşçisi olarak yapılandırılır.

Çözüm tarafından yönetilen her Windows makinesi, Karma alt gruplar bölmesinde Otomasyon hesabı için sistem hibrit işçi grubu olarak listelenir. Çözümler adlandırma `Hostname FQDN_GUID` kuralını kullanır. Hesabınızda runbook'lar bulunan bu grupları hedef alamazsınız. Denerseniz, deneme başarısız olur. Bu gruplar yalnızca bu yönetim çözümlerini desteklemek için tasarlanmıştır.

Hem çözüm hem de Karma Runbook İşçigrubu üyeliği için aynı hesabı kullanıyorsanız, Otomasyon runbook kitaplarını desteklemek için Windows makinesini Otomasyon hesabınızdaki Karma Runbook İşçi grubuna ekleyebilirsiniz. Bu işlev, Karma Runbook Worker'ın 7.2.12024.0 sürümüne eklendi.

### <a name="management-packs"></a>Yönetim paketleri

Sistem Merkezi Operasyon Yöneticisi yönetim grubunuz [bir Log Analytics çalışma alanına bağlıysa,](../azure-monitor/platform/om-agents.md)Aşağıdaki yönetim paketleri Operations Manager'a yüklenir. Bu yönetim paketleri, çözümü ekledikten sonra doğrudan bağlı Windows makinelerine de yüklenir. Bu yönetim paketlerini yapılandırmanız veya yönetmeniz gerekmez.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* MP Dağıtımını güncelleştirme

> [!NOTE]
> Günlük verilerini toplamak için yönetim grubunda yapılandırılan aracılarla bir Log Analytics çalışma alanına bağlı bir Operations Manager 1807 veya 2019 yönetim grubunuz varsa, parametreyi `IsAutoRegistrationEnabled` geçersiz kılmanız ve **Microsoft.IntelligencePacks.AzureAutomation.HybridAgent.Init** kuralında True olarak ayarlamanız gerekir.

Çözüm yönetim paketlerinin nasıl güncelleştirileştirişleri hakkında daha fazla bilgi için [bkz.](../azure-monitor/platform/om-agents.md)

> [!NOTE]
> Operations Manger agent'ına sahip makinelerin Güncelleme Yönetimi tarafından tam olarak yönetilemesi için aracının Windows için Log Analytics temsilcisine veya Linux için Log Analytics temsilcisine güncellenmesi gerekir. Aracıyı nasıl güncelleştirileştirin, [operasyon yöneticisi aracısını nasıl yükseltin.](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents) Operations Manager'ı kullanan ortamlarda, System Center Operations Manager 2012 R2 UR 14 veya sonraki leri çalıştırıyor olmalısınız.

## <a name="data-collection"></a>Veri toplama

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Destekleniyor | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Çözüm, Windows aracılarından sistem güncelleştirmeleri hakkında bilgi toplar ve gerekli güncelleştirmelerin yüklenmesini başlatır. |
| Linux aracıları |Evet |Çözüm, Linux ajanlarından sistem güncellemeleri hakkında bilgi toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklenmesini başlatır. |
| Operations Manager yönetim grubu |Evet |Çözüm, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar.<br/><br/>Operations Manager aracısından Azure Monitor günlüklerine doğrudan bağlantı gerekmez. Veriler yönetim grubundan Log Analytics çalışma alanına iletilir. |

### <a name="collection-frequency"></a>Toplama sıklığı

Yönetilen her Windows makinesi için günde iki kez tarar. Her 15 dakikada bir, Windows API'si, durumun değişip değişmediğini belirlemek için son güncelleştirme saatini sorgulamak için çağrılır. Durum değiştiyse, uyumluluk tayini başlatıldı.

Yönetilen her Linux makinesi için her saat başı bir tarar.

Panoda yönetilen makinelerden güncelleştirilmiş verileri görüntülemesi 30 dakika ile 6 saat arasında sürebilir.

Azure Monitor'un Güncelleştirme Yönetimi'ni kullanan bir makine için günlüklerinin ortalama veri kullanımı ayda yaklaşık 25 megabayt (MB) olur. Bu değer yalnızca bir yaklaşımdır ve ortamınıza bağlı olarak değişebilir. Tam kullanımınızı izlemek için ortamınızı izlemenizi öneririz. Veri kullanımını analiz etmek için daha fazla bilgi için [bkz.](../azure-monitor/platform/manage-cost-storage.md)

## <a name="network-planning"></a><a name="ports"></a>Ağ planlama

Güncelleştirme Yönetimi için özel olarak aşağıdaki adresler gereklidir. Bu adreslere iletişim 443 bağlantı noktası üzerinden gerçekleşir.

|Azure Genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com    | *.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net | *.blob.core.usgovcloudapi.net|
|*.azure-automation.net | *.azure-automation.us|

Windows makineleri için, Windows Update tarafından gerekli olan uç noktalara trafik izni de vermelisiniz. [HTTP/Proxy](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy)ile ilgili sorunlarda gerekli uç noktaların güncelleştirilmiş bir listesini bulabilirsiniz. Yerel bir [Windows Update sunucunuz](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment)varsa, [WSUS anahtarınızda](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry)belirtilen sunucuya trafik izni de vermelisiniz.

Red Hat Linux makineleri için, gerekli uç noktalar [için RHUI içerik dağıtım sunucuları için IP'lere](../virtual-machines/workloads/redhat/redhat-rhui.md#the-ips-for-the-rhui-content-delivery-servers) bakın. Diğer Linux dağıtımları için sağlayıcı belgelerinize bakın.

Karma Runbook Worker için gereken bağlantı noktaları hakkında daha fazla bilgi için Bkz. [Karma İşçi rol bağlantı noktaları.](automation-hybrid-runbook-worker.md#hybrid-worker-role)

Özel durumları tanımlarken listelenen adresleri kullanmanızı öneririz. IP adresleri için Microsoft [Azure Veri Merkezi IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)indirebilirsiniz. Bu dosya haftalık olarak güncellenir ve şu anda dağıtılan aralıkları ve IP aralıklarında yapılacak değişiklikleri yansıtır.

Internet erişimi olmayan makineleri yapılandırmak için [internet erişimi olmayan Connect bilgisayarlarındaki](../azure-monitor/platform/gateway.md) yönergeleri izleyin.

## <a name="update-classifications"></a>Update classifications

Aşağıdaki tablolar, güncelleştirme yönetimindeki güncelleştirme sınıflandırmalarını her sınıflandırma için bir tanımla birlikte listeleyir.

### <a name="windows"></a>Windows

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler     | Güvenlikle ilgili kritik bir hatayı gideren belirli bir sorun için güncelleştirme.        |
|Güvenlik güncelleştirmeleri     | Ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirme.        |
|Güncelleştirme paketleri     | Kolay dağıtım için birlikte paketlenmiş bir birikimli düzeltme kümesi.        |
|Özellik paketleri     | Ürün sürümü dışında dağıtılan yeni ürün özellikleri.        |
|Hizmet paketleri     | Bir uygulamaya uygulanan birikmeli düzeltme kümesi.        |
|Tanım güncelleştirmeleri     | Virüs veya diğer tanım dosyaları için bir güncelleştirme.        |
|Araçlar     | Bir veya daha fazla görevi tamamlamaya yardımcı olan bir yardımcı program veya özellik.        |
|Güncelleştirmeler     | Şu anda yüklü olan bir uygulama veya dosyaiçin güncelleştirme.        |

### <a name="linux"></a><a name="linux-2"></a>Linux

|Sınıflandırma  |Açıklama  |
|---------|---------|
|Kritik güncelleştirmeler ve güvenlik güncelleştirmeleri     | Belirli bir sorun veya ürüne özgü, güvenlikle ilgili bir sorun için güncelleştirmeler.         |
|Diğer güncelleştirmeler     | Doğada kritik olmayan veya güvenlik güncelleştirmeleri olmayan diğer tüm güncelleştirmeler.        |

Linux için, Update Management buluttaki veri zenginleştirme nedeniyle değerlendirme verilerini görüntülerken buluttaki kritik güncelleştirmeleri ve güvenlik güncelleştirmelerini ayırt edebilir. Düzeltme için Güncelleştirme Yönetimi, makinede bulunan sınıflandırma verilerine dayanır. Diğer dağıtımların aksine, CentOS'ta RTM sürümünde bu bilgiler bulunmaz. Aşağıdaki komut için güvenlik verilerini döndürecek şekilde yapılandırılan CentOS makineleriniz varsa, Güncelleştirme Yönetimi sınıflandırmalara göre yama yapabilir.

```bash
sudo yum -q --security check-update
```

Şu anda CentOS'ta yerel sınıflandırma verilerinin kullanılabilirliğini etkinleştirmek için desteklenen bir yöntem yok. Şu anda, bunu kendi başlarına etkinleştirmiş olabilecek müşterilere yalnızca en iyi çaba desteği sağlanır. 

Red Hat Enterprise sürüm 6'daki güncellemeleri sınıflandırmak için yum-security eklentisini yüklemeniz gerekir. Red Hat Enterprise Linux 7, eklenti zaten yum kendisi bir parçasıdır, bir şey yüklemek için gerek yoktur. Daha fazla bilgi için aşağıdaki Red Hat [bilgi makalesine](https://access.redhat.com/solutions/10021)bakın.

## <a name="integrate-with-configuration-manager"></a>Configuration Manager ile tümleştirme

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için Microsoft Endpoint Configuration Manager'a yatırım yapmış olan müşteriler, yazılım güncelleştirmelerini yönetmelerine yardımcı olmak için Configuration Manager'ın gücüne ve olgunluğuna da güvenir. Configuration Manager, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçasıdır.

Yönetim çözümünü Configuration Manager ile nasıl entegre edileceğini öğrenmek [için](oms-solution-updatemgmt-sccmintegration.md)bkz.

### <a name="third-party-updates-on-windows"></a>Windows'da üçüncü taraf güncelleştirmeleri

Güncelleştirme Yönetimi, desteklenen Windows sistemlerini güncelleştirmek için yerel olarak yapılandırılan güncelleştirme deposuna dayanır. Bu WSUS veya Windows Update'tir. System [Center Updates Publisher](https://docs.microsoft.com/configmgr/sum/tools/updates-publisher) (Updates Publisher) gibi araçlar, WSUS ile özel güncelleştirmeleri içe aktarmanıza ve yayımlamanıza olanak tanır. Bu senaryo, Update Management'ın Configuration Manager'ı üçüncü taraf yazılımlarla güncelleştirme deposu olarak kullanan makineleri güncelleştirmesine olanak tanır. Güncelleştirmeleri Yayımcı'yı nasıl yapılandırılabildiğini öğrenmek için [Güncelleştirmeleri Yayımcı'yı Yükle'ye](https://docs.microsoft.com/configmgr/sum/tools/install-updates-publisher)bakın.

## <a name="enable-update-management"></a><a name="onboard"></a>Güncelleştirme Yönetimi’ni etkinleştirme

Sistemleri güncelleştirmeye başlamak için Güncelleştirme Yönetimi çözümlüne etkinleştirmeniz gerekir. Çözüme yönelik önerilen ve desteklenen yöntemler şunlardır:

- [Sanal bir makineden](automation-onboard-solutions-from-vm.md)
- [Birden fazla makineye göz atma](automation-onboard-solutions-from-browse.md)
- [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md)
- [Azure Otomasyon runbook ile](automation-onboard-solutions.md)
- [Azure Kaynak Yöneticisi şablonuyla](automation-update-management-deploy-template.md)

## <a name="next-steps"></a>Sonraki adımlar

Bu çözümle ilgili sık sorulan soruları incelemek için Azure Otomasyon [SSS'sini](automation-faq.md) gözden geçirin.
