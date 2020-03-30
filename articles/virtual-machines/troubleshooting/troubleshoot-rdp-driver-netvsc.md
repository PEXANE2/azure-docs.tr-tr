---
title: Azure'da Windows 10 veya Windows Server 2016 VM'ye uzaktan bağlandığınızda netvsc.sys sorununu giderme | Microsoft Dokümanlar
description: Azure'da bir Windows 10 veya Windows Server 2016 VM'ye bağlanırken netsvc.sys ile ilgili RDP sorununu nasıl giderdiğinizi öğrenin.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 11/19/2018
ms.author: genli
ms.openlocfilehash: 4c10a2dcd55c1605cfafe6c67cfefd9d8a3c5f9d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71057979"
---
# <a name="cannot-connect-remotely-to-a-windows-10-or-windows-server-2016-vm-in-azure-because-of-netvscsys"></a>Netvsc.sys sayesinde Azure'daki Windows 10 veya Windows Server 2016 VM'ye uzaktan bağlanamıyor

Bu makalede, Hyper-V Server 2016 ana bilgisayarındaki bir Windows 10 veya Windows Server 2016 Datacenter sanal makinesine (VM) bağlandığınızda ağ bağlantısı nın olmadığı bir sorunu nasıl giderebilirsiniz.

## <a name="symptoms"></a>Belirtiler

Uzak Masaüstü Protokolü (RDP) kullanarak Azure Windows 10 veya Windows Server 2016 VM'ye bağlanamazsınız. [Önyükleme tanılama,](boot-diagnostics.md)ekran ağ arabirim kartı (NIC) üzerinde bir kırmızı çapraz gösterir. Bu, işletim sistemi tamamen yüklendikten sonra VM'nin bağlantısı olmadığını gösterir.

Genellikle, bu sorun Windows [build 14393](https://support.microsoft.com/help/4093120/) ve [15063 oluşturur.](https://support.microsoft.com/help/4015583/) İşletim sisteminizin sürümü bu sürümlerden daha geçise, bu makale senaryonuz için geçerli değildir. Sistemin sürümünü denetlemek için, [Seri Erişim Konsolu özelliğinde](serial-console-windows.md)bir CMD oturumu açın ve ardından **Ver'i**çalıştırın.

## <a name="cause"></a>Nedeni

Yüklü netvsc.sys sistem dosyasının sürümü **10.0.14393.594** veya **10.0.15063.0**ise bu sorun oluşabilir. Netvsc.sys'nin bu sürümleri, sistemin Azure platformuyla etkileşim kurmasını engelleyebilir.


## <a name="solution"></a>Çözüm

Bu adımları izlemeden önce, yedek olarak etkilenen [VM'nin sistem diskinin anlık görüntüsünü alın.](../windows/snapshot-copy-managed-disk.md) Bu sorunu gidermek için Seri Konsolu'nu kullanın veya [VM'nin](#repair-the-vm-offline) sistem diskini kurtarma VM'ine takarak VM çevrimdışı onarımını tamamlayın.


### <a name="use-the-serial-console"></a>Seri Konsolu Kullanma

Seri [Konsol'a bağlanın, PowerShell örneğini açın](serial-console-windows.md)ve aşağıdaki adımları izleyin.

> [!NOTE]
> VM'nizde Seri Konsol etkinleştirilemiyorsa, VM çevrimdışı bölümünü [onarmaya](#repair-the-vm-offline) gidin.

1. Dosyanın sürümünü almak için powershell örneğinde aşağıdaki komutu çalıştırın (**c:\windows\system32\drivers\netvsc.sys):**

   ```
   (get-childitem "$env:systemroot\system32\drivers\netvsc.sys").VersionInfo.FileVersion
   ```

2. Aynı bölgeden çalışan bir VM'ye bağlı yeni veya varolan bir veri diskine uygun güncelleştirmeyi karşıdan yükleyin:

   - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562) veya daha sonraki güncelleme
   - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) veya daha sonraki güncelleme

3. Yardımcı diski çalışan VM'den ayırın ve kırık VM'ye takın.

4. Güncelleştirmeyi VM'ye yüklemek için aşağıdaki komutu çalıştırın:

   ```
   dism /ONLINE /add-package /packagepath:<Utility Disk Letter>:\<KB .msu or .cab>
   ```

5. VM’yi yeniden başlatın.

### <a name="repair-the-vm-offline"></a>VM Çevrimdışı Onarım

1. [Sistem diskini kurtarma VM'ine takın.](../windows/troubleshoot-recovery-disks-portal.md)

2. Kurtarma VM'sine Uzak Masaüstü bağlantısı başlatın.

3. Diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın. Ekli sistem diskine atanan sürücü mektubuna dikkat edin.

4. Değişikliklerin geri alınması gerektiğinde **\Windows\System32\config** klasörünün bir kopyasını oluşturun.

5. Kurtarma VM günü, Kayıt Defteri Düzenleyicisi (regedit.exe) başlatın.

6. **HKEY_LOCAL_MACHINE** tuşunu seçin ve ardından menüden **Dosya** > **Yük Kovanı'nı** seçin.

7. **\Windows\System32\config** klasöründe System dosyasını bulun.

8. Ad için **BrokenSYSTEM** yazın, **HKEY_LOCAL_MACHINE** tuşunu genişletin ve ardından **Open** **BROKENSYSTEM**adlı ek anahtarı bulun.

9. Aşağıdaki konuma gidin:

   ```
   HKLM\BROKENSYSTEM\ControlSet001\Control\Class\{4d36e972-e325-11ce-bfc1-08002be10318}
   ```

10. Her alt anahtarda (0000 gibi) **Microsoft HYPER-V Ağ Bağdaştırıcısı**olarak görüntülenen **DriverDesc** değerini inceleyin.

11. Alt anahtarda, VM ağ bağdaştırıcısının sürücü sürümü olan **DriverVersion** değerini inceleyin.

12. Uygun güncelleştirmeyi indirin:

    - **10.0.14393.594**: [KB4073562](https://support.microsoft.com/help/4073562) veya daha sonraki güncelleme
    - **10.0.15063.0**: [KB4016240](https://support.microsoft.com/help/4016240) veya daha sonraki güncelleme

13. Sistem diskini, güncelleştirmeyi karşıdan yükleyebileceğiniz bir kurtarma VM'ine veri diski olarak takın.

14. Güncelleştirmeyi VM'ye yüklemek için aşağıdaki komutu çalıştırın:

    ```
    dism /image:<OS Disk letter>:\ /add-package /packagepath:c:\temp\<KB .msu or .cab>
    ```

15. Kovanları sökmek için aşağıdaki komutu çalıştırın:

    ```
    reg unload HKLM\BROKENSYSTEM
    ```

16. [Sistem diskini ayırın ve VM'yi yeniden oluşturun.](../windows/troubleshoot-recovery-disks-portal.md)

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun

Hala yardıma ihtiyacınız varsa, sorunuzun hızla çözülmesi için [Azure Desteği'ne başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
