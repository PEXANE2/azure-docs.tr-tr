---
title: Güvenlik Ilkesi sisteme uygulanırken Azure VM yanıt vermiyor
description: Bu makalede, bir Azure sanal makinesinde sisteme güvenlik ilkesi uygulanırken, VM yanıt vermediğinde yükleme ekranının takılı olduğu sorunları çözmek için adımlar sağlanmaktadır.
services: virtual-machines-windows
documentationcenter: ''
author: TobyTu
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: a97393c3-351d-4324-867d-9329e31b5629
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 06/15/2020
ms.author: v-mibufo
ms.openlocfilehash: 6b50bffd1a44c0cf53f15650f5ff4d938f45df4d
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "84908223"
---
# <a name="azure-vm-is-unresponsive-while-applying-security-policy-to-the-system"></a>Güvenlik Ilkesi sisteme uygulanırken Azure VM yanıt vermiyor

Bu makalede, işletim sisteminin askıda olduğu ve bir Azure VM 'de güvenlik ilkesi uygulanırken yanıt vermeyen sorunları gidermeye yönelik adımlar sağlanmaktadır.

## <a name="symptoms"></a>Belirtiler

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](boot-diagnostics.md) kullandığınızda, ekran görüntüsünde şu iletiyle önyükleme sırasında takılı olan işletim sistemi görüntülenir:

> ' Sisteme güvenlik ilkesi uygulanıyor '.

:::image type="content" source="media/unresponsive-vm-apply-security-policy/apply-policy.png" alt-text="Windows Server 2012 R2 başlangıç ekranının ekran görüntüsü takıldı.":::

:::image type="content" source="media/unresponsive-vm-apply-security-policy/apply-policy-2.png" alt-text="Windows Server 2012 R2 başlangıç ekranının ekran görüntüsü takıldı.":::

## <a name="cause"></a>Nedeni

Bu sorunun olası nedenlerine ilişkin bir plethora vardır. Bir bellek dökümü Analizi gerçekleştirilene kadar kaynağı bilmeniz mümkün olmayacaktır.

## <a name="resolution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. [Bir onarım VM 'si oluşturma ve erişme](#create-and-access-a-repair-vm)
2. [Seri konsolu ve bellek dökümü toplamayı etkinleştir](#enable-serial-console-and-memory-dump-collection)
3. [VM 'yi yeniden oluşturma](#rebuild-the-vm)
4. [Bellek dökümü dosyasını toplayın](#collect-the-memory-dump-file)

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
2. Onarım VM 'sine bağlanmak Uzak Masaüstü Bağlantısı kullanın.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Seri konsolu ve bellek dökümü toplamayı etkinleştir

Bellek dökümü toplamayı ve seri konsolunu etkinleştirmek için şu betiği çalıştırın:

1. Yükseltilmiş bir komut istemi oturumu açın (yönetici olarak çalıştır).
2. BCD deposu verilerini listeleyin ve sonraki adımda kullanacağınız önyükleme Yükleyici tanımlayıcısını saptayın.

     1. 1. nesil bir VM için aşağıdaki komutu girin ve listelenen tanımlayıcıyı aklınızda yapın:

        ```console
        bcdedit /store <BOOT PARTITON>:\boot\bcd /enum
        ```

        Komutunda, öğesini \<BOOT PARTITON> önyükleme klasörünü içeren, eklenen diskteki bölümünün harfiyle değiştirin.

        :::image type="content" source="media/unresponsive-vm-apply-security-policy/store-data.png" alt-text="Windows Server 2012 R2 başlangıç ekranının ekran görüntüsü takıldı." /v NMICrashDump /t REG_DWORD /d 1 /f
        ```

        Bozuk işletim sistemi diskini kaldır:

        ```console
        REG UNLOAD HKLM\BROKENSYSTEM
        ```

### <a name="rebuild-the-vm"></a>VM 'yi yeniden oluşturma

VM 'yi yeniden birleştirmek için [VM onarım komutlarının 5. adımını](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.

### <a name="collect-the-memory-dump-file"></a>Bellek dökümü dosyasını toplayın

Bu sorunu çözmek için öncelikle kilitlenme için bellek dökümü dosyasını toplamanız ve bellek döküm dosyası ile desteğe başvurmanız gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

1. İşletim sistemi diskini yeni bir onarım VM 'sine ekleyin:

    - Yeni bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](repair-windows-vm-using-azure-virtual-machine-repair-commands.md#repair-process-example) kullanın.
    - Onarım VM 'sine bağlanmak Uzak Masaüstü Bağlantısı kullanın.

2. Döküm dosyasını bulun ve bir destek bileti gönderin:

    - VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörü ' ne gidin. Eklenen işletim sistemi diskine atanmış olan sürücü harfi `F` ise, `F:\Windows` konumuna gitmelisiniz.
    - Memory. dmp dosyasını bulun ve ardından bellek dökümü dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
    - Memory. dmp dosyasını bulmada sorun yaşıyorsanız, bunun yerine, [seri konsolundaki maskelenemeyen kesme (NMI) çağrılarını](serial-console-windows.md#use-the-serial-console-for-nmi-calls) kullanmak isteyebilirsiniz. [NMI çağrılarını kullanarak kilitlenme bilgi döküm dosyası oluşturmak](/windows/client-management/generate-kernel-or-complete-crash-dump)için Kılavuzu izleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Yerel Kullanıcılar ve Gruplar ilkesi uyguladığınızda sorunlarla karşılaşırsanız, bkz. [yerel kullanıcılar ve Gruplar ilkesi Grup ilkesi uygulanırken VM yanıt vermiyor](unresponsive-vm-apply-group-policy.md)