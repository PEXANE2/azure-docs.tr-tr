---
title: Azure Log Analytics VM Uzantısı sorunlarını giderme
description: Windows ve Linux Azure VM 'Leri için Log Analytics VM uzantısıyla ilgili en yaygın sorunların belirtilerini, nedenlerini ve çözümlemesini tanıtın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/06/2019
ms.openlocfilehash: e16531484505f055c1383aff5adb40518719d98a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80054591"
---
# <a name="troubleshooting-the-log-analytics-vm-extension-in-azure-monitor"></a>Azure İzleyici'deki Log Analytics VM uzantısının sorunlarını giderme
Bu makale, Microsoft Azure üzerinde çalışan Windows ve Linux sanal makineleri için Log Analytics VM uzantısıyla karşılaşabileceğiniz sorun giderme konusunda yardım sağlar ve bunları çözmek için olası çözümler önerir.

Uzantının durumunu doğrulamak için Azure portal aşağıdaki adımları gerçekleştirin.

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **sanal makineler**yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Sanal makineler**'i seçin.
3. Sanal makineler listenizde, bulun ve seçin.
3. Sanal makinede, **Uzantılar**' a tıklayın.
4. Listeden Log Analytics uzantısının etkin olup olmadığını denetleyin.  Linux için aracı, **Omsagentforlinux** ve Windows için listelenir. Aracı, **microsoftmonitoringagent**olarak listelenir.

   ![VM uzantısı görünümü](./media/vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Ayrıntıları görüntülemek için uzantıya tıklayın. 

   ![VM Uzantısı ayrıntıları](./media/vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Azure Windows VM Uzantısı sorunlarını giderme

*Microsoft Monitoring Agent* VM Uzantısı yükleme veya raporlama değilse, sorunu gidermek için aşağıdaki adımları uygulayabilirsiniz.

1. [KB 2965986](https://support.microsoft.com/kb/2965986#mt1)' deki adımları kullanarak Azure VM Aracısı 'nın yüklenip düzgün çalışıp çalışmadığını denetleyin.
   * VM Aracısı günlük dosyasını da gözden geçirebilirsiniz`C:\WindowsAzure\logs\WaAppAgent.log`
   * Günlük yoksa, VM Aracısı yüklenmez.
   * [Azure VM aracısını yükler](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. İçindeki Microsoft Monitoring Agent VM Uzantısı günlük dosyalarını gözden geçirin`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Sanal makinenin PowerShell betikleri çalıştırabağlanabildiğinden emin olun
4. C:\Windows\temp üzerinde izinlerin değişmediğinden emin olun
5. Sanal makinede yükseltilmiş bir PowerShell penceresine aşağıdakini yazarak Microsoft Monitoring Agent durumunu görüntüleyin`(New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Microsoft Monitoring Agent kurulum günlük dosyalarını gözden geçirin`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Daha fazla bilgi için bkz. [Windows uzantılarında sorun giderme](../../virtual-machines/extensions/oms-windows.md).

## <a name="troubleshooting-linux-vm-extension"></a>Linux VM Uzantısı sorunlarını giderme
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 
Linux VM Uzantısı *için Log Analytics Aracısı* yükleme veya raporlama değilse, sorunu gidermek için aşağıdaki adımları uygulayabilirsiniz.

1. Uzantı durumu *bilinmiyor* Ise, VM Aracısı günlük dosyasını Inceleyerek Azure VM aracısının yüklenip düzgün çalışıp çalışmadığını denetleyin`/var/log/waagent.log`
   * Günlük yoksa, VM Aracısı yüklenmez.
   * [Linux VM 'lerine Azure VM Aracısı 'nı yükler](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Diğer sağlıksız durumlar için, ve içindeki Linux VM Uzantısı günlük dosyaları Log Analytics aracısını gözden geçirin. `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log``/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Uzantı durumu sağlıklı ise, ancak veriler karşıya yüklenemediğinden, içindeki Linux günlük dosyaları için Log Analytics aracısını gözden geçirin.`/var/opt/microsoft/omsagent/log/omsagent.log`

Daha fazla bilgi için bkz. [Linux uzantılarında sorun giderme](../../virtual-machines/extensions/oms-linux.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure dışındaki bilgisayarlarda barındırılan Linux için Log Analytics aracısıyla ilgili ek sorun giderme kılavuzu için bkz. [azure Log Analytics Linux Aracısı sorunlarını giderme](agent-linux-troubleshoot.md).  
