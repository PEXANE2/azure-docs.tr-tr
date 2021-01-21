---
title: Yerel oturum Yöneticisi hizmeti beklenirken VM yanıt vermiyor
description: Bu makalede, bir Azure VM 'yi önyüklerken, Konuk işletim sisteminin, işlemeyi bitirmesini bekleyen sorunları çözmek için adımlar sağlanmaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: troubleshooting
ms.date: 10/22/2020
ms.author: v-mibufo
ms.openlocfilehash: fc3bd5d2590e969db07e9dffa61b4902ea4604c3
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98632555"
---
# <a name="vm-is-unresponsive-while-waiting-for-the-local-session-manager-service"></a>Yerel oturum Yöneticisi hizmeti beklenirken VM yanıt vermiyor

Bu makalede, bir Azure sanal makinesini (VM) önyüklendiğinde, Konuk işletim sisteminin (konuk işletim sistemi), işlemeyi bitirmesini bekleyen sorunları çözmek için adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin çıktısının bir ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünde "lütfen yerel oturum Yöneticisi için bekleyin" iletisini içeren bir istem görüntülediğini görürsünüz.

![Windows Server 2012 R2 'de takılı Konuk işletim sistemini gösteren ekran görüntüsü, "lütfen yerel oturum Yöneticisi için bekleyin" iletisini gösterir.](media/vm-unresponsive-wait-local-session-manager/vm-unresponsive-wait-local-session-manager-1.png)

## <a name="cause"></a>Nedeni

Yerel oturum Yöneticisi için bekleyen bir VM 'nin takılmasının birden çok nedeni olabilir. Bu sorun devam ederse, analiz için bir bellek dökümü toplamanız gerekir.

## <a name="solution"></a>Çözüm

> [!TIP]
> VM 'nin son yedeğine sahipseniz önyükleme sorununu çözmek için [VM 'yi yedekten geri yüklemeyi](../../backup/backup-azure-arm-restore-vms.md) deneyebilirsiniz.

Bazı durumlarda, işlemin bitmesini beklemek sorunu çözmeyecektir. VM 'niz yanıt vermezse ve bir saatten daha fazla süre boyunca kalırsa, bir bellek dökümü toplayıp Microsoft destek 'e başvurmanız gerekir.

### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>İşletim sistemi diskini yeni bir onarım VM 'sine iliştirme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md)1-3 adımlarını izleyin.
1. Uzak Masaüstü Bağlantısı kullanarak onarım sanal makinesine bağlanın.

### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulma ve destek bileti gönderme

1. VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörüne gidin. Örneğin, ekli işletim sistemi diskine atanan sürücü harfi *F* olarak etiketlenmişse, adresine gidin `F:\Windows` .
1. *Memory. dmp* dosyasını bulun ve ardından bellek dökümü dosyası eklenmiş [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
1. *Memory. dmp* dosyasını bulmakta sorun yaşıyorsanız, [maskelenemeyen kesme (NMI) çağrılarını kullanarak kilitlenme bilgi döküm dosyası oluşturma](/windows/client-management/generate-kernel-or-complete-crash-dump)kılavuzunu izleyin.

NMI çağrıları hakkında daha fazla bilgi için bkz. [Azure seri konsol Kullanıcı Kılavuzu 'Nda NMI çağrıları](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) .

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure sanal makine önyükleme hatalarında sorun giderme](boot-error-troubleshoot.md)