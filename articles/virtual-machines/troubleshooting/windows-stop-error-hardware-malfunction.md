---
title: Windows durma hatası - Donanım arızası
description: Bu makalede, Windows Server 2008 sanal makinelerinin bir donanım arızası olduğunu belirten bir hata iletisiyle çöktüğünde oluşan sorunları gidermek için adımlar sağlanmaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 11/13/2020
ms.author: v-mibufo
ms.openlocfilehash: 89faa5b29e0a972f31ad51a7354635a53176541a
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98661366"
---
# <a name="windows-stop-error---hardware-malfunction"></a>Windows durma hatası - Donanım arızası

Bu makalede, Windows Server 2008 sanal makinelerinin bir donanım arızası olduğunu belirten bir hata iletisiyle çöktüğünde oluşan sorunları gidermek için adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünde şu iletiyle bir mavi ekran görüntülediğini görürsünüz:

**\*\*\* Donanım arızası**

**Destek için satıcınıza çağrı**

**\*\*\* Sistem durdu \*\*\***

#### <a name="blue-screen"></a>Mavi ekran

![Ekran görüntüsünde mavi bir ekran donanım arızası kilitlenmesi gösterilmektedir.](media/windows-stop-error-hardware-malfunction/windows-stop-error-hardware-malfunction-1.png)

#### <a name="serial-console"></a>Seri konsol

![Seri konsol etkinleştirilmişse, ekran görüntüsü seri konsol özelliğindeki "donanım arızası" iletisini gösterir.](media/windows-stop-error-hardware-malfunction/windows-stop-error-hardware-malfunction-2.png)

## <a name="cause"></a>Nedeni

Konuk işletim sistemi doğru şekilde ayarlanmamışsa ve maskelenemeyen bir kesme (NMI) gönderildiğinde bu ekran görünür. Hata iletisi, bir çekirdek modu programının işleyicinin yakalanmadığını bir özel durum oluşturmuş olduğunu gösterir. Bir bellek dökümü toplayarak hangi özel durumun oluşturulduğunu belirleyebilirsiniz.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış 

> [!TIP]
> VM 'nin son yedeğine sahipseniz önyükleme sorununu çözmek için [VM 'yi yedekten geri yüklemeyi](../../backup/backup-azure-arm-restore-vms.md) deneyebilirsiniz.

1. Maskelenemeyen kesme (NMI) kayıt defteri anahtarını ayarlama 
2. Bir onarım VM 'si oluşturma ve erişme 
3. Seri konsolu ve bellek dökümü toplamayı etkinleştir 
4. VM 'yi yeniden oluşturma 

### <a name="set-up-the-non-maskable-interrupt-nmi-registry-key"></a>Maskelenemeyen kesme (NMI) kayıt defteri anahtarını ayarlama

1. Azure portal kullanarak, Konuk işletim sisteminin normal şekilde önyüklendiği şekilde sanal makineyi yeniden başlatın. 
2. SANAL makineye bazı erişim geri yüklendikten sonra, yükseltilmiş bir komut istemi açın (yönetici olarak çalıştır). 
3. Aşağıdaki komutla NMI kayıt defteri anahtarını ayarlayın:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f
    ```
    [REG ADD komutu hakkında daha fazla bilgi görüntüleyin](/windows-server/administration/windows-commands/reg-add)
4. *(Isteğe bağlı)* Kurulum bellek dökümü koleksiyonu:

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f  
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 

    ```
5. *(Isteğe bağlı)* Seri konsol erişimi ayarlayın:

    ```
    BCDEDIT /ems {current} on, or bcdedit /ems '{current}' on if you are using PowerShell
    BCDEDIT /emssettings EMSPORT:1 EMSBAUDRATE:115200 
    ```
    [BCDEDIT komutu hakkında daha fazla bilgi görüntüleyin](/windows-server/administration/windows-commands/bcdedit)
6. Aşağıdaki komutla VM 'yi yeniden başlatın:

    ```
    SHUTDOWN /r /t 0 /f 
    ```
    [KAPANıYOR komutu hakkında daha fazla bilgi görüntüleyin](/windows-server/administration/windows-commands/shutdown)

> [!IMPORTANT]
> Sorun şimdi düzeltilmelidir!

> [!NOTE]
> Yeniden başlattıktan sonra, normal gibi çalıştığından emin olmak için sanal makinenizi test edin. Sorun yaşamaya devam ediyorsanız, daha fazla yönerge için sonraki bölüme devam edebilirsiniz.

> [!TIP]
> Yukarıdaki bölümde maskelenemeyen kesme (NMI) kayıt defteri anahtarının kurulması önerilir, ancak VM 'niz normal olarak önyüklenemediyse, Konuk işletim sistemi kayıt defterinde amaçlanan değişiklikler gerçekleşmeyebilir. Bu durumda, bunun yerine kayıt defteri ayarlarını el ile eklemek için aşağıdaki yönergeleri izleyebilirsiniz.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) 1-3 adımlarını kullanın.
2. Uzak Masaüstü Bağlantısı kullanarak onarım sanal makinesine bağlanın.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Seri konsolu ve bellek dökümü toplamayı etkinleştir

VM 'yi yeniden oluşturmadan önce, bellek dökümü toplama ve seri konsolunun etkinleştirilmesi önerilir. Bunu yapmak için aşağıdaki betiği çalıştırın: 

1. Yükseltilmiş bir komut istemi oturumu açın (yönetici olarak çalıştır). 
2. BCD deposu verilerini listeleyin ve sonraki adımda kullanacağınız önyükleme Yükleyici tanımlayıcısını saptayın. 
    1. 1. nesil bir VM için aşağıdaki komutu girin ve listelenen tanımlayıcıyı aklınızda yapın: 
 
        ```
        bcdedit /store <BOOT PARTITON>:\boot\bcd /enum
        ```
        Komutunda, öğesini `<BOOT PARTITON>` önyükleme klasörünü içeren, eklenen diskteki bölümünün harfiyle değiştirin. 

        ![Ekran görüntüsünde, Windows önyükleme yükleyicisi 'nin tanımlayıcı numarasını listeleyen 1. nesil bir VM 'de BCD deposunun listelenmesinin çıktısı gösterilmektedir.](media/windows-stop-error-hardware-malfunction/windows-stop-error-hardware-malfunction-3.png)
    2. 2. nesil bir VM için aşağıdaki komutu girin ve listelenen tanımlayıcıyı aklınızda yapın:
    
        ```
        BCDEDIT /store <LETTER OF THE EFI SYSTEM PARTITION>:EFI\Microsoft\boot\bcd /enum 
        ```
        * Komutunda, `<LETTER OF THE EFI SYSTEM PARTITION>` EFI sistem bölümünün harfiyle değiştirin.
        * *EFI sistem bölümü* olarak etiketlenmiş uygun sistem bölümünü belirlemek Için Disk Yönetimi konsolunu başlatmak faydalı olabilir.
        * Tanımlayıcı, benzersiz bir GUID olabilir veya varsayılan *Bootmgr* olabilir.
3. Seri konsolunu etkinleştirmek için aşağıdaki komutları çalıştırın:

    ```
    BCDEDIT /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON  
    BCDEDIT /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200 

    ```
    * Komutunda, `<VOLUME LETTER WHERE THE BCD FOLDER IS>` BCD klasörünün harfiyle değiştirin.
    * Komutunda, `<BOOT LOADER IDENTIFIER>` önceki adımda bulduğunuz tanımlayıcıyla değiştirin.
4. İşletim sistemi diskindeki boş alanın, VM 'deki bellek boyutundan (RAM) daha büyük olduğunu doğrulayın. 
    1. İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirmelisiniz. Dosyayı işletim sistemi diskinde oluşturmak yerine, yeterli boş alana sahip olan VM 'ye bağlı başka bir veri diskine de başvurabilirsiniz. Konumu değiştirmek için, **% systemroot%** değerini aşağıda listelenen komutlarda veri diskinin sürücü harfiyle (örneğin, **F:**) değiştirin. 
    2. Aşağıdaki komutları girin (önerilen döküm yapılandırması):

    **Bozuk işletim sistemi diskinden kayıt defteri kovanını yükle:**

    ```
    REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM
    ```

    **ControlSet001 üzerinde etkinleştir:**

    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```

    **ControlSet002 üzerinde etkinleştir:**

    ```
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f 
    REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f 
    ```

    **Bozuk işletim sistemi diskini kaldır:**

    ```
    REG UNLOAD HKLM\BROKENSYSTEM
    ```
### <a name="rebuild-the-virtual-machine"></a>Sanal makineyi yeniden derle

* VM 'yi yeniden derlemek için [VM onarım komutlarının 5. adımını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure sanal makine önyükleme hatalarında sorun giderme](./boot-error-troubleshoot.md)