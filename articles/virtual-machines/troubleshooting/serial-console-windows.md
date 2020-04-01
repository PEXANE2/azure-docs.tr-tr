---
title: Windows için Azure Seri Konsolu | Microsoft Dokümanlar
description: Azure Sanal Makineler ve Sanal Makine Ölçek Setleri için Çift Yönlü Seri Konsol.
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
ms.openlocfilehash: 68089a86b8b832638abd30aa7c36aa1c5bd84225
ms.sourcegitcommit: 27bbda320225c2c2a43ac370b604432679a6a7c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80410127"
---
# <a name="azure-serial-console-for-windows"></a>Windows için Azure Seri Konsolu

Azure portalındaki Seri Konsol, Windows sanal makineleri (VM' ler) ve sanal makine ölçeği seti örnekleri için metin tabanlı bir konsola erişim sağlar. Bu seri bağlantısı, VM'nin VEYA sanal makine ölçeği kümesi örneğinin COM1 seri bağlantı noktasına bağlanarak ağa veya işletim sistemi durumundan bağımsız olarak bu bağlantıya erişim sağlar. Seri konsola yalnızca Azure portalı kullanılarak erişilebilir ve yalnızca Katılımcı veya VM veya sanal makine ölçeği kümesine erişim rolü olan veya daha yüksek olan kullanıcılar için izin verilir.

Seri Konsol, VM'ler ve sanal makine ölçeği seti örnekleri için aynı şekilde çalışır. Bu dokümanda, aksi belirtilmedikçe, VM'lere yapılan tüm sözlerle sanal makine ölçeği seti örnekleri dolaylı olarak yer alacaktır.

Linux için seri konsol belgeleri [için Linux için Azure Seri Konsolu'na](serial-console-linux.md)bakın.

> [!NOTE]
> Seri Konsol genellikle genel Azure bölgelerinde ve Azure Resmi'nde genel önizlemede kullanılabilir. Azure Çin bulutunda henüz kullanıma sunulmadı.


## <a name="prerequisites"></a>Ön koşullar

* VM veya sanal makine ölçeği set örneğiniz kaynak yönetimi dağıtım modelini kullanmalıdır. Klasik dağıtımlar desteklenmez.

- Seri konsol kullanan hesabınız, VM ve [önyükleme tanılama](boot-diagnostics.md) depolama hesabı için [Sanal Makine Katılımcısı rolüne](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) sahip olmalıdır

- VM veya sanal makine ölçeği set örneğinizin parola tabanlı bir kullanıcısı olmalıdır. VM erişim uzantısı sıfırlama [parola](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) işlevi ile bir oluşturabilirsiniz. Destek + sorun **giderme** bölümünden **parolayı sıfırla'yı** seçin.

* Sanal makine ölçeği kümesi örneği için VM [önyükleme tanılama](boot-diagnostics.md) etkin olmalıdır.

    ![Önyükleme tanılama ayarları](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

## <a name="enable-serial-console-functionality-for-windows-server"></a>Windows Server için Seri Konsol işlevini etkinleştirme

> [!NOTE]
> Seri konsolda bir şey görmüyorsanız, VM veya sanal makine ölçek kümenizde önyükleme tanılamanın etkin olduğundan emin olun.

### <a name="enable-the-serial-console-in-custom-or-older-images"></a>Seri konsolu özel veya eski görüntülerde etkinleştirme
Azure'daki yeni Windows Server görüntülerinde varsayılan olarak [Özel Yönetim Konsolu](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) etkinleştirilir. SAC, Windows'un sunucu sürümlerinde desteklenir, ancak istemci sürümlerinde (örneğin, Windows 10, Windows 8 veya Windows 7) kullanılamaz.

Eski Windows Server görüntüleri (Şubat 2018'den önce oluşturulmuş) için, Azure portalının çalıştır komut uyruğu özelliği aracılığıyla seri konsolu otomatik olarak etkinleştirebilirsiniz. Azure portalında **Çalıştır komutunu**seçin ve listeden **EnableEMS** adlı komutu seçin.

![Komut listesini çalıştır](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-runcommand.png)

Alternatif olarak, Şubat 2018'den önce oluşturulan Windows VM'ler/sanal makine ölçeği kümesi için seri konsolu el ile etkinleştirmek için aşağıdaki adımları izleyin:

1. Uzak Masaüstü'nü kullanarak Windows sanal makinenize bağlanın
1. İdari komut isteminden aşağıdaki komutları çalıştırın:
    - `bcdedit /ems {current} on`powershell `bcdedit /ems '{current}' on` kullanıyorsanız
    - `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
1. Etkinleştirilecek SAC konsolu için sistemi yeniden başlatın.

    ![SAC konsolu](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Gerekirse, SAC çevrimdışı da etkinleştirilebilir:

1. SAC'Nin veri diski olarak yapılandırılmasını istediğiniz windows diskini varolan VM'ye takın.

1. İdari komut isteminden aşağıdaki komutları çalıştırın:
   - `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
   - `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

#### <a name="how-do-i-know-if-sac-is-enabled"></a>SAC etkin olup olmadığını nasıl anlarım?

[SAC](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) etkin değilse, seri konsol SAC komut istemini görüntülemez. Bazı durumlarda, VM sistem durumu bilgileri gösterilir ve diğer durumlarda boştur. Şubat 2018'den önce oluşturulmuş bir Windows Server görüntüsü kullanıyorsanız, SAC büyük olasılıkla etkinleştirilmez.

### <a name="enable-the-windows-boot-menu-in-the-serial-console"></a>Seri konsolunda Windows önyükleme menüsünü etkinleştirme

Windows önyükleme yükleyici istemlerinin seri konsolunda görüntülenmesini etkinleştirmeniz gerekiyorsa, önyükleme yapılandırma verilerinize aşağıdaki ek seçenekleri ekleyebilirsiniz. Daha fazla bilgi için [bcdedit'e](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set)bakın.

1. Uzak Masaüstü'nü kullanarak Windows VM'nize veya sanal makine ölçeğinize bağlayın.

1. İdari komut isteminden aşağıdaki komutları çalıştırın:
   - `bcdedit /set {bootmgr} displaybootmenu yes`
   - `bcdedit /set {bootmgr} timeout 10`
   - `bcdedit /set {bootmgr} bootems yes`

1. Önyükleme menüsünün etkinleştirilmesi için sistemi yeniden başlatın

> [!NOTE]
> Önyükleme yöneticisi menüsünün görüntülenmesi için ayarladığınız zaman alacaktır, işletim sistemi önyükleme sürenizi etkiler. 10 saniyelik zaman dilimi değerinin çok kısa veya çok uzun olduğunu düşünüyorsanız, farklı bir değere ayarlayın.

## <a name="use-serial-console"></a>Seri Konsol kullan

### <a name="use-cmd-or-powershell-in-serial-console"></a>Seri Konsolda CMD veya PowerShell kullanın

1. Seri konsoluna bağlanın. Başarılı bir şekilde bağlanırsanız, komut istemi **SAC>: **

    ![SAC'a bağlan](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect-sac.png)

1.    CMD örneği olan bir kanal oluşturmak için girin. `cmd`

1.    CMD örneğini çalıştıran kanala geçmek için kısayol tuşlarına girin `ch -si 1` veya basın. `<esc>+<tab>`

1.    **Enter**tuşuna basın ve ardından yönetim izinleriyle oturum açma kimlik bilgilerini girin.

1.    Geçerli kimlik bilgilerini girdikten sonra CMD örneği açılır.

1.    PowerShell örneğini başlatmak `PowerShell` için CMD örneğine girin ve sonra **Enter**tuşuna basın.

    ![PowerShell örneğini aç](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-powershell.png)

### <a name="use-the-serial-console-for-nmi-calls"></a>NMI aramaları için seri konsolu kullanma
Maskelenmeyen bir kesme (NMI), sanal makinedeki yazılımın göz ardı edilmeyeceği bir sinyal oluşturmak üzere tasarlanmıştır. Tarihsel olarak, NMI'ler belirli yanıt süreleri gerektiren sistemlerdeki donanım sorunlarını izlemek için kullanılmıştır. Bugün, programcılar ve sistem yöneticileri genellikle yanıt vermeyen sistemleri hata ayıklamak veya sorun giderme mekanizması olarak NMI kullanır.

Seri konsol, komut çubuğundaki klavye simgesini kullanarak Bir Azure sanal makinesine NMI göndermek için kullanılabilir. NMI teslim edildikten sonra, sanal makine yapılandırması sistemin nasıl yanıt vereceğini denetler. Windows, NMI alırken kilitlenecek ve bellek dökümü dosyası oluşturacak şekilde yapılandırılabilir.

![NMI gönder](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Windows'u bir NMI aldığında kilitlenme dökümü dosyası oluşturacak şekilde yapılandırmak için, [NMI kullanarak kilitlenme döküm dosyasının nasıl oluşturulacağı](https://support.microsoft.com/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file)bilgisini görün.

### <a name="use-function-keys-in-serial-console"></a>Seri konsolda işlev tuşlarını kullanma
Windows VM'lerde seri konsol kullanımı için işlev tuşları etkinleştirilir. Seri konsol açılır menüsündeki F8, Gelişmiş Önyükleme Ayarları menüsüne kolayca girme kolaylığı sağlar, ancak seri konsol diğer tüm işlev tuşları ile uyumludur. Seri konsol kullandığınız bilgisayara bağlı olarak klavyenizdeki **Fn** + **F1** (veya F2, F3, vb.) tuşuna basmanız gerekebilir.

### <a name="use-wsl-in-serial-console"></a>SERI konsolda WSL kullanın
Linux için Windows Alt Sistemi (WSL) Windows Server 2019 veya sonraki sürümleri için etkinleştirildiğinden, Windows Server 2019 veya daha sonra çalıştırıyorsanız, seri konsolda kullanılmak üzere WSL'yi etkinleştirmek de mümkündür. Bu, Linux komutlarına aşina olan kullanıcılar için de yararlı olabilir. Windows Server için WSL'yi etkinleştirmek için yükleme [kılavuzuna](https://docs.microsoft.com/windows/wsl/install-on-server)bakın.

### <a name="restart-your-windows-vmvirtual-machine-scale-set-instance-within-serial-console"></a>Windows VM/sanal makine ölçeği ayar örneğini Seri Konsol'da yeniden başlatın
Güç düğmesine gidip "VM'yi Yeniden Başlat"ı tıklatarak seri konsoliçinde yeniden başlatma başlatabilirsiniz. Bu, bir VM yeniden başlatma başlatacak ve Azure portalında yeniden başlatmayla ilgili bir bildirim görürsünüz.

Bu, seri konsol deneyiminden ayrılmadan önyükleme menüsüne erişmek isteyebileceğiniz durumlarda yararlıdır.

![Windows Seri Konsol yeniden başlat](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-windows.gif)

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
> Konsol için erişim parolası günlüğe kaydedilmez. Ancak, konsol içinde çalıştırılan komutlar parolalar, sırlar, kullanıcı adları veya kişisel olarak tanımlanabilir bilgilerin (PII) başka bir biçimini içeriyorsa veya çıktıalıyorsa, bunlar VM önyükleme tanılama günlüklerine yazılır. Seri konsolun kaydırma geri kaydırma işlevinin uygulanmasının bir parçası olarak diğer tüm görünür metinle birlikte yazılır. Bu günlükler daireseldir ve yalnızca tanılama depolama hesabına okuma izni olan kişiler bunlara erişebilir. Ancak, sırlar ve/veya kişisel bilgiler içerebilecek herhangi bir şey için Uzak Masaüstünü kullanmanın en iyi uygulamasını izlemenizi öneririz.

### <a name="concurrent-usage"></a>Eşzamanlı kullanım
Bir kullanıcı seri konsoluna bağlıysa ve başka bir kullanıcı aynı sanal makineye başarıyla erişim isterse, ilk kullanıcının bağlantısı kesilir ve ikinci kullanıcı aynı oturuma bağlanır.

> [!CAUTION]
> Bu, bağlantısı kesilen bir kullanıcının oturumukapatılamayacağı anlamına gelir. Bağlantıyı kesme üzerine bir oturum açma yı (SIGHUP veya benzeri mekanizmayı kullanarak) uygulama özelliği hala yol haritasındadır. Windows için, SAC'da otomatik bir zaman ayarı etkinleştirilir; Linux için, terminal zaman aşım ayarını yapılandırabilirsiniz.

## <a name="accessibility"></a>Erişilebilirlik
Erişilebilirlik, Azure seri konsolu için önemli bir odak noktasıdır. Bu amaçla, seri konsolun görme ve işitme engellilerin yanı sıra fare kullanamayan kişiler için erişilebilir olmasını sağladık.

### <a name="keyboard-navigation"></a>Klavye ile gezinme
Azure portalından seri konsol arabiriminde gezinmek için klavyenizdeki **Sekme** tuşunu kullanın. Konumunuz ekranda vurgulanır. Seri konsol penceresinin odağında bırakmak için klavyenizdeki **Ctrl**+**F6** tuşuna basın.

### <a name="use-the-serial-console-with-a-screen-reader"></a>Seri konsolu ekran okuyucuyla kullanma
Seri konsolyerleşik ekran okuyucu desteğine sahiptir. Ekran okuyucu açıkken gezinmek, şu anda seçili olan düğmenin alt metninin ekran okuyucu tarafından yüksek sesle okunmasını sağlar.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Seri konsoluna erişmek için sık karşılaşılan senaryolar

Senaryo          | Seri konsoldaki eylemler
:------------------|:-----------------------------------------
Yanlış güvenlik duvarı kuralları | Seri konsola erişin ve Windows güvenlik duvarı kurallarını düzeltin.
Dosya sistemi bozulması/denetimi | Seri konsoluna erişin ve dosya sistemini kurtarın.
RDP yapılandırma sorunları | Seri konsoluna erişin ve ayarları değiştirin. Daha fazla bilgi için [RDP belgelerine](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access)bakın.
Ağ kilitleme sistemi | Sistemi yönetmek için Azure portalından seri konsola erişin. Bazı ağ komutları [Windows komutlarında listelenir: CMD ve PowerShell.](serial-console-cmd-ps-commands.md)
Bootloader ile etkileşim | BCD'ye seri konsoldan erişin. Daha fazla bilgi için seri [konsolundaki Windows önyükleme menüsünü etkinleştir](#enable-the-windows-boot-menu-in-the-serial-console)meslebine bakın.

## <a name="known-issues"></a>Bilinen sorunlar
Seri konsol ve VM'nin işletim sistemiyle ilgili bazı sorunların farkındayız. Windows VM'leri için bu sorunların ve azaltma adımlarının bir listesi aşağıda veda edebilirsiniz. Bu sorunlar ve azaltıcı etkenler hem VM'ler hem de sanal makine ölçeği kümesi örnekleri için geçerlidir. Bunlar gördüğünüz hatayla eşleşmiyorsa, [Ortak Seri Konsol hatalarında](./serial-console-errors.md)sık karşılaşılan seri konsol hizmet hatalarını görün.

Sorun                             |   Risk azaltma
:---------------------------------|:--------------------------------------------|
Bağlantı başlığından sonra **Enter** tuşuna bastığınızda oturum açma istemi görüntülenmez. | Daha fazla bilgi için [bkz.](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md) Bu hata, Windows'un seri bağlantı noktasına düzgün bir şekilde bağlanmamasına neden olan özel bir VM, sertleştirilmiş bir cihaz veya önyükleme config çalıştırıyorsanız oluşabilir. Yalnızca Windows Server VM'leri EMS'yi etkin olacak şekilde yapılandırıldığından, windows 10 VM çalıştırıyorsanız bu hata da oluşur.
Yalnızca windows vm'ye bağlanırken sistem durumu bilgileri gösterilir| Bu hata, Windows görüntünüz için Özel Yönetim Konsolu etkinleştirilmediyse oluşur. Bkz. Windows VM'nizde SAC'yi el ile nasıl etkinleştireceğimize ilişkin talimatlar için [seri konsolu özel veya eski görüntülerde etkinleştirin.](#enable-the-serial-console-in-custom-or-older-images) Daha fazla bilgi için [Windows sistem durumu sinyallerine](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md)bakın.
SAC tarayıcıdaki tüm Seri Konsol alanını kaplamıyor | Bu, Windows ve terminal emülatörü yle ilgili bilinen bir sorundur. Bu sorunu her iki takımla da takip ediyoruz ama şimdilik bir hafifletme yok.
Çekirdek hata ayıklama etkinse SAC komut isteminde yazamaz. | RDP'den VM'ye ve yükseltilmiş komut isteminden çalıştırın. `bcdedit /debug {current} off` RDP'yi yapamıyorsanız, bunun yerine işletim sistemi diskini başka bir Azure VM'sine `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`ekleyebilir ve çalıştırarak veri diski olarak eklenmiş ken değiştirebilir ve diski geri değiştirebilirsiniz.
PowerShell'e SAC'da yapıştırma, orijinal içeriğin yinelenen bir karakteri varsa üçüncü bir karakterle sonuçlanır. | Geçici çözüm için, `Remove-Module PSReadLine` PSReadLine modüllerini geçerli oturumdan boşaltmak için çalıştırın. Bu eylem modülü silmez veya kaldırmaz.
Bazı klavye girişleri garip SAC çıkışı üretir (örneğin, **[A**, **[3~**). | [VT100](https://aka.ms/vtsequences) kaçış sekansları SAC komut istemi tarafından desteklenmez.
Uzun dizeleri yapıştırma işe yaramaz. | Seri konsol, seri bağlantı noktası bant genişliğinin aşırı yüklenmesini önlemek için terminale yapıştırılan dizelerin uzunluğunu 2048 karakterle sınırlar.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

**S. Nasıl geribildirim gönderebilirim?**

A. Bir GitHub sorunu oluşturarak https://aka.ms/serialconsolefeedbackgeri bildirim sağlayın. Alternatif olarak (daha az tercih azserialhelp@microsoft.com edilen), geribildirim gönderebilirsiniz üzerinden veya sanal makine kategorisinde https://feedback.azure.com.

**S. Seri konsol kopyalama/yapıştır'ı destekliyor mu?**

A. Evet. Terminale kopyalamak ve yapıştırmak için **Ctrl**+**Shift**+**C** ve **Ctrl**+**Shift**+**V'yi** kullanın.

**S. Aboneliğim için seri konsolu kimetkinleştirebilir veya devre dışı bırakabilir?**

A. Seri konsolu abonelik genelinde bir düzeyde etkinleştirmek veya devre dışı kardık yapmak için, abonelığa yazma izinleriniz olması gerekir. Yazma izni olan roller yönetici veya sahip rollerini içerir. Özel rollerde yazma izinleri de olabilir.

**S. VM'imin seri konsoluna kimler erişebilir?**

A. Bir VM'nin VM'nin seri konsoluna erişmesi için Sanal Makine Katılımcısı rolüne veya daha yüksek olması gerekir.

**S. Benim seri konsol bir şey görüntülemiyor, ne yapmalıyım?**

A. Resminiz büyük olasılıkla seri konsol erişimi için yanlış yapılandırılmıştır. Seri konsolunu etkinleştirmek için resminizi yapılandırma hakkında daha fazla bilgi [için](#enable-the-serial-console-in-custom-or-older-images)bkz.

**S. Seri konsol sanal makine ölçek kümeleri için kullanılabilir mi?**

A. Evet öyle! [Sanal Makine Ölçek Setleri için Seri Konsol'a](./serial-console-overview.md#serial-console-for-virtual-machine-scale-sets) bakın

## <a name="next-steps"></a>Sonraki adımlar
* Windows SAC'da kullanabileceğiniz CMD ve PowerShell komutları için ayrıntılı bir kılavuz için [Windows komutlarına bakın: CMD ve PowerShell.](serial-console-cmd-ps-commands.md)
* Seri konsol [Linux](serial-console-linux.md) VM'ler için de kullanılabilir.
* [Önyükleme tanılama](boot-diagnostics.md)hakkında daha fazla bilgi edinin.
