---
title: Veri kaynaklarını Azure Sentinel'e bağlama | Microsoft Dokümanlar
description: Veri kaynaklarını Azure Sentinel'e nasıl bağlayabilirsiniz öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: angrobe
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: yelevin
ms.openlocfilehash: 9d2d1985b23e1c7f5e0f7d9fd2795bd85e28ace0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "79240089"
---
# <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

Yerleşik Azure Sentinel için öncelikle veri kaynaklarınıza bağlanmanız gerekir. Azure Sentinel, Microsoft çözümleri için kutudan çıkabilen ve Microsoft Tehdit Koruması çözümleri ve Office 365, Azure AD, Azure ATP gibi Microsoft 365 kaynakları ve gerçek zamanlı tümleştirme sağlayan bir dizi bağlayıcıyla birlikte gelir ve Microsoft Cloud App Security ve daha fazlası. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar da vardır. Veri kaynaklarınızı Azure Sentinel'e bağlamak için ortak etkinlik biçimini, Syslog'u veya REST-API'yi de kullanabilirsiniz.  

1. Menüde Veri **bağlayıcılarını**seçin. Bu sayfa, Azure Sentinel'in sağladığı bağlayıcıların tam listesini ve durumlarını görmenizi sağlar. Bağlanmak istediğiniz bağlayıcıyı seçin ve **Bağlayıcıyı Aç sayfasını**seçin. 

   ![Veri toplayıcıları](./media/collect-data/collect-data-page.png)

1. Belirli bağlayıcı sayfasında, tüm ön koşulları yerine getirdiğinden emin olun ve verileri Azure Sentinel'e bağlamak için yönergeleri izleyin. Günlüklerin Azure Sentinel ile eşitlemeye başlaması biraz zaman alabilir. Bağlandıktan sonra, **Alınan Veriler** grafiğinde verilerin bir özetini ve veri türlerinin bağlantı durumunu görürsünüz.

   ![Toplayıcıları bağlayın](./media/collect-data/opened-connector-page.png)
  
1. Azure Sentinel'in belirli veri türü için sağladığı hazır içeriğin listesini almak için **Sonraki adımlar** sekmesini tıklatın.

   ![Veri toplayıcıları](./media/collect-data/data-insights.png)
 

## <a name="data-connection-methods"></a>Veri bağlantısı yöntemleri

Aşağıdaki veri bağlantı yöntemleri Azure Sentinel tarafından desteklenir:

- **Hizmet entegrasyonuna hizmet:**<br> AWS ve Microsoft hizmetleri gibi bazı hizmetler yerel olarak bağlanır, bu hizmetler kutudan çıkarma tümleştirmesi için Azure temelinden yararlanır, aşağıdaki çözümler birkaç tıklamayla bağlanabilir:
    - [Amazon Web Hizmetleri - CloudTrail](connect-aws.md)
    - [Azure Etkinliği](connect-azure-activity.md)
    - [Azure AD denetim günlükleri ve oturum açma](connect-azure-active-directory.md)
    - [Azure AD Kimlik Koruması](connect-azure-ad-Identity-protection.md)
    - [Azure Gelişmiş Tehdit Koruması](connect-azure-atp.md)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Azure Güvenlik Merkezi](connect-azure-security-center.md)
    - [Cloud App Security](connect-cloud-app-security.md)
    - [Etki alanı adı sunucusu](connect-dns.md)
    - [Ofis 365](connect-office-365.md)
    - [Microsoft Defender ATP](connect-microsoft-defender-advanced-threat-protection.md)
    - [Microsoft web uygulaması güvenlik duvarı](connect-microsoft-waf.md)
    - [Windows güvenlik duvarı](connect-windows-firewall.md)
    - [Windows güvenlik olayları](connect-windows-security-events.md)

- **API ile dış çözümler**: Bazı veri kaynakları, bağlı veri kaynağı tarafından sağlanan API'ler kullanılarak bağlanır. Genellikle, çoğu güvenlik teknolojisi, olay günlüklerinin alınabileceği bir API kümesi sağlar. API'ler Azure Sentinel'e bağlanır ve belirli veri türlerini toplar ve Azure Günlük Analizi'ne gönderir. API ile bağlanan cihazlar şunlardır:
    - [Barracuda](connect-barracuda.md)
    - [Barracuda CloudGen Firewall](connect-barracuda-cloudgen-firewall.md)
    - [Citrix Analytics (Güvenlik)](connect-citrix-analytics.md)
    - [F5 BIG-IP](connect-f5-big-ip.md)
    - [Forcepoint DLP](connect-forcepoint-dlp.md)
    - [Squadra Technologies secRMM](connect-squadra-secrmm.md)
    - [Symantec ICDX](connect-symantec.md)
    - [Zimperium](connect-zimperium-mtd.md)


- **Aracı aracılığıyla dış çözümler**: Azure Sentinel, bir aracı aracılığıyla Syslog protokolünü kullanarak gerçek zamanlı günlük akışı gerçekleştirebilen diğer tüm veri kaynaklarına bağlanabilir. <br>Çoğu cihaz, günlük kendisi ve günlük hakkında veri içeren olay iletileri göndermek için Syslog protokolünü kullanır. Günlüklerin biçimi değişir, ancak çoğu cihaz günlük verileri için Ortak Olay Biçimi (CEF) tabanlı biçimlendirmeyi destekler. <br>Log Analytics aracısını temel alan Azure Sentinel aracısı, CEF biçimli günlükleri Log Analytics tarafından yutulabilecek bir biçime dönüştürür. Cihaz türüne bağlı olarak, aracı doğrudan cihaza veya özel bir Linux sunucusuna yüklenir. Linux aracısı UDP üzerinden Syslog daemon olayları alır, ancak bir Linux makine Syslog olaylar yüksek hacimli toplamak için bekleniyor, onlar Syslog daemon gelen ajan ve oradan Log Analytics TCP üzerinden gönderilir.
    - Güvenlik duvarları, yakınlıklar ve uç noktalar:
        - [Denetim Noktası](connect-checkpoint.md)
        - [Cisco ASA](connect-cisco.md)
        - [ExtraHop Reveal(x)](connect-extrahop.md)
        - [F5](connect-f5.md)
        - [Forcepoint ürünleri](connect-forcepoint-casb-ngfw.md)
        - [Fortinet](connect-fortinet.md)
        - [Palo Alto Networks](connect-paloalto.md)
        - [One Identity Safeguard](connect-one-identity.md)
        - [Diğer CEF cihazları](connect-common-event-format.md)
        - [Diğer Syslog cihazları](connect-syslog.md)
        - [Trend Micro Deep Security](connect-trend-micro.md)
        - [Zscaler](connect-zscaler.md)
    - DLP çözümleri
    - [Tehdit istihbarat sağlayıcıları](connect-threat-intelligence.md)
    - [DNS makineleri](connect-dns.md) - doğrudan DNS makinesine monte edilen ajan
    - Linux sunucuları
    - Diğer bulutlar
    
## <a name="agent-connection-options"></a>Aracı bağlantı seçenekleri<a name="agent-options"></a>

Harici cihazınızı Azure Sentinel'e bağlamak için aracının, cihaz la Azure Sentinel arasındaki iletişimi desteklemek için özel bir makinede (VM veya şirket içinde) dağıtılması gerekir. Aracıyı otomatik olarak veya el ile dağıtabilirsiniz. Otomatik dağıtım yalnızca özel makineniz Azure'da oluşturduğunuz yeni bir VM ise kullanılabilir. 


![Azure'da CEF](./media/connect-cef/cef-syslog-azure.png)

Alternatif olarak, aracıyı varolan bir Azure VM'de, başka bir buluttaki bir VM'de veya şirket içi bir makinede el ile dağıtabilirsiniz.

![CEF tesislerinde](./media/connect-cef/cef-syslog-onprem.png)

## <a name="map-data-types-with-azure-sentinel-connection-options"></a>Veri türlerini Azure Sentinel bağlantı seçenekleriyle eşleme


| **Veri türü** | **Bağlanma** | **Veri konektörü mü?** | **Açıklamalar** |
|------|---------|-------------|------|
| AWSCloudTrail | [AWS’ye bağlanma](connect-aws.md) | V | |
| AzureActivity | Azure Etkinlik ve [Etkinlik günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md) [bağlayın](connect-azure-activity.md)| V | |
| Denetim Kayıtları | [Azure AD’yi bağlama](connect-azure-active-directory.md)  | V | |
| SigninLogs | [Azure AD’yi bağlama](connect-azure-active-directory.md)  | V | |
| AzureFirewall |[Azure Tanılama](../firewall/tutorial-diagnostics.md) | V | |
| InformationProtectionLogs_CL  | [Azure Bilgi Koruması raporları](https://docs.microsoft.com/azure/information-protection/reports-aip)<br>[Azure Information Protection’ı bağlama](connect-azure-information-protection.md)  | V | Bu genellikle veri türüne ek olarak **InformationProtectionEvents** işlevini kullanır. Daha fazla bilgi için [raporları değiştirme ve özel sorgular oluşturma](https://docs.microsoft.com/azure/information-protection/reports-aip#how-to-modify-the-reports-and-create-custom-queries)|
| AzureNetworkAnalytics_CL  | [Trafik analitiği Trafik](../network-watcher/traffic-analytics.md) [analitiği](../network-watcher/traffic-analytics.md)  | | |
| CommonSecurityLog  | [CEF'i Bağla](connect-common-event-format.md)  | V | |
| Ofis Etkinliği | [Office 365'i bağlama](connect-office-365.md) | V | |
| Güvenlik Etkinlikleri | [Windows güvenlik olaylarını bağlama](connect-windows-security-events.md)  | V | Güvenli Olmayan Protokoller çalışma kitapları için [bkz.](/azure/sentinel/quickstart-get-visibility#use-built-in-workbooks)  |
| Syslog | [Syslog’u bağlama](connect-syslog.md) | V | |
| Microsoft Web Uygulama Güvenlik Duvarı (WAF) - (AzureDiagnostics) |[Microsoft Web Uygulaması Güvenlik Duvar'ı Bağla](connect-microsoft-waf.md) | V | |
| SymantecICDx_CL | [Symantec'i bağla](connect-symantec.md) | V | |
| Tehdit İstihbarat Göstergesi  | [Tehdit zekasını bağlama](connect-threat-intelligence.md)  | V | |
| VMConnection <br> ServiceMapComputer_CL<br> ServiceMapProcess_CL|  [Azure Monitör hizmet haritası](../azure-monitor/insights/service-map.md)<br>[Azure Monitör VM öngörüleri onboarding](../azure-monitor/insights/vminsights-onboard.md) <br> [Azure Monitör VM öngörülerini etkinleştirme](../azure-monitor/insights/vminsights-enable-overview.md) <br> [Tek VM Biniş Kullanma](../azure-monitor/insights/vminsights-enable-single-vm.md)<br>  [Biniş Via Politikasını Kullanma](../azure-monitor/insights/vminsights-enable-at-scale-policy.md)| X | VM öngörüleri çalışma kitabı  |
| DnsEtkinlikler | [DNS'yi bağla](connect-dns.md) | V | |
| W3CIISLog | [IIS günlüklerini bağla](../azure-monitor/platform/data-sources-iis-logs.md)  | X | |
| WireData | [Tel Verilerini Bağla](../azure-monitor/insights/wire-data.md) | X | |
| WindowsFirewall | [Windows Güvenlik DuvarIna Bağlanma](connect-windows-firewall.md) | V | |
| AADIP Güvenlik Uyarısı  | [Azure AD Kimlik Koruması bağlantısı kurma](connect-azure-ad-identity-protection.md)  | V | |
| AATP Güvenlik Uyarısı  | [Azure ATP'yi bağlama](connect-azure-atp.md) | V | |
| ASC Güvenlik Uyarısı  | [Azure Güvenlik Merkezi’ni bağlama](connect-azure-security-center.md)  | V | |
| MCAS Güvenlik Uyarısı  | [Microsoft Bulut Uygulaması Güvenliğini Bağlayın](connect-cloud-app-security.md)  | V | |
| Güvenlik Uyarısı | | | |
| Sysmon (Olay) | [Bağlantı Sysmon](https://azure.microsoft.com/blog/detecting-in-memory-attacks-with-sysmon-and-azure-security-center)<br> [Windows Etkinliklerini Bağla](../azure-monitor/platform/data-sources-windows-events.md) <br> [Sysmon Parser alın](https://github.com/Azure/Azure-Sentinel/blob/master/Parsers/SysmonParser.txt)| X | Sysmon koleksiyonu varsayılan olarak sanal makinelere yüklenmez. Sysmon Aracısı nasıl yüklenir hakkında daha fazla bilgi için Bkz. [Sysmon](https://docs.microsoft.com/sysinternals/downloads/sysmon). |
| YapılandırmaVerileri  | [VM envanterini otomatikleştirin](../automation/automation-vm-inventory.md)| X | |
| Yapılandırma Değişikliği  | [VM izlemeyi otomatikleştirin](../automation/change-tracking.md) | X | |
| F5 BIG-IP | [F5 BIG-IP'yi bağlama](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel)  | X | |
| McasShadowItReporting  |  | X | |
| Barracuda_CL | [Barracuda’yı bağlama](connect-barracuda.md) | V | |


## <a name="next-steps"></a>Sonraki adımlar

- Azure Sentinel'e başlamak için Microsoft Azure aboneliğine ihtiyacınız var. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- [Verilerinizi Azure Sentinel'e nasıl ekeceklerinizi](quickstart-onboard.md)ve [verilerinizle ve olası tehditlerde görünürlüğü](quickstart-get-visibility.md)nasıl elde edebilirsiniz öğrenin.
