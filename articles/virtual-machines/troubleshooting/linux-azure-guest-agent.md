---
title: Azure Linux Konuk Aracısı sorunlarını giderme
description: Azure Linux konuk aracısının sorun giderme sorunları
services: virtual-machines-linux
ms.service: virtual-machines-linux
author: axelg
manager: dcscontentpm
editor: ''
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/17/2020
ms.author: axelg
ms.openlocfilehash: fc609b60c9d5d4d4734c3d73cbda87935b533caf
ms.sourcegitcommit: d60976768dec91724d94430fb6fc9498fdc1db37
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/02/2020
ms.locfileid: "96500266"
---
# <a name="troubleshooting-azure-linux-guest-agent"></a>Azure Linux Konuk Aracısı sorunlarını giderme

[Azure Linux Konuk Aracısı](../extensions/agent-linux.md) , bir sanal makıne (VM) aracısıdır. Bu, sanal makinenin 168.63.129.16 IP adresinde doku denetleyicisi (VM 'nin barındırıldığı temeldeki fiziksel sunucu) ile iletişim kurmasını sağlar. Bu IP adresi, iletişimi kolaylaştıran bir sanal genel IP adresidir. Daha fazla bilgi için bkz. [IP adresi 168.63.129.16 nedir](../../virtual-network/what-is-ip-address-168-63-129-16.md).

## <a name="checking-agent-status-and-version"></a>Aracı durumu ve sürümü denetleniyor

Bkz. https://github.com/Azure/WALinuxAgent/wiki/FAQ#what-does-goal-state-agent-mean-in-waagent---version-output

## <a name="troubleshooting-vm-agent-that-is-in-not-ready-status"></a>Hazırlama durumunda olan VM Aracısı sorunlarını giderme

### <a name="step-1-check-whether-the-azure-linux-guest-agent-service-is-running"></a>1. adım Azure Linux Konuk Aracısı hizmetinin çalışıp çalışmadığını denetleyin

Hizmete bağlı olarak, hizmet adı walınuxagent veya waagent olabilir:

```
root@nam-u18:/home/nam# service walinuxagent status
● walinuxagent.service - Azure Linux Agent
   Loaded: loaded (/lib/systemd/system/walinuxagent.service; enabled; vendor preset: enabled)
   Active: active (running) since Thu 2020-10-08 17:10:29 UTC; 3min 9s ago
 Main PID: 1036 (python3)
    Tasks: 4 (limit: 4915)
   CGroup: /system.slice/walinuxagent.service
           ├─1036 /usr/bin/python3 -u /usr/sbin/waagent -daemon
           └─1156 python3 -u bin/WALinuxAgent-2.2.51-py2.7.egg -run-exthandlers

Oct 08 17:10:33 nam-u18 python3[1036]: 2020-10-08T17:10:33.129375Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.OSTCExtensions.VMAccessForLinux-1.5.10, path: /sys/fs/cgroup/memory/sys
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.189020Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Target handler state: enabled [incarnation 2]
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.197932Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] [Enable] current handler state is: enabled
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.212316Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Update settings file: 0.settings
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.224062Z INFO ExtHandler [Microsoft.CPlat.Core.RunCommandLinux-1.0.1] Enable extension [bin/run-command-shim enable]
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.236993Z INFO ExtHandler ExtHandler Started extension in unit 'Microsoft.CPlat.Core.RunCommandLinux_1.0.1_db014406-294a-49ed-b112-c7912a86ae9e
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.263572Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.CPlat.Core.RunCommandLinux-1.0.1, path: /sys/fs/cgroup/cpu,cpuacct/syst
Oct 08 17:10:35 nam-u18 python3[1036]: 2020-10-08T17:10:35.280691Z INFO ExtHandler ExtHandler Started tracking cgroup: Microsoft.CPlat.Core.RunCommandLinux-1.0.1, path: /sys/fs/cgroup/memory/system.sl
Oct 08 17:10:37 nam-u18 python3[1036]: 2020-10-08T17:10:37.349090Z INFO ExtHandler ExtHandler ProcessGoalState completed [incarnation 2; 4496 ms]
Oct 08 17:10:37 nam-u18 python3[1036]: 2020-10-08T17:10:37.365590Z INFO ExtHandler ExtHandler [HEARTBEAT] Agent WALinuxAgent-2.2.51 is running as the goal state agent [DEBUG HeartbeatCounter: 1;Heartb
root@nam-u18:/home/nam#
```


Hizmetleri görebilir ve çalışıyorsa, sorunun çözümlenip çözümlenmediğini görmek için hizmeti yeniden başlatın. Hizmetler durdurulmuşsa, bunları başlatın ve birkaç dakika bekleyin. Ardından, **Aracı durumunun** **hazırlık** olarak raporlanıp raporlanmadığını kontrol edin. Bu sorunların daha fazla giderilmesi için [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)başvurun.

### <a name="step-2-check-whether-auto-update-is-enabled"></a>2. adım otomatik güncelleştirme özelliğinin etkinleştirilip etkinleştirilmediğini denetleyin

/Etc/waagent.exe içindeki bu ayarı işaretleyin:

```
AutoUpdate.Enabled=y
```

Azure Linux aracısını güncelleştirme hakkında daha fazla bilgi için lütfen bkz. https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent 
    

### <a name="step-3-check-whether-the-vm-can-connect-to-the-fabric-controller"></a>3. adım VM 'nin doku denetleyicisine bağlanıp bağlanamamadığını denetleyin

Sanal makinenin 80, 32526 ve 443 bağlantı noktalarında 168.63.129.16 'ye bağlanıp bağlanamamadığını test etmek için kıvrımlı gibi bir araç kullanın. VM beklendiği gibi bağlanmazsa, 80, 443 ve 32526 bağlantı noktaları üzerinden giden iletişimin VM 'deki yerel güvenlik duvarınızda açık olup olmadığını denetleyin. Bu IP adresi engellendiyse, Sanal Makine Aracısı çeşitli senaryolarda beklenmeyen davranışlar gösterebilir.

## <a name="advanced-troubleshooting"></a>Gelişmiş sorun giderme

Azure Linux Konuk Aracısı sorunlarını gidermeye yönelik olaylar aşağıdaki günlük dosyalarına kaydedilir:

- /var/log/waagent.log


  
### <a name="unable-to-connect-to-wireserver-ip-host-ip"></a>Kablolu sunucu IP 'si (konak IP 'si) ile bağlantı kurulamıyor 

/Var/log/waagent.log dosyasında aşağıdaki hata girişlerine dikkat edin:

```
2020-10-02T18:11:13.148998Z WARNING ExtHandler ExtHandler An error occurred while retrieving the goal state:
```

**Çözümlemeleri**

VM, ana bilgisayar sunucusundaki kablolu sunucu IP 'sine erişemiyor.

**Çözüm**

1. Kablolu sunucu IP 'si erişilebilir olmadığı için SSH kullanarak VM 'ye bağlanın ve ardından aşağıdaki URL 'YI kıvlamadan erişmeyi deneyin: http://168.63.129.16/?comp=versions 
1. 168.63.129.16 IP adresine erişimi engelleyebilecek bir güvenlik duvarı, ara sunucu veya başka bir kaynaktan kaynaklanmış olabilecek herhangi bir sorun olup olmadığını denetleyin.
1. Linux IPTables veya bir üçüncü taraf güvenlik duvarının 80, 443 ve 32526 bağlantı noktalarına erişimi engelleyip engellemediğini denetleyin. Bu adresin neden engellenmediği hakkında daha fazla bilgi için bkz. [IP adresi nedir 168.63.129.16](../../virtual-network/what-is-ip-address-168-63-129-16.md).


## <a name="next-steps"></a>Sonraki adımlar

"Windows Azure Konuk Aracısı çalışmıyor" sorununu gidermek için [Microsoft destek 'e başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).