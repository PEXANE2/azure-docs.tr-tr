---
title: Azure Otomasyon Güncelleme Yönetimi'nde Windows güncelleştirme aracısı sorunlarını giderme
description: Güncelleştirme Yönetimi çözümlemeyi kullanarak Windows update aracısıyla ilgili sorunları nasıl gidereceğinizi ve gidereceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/16/2020
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: 6983a2ac7ab5fafcb00aee0b72221a8540ea1668
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678981"
---
# <a name="troubleshoot-windows-update-agent-issues"></a>Windows güncelleştirme aracısı sorunları sorun giderme

Güncelleştirme Yönetimi'nde makinenizin hazır (sağlıklı) olarak görünmemesinin birçok nedeni olabilir. Güncelleştirme Yönetimi'nde, altta yatan sorunu belirlemek için bir Karma Runbook Worker aracısının durumunu denetleyebilirsiniz. Bu makalede, Azure portalından Azure makineleri için sorun giderici nin nasıl çalıştırılacak olduğu ve [çevrimdışı senaryoda](#troubleshoot-offline)Azure olmayan makinelerüzerinde nasıl çalıştırılacakaçık.

Bir makine için üç hazırlık durumu şunlardır:

* Hazır - Hybrid Runbook Worker dağıtılır ve en son 1 saatten daha az bir süre önce görülmüştür.
* Bağlantısız - Hybrid Runbook Worker dağıtıldı ve en son 1 saat önce görüldü.
* Yapılandırılmamış - Karma Runbook Worker bulunamadı veya onboarding tamamlanmadı.

> [!NOTE]
> Azure portalının gösterdiği yle bir makinenin geçerli durumu arasında küçük bir gecikme olabilir.

## <a name="start-the-troubleshooter"></a>Sorun gidericiyi başlatın

Azure makineleri için, portaldaki **Temsilciye Hazırlık Durumunu Güncelleştir** sütunundaki **Sorun Giderme** bağlantısını tıklattığınızda Sorun Giderme Durumu Temsilcisi sayfası açılır. Azure olmayan makineler için bağlantı sizi bu makaleye getirir. Azure olmayan bir makinenin sorun gidermesini sağlamak için [çevrimdışı yönergeleri](#troubleshoot-offline) görün.

![Sanal makinelerin yönetim listesini güncelleştirme](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Hibrit Runbook Worker'ın durumunu kontrol etmek için VM çalışıyor olmalıdır. VM çalışmıyorsa, **VM başlat** düğmesi görüntülenir.

Sorun Giderme Aracısı sayfasında, sorun gidericiyi başlatmak için **Denetimleri Çalıştır'ı** seçin. Sorun giderici, bağımlılıkları doğrulamak için makinede bir komut dosyası çalıştırmak için [Çalıştır Komutu](../../virtual-machines/windows/run-command.md) kullanır. Sorun giderici tamamlandığında, denetimlerin sonucunu döndürür.

![Sorun Giderme Durumu Güncelle Aracısı sayfası](../media/update-agent-issues/troubleshoot-page.png)

Sonuçlar hazır olduklarında sayfada gösterilir. Denetim bölümleri her çeke nelerin eklenmiş olduğunu gösterir.

![Sorun Giderme Güncelleştirme Aracısı denetimleri](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Önkoşul denetimleri

### <a name="operating-system"></a>İşletim sistemi

İşletim sistemi denetimi, Karma Runbook Worker'ın aşağıdaki işletim sistemlerinden birini çalıştırıp çalıştırmadığını doğrular:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2012 ve sonrası |.NET Framework 4.6 veya sonraki gereklidir. ([.NET Çerçevesini İndir )](/dotnet/framework/install/guide-for-developers)<br/> Windows PowerShell 5.1 gereklidir.  ([Windows Yönetim Çerçevesi 5.1'i karşıdan yükleyin](https://www.microsoft.com/download/details.aspx?id=54616))        |

### <a name="net-462"></a>.NET 4.6.2

.NET Framework denetimi, sistemin minimum [.NET Framework 4.6.2](https://www.microsoft.com/en-us/download/details.aspx?id=53345) yüklü olduğunu doğrular.

### <a name="wmf-51"></a>WMF 5.1

WMF denetimi, sistemin Windows Yönetim Çerçevesi 'nin (WMF) gerekli sürümüne sahip olduğunu doğrular - [Windows Management Framework 5.1.](https://www.microsoft.com/download/details.aspx?id=54616)

### <a name="tls-12"></a>TLS 1.2

Bu denetim, iletişimlerinizi şifrelemek için TLS 1.2 kullanıp kullanmadığınızı belirler. TLS 1.0 artık platform tarafından desteklenmiş. Müşterilerin Güncelleme Yönetimi ile iletişim kurmak için TLS 1.2 kullanmalarını öneririz.

## <a name="connectivity-checks"></a>Bağlantı denetimleri

### <a name="registration-endpoint"></a>Kayıt bitiş noktası

Bu denetim, aracının aracı hizmetiyle düzgün bir şekilde iletişim kurup kuramayacağını belirler.

Proxy ve güvenlik duvarı yapılandırmaları, Karma Runbook Worker aracısının kayıt bitiş noktasıyla iletişim kurmasına izin vermelidir. Açılacak adresler ve bağlantı noktaları listesi [için, Karma Çalışanlar için Ağ planlamasına](../automation-hybrid-runbook-worker.md#network-planning)bakın.

### <a name="operations-endpoint"></a>İşlemler bitiş noktası

Bu denetim, aracının İş Çalıştırma Zamanı Veri Hizmeti ile düzgün iletişim kurup kuramayacağını belirler.

Proxy ve güvenlik duvarı yapılandırmaları, Karma Runbook Worker aracısının İş Çalıştırma Zamanı Veri Hizmeti ile iletişim kurmasına izin vermelidir. Açılacak adresler ve bağlantı noktaları listesi [için, Karma Çalışanlar için Ağ planlamasına](../automation-hybrid-runbook-worker.md#network-planning)bakın.

## <a name="vm-service-health-checks"></a>VM hizmeti sağlık kontrolleri

### <a name="monitoring-agent-service-status"></a>Temsilci hizmet durumunu izleme

Bu denetim, Windows için Log Analytics`healthservice`aracısının ( ) makinede çalışıyor mu olduğunu belirler. Hizmeti sorun giderme hakkında daha fazla bilgi edinmek [için Windows için Log Analytics aracısının çalışmıyor'](hybrid-runbook-worker.md#mma-not-running)a bakın.

Windows için Log Analytics aracısını yeniden yüklemek [için Windows için Log Analytics aracısını yükleyin ve Windows için Log Analytics aracısını yapılandırın.](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows)

### <a name="monitoring-agent-service-events"></a>Aracı servis olaylarını izleme

Bu denetim, son 24 saat içinde makinedeki Azure İşlemleri Yöneticisi günlüğünde 4502 olayın görünüp görünmediğini belirler.

Bu olay hakkında daha fazla bilgi edinmek için bu etkinliğin [sorun giderme kılavuzuna](hybrid-runbook-worker.md#event-4502) bakın.

## <a name="access-permissions-checks"></a>Erişim izinleri denetimleri

### <a name="machinekeys-folder-access"></a>MachineKeys klasör erişimi

Crypto klasörerişim denetimi, Yerel Sistem Hesabının C:\ProgramData\Microsoft\Crypto\RSA erişimi olup olmadığını belirler.

## <a name="troubleshoot-offline"></a><a name="troubleshoot-offline"></a>Sorun giderme çevrimdışı

Komut dosyasını yerel olarak çalıştırarak Karma Runbook Çalışanı'ndaki sorun gidericiyi çevrimdışı olarak kullanabilirsiniz. Komut dosyası, [Troubleshoot-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration), PowerShell Galerisi'nde alabilirsiniz. Komut dosyasını çalıştırmak için WMF 4.0 veya daha büyük yüklü olmalıdır. PowerShell'in en son sürümünü indirmek için [PowerShell'in çeşitli sürümlerini yükleme bölümüne](https://docs.microsoft.com/powershell/scripting/install/installing-powershell)bakın.

Bu komut dosyasının çıktısı aşağıdaki örnek gibi görünür:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.2.9200 (Windows Server 2012) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .NET Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             :
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Log Analytics for Windows service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Log Analytics agent for Windows, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Log Analytics agent for Windows service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Log Analytics agent for Windows, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Sonraki adımlar

Karma Runbook Çalışanlarınızla ilgili daha fazla sorunu gidermek için [Bkz.](hybrid-runbook-worker.md)
