---
title: Azure Linux Aracısı sorunlarını giderme
description: Azure Linux Aracısı ile ilgili sorunları çözün.
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
ms.openlocfilehash: 247324c30bbe0edaef78c0b0d5e6a6d593e8cac9
ms.sourcegitcommit: d2d1c90ec5218b93abb80b8f3ed49dcf4327f7f4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/16/2020
ms.locfileid: "97586406"
---
# <a name="troubleshoot-the-azure-linux-agent"></a>Azure Linux Aracısı sorunlarını giderme

[Azure Linux Aracısı](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) , 168.63.129.16 IP adresinde bir sanal makınenın (VM) yapı DENETLEYICISI (VM 'nin barındırıldığı temeldeki fiziksel sunucu) ile iletişim kurmasını sağlar.

>[!NOTE]
>Bu IP adresi, iletişimi kolaylaştıran ve engellenmemelidir olması gereken sanal bir genel IP adresidir. Daha fazla bilgi için bkz. [IP adresi 168.63.129.16 nedir?](../../virtual-network/what-is-ip-address-168-63-129-16.md).

## <a name="before-you-begin"></a>Başlamadan önce

Hala desteklendiğinden emin olmak için aracı durumunu ve sürümünü denetleyin. Sürüm desteğini denetlemek için bkz. [Azure 'da sanal makine aracıları Için en düşük sürüm desteği](https://docs.microsoft.com/troubleshoot/azure/virtual-machines/support-extensions-agent-version) veya durumu ve sürümü bulma adımları Için [WALıNUXAGENT SSS](https://github.com/Azure/WALinuxAgent/wiki/FAQ#what-does-goal-state-agent-mean-in-waagent---version-output) .

## <a name="troubleshoot-a-not-ready-status"></a>Hazırlanma durumunda sorun giderme

1. Çalıştığından emin olmak için Azure Linux aracısının hizmet durumunu denetleyin. Hizmet adı **walınuxagent** veya **waagent** olabilir.

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

   Hizmet çalışıyorsa, sorunu çözmek için yeniden başlatın. Hizmet durdurulmuşsa, uygulamayı başlatın, birkaç dakika bekleyin ve sonra durumu tekrar denetleyin.

1. Otomatik güncelleştirmenin etkinleştirildiğinden emin olun. **/Etc/waagent.exe** içindeki otomatik güncelleştirme ayarını denetleyin.

   ```
   AutoUpdate.Enabled=y
   ```

   Azure Linux aracısını güncelleştirme hakkında daha fazla bilgi için bkz. [VM 'de Azure Linux aracısını güncelleştirme](https://docs.microsoft.com/azure/virtual-machines/extensions/update-linux-agent).

1. VM 'nin doku denetleyicisine bağlanabildiğinizden emin olun. Sanal makinenin 80, 443 ve 32526 bağlantı noktalarında 168.63.129.16 'ye bağlanıp bağlanamamadığını test etmek için kıvrımlı gibi bir araç kullanın. VM beklendiği gibi bağlanmazsa, 80, 443 ve 32526 bağlantı noktaları üzerinden giden iletişimin VM 'deki yerel güvenlik duvarınızda açık olup olmadığını denetleyin. Bu IP adresi engellenirse, VM Aracısı beklenmeyen bir davranış gösterebilir.

## <a name="advanced-troubleshooting"></a>Gelişmiş sorun giderme

Azure Linux Aracısı sorunlarını gidermeye yönelik olaylar **/var/log/waagent.log** dosyasına kaydedilir.

### <a name="unable-to-connect-to-wireserver-ip-host-ip"></a>Kablolu sunucu IP 'si (konak IP 'si) ile bağlantı kurulamıyor

VM, ana bilgisayar sunucusundaki kablolu sunucu IP 'sine ulaşamadığınızda, **/var/log/waagent.log** dosyasında aşağıdaki hata görüntülenir.

```
2020-10-02T18:11:13.148998Z WARNING ExtHandler ExtHandler An error occurred while retrieving the goal state:
```

Bu sorunu çözmek için:

* SSH kullanarak VM 'ye bağlanın ve ardından Şu URL 'YI kıvbir biçimde erişmeyi deneyin: http://168.63.129.16/?comp=versions .
* 168.63.129.16 IP adresine erişimi engelleyebilecek bir güvenlik duvarı, ara sunucu veya başka bir kaynaktan kaynaklanmış olabilecek herhangi bir sorun olup olmadığını denetleyin.
* Linux IPTables veya bir üçüncü taraf güvenlik duvarının 80, 443 ve 32526 bağlantı noktalarına erişimi engelleyip engellemediğini denetleyin.

## <a name="next-steps"></a>Sonraki adımlar

Azure Linux Aracısı sorunlarını gidermek için [Microsoft destek 'e başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
