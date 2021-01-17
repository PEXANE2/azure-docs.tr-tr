---
title: Windows sanal masaüstü Aracısı sorunlarını giderme-Azure
description: Ortak aracı ve bağlantı sorunlarını çözme.
author: Sefriend
ms.topic: troubleshooting
ms.date: 12/16/2020
ms.author: sefriend
manager: clarkn
ms.openlocfilehash: 5f9d772a4cc5722201891450707a68fe487acc3a
ms.sourcegitcommit: fc23b4c625f0b26d14a5a6433e8b7b6fb42d868b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2021
ms.locfileid: "98540770"
---
# <a name="troubleshoot-common-windows-virtual-desktop-agent-issues"></a>Yaygın Windows sanal masaüstü Aracısı sorunlarını giderme

Windows sanal masaüstü Aracısı, birden çok etken nedeniyle bağlantı sorunlarına neden olabilir:
   - Aracıda aracının hizmeti durdurmasına neden olan bir hata.
   - Güncelleştirmeler ile ilgili sorunlar.
   - Aracı yüklemesi sırasında, oturum ana bilgisayarına bağlantıyı kesintiye uğradan yüklemeyle ilgili sorunlar.

Bu makale, bu yaygın senaryolara yönelik çözümler ve bağlantı sorunlarının nasıl ele alınacağını size kılavuzluk eder.

## <a name="error-the-rdagentbootloader-andor-remote-desktop-agent-loader-has-stopped-running"></a>Hata: Rdadgentönyükleme yükleyicisi ve/veya uzak masaüstü Aracısı yükleyicisi çalışmayı durdurdu

Aşağıdaki sorunlardan birini görüyorsanız, bu, aracıyı yükleyen önyükleme yükleyicisinin aracıyı düzgün şekilde yükleyemediği ve Aracı hizmetinin çalışmadığı anlamına gelir:
- **Rdadgentönyükleme yükleyicisi** durdurulmuş ya da çalışmıyor.
- **Uzak Masaüstü Aracısı yükleyicisi** için durum yok.

Bu sorunu çözmek için Rdadgent önyükleme yükleyicisini başlatın:

1. Hizmetler penceresinde, **Uzak Masaüstü Aracısı yükleyicisi**' ne sağ tıklayın.
2. **Başlat**'ı seçin. Bu seçenek sizin için gri değilse, yönetici izinleriniz olmaz ve hizmeti başlatmak için bunları almanız gerekir.
3. 10 saniye bekleyin, ardından **Uzak Masaüstü Aracısı yükleyicisi öğesine** sağ tıklayın.
4. **Yenile**' yi seçin.
5. Hizmet başladıktan ve yenilendikten sonra, bir kayıt hatası olabilir. Daha fazla bilgi için bkz. [INVALID_REGISTRATION_TOKEN](#error-invalid_registration_token).

## <a name="error-invalid_registration_token"></a>Hata: INVALID_REGISTRATION_TOKEN

**Olay Görüntüleyicisi**  >  **Windows günlükleri**  >  **uygulamasına** gidin. KIMLIĞI 3277 olan bir olay görürseniz, açıklamada **INVALID_REGISTRATION_TOKEN** olduğunu belirten kayıt belirteci geçerli olarak tanınmamaktadır.

Bu sorunu çözmek için geçerli bir kayıt belirteci oluşturun:

1. Yeni bir kayıt belirteci oluşturmak için [VM için yeni bir kayıt anahtarı oluşturma](#step-3-generate-a-new-registration-key-for-the-vm) bölümündeki adımları izleyin.
2. Kayıt Defteri Düzenleyicisi'ni açın. 
3. **HKEY_LOCAL_MACHINE**  >  **yazılım**  >  **Microsoft**  >  **rdınfraagent** bölümüne gidin.
4. **Iregistered**' i seçin. 
5. **Değer verisi:** giriş kutusuna **0** yazın ve **Tamam**' ı seçin. 
6. **Registrationtoken**' ı seçin. 
7. **Değer verisi:** giriş kutusunda, 1. adımdaki kayıt belirtecini yapıştırın. 

   > [!div class="mx-imgBorder"]
   > ![Ikaydettirilen 0 ekran görüntüsü](media/isregistered-token.png)

8. Yönetici olarak bir komut istemi açın.
9. **Net stop Rdadgentönyükleme yükleyicisine** girin. 
10. **Net start Rdadgentönyükleme yükleyicisine** girin. 
11. Kayıt Defteri Düzenleyicisi'ni açın.
12. **HKEY_LOCAL_MACHINE**  >  **yazılım**  >  **Microsoft**  >  **rdınfraagent** bölümüne gidin.
13. **Iregistered** 'in 1 olarak ayarlandığını ve **registrationtoken** için veri sütununda hiçbir şey olmadığını doğrulayın. 

   > [!div class="mx-imgBorder"]
   > ![Ikaydettirilen 1 ekran görüntüsü](media/isregistered-registry.png)

## <a name="error-agent-cannot-connect-to-broker-with-invalid_form-or-not_found-url"></a>Hata: aracı INVALID_FORM veya NOT_FOUND ile aracıya bağlanamıyor. URL

**Olay Görüntüleyicisi**  >  **Windows günlükleri**  >  **uygulamasına** gidin. KIMLIĞI 3277 olan bir olay görürseniz **INVALID_FORM** veya **NOT_FOUND diyor. Açıklamada URL** , aracı ve aracı arasındaki iletişimde bir sorun oluştu. Aracı aracıya bağlanamıyor ve belirli bir URL 'ye ulaşamıyor. Bunun nedeni, güvenlik duvarınız veya DNS ayarlarınız olabilir.

Bu sorunu çözmek için, BrokerURI ve BrokerURIGlobal öğesine ulaşabilseniz denetleyin:
1. Kayıt defteri düzenleyicisini açın. 
2. **HKEY_LOCAL_MACHINE**  >  **yazılım**  >  **Microsoft**  >  **rdınfraagent** bölümüne gidin. 
3. **Brokeruri** ve **Brokeruriglobal** değerlerini unutmayın.

   > [!div class="mx-imgBorder"]
   > ![Aracı URI ve aracı URI 'si genel ekran görüntüsü](media/broker-uri.png)

 
4. Bir tarayıcı açın ve *\<BrokerURI\> API/sistem durumu*' na gidin. 
   - **Aracılı URI** içindeki 3. adımdaki değeri kullandığınızdan emin olun. Bu bölümün örneğinde, olacaktır <https://rdbroker-g-us-r0.wvd.microsoft.com/api/health> .
5. Tarayıcıda başka bir sekme açın ve *\<BrokerURIGlobal\> API/sistem durumu*' na gidin. 
   - **Brokeruriglobal** bağlantısında 3. adımdaki değeri kullandığınızdan emin olun. Bu bölümün örneğinde, olacaktır <https://rdbroker.wvd.microsoft.com/api/health> .
6. Ağ, aracı bağlantısını engellemiyor, her iki sayfa da başarıyla yüklenir ve aşağıdaki ekran görüntülerinde gösterildiği gibi **"RD Aracısı sağlıklı"** ifadesini bildiren bir ileti gösterir. 

   > [!div class="mx-imgBorder"]
   > ![Başarıyla yüklenen aracı URI erişiminin ekran görüntüsü](media/broker-uri-web.png)

   > [!div class="mx-imgBorder"]
   > ![Başarıyla yüklenen aracı genel URI erişiminin ekran görüntüsü](media/broker-global.png)
 

7. Ağ, aracı bağlantısını engelliyorsa, aşağıdaki ekran görüntüsünde gösterildiği gibi sayfalar yüklenmez. 

   > [!div class="mx-imgBorder"]
   > ![Başarısız yüklü aracı erişiminin ekran görüntüsü](media/unsuccessful-broker-uri.png)

   > [!div class="mx-imgBorder"]
   > ![Başarısız yüklü aracı genel erişiminin ekran görüntüsü](media/unsuccessful-broker-global.png)

8. Ağ Bu URL 'Leri engelliyorsa, gerekli URL 'Lerin engellemesini kaldırmanız gerekecektir. Daha fazla bilgi için bkz. [gereklı URL listesi](safe-url-list.md).
9. Bu sorunu çözmezse, şifrelemeleri olan ve aracının aracı bağlantısını engelleyecek bir grup ilkesi olmadığından emin olun. Windows sanal masaüstü, [Azure ön kapısının](../frontdoor/front-door-faq.MD#what-are-the-current-cipher-suites-supported-by-azure-front-door)kullandığı TLS 1,2 şifrelemeleri kullanır. Daha fazla bilgi için bkz. [bağlantı güvenliği](network-connectivity.md#connection-security).

## <a name="error-3703-or-3019"></a>Hata: 3703 veya 3019

**Olay Görüntüleyicisi**  >  **Windows günlükleri**  >  **uygulamasına** gidin. KIMLIĞI 3703 olan bir olay görürseniz **RD Ağ Geçidi URL: erişilebilir değil** veya açıklamada kimliği 3019 olan herhangi bir olay görürseniz, aracı ağ geçidi URL 'lerine veya Web yuva Aktarım URL 'lerine erişemez. Oturum ana bilgisayarınıza başarıyla bağlanmak ve bu uç noktalara yönelik ağ trafiğinin kısıtlamaları atlamasına izin vermek için [gereklı URL listesinden](safe-url-list.md)URL 'lerin engellemesini kaldırmanız gerekir. Ayrıca güvenlik duvarınızın veya ara sunucu ayarlarınızın bu URL 'Leri engellemediğinden emin olun. Windows sanal masaüstü 'Nü kullanmak için bu URL 'Lerin engellemesini kaldırma işlemi gereklidir.

Bu sorunu çözmek için güvenlik duvarınızın ve/veya DNS ayarlarınızın bu URL 'Leri engellemediğinden emin olun:
1. [Windows sanal masaüstü dağıtımlarını korumak Için Azure Güvenlik Duvarı 'Nı kullanın.](../firewall/protect-windows-virtual-desktop.md)
2. [Azure Güvenlik DUVARı DNS ayarlarınızı](../firewall/dns-settings.md)yapılandırın.

## <a name="error-installmsiexception"></a>Hata: ınstallmsıexception

**Olay Görüntüleyicisi**  >  **Windows günlükleri**  >  **uygulamasına** gidin. Açıklamasında **ınstallmsıexception** ' ı belirten kimliği 3277 olan bir olay görürseniz, aracıyı yüklemeye çalışırken yükleyici zaten başka bir uygulama için çalışıyor ya da bir ilke msiexec.exe programın çalışmasını engelliyor.

Bu sorunu çözmek için aşağıdaki ilkeyi devre dışı bırakın:
   - Windows Installer kapat  
      - Kategori yolu: Computer Configuration\Administrative Templates\Windows Components\Windows Installer
   
>[!NOTE]
>Bu, yalnızca şu anda farkında olduğumuz ilkelerin kapsamlı bir listesi değildir.

Bir ilkeyi devre dışı bırakmak için:
1. Yönetici olarak bir komut istemi açın.
2. **RSoP. msc** girin ve çalıştırın.
3. Açılan **Ilke sonuç kümesi** penceresinde, kategori yoluna gidin.
4. İlkeyi seçin.
5. **Devre dışı** girişini seçin.
6. **Apply** (Uygula) seçeneğini belirleyin.   

   > [!div class="mx-imgBorder"]
   > ![İlke sonuç kümesinde Windows Installer ilkesinin ekran görüntüsü](media/gpo-policy.png)

## <a name="error-win32exception"></a>Hata: Win32Exception

**Olay Görüntüleyicisi**  >  **Windows günlükleri**  >  **uygulamasına** gidin. Açıklamasında **ınstallmsıexception** ' ı BELIRTEN 3277 kimlikli bir olay görürseniz bir ilke cmd.exe engelliyor olur. Bu programı engellemek, aracı her güncelleştirildiğinde hizmeti yeniden başlatmak için kullanmanız gereken konsol penceresini çalıştırmayı önler.

Bu sorunu çözmek için aşağıdaki ilkeyi devre dışı bırakın:
   - Komut istemine erişimi engelleme   
      - Kategori yolu: Kullanıcı Configuration\Administrative Templates\System
    
>[!NOTE]
>Bu, yalnızca şu anda farkında olduğumuz ilkelerin kapsamlı bir listesi değildir.

Bir ilkeyi devre dışı bırakmak için:
1. Yönetici olarak bir komut istemi açın.
2. **RSoP. msc** girin ve çalıştırın.
3. Açılan **Ilke sonuç kümesi** penceresinde, kategori yoluna gidin.
4. İlkeyi seçin.
5. **Devre dışı** girişini seçin.
6. **Apply** (Uygula) seçeneğini belirleyin.   

## <a name="error-stack-listener-isnt-working-on-windows-10-2004-vm"></a>Hata: yığın dinleyicisi Windows 10 2004 VM 'de çalışmıyor

Komut istemindeki **qwinsta** komutunu çalıştırın ve **RDP-sxs**' nin yanında görünen sürüm numarasını unutmayın. **RDP-TCP** ve **RDP-sxs** bileşenlerini bir sonraki **dinlemek** gerektiğini söylerseniz veya **qwinsta** çalıştırıldıktan sonra hiç gösterilmiyorsa, bir yığın sorunu olduğu anlamına gelir. Yığın güncelleştirmeleri aracı güncelleştirmeleriyle birlikte yüklenir ve bu yükleme Ise geldiğinde Windows sanal masaüstü dinleyicisi çalışmaz.

Bu sorunu çözmek için:
1. Kayıt Defteri Düzenleyicisi'ni açın.
2. **HKEY_LOCAL_MACHINE**  >  **System**  >  **CurrentControlSet**  >  **Control**  >  **Terminal Server**  >  **winistasyonlara** gidin.
3. **Winistasyonların** altında farklı yığın sürümleri için birkaç klasör görebilirsiniz, komut istemindeki **qwinsta** çalıştırırken gördüğünüz sürüm bilgileriyle eşleşen klasörü seçin.
4. **Fsmarconnectmode** bulun ve veri değerinin **1** olduğundan emin olun. Ayrıca, **Fenablewinstation** 'ın **1** olarak ayarlandığından emin olun.

   > [!div class="mx-imgBorder"]
   > ![Fsmarconnectmode ekran görüntüsü](media/fenable-2.png)

5. **Fsmarconnectmode** değeri **1** olarak ayarlanmamışsa **fsmarconnectmode** ' ı seçin ve değer alanına **1** yazın. 
6. **Fenablewinstation** **1** olarak ayarlanmamışsa, **fenablewinstation** ' ı seçin ve değer alanına **1** girin.
7. Sanal makineyi yeniden başlatın. 

>[!NOTE]
>Tek seferde birden çok VM için **Fsmarconnectmode** veya **Fenablewinstation** modunu değiştirmek için aşağıdaki iki işlemi yapabilirsiniz:
>
>- Zaten çalıştığınız makineden kayıt defteri anahtarını dışarı aktarın ve bu değişikliği gerektiren diğer makinelere içeri aktarın.
>- Değişiklik gerektiren makineler için kayıt defteri anahtarı değerini ayarlayan bir genel ilke nesnesi (GPO) oluşturun.

7. **HKEY_LOCAL_MACHINE**  >  **System**  >  **CurrentControlSet**  >  **Control**  >  **Terminal Server**  >  **kümeayarları**' na gidin.
8. **Clustersettings** altında **sessiondirectorylistener** bulun ve veri değerinin **RDP-sxs olduğundan emin olun...**.
9. **Sessiondirectorylistener** , **RDP-sxs...** olarak ayarlanmamışsa, önce aracıyı, önyükleme yükleyicisini ve yığın bileşenlerini kaldırmak ve sonra [aracıyı ve önyükleme yükleyicisini yeniden yüklemek](#step-4-reinstall-the-agent-and-boot-loader)için [Aracı ve önyükleme yükleyicisini kaldırma](#step-1-uninstall-all-agent-boot-loader-and-stack-component-programs) bölümündeki adımları izlemeniz gerekir. Bu işlem, yan yana yığını yeniden yükler.

## <a name="error-users-keep-getting-disconnected-from-session-hosts"></a>Hata: kullanıcıların oturum konaklarından bağlantısı kesilmeleri devam eden

**Olay Görüntüleyicisi**  >  **Windows günlükleri**  >  **uygulamasına** gidin. KIMLIĞI 0 olan ve/veya kullanıcıların oturum konaklarından bağlantısı **kesildiğini belirten bir** olay görürseniz, sunucunuz Windows sanal masaüstü hizmetinden bir sinyal çekmez.

Bu sorunu çözmek için sinyal eşiğini değiştirin:
1. Komut isteinizi yönetici olarak açın.
2. **Qwinsta** komutunu girin ve çalıştırın.
3. İki yığın bileşeni görüntülenmelidir: **RDP-TCP** ve **RDP-sxs**. 
   - Kullanmakta olduğunuz işletim sisteminin sürümüne bağlı olarak, aşağıdaki ekran görüntüsünde gösterildiği gibi, **RDP-sxs** tarafından izlenen yapı numarası gelebilir. Varsa, daha sonra bu numarayı daha sonra yazmak istediğinizden emin olun.
4. Kayıt Defteri Düzenleyicisi'ni açın.
5. **HKEY_LOCAL_MACHINE**  >  **System**  >  **CurrentControlSet**  >  **Control**  >  **Terminal Server**  >  **winistasyonlara** gidin.
6. **Winistasyonların** altında farklı yığın sürümleri için birkaç klasör görebilirsiniz. 3. adımdaki sürüm numarasıyla eşleşen klasörü seçin.
7. Kayıt defteri düzenleyicisine sağ tıklayıp **Yeni**  >  **DWORD (32-bit) değeri**' ni seçerek yeni bir kayıt defteri DWORD oluşturun. DWORD öğesini oluşturduğunuzda, aşağıdaki değerleri girin:
   - HeartbeatInterval: 10000
   - HeartbeatWarnCount: 30 
   - HeartbeatDropCount: 60 
8. Sanal makineyi yeniden başlatın.

## <a name="error-downloadmsiexception"></a>Hata: DownloadMsiException

**Olay Görüntüleyicisi**  >  **Windows günlükleri**  >  **uygulamasına** gidin. Açıklama 3277 olan bir olay görürseniz, açıklamada **Downloadmsiexception** ' ı belirten, Rdadgent için diskte yeterli alan yok.

Bu sorunu çözmek için diskinizde yer açın:
   - Artık Kullanıcı olmayan dosyaları silme
   - VM 'nizin depolama kapasitesini artırma

## <a name="error-vms-are-stuck-in-unavailable-or-upgrading-state"></a>Hata: VM 'Ler kullanılamıyor veya yükseltme durumunda takılmış

Yönetici olarak bir PowerShell penceresi açın ve aşağıdaki cmdlet 'i çalıştırın:

```powershell
Get-AzWvdSessionHost -TenantName <tenantname> -HostPoolName <hostpoolname>|Select-Object*
```

Konak havuzunuzdaki oturum ana bilgisayarı veya konaklar için listelenen durum her zaman **kullanılamıyor** ya da **yükseltme** durumunda olduğunda, aracı veya yığın yüklemesi başarısız olmuş olabilir

Bu sorunu çözmek için, yan yana yığını yeniden yükleyin:
1. Yönetici olarak bir komut istemi açın.
2. **Net stop Rdadgentönyükleme yükleyicisine** girin. 
3. **Denetim Masası**  >  **Programlar**  >  **Programlar ve Özellikler '** e gidin.
4. **Uzak Masaüstü Hizmetleri sxs ağ yığınının** en son sürümünü veya **HKEY_LOCAL_MACHINE**  >  **System**  >  **CurrentControlSet**  >  **Control**  >  **Terminal Server**  >  **winistasyonlarında** listelenen sürümü, **smarconnectlistener** altında kaldırın.
5. Konsol penceresini yönetici olarak açın ve **Program dosyaları**  >  **Microsoft RDInfra**' a gidin.
6. MSI yüklemek için **sxsstack** bileşenini seçin veya **msiexec/I sxsstack- <version> . msi** komutunu çalıştırın.
8. Sanal makineyi yeniden başlatın.
9. Komut istemine geri dönün ve **qwinsta** komutunu çalıştırın.
10. 6. adımda yüklenen yığın bileşeninin yanında **dinler** olduğunu doğrulayın.
   - Bu durumda, komut istemine **net start Rdadgentönyükleme yükleyicisine** girin ve sanal makineyi yeniden başlatın.
   - Aksi takdirde, [VM 'nizi yeniden kaydetmeniz ve aracı bileşenini yeniden yüklemeniz](#your-issue-isnt-listed-here-or-wasnt-resolved) gerekir.

## <a name="error-connection-not-found-rdagent-does-not-have-an-active-connection-to-the-broker"></a>Hata: bağlantı bulunamadı: Rdadgent aracısına etkin bir bağlantısı yok

VM 'niz bağlantı sınırında olabilir, bu nedenle VM yeni bağlantıları kabul edemiyor.

Bu sorunu çözmek için:
   - Maksimum oturum sınırını azaltın. Bu, kaynakların oturum konakları arasında daha eşit bir şekilde dağıtılmasını sağlar ve kaynak tükenmesi önler.
   - VM 'lerin kaynak kapasitesini artırın.

## <a name="error-operating-a-pro-vm-or-other-unsupported-os"></a>Hata: Pro VM veya desteklenmeyen başka bir işletim SISTEMI çalışıyor

Yan yana yığın yalnızca Windows Enterprise veya Windows Server SKU 'Ları tarafından desteklenir. Bu, Pro VM gibi işletim sistemlerinin olmadığı anlamına gelir. Bir kuruluş veya sunucu SKU 'SU yoksa, yığın sanal makinenize yüklenir, ancak etkinleştirilmeyecektir, bu nedenle komut satırınıza **QWINSTA** çalıştırdığınızda gösterilmez.

Bu sorunu çözmek için, Windows Enterprise veya Windows Server olan bir VM oluşturun.
1. [Sanal makine ayrıntıları](create-host-pools-azure-marketplace.md#virtual-machine-details) ' na gidin ve aşağıdaki önerilen görüntülerden birini ayarlamak için 1-12 adımlarını izleyin:
   - Windows 10 Enterprise multi-session, sürüm 1909
   - Windows 10 Enterprise çoklu oturum, sürüm 1909 + Microsoft 365 uygulamalar
   - Windows Server 2019 Datacenter
   - Windows 10 Enterprise multi-session, sürüm 2004
   - Windows 10 Enterprise çoklu oturum, sürüm 2004 + Microsoft 365 uygulamalar
2. **Gözden geçir ve Oluştur '** u seçin.

## <a name="error-name_already_registered"></a>Hata: NAME_ALREADY_REGISTERED

VM 'nizin adı zaten kayıtlı ve muhtemelen yineleniyor.

Bu sorunu çözmek için:
1. [Konak havuzundan oturum konağını kaldırma](#step-2-remove-the-session-host-from-the-host-pool) bölümündeki adımları izleyin.
2. [Başka BIR VM oluşturun](expand-existing-host-pool.md#add-virtual-machines-with-the-azure-portal). Bu VM için benzersiz bir ad seçtiğinizden emin olun.
3. Azure portal] sayfasına gidin ( https://portal.azure.com) ve VM 'nizin bulunduğu konak havuzu Için **genel bakış** sayfasını açın. 
4. **Oturum Konakları** sekmesini açın ve tüm oturum ana bilgisayarlarının bu konak havuzunda bulunduğundan emin olun.
5. Oturum Ana bilgisayar durumunun **kullanılabilir** olması için 5-10 dakika bekleyin.

   > [!div class="mx-imgBorder"]
   > ![Kullanılabilir oturum ana bilgisayarının ekran görüntüsü](media/hostpool-portal.png)

## <a name="your-issue-isnt-listed-here-or-wasnt-resolved"></a>Sorununuz burada listelenmedi veya çözülmedi

Sorununuzu Bu makalede bulamazsanız veya yönergeler size yardımcı olmadıysa, Windows sanal masaüstü aracısını kaldırmanızı, yeniden yüklemenizi ve yeniden kaydetmenizi öneririz. Bu bölümdeki yönergeler, tüm aracı, önyükleme yükleyicisi ve yığın bileşenlerini kaldırarak, konak havuzundan oturum konağını kaldırarak, sanal makine için yeni bir kayıt anahtarı oluşturarak ve aracıyı ve önyükleme yükleyicisini yeniden yükleyerek VM 'nizi Windows sanal masaüstü hizmetine nasıl yeniden kaydetmeniz gerektiğini gösterir. Aşağıdaki senaryolardan biri veya daha fazlası sizin için geçerlidir, şu yönergeleri izleyin:
- VM 'niz **yükseltme** veya **kullanılamaz duruma** takılmış
- Yığın dinleyiciniz çalışmıyor ve Windows 10 1809, 1903 veya 1904 üzerinde çalışıyor
- **EXPIRED_REGISTRATION_TOKEN** hatası alıyorsunuz
- Sanal makinelerinizin oturum Konakları listesinde görünür olduğunu görmüyorsunuz
- Hizmetler penceresinde **Uzak Masaüstü Aracısı yükleyicisini** görmezsiniz
- Görev yöneticisinde **Rdadgentönyükleme yükleyicisi** bileşenini görmezsiniz
- Bu makaledeki yönergeler sorununuzu çözmedi

### <a name="step-1-uninstall-all-agent-boot-loader-and-stack-component-programs"></a>1. Adım: tüm aracıyı, önyükleme yükleyicisini ve yığın bileşeni programlarını kaldırma

Aracıyı, önyükleme yükleyicisini ve yığını yeniden yüklemeden önce, mevcut bileşen programlarını sanal makinenizde kaldırmanız gerekir. Tüm aracıyı, önyükleme yükleyicisini ve Stack bileşen programlarını kaldırmak için:
1. VM 'niz üzerinde yönetici olarak oturum açın. 
2. **Denetim Masası**  >  **Programlar**  >  **Programlar ve Özellikler '** e gidin.
3. Aşağıdaki programları kaldırın:
   - Uzak Masaüstü Aracısı önyükleme yükleyicisi
   - Uzak Masaüstü Hizmetleri altyapı Aracısı
   - Uzak Masaüstü Hizmetleri altyapı Genfiliz Aracısı
   - Uzak Masaüstü Hizmetleri SxS ağ yığını
   
>[!NOTE]
>Bu programların birden çok örneğini görebilirsiniz. Bunların tümünü kaldırdığınızdan emin olun.

   > [!div class="mx-imgBorder"]
   > ![Programları kaldırma ekran görüntüsü](media/uninstall-program.png)

### <a name="step-2-remove-the-session-host-from-the-host-pool"></a>2. Adım: oturum konağını konak havuzundan kaldırma

Oturum konağını konak havuzundan kaldırdığınızda, oturum ana bilgisayarı artık bu konak havuzuna kayıtlı değildir. Bu, oturum ana bilgisayar kaydı için bir sıfırlama işlevi görür. Oturum konağını konak havuzundan kaldırmak için:
1. VM 'nizin içinde bulunduğu konak havuzunun **genel bakış** sayfasına gidin [Azure Portal](https://portal.azure.com). 
2. Bu konak havuzundaki tüm oturum ana bilgisayarlarının listesini görmek için **oturum Konakları** sekmesine gidin.
3. Oturum Konakları listesine bakın ve kaldırmak istediğiniz VM 'yi seçin.
4. **Kaldır**' ı seçin.  

   > [!div class="mx-imgBorder"]
   > ![VM 'yi konak havuzundan kaldırma ekran görüntüsü](media/remove-sh.png)

### <a name="step-3-generate-a-new-registration-key-for-the-vm"></a>3. Adım: VM için yeni bir kayıt anahtarı oluşturma

VM 'nizi konak havuzuna ve hizmetine yeniden kaydetmek için kullanılan yeni bir kayıt anahtarı oluşturmanız gerekir. VM için yeni bir kayıt anahtarı oluşturmak için:
1. [Azure Portal](https://portal.azure.com) açın ve düzenlemek istediğiniz VM 'nin konak havuzu Için **genel bakış** sayfasına gidin.
2. **Kayıt anahtarı**' nı seçin.

   > [!div class="mx-imgBorder"]
   > ![Portalda kayıt anahtarının ekran görüntüsü](media/reg-key.png)

3. **Kayıt anahtarı** sekmesini açın ve **Yeni anahtar oluştur**' u seçin.
4. Sona erme tarihini girip **Tamam**' ı seçin.  

>[!NOTE]
>Sona erme tarihi bir saatten az olamaz ve oluşturulma zamanı ve tarihinden itibaren 27 günden daha uzun olamaz. Son kullanma tarihini en fazla 27 gün olarak ayarlamanızı öneririz.

5. Yeni oluşturulan anahtarı panonuza kopyalayın. Bu anahtara daha sonra ihtiyacınız olacak.

### <a name="step-4-reinstall-the-agent-and-boot-loader"></a>4. Adım: aracıyı ve önyükleme yükleyicisini yeniden yükleme

Aracı ve önyükleme yükleyicisinin en güncel sürümünü yeniden yükleyerek, yan yana yığın ve Geneva izleme Aracısı da otomatik olarak yüklenir. Aracıyı ve önyükleme yükleyicisini yeniden yüklemek için:
1. VM 'niz üzerinde yönetici olarak oturum açın ve **Windows sanal masaüstü aracısını** ve **Windows sanal masaüstü Aracısı önyükleme yükleyicisine** Indirmek için [sanal makineleri kaydetme](create-host-pools-powershell.md#register-the-virtual-machines-to-the-windows-virtual-desktop-host-pool) bölümündeki yönergeleri izleyin.

   > [!div class="mx-imgBorder"]
   > ![Aracının ve önyükleme yükleyicisinden indirme sayfasının ekran görüntüsü](media/download-agent.png)

2. Az önce indirdiğiniz aracıya ve önyükleme yükleyicisi yükleyicilerine sağ tıklayın.
3. **Özellikler**’i seçin.
4. **Engellemeyi kaldır**' ı seçin.
5. **Tamam**'ı seçin.
6. Aracı yükleyicisini çalıştırın.
7. Yükleyici kayıt belirtecini istediğinde kayıt defteri anahtarını panodan yapıştırın. 

   > [!div class="mx-imgBorder"]
   > ![Yapıştırılan kayıt belirtecinin ekran görüntüsü](media/pasted-agent-token.png)

8. Önyükleme yükleyicisi yükleyicisini çalıştırın.
9. Sanal makineyi yeniden başlatın. 
10. [Azure Portal](https://portal.azure.com) gıdın ve VM 'nizin ait olduğu konak havuzunun **genel bakış** sayfasını açın.
11. Bu konak havuzundaki tüm oturum ana bilgisayarlarının listesini görmek için **oturum Konakları** sekmesine gidin.
12. Artık, **kullanılabilir** duruma sahip ana bilgisayar havuzunda kayıtlı oturum konağını görmeniz gerekir. 

   > [!div class="mx-imgBorder"]
   > ![Kullanılabilir oturum ana bilgisayarının ekran görüntüsü](media/hostpool-portal.png)

## <a name="next-steps"></a>Sonraki adımlar

Sorun devam ederse, bir destek talebi oluşturun ve karşılaştığınız sorun hakkında ayrıntılı bilgileri ve sorunu çözmeye çalışmak için yaptığınız eylemleri ekleyin. Aşağıdaki liste, Windows sanal masaüstü dağıtımınızdaki sorunları gidermek için kullanabileceğiniz diğer kaynakları içerir.

- Windows sanal masaüstü ve yükseltme izlemelerinin sorunlarını giderme hakkında genel bilgi için bkz. [sorun giderme genel bakış, geri bildirim ve destek](troubleshoot-set-up-overview.md).
- Windows sanal masaüstü ortamında bir konak havuzu oluştururken oluşan sorunları gidermek için, bkz. [ortam ve konak havuzu oluşturma](troubleshoot-set-up-issues.md).
- Windows sanal masaüstündeki bir sanal makineyi (VM) yapılandırırken oluşan sorunları gidermek için bkz. [oturum ana bilgisayarı sanal makine yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için bkz. [Windows sanal masaüstü hizmeti bağlantıları](troubleshoot-service-connection.md).
- Uzak Masaüstü istemcileriyle ilgili sorunları gidermek için bkz. [Uzak Masaüstü Istemcisinde sorun giderme](troubleshoot-client.md).
- Windows sanal masaüstü ile PowerShell kullanırken karşılaşılan sorunları gidermek için bkz. [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Hizmet hakkında daha fazla bilgi edinmek için bkz. [Windows sanal masaüstü ortamı](environment-setup.md).
- Sorun giderme öğreticisini öğrenmek için bkz. [öğretici: Kaynak Yöneticisi şablonu dağıtımlarının sorunlarını giderme](../azure-resource-manager/templates/template-tutorial-troubleshoot.md).
- Denetim eylemleri hakkında bilgi edinmek için bkz. [Kaynak Yöneticisi Ile denetim işlemleri](../azure-resource-manager/management/view-activity-logs.md).
- Dağıtım sırasında hataları belirleme eylemleri hakkında bilgi edinmek için bkz. [dağıtım Işlemlerini görüntüleme](../azure-resource-manager/templates/deployment-history.md).
