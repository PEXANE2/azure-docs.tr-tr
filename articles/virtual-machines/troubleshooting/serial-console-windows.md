---
title: Windows için Azure seri konsol | Microsoft Docs
description: Azure sanal makineleri ve sanal makine ölçek kümeleri için iki yönlü seri konsol.
services: virtual-machines-windows
documentationcenter: ''
author: asinn826
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 5/1/2019
ms.author: alsin
ms.openlocfilehash: 87ccb1c4995337b385f685797980a9fc3962bc6f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75451251"
---
# <a name="azure-serial-console-for-windows"></a>Windows için Azure seri konsol

Azure portal seri konsol, Windows sanal makineleri (VM 'Ler) ve sanal makine ölçek kümesi örnekleri için metin tabanlı bir konsola erişim sağlar. Bu seri bağlantı, ağ veya işletim sistemi durumundan bağımsız olarak erişim sağlayan VM veya sanal makine ölçek kümesi örneğinin COM1 seri bağlantı noktasına bağlanır. Seri konsoluna yalnızca Azure portal kullanılarak erişilebilir ve yalnızca VM veya sanal makine ölçek kümesine katkıda bulunan veya daha yüksek bir erişim rolüne sahip olan kullanıcılar için izin verilir.

Seri konsol, VM 'Ler ve sanal makine ölçek kümesi örnekleri için aynı şekilde çalışmaktadır. Bu belgede, aksi belirtilmediği takdirde VM 'lerdeki tüm bahsetmeler, sanal makine ölçek kümesi örneklerini örtülü olarak içerir.

Linux için seri konsol belgeleri için bkz. [Linux Için Azure seri konsolu](serial-console-linux.md).

> [!NOTE]
> Seri konsol Genel Azure bölgelerinde ve Azure Kamu 'da genel önizlemede kullanılabilir. Henüz Azure Çin bulutu 'nda mevcut değildir.


## <a name="prerequisites"></a>Ön koşullar

* VM 'niz veya sanal makine ölçek kümesi örneğinizin kaynak yönetimi dağıtım modelini kullanması gerekir. Klasik dağıtımlar desteklenmez.

- Seri konsol kullanan hesabınızda VM ve [önyükleme tanılama](boot-diagnostics.md) depolama hesabı Için [sanal makine katılımcısı rolü](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) olmalıdır

- VM 'niz veya sanal makine ölçek kümesi örneğinizin parola tabanlı bir kullanıcısı olmalıdır. İle bir tane oluşturabilirsiniz [parolayı Sıfırla](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) işlevi VM erişimi uzantısı. Seçin **parolayı Sıfırla** gelen **destek + sorun giderme** bölümü.

* Sanal makine ölçek kümesi örneği için VM 'nin [önyükleme tanılaması](boot-diagnostics.md) etkinleştirilmiş olması gerekir.

    ![Önyükleme tanılama ayarları](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

## <a name="enable-serial-console-functionality-for-windows-server"></a>Windows Server için seri konsol işlevselliğini etkinleştir

> [!NOTE]
> Seri konsolda hiçbir şey görmüyorsanız, VM 'niz veya sanal makine ölçek kümesinde önyükleme tanılaması 'nın etkinleştirildiğinden emin olun.

### <a name="enable-the-serial-console-in-custom-or-older-images"></a>Seri konsol özel veya eski resimlerdeki etkinleştir
Azure 'da daha yeni Windows Server görüntülerinin varsayılan olarak etkin olan [Özel Yönetim Konsolu](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (sac) vardır. SAC Windows server sürümlerinde desteklenir, ancak (örneğin, Windows 10, Windows 8 veya Windows 7) istemci sürümlerinde kullanılamaz.

(Şubat 2018 tarihinden önce oluşturulan) daha eski Windows Server görüntüleri için otomatik olarak seri konsol üzerinden Azure portalının komutu çalıştır özelliğini etkinleştirebilirsiniz. Azure portal, **Çalıştır komutunu**seçin ve ardından listeden **enableems** adlı komutu seçin.

![Komut listesini çalıştırın.](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-runcommand.png)

Alternatif olarak, 2018 Şubat 'tan önce oluşturulan Windows VM 'Leri/sanal makine ölçek kümesinin seri konsolunu el ile etkinleştirmek için şu adımları izleyin:

1. Uzak Masaüstü'nü kullanarak Windows sanal makinenize bağlanın
1. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın:
    - `bcdedit /ems {current} on`
    - `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
1. SAC Konsolu etkinleştirilmesi için sistemi yeniden başlatın.

    ![SAC Konsolu](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Gerekirse, SAC çevrimdışı de etkinleştirilebilir:

1. Var olan VM'ye veri diski olarak yapılandırılmış SAC istediğiniz windows disk ekleyin.

1. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın:
   - `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
   - `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

#### <a name="how-do-i-know-if-sac-is-enabled"></a>SAC etkin olup olmadığını nasıl anlarım?

Varsa [SAC](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) değil etkin seri konsol SAC istemi görüntülemez. Bazı durumlarda, VM sistem durumu bilgileri görüntülenir ve diğer durumlarda boş olur. Şubat 2018 tarihinden önce oluşturulan bir Windows Server görüntüsü kullanıyorsanız, SAC büyük olasılıkla etkin olmayacaktır.

### <a name="enable-the-windows-boot-menu-in-the-serial-console"></a>Seri konsol içinde Windows önyükleme menüsünü etkinleştir

Windows önyükleme yükleyicisi istemleri seri konsolunda görüntülenecek etkinleştirmeniz gerekirse, aşağıdaki ek seçenekler önyükleme yapılandırma verilerinizi ekleyebilirsiniz. Daha fazla bilgi için [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set).

1. Uzak Masaüstü 'Nü kullanarak Windows sanal makinmenize veya sanal makine ölçek kümesi örneğine bağlanın.

1. Bir yönetim komut isteminden aşağıdaki komutları çalıştırın:
   - `bcdedit /set {bootmgr} displaybootmenu yes`
   - `bcdedit /set {bootmgr} timeout 10`
   - `bcdedit /set {bootmgr} bootems yes`

1. Önyükleme menüsünün etkinleştirilmesi sistemi yeniden başlatın

> [!NOTE]
> Önyükleme Yöneticisi menüsünü görüntülenecek kümesi zaman aşımı, işletim sistemi önyükleme süresini etkiler. 10 saniyelik zaman aşımı değeri çok kısa veya uzun olduğunu düşünüyorsanız, farklı bir değere ayarlayın.

## <a name="use-serial-console"></a>Seri konsol kullan

### <a name="use-cmd-or-powershell-in-serial-console"></a>Seri konsolunda CMD veya PowerShell kullanma

1. Seri konsoluna bağlanın. Başarıyla bağlanmanız durumunda, istemidir **SAC >** :

    ![SAC için Bağlan](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect-sac.png)

1.  Girin `cmd` CMD örneğine sahip bir kanal oluşturmak için.

1.  CMD örneğini çalıştıran kanala geçmek için `ch -si 1` girin veya `<esc>+<tab>` kısayol tuşlarına basın.

1.  Tuşuna **Enter**, yönetim izinleriyle oturum açma kimlik bilgilerini girin.

1.  Geçerli kimlik bilgilerini girdikten sonra CMD örneği açılır.

1.  Bir PowerShell örneği başlatmak için girin `PowerShell` CMD örneği ve ENTER tuşuna **Enter**.

    ![PowerShell örneği açın](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-powershell.png)

### <a name="use-the-serial-console-for-nmi-calls"></a>Seri konsol NMI çağrıları için kullanın.
Maskelenemez olmayan bir kesinti (NMI) bir sanal makinede yazılım yoksay olmaz bir sinyal oluşturmak için tasarlanmıştır. Tarihsel olarak, NMIs belirli yanıt süreleri gerektiren sistemleri donanım sorunları izlemek için kullanılır. Günümüzde programcılar ve sistem yöneticileri, yanıt vermeyen sistemlerde hata ayıklamak veya sorunları gidermek için genellikle NMI 'yi bir mekanizma olarak kullanır.

Seri konsol, komut çubuğunda klavye simgesini kullanarak bir Azure sanal makinesi için bir NMI göndermek için kullanılabilir. NMI aldıktan sonra sanal makine yapılandırması sistemin nasıl yanıt vereceğini denetler. Windows için kilitlenme yapılandırılabilir ve bir bellek dökümü dosyasına bir NMI alırken oluşturun.

![NMI Gönder](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Bir NMI aldığında bir kilitlenme bilgi döküm dosyası oluşturmak için Windows yapılandırma hakkında daha fazla bilgi için bkz. [bir NMI kilitlenme bilgi döküm dosyası oluşturmak nasıl](https://support.microsoft.com/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file).

### <a name="use-function-keys-in-serial-console"></a>Seri konsol içinde işlev tuşlarını kullanın
İşlev tuşları, Windows vm'lerinde seri Konsolu kullanımı için etkinleştirilir. Seri konsol açılır F8 kolayca Gelişmiş Önyükleme ayarlar menüsünü girme kolaylık sunar, ancak seri konsol diğer tüm işlev tuşlarını ile uyumludur. Seri konsol kullanmakta olduğunuz bilgisayara bağlı olarak, klavyenizde **Fn** + **F1** (veya F2, F3, vb.) tuşlarına basmanız gerekebilir.

### <a name="use-wsl-in-serial-console"></a>Seri konsol WSL kullanımda
Seri konsol içinde kullanmak için Windows Server 2019 çalıştırıyorsanız WSL etkinleştirmek olası veya sonraki bir sürümü Ayrıca, bu nedenle Linux (WSL) için Windows alt sistemi veya üzeri, Windows Server 2019 için etkinleştirildi. Bu, Linux komutlarını konusunda de kullanıcılar için yararlı olabilir. Windows Server için WSL etkinleştirmek yönergeler için bkz: [Yükleme Kılavuzu](https://docs.microsoft.com/windows/wsl/install-on-server).

### <a name="restart-your-windows-vmvirtual-machine-scale-set-instance-within-serial-console"></a>Windows VM/sanal makine ölçek kümesi örneğinizi seri konsol içinde yeniden başlatın
Güç düğmesine gidip "VM 'yi yeniden Başlat" seçeneğine tıklayarak seri konsol içinde yeniden başlatma başlatabilirsiniz. Bu işlem bir VM yeniden başlatması başlatır ve Azure portal içinde yeniden başlatma ile ilgili bir bildirim görürsünüz.

Bu, seri konsol deneyiminden çıkmadan önyükleme menüsüne erişmek isteyebileceğiniz durumlarda faydalıdır.

![Windows seri konsol yeniden başlatması](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-windows.gif)

## <a name="disable-the-serial-console"></a>Seri konsolunu devre dışı bırakma
Varsayılan olarak, tüm aboneliklerde seri konsol erişimi etkindir. Seri konsolunu abonelik düzeyinde ya da VM/sanal makine ölçek kümesi düzeyinde devre dışı bırakabilirsiniz. Ayrıntılı yönergeler için [Azure seri konsolunu etkinleştir ve devre dışı bırak](./serial-console-enable-disable.md)' ı ziyaret edin.

## <a name="serial-console-security"></a>Seri konsol güvenlik

### <a name="access-security"></a>Erişimi güvenliği
Seri konsol erişimi için bir erişim rolüne sahip kullanıcılar sınırlıdır, [sanal makine Katılımcısı](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) veya sanal makine için daha yüksek. Çok faktörlü kimlik doğrulaması (MFA), Azure Active Directory kiracısı gerektiren sonra seri konsolun erişim aracılığıyla olduğundan seri konsoluna erişimi MFA ayrıca gerekir [Azure portalında](https://portal.azure.com).

### <a name="channel-security"></a>Kanalı güvenliği
İleri ve geri gönderilen tüm veriler kablo şifrelenir.

### <a name="audit-logs"></a>Denetim günlükleri
Seri konsola tüm erişim şu anda oturum [önyükleme tanılaması](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) sanal makinenin günlükleri. Bu günlükler için erişim sahibi ve Azure sanal makine yöneticisi tarafından denetlenen.

> [!CAUTION]
> Konsolu için erişim parolası günlüğe kaydedilir. Ancak, bu konsolda Çalıştır komutları içeren veya parolaları, parola, kullanıcı adlarını veya diğer tür kişisel bilgileri (PII) çıktı, VM önyükleme tanılama günlüklerine yazılır. Bunlar tüm diğer görünen metni yanı sıra, seri konsolun kaydırma uygulamasının bir parçası olarak işlev yazılır. Bu günlükler döngüsel ve onlara yönelik erişimi yalnızca Kişiler tanılama depolama hesabı için Okuma izinlerine sahip olması. Ancak, gizli dizileri ve/veya PII içerebilir Uzak Masaüstü için herhangi bir şey kullanmanın en iyi yöntemin izlenmesi önerilir.

### <a name="concurrent-usage"></a>Eşzamanlı kullanım
Seri konsola bir kullanıcı bağlandığından ve başka bir kullanıcı, aynı sanal makineye erişimi başarıyla istekleri, ilk kullanıcı bağlantısı kesilir ve ikinci kullanıcı aynı oturuma bağlı.

> [!CAUTION]
> Bu, bağlantısı kesilen bir kullanıcının oturum açmayacağı anlamına gelir. Bağlantı kesildikten sonra (SıGHUP veya benzer mekanizmayı kullanarak) bir oturumu kapatma özelliği hala yol haritasında bulunur. Windows için SAC içinde etkin otomatik bir zaman aşımı yoktur; Linux için terminal zaman aşımı ayarını yapılandırabilirsiniz.

## <a name="accessibility"></a>Erişilebilirlik
Erişilebilirlik bir anahtar Azure seri konsol biridir. Bu amaçla, biz seri konsol görsel ve işitme engelli yanı için fare kullanmanız mümkün olmayabilir kişiler erişilebilir olduğunu geçtiğinden emin olduk.

### <a name="keyboard-navigation"></a>Klavye ile gezinme
Kullanım **sekmesini** anahtar klavyenizde seri konsol arabirimi Azure portalından gidebilirsiniz. Konumunuz ekranda vurgulanır. Seri konsol penceresinin odağı bırakmak için basın **Ctrl**+**F6** klavyenizde.

### <a name="use-the-serial-console-with-a-screen-reader"></a>Seri konsol ekran okuyucuyla kullanma
Seri konsol ekran okuyucu desteği yerleşik olarak sahiptir. Açık bir ekran okuyucu ile geçici olarak gezinmek sesli ekran okuyucu tarafından okunacak şu anda seçili düğme için alternatif metin izin verir.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Seri konsoluna erişmek için genel senaryolar

Senaryo          | Seri konsol eylemleri
:------------------|:-----------------------------------------
Yanlış güvenlik duvarı kuralları | Seri konsol ve düzeltme Windows Güvenlik duvarı kuralları erişin.
Dosya Sistemi Bozulması/işaretleyin | Seri konsol erişmek ve dosya sistemi kurtarın.
RDP yapılandırma sorunları | Seri konsola erişin, ayarları değiştirin. Daha fazla bilgi için [RDP belgeleri](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).
Sistem ağ kilitleme | Seri konsol sistemini yönetmek için Azure portalından erişim. İçinde listelenen bazı ağ komutları [Windows komutları: CMD ve PowerShell](serial-console-cmd-ps-commands.md).
Önyükleme yükleyicisi ile etkileşim kurma | Seri Konsolu aracılığıyla BCD erişim. Bilgi için [seri konsolunda Windows önyükleme menüsünü etkinleştir](#enable-the-windows-boot-menu-in-the-serial-console).

## <a name="known-issues"></a>Bilinen sorunlar
Seri konsol ve VM 'nin işletim sistemi ile ilgili bazı sorunları fark ediyoruz. Bu sorunların ve Windows VM 'Leri hafifletme adımlarının bir listesi aşağıda verilmiştir. Bu sorunlar ve azaltmaları, hem VM 'Ler hem de sanal makine ölçek kümesi örnekleri için geçerlidir. Bunlar gördüğünüz hatayla eşleşmiyorsa, yaygın [seri konsol hatalarıyla](./serial-console-errors.md)ortak seri konsol hizmeti hatalarına bakın.

Sorun                             |   Risk azaltma
:---------------------------------|:--------------------------------------------|
Tuşuna basarak **Enter** sonra bağlantı başlığı görüntülenecek bir oturum açma istemine neden olmaz. | Daha fazla bilgi için [Hitting girin hiçbir şey yapmaz](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Özel VM, sağlamlaştırılmış gereç veya Windows düzgün bir şekilde seri bağlantı noktasına bağlanmak başarısız olmasına neden olan önyükleme yapılandırması çalıştırıyorsanız, bu hata oluşabilir. Yalnızca Windows Server VM 'Leri EMS 'nin etkin olacak şekilde yapılandırıldığı için, bu hata bir Windows 10 VM çalıştırıyorsanız da meydana gelir.
Yalnızca sistem durumu bilgileri, bir Windows VM'ye bağlanırken gösterilir.| Bu hata, Özel Yönetim Konsolu Windows görüntünüz için etkinleştirilmemişse oluşur. Bkz: [eski veya özel görüntüleri seri konsolundan etkinleştirme](#enable-the-serial-console-in-custom-or-older-images) SAC, Windows VM'de el ile etkinleştirme hakkında yönergeler için. Daha fazla bilgi için [Windows durum sinyallerini](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).
SAC, tarayıcıda tüm seri konsol alanını gerçekleştirmez | Bu, Windows ve Terminal Öykünücüde bilinen bir sorundur. Bu sorunu her iki ekiple izliyoruz, ancak şimdilik bir azaltma yoktur.
Çekirdek hata ayıklamasını etkin olduğunda SAC komut istemi türü oluşturulamıyor. | VM ve çalıştırmak için RDP `bcdedit /debug {current} off` yükseltilmiş bir komut isteminden. RDP gerçekleştiremezsiniz, bunun yerine başka bir Azure VM için işletim sistemi diski ve çalıştırarak veri diski olarak bağlı durumdayken değiştirmek `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, ardından diski geri değiştirme.
Özgün içerik yinelenen bir karakter varsa PowerShell içinde SAC sonuçları üçüncü bir karakter yapıştırma. | Geçici bir çözüm için çalıştırma `Remove-Module PSReadLine` geçerli oturumun PSReadLine modülünü kaldırmak için. Bu eylem silmez veya modül kaldırın.
Bazı klavye girişleri garip SAC çıkış üretmesi (örneğin, **[A**, **[3 ~** ). | [VT100](https://aka.ms/vtsequences) kaçış dizileri SAC istemi tarafından desteklenmez.
Uzun dizeler yapıştırma çalışmaz. | Seri konsol seri bağlantı noktası bant genişliği aşırı yüklemesini önlemek için 2048 karakter terminale içine yapıştırdığınız dize uzunluğunu kısıtlar.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S. nasıl geri bildirim gönderebilirim?**

A. Bir GitHub sorunu en oluşturarak geri bildirim sağlamak https://aka.ms/serialconsolefeedback. Alternatif olarak (daha az tercih edilir), aracılığıyla geri bildirim gönderebilirsiniz azserialhelp@microsoft.com veya sanal makine kategorisi https://feedback.azure.com.

**S. seri konsol kopyalamayı/yapıştırmayı destekliyor mu?**

A. Evet. Kullanım **Ctrl**+**Shift**+**C** ve **Ctrl**+**Shift** + **V** kopyalayıp terminale yapıştırabilirsiniz.

**S. Aboneliğimin seri konsolunu kimler etkinleştirebilir veya devre dışı bırakabilirim?**

A. Etkinleştirmek veya aboneliği genelinde düzeyinde seri konsol devre dışı bırakmak için abonelik için yazma izinleri olmalıdır. Yönetici veya sahip rollerinin yazmak için izne sahip roller içerir. Özel roller ayrıca yazma izinlerine sahip olabilir.

**S. sanal sunucum için seri konsoluna kimler erişebilir?**

A. Sanal makine Katılımcısı rolü olmalıdır veya sanal makinenin seri konsol erişimi için bir VM için daha yüksek.

**S. seri konsolum hiçbir şeyi görüntülüyor, ne yapmam gerekir?**

A. Görüntünüzü seri konsol erişimi için büyük olasılıkla yanlış yapılandırılmış. Görüntünüzü seri konsol etkinleştirmek için yapılandırma hakkında daha fazla bilgi için bkz: [eski veya özel görüntüleri seri konsolundan etkinleştirme](#enable-the-serial-console-in-custom-or-older-images).

**S. sanal makine ölçek kümeleri için seri konsol kullanılabilir mi?**

A. Evet öyle! Bkz. [Sanal Makine Ölçek Kümeleri Için seri konsol](./serial-console-overview.md#serial-console-for-virtual-machine-scale-sets)

## <a name="next-steps"></a>Sonraki adımlar
* CMD ve PowerShell komutları, Windows SAC içinde kullanabileceğiniz kapsamlı bir kılavuzu için bkz: [Windows komutları: CMD ve PowerShell](serial-console-cmd-ps-commands.md).
* Seri konsol için de kullanılabilir olan [Linux](serial-console-linux.md) VM'ler.
* Daha fazla bilgi edinin [önyükleme tanılaması](boot-diagnostics.md).
