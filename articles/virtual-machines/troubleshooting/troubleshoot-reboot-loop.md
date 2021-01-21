---
title: Azure VM 'de Windows yeniden başlatma döngüsü | Microsoft Docs
description: Windows yeniden başlatma döngüsü sorunlarını nasıl giderebileceğinizi öğrenin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: ''
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/15/2018
ms.author: genli
ms.openlocfilehash: ad0ed7e9619f0b789bf8949fe398aa27bc36b9e0
ms.sourcegitcommit: 484f510bbb093e9cfca694b56622b5860ca317f7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/21/2021
ms.locfileid: "98629649"
---
# <a name="windows-reboot-loop-on-an-azure-vm"></a>Azure VM 'de Windows yeniden başlatma döngüsü
Bu makalede, Microsoft Azure ' de bir Windows sanal makinesinde (VM) karşılaşabileceğiniz yeniden başlatma döngüsü açıklanır.

## <a name="symptom"></a>Belirti

Bir VM 'nin ekran görüntülerini almak için [önyükleme tanılamayı](./boot-diagnostics.md) kullandığınızda, sanal makinenin önbaşlatılmakta olduğunu ancak önyükleme işleminin kesintiye uğratılmakta olduğunu ve işlemin üzerine başlatılmakta olduğunu fark edersiniz.

![Başlangıç ekranı 1](./media/troubleshoot-reboot-loop/start-screen-1.png)

## <a name="cause"></a>Nedeni

Yeniden başlatma döngüsü aşağıdaki nedenlerle oluşur:

### <a name="cause-1"></a>1\. Neden

Kritik olarak işaretlenen bir üçüncü taraf hizmeti var ve başlatılamıyor. Bu, işletim sisteminin yeniden başlatılmasına neden olur.

### <a name="cause-2"></a>2\. Neden

İşletim sisteminde bazı değişiklikler yapılmıştır. Genellikle bunlar güncelleştirme yüklemesiyle, uygulama yüklemesiyle veya yeni ilkeyle ilgilidir. Ek ayrıntılar için aşağıdaki günlükleri denetlemeniz gerekebilir:

- Olay Günlükleri
- CBS. logWindows
- Update. log

### <a name="cause-3"></a>Neden 3

Dosya sistemi bozulması buna neden olabilir. Ancak, işletim sisteminin bozulmasına neden olan değişikliği tanılamak ve tanımlamak zordur.

## <a name="solution"></a>Çözüm

> [!TIP]
> VM 'nin son yedeğine sahipseniz önyükleme sorununu çözmek için [VM 'yi yedekten geri yüklemeyi](../../backup/backup-azure-arm-restore-vms.md) deneyebilirsiniz.

Bu sorunu çözmek için, [işletim sistemi diskini yedekleyin](../windows/snapshot-copy-managed-disk.md)ve [işletim sistemi DISKINI bir kurtarma VM 'sine bağlayın](./troubleshoot-recovery-disks-portal-windows.md), sonra çözüm seçeneklerini uygun şekilde uygulayın veya çözümleri tek tek deneyin.

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

1. İşletim sistemi diski çalışan bir VM 'ye eklendikten sonra, diskin disk yönetim konsolunda **çevrimiçi** olarak işaretlendiğinden emin olun ve **\Windows** klasörünü tutan bölümün sürücü harfini unutmayın.

2. Disk **çevrimdışı** olarak ayarlandıysa, **çevrimiçi** olarak ayarlayın.

3. Değişiklikler üzerinde geri alma işleminin gerekli olması durumunda **\Windows\system32\config** klasörünün bir kopyasını oluşturun.

4. Kurtarma VM 'sinde, Windows kayıt defteri Düzenleyicisi 'Ni (regedit) açın.

5. **HKEY_LOCAL_MACHINE** anahtarını seçin ve sonra menüden **Dosya**  >  **yükleme Hive** ' yi seçin.

6. **\Windows\system32\config** klasöründeki sistem dosyasına gidin.

7. **Aç**' ı seçin, ad Için **brokensystem** yazın, **HKEY_LOCAL_MACHINE** anahtarını genişletin ve sonra **brokensystem** adlı ek bir anahtar görürsünüz.

8. Bilgisayarın hangi ControlSet 'e önyükleme olduğunu denetleyin. Anahtar numarasını aşağıdaki kayıt defteri anahtarında görürsünüz.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\Select\Current`

9. Aşağıdaki kayıt defteri anahtarı aracılığıyla VM Aracısı hizmetinin önemi olduğunu denetleyin.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\RDAgent\ErrorControl`

10. Kayıt defteri anahtarının değeri **2** olarak ayarlanmamışsa, sonraki hafifletme bölümüne gidin.

11. Kayıt defteri anahtarının değeri **2** olarak ayarlandıysa, değeri **2** ' den **1**' e değiştirin.

12. Aşağıdaki anahtarlardan biri varsa ve **2** veya **3** değeri varsa ve bu değerleri uygun şekilde **1** olarak değiştirin:

    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupCoordinatorSvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupInquirySvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupPluginSvc\ErrorControl`

13. **Brokensystem** anahtarını seçin ve menüden **Dosya**  >  **Kaldır Hive** öğesini seçin.

14. İşletim sistemi diskini sorun giderme VM 'sinden ayırın.

15. Diski sorun giderme VM 'sinden kaldırın ve Azure 'un bu diski serbest bırakmasıyla ilgili 2 dakika bekleyin.

16. [İşletim sistemi diskinden yeni BIR VM oluşturun](../windows/create-vm-specialized.md).

17. Sorun giderilirse, [Rdadgent](/archive/blogs/mast/install-the-vm-agent-on-an-existing-azure-vm) 'ı (WaAppAgent.exe) yeniden yüklemeniz gerekebilir.

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

VM 'yi bilinen son iyi yapılandırmaya geri yükleme, [Azure WINDOWS VM 'Yi bilinen son Iyi yapılandırmayla başlatma](https://support.microsoft.com/help/4016731/)bölümündeki adımları izleyin.

### <a name="solution-for-cause-3"></a>Neden 3 için çözüm
>[!NOTE]
>Aşağıdaki yordam yalnızca son kaynak olarak kullanılmalıdır. Regback 'ten geri yükleme işlemi makineye geri yüklenebilirken, kayıt defterinde Hive ve geçerli gün arasında kaybolan veriler olduğundan, işletim sistemi kararlı kabul edilmez. Yeni bir VM oluşturmanız ve verileri geçirmeye yönelik planlar yapmanız gerekir.

1. Disk, sorun giderme sanal makinesine eklendikten sonra, diskin Disk Yönetimi konsolunda **çevrimiçi** olarak işaretlendiğinden emin olun.

2. Değişiklikler üzerinde geri alma işleminin gerekli olması durumunda **\Windows\system32\config** klasörünün bir kopyasını oluşturun.

3. **\Windows\system32\config\regback** klasöründeki dosyaları kopyalayın ve **\Windows\System32\Config** klasöründeki dosyaları değiştirin.

4. Diski sorun giderme VM 'sinden kaldırın ve Azure 'un bu diski serbest bırakmasıyla ilgili 2 dakika bekleyin.

5. [İşletim sistemi diskinden yeni BIR VM oluşturun](../windows/create-vm-specialized.md).
