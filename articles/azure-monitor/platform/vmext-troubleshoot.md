---
title: Azure Izleyici 'de Azure Log Analytics VM Uzantısı sorunlarını giderme | Microsoft Docs
description: Belirtiler, nedenleri ve Log Analytics VM uzantısı ile çözüm en sık karşılaşılan sorunlara yönelik Windows ve Linux Azure Vm'leri için açıklar.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/06/2019
ms.openlocfilehash: 88d76fc0c215653cf732ba7b827d82187d738fd9
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77658481"
---
# <a name="troubleshooting-the-log-analytics-vm-extension-in-azure-monitor"></a>Azure Izleyici 'de Log Analytics VM Uzantısı sorunlarını giderme
Bu makalede, Windows ve Linux'ta Microsoft Azure üzerinde çalışan sanal makineler için Log Analytics VM uzantısı ile karşılaşabilirsiniz ve bunların çözülmesine yönelik olası çözümler önerir hatalarını giderme hakkında Yardım sağlar.

Uzantı durumu doğrulamak için Azure portalında aşağıdaki adımları gerçekleştirin.

1. [Azure portalında](https://portal.azure.com) oturum açın.
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynak listesinde **sanal makineler**yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Sanal makineleri**seçin.
3. Sanal makineler listesinde, bulun ve seçin.
3. Sanal makinede, **Uzantılar**' a tıklayın.
4. Listeden Log Analytics uzantısı etkin'in etkin olup olmadığını denetleyin.  Linux için aracı, **Omsagentforlinux** ve Windows için listelenir. Aracı, **microsoftmonitoringagent**olarak listelenir.

   ![VM uzantısı görüntüle](./media/vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Ayrıntılarını görüntülemek için uzantısına tıklayın. 

   ![VM uzantısı ayrıntıları](./media/vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Azure Windows VM uzantısı sorunlarını giderme

*Microsoft Monitoring Agent* VM Uzantısı yükleme veya raporlama değilse, sorunu gidermek için aşağıdaki adımları uygulayabilirsiniz.

1. [KB 2965986](https://support.microsoft.com/kb/2965986#mt1)' deki adımları kullanarak Azure VM Aracısı 'nın yüklenip düzgün çalışıp çalışmadığını denetleyin.
   * VM Aracısı günlük dosyasını da gözden geçirebilirsiniz `C:\WindowsAzure\logs\WaAppAgent.log`
   * Günlük yoksa, VM aracısı yüklü değil.
   * [Azure VM aracısını yükler](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent` Microsoft Monitoring Agent VM Uzantısı günlük dosyalarını gözden geçirin
3. Sanal makine PowerShell betiklerini çalıştırabilirsiniz emin olun.
4. C:\Windows\temp izinlerini değiştirilmemiştir emin olun.
5. Microsoft Monitoring Agent durumunu, sanal makinede yükseltilmiş bir PowerShell penceresine aşağıdakini yazarak görüntüleyin `(New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs` Microsoft Monitoring Agent kurulum günlük dosyalarını gözden geçirin

Daha fazla bilgi için bkz. [Windows uzantılarında sorun giderme](../../virtual-machines/extensions/oms-windows.md).

## <a name="troubleshooting-linux-vm-extension"></a>Linux VM uzantı sorunlarını giderme
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 
Linux VM Uzantısı *için Log Analytics Aracısı* yükleme veya raporlama değilse, sorunu gidermek için aşağıdaki adımları uygulayabilirsiniz.

1. Uzantı durumu *bilinmiyor* Ise, VM Aracısı günlük dosyasını Inceleyerek Azure VM aracısının yüklenip düzgün çalışıp çalışmadığını denetleyin `/var/log/waagent.log`
   * Günlük yoksa, VM aracısı yüklü değil.
   * [Linux VM 'lerine Azure VM Aracısı 'nı yükler](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Diğer sağlıksız durumlar için, `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` ve `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log` Linux sanal makine uzantısı günlük dosyaları için Log Analytics aracısını gözden geçirin.
3. Uzantı durumu sağlıklı ise, ancak veriler karşıya yüklenemediğinden, `/var/opt/microsoft/omsagent/log/omsagent.log` içindeki Linux günlük dosyaları için Log Analytics aracısını gözden geçirin

Daha fazla bilgi için bkz. [Linux uzantılarında sorun giderme](../../virtual-machines/extensions/oms-linux.md).

## <a name="next-steps"></a>Sonraki adımlar

Azure dışındaki bilgisayarlarda barındırılan Linux için Log Analytics aracısıyla ilgili ek sorun giderme kılavuzu için bkz. [azure Log Analytics Linux Aracısı sorunlarını giderme](agent-linux-troubleshoot.md).  
