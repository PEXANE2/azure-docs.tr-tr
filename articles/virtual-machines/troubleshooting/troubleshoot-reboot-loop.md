---
title: Azure VM'de Windows yeniden başlatma döngüsü | Microsoft Dokümanlar
description: Windows yeniden başlatma döngüsüyle nasıl sorun giderilenöğrenin | Microsoft Dokümanlar
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
ms.openlocfilehash: 3fd0a8bf6bacfec5e2be6dfa52ca51e46c7025f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75443593"
---
# <a name="windows-reboot-loop-on-an-azure-vm"></a>Azure VM'de Windows yeniden başlatma döngüsü
Bu makalede, Microsoft Azure'daki bir Windows Sanal Makine'de (VM) karşılaşabileceğiniz yeniden başlatma döngüsü açıklanmaktadır.

## <a name="symptom"></a>Belirti

Bir VM ekran görüntüsünü almak için [Önyükleme tanılama](./boot-diagnostics.md) kullandığınızda, sanal makine önyükleme ama önyükleme işlemi kesintiye oluyor ve işlem baştan başlıyor bulabilirsiniz.

![Başlangıç ekranı 1](./media/troubleshoot-reboot-loop/start-screen-1.png)

## <a name="cause"></a>Nedeni

Yeniden başlatma döngüsü aşağıdaki nedenlerden dolayı oluşur:

### <a name="cause-1"></a>Neden 1

Kritik olarak işaretlenmiş ve başlatılamayan bir üçüncü taraf hizmeti vardır. Bu, işletim sisteminin yeniden başlatılmasına neden olur.

### <a name="cause-2"></a>Neden 2

İşletim sisteminde bazı değişiklikler yapıldı. Bunlar genellikle güncelleştirme yüklemesi, uygulama yüklemesi veya yeni bir ilkeyle ilişkilidir. Ek ayrıntılar için aşağıdaki günlükleri kontrol etmeniz gerekebilir:

- Olay Günlükleri
- CBS.logWindows
- Update.log

### <a name="cause-3"></a>Neden 3

Dosya sistemi bozulması buna neden olabilir. Ancak, işletim sisteminin bozulmasına neden olan değişikliği tanılamak ve tanımlamak zordur.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için [işletim sistemi diskini yedekleyin](../windows/snapshot-copy-managed-disk.md)ve [işletim sistemi diskini kurtarma VM'ine takın](../windows/troubleshoot-recovery-disks-portal.md)ve ardından çözüm seçeneklerini buna göre izleyin veya çözümleri tek tek deneyin.

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

1. İşletim sistemi diski çalışan bir VM'ye iliştirildikten sonra, diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın ve **\Windows** klasörünü tutan bölümün sürücü harfine dikkat edin.

2. Disk **Çevrimdışı**olarak ayarlanmışsa, çevrimiçi **olarak**ayarlayın.

3. Değişikliklerin geri alabilmesi durumunda **\Windows\System32\config** klasörünün bir kopyasını oluşturun.

4. Kurtarma VM'inde Windows Kayıt Defteri Düzenleyicisi'ni (regedit) açın.

5. **HKEY_LOCAL_MACHINE** tuşunu seçin ve ardından menüden **Dosya** > **Yükle Hive'ı** seçin.

6. **\Windows\System32\config** klasöründeki System dosyasına göz atın.

7. Ad için **BrokenSYSTEM** yazın, **HKEY_LOCAL_MACHINE** tuşunu genişletin ve ardından **Open** **BROKENSYSTEM**adlı ek bir anahtar göreceksiniz.

8. Bilgisayarın hangi ControlSet'ten önyükleme yesahip olduğunu kontrol edin. Anahtar numarasını aşağıdaki kayıt defteri anahtarında görürsünüz.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\Select\Current`

9. Aşağıdaki kayıt defteri anahtarı aracılığıyla VM aracı hizmetinin kritikliğini kontrol edin.

    `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\RDAgent\ErrorControl`

10. Kayıt defteri anahtarının değeri **2**olarak ayarlanmazsa, bir sonraki azaltma bölümüne gidin.

11. Kayıt defteri anahtarının değeri **2**olarak ayarlanırsa, değeri **2'den** **1'e**değiştirin.

12. Aşağıdaki anahtarlardan herhangi biri varsa ve değeri **2** veya **3**ise ve bu değerleri buna göre **1** olarak değiştirin:

    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupCoordinatorSvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupInquirySvc\ErrorControl`
    - `HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet00x\Services\AzureWLBackupPluginSvc\ErrorControl`

13. **BROKENSYSTEM** tuşunu seçin ve ardından menüden **Dosya** > **Boşaltma Kovanı'nı** seçin.

14. İşletim sistemi diskini sorun giderme VM'den ayırın.

15. Diski sorun giderme VM'sinden çıkarın ve Azure'un bu diski yayınlaması için yaklaşık 2 dakika bekleyin.

16. [İşletim sistemi diskinden yeni bir VM oluşturun.](../windows/create-vm-specialized.md)

17. Sorun giderilirse, [RDAgent'ı](https://blogs.msdn.microsoft.com/mast/2014/04/07/install-the-vm-agent-on-an-existing-azure-vm/) (WaAppAgent.exe) yeniden yüklemeniz gerekebilir.

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

VM'yi bilinen son iyi yapılandırmaya geri yükleyin, [Azure Windows VM'yi Son Bilinen İyi Yapılandırma ile başlatma](https://support.microsoft.com/help/4016731/)adımlarını izleyin.

### <a name="solution-for-cause-3"></a>Neden 3 için çözüm
>[!NOTE]
>Aşağıdaki yordam yalnızca son kaynak olarak kullanılmalıdır. Regback'ten geri yükleme makineye erişimi geri yükleyebilir, ancak kovanın zaman damgası ile geçerli gün arasında kayıt defterinde kaybolan veriler olduğundan işletim sistemi kararlı kabul edilmez. Yeni bir VM oluşturmanız ve verileri geçirme planları yapmanız gerekir.

1. Disk bir sorun giderme VM'ine bağlandıktan sonra, diskin Disk Yönetimi konsolunda **Çevrimiçi** olarak işaretlendiğini unutmayın.

2. Değişikliklerin geri alabilmesi durumunda **\Windows\System32\config** klasörünün bir kopyasını oluşturun.

3. **\Windows\System32\config\regback** klasöründeki dosyaları kopyalayın ve **\Windows\System32\config** klasöründeki dosyaları değiştirin.

4. Diski sorun giderme VM'sinden çıkarın ve Azure'un bu diski yayınlaması için yaklaşık 2 dakika bekleyin.

5. [İşletim sistemi diskinden yeni bir VM oluşturun.](../windows/create-vm-specialized.md)


