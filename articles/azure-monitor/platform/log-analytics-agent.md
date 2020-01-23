---
title: Azure Log Analytics aracısını log verileri toplama | Microsoft Docs
description: Bu konuda, veri toplamak ve Azure'da barındırılan bilgisayarlarını izlemek nasıl anlamanıza yardımcı olur şirket içinde veya diğer bulut ortamında Log Analytics ile.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/24/2019
ms.openlocfilehash: 8e563ae095cf39cdce3e671d4099d2bf1592100a
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2020
ms.locfileid: "76513635"
---
# <a name="collect-log-data-with-the-log-analytics-agent"></a>Log Analytics aracısıyla günlük verilerini toplama

Daha önce Microsoft Monitoring Agent (MMA) veya OMS Linux Aracısı olarak adlandırılan Azure Log Analytics Aracısı, şirket içi makineler, [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/)tarafından izlenen bilgisayarlar ve herhangi bir buluttaki sanal makineler genelinde kapsamlı yönetim için geliştirilmiştir. Windows ve Linux aracıları, Azure Izleyici 'ye ekler ve Log Analytics çalışma alanınızdaki farklı kaynaklardan toplanan günlük verilerini, ayrıca bir izleme çözümünde tanımlanan benzersiz Günlükler veya ölçümleri depolar. 

Bu makalede, aracı, sistem ve ağ gereksinimleri ve farklı dağıtım yöntemlerini ayrıntılı bir genel bakış sağlar.

## <a name="overview"></a>Genel Bakış

![Log Analytics Aracısı iletişimi diyagramı](./media/log-analytics-agent/log-analytics-agent-01.png)

Toplanan verileri analiz etmek ve üzerinde işlem yapmadan önce, Azure Izleyici hizmetine veri göndermek istediğiniz tüm makineler için aracıları yüklemeniz ve bağlamanız gerekir. Windows ve Linux için ve Kurulum, komut satırı kullanarak karma bir ortamı veya makineleri ile Desired State Configuration (DSC) Azure automation'da Azure Log Analytics VM uzantısı kullanarak, Azure sanal makinelerinde aracıları yükleyebilirsiniz. 

Linux ve Windows için aracı, TCP bağlantı noktası 443 üzerinden Azure Izleyici hizmetine giden iletişim kurar ve makine Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya ara sunucu üzerinden bağlanıyorsa, ağ yapılandırmasını anlamak için aşağıdaki gereksinimleri gözden geçirin Gerekli. BT güvenlik ilkeleriniz ağdaki bilgisayarların Internet 'e bağlanmasına izin vermediğinden, bir [Log Analytics ağ geçidi](gateway.md) ayarlayabilir ve ardından aracıyı ağ geçidinden Azure izleyici günlüklerine bağlanacak şekilde yapılandırabilirsiniz. Aracı daha sonra yapılandırma bilgilerini alabilir ve çalışma alanınızda etkinleştirdiğiniz veri toplama kurallarına ve izleme çözümlerine bağlı olarak toplanan verileri gönderebilir. 

Veri toplamak için Log Analytics aracılarını kullanırken, aracı dağıtımınızı planlamak için aşağıdakileri anlamanız gerekir:

* Windows aracılarından veri toplamak için, [her aracıyı bir veya daha fazla çalışma alanına rapor verecek şekilde yapılandırabilirsiniz](agent-windows.md), bu da bir System Center Operations Manager yönetim grubuna bildirimde bulunur. Windows Aracısı en fazla dört çalışma alanı rapor edebilir.
* Linux Aracısı çoklu barındırıltı desteklemez ve yalnızca tek bir çalışma alanına rapor verebilir.
* Windows Aracısı [fıps 140 standardını](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation)destekler, ancak Linux Aracısı tarafından desteklenmez.  

System Center Operations Manager 2012 R2 veya sonraki bir sürümünü kullanıyorsanız:

* Her bir Operations Manager yönetim grubu [yalnızca bir çalışma alanına bağlanabilir](om-agents.md).
* Bir yönetim grubuna rapor veren Linux bilgisayarların doğrudan bir Log Analytics çalışma alanına rapor verecek şekilde yapılandırılması gerekir. Linux bilgisayarlarınız zaten bir çalışma alanına doğrudan raporlama yaptıysanız ve bunları Operations Manager izlemek istiyorsanız, [bir Operations Manager yönetim grubuna raporlamak](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group)için aşağıdaki adımları izleyin.
* Log Analytics Windows aracısını Windows bilgisayarına yükleyebilir ve hem bir çalışma alanıyla hem de bir çalışma alanı ile tümleşik Operations Manager, farklı bir çalışma alanı için rapor verebilirsiniz.

Linux ve Windows için aracı yalnızca Azure Izleyici 'ye bağlanmıyor, karma Runbook Worker rolünü ve [değişiklik izleme](../../automation/change-tracking.md), [güncelleştirme yönetimi](../../automation/automation-update-management.md)ve [Azure Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer hizmetleri barındırmak için Azure Otomasyonu 'nu da destekler. Karma Runbook çalışanı rolü hakkında daha fazla bilgi için bkz. [Azure Otomasyon karma Runbook çalışanı](../../automation/automation-hybrid-runbook-worker.md).  

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
* Yeni sürümlerini [Azure destekli Linux dağıtımları](../../virtual-machines/linux/endorsed-distros.md) her zaman desteklenir.  
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
|Curl | Web istemcisini kıvır | 7.15.5 |
|Python-ctypes | | 
|PAM | Eklenebilir Kimlik Doğrulaması Modülleri | | 

>[!NOTE]
>Syslog iletilerini toplamak için rsyslog veya Syslog-ng gerekir. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) 5 sürümünde varsayılan syslog daemon'u syslog olay toplaması için desteklenmiyor. Bu dağıtımların bu sürümünden Syslog verileri toplamak için rsyslog arka plan programı yüklenmeli ve sysklog ' ı değiştirecek şekilde yapılandırılmalıdır.

## <a name="tls-12-protocol"></a>TLS 1.2 Protokolü

Azure Izleyici günlüklerine geçiş sırasındaki verilerin güvenliğini sağlamak için aracıyı en az Aktarım Katmanı Güvenliği (TLS) 1,2 kullanacak şekilde yapılandırmanızı önemle öneririz. TLS/Güvenli Yuva Katmanı (SSL) daha eski sürümleri, savunmasız bulundu ve bunlar yine de şu anda geriye dönük uyumluluk izin vermek için çalışırken, bunlar **önerilmez**.  Ek bilgi için gözden [TLS 1.2 kullanarak güvenli bir şekilde veri gönderen](data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri

Aşağıdaki bilgiler, Linux ve Windows aracısının Azure Izleyici günlükleriyle iletişim kurması için gereken proxy ve güvenlik duvarı yapılandırma bilgilerini listeler.  

|Aracı Kaynağı|Bağlantı Noktaları |Yön |HTTPS denetlemesini atlama|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Bağlantı noktası 443 |Giden|Evet |  
|*.oms.opinsights.azure.com |Bağlantı noktası 443 |Giden|Evet |  
|*.blob.core.windows.net |Bağlantı noktası 443 |Giden|Evet |  

Azure Kamu için gereken güvenlik duvarı bilgileri için bkz. [Azure Kamu Yönetimi](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Ortamınızdaki runbook 'ları veya yönetim çözümlerini kullanmak üzere otomasyon hizmetine bağlanmak ve kaydolmak için Azure Otomasyonu karma Runbook Worker kullanmayı planlıyorsanız, bağlantı noktası numarasına ve [ağınızı karma Runbook Worker Için yapılandırma](../../automation/automation-hybrid-runbook-worker.md#network-planning)bölümünde açıklanan URL 'lere erişimi olmalıdır. 

Windows ve Linux Aracısı, HTTPS protokolünü kullanarak bir proxy sunucu aracılığıyla veya Azure Izleyici 'ye Log Analytics ağ geçidiyle iletişim kurmasını destekler.  Anonim ve temel kimlik doğrulaması (kullanıcı adı/parola) desteklenir.  Yüklemesi sırasında belirtilen hizmete doğrudan bağlı Windows aracısı için proxy yapılandırmasını veya [dağıtımdan sonra](agent-manage.md#update-proxy-settings) Denetim Masası'ndan veya PowerShell ile.  

Linux aracısı için proxy sunucusu yüklemesi sırasında belirtilen veya [yüklemeden sonra](agent-manage.md#update-proxy-settings) proxy.conf yapılandırma dosyasını değiştirerek.  Linux Aracısı proxy yapılandırması değeri sözdizimi aşağıdaki gibidir:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Ara sunucunuz kimlik doğrulamasından geçmesini gerektirmiyorsa, Linux Aracısı hala bir sanal kullanıcı/parola sağlama gerektirir. Bu, herhangi bir kullanıcı adı veya parola olabilir.

|Özellik| Açıklama |
|--------|-------------|
|Protokol | https |
|user | Ara sunucu kimlik doğrulaması için isteğe bağlı bir kullanıcı adı |
|parola | Proxy kimlik doğrulaması için isteğe bağlı parola |
|proxyhost | Adresi veya FQDN proxy sunucusu/Log Analytics ağ geçidi |
|port | Proxy sunucusu/Log Analytics ağ geçidi için isteğe bağlı bağlantı noktası numarası |

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Gibi özel karakterleri kullanırsanız "\@" parolanızı, değeri yanlış ayrıştırılır için proxy bağlantı hatası alırsınız.  Bu sorunu geçici olarak çözmek için parola gibi bir araç kullanarak URL kodlama [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Aracıyı yükle ve yapılandır

Azure aboneliğinizdeki veya karma ortamınızdaki makineleri doğrudan Azure Izleyici günlükleriyle bağlamak, gereksinimlerinize bağlı olarak farklı yöntemler kullanılarak gerçekleştirilebilir. Aşağıdaki tabloda, kuruluşunuzda en iyi şekilde çalıştığı belirlemek için her bir yöntemin vurgulanmaktadır.

|Kaynak | Yöntem | Açıklama|
|-------|-------------|-------------|
|Azure VM| -Log Analytics VM uzantısı için [Windows](../../virtual-machines/extensions/oms-windows.md) veya [Linux](../../virtual-machines/extensions/oms-linux.md) Azure CLI kullanarak veya Azure Resource Manager şablonu ile<br>[Azure Portal el ile](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json) - <br>[Azure Güvenlik Merkezi otomatik sağlama](../../security-center/security-center-enable-data-collection.md) - | -Uzantı Log Analytics aracısını Azure sanal makinelerine yükleyip mevcut bir Azure Izleyici çalışma alanına kaydeder.<br>-Azure Güvenlik Merkezi, desteklenen tüm Azure VM 'lerinde Log Analytics aracısını ve güvenlik açıklarını ve tehditleri izlemek için bu uygulamayı etkinleştirdiğinizde oluşturulan tüm yeni olanları temin edebilir. Etkinleştirilirse, yüklü bir aracı olmayan yeni veya mevcut VM 'ler temin edilir.|
| Hibrit Windows bilgisayarı|- [El ile yükleme](agent-windows.md)<br>- [Azure Otomasyonu DSC](agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Azure Stack ile Resource Manager şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Veri merkezinizde Microsoft Azure Stack dağıttıysanız, Microsoft Monitoring Agent 'ı komut satırından veya Azure Automation DSC, [Configuration Manager](https://docs.microsoft.com/configmgr/apps/deploy-use/deploy-applications)gibi otomatikleştirilmiş bir yöntem veya Azure Resource Manager şablonuyla birlikte kullanarak yükleyebilirsiniz.| 
| Hibrit Linux bilgisayarı| [El ile yükleme](../../azure-monitor/learn/quick-collect-linux-computer.md)|Github'da barındırılan bir sarmalayıcı betik çağırma Linux için aracıyı yükleyin. | 
| System Center Operations Manager|[Operations Manager'ı Log Analytics ile tümleştirme](om-agents.md) | Toplanan verileri Windows bilgisayarlardan bir yönetim grubuna iletmek için Operations Manager ile Azure Izleyici günlükleri arasındaki tümleştirmeyi yapılandırın.|  

## <a name="next-steps"></a>Sonraki adımlar

* Gözden geçirme [veri kaynakları](agent-data-sources.md) Windows veya Linux sisteminizden veri toplamak kullanılabilir veri kaynaklarını anlamasına olanak. 

* Hakkında bilgi edinin [oturum sorguları](../log-query/log-query-overview.md) veri kaynakları ve çözümlerinden toplanan verileri analiz etmek için. 

* Hakkında bilgi edinin [izleme çözümleri](../insights/solutions.md) işlevselliği eklemek için Azure İzleyici ve ayrıca Log Analytics çalışma alanına veri toplayın.
