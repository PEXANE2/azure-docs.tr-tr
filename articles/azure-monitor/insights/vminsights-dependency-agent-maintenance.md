---
title: VM'ler için Azure İzleyici bağımlılık aracısını yükseltme | Microsoft Docs
description: Bu makalede komut satırı, Kurulum Sihirbazı ve diğer yöntemleri kullanarak VM'ler için Azure İzleyici bağımlılık aracısının nasıl yükseltileceği açıklanır.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 09/30/2019
ms.openlocfilehash: 28b7a2b95e8ad23226f92f2b6fba085cc0fa1bfd
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77565562"
---
# <a name="how-to-upgrade-the-azure-monitor-for-vms-dependency-agent"></a>VM'ler için Azure İzleyici bağımlılık aracısını yükseltme

VM'ler için Azure İzleyici bağımlılık aracısının ilk dağıtımından sonra, hata düzeltmeleri veya yeni özellik ya da işlevsellik desteği içeren güncelleştirmeler serbest bırakılır.  Bu makale, kullanılabilir yöntemleri ve yükseltmenin el ile veya Otomasyon aracılığıyla nasıl gerçekleştirileceğini anlamanıza yardımcı olur.

## <a name="upgrade-options"></a>Yükseltme seçenekleri 

Windows ve Linux 'un bağımlılık Aracısı en son sürüme el ile veya makinenin üzerinde çalıştığı ortama ve dağıtım senaryosuna bağlı olarak otomatik olarak yükseltilebilir. Aşağıdaki yöntemler aracıyı yükseltmek için kullanılabilir.

|Ortam |Yükleme yöntemi |Yükseltme yöntemi |
|------------|--------------------|---------------|
|Azure VM | [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) ve [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) için bağımlılık Aracısı VM Uzantısı | *Otomatik* olarak, Azure Resource Manager şablonunuzu otomatik olarak varsayılan olarak yükseltilir. bu özelliği otomatik yükseltme özelliğini **yanlış**olarak ayarlayarak geri çevirebilirsiniz. Otomatik yükseltmenin devre dışı bırakıldığı ikincil sürüm için yükseltme, ana sürüm yükseltmesi de aynı yöntemi izler-uzantıyı kaldırın ve yeniden yükleyin. |
| Özel Azure VM görüntüleri | Windows/Linux için bağımlılık aracısının el ile yüklenmesi | VM 'Lerin aracının en yeni sürümüne güncelleştirilmesi için Windows Installer paketi veya Linux kendiliğinden ayıklanan ve yüklenebilir kabuk betik paketi çalıştıran komut satırından gerçekleştirilmesi gerekir.|
| Azure dışı VM 'Ler | Windows/Linux için bağımlılık aracısının el ile yüklenmesi | VM 'Lerin aracının en yeni sürümüne güncelleştirilmesi için Windows Installer paketi veya Linux kendiliğinden ayıklanan ve yüklenebilir kabuk betik paketi çalıştıran komut satırından gerçekleştirilmesi gerekir. |

## <a name="upgrade-windows-agent"></a>Windows aracısını yükselt 

Bir Windows VM 'deki aracıyı, bağımlılık Aracısı VM uzantısı kullanılarak yüklenmeyen en son sürüme güncelleştirmek için, komut Istemi, komut dosyası veya başka bir Otomasyon çözümünden ya da InstallDependencyAgent-Windows. exe Kurulum Sihirbazı ' nı kullanarak komutunu çalıştırın.  

Windows aracısının en son sürümünü [buradan](https://aka.ms/dependencyagentwindows)indirebilirsiniz.

### <a name="using-the-setup-wizard"></a>Kurulum Sihirbazı 'Nı kullanma

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Kurulum sihirbazını başlatmak için **InstallDependencyAgent-Windows. exe** ' yi yürütün.
   
3. Bağımlılık aracısının önceki sürümünü kaldırmak ve sonra en son sürümü yüklemek için **Dependency Agent kurulum** Sihirbazı ' nı izleyin.


### <a name="from-the-command-line"></a>Komut satırından

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Aşağıdaki komutu çalıştırın.

    ```dos
    InstallDependencyAgent-Windows.exe /S /RebootMode=manual
    ```

    `/RebootMode=manual` parametresi, bazı süreçler önceki sürümden dosya kullanıyorsa ve bunlara bir kilit varsa, yükseltmenin makinenin otomatik olarak yeniden başlatılmasını önler. 

3. Yükseltmenin başarılı olduğunu doğrulamak için, ayrıntılı kurulum bilgileri için `install.log` denetleyin. Günlük dizini *%ProgramFiles%\Microsoft Dependency Fıles\logs*dizinidir.

## <a name="upgrade-linux-agent"></a>Linux aracısını yükselt 

Linux üzerinde bağımlılık aracısının önceki sürümlerinden yükseltme desteklenir ve yeni bir yüklemeyle aynı komutu izleyerek gerçekleştirilir.

Windows aracısının en son sürümünü [buradan](https://aka.ms/dependencyagentlinux)indirebilirsiniz.

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Kök`sh InstallDependencyAgent-Linux64.bin -s`olarak aşağıdaki komutu çalıştırın. 

Bağımlılık Aracısı'nı başlatmak başarısız olursa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracılarında günlük dizini */var/seçenek/Microsoft/Dependency-Agent/log*olur. 

## <a name="next-steps"></a>Sonraki adımlar

VM 'lerinizi bir süre izlemeyi durdurmak veya VM'ler için Azure İzleyici tamamen kaldırmak istiyorsanız, bkz. [VM'ler için Azure izleyici sanal makinelerinizin Izlenmesini devre dışı bırakma](vminsights-optout.md).
