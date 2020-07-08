---
title: VM'ler için Azure İzleyici bağımlılık aracısını yükseltme
description: Bu makalede komut satırı, Kurulum Sihirbazı ve diğer yöntemleri kullanarak VM'ler için Azure İzleyici bağımlılık aracısının nasıl yükseltileceği açıklanır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/16/2020
ms.openlocfilehash: 01dd8422658aa0c8982733e48782efd27c1bf5be
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81617852"
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

Bir Windows sanal makinesi üzerindeki aracıyı, bağımlılık Aracısı VM uzantısı kullanılarak yüklenmeyen en son sürüme güncelleştirmek için komut Istemi, komut dosyası veya başka bir Otomasyon çözümünden veya InstallDependencyAgent-Windows.exe Kurulum Sihirbazı ' nı kullanarak çalıştırırsınız.  

Windows aracısının en son sürümünü [buradan](https://aka.ms/dependencyagentwindows)indirebilirsiniz.

### <a name="using-the-setup-wizard"></a>Kurulum Sihirbazı 'Nı kullanma

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Kurulum sihirbazını başlatmak için **InstallDependencyAgent-Windows.exe** yürütün.
   
3. Bağımlılık aracısının önceki sürümünü kaldırmak ve sonra en son sürümü yüklemek için **Dependency Agent kurulum** Sihirbazı ' nı izleyin.


### <a name="from-the-command-line"></a>Komut satırından

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Şu komutu çalıştırın.

    ```dos
    InstallDependencyAgent-Windows.exe /S /RebootMode=manual
    ```

    `/RebootMode=manual`Parametresi, bazı süreçler önceki sürümden dosya kullanıyorsa ve bunlara bir kilit varsa, yükseltmenin makinenin otomatik olarak yeniden başlatılmasını önler. 

3. Yükseltmenin başarılı olduğunu doğrulamak için `install.log` ayrıntılı kurulum bilgilerini inceleyin. Günlük dizini *%ProgramFiles%\Microsoft Dependency Fıles\logs*dizinidir.

## <a name="upgrade-linux-agent"></a>Linux aracısını yükselt 

Linux üzerinde bağımlılık aracısının önceki sürümlerinden yükseltme desteklenir ve yeni bir yüklemeyle aynı komutu izleyerek gerçekleştirilir.

Linux aracısının en son sürümünü [buradan](https://aka.ms/dependencyagentlinux)indirebilirsiniz.

1. Yönetici haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Aşağıdaki komutu kök olarak çalıştırın `sh InstallDependencyAgent-Linux64.bin -s` . 

Bağımlılık Aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlüklere bakın. Linux aracılarında günlük dizini */var/seçenek/Microsoft/Dependency-Agent/log*olur. 

## <a name="next-steps"></a>Sonraki adımlar

VM 'lerinizi bir süre izlemeyi durdurmak veya VM'ler için Azure İzleyici tamamen kaldırmak istiyorsanız, bkz. [VM'ler için Azure izleyici sanal makinelerinizin Izlenmesini devre dışı bırakma](vminsights-optout.md).
