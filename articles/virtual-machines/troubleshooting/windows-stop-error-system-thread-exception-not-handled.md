---
title: Windows Dur hatası-0x0000007E sistem iş parçacığı özel durumu işlenmedi
description: Bu makalede, Konuk işletim sisteminin bir sorunla karşılaştığı ve Azure VM 'nizi yeniden başlatması gereken sorunları gidermeye yönelik adımlar sağlanmaktadır. İleti "sistem iş parçacığı özel durumu işlenmedi" olarak değişir.
services: virtual-machines-windows
documentationcenter: ''
author: mibufo
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 11/04/2020
ms.author: v-mibufo
ms.openlocfilehash: 1ce594d9e3ffddf781c61717ae4534f0c7bd40f8
ms.sourcegitcommit: c157b830430f9937a7fa7a3a6666dcb66caa338b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/17/2020
ms.locfileid: "94681899"
---
# <a name="windows-stop-error---0x0000007e-system-thread-exception-not-handled"></a>Windows Dur hatası-0x0000007E sistem iş parçacığı özel durumu işlenmedi

Bu makalede, Konuk işletim sisteminin (konuk IŞLETIM sistemi) bir sorunla karşılaştığı ve Azure sanal makinenizi (VM) yeniden başlatmaya çalıştığı sorunları gidermeye yönelik adımlar sağlanmaktadır. Görüntülenen hata iletisi, "sistem iş parçacığı özel durumu işlenmedi." ifadesini belirtir.

## <a name="symptoms"></a>Belirtiler

VM çıkışının ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, Windows 'un "SISTEM Iş parçacığı özel durumu işlenmedi" durdurma kodu veya "0x0000007E" hata kodu ile yeniden başlatılması gerektiğini görürsünüz.

!["BILGISAYARıNıZ bir sorunla karşılaştı ve yeniden başlatılması gerekiyor" ile önyükleme sırasında takılı pencereleri gösteren ekran görüntüsü. Sizin için yeniden başlatacağız. " hata iletisi ve "SISTEM Iş parçacığı özel durumu IŞLENMEDI" durdurma kodu.](media/windows-stop-error-system-thread-exception-not-handled/windows-stop-error-system-thread-exception-not-handled-1.png)

## <a name="cause"></a>Nedeni

Nedeni, bir bellek dökümü dosyası çözümlenene kadar belirlenemez. Bellek dökümü dosyasını toplamaya devam edin.

## <a name="solution"></a>Çözüm

Bu sorunu çözmek için öncelikle kilitlenme için bellek dökümü dosyasını toplamanız ve sonra dosyayı Microsoft destek 'e göndermeniz gerekir. Döküm dosyasını toplamak için sonraki iki bölüm içindeki yönergeleri izleyin.

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
> [Azure sanal makine önyükleme hatalarında sorun giderme](./boot-error-troubleshoot.md)
