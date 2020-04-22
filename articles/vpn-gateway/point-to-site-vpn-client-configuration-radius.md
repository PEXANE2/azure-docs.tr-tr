---
title: 'Azure VPN Ağ Geçidi: VPN istemci config dosyalarını yüklemek & oluşturun - P2S RADIUS bağlantıları'
description: RADIUS kimlik doğrulaması kullanan bağlantılar için Windows, Mac OS X ve Linux VPN istemci yapılandırma dosyaları oluşturun.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/10/2020
ms.author: cherylmc
ms.openlocfilehash: 69517d69a26364cf1cc950d7aaa849522decacf1
ms.sourcegitcommit: ffc6e4f37233a82fcb14deca0c47f67a7d79ce5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81732748"
---
# <a name="create-and-install-vpn-client-configuration-files-for-p2s-radius-authentication"></a>P2S RADIUS kimlik doğrulaması için VPN istemci yapılandırma dosyaları oluşturma ve yükleme

Bir sanal ağa noktadan siteye (P2S) bağlanmak için, bağlanacağınız istemci aygıtını yapılandırmanız gerekir. Windows, Mac OS X ve Linux istemci aygıtlarından P2S VPN bağlantıları oluşturabilirsiniz. 

RADIUS kimlik doğrulaması kullanırken, birden çok kimlik doğrulama seçeneği vardır: kullanıcı adı/parola kimlik doğrulaması, sertifika kimlik doğrulaması ve diğer kimlik doğrulama türleri. VPN istemcisi yapılandırması her kimlik doğrulama türü için farklıdır. VPN istemcisini yapılandırmak için, gerekli ayarları içeren istemci yapılandırma dosyalarını kullanırsınız. Bu makale, kullanmak istediğiniz RADIUS kimlik doğrulama türü için VPN istemcisi yapılandırmasını oluşturmanıza ve yüklemenize yardımcı olur.

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

P2S RADIUS kimlik doğrulaması için yapılandırma iş akışı aşağıdaki gibidir:

1. [P2S bağlantısı için Azure VPN ağ geçidini ayarlayın.](point-to-site-how-to-radius-ps.md)
2. [Radius sunucunuzu kimlik doğrulaması için ayarlayın.](point-to-site-how-to-radius-ps.md#radius) 
3. **Seçtiğiniz kimlik doğrulama seçeneği için VPN istemcisi yapılandırmasını edinin ve VPN istemcisini** (bu makalede) ayarlamak için kullanın.
4. [P2S yapılandırmanızı tamamlayın ve bağlanın.](point-to-site-how-to-radius-ps.md)

>[!IMPORTANT]
>VPN protokol türü veya kimlik doğrulama türü gibi VPN istemci yapılandırma profilini oluşturduktan sonra siteye nokta VPN yapılandırmada herhangi bir değişiklik olursa, kullanıcılarınızın cihazlarında yeni bir VPN istemcisi yapılandırması oluşturmanız ve yüklemeniz gerekir.
>
>

Bu makaledeki bölümleri kullanmak için öncelikle hangi kimlik doğrulaması türünü kullanmak istediğinize karar verin: kullanıcı adı/parola, sertifika veya diğer kimlik doğrulama türleri. Her bölümde Windows, Mac OS X ve Linux için adımlar vardır (şu anda sınırlı adımlar kullanılabilir).


## <a name="usernamepassword-authentication"></a><a name="adeap"></a>Kullanıcı adı/parola kimlik doğrulaması

Kullanıcı adı/parola kimlik doğrulaması,Etkin Dizin'i kullanacak veya Active Directory'yi kullanmayacak şekilde yapılandırabilirsiniz. Her iki senaryoda da, tüm bağlantı kullanıcılarının RADIUS üzerinden kimlik doğrulaması yapabilen kullanıcı adı/parola kimlik bilgilerine sahip olduğundan emin olun.

Kullanıcı adı/parola kimlik doğrulaması yapılandırdığınızda, yalnızca EAP-MSCHAPv2 kullanıcı adı/parola kimlik doğrulama protokolü için bir yapılandırma oluşturabilirsiniz. Komutlarda, `-AuthenticationMethod` . `EapMSChapv2`

### <a name="1-generate-vpn-client-configuration-files"></a><a name="usernamefiles"></a>1. VPN istemci yapılandırma dosyaları oluşturma

Azure portalını kullanarak veya Azure PowerShell'i kullanarak VPN istemci yapılandırma dosyalarını oluşturabilirsiniz.

#### <a name="azure-portal"></a>Azure portal

1. Sanal ağ ağ geçidine gidin.
2. **Siteye Puan yapılandırmasını**tıklatın.
3. **VPN istemcisini indir'i**tıklatın.
4. İstemciyi seçin ve istenen bilgileri doldurun.
5. .zip dosyasını oluşturmak için **İndir'i** tıklatın.
6. .zip dosyası genellikle İndirilenler klasörünüze indirilir.

#### <a name="azure-powershell"></a>Azure PowerShell

Kullanıcı adı/parola kimlik doğrulaması ile kullanılmak üzere VPN istemci yapılandırma dosyaları oluşturun. Aşağıdaki komutu kullanarak VPN istemci yapılandırma dosyaları oluşturabilirsiniz:

```azurepowershell-interactive
New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapMSChapv2"
```
 
Komutu çalıştırmak bir bağlantı döndürür. **VpnClientConfiguration.zip**indirmek için bir web tarayıcısına bağlantı kopyalayın ve yapıştırın. Aşağıdaki klasörleri görüntülemek için dosyanın zip'ini açın: 
 
* **WindowsAmd64** ve **WindowsX86**: Bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyici paketlerini içerir. 
* **Genel**: Bu klasör, kendi VPN istemci yapılandırmanızı oluşturmak için kullandığınız genel bilgileri içerir. Kullanıcı adı/parola kimlik doğrulama yapılandırmaları için bu klasöre ihtiyacınız yoktur.
* **Mac**: Sanal ağ ağ ağ geçidini oluşturduğunuzda IKEv2'yi yapılandırmışsanız, **Mac** adında bir **mobileconfig** dosyası içeren bir klasör görürsünüz. Mac istemcilerini yapılandırmak için bu dosyayı kullanırsınız.

İstemci yapılandırma dosyalarını zaten oluşturduysanız, `Get-AzVpnClientConfiguration` bunları cmdlet'i kullanarak alabilirsiniz. Ancak VPN protokol türü veya kimlik doğrulama türü gibi P2S VPN yapılandırmanızda herhangi bir değişiklik yaparsanız, yapılandırma otomatik olarak güncelleştirolmaz. Yeni bir `New-AzVpnClientConfiguration` yapılandırma indirme oluşturmak için cmdlet çalıştırmanız gerekir.

Daha önce oluşturulmuş istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW"
```

### <a name="2-configure-vpn-clients"></a><a name="setupusername"></a>2. VPN istemcilerini yapılandırın

Aşağıdaki VPN istemcilerini yapılandırabilirsiniz:

* [Windows](#adwincli)
* [Mac (OS X)](#admaccli)
* [Linux strongSwan kullanarak](#adlinuxcli)
 
#### <a name="windows-vpn-client-setup"></a><a name="adwincli"></a>Windows VPN istemci kurulumu

Sürüm istemcinin mimarisiyle eşleşip eşleşiyorsa, her Windows istemci bilgisayarında aynı VPN istemci yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemleri listesi için [SSS](vpn-gateway-vpn-faq.md#P2S)bölümüne bakın.

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için **VpnClientSetupAmd64** yükleyici paketini seçin. 32 bit işlemci mimarisi için **VpnClientSetupX86** yükleyici paketini seçin. 
2. Paketi yüklemek için çift tıklatın. SmartScreen açılır penceresi görürseniz, **daha fazla bilgi** > **çalıştır'ı seçin.**
3. İstemci bilgisayarında **Ağ Ayarları'na** göz atın ve **VPN'i**seçin. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 

#### <a name="mac-os-x-vpn-client-setup"></a><a name="admaccli"></a>Mac (OS X) VPN istemci kurulumu

1. **VpnClientSetup mobileconfig** dosyasını seçin ve her kullanıcıya gönderin. E-posta veya başka bir yöntem kullanabilirsiniz.

2. Mac'teki **mobileconfig** dosyasını bulun.

   ![Mobileconfig dosyasının konumu](./media/point-to-site-vpn-client-configuration-radius/admobileconfigfile.png)

3. İsteğe Bağlı Adım - Özel bir DNS belirtmek istiyorsanız, **mobileconfig** dosyasına aşağıdaki satırları ekleyin:

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
4. Yüklemek için profili çift tıklatın ve **Devam et'i**seçin. Profil adı, sanal ağınızın adı ile aynıdır.

   ![Yükleme iletisi](./media/point-to-site-vpn-client-configuration-radius/adinstall.png)
5. Profilin gönderenine güvenmeye **devam** et'i seçin ve yüklemeye devam edin.

   ![Onay iletisi](./media/point-to-site-vpn-client-configuration-radius/adcontinue.png)
6. Profil yükleme sırasında VPN kimlik doğrulaması için kullanıcı adını ve parolayı belirtme seçeneğiniz vardır. Bu bilgileri girmek zorunlu değildir. Bunu yaparsanız, bir bağlantı başlattığınızda bilgiler kaydedilir ve otomatik olarak kullanılır.Devam etmek için **Yükle'yi** seçin.

   ![VPN için kullanıcı adı ve şifre kutuları](./media/point-to-site-vpn-client-configuration-radius/adsettings.png)
7. Profili bilgisayarınıza yüklemek için gereken ayrıcalıklar için bir kullanıcı adı ve parola girin. **Tamam'ı**seçin.

   ![Profil kurulumu için kullanıcı adı ve şifre kutuları](./media/point-to-site-vpn-client-configuration-radius/adusername.png)
8. Profil yüklendikten sonra **Profiller** iletişim kutusunda görünür. Ayrıca bu iletişim kutusunu daha sonra **Sistem Tercihleri'nden**de açabilirsiniz.

   !["Profiller" iletişim kutusu](./media/point-to-site-vpn-client-configuration-radius/adsystempref.png)
9. VPN bağlantısına erişmek için **Sistem Tercihleri'nden** **Ağ** iletişim kutusunu açın.

   ![Sistem Tercihlerinde Simgeler](./media/point-to-site-vpn-client-configuration-radius/adnetwork.png)
10. VPN bağlantısı **IkeV2-VPN**olarak görünür. **Mobileconfig** dosyasını güncelleştirerek adı değiştirebilirsiniz.

    ![VPN bağlantısı için ayrıntılar](./media/point-to-site-vpn-client-configuration-radius/adconnection.png)
11. **Kimlik Doğrulama Ayarlarını**seçin. Listede **Kullanıcı Adı'nı** seçin ve kimlik bilgilerinizi girin. Kimlik bilgilerini daha önce girdiyseniz, **kullanıcı adı** otomatik olarak listede seçilir ve kullanıcı adı ve parola önceden doldurulur. Ayarları kaydetmek için **Tamam'ı** seçin.

    ![Kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-radius/adauthentication.png)
12. **Ağ** iletişim kutusunda değişiklikleri kaydetmek için **Uygula'yı** seçin. Bağlantıyı başlatmak için **Bağlan'ı**seçin.

#### <a name="linux-vpn-client-setup-through-strongswan"></a><a name="adlinuxcli"></a>StrongSwan ile Linux VPN istemci kurulumu

Aşağıdaki talimatlar Ubuntu 17.0.4 üzerinde strongSwan 5.5.1 ile oluşturuldu. Gerçek ekranlar, Linux ve strongSwan sürümünüze bağlı olarak farklı olabilir.

1. Örnekteki komutu çalıştırarak **strongSwan** ve Network Manager'ı yüklemek için **Terminal'i** açın. İlgili bir hata `libcharon-extra-plugins`alırsanız, `strongswan-plugin-eap-mschapv2`değiştirin.

   ```Terminal
   sudo apt-get install strongswan libcharon-extra-plugins moreutils iptables-persistent network-manager-strongswan
   ```
2. Ağ **Yöneticisi** simgesini (yukarı ok/aşağı ok) seçin ve **Bağlantıları Edit'i**seçin.

   ![Ağ Yöneticisi'nde "Bağlantıları Edin" seçimi](./media/point-to-site-vpn-client-configuration-radius/EditConnection.png)
3. Yeni bir bağlantı oluşturmak için **Ekle** düğmesini seçin.

   ![Bağlantı için "Ekle" düğmesi](./media/point-to-site-vpn-client-configuration-radius/AddConnection.png)
4. Açılan menüden **IPsec/IKEv2 (strongswan)** seçeneğini seçin ve ardından **Oluştur'u**seçin. Bu adımda bağlantınızı yeniden adlandırabilirsiniz.

   ![Bağlantı türünü seçme](./media/point-to-site-vpn-client-configuration-radius/AddIKEv2.png)
5. İndirilen istemci yapılandırma dosyalarının **Genel** klasöründen **VpnSettings.xml** dosyasını açın. Adı verilen `VpnServer` etiketi bulun ve `azuregateway` `.cloudapp.net`'ile başlayıp ' la biten adı kopyalayın.

   ![VpnSettings.xml dosyasının içeriği](./media/point-to-site-vpn-client-configuration-radius/VpnSettings.png)
6. Bu adı **Ağ Geçidi** bölümündeki yeni VPN bağlantınızın **Adres** alanına yapıştırın. Ardından, **Sertifika** alanının sonundaki klasör simgesini seçin, **Genel** klasöre göz atın ve **VpnServerRoot** dosyasını seçin.
7. Bağlantının **İstemci** bölümünde, Kimlik **Doğrulaması**için **EAP'yi** seçin ve kullanıcı adınızı ve parolanızı girin. Bu bilgileri kaydetmek için sağdaki kilit simgesini seçmeniz gerekebilir. Ardından **Kaydet**’i seçin.

   ![Bağlantı ayarlarını düzenleme](./media/point-to-site-vpn-client-configuration-radius/editconnectionsettings.png)
8. Ağ **Yöneticisi** simgesini (yukarı ok/aşağı ok) seçin ve **VPN Bağlantıları'nın**üzerine tıklayın. Oluşturduğunuz VPN bağlantısını görürsünüz. Bağlantıyı başlatmak için, bağlantıyı seçin.

   ![Network Manager'da "VPN Yarıçapı" bağlantısı](./media/point-to-site-vpn-client-configuration-radius/ConnectRADIUS.png)

## <a name="certificate-authentication"></a><a name="certeap"></a>Sertifika kimlik doğrulaması
 
EAP-TLS protokolünü kullanan RADIUS sertifika kimlik doğrulaması için VPN istemci yapılandırma dosyaları oluşturabilirsiniz. Genellikle, vpn için bir kullanıcının kimliğini doğrulamak için kurumsal tarafından verilen bir sertifika kullanılır. Tüm bağlantı kullanıcılarının cihazlarında bir sertifika yüklü olduğundan ve RADIUS sunucunuzun sertifikayı doğrulayabilmesini sağlayın.

>[!NOTE]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

Komutlarda, `-AuthenticationMethod` . `EapTls` Sertifika kimlik doğrulaması sırasında istemci, sertifikasını doğrulayarak RADIUS sunucusunu doğrular. `-RadiusRootCert`RADIUS sunucusunu doğrulamak için kullanılan kök sertifikasını içeren .cer dosyasıdır.

Her VPN istemci aygıtı yüklü bir istemci sertifikası gerektirir. Bazen bir Windows aygıtının birden çok istemci sertifikası vardır. Kimlik doğrulama sırasında, bu durum tüm sertifikaları listeleyen bir açılır iletişim kutusuyla sonuçlanabilir. Kullanıcı daha sonra kullanılacak sertifikayı seçmelidir. Doğru sertifika, istemci sertifikasının zincirlemesi gereken kök sertifikayı belirterek filtrelenebilir. 

`-ClientRootCert`kök sertifikasını içeren .cer dosyasıdır. İsteğe bağlı bir parametre. Bağlanmak istediğiniz aygıtın yalnızca bir istemci sertifikası varsa, bu parametreyi belirtmeniz gerekir.

### <a name="1-generate-vpn-client-configuration-files"></a><a name="certfiles"></a>1. VPN istemci yapılandırma dosyaları oluşturma

Sertifika kimlik doğrulaması ile kullanılmak üzere VPN istemci yapılandırma dosyaları oluşturun. Aşağıdaki komutu kullanarak VPN istemci yapılandırma dosyaları oluşturabilirsiniz:
 
```azurepowershell-interactive
New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls" -RadiusRootCert <full path name of .cer file containing the RADIUS root> -ClientRootCert <full path name of .cer file containing the client root> | fl
```

Komutu çalıştırmak bir bağlantı döndürür. VpnClientConfiguration.zip'i indirmek için bağlantıyı kopyalayıp bir web tarayıcısına yapıştırın. Aşağıdaki klasörleri görüntülemek için dosyanın zip'ini açın:

* **WindowsAmd64** ve **WindowsX86**: Bu klasörler sırasıyla Windows 64-bit ve 32-bit yükleyici paketlerini içerir. 
* **GenericDevice**: Bu klasör, kendi VPN istemci yapılandırmanızı oluşturmak için kullanılan genel bilgileri içerir.

İstemci yapılandırma dosyalarını zaten oluşturduysanız, `Get-AzVpnClientConfiguration` bunları cmdlet'i kullanarak alabilirsiniz. Ancak VPN protokol türü veya kimlik doğrulama türü gibi P2S VPN yapılandırmanızda herhangi bir değişiklik yaparsanız, yapılandırma otomatik olarak güncelleştirolmaz. Yeni bir `New-AzVpnClientConfiguration` yapılandırma indirme oluşturmak için cmdlet çalıştırmanız gerekir.

Daha önce oluşturulmuş istemci yapılandırma dosyalarını almak için aşağıdaki komutu kullanın:

```azurepowershell-interactive
Get-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" | fl
```
 
### <a name="2-configure-vpn-clients"></a><a name="setupusername"></a>2. VPN istemcilerini yapılandırın

Aşağıdaki VPN istemcilerini yapılandırabilirsiniz:

* [Windows](#certwincli)
* [Mac (OS X)](#certmaccli)
* Linux (desteklenen, henüz hiçbir makale adımları)

#### <a name="windows-vpn-client-setup"></a><a name="certwincli"></a>Windows VPN istemci kurulumu

1. Bir yapılandırma paketi seçin ve istemci aygıtına yükleyin. 64 bit işlemci mimarisi için **VpnClientSetupAmd64** yükleyici paketini seçin. 32 bit işlemci mimarisi için **VpnClientSetupX86** yükleyici paketini seçin. SmartScreen açılır penceresi görürseniz, **daha fazla bilgi** > **çalıştır'ı seçin.** Paketi ayrıca diğer istemci bilgisayarlara yüklemek üzere kaydedebilirsiniz.
2. Her istemci kimlik doğrulaması için bir istemci sertifikası gerektirir. İstemci sertifikasını yükleyin. İstemci sertifikaları hakkında bilgi için, [siteye bakış için İstemci sertifikalarına](vpn-gateway-certificates-point-to-site.md)bakın. Oluşturulan bir sertifikayı yüklemek için [Windows istemcilerine sertifika yükle'ye](point-to-site-how-to-vpn-client-install-azure-cert.md)bakın.
3. İstemci bilgisayarında **Ağ Ayarları'na** göz atın ve **VPN'i**seçin. VPN bağlantısı, bağlandığı sanal ağın adını gösterir.

#### <a name="mac-os-x-vpn-client-setup"></a><a name="certmaccli"></a>Mac (OS X) VPN istemci kurulumu

Azure sanal ağına bağlanan her Mac aygıtı için ayrı bir profil oluşturmanız gerekir. Bunun nedeni, bu aygıtların profilde kimlik doğrulamasının belirtilmesi için kullanıcı sertifikasını gerektirmesidir. **Genel** klasör, profil oluşturmak için gereken tüm bilgilere sahiptir:

* **VpnSettings.xml** sunucu adresi ve tünel türü gibi önemli ayarları içerir.
* **VpnServerRoot.cer** P2S bağlantı kurulumu sırasında VPN ağ geçidi ni doğrulamak için gerekli olan kök sertifikasını içerir.
* **RadiusServerRoot.cer** kimlik doğrulama sırasında RADIUS sunucusunu doğrulamak için gereken kök sertifikasını içerir.

Sertifika kimlik doğrulaması için mac'te yerel VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın:

1. **VpnServerRoot** ve **RadiusServerRoot** kök sertifikalarını Mac'inize aktarın. Her dosyayı Mac'inize kopyalayın, çift tıklatın ve sonra **Ekle'yi**seçin.

   ![VpnServerRoot sertifikası ekleme](./media/point-to-site-vpn-client-configuration-radius/addcert.png)

   ![RadiusServerRoot sertifikası ekleme](./media/point-to-site-vpn-client-configuration-radius/radiusrootcert.png)
2. Her istemci kimlik doğrulaması için bir istemci sertifikası gerektirir. İstemci sertifikasını istemci aygıtına yükleyin.
3. **Ağ Tercihleri**altında **Ağ** iletişim kutusunu açın. Azure **+** sanal ağına Bir P2S bağlantısı için yeni bir VPN istemcisi bağlantı profili oluşturmak için seçin.

   **Arayüz** değeri **VPN'dir**ve **VPN Türü** değeri **IKEv2'dir.** **Hizmet Adı** kutusundaprofil için bir ad belirtin ve ardından VPN istemci bağlantı profilini oluşturmak için **Oluştur'u** seçin.

   ![Arayüz ve hizmet adı bilgileri](./media/point-to-site-vpn-client-configuration-radius/network.png)
4. **Genel** klasörde, **VpnSettings.xml** dosyasından **VpnServer** etiket değerini kopyalayın. Bu değeri profilin **Sunucu Adresi** ve Uzak **Kimlik** kutularına yapıştırın. Yerel **kimlik** kutusunu boş bırakın.

   ![Sunucu bilgileri](./media/point-to-site-vpn-client-configuration-radius/servertag.png)
5. **Kimlik Doğrulama Ayarları'nı**seçin ve **Sertifika'yı**seçin. 

   ![Kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-radius/certoption.png)
6. Kimlik doğrulama için kullanmak istediğiniz sertifikayı seçmek için **Seç'i** tıklatın.

   ![Kimlik doğrulama için sertifika seçme](./media/point-to-site-vpn-client-configuration-radius/certificate.png)
7. **Kimlik seçin,** aralarından seçim yapabileceğiniz sertifikaların listesini görüntüler. Uygun sertifikayı seçin ve sonra **Devam et'i**seçin.

   !["Kimlik Seçin" listesi](./media/point-to-site-vpn-client-configuration-radius/identity.png)
8. Yerel **kimlik** kutusunda, sertifikanın adını (Adım 6'dan) belirtin. Bu örnekte, **ikev2Client.com.** Ardından, değişiklikleri kaydetmek için **Uygula** düğmesini seçin.

   !["Yerel Kimlik" kutusu](./media/point-to-site-vpn-client-configuration-radius/applyconnect.png)
9. **Ağ** iletişim kutusunda, tüm değişiklikleri kaydetmek için **Uygula'yı** seçin. Ardından, Azure sanal ağına P2S bağlantısını başlatmak için **Bağlan'ı** seçin.

## <a name="working-with-other-authentication-types-or-protocols"></a><a name="otherauth"></a>Diğer kimlik doğrulama türleri veya protokollerle çalışma

Farklı bir kimlik doğrulama türü (örneğin, OTP) kullanmak veya farklı bir kimlik doğrulama protokolü (EAP-MSCHAPv2 yerine PEAP-MSCHAPv2 gibi) kullanmak için kendi VPN istemci yapılandırma profilinizi oluşturmanız gerekir. Profili oluşturmak için sanal ağ ağ ağ geçidi IP adresi, tünel türü ve bölünmüş tünel yolları gibi bilgilere ihtiyacınız var. Aşağıdaki adımları kullanarak bu bilgileri alabilirsiniz:

1. EapMSChapv2 için VPN istemcisi yapılandırmasını oluşturmak için `Get-AzVpnClientConfiguration` cmdlet'i kullanın.

2. VpnClientConfiguration.zip dosyasını açın ve **GenericDevice** klasörünü arayın. 64 bit ve 32 bit mimariler için Windows yükleyicileri içeren klasörleri yoksayın.
 
3. **GenericDevice** klasörü **VpnSettings**adlı bir XML dosyası içerir. Bu dosya gerekli tüm bilgileri içerir:

   * **VpnServer**: Azure VPN ağ geçidinin FQDN'si. Bu, istemcinin bağlandığı adrestir.
   * **VpnType**: Bağlanmak için kullandığınız tünel türü.
   * **Rotalar**: Yalnızca Azure sanal ağına bağlı trafiğin P2S tüneli üzerinden gönderilmesi için profilinizde yapılandırmanız gereken rotalar.
   
   **GenericDevice** klasörü de **VpnServerRoot**adlı bir .cer dosyası içerir. Bu dosya, P2S bağlantı kurulumu sırasında Azure VPN ağ geçidini doğrulamak için gereken kök sertifikasını içerir. Sertifikayı Azure sanal ağına bağlanacak tüm aygıtlara yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

[P2S yapılandırmanızı tamamlamak](point-to-site-how-to-radius-ps.md)için makaleye geri dönün.

P2S sorun giderme bilgileri için, [Azure noktadan siteye bağlantılarını giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)konusuna bakın.
