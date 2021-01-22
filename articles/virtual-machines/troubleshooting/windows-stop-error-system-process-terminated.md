---
title: Windows Dur hata durumu sistem Işlemi sonlandırıldı
description: Bu makalede, işletim sisteminin bir Azure sanal makinesinin önyüklemesini önleyen durma hatası 0xC000021A ile karşılaştığı sorunları çözmek için adımlar sağlanmaktadır.
services: virtual-machines-windows, azure-resource-manager
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: 037d0858-4611-4707-bd46-cc8085d011ed
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 09/21/2020
ms.author: v-mibufo
ms.openlocfilehash: 8af656e00c457dfa3d438f0cb104d85ccc687745
ms.sourcegitcommit: 52e3d220565c4059176742fcacc17e857c9cdd02
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98661247"
---
# <a name="windows-stop-error---0xc000021a-status-system-process-terminated"></a>Windows Dur hatası-0xC000021A durum sistem Işlemi sonlandırıldı

Bu makalede, işletim sisteminin (OS) bir Azure sanal makinesini (VM) önyüklemesine devam eden durma hatası 0xC000021A ile karşılaştığı sorunları çözmek için adımlar sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda ekran görüntüsü, önyükleme sırasında aşağıdaki iletiyle birlikte işletim sisteminin bir hatayla karşılaştığı iletiyi görüntüler:

**Bilgisayarınız bir sorunla karşılaştı ve yeniden başlatılması gerekiyor. Yalnızca bazı hata bilgilerini topluyoruz ve sonra yeniden başlatabilirsiniz. (% #% Tamam) Daha fazla bilgi edinmek istiyorsanız, bu hatayı daha sonra çevrimiçi olarak arayabilirsiniz: 0xC000021a**.

  ![Şekil 1 ' de, "BILGISAYARıNıZ bir sorunla karşılaştı ve yeniden başlatılması gerekiyor" iletisiyle birlikte hata #0xC000021A kodu görüntülenir. Yalnızca bazı hata bilgilerini topluyoruz ve sonra yeniden başlatabilirsiniz. ".](./media/windows-stop-error-system-process-terminated/1-pc-problem-restart.png)

## <a name="cause"></a>Nedeni

Hata 0xC000021A **STATUS_SYSTEM_PROCESS_TERMINATED** anlamına gelir.

Bu hata, WinLogon (winlogon.exe) veya Istemci sunucusu Run-Time alt sistemi (csrss.exe) gibi kritik bir işlem başarısız olduğunda oluşur. Çekirdek bu hizmetlerden birinin durdurulduğunu algıladığında, **Dur 0xC000021A** hatasını yükseltir. Bu hata, aşağıdakiler de dahil olmak üzere birkaç nedenden kaynaklanabilir:

- Eşleşmeyen sistem dosyaları yüklendi.
- Bir hizmet paketi veya BB güncelleştirme yüklemesi başarısız oldu.
- Sabit diski geri yüklemek için kullanılan bir yedekleme programı kullanımda olan dosyaları doğru şekilde geri yüklemedi.
- Uyumsuz bir üçüncü taraf program yüklendi.

## <a name="solution"></a>Çözüm

### <a name="collect-the-memory-dump-file"></a>Bellek dökümü dosyasını toplayın

> [!TIP]
> VM 'nin son yedeğine sahipseniz önyükleme sorununu çözmek için [VM 'yi yedekten geri yüklemeyi](../../backup/backup-azure-arm-restore-vms.md) deneyebilirsiniz.


Bu sorunu çözmek için kilitlenme bilgi dökümünü analiz etmeniz gerekir. Kilitlenme için bellek dökümü dosyasını toplayın ve desteğe başvurun. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>İşletim sistemi diskini yeni bir onarım VM 'sine iliştirme

1.  Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) 1-3 adımlarını kullanın.
2.  **Uzak Masaüstü bağlantısı** kullanarak, onarım sanal makinesine bağlanın.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulma ve destek bileti gönderme

1.  VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörü ' ne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi F ise, F:\windowsadresine gidin.
2.  Memory. dmp dosyasını bulun ve ardından bellek dökümü dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
3.  Memory. dmp dosyasını bulmada sorun yaşıyorsanız, bunun yerine, [seri konsolundaki maskelenemeyen kesme (NMI) çağrılarını](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) kullanmak isteyebilirsiniz. Kılavuzu izleyip [burada NMI çağrılarını kullanarak bir kilitlenme bilgi dökümü oluşturabilirsiniz](/windows/client-management/generate-kernel-or-complete-crash-dump).

## <a name="next-steps"></a>Sonraki Adımlar

- Daha fazla sorun giderme bilgisi için bkz. [genel önyükleme hatalarıyla ilgili sorunları giderme](./boot-error-troubleshoot.md) veya [işletim sistemi DISKINI bir kurtarma VM 'Sine ekleyerek bir Windows VM sorunlarını](./troubleshoot-recovery-disks-windows.md)giderme. Ayrıca, [bir sanal makinede sorun gidermek için önyükleme tanılamayı nasıl kullanacağınızı](./boot-diagnostics.md)öğrenmeniz gerekir.
- Kaynak Yöneticisi kullanma hakkında daha fazla bilgi için bkz. [Azure Resource Manager genel bakış](../../azure-resource-manager/management/overview.md).
- Sanal makinenize bağlanamıyorsanız bkz. [Azure VM 'ye YÖNELIK RDP bağlantılarında sorun giderme](./troubleshoot-rdp-connection.md).