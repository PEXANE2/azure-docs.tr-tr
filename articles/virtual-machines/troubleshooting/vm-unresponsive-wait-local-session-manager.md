---
title: Yerel oturum Yöneticisi beklenirken VM yanıt vermiyor
description: Bu makalede, bir Azure VM 'yi önyüklerken yerel oturum yöneticisinin işleme bitmesini bekleyen Konuk işletim sisteminin takılmasına neden olan sorunları çözmek için adımlar sağlanmaktadır.
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
ms.openlocfilehash: f2f0177b5fe8bb97773d297319f6c9196d8178d2
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94536243"
---
# <a name="vm-is-unresponsive-while-waiting-for-the-local-session-manager"></a>Yerel oturum Yöneticisi beklenirken VM yanıt vermiyor

Bu makalede, bir Azure sanal makinesini (VM) önyüklerken yerel oturum yöneticisinin işlemeyi bitirmesini bekleyen Konuk işletim sisteminin takılı olduğu sorunları çözme adımları sunulmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, ekran görüntüsünde şu iletiyle bir istem görüntülediğini görürsünüz: " *lütfen yerel oturum Yöneticisi için bekleyin* "

![Ekran görüntüsünde, Konuk işletim sisteminin Windows Server 2012 R2 'de "lütfen yerel oturum Yöneticisi için bekleyin" iletisiyle birlikte takılmış olduğu gösterilmektedir.](media/vm-unresponsive-wait-local-session-manager/vm-unresponsive-wait-local-session-manager-1.png)

## <a name="cause"></a>Nedeni

Yerel oturum Yöneticisi için bekleyen bir sanal makinenin takılmasının birden çok nedeni olabilir. Yerel oturum yöneticisinin kalıcı bir sorun olması beklenirse, analiz için bir bellek dökümü toplamanız gerekir.

## <a name="solution"></a>Çözüm

Bazı durumlarda, işlemin tamamlanması için yeterince uzun sürmeniz sorunu çözmeyecektir. VM 'niz yanıt vermiyorsa ve bekleme ekranında bir saat boyunca kalırsa, bir bellek dökümü toplayıp Microsoft desteği 'ne başvurmanız gerekir.

### <a name="collect-the-memory-dump-file"></a>Bellek dökümü dosyasını toplayın

Bu sorunu çözmek için öncelikle kilitlenme için bellek dökümü dosyasını toplamanız ve ardından bellek dökümü dosyası ile desteğe başvurmanız gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>İşletim sistemi diskini yeni bir onarım VM 'sine iliştirme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](./repair-windows-vm-using-azure-virtual-machine-repair-commands.md) izleyin.
2. Uzak Masaüstü Bağlantısı kullanarak onarım sanal makinesine bağlanın.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulma ve destek bileti gönderme

1. VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörüne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi *F* olarak etiketlenmişse, adresine gitmeniz gerekir `F:\Windows` .
2. **Memory. dmp** dosyasını bulun ve ardından bellek dökümü dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
3. **Memory. dmp** dosyasını bulmakta sorun yaşıyorsanız, [maskelenemeyen kesme (NMI) çağrılarını kullanarak kilitlenme bilgi döküm dosyası oluşturma](/windows/client-management/generate-kernel-or-complete-crash-dump)kılavuzunu izleyin.

NMI çağrıları hakkında daha fazla bilgi için, [seri konsol Kullanıcı kılavuzundaki maskelenemeyen kesme (NMI) çağrılarına](./serial-console-windows.md#use-the-serial-console-for-nmi-calls) bakın.

## <a name="next-steps"></a>Sonraki Adımlar

> [!div class="nextstepaction"]
> [Azure sanal makine önyükleme hatalarında sorun giderme](boot-error-troubleshoot.md)