---
title: Sorun Giderme Azure Günlük Analizi VM Uzantısı
description: Windows ve Linux Azure VM'leri için Log Analytics VM uzantısı ile en sık karşılaşılan sorunların belirtilerini, nedenlerini ve çözümünü açıklayın.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 06/06/2019
ms.openlocfilehash: e16531484505f055c1383aff5adb40518719d98a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80054591"
---
# <a name="troubleshooting-the-log-analytics-vm-extension-in-azure-monitor"></a>Azure İzleyici'deki Log Analytics VM uzantısının sorunlarını giderme
Bu makalede, Microsoft Azure'da çalışan Windows ve Linux sanal makineler için Log Analytics VM uzantısı ile karşılaşabileceğiniz sorun giderme hataları sağlar ve bunları çözmek için olası çözümler önerir.

Uzantının durumunu doğrulamak için Azure portalından aşağıdaki adımları gerçekleştirin.

1. [Azure portalında](https://portal.azure.com)oturum açın.
2. Azure portalında **Tüm hizmetler**’e tıklayın. Kaynaklar **listesinde, sanal makineleri**yazın. Yazmaya başladığınızda liste, girişinize göre filtrelenir. **Sanal makineler**'i seçin.
3. Sanal makineler listenizde bulun ve seçin.
3. Sanal makinede **Uzantılar'ı**tıklatın.
4. Listeden, Günlük Analizi uzantısıetkin olup olmadığını kontrol edin.  Linux için, aracı **OMSAgentforLinux** olarak listelenir ve Windows için, aracı **MicrosoftMonitoringAgent**olarak listelenir.

   ![VM Uzantı Görünümü](./media/vmext-troubleshoot/log-analytics-vmview-extensions.png)

4. Ayrıntıları görüntülemek için uzantıyı tıklatın. 

   ![VM Uzantı Detayları](./media/vmext-troubleshoot/log-analytics-vmview-extensiondetails.png)

## <a name="troubleshooting-azure-windows-vm-extension"></a>Azure Windows VM uzantısı sorun giderme

Microsoft *Monitoring Agent* VM uzantısı yüklenmiyor veya raporlama değilse, sorunu gidermek için aşağıdaki adımları gerçekleştirebilirsiniz.

1. [KB 2965986'daki](https://support.microsoft.com/kb/2965986#mt1)adımları kullanarak Azure VM aracısının yüklü olup olmadığını ve doğru çalışıp çalışmayabilir kontrol edin.
   * VM aracısı günlük dosyasını da inceleyebilirsiniz`C:\WindowsAzure\logs\WaAppAgent.log`
   * Günlük yoksa, VM aracısı yüklü değil.
   * [Azure VM Aracısını Yükleme](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Microsoft Monitoring Agent VM uzantılı günlük dosyalarını gözden geçirin`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
3. Sanal makinenin PowerShell komut dosyalarını çalıştırabilmesini sağlayın
4. C:\Windows\temp üzerindeki izinlerin değiştirilmediğinden emin olun
5. Sanal makinedeki yükseltilmiş PowerShell penceresine aşağıdakileri yazarak Microsoft İzleme Aracısı'nın durumunu görüntüleyin`(New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
6. Microsoft Monitoring Agent kurulum günlük dosyalarını gözden geçirin`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Daha fazla bilgi için [Windows uzantılarının sorun giderme sorununa](../../virtual-machines/extensions/oms-windows.md)bakın.

## <a name="troubleshooting-linux-vm-extension"></a>Sorun giderme Linux VM uzantısı
[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 
Linux VM uzantısı *için Log Analytics aracısı* yüklenmiyor veya raporlama etmiyorsa, sorunu gidermek için aşağıdaki adımları gerçekleştirebilirsiniz.

1. Uzantı durumu *Bilinmiyorsa,* Azure VM aracısının yüklü olup olmadığını ve VM aracısı günlük dosyasını gözden geçirerek doğru çalışıp çalışmayabilir`/var/log/waagent.log`
   * Günlük yoksa, VM aracısı yüklü değil.
   * [Azure VM Aracısını Linux VM'lere yükleme](../../azure-monitor/learn/quick-collect-azurevm.md#enable-the-log-analytics-vm-extension)
2. Diğer sağlıksız durumlar için, Linux VM uzantısı için Log Analytics `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` aracısını gözden geçirin ve`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Uzantı durumu sağlıklıysa, ancak veriler yüklenmiyorsa Linux günlük dosyaları için Log Analytics aracısını inceleyin`/var/opt/microsoft/omsagent/log/omsagent.log`

Daha fazla bilgi için sorun [giderme Linux uzantılarına](../../virtual-machines/extensions/oms-linux.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Azure dışındaki bilgisayarlarda barındırılan Linux için Log Analytics aracısı ile ilgili ek sorun giderme kılavuzu [için](agent-linux-troubleshoot.md)Bkz.  
