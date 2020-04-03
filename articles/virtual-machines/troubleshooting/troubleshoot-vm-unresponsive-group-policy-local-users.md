---
title: Sanal Makine ,'Grup İlkesi Yerel Kullanıcılar & Grupları' ilkesini uygularken yanıt vermiyor
description: Bu makalede, yükleme ekranının bir Azure Sanal Makine 'nde (VM) önyükleme sırasında bir ilke uygulayarak takılıkaldığı sorunları gidermek için adımlar bulunur.
services: virtual-machines-windows
documentationcenter: ''
author: v-miegge
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.assetid: ff113268-f5bf-4e6a-986e-63b9b0ceff20
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: troubleshooting
ms.date: 04/02/2020
ms.author: v-mibufo
ms.openlocfilehash: 085880122e9a80e976cfe59686748b58aeba1922
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80620863"
---
# <a name="virtual-machine-is-unresponsive-while-applying-group-policy-local-users--groups-policy"></a>Sanal Makine ,'Grup İlkesi Yerel Kullanıcılar & Grupları' ilkesini uygularken yanıt vermiyor

Bu makalede, yükleme sırasında bir Azure Sanal Makine'de (VM) yükleme sırasında yük ekranının bir ilke uygulayarak takılıp kaldığı sorunları gidermek için adımlar bulunur.

## <a name="symptom"></a>Belirti

VM ekran görüntüsünü görüntülemek için [Önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekranın ileti ile yükleme sıkışmış olduğunu görürsünüz: *Grup İlkesi Yerel Kullanıcılar ve Gruplar ilkesi uygulama.*

![Alt metin: Uygulama Grubu İlkesi Yerel Kullanıcılar ve Gruplar ilkesi yüklemeyi gösteren ekran (Windows Server 2012).](media/troubleshoot-vm-unresponsive-group-policy-local-users/1.png)

Windows Server 2012

![Alt metin: Uygulama Grubu İlkesi Yerel Kullanıcılar ve Gruplar ilkesi yüklemeyi gösteren ekran (Windows Server 2012 R2).](media/troubleshoot-vm-unresponsive-group-policy-local-users/2.png)

Windows Server 2012 R2

## <a name="cause"></a>Nedeni

Bu dondurma belirtileri Windows Profil Hizmeti Dinamik Bağlantı Kitaplığı *(profsvc.dll)* bir kod hatası neden olur.

> [!NOTE]
> Bu hata yalnızca Windows Server 2012 ve Windows Server 2012 R2'de geçerlidir.

### <a name="the-policy-in-question"></a>Söz konusu politika

İşlemlerini tamamlamayacak şekilde uygulanan ilke:

* *Bilgisayar Yapılandırması\İlkeler\Yönetim Şablonları\Sistem/Kullanıcı Profilleri\Sistem yeniden başlatmada belirli bir gün sayısından daha eski kullanıcı profillerini silme*

Bu ilke yalnızca aşağıdaki altı koşul doğruysa geçerli olacaktır:

* *Sistem yeniden başlatma ilkesinde belirtilen gün sayısından daha eski kullanıcı profillerini sil* etkinleştirin.
* Temizleme gerektiren yaş gereksinimlerini karşılayan profilleriniz var.
* Profiller için silme bildirimi için kaydolmuş bileşenleriniz vardır.
* Bileşenler, Windows'un Hizmet Denetim Yöneticisi (SCM) bileşenlerinden (bir hizmetle ilgili Başlat, Durdur veya Sorgu bilgileri gibi) veri elde etmesi gereken aramalar (doğrudan veya dolaylı) yapar.
* *Otomatik*olarak başlamak üzere yapılandırılan bir hizmetiniz var.
* Bu hizmet, bir etki alanı hesabı bağlamında (yerel bir sistem gibi yerleşik bir hesap kullanmanın aksine) çalışacak şekilde ayarlanır.

### <a name="the-code-defect"></a>Kod hatası

Kod hatası, Servis Denetim Yöneticisi (SCM) ve Profil hizmetlerinin aynı anda birbirlerine kilit ler uygulamaya çalışmasından kaynaklanmaktadır. Kilitler, birden çok hizmetin aynı anda aynı verilerüzerinde değişiklik yapmasını önlemek için vardır ve bu da bozulmaya neden olur. Normalde, birden çok kilit isteği bir soruna neden olmaz. Ancak, bu önyükleme sırasında oluyor bu yana, ne hizmet birbirlerini bekleyen sıkışmış olarak, süreçlerini tamamlayabilirsiniz.

### <a name="os-bug-5880648---service-control-manager-deadlocks-with-the-delete-user-profiles-on-restart-policy"></a>OS Bug 5880648 - Servis Kontrol Yöneticisi "Yeniden başlatmada kullanıcı profillerini sil" ilkesiyle kilitlenir

Çakışan iki eylem vardır:

* Eylem 1 profil kilidini edinir, ancak henüz SCM kilidi elde etmemiştir.

  **Ve**

* Eylem 2 SCM kilidi satın aldı, ancak henüz profil kilidi elde değil.

Bu kilitlenme oluştuğunda, ikinci gerekli kilidi edinme girişimi eylemi kapatır.

### <a name="action-1---old-profile-deletion-notification-has-profile-lock-needs-scm-lock"></a>Eylem 1 - Eski profil silme bildirimi **(Profil Kilidi**vardır, **SCM Kilidi**gerekir)

1. İlk olarak, eski profilleri silmek için ayarlanan ilke bir iç profil hizmet kilidi kazanır.

   * Bu kilit, *silme işlemi* devam ederken iki iş parçacığının profillerle etkileşimkurmasını önlemek için vardır.

2. İlke, silinecek kadar eski profilleri bulur.
3. Profil silme işleminin bir parçası olarak, profilin silinmebildirimleri bildirimleri için kaydedilmiş bir bileşen **bir hizmet başlatmaya**çalışır.
4. Hizmeti başlatmadan önce, Hizmet Kontrol Yöneticisi'nin (SCM) **Eylem 2'deki**iş parçacıkları tarafından tutulan dahili bir **SCM kilidi** edinmesi gerekir.

### <a name="action-2---profile-loadcreation-for-user-specific-data-has-scm-lock-needs-profile-lock"></a>Eylem 2 - Kullanıcıya özel veriler için profil yükü/oluşturma **(SCM Kilidi**vardır, **Profil Kilidi**gerekir)

1. Önyüklemede, SCM'nin grubuna göre tüm *otomatik başlatma* hizmetlerinin yanı sıra bu hizmetlerin bağımlı olduğu tüm hizmetleri sipariş etmesi gerekir.

2. **SCM,** hizmetleri siparişleri vererken başlatma, durdurma veya yapılandırma hizmetlerine erişimi denetlemek için kullanılan dahili bir SCM kilidi edinir.

3. Hizmetler sırayla sıraladıktan sonra, SCM her hizmet aracılığıyla döngüler ve başlar.

4. Hizmet bir etki alanı hesabı bağlamında çalışıyorsa, kullanıcıya özel verileri depolayabilmek için bir profilin etki alanı hesabı için yüklenmesi veya oluşturulması gerekir.

5. Bu istek Profil **Servisi'ne**gönderilir.

6. Profil hizmetinin **Eylem 1'de**edinilen **dahili kilitlere** erişmesi gerekir.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme Genel Bakış

1. Onarım VM Oluşturma ve Eriş
2. Seri Konsol ve Bellek Döküm Toplamaetkinleştir
3. VM'yi yeniden oluşturma
4. Bellek Döküm Dosyasını Topla

   > [!NOTE]
   > Bu önyükleme hatasıyla karşılaştığında, Konuk Işletim Sistemi çalışmaz. Bu sorunu gidermek için Çevrimdışı modda sorun giderme olacak.

### <a name="create-and-access-a-repair-vm"></a>Onarım VM Oluşturma ve Eriş

1. Onarım VM'i hazırlamak için [VM Onarım Komutları'nın 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.
2. Uzak Masaüstü Bağlantısını Kullanarak Onarım VM'sine bağlanın.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Seri Konsol ve Bellek Döküm Toplamaetkinleştir

Bellek dökümü toplama ve Seri Konsol etkinleştirmek için aşağıdaki komut dosyasını çalıştırın:

1. Yükseltilmiş bir komut istemi oturumu açın (Yönetici olarak çalıştırın).
2. Aşağıdaki komutları çalıştırın:

   * Seri Konsolu Etkinleştir:

     `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

     `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

3. Os diskindeki boş alanın VM'deki bellek boyutu (RAM) kadar olduğunu doğrulayın.

   * Os diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirmeli ve bunu Yeterli boş alana sahip VM'ye bağlı herhangi bir veri diskine yönlendirmelisiniz. Konumu değiştirmek için, `%SystemRoot%` aşağıdaki komutlarda veri diskinin sürücü harfini ("F:") değiştirin.

#### <a name="suggested-configuration-to-enable-os-dump"></a>İşletim Sistemi Döküm'ü etkinleştirmek için önerilen yapılandırma

**Yük Kırık İşletim Sistemi Diski:**

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**ControlSet001'de etkinleştirin:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**ControlSet002'de etkinleştirin:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

### <a name="rebuild-the-vm"></a>VM'yi yeniden oluşturma

VM'yi yeniden birleştirmek için [VM Onarım Komutlarının adım](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) 5'ini kullanın.

### <a name="collect-the-memory-dump-file"></a>Bellek Döküm Dosyasını Topla

Bu sorunu gidermek için, öncelikle kilitlenme için bellek döküm dosyasını toplamak ve bellek dökümü dosyası ile destek başvurun gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>Os diskini yeni bir Onarım VM'ine takın

1. Yeni bir Onarım VM hazırlamak için [VM Onarım Komutlarının 1-3](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) adımlarını kullanın.

2. Uzak Masaüstü Bağlantısını Kullanarak Onarım VM'sine bağlanın.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulun ve destek bileti gönderin

1. Onarım VM'sinde, bağlı işletim sistemi diskindeki windows klasörüne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi F ise, F:\Windows'a gitmeniz gerekir.

2. Memory.dmp dosyasını bulun ve ardından bellek dökümü dosyasıyla [birlikte bir destek bileti gönderin.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)

3. Memory.dmp dosyasını bulmakta sorun yaşıyorsanız, bunun yerine [seri konsolda maskelenebilir olmayan kesme (NMI) çağrıları](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows#use-the-serial-console-for-nmi-calls) kullanmak isteyebilirsiniz. NMI çağrılarını kullanarak [bir çekirdek veya tam kilitlenme dökümü](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump) dosyası oluşturmak için kılavuzu izleyebilirsiniz.
