---
title: 'P2S VPN istemci yapılandırma dosyaları oluşturun & install: sertifika kimlik doğrulaması'
titleSuffix: Azure VPN Gateway
description: P2S sertifikası kimlik doğrulaması için Windows, Linux, Linux (Strongswa) ve macOS X VPN istemci yapılandırma dosyaları oluşturun ve bunları yükler.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: how-to
ms.date: 11/11/2020
ms.author: cherylmc
ms.openlocfilehash: c7b186aa1a6f63b1bc3e9dbefa5001faac967762
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "94556217"
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>Yerel Azure sertifikası kimlik doğrulaması P2S yapılandırmaları için VPN istemcisi yapılandırma dosyalarını oluşturma ve yükleme

VPN istemcisi yapılandırma dosyaları bir ZIP dosyasında bulunur. Yapılandırma dosyaları, yerel bir Windows, Mac IKEv2 VPN veya Linux istemcilerinin yerel Azure sertifika kimlik doğrulaması kullanan Noktadan siteye bağlantılar üzerinden bir sanal ağa bağlanması için gereken ayarları sağlar.

İstemci yapılandırma dosyaları, sanal ağ için VPN yapılandırmasına özeldir. VPN protokolü türü veya kimlik doğrulama türü gibi VPN istemcisi yapılandırma dosyalarını oluşturduktan sonra Noktadan siteye VPN yapılandırmasında herhangi bir değişiklik varsa, Kullanıcı cihazlarınız için yeni VPN istemci yapılandırma dosyaları üretdiğinizden emin olun.

* Noktadan Siteye bağlantılar hakkında daha fazla bilgi edinmek için bkz. [Noktadan Siteye VPN hakkında](point-to-site-about.md).
* OpenVPN yönergeleri için bkz. [P2S Için OpenVPN yapılandırma](vpn-gateway-howto-openvpn.md) ve [OpenVPN istemcilerini yapılandırma](vpn-gateway-howto-openvpn-clients.md).

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="generate-vpn-client-configuration-files"></a><a name="generate"></a>VPN istemcisi yapılandırma dosyaları oluştur

Başlamadan önce, tüm bağlanan kullanıcıların, kullanıcının cihazında geçerli bir sertifika yüklü olduğundan emin olun. İstemci sertifikası yükleme hakkında daha fazla bilgi için bkz. [istemci sertifikası yükleme](point-to-site-how-to-vpn-client-install-azure-cert.md).

PowerShell kullanarak veya Azure portal kullanarak istemci yapılandırma dosyaları oluşturabilirsiniz. Her iki yöntem de aynı ZIP dosyasını döndürür. Aşağıdaki klasörleri görüntülemek için dosyayı sıkıştırmayı açın:

* Sırasıyla Windows 32-bit ve 64-bit yükleyici paketlerini içeren **WindowsAmd64** ve **WindowsX86**. **WindowsAmd64** Installer paketi yalnızca AMD değil, desteklenen tüm 64-bit Windows istemcilerine yöneliktir.
* **Genel**, kendi VPN istemci yapılandırmanızı oluşturmak için kullanılan genel bilgileri içerir. Ağ geçidinde Ikev2 veya SSTP + Ikev2 yapılandırılmışsa genel klasör sağlanır. Yalnızca SSTP yapılandırılmışsa, genel klasör mevcut değildir.

### <a name="generate-files-using-the-azure-portal"></a><a name="zipportal"></a>Azure portal kullanarak dosya oluşturma

1. Azure portal, bağlanmak istediğiniz sanal ağ için sanal ağ geçidine gidin.
1. Sanal ağ geçidi sayfasında, **Noktadan siteye yapılandırma**' yı seçin.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/download-client.png" alt-text="VPN istemcisini indirin":::
1. Noktadan siteye yapılandırma sayfasının en üstünde **VPN Istemcisini indir**' i seçin. İstemci yapılandırma paketinin oluşturması birkaç dakika sürer.
1. Tarayıcınız, bir istemci yapılandırması ZIP dosyasının kullanılabilir olduğunu gösterir. Bu, ağ geçidiniz ile aynı adı taşır. Klasörleri görüntülemek için dosyayı sıkıştırmayı açın.

### <a name="generate-files-using-powershell"></a><a name="zipps"></a>PowerShell kullanarak dosyalar oluşturma

1. VPN istemci yapılandırma dosyalarını oluştururken, '-AuthenticationMethod ' değeri ' EapTls ' olur. Aşağıdaki komutu kullanarak VPN istemci yapılandırma dosyalarını oluşturun:

   ```azurepowershell-interactive
   $profile=New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

   $profile.VPNProfileSASUrl
   ```

1. Zip dosyasını indirmek için URL 'YI tarayıcınıza kopyalayın ve ardından klasörleri görüntülemek için dosyayı açın.

## <a name="windows"></a><a name="installwin"></a>Windows

[!INCLUDE [Windows instructions](../../includes/vpn-gateway-p2s-client-configuration-windows.md)]

## <a name="mac-os-x"></a><a name="installmac"></a>Mac (OS X)

 Yerel IKEv2 VPN istemcisini Azure’a bağlanacak her Mac cihazda el ile yapılandırmanız gerekir. Azure, yerel Azure sertifika kimlik doğrulaması için mobileconfig dosyası sağlamaz. **Genel** , yapılandırma için ihtiyaç duyduğunuz tüm bilgileri içerir. İndirdiğiniz pakette Genel klasörünü görmüyorsanız, tünel türü olarak IKEv2 seçilmemiş olabilir. VPN Gateway temel SKU 'sunun Ikev2 'yi desteklemediğini unutmayın. IKEv2 seçildikten sonra zip dosyasını tekrar oluşturarak Genel klasörünü alın.<br>Genel klasörü aşağıdaki dosyaları içerir:

* Sunucu adresi ve tünel türü gibi önemli ayarları içeren **VpnSettings.xml**. 
* P2S bağlantı kurulumu sırasında Azure VPN Gateway doğrulamak için gereken kök sertifikayı içeren **Vpnserverroot. cer**.

Mac 'te sertifika kimlik doğrulaması için yerel VPN istemcisini yapılandırmak üzere aşağıdaki adımları kullanın. Azure 'a bağlanacak her Mac üzerinde bu adımları gerçekleştirmeniz gerekir:

1. **Vpnserverroot** kök sertifikasını Mac 'e aktarın. Bu işlem, dosyanın Mac 'nize kopyalanarak ve çift tıklanarak yapılabilir. İçeri aktarılacak **Ekle** ' yi seçin.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/add-certificate.png" alt-text="Ekran görüntüsü sertifikalar sayfasını gösterir":::
  
    >[!NOTE]
    >Sertifikaya çift tıkladığınızda **Ekle** iletişim kutusu görüntülenmeyebilir, ancak sertifika doğru depoya yüklenmiş olabilir. Sertifika kategorisi altındaki oturum açma anahtarlığınızdan sertifikayı kontrol edebilirsiniz.
    >
  
1. P2S ayarlarını yapılandırdığınız zaman Azure 'a yüklediğiniz kök sertifika tarafından verilen bir istemci sertifikası yüklediğinizi doğrulayın. Bu, önceki adımda yüklediğiniz VPNServerRoot öğesinden farklıdır. İstemci sertifikası kimlik doğrulaması için kullanılır ve gereklidir. Sertifika oluşturma hakkında daha fazla bilgi için bkz. [sertifika oluşturma](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert). İstemci sertifikasını nasıl yükleyeceğiniz hakkında bilgi için bkz. [istemci sertifikası yüklemesi](point-to-site-how-to-vpn-client-install-azure-cert.md).
1. **Ağ tercihleri** altında **ağ** Iletişim kutusunu açın ve Azure sanal ağına P2S BAĞLANTıSı için yeni bir VPN istemci bağlantı profili oluşturmak üzere **' + '** seçeneğini belirleyin.

   **Arabirim** DEĞERI ' VPN ' ve **VPN türü** değeri ' Ikev2 '. **Hizmet adı** alanında profil için bir ad belirtin ve ardından **Oluştur** ' u seçerek VPN istemci bağlantı profilini oluşturun.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/network.png" alt-text="Ekran görüntüsü, bir arabirim seçme, VPN türü seçme ve hizmet adı girme seçeneğiyle ağ penceresini gösterir":::
1. **Genel** klasörde, **VpnSettings.xml** dosyasından, **VPNServer** etiket değerini kopyalayın. Bu değeri, profilin **sunucu adresine** ve **uzak kimlik** alanlarına yapıştırın.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/server.png" alt-text="Ekran görüntüsü sunucu bilgilerini gösterir.":::
1. **Kimlik doğrulama ayarlarını** seçin ve **sertifika**' yı seçin. **Catalina** için **hiçbiri**' ni ve ardından **sertifika**' ı seçin.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/authentication-settings.png" alt-text="Ekran görüntüsü, kimlik doğrulama ayarlarını gösterir.":::

   Catalina için **hiçbiri** ' ni ve ardından **sertifika**' ı seçin. Doğru sertifikayı **seçin** :
   
   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/catalina.png" alt-text="Ekran görüntüsü, kimlik doğrulama ayarları ve sertifika seçili için seçili olmayan ağ penceresini gösterir.":::

1. **Seç...** öğesine tıklayın kimlik doğrulaması için kullanmak istediğiniz istemci sertifikasını seçmek için. Bu, adım 2 ' de yüklediğiniz sertifikadır.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png" alt-text="Ekran görüntüsü, bir sertifika seçebileceğiniz kimlik doğrulama ayarlarına sahip ağ penceresini gösterir.":::
1. **Bir kimlik seçin seçim** yapabileceğiniz sertifikaların listesini görüntüler. Doğru sertifikayı seçip **devam**' ı seçin.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/identity.png" alt-text="Ekran görüntüsü, uygun sertifikayı seçebileceğiniz bir kimlik seçin iletişim kutusu gösterir.":::

1. **Yerel kimlik** alanında, sertifikanın (adım 6 ' dan) adını belirtin. Bu örnekte bu değer `ikev2Client.com`’dur. Ardından, değişiklikleri kaydetmek için **Uygula** ' yı seçin.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/apply-connect.png" alt-text="Ekran görüntüsü Uygula ' yı gösterir.":::
1. Tüm değişiklikleri kaydetmek için **ağ** Iletişim kutusunda **Uygula** ' yı seçin. Ardından, Azure sanal ağına P2S bağlantısını başlatmak için **Bağlan** ' ı seçin.

## <a name="linux-strongswan-gui"></a><a name="linuxgui"></a>Linux (Strongswa GUI)

### <a name="install-strongswan"></a><a name="installstrongswan"></a>Strongswa 'yi yükler

[!INCLUDE [install strongSwan](../../includes/vpn-gateway-strongswan-install-include.md)]

### <a name="generate-certificates"></a><a name="genlinuxcerts"></a>İstemci sertifikaları oluşturma

Önceden sertifika oluşturduysanız aşağıdaki adımları kullanın:

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

### <a name="install-and-configure"></a><a name="install"></a>Yükleme ve yapılandırma

Ubuntu 18.0.4 üzerinde aşağıdaki yönergeler oluşturulmuştur. Ubuntu 16.0.10, Strongswa GUI 'yi desteklemez. Ubuntu 16.0.10 kullanmak istiyorsanız, [komut satırını](#linuxinstallcli)kullanmanız gerekir. Aşağıdaki örnekler, Linux sürümünüze ve Strongswa sürümüne bağlı olarak gördüğünüz ekranlarla eşleşmeyebilir.

1. Örnekteki komutu çalıştırarak **Strongswa** ve Ağ yöneticisini yüklemek için **terminali** açın.

   ```
   sudo apt install network-manager-strongswan
   ```
1. **Ayarlar**' ı ve ardından **ağ**' ı seçin. **+** Yeni bir bağlantı oluşturmak için düğmeyi seçin.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/edit-connections.png" alt-text="Ekran görüntüsü, ağ bağlantıları sayfasını gösterir.":::

1. Menüden **IPSec/Ikev2 (Strongswa)** öğesini seçin ve çift tıklayın.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/add-connection.png" alt-text="Ekran görüntüsü, VPN Ekle sayfasını gösterir.":::

1. **VPN Ekle** SAYFASıNDA, VPN bağlantınız için bir ad ekleyin.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/choose-type.png" alt-text="Ekran görüntüsü bir bağlantı türü seçin.":::
1. İndirilen istemci yapılandırma dosyalarında bulunan **genel** klasörden **VpnSettings.xml** dosyasını açın. **VPNServer** adlı etiketi bulun ve ' azuregateway ' ile başlayıp '. cloudapp.net ' ile biten adı kopyalayın.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/vpn-server.png" alt-text="Ekran görüntüsü verileri Kopyala ' yı gösterir.":::
1. Adı **ağ geçidi** bölümüne yeni VPN bağlantınızın **Adres** alanına yapıştırın. Sonra, **sertifika** alanının sonundaki klasör simgesini seçin, **genel** klasöre gidin ve **vpnserverroot** dosyasını seçin.
1. Bağlantının **istemci** bölümünde, **kimlik doğrulaması** için **sertifika/özel anahtar**' ı seçin. **Sertifika** ve **özel anahtar** için, daha önce oluşturulmuş sertifikayı ve özel anahtarı seçin. **Seçenekler**' de, **Iç IP adresi iste**' yi seçin. Ardından **Ekle**' yi seçin.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/ip-request.png" alt-text="Ekran görüntüsünde bir iç IP adresi Isteği gösterilir.":::

1. **Bağlantıyı açın**.

   :::image type="content" source="./media/point-to-site-vpn-client-configuration-azure-cert/turn-on.png" alt-text="Ekran görüntüsü kopyalamayı gösterir.":::

## <a name="linux-strongswan-cli"></a><a name="linuxinstallcli"></a>Linux (strongSwan CLı)

### <a name="install-strongswan"></a>Strongswa 'yi yükler

[!INCLUDE [install strongSwan](../../includes/vpn-gateway-strongswan-install-include.md)]

### <a name="generate-certificates"></a>İstemci sertifikaları oluşturma

Önceden sertifika oluşturduysanız aşağıdaki adımları kullanın:

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

### <a name="install-and-configure"></a>Yükleme ve yapılandırma

1. Azure portal adresinden VPNClient paketini indirin.
1. Dosyayı ayıklayın.
1. **Genel** klasöründen, **vpnserverroot. cer** **dosyasını/etc/IPSec.exe** dizinine kopyalayın veya taşıyın.
1. **CP Client. p12** öğesini **/etc/IPSec.exe dizinine** kopyalayın veya taşıyın. Bu dosya, VPN ağ geçidinin istemci sertifikasıdır.
1. **VpnSettings.xml** dosyasını açın ve `<VpnServer>` değeri kopyalayın. Bu değeri bir sonraki adımda kullanacaksınız.
1. Aşağıdaki örnekteki değerleri ayarlayın ve ardından **/etc/IPSec.exe** yapılandırmasına örnek ekleyin.
  
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
1. **/Etc/IPSec.exe** için aşağıdaki değerleri ekleyin.

   ```
   : P12 client.p12 'password' # key filename inside /etc/ipsec.d/private directory
   ```

1. Aşağıdaki komutları çalıştırın:

   ```
   # ipsec restart
   # ipsec up azure
   ```

## <a name="next-steps"></a>Sonraki adımlar

Üzerinde çalıştığınız orijinal makaleye dönün ve P2S yapılandırmanızı doldurun.

* [PowerShell yapılandırma adımları](vpn-gateway-howto-point-to-site-rm-ps.md).
* [Yapılandırma adımlarını Azure Portal](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
