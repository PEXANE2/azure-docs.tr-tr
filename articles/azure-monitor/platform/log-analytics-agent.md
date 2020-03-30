---
title: Log Analytics acentesine genel bakış
description: Bu konu, Log Analytics ile Azure' da, şirket içinde veya diğer bulut ortamında barındırılan verileri nasıl toplayacağınızı ve bilgisayarları nasıl izleyeceğinizi anlamanıza yardımcı olur.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 02/04/2020
ms.openlocfilehash: 1ca03cde57a9496054d0860fbb70bd286caabe46
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79533258"
---
# <a name="log-analytics-agent-overview"></a>Log Analytics acentesine genel bakış
Azure Log Analytics aracısı, sanal makineler arasında herhangi bir bulutta, şirket içi makinelerde ve [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/)tarafından izlenen kapsamlı yönetim için geliştirilmiştir. Windows ve Linux aracıları, farklı kaynaklardan toplanan verileri Azure Monitor'daki Log Analytics çalışma alanınıza ve bir izleme çözümünde tanımlandığı gibi benzersiz günlüklere veya ölçümlere gönderir. Log Analytics aracısı, Azure Monitor'daki öngörüleri ve [VM'ler için Azure Monitor,](../insights/vminsights-enable-overview.md) [Azure Güvenlik Merkezi](/azure/security-center/)ve Azure [Otomasyonu](../../automation/automation-intro.md)gibi diğer hizmetleri de destekler.

Bu makalede, aracı, sistem ve ağ gereksinimleri ve farklı dağıtım yöntemleri ayrıntılı bir genel bakış sağlar.

> [!NOTE]
> Microsoft İzleme Aracısı (MMA) veya OMS Linux aracısı olarak adlandırılan Log Analytics aracısını da görebilirsiniz.

> [!NOTE]
> Azure Tanılama uzantısı, bilgi işlem kaynaklarının konuk işletim sisteminden izleme verilerini toplamak için kullanılabilen aracılardan biridir. Farklı aracıların açıklaması için [Azure Monitor aracılarının genel görünümü](agents-overview.md) ve gereksinimleriniz için uygun aracıların seçilmesi yle ilgili kılavuza bakın.

## <a name="comparison-to-azure-diagnostics-extension"></a>Azure tanılama uzantısı ile karşılaştırma
Azure Monitor'daki [Azure tanılama uzantısı,](diagnostics-extension-overview.md) Azure sanal makinelerin konuk işletim sisteminden izleme verileri toplamak için de kullanılabilir. Gereksinimlerinize bağlı olarak her ikisini veya her ikisini de kullanmayı seçebilirsiniz. Azure Monitörü aracılarının ayrıntılı bir karşılaştırması için [Azure Monitor aracılarının genel görünümüne](agents-overview.md) bakın. 

Göz önünde bulundurulması gereken temel farklar şunlardır:

- Azure Tanı Uzantısı yalnızca Azure sanal makineleriyle kullanılabilir. Log Analytics aracısı Azure'daki sanal makinelerle, diğer bulutlarda ve şirket içinde kullanılabilir.
- Azure Tanılama uzantısı verileri Azure Depolama, [Azure Monitör Ölçümleri](data-platform-metrics.md) (yalnızca Windows) ve Etkinlik Hub'larına gönderir. Log Analytics aracısı [Azure Monitör Günlükleri](data-platform-logs.md)için veri toplar.
- Log Analytics aracısı [çözümler,](../monitor-reference.md#insights-and-core-solutions) [VM'ler için Azure Monitör](../insights/vminsights-overview.md)ve [Azure Güvenlik Merkezi](/azure/security-center/)gibi diğer hizmetler için gereklidir.

## <a name="costs"></a>Maliyetler
Log Analytics aracısı için herhangi bir ücret yoktur, ancak alınan veriler için ücrete tabi olabilirsiniz. Bir Log Analytics çalışma alanında toplanan verilerin fiyatlandırması hakkında ayrıntılı bilgi için [Azure Monitör Günlükleri ile kullanımı ve maliyetleri yönet'i](manage-cost-storage.md) kontrol edin.

## <a name="data-collected"></a>Toplanan veriler
Aşağıdaki tablo, bağlı tüm aracılardan toplamak üzere bir Log Analytics çalışma alanını yapılandırabileceğiniz veri türlerini listeler. Bkz. [Azure Monitor tarafından neler izlenir?](../monitor-reference.md)

| veri kaynağı | Açıklama |
| --- | --- |
| [Windows Olay günlükleri](data-sources-windows-events.md) | Windows olay günlüğü sistemine gönderilen bilgiler. |
| [Syslog](data-sources-syslog.md)                     | Linux olay günlüğü sistemine gönderilen bilgiler. |
| [Performans](data-sources-performance-counters.md)  | İşletim sisteminin ve iş yüklerinin farklı yönlerinin performansını ölçen sayısal değerler. |
| [IIS günlükleri](data-sources-iis-logs.md)                 | Konuk işletim sisteminde çalışan IIS web sitelerinin kullanım bilgileri. |
| [Özel günlükler](data-sources-custom-logs.md)           | Hem Windows hem de Linux bilgisayarlardaki metin dosyalarından gelen olaylar. |

## <a name="data-destinations"></a>Veri hedefleri
Log Analytics aracısı, Azure Monitor'daki Bir Günlük Analizi çalışma alanına veri gönderir. Windows aracısı, birden çok çalışma alanı ve Sistem Merkezi Operasyon Yöneticisi yönetim gruplarına veri göndermek için çok yuvalı olabilir. Linux aracısı yalnızca tek bir hedefe gönderebilir.

## <a name="other-services"></a>Diğer hizmetler
Linux ve Windows aracısı yalnızca Azure Monitor'a bağlanmak için değil, Aynı zamanda Karma Runbook çalışanı rolünü ve [Değişiklik İzleme,](../../automation/change-tracking.md) [Güncelleştirme Yönetimi](../../automation/automation-update-management.md)ve Azure [Güvenlik Merkezi](../../security-center/security-center-intro.md)gibi diğer hizmetleri barındırmak için Azure Otomasyonu'nu da destekler. Karma Runbook Worker rolü hakkında daha fazla bilgi için Azure [Otomasyon Karma Runbook Worker'a](../../automation/automation-hybrid-runbook-worker.md)bakın.  

## <a name="installation-and-configuration"></a>Yükleme ve yapılandırma

Veri toplamak için Log Analytics aracılarını kullanırken, aracı dağıtımınızı planlamak için aşağıdakileri anlamanız gerekir:

* Windows aracılarından veri toplamak için, sistem merkezi operasyon yöneticisi yönetim grubuna rapor verirken bile [her aracıbir veya daha fazla çalışma alanlarına rapor layacak](agent-windows.md)şekilde yapılandırabilirsiniz. Windows aracısı en fazla dört çalışma alanı bildirebilir.
* Linux aracısı çoklu homing'i desteklemez ve yalnızca tek bir çalışma alanına rapor verebilir.
* Windows aracısı [FIPS 140 standardını](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation)desteklerken, Linux aracısı bunu desteklemez.  

System Center Operations Manager 2012 R2 veya sonraki lerini kullanıyorsanız:

* Her Operasyon Yöneticisi yönetim grubu [yalnızca bir çalışma alanına bağlanabilir.](om-agents.md)
* Bir yönetim grubuna rapor eden Linux bilgisayarları, doğrudan bir Log Analytics çalışma alanına raporlanacak şekilde yapılandırılmalıdır. Linux bilgisayarlarınız zaten doğrudan bir çalışma alanına rapor veriyorsa ve bunları Operations Manager ile izlemek istiyorsanız, [bir Operasyon Yöneticisi yönetim grubuna rapor](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group)vermek için aşağıdaki adımları izleyin.
* Windows bilgisayarına Log Analytics Windows aracısını yükleyebilir ve hem çalışma alanıyla tümleşik operations manager'a hem de farklı bir çalışma alanına rapor etmesini sağlayabilirsiniz.


Log Analytics aracısını yüklemek ve makinenizi gereksinimlerinize bağlı olarak Azure Monitörüne bağlamak için birden çok yöntem vardır. Aşağıdaki tablo, kuruluşunuzda hangisinin en iyi çalıştığını belirlemek için her yöntemi vurgular.

|Kaynak | Yöntem | Açıklama|
|-------|-------------|-------------|
|Azure VM| [Azure portalından el ile](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json) | Log Analytics çalışma alanından dağıtmak için VM'ler belirtin. |
| | Azure CLI kullanarak veya Azure Kaynak Yöneticisi şablonu yla [Windows](../../virtual-machines/extensions/oms-windows.md) veya [Linux](../../virtual-machines/extensions/oms-linux.md) için Günlük Analytics VM uzantısı | Uzantı, Log Analytics aracısını Azure sanal makinelerine yükler ve bunları mevcut bir Azure Monitor çalışma alanına kaydeder. |
| | [VM'ler için Azure İzleyici](../insights/vminsights-enable-overview.md) | VM'ler için Azure Monitor ile izlemeyi etkinleştirdiğinizde, Log Analytics uzantısını ve aracısını yükler. |
| | [Azure Güvenlik Merkezi Otomatik sağlama](../../security-center/security-center-enable-data-collection.md) | Azure Güvenlik Merkezi, Log Analytics aracısını desteklenen tüm Azure VM'lerinde ve güvenlik açıkları ve tehditleri izlemesini sağlarsanız oluşturulan yeni leri sağlayabilir. Etkinleştirilirse, yüklü bir aracısı olmayan yeni veya varolan herhangi bir VM sağlanacaktır. |
| Hibrit Windows bilgisayarı| [Manuel yükleme](agent-windows.md) | Microsoft İzleme aracısını komut satırından yükleyin. |
| | [Azure Automation DSC](agent-windows.md#install-the-agent-using-dsc-in-azure-automation) | Yüklemeyi Azure Automation DSC ile otomatikleştirin. |
| | [Azure Yığını ile Kaynak Yöneticisi şablonu](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) | Veri merkezinizde Microsoft Azure Yığını'nı dağıttıysanız Azure Kaynak Yöneticisi şablonunu kullanın.| 
| Hibrit Linux bilgisayarı| [Manuel yükleme](../../azure-monitor/learn/quick-collect-linux-computer.md)|Linux aracısını GitHub'da barındırılan bir sarmalayıcı komut dosyası olarak adlandırarak yükleyin. | 
| System Center Operations Manager|[İşlem Yöneticisini Log Analytics ile Tümleştir](../../azure-monitor/platform/om-agents.md) | Bir yönetim grubuna rapor eden Windows bilgisayarlarından toplanan verileri iletmek için Operations Manager ve Azure Monitor günlükleri arasındaki tümleştirmeyi yapılandırın.|  


## <a name="supported-windows-operating-systems"></a>Desteklenen Windows işletim sistemleri

Windows işletim sisteminin aşağıdaki sürümleri resmi olarak Windows aracısı için desteklenir:

* Windows Server 2019
* Windows Server 2008 SP2 (x64), 2008 R2, 2012, 2012 R2, 2016, sürüm 1709 ve 1803
* Windows 7 SP1, Windows 8 Enterprise ve Pro ve Windows 10 Kurumsal ve Pro

>[!NOTE]
>Windows için Log Analytics aracısı sunucu izleme senaryolarını desteklemek üzere tasarlanmış olsa da, sunucu işletim sistemi için yapılandırılan ve optimize edilmiş iş yüklerini desteklemek için Windows istemcisini çalıştırabileceğinizi fark ettik. Aracı Windows istemcisini destekler, ancak izleme çözümlerimiz açıkça belirtilmedikçe istemci izleme senaryolarına odaklanmaz.

## <a name="supported-linux-operating-systems"></a>Desteklenen Linux işletim sistemleri

Bu bölümde desteklenen Linux dağıtımları hakkında ayrıntılı bilgi verilmektedir.

Ağustos 2018'den sonra piyasaya sürülen sürümlerden başlayarak destek modelimizde aşağıdaki değişiklikleri yapıyoruz:  

* İstemci değil, yalnızca sunucu sürümleri desteklenir.  
* Desteği Azure Linux [Onaylı dağıtımlardan](../../virtual-machines/linux/endorsed-distros.md)herhangi biri üzerinde odaklayın. Azure Linux'un Onaylandığı yeni bir dağıtım/sürüm ile Log Analytics Linux aracısı için desteklenmesi arasında bir gecikme olabileceğini unutmayın.
* Tüm küçük sürümler listelenen her ana sürüm için desteklenir.
* Üreticilerinin destek bitiş tarihini geçen sürümler desteklenmez.  
* AMI'nin yeni sürümleri desteklenmez.  
* Yalnızca varsayılan olarak SSL 1.x çalıştıran sürümler desteklenir.

>[!NOTE]
>Şu anda desteklenmeyen ve destek modelimizle uyumlu olmayan bir dağıtım veya sürüm kullanıyorsanız, Microsoft desteğinin çatallı aracı sürümleriyle ilgili yardım sağlamadığını kabul ederek bu repo'yu çatallamanızı öneririz.

* Amazon Linux 2017.09 (x64)
* CentOS Linux 6 (x86/x64) ve 7 (x64)  
* Oracle Linux 6 ve 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) ve 7 (x64)
* Debian GNU/Linux 8 ve 9 (x86/x64)
* Ubuntu 14,04 LTS (x86/x64), 16,04 LTS (x86/x64) ve 18,04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64) ve 15 (x64)

>[!NOTE]
>OpenSSL 1.1.0 yalnızca x86_x64 platformlarda (64 bit) ve OpenSSL 1.x'ten önce herhangi bir platformda desteklenmez.
>

### <a name="agent-prerequisites"></a>Acente ön koşulları

Aşağıdaki tabloda, aracının yükleneceği desteklenen Linux dağıtımları için gereken paketler vurgulanır.

|Gerekli paket |Açıklama |En düşük sürüm |
|-----------------|------------|----------------|
|Glibc |    GNU C Kütüphanesi | 2.5-12 
|Openssl    | OpenSSL Kitaplıkları | 1.0.x veya 1.1.x |
|Curl | cURL web istemcisi | 7.15.5 |
|Python-ctypes | | 
|PAM | Eklenebilir Kimlik Doğrulaması Modülleri | | 

>[!NOTE]
>Syslog mesajlarını toplamak için rsyslog veya syslog-ng gereklidir. Red Hat Enterprise Linux, CentOS ve Oracle Linux sürümü (sysklog) sürüm 5 varsayılan syslog daemon syslog olay koleksiyonu için desteklenmez. Bu dağıtımların bu sürümünden syslog verileri toplamak için, rsyslog daemon yüklü olmalı ve sysklog yerine yapılandırılmalıdır.

## <a name="tls-12-protocol"></a>TLS 1.2 protokolü

Azure Monitor günlüklerine aktarım halindeki verilerin güvenliğini sağlamak için, aracıyı en az Aktarım Katmanı Güvenliği (TLS) 1,2 kullanacak şekilde yapılandırmanızı önemle öneririz. TLS/Secure Sockets Layer'ın (SSL) eski sürümlerinin savunmasız olduğu tespit edilmiştir ve hala geriye dönük uyumluluğa izin vermek için **çalışırken, bunlar önerilmez.**  Daha fazla bilgi için [TLS 1.2'yi kullanarak veri gönderme'yi](data-security.md#sending-data-securely-using-tls-12)güvenli bir şekilde gözden geçirin. 


## <a name="sha-2-code-signing-support-requirement-for-windows"></a>Windows için SHA-2 Kod İmzalama Destek Gereksinimi
Windows aracısı sadece 18 Mayıs 2020 tarihinde SHA-2 imza kullanmaya başlayacaktır. Bu değişiklik, herhangi bir Azure hizmetinin (Azure Monitor, Azure Otomasyonu, Azure Güncelleştirme Yönetimi, Azure Değişiklik İzleme, Azure Güvenlik Merkezi, Azure Sentinel, Windows Defender ATP) bir parçası olarak eski bir işletim sistemi üzerinde Log Analytics aracısını kullanan müşterileri etkileyecektir. Bu değişiklik, aracıyı eski bir işletim sistemi sürümünde çalıştırmadığınız sürece (Windows 7, Windows Server 2008 R2 ve Windows Server 2008) herhangi bir müşteri eylemi gerektirmez. Eski bir işletim sistemi sürümünde çalışan müşterilerin 18 Mayıs 2020'den önce makinelerinde aşağıdaki işlemleri yaptırmaları gerekir veya aracıları Günlük Analizi çalışma alanlarına veri göndermeyi durdurur:

1. İşletim sisteminiz için en son Hizmet Paketini yükleyin. Gerekli hizmet paketi sürümleri şunlardır:
    - Windows 7 SP1
    - Windows Server 2008 SP2
    - Windows Server 2008 R2 SP1

2. [Windows ve WSUS için 2019 SHA-2 Kod İmzalama Desteği gereksiniminde](https://support.microsoft.com/help/4472027/2019-sha-2-code-signing-support-requirement-for-windows-and-wsus) açıklandığı gibi işletim sisteminiz için SHA-2 imzalama Windows güncelleştirmelerini yükleyin
3. Windows aracısının en son sürümüne (sürüm 10.20.18029) güncelleştirin.
4. [TLS 1.2 kullanmak](agent-windows.md#configure-agent-to-use-tls-12)için aracıyı yapılandırmak için önerilir. 


## <a name="network-requirements"></a>Ağ gereksinimleri
Linux ve Windows aracısı, TCP bağlantı noktası 443 üzerinden Azure Monitor hizmetine giden iletişim kurar ve makine Internet üzerinden iletişim kurmak için bir güvenlik duvarı veya proxy sunucusu üzerinden bağlanırsa, ağ yapılandırmasını anlamak için aşağıdaki gereksinimleri gözden geçirin Gerekli. BT güvenlik ilkeleriniz ağdaki bilgisayarların Internet'e bağlanmasına izin vermiyorsa, bir [Log Analytics ağ geçidi](gateway.md) ayarlayabilir ve aracıyı Azure Monitor günlüklerine ağ geçidinden bağlanacak şekilde yapılandırabilirsiniz. Aracı daha sonra yapılandırma bilgilerini alabilir ve çalışma alanınızda etkinleştirdiğiniz veri toplama kurallarına ve izleme çözümlerine bağlı olarak toplanan verileri gönderebilir.

![Log Analytics aracıiletişim diyagramı](./media/log-analytics-agent/log-analytics-agent-01.png)


## <a name="network-firewall-requirements"></a>Ağ güvenlik duvarı gereksinimleri
Aşağıdaki bilgiler, Linux ve Windows aracısının Azure Monitor günlükleriyle iletişim kurması için gereken proxy ve güvenlik duvarı yapılandırma bilgilerini listelemektedir.  

|Aracı Kaynağı|Bağlantı Noktaları |Yön |HTTPS denetlemesini atlama|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Bağlantı Noktası 443 |Giden|Evet |  
|*.oms.opinsights.azure.com |Bağlantı Noktası 443 |Giden|Evet |  
|*.blob.core.windows.net |Bağlantı Noktası 443 |Giden|Evet |  

Azure Kamu için gerekli güvenlik duvarı bilgileri için Bkz. [Azure Kamu yönetimi.](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs) 

Ortamınızdaki runbook'ları veya yönetim çözümlerini kullanmak için Otomasyon hizmetine bağlanmak ve ona kaydolmak için Azure Otomasyon Karma Runbook Worker'ı kullanmayı planlıyorsanız, bu hizmet noktası numarasına ve [Karma Runbook Çalışanı için ağınızı Yapılandırma'da](../../automation/automation-hybrid-runbook-worker.md#network-planning)açıklanan URL'lere erişimi olmalıdır. 

Windows ve Linux aracısı, HTTPS protokolünü kullanarak bir proxy sunucusu veya Log Analytics ağ geçidi aracılığıyla Azure Monitor'a iletişim kurmayı destekler.  Hem anonim hem de temel kimlik doğrulama (kullanıcı adı/parola) desteklenir.  Doğrudan hizmete bağlı Windows aracısı için proxy yapılandırması yükleme sırasında veya Denetim Masası'ndan dağıtım [sırasında](agent-manage.md#update-proxy-settings) veya PowerShell ile belirtilir.  

Linux aracısı için proxy sunucusu, proxy.conf yapılandırma dosyasını değiştirerek yükleme sırasında veya [kurulumdan sonra](agent-manage.md#update-proxy-settings) belirtilir.  Linux aracısı proxy yapılandırma değeri aşağıdaki sözdizimine sahiptir:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Proxy sunucunuz kimlik doğrulamanızı gerektirmiyorsa, Linux aracısı hala sözde bir kullanıcı/parola sağlamayı gerektirir. Bu herhangi bir kullanıcı adı veya parola olabilir.

|Özellik| Açıklama |
|--------|-------------|
|Protokol | https |
|kullanıcı | Proxy kimlik doğrulaması için isteğe bağlı kullanıcı adı |
|password | Proxy kimlik doğrulaması için isteğe bağlı parola |
|proxyhost | Proxy sunucusunun adresi veya FQDN'si/Log Analytics ağ geçidi |
|port | Proxy sunucusu/Log Analytics ağ geçidi için isteğe bağlı bağlantı noktası numarası |

Örneğin, `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Parolanızda " "\@gibi özel karakterler kullanırsanız, değer yanlış ayrıştırıldığı için proxy bağlantı hatası alırsınız.  Bu sorunu çözmek için [URLDecode](https://www.urldecoder.org/)gibi bir aracı kullanarak URL'deki parolayı kodlayın.  



## <a name="next-steps"></a>Sonraki adımlar

* Windows veya Linux sisteminizden veri toplamak için kullanılabilen veri kaynaklarını anlamak için [veri kaynaklarını](agent-data-sources.md) gözden geçirin. 
* Veri kaynaklarından ve çözümlerinden toplanan verileri analiz etmek için [günlük sorguları](../log-query/log-query-overview.md) hakkında bilgi edinin. 
* Azure Monitor'a işlevsellik katan ve Ayrıca Log Analytics çalışma alanına veri toplayan [izleme çözümleri](../insights/solutions.md) hakkında bilgi edinin.

