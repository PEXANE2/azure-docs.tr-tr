---
title: "' Grup ilkesi yerel kullanıcılar & grupları ' ilkesi uygulanırken sanal makine yanıt vermiyor"
description: Bu makalede, bir Azure sanal makinesinde (VM) önyükleme sırasında bir ilke uygulamak için yükleme ekranının takılmasına neden olan sorunları gidermeye yönelik adımlar sağlanmaktadır.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80620863"
---
# <a name="virtual-machine-is-unresponsive-while-applying-group-policy-local-users--groups-policy"></a>' Grup ilkesi yerel kullanıcılar & grupları ' ilkesi uygulanırken sanal makine yanıt vermiyor

Bu makalede, bir Azure sanal makinesinde (VM) önyükleme sırasında bir ilke uygulamak için yükleme ekranının takılı olduğu sorunları çözme adımları sağlanmaktadır.

## <a name="symptom"></a>Belirti

VM 'nin ekran görüntüsünü görüntülemek için [önyükleme tanılamayı](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/boot-diagnostics) kullandığınızda, ekranın şu iletiyle takılmış olduğunu görürsünüz: *Grup ilkesi yerel kullanıcılar ve Gruplar ilkesi uygulanıyor*.

![Alternatif metin: grup ilkesi yerel kullanıcılar ve Gruplar ilkesini yüklemeyi gösteren ekran (Windows Server 2012).](media/troubleshoot-vm-unresponsive-group-policy-local-users/1.png)

Windows Server 2012

![Alternatif metin: grup ilkesi yerel kullanıcılar ve Gruplar ilkesini yüklemeyi gösteren ekran (Windows Server 2012 R2).](media/troubleshoot-vm-unresponsive-group-policy-local-users/2.png)

Windows Server 2012 R2

## <a name="cause"></a>Nedeni

Bu dondurma belirtilerinin nedeni, Windows profil hizmeti dinamik bağlantı kitaplığındaki (*ProfSvc. dll*) bir kod hatası nedeniyle oluşur.

> [!NOTE]
> Bu hata yalnızca Windows Server 2012 ve Windows Server 2012 R2 'de geçerlidir.

### <a name="the-policy-in-question"></a>Söz konusu ilke

Uygulanmakta olan ilke, işlemini tamamlayamayacak:

* *Bilgisayar Yapılandırması \ Yönetim Şablonları \ sistem/Kullanıcı profiller sistem yeniden başlatıldığında belirtilen gün sayısından daha eski Kullanıcı profillerini Sil*

Bu ilke yalnızca aşağıdaki altı koşulun doğru olması durumunda askıda kalır:

* *Sistem yeniden başlatma ilkesinde belirtilen gün sayısından daha eski Kullanıcı profillerini Sil* özelliği etkinleştirilmiştir.
* Temizlenmesi gereken yaş gereksinimlerini karşılayan profilleriniz var.
* Profiller için silme bildirimine kayıtlı bileşenleriniz var.
* Bileşenler, Windows 'un Service Control Manager (SCM) bileşenlerinden veri almaları gereken (doğrudan veya dolaylı), hizmet hakkındaki bilgileri başlatma, durdurma veya sorgulama gibi çağrılar yapar.
* *Otomatik*olarak başlayacak şekilde yapılandırılmış bir hizmetiniz var.
* Bu hizmet, bir etki alanı hesabı bağlamı altında çalışacak şekilde ayarlanır (yerel sistem gibi yerleşik bir hesap kullanmanın aksine).

### <a name="the-code-defect"></a>Kod hatası

Kod hatası, hizmet Denetim Yöneticisi (SCM) ve aynı anda bir diğeri üzerinde kilit uygulamaya çalışan profil hizmetlerinden kaynaklanır. Birden fazla hizmetin aynı anda aynı verilerde değişiklik yapmasını engellemek için kilitler vardır ve bu durum bozulmaya neden olur. Genellikle, birden fazla kilit isteği bir soruna neden olmaz. Ancak, bu önyükleme sırasında gerçekleşmekte olduğundan, bir diğeri üzerinde beklerken, hiçbir hizmetin işlemleri tamamlanamazlar.

### <a name="os-bug-5880648---service-control-manager-deadlocks-with-the-delete-user-profiles-on-restart-policy"></a>İşletim sistemi hatası 5880648-"yeniden başlatmada Kullanıcı profillerini Sil" ilkesinde hizmet denetimi Yöneticisi kilitlenmeler

Çakışan iki eylem vardır:

* 1. eylem, profil kilidini alır ancak henüz SCM kilidini almış değildir.

  **'**

* 2. işlem SCM kilidini alır, ancak henüz profil kilidini almış değildir.

Bu kilitlenme oluştuktan sonra, gereken ikinci kilidi alma girişimi eylemi askıda kalır.

### <a name="action-1---old-profile-deletion-notification-has-profile-lock-needs-scm-lock"></a>Eylem 1-eski profil silme bildirimi ( **profil kilidi**vardır, **SCM kilidi**gerekir)

1. İlk olarak, eski profilleri sil olarak ayarlanan ilke bir iç profil hizmeti kilidi alır.

   * Bu kilit, *silme işlemi* devam ederken iki iş parçacığının profillerle etkileşimini engellemek için gereklidir.

2. İlke, silinmek üzere yeterince eski olan profilleri bulur.
3. Profil silme işleminin bir parçası olarak, bir profilin silinmesinin bildirimleri için kaydedilen bir bileşen **bir hizmeti başlatmaya**çalışır.
4. Hizmeti başlatmadan önce, hizmet Denetim Yöneticisi (SCM), **işlem 2**' deki iş parçacıkları tarafından BIR **iç SCM kilidi** almaları gerekir.

### <a name="action-2---profile-loadcreation-for-user-specific-data-has-scm-lock-needs-profile-lock"></a>Eylem 2-kullanıcıya özgü veriler için profil yükleme/oluşturma ( **SCM kilidine**sahip, **profil kilidi**gerekir)

1. Önyüklemede, SCM 'nin tüm *otomatik başlatma* hizmetlerini kendi gruplarına göre ve bu hizmetlerin bağımlı olduğu tüm hizmetleri siparişi sağlaması gerekir.

2. SCM, hizmetleri sipariş eden Hizmetleri başlatma, durdurma veya yapılandırmaya erişimi denetlemek için kullanılan **bir Iç SCM kilidi** edinir.

3. Hizmetler sıraya alındıktan sonra, SCM her bir hizmet üzerinden döngü başlatır ve başlatılır.

4. Hizmet bir etki alanı hesabı bağlamında çalışıyorsa, kullanıcıya özgü verileri depolayabilmesi için bir profilin etki alanı hesabı için yüklenmesi veya oluşturulması gerekir.

5. Bu istek **profil hizmetine**gönderilir.

6. Profil hizmeti, **1. işlem**içinde **iç kilit** elde etmek için erişim gerektirir.

## <a name="solution"></a>Çözüm

### <a name="process-overview"></a>İşleme genel bakış

1. Bir onarım VM 'si oluşturma ve erişme
2. Seri konsolu ve bellek dökümü toplamayı etkinleştir
3. VM 'yi yeniden oluşturma
4. Bellek dökümü dosyasını toplayın

   > [!NOTE]
   > Bu önyükleme hatasıyla karşılaşıldığında, Konuk işletim sistemi çalışır durumda değildir. Bu sorunu çözmek için çevrimdışı modda sorun gidermeye başlayacaksınız.

### <a name="create-and-access-a-repair-vm"></a>Bir onarım VM 'si oluşturma ve erişme

1. Bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3 adımlarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.
2. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

### <a name="enable-serial-console-and-memory-dump-collection"></a>Seri konsolu ve bellek dökümü toplamayı etkinleştir

Bellek dökümü toplamayı ve seri konsolunu etkinleştirmek için aşağıdaki betiği çalıştırın:

1. Yükseltilmiş bir komut istemi oturumu açın (yönetici olarak çalıştır).
2. Aşağıdaki komutları çalıştırın:

   * Seri konsolunu etkinleştir:

     `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /ems {<BOOT LOADER IDENTIFIER>} ON`

     `bcdedit /store <VOLUME LETTER WHERE THE BCD FOLDER IS>:\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

3. İşletim sistemi diskindeki boş alanın, VM 'deki bellek boyutu (RAM) kadar olduğundan emin olun.

   * İşletim sistemi diskinde yeterli alan yoksa, bellek dökümü dosyasının oluşturulacağı konumu değiştirmeniz ve yeterli boş alana sahip olan VM 'ye bağlı tüm veri diskine başvurmanız gerekir. Konumu değiştirmek için, aşağıdaki komutlarda `%SystemRoot%` veri diskinin sürücü harfiyle değiştirin ("F:" gibi).

#### <a name="suggested-configuration-to-enable-os-dump"></a>İşletim sistemi dökümünü etkinleştirmek için önerilen yapılandırma

**Bozuk işletim sistemi diski yükle:**

`REG LOAD HKLM\BROKENSYSTEM <VOLUME LETTER OF BROKEN OS DISK>:\windows\system32\config\SYSTEM`

**ControlSet001 üzerinde etkinleştir:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet001\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

**ControlSet002 üzerinde etkinleştir:**

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 1 /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v DumpFile /t REG_EXPAND_SZ /d "%SystemRoot%\MEMORY.DMP" /f`

`REG ADD "HKLM\BROKENSYSTEM\ControlSet002\Control\CrashControl" /v NMICrashDump /t REG_DWORD /d 1 /f`

### <a name="rebuild-the-vm"></a>VM 'yi yeniden oluşturma

VM 'yi yeniden birleştirmek için [VM onarım komutlarının 5. adımını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) kullanın.

### <a name="collect-the-memory-dump-file"></a>Bellek dökümü dosyasını toplayın

Bu sorunu çözmek için öncelikle kilitlenme için bellek dökümü dosyasını toplamanız ve bellek döküm dosyası ile desteğe başvurmanız gerekir. Döküm dosyasını toplamak için aşağıdaki adımları izleyin:

#### <a name="attach-the-os-disk-to-a-new-repair-vm"></a>İşletim sistemi diskini yeni bir onarım VM 'sine iliştirme

1. Yeni bir onarım VM 'si hazırlamak için [VM onarım komutlarının 1-3](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/repair-windows-vm-using-azure-virtual-machine-repair-commands#repair-process-example) adımlarını kullanın.

2. Uzak Masaüstü Bağlantısı kullanarak, onarım VM 'ye bağlanın.

#### <a name="locate-the-dump-file-and-submit-a-support-ticket"></a>Döküm dosyasını bulma ve destek bileti gönderme

1. VM 'yi Onar sayfasında, bağlı işletim sistemi diskinde Windows klasörü ' ne gidin. Bağlı işletim sistemi diskine atanan sürücü harfi F ise, F:\windowsadresine gitmeniz gerekir.

2. Memory. dmp dosyasını bulun ve ardından bellek dökümü dosyası ile [bir destek bileti gönderebilirsiniz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .

3. Memory. dmp dosyasını bulmada sorun yaşıyorsanız, bunun yerine, [seri konsolundaki maskelenemeyen kesme (NMI) çağrılarını](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/serial-console-windows#use-the-serial-console-for-nmi-calls) kullanmak isteyebilirsiniz. NMI çağrılarını kullanarak [bir çekirdek veya bir kilitlenme bilgi döküm dosyası oluşturmak](https://docs.microsoft.com/windows/client-management/generate-kernel-or-complete-crash-dump) için Kılavuzu izleyebilirsiniz.
