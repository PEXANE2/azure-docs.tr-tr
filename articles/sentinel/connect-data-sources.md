---
title: Veri kaynaklarını Azure Sentinel 'e bağlama | Microsoft Docs
description: Microsoft 365 Defender (eski adıyla Microsoft tehdit koruması), Microsoft 365 ve Office 365, Azure AD, ATP ve Cloud App Security gibi veri kaynaklarını Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/08/2020
ms.author: yelevin
ms.openlocfilehash: 4618bb4ba65c48808bc738b51c90a6fd04f0eca3
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89659629"
---
# <a name="connect-data-sources"></a>Veri kaynaklarını bağlama

Azure Sentinel etkinleştirildikten sonra yapmanız gereken ilk şey, veri kaynaklarınızı bağlamak. Azure Sentinel, Microsoft çözümleri için çok sayıda bağlayıcı sunar ve Microsoft 365 Defender (eski adıyla Microsoft tehdit koruması) çözümleri, Microsoft 365 kaynakları (Office 365 dahil), Azure AD, kimlik için Microsoft Defender (eskiden Azure ATP), Microsoft Cloud App Security ve daha fazlasını içeren gerçek zamanlı tümleştirme sağlar. Ayrıca, Microsoft dışı çözümler için daha geniş güvenlik ekosistemine yerleşik bağlayıcılar vardır. Veri kaynaklarınızı Azure Sentinel 'e bağlamak için ortak olay biçimi (CEF), syslog veya REST API 'yi de kullanabilirsiniz.

1. Menüsünde, **veri bağlayıcıları**' nı seçin. Bu sayfa, Azure Sentinel 'in sağladığı bağlayıcıların tam listesini ve bunların durumlarını görmenizi sağlar. Bağlanmak istediğiniz bağlayıcıyı seçin ve **bağlayıcı sayfasını aç**' ı seçin. 

   ![Veri bağlayıcıları Galerisi](./media/collect-data/collect-data-page.png)

1. Belirli bağlayıcı sayfasında, tüm önkoşulları karşıladığınızdan emin olun ve verileri Azure Sentinel 'e bağlamak için yönergeleri izleyin. Günlüklerin Azure Sentinel ile eşitlenmeye başlaması biraz zaman alabilir. Bağlandıktan sonra, **alınan veri** grafiğindeki verilerin özetini ve veri türlerinin bağlantı durumunu görürsünüz.

   ![Veri bağlayıcıları yapılandırma](./media/collect-data/opened-connector-page.png)
  
1. Belirli veri türü için Azure Sentinel 'in sağladığı kullanıma hazır içeriğin bir listesini almak için **sonraki adımlar** sekmesine tıklayın.

   ![Bağlayıcılar için sonraki adımlar](./media/collect-data/data-insights.png)
 
## <a name="data-connection-methods"></a>Veri bağlantısı yöntemleri

Aşağıdaki veri bağlantısı yöntemleri Azure Sentinel tarafından desteklenir:

- **Hizmetten hizmete tümleştirme**:<br> Bazı hizmetler yerel olarak (AWS ve Microsoft Hizmetleri gibi) bağlı olduğundan, bu hizmetler, kullanıma hazır tümleştirme için Azure Foundation 'dan yararlanır ve aşağıdaki çözümler birkaç tıklamayla bağlanabilir:
    - [Amazon Web Services-Cloudtratem](connect-aws.md)
    - [Azure etkinliği](connect-azure-activity.md)
    - [Azure Active Directory](connect-azure-active-directory.md) -denetim günlükleri ve oturum açma günlükleri
    - [Azure AD Kimlik Koruması](connect-azure-ad-Identity-protection.md)
    - [Kimlik Için Microsoft Defender](connect-azure-atp.md) (eski adıyla Azure Gelişmiş tehdit koruması)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Azure Defender](connect-azure-security-center.md) (eski adıyla Azure Güvenlik Merkezi)
    - [Cloud App Security](connect-cloud-app-security.md)
    - [Etki alanı adı sunucusu](connect-dns.md)
    - [Office 365](connect-office-365.md)
    - [Uç nokta Için Microsoft Defender](connect-microsoft-defender-advanced-threat-protection.md) (eski adıyla Microsoft Defender Gelişmiş tehdit koruması)
    - [Microsoft web uygulaması güvenlik duvarı](connect-microsoft-waf.md)
    - [Windows güvenlik duvarı](connect-windows-firewall.md)
    - [Windows güvenlik olayları](connect-windows-security-events.md)

- **API aracılığıyla dış çözümler**: bazı veri kaynakları, bağlı veri kaynağı tarafından sağlanmış API 'ler kullanılarak bağlanır. Genellikle, çoğu güvenlik teknolojisi, olay günlüklerinin alınabileceği bir API kümesi sağlar. API 'Ler Azure Sentinel 'e bağlanır ve belirli veri türlerini toplayıp Azure Log Analytics 'e gönderir. API aracılığıyla bağlı olan gereçler şunları içerir:
    
    - [Alcide kAudit](connect-alcide-kaudit.md)
    - [Barracuda WAF](connect-barracuda.md)
    - [Barracuda CloudGen Firewall](connect-barracuda-cloudgen-firewall.md)
    - [Citrix Analytics (Güvenlik)](connect-citrix-analytics.md)
    - [F5 BIG-IP](connect-f5-big-ip.md)
    - [Forcepoint DLP](connect-forcepoint-dlp.md)
    - [Perimeter 81 günlükleri](connect-perimeter-81-logs.md)
    - [Squadra Technologies secRMM](connect-squadra-secrmm.md)
    - [Symantec ICDX](connect-symantec.md)
    - [Zimperium](connect-zimperium-mtd.md)


- **Aracı aracılığıyla dış çözümler**: Azure Sentinel, syslog protokolünü kullanarak gerçek zamanlı günlük akışı gerçekleştirebilen başka bir veri kaynağına aracı aracılığıyla bağlanabilir.

    Çoğu gereçleri, günlüğü kapsayan olay iletilerini ve günlük bilgilerini içeren olay iletilerini göndermek için Syslog protokolünü kullanır. Günlüklerin biçimi değişir, ancak çoğu gereçte günlük verileri için CEF tabanlı biçimlendirme desteklenir. 

    Aslında Log Analytics aracı olan Azure Sentinel Aracısı, CEF biçimli günlükleri Log Analytics tarafından kullanılabilecek bir biçime dönüştürür. Gereç türüne bağlı olarak, aracı doğrudan gereç üzerine ya da ayrılmış bir Linux tabanlı günlük ileticisine yüklenir. Linux Aracısı, syslog arka plan programından UDP üzerinden olay alır, ancak bir Linux makinenin yüksek hacimli bir Syslog olayları toplaması bekleniyorsa, bunlar Syslog arka plan programından aracıya ve Log Analytics için Buradan TCP üzerinden gönderilir.

    - **Güvenlik duvarları, proxy 'ler ve uç noktalar:**
        - [AI Vectra Detect](connect-ai-vectra-detect.md)
        - [Denetim Noktası](connect-checkpoint.md)
        - [Cisco ASA](connect-cisco.md)
        - [ExtraHop Reveal(x)](connect-extrahop.md)
        - [F5 ASM](connect-f5.md)
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
| Awscses Izi | [AWS’yi bağlama](connect-aws.md) | &#10003; | |
| AzureActivity | [Azure etkinliğine bağlanma](connect-azure-activity.md) ve [etkinlik günlüklerine genel bakış](../azure-monitor/platform/platform-logs-overview.md)| &#10003; | |
| AuditLogs | [Azure AD’yi bağlama](connect-azure-active-directory.md)  | &#10003; | |
| Signınlogs | [Azure AD’yi bağlama](connect-azure-active-directory.md)  | &#10003; | |
| AzureFirewall |[Azure Tanılama](../firewall/tutorial-diagnostics.md) | &#10003; | |
| InformationProtectionLogs_CL  | [Azure Information Protection raporları](https://docs.microsoft.com/azure/information-protection/reports-aip)<br>[Azure Information Protection’ı bağlama](connect-azure-information-protection.md)  | &#10003; | Bu genellikle veri türüne ek olarak **ınformationprotectionevents** işlevini kullanır. Daha fazla bilgi için bkz [. raporları değiştirme ve özel sorgular oluşturma](https://docs.microsoft.com/azure/information-protection/reports-aip#how-to-modify-the-reports-and-create-custom-queries)|
| AzureNetworkAnalytics_CL  | [Trafik analitik şeması](../network-watcher/traffic-analytics.md) [Trafik Analizi](../network-watcher/traffic-analytics.md)  | | |
| CommonSecurityLog  | [CEF 'yi bağlama](connect-common-event-format.md)  | &#10003; | |
| Officeetkinliği | [Office 365’i bağlama](connect-office-365.md) | &#10003; | |
| SecurityEvents | [Windows güvenlik olaylarını bağlama](connect-windows-security-events.md)  | &#10003; | Güvenli olmayan protokoller çalışma kitapları için bkz. [güvenli olmayan protokoller çalışma kitabı kurulumu](/azure/sentinel/quickstart-get-visibility#use-built-in-workbooks)  |
| Syslog | [Syslog’u bağlama](connect-syslog.md) | &#10003; | |
| Microsoft Web uygulaması güvenlik duvarı (WAF)-(AzureDiagnostics) |[Microsoft Web uygulaması güvenlik duvarını bağlama](connect-microsoft-waf.md) | &#10003; | |
| SymantecICDx_CL | [Symantec 'e bağlanma](connect-symantec.md) | &#10003; | |
| Threatıntelligenceındicator  | [Tehdit zekasını bağlama](connect-threat-intelligence.md)  | &#10003; | |
| VMConnection <br> ServiceMapComputer_CL<br> ServiceMapProcess_CL|  [Azure Izleyici hizmeti Haritası](../azure-monitor/insights/service-map.md)<br>[Azure Izleyici VM öngörüleri ekleme](../azure-monitor/insights/vminsights-onboard.md) <br> [Azure Izleyici VM öngörülerini etkinleştirin](../azure-monitor/insights/vminsights-enable-overview.md) <br> [Tek VM 'Yi görsel taslak kullanma](../azure-monitor/insights/vminsights-enable-single-vm.md)<br>  [Ilke aracılığıyla görsel taslak kullanma](../azure-monitor/insights/vminsights-enable-at-scale-policy.md)| &#10007; | VM öngörüleri çalışma kitabı  |
| DnsEvents | [DNS bağlama](connect-dns.md) | &#10003; | |
| W3CIISLog | [IIS günlüklerini bağlama](../azure-monitor/platform/data-sources-iis-logs.md)  | &#10007; | |
| Iletilen veri | [Kablo verilerini bağlama](../azure-monitor/insights/wire-data.md) | &#10007; | |
| WindowsFirewall | [Windows güvenlik duvarını bağlama](connect-windows-firewall.md) | &#10003; | |
| AADıP SecurityAlert  | [Azure AD Kimlik Koruması bağlantısı kurma](connect-azure-ad-identity-protection.md)  | &#10003; | |
| AATP SecurityAlert  | [Kimlik Için Microsoft Defender 'ı bağlama](connect-azure-atp.md) (eski ADıYLA Azure ATP) | &#10003; | |
| ASC SecurityAlert  | [Azure Defender 'ı bağlama](connect-azure-security-center.md) (eski adıyla Azure Güvenlik Merkezi)  | &#10003; | |
| MCAS SecurityAlert  | [Microsoft Cloud App Security Bağlan](connect-cloud-app-security.md)  | &#10003; | |
| SecurityAlert | | | |
| Sysmon (olay) | [Sysmon bağlama](https://azure.microsoft.com/blog/detecting-in-memory-attacks-with-sysmon-and-azure-security-center)<br> [Windows olaylarını bağlama](../azure-monitor/platform/data-sources-windows-events.md) <br> [Sysmon ayrıştırıcısına ulaşın](https://github.com/Azure/Azure-Sentinel/blob/master/Parsers/Sysmon/Sysmon-v10.42-Parser.txt)| &#10007; | Sysmon koleksiyonu, sanal makinelerde varsayılan olarak yüklü değildir. Sysmon aracısının nasıl yükleneceği hakkında daha fazla bilgi için bkz. [Sysmon](https://docs.microsoft.com/sysinternals/downloads/sysmon). |
| ConfigurationData  | [VM envanterini otomatikleştirme](../automation/automation-vm-inventory.md)| &#10007; | |
| ConfigurationChange  | [VM izlemeyi otomatikleştirin](../automation/change-tracking.md) | &#10007; | |
| F5 BIG-IP | [F5 BIG-IP'yi bağlama](https://devcentral.f5.com/s/articles/Integrating-the-F5-BIGIP-with-Azure-Sentinel)  | &#10007; | |
| McasShadowItReporting  |  | &#10007; | |
| Barracuda_CL | [Barracuda’yı bağlama](connect-barracuda.md) | &#10003; | |


## <a name="next-steps"></a>Sonraki adımlar

- Azure Sentinel ile çalışmaya başlamak için Microsoft Azure aboneliğine sahip olmanız gerekir. Aboneliğiniz yoksa [ücretsiz deneme sürümü](https://azure.microsoft.com/free/) için kaydolabilirsiniz.
- [Verilerinizi Azure Sentinel 'e](quickstart-onboard.md) [ekleme ve verilerinize ilişkin görünürlük alma ve olası tehditler](quickstart-get-visibility.md)hakkında bilgi edinin.
