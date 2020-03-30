---
title: 'P2S VPN istemci yapılandırma dosyalarını yüklemek & oluşturun: sertifika kimlik doğrulaması'
titleSuffix: Azure VPN Gateway
description: P2S sertifika kimlik doğrulaması için Windows, Linux, Linux (strongSwan) ve Mac OS X VPN istemci yapılandırma dosyaları oluşturun ve yükleyin.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 03/04/2020
ms.author: cherylmc
ms.openlocfilehash: d15efee635e131d658cd650b7f80eb9e670a0dea
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79279422"
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Yerel Azure sertifikası kimlik doğrulama Sıtkı yapılandırmaları için VPN istemci yapılandırma dosyaları oluşturma ve yükleme

VPN istemci yapılandırma dosyaları bir zip dosyasında bulunur. Yapılandırma dosyaları, yerel Azure sertifika kimlik doğrulaması kullanan Yer-To-Site bağlantıları üzerinden sanal ağa bağlanmak için yerel bir Windows, Mac IKEv2 VPN veya Linux istemcileri için gereken ayarları sağlar.

İstemci yapılandırma dosyaları sanal ağ için VPN yapılandırmasına özgür. VPN protokol türü veya kimlik doğrulama türü gibi VPN istemci yapılandırma dosyalarını oluşturduktan sonra Site'ye Nokta VPN yapılandırmayapılandırmada herhangi bir değişiklik olursa, kullanıcı aygıtlarınız için yeni VPN istemci yapılandırma dosyaları oluşturduğunuzdan emin olun. 

* Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bkz. [Noktadan Siteye VPN hakkında](point-to-site-about.md).
* OpenVPN yönergeleri [için P2S için OpenVPN'i Yapılandır](vpn-gateway-howto-openvpn.md) ve [OpenVPN istemcilerini yapılandırın.](vpn-gateway-howto-openvpn-clients.md)

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="generate-vpn-client-configuration-files"></a><a name="generate"></a>VPN istemci yapılandırma dosyaları oluşturma

Başlamadan önce, tüm bağlantı kullanıcılarının kullanıcının aygıtında yüklü geçerli bir sertifikaya sahip olduğundan emin olun. İstemci sertifikası yükleme hakkında daha fazla bilgi [için](point-to-site-how-to-vpn-client-install-azure-cert.md)bkz.

PowerShell'i kullanarak veya Azure portalını kullanarak istemci yapılandırma dosyaları oluşturabilirsiniz. Her iki yöntem de aynı zip dosyasını döndürür. Aşağıdaki klasörleri görüntülemek için dosyanın zip'ini açın:

  * **Windows Amd64** ve **WindowsX86,** sırasıyla Windows 32-bit ve 64-bit yükleyici paketleri içerir. **WindowsAmd64** yükleyici paketi sadece Amd için değil, tüm desteklenen 64 bit Windows istemcileri içindir.
  * **Kendi**VPN istemci yapılandırmanızı oluşturmak için kullanılan genel bilgileri içeren genel bilgiler. IKEv2 veya SSTP+IKEv2 ağ geçidinde yapılandırıldıysa Genel klasör sağlanır. Yalnızca SSTP yapılandırılırsa, Genel klasör bulunmaz.

### <a name="generate-files-using-the-azure-portal"></a><a name="zipportal"></a>Azure portalını kullanarak dosya oluşturma

1. Azure portalında, bağlanmak istediğiniz sanal ağ için sanal ağ ağ geçidine gidin.
2. Sanal ağ ağ geçidi sayfasında, **Siteye Giriş yapılandırmasını**tıklatın.

   ![istemci portalı indir](./media/point-to-site-vpn-client-configuration-azure-cert/client-configuration-portal.png)
3. Siteye Nokta yapılandırma sayfasının üst kısmında **VPN istemcisini indir'i**tıklatın. İstemci yapılandırma paketinin oluşturulması birkaç dakika sürer.
4. Tarayıcınız, istemci yapılandırma zip dosyasının kullanılabilir olduğunu gösterir. Ağ geçidinizle aynı adverilir. Klasörleri görüntülemek için dosyanın fermuarını açın.

### <a name="generate-files-using-powershell"></a><a name="zipps"></a>PowerShell kullanarak dosya oluşturma


1. VPN istemci yapılandırma dosyaları oluştururken, '-AuthenticationMethod' için değer 'EapTls'. Vpn istemcisi yapılandırma dosyalarını aşağıdaki komutu kullanarak oluşturun:

   ```azurepowershell-interactive
   $profile=New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

   $profile.VPNProfileSASUrl
   ```
2. Zip dosyasını indirmek için URL'yi tarayıcınıza kopyalayın ve ardından klasörleri görüntülemek için dosyanın zip'ini açın.

## <a name="windows"></a><a name="installwin"></a>Windows

Sürüm istemcinin mimarisiyle eşleşip eşleşiyorsa, her Windows istemci bilgisayarında aynı VPN istemci yapılandırma paketini kullanabilirsiniz. Desteklenen istemci işletim sistemleri listesi için [VPN Ağ Geçidi SSS'sinin](vpn-gateway-vpn-faq.md#P2S)Site'den Siteye Noktası bölümüne bakın.

>[!NOTE]
>Bağlanmak istediğiniz Windows istemci bilgisayarında Yönetici haklarına sahip olmalısınız.
>
>

Sertifika kimlik doğrulaması için yerel Windows VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın:

1. Windows bilgisayarın mimarisine karşılık gelen VPN istemcisi yapılandırma dosyalarını seçin. 64 bit işlemci mimarisi için 'VpnClientSetupAmd64' yükleyici paketini seçin. 32 bit işlemci mimarisi için 'VpnClientSetupX86' yükleyici paketini seçin. 
2. Yüklemek için pakete çift tıklayın. Bir SmartScreen açılır penceresi görürseniz **Daha fazla bilgi**’ye ve ardından **Yine de çalıştır**’a tıklayın.
3. İstemci bilgisayarında **Ağ Ayarları'na** gidin ve **VPN'i**tıklatın. VPN bağlantısı, bağlandığı sanal ağın adını gösterir. 
4. Bağlanmayı denemeden önce, istemci bilgisayara bir istemci sertifikası yüklediğinizi doğrulayın. Yerel Azure sertifika kimlik doğrulaması türü kullanılırken kimlik doğrulaması için bir istemci sertifikası gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için [bkz.](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert) İstemci sertifikasının nasıl yüklenirhakkında bilgi [için](point-to-site-how-to-vpn-client-install-azure-cert.md)bkz.

## <a name="mac-os-x"></a><a name="installmac"></a>Mac (OS X)

 Yerel IKEv2 VPN istemcisini Azure’a bağlanacak her Mac cihazda el ile yapılandırmanız gerekir. Azure, yerel Azure sertifika kimlik doğrulaması için mobileconfig dosyası sağlamaz. **Genel** yapılandırma için gereken tüm bilgileri içerir. İndirdiğiniz pakette Genel klasörünü görmüyorsanız, tünel türü olarak IKEv2 seçilmemiş olabilir. VPN ağ geçidi Basic SKU'nun IKEv2'yi desteklemediğini unutmayın. IKEv2 seçildikten sonra zip dosyasını tekrar oluşturarak Genel klasörünü alın.<br>Genel klasörü aşağıdaki dosyaları içerir:

* **VpnSettings.xml**, sunucu adresi ve tünel türü gibi önemli ayarları içerir. 
* P2S bağlantı kurulumu sırasında Azure VPN Ağ Geçidi'ni doğrulamak için gereken kök sertifikasını içeren **VpnServerRoot.cer.**

Sertifika kimlik doğrulaması için Mac'teki yerel VPN istemcisini yapılandırmak için aşağıdaki adımları kullanın. Azure'a bağlanacak her Mac'te şu adımları tamamlamanız gerekir:

1. **VpnServerRoot** kök sertifikasını Mac'inize aktarın. Bu işlem, dosyayı Mac'inize kopyalayarak ve üzerine çift tıklayarak yapılabilir. **İçe aktarmak** için Ekle'yi tıklatın.

   ![sertifika ekleme](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Sertifikaya çift tıkladığınızda **Ekle** iletişim kutusunu görüntülemeyebilir, ancak sertifika doğru mağazaya yüklenir. Sertifikakategorisi altında giriş anahtarlığınızdasertifika yı kontrol edebilirsiniz.
    >
  
2. P2S ayarlarınızı yapılandırdığınızda Azure'a yüklediğiniz kök sertifikası tarafından verilen bir istemci sertifikası yüklediğinizi doğrulayın. Bu, önceki adımda yüklediğiniz VPNServerRoot'dan farklıdır. İstemci sertifikası kimlik doğrulaması için kullanılır ve gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için [bkz.](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert) İstemci sertifikasının nasıl yüklenirhakkında bilgi [için](point-to-site-how-to-vpn-client-install-azure-cert.md)bkz.
3. **Ağ Tercihleri** altında **Ağ** iletişim kutusunu açın ve Azure sanal ağına P2S bağlantısı için yeni bir VPN istemci bağlantısı profili oluşturmak için **'+'** seçeneğini tıklayın.

   **Arayüz** değeri 'VPN' ve **VPN Türü** değeri 'IKEv2'dir. **Hizmet Adı** alanında profil için bir ad belirtin, ardından VPN istemci bağlantı profilini oluşturmak için **Oluştur'u** tıklatın.

   ![network](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
4. **Genel** klasörde, **VpnSettings.xml** dosyasından **VpnServer** etiket değerini kopyalayın. Bu değeri profilin **Sunucu Adresi** ve Uzak **Kimlik** alanlarına yapıştırın.

   ![sunucu bilgileri](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
5. **Kimlik Doğrulama Ayarları'nı** tıklatın ve **Sertifika'yı**seçin.**Catalina**için, **Yok'u** tıklatın ve ardından **sertifika**

   ![kimlik doğrulama ayarları](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)

   * Catalina için **Yok'u** ve ardından **Sertifika'yı**seçin. Doğru sertifikayı **seçin:**
   
   ![Catalina](./media/point-to-site-vpn-client-configuration-azure-cert/catalina.png)

6. **Seç'i tıklatın...** kimlik doğrulaması için kullanmak istediğiniz istemci sertifikasını seçmek için. Bu, Adım 2'de yüklediğiniz sertifikadır.

   ![sertifika](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
7. **Kimlik seçin,** aralarından seçim yapabileceğiniz sertifikaların listesini görüntüler. Uygun sertifikayı seçin ve **ardından Devam et'i**tıklatın.

   ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
8. Yerel **kimlik** alanında, sertifikanın adını (Adım 6'dan) belirtin. Bu örnekte, "ikev2Client.com". Ardından, değişiklikleri kaydetmek için **Uygula** düğmesini tıklatın.

   ![apply](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
9. **Ağ** iletişim kutusunda, tüm değişiklikleri kaydetmek için **Uygula'yı** tıklatın. Ardından, Azure sanal ağına P2S bağlantısını başlatmak için **Bağlan'ı** tıklatın.

## <a name="linux-strongswan-gui"></a><a name="linuxgui"></a>Linux (güçlü Kuğu GUI)

### <a name="install-strongswan"></a><a name="installstrongswan"></a>strongSwan'ı yükleyin

[!INCLUDE [install strongSwan](../../includes/vpn-gateway-strongswan-install-include.md)]

### <a name="generate-certificates"></a><a name="genlinuxcerts"></a>İstemci sertifikaları oluşturma

Sertifikaları zaten oluşturmuyorsanız, aşağıdaki adımları kullanın:

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

### <a name="install-and-configure"></a><a name="install"></a>Yükleme ve yapılandırma

Aşağıdaki talimatlar Ubuntu 18.0.4 üzerinde oluşturulmuştur. Ubuntu 16.0.10 güçlü Swan GUI desteklemiyor. Ubuntu 16.0.10 kullanmak istiyorsanız, [komut satırını](#linuxinstallcli)kullanmanız gerekir. Aşağıdaki örnekler, Linux ve strongSwan sürümünüze bağlı olarak gördüğünüz ekranla eşleşmeyebilir.

1. Örnekteki komutu çalıştırarak **strongSwan** ve Network Manager'ı yüklemek için **Terminal'i** açın.

   ```
   sudo apt install network-manager-strongswan
   ```
2. **Ayarlar'ı**seçin, ardından **Ağ'ı**seçin.

   ![bağlantıları edin](./media/point-to-site-vpn-client-configuration-azure-cert/editconnections.png)
3. Yeni **+** bir bağlantı oluşturmak için düğmeyi tıklatın.

   ![bağlantı ekleme](./media/point-to-site-vpn-client-configuration-azure-cert/addconnection.png)
4. Menüden **IPsec/IKEv2 (strongSwan) seçeneğini** belirleyin ve çift tıklatın. Bu adımda bağlantınızı adlandırabilirsiniz.

   ![bir bağlantı türü seçin](./media/point-to-site-vpn-client-configuration-azure-cert/choosetype.png)
5. İndirilen istemci yapılandırma dosyalarında bulunan **Genel** klasörden **VpnSettings.xml** dosyasını açın. **VpnServer** adlı etiketi bulun ve 'azuregateway' ile başlayıp '.cloudapp.net' ile biten adı kopyalayın.

   ![kopya adı](./media/point-to-site-vpn-client-configuration-azure-cert/vpnserver.png)
6. Bu adı **Ağ Geçidi** bölümündeki yeni VPN bağlantınızın **Adres** alanına yapıştırın. Ardından, **Sertifika** alanının sonundaki klasör simgesini seçin, **Genel** klasöre göz atın ve **VpnServerRoot** dosyasını seçin.
7. Bağlantının **İstemci** bölümünde, **Kimlik Doğrulama**için **Sertifika/özel anahtarı**nı seçin. **Sertifika** ve **Özel anahtarı**için, sertifikayı ve daha önce oluşturulan özel anahtarı seçin. Seçenekler'de, **iç IP adresi isteyin'i**seçin. **Options** Ardından **Ekle'yi**tıklatın.

   ![iç bir IP adresi isteyin](./media/point-to-site-vpn-client-configuration-azure-cert/turnon.png)
8. **Bağlantıyı**aç.

## <a name="linux-strongswan-cli"></a><a name="linuxinstallcli"></a>Linux (güçlü Kuğu CLI)

### <a name="install-strongswan"></a>strongSwan'ı yükleyin

[!INCLUDE [install strongSwan](../../includes/vpn-gateway-strongswan-install-include.md)]

### <a name="generate-certificates"></a>İstemci sertifikaları oluşturma

Sertifikaları zaten oluşturmuyorsanız, aşağıdaki adımları kullanın:

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

### <a name="install-and-configure"></a>Yükleme ve yapılandırma

1. AZURE portalından VPNClient paketini indirin.
2. Dosyayı ayıklayın.
3. **Genel** klasörden VpnServerRoot.cer'i kopyalayın veya /etc/ipsec.d/cacerts adresine taşıyın.
4. Cp client.p12'yi /etc/ipsec.d/private/'a kopyalayın veya taşıyın. Bu dosya Azure VPN Ağ Geçidi için istemci sertifikasıdır.
5. VpnSettings.xml dosyasını açın `<VpnServer>` ve değeri kopyalayın. Bu değeri bir sonraki adımda kullanırsınız.
6. Aşağıdaki örnekteki değerleri ayarlayın ve /etc/ipsec.conf yapılandırmasına örneği ekleyin.
  
   ```
   conn azure
         keyexchange=ikev2
         type=tunnel
         leftfirewall=yes
         left=%any
         leftauth=eap-tls
         leftid=%client # use the DNS alternative name prefixed with the %
         right= Enter the VPN Server value here# Azure VPN gateway address
         rightid=% # Enter the VPN Server value here# Azure VPN gateway FQDN with %
         rightsubnet=0.0.0.0/0
         leftsourceip=%config
         auto=add
   ```
6. */etc/ipsec.secrets*için aşağıdaki leri ekleyin.

   ```
   : P12 client.p12 'password' # key filename inside /etc/ipsec.d/private directory
   ```

7. Aşağıdaki komutları çalıştırın:

   ```
   # ipsec restart
   # ipsec up azure
   ```

## <a name="next-steps"></a>Sonraki adımlar

[P2S yapılandırmanızı tamamlamak](vpn-gateway-howto-point-to-site-rm-ps.md)için makaleye geri dönün.

P2S bağlantılarını gidermek için aşağıdaki makalelere bakın:

  * [Azure noktadan siteye bağlantılarını sorun giderme](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
  * [Mac OS X VPN istemcilerinden VPN bağlantılarını giderme](vpn-gateway-troubleshoot-point-to-site-osx-ikev2.md)
