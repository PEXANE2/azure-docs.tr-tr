---
title: 'Azure VPN Gateway: VPN istemcisi yapılandırma dosyalarını oluşturma & yüklemesi-P2S RADIUS bağlantıları'
description: RADIUS kimlik doğrulaması kullanan bağlantılar için Windows, Mac OS X ve Linux VPN istemci yapılandırma dosyaları oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 01/10/2020
ms.author: cherylmc
ms.openlocfilehash: 5f16a902980b8cf88fb3e8a7f888a0f58ed34355
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84986552"
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>P2S RADIUS kimlik doğrulaması için VPN istemcisi yapılandırma dosyalarını oluşturma ve yüklemeyi

Noktadan siteye (P2S) üzerinden bir sanal ağa bağlanmak için, bağlandığınız istemci cihazını yapılandırmanız gerekir. Windows, Mac OS X ve Linux istemci cihazlarından P2S VPN bağlantıları oluşturabilirsiniz. 

RADIUS kimlik doğrulamasını kullanırken, birden çok kimlik doğrulama seçeneği vardır: Kullanıcı adı/parola kimlik doğrulaması, sertifika kimlik doğrulaması ve diğer kimlik doğrulama türleri. VPN istemci yapılandırması her bir kimlik doğrulama türü için farklıdır. VPN istemcisini yapılandırmak için gerekli ayarları içeren istemci yapılandırma dosyalarını kullanırsınız. Bu makale, kullanmak istediğiniz RADIUS kimlik doğrulama türü için VPN istemci yapılandırmasını oluşturmanıza ve yüklemenize yardımcı olur.

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

P2S RADIUS kimlik doğrulaması için yapılandırma iş akışı aşağıdaki gibidir:

1. [P2S bağlantısı Için Azure VPN ağ geçidini ayarlayın](point-to-site-how-to-radius-ps.md).
2. [RADIUS sunucunuzu kimlik doğrulaması Için ayarlayın](point-to-site-how-to-radius-ps.md#radius). 
3. **Tercih ettiğiniz kimlik doğrulama seçeneği IÇIN VPN istemcisi yapılandırmasını alın ve VPN istemcisini** (Bu makale) ayarlamak için kullanın.
4. [P2S yapılandırmanızı tamamlayıp bağlanın](point-to-site-how-to-radius-ps.md).

>[!IMPORTANT]
>VPN protokolü türü veya kimlik doğrulama türü gibi VPN istemcisi yapılandırma profilini oluşturduktan sonra Noktadan siteye VPN yapılandırmasında herhangi bir değişiklik varsa, kullanıcılarınızın cihazlarında yeni bir VPN istemci yapılandırması oluşturmanız ve kurmanız gerekir.
>
>

Bu makaledeki bölümleri kullanmak için öncelikle hangi kimlik doğrulaması türünü kullanmak istediğinize karar verin: Kullanıcı adı/parola, sertifika veya diğer kimlik doğrulama türleri. Her bölümde Windows, Mac OS X ve Linux için adımlar bulunur (Şu anda sınırlı adımlar mevcuttur).


## <a name="usernamepassword-authentication"></a><a name="adeap"></a>Kullanıcı adı/parola kimlik doğrulaması

Kullanıcı adı/parola kimlik doğrulamasını, Active Directory kullanmak veya Active Directory kullanmak için yapılandırabilirsiniz. Her iki senaryoda, tüm bağlanan kullanıcıların, RADIUS aracılığıyla kimlik doğrulaması yapabilirler Kullanıcı adı/parola kimlik bilgilerine sahip olduğundan emin olun.

Kullanıcı adı/parola kimlik doğrulamasını yapılandırırken yalnızca EAP-MSCHAPv2 Kullanıcı adı/parola kimlik doğrulama protokolü için bir yapılandırma oluşturabilirsiniz. Komutlarda, `-AuthenticationMethod` `EapMSChapv2` .

### <a name="1-generate-vpn-client-configuration-files"></a><a name="usernamefiles"></a>1. VPN istemcisi yapılandırma dosyalarını oluştur

Azure portal kullanarak veya Azure PowerShell kullanarak VPN istemci yapılandırma dosyalarını oluşturabilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Sanal ağ geçidine gidin.
2. **Noktadan siteye yapılandırma**' ya tıklayın.
3. **VPN Istemcisini indir**' e tıklayın.
4. İstemcisini seçin ve istenen tüm bilgileri doldurun.
5. . Zip dosyasını oluşturmak için **İndir** ' e tıklayın.
6. . Zip dosyası, genellikle Indirmeler klasörünüze indirilir.

#### <a name="azure-powershell"></a>Azure PowerShell

Kullanıcı adı/parola kimlik doğrulamasıyla kullanılmak üzere VPN istemci yapılandırma dosyaları oluşturun. Aşağıdaki komutu kullanarak VPN istemci yapılandırma dosyalarını oluşturabilirsiniz:

```azurepowershell-interactive
New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Komut çalıştırıldığında bir bağlantı döndürülür. **VpnClientConfiguration.zip**indirmek için bağlantıyı kopyalayıp bir Web tarayıcısına yapıştırın. Aşağıdaki klasörleri görüntülemek için dosyayı sıkıştırmayı açın: 
 
* **WindowsAmd64** ve **WindowsX86**: bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyici paketlerini içerir. 
* **Genel**: Bu klasör kendi VPN istemci yapılandırmanızı oluşturmak için kullandığınız genel bilgileri içerir. Kullanıcı adı/parola kimlik doğrulama yapılandırması için bu klasöre ihtiyacınız yoktur.
* **Mac**: sanal ağ geçidini oluştururken Ikev2 yapılandırdıysanız, **mobileconfig** dosyası içeren **Mac** adlı bir klasör görürsünüz. Mac istemcilerini yapılandırmak için bu dosyayı kullanın.

İstemci yapılandırma dosyalarını zaten oluşturduysanız, cmdlet 'ini kullanarak bunları alabilirsiniz `Get-AzVpnClientConfiguration` . Ancak P2S VPN yapılandırmanızda, VPN protokol türü veya kimlik doğrulama türü gibi herhangi bir değişiklik yaparsanız yapılandırma otomatik olarak güncellenmez.  `New-AzVpnClientConfiguration`Yeni bir yapılandırma indirmesi oluşturmak için cmdlet 'ini çalıştırmanız gerekir.

Önceden oluşturulan istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="2-configure-vpn-clients"></a><a name="setupusername"></a>2. VPN istemcilerini yapılandırma

Aşağıdaki VPN istemcilerini yapılandırabilirsiniz:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [StrongSwan kullanan Linux](#adlinuxcli)
 
#### <a name="windows-vpn-client-setup"></a><a name="adwincli"></a>Windows VPN istemcisi kurulumu

Sürüm istemcinin mimarisiyle eşleştiği sürece, her bir Windows istemci bilgisayarında aynı VPN istemcisi yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemlerinin listesi için bkz. [SSS](vpn-gateway-vpn-faq.md#P2S).

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bitlik bir işlemci mimarisi için **VpnClientSetupAmd64** yükleyici paketini seçin. 32 bitlik bir işlemci mimarisi için **VpnClientSetupX86** yükleyici paketini seçin. 
2. Paketi yüklemek için çift tıklayın. Bir SmartScreen açılır penceresi görürseniz **daha fazla bilgi**  >  **Çalıştır**' ı seçin.
3. İstemci bilgisayarda, **ağ ayarları** ' na gidin ve **VPN**' yi seçin. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 

#### <a name="mac-os-x-vpn-client-setup"></a><a name="admaccli"></a>Mac (OS X) VPN istemcisi kurulumu

1. **Vpnclientsetup mobileconfig** dosyasını seçin ve kullanıcıların her birine gönderin. E-posta veya başka bir yöntemi kullanabilirsiniz.

2. Mac üzerinde **mobileconfig** dosyasını bulun.

   ![Mobileconfig dosyasının konumu](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)

3. İsteğe bağlı adım-özel bir DNS belirtmek istiyorsanız, **mobileconfig** dosyasına aşağıdaki satırları ekleyin:

   ```xml
    <key>DNS</key>
    <dict>
      <key>ServerAddresses</key>
        <array>
            <string>10.0.0.132</string>
        </array>
      <key>SupplementalMatchDomains</key>
        <array>
            <string>TestDomain.com</string>
        </array>
    </dict> 
   ```
4. Yüklemek için profile çift tıklayın ve **devam**' ı seçin. Profil adı, sanal ağınızın adıyla aynıdır.

   ![Yükleme iletisi](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
5. Profil göndericisine güvenmek için **devam** ' ı seçin ve yüklemeye devam edin.

   ![Onay iletisi](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
6. Profil yüklemesi sırasında VPN kimlik doğrulaması için Kullanıcı adını ve parolayı belirtme seçeneğiniz vardır. Bu bilgilerin girilmesi zorunlu değildir. Bunu yaparsanız, bilgiler kaydedilir ve bir bağlantı başlattığınızda otomatik olarak kullanılır.Devam etmek için **yüklemeyi** seçin.

   ![VPN için Kullanıcı adı ve parola kutuları](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
7. Profili bilgisayarınıza yüklemek için gerekli olan ayrıcalıklar için bir Kullanıcı adı ve parola girin. **Tamam**’ı seçin.

   ![Profil yüklemesi için Kullanıcı adı ve parola kutuları](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
8. Profil yüklendikten sonra, **profiller** iletişim kutusunda görünür. Bu iletişim kutusunu, daha sonra **Sistem Tercihlerinden**de açabilirsiniz.

   !["Profiller" iletişim kutusu](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
9. VPN bağlantısına erişmek için, **Sistem Tercihleri**' nden **ağ** iletişim kutusunu açın.

   ![Sistem tercihlerindeki simgeler](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
10. VPN bağlantısı **ıKEV2 VPN**olarak görünür. **Mobileconfig** dosyasını güncelleştirerek adı değiştirebilirsiniz.

    ![VPN bağlantısının ayrıntıları](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
11. **Kimlik doğrulama ayarları**' nı seçin. Listeden **Kullanıcı adı** ' nı seçin ve kimlik bilgilerinizi girin. Daha önce kimlik bilgilerini girdiyseniz, **Kullanıcı adı** otomatik olarak listede seçilir ve Kullanıcı adı ve parola önceden doldurulur. Ayarları kaydetmek için **Tamam ' ı** seçin.

    ![Kimlik doğrulaması ayarları](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
12. **Ağ** iletişim kutusuna geri döndüğünüzde, değişiklikleri kaydetmek için **Uygula** ' yı seçin. Bağlantıyı başlatmak için **Bağlan**' ı seçin.

#### <a name="linux-vpn-client-setup-through-strongswan"></a><a name="adlinuxcli"></a>StrongSwan aracılığıyla Linux VPN istemcisi kurulumu

Ubuntu 17.0.4 üzerinde Strongswa 5.5.1 aracılığıyla aşağıdaki yönergeler oluşturulmuştur. Linux sürümünüze ve strongSwan sürümüne bağlı olarak gerçek ekranlar farklı olabilir.

1. Örnekteki komutu çalıştırarak **Strongswa** ve Ağ yöneticisini yüklemek için **terminali** açın. İle ilgili bir hata alırsanız `libcharon-extra-plugins` , ile değiştirin `strongswan-plugin-eap-mschapv2` .

   ```Terminal
   sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
   ```
2. **Ağ Yöneticisi** simgesini (yukarı ok/aşağı ok) seçin ve **Bağlantıları Düzenle**' yi seçin.

   ![Ağ Yöneticisi 'nde "bağlantıları düzenleme" seçimi](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Yeni bir bağlantı oluşturmak için **Ekle** düğmesini seçin.

   ![Bağlantı için "Ekle" düğmesi](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Açılan menüden **IPSec/Ikev2 (strongswa)** öğesini seçin ve ardından **Oluştur**' u seçin. Bu adımda, bağlantınızı yeniden adlandırabilirsiniz.

   ![Bağlantı türünü seçme](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. İndirilen istemci yapılandırma dosyalarının **genel** klasöründen **VpnSettings.xml** dosyasını açın. Adlı etiketi bulun ve ile başlayıp şununla `VpnServer` biten adı kopyalayın `azuregateway` `.cloudapp.net` .

   ![VpnSettings.xml dosyasının içeriği](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Bu adı, **ağ geçidi** bölümünde yer alan yeni VPN bağlantınızın **Adres** alanına yapıştırın. Sonra, **sertifika** alanının sonundaki klasör simgesini seçin, **genel** klasöre gidin ve **vpnserverroot** dosyasını seçin.
7. Bağlantının **istemci** bölümünde, **kimlik doğrulama**için **EAP** ' yi seçin ve Kullanıcı adınızı ve parolanızı girin. Bu bilgileri kaydetmek için sağ taraftaki kilit simgesini seçmeniz gerekebilir. Sonra **Kaydet**' i seçin.

   ![Bağlantı ayarlarını Düzenle](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. **Ağ Yöneticisi** simgesini (yukarı ok/aşağı ok) seçin ve **VPN bağlantıları**üzerine gelin. Oluşturduğunuz VPN bağlantısını görürsünüz. Bağlantıyı başlatmak için seçin.

   ![Ağ yöneticisinde "VPN Radius" bağlantısı](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certificate-authentication"></a><a name="certeap"></a>Sertifika kimlik doğrulaması
 
EAP-TLS protokolünü kullanan RADIUS sertifikası kimlik doğrulaması için VPN istemci yapılandırma dosyaları oluşturabilirsiniz. Genellikle, bir kuruluşun verdiği sertifika, VPN için bir kullanıcının kimliğini doğrulamak için kullanılır. Tüm bağlanan kullanıcıların cihazlarında yüklü bir sertifika olduğundan ve RADIUS sunucunuzun sertifikayı doğrulayabileceği emin olun.

>[!NOTE]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

Komutlarda, `-AuthenticationMethod` `EapTls` . Sertifika kimlik doğrulaması sırasında istemci, sertifikasını doğrulayarak RADIUS sunucusunu doğrular. `-RadiusRootCert`, RADIUS sunucusunu doğrulamak için kullanılan kök sertifikayı içeren. cer dosyasıdır.

Her VPN istemci cihazı için yüklü bir istemci sertifikası gerekir. Bazen bir Windows cihazında birden çok istemci sertifikası vardır. Kimlik doğrulama sırasında, bu, tüm sertifikaları listeleyen bir açılır iletişim kutusu ile sonuçlanabilir. Kullanıcının kullanılacak sertifikayı seçmesi gerekir. Doğru sertifika, istemci sertifikasının zincirine gereken kök sertifika belirtilerek filtrelenebilir. 

`-ClientRootCert`, kök sertifikayı içeren. cer dosyasıdır. Bu, isteğe bağlı bir parametredir. Bağlanmak istediğiniz cihazın yalnızca bir istemci sertifikası varsa, bu parametreyi belirtmeniz gerekmez.

### <a name="1-generate-vpn-client-configuration-files"></a><a name="certfiles"></a>1. VPN istemcisi yapılandırma dosyalarını oluştur

Sertifika kimlik doğrulamasıyla kullanılmak üzere VPN istemci yapılandırma dosyaları oluşturun. Aşağıdaki komutu kullanarak VPN istemci yapılandırma dosyalarını oluşturabilirsiniz:
 
```azurepowershell-interactive
New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Komut çalıştırıldığında bir bağlantı döndürülür. VpnClientConfiguration.zip indirmek için bağlantıyı kopyalayıp bir Web tarayıcısına yapıştırın. Aşağıdaki klasörleri görüntülemek için dosyayı sıkıştırmayı açın:

* **WindowsAmd64** ve **WindowsX86**: bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyici paketlerini içerir. 
* **Genericdevice**: Bu klasör kendi VPN istemci yapılandırmanızı oluşturmak için kullanılan genel bilgileri içerir.

İstemci yapılandırma dosyalarını zaten oluşturduysanız, cmdlet 'ini kullanarak bunları alabilirsiniz `Get-AzVpnClientConfiguration` . Ancak P2S VPN yapılandırmanızda, VPN protokol türü veya kimlik doğrulama türü gibi herhangi bir değişiklik yaparsanız yapılandırma otomatik olarak güncellenmez.  `New-AzVpnClientConfiguration`Yeni bir yapılandırma indirmesi oluşturmak için cmdlet 'ini çalıştırmanız gerekir.

Önceden oluşturulan istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="2-configure-vpn-clients"></a><a name="setupusername"></a>2. VPN istemcilerini yapılandırma

Aşağıdaki VPN istemcilerini yapılandırabilirsiniz:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (desteklenen, henüz makale adımı yok)

#### <a name="windows-vpn-client-setup"></a><a name="certwincli"></a>Windows VPN istemcisi kurulumu

1. Bir yapılandırma paketi seçin ve istemci cihazına yükler. 64 bitlik bir işlemci mimarisi için **VpnClientSetupAmd64** yükleyici paketini seçin. 32 bitlik bir işlemci mimarisi için **VpnClientSetupX86** yükleyici paketini seçin. Bir SmartScreen açılır penceresi görürseniz **daha fazla bilgi**  >  **Çalıştır**' ı seçin. Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.
2. Her istemci kimlik doğrulaması için bir istemci sertifikası gerektirir. İstemci sertifikasını yükler. İstemci sertifikaları hakkında daha fazla bilgi için bkz. [Noktadan siteye istemci sertifikaları](vpn-gateway-certificates-point-to-site.md). Oluşturulan bir sertifikayı yüklemek için bkz. [Windows istemcilerine sertifika yüklemek](point-to-site-how-to-vpn-client-install-azure-cert.md).
3. İstemci bilgisayarda, **ağ ayarları** ' na gidin ve **VPN**' yi seçin. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.

#### <a name="mac-os-x-vpn-client-setup"></a><a name="certmaccli"></a>Mac (OS X) VPN istemcisi kurulumu

Azure sanal ağına bağlanan her Mac cihaz için ayrı bir profil oluşturmanız gerekir. Bunun nedeni, bu cihazların profilde kimlik doğrulaması için Kullanıcı sertifikasının belirtilmesini gerektiridir. **Genel** klasör, bir profil oluşturmak için gereken tüm bilgileri içerir:

* **VpnSettings.xml** , sunucu adresi ve tünel türü gibi önemli ayarları içerir.
* **Vpnserverroot. cer** , P2S bağlantı kurulumu sırasında VPN Gateway 'i doğrulamak için gereken kök sertifikayı içerir.
* **Radiusserverroot. cer** , kimlik doğrulaması sırasında RADIUS sunucusunu doğrulamak için gereken kök sertifikayı içerir.

Sertifika kimlik doğrulaması için bir Mac üzerinde yerel VPN istemcisini yapılandırmak üzere aşağıdaki adımları kullanın:

1. **Vpnserverroot** ve **radiusserverroot** kök sertifikalarını Mac 'e aktarın. Her dosyayı Mac 'e kopyalayın, çift tıklayın ve ardından **Ekle**' yi seçin.

   ![VpnServerRoot sertifikası ekleniyor](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

   ![RadiusServerRoot sertifikası ekleme](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Her istemci kimlik doğrulaması için bir istemci sertifikası gerektirir. İstemci sertifikasını istemci cihazına yükler.
3. **Ağ tercihleri**altında **ağ** iletişim kutusunu açın. **+** Azure sanal AĞıNA P2S bağlantısı için yeni BIR VPN istemci bağlantı profili oluşturmayı seçin.

   **Arabirim** değeri **VPN**'dir ve **VPN türü** değeri **Ikev2**' dir. **Hizmet adı** kutusunda profil için bir ad belirtin ve ardından **Oluştur** ' u seçerek VPN istemci bağlantı profilini oluşturun.

   ![Arabirim ve hizmet adı bilgileri](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. **Genel** klasörde, **VpnSettings.xml** dosyasından, **VPNServer** etiket değerini kopyalayın. Bu değeri, profilin **sunucu adresine** ve **uzak kimlik** kutularına yapıştırın. **Yerel kimlik** kutusunu boş bırakın.

   ![Sunucu bilgileri](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. **Kimlik doğrulama ayarları**' nı seçin ve **sertifika**' yı seçin. 

   ![Kimlik doğrulaması ayarları](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Kimlik doğrulaması için kullanmak istediğiniz sertifikayı seçmek için **Seç** ' e tıklayın.

   ![Kimlik doğrulaması için bir sertifika seçme](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Bir kimlik seçin seçim** yapabileceğiniz sertifikaların listesini görüntüler. Doğru sertifikayı seçip **devam**' ı seçin.

   !["Kimlik seçme" listesi](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. **Yerel kimlik** kutusunda, sertifikanın (adım 6 ' dan) adını belirtin. Bu örnekte, **ikev2Client.com**. Ardından, değişiklikleri kaydetmek için **Uygula** düğmesini seçin.

   !["Yerel KIMLIK" kutusu](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. Tüm değişiklikleri kaydetmek için **ağ** Iletişim kutusunda **Uygula** ' yı seçin. Ardından, Azure sanal ağına P2S bağlantısını başlatmak için **Bağlan** ' ı seçin.

## <a name="working-with-other-authentication-types-or-protocols"></a><a name="otherauth"></a>Diğer kimlik doğrulama türleri veya protokolleriyle çalışma

Farklı bir kimlik doğrulama türü (örneğin, OTP) veya farklı bir kimlik doğrulama protokolü (EAP-MSCHAPv2 yerine PEAP-MSCHAPv2) kullanmak için kendi VPN istemci yapılandırma profilinizi oluşturmanız gerekir. Profili oluşturmak için sanal ağ geçidi IP adresi, tünel türü ve bölünmüş tünel yolları gibi bilgilere ihtiyacınız vardır. Aşağıdaki adımları kullanarak bu bilgileri alabilirsiniz:

1. `Get-AzVpnClientConfiguration`EapMSChapv2 IÇIN VPN istemci yapılandırmasını oluşturmak için cmdlet 'ini kullanın.

2. VpnClientConfiguration.zip dosyasını sıkıştırmayı açın ve **Genericdevice** klasörünü arayın. 64-bit ve 32-bit mimarilerinin Windows yükleyicilerini içeren klasörleri yoksayın.
 
3. **Genericdevice** klasörü, **vpnsettings**adlı bir XML dosyası içerir. Bu dosya, gerekli tüm bilgileri içerir:

   * **VPNServer**: Azure VPN ağ geçidinin FQDN 'si. Bu, istemcinin bağlandığı adrestir.
   * **Vpntype**: bağlanmak için kullandığınız tünel türü.
   * **Rotalar**: profilinizde yapılandırmanız gereken yollar, böylece yalnızca Azure sanal ağına bağlanan trafik P2S tüneli üzerinden gönderilir.
   
   **Genericdevice** klasörü, **vpnserverroot**adlı bir. cer dosyası da içerir. Bu dosya, P2S bağlantı kurulumu sırasında Azure VPN Gateway 'i doğrulamak için gereken kök sertifikayı içerir. Sertifikayı Azure sanal ağına bağlanacak tüm cihazlara yükler.

## <a name="next-steps"></a>Sonraki adımlar

[P2S yapılandırmanızı tamamlamaya](point-to-site-how-to-radius-ps.md)yönelik makaleye geri dönün.

P2S sorun giderme bilgileri için bkz. [Azure Noktadan siteye bağlantıları sorunlarını giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md).
