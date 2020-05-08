---
title: Azure Automation Güncelleştirme Yönetimi Windows Update Aracısı sorunlarını giderme
description: Güncelleştirme Yönetimi çözümünü kullanarak Windows Update Aracısı ile ilgili sorunları nasıl giderebileceğinizi ve çözeceğinizi öğrenin.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 01/16/2020
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: 1b4467128fae3fd71a6e588e3c05d287c153e168
ms.sourcegitcommit: a6d477eb3cb9faebb15ed1bf7334ed0611c72053
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82927896"
---
# <a name="troubleshoot-windows-update-agent-issues"></a>Windows Update Aracısı sorunlarını giderme

Güncelleştirme Yönetimi ' de makinenizin (sağlıklı) olarak görünmesinin pek çok nedeni olabilir. Temeldeki sorunu tespit etmek için bir Windows karma Runbook Worker aracısının sistem durumunu kontrol edebilirsiniz. Bir makineye yönelik üç hazırlık durumu aşağıda verilmiştir:

* Hazırlanıyor: karma Runbook Worker dağıtıldı ve en son bir saatten önce görüldü.
* Bağlantısı kesik: karma runbook çalışanı dağıtıldı ve en son bir saat önce görüldü.
* Yapılandırılmadı: karma Runbook Worker bulunamadı veya ekleme bitmedi.

> [!NOTE]
> Azure portal gösterdiği ve makinenin geçerli durumu arasında hafif bir gecikme olabilir.

Bu makalede, Azure makineler için sorun gidericinin Azure portal, [çevrimdışı senaryoda](#troubleshoot-offline)Azure dışı makinelerden nasıl çalıştırılacağı açıklanmaktadır. Sorun giderici artık Windows Server Update Services (WSUS) ve oto indirme ve yükleme anahtarları için denetimler içerir.

> [!NOTE]
> Sorun giderici betiği, bir proxy sunucusu yapılandırılmışsa trafiği Şu anda yönlendirmez.

## <a name="start-the-troubleshooter"></a>Sorun gidericiyi Başlat

Azure makinelerinde, portalda güncelleştirme **Aracısı hazırlığı** sütununda **sorun gider** bağlantısını seçerek **Güncelleştirme Aracısı sorunlarını giderme** sayfasını başlatabilirsiniz. Azure dışı makineler için bağlantı sizi bu makaleye getirir. Azure olmayan bir makinede sorun gidermeye yönelik [çevrimdışı yönergelere](#troubleshoot-offline) bakın.

![Sanal makinelerin Güncelleştirme Yönetimi listesinin ekran görüntüsü](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> Karma Runbook Worker 'ın durumunu denetlemek için VM 'nin çalışıyor olması gerekir. VM çalışmıyorsa **VM 'Yi Başlat** düğmesi görüntülenir.

**Güncelleştirme Aracısı sorunlarını giderme** sayfasında, sorun gidericiyi başlatmak Için **denetimleri Çalıştır** ' ı seçin. Sorun giderici, bağımlılıkları doğrulamak için makinede bir betiği çalıştırmak üzere [Run komutunu](../../virtual-machines/windows/run-command.md) kullanır. Sorun giderici tamamlandığında, denetimlerin sonucunu döndürür.

![Güncelleştirme Aracısı sorunlarını giderme sayfasının ekran görüntüsü](../media/update-agent-issues/troubleshoot-page.png)

Sonuçlar, varsa sayfada gösterilir. Denetimler bölümünde her bir denetimin içerdiği özellikler gösterilir.

![Güncelleştirme Aracısı denetimlerinin sorun giderme ekran görüntüsü](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Önkoşul denetimleri

### <a name="operating-system"></a>İşletim sistemi

İşletim sistemi denetimi, hibrit Runbook Worker 'ın şu işletim sistemlerinden birini çalıştırıp çalıştırmadığını doğrular:

|İşletim sistemi  |Notlar  |
|---------|---------|
|Windows Server 2012 ve üzeri |.NET Framework 4,6 veya üzeri gereklidir. ([.NET Framework indirin](/dotnet/framework/install/guide-for-developers).)<br/> Windows PowerShell 5,1 gereklidir.  ([Windows Management Framework 5,1](https://www.microsoft.com/download/details.aspx?id=54616)' i indirin.)        |

### <a name="net-462"></a>.NET 4.6.2

.NET Framework denetimi sistemin en az [.NET Framework 4.6.2](https://www.microsoft.com/en-us/download/details.aspx?id=53345) yüklü olduğunu doğrular.

### <a name="wmf-51"></a>WMF 5.1

WMF Check, sistemin gerekli Windows Management Framework (WMF) sürümüne sahip olduğunu doğrular: [Windows Management framework 5,1](https://www.microsoft.com/download/details.aspx?id=54616).

### <a name="tls-12"></a>TLS 1.2

Bu denetim, iletişimlerinizi şifrelemek için TLS 1,2 kullanıp kullanmayacağınızı belirler. TLS 1,0 artık platform tarafından desteklenmiyor. Güncelleştirme Yönetimi ile iletişim kurmak için TLS 1,2 kullanın.

## <a name="connectivity-checks"></a>Bağlantı denetimleri

### <a name="registration-endpoint"></a>Kayıt uç noktası

Bu denetim aracının Aracı hizmetiyle düzgün şekilde iletişim kurup kuramayacağını belirler.

Proxy ve güvenlik duvarı yapılandırmalarının, karma Runbook Worker aracısının kayıt uç noktasıyla iletişim kurmasına izin verilmelidir. Açılacak adreslerin ve bağlantı noktalarının listesi için bkz. [karma çalışanlar Için ağ planlaması](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="operations-endpoint"></a>İşlemler uç noktası

Bu denetim, aracının Iş çalışma zamanı veri hizmetiyle düzgün şekilde iletişim kurup kuramayacağını belirler.

Proxy ve güvenlik duvarı yapılandırmalarının, karma Runbook Worker aracısının Iş çalışma zamanı veri hizmetiyle iletişim kurmasına izin verilmelidir. Açılacak adreslerin ve bağlantı noktalarının listesi için bkz. [karma çalışanlar Için ağ planlaması](../automation-hybrid-runbook-worker.md#network-planning).

## <a name="vm-service-health-checks"></a>VM hizmeti durum denetimleri

### <a name="monitoring-agent-service-status"></a>İzleme Aracısı hizmet durumu

Bu denetim, makinede Windows (`healthservice`) için Log Analytics aracısının çalışıp çalışmadığını belirler. Hizmette sorun giderme hakkında daha fazla bilgi edinmek için bkz. [Windows için Log Analytics Aracısı çalışmıyor](hybrid-runbook-worker.md#mma-not-running).

Windows için Log Analytics Aracısı 'nı yeniden yüklemek için, bkz. [Windows için Log Analytics aracısını yükleme ve yapılandırma](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows).

### <a name="monitoring-agent-service-events"></a>Aracı hizmeti olaylarını izleme

Bu denetim, son 24 saat içindeki makinedeki Azure Operations Manager günlüğünde herhangi bir 4502 olayının görünüp görünmeyeceğini belirler.

Bu olay hakkında daha fazla bilgi edinmek için bu olayla ilgili [sorun giderme kılavuzuna](hybrid-runbook-worker.md#event-4502) bakın.

## <a name="access-permissions-checks"></a>Erişim izinleri denetimleri

### <a name="crypto-folder-access"></a>Şifre klasörü erişimi

Şifreleme klasörü erişim denetimi, yerel sistem hesabının C:\programdata\microsoft\crypto\rsaa erişimi olup olmadığını belirler.

## <a name="troubleshoot-offline"></a><a name="troubleshoot-offline"></a>Çevrimdışı sorun giderme

Betiği yerel olarak çalıştırarak karma Runbook Worker 'daki sorun gidericiyi çevrimdışı olarak kullanabilirsiniz. PowerShell Galerisi şu betiği alın: [sorun giderme-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration). Betiği çalıştırmak için WMF 4,0 veya sonraki bir sürümünün yüklü olması gerekir. PowerShell 'in en son sürümünü indirmek için bkz. [PowerShell 'in çeşitli sürümlerini yükleme](https://docs.microsoft.com/powershell/scripting/install/installing-powershell).

Bu Betiğin çıktısı aşağıdaki örneğe benzer şekilde görünür:

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

[Karma runbook çalışanları sorunlarını giderme](hybrid-runbook-worker.md)
