---
title: StorSimple cihazınıza uzaktan bağlanın
description: Uzaktan yönetim için cihazınızı nasıl yapılandırıştırılacayacağınızı ve HTTP veya HTTPS yoluyla StorSimple için Windows PowerShell' e nasıl bağlayıcılığını açıklar.
author: alkohli
ms.service: storsimple
ms.topic: conceptual
ms.date: 01/02/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 58d61df932da06e32bb4c8f21a3a296b185f02d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80299000"
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınıza uzaktan bağlanın

## <a name="overview"></a>Genel Bakış

Windows PowerShell üzerinden cihazınıza uzaktan bağlanabilirsiniz. Bu şekilde bağlandığınızda, bir menü görmezsiniz. (Yalnızca bağlanmak için aygıttaki seri konsolu kullanırsanız bir menü görürsünüz.) Windows PowerShell remoting ile belirli bir çalışma alanına bağlanırsınız. Görüntü dilini de belirtebilirsiniz.

Cihazınızı yönetmek için Windows PowerShell remoting kullanma hakkında daha fazla bilgi için [StorSimple cihazınızı yönetmek için StorSimple için Windows PowerShell'i kullanın'a](storsimple-8000-windows-powershell-administration.md)gidin.

Bu öğretici, uzaktan yönetim için cihazınızı nasıl yapılandırıştırılacayave StorSimple için Windows PowerShell'e nasıl bağlanılacak açıklanmaktadır. Windows PowerShell üzerinden uzaktan bağlanmak için HTTP veya HTTPS'yi kullanabilirsiniz. Ancak, StorSimple için Windows PowerShell'e nasıl bağlanabileceğinize karar verirken aşağıdaki bilgileri göz önünde bulundurun:

* Doğrudan aygıt seri konsoluna bağlanmak güvenlidir, ancak seri konsola ağ anahtarları üzerinden bağlanmak güvenli değildir. Ağ anahtarları üzerinden aygıt seri konsoluna bağlanırken güvenlik riskine karşı dikkatli olun.
* BIR HTTP oturumu aracılığıyla bağlanmak, ağ üzerinden seri konsol üzerinden bağlanmaktan daha fazla güvenlik sağlayabilir. Bu en güvenli yöntem olmasa da, güvenilen ağlarda kabul edilebilir.
* Https oturumu aracılığıyla kendi imzalı bir sertifikayla bağlanmak en güvenli ve önerilen seçenektir.

Windows PowerShell arabirimine uzaktan bağlanabilirsiniz. Ancak, StorSimple cihazınıza Windows PowerShell arabirimi üzerinden uzaktan erişim varsayılan olarak etkinleştirilemez. Önce aygıtta, sonra da cihazınıza erişmek için kullanılan istemcide uzaktan yönetimi etkinleştirmeniz gerekir.

Bu makalede açıklanan adımlar, Windows Server 2012 R2 çalıştıran bir ana bilgisayar sisteminde gerçekleştirilmiştir.

## <a name="connect-through-http"></a>HTTP üzerinden bağlanın

Bir HTTP oturumu aracılığıyla StorSimple için Windows PowerShell'e bağlanmak, StorSimple cihazınızın seri konsoluna bağlanmaktan daha fazla güvenlik sağlar. Bu en güvenli yöntem olmasa da, güvenilen ağlarda kabul edilebilir.

Uzaktan yönetimi yapılandırmak için Azure portalını veya seri konsolu kullanabilirsiniz. Aşağıdaki yordamlardan birini seçin:

* HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanın
* [HTTP üzerinden uzaktan yönetimi etkinleştirmek için seri konsolu kullanın](#use-the-serial-console-to-enable-remote-management-over-http)

Uzaktan yönetimi etkinleştirdikten sonra, istemciyi uzak bir bağlantıya hazırlamak için aşağıdaki yordamı kullanın.

* [İstemciyi uzaktan bağlantıya hazırlama](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a>HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanın

HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-azure-portal"></a>Azure portalı üzerinden uzaktan yönetimi etkinleştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Aygıtlar'ı** seçin ve ardından uzaktan yönetim için yapılandırmak istediğiniz aygıtı seçin ve tıklatın. Güvenlik **> Aygıt ayarlarına**gidin.
2. Güvenlik **ayarları** bıçak, **Uzaktan Yönetim'i**tıklatın.
3. Uzaktan **yönetim** bıçağında, Uzaktan Yönetimi **Evet'e** **etkinleştir'i** ayarlayın.
4. Artık HTTP kullanarak bağlanmayı seçebilirsiniz. (Varsayılan olarak HTTPS üzerinden bağlanmaktır.) HTTP'nin seçildiğinden emin olun.
   
   > [!NOTE]
   > HTTP üzerinden bağlanma yalnızca güvenilen ağlarda kabul edilebilir.
   
5. **Kaydet'i** tıklatın ve onay istendiğinde **Evet'i**seçin.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>HTTP üzerinden uzaktan yönetimi etkinleştirmek için seri konsolu kullanın
Uzaktan yönetimi etkinleştirmek için aygıt seri konsolunda aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Aygıt seri konsolu üzerinden uzaktan yönetimi etkinleştirmek için
1. Seri konsol menüsünde 1 seçeneğini seçin. Aygıttaki seri konsolu kullanma hakkında daha fazla bilgi için, [aygıt seri konsolu üzerinden StorSimple için Windows PowerShell'e Bağlan'a](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)gidin.
2. İsteyerek, yazın:`Enable-HcsRemoteManagement –AllowHttp`
3. Cihaza bağlanmak için HTTP'yi kullanmanın güvenlik açıkları hakkında bilgilendirilirsiniz. İstendiğinde, **Y**yazarak onaylayın.
4. HTTP'nin yazarak etkinleştirildiğinden doğrulayın:`Get-HcsSystem`
5. **RemoteManagementMode** alanının **HttpsAndHttpEnabled'i**gösterdiğini doğrulayın. Aşağıdaki resimde PuTTY bu ayarları gösterir.
   
     ![Seri HTTPS ve HTTP etkin](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>İstemciyi uzaktan bağlantıya hazırlama
Uzaktan yönetimi etkinleştirmek için istemci üzerinde aşağıdaki adımları gerçekleştirin.

#### <a name="to-prepare-the-client-for-remote-connection"></a>İstemciyi uzaktan bağlantıya hazırlamak için
1. Bir Windows PowerShell oturumunu yönetici olarak başlatın. Bir Windows 10 istemcisi kullanıyorsanız, varsayılan olarak, Windows Uzaktan Yönetim hizmeti el ile ayarlanır. Aşağıdakileri yazarak hizmeti başlatmanız gerekebilir:

    `Start-Service WinRM`
    
2. StorSimple aygıtının IP adresini istemcinin güvenilir ana bilgisayarlar listesine eklemek için aşağıdaki komutu yazın:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <*device_ip*> cihazınızın IP adresiyle değiştirin; örneğin: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Aygıt kimlik bilgilerini bir değişkene kaydetmek için aşağıdaki komutu yazın: 
   
    ```
    $cred = Get-Credential
    ```
    
4. Görünen iletişim kutusunda:
   
   1. Bu biçimde kullanıcı adını yazın: *device_ip\SSAdmin*.
   2. Aygıt kurulum sihirbazıyla yapılandırıldığında ayarlanan aygıt yöneticisi parolasını yazın. Varsayılan parola *Password1'dir.*
5. Bu komutu yazarak aygıtta bir Windows PowerShell oturumu başlatın:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > StorSimple sanal aygıtıyla kullanılmak üzere bir Windows PowerShell `–Port` oturumu oluşturmak için, parametreyi ekleyin ve StorSimple Virtual Appliance için Remoting'de yapılandırdığınız ortak bağlantı noktasını belirtin.
   
   
Bu noktada, aygıta etkin bir uzak Windows PowerShell oturumu olmalıdır.
   
![HTTP kullanarak PowerShell remoting](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>HTTPS ile bağlanın

Bir HTTPS oturumu aracılığıyla StorSimple için Windows PowerShell'e bağlanmak, Microsoft Azure StorSimple aygıtınıza uzaktan bağlanmanın en güvenli ve önerilen yöntemidir. Aşağıdaki yordamlar, StorSimple için Windows PowerShell'e bağlanmak için HTTPS'yi kullanabilmeniz için seri konsolve istemci bilgisayarlarının nasıl ayarlanabileceğinizi açıklar.

Uzaktan yönetimi yapılandırmak için Azure portalını veya seri konsolu kullanabilirsiniz. Aşağıdaki yordamlardan birini seçin:

* HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanın
* [HTTPS üzerinden uzaktan yönetimi etkinleştirmek için seri konsolu kullanın](#use-the-serial-console-to-enable-remote-management-over-https)

Uzaktan yönetimi etkinleştirdikten sonra, ana bilgisayarı uzaktan yönetime hazırlamak ve uzaktan ana bilgisayardan aygıta bağlanmak için aşağıdaki yordamları kullanın.

* [Ev sahibini uzaktan yönetime hazırlayın](#prepare-the-host-for-remote-management)
* [Aygıta uzak ana bilgisayardan bağlanma](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a>HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanın

HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a>Azure portalından HTTPS üzerinden uzaktan yönetimi etkinleştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Aygıtlar'ı** seçin ve ardından uzaktan yönetim için yapılandırmak istediğiniz aygıtı seçin ve tıklatın. Güvenlik **> Aygıt ayarlarına**gidin.
2. Güvenlik **ayarları** bıçak, **Uzaktan Yönetim'i**tıklatın.
3. **Uzaktan yönetimi Etkinleştir**’i **Evet** olarak ayarlayın.
4. Artık HTTPS'yi kullanarak bağlanmayı seçebilirsiniz. (Varsayılan olarak HTTPS üzerinden bağlanmaktır.) HTTPS'nin seçildiğinden emin olun.
5. Tıklatın... ve ardından **Uzaktan Yönetim Sertifikası İndir'i**tıklatın. Bu dosyayı kaydetmek için bir konum belirtin. Bu sertifikayı aygıta bağlanmak için kullanacağınız istemci veya ana bilgisayara yüklemeniz gerekir.
6. **Kaydet'i** tıklatın ve onay istendiğinde **Evet'i** tıklatın.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>HTTPS üzerinden uzaktan yönetimi etkinleştirmek için seri konsolu kullanın

Uzaktan yönetimi etkinleştirmek için aygıt seri konsolunda aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Aygıt seri konsolu üzerinden uzaktan yönetimi etkinleştirmek için
1. Seri konsol menüsünde 1 seçeneğini seçin. Aygıttaki seri konsolu kullanma hakkında daha fazla bilgi için, [aygıt seri konsolu üzerinden StorSimple için Windows PowerShell'e Bağlan'a](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)gidin.
2. İsteyerek, yazın:
   
     `Enable-HcsRemoteManagement`
   
    Bu, cihazınızda HTTPS'yi etkinleştirmelidir.
3. HTTPS'nin yazarak etkinleştirildiğini doğrulayın: 
   
     `Get-HcsSystem`
   
    **RemoteManagementMode** alanının **HttpsEnabled'ı**gösterdiğinden emin olun. Aşağıdaki resimde PuTTY bu ayarları gösterir.
   
     ![Seri HTTPS etkin](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. `Get-HcsSystem`Çıktıdan, aygıtın seri numarasını kopyalayın ve daha sonra kullanmak üzere kaydedin.
   
   > [!NOTE]
   > Seri numarası, sertifikadaki CN adı ile eşler.
   
5. Yazarak uzaktan yönetim sertifikası edinin: 
   
     `Get-HcsRemoteManagementCert`
   
    Aşağıdakine benzer bir sertifika görüntülenir.
   
    ![Uzaktan yönetim sertifikası alın](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Sertifikadaki bilgileri **-----BEGIN CERTIFICATE-----** dan **-----END CERTIFICATE-----** not defteri gibi bir metin düzenleyicisine kopyalayın ve .cer dosyası olarak kaydedin. (Ana bilgisayarı hazırlarken bu dosyayı uzak ana bilgisayara kopyalarsınız.)
   
   > [!NOTE]
   > Yeni bir sertifika oluşturmak `Set-HcsRemoteManagementCert` için cmdlet kullanın.
   
### <a name="prepare-the-host-for-remote-management"></a>Ev sahibini uzaktan yönetime hazırlayın

Ana bilgisayar, HTTPS oturumu kullanan bir uzak bağlantı için hazırlamak için aşağıdaki yordamları gerçekleştirin:

* [.cer dosyasını istemcinin veya uzak ana bilgisayarın kök deposuna aktarın.](#to-import-the-certificate-on-the-remote-host)
* [Aygıt seri numaralarını uzak ana bilgisayarınızdaki ana bilgisayar dosyasına ekleyin.](#to-add-device-serial-numbers-to-the-remote-host)

Her önceki yordamlar, aşağıda açıklanmıştır.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Sertifikayı uzak ana bilgisayarda almak için
1. .cer dosyasına sağ tıklayın ve **sertifikayı yükle'yi**seçin. Bu Sertifika Alma Sihirbazı başlar.
   
    ![Sertifika Alma Sihirbazı 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. **Mağaza konumu**için **Yerel Makine'yi**seçin ve sonra **İleri'yi**tıklatın.
3. **Aşağıdaki mağazadaki tüm sertifikaları yerleştir'i**seçin ve ardından **Gözat'ı**tıklatın. Uzak ana makinenizin kök deposuna gidin ve sonra **İleri'yi**tıklatın.
   
    ![Sertifika Alma Sihirbazı 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. **Son**'a tıklayın. Alma işleminin başarılı olduğunu belirten bir ileti görüntülenir.
   
    ![Sertifika Alma Sihirbazı 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Aygıt seri numaralarını uzak ana bilgisayara eklemek için
1. Not Defteri'ni yönetici olarak başlatın ve \Windows\System32\Drivers\etc adresinde bulunan ana bilgisayar dosyasını açın.
2. Ev sahibi dosyanıza aşağıdaki üç girişi ekleyin: **DATA 0 IP adresi**, Denetleyici 0 Sabit IP **adresi**ve Denetleyici 1 Sabit **IP adresi**.
3. Daha önce kaydettiğiniz aygıt seri numarasını girin. Bunu aşağıdaki resimde gösterildiği gibi IP adresiyle eşleyin. Controller 0 ve Controller 1 için, seri numarasının (CN adı) sonuna **Controller0** ve **Controller1'i** ekler.
   
    ![Ana bilgisayarlardosyasına CN Adı ekleme](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Ana bilgisayar dosyasını kaydedin.

### <a name="connect-to-the-device-from-the-remote-host"></a>Aygıta uzak ana bilgisayardan bağlanma

Uzak bir ana bilgisayardan veya istemciden cihazınıza bir SSAdmin oturumu girmek için Windows PowerShell ve TLS'yi kullanın. SSAdmin oturumu, cihazınızın [seri konsol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menüsünde seçenek 1'e eşlenir.

Uzak Windows PowerShell bağlantısını yapmak istediğiniz bilgisayarda aşağıdaki yordamı gerçekleştirin.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-tls"></a>Windows PowerShell ve TLS kullanarak cihaza SSAdmin oturumu girmek için
1. Bir Windows PowerShell oturumunu yönetici olarak başlatın. Bir Windows 10 istemcisi kullanıyorsanız, varsayılan olarak, Windows Uzaktan Yönetim hizmeti el ile ayarlanır. Aşağıdakileri yazarak hizmeti başlatmanız gerekebilir:

    `Start-Service WinRM`

2. Aygıt IP adresini, aşağıdakileri yazarak istemcinin güvenilir ana bilgisayarlarına ekleyin:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <*device_ip*> cihazınızın IP adresidir; örneğin: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Yeni bir kimlik bilgisi oluşturmak için yazın:
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Hedef *aygıtın <IP'sinin*> cihazınız için DATA 0 IP adresi olduğu durumlarda; örneğin, **10.126.173.90** ana bilgisayarların önceki resimde gösterildiği gibi. Ayrıca, aygıtınız için yönetici parolasını girin.
4. Yazarak oturum oluşturun:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    cmdlet'teki -ComputerName parametresi için *hedef aygıtın* <seri numarasını>. Bu seri numarası, uzak ana makinenizdeki ana bilgisayar dosyasındaki DATA 0'ın IP adresine eşlenmiştir; örneğin, **SHX0991003G44MT** aşağıdaki resimde gösterildiği gibi.
5. Şunu yazın:
   
     `Enter-PSSession $session`
6. Birkaç dakika beklemeniz gerekir ve ardından TLS üzerinden HTTPS üzerinden cihazınıza bağlanırsınız. Cihazınıza bağlı olduğunuzu belirten bir ileti görürsünüz.
   
    ![HTTPS ve TLS kullanarak PowerShell remoting](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple cihazınızı yönetmek için Windows PowerShell'i kullanma](storsimple-8000-windows-powershell-administration.md)hakkında daha fazla bilgi edinin.
* [StorSimple cihazınızı yönetmek için StorSimple Device Manager hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

