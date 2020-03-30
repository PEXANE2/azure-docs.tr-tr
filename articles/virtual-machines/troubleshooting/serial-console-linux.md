---
title: Linux için Azure Seri Konsolu | Microsoft Dokümanlar
description: Azure Sanal Makineler ve Sanal Makine Ölçek Setleri için Çift Yönlü Seri Konsol.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
manager: borisb
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 5/1/2019
ms.author: alsin
ms.openlocfilehash: b1f7708c9bd213e201ba4eb8837a191dca68ca9e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77167009"
---
# <a name="azure-serial-console-for-linux"></a>Linux için Azure Serial Console

Azure portalındaki Seri Konsol, Linux sanal makineleri (VM' ler) ve sanal makine ölçeği seti örnekleri için metin tabanlı bir konsola erişim sağlar. Bu seri bağlantısı VM veya sanal makine ölçeği kümesi örneğinin ttys0 seri bağlantı noktasına bağlanır ve ağa veya işletim sistemi durumundan bağımsız olarak erişim sağlar. Seri konsola yalnızca Azure portalı kullanılarak erişilebilir ve yalnızca Katılımcı veya VM veya sanal makine ölçeği kümesine erişim rolü olan veya daha yüksek olan kullanıcılar için izin verilir.

Seri Konsol, VM'ler ve sanal makine ölçeği seti örnekleri için aynı şekilde çalışır. Bu dokümanda, aksi belirtilmedikçe, VM'lere yapılan tüm sözlerle sanal makine ölçeği seti örnekleri dolaylı olarak yer alacaktır.

Windows için Seri Konsol belgeleri [için Windows için Seri Konsol'a](../windows/serial-console.md)bakın.

> [!NOTE]
> Seri Konsol genellikle genel Azure bölgelerinde ve Azure Resmi'nde genel önizlemede kullanılabilir. Azure Çin bulutunda henüz kullanıma sunulmadı.


## <a name="prerequisites"></a>Ön koşullar

- VM veya sanal makine ölçeği set örneğiniz kaynak yönetimi dağıtım modelini kullanmalıdır. Klasik dağıtımlar desteklenmez.

- Seri konsol kullanan hesabınız, VM ve [önyükleme tanılama](boot-diagnostics.md) depolama hesabı için [Sanal Makine Katılımcısı rolüne](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) sahip olmalıdır

- VM veya sanal makine ölçeği set örneğinizin parola tabanlı bir kullanıcısı olmalıdır. VM erişim uzantısı sıfırlama [parola](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) işlevi ile bir oluşturabilirsiniz. Destek + sorun **giderme** bölümünden **parolayı sıfırla'yı** seçin.

- VM veya sanal makine ölçeği set örneğinizin [önyükleme tanılama özelliği](boot-diagnostics.md) etkin leştirilmiş olmalıdır.

    ![Önyükleme tanılama ayarları](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

- Linux dağıtımlarına özel ayarlar için [Seri konsol Linux dağıtım kullanılabilirliğine](#serial-console-linux-distribution-availability)bakın.

- VM veya sanal makine ölçeği set örneğiniz üzerinde `ttys0`seri çıktı için yapılandırılmalıdır. Bu, Azure resimleri için varsayılan değerdir, ancak bunu özel görüntülerde iki kez denetlemek istersiniz. Ayrıntılar [aşağıda](#custom-linux-images).


> [!NOTE]
> Seri konsol, yapılandırılmış bir parolaya sahip yerel bir kullanıcı gerektirir. Yalnızca Bir SSH ortak anahtarıyla yapılandırılan VM'ler veya sanal makine ölçeği kümeleri seri konsolda oturum açamaz. Parolalı yerel bir kullanıcı oluşturmak için, Azure portalında Parolayı **Sıfırla'yı** seçerek portalda bulunan [VMAccess Uzantısı'nı](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension)kullanın ve parolalı yerel bir kullanıcı oluşturun.
> Ayrıca, [tek kullanıcı moduna önyükleme için GRUB kullanarak](./serial-console-grub-single-user-mode.md)hesabınızdaki yönetici parolasını sıfırlayabilirsiniz.

## <a name="serial-console-linux-distribution-availability"></a>Seri Konsol Linux dağıtım durumu
Seri konsolun düzgün çalışması için konuk işletim sisteminin konsol iletilerini seri bağlantı noktasına okuyup yazacak şekilde yapılandırılması gerekir. Onaylanan çoğu [Azure Linux dağıtımında](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) seri konsol varsayılan olarak yapılandırılır. Azure portalının **Destek + sorun giderme** bölümünde Seri **konsolu** seçilmesi seri konsoluna erişim sağlar.

> [!NOTE]
> Seri konsolunda bir şey görmüyorsanız, VM'nizde önyükleme tanılamanın etkin olduğundan emin olun. **Enter** tuşuna basmak genellikle seri konsolda hiçbir şeyin görünmediği sorunları giderir.

Dağıtım      | Seri konsol erişimi
:-----------|:---------------------
Red Hat Enterprise Linux    | Varsayılan olarak seri konsol erişimi etkindir.
CentOS      | Varsayılan olarak seri konsol erişimi etkindir.
Debian      | Varsayılan olarak seri konsol erişimi etkindir.
Ubuntu      | Varsayılan olarak seri konsol erişimi etkindir.
CoreOS      | Varsayılan olarak seri konsol erişimi etkindir.
SUSE        | Azure'da kullanılabilen yeni SLES görüntüleri varsayılan olarak seri konsol erişimine sahiptir. Azure'da SLES'in eski sürümlerini (10 veya daha eski) kullanıyorsanız, seri konsolu etkinleştirmek için [KB makalesine](https://www.novell.com/support/kb/doc.php?id=3456486) bakın.
Oracle Linux        | Varsayılan olarak seri konsol erişimi etkindir.

### <a name="custom-linux-images"></a>Özel Linux görüntüleri
Özel Linux VM görüntünüz için seri konsolu etkinleştirmek için, bir terminali çalıştırmak `ttyS0`için dosya */etc/inittab'daki* konsol erişimini etkinleştirin. Örneğin: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Ayrıca ttyS0 bir getty yumurtlamak gerekebilir. Bu yapılabilir `systemctl start serial-getty@ttyS0.service`.

Ayrıca seri çıktı için hedef olarak ttys0 eklemek isteyeceksiniz. Seri konsoluyla çalışacak özel bir görüntüyü yapılandırma hakkında daha fazla bilgi için Create'deki genel sistem gereksinimlerine bakın [ve Azure'da bir Linux VHD yükleyin.](https://aka.ms/createuploadvhd#general-linux-system-requirements)

Özel bir çekirdek oluşturuyorsanız, bu çekirdek bayraklarını etkinleştirmeyi `CONFIG_SERIAL_8250=y` düşünün: ve. `CONFIG_MAGIC_SYSRQ_SERIAL=y` Yapılandırma dosyası genellikle */boot/* yolunda bulunur.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Seri Konsola erişmek için sık karşılaşılan senaryolar

Senaryo          | Seri Konsoldaki Eylemler
:------------------|:-----------------------------------------
Kırık *FSTAB* dosyası | Devam etmek için **Enter** tuşuna basın ve *FSTAB* dosyasını düzeltmek için metin düzenleyicisini kullanın. Bunu yapmak için tek kullanıcı modunda olmanız gerekebilir. Daha fazla bilgi için [fstab sorunlarının nasıl giderilir](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) ve [GRUB ve tek kullanıcı moduna erişmek için seri konsolu kullanma](serial-console-grub-single-user-mode.md)nın seri konsol bölümüne bakın.
Yanlış güvenlik duvarı kuralları |  Iptables'ı SSH bağlantısını engelleyecek şekilde yapılandırıldıysanız, SSH'ye gerek kalmadan VM'inizle etkileşimkurmak için seri konsolu kullanabilirsiniz. Daha fazla bilgi [iptables man sayfasında](https://linux.die.net/man/8/iptables)bulunabilir.<br>Benzer şekilde, güvenlik duvarınız SSH erişimini engelliyorsa, Seri konsol aracılığıyla VM'ye erişebilir ve güvenlik duvarını yeniden yapılandırabilirsiniz. Daha fazla bilgi [güvenlik duvarılı belgelerde](https://firewalld.org/documentation/)bulunabilir.
Dosya sistemi bozulması/denetimi | Seri konsolu kullanarak bozuk dosya sistemleriyle ilgili sorun giderme hakkında daha fazla bilgi için dosya sistemi hataları nedeniyle Azure Linux VM'nin seri konsol bölümünün [başlatılamaması](https://support.microsoft.com/en-us/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) bölümüne bakın.
SSH yapılandırma sorunları | Seri konsoluna erişin ve ayarları değiştirin. Seri konsol, VM'nin ağ bağlantısının çalışmasını gerektirmediği için VM'nin SSH yapılandırmasına bakılmaksızın kullanılabilir. Bir [Azure Linux VM'nin Sorun Giderme SSH bağlantılarında](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-ssh-connection)başarısız olan, hatalar dan çıkan veya reddedilen bir sorun giderme kılavuzu kullanılabilir. [Azure'da Bir Linux VM'ye bağlanan sorunlar için Ayrıntılı SSH sorun giderme adımlarında](./detailed-troubleshoot-ssh-connection.md) daha fazla ayrıntı mevcuttur
Bootloader ile etkileşim | Linux VM'nizde GRUB'a erişmek için VM'inizi seri konsol bıçağının içinden yeniden başlatın. Daha fazla ayrıntı ve dağıtıma özgü bilgiler [için GRUB'a ve tek kullanıcı moduna erişmek için seri konsolu kullan'a](serial-console-grub-single-user-mode.md)bakın.

## <a name="disable-the-serial-console"></a>Seri Konsolu devre dışı

Varsayılan olarak, tüm aboneliklerin seri konsol erişimi etkindir. Seri konsolu abonelik düzeyinde veya VM/sanal makine ölçeği ayar düzeyinde devre dışı bırakabilirsiniz. Ayrıntılı talimatlar için [Azure Seri Konsolu Etkinleştir'i](./serial-console-enable-disable.md)ve devre dışı kaltın'ı ziyaret edin.

## <a name="serial-console-security"></a>Seri konsol güvenliği

### <a name="access-security"></a>Erişim güvenliği
Seri konsola erişim, [Sanal Makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) veya sanal makineye daha yüksek erişim rolü ne sahip kullanıcılarla sınırlıdır. Azure Etkin Dizin kiracınız çok faktörlü kimlik doğrulaması (MFA) gerektiriyorsa, seri konsolunuzun erişimi [Azure portalı](https://portal.azure.com)üzerinden olduğu için seri konsola erişim de MFA'ya ihtiyaç duyar.

### <a name="channel-security"></a>Kanal güvenliği
İleri geri gönderilen tüm veriler tel üzerinde şifrelenir.

### <a name="audit-logs"></a>Denetim günlükleri
Seri konsola tüm erişim şu anda sanal makinenin [önyükleme tanılama](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) günlüklerinde günlüğe kaydedilir. Bu günlüklere erişim Azure sanal makine yöneticisine aittir ve denetlenir.

> [!CAUTION]
> Konsol için erişim parolası günlüğe kaydedilmez. Ancak, konsol içinde çalıştırılan komutlar parolalar, sırlar, kullanıcı adları veya kişisel olarak tanımlanabilir bilgilerin (PII) başka bir biçimini içeriyorsa veya çıktıalıyorsa, bunlar VM önyükleme tanılama günlüklerine yazılır. Seri konsolun kaydırma geri kaydırma işlevinin uygulanmasının bir parçası olarak diğer tüm görünür metinle birlikte yazılır. Bu günlükler daireseldir ve yalnızca tanılama depolama hesabına okuma izni olan kişiler bunlara erişebilir. Sırlar veya KIŞISEL bilgiler içeren herhangi bir veri veya komut giriyorsanız, seri konsol kesinlikle gerekli olmadığı sürece SSH kullanmanızı öneririz.

### <a name="concurrent-usage"></a>Eşzamanlı kullanım
Bir kullanıcı seri konsoluna bağlıysa ve başka bir kullanıcı aynı sanal makineye başarıyla erişim isterse, ilk kullanıcının bağlantısı kesilir ve ikinci kullanıcı aynı oturuma bağlanır.

> [!CAUTION]
> Bu, bağlantısı kesilen bir kullanıcının oturumukapatılamayacağı anlamına gelir. Bağlantıyı kesme üzerine bir oturum açma yı (SIGHUP veya benzeri mekanizmayı kullanarak) uygulama özelliği hala yol haritasındadır. Windows için Özel Yönetim Konsolu'nda (SAC) otomatik bir zaman ayarı etkindir; ancak, Linux için terminal zaman aşım ayarını yapılandırabilirsiniz. Bunu yapmak için `export TMOUT=600` konsolda oturum açmak için kullandığınız kullanıcı için *.bash_profile* veya *.profile* dosyanızı ekleyin. Bu ayar, oturumu 10 dakika sonra zamana göre ayarlar.

## <a name="accessibility"></a>Erişilebilirlik
Erişilebilirlik, Azure Seri Konsolu için önemli bir odak noktasıdır. Bu amaçla, seri konsolun tamamen erişilebilir olmasını sağladık.

### <a name="keyboard-navigation"></a>Klavye ile gezinme
Azure portalından seri konsol arabiriminde gezinmek için klavyenizdeki **Sekme** tuşunu kullanın. Konumunuz ekranda vurgulanır. Seri konsol penceresinin odağında bırakmak için klavyenizdeki **Ctrl**+**F6** tuşuna basın.

### <a name="use-serial-console-with-a-screen-reader"></a>Seri Konsolu ekran okuyucuyla kullanma
Seri konsolyerleşik ekran okuyucu desteğine sahiptir. Ekran okuyucu açıkken gezinmek, şu anda seçili olan düğmenin alt metninin ekran okuyucu tarafından yüksek sesle okunmasını sağlar.

## <a name="known-issues"></a>Bilinen sorunlar
Seri konsol ve VM'nin işletim sistemiyle ilgili bazı sorunların farkındayız. Linux VM'leri için bu sorunların ve azaltma adımlarının bir listesi aşağıda verilmiştir. Bu sorunlar ve azaltıcı etkenler hem VM'ler hem de sanal makine ölçeği kümesi örnekleri için geçerlidir. Bunlar gördüğünüz hatayla eşleşmiyorsa, [Ortak Seri Konsol hatalarında](./serial-console-errors.md)sık karşılaşılan seri konsol hizmet hatalarını görün.

Sorun                           |   Risk azaltma
:---------------------------------|:--------------------------------------------|
Bağlantı başlığından sonra **Enter** tuşuna bastığınızda oturum açma istemi görüntülenmez. | GRUB doğru yapılandırılamayabilir. Aşağıdaki komutları çalıştırın: `grub2-mkconfig -o /etc/grub2-efi.cfg` `grub2-mkconfig -o /etc/grub2.cfg`ve/veya . Daha fazla bilgi için [bkz.](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md) Linux'un seri bağlantı noktasına bağlanmamasına neden olan özel bir VM, sertleştirilmiş cihaz veya GRUB config çalıştırıyorsanız bu sorun oluşabilir.
Seri konsol metni yalnızca ekran boyutunun bir kısmını kaplar (genellikle metin düzenleyicisi kullandıktan sonra). | Seri konsollar pencere boyutu[(RFC 1073)](https://www.ietf.org/rfc/rfc1073.txt)ile ilgili görüşmeleri desteklemez, bu da ekran boyutunu güncelleştirmek için gönderilen SIGWINCH sinyali olmayacağı ve VM'nin terminalinizin boyutu hakkında hiçbir bilgisi olmayacağı anlamına gelir. Komutu sağlamak için xterm veya `resize` benzer bir yardımcı `resize`program yükleyin ve sonra çalıştırın.
Uzun dizeleri yapıştırma işe yaramaz. | Seri konsol, seri bağlantı noktası bant genişliğinin aşırı yüklenmesini önlemek için terminale yapıştırılan dizelerin uzunluğunu 2048 karakterle sınırlar.
SLES BYOS görüntülerinde hatalı klavye girişi. Klavye girişi yalnızca ara sıra tanınır. | Bu Plymouth paketi ile ilgili bir sorundur. Bir sıçrama ekranına ihtiyacınız olmadığı ve Plymouth'un Seri Konsol'u kullanma platformunun yeteneğini engellemesi nedeniyle Plymouth Azure'da çalıştırılmamalıdır. Plymouth'u `sudo zypper remove plymouth` çıkarın ve yeniden başlatın. Alternatif olarak, satırın sonuna ekleyerek `plymouth.enable=0` GRUB config çekirdek satırını değiştirin. Bunu [önyükleme sırasında önyükleme girişini düzenleyerek](https://aka.ms/serialconsolegrub#single-user-mode-in-suse-sles)veya GRUB_CMDLINE_LINUX satırını `/etc/default/grub`düzenleyerek, `grub2-mkconfig -o /boot/grub2/grub.cfg`GRUB ile yeniden oluşturarak ve sonra yeniden başlatarak yapabilirsiniz.


## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S. Nasıl geribildirim gönderebilirim?**

A. Bir GitHub sorunu oluşturarak https://aka.ms/serialconsolefeedbackgeri bildirim sağlayın. Alternatif olarak (daha az tercih azserialhelp@microsoft.com edilen), geribildirim gönderebilirsiniz üzerinden veya sanal makine kategorisinde https://feedback.azure.com.

**S. Seri konsol kopyalama/yapıştır'ı destekliyor mu?**

A. Evet. Terminale kopyalamak ve yapıştırmak için **Ctrl**+**Shift**+**C** ve **Ctrl**+**Shift**+**V'yi** kullanın.

**S. SSH bağlantısı yerine seri konsol kullanabilir miyim?**

A. Bu kullanım teknik olarak mümkün gibi görünse de, seri konsolun öncelikle SSH üzerinden bağlantının mümkün olmadığı durumlarda sorun giderme aracı olarak kullanılması amaçlanmıştır. Aşağıdaki nedenlerden dolayı seri konsolun SSH yerine kullanılmasına karşı çıkmanızı öneririz:

- Seri konsol, SSH kadar bant genişliğine sahip değildir. Yalnızca metin bağlantısı olduğundan, gui ağırlıklı etkileşimler zordur.
- Seri konsol erişimi şu anda yalnızca bir kullanıcı adı ve parola kullanılarak mümkündür. SSH tuşları, oturum açma güvenliği açısından kullanıcı adı/parola birleşimlerinden çok daha güvenli olduğundan, Seri konsol üzerinden SSH'yi öneririz.

**S. Aboneliğim için seri konsolu kimler etkinleştirebilir veya devre dışı bırakabilir?**

A. Seri konsolu abonelik genelinde bir düzeyde etkinleştirmek veya devre dışı kardık yapmak için, abonelığa yazma izinleriniz olması gerekir. Yazma izni olan roller yönetici veya sahip rollerini içerir. Özel rollerde yazma izinleri de olabilir.

**S. VM/sanal makine ölçek setim için seri konsola kimler erişebilir?**

A. Seri konsola erişmek için VM veya sanal makine ölçeği ayarlı sanal makine katılımcısı rolüne veya daha yüksek olması gerekir.

**S. Benim seri konsol bir şey görüntülemiyor, ne yapmalıyım?**

A. Resminiz büyük olasılıkla seri konsol erişimi için yanlış yapılandırılmıştır. Seri konsolu etkinleştirmek için resminizi yapılandırma hakkında daha fazla bilgi için [Seri konsol Linux dağıtım kullanılabilirliğine](#serial-console-linux-distribution-availability)bakın.

**S. Seri konsol sanal makine ölçek kümeleri için kullanılabilir mi?**

A. Evet öyle! [Sanal Makine Ölçek Setleri için Seri Konsol'a](serial-console-overview.md#serial-console-for-virtual-machine-scale-sets) bakın

**S. VM veya sanal makine ölçeğimi yalnızca SSH anahtar kimlik doğrulamasını kullanarak ayarlarsam, VM/sanal makine ölçeği set örneğime bağlanmak için seri konsolu kullanmaya devam edebilir miyim?**

A. Evet. Seri konsolSSH tuşları gerektirmediği için, yalnızca bir kullanıcı adı/parola kombinasyonu ayarlamanız yeterlidir. Bunu, Azure portalında **parolayı sıfırla'yı** seçerek ve seri konsolda oturum açtırmak için bu kimlik bilgilerini kullanarak yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [GRUB ve tek kullanıcı moduna erişmek](serial-console-grub-single-user-mode.md)için seri konsolu kullanın.
* [NMI ve SysRq aramaları](serial-console-nmi-sysrq.md)için seri konsolu kullanın.
* [Çeşitli dağıtımlarda GRUB'u etkinleştirmek](serial-console-grub-proactive-configuration.md) için seri konsolu nasıl kullanacağınızı öğrenin
* Seri konsol [Windows VM'ler](../windows/serial-console.md)için de kullanılabilir.
* [Önyükleme tanılama](boot-diagnostics.md)hakkında daha fazla bilgi edinin.

