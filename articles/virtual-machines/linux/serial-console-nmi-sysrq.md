---
title: SysRq ve NMI aramaları için Azure Seri Konsolu
description: Azure sanal makinelerde SysRq ve NMI aramaları için Seri Konsol kullanma.
author: asinn826
ms.service: virtual-machines-linux
ms.topic: article
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 5541dec748f31818a0e9485fc0c56b7926ccaae7
ms.sourcegitcommit: 31e9f369e5ff4dd4dda6cf05edf71046b33164d3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81758493"
---
# <a name="use-serial-console-for-sysrq-and-nmi-calls"></a>SysRq ve NMI aramaları için Seri Konsol'u kullanma

## <a name="system-request-sysrq"></a>Sistem İsteği (SysRq)
SysRq, Linux işletim sistemi çekirdeği tarafından anlaşılan ve önceden tanımlanmış bir dizi eylemi tetiklenebilen bir dizi anahtardizisidir. Bu komutlar genellikle sanal makine sorun giderme veya kurtarma geleneksel yönetim yoluyla gerçekleştirilemediğinde (örneğin, VM yanıt vermiyorsa) kullanılır. Azure Seri Konsolunun SysRq özelliğini kullanmak, SysRq tuşuna basarak fiziksel klavyeye girilen karakterleri taklit eder.

SysRq sırası teslim edildikten sonra çekirdek yapılandırması sistemin nasıl yanıt vereceğini denetler. SysRq'u etkinleştirme ve devre dışı bırakma hakkında daha fazla bilgi için *SysRq Yönetici Kılavuzu* [metin](https://aka.ms/kernelorgsysreqdoc) | [işaretleme](https://aka.ms/linuxsysrq)sine bakın.  

Azure Seri Konsolu, aşağıda gösterilen komut çubuğundaki klavye simgesini kullanarak Azure sanal makinesine SysRq göndermek için kullanılabilir.

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-command-menu.jpg)

"SysRq Komutu Gönder" seçeneğinin seçilmesi, ortak SysRq seçenekleri sağlayacak veya iletişim e girilen SysRq komutlarının bir dizisini kabul edecek bir iletişim kutusu açar.  Bu SysRq serisi için güvenli bir yeniden başlatma kullanarak gibi üst düzey `REISUB`bir işlem gerçekleştirmek için izin verir: .

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-sysreq_UI.png)

SysRq komutu durdurulan veya çekirdeği yanıt vermeyen durumda olan sanal makinelerde kullanılamaz. (örneğin bir çekirdek panik).

### <a name="enable-sysrq"></a>SysRq'u etkinleştir 
Yukarıdaki *SysRq Yönetici Kılavuzu'nda* açıklandığı gibi, SysRq tüm, hiçbiri veya sadece belirli komutları kullanılabilir şekilde yapılandırılabilir. Aşağıdaki adımı kullanarak tüm SysRq komutlarını etkinleştirebilirsiniz, ancak yeniden başlatmadan sağ çıkamaz:
```
echo "1" >/proc/sys/kernel/sysrq
```
SysReq yapılandırmasını kalıcı hale getirmek için, tüm SysRq komutlarını etkinleştirmek için aşağıdakileri yapabilirsiniz
1. */etc/sysctl.conf* bu satırı ekleme <br>
    `kernel.sysrq = 1`
1. Çalıştırarak sysctl'yi yeniden başlatma veya güncelleştirme <br>
    `sysctl -p`

### <a name="command-keys"></a>Komut Tuşları 
Yukarıdaki SysRq Yönetici Kılavuzu Gönderen:

|Komut| İşlev
| ------| ----------- |
|``b``  |   Disklerinizi eşitlemeden veya sökmeden sistemi hemen yeniden başlatAcaktır.
|``c``  |   NULL işaretçisi dereference tarafından bir sistem çökmesi gerçekleştirecek. Yapılandırılırsa bir çökme dökümü alınır.
|``d``  |   Tutulan tüm kilitleri gösterir.
|``e``  |   Init hariç tüm işlemlere bir SIGTERM gönderin.
|``f``  |   Bir bellek domuz süreci öldürmek için oom katil arayacak, ama hiçbir şey öldürülebilir eğer panik yok.
|``g``  |   kgdb (çekirdek hata ayıklama) tarafından kullanılır
|``h``  |   Yardım görüntüler (burada listelenenler dışında başka bir anahtar ``h`` da yardım görüntüler, ancak hatırlamak kolaydır :-)
|``i``  |    Init hariç tüm işlemlere bir SIGKILL gönderin.
|``j``  |    Zorla "Sadece çözülme" - filesystems FIFREEZE ioctl tarafından dondurulmuş.
|``k``  |    Secure Access Key (SAK) Geçerli sanal konsoldaki tüm programları öldürür. NOT: SAK bölümünde aşağıdaki önemli yorumlara bakın.
|``l``  |    Tüm etkin CPU'lar için bir yığın arka izi gösterir.
|``m``  |    Geçerli bellek bilgilerini konsolunuza atar.
|``n``  |    RT görevlerini güzel bir şekilde yapabilmek için kullanılır
|``o``  |    Sisteminizi kapatır (yapılandırılır ve desteklenirse).
|``p``  |    Geçerli kayıtları ve bayrakları konsolunuza atar.
|``q``  |    Tüm silahlı hrtimers (ama not düzenli timer_list zamanlayıcıları) ve tüm clockevent cihazlar hakkında ayrıntılı bilgi CPU listeleri başına dökümü.
|``r``  |    Klavye nin ham modunu kapatır ve XLATE'ye ayarlar.
|``s``  |    Tüm monte edilmiş dosya sistemlerini eşitlemeye çalışır.
|``t``  |    Geçerli görevlerin ve bilgilerin listesini konsolunuza bırakır.
|``u``  |    Yalnızca okunan tüm monte edilmiş dosya sistemlerini yeniden monte etmeye çalışacaktır.
|``v``  |    Framebuffer konsolu zorla geri yüklenir
|``v``  |    ETM arabellek dökümü [ARM'a özgü] neden olur
|``w``  |    Kesintisiz (engellenen) durumda olan görevleri boşaltıyor.
|``x``  |    Ppc / powerpc platformlarda xmon arayüzü tarafından kullanılır. Sparc64'te global PMU Registers'ı göster. Tüm TLB girişlerini MIPS'e boşaltın.
|``y``  |    Genel CPU Kayıtları [SPARC-64'e özgü]
|``z``  |    Ftrace arabelleği dökümü
|``0``-``9`` | Konsolunuza hangi çekirdek iletilerinin yazdırılacağını kontrol ederek konsol günlük düzeyini ayarlar. (``0``örneğin, yalnızca PANIC'ler veya OOP'lar gibi acil durum iletileri konsolunuza uyabilecek şekilde yapar.)

### <a name="distribution-specific-documentation"></a>Dağıtıma özel belgeler ###
SysRq'da dağıtıma özgü belgeler ve SysRq "Crash" komutu aldığında bir çökme dökümü oluşturmak üzere Linux'u yapılandırma adımları için aşağıdaki bağlantılara bakın:

#### <a name="ubuntu"></a>Ubuntu ####
 - [Çekirdek Crash Dökümü](https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html)

#### <a name="red-hat"></a>Red Hat ####
- [SysRq Tesisi nedir ve nasıl kullanırım?](https://access.redhat.com/articles/231663)
- [RhEL sunucusundan bilgi toplamak için SysRq tesisini nasıl kullanılır?](https://access.redhat.com/solutions/2023)

#### <a name="suse"></a>SUSE ####
- [Çekirdek çekirdek döküm yakalama yapılandırma](https://www.suse.com/support/kb/doc/?id=3374462)

#### <a name="coreos"></a>CoreOS ####
- [Kilitlenme günlüklerini toplama](https://coreos.com/os/docs/latest/collecting-crash-logs.html)

## <a name="non-maskable-interrupt-nmi"></a>Maskelenebilir Olmayan Kesme (NMI) 
Maskelenmeyen bir kesme (NMI), sanal makinedeki yazılımın göz ardı edilmeyeceğini belirten bir sinyal oluşturmak üzere tasarlanmıştır. Tarihsel olarak, NMI'ler belirli yanıt süreleri gerektiren sistemlerdeki donanım sorunlarını izlemek için kullanılmıştır.  Bugün, programcılar ve sistem yöneticileri genellikle yanıt vermeyen sistemleri hata ayıklamak veya sorun giderme mekanizması olarak NMI kullanır.

Seri Konsol, aşağıda gösterilen komut çubuğundaki klavye simgesini kullanarak Azure sanal makinesine NMI göndermek için kullanılabilir. NMI teslim edildikten sonra, sanal makine yapılandırması sistemin nasıl yanıt vereceğini denetler.  Linux işletim sistemleri çökmesine ve işletim sistemi bir NMI alır bir bellek dökümü oluşturmak için yapılandırılabilir.

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-command-menu.jpg) <br>

Çekirdek parametrelerini yapılandırmak için sysctl destekleyen Linux sistemleri için, aşağıdakileri kullanarak bu NMI'yi alırken paniğe neden olabilirsiniz:
1. */etc/sysctl.conf* bu satırı ekleme <br>
    `kernel.panic_on_unrecovered_nmi=1`
1. Çalıştırarak sysctl'yi yeniden başlatma veya güncelleştirme <br>
    `sysctl -p`

Linux çekirdek yapılandırmaları hakkında daha fazla `unknown_nmi_panic`bilgi `panic_on_io_nmi`için `panic_on_unrecovered_nmi`, , ve , bakınız: [/proc/sys/kernel/* için dokümantasyon.](https://www.kernel.org/doc/Documentation/sysctl/kernel.txt) NMI'deki dağıtıma özgü belgeler ve NMI aldığında çökme dökümü oluşturacak şekilde Linux'u yapılandırma adımları için aşağıdaki bağlantılara bakın:
 
### <a name="ubuntu"></a>Ubuntu 
 - [Çekirdek Crash Dökümü](https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html)

### <a name="red-hat"></a>Red Hat 
 - [NMI nedir ve ne için kullanabilirim?](https://access.redhat.com/solutions/4127)
 - [NMI anahtarı itildiğinde sistemimi kilitlenme olacak şekilde nasıl yapılandırabilirim?](https://access.redhat.com/solutions/125103)
 - [Crash Dump Admin Kılavuzu](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/pdf/kernel_crash_dump_guide/kernel-crash-dump-guide.pdf)

### <a name="suse"></a>SUSE 
- [Çekirdek çekirdek döküm yakalama yapılandırma](https://www.suse.com/support/kb/doc/?id=3374462)

### <a name="coreos"></a>CoreOS 
- [Kilitlenme günlüklerini toplama](https://coreos.com/os/docs/latest/collecting-crash-logs.html)

## <a name="next-steps"></a>Sonraki adımlar
* Ana Seri Konsol Linux dokümantasyon sayfası [burada](serial-console.md)yer almaktadır.
* GRUB'a önyükleme yapmak ve tek kullanıcı moduna girmek için Seri [Konsol'u](serial-console-grub-single-user-mode.md) kullanın
* Seri Konsol, [Windows](../windows/serial-console.md) VM'ler için de kullanılabilir
* [Önyükleme tanılama](boot-diagnostics.md) hakkında daha fazla bilgi edinin