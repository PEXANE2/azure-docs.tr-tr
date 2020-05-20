---
title: Veri kaynaklarını Azure Sentinel 'e bağlama | Microsoft Docs
description: Veri kaynaklarını Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: angrobe
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/04/2019
ms.author: yelevin
ms.openlocfilehash: 4060cfe08e91c87467a8ef6801adab6f027473bf
ms.sourcegitcommit: 595cde417684e3672e36f09fd4691fb6aa739733
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2020
ms.locfileid: "83696860"
---
# <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

Azure Sentinel 'e kadar, önce veri kaynaklarınıza bağlanmanız gerekir. Azure Sentinel, Microsoft çözümleri için çok sayıda bağlayıcı sunar ve Microsoft tehdit koruması çözümleri dahil gerçek zamanlı tümleştirme ve Office 365, Azure AD, Azure ATP ve Microsoft Cloud App Security gibi Microsoft 365 kaynaklar sağlar. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Ayrıca, veri kaynaklarınızı Azure Sentinel 'e bağlamak için ortak olay biçimi, syslog veya REST API de kullanabilirsiniz.  

1. Menüsünde, **veri bağlayıcıları**' nı seçin. Bu sayfa, Azure Sentinel 'in sağladığı bağlayıcıların tam listesini ve bunların durumlarını görmenizi sağlar. Bağlanmak istediğiniz bağlayıcıyı seçin ve **bağlayıcı sayfasını aç**' ı seçin. 

   ![Veri toplayıcılar](./media/collect-data/collect-data-page.png)

1. Belirli bağlayıcı sayfasında, tüm önkoşulları karşıladığınızdan emin olun ve verileri Azure Sentinel 'e bağlamak için yönergeleri izleyin. Günlüklerin Azure Sentinel ile eşitlenmeye başlaması biraz zaman alabilir. Bağlandıktan sonra, **alınan veri** grafiğindeki verilerin özetini ve veri türlerinin bağlantı durumunu görürsünüz.

   ![Toplayıcıların bağlantısını yapın](./media/collect-data/opened-connector-page.png)
  
1. Belirli veri türü için Azure Sentinel 'in sağladığı kullanıma hazır içeriğin bir listesini almak için **sonraki adımlar** sekmesine tıklayın.

   ![Veri toplayıcılar](./media/collect-data/data-insights.png)
 

## <a name="data-connection-methods"></a>Veri bağlantısı yöntemleri

Aşağıdaki veri bağlantısı yöntemleri Azure Sentinel tarafından desteklenir:

- **Hizmetten hizmete tümleştirme**:<br> Bazı hizmetler yerel olarak (AWS ve Microsoft Hizmetleri gibi) bağlı olduğundan, bu hizmetler, kullanıma hazır tümleştirme için Azure Foundation 'dan yararlanır ve aşağıdaki çözümler birkaç tıklamayla bağlanabilir:
    - [Amazon Web Services-Cloudtratem](connect-aws.md)
    - [Azure etkinliği](connect-azure-activity.md)
    - [Azure AD denetim günlükleri ve oturum açmalar](connect-azure-active-directory.md)
    - [Azure AD Kimlik Koruması](connect-azure-ad-Identity-protection.md)
    - [Azure Gelişmiş Tehdit Koruması](connect-azure-atp.md)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Azure Güvenlik Merkezi](connect-azure-security-center.md)
    - [Cloud App Security](connect-cloud-app-security.md)
    - [Etki alanı adı sunucusu](connect-dns.md)
    - [Office 365](connect-office-365.md)
    - [Microsoft Defender ATP](connect-microsoft-defender-advanced-threat-protection.md)
    - [Microsoft web uygulaması güvenlik duvarı](connect-microsoft-waf.md)
    - [Windows güvenlik duvarı](connect-windows-firewall.md)
    - [Windows güvenlik olayları](connect-windows-security-events.md)

- **API aracılığıyla dış çözümler**: bazı veri kaynakları, bağlı veri kaynağı tarafından sağlanmış API 'ler kullanılarak bağlanır. Genellikle, çoğu güvenlik teknolojisi, olay günlüklerinin alınabileceği bir API kümesi sağlar. API 'Ler Azure Sentinel 'e bağlanır ve belirli veri türlerini toplayıp Azure Log Analytics 'e gönderir. API aracılığıyla bağlı olan gereçler şunları içerir:
    - [Barracuda](connect-barracuda.md)
    - [Barracuda CloudGen Firewall](connect-barracuda-cloudgen-firewall.md)
    - [Citrix Analytics (Güvenlik)](connect-citrix-analytics.md)
    - [F5 BIG-IP](connect-f5-big-ip.md)
    - [Forcepoint DLP](connect-forcepoint-dlp.md)
    - [Squadra Technologies secRMM](connect-squadra-secrmm.md)
    - [Symantec ICDX](connect-symantec.md)
    - [Zimperium](connect-zimperium-mtd.md)


- **Aracı aracılığıyla dış çözümler**: Azure Sentinel, bir aracı aracılığıyla Syslog protokolünü kullanarak gerçek zamanlı günlük akışı gerçekleştirebilen diğer tüm veri kaynaklarına bağlanabilir. <br>Çoğu gereçleri, günlüğü kapsayan olay iletilerini ve günlük bilgilerini içeren olay iletilerini göndermek için Syslog protokolünü kullanır. Günlüklerin biçimi değişir, ancak çoğu gereçte, günlük verilerine yönelik ortak olay biçimi (CEF) tabanlı biçimlendirme desteklenir. <br>Log Analytics aracısına dayalı Azure Sentinel Aracısı, CEF biçimli günlükleri Log Analytics tarafından kullanılabilecek bir biçime dönüştürür. Gereç türüne bağlı olarak, aracı doğrudan gereç üzerine ya da adanmış bir Linux sunucusuna yüklenir. Linux Aracısı, syslog arka plan programından UDP üzerinden olay alır, ancak bir Linux makinenin yüksek hacimli bir Syslog olayları toplaması bekleniyorsa, bunlar Syslog arka plan programından aracıya ve Log Analytics için Buradan TCP üzerinden gönderilir.
    - Güvenlik duvarları, proxy 'ler ve uç noktalar:
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
    - [Tehdit bilgileri sağlayıcıları](connect-threat-intelligence.md)
    - DNS [makineleri](connect-dns.md) -aracı doğrudan DNS makinesine yüklendi
    - Linux sunucuları
    - Diğer bulutlar
    
## <a name="agent-connection-options"></a>Aracı bağlantı seçenekleri<a name="agent-options"></a>

Dış gerecinizi Azure Sentinel 'e bağlamak için aracının, Gereç ve Azure Sentinel arasındaki iletişimi desteklemesi için adanmış bir makineye (VM veya şirket içi) dağıtılması gerekir. Aracıyı otomatik olarak veya el ile dağıtabilirsiniz. Otomatik dağıtım yalnızca adanmış makineniz Azure 'da oluşturmakta olduğunuz yeni bir VM ise kullanılabilir. 


![Azure 'da CEF](./media/connect-cef/cef-syslog-azure.png)

Alternatif olarak, aracıyı mevcut bir Azure VM 'de, başka bir buluttaki VM 'de veya şirket içi bir makinede el ile dağıtabilirsiniz.

![Şirket içi CEF](./media/connect-cef/cef-syslog-onprem.png)

## <a name="map-data-types-with-azure-sentinel-connection-options"></a>Veri türlerini Azure Sentinel bağlantı seçenekleriyle eşleme


| **Veri türü** | **Bağlanma** | **Veri Bağlayıcısı?** | **Açıklamalar** |
|------|---------|-------------|------|
| Awscses Izi | [AWS’yi bağlama](connect-aws.md) | V | |
| AzureActivity | [Azure etkinliğine bağlanma](connect-azure-activity.md) ve [etkinlik günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)| V | |
| AuditLogs | [Azure AD’yi bağlama](connect-azure-active-directory.md)  | V | |
| Signınlogs | [Azure AD’yi bağlama](connect-azure-active-directory.md)  | V | |
| AzureFirewall |[Azure Tanılama](../firewall/tutorial-diagnostics.md) | V | |
| InformationProtectionLogs_CL  | [Azure Information Protection raporları](https://docs.microsoft.com/azure/information-protection/reports-aip)<br>[Azure Information Protection’ı bağlama](connect-azure-information-protection.md)  | V | Bu genellikle veri türüne ek olarak **ınformationprotectionevents** işlevini kullanır. Daha fazla bilgi için bkz [. raporları değiştirme ve özel sorgular oluşturma](https://docs.microsoft.com/azure/information-protection/reports-aip#how-to-modify-the-reports-and-create-custom-queries)|
| AzureNetworkAnalytics_CL  | [Trafik analitik şeması](../network-watcher/traffic-analytics.md) [Trafik Analizi](../network-watcher/traffic-analytics.md)  | | |
| CommonSecurityLog  | [CEF 'yi bağlama](connect-common-event-format.md)  | V | |
| Officeetkinliği | [Office 365’i bağlama](connect-office-365.md) | V | |
| SecurityEvents | [Windows güvenlik olaylarını bağlama](connect-windows-security-events.md)  | V | Güvenli olmayan protokoller çalışma kitapları için bkz. [güvenli olmayan protokoller çalışma kitabı kurulumu](/azure/sentinel/quickstart-get-visibility#use-built-in-workbooks)  |
| Syslog | [Syslog’u bağlama](connect-syslog.md) | V | |
| Microsoft Web uygulaması güvenlik duvarı (WAF)-(AzureDiagnostics) |[Microsoft Web uygulaması güvenlik duvarını bağlama](connect-microsoft-waf.md) | V | |
| SymantecICDx_CL | [Symantec 'e bağlanma](connect-symantec.md) | V | |
| Threatıntelligenceındicator  | [Tehdit zekasını bağlama](connect-threat-intelligence.md)  | V | |
| VMConnection <br> ServiceMapComputer_CL<br> ServiceMapProcess_CL|  [Azure Izleyici hizmeti Haritası](../azure-monitor/insights/service-map.md)<br>[Azure Izleyici VM öngörüleri ekleme](../azure-monitor/insights/vminsights-onboard.md) <br> [Azure Izleyici VM öngörülerini etkinleştirin](../azure-monitor/insights/vminsights-enable-overview.md) <br> [Tek VM 'Yi görsel taslak kullanma](../azure-monitor/insights/vminsights-enable-single-vm.md)<br>  [Ilke aracılığıyla görsel taslak kullanma](../azure-monitor/insights/vminsights-enable-at-scale-policy.md)| X | VM öngörüleri çalışma kitabı  |
| DnsEvents | [DNS bağlama](connect-dns.md) | V | |
| W3CIISLog | [IIS günlüklerini bağlama](../azure-monitor/platform/data-sources-iis-logs.md)  | X | |
| Iletilen veri | [Kablo verilerini bağlama](../azure-monitor/insights/wire-data.md) | X | |
| WindowsFirewall | [Windows güvenlik duvarını bağlama](connect-windows-firewall.md) | V | |
| AADıP SecurityAlert  | [Azure AD Kimlik Koruması bağlantısı kurma](connect-azure-ad-identity-protection.md)  | V | |
| AATP SecurityAlert  | [Azure ATP'yi bağlama](connect-azure-atp.md) | V | |
| ASC SecurityAlert  | [Azure Güvenlik Merkezi’ni bağlama](connect-azure-security-center.md)  | V | |
| MCAS SecurityAlert  | [Microsoft Cloud App Security Bağlan](connect-cloud-app-security.md)  | V | |
| SecurityAlert | | | |
| Sysmon (olay) | [Sysmon bağlama](https://azure.microsoft.com/blog/detecting-in-memory-attacks-with-sysmon-and-azure-security-center)<br> [Windows olaylarını bağlama](../azure-monitor/platform/data-sources-windows-events.md) <br> [Sysmon ayrıştırıcısına ulaşın](https://github.com/Azure/Azure-Sentinel/blob/master/Parsers/Sysmon/Sysmon-v10.42-Parser.txt)| X | Sysmon koleksiyonu, sanal makinelerde varsayılan olarak yüklü değildir. Sysmon aracısının nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Sysmon](https://docs.microsoft.com/sysinternals/downloads/sysmon). |
| ConfigurationData  | [VM envanterini otomatikleştirme](../automation/automation-vm-inventory.md)| X | |
| ConfigurationChange  | [VM izlemeyi otomatikleştirin](../automation/change-tracking.md) | X | |
| F5 BIG-IP | [F5 BIG-IP'yi bağlama](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel)  | X | |
| McasShadowItReporting  |  | X | |
| Barracuda_CL | [Barracuda’yı bağlama](connect-barracuda.md) | V | |


## <a name="next-steps"></a>Sonraki adımlar

- Azure Sentinel ile çalışmaya başlamak için Microsoft Azure aboneliğine sahip olmanız gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- [Verilerinizi Azure Sentinel 'e](quickstart-onboard.md) [ekleme ve verilerinize ilişkin görünürlük alma ve olası tehditler](quickstart-get-visibility.md)hakkında bilgi edinin.
