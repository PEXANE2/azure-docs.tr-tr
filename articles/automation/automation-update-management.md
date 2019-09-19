---
title: Azure 'da Güncelleştirme Yönetimi çözümü
description: Bu makale, Windows ve Linux bilgisayarlarınıza yönelik güncelleştirmeleri yönetmek için Azure Güncelleştirme Yönetimi çözümünü nasıl kullanacağınızı anlamanıza yardımcı olmaya yöneliktir.
services: automation
ms.service: automation
ms.subservice: update-management
author: bobbytreed
ms.author: robreed
ms.date: 05/22/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: f4816ea2dc67df717e46df61c955d6d156b14d7e
ms.sourcegitcommit: 2ed6e731ffc614f1691f1578ed26a67de46ed9c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2019
ms.locfileid: "71129677"
---
# <a name="update-management-solution-in-azure"></a>Azure 'da Güncelleştirme Yönetimi çözümü

Azure 'da, şirket içi ortamlarda veya diğer bulut sağlayıcılarında Windows ve Linux bilgisayarlarınıza yönelik işletim sistemi güncelleştirmelerini yönetmek için Azure Otomasyonu 'nda Güncelleştirme Yönetimi çözümünü kullanabilirsiniz. Tüm aracı bilgisayarlardaki kullanılabilir güncelleştirmelerin durumunu hızla değerlendirebilir ve sunucular için gerekli güncelleştirmeleri yükleme işlemini yönetebilirsiniz.

Sanal makineler için Güncelleştirme Yönetimi doğrudan Azure Otomasyonu hesabınızdan etkinleştirebilirsiniz. Otomasyon hesabınızdan sanal makineler için Güncelleştirme Yönetimi etkinleştirmeyi öğrenmek için bkz. [birden çok sanal makine için güncelleştirmeleri yönetme](manage-update-multi.md). Ayrıca, bir sanal makine için Güncelleştirme Yönetimi, Azure portal sanal makine sayfasından da etkinleştirebilirsiniz. Bu senaryo, [Linux](../virtual-machines/linux/tutorial-monitoring.md#enable-update-management) ve [Windows](../virtual-machines/windows/tutorial-monitoring.md#enable-update-management) sanal makineleri için kullanılabilir.

> [!NOTE]
> Güncelleştirme Yönetimi çözümü, Otomasyon hesabınıza bir Log Analytics çalışma alanı bağlamayı gerektirir. Desteklenen bölgelerin kesin listesi için bkz. [Azure çalışma alanı eşlemeleri](./how-to/region-mappings.md). Bölge eşlemeleri, Otomasyon hesabınızdan ayrı bir bölgedeki sanal makineleri yönetme özelliğini etkilemez.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="solution-overview"></a>Çözüme genel bakış

Güncelleştirme Yönetimi tarafından yönetilen bilgisayarlar, değerlendirme ve güncelleştirme dağıtımları gerçekleştirmek için aşağıdaki konfigürasyonları kullanır:

* Windows veya Linux için Microsoft Monitoring Agent (MMA)
* Linux için PowerShell İstenen Durum Yapılandırması (DSC)
* Otomasyon Karma Runbook Çalışanı
* Windows bilgisayarlar için Microsoft Update veya Windows Server Update Services (WSUS)

Aşağıdaki diyagramda, çözümün bir çalışma alanındaki tüm bağlı Windows Server ve Linux bilgisayarlarına nasıl değerlendirir ve güvenlik güncelleştirmeleri uyguladığı, davranışın ve veri akışının kavramsal görünümü gösterilmektedir:

![İşlem akışı Güncelleştirme Yönetimi](./media/automation-update-management/update-mgmt-updateworkflow.png)

Güncelleştirme Yönetimi, makineleri aynı kiracıda birden çok aboneliğe yerel olarak eklemek için kullanılabilir.

Paket yayımlandıktan sonra, düzeltme ekinin değerlendirme için Linux makinelere gösterilmesi 2-3 saat sürer. Windows makineleri için, düzeltme ekinin yayımlandıktan sonra değerlendirme için gösterilmesi 12-15 saat sürer.

Bir bilgisayar güncelleştirme uyumluluğu taramasını tamamladıktan sonra, aracı bilgileri toplu olarak Azure Izleyici günlüklerine iletir. Bir Windows bilgisayarda, uyumluluk taraması varsayılan olarak her 12 saatte bir çalıştırılır.

Tarama zamanlamasının yanı sıra, güncelleştirme uyumluluğuna yönelik tarama, güncelleştirme yüklemesinden önce ve güncelleştirme yüklemesinden sonra, 15 dakika içinde başlatılır.

Bir Linux bilgisayar için, uyumluluk taraması varsayılan olarak saatte bir gerçekleştirilir. MMA Aracısı yeniden başlatılırsa, 15 dakika içinde bir uyumluluk taraması başlatılır.

Çözüm, bilgisayarın güncel olduğunu, hangi kaynağa eşitlemek üzere yapılandırılacağını bildirir. Windows bilgisayarı WSUS 'e rapor verecek şekilde yapılandırıldıysa, WSUS Microsoft Update ile en son ne zaman eşitlendiğine bağlı olarak sonuçlar Microsoft güncelleştirmelerinin gösterdiği gibi farklılık gösterebilir. Bu davranış, bir genel depoya değil yerel depoya raporlamak üzere yapılandırılmış Linux bilgisayarları için aynıdır.

> [!NOTE]
> Hizmete doğru bir şekilde raporlamak için, Güncelleştirme Yönetimi belirli URL 'Lerin ve bağlantı noktalarının etkinleştirilmesini gerektirir. Bu gereksinimler hakkında daha fazla bilgi edinmek için bkz. [karma çalışanlar Için ağ planlaması](automation-hybrid-runbook-worker.md#network-planning).

Zamanlanmış bir dağıtım oluşturarak, yazılım güncelleştirmelerinin gerekli olduğu bilgisayarlara güncelleştirmeleri dağıtabilir ve yükleyebilirsiniz. *Isteğe bağlı* olarak sınıflandırılan güncelleştirmeler Windows bilgisayarları için dağıtım kapsamına dahil edilmez. Dağıtım kapsamında yalnızca gerekli güncelleştirmeler bulunur.

Zamanlanan dağıtım, açıkça bilgisayarları belirterek veya belirli bir bilgisayar kümesinin veya bir [Azure sorgusunun](#azure-machines) günlük aramalarını temel alan bir [bilgisayar grubu](../azure-monitor/platform/computer-groups.md) seçerek hangi hedef bilgisayarların ilgili güncelleştirmeleri alacağını tanımlar. Azure VM 'Leri belirtilen ölçütlere göre dinamik olarak seçer. Bu gruplar, yalnızca çözümü etkinleştiren yönetim paketlerini hangi makinelerin kullandığını belirlemek için kullanılan [kapsam yapılandırmasından](../azure-monitor/insights/solution-targeting.md)farklıdır.

Ayrıca, güncelleştirmelerin yüklenebileceği süreyi onaylamak ve ayarlamak için bir zamanlama da belirtirsiniz. Bu süre, bakım penceresi olarak adlandırılır. Yeniden başlatma gerekirse ve uygun yeniden başlatma seçeneğini belirlediyseniz, bakım penceresinin on dakikası yeniden başlatmalar için ayrılmıştır. Düzeltme eki uygulama beklenenden uzun sürüyorsa ve bakım penceresinde on dakikadan az varsa, yeniden başlatma gerçekleşmeyecektir.

Güncelleştirmeler Azure Automation’daki runbook'lar tarafından yüklenir. Bu runbook 'ları görüntüleyemezsiniz ve Runbook 'lar herhangi bir yapılandırma gerektirmez. Güncelleştirme dağıtımı oluşturulduğunda, güncelleştirme dağıtımı, dahil edilen bilgisayarlar için belirtilen zamanda ana güncelleştirme runbook 'unu Başlatan bir zamanlama oluşturur. Ana runbook, gerekli güncelleştirmeleri yüklemek için her bir aracıda bir alt runbook başlatır.

Güncelleştirme dağıtımında belirtilen tarih ve saatte, hedef bilgisayarlar dağıtımı paralel olarak yürütür. Yüklemeden önce, güncelleştirmelerin hala gerekli olduğunu doğrulamak üzere bir tarama çalıştırılır. WSUS istemci bilgisayarları için güncelleştirmeler WSUS 'ta onaylanmamışsa güncelleştirme dağıtımı başarısız olur.

Birden fazla Log Analytics çalışma alanında (çoklu barındırma) Güncelleştirme Yönetimi için kayıtlı bir makine olması desteklenmez.

## <a name="clients"></a>İstemciler

### <a name="supported-client-types"></a>Desteklenen istemci türleri

Aşağıdaki tabloda desteklenen işletim sistemlerinin bir listesi gösterilmektedir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2019 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2016 (Datacenter/Datacenter Core/Standard)<br><br>Windows Server 2012 R2 (Datacenter/Standard)<br><br>Windows Server 2012<br><br>Windows Server 2008 R2 (RTM ve SP1 standart)|**Güncelleştirme değerlendirmeleri**: Desteklenen<br><br>**Düzeltme eki uygulama**: Karma Runbook Worker gerektirir. [Karma Runbook Worker gereksinimlerine](automation-windows-hrw-install.md#installing-the-windows-hybrid-runbook-worker) bakın|
|CentOS 6 (x86/x64) ve 7 (x64)      | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır. Sınıflandırma tabanlı düzeltme eki uygulama, CentOS 'ın kutudan çıkan güvenlik verilerini döndürmesi için ' yıum ' gerektirir. CentOS üzerinde sınıflandırma tabanlı düzeltme eki uygulama hakkında daha fazla bilgi için bkz. [Linux 'ta sınıflandırmaları güncelleştirme](#linux-2)          |
|Red Hat Enterprise 6 (x86/x64) ve 7 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|SUSE Linux Enterprise Server 11 (x86/x64) ve 12 (x64)     | Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.        |
|Ubuntu 14,04 LTS, 16,04 LTS ve 18,04 (x86/x64)      |Linux aracılarının bir güncelleştirme havuzuna erişimi olmalıdır.         |

> [!NOTE]
> Azure sanal makine ölçek kümeleri, Güncelleştirme Yönetimi ile yönetilebilir. Güncelleştirme Yönetimi, temel görüntü değil örneklerin kendileri üzerinde işe yarar. Tüm sanal makine örneklerini tek seferde güncelleştirmeme gibi, güncelleştirmeleri artımlı bir şekilde zamanlamanız gerekir.
> VMSS düğümleri, [Azure olmayan bir makine ekleme](automation-tutorial-installed-software.md#onboard-a-non-azure-machine)altındaki adımları izleyerek eklenebilir.

### <a name="unsupported-client-types"></a>Desteklenmeyen istemci türleri

Aşağıdaki tabloda, desteklenmeyen bir işletim sistemleri listelenmektedir:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows istemcisi     | İstemci işletim sistemleri (örneğin, Windows 7 ve Windows 10) desteklenmez.        |
|Windows Server 2016 Nano sunucu     | Desteklenmiyor.       |
|Azure Kubernetes hizmet düğümleri | Desteklenmiyor. [Azure Kubernetes Service (AKS) Içindeki Linux düğümlerine güvenlik ve çekirdek güncelleştirmelerini uygulama](../aks/node-updates-kured.md) bölümünde ayrıntılı düzeltme eki uygulama işlemini kullanın|

### <a name="client-requirements"></a>İstemci gereksinimleri

#### <a name="windows"></a>Windows

Windows aracılarının bir WSUS sunucusuyla iletişim kuracak şekilde yapılandırılması veya Microsoft Update erişimi olması gerekir. System Center Configuration Manager Güncelleştirme Yönetimi kullanabilirsiniz. Tümleştirme senaryoları hakkında daha fazla bilgi için bkz. [Güncelleştirme Yönetimi System Center Configuration Manager tümleştirme](oms-solution-updatemgmt-sccmintegration.md#configuration). [Windows Aracısı](../azure-monitor/platform/agent-windows.md) gereklidir. Bir Azure sanal makinesini oluşturuyorsanız, aracı otomatik olarak yüklenir.

> [!NOTE]
> Bir kullanıcının, makine yeniden başlatmaları yalnızca Kullanıcı tarafından değil, sistem tarafından gerçekleştirilebileceği şekilde grup ilkesi değiştirmesi mümkündür. Güncelleştirme Yönetimi yönetilen makineler, kullanıcıdan el ile etkileşim kurmadan makineyi yeniden başlatma haklarına sahip değilse kesilebilir.
>
> Daha fazla bilgi için bkz. [Otomatik Güncelleştirmeler için Grup İlkesi ayarlarını yapılandırma](https://docs.microsoft.com/en-us/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates).

#### <a name="linux"></a>Linux

Linux için, makinenin bir güncelleştirme deposuna erişimi olması gerekir. Güncelleştirme deposu özel veya genel olabilir. Güncelleştirme Yönetimi etkileşimde bulunmak için TLS 1,1 veya TLS 1,2 gereklidir. Linux için birden fazla Log Analytics çalışma alanına raporlamak üzere yapılandırılmış bir Log Analytics Aracısı bu çözümle desteklenmez.  Makinede Python 2. x de yüklü olmalıdır.

Linux için Log Analytics aracısını yükleme ve en son sürümü indirme hakkında daha fazla bilgi için bkz. [Linux için Log Analytics Aracısı](https://github.com/microsoft/oms-agent-for-linux). Windows için Log Analytics aracısının nasıl yükleneceği hakkında bilgi için bkz. [Windows için Microsoft Monitoring Agent](../log-analytics/log-analytics-windows-agent.md).

## <a name="permissions"></a>İzinler

Güncelleştirme dağıtımları oluşturmak ve yönetmek için belirli izinlere sahip olmanız gerekir. Bu izinler hakkında bilgi edinmek için bkz. [rol tabanlı erişim-güncelleştirme yönetimi](automation-role-based-access-control.md#update-management).

## <a name="solution-components"></a>Çözüm bileşenleri

Çözüm aşağıdaki kaynaklardan oluşur. Kaynaklar Otomasyon hesabınıza eklenir. Bunlar doğrudan bağlı aracılardan veya Operations Manager bağlı bir yönetim grubunda yer alırlar.

### <a name="hybrid-worker-groups"></a>Karma Çalışanı grupları

Bu çözümü etkinleştirdikten sonra, Log Analytics çalışma alanınıza doğrudan bağlı olan tüm Windows bilgisayarları, bu çözüme dahil olan runbook 'ları destekleyecek bir karma runbook çalışanı olarak otomatik olarak yapılandırılır.

Çözüm tarafından yönetilen her Windows bilgisayarı, Otomasyon hesabı için bir **sistem karma çalışanı grubu** olarak **karma çalışan grupları** bölmesinde listelenir. Çözümler, *hostname FQDN_GUID*adlandırma kuralını kullanır. Bu grupları hesabınızdaki runbook 'lar ile hedefleyebilirsiniz. Çalışırsanız, bunlar başarısız olur. Bu gruplar yalnızca yönetim çözümünü desteklemek üzere tasarlanmıştır.

Hem çözüm hem de karma runbook çalışanı grup üyeliği için aynı hesabı kullanırsanız Otomasyon Runbook 'larını desteklemek için Otomasyon hesabınızdaki karma Runbook Worker grubuna Windows bilgisayarları ekleyebilirsiniz. Bu işlev, karma Runbook Worker 'ın 7.2.12024.0 sürümüne eklenmiştir.

### <a name="management-packs"></a>Yönetim paketleri

System Center Operations Manager yönetim grubunuz bir Log Analytics çalışma alanına bağlıysa, aşağıdaki yönetim paketleri Operations Manager yüklenir. Bu yönetim paketleri, çözümü ekledikten sonra doğrudan bağlı Windows bilgisayarlarına da yüklenir. Bu yönetim paketlerini yapılandırmanıza veya yönetmenize gerek yoktur.

* Microsoft System Center Advisor Update Assessment Intelligence Pack (Microsoft.IntelligencePacks.UpdateAssessment)
* Microsoft.IntelligencePack.UpdateAssessment.Configuration (Microsoft.IntelligencePack.UpdateAssessment.Configuration)
* MP Dağıtımını güncelleştirme

> [!NOTE]
> Yönetim grubu düzeyinde bir çalışma alanı ile ilişkilendirilecek aracıların yapılandırıldığı bir Operations Manager 1807 veya 2019 yönetim grubunuz varsa, bu **koşulların** gösterilmesi için geçerli geçici çözüm **Microsoft. ıntelligencepacks. AzureAutomation. Hybridadgent. Init** kuralı.

Çözüm Yönetimi paketlerinin nasıl güncelleştirildiği hakkında daha fazla bilgi için bkz. [Azure izleyici günlüklerine bağlanma Operations Manager](../azure-monitor/platform/om-agents.md).

> [!NOTE]
> Operations Manager aracısı olan sistemlerde, Güncelleştirme Yönetimi tarafından tam olarak yönetilebilmeleri için aracının Microsoft Monitoring Agent güncelleştirilmesi gerekir. Aracıyı güncelleştirme hakkında bilgi edinmek için bkz. [Operations Manager aracısını yükseltme](https://docs.microsoft.com/system-center/scom/deploy-upgrade-agents). Operations Manager kullanan ortamlar için, System Center Operations Manager 2012 R2 UR 14 veya sonraki bir sürümünü çalıştırıyor olmanız gerekir.

## <a name="onboard"></a>Güncelleştirme Yönetimi etkinleştir

Düzeltme eki uygulama sistemlerine başlamak için Güncelleştirme Yönetimi çözümünü etkinleştirmeniz gerekir. Güncelleştirme Yönetimi makineleri eklemek için birçok yol vardır. Çözümü eklemek için önerilen ve desteklenen yollar şunlardır:

* [Bir sanal makineden](automation-onboard-solutions-from-vm.md)
* [Birden çok makineye göz atmaya](automation-onboard-solutions-from-browse.md)
* [Otomasyon hesabınızdan](automation-onboard-solutions-from-automation-account.md)
* [Bir Azure Otomasyonu runbook 'u ile](automation-onboard-solutions.md)

### <a name="confirm-that-non-azure-machines-are-onboarded"></a>Azure olmayan makinelerin eklendi olduğunu onaylayın

Doğrudan bağlı makinelerin Azure Izleyici günlükleriyle iletişim kurduğunu doğrulamak için, birkaç dakika sonra aşağıdaki günlük aramalarından birini çalıştırabilirsiniz.

#### <a name="linux"></a>Linux

```loganalytics
Heartbeat
| where OSType == "Linux" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

#### <a name="windows"></a>Windows

```loganalytics
Heartbeat
| where OSType == "Windows" | summarize arg_max(TimeGenerated, *) by SourceComputerId | top 500000 by Computer asc | render table
```

Bir Windows bilgisayarında, Azure Izleyici günlükleriyle aracı bağlantısını doğrulamak için aşağıdaki bilgileri gözden geçirebilirsiniz:

1. Denetim Masası 'nda **Microsoft Monitoring Agent**açın. **Azure Log Analytics** sekmesinde, aracı aşağıdaki iletiyi görüntüler: **Microsoft Monitoring Agent Log Analytics başarıyla bağlandı**.
2. Windows olay günlüğü 'Nü açın. **Uygulama ve hizmetler Logs\Operations Manager** ' a gidin ve kaynak **hizmeti bağlayıcısından**olay KIMLIĞI 3000 ve olay kimliği 5002 ' ni arayın. Bu olaylar, bilgisayarın Log Analytics çalışma alanına kaydoldığını ve yapılandırma aldığını gösterir.

Aracı Azure Izleyici günlükleriyle iletişim kuramıyorsa ve aracı bir güvenlik duvarı veya ara sunucu üzerinden internet ile iletişim kurmak üzere yapılandırılmışsa, güvenlik duvarının veya ara sunucunun düzgün yapılandırıldığını onaylayın. Güvenlik duvarının veya proxy sunucusunun düzgün bir şekilde yapılandırıldığını nasıl doğrulayacağınızı öğrenmek için bkz. [Windows Aracısı Için ağ yapılandırması](../azure-monitor/platform/agent-windows.md) veya [Linux Aracısı için ağ yapılandırması](../log-analytics/log-analytics-agent-linux.md).

> [!NOTE]
> Linux sistemleriniz bir ara sunucu veya Log Analytics ağ geçidiyle iletişim kuracak şekilde yapılandırıldıysa ve bu çözümü eklediğinizde, aşağıdaki komutları kullanarak dosya üzerinde omıuser grubuna Okuma izni vermek için *proxy. conf* izinlerini güncelleştirin:
>
> `sudo chown omsagent:omiusers /etc/opt/microsoft/omsagent/proxy.conf`
> `sudo chmod 644 /etc/opt/microsoft/omsagent/proxy.conf`

Yeni eklenen Linux aracıları, bir değerlendirme gerçekleştirildikten sonra **güncelleştirilmiş** durumunu gösterir. Bu işlem 6 saat kadar sürebilir.

Operations Manager yönetim grubunun Azure Izleyici günlükleriyle iletişim kurduğunu onaylamak için bkz. [Azure izleyici günlükleriyle Operations Manager tümleştirmeyi doğrulama](../azure-monitor/platform/om-agents.md#validate-operations-manager-integration-with-azure-monitor).

## <a name="data-collection"></a>Veri toplama

### <a name="supported-agents"></a>Desteklenen aracılar

Aşağıdaki tabloda bu çözüm tarafından desteklenen bağlı kaynaklar açıklanmaktadır:

| Bağlı kaynak | Desteklenen | Açıklama |
| --- | --- | --- |
| Windows aracıları |Evet |Çözüm, Windows aracılarından sistem güncelleştirmeleri hakkında bilgi toplar ve gerekli güncelleştirmelerin yüklemesini başlatır. |
| Linux aracıları |Evet |Çözüm, Linux aracılarından sistem güncelleştirmeleri hakkında bilgi toplar ve desteklenen dağıtımlarda gerekli güncelleştirmelerin yüklemesini başlatır. |
| Operations Manager yönetim grubu |Evet |Çözüm, bağlı bir yönetim grubundaki aracılardan sistem güncelleştirmeleri hakkında bilgi toplar.<br/>Operations Manager aracısından Azure Izleyici günlüklerine doğrudan bağlantı gerekli değildir. Verileri yönetim grubundan Log Analytics çalışma alanına iletilir. |

### <a name="collection-frequency"></a>Toplama sıklığı

Yönetilen her Windows bilgisayarı için günde iki kez tarama gerçekleştirilir. Her 15 dakikada bir Windows API 'si, durumun değişip değişmediğini belirlemekte son güncelleştirme zamanını sorgulamak için çağırılır. Durum değiştiyse, bir uyumluluk taraması başlatılır.

Her yönetilen Linux bilgisayar için saatte bir tarama gerçekleştirilir.

Panonun yönetilen bilgisayarlardan güncelleştirilmiş verileri görüntülemesi 30 dakika ile 6 saat arasında sürebilir.

Güncelleştirme Yönetimi kullanan bir makineye yönelik ortalama Azure Izleyici günlük veri kullanımı yaklaşık olarak aylık 25 MB 'tır. Bu değer yalnızca bir yaklaşık değerdir ve ortamınıza bağlı olarak değişebilir. Sahip olduğunuz tam kullanımı görmek için ortamınızı izlemeniz önerilir.

## <a name="viewing-update-assessments"></a>Güncelleştirme değerlendirmelerini görüntüle

Otomasyon hesabınızda, makinelerinizin durumunu görüntülemek için **güncelleştirme yönetimi** ' yi seçin.

Bu görünüm, makineleriniz, eksik güncelleştirmeler, güncelleştirme dağıtımları ve zamanlanmış güncelleştirme dağıtımları hakkında bilgi sağlar. **Uyumluluk sütununda**, makinenin en son değerlendirildiğini görebilirsiniz. **GÜNCELLEŞTIRME ARACıSı hazırlığı** sütununda, güncelleştirme aracısının sistem durumunu görebilirsiniz. Sorun varsa, sorunu gidermek için hangi adımların yapılacağını öğrenmenize yardımcı olabilecek sorun giderme belgelerine gitmek için bağlantıyı seçin.

Makine, güncelleştirme veya dağıtım hakkında bilgi döndüren bir günlük araması çalıştırmak için listeden öğeyi seçin. **Günlük araması** bölmesi, seçilen öğe için bir sorgu ile açılır:

![Güncelleştirme Yönetimi varsayılan görünüm](media/automation-update-management/update-management-view.png)

## <a name="install-updates"></a>Güncelleştirmeleri yükle

Çalışma alanınızdaki tüm Linux ve Windows bilgisayarları için güncelleştirmeler değerlendirildikten sonra, bir *güncelleştirme dağıtımı*oluşturarak gerekli güncelleştirmeleri yükleyebilirsiniz. Bir güncelleştirme dağıtımı oluşturmak için, Otomasyon hesabına yazma erişiminizin olması ve dağıtımda hedeflenen tüm Azure VM 'lerine yazma erişiminizin olması gerekir. Güncelleştirme dağıtımı, bir veya daha fazla bilgisayar için gerekli güncelleştirmelerin zamanlanmış bir yüklemesidir. Dağıtımın ve bir bilgisayarın veya bilgisayar grubunun bir dağıtım kapsamına dahil edilecek tarih ve saati belirtirsiniz. Bilgisayar grupları hakkında daha fazla bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../azure-monitor/platform/computer-groups.md).

Güncelleştirme dağıtımınıza bilgisayar gruplarını eklediğinizde, zamanlama oluşturma sırasında grup üyeliği yalnızca bir kez değerlendirilir. Bir gruptaki sonraki değişiklikler yansıtılmaz. Bu sorunu çözmek için [dinamik grupları](#using-dynamic-groups)kullanın, bu gruplar dağıtım zamanında çözümlenir ve Azure VM 'leri için bir sorgu ya da Azure dışı VM 'ler için kaydedilmiş bir arama tarafından tanımlanır.

> [!NOTE]
> Azure Marketi 'nden dağıtılan Windows sanal makineleri varsayılan olarak Windows Update hizmetinden otomatik güncelleştirmeleri alacak şekilde ayarlanır. Bu çözümü eklediğinizde veya çalışma alanınıza Windows sanal makineleri eklediğinizde bu davranış değişmez. Bu çözümü kullanarak güncelleştirmeleri etkin bir şekilde yönetmezseniz varsayılan davranış (güncelleştirmeleri otomatik olarak uygulamak için) geçerlidir.

Güncelleştirmelerin Ubuntu 'da bakım penceresi dışında uygulanmasını önlemek için, katılımsız yükseltme paketini otomatik güncelleştirmeleri devre dışı bırakacak şekilde yeniden yapılandırın. Paketin nasıl yapılandırılacağı hakkında daha fazla bilgi için [Ubuntu sunucu kılavuzunda otomatik güncelleştirmeler konusuna](https://help.ubuntu.com/lts/serverguide/automatic-updates.html)bakın.

Azure Marketi 'nde bulunan isteğe bağlı Red Hat Enterprise Linux (RHEL) görüntülerinden oluşturulan sanal makineler, Azure 'da dağıtılan [Red Hat güncelleştirme altyapısına (rhuı)](../virtual-machines/virtual-machines-linux-update-infrastructure-redhat.md) erişmek için kaydedilir. Diğer tüm Linux dağıtımı, dağıtımın desteklenen yöntemlerini izleyerek dağıtımın çevrimiçi dosya deposundan güncelleştirilmeleri gerekir.

Yeni bir güncelleştirme dağıtımı oluşturmak için **güncelleştirme dağıtımı zamanla**' yı seçin. **Yeni güncelleştirme dağıtım** sayfası açılır. Aşağıdaki tabloda açıklanan özellikler için değerler girin ve ardından **Oluştur**' a tıklayın:

| Özellik | Description |
| --- | --- |
| Adı |Güncelleştirme dağıtımını tanımlamak için benzersiz bir ad. |
|İşletim sistemi| Linux veya Windows|
| Güncelleştirilecek gruplar |Azure makinelerinde, dağıtımınıza dahil etmek için dinamik bir Azure VM grubu oluşturmaya yönelik bir abonelik, kaynak grubu, konum ve etiket birleşimine göre bir sorgu tanımlayın. </br></br>Azure dışı makineler için, dağıtıma dahil edilecek bir Azure dışı makine grubu seçmek üzere mevcut kayıtlı bir aramayı seçin. </br></br>Daha fazla bilgi için bkz. [Dinamik Gruplar](automation-update-management.md#using-dynamic-groups)|
| Güncelleştirilecek makineler |Açılan listeden kaydedilmiş bir arama, Içeri aktarılan grup veya Select Machine seçin ve tek tek makineler ' i seçin. **Makineler**'i seçerseniz makinenin hazır olma durumu **GÜNCELLEŞTİRME ARACISI HAZIRLIĞI** sütununda gösterilir.</br> Azure Izleyici günlüklerinde bilgisayar grupları oluşturmaya yönelik farklı yöntemler hakkında bilgi edinmek için bkz. [Azure izleyici günlüklerinde bilgisayar grupları](../azure-monitor/platform/computer-groups.md) |
|Güncelleştirme sınıflandırmaları|İhtiyaç duyduğunuz tüm güncelleştirme sınıflandırmalarını seçin|
|Güncelleştirmeleri dahil et/Dışla|Bu, **dahil etme/hariç tutma** sayfasını açar. Dahil edilecek veya dışlanacak güncelleştirmeler ayrı sekmelerdedir. İçerme hakkında daha fazla bilgi için bkz. [içerme davranışı](automation-update-management.md#inclusion-behavior) |
|Zamanlama ayarları|Başlatılacak saati seçin ve yinelenme için bir kez veya yineleme seçin|
| Ön betikler + betikleri sonrası|Dağıtımdan önce ve sonra çalıştırılacak betikleri seçin|
| Bakım penceresi |Güncelleştirmeler için ayarlanan dakika sayısı. Değer 30 dakikadan kısa olamaz ve 6 saatten fazla olamaz |
| Yeniden başlatma denetimi| Yeniden başlatmaları nasıl ele alınacağını belirler. Kullanılabilen seçenekler:</br>Gerekirse yeniden başlat (Varsayılan)</br>Her zaman yeniden başlat</br>Hiçbir zaman yeniden başlatma</br>Yalnızca yeniden başlatma - güncelleştirmeleri yüklemez|

Güncelleştirme dağıtımları da programlı bir şekilde oluşturulabilir. REST API güncelleştirme dağıtımı oluşturmayı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması-oluştur](/rest/api/automation/softwareupdateconfigurations/create). Ayrıca, haftalık güncelleştirme dağıtımı oluşturmak için kullanılabilecek bir örnek runbook vardır. Bu runbook hakkında daha fazla bilgi edinmek için bkz. [bir kaynak grubundaki bir veya daha fazla VM için haftalık güncelleştirme dağıtımı oluşturma](https://gallery.technet.microsoft.com/scriptcenter/Create-a-weekly-update-2ad359a1).

### <a name="maintenance-windows"></a>Bakım pencereleri

Bakım pencereleri, güncelleştirmelerin yüklenmesi için izin verilen süre miktarını denetler. Bir bakım penceresi belirtirken aşağıdaki ayrıntıları göz önünde bulundurun.

* Bakım pencereleri, kaç güncelleştirme yüklenedenebir şekilde kontrol edilir.
* Bakım penceresinin sonuna yaklaşıyorsa Güncelleştirme Yönetimi yeni güncelleştirmeleri yüklemeyi durdurmaz.
* Bakım penceresi aşıldığında Güncelleştirme Yönetimi devam eden güncelleştirmeleri sonlandıramaz.
* Bakım penceresi Windows üzerinde aşılırsa, çoğu zaman bir hizmet paketi güncelleştirmesinin yüklenmesi uzun sürmesi nedeniyle oluşur.

### <a name="multi-tenant"></a>Çapraz kiracı güncelleştirme dağıtımları

Başka bir Azure kiracı raporlarında, düzeltme eki uygulamanız gerektiğini Güncelleştirme Yönetimi makineniz varsa, bunları zamanlanmak için aşağıdaki geçici çözümü kullanmanız gerekir. [Yeni-azurermautomationschedule](/powershell/module/azurerm.automation/new-azurermautomationschedule) cmdlet 'ini bir zamanlama oluşturmak için anahtarıyla `-ForUpdate` kullanabilir [, New-azurermautomationsoftwareupdateconfiguration](/powershell/module/azurerm.automation/new-azurermautomationsoftwareupdateconfiguration
) cmdlet 'ini kullanabilir ve makineleri diğerine geçirebilirsiniz `-NonAzureComputer` parametreye kiracı. Aşağıdaki örnek bunun nasıl yapılacağı hakkında bir örnek göstermektedir:

```azurepowershell-interactive
$nonAzurecomputers = @("server-01", "server-02")

$startTime = ([DateTime]::Now).AddMinutes(10)

$sched = New-AzureRmAutomationSchedule -ResourceGroupName mygroup -AutomationAccountName myaccount -Name myupdateconfig -Description test-OneTime -OneTime -StartTime $startTime -ForUpdate

New-AzureRmAutomationSoftwareUpdateConfiguration  -ResourceGroupName $rg -AutomationAccountName <automationAccountName> -Schedule $sched -Windows -NonAzureComputer $nonAzurecomputers -Duration (New-TimeSpan -Hours 2) -IncludedUpdateClassification Security,UpdateRollup -ExcludedKbNumber KB01,KB02 -IncludedKbNumber KB100
```

## <a name="view-missing-updates"></a>Eksik güncelleştirmeleri görüntüle

Makinelerinizde bulunmayan güncelleştirmelerin listesini görüntülemek için **eksik güncelleştirmeler** ' i seçin. Her güncelleştirme listelenir ve seçilebilir. Güncelleştirme, işletim sistemi ve daha fazla bilgi için bir bağlantı gerektiren makine sayısı hakkında bilgi gösterilir. **Günlük araması** bölmesinde güncelleştirmeler hakkında daha fazla ayrıntı görüntülenir.

## <a name="view-update-deployments"></a>Güncelleştirme dağıtımlarını görüntüle

Mevcut güncelleştirme dağıtımlarının listesini görüntülemek için **dağıtımları Güncelleştir** sekmesini seçin. Bu güncelleştirme dağıtımı için **güncelleştirme dağıtımı çalıştırma** bölmesini açmak için tablodaki güncelleştirme dağıtımlarından herhangi birini seçin. İş günlükleri en fazla 30 gün boyunca depolanır.

![Güncelleştirme dağıtım sonuçlarına genel bakış](./media/automation-update-management/update-deployment-run.png)

REST API bir güncelleştirme dağıtımını görüntülemek için bkz. [yazılım güncelleştirme yapılandırması çalıştırmaları](/rest/api/automation/softwareupdateconfigurationruns).

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

Linux için Güncelleştirme Yönetimi, bulutta veri zenginleştirmesi nedeniyle değerlendirme verilerini görüntülerken buluttaki kritik ve güvenlik güncelleştirmeleri arasında ayrım yapabilir. Düzeltme eki uygulama Güncelleştirme Yönetimi makinede bulunan sınıflandırma verilerine bağımlıdır. Diğer dağıtımlardan farklı olarak, CentOS bu bilgileri kutudan çıkar. Aşağıdaki komutun güvenlik verilerini döndürmek için bir şekilde yapılandırılmış CentOS makineleriniz varsa Güncelleştirme Yönetimi sınıflandırmalara göre düzeltme eki uygulanabilir.

```bash
sudo yum -q --security check-update
```

Şu anda yerel sınıflandırmayı etkinleştirmek için yöntem desteklenmekte olan yöntem yok-CentOS üzerinde veri kullanılabilirliği. Şu anda, bu özelliği kendi kendilerine etkinleştirmiş olabilecek müşterilere yalnızca en iyi çaba desteği sağlanır.

## <a name="firstparty-predownload"></a>Gelişmiş ayarlar

Güncelleştirme Yönetimi, Windows güncelleştirmelerini indirmek ve yüklemek için Windows Update bağımlıdır. Sonuç olarak, Windows Update tarafından kullanılan birçok ayarı sunuyoruz. Windows dışı güncelleştirmeleri etkinleştirmek için ayarları kullanıyorsanız, Güncelleştirme Yönetimi bu güncelleştirmeleri de yönetecektir. Güncelleştirme dağıtımı gerçekleşmeden önce güncelleştirme indirmeyi etkinleştirmek istiyorsanız, güncelleştirme dağıtımları daha hızlı gidip bakım penceresini aşmaya daha az olabilir.

### <a name="pre-download-updates"></a>İndirme öncesi güncelleştirmeler

Grup ilkesi güncelleştirmeleri otomatik olarak karşıdan yüklemeyi yapılandırmak için [otomatik güncelleştirmeleri Yapılandır ayarını](/windows-server/administration/windows-server-update-services/deploy/4-configure-group-policy-settings-for-automatic-updates##configure-automatic-updates) **3**olarak ayarlayabilirsiniz. Bu, arka planda gerekli olan güncelleştirmeleri indirir, ancak yüklemez. Bu, zamanlamaların denetiminde Güncelleştirme Yönetimi korur ancak güncelleştirmelerin Güncelleştirme Yönetimi bakım penceresi dışında indirilmesine izin verir. Bu, **bakım penceresinin** güncelleştirme yönetimi hatalarını aşılmasına engel olabilir.

Bunu PowerShell ile de ayarlayabilirsiniz, güncelleştirmeleri otomatik olarak indirmek istediğiniz bir sistemde aşağıdaki PowerShell 'i çalıştırın.

```powershell
$WUSettings = (New-Object -com "Microsoft.Update.AutoUpdate").Settings
$WUSettings.NotificationLevel = 3
$WUSettings.Save()
```

### <a name="disable-automatic-installation"></a>Otomatik yüklemeyi devre dışı bırak

Azure VM 'Leri varsayılan olarak etkinleştirilen güncelleştirmelerin otomatik olarak yüklenmesine sahiptir. Bu, güncelleştirmelerin Güncelleştirme Yönetimi tarafından yüklenmek üzere zamanlanmadan önce yüklenememesine neden olabilir. `NoAutoUpdate` Kayıt defteri anahtarını olarak `1`ayarlayarak bu davranışı devre dışı bırakabilirsiniz. Aşağıdaki PowerShell kod parçacığı bunu yapmanın bir yolunu gösterir.

```powershell
$AutoUpdatePath = "HKLM:SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU"
Set-ItemProperty -Path $AutoUpdatePath -Name NoAutoUpdate -Value 1
```

### <a name="enable-updates-for-other-microsoft-products"></a>Diğer Microsoft ürünleri için güncelleştirmeleri etkinleştir

Varsayılan olarak Windows Update yalnızca Windows için güncelleştirmeler sağlar. **Windows 'u güncelleştirdiğimde diğer Microsoft ürünleri için güncelleştirmeleri Izin ver**' i etkinleştirirseniz, SQL Server veya diğer birinci taraf yazılımlar için güvenlik düzeltme ekleri de dahil olmak üzere diğer ürünlerle ilgili güncelleştirmeler sunulur. Bu seçenek grup ilkesi tarafından yapılandırılamaz. Diğer birinci taraf düzeltme eklerini etkinleştirmek istediğiniz sistemlerde aşağıdaki PowerShell 'i çalıştırın ve Güncelleştirme Yönetimi Bu ayarı kabul eder.

```powershell
$ServiceManager = (New-Object -com "Microsoft.Update.ServiceManager")
$ServiceManager.Services
$ServiceID = "7971f918-a847-4430-9279-4a52d1efe18d"
$ServiceManager.AddService2($ServiceId,7,"")
```

## <a name="third-party"></a>Windows 'da üçüncü taraf düzeltme ekleri

Güncelleştirme Yönetimi, desteklenen Windows sistemlerine yama yapmak için yerel olarak yapılandırılmış güncelleştirme deposuna bağımlıdır. Bu, WSUS veya Windows Update. [SystemCenterUpdatesPublisher](/sccm/sum/tools/updates-publisher
) (Updates Publisher) gibi araçlar, özel güncelleştirmeleri WSUS 'ta yayımlamanıza izin verir. Bu senaryo Güncelleştirme Yönetimi, üçüncü taraf yazılımlarla güncelleştirme deposu olarak System Center Configuration Manager kullanan makinelere yaması sağlar. Updates Publisher 'ı yapılandırma hakkında bilgi edinmek için bkz. [Updates Publisher 'ı yüklemek](/sccm/sum/tools/install-updates-publisher).

## <a name="ports"></a>Ağ planlama

Aşağıdaki adresler Güncelleştirme Yönetimi için özel olarak gereklidir. 443 numaralı bağlantı noktası üzerinden bu adreslerle iletişim oluşur.

|Azure genel  |Azure Kamu  |
|---------|---------|
|*.ods.opinsights.azure.com     |*.ods.opinsights.azure.us         |
|*.oms.opinsights.azure.com     | *.oms.opinsights.azure.us        |
|*.blob.core.windows.net|*.blob.core.usgovcloudapi.net|
|*.azure-automation.net|*. azure-automation.us|

Windows makinelerinde, Windows Update için gereken tüm uç noktalara giden trafiğe de izin vermeniz gerekir.  [Http/proxy ile Ilgili sorunlarda](/windows/deployment/update/windows-update-troubleshooting#issues-related-to-httpproxy), gerekli uç noktaların güncelleştirilmiş bir listesini bulabilirsiniz. Yerel bir [Windows Update sunucunuz](/windows-server/administration/windows-server-update-services/plan/plan-your-wsus-deployment)varsa, [WSUS anahtarınıza](/windows/deployment/update/waas-wu-settings#configuring-automatic-updates-by-editing-the-registry)belirtilen sunucuya giden trafiğe de izin vermeniz gerekir.

Red Hat Linux makinelerinde, gerekli uç noktalar için lütfen [rhuı içerik teslimi sunucularının IP](../virtual-machines/linux/update-infrastructure-redhat.md#the-ips-for-the-rhui-content-delivery-servers) 'lerine bakın. Diğer Linux dağıtımları için sağlayıcı belgelerine bakın.

Karma Runbook Worker için gereken bağlantı noktaları hakkında daha fazla bilgi için bkz. [karma çalışan rolü bağlantı noktaları](automation-hybrid-runbook-worker.md#hybrid-worker-role).

Özel durumları tanımlarken listelenen adreslerin kullanılması önerilir. IP adresleri için [Microsoft Azure veri MERKEZI IP aralıklarını](https://www.microsoft.com/download/details.aspx?id=41653)indirebilirsiniz. Bu dosya haftalık olarak güncelleştirilir ve şu anda dağıtılmış aralıkları ve IP aralıklarında yapılan yaklaşan değişiklikleri yansıtır.

İnternet erişimine sahip olmayan makineleri yapılandırmak için [İnternet erişimi olmadan bilgisayarları bağlama](../azure-monitor/platform/gateway.md) bölümündeki yönergeleri izleyin.

## <a name="search-logs"></a>Günlük ara

Azure portal belirtilen ayrıntılara ek olarak günlüklere göre aramalar yapabilirsiniz. Çözüm sayfalarında **Log Analytics**' yi seçin. **Günlük araması** bölmesi açılır.

Ayrıca, ziyaret ederek sorguları özelleştirmeyi veya farklı istemcilerden ve daha fazlasını kullanmayı da öğrenebilirsiniz:  [API belgelerini](
https://dev.loganalytics.io/)Log Analytics arama.

### <a name="sample-queries"></a>Örnek sorgular

Aşağıdaki bölümler, bu çözüm tarafından toplanan güncelleştirme kayıtları için örnek günlük sorguları sağlar:

#### <a name="single-azure-vm-assessment-queries-windows"></a>Tek Azure VM değerlendirme sorguları (Windows)

VMUUID değerini, sorguladığınız sanal makinenin VM GUID 'SI ile değiştirin. Azure Izleyici günlüklerinde aşağıdaki sorguyu çalıştırarak kullanılması gereken VMUUID 'yi bulabilirsiniz:`Update | where Computer == "<machine name>" | summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler Özeti

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"b08d5afa-1471-4b52-bd95-a44fea6e4ca8"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Eksik güncelleştirmeler listesi

```loganalytics
Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and VMUUID=~"8bf1ccc6-b6d3-4a0b-a643-23f346dfdf82"
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

#### <a name="single-azure-vm-assessment-queries-linux"></a>Tek Azure VM değerlendirmesi sorguları (Linux)

Bazı Linux 'lar için, Azure Resource Manager ve Azure Izleyici günlüklerinde depolanan VMUUID değeri ile bir [bitiçlik](https://en.wikipedia.org/wiki/Endianness) uyumsuzluğu vardır. Aşağıdaki sorgu, her iki durumda da bir eşleşme olup olmadığını denetler. Sonuçları doğru bir şekilde döndürmek için VMUUID değerlerini, GUID 'nin büyük endian ve little-endian biçimiyle değiştirin. Azure Izleyici günlüklerinde aşağıdaki sorguyu çalıştırarak kullanılması gereken VMUUID 'yi bulabilirsiniz:`Update | where Computer == "<machine name>"
| summarize by Computer, VMUUID`

##### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler Özeti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="missing-updates-list"></a>Eksik güncelleştirmeler listesi

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and (VMUUID=~"625686a0-6d08-4810-aae9-a089e68d4911" or VMUUID=~"a0865662-086d-1048-aae9-a089e68d4911")
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl

```

#### <a name="multi-vm-assessment-queries"></a>Çoklu VM değerlendirmesi sorguları

##### <a name="computers-summary"></a>Bilgisayar Özeti

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Approved, Optional, Classification) by SourceComputerId, UpdateID
    | distinct SourceComputerId, Classification, UpdateState, Approved, Optional
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed" and (Optional==false or Classification has "Critical" or Classification has "Security") and Approved!=false, iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity
| union (Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux"
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by SourceComputerId, Product, ProductArch
    | distinct SourceComputerId, Classification, UpdateState
    | summarize WorstMissingUpdateSeverity=max(iff(UpdateState=~"Needed", iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1)), 0)) by SourceComputerId
)
on SourceComputerId
| extend WorstMissingUpdateSeverity=coalesce(WorstMissingUpdateSeverity, -1)
| summarize computersBySeverity=count() by WorstMissingUpdateSeverity)
| summarize assessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity>-1), notAssessedComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==-1), computersNeedCriticalUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==4), computersNeedSecurityUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==2), computersNeedOtherUpdatesCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==1), upToDateComputersCount=sumif(computersBySeverity, WorstMissingUpdateSeverity==0)
| summarize assessedComputersCount=sum(assessedComputersCount), computersNeedCriticalUpdatesCount=sum(computersNeedCriticalUpdatesCount),  computersNeedSecurityUpdatesCount=sum(computersNeedSecurityUpdatesCount), computersNeedOtherUpdatesCount=sum(computersNeedOtherUpdatesCount), upToDateComputersCount=sum(upToDateComputersCount), notAssessedComputersCount=sum(notAssessedComputersCount)
| extend allComputersCount=assessedComputersCount+notAssessedComputersCount


```

##### <a name="missing-updates-summary"></a>Eksik güncelleştirmeler Özeti

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification) by Computer, SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| summarize by Product, ProductArch, Classification
| union (Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| summarize by UpdateID, Classification )
| summarize allUpdatesCount=count(), criticalUpdatesCount=countif(Classification has "Critical"), securityUpdatesCount=countif(Classification has "Security"), otherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security")
```

##### <a name="computers-list"></a>Bilgisayar listesi

```loganalytics
Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Product, Computer, ComputerEnvironment) by SourceComputerId, Product, ProductArch
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed"), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed"), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed"), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=1, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2)
| union(Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions, Computer, ResourceId, ComputerEnvironment, VMUUID) by SourceComputerId
| where Solutions has "updates"
| extend vmuuId=VMUUID, azureResourceId=ResourceId, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), scopedToUpdatesSolution=true, lastUpdateAgentSeenTime=""
| join kind=leftouter
(
    Update
    | where TimeGenerated>ago(14h) and OSType!="Linux" and SourceComputerId in ((Heartbeat
    | where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
    | summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
    | where Solutions has "updates"
    | distinct SourceComputerId))
    | summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, Optional, Approved, Computer, ComputerEnvironment) by Computer, SourceComputerId, UpdateID
    | summarize Computer=any(Computer), ComputerEnvironment=any(ComputerEnvironment), missingCriticalUpdatesCount=countif(Classification has "Critical" and UpdateState=~"Needed" and Approved!=false), missingSecurityUpdatesCount=countif(Classification has "Security" and UpdateState=~"Needed" and Approved!=false), missingOtherUpdatesCount=countif(Classification !has "Critical" and Classification !has "Security" and UpdateState=~"Needed" and Optional==false and Approved!=false), lastAssessedTime=max(TimeGenerated), lastUpdateAgentSeenTime="" by SourceComputerId
    | extend compliance=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0, 2, 1)
    | extend ComplianceOrder=iff(missingCriticalUpdatesCount > 0 or missingSecurityUpdatesCount > 0 or missingOtherUpdatesCount > 0, 1, 3)
)
on SourceComputerId
| project id=SourceComputerId, displayName=Computer, sourceComputerId=SourceComputerId, scopedToUpdatesSolution=true, missingCriticalUpdatesCount=coalesce(missingCriticalUpdatesCount, -1), missingSecurityUpdatesCount=coalesce(missingSecurityUpdatesCount, -1), missingOtherUpdatesCount=coalesce(missingOtherUpdatesCount, -1), compliance=coalesce(compliance, 4), lastAssessedTime, lastUpdateAgentSeenTime, osType=2, environment=iff(ComputerEnvironment=~"Azure", 1, 2), ComplianceOrder=coalesce(ComplianceOrder, 2) )
| order by ComplianceOrder asc, missingCriticalUpdatesCount desc, missingSecurityUpdatesCount desc, missingOtherUpdatesCount desc, displayName asc
| project-away ComplianceOrder
```

##### <a name="missing-updates-list"></a>Eksik güncelleştirmeler listesi

```loganalytics
Update
| where TimeGenerated>ago(5h) and OSType=="Linux" and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=="Linux" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, BulletinUrl, BulletinID) by SourceComputerId, Product, ProductArch
| where UpdateState=~"Needed"
| project-away UpdateState, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(Product, "_", ProductArch), displayName=Product, productArch=ProductArch, classification=Classification, InformationId=BulletinID, InformationUrl=tostring(split(BulletinUrl, ";", 0)[0]), osType=1
| union(Update
| where TimeGenerated>ago(14h) and OSType!="Linux" and (Optional==false or Classification has "Critical" or Classification has "Security") and SourceComputerId in ((Heartbeat
| where TimeGenerated>ago(12h) and OSType=~"Windows" and notempty(Computer)
| summarize arg_max(TimeGenerated, Solutions) by SourceComputerId
| where Solutions has "updates"
| distinct SourceComputerId))
| summarize hint.strategy=partitioned arg_max(TimeGenerated, UpdateState, Classification, Title, KBID, PublishedDate, Approved) by Computer, SourceComputerId, UpdateID
| where UpdateState=~"Needed" and Approved!=false
| project-away UpdateState, Approved, TimeGenerated
| summarize computersCount=dcount(SourceComputerId, 2), displayName=any(Title), publishedDate=min(PublishedDate), ClassificationWeight=max(iff(Classification has "Critical", 4, iff(Classification has "Security", 2, 1))) by id=strcat(UpdateID, "_", KBID), classification=Classification, InformationId=strcat("KB", KBID), InformationUrl=iff(isnotempty(KBID), strcat("https://support.microsoft.com/kb/", KBID), ""), osType=2)
| sort by ClassificationWeight desc, computersCount desc, displayName asc
| extend informationLink=(iff(isnotempty(InformationId) and isnotempty(InformationUrl), toobject(strcat('{ "uri": "', InformationUrl, '", "text": "', InformationId, '", "target": "blank" }')), toobject('')))
| project-away ClassificationWeight, InformationId, InformationUrl
```

## <a name="using-dynamic-groups"></a>Dinamik grupları kullanma

Güncelleştirme Yönetimi, güncelleştirme dağıtımları için Azure veya Azure olmayan VM 'Lerin dinamik bir grubunu hedefleyebilme olanağı sağlar. Bu gruplar dağıtım zamanında değerlendirilir, böylece, makineleri eklemek için dağıtımınızı düzenlemeniz gerekmez.

> [!NOTE]
> Güncelleştirme dağıtımı oluştururken uygun izinlere sahip olmanız gerekir. Daha fazla bilgi için bkz. [güncelleştirmeleri yüklemek](#install-updates).

### <a name="azure-machines"></a>Azure makineleri

Bu gruplar bir sorgu tarafından tanımlanır, bir güncelleştirme dağıtımı başladığında, o grubun üyeleri değerlendirilir. Dinamik Gruplar klasik VM 'Ler ile çalışmaz. Sorgunuzu tanımlarken, dinamik grubu doldurmak için aşağıdaki öğeler birlikte kullanılabilir

* Subscription
* Kaynak grupları
* Konumlar
* Tags

![Grupları seçin](./media/automation-update-management/select-groups.png)

Dinamik bir grubun sonuçlarını önizlemek için **Önizleme** düğmesine tıklayın. Bu önizlemede, bu sırada grup üyeliği gösterilmektedir. Bu örnekte, Tag **rolüne** sahip makineleri **BackEndServer**'a eşit olarak arıyor. Bu etiket daha fazla makineye eklendiyse, bu Etiketler bu gruba karşı gelecekteki tüm dağıtımlara eklenecektir.

![Önizleme grupları](./media/automation-update-management/preview-groups.png)

### <a name="non-azure-machines"></a>Azure dışı makineler

Azure dışı makinelerde, bilgisayar grupları olarak da bilinen kayıtlı aramalar, dinamik grubu oluşturmak için kullanılır. Kayıtlı bir aramanın nasıl oluşturulduğunu öğrenmek için bkz. [bilgisayar grubu oluşturma](../azure-monitor/platform/computer-groups.md#creating-a-computer-group). Grubunuz oluşturulduktan sonra, kaydedilen aramalar listesinden bunu seçebilirsiniz. Kaydedilen aramada bilgisayarları o anda önizlemek için **Önizleme** ' ye tıklayın.

![Grupları seçin](./media/automation-update-management/select-groups-2.png)

## <a name="integrate-with-system-center-configuration-manager"></a>System Center Configuration Manager ile tümleştirme

Bilgisayarları, sunucuları ve mobil cihazları yönetmek için System Center Configuration Manager yatırım yapmış müşteriler, yazılım güncelleştirmelerini yönetmeye yardımcı olmak için Configuration Manager kuvvetini ve vadede yararlanır. Configuration Manager, yazılım güncelleştirme yönetimi (SUM) döngüsünün bir parçasıdır.

Yönetim çözümünü System Center Configuration Manager ile tümleştirmeyi öğrenmek için bkz. [System Center Configuration Manager güncelleştirme yönetimi Ile tümleştirme](oms-solution-updatemgmt-sccmintegration.md).

## <a name="inclusion-behavior"></a>İçerme davranışı

Dahil etme, uygulanacak belirli güncelleştirmeleri belirtmenize olanak tanır. Dahil edilen düzeltme ekleri veya paketleri yüklenir. Yamaları veya paketleri dahil edildiğinde ve sınıflandırma her ikisi de seçildiğinde, hem dahil edilen öğeler hem de sınıflandırmayı karşılayan öğeler yüklenir.

Dışlamaları geçersiz kılmanın dahil olduğunu bildirmek önemlidir. Örneğin, hariç tutma kuralı `*`tanımlarsanız, hepsi hariç tutulduklarında hiçbir düzeltme eki veya paket yüklenmez. Dışlanan düzeltme ekleri hala makinede eksik olarak gösterilir. Linux makineler için bir paket dahil, ancak dışlanan bir bağımlı paketi varsa, paket yüklenmez.

## <a name="patch-linux-machines"></a>Linux makinelerini yama

Aşağıdaki bölümlerde, Linux düzeltme ekiyle ilgili olası sorunlar açıklanmaktadır.

### <a name="unexpected-os-level-upgrades"></a>Beklenmeyen işletim sistemi düzeyi yükseltmeleri

Red Hat Enterprise Linux gibi bazı Linux değişkenlerde, işletim sistemi düzeyinde yükseltmeler paketler aracılığıyla gerçekleşebilir. Bu, işletim sistemi sürümü numarasının değiştiği Güncelleştirme Yönetimi çalışmasına yol açabilir. Güncelleştirme Yönetimi, bir yöneticinin Linux bilgisayarda yerel olarak kullanacağı paketleri güncelleştirmek için aynı yöntemleri kullandığından, bu davranış bilerek yapılır.

Güncelleştirme Yönetimi çalıştırmaları aracılığıyla işletim sistemi sürümünün güncelleştirilmesini önlemek için **dışlama** özelliğini kullanın.

Red Hat Enterprise Linux, dışlanacak paket adı RedHat-Release-Server. x86_64 ' dır.

![Linux için dışlanacak paketler](./media/automation-update-management/linuxpatches.png)

### <a name="critical--security-patches-arent-applied"></a>Kritik/güvenlik düzeltme ekleri uygulanmadı

Güncelleştirmeleri bir Linux makinesine dağıtırken, güncelleştirme sınıflandırmalarını seçebilirsiniz. Bu, belirtilen ölçütlere uyan makineye uygulanan güncelleştirmelere filtre uygular. Bu filtre, güncelleştirme dağıtıldığında makinede yerel olarak uygulanır.

Güncelleştirme Yönetimi bulutta güncelleştirme zenginleştirmesini gerçekleştirdiğinden, bazı güncelleştirmeler, yerel makinede bu bilgilere sahip olmasa bile güvenlik etkisi olan Güncelleştirme Yönetimi olarak işaretlenir. Sonuç olarak, bir Linux makinesine kritik güncelleştirmeler uygularsanız, bu makinede güvenlik etkisi olan olarak işaretlenmemiş güncelleştirmeler olabilir ve güncelleştirmeler uygulanmaz.

Ancak, Güncelleştirme Yönetimi ilgili güncelleştirme hakkında ek bilgi içerdiğinden, bu makineyi hala uyumsuz olarak raporlayabilir.

Güncelleştirmelerin güncelleştirme sınıflandırmasına göre dağıtımı, kutudan çıkan CentOS üzerinde çalışmaz. CentOS güncelleştirmelerini doğru bir şekilde dağıtmak için, güncelleştirmelerin uygulandığından emin olmak için tüm sınıflandırmaları seçin. SUSE için, sınıflandırma olarak *yalnızca* ' diğer güncelleştirmeler ' seçilmesi, zypper (Paket Yöneticisi) ile ilgili güvenlik güncelleştirmelerinin veya bağımlılıklarından önce gerekli olduğu durumlarda bazı güvenlik güncelleştirmelerinin de yüklenmesini sağlayabilir. Bu davranış, zypper kısıtlamasıdır. Bazı durumlarda, güncelleştirme dağıtımını yeniden çalıştırmanız gerekebilir. Doğrulamak için güncelleştirme günlüğünü kontrol edin.

## <a name="remove-a-vm-from-update-management"></a>Güncelleştirme Yönetimi bir VM 'yi kaldırma

Bir sanal makineyi Güncelleştirme Yönetimi kaldırmak için:

* Log Analytics çalışma alanınızda, VM 'yi, kapsam yapılandırması `MicrosoftDefaultScopeConfig-Updates`için kaydedilen aramadan kaldırın. Kayıtlı aramalar, çalışma alanınızda **genel** altında bulunabilir.
* Linux için [Microsoft Monitoring Agent](../azure-monitor/learn/quick-collect-windows-computer.md#clean-up-resources) 'ı veya [Log Analytics aracısını](../azure-monitor/learn/quick-collect-linux-computer.md#clean-up-resources)kaldırın.

## <a name="next-steps"></a>Sonraki adımlar

Windows sanal makineleriniz için güncelleştirmelerin nasıl yönetileceğini öğrenmek için öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Azure Windows VM 'leriniz için güncelleştirmeleri ve düzeltme eklerini yönetme](automation-tutorial-update-management.md)

* Ayrıntılı güncelleştirme verilerini görüntülemek için [Azure izleyici günlüklerinde](../log-analytics/log-analytics-log-searches.md) günlük aramalarını kullanın.
* Güncelleştirme dağıtım durumu için [uyarı oluştur](automation-tutorial-update-management.md#configure-alerts) .

* REST API üzerinden Güncelleştirme Yönetimi nasıl etkileşim kuracağınızı öğrenmek için bkz. [yazılım güncelleştirme yapılandırması](/rest/api/automation/softwareupdateconfigurations)
* Güncelleştirme Yönetimi sorunlarını giderme hakkında bilgi edinmek için bkz. [sorun giderme güncelleştirme yönetimi](troubleshoot/update-management.md)
