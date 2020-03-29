---
title: StorSimple cihaz yönetimi için PowerShell
description: StorSimple cihazınızı yönetmek için StorSimple için Windows PowerShell'i nasıl kullanacağınızı öğrenin.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/09/2018
ms.author: alkohli
ms.openlocfilehash: 7d59f00d655bc7b2395c46713a56f52c61ffa42c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76277101"
---
# <a name="use-windows-powershell-for-storsimple-to-administer-your-device"></a>StorSimple için Windows PowerShell’i kullanarak cihazınızı yönetme

## <a name="overview"></a>Genel Bakış

StorSimple için Windows PowerShell, Microsoft Azure StorSimple aygıtınızı yönetmek için kullanabileceğiniz bir komut satırı arabirimi sağlar. Adından da anlaşılacağı gibi, kısıtlı bir runspace yerleşik bir Windows PowerShell tabanlı, komut satırı arabirimidir. Komut satırındaki kullanıcının bakış açısından, Kısıtlı bir çalışma alanı Windows PowerShell'in kısıtlı bir sürümü olarak görünür. Windows PowerShell'in bazı temel özelliklerini korurken, bu arabirimde Microsoft Azure StorSimple aygıtınızı yönetmeye yönelik ek özel cmdletler bulunur.

Bu makalede, bu arabirime nasıl bağlanabileceğiniz de dahil olmak üzere StorSimple özellikleri için Windows PowerShell açıklanır ve bu arabirimi kullanarak gerçekleştirebileceğiniz adım adım yordamlara veya iş akışlarına bağlantılar içerir. İş akışları, cihazınızı nasıl kaydedeceğiniz, cihazınızdaki ağ arabirimini yapılandırmayı, aygıtın bakım modunda olmasını gerektiren güncelleştirmeleri yüklemeyi, aygıt durumunu değiştirmeyi ve karşılaşabileceğiniz sorunları nasıl gidereceğinizi içerir.

Bu makaleyi okuduktan sonra, şunları yapabileceksiniz:

* StorSimple için Windows PowerShell'i kullanarak StorSimple cihazınıza bağlanın.
* StorSimple için Windows PowerShell'i kullanarak StorSimple cihazınızı yönetin.
* StorSimple için Windows PowerShell'den yardım alın.

> [!NOTE]
> * StorSimple cmdlet'ler için Windows PowerShell, StorSimple cihazınızı bir seri konsoldan veya Windows PowerShell remoting'den uzaktan yönetmenize olanak tanır. Bu arabirimde kullanılabilen cmdletlerin her biri hakkında daha fazla bilgi için [StorSimple için Windows PowerShell için cmdlet referansına](https://technet.microsoft.com/library/dn688168.aspx)gidin.
> * Azure PowerShell StorBasit cmdlets, Komut satırından StorSimple hizmet düzeyi ve geçiş görevlerini otomatikleştirmenize olanak tanıyan farklı bir cmdlet koleksiyonudur. StorSimple için Azure PowerShell cmdlets hakkında daha fazla bilgi için [Azure StorSimple cmdlet referansına](https://docs.microsoft.com/powershell/module/servicemanagement/azure/?view=azuresmps-4.0.0&viewFallbackFrom=azuresmps-3.7.0#azure)gidin.


StorSimple için Windows PowerShell'e aşağıdaki yöntemlerden birini kullanarak erişebilirsiniz:

* [StorSimple aygıt seri konsoluna bağlanın](#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)
* [Windows PowerShell'i kullanarak StorSimple'a uzaktan bağlanma](#connect-remotely-to-storsimple-using-windows-powershell-for-storsimple)

## <a name="connect-to-windows-powershell-for-storsimple-via-the-device-serial-console"></a>Aygıt seri konsolu üzerinden StorSimple için Windows PowerShell'e bağlanın

StorSimple için Windows PowerShell'e bağlanmak için [PuTTY](https://www.putty.org/) veya benzeri terminal öykünme yazılımlarını indirebilirsiniz. Microsoft Azure StorSimple aygıtına erişmek için özellikle PuTTY'yi yapılandırmanız gerekir. Aşağıdaki konular, PuTTy'nin nasıl yapılandırılabilen ve aygıta nasıl bağlanıştırılabilen ayrıntılı adımlar içerir. Seri konsoldaki çeşitli menü seçenekleri de açıklanmıştır.

### <a name="putty-settings"></a>PuTTY ayarları

Seri konsoldan Windows PowerShell arabirimine bağlanmak için aşağıdaki PuTTY ayarlarını kullandığınızdan emin olun.

#### <a name="to-configure-putty"></a>PuTTY'yi yapılandırmak için

1. PuTTY **Yeniden Yapılandırma** iletişim kutusunda, **Kategori** bölmesinde **Klavye'yi**seçin.
2. Aşağıdaki seçeneklerin seçildiğinden emin olun (bunlar yeni bir oturumbaşlattığınızda varsayılan ayarlardır).
   
   | Klavye öğesi | Şunu seçin: |
   | --- | --- |
   | Arka boşluk tuşu |Kontrol mü? (127) |
   | Ev ve Bitiş tuşları |Standart |
   | Fonksiyon tuşları ve tuş takımı |ESC[n~ |
   | İmleç anahtarlarının ilk durumu |Normal |
   | Sayısal tuş takımının ilk durumu |Normal |
   | Ekstra klavye özelliklerini etkinleştirme |Control-Alt Alt'tan farklıdır |
   
    ![Desteklenen Macun Ayarları](./media/storsimple-windows-powershell-administration/IC740877.png)
3. **Uygula**’ya tıklayın.
4. **Kategori** bölmesinde **Çeviri'yi**seçin.
5. Uzak **karakter set** listesi kutusunda **UTF-8'i**seçin.
6. **Çizgi çizim karakterleri işleme**altında, **Unicode çizgi çizim kod noktaları kullanın**seçin. Aşağıdaki ekran görüntüsü doğru PuTTY seçimlerini gösterir.
   
    ![UTF Macun Ayarları](./media/storsimple-windows-powershell-administration/IC740878.png)
7. **Uygula**’ya tıklayın.

Artık aşağıdaki adımları yaparak aygıt seri konsoluna bağlanmak için PuTTY'yi kullanabilirsiniz.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="about-the-serial-console"></a>Seri konsol hakkında

StorSimple cihazınızın Windows PowerShell arayüzüne seri konsol üzerinden erişdiğinizde, menü seçeneklerinin ardından bir başlık iletisi sunulur.

Banner iletisi, model, ad, yüklü yazılım sürümü ve erişediğiniz denetleyicinin durumu gibi temel StorSimple aygıt bilgilerini içerir. Aşağıdaki resimde bir banner iletisi örneği gösterilmektedir.

![Seri afiş iletisi](./media/storsimple-windows-powershell-administration/IC741098.png)

> [!IMPORTANT]
> Bağlı olduğunuz denetleyicinin _Etkin_ mi yoksa _Pasif_mi olduğunu belirlemek için banner iletisini kullanabilirsiniz.

Aşağıdaki resimde seri konsol menüsünde bulunan çeşitli çalışma alanı seçenekleri gösterilmektedir.

![Cihazınızı kaydedin 2](./media/storsimple-windows-powershell-administration/IC740906.png)

Aşağıdaki ayarlardan birini seçebilirsiniz:

1. **Tam erişimle giriş yapın** Bu seçenek, yerel denetleyicideki **SSAdminConsole** runspace'e (uygun kimlik bilgileriyle) bağlanmanızı sağlar. (Yerel denetleyici, StorSimple cihazınızın seri konsolundan şu anda erişebildiğiniz denetleyicidir.) Bu seçenek, Microsoft Destek'in olası aygıt sorunlarını gidermek için sınırsız çalışma alanına (destek oturumu) erişmesine izin vermek için de kullanılabilir. Oturum açmak için seçenek 1'i kullandıktan sonra, Microsoft Destek mühendisinin belirli bir cmdlet çalıştırarak sınırsız çalışma alanına erişmesine izin verebilirsiniz. Ayrıntılar için [destek oturumu başlat'a](storsimple-8000-contact-microsoft-support.md#start-a-support-session-in-windows-powershell-for-storsimple)bakın.
   
2. **Tam erişime sahip eş denetleyicisine giriş yapın** Bu seçenek, eş denetleyicisindeki **SSAdminConsole** runspace'e (uygun kimlik bilgileriyle) bağlanabildiğiniz dışında seçenek 1 ile aynıdır. StorSimple aygıtı etkin-pasif yapılandırmada iki denetleyicisi olan yüksek kullanılabilirlik aygıtı olduğundan, eş, seri konsoldan erişebildiğiniz aygıttaki diğer denetleyiciyi ifade eder).
   Seçenek 1'e benzer şekilde, bu seçenek Microsoft Destek'in eş denetleyicisi üzerinde sınırsız çalışma alanına erişmesine izin vermek için de kullanılabilir.

3. **Sınırlı erişimle bağlanın** Bu seçenek, sınırlı modda Windows PowerShell arabirimine erişmek için kullanılır. Erişim kimlik bilgileri için istenmezsiniz. Bu seçenek, 1 ve 2 seçenekleriyle karşılaştırıldığında daha sınırlı bir çalışma alanına bağlanır.  Bu çalışma alanında **yapılamaz* seçenek 1 üzerinden kullanılabilir görevlerin bazıları şunlardır:
   
   * Fabrika ayarlarına sıfırlama
   * Parolayı değiştirme
   * Destek erişimini etkinleştirme veya devre dışı
   * Güncelleştirme uygulama
   * Düzeltmeleri yükleme

     > [!NOTE]
     > Aygıt yöneticisi parolasını unuttuysanız ve seçenek 1 veya 2 üzerinden bağlanamıyorsanız, tercih edilen seçenek budur.

4. **Dili değiştirme** Bu seçenek, Windows PowerShell arabirimindeki görüntü dilini değiştirmenize olanak tanır. Desteklenen diller İngilizce, Japonca, Rusça, Fransızca, Güney Korece, İspanyolca, İtalyanca, Almanca, Çince ve Brezilya Portekizcesi'dir.

## <a name="connect-remotely-to-storsimple-using-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'i kullanarak StorSimple'a uzaktan bağlanma

StorSimple cihazınıza bağlanmak için Windows PowerShell remoting'i kullanabilirsiniz. Bu şekilde bağlandığınızda, bir menü görmezsiniz. (Yalnızca bağlanmak için aygıttaki seri konsolu kullanırsanız bir menü görürsünüz. Uzaktan bağlanmak sizi doğrudan seri konsoldaki "seçenek 1 – tam erişim" eşdeğerine götürür.) Windows PowerShell remoting ile belirli bir çalışma alanına bağlanırsınız. Görüntü dilini de belirtebilirsiniz.

Görüntü dili, seri konsol menüsündeki Dili **Değiştir** seçeneğini kullanarak ayarladığınız dilden bağımsızdır. Remote PowerShell, hiçbiri belirtilmemişse bağlandığınız aygıtın yerel ayarını otomatik olarak alır.

> [!NOTE]
> Microsoft Azure sanal ana bilgisayarları ve StorSimple Cloud Appliances ile çalışıyorsanız, bulut cihazına bağlanmak için Windows PowerShell remoting'i ve sanal ana bilgisayarı kullanabilirsiniz. Windows PowerShell oturumundan bilgi kaydetmek için ana bilgisayarda bir paylaşım konumu ayarladıysanız, _Herkes_ ilkesinin yalnızca kimlik doğrulaması yapılan kullanıcıları içerdiğini unutmayın. Bu nedenle, paylaşıma _Herkes_ tarafından erişilmesine izin verecek şekilde ayarladıysanız ve kimlik bilgilerini belirtmeden bağlanırsanız, kimlik doğrulaması olmayan Anonim ilkesi kullanılır ve bir hata görürsünüz. Bu sorunu gidermek için, paylaşım ana bilgisayarında Konuk hesabını etkinleştirmeniz ve ardından Konuk hesabına paylaşıma tam erişim sağlamanız veya Windows PowerShell cmdlet ile birlikte geçerli kimlik bilgilerini belirtmeniz gerekir.


Windows PowerShell remoting üzerinden bağlanmak için HTTP veya HTTPS'yi kullanabilirsiniz. Aşağıdaki öğreticilerde talimatları kullanın:

* [HTTP'yi kullanarak uzaktan bağlan](storsimple-8000-remote-connect.md#connect-through-http)
* [HTTPS'yi kullanarak uzaktan bağlanma](storsimple-8000-remote-connect.md#connect-through-https)

## <a name="connection-security-considerations"></a>Bağlantı güvenliği hususları

StorSimple için Windows PowerShell'e nasıl bağlanabileceğinize karar verirken aşağıdakileri göz önünde bulundurun:

* Doğrudan aygıt seri konsoluna bağlanmak güvenlidir, ancak seri konsola ağ anahtarları üzerinden bağlanmak güvenli değildir. Ağ anahtarları üzerinden aygıt serilerine bağlanırken güvenlik riskine karşı dikkatli olun.
* BIR HTTP oturumu aracılığıyla bağlanmak, seri konsol üzerinden ağ üzerinden bağlanmaktan daha fazla güvenlik sağlayabilir. Bu en güvenli yöntem olmasa da, güvenilen ağlarda kabul edilebilir.
* HTTPS oturumu aracılığıyla bağlanmak en güvenli ve önerilen seçenektir.

## <a name="administer-your-storsimple-device-using-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'i kullanarak StorSimple cihazınızı yönetin

Aşağıdaki tablo, StorSimple cihazınızın Windows PowerShell arabirimi içinde gerçekleştirilebilecek tüm ortak yönetim görevlerinin ve karmaşık iş akışlarının bir özetini gösterir. Her iş akışı hakkında daha fazla bilgi için tablodaki uygun girişi tıklatın.

#### <a name="windows-powershell-for-storsimple-workflows"></a>StorSimple iş akışları için Windows PowerShell

| Bunu yapmak istiyorsanız ... | Bu yordamı kullanın. |
| --- | --- |
| Cihazınızı kaydetme |[StorSimple için Windows PowerShell kullanarak aygıtı yapılandırma ve kaydetme](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple) |
| Web proxy yapılandırma</br>Web proxy ayarlarını görüntüleme |[StorSimple aygıtınız için web proxy'yi yapılandırın](storsimple-8000-configure-web-proxy.md) |
| Cihazınızdaki DATA 0 ağ arabirimi ayarlarını değiştirme |[StorSimple aygıtınız için DATA 0 ağ arabirimini değiştirin](storsimple-8000-modify-data-0.md) |
| Denetleyiciyi durdurma </br> Denetleyiciyi yeniden başlatma veya kapatma </br> Aygıtı kapatma</br>Cihazı varsayılan fabrika ayarlarına sıfırlama |[Cihaz denetleyicilerini yönetme](storsimple-8000-manage-device-controller.md) |
| Bakım modu güncelleştirmelerini ve düzeltmeleri yükleme |[Cihazınızı güncelleştirme](storsimple-update-device.md) |
| Bakım moduna girin </br>Çıkış bakım modu |[StorBasit aygıt modları](storsimple-8000-device-modes.md) |
| Destek paketi oluşturma</br>Destek paketinin şifresini çözme ve yeniden arama |[Destek paketi oluşturma ve yönetme](storsimple-8000-create-manage-support-package.md) |
| Destek oturumu başlat</br> |[StorSimple için Windows PowerShell'de destek oturumu başlatma](storsimple-8000-create-manage-support-package.md#create-a-support-package) |

## <a name="get-help-in-windows-powershell-for-storsimple"></a>StorSimple için Windows PowerShell'den Yardım Alın

StorSimple için Windows PowerShell'de cmdlet Help mevcuttur. Bu Yardımın çevrimiçi ve güncel bir sürümü de mevcuttur ve bu sürümü sisteminizde Yardım'ı güncelleştirmek için kullanabilirsiniz.

Bu arabirimde Yardım almak Windows PowerShell'dekine benzer ve Yardımla ilgili cmdletlerin çoğu çalışacaktır. Windows PowerShell için Yardım'ı çevrimiçi olarak bulabilirsiniz: [Microsoft.PowerShell.Core](/powershell/module/Microsoft.PowerShell.Core/).

Aşağıda, Bu Windows PowerShell arabirimi için Yardım türlerinin kısa bir açıklaması ve Yardım'ın nasıl güncelleştirileni de dahildir.

### <a name="to-get-help-for-a-cmdlet"></a>Bir cmdlet için yardım almak için

* Herhangi bir cmdlet veya işlev için Yardım almak için aşağıdaki komutu kullanın:`Get-Help <cmdlet-name>`
* Herhangi bir cmdlet için çevrimiçi Yardım almak için, `-Online` parametre ile önceki cmdlet kullanın:`Get-Help <cmdlet-name> -Online`
* Tam Yardım için parametreyi `–Full` ve örneğin parametreyi `–Examples` kullanabilirsiniz.

### <a name="to-update-help"></a>Yardım'ı güncelleştirmek için

Windows PowerShell arabiriminde Yardım'ı kolayca güncelleyebilirsiniz. Sisteminizdeki Yardım'ı güncelleştirmek için aşağıdaki adımları gerçekleştirin.

#### <a name="to-update-cmdlet-help"></a>cmdlet Yardım güncellemek için
1. Windows PowerShell'i **çalıştır seçeneğiyle** başlatın.
2. Komut isteminde, yazın:`Update-Help`
3. Güncelleştirilmiş Yardım dosyaları yüklenir.
4. Yardım dosyaları yüklendikten sonra yazın: `Get-Help Get-Command`. Bu, Yardım'ın kullanılabildiği cmdletlerin bir listesini görüntüler.

> [!NOTE]
> Bir runspace'te bulunan tüm cmdletlerin listesini almak için ilgili menü `Get-Command` seçeneğine giriş yapın ve cmdlet'i çalıştırın.


## <a name="next-steps"></a>Sonraki adımlar

Yukarıdaki iş akışlarından birini gerçekleştirirken StorSimple aygıtınızla ilgili herhangi bir sorunla karşılaşırsanız, [StorSimple dağıtımlarını gidermek için Araçlar](storsimple-8000-troubleshoot-deployment.md#tools-for-troubleshooting-storsimple-deployments)bölümüne bakın.

