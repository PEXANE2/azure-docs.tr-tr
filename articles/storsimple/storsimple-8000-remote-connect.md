---
title: StorSimple cihazınıza uzaktan bağlanma
description: Cihazınızın uzaktan yönetim için nasıl yapılandırılacağını ve HTTP veya HTTPS üzerinden StorSimple için Windows PowerShell nasıl bağlanacağını açıklar.
author: alkohli
ms.service: storsimple
ms.topic: how-to
ms.date: 01/02/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1da688dfb00b26ca6b561d5aa0fb548c221381c5
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85514567"
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınıza uzaktan bağlanma

## <a name="overview"></a>Genel Bakış

Windows PowerShell aracılığıyla cihazınıza uzaktan bağlanabilirsiniz. Bu şekilde bağladığınızda, bir menü görmezsiniz. (Bir menü yalnızca, bağlanmak için cihazda seri Konsolu kullanıyorsanız görüntülenir.) Windows PowerShell uzaktan iletişimi ile belirli bir çalışma alanına bağlanırsınız. Ayrıca, görüntüleme dilini de belirtebilirsiniz.

Cihazınızı yönetmek için Windows PowerShell uzaktan iletişimini kullanma hakkında daha fazla bilgi için, [StorSimple için Windows PowerShell kullanma bölümüne giderek StorSimple cihazınızı](storsimple-8000-windows-powershell-administration.md)yönetin.

Bu öğreticide, cihazınızın uzaktan yönetimi için nasıl yapılandırılacağı ve ardından StorSimple için Windows PowerShell nasıl bağlanacağı açıklanmaktadır. HTTP veya HTTPS kullanarak Windows PowerShell aracılığıyla uzaktan bağlanabilirsiniz. Ancak StorSimple için Windows PowerShell bağlanma konusunda karar verirken aşağıdaki bilgileri göz önünde bulundurun:

* Doğrudan cihaz seri konsoluna bağlanmak güvenlidir, ancak ağ anahtarları üzerinden seri konsoluna bağlanmak değildir. Ağ anahtarları üzerinden cihaz seri konsoluna bağlanılırken güvenlik riskine karşı dikkatli olun.
* HTTP oturumundan bağlantı, ağ üzerinden seri konsol üzerinden bağlanılarak daha fazla güvenlik sağlayabilir. Bu en güvenli yöntem olmasa da, güvenilen ağlarda kabul edilebilir.
* Otomatik olarak imzalanan bir sertifika ile HTTPS oturumu üzerinden bağlanmak en güvenli ve önerilen seçenektir.

Windows PowerShell arabirimine uzaktan bağlanabilirsiniz. Ancak, Windows PowerShell arabirimi aracılığıyla StorSimple cihazınıza Uzaktan erişim varsayılan olarak etkin değildir. Önce cihazda ve sonra cihazınıza erişmek için kullanılan istemcisinde uzaktan yönetimi etkinleştirmeniz gerekir.

Bu makalede açıklanan adımlar, Windows Server 2012 R2 çalıştıran bir konak sisteminde gerçekleştirilmiştir.

## <a name="connect-through-http"></a>HTTP üzerinden Bağlan

HTTP oturumu üzerinden StorSimple için Windows PowerShell bağlanmak, StorSimple cihazınızın seri konsolundan bağlantı kurarak daha fazla güvenlik sunar. Bu en güvenli yöntem olmasa da, güvenilen ağlarda kabul edilebilir.

Uzaktan yönetimi yapılandırmak için Azure portal ya da seri konsolunu kullanabilirsiniz. Aşağıdaki yordamlardan seçim yapın:

* HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portal kullanın
* [HTTP üzerinden uzaktan yönetimi etkinleştirmek için seri konsolunu kullanma](#use-the-serial-console-to-enable-remote-management-over-http)

Uzaktan Yönetimi etkinleştirdikten sonra, istemciyi uzak bağlantı için hazırlamak üzere aşağıdaki yordamı kullanın.

* [İstemciyi uzak bağlantı için hazırlama](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a>HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portal kullanın

HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portal aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-azure-portal"></a>Azure portal aracılığıyla uzaktan yönetimi etkinleştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Cihazlar** ' ı seçin ve ardından uzaktan yönetim için yapılandırmak istediğiniz cihazı seçin ve tıklatın. **Cihaz ayarları > güvenlik**' e gidin.
2. **Güvenlik ayarları** dikey penceresinde **Uzaktan Yönetim**' e tıklayın.
3. **Uzaktan Yönetim** dikey penceresinde, **uzaktan yönetimi etkinleştir** ' i **Evet**olarak ayarlayın.
4. Artık HTTP kullanarak bağlanmayı seçebilirsiniz. (Varsayılan, HTTPS üzerinden bağlandır.) HTTP 'nin seçili olduğundan emin olun.
   
   > [!NOTE]
   > HTTP üzerinden bağlanma yalnızca güvenilen ağlarda kabul edilebilir.
   
5. **Kaydet** ' e tıklayın ve onay istendiğinde **Evet**' i seçin.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>HTTP üzerinden uzaktan yönetimi etkinleştirmek için seri konsolunu kullanma
Uzaktan yönetimi etkinleştirmek için cihaz seri konsolunda aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Cihaz seri konsolu aracılığıyla uzaktan yönetimi etkinleştirmek için
1. Seri konsol menüsünde, 1 seçeneğini belirleyin. Cihaza seri konsolu kullanma hakkında daha fazla bilgi için [cihaz seri konsolu aracılığıyla StorSimple için Windows PowerShell Bağlan](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)' a gidin.
2. İsteminde şunu yazın:`Enable-HcsRemoteManagement –AllowHttp`
3. Cihaza bağlanmak için HTTP kullanma güvenlik açıkları hakkında bildirim alırsınız. İstendiğinde, **Y**yazarak doğrulayın.
4. Aşağıdakileri yazarak HTTP 'nin etkinleştirildiğini doğrulayın:`Get-HcsSystem`
5. **Remotemanagementmode** alanında **Httpsandhttpenabled**' ın görüntülendiğini doğrulayın. Aşağıdaki çizimde, PuTTY içindeki bu ayarlar gösterilmektedir.
   
     ![Seri HTTPS ve HTTP etkin](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>İstemciyi uzak bağlantı için hazırlama
Uzaktan yönetimi etkinleştirmek için istemcisinde aşağıdaki adımları gerçekleştirin.

#### <a name="to-prepare-the-client-for-remote-connection"></a>İstemciyi uzak bağlantı için hazırlamak için
1. Yönetici olarak bir Windows PowerShell oturumu başlatın. Bir Windows 10 istemcisi kullanıyorsanız, varsayılan olarak Windows Uzaktan Yönetimi hizmeti el ile olarak ayarlanır. Hizmeti şunu yazarak başlatmanız gerekebilir:

    `Start-Service WinRM`
    
2. StorSimple cihazının IP adresini istemcinin güvenilen konaklar listesine eklemek için aşağıdaki komutu yazın:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     <*device_ip*>, cihazınızın IP adresiyle değiştirin; Örneğin: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Cihaz kimlik bilgilerini bir değişkende kaydetmek için aşağıdaki komutu yazın: 
   
    ```
    $cred = Get-Credential
    ```
    
4. Görüntülenen iletişim kutusunda:
   
   1. Kullanıcı adını şu biçimde yazın: *device_ip \SSAdmin*.
   2. Cihaz, Kurulum Sihirbazı ile yapılandırıldığında ayarlanan cihaz yönetici parolasını yazın. Varsayılan parola *Parola1*' dir.
5. Şu komutu yazarak cihazda bir Windows PowerShell oturumu başlatın:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > StorSimple Sanal cihazından kullanılmak üzere bir Windows PowerShell oturumu oluşturmak için, `–Port` parametresini ekleyin ve StorSimple Sanal Gereci Için uzaktan iletişim kutusunda yapılandırdığınız genel bağlantı noktasını belirtin.
   
   
Bu noktada, cihaza etkin bir uzak Windows PowerShell oturumunuz olması gerekir.
   
![HTTP kullanarak PowerShell uzaktan iletişimi](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>HTTPS üzerinden Bağlan

HTTPS oturumu üzerinden StorSimple için Windows PowerShell bağlanmak, Microsoft Azure StorSimple cihazınıza uzaktan bağlanmak için en güvenli ve önerilen yöntemdir. Aşağıdaki yordamlarda, StorSimple için Windows PowerShell bağlanmak üzere HTTPS kullanabilmeniz için seri konsol ve istemci bilgisayarlarının nasıl ayarlanacağı açıklanmaktadır.

Uzaktan yönetimi yapılandırmak için Azure portal ya da seri konsolunu kullanabilirsiniz. Aşağıdaki yordamlardan seçim yapın:

* HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portal kullanın
* [HTTPS üzerinden uzaktan yönetimi etkinleştirmek için seri konsolunu kullanma](#use-the-serial-console-to-enable-remote-management-over-https)

Uzaktan Yönetimi etkinleştirdikten sonra, Konağı bir uzaktan yönetime hazırlamak ve uzak ana bilgisayardan cihaza bağlanmak için aşağıdaki yordamları kullanın.

* [Konağı uzaktan yönetim için hazırlama](#prepare-the-host-for-remote-management)
* [Uzak konaktan cihaza Bağlan](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a>HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portal kullanın

HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portal aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a>Azure portal HTTPS üzerinden uzaktan yönetimi etkinleştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. **Cihazlar** ' ı seçin ve ardından uzaktan yönetim için yapılandırmak istediğiniz cihazı seçin ve tıklatın. **Cihaz ayarları > güvenlik**' e gidin.
2. **Güvenlik ayarları** dikey penceresinde **Uzaktan Yönetim**' e tıklayın.
3. **Uzaktan yönetimi Etkinleştir**’i **Evet** olarak ayarlayın.
4. Artık HTTPS kullanarak bağlanmayı seçebilirsiniz. (Varsayılan, HTTPS üzerinden bağlandır.) HTTPS 'nin seçili olduğundan emin olun.
5. Tıklayın... ve sonra **Uzaktan yönetim sertifikasını indir**' e tıklayın. Bu dosyayı kaydetmek için bir konum belirtin. Bu sertifikayı cihaza bağlanmak için kullanacağınız istemci veya ana bilgisayara yüklemeniz gerekir.
6. **Kaydet** ' e tıklayın ve onay istendiğinde **Evet** ' e tıklayın.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>HTTPS üzerinden uzaktan yönetimi etkinleştirmek için seri konsolunu kullanma

Uzaktan yönetimi etkinleştirmek için cihaz seri konsolunda aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Cihaz seri konsolu aracılığıyla uzaktan yönetimi etkinleştirmek için
1. Seri konsol menüsünde, 1 seçeneğini belirleyin. Cihaza seri konsolu kullanma hakkında daha fazla bilgi için [cihaz seri konsolu aracılığıyla StorSimple için Windows PowerShell Bağlan](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console)' a gidin.
2. İsteminde şunu yazın:
   
     `Enable-HcsRemoteManagement`
   
    Bu, cihazınızda HTTPS 'yi etkinleştirmelidir.
3. Şunu yazarak HTTPS 'nin etkinleştirildiğini doğrulayın: 
   
     `Get-HcsSystem`
   
    **Remotemanagementmode** alanında **httpsenabled**gösterildiğinizden emin olun. Aşağıdaki çizimde, PuTTY içindeki bu ayarlar gösterilmektedir.
   
     ![Seri HTTPS etkin](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Çıktısından `Get-HcsSystem` , cihazın seri numarasını kopyalayın ve daha sonra kullanmak üzere kaydedin.
   
   > [!NOTE]
   > Seri numarası, sertifikadaki CN adıyla eşlenir.
   
5. Şunu yazarak bir uzaktan yönetim sertifikası edinin: 
   
     `Get-HcsRemoteManagementCert`
   
    Aşağıdakine benzer bir sertifika görüntülenir.
   
    ![Uzaktan Yönetim sertifikası al](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. **-----Başlangıç sertifikası-----** , **son sertifika-----** Not Defteri gibi bir metin düzenleyicisine-----ve. cer dosyası olarak kaydetmek için sertifikadaki bilgileri kopyalayın. (Konağı hazırlarken bu dosyayı uzak ana bilgisayarınıza kopyalayacaksınız.)
   
   > [!NOTE]
   > Yeni bir sertifika oluşturmak için `Set-HcsRemoteManagementCert` cmdlet 'ini kullanın.
   
### <a name="prepare-the-host-for-remote-management"></a>Konağı uzaktan yönetim için hazırlama

Ana bilgisayarı HTTPS oturumu kullanan bir uzak bağlantı için hazırlamak üzere aşağıdaki yordamları gerçekleştirin:

* [. Cer dosyasını istemcinin veya uzak ana bilgisayarın kök deposuna aktarın](#to-import-the-certificate-on-the-remote-host).
* [Cihaz seri numaralarını uzak ana bilgisayarınızdaki Hosts dosyasına ekleyin](#to-add-device-serial-numbers-to-the-remote-host).

Yukarıdaki yordamların her biri aşağıda açıklanmıştır.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Uzak konaktaki sertifikayı içeri aktarmak için
1. . Cer dosyasına sağ tıklayın ve **sertifikayı yükler**' i seçin. Bu, sertifika alma Sihirbazı 'nı başlatır.
   
    ![Sertifika Içeri aktarma Sihirbazı 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. **Depo konumu**Için **yerel makine**' yi seçin ve ardından **İleri**' ye tıklayın.
3. **Tüm sertifikaları aşağıdaki depolama alanına yerleştir**' i seçin ve ardından **görüntüle**' ye tıklayın. Uzak konağın kök deposuna gidin ve ardından **İleri**' ye tıklayın.
   
    ![Sertifika Içeri aktarma Sihirbazı 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. **Son**'a tıklayın. İçeri aktarmanın başarılı olduğunu belirten bir ileti görüntülenir.
   
    ![Sertifika Içeri aktarma Sihirbazı 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Uzak konağa cihaz seri numaraları eklemek için
1. Not defteri 'Ni yönetici olarak başlatın ve \Windows\system32\drivers\etckonumunda bulunan Hosts dosyasını açın.
2. Hosts dosyanıza aşağıdaki üç girişi ekleyin: **veri 0 IP adresi**, **DENETLEYICI 0 sabit IP adresi**ve **Denetleyici 1 sabit IP adresi**.
3. Daha önce kaydettiğiniz cihaz seri numarasını girin. Bunu, aşağıdaki görüntüde gösterildiği gibi IP adresiyle eşleyin. Denetleyici 0 ve denetleyici 1 için, seri numarasının (CN adı) sonuna **Controller0** ve **Controller1** ekleyin.
   
    ![Hosts dosyasına CN adı ekleniyor](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Hosts dosyasını kaydedin.

### <a name="connect-to-the-device-from-the-remote-host"></a>Uzak konaktan cihaza Bağlan

Uzak bir ana bilgisayardan veya istemciden cihazınıza bir SSAdmin oturumu girmek için Windows PowerShell ve TLS kullanın. SSAdmin oturumu, cihazınızın [seri konsol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) menüsündeki 1. seçeneğe eşlenir.

Uzak Windows PowerShell bağlantısını yapmak istediğiniz bilgisayarda aşağıdaki yordamı gerçekleştirin.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-tls"></a>Windows PowerShell ve TLS kullanarak cihaza bir SSAdmin oturumu girmek için
1. Yönetici olarak bir Windows PowerShell oturumu başlatın. Bir Windows 10 istemcisi kullanıyorsanız, varsayılan olarak Windows Uzaktan Yönetimi hizmeti el ile olarak ayarlanır. Hizmeti şunu yazarak başlatmanız gerekebilir:

    `Start-Service WinRM`

2. Aşağıdakileri yazarak cihaz IP adresini istemcinin güvenilen konaklarına ekleyin:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    <*device_ip*> cihazınızın IP adresidir; Örneğin: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Yeni bir kimlik bilgisi oluşturmak için şunu yazın:
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    *Hedef cihazın <IP 'si*> CIHAZıNıZ için veri 0 ' ın IP adresidir; Örneğin, ana bilgisayar dosyasının önceki görüntüsünde gösterildiği gibi **10.126.173.90** . Ayrıca, cihazınızın yönetici parolasını sağlayın.
4. Şunu yazarak bir oturum oluşturun:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    Cmdlet 'teki-ComputerName parametresi için, *hedef cihaz> <seri numarasını* girin. Bu seri numarası, uzak ana bilgisayarınızdaki Hosts dosyasındaki 0 VERI IP adresine eşlendi; Örneğin, aşağıdaki görüntüde gösterildiği gibi **SHX0991003G44MT** .
5. Şunu yazın:
   
     `Enter-PSSession $session`
6. Birkaç dakika beklemeniz gerekir ve ardından, TLS üzerinden HTTPS aracılığıyla cihazınıza bağlanırsınız. Cihazınıza bağlı olduğunu belirten bir ileti görürsünüz.
   
    ![HTTPS ve TLS kullanarak PowerShell uzaktan iletişimi](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Sonraki adımlar

* [StorSimple cihazınızı yönetmek Için Windows PowerShell kullanma](storsimple-8000-windows-powershell-administration.md)hakkında daha fazla bilgi edinin.
* StorSimple [cihazınızı yönetmek Için storsimple Aygıt Yöneticisi hizmetini kullanma](storsimple-8000-manager-service-administration.md)hakkında daha fazla bilgi edinin.

