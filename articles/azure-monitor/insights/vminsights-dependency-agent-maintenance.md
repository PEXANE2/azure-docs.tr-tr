---
title: VM Bağımlılık aracısı için Azure Monitörü nasıl yükseltilir?
description: Bu makalede, komut satırı, kurulum sihirbazı ve diğer yöntemleri kullanarak VM Bağımlılık aracısı için Azure Monitörü nasıl yükseltilir açıklanmaktadır.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 04/16/2020
ms.openlocfilehash: 01dd8422658aa0c8982733e48782efd27c1bf5be
ms.sourcegitcommit: eefb0f30426a138366a9d405dacdb61330df65e7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81617852"
---
# <a name="how-to-upgrade-the-azure-monitor-for-vms-dependency-agent"></a>VM Bağımlılık aracısı için Azure Monitörü nasıl yükseltilir?

VM Bağımlılık aracısı için Azure Monitörü'nün ilk dağıtımından sonra, hata düzeltmeleri veya yeni özellikler veya işlevler desteği içeren güncelleştirmeler yayımlanır.  Bu makale, mevcut yöntemleri ve yükseltmenin el ile veya otomasyon yoluyla nasıl gerçekleştirilip gerçekleştirililebildiğini anlamanıza yardımcı olur.

## <a name="upgrade-options"></a>Yükseltme seçenekleri 

Windows ve Linux için Bağımlılık aracısı, dağıtım senaryosuna ve makinenin içinde çalışan ortama bağlı olarak el ile veya otomatik olarak en son sürüme yükseltilebilir. Aracıyı yükseltmek için aşağıdaki yöntemler kullanılabilir.

|Ortam |Kurulum yöntemi |Yükseltme yöntemi |
|------------|--------------------|---------------|
|Azure VM | [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) ve [Linux](../../virtual-machines/extensions/agent-dependency-linux.md) için Bağımlılık aracısı VM uzantısı | Azure Kaynak Yöneticisi şablonunuzu, özellik *otomatikYükseltmeMinorVersion'u* **false**olarak ayarlayarak devre dışı bırakmak üzere yapılandırmadığınız sürece aracı otomatik olarak otomatik olarak yükseltilir. Otomatik yükseltmenin devre dışı bırakıldığı küçük sürüm için yükseltme ve ana sürüm yükseltmesi de aynı yöntemi izleyin - uzantıyı kaldırın ve yeniden yükleyin. |
| Özel Azure VM görüntüleri | Windows/Linux için Bağımlılık aracısının el ile yüklenmesi | VM'leri aracının en yeni sürümüne güncellemenin, Windows yükleyici paketini veya Linux'un kendi kendine ayıklama ve yüklenebilir kabuk komut paketi paketini çalıştıran komut satırından gerçekleştirilmesi gerekir.|
| Azure Olmayan VM'ler | Windows/Linux için Bağımlılık aracısının el ile yüklenmesi | VM'leri aracının en yeni sürümüne güncellemenin, Windows yükleyici paketini veya Linux'un kendi kendine ayıklama ve yüklenebilir kabuk komut paketi paketini çalıştıran komut satırından gerçekleştirilmesi gerekir. |

## <a name="upgrade-windows-agent"></a>Windows aracıyı yükseltme 

Windows VM'deki aracıyı Bağımlılık aracısı VM uzantısı kullanılarak yüklü olmayan en son sürüme güncelleştirmek için Komut Komut Komut Ustem'den, komut dosyasından veya diğer otomasyon çözümünden veya InstallDependencyAgent-Windows.exe Kurulum Sihirbazı'nı kullanarak çalışırsınız.  

Windows aracısının en son sürümünü [buradan](https://aka.ms/dependencyagentwindows)indirebilirsiniz.

### <a name="using-the-setup-wizard"></a>Kurulum Sihirbazını Kullanma

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Kurulum Sihirbazı'nı başlatmak için **InstallDependencyAgent-Windows.exe'yi** çalıştırın.
   
3. Bağımlılık **aracısının** önceki sürümünü kaldırmak ve ardından en son sürümü yüklemek için Bağımlılık Aracısı Kurulumu sihirbazını izleyin.


### <a name="from-the-command-line"></a>Komut satırından

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Şu komutu çalıştırın.

    ```dos
    InstallDependencyAgent-Windows.exe /S /RebootMode=manual
    ```

    Parametre, `/RebootMode=manual` bazı işlemler önceki sürümdeki dosyaları kullanıyorsa ve üzerinde kilit varsa yükseltmenin makineyi otomatik olarak yeniden başlatmasını önler. 

3. Yükseltmenin başarılı olduğunu doğrulamak `install.log` için ayrıntılı kurulum bilgilerini kontrol edin. Günlük dizini *%Programfiles%\Microsoft Bağımlılık Aracısı\günlükleridir.*

## <a name="upgrade-linux-agent"></a>Linux aracıyı yükseltme 

Linux'taki Bağımlılık aracısının önceki sürümlerinden yükseltme, yeni bir yüklemeyle aynı komutu izleyerek desteklenir ve gerçekleştirilir.

Linux aracısının en son sürümünü [buradan](https://aka.ms/dependencyagentlinux)indirebilirsiniz.

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Aşağıdaki komutu kök`sh InstallDependencyAgent-Linux64.bin -s`olarak çalıştırın. 

Bağımlılık aracısı başlatılamazsa, ayrıntılı hata bilgileri için günlükleri denetleyin. Linux aracıları üzerinde, günlük dizini */var/opt/microsoft/dependency-agent/log'* dur. 

## <a name="next-steps"></a>Sonraki adımlar

VM'lerinizi bir süre izlemeyi durdurmak veya VM'ler için Azure Monitörünü tamamen kaldırmak istiyorsanız, [VM'ler için Azure Monitör'de VM'lerinizin izlemesini devre dışı](vminsights-optout.md)bırakın.
