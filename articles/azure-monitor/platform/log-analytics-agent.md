---
title: Log Analytics aracısına genel bakış
description: Bu konuda, veri toplamak ve Azure'da barındırılan bilgisayarlarını izlemek nasıl anlamanıza yardımcı olur şirket içinde veya diğer bulut ortamında Log Analytics ile.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/04/2020
ms.openlocfilehash: bf2939c28afb682d4053a27920b9cf57795d2e86
ms.sourcegitcommit: 64def2a06d4004343ec3396e7c600af6af5b12bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2020
ms.locfileid: "77467241"
---
# <a name="log-analytics-agent-overview"></a>Log Analytics aracısına genel bakış
Azure Log Analytics Aracısı, tüm bulutta, şirket içi makinelerde ve [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/)tarafından izlenen sanal makineler arasında kapsamlı yönetim için geliştirilmiştir. Windows ve Linux aracıları, toplanan verileri Azure Izleyici 'deki Log Analytics çalışma alanınıza, ayrıca bir izleme çözümünde tanımlanan tüm benzersiz günlüklere veya ölçümlere gönderir. Log Analytics Aracısı Ayrıca Azure Izleyici 'de [VM'ler için Azure izleyici](), [Azure Güvenlik Merkezi]()ve [Azure Otomasyonu]()gibi diğer hizmetleri de destekler.

Bu makalede, aracı, sistem ve ağ gereksinimleri ve farklı dağıtım yöntemlerini ayrıntılı bir genel bakış sağlar.

> [!NOTE]
> Ayrıca, Microsoft Monitoring Agent (MMA) veya OMS Linux Aracısı olarak adlandırılan Log Analytics aracısını da görebilirsiniz.

> [!NOTE]
> Azure Tanılama uzantısı, işlem kaynaklarının Konuk işletim sisteminden izleme verilerini toplamak için kullanılabilen aracılardan biridir. Gereksinimleriniz için uygun aracıları seçme konusunda farklı aracıların ve yönergelerin açıklaması için bkz. [Azure izleyici aracılarına genel bakış](agents-overview.md) .

## <a name="comparison-to-azure-diagnostics-extension"></a>Azure tanılama uzantısına kıyasla
Azure Izleyici 'de Azure [Tanılama uzantısı](diagnostics-extension-overview.md) , Azure sanal makinelerinin Konuk işletim sisteminden izleme verilerini toplamak için de kullanılabilir. Gereksinimlerinize bağlı olarak ya da her ikisini de kullanmayı seçebilirsiniz. Azure izleyici aracılarının ayrıntılı bir karşılaştırması için bkz. [Azure izleyici aracılarına genel bakış](agents-overview.md) . 

Dikkate alınması gereken önemli farklılıklar şunlardır:

- Azure Tanılama uzantısı yalnızca Azure sanal makineler ile kullanılabilir. Log Analytics Aracısı, Azure 'da, diğer bulutlarda ve Şirket içindeki sanal makinelerle birlikte kullanılabilir.
- Azure Tanılama uzantısı, verileri Azure depolama 'ya, [Azure Izleme ölçümlerine](data-platform-metrics.md) (yalnızca Windows) ve Event Hubs gönderir. Log Analytics Aracısı verileri [Azure Izleyici günlüklerine](data-platform-logs.md)toplar.
- Log Analytics Aracısı, [Azure Güvenlik Merkezi](/azure/security-center/)gibi [çözümler](../monitor-reference.md#insights-and-core-solutions), [VM'ler için Azure izleyici](../insights/vminsights-overview.md)ve diğer hizmetler için gereklidir.

## <a name="costs"></a>Maliyetler
Log Analytics Aracısı için maliyet yoktur, ancak alınan veriler için ücret ödemeniz gerekebilir. Log Analytics çalışma alanında toplanan verilerin fiyatlandırmasıyla ilgili ayrıntılı bilgi için [Azure Izleyici günlükleriyle kullanımı ve maliyetleri yönetin ' i](manage-cost-storage.md) işaretleyin.

## <a name="data-collected"></a>Toplanan veriler
Aşağıdaki tabloda, tüm bağlı aracılardan toplanacak bir Log Analytics çalışma alanını yapılandırabileceğiniz veri türleri listelenmektedir. Diğer veri türlerini toplamak için Log Analytics aracısını kullanan Öngörüler, çözümler ve diğer çözümlerin listesi için bkz. [Azure izleyici tarafından Izlenen nedir?](../monitor-reference.md) .

| Veri Kaynağı | Açıklama |
| --- | --- |
| [Windows olay günlükleri](data-sources-windows-events.md) | Windows olay günlüğü sistemine gönderilen bilgiler. |
| [Syslog](data-sources-syslog.md)                     | Linux olay günlüğü sistemine gönderilen bilgiler. |
| [Performans](data-sources-performance-counters.md)  | Sayısal değerler, işletim sistemi ve iş yüklerinin farklı yönlerinin performansını ölçerek. |
| [IIS günlükleri](data-sources-iis-logs.md)                 | Konuk işletim sisteminde çalışan IIS Web siteleri için kullanım bilgileri. |
| [Özel günlükler](data-sources-custom-logs.md)           | Hem Windows hem de Linux bilgisayarlardaki metin dosyalarından gelen olaylar. |

## <a name="data-destinations"></a>Veri hedefleri
Log Analytics Aracısı verileri Azure Izleyici 'de bir Log Analytics çalışma alanına gönderir. Birden çok çalışma alanına ve System Center Operations Manager yönetim grubuna veri göndermek için Windows aracısının birden çok bağlantısı olabilir. Linux Aracısı yalnızca tek bir hedefe gönderebilir.

## <a name="other-services"></a>Diğer hizmetler
Linux ve Windows için aracı yalnızca Azure Izleyici 'ye bağlanmıyor, karma Runbook Worker rolünü ve [değişiklik izleme](../../automation/change-tracking.md), [güncelleştirme yönetimi](../../automation/automation-update-management.md)ve [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer hizmetleri barındırmak için Azure Otomasyonu 'nu da destekler. Karma Runbook Worker rolü hakkında daha fazla bilgi için bkz. [Azure Otomasyonu karma Runbook Worker](../../automation/automation-hybrid-runbook-worker.md).  

## <a name="installation-and-configuration"></a>Yükleme ve yapılandırma

Veri toplamak için Log Analytics aracılarını kullanırken, aracı dağıtımınızı planlamak için aşağıdakileri anlamanız gerekir:

* Windows aracılarından veri toplamak için, [her aracıyı bir veya daha fazla çalışma alanına rapor verecek şekilde yapılandırabilirsiniz](agent-windows.md), bu da bir System Center Operations Manager yönetim grubuna bildirimde bulunur. Windows Aracısı en fazla dört çalışma alanı rapor edebilir.
* Linux Aracısı çoklu barındırıltı desteklemez ve yalnızca tek bir çalışma alanına rapor verebilir.
* Windows Aracısı [fıps 140 standardını](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation)destekler, ancak Linux Aracısı tarafından desteklenmez.  

System Center Operations Manager 2012 R2 veya sonraki bir sürümünü kullanıyorsanız:

* Her bir Operations Manager yönetim grubu [yalnızca bir çalışma alanına bağlanabilir](om-agents.md).
* Bir yönetim grubuna rapor veren Linux bilgisayarların doğrudan bir Log Analytics çalışma alanına rapor verecek şekilde yapılandırılması gerekir. Linux bilgisayarlarınız zaten bir çalışma alanına doğrudan raporlama yaptıysanız ve bunları Operations Manager izlemek istiyorsanız, [bir Operations Manager yönetim grubuna raporlamak](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group)için aşağıdaki adımları izleyin.
* Log Analytics Windows aracısını Windows bilgisayarına yükleyebilir ve hem bir çalışma alanıyla hem de bir çalışma alanı ile tümleşik Operations Manager, farklı bir çalışma alanı için rapor verebilirsiniz.


Log Analytics aracısını yüklemeye ve gereksinimlerinize bağlı olarak makinenizi Azure Izleyici 'ye bağlamaya yönelik birden çok yöntem vardır. Aşağıdaki tabloda, kuruluşunuzda en iyi şekilde çalıştığı belirlemek için her bir yöntemin vurgulanmaktadır.

|Kaynak | Yöntem | Açıklama|
|-------|-------------|-------------|
|Azure VM| [Azure portal el ile](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json) | Log Analytics çalışma alanından dağıtılacak VM 'Leri belirtin. |
| | Azure CLı kullanarak veya bir Azure Resource Manager şablonuyla [Windows](../../virtual-machines/extensions/oms-windows.md) veya [LINUX](../../virtual-machines/extensions/oms-linux.md) için Log Analytics VM Uzantısı | Uzantı, Azure sanal makinelerinde Log Analytics aracısını yükler ve bunları mevcut bir Azure İzleyici çalışma alanına kaydeder. |
| | [VM'ler için Azure İzleyici](../insights/vminsights-enable-overview.md) | VM'ler için Azure İzleyici izleme etkinleştirdiğinizde, Log Analytics uzantısı ve Aracısı yüklenir. |
| | [Azure Güvenlik Merkezi otomatik sağlama](../../security-center/security-center-enable-data-collection.md) | Azure Güvenlik Merkezi, desteklenen tüm Azure VM 'lerinde Log Analytics aracısını ve güvenlik açıklarını ve tehditleri izlemek için bunu etkinleştirdiğinizde oluşturulan tüm yeni olanları temin edebilir. Etkinleştirilirse, yüklü bir aracı olmayan yeni veya mevcut VM 'ler temin edilir. |
| Hibrit Windows bilgisayarı| [El ile yüklemesi](agent-windows.md) | Komut satırından Microsoft Monitoring Agent 'ı yükler. |
| | [Azure Automation DSC](agent-windows.md#install-the-agent-using-dsc-in-azure-automation) | Yüklemeyi Azure Automation DSC ile otomatikleştirin. |
| | [Azure Stack içeren şablon Kaynak Yöneticisi](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) | Veri merkezinizde Microsoft Azure Stack dağıttıysanız bir Azure Resource Manager şablonu kullanın.| 
| Hibrit Linux bilgisayarı| [El ile yüklemesi](../../azure-monitor/learn/quick-collect-linux-computer.md)|Github'da barındırılan bir sarmalayıcı betik çağırma Linux için aracıyı yükleyin. | 
| System Center Operations Manager|[Operations Manager Log Analytics ile tümleştirin](../../azure-monitor/platform/om-agents.md) | Toplanan verileri Windows bilgisayarlardan bir yönetim grubuna iletmek için Operations Manager ile Azure Izleyici günlükleri arasındaki tümleştirmeyi yapılandırın.|  


## <a name="supported-windows-operating-systems"></a>Desteklenen Windows işletim sistemleri

Aşağıdaki Windows işletim sistemi sürümleri Windows aracısı için resmi olarak desteklenir:

* Windows Server 2019
* Windows Server 2008 SP2 (x64), 2008 R2, 2012, 2012 R2, 2016, sürüm 1709 ve 1803
* Windows 7 SP1, Windows 8 Enterprise ve Pro ve Windows 10 Enterprise ve Pro

>[!NOTE]
>Windows için Log Analytics Aracısı, sunucu izleme senaryolarını destekleyecek şekilde tasarlandıkça, sunucu işletim sistemi için yapılandırılmış ve iyileştirilmiş iş yüklerini desteklemek üzere Windows istemcisini çalıştırabiliriz. Aracı Windows istemcisini destekler, ancak izleme çözümlerimiz açıkça belirtilmedikçe istemci izleme senaryolarına odaklanmaz.

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Bu bölümde, desteklenen Linux dağıtımları hakkında ayrıntılar sağlar.

Ağustos 2018 tarihinden sonraki sürümleri ile başlayarak, aşağıdaki değişiklikleri destek modelimizi yapıyoruz:  

* Yalnızca sunucu sürümleri desteklenir, istemci değil.  
* Azure Linux tarafından onaylanan yeni [kaldırmalar](../../virtual-machines/linux/endorsed-distros.md) sürümleri her zaman desteklenmektedir.  
* Listelenen her ana sürümünün tüm ikincil sürümleri desteklenir.
* Üreticinin destek bitiş tarihi geçmiş sürümleri desteklenmez.  
* AMI yeni sürümleri desteklenmez.  
* SSL çalıştıran sürümler 1.x varsayılan olarak desteklenir.

>[!NOTE]
>Bir distro veya destek modelimizi hizalama değil ve şu anda desteklenmeyen bir sürümünü kullanıyorsanız, Microsoft destek sürümleri çatalı aracı ile ilgili Yardım sağlamaz sıkan Bu deponun çatalını oluşturmanız önerilir.

* Amazon Linux 2017.09 (x 64)
* CentOS Linux 6 (x86/x64) ve 7 (x 64)  
* Oracle Linux 6 ve 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) ve 7 (x 64)
* Debian GNU/Linux 8 ve 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) ve 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64) ve 15 (x64)

>[!NOTE]
>OpenSSL 1.1.0 yalnızca x86_x64 platformları (64-bit) ve OpenSSL 1.x herhangi bir platformda desteklenmiyor ' den önceki desteklenir.
>

### <a name="agent-prerequisites"></a>Aracı önkoşulları

Aşağıdaki tabloda, aracının yükleneceği desteklenen Linux destekleri için gereken paketler vurgulanmıştır.

|Gerekli paket |Açıklama |En düşük sürüm |
|-----------------|------------|----------------|
|GLIBC |    GNU C Kitaplığı | 2.5-12 
|Openssl    | OpenSSL kitaplıkları | 1.0. x veya 1.1. x |
|Kıvr | Web istemcisini kıvır | 7.15.5 |
|Python-ctypes | | 
|KULLANıCıSı | Takılabilir kimlik doğrulama modülleri | | 

>[!NOTE]
>Syslog iletilerini toplamak için rsyslog veya Syslog-ng gerekir. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) 5 sürümünde varsayılan syslog daemon'u syslog olay toplaması için desteklenmiyor. Bu dağıtımların bu sürümünden Syslog verileri toplamak için rsyslog arka plan programı yüklenmeli ve sysklog ' ı değiştirecek şekilde yapılandırılmalıdır.

## <a name="tls-12-protocol"></a>TLS 1.2 Protokolü

Azure Izleyici günlüklerine aktarılan verilerin güvenliğini sağlamak için, aracıyı en az Aktarım Katmanı Güvenliği (TLS) 1,2 kullanacak şekilde yapılandırmanızı kesinlikle öneririz. TLS/Güvenli Yuva Katmanı (SSL) uygulamasının güvenlik açığı olduğu ve geriye dönük uyumlulukla hala çalışmaya devam eden daha eski sürümlerinin **kullanılması önerilmez**.  Daha fazla bilgi için [TLS 1,2 kullanarak verileri güvenli bir şekilde göndermeyi](data-security.md#sending-data-securely-using-tls-12)inceleyin. 


## <a name="network-requirements"></a>Ağ gereksinimleri
Linux ve Windows için aracı, TCP bağlantı noktası 443 üzerinden Azure Izleyici hizmetine giden iletişim kurar ve makine Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya ara sunucu üzerinden bağlanıyorsa, ağ yapılandırmasını anlamak için aşağıdaki gereksinimleri gözden geçirin Gerekli. BT güvenlik ilkeleriniz ağdaki bilgisayarların Internet 'e bağlanmasına izin vermediğinden, bir [Log Analytics ağ geçidi](gateway.md) ayarlayabilir ve ardından aracıyı ağ geçidinden Azure izleyici günlüklerine bağlanacak şekilde yapılandırabilirsiniz. Aracı daha sonra yapılandırma bilgilerini alabilir ve çalışma alanınızda etkinleştirdiğiniz veri toplama kurallarına ve izleme çözümlerine bağlı olarak toplanan verileri gönderebilir.

![Log Analytics Aracısı iletişimi diyagramı](./media/log-analytics-agent/log-analytics-agent-01.png)


## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri
Aşağıdaki bilgiler, Linux ve Windows aracısının Azure Izleyici günlükleriyle iletişim kurması için gereken proxy ve güvenlik duvarı yapılandırma bilgilerini listeler.  

|Aracı Kaynağı|Bağlantı Noktaları |Yön |HTTPS denetlemesini atlama|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Bağlantı noktası 443 |Giden|Yes |  
|*.oms.opinsights.azure.com |Bağlantı noktası 443 |Giden|Yes |  
|*.blob.core.windows.net |Bağlantı noktası 443 |Giden|Yes |  

Azure Kamu için gereken güvenlik duvarı bilgileri için bkz. [Azure Kamu Yönetimi](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Ortamınızdaki runbook 'ları veya yönetim çözümlerini kullanmak üzere otomasyon hizmetine bağlanmak ve kaydolmak için Azure Otomasyonu karma Runbook Worker kullanmayı planlıyorsanız, bağlantı noktası numarasına ve [ağınızı karma Runbook Worker Için yapılandırma](../../automation/automation-hybrid-runbook-worker.md#network-planning)bölümünde açıklanan URL 'lere erişimi olmalıdır. 

Windows ve Linux Aracısı, HTTPS protokolünü kullanarak bir proxy sunucu aracılığıyla veya Azure Izleyici 'ye Log Analytics ağ geçidiyle iletişim kurmasını destekler.  Anonim ve temel kimlik doğrulaması (kullanıcı adı/parola) desteklenir.  Doğrudan hizmete bağlı olan Windows Aracısı için, proxy yapılandırması yükleme sırasında veya denetim masasından ya da PowerShell ile [dağıtımdan sonra](agent-manage.md#update-proxy-settings) belirtilir.  

Linux Aracısı için ara sunucu, yükleme sırasında veya proxy. conf yapılandırma dosyası değiştirilerek [yüklendikten sonra](agent-manage.md#update-proxy-settings) belirtilir.  Linux Aracısı proxy yapılandırması değeri sözdizimi aşağıdaki gibidir:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Ara sunucunuz kimlik doğrulamasından geçmesini gerektirmiyorsa, Linux Aracısı hala bir sanal kullanıcı/parola sağlama gerektirir. Bu, herhangi bir kullanıcı adı veya parola olabilir.

|Özellik| Açıklama |
|--------|-------------|
|Protokol | https |
|kullanıcı | Ara sunucu kimlik doğrulaması için isteğe bağlı bir kullanıcı adı |
|password | Proxy kimlik doğrulaması için isteğe bağlı parola |
|proxyhost | Adresi veya FQDN proxy sunucusu/Log Analytics ağ geçidi |
|port | Proxy sunucusu/Log Analytics ağ geçidi için isteğe bağlı bağlantı noktası numarası |

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Parolandaki "\@" gibi özel karakterler kullanırsanız, değer yanlış ayrıştırılacağından bir ara sunucu bağlantı hatası alırsınız.  Bu sorunu geçici olarak çözmek için URL 'deki parolayı [Urlşifre çözme](https://www.urldecoder.org/)gibi bir araç kullanarak kodlayın.  



## <a name="next-steps"></a>Sonraki adımlar

* Windows veya Linux sisteminizden veri toplamak için kullanılabilir veri kaynaklarını anlamak üzere [veri kaynaklarını](agent-data-sources.md) gözden geçirin. 
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin. 
* Azure Izleyici 'ye işlevsellik ekleyen ve ayrıca Log Analytics çalışma alanına veri toplayacağınız [çözümleri izleme](../insights/solutions.md) hakkında bilgi edinin.

